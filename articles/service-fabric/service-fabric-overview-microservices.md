<properties
   pageTitle="了解微服务 | Microsoft Azure"
   description="概述为何使用微服务方法构建云应用程序对于开发现代应用程序非常重要，以及 Azure Service Fabric 如何提供一个平台用于实现此目的"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="bscholl"/>

<tags
   ms.service="service-fabric"
   ms.date="11/18/2015"
   wacn.date=""/>

# 为什么通过微服务的方法构建应用程序？
作为软件开发人员，我们已知道思考如何将应用程序因数分解成组件部分。这是对象导向、软件抽象和组件化的中心模式。当今，这种因数分解往往以共享库和技术层之间的类与接口呈现（通常通过一种分层方法），有后端存储、中间层业务逻辑和前端 UI。过去几年来的变化是身为开发人员的我们，开始为业务驱动的云构建分布式应用程序。

不断变化的业务需求包括：

- 需要大规模创建和操作一项服务，以吸引更大的客户群 -- 例如，开拓新的地理区域，或不需要在客户地点进行部署。
- 更快速地提供特性与功能，灵活应对客户的需求。
- 提高资源利用率以降低成本。

这些业务要求影响我们 *如何* 构建应用程序。

## 单一式设计方法与微服务设计方法
所有应用程序会随着时间而发展。成功的应用程序因为有实用性而发展。失败的应用程序不会发展，最终将被取代。问题在于现在你对要求了解多少，以及未来有何变化？ 例如，如果你要为某个部门构建报告应用程序，并确定该应用程序将保留于公司范围内，而且报告只是短暂存在，那么，选择的方法将不同于构建服务向数千万个客户传送视频内容的方式。在已知后来可以重新设计应用程序的情况下，有时向外寻求概念证明才是驱动因素。过度设计永不使用的功能并没有太大意义。这就是通常所谓的工程取舍。另一方面，公司谈论构建云时都期望成长和使用量。问题在于成长和规模不可预测。我们想要能够快速创建原型，同时还要了解我们正在通往未来成功的路上。这是简练的启动方法：构建、测量、学习、迭代。

在客户端-服务器时代，我们倾向专注于构建分层式应用程序，每一层采用特定的技术。已针对这些方法派生出“单一式”应用程序一词。接口通常存在于各层之间，而在一层内的各组件之间采用更紧密耦合的设计。开发人员设计并分解类，将类编译成库，然后链接起来成为一些 exe 和 dll。这类单一式设计方法有一些优点。设计较简单，组件之间通常通过 IPC，因此调用更快。此外，每个人都只测试单一产品，人力资源运用更有效率。缺点是应用程序在分层之内紧密耦合，无法缩放单个组件。如果需要执行修复或升级，则必须等待其他人完成其测试，因此更难以发挥灵活性。

微服务解决了这些缺点，更密切配合上述业务要求，但它们本身也都有优缺点。微服务的优点是通常各自封装较为简单的业务功能，可独立扩展或缩减、测试、部署和管理。微服务方法的一个重要优点是团队倾向于以业务方案为导向，而不是以分层方法建议的技术为导向。实际上，这意味着较小的团队可以采用他们选择的任何技术，根据客户方案来开发微服务。换句话说，组织不需要为了维护单一性而将技术标准化。此外，拥有服务的单个团队可以根据团队的专业知识，或什么是最适合服务尝试解决的问题，各自发挥所长。当然，实际上，最好有一组建议的技术，例如特定的 NoSQL 存储或 Web 应用程序框架。
微服务的缺点包括需要管理越来越多的独立实体、处理更复杂的部署和版本控制、让微服务之间拥有更多的网络流量，以及相应的网络延迟。经过大量的讨论之后，粒度极高的服务是性能瓶颈的解决良方。如果没有工具帮助查看这些依赖性，很难“看到”整个系统。最后，标准就是让微服务方法奏效的关键、在通信方式上达成共识，以及只在乎需要从服务获得什么，而不是僵化的约定。必须在设计的初期定义这些约定，因为之后服务将各自独立更新。在设计微服务方法时出现的另一个描述是“精细 SOA”。


***简而言之，微服务设计方法是低耦合的服务联合，各自独立更改，并达成一致的通信标准。***


随着越来越多云应用程序的生成，更多人会发现这种将整体应用程序分解成独立、方案焦点式服务的做法，在长期上是较好的方法。
## 应用程序开发方法的比较

![Service Fabric 平台应用程序开发][Image1]

1. 单一式应用包含域特定的功能，通常按照功能层来划分，例如 Web、业务和数据。

2. 单一式应用可通过复制到多个服务器/VM/容器上进行扩展。

3. 微服务应用程序将单个功能分隔成单个较小的服务。

4. 这种方法可通过独立部署每个服务而扩展，跨服务器/VM/容器创建这些服务的实例。


使用微服务方法进行设计并非所有项目的灵丹妙药，但确实更符合前面所述的业务目标。另外，如果知道以后有机会将代码修改为微服务设计，也许可以接受从使用单一式方法开始。较常见的情况是一开始设计单一式应用，可以从需要更高缩放性或灵活性的功能领域开始，分阶段缓慢地将此框架分解。

总而言之，微服务方法是以部署在群集各处的容器中执行的许多较小服务来组成应用程序。每个服务都是由专门负责某个方案的较小团队开发，且每个服务各自独立进行测试、控制版本、部署和缩放，因此应用程序在整体上可以得到发展。

## 什么是微服务？

微服务有不同的定义，搜索 Internet 可以找到许多很好的资源，各有其观点和定义。但在微服务的以下大部分特性上，已广泛达成共识：

- 它们封装客户方案或业务方案。你要解决什么问题？
- 它们是由小型工程团队开发。
- 它们可以使用任何编程语言编写，以及使用任何框架。
- 它们是由独立控制版本、部署及缩放的代码和（可选）状态组成。
- 它们通过定义完善的接口和协议来与其他微服务交互。
- 它们具有可用来解析位置的唯一名称 (URL)。
- 它们在出现故障时可保持一致且可用。

概括而言：

***微服务应用程序是由独立控制版本和可缩放的、以客户为中心的服务组成，这些服务通过标准协议和定义完善的接口彼此通信。***


我们在上一部分已介绍了前两点，接下来进一步澄清其他各要点。

### 可以使用任何编程语言编写并使用任何框架
作为开发人员，我们应该根据本身的技能或服务需求，自由选择任何所需的语言或框架。在某些服务中，你可能认为 C++ 的性能优点胜于一切，而在其他服务中，C# 或 Java 的简易管理开发可能才是最重要的。在某些情况下，可能需要使用特定的第三方库、数据存储技术，或向客户端公开服务的方式。

选择技术之后，接下来的课题就是服务的操作或生命周期管理和缩放。

### 允许独立控制版本、部署及缩放的代码和状态  

无论选择何种方式来编写微服务，都应该针对代码和（可选）状态单独部署、升级和缩放每个微服务。这确实是难以解决的问题之一，因为这涉及到选择的技术，以及在缩放方面，需要了解如何分区（或分片）代码和状态。当代码和状态使用不同的技术时（目前的普遍情况），微服务的部署脚本必须能够妥善缩放两者。这也关乎到灵活性和弹性，以便你可以升级某些微服务，而无需一次性全部升级。
暂时回到单一式和微服务方法的比较，下图显示了状态存储方法的差异。

#### 应用程序样式之间的状态存储
![Service Fabric 平台状态存储][Image2]

***左侧是单一式方法，有单一数据库和多层的特定技术。***

***右边是微服务方法，图中显示互连的微服务，其中状态通常以微服务为范围，并使用各种技术。***

在单一式方法中，应用程序通常使用单一数据库。优点是这是单一位置，很容易部署。每个组件可以通过单个表来存储其状态。困难之处是团队必须严格区分状态，无可避免地需要直接将新的列添加到现有客户表、在表之间执行联接，并且通常对存储层形成依赖性。发生这种情况后，你无法缩放各个组件。在微服务方法中，每个服务都管理并存储自己的状态，这意味着它将负责同时缩放代码和状态，以满足服务的需求。在需要对应用程序的数据创建任何视图或查询时，就能看出缺点，因为必须跨这些不同的状态存储进行查询。为了解决此问题，通常由一个独立的微服务构建一个跨许多微服务的视图。如果需要对数据执行多个即席查询，每个微服务应该考虑将其数据写入数据仓库服务以供脱机分析。


版本控制特定于部署的微服务版本。它也是为了能够推出和并列运行多个不同版本所不可或缺的。当较新版的微服务在升级期间失败，因而需要回滚到旧版时，版本控制可以解决这种情况。版本控制的另一种情况是执行 A/B 式测试，其中不同的用户将体验到不同版本的服务。例如，在更广泛推出新功能之前，通常先对一组特定的客户升级微服务以测试新功能。在微服务的生命周期管理之后，便可以在微服务之间的通信。


### 通过定义完善的接口和协议来与其他微服务交互

本主题的内容较少，请阅读过去 10 年来发布的大量服务导向型框架文献，其中有许多都以通信模式为主题。一般而言，现在可归结为使用 REST 方法，并配合 HTTP 与 TCP 协议及 XML 或 JSON 作为序列化格式。从接口观点来看，这涉及到采用 Web 设计方法。但你仍可以使用二进制协议或自己的数据格式。只是公开的微服务较难使用，因此要有心理准备。

### 具有可用来解析位置的唯一名称 (URL)

记得我们一直在说，微服务与 Web 有点类似吗？ 就像 Web 一样，微服务无论在何处运行，都必须可寻址。如果你还在考虑要在哪一台计算机上运行特定的微服务，很快就会陷入困境。就像 DNS 解析特定计算机的特定 URL 一样，微服务需要有唯一的名称来发现它目前所在的位置。微服务需要有可寻址的名称才能独立于它们运行所在的基础结构之外。当然，这意味着服务的部署和发现方式之间互相影响，因为需要有服务注册表。同样地，发生计算机故障时间与微服务状况之间也需要有互相影响，注册表服务才能指出它现在运行的位置。接下来的主题：复原能力和一致性。

### 在出现故障时可保持一致且可用

处理意外的故障是最难解决的问题之一，特别是在分布式系统中。开发人员编写的代码大多是在处理异常，这也是测试时花费最多时间的地方。但问题比编写代码来处理故障更复杂。当运行微服务的计算机发生故障时，该怎么办？ 你不仅需要检测这种微服务故障（本身就是棘手的问题），还需要设法重新启动微服务。微服务必须能够从故障复原，出于可用性理由，通常还必须能够在另一台计算机重新启动。这也涉及到代表微服务存储了何种状态、可从何处恢复此状态，以及是否能够成功重新启动它。换句话说，需要能够恢复计算（也就是重新启动进程），以及状态或数据的复原能力（不丢失任何数据，数据保持一致）。

在其他情况下，复原能力的问题更难处理，例如应用程序升级期间失败。在配合部署系统一起运行时，微服务不仅需要恢复。它还需要确定是要继续升级到更新版本，还是回滚到旧版以维持一致的状态。需要考虑一些问题，例如，是否有足够的计算机可用于继续升级，以及如何恢复旧版的微服务。需要微服务发出运行状况信息才能做出这些决定。

### 报告运行状况和诊断

微服务必须报告其运行状况和诊断，这一点看似明显，但却经常被忽视。否则，难以从操作观点上深入了解。面临的难题是查看一组独立服务的诊断事件之间的相关性（每一个服务独立记录），并修正计算机时钟偏差以识别事件顺序。同样地，通过议定的协议和数据格式来与微服务交互时，需要将运行状况和诊断事件的记录方式标准化，这些事件最终将写入可供查询和查看的事件存储。在微服务方法中，关键在于不同团队同意采用单一日志记录格式，因为需要有一致的方法来查看整个应用程序中的诊断事件。

运行状况与诊断不同。运行状况是指微服务报告其当前状态，以便采取措施。最明显的情况是使用升级和部署机制来保持可用性。例如，当前服务可能由于进程崩溃或计算机重新启动而状况不正常，但仍可运行。不应该执行升级而让情况恶化。最好是先进行调查，或让微服务有时间恢复。因此，微服务的运行状况事件可让我们做出明智的决策，实际上有助于创建自我修复的服务。

## Service Fabric 作为微服务平台

Microsoft 从提供盒装产品（通常是单一式）转换到提供服务后，Azure Service Fabric 横空问世。Service Fabric 主要是由构建和操作大型服务的经验来推动，例如 Azure SQL 数据库、DocumentDB 和其他核心 Azure 服务。我们完全接近缩放性、灵活性和独立团队的业务需求，并让平台随着越来越多的服务采用它而得到发展。重要的是，Service Fabric 必须在任何地方运行，不仅在 Azure 中，还有在独立的 Windows Server 部署中。

***Service Fabric 旨在解决构建和执行服务方面的难题（例如失败与升级），并有效地利用基础结构资源，使团队可以使用微服务方法来解决业务问题。***

Service Fabric 提供两个广泛的领域，帮助你使用微服务方法来构建应用程序：

- 由一套系统服务组成的平台，这些系统服务负责部署、升级、检测和重新启动失败的服务、发现当前运行服务的位置、状态管理、运行状况监视等等。这些系统服务实际上具备上述微服务的许多特性。

-  编程 API 或框架，帮助你将应用程序构建成微服务。提供的编程 API 称为 [Reliable Actors 和 Reliable Services](/documentation/articles/service-fabric-choose-framework)。当然，你可以使用所选的任何代码来构建微服务。但使用这些 API 不仅可让作业变得更简单，也能更深入地与平台集成。例如，你可以获取运行状况和诊断信息，或利用内置的高可用性。

***Service Fabric 不限制构建服务的方式，你可以使用任何技术。
不过，它提供可让你轻松构建微服务的内置编程 API。***

### 微服务适合我的应用程序吗？

也许。根据我们的经验，随着 Microsoft 中越来越多团队被告知出于商业理由应该以云为目标来构建，有许多团队都了解到采用类似微服务的方法所带来的优点。例如，多年以来，必应在搜索方面就一直这样做。这对于其他团队而言相当新颖。他们发现需要解决困难的问题，但这并非他们的强项。这就是为什么 Service Fabric 受到重视而成为构建服务的最佳技术。

Service Fabric 的目标是将使用微服务方法构建应用程序时的复杂性降低，使你不需要经历许多耗费成本的重新设计工作。从小规模开始，需要时缩放，淘汰服务，添加新服务，随客户用法而改进。我们也知道，为了让微服务更易于为大部分开发人员所接受，事实上还有许多其他尚待解决的问题。容器和执行组件编程模型都是朝此目标前进的一小步，我们确信将涌现出更多的创新来轻松实现目标。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 后续步骤

* 更多相关信息：
	* [Service Fabric 概述](/documentation/articles/service-fabric-overview)
	* [技术概述](/documentation/articles/service-fabric-technical-overview)
* 设置 Service Fabric [开发环境](/documentation/articles/service-fabric-get-started)
* 为你的服务选择[编程模型框架](/documentation/articles/service-fabric-choose-framework)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png

<!---HONumber=Mooncake_0314_2016-->