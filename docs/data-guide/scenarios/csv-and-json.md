---
title: Обработка CSV- и JSON-файлов
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 52de5207a5169d2f2a7f701fbc078fb422e61e13
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58244915"
---
# <a name="working-with-csv-and-json-files-for-data-solutions"></a>Работа с CSV- и JSON-файлами в контексте решений для работы с данными

CSV и JSON — вероятно, наиболее распространенные форматы, используемые для приема, хранения неструктурированных или частично структурированных данных и обмена ими.

## <a name="about-csv-format"></a>Сведения о формате CSV

CSV-файлы (значения, разделенные запятыми) обычно используются для обмена табличными данными между системами в обычном тексте. В основном они содержат строку заголовка, которая предоставляет имена столбцов для данных, но в остальном они считаются частично структурированными. Это связано с тем, что CSV-файлы изначально не могут представлять иерархические или реляционные данные. Связи между данными обычно обрабатываются с использованием нескольких CSV-файлов. Внешние ключи хранятся в столбцах одного или нескольких файлов, однако связи между этими файлами не выражаются самим форматом. Файлы в формате CSV могут использовать другие разделители, помимо запятых, например символы табуляции или пробелы.

Несмотря на ограничения, CSV-файлы являются популярным выбором для обмена данными, так как они поддерживаются широким рядом бизнес-приложений, потребительских и научных приложений. Например, программы баз данных и электронных таблиц могут импортировать и экспортировать CSV-файлы. Аналогичным образом большинство модулей обработки пакетных и потоковых данных (например, Spark и Hadoop) изначально поддерживают сериализацию и десериализацию CSV-файлов и предлагают способы применения схемы при чтении. Они упрощают работу с данными, предоставляя варианты выполнения запроса и сохраняя сведения в более эффективном формате для быстрой обработки.

## <a name="about-json-format"></a>Сведения о формате JSON

Данные JSON (нотация объектов JavaScript) представлены в виде пар "ключ — значение" в частично структурированном формате. JSON часто сравнивают с XML, так как они могут хранить данные в иерархическом формате. Дочерние данные в них представлены вместе с родительскими. Оба формата являются самоописывающими и удобочитаемыми для пользователя, однако документы JSON, как правило, намного меньше. Поэтому их чаще используют при сетевом обмене данными, особенно с появлением веб-служб на базе REST.

Файлы в формате JSON имеют несколько преимуществ по сравнению с CSV-файлами.

- JSON поддерживает иерархические структуры, упрощая хранение связанных данных в одном документе и представление сложных связей.
- Большинство языков предоставляют упрощенные библиотеки сериализации JSON или встроенную поддержку десериализации JSON в объекты.
- JSON поддерживает списки объектов, помогая избежать беспорядочных преобразований списков в реляционную модель данных.
- JSON — это широко используемый формат файлов для баз данных NoSQL, таких как MongoDB, Couchbase и Azure Cosmos DB.

Так как множество данных передачи уже находится в формате JSON, большинство веб-языков изначально поддерживают работу с JSON или используют внешние библиотеки для сериализации и десериализации данных JSON. Благодаря этой поддержке JSON используют в логических форматах посредством представления структуры данных, форматах обмена для горячих данных и хранилищах для холодных данных.

Многие модули обработки пакетных и потоковых данных изначально поддерживают сериализацию и десериализацию JSON. Несмотря на то что данные, содержащиеся в документах JSON, в конечном итоге могут быть сохранены в более оптимизированных по производительности форматах, таких как Parquet или Avro, они служат в качестве необработанных исходных данных, что очень важно для повторной обработки данных (при необходимости).

## <a name="when-to-use-csv-or-json-formats"></a>Использование форматов CSV и JSON

CSV-файлы чаще используются для экспорта и импорта данных или их обработки для задач аналитики и машинного обучения. Файлы в формате JSON имеют те же преимущества, но чаще используются в решениях обмена горячими данными. Документы JSON часто отправляются с веб- и мобильных устройств, выполняющих онлайн-транзакции, устройств Интернета вещей для односторонней или двунаправленной связи или клиентских приложений, обменивающихся данными со службами SaaS и PaaS или бессерверными архитектурами.

CSV- и JSON-файлы облегчают обмен данными между разными системами или устройствами. Частично структурированные форматы позволяют переносить практически любые типы данных, а универсальная поддержка упрощает работу. Оба формата могут использоваться как источник истины в случаях, когда обработанные данные хранятся в двоичных форматах для более эффективного выполнения запроса.

## <a name="working-with-csv-and-json-data-in-azure"></a>Работа с CSV- и JSON-файлами в Azure

Azure предлагает несколько решений для работы с CSV- и JSON-файлами, выбор которых зависит от ваших потребностей. Обычно эти файлы размещены либо в службе хранилища Azure, либо в Azure Data Lake Store. Большинство служб Azure, которые работают с этими и другими текстовыми файлами, интегрируются с любой службой хранения объектов. Однако в некоторых случаях вы можете напрямую импортировать данные в SQL Azure или в другое хранилище данных. SQL Server имеет встроенную поддержку для хранения документов JSON и работы с ними, что упрощает [импорт и обработку этих типов файлов](/sql/relational-databases/json/import-json-documents-into-sql-server). Для [импорта CSV-файлов](/sql/relational-databases/json/import-json-documents-into-sql-server) можно использовать такую служебную программу, как средство массового импорта SQL.

Можно также запросить JSON-файлы непосредственно из хранилища BLOB-объектов Azure без их импорта в Azure SQL. Полный пример см. в руководстве по [использованию JSON-файлов в Azure SQL](https://medium.com/@mauridb/work-with-json-files-with-azure-sql-8946f066ddd4). Этот вариант недоступен для CSV-файлов.

В зависимости от сценария вы можете выполнять [пакетную обработку](../big-data/batch-processing.md) данных или [обработку данных в режиме реального времени](../big-data/real-time-processing.md).

## <a name="challenges"></a>Сложности

При работе с этими форматами возникают следующие проблемы.

- Без каких-либо ограничений в модели данных CSV- и JSON-файлы подвержены повреждению данных (ситуация "мусор на входе, мусор на выходе"). Например, в файлах нет понятия объекта даты и времени, поэтому в поле даты можно ввести, к примеру, ABC123.

- CSV- и JSON-файлы в качестве решения для холодного хранения плохо масштабируются при работе с большими данными. В большинстве случаев их невозможно разделить на секции для параллельной обработки. Кроме того, их невозможно сжать так же, как и двоичные форматы. Поэтому эти данные часто обрабатываются и хранятся в оптимизированных для чтения форматах, таких как Parquet и ORC, которые также предоставляют индексы и встроенную статистику о данных.

- Возможно, нужно будет применить схему для частично структурированных данных, чтобы упростить выполнение запросов и анализ. Как правило, для этого файлы должны храниться в форме, соответствующей требованиям хранилища данных вашей среды, например в базе данных.
