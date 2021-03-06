---
title: Рефакторинг приложения, перенесенного из облачных служб Azure в Azure Service Fabric
description: Как выполнить рефакторинг существующего приложения, перенесенного из облачных служб Azure в Azure Service Fabric
author: petertay
ms.date: 02/02/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.openlocfilehash: 1fd6bb5df18b46c8df3719fd107dd53a18dfd4ff
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2019
ms.locfileid: "54487293"
---
# <a name="refactor-an-azure-service-fabric-application-migrated-from-azure-cloud-services"></a>Рефакторинг приложения, перенесенного из облачных служб Azure в Azure Service Fabric

[![GitHub](../_images/github.png) Пример кода][sample-code]

В этой статье описывается выполнение рефакторинга существующего приложения Azure Service Fabric для детализированной настройки архитектуры. Статья содержит рекомендации по проектированию, упаковке, обеспечению производительности и развертыванию оптимизированного приложения Service Fabric.

## <a name="scenario"></a>Сценарий

Как описано в предыдущей статье о [переносе приложения облачных служб Azure в Azure Service Fabric][migrate-from-cloud-services], команда, занимающаяся разработкой шаблонов и рекомендаций, выпустила в 2012 г. книгу, в которой описан процесс проектирования и реализации приложения облачных служб в Azure вымышленной компанией Tailspin. Эта компания проектирует и внедряет приложение облачных служб, которое называется **Surveys**. Приложение Surveys позволяет пользователям создавать и публиковать опросы, на которые можно отвечать в Интернете. На следующей схеме показана архитектура этой версии приложения Surveys:

![](./images/tailspin01.png)

Веб-роль **Tailspin.Web** размещает сайт MVC ASP.NET, который клиенты Tailspin используют следующим образом:
* для регистрации в приложении Surveys;
* для создания или удаления отдельных запросов;
* для просмотра результатов отдельных опросов;
* для экспорта результатов опроса в SQL;
* для просмотра и анализа агрегированных результатов опроса.

Веб-роль **Tailspin.Web.Survey.Public** также размещает сайт MVC ASP.NET, на который пользователи заходят для заполнения опросов. Эти ответы помещаются в очередь для сохранения.

Рабочая роль **Tailspin.Workers.Survey** выполняет фоновую обработку, собирая запросы из нескольких очередей.

Группа, разрабатывающая шаблоны и практические рекомендации, затем создала новый проект, чтобы перенести это приложение в Azure Service Fabric. Задачей этого проекта было изменение кода, необходимое только для запуска приложения в кластере Azure Service Fabric. В результате архитектура исходных рабочих ролей и веб-ролей осталась неизменной. Эта архитектура очень похожа на версию приложения облачной службы:

![](./images/tailspin02.png)

Служба **Tailspin.Web** переносится из исходной веб-роли *Tailspin.Web*.

Служба **Tailspin.Web.Survey.Public** переносится из исходной веб-роли *Tailspin.Web.Survey.Public*.

Служба **Tailspin.AnswerAnalysisService** переносится из исходной рабочей роли *Tailspin.Workers.Survey*.

> [!NOTE] 
> Хотя в каждую из рабочих ролей и веб-ролей были внесены минимальные изменения кода, **Tailspin.Web** и **Tailspin.Web.Survey.Public** были изменены для резидентного размещения веб-сервера [Kestrel]. Предыдущая версия приложения Surveys представляла собой приложение ASP.NET, которое было размещено с использованием Interet Information Services (IIS), но без возможности запуска IIS в качестве службы в Service Fabric. Следовательно, любой веб-сервер должен иметь возможность резидентного размещения, как например [Kestrel]. В некоторых ситуациях возможно запускать службы IIS в контейнере в Service Fabric. См. [сценарии использования контейнеров][container-scenarios].  

Теперь компания Tailspin выполняет рефакторинг приложения для обеспечения более детализированной архитектуры. Это делается для того, чтобы упростить разработку, сборку и развертывание приложения Surveys. Разбивая существующие рабочие роли и веб-роли для получения более детализированной архитектуры, компания Tailspin хочет удалить тесно связанные зависимости данных и передачи данных между этими ролями.

Компания Tailspin ожидает от рефакторинга приложения и другие преимущества.
* Каждая служба может быть упакована в независимые проекты с достаточно небольшой областью, чтобы ими могла управлять небольшая команда.
* Каждая из этих служб может быть отдельно развернута и обновлена до определенной версии.
* Каждая служба может быть реализована с использованием самой оптимальной технологии для этой службы. Например, кластер Service Fabric может включать службы, созданные с помощью разных версий платформы .Net, Java или других языков, например C или C++.
* Каждая служба может масштабироваться независимо друг от друга в ответ на увеличение и уменьшение нагрузки.

> [!NOTE] 
> Мультитенантность не входит в задачи, решаемые с помощью рефакторинга этого приложения. У компании Tailspin есть несколько вариантов для обеспечения мультитенантности. Эти решения можно реализовать позже без необходимости изменять исходный проект. Например, Tailspin может создать отдельные экземпляры служб для каждого клиента в кластере или отдельный кластер для каждого клиента.

## <a name="design-considerations"></a>Рекомендации по проектированию
 
На следующей схеме показана оптимизированная архитектура приложения Surveys:

![](./images/surveys_03.png)

**Tailspin.Web** — это размещенное резидентно приложение MVC ASP.NET службы без отслеживания состояния, которое клиенты Tailspin посещают для создания опросов и просмотра результатов. Большая часть кода используется службой совместно с *Tailspin.Web* из перенесенного приложения Service Fabric. Как упоминалось ранее, эта служба использует ASP.NET Core, переключаясь с использования Kestrel в качестве веб-интерфейса к реализации WebListener.

**Tailspin.Web.Survey.Public** — это служба без отслеживания состояния, в которой также автономно размещается сайт MVC ASP.NET. Пользователи посещают этот сайт для выбора опросов из списка и их последующего заполнения. Эта служба совместно использует большую часть своего кода со службой *Tailspin.Web.Survey.Public* из перенесенного приложения Service Fabric. Эта служба также использует ASP.NET Core, переключаясь с использования Kestrel в качестве веб-интерфейса к реализации WebListener.

**Tailspin.SurveyResponseService** — это служба с отслеживанием состояния, которая хранит ответы на опросы в хранилище BLOB-объектов Azure. Она также объединяет ответы в данные для анализа опросов. Служба реализована как служба с отслеживанием состояния, так как она использует [ReliableConcurrentQueue][reliable-concurrent-queue] для пакетной обработки ответов. Эта функция была изначально реализована в службе *Tailspin.AnswerAnalysisService* в перенесенном приложении Service Fabric.

**Tailspin.SurveyManagementService** — это служба без отслеживания состояния, которая сохраняет и извлекает опросы, а также вопросы из опросов. Эта служба использует хранилище BLOB-объектов Azure. Эта возможность также была изначально реализована в компонентах доступа к данным служб *Tailspin.Web* и *Tailspin.Web.Survey.Public* в перенесенном приложении Service Fabric. Компания Tailspin выполнила рефакторинг исходных функций этой службы, чтобы ее можно было независимо масштабировать.

**Tailspin.SurveyAnswerService** — это служба без сохранения состояния, которая извлекает ответы на опросы и выполняет анализ опросов. Эта служба также использует хранилище BLOB-объектов Azure. Эта возможность также была изначально реализована в компонентах доступа к данным службы *Tailspin.Web* в перенесенном приложении Service Fabric. Компания Tailspin выполнила рефакторинг исходных функций этой службы, чтобы обеспечить меньшую нагрузку и использовать меньшее число экземпляров для экономии ресурсов.

**Tailspin.SurveyAnalysisService** — это служба без сохранения состояния, которая сохраняет сводные данные с ответами на опросы в кэше Redis для быстрого извлечения. Эта служба вызывается службой *Tailspin.SurveyResponseService* при получении каждого ответа на опрос. Затем новые данные с ответами на опросы добавляются в сводные данные. Эта служба включает функцию, которая была изначально реализована в службе *Tailspin.AnswerAnalysisService* из перенесенного приложения Service Fabric.

## <a name="stateless-versus-stateful-services"></a>Службы без отслеживания состояния и с отслеживанием состояния

Azure Service Fabric поддерживает следующие модели программирования:
* Гостевая исполняемая модель позволяет упаковывать любые исполняемые файлы в качестве службы с последующим развертыванием в кластере Service Fabric. Service Fabric организует и администрирует выполнение гостевого исполняемого файла.
* Модель контейнера обеспечивает развертывание служб в образы контейнеров. Service Fabric поддерживает создание и администрирование контейнеров на основе ядра Linux, а также контейнеры Windows Server. 
* Модель программирования на основе надежных служб позволяет создавать службы без отслеживания состояния или с отслеживанием состояния, интегрируемые со всеми функциями платформы Service Fabric. Службы с отслеживанием состояния предусматривают сохранение реплицированного состояния в кластере Service Fabric. А службы без отслеживания состояния — нет.
* Модель программирования на основе надежных субъектов позволяет создавать службы, реализующие шаблон виртуального субъекта.

Все службы в приложении Surveys являются надежными службами без сохранения состояния, за исключением службы *Tailspin.SurveyResponseService*. Эта служба реализует очередь [ReliableConcurrentQueue][reliable-concurrent-queue] для обработки ответов на опросы при их получении. Ответы в ReliableConcurrentQueue сохраняются в хранилище BLOB-объектов Azure, а затем передаются в *Tailspin.SurveyAnalysisService* для анализа. Компания Tailspin использует ReliableConcurrentQueue, так как ответы не требуют строгой последовательности , обеспечиваемой такими решениями, как служебная шина Azure. ReliableConcurrentQueue также используется для обеспечения высокой пропускной способности и низкой задержки для операций включения данных в очередь и исключения из нее.

Обратите внимание, что операции для сохранения элементов, исключенных из очереди ReliableConcurrentQueue, в идеале должны быть идемпотентными. Если во время обработки элемента в очереди создается исключение, один и тот же элемент может обрабатываться несколько раз. В приложении Surveys операции слияния ответов на опросы в *Tailspin.SurveyAnalysisService* не являются идемпотентными, так как компания Tailspin решила, что данные анализа опроса представляют только текущий моментальный снимок данных, которые могут не быть согласованными. Ответы на опросы, сохраненные в хранилище BLOB-объектов Azure, характеризуются конечной согласованностью, так что итоговый анализ опроса всегда можно выполнить повторно на основе этих данных.

## <a name="communication-framework"></a>Платформа взаимодействия

Каждая служба в приложении Surveys взаимодействует с помощью веб-API RESTful. API RESTful предлагают следующие преимущества:
* Простота использования. Каждая служба создана с использованием MVC ASP.NET Core с изначальной поддержкой создания веб-API.
* Безопасность. Хотя не всем службам требуется SSL, компания Tailspin может настроить использование этого протокола. 
* Управление версиями. Клиенты могут создаваться и тестироваться для конкретной версии веб-API.

Службы в приложении Survey используют [обратный прокси-сервер][reverse-proxy], реализуемый Service Fabric. Обратный прокси-сервер — это служба, которая работает на каждом узле в кластере Service Fabric, обеспечивая разрешение конечных точек и автоматический повтор операций, а также обработку других типов сбоев подключения. Чтобы использовать обратный прокси-сервер, каждый вызов API RESTful к определенной службе выполняется с помощью предопределенного порта обратного прокси-сервера.  Например, если задан порт **19081** обратного прокси-сервера, вызов *Tailspin.SurveyAnswerService* можно выполнить следующим образом:

```csharp
static SurveyAnswerService()
{
    httpClient = new HttpClient
    {
        BaseAddress = new Uri("http://localhost:19081/Tailspin/SurveyAnswerService/")
    };
}
```
Чтобы включить обратный прокси-сервер, укажите порт обратного прокси-сервера во время создания кластера Service Fabric. См. дополнительные сведения об [обратном прокси-сервере][reverse-proxy] в Azure Service Fabric.

## <a name="performance-considerations"></a>Рекомендации по производительности

Компания Tailspin создала службы ASP.NET Core *Tailspin.Web* и *Tailspin.Web.Surveys.Public* с помощью шаблонов Visual Studio. По умолчанию эти шаблоны включают ведение журнала в консоли. Ведение журнала в консоли может выполняться во время разработки и отладки, но при развертывании приложения в рабочей среде следует удалить все операции ведения журнала.

> [!NOTE]
> Дополнительные сведения о настройке мониторинга и диагностике приложений Service Fabric, выполняющихся в рабочей среде, см. в руководстве по [мониторингу и диагностике][monitoring-diagnostics] для Azure Service Fabric.

Например, следует закомментировать следующие строки в файле *startup.cs* для каждой службы веб-интерфейса:

```csharp
// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    //loggerFactory.AddConsole(Configuration.GetSection("Logging"));
    //loggerFactory.AddDebug();

    app.UseMvc();
}
```

> [!NOTE]
> Эти строки могут условно исключаться, когда Visual Studio настраивается на режим выпуска при публикации.

Наконец, когда компания Tailspin развертывает приложения Tailspin в рабочей среде, она переключает Visual Studio в режим **выпуска**.

## <a name="deployment-considerations"></a>Рекомендации по развертыванию

Оптимизированное приложение Surveys состоит из пяти служб без сохранения состояния и одной службы с отслеживанием состояния, поэтому планирование кластера ограничивается определением правильного размера виртуальной машины и числа узлов. В файле *applicationmanifest.xml*, который описывает кластер, компания Tailspin настраивает для атрибута *InstanceCount* тега *StatelessService* значение -1 для каждой из служб. Значение -1 указывает Service Fabric создавать экземпляр службы на каждом узле в кластере.

> [!NOTE]
> Службы с отслеживанием состояния требуют выполнения дополнительного шага — планирования правильного числа разделов и реплик для своих данных.

Компания Tailspin развертывает кластер с помощью портала Azure. Тип ресурса кластера Service Fabric развертывает всю необходимую инфраструктуру, включая масштабируемый набор виртуальных машин и подсистему балансировки нагрузки. Рекомендуемые размеры виртуальных машин отображаются на портале Azure во время процесса подготовки кластера Service Fabric. Обратите внимание: так как виртуальные машины развертываются в масштабируемом наборе виртуальных машин, они могут масштабироваться в соответствии с изменением пользовательской нагрузки.

> [!NOTE]
> Как было сказано ранее, в перенесенной версии приложения Surveys два веб-интерфейса были размещены в локальной среде с помощью ASP.NET Core и Kestrel в качестве веб-сервера. Хотя перенесенная версия приложения Surveys не использует обратный прокси-сервер, настоятельно рекомендуется использовать обратный прокси-сервер, например IIS, Nginx или Apache. Дополнительные сведения см. в руководстве по [реализации веб-сервера Kestrel в ASP.NET Core][kestrel-intro].
> В перенесенной версии приложения Surveys два веб-интерфейса размещаются в локальной среде с помощью [WebListener][weblistener] в качестве веб-сервера, следовательно, обратный веб-сервер не требуется.

## <a name="next-steps"></a>Дополнительная информация

Код приложения Surveys доступен на [GitHub][sample-code].

Если вы только начинаете работу с [Azure Service Fabric][service-fabric], сначала установите среду разработки, а затем скачайте последнюю версию [пакета SDK для Azure ][azure-sdk] и [пакета SDK для Azure Service Fabric][service-fabric-sdk]. Пакет SDK включает диспетчер кластеров OneBox, так что вы можете развертывать и тестировать приложение Surveys локально с возможностью полной отладки с помощью клавиши F5.

<!-- links -->
[azure-sdk]: https://azure.microsoft.com/downloads/archive-net-downloads/
[container-scenarios]: /azure/service-fabric/service-fabric-containers-overview
[kestrel]: https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel?tabs=aspnetcore2x
[kestrel-intro]: https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel?tabs=aspnetcore1x
[migrate-from-cloud-services]: migrate-from-cloud-services.md
[monitoring-diagnostics]: /azure/service-fabric/service-fabric-diagnostics-overview
[reliable-concurrent-queue]: /azure/service-fabric/service-fabric-reliable-services-reliable-concurrent-queue
[reverse-proxy]: /azure/service-fabric/service-fabric-reverseproxy
[sample-code]: https://github.com/mspnp/cloud-services-to-service-fabric/tree/master/servicefabric-phase-2
[service-fabric]: /azure/service-fabric/service-fabric-get-started
[service-fabric-sdk]: /azure/service-fabric/service-fabric-get-started
[weblistener]: https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener
