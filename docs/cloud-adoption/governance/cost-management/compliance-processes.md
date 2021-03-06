---
title: CAF. Процессы обеспечения соответствия политикам управления затратами
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Процессы обеспечения соответствия политикам управления затратами
author: BrianBlanchard
ms.openlocfilehash: 5fd007546589020f376107382c54c391cc5d05ad
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901331"
---
# <a name="cost-management-policy-compliance-processes"></a>Процессы обеспечения соответствия политикам управления затратами

В этой статье обсуждается подход к созданию процессов, поддерживающих дисциплину управления службой [Управление затратами](./overview.md). Эффективное управление облачными затратами начинается с повторяющихся неавтоматизированных процессов, разработанных для поддержки соответствия политике. Для этого необходимо регулярно привлекать команду управления облачными решениями, а также бизнес-партнеров и заинтересованные лица для просмотра и обновления политики, а также обеспечения соответствия ее требованиям. Кроме того, многие текущие процессы мониторинга и принудительного применения можно автоматизировать или дополнить с помощью набора инструментов, чтобы сократить расходы на управление и обеспечить более быстрый отклик на отклонение политики.

## <a name="planning-review-and-reporting-processes"></a>Планирование, проверка и составление отчетности

Лучшие средства Управления затратами в облаке хороши только в тех процессах и политиках, которые они поддерживают. Ниже приведен набор примеров процессов, часто использующихся в дисциплине "Управление затратами". Используйте эти примеры в качестве отправной точки при планировании процессов, что позволит вам постоянно обновлять политику затрат на основе изменений в бизнесе и обратной связи от бизнес-групп с учетом руководств по управлению затратами.

**Начальная оценка рисков и планирование**. В рамках начального внедрения дисциплины "Управление затратами" определите основные бизнес-риски и отклонения, связанные с расходами на облако. Используйте эти сведения для обсуждения бюджета и рисков, связанных с затратами, с членами своих бизнес-групп и команд по безопасности и для разработки базового плана политик для снижения этих рисков, чтобы установить начальную стратегию управления.

**Планирование развертывания**. Перед развертыванием любого ресурса установите прогнозируемый бюджет на основе ожидаемого облачного распределения. Убедитесь, что информация о владельцах и учетных данных для развертывания задокументирована.  

**Ежегодное планирование**. Ежегодно проводите сводный анализ всех развернутых и подлежащих развертыванию ресурсов. Приведите в соответствие бюджеты по бизнес-подразделениям, отделам, командам и другим соответствующим подразделениям, чтобы расширить возможности внедрения самообслуживания. Убедитесь, что руководитель каждого платежного подразделения осведомлен о бюджете и о том, как отслеживать расходы.

Это лучшее время, чтобы сделать предварительное ассигнование средств или предварительную покупку для максимизации дисконтирования. Разумно привести годовой бюджет в соответствие с финансовым годом поставщика облачных ресурсов, чтобы в дальнейшем выгодно использовать варианты скидок в конце года.

**Ежеквартальное планирование**. Ежеквартально пересматривайте бюджеты с каждым руководителем платежного подразделения, чтобы согласовать прогнозированные и фактические расходы. Если имеются изменения плана или неожиданные шаблоны расходов, приведите в соответствие бюджет и перераспределите его.

При ежеквартальном планировании самое время проверить участников команды управления облаком на наличие пробелов в знаниях относительно текущих и будущих бизнес-планов. Пригласите соответствующих специалистов и владельцев рабочих нагрузок для участия в обзорах и планировании в качестве временных консультантов или постоянных участников вашей команды.

**Образование и обучение**. Каждые два месяца проводите обучающие сессии, чтобы убедиться в том, что ИТ- и бизнес-специалисты в курсе новейших требований политики управления затратами. В ходе этого процесса просмотрите и обновите любую документацию, руководства или другие обучающие ресурсы, чтобы убедиться, что они соответствуют новым положениям корпоративной политики.

**Ежемесячные отчеты**. Ежемесячно сообщайте о соотношении фактических и прогнозированных расходов. Уведомляйте руководителей платежных подразделений о любых неожиданных отклонениях.

Эти базовые процессы помогут привести в соответствие расходы и заложить основу для дисциплины "Управление затратами".

## <a name="ongoing-monitoring-processes"></a>Текущие процессы мониторинга

Успешная стратегия управления затратами зависит от прозрачности прошлых, текущих и запланированных будущих расходов на облако. Без возможности анализировать соответствующие метрики и данные имеющихся расходов вы не можете выявить изменения в рисках или обнаружить нарушения рискоустойчивости. Текущие процессы управления, описанные выше, требуют данных о качестве, чтобы обеспечить возможность изменения политики для оптимальной защиты инфраструктуры от изменяющихся бизнес-требований и облачного потребления.

Убедитесь, что ваши ИТ-команды реализовали автоматизированные системы мониторинга расходов на облако и потребление в случае незапланированных отклонений от ожидаемых расходов. Предоставьте системы отчетности и оповещений, чтобы быстрое обнаружение и устранение рисков потенциальных нарушений политики.

## <a name="compliance-violation-triggers-and-enforcement-actions"></a>Триггеры нарушения соответствия и принудительные действия

При обнаружении нарушений следует предпринять принудительные действия для обеспечения соблюдения политики. Вы можете автоматизировать большинство триггеров нарушения с помощью средств, описанных в [цепочке инструментов Управления затратами для Azure](toolchain.md).

Ниже приведены примеры триггеров.

* Отклонения от ежемесячного бюджета. Обсудите любые отклонения ежемесячных расходов, превышающие 20 % от прогнозированного, с руководителем платежного подразделения. Запишите решение проблемы и изменения запланированного бюджета.
* Темп внедрения. При любых отклонениях уровня подписки, превышающих 20 %, необходимо провести проверку с руководителем платежного подразделения. Запишите решение проблемы и изменения запланированного бюджета.

## <a name="next-steps"></a>Дополнительная информация

Используя [шаблон управления облаком](./template.md), задокументируйте процессы и триггеры, соответствующие текущему плану внедрения облака.

Рекомендации по выполнению политик управления облаком, согласованных с планами внедрения, см. в статье об улучшении дисциплины "Управление затратами".

> [!div class="nextstepaction"]
> [Улучшения дисциплины "Управление затратами"](./discipline-improvement.md)
