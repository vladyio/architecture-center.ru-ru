---
title: Непрерывная интеграция и непрерывная поставка микрослужб
description: Непрерывная интеграция и непрерывная поставка для микрослужб.
author: MikeWasson
ms.date: 03/27/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: c52ff3d0a330f564e5f7e9b0b07f0ba84c328c8b
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59639790"
---
# <a name="cicd-for-microservices-architectures"></a>CI/CD для архитектур микрослужб

Ускорение жизненного цикла — это один из основных преимуществ архитектуры микрослужб. Но без Непрерывной интеграции и если процесс налажен, не добиться гибкости, promise микрослужб. В этой статье описываются проблемы и рекомендованные подходы к проблеме.

## <a name="what-is-cicd"></a>Что такое CI/CD?

Когда мы говорим о CI/CD, мы действительно речь идет о несколько связанных процессов: непрерывная интеграция, непрерывная поставка и непрерывное развертывание.

- **Непрерывная интеграция**. Изменения в коде часто объединяются с главной ветвью. Автоматической сборки и процессов тестирования убедитесь, что код в главной ветви всегда промышленного качества.

- **Непрерывная Поставка**. Любые изменения кода, которые прошли процесс непрерывной Интеграции, автоматически публикуются в рабочей среде. Для развертывания в рабочей среде может потребоваться утверждение вручную, в противном случае оно выполняется автоматически. В результате этого код всегда будет *готов* к развертыванию в рабочей среде.

- **Осуществлять непрерывное развертывание.** Код изменяет этого этапа, предыдущие два шага, автоматически развертываются *в рабочую среду*.

Ниже приведены некоторые цели надежного процесса CI/CD для архитектуры микрослужб:

- Каждая команда может создавать и развертывать службы, которыми она владеет независимо, не влияя на другие команды и не нарушая их работу.

- Перед развертыванием новой версии службы в рабочей среде она развертывается в средах разработки, тестирования и контроля качества для проверки. Шлюзы качества применяются на каждом этапе.

- Новую версию службы можно развернуть параллельно с предыдущей версии.

- Заданы достаточные политики контроля доступа.

- Для контейнерных рабочих нагрузок можно доверять образы контейнеров, развернутых в рабочей среде.

## <a name="why-a-robust-cicd-pipeline-matters"></a>Почему важна надежные конвейера CI/CD

В традиционных монолитного приложения есть один конвейер сборки, выходные данные которого является исполняемым файлом приложения. Все данные разработки поступают в этот конвейер. При обнаружении ошибки с высоким приоритетом необходимо интегрировать, протестировать и опубликовать исправление, из-за чего может быть отложен выпуск новых функций. Этих проблем можно избежать при наличии хорошо организованных модулей и использовании ветвей компонентов, чтобы свести к минимуму влияние изменений кода. Но так как объем данных приложения со временем увеличивается и оно становится более сложным, а также добавляются дополнительные функции, процесс выпуска для монолитного приложения становится нестабильным и часто нарушается.

Принципы работы с микрослужбами предусматривают, что цепочка выпуска не может быть длинной, когда каждая команда должна становиться в очередь. Команда, создающая службу "A", может выпустить обновление в любое время, не ожидая объединения, тестирования и развертывания изменений в службе "B".

![Схема монолитного процесса CI/CD](./images/cicd-monolith.png)

Для достижения выпуск на высокой скорости, конвейер выпуска должен быть автоматическим и очень надежным, чтобы свести к минимуму риск. Если отпустить в рабочую среду каждый день или несколько раз в день, регрессии или перерывы должны возникать очень редко. В то же время в случае развертывания неправильного обновления необходимо располагать надежным способом быстро выполнить откат или накат до предыдущей версии службы.

## <a name="challenges"></a>Сложности

- **Множество небольших независимых баз кода**. Каждая команда отвечает за создание собственной службы со своим собственным конвейером сборки. В некоторых организациях команды могут использовать отдельные репозитории кода. Отдельные репозитории может привести к ситуации, когда многие команды знают, как создать систему, и никто в организации знает, как развертывать все приложение. Например, что происходит в сценарии аварийного восстановления, если необходимо быстро выполнить развертывание в новый кластер?

    **Устранение.** Иметь единое и автоматических конвейер для сборки и развертывания служб, таким образом, чтобы эти знания «отображается» в каждой команде.

- **Множество языков программирования и платформ**. При том, что каждая команда использует свой собственный набор технологий, могут возникнуть сложности в создании процесса сборки, применимого для всей организации. Процесс сборки должен быть достаточно гибким, чтобы любая команда могла использовать его независимо от выбранного языка программирования или платформы.

    **Устранение.** Контейнеризация процесс сборки для каждой службы. Таким образом, система сборки просто нужно иметь возможность запускать контейнеры.

- **Интеграция и нагрузочное тестирование**. Так как команды выпускают обновления независимо друг от друга, с разработкой надежного и комплексного тестирования могут возникнуть сложности, особенно если у службы есть зависимости от других служб. Кроме того запуск полного рабочего кластера может быть дорогостоящим, поэтому маловероятно, что каждая команда будет выполняться весь собственный кластер в производственных масштабах только для тестирования.

- **Управление выпусками**. Каждая команда должна иметь возможность развернуть обновление в рабочей среде. Это не означает, что у каждого участника команды есть на это разрешения. Но при наличии централизованной роли диспетчера выпусков может снизиться скорость развертываний.

    **Устранение.** Чем больше автоматизирован процесс непрерывной интеграции и поставки и чем выше его надежность, тем меньше потребность в центральном администрировании. С другой стороны, могут применяться различные политики для выпуска обновлений основных компонентов и небольших исправлений ошибок. Децентрализация вовсе не означает отсутствие управления.

- **Обновления службы**. Обновление службы до новой версии не должно нарушать работу других служб, зависящих от нее.

    **Устранение.** Используйте методы развертывания, такие как blue-green или ранние версии для некритических изменений. Критически важных изменениях API, разверните новую версию параллельно с предыдущей версии. Таким образом, служб, которые используют предыдущий API может обновляться и протестированы для нового API. См. в разделе [службах обновления](#updating-services)ниже.

## <a name="monorepo-vs-multi-repo"></a>Нескольких репозиториев Monorepo vs

Прежде чем создавать рабочий процесс CI/CD, необходимо знать, как будет структурирована база кода и как ею управлять.

- Команды работают в отдельном репозитории или monorepo (один репозиторий)?
- Что такое стратегия создания ветви?
- Кто может отправлять код в рабочую среду? Существует ли роль менеджера выпуска?

Подход единого репозитория получил приоритет, но есть определенные преимущества и недостатки.

| &nbsp; | Единый репозиторий | Несколько репозиториев |
|--------|----------|----------------|
| **Преимущества** | Общий доступ к коду<br/>Простой способ стандартизировать код и средства<br/>Простой рефакторинг кода<br/>Обнаруживаемость – единое представление кода<br/> | Очистка владельца в команде<br/>Потенциально меньше конфликтов слияния<br/>Помогает применить разделение микрослужб |
| **Сложности** | Изменение совместного использования кода может повлиять на несколько микрослужб<br/>Большая вероятность конфликтов слияния<br/>Инструментарий необходимо масштабировать в большую базу кода<br/>управление доступом;<br/>Более сложный процесс развертывания | Труднее совместно использовать код<br/>Труднее применять стандарты кодирования<br/>Управление зависимостями<br/>Диффузная база кода, низкая обнаруживаемость<br/>Отсутствие общей инфраструктуры

## <a name="updating-services"></a>Обновление служб

Есть различные стратегии обновления службы, которая уже находится в рабочей среде. Мы рассмотрим три распространенных варианта: последовательное обновление, развертывание по схеме blue-green и ранний выпуск.

### <a name="rolling-updates"></a>Последовательные обновления

В последовательном обновлении развертываются новые экземпляры службы, которые сразу же начинают получать запросы. При появлении новых экземпляров предыдущие экземпляры удаляются.

**Пример.** В Kubernetes последовательные обновления — это поведение по умолчанию, при обновлении спецификации pod для развертывания. Контроллер развертывания создает новый набор реплик для обновленных модулей pod. Затем он масштабирует новый набор реплик, при этом уменьшая масштаб старого набора реплик, чтобы сохранить необходимое количество реплик. Пока не готовы новые модули pod, старые модули не удаляются. Kubernetes сохраняет журнал обновления, поэтому можно выполнить откат обновления при необходимости.

Одной из проблем последовательного обновления является то, что во время обновления и старая, и новая версии выполняются и получают трафик. В течение этого периода любой запрос может перенаправляться в одну из двух версий.

Критически важных изменениях API, мы рекомендуем для поддержки обеих версий параллельно, пока не будут обновлены все клиенты с предыдущей версии. См. в разделе [управление версиями API](./design/api-design.md#api-versioning).

### <a name="blue-green-deployment"></a>"Сине-зеленое" развертывание

При "сине-зеленом" развертывании новая версия развертывается параллельно с предыдущей версией. После проверки новой версии весь трафик сразу же переходит из предыдущей версии в новую. После переключения трафика выполняется мониторинг приложения на наличие проблем. Если что-то пойдет не так, можно вернуться обратно к старой версии. При условии, что проблем нет, старую версию можно удалить.

В более традиционных монолитных или n-уровневых приложениях "сине-зеленое" развертывание обычно подразумевает подготовку двух идентичных сред. Понадобится развернуть новую версию в промежуточной среде, а затем перенаправить трафик клиента в промежуточную среду. Например, путем переключения виртуальных IP-адресов. В архитектуре микрослужб обновления происходят на уровне микрослужб, что обычно будет развернуть обновление в ту же среду и использовать механизм обнаружения служб для замены.

**Пример**. В Kubernetes для выполнения "сине-зеленых" развертываний не нужно подготавливать отдельный кластер. Вместо этого можно воспользоваться селекторами. Создайте новый ресурс развертывания с новой спецификацией pod и другим набором меток. Создайте это развертывание, не удаляя предыдущее развертывание и не изменяя службу, которая указывает на него. После запуска новых модулей pod можно обновить селектор службы в соответствии с новым развертыванием.

Одним из недостатков развертывания blue-green является то, что во время обновления, выполняется два раза больше модулей POD для службы (текущих и следующих). Если для модулей pod требуется больший объем ресурсов ЦП или памяти, может потребоваться временно развернуть кластер, чтобы справиться с потреблением ресурсов.

### <a name="canary-release"></a>Ранний выпуск

В раннем выпуске обновленная версия развертывается в небольшое количество клиентов. Затем отслеживается поведение новой службы, прежде чем развернуть ее для всех клиентов. Так вы можете выполнить медленный выпуск контролируемым способом, наблюдать за реальными данными, а также обнаруживать проблемы, прежде чем они затронут всех клиентов.

Ранний выпуск сложнее в управлении, чем "сине-зеленое" развертывание или последовательное обновление, так как необходимо динамически направлять запросы в разные версии службы.

**Пример**. В Kubernetes можно настроить службу так, чтобы охватить два набора реплик (по одному для каждой версии) и регулировать счетчики реплики вручную. Тем не менее этот подход не детализирован из-за того, как Kubernetes распределяет нагрузку между модулями pod. Например если у вас есть всего 10 реплик, вы сместить трафик можно только с шагом в 10%. При использовании ПО слоя взаимодействия между службами можно использовать правила маршрутизации для реализации более сложной стратегии раннего выпуска.

## <a name="next-steps"></a>Дальнейшие действия

Сведения конкретного процессам CI/CD для микрослужб, выполняющихся на платформе Kubernetes.

- [Непрерывные Интеграция и развертывание микрослужб в Kubernetes](./ci-cd-kubernetes.md)