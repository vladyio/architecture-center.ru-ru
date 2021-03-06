---
title: Минимизация координации
titleSuffix: Azure Application Architecture Guide
description: Сведите к минимуму координацию между службами приложений, чтобы обеспечить масштабируемость.
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: e124e6720b909bafc74a4c074454ec0a3ec30c59
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245615"
---
# <a name="minimize-coordination"></a>Минимизация координации

## <a name="minimize-coordination-between-application-services-to-achieve-scalability"></a>Сведите к минимуму координацию между службами приложений, чтобы обеспечить масштабируемость

Большинство облачных приложений состоит из нескольких служб приложений и веб-интерфейсов, баз данных, бизнес-процессов, отчетов и аналитики и т. д. Чтобы обеспечить масштабируемость и надежность, каждая из этих служб должна выполняться на нескольких экземплярах.

Что произойдет, если два экземпляра попытаются одновременно выполнить операции, влияющие на общее состояние? В некоторых случаях требуется координация действий между узлами, например для поддержки гарантий ACID. На этой схеме служба `Node2` ожидает, пока `Node1` снимет блокировку базы данных:

![Схема блокировки базы данных](./images/database-lock.svg)

Любая координация связана с ограничением возможностей, предоставляемых горизонтальным масштабированием, и определенными сложностями. В этом примере конфликты блокировки будут возникать все чаще по мере увеличения масштаба приложения и числа экземпляров. В самом худшем случае экземпляры интерфейса будут находиться в состоянии блокировки большую часть времени.

Еще одна распространенная причина, требующая координации, — семантика запроса "только один раз". Например, каждый заказ должен быть обработан только один раз. Два рабочих процесса ожидают появления новых заказов. `Worker1` извлекает заказ для обработки. Теперь приложение должно гарантировать следующее: 1) `Worker2` не будет дублировать эту работу, 2) заказ не будет потерян в случае сбоя `Worker1`.

![Схема координации](./images/coordination.svg)

Вы можете применить для координации рабочих потоков шаблон [Планировщик, агент, контролер][sas-pattern] Но в нашем примере удобнее будет секционировать работу. Каждой рабочей роли назначается некоторое число заказов (например, с привязкой к региону выставления счетов). Если рабочий процесс завершится сбоем, его новый экземпляр начнет работу там, где остановился предыдущий, но соседние экземпляры не будут конкурировать.

## <a name="recommendations"></a>Рекомендации

**Учитывайте итоговую согласованность**. Если данные распределяются между несколькими объектами, только координация между ними обеспечит строгую согласованность. Предположим, что определенная операция обновляет две базы данных. Вместо того, чтобы включать эту операцию в одну транзакцию, создайте в системе механизм обеспечения итоговой согласованности. Например, шаблон [компенсирующих транзакций][compensating-transaction] позволит выполнить логический откат в случае сбоя.

**Используйте события предметной области для синхронизации состояний**. [Событие предметной области][domain-event] регистрируется, когда в пределах некоторой предметной области происходит что-то важное. Такие события могут прослушиваться всеми заинтересованными службами, что позволяет обойтись без глобальных транзакций для координации действий между несколькими службами. Чтобы применить такой подход, система должна поддерживать итоговую согласованность (см. предыдущий пункт).

**Изучите такие шаблоны, как CQRS и шаблон источников событий**. Эти два шаблона минимизируют вероятность конфликтов между рабочими нагрузками чтения и записи.

- [Шаблон CQRS][cqrs-pattern] разделяет операции чтения и записи. В некоторых реализациях данные для чтения физически отделены от данных для записи.

- В [шаблоне источников событий][event-sourcing] все изменения состояния сохраняются в виде последовательности событий в хранилище постоянного пополнения. Добавление одного события в такой поток является атомарной операцией и требует минимальных блокировок.

Эти два шаблона дополняют друг друга. Если к хранилищу, которое в соответствии с шаблоном CQRS используется только для записи, применить источники событий, такое хранилище для чтения сможет ожидать те же события и воспроизводить моментальный снимок текущего состояния, оптимизированный для обработки запросов. Но прежде, чем внедрять CQRS или источники событий, ознакомьтесь с особенностями такого подхода. Дополнительные сведения см. в статье о [стиле архитектуры CQRS][cqrs-style].

**Секционирование данных**.  Не включайте все данные в одну схему данных, которую используют сразу несколько служб приложения. Архитектура микрослужб принудительно применяет этот принцип, назначая каждой службе собственное хранилище данных. В пределах одной базы данных секционирование данных может оптимизировать параллелизм, так как запись в любой из сегментов никак не влияет на службы, использующие для записи другие сегменты.

**Применяйте идемпотентные операции**. Везде, где это возможно, применяйте идемпотентные операции. Это позволит всегда обрабатывать их с использованием семантики "как минимум один раз". Предположим, вы помещаете рабочие элементы в очередь. Если при выполнении операции рабочая роль завершится сбоем, следующая рабочая роль просто выберет тот же рабочий элемент.

**Используйте параллельную и асинхронную обработку.** Если для операция выполняется в несколько этапов, которые выполняются асинхронно (например, вызовы удаленных служб), вы можете вызывать их параллельно и объединять полученные результаты. Такой подход предполагает, что каждый шаг никак не зависит от результатов выполнения предыдущего шага.

**Используйте оптимистическую блокировку, если это возможно**. Пессимистическое управление параллелизмом подразумевает блокировку базы данных для предотвращения конфликтов. Это может снижать производительность и доступность системы. Если применить оптимистическое управление параллелизмом, каждая транзакция изменит собственную копию (моментальный снимок) данных. Когда транзакция будет зафиксирована, ядро СУБД проверит эту транзакцию и отклонит ее, если она может повлиять на согласованность базы данных.

База данных SQL Azure и SQL Server поддерживают оптимистический параллелизм, выполняя [изоляцию моментальных снимков][sql-snapshot-isolation]. Некоторые службы хранилища Azure, например [Azure Cosmos DB][cosmosdb-faq] и [служба хранилища Azure][storage-concurrency], поддерживают оптимистическую блокировку с помощью тэгов eTag.

**Применяйте MapReduce или другой алгоритм распределенной параллельной обработки**. В зависимости от характера данных и выполняемой работы, иногда ее можно разделить на несколько независимых задач, чтобы выполнять эти задачи параллельно на нескольких узлах. См. статью [Стиль архитектуры для больших вычислений][big-compute].

**Используйте шаблон выбора лидера для координации действий**. В тех случаях, когда операции необходимо координировать, сделайте так, чтобы координатор не был единой точкой отказа в приложении. Если реализовать [шаблон выборов лидера][leader-election], в каждый момент времени один из экземпляров будет лидером, который выполняет функции координатора. Если в этом экземпляре происходит сбой, выбирается новый лидер.

<!-- links -->

[big-compute]: ../architecture-styles/big-compute.md
[compensating-transaction]: ../../patterns/compensating-transaction.md
[cqrs-style]: ../architecture-styles/cqrs.md
[cqrs-pattern]: ../../patterns/cqrs.md
[cosmosdb-faq]: /azure/cosmos-db/faq
[domain-event]: https://martinfowler.com/eaaDev/DomainEvent.html
[event-sourcing]: ../../patterns/event-sourcing.md
[leader-election]: ../../patterns/leader-election.md
[sas-pattern]: ../../patterns/scheduler-agent-supervisor.md
[sql-snapshot-isolation]: /sql/t-sql/statements/set-transaction-isolation-level-transact-sql
[storage-concurrency]: https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/