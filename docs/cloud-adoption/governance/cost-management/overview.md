---
title: CAF. Дисциплина "Управление затратами"
description: Описание управления затратами в отношении управления облаком
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: a86b1a75da57cec9c9aaf88abb70049f70731561
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243705"
---
# <a name="cost-management-discipline-overview"></a>Обзор дисциплины "Управление затратами"

Управление затратами является одной из [пяти дисциплин системы управления облаком](../governance-disciplines.md) в [модели управления CAF](../overview.md). Для многих клиентов управление затратами считается серьезной проблемой при внедрении облачных технологий. Найти баланс между требованиями к производительности, скоростью внедрения и затратами на облачные службы может оказаться сложной задачей. Это особенно важно во время основных бизнес-преобразований, которые реализуют облачные технологии. В этом разделе описывается подход к разработке дисциплины "Управление затратами" как части стратегии системы управления облаком.  

> [!NOTE]
> Система управления "Управление затратами" не заменяет имеющиеся бизнес-отделы, методы учета и процедуры, задействованные в финансовом управлении ИТ-затрат организации. Основная цель этой дисциплины — идентифицировать потенциальные облачные риски, связанные с ИТ-затратами, и предоставить рекомендации по их устранению для бизнес-команд и ИТ-отделов, ответственных за развертывание и управление облачными развертываниями. При разработке политики управления процессами не забудьте привлечь соответствующих бизнес и ИТ-специалистов к процессам планирования и анализа.

Основная аудитория этого руководства — облачные архитекторы вашей организации и другие члены вашей команды системы управления облаком. Тем не менее решения, политики и процессы, вытекающие из этой дисциплины, должны включать взаимодействие и обсуждения с соответствующими членами ваших бизнес- и ИТ-отделов, особенно с теми, кто отвечает за владение, управление и оплату за использование облачных рабочих нагрузок.

## <a name="policy-statements"></a>Правила политики

Действующие правила политики и соответствующие требования к архитектуре служат основой дисциплины "Управление затратами". Примеры правил политики см. в статье [Cost Management sample policy statements](./policy-statements.md) (Примеры правил политики Управления затратами). Эти примеры можно использовать в качестве отправной точки для политик системы управления организации.

> [!CAUTION]
> Примеры политик взяты из общего опыта клиентов. Чтобы лучше согласовать эти политики с конкретными потребностями системы управления облаком, выполните следующие шаги для создания политик, соответствующих уникальным потребностям вашей компании.

## <a name="developing-cost-management-governance-policy-statements"></a>Разработка правил политики системы управления "Управление затратами"

Следующие шесть шагов помогут вам определить политики системы управления для контроля затрат в вашей среде.

<!-- markdownlint-disable MD033 -->

<ul  class="panelContent cardsE">
<li style="display: flex; flex-direction: column;">
    <a href="./template.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-template.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Шаблон Управления затратами</h3>
                        <p class="x-hidden-focus">Скачайте шаблон для документирования дисциплины "Управление затратами".</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li><li style="display: flex; flex-direction: column;">
    <a href="./business-risks.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-risks.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Бизнес-риски</h3>
                        <p class="x-hidden-focus">Вы должны распознать мотивы и риски, связанные с дисциплиной "Управление затратами".</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./metrics-tolerance.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-metrics.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Индикаторы и метрики</h3>
                        <p class="x-hidden-focus">Показатели, которые помогут понять, настало ли время для инвестиций в дисциплину "Управление затратами".</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./compliance-processes.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-enforce.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Процессы, обеспечивающие соблюдение политики</h3>
                        <p class="x-hidden-focus">Здесь предложены процессы для поддержки соблюдения политики в дисциплине "Управление затратами".</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./discipline-improvement.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-maturity.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Зрелость</h3>
                        <p class="x-hidden-focus">Сопоставление развития системы управления облаком с этапами внедрения облачных технологий.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./toolchain.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-toolchain.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Цепочка инструментов</h3>
                        <p class="x-hidden-focus">Службы Azure, которые можно использовать для поддержки дисциплины "Управление затратами".</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="next-steps"></a>Дополнительная информация

Начните с оценки бизнес-рисков в конкретной среде.

> [!div class="nextstepaction"]
> [Описание бизнес-рисков](./business-risks.md)

<!-- markdownlint-enable MD033 -->
