---
title: Крупное предприятие. Эволюция согласованности ресурсов
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Крупное предприятие. Эволюция согласованности ресурсов
author: BrianBlanchard
ms.openlocfilehash: 9fc6ca015310c02338463d3c8aa53ddfc74699df
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901636"
---
# <a name="large-enterprise-resource-consistency-evolution"></a>Крупное предприятие. Эволюция согласованности ресурсов

В этой статье описывается добавление в MVP средств обеспечения согласованности ресурсов для поддержки критически важных приложений.

## <a name="evolution-of-the-narrative"></a>Эволюция описания

Команды по внедрению облака выполнили все требования для перемещения защищаемых данных. С этими приложениями поставляются Соглашения об уровне обслуживания для бизнеса, и для этих приложений необходима поддержка со стороны отдела ИТ-операций. Когда соответствующая команда перенесет два центра обработки данных, множество команд по разработке приложений и команд бизнес-аналитики будут готовы к разработке новых решений. Отдел ИТ-операций еще не знаком с облачными операциями, и им необходим способ, позволяющий быстро интегрировать существующие рабочие процессы.

### <a name="evolution-of-current-state"></a>Эволюция текущего состояния

- ИТ-отдел активно перемещает рабочие нагрузки с защищенными данными в Azure. Несколько рабочих нагрузок с низким приоритетом обрабатывают рабочий трафик. Как только отдел ИТ-операций подтвердит готовность поддержать рабочие нагрузки, можно будет перенести большее их количество.
- Команды по разработке приложений готовы к рабочему трафику.
- Команда бизнес-аналитики готова интегрировать прогнозы и аналитику в системы, которые выполняют операции для трех бизнес-подразделений.

### <a name="evolution-of-the-future-state"></a>Эволюция будущего состояния

- Отдел ИТ-операций еще не знаком с облачными операциями, и им необходим способ, позволяющий быстро интегрировать существующие рабочие процессы.

Изменения в текущем и будущем состояниях представляют новые риски, требующие новых положений политики.

## <a name="evolution-of-tangible-risks"></a>Эволюция реальных рисков

**Прерывание работы бизнеса**. Существует неизбежный риск того, что любая новая платформа может вызвать прерывание работы критически важных бизнес-процессов. Команда ИТ-операций и команды, внедряющие различные облачные решения, относительно неопытны в работе с облачными операциями. Это повышает риск прерывания. Нужно уменьшить этот риск и реализовать механизм управления им.

Бизнес-риск можно разделить на несколько технических рисков:

- Несопоставленные рабочие процессы могут вызвать простои, которые не удается обнаружить или быстро исправить.
- Внешнее вторжение или атака типа "отказ в обслуживании" могут привести к прерыванию работы бизнеса.
- Критически важные ресурсы могут быть обнаружены неправильно и, следовательно, могут работать ненадлежащим образом.
- Необнаруженные или неправильно помеченные ресурсы могут не поддерживаться существующими процессами операционного управления.
- Конфигурация развернутых ресурсов может не соответствовать ожидаемым показателям производительности.
- Журналы могут записываться ненадлежащим образом и сохраняться не в центральном расположении, что не позволяет исправлять проблемы с производительностью.
- Политики восстановления могут завершиться ошибкой или выполняться дольше, чем ожидается.
- Несогласованные процессы развертывания могут привести к брешам в системе безопасности с последующими утечками данных и прерываниями работы.
- Изменения конфигурации и отсутствие требуемых обновлений могут привести к брешам в системе безопасности с последующими утечками данных и прерываниями работы.
- Конфигурация может не требовать применения требований определенных Соглашений об уровне обслуживания или применения восстановления.
- Развернутые операционные системы или приложения могут не соответствовать требованиям ОС и требованиям по усилению безопасности приложения.
- Из-за того, что несколько команд работает в облаке, существует риск несогласованности.

## <a name="evolution-of-the-policy-statements"></a>Эволюция положений политики

Описанные ниже изменения политики уменьшают новые риски и упрощают реализацию. Список выглядит длинным, но внедрять эти политики проще, чем кажется.

1. Все развернутые ресурсы необходимо классифицировать по важности и типу данных. Перед развертыванием в облаке классификации должны быть проверены командой управления облачными решениями и владельцем приложения.
2. Подсети, содержащие критически важные приложения, должны быть защищены с помощью решения брандмауэра, которое может обнаруживать вторжения и реагировать на атаки.
3. Средства управления должны выполнять аудит и обеспечивать соблюдение требований сетевой конфигурации, определенных командой по базовой системе безопасности.
4. Средства управления должны проверять, что все ресурсы, связанные с критически важными приложениями или защищенными данными, включены в мониторинг для оптимизации и наблюдения за истощением ресурсов.
5. Средства управления должны проверять, что данные журнала соответствующего уровня собираются для всех критически важных приложений или защищенных данных.
6. Процесс управления должен проверять правильную реализацию резервного копирования, восстановления и соблюдения Соглашению об уровне обслуживания для критически важных приложений и защищенных данных.
7. Средства управления должны ограничивать развертывание виртуальных машин, разрешая использование только утвержденных образов.
8. Средства управления должны **предотвращать** автоматические обновления во всех развернутых ресурсах, поддерживающих критически важные приложения. Нарушения должны быть рассмотрены с привлечением команд операционного управления и устранены в соответствии с политиками операций. Ресурсы, которые не обновляются автоматически, необходимо включить в процессы, которые выполняет отдел ИТ-операций.
9. Средства управления должны проверять теги, относящиеся к классификации затрат, уровню важности, Соглашений об уровне обслуживания, приложений и данных. Все значения должны совпадать с предустановленными значениями, управляемыми командой управления облачными решениями.
10. Процессы управления должны включать в себя аудит в точке развертывания, а также регулярный аудит через определенные периоды времени для обеспечения согласованности всех ресурсов.
11. Тенденции и уязвимости, которые могут повлиять на облачные развертывания, должны регулярно проверяться командой по обеспечению безопасности для подготовки обновлений средств базовой системы безопасности, используемых в облаке.
12. До запуска в рабочей среде все критически важные приложения и защищенные данные должны быть добавлены в указанное решение для операционного мониторинга. Ресурсы, которые не могут быть обнаружены в выбранном средстве ИТ-операций, нельзя выпускать для использования в рабочей среде. Чтобы обеспечить возможность обнаружения ресурсов в будущих развертываниях, любые изменения, которые нужны для обеспечения такой возможности, должны быть внесены в соответствующие процессы развертывания.
13. Команды операционного управления должны проверить размер обнаруженного ресурса, чтобы убедиться в соответствии ресурса требованиям к производительности.
14. Средства развертывания должны быть утверждены командой управления облачными решениями для обеспечения непрерывного управления развернутыми ресурсами.
15. Сценарии развертывания должны храниться в центральном репозитории, который доступен команде управления облачными решениями для периодической проверки и аудита.
16. В рамках процессов проверки управления необходимо проверять, чтобы развернутые ресурсы были настроены в соответствии с Соглашением об уровне обслуживания и требованиями к восстановлению.

## <a name="evolution-of-the-best-practices"></a>Эволюция рекомендаций

В этом разделе статьи в проект MVP управления будут включены новые политики Azure и реализована служба "Управление затратами Azure". Совокупно такие изменения проекта станут новыми положениями корпоративной политики.

Следуя этому вымышленному примеру, предполагается, что эволюция защищенных данных уже произошла. Исходя из этой рекомендации, ниже будут добавлены требования к мониторингу операций, подготавливающие подписку для критически важных приложений.

**Подписка для корпоративных ИТ-отделов**. Добавьте следующие элементы в подписку корпоративных ИТ-отделов, которая выступает в качестве концентратора.

1. В качестве внешней зависимости команде облачных операций нужно будет определить средство операционного мониторинга, средство обеспечения непрерывности бизнес-процессов и аварийного восстановления (BCDR) и средство автоматического исправления. Затем команда управления облачными решениями сможет поддерживать необходимые процессы обнаружения.
    1. В этом варианте использования команда облачных операций выбирает Azure Monitor в качестве основного средства мониторинга критически важных приложений.
    2. Команда также выбрала Azure Site Recovery в качестве основного средства BCDR.
2. Реализация Azure Site Recovery.
    1. Определите и разверните хранилище Azure для резервного копирования и восстановления.
    2. Создайте шаблон управления ресурсами Azure для создания хранилища в каждой подписке.
3. Реализация Azure Monitor.
    1. После определения критически важной подписки можно создать рабочую область Log Analytics с помощью PowerShell. Это процесс подготовки к развертыванию.

**Отдельная подписка для внедрения облака**. Ниже приведены действия, которые помогут обеспечить возможность обнаружения каждой подписки с помощью решения мониторинга и ее готовности к включению в операции BCDR.

1. Служба "Политика Azure" для критически-важных узлов.
    1. Выполняйте аудит и применяйте только стандартные роли.
    2. Выполняйте аудит и обеспечьте шифрование всех учетных записей хранения.
    3. Выполняйте аудит и обеспечивайте использование утвержденного набора подсети и виртуальной сети для каждого сетевого интерфейса.
    4. Выполняйте аудит и применяйте ограничения определенных пользователем таблиц маршрутизации.
    5. Выполняйте аудит и обеспечивайте развертывание агентов Log Analytics для виртуальных машин Windows и Linux.
2. Схема Azure.
    1. Создайте схему Azure с именем `mission-critical-workloads-and-protected-data`. Эта схема будет применять ресурсы в дополнение к схеме защищенных данных.
    2. Добавьте в схему новые политики Azure.
    3. Применяйте схему к любой подписке, которая должна размещать критически важное приложение.

## <a name="conclusion"></a>Заключение

Добавление этих процессов и изменений в MVP управления позволяет снизить множество рисков, связанных с управлением ресурсом. Вместе они добавляют элементы управления восстановлением, масштабированием и мониторингом, необходимые для расширения возможностей операций с использованием облака.

## <a name="next-steps"></a>Дополнительная информация

Так как внедрение облака продолжает развиваться и предоставлять дополнительные бизнес-преимущества, механизмы управления рисками и облачными операциями также нужно совершенствовать. Для рассматриваемой вымышленной компании следующий переломный этап настанет, когда в облаке будет развернуто более 1000 ресурсов или расходы превысят сумму в 10 000 долл. США в месяц. На этом этапе команде управления облачными решениями нужно добавить элементы управления затратами.

> [!div class="nextstepaction"]
> [Эволюция управления затратами](./cost-management-evolution.md)