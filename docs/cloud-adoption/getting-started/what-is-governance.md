---
title: CAF. Общее представление о системе управления облачными ресурсами
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.date: 02/11/2019
description: Объяснение системы управления облачными ресурсами в Azure
author: petertaylor9999
ms.openlocfilehash: 0989a5aad8a6290cce07fd71771c690bbd615e0d
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068860"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-cloud-resource-governance"></a>Общее представление о системе управления облачными ресурсами

В [как работает Azure?](what-is-azure.md), вы узнали, что Azure — это совокупность серверов и сетевого оборудования, запуска виртуализированного оборудования и программного обеспечения, от имени пользователей. Azure позволяет вашей организации в разработке приложений и ИТ-отделы agile, упрощая создание, чтение, обновление и удаление ресурсов, при необходимости.

Тем не менее хотя неограниченный доступ к ресурсам мог разработчиков очень гибкой, она может также привести к непредвиденных расходов. Например, команде разработчиков может быть разрешено развернуть набор ресурсов для тестирования, но после завершения команда может забыть их удалить. Эти ресурсы будут продолжать накапливать затраты, несмотря на то, что они больше не утвержденных или не нужны.

Решением является управление ресурсами доступа. **Управление** — непрерывный процесс управления, мониторинг и аудит использования ресурсов Azure в соответствии с требованиями вашей организации.

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ii94]

<!-- markdownlint-enable MD034 -->

Эти требования являются уникальными для каждой организации, поэтому не рекомендуется использовать универсального подхода к управлению. Вместо этого он работает для каждой организации для разработки их модель управления, использовать два средства основной системы управления Azure: **управления доступом на основе ролей (RBAC)** и **политики ресурсов**.

RBAC определяет роли и роли определяют возможности каждого пользователя, назначенного этой роли. Например **владельца** роль предоставляет все возможности (Создание, чтение, обновление и удаление) для ресурса, хотя **чтения** роль позволяет только возможность чтения. Роли могут определяться с широкой области, к которому применяется множество типов ресурсов или узкой области, к которому применяется несколько.

Правила создания ресурсов определяются политиками ресурсов. Например политику ресурсов можно ограничить номер SKU виртуальной машины для предварительно утвержденных определенный размер. Другой политики ресурсов удалось применить приложение тега для центра затрат, назначенный при запросе для создания ресурса.

При настройке этих средств, управление и гибкость организации все описанное выше. Более строгие политики управления, менее гибкой разработки ваших разработчиков и ИТ-специалисты будут. Политика ограничения управления может требовать дополнительных действий вручную, как требование разработчику заполнить форму или отправить сообщение электронной почты членом команды управления, чтобы вручную создать ресурс. Группа управления имеет ограничение по масштабируемой емкости и может стать узким местом, приводит к группам разработчиков непродуктивно ожидание их ресурсы были созданный или ненужные ресурсы, накапливаемый затраты, прежде чем они будут удалены.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы понимаете концепцию облачной управление ресурсами, Дополнительные сведения о том, как осуществляется доступ к ресурсам в Azure.

> [!div class="nextstepaction"]
> [Доступ к ресурсам в Azure](azure-resource-access.md)
