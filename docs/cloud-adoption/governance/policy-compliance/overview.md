---
title: CAF. Подготовка корпоративной ИТ-политики к использованию в облаке
description: Описание концепции корпоративной политики в отношении облачной системы управления
author: BrianBlanchard
ms.date: 2/8/2019
ms.openlocfilehash: e15f4ef4e6fa9a64ea2bb78cd1df6c5867528383
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902584"
---
<!-- markdownlint-disable MD026 -->

# <a name="caf-how-can-corporate-it-policy-become-cloud-ready"></a>CAF. Подготовка корпоративной ИТ-политики к использованию в облаке

Облачная система управления формируется в результате постоянного внедрения облачных технологий в течение длительного времени, так как настоящее устойчивое преобразование невозможно выполнить за один раз. Попытка быстро создать завершенную облачную систему управления без изменения ключевых аспектов корпоративной политики редко приносит желаемые результаты. Вместо этого мы рекомендуем применять поэтапный подход.

Важнейшими отличиями нашей платформы внедрения облака являются цикл покупки и то, как этот цикл приводит к реальным преобразованиям. Благодаря тому, что приобретение не сопровождается значительными капитальными расходами (CapEx), инженеры могут начать эксперименты и попытки внедрения намного раньше. В большинстве корпоративных сред устранение ограничения в виде капитальных расходов на внедрение позволяет сократить циклы обратной связи, а также обеспечить органичное развитие и поэтапное выполнение работ.

Переход в облако требует изменений и в системе управления. Во многих организациях преобразование корпоративной политики позволяет улучшить систему управления и повысить показатели соблюдения политики благодаря поэтапному подходу к изменениям. Кроме того, можно автоматизировать их применение на базе новых возможностей, которые настраиваются в системах поставщика облачной службы.

В этой статье описаны основные действия, которые могут вам помочь в преобразовании корпоративной политики для реализации расширенной модели управления.

## <a name="define-corporate-policy-to-mature-cloud-governance"></a>Определение корпоративной политики для зрелой облачной системы управления

В традиционной системе управления с поэтапным подходом корпоративная политика создает рабочее определение управления. Большинство действий отделов ИТ-управления ориентированы на внедрение технологий мониторинга, принудительного применения, эксплуатации и автоматизации этих корпоративных политик. Облачная система управления основана на аналогичных концепциях.

![Корпоративная система управления и дисциплины управления](../../_images/operational-transformation-govern.png)

*Рис. 1. Корпоративная система управления и дисциплины управления.*

Представленное выше изображение демонстрирует взаимодействия между риском, политикой, соответствием, мониторингом и применением в рамках создания стратегии управления. Далее следуют пять дисциплин облачного управления, по которым вы будете создавать свою стратегию.

## <a name="review-existing-policies"></a>Изучение имеющихся политик

На приведенном выше изображении стратегии управления (риск, политика и соответствие требованиям, мониторинг и применение) начинаются с выявления бизнес-рисков. Понимание того, как в облаке изменяются [бизнес-риски](understanding-business-risk.md), будет первым шагом к созданию устойчивой стратегии для организации облачной системы управления. Сотрудничайте с бизнес-подразделениями, чтобы получить точный инструмент [оценки допустимости риска](risk-tolerance.md), который поможет вам понять, риск какого уровня необходимо устранять. Оценка новых рисков и их допустимости может стать поводом для [пересмотра имеющихся политик](what-is-a-cloud-policy-review.md), который поможет определить необходимый уровень управления для конкретной организации.

> [!TIP]
> Если в вашей организации действуют сторонние нормы, одним из основных бизнес-рисков следует считать угрозу несоблюдения [нормативных требований](what-is-regulatory-compliance.md). Зачастую такой риск невозможно устранить, поэтому нужно применять стратегию строгого соответствия. Обязательно сформируйте четкое представление о нормативных требованиях третьих сторон, прежде чем начинать пересмотр политики.

## <a name="an-incremental-approach-to-cloud-governance"></a>Поэтапный подход к организации облачной системы управления

Поэтапный подход к организации облачной системы управления основан на предположении, что нельзя превышать определенный уровень [допустимости бизнес-рисков](risk-tolerance.md). Вместо этого для системы управления ставятся задачи ускорять изменения бизнеса, помогать инженерам разобраться в рекомендациях по архитектуре, обеспечить регулярное информирование о [бизнес-рисках](understanding-business-risk.md) и их устранение. Сохранение традиционной роли системы управления может стать непреодолимым барьером на пути освоения новых систем инженерами и компанией в целом.

Поэтапный подход к организации системы управления в облаке иногда создает естественные трения между командами, которые создают новые решения для бизнеса, и командами, которые защищают бизнес от рисков. Но в нашей модели эти две команды могут слаженно работать в рамках процесса, разделенного на четкие этапы, которые также называются спринтами. Команда облачной системы управления и команда внедрения облака будут согласованно сотрудничать для выявления, оценки и устранения бизнес-рисков. Эти усилия позволят создать естественную среду позитивного взаимодействия, которая снизит трения между командами.

## <a name="minimum-viable-product-mvp-for-policy"></a>MVP (продукт с минимальной функциональностью) для политики

Первым шагом в развитии новой связи между группой облачной системы управления и группой внедрения облака должно стать соглашение по MVP для политики. В MVP для облачной системы управления следует учесть, что бизнес-риски в начале процесса незначительны, но, скорее всего, увеличатся по мере применения в организации новых облачных служб.

Например:  Для предприятия, которое развертывает 5 виртуальных машин, не содержащих данных с высоким уровнем влияния на бизнес, бизнес-риски невысоки. Спустя несколько этапов, когда количество виртуальных машин достигнет 1000 и будут перемещаться данные с высоким уровнем влияния на бизнес, риски существенно увеличиваются.

MVP для политики является попыткой определить "фундамент" для политик, необходимых при развертывании некоторого количества первых виртуальных машин и (или) приложений. Речь идет о достаточно малом, но существенном количестве развертываемых единиц. В этом наборе политик не должно быть много ограничений, но нужно описать основы для быстрого перехода от одного этапа к следующему. Разработка поэтапной политики позволит постепенно развивать эту стратегию управления. Небольшие и медленные изменения постепенно преобразуют MVP для политики в полное решение, реализующее все возможности, выявленные на этапе пересмотра существующей политики.

## <a name="incremental-policy-growth"></a>Поэтапное развитие политики

Поэтапное развитие политики является ключевым механизмом в жизненном цикле самой политики и облачной системе управления. В то же время это и важнейшее предварительное условие для поэтапного подхода к организации системы управления. Чтобы такая модель работала правильно, группа системы управления должна быть твердо готова выделять определенное время для каждого очередного спринта, чтобы оценить и реализовать изменения в дисциплинах системы управления.

**Требования к времени выполнения спринтов.** В начале каждой итерации все команды внедрения облака создают список ресурсов, которые будут переноситься или адаптироваться на этом этапе. Ожидается, что команда облачной системы управления выделит достаточно времени для изучения этих списков, проверки классификации данных для всех указанных ресурсов, оценки новых рисков для каждого ресурса, обновления рекомендаций по архитектуре и информирования остальных групп о внесенных изменениях. Обычно на эти задачи в каждом спринте требуется от 10 до 30 часов. Также ожидается, что для взаимодействия на этом уровне будет выделен как минимум один сотрудник, который будет отвечать за развитие систем управления в рамках масштабного процесса внедрения облака.

**Требования к времени для выпусков.** В начале каждого выпуска группе внедрения облака и группе разработки облачной стратегии следует составить упорядоченный по приоритетам список приложений и (или) рабочих нагрузок, которые будут переноситься в текущей итерации, а также других действий по изменению бизнеса. Эти точки данных позволяют команде облачной системы управления заблаговременно выявить новые бизнес-риски. Так вы получите достаточно времени для согласования с бизнес-подразделениями и оценки допустимости рисков.