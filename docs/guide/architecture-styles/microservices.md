---
title: Стиль архитектуры микрослужб
titleSuffix: Azure Application Architecture Guide
description: В этой статье описаны преимущества и недостатки архитектур микрослужб в Azure, а также приведены рекомендации по работе с ними.
author: MikeWasson
ms.date: 11/13/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19, microservices
ms.openlocfilehash: 87a9dd31b6d935dd11a5a2a2950b6de11f337741
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59639977"
---
# <a name="microservices-architecture-style"></a>Стиль архитектуры микрослужб

Архитектура микрослужб представляет собой набор небольших автономных служб. Каждая служба является самодостаточной и должна реализовывать возможности одной компании.

![Логическая схема архитектуры микрослужб](./images/microservices-logical.svg)

В каком-то смысле микрослужбы являются естественным результатом развития сервисноориентированных архитектур (SOA). Но микрослужбы и SOA отличаются. Ниже приведены определяющие характеристики микрослужбы.

- Архитектура микрослужб состоит из небольших, независимых и слабо связанных между собой служб.

- Каждая служба является отдельной базой кода, которой может управлять небольшая команда разработчиков.

- Службы можно развертывать независимо друг от друга. Разработчики могут обновлять существующую службу без повторной сборки и повторного развертывания всего приложения.

- Службы отвечают за сохранение собственных данных или внешнего состояния. В этом состоит отличие от традиционной модели, в которой сохранение данных обрабатывается на отдельном уровне.

- Службы взаимодействуют между собой с помощью четко определенных API-интерфейсов. Сведения о внутренней реализации каждой службы скрыты от других служб.

- Службы не должны совместно использовать один и тот же стек технологий, библиотеки или платформы.

Помимо самих служб, некоторые компоненты обладают типичной архитектурой микрослужб:

**Управление.** Компонент управления отвечает за размещение служб на узлах, определение сбоев, перераспределение служб между узлами и другие операции.

**Обнаружение служб.** Этот компонент ведет список служб и узлов, на которых находятся службы. Он поддерживает поиск в службах, позволяя найти конечную точку службы.

**Шлюз API.** Шлюз API является точкой входа для клиентов. Клиенты не вызывают службы напрямую. Вместо этого они вызывают шлюз API, который пересылает вызов соответствующим службам в серверной части. Шлюз API может собирать ответы от нескольких служб и возвращать совокупный ответ.

Преимущества использования API шлюза:

- Он разделяет клиенты и службы. Можно управлять версиями служб и выполнять их рефакторинг без обновления всех клиентов.

- Службы могут использовать протоколы обмена сообщениями, которые не работают в Интернете, например AMQP.

- Шлюз API может выполнять другие сквозные функции, например аутентификацию, ведение журнала, завершение запросов SSL и балансировку нагрузки.

## <a name="when-to-use-this-architecture"></a>Когда следует использовать эту архитектуру

Используйте эту архитектуру для следующих сценариев:

- крупные приложения, для которых требуется выпуск на высокой скорости;

- сложные приложения, которым нужна высокая степень масштабируемости;

- приложения с так называемыми "толстыми" доменами или большим количеством поддоменов;

- организация, состоящая из небольших команд разработчиков.

## <a name="benefits"></a>Преимущества

- **Независимые развертывания.** Вы можете обновить службу без повторного развертывания всего приложения, а также выполнить откат или накат обновления, если что-то работает неправильно. Исправления ошибок и выпуски компонентов лучше поддаются управлению и в меньшей степени сопряжены с рисками.

- **Независимая разработка.** Одна команда разработчиков может выполнить сборку, тестирование и развертывание службы. Это позволяет непрерывно внедрять инновации и быстрее выпускать службы.

- **Небольшие специализированные команды.** Команда может сосредоточиться на одной службе. Чем меньше объем каждой службы, тем проще понять ее базу кода и тем легче будет его освоить новым членам команды.

- **Изоляция ошибок.** Если одна служба выходит из строя, приложение продолжает работать. Но это не означает, что такая устойчивость доступна по умолчанию. Вам необходимо соблюдать рекомендации по обеспечению устойчивости и следовать конструктивным шаблонам. См. в разделе [Проектирование надежных приложений Azure][resiliency-overview].

- **Стеки смешанных технологий.** Команды могут выбрать технологию, которая лучше всего подходит их службе.

- **Детальное масштабирование.** Службы можно масштабировать независимо друг от друга. В то же время из-за более высокой плотности служб на одной виртуальной машине ее ресурсы будут полностью использованы. Благодаря ограничениям размещения службы можно привести в соответствие с профилем виртуальной машины (высокая загрузка ЦП, большой объем памяти и т. д.).

## <a name="challenges"></a>Сложности

- **Сложность.** Приложение для микрослужб содержит больше перемещаемых частей, чем эквивалентное монолитное приложение. Каждая служба является простой структурой, но вся система в целом гораздо сложнее.

- **Разработка и тестирование.** Разработка с учетом зависимостей между службами требует другого подхода. Существующие средства не всегда предназначены для работы с зависимостями служб. Рефакторинг между службами может быть сложной задачей. Также непросто тестировать зависимости служб, особенно если приложение быстро изменяется.

- **Недостаток управления.** Децентрализованный подход к созданию микрослужб имеет свои преимущества, но он также может привести к проблемам. В приложении может появиться так много разных языков и платформ, что его будет трудно обслуживать. Можно попробовать установить некоторые стандарты для всего проекта, не ограничивая чрезмерно гибкость разработчиков. В частности, это касается сквозных функций, таких как ведение журнала.

- **Перегрузки и задержки сети.** Использование большого количества мелких детальных служб может привести к интенсивному взаимодействию между ними. Кроме того, если цепочка зависимостей службы становится слишком длинной (служба A вызывает службу B, которая вызывает службу C и так далее), дополнительная задержка может стать проблемой. Необходимо тщательно разрабатывать API-интерфейсы. Не создавайте API-интерфейсы, которые отправляют слишком много сообщений, и найдите места, в которых можно использовать асинхронные модели связи.

- **Целостность данных.** Каждая микрослужба отвечает за сохранение своих данных. Поэтому может быть сложно поддерживать согласованность данных. Применяйте итоговую согласованность, где это возможно.

- **Управление.** Для успешной работы с микрослужбами требуется современная культура DevOps. Коррелированное ведение журнала для разных служб может оказаться сложной задачей. Как правило, ведение журнала необходимо коррелировать с несколькими вызовами службы для одной операции пользователя.

- **Управление версиями.** Обновления службы не должны нарушать работу служб, зависящих от нее. Несколько служб могут обновляться в любой момент времени, поэтому без тщательного планирования могут возникнуть проблемы с обратной или прямой совместимостью.

- **Набор навыков.** Микрослужбы являются распределенными системами. Тщательно оцените знания и опыт своей команды для работы с этими службами.

## <a name="best-practices"></a>Рекомендации

- Моделируйте службы для определенной сферы деятельности.

- Децентрализуйте все данные. Отдельные команды разработчиков отвечают за проектирование и создание служб. Избегайте совместного использования кода или схем данных.

- Данные должны храниться в службе, к которой они относятся. Используйте наиболее подходящее хранилище для каждой службы и каждого типа данных.

- Службы взаимодействуют через хорошо спроектированные API-интерфейсы. Избегайте утечки сведений о реализации. API-интерфейсы должны моделировать домен, а не внутреннюю реализацию службы.

- Избегайте взаимозависимостей между службами. Взаимозависимости могут возникать при использовании общих схем баз данных и жестких протоколов связи.

- Перенесите нагрузку сквозных функций, таких как аутентификация и завершение SSL-запросов, в шлюз.

- Храните набор знаний домена за пределами шлюза. Шлюз должен обрабатывать и перенаправлять клиентские запросы без знания бизнес-правил или логики домена. Иначе шлюз становится зависимостью и может привести к взаимозависимости между службами.

- Службы должны иметь слабую взаимозависимость и высокую функциональную слаженность. Функции, которые, вероятно, будут изменяться одновременно, должны упаковываться и развертываться одновременно. Если они находятся в отдельных службах, эти службы в конечном итоге становятся тесно связанными, потому что изменения в одной службе требуют обновления другой службы. Слишком частый обмен данными между двумя службами может быть признаком тесной взаимозависимости и низкой слаженности.

- Изолируйте сбои. Используйте стратегии обеспечения устойчивости для предотвращения каскадных сбоев в службе. См. в разделе [шаблоны устойчивости] [ resiliency-patterns] и [Проектирование надежных приложений][resiliency-overview].

## <a name="next-steps"></a>Дальнейшие действия

Подробное руководство по разработке архитектуры микрослужб в Azure см. в статье [Проектирование, создание и использование микрослужб в Azure](../../microservices/index.md).

<!-- links -->

[resiliency-overview]: ../../reliability/index.md
[resiliency-patterns]: ../../patterns/category/resiliency.md
