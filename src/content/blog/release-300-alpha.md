---
title: Nacos 3.0 Alpha 发布，在安全、泛用、云原生更进一步
keywords: [3.0.0, Alpha, 安全, 泛用, 云原生]
description: Nacos 3.0 Alpha 发布，在安全、泛用、云原生更进一步
date: "2024-12-05"
category: announcement
---
# Nacos 3.0 Alpha 发布，在安全、泛用、云原生更进一步

自2021年发布以来，Nacos 2.0 在社区的支持下已走过近三年，期间取得了诸多成就。在`高性能`与`易扩展性`方面，Nacos 2.0 取得了显著进展，同时在`易用性`和`安全性`上也不断提升。想了解更多详细信息，欢迎阅读我们之前发布的回顾文章： [Star 3w+，向更安全、更泛化、更云原生的 Nacos3.0 演进](./nacos-gvr7dx_awbbpb_erclsbfm2qqmwrx1.md)。

近期，我们欣喜地宣布 Nacos 3.0 的第一个版本 `Nacos 3.0-ALPHA` 已经发布。Nacos 3.0 的目标是在 2.0 的基础上，进一步优化`安全性`、`易用性`和`标准化`。同时，我们将引入更多功能，帮助用户在`分布式协调`、`AI大模型`、`云原生`等多种场景中更好地使用 Nacos，以提升其`广泛适应性`。

## Nacos 3.0 Alpha 主要更新亮点
在 Nacos 3.0-ALPHA 的发布中，我们将重心放在了提升`安全性`和`标准化`上，这一部分内容将 Nacos 3.0 的后续更新的基础。

### API 分类更为精细化
在Nacos 3.0之前，API主要分为两大类：`面向客户端和应用程序的OpenAPI`，以及`供运维人员管理使用的AdminAPI`。这种分类方式在实际使用场景中存在一定的矛盾，比如用于引擎间数据同步的API和控制台上进行数据检索的API，导致这部分API无法合理地对外开放和描述。同时，由于不同API对安全认证需求的差异，粗略将API分为两类也难以满足安全认证的多样化要求。

为了解决这些问题，Nacos 3.0 对API进行了更精细化的分类，具体包括：`提供给客户端和应用程序的OpenAPI`、`供运维人员和管理平面使用的AdminAPI`、`用于控制台UI的ConsoleAPI`，以及`引擎节点之间的InnerAPI`。这种新分类方式不仅针对不同使用场景提供了多维度的数据访问API，同时也为不同类型的API实施相应的安全认证机制奠定了基础。

![](/img/blog/3_0_0-release/3.0_API.svg)

例如，客户端和应用程序往往更关心特定服务和配置，因此OpenAPI仅提供有限范围（如单个配置）的数据访问。而控制台则需要展示所有相关的数据，因此ConsoleAPI提供了更广泛的list范围API。这种灵活的设计使得Nacos能够更好地满足不同用户和场景的需求。

### 按 API 类型默认启用安全认证
在Nacos 3.0之前，所有API都采用统一的安全认证方式，这对于`InnerAPI`和`AdminAPI`等主要用于内部调用的API来说并不适合。此外，由于所有API的安全认证采用同一个开关，导致在客户端和应用程序完成身份设置之前无法开启安全认证，从而带来了更高的安全风险。

为了解决这些问题，Nacos 3.0将根据不同API类型默认采用不同的安全认证策略：对于`InnerAPI`和`AdminAPI`，将默认启用`ServerIdentity`进行身份验证；对于`ConsoleAPI`，将默认启用用户名和密码的身份及权限校验；而对于客户端和应用程序使用的`OpenAPI`，则默认与Nacos 2.0保持一致，即默认不开启安全认证，需要用户自行查找并启用。这样做不仅提升了Nacos集群的数据安全性，还增加了用户在可信环境中的易用性，以及在升级启用安全认证过渡期间的稳定性。

![](/img/blog/3_0_0-release/3.0_API_Auth.svg)

### 优化默认命名空间
在Nacos中，命名空间ID是命名空间的唯一标识符。然而，许多用户在使用默认命名空间`public`时，错误地将名称`“public”`作为ID配置到应用程序中，这导致了一些问题。同时，其他正常使用此命名空间的用户对默认命名空间`public`的ID为`空字符串''`感到困惑。这种困惑源于服务发现模块可以使用`public`作为命名空间ID并将其视为默认处理方式，而在配置管理模块中却并非如此。简而言之，默认命名空间ID的处理方式存在不一致性。

此外，自1.2.0版本起，auth插件依赖于命名空间ID，而这种处理差异也引发了默认命名空间的权限问题。在适应默认命名空间''方面，数据源插件也遇到了一些困难。相关问题已在以下ISSUE中被讨论：[#3525](https://github.com/alibaba/nacos/issues/3525)、[#8774](https://github.com/alibaba/nacos/issues/8774)、[#9773](https://github.com/alibaba/nacos/issues/9773)、[#9783](https://github.com/alibaba/nacos/issues/9783)等。

为了解决默认命名空间ID的使用问题，Nacos 3.0计划对默认命名空间的ID进行调整。根据社区讨论的[ISSUE#9846](https://github.com/alibaba/nacos/issues/9846)，默认命名空间的ID将被修改为`public`，与其名称相同。在访问API时，如果未传入命名空间ID或仍然传入`空字符串''`，Nacos 3.0将自动将其匹配为`public`以进行后续处理，从而兼容旧客户端的访问请求。

![](/img/blog/3_0_0-release/3.0_namespace.svg)

> 需要注意的是，Nacos 3.0 Alpha版本在数据存储的平滑迁移和适配方面尚未进行处理，因此进行直接升级会导致配置数据无法获取，并且目前无法实现平滑升级。不过，Nacos 3.0的正式版本将会支持平滑升级。

### 支持先进的 xDS 协议
xDS（Extended Discovery Service）是一组由Envoy proxy团队提出的协议，广泛应用于服务网格中，旨在服务发现和配置管理，以支持现代微服务架构下的动态配置。随着服务网格概念的普及，xDS协议逐渐获得了社区的认可。Nacos作为微服务生态体系中的注册与配置中心，通过标准化协议来满足服务网格的功能需求，成为云原生化的核心要求之一。

在Nacos 2.0版本中，Nacos通过Istio的MCP协议获取服务数据，并将其转换为xDS协议数据。然而，这一过程依赖于中间组件Istio，这导致系统的复杂性和稳定性面临风险。而在Nacos 3.0版本中，Nacos直接支持xDS协议中的`EDS`、`LDS`、`RDS`和`CDS`协议，显著降低了对Istio组件的依赖，提高了系统的易用性和稳定性。

## Nacos 3.0 即将推出的新功能
基于Nacos 3.0-Alpha版本所提供的基础功能，在Nacos3.0 正式版中计划进一步从架构上提升提升`安全性`和`标准化`能力。

### 全新 Admin API 的推出
在Nacos的早期版本中，AdminAPI主要面向运维人员，专注于Nacos集群的维护操作。由于当时的设计场景多以人为本地调用为主，因此AdminAPI的定义较为随意，导致其安全性和标准化程度不足。这使得后续的控制台在复用AdminAPI时面临困难，同时也给希望开发自定义控制台或构建管理平台的开发者带来了挑战。

为了解决这些问题，Nacos 3.0正式版将对AdminAPI进行全面的重新设计。我们将规范API的请求体、返回体和错误码等标准，提升整体的`标准化`水平。同时，默认启用并优化AdminAPI的身份验证，以增强`安全性`。此外，我们将提供`Maintainer-SDK`，以便希望开发自定义管理程序的开发者方便使用。这些改进将为Nacos控制台与引擎的灵活拆分和部署奠定坚实基础。

![](/img/blog/3_0_0-release/3.0_admin_API.svg)

### 控制台与引擎的灵活拆合部署
在之前的Nacos版本中，为了方便用户的部署和使用，控制台与引擎程序一直合并部署，且共用同一个端口。这种方式虽然增强了使用的便利性，但也带来了一些安全风险。此外，由于控制台和引擎在使用场景上存在差异，它们对于开放网络访问范围及安全认证需求的预期也不尽相同。基于此，Nacos计划在新版本中对控制台和引擎的部署架构进行较大调整。

在Nacos 3.0中，控制台将独立在一个Web容器中运行，允许用户设定独立的访问端口。这一改变使得Nacos集群的运维人员能够更灵活地配置网络访问控制列表（ACL），例如，仅将控制台端口开放给办公网络。同时，配合控制台默认启用的安全认证，这将显著提高Nacos的`安全性`。此外，独立的Web容器还将与全新的Admin API相结合，实现控制台和引擎节点的灵活拆分部署，使得它们能够在不同节点上运行，进一步增强`安全性`。

![](/img/blog/3_0_0-release/3.0_deploy.svg)

### 引入分布式锁支持
Nacos社区向用户征集了他们对Nacos 3.0的期望功能，其中支持分布式锁的需求是呼声最高的功能之一。分布式锁是一项在分布式应用中常用的功能，目前大多数实现依赖于Zookeeper或Redis等产品。许多用户已经将Nacos替换为Zookeeper来进行服务和配置管理，但由于Nacos尚未支持分布式锁，用户仍需额外运维Zookeeper集群，增加了系统的复杂性。

因此，Nacos 3.0正式版本计划引入分布式锁的实验性功能，以满足部分用户对轻量级分布式锁的需求。这一功能的推出将帮助用户减少对额外系统的依赖，从而简化微服务应用架构，拓展Nacos的使用场景。

### Spring Boot 3 和原生启动的支持
Spring社区已经停止了Spring Boot2的支持，同时最新的Spring Boot3支持了Java 原生启动的支持；考虑到Spring Boot 3要求将JDK升级至17及以上版本，这可能会导致许多用户在升级时遇到阻碍，因此Nacos 2.X版本依然基于JDK 8和Spring Boot 2，并未升级至Spring Boot 3。

然而，随着时间的推移，失去支持的Spring Boot 2将会产生越来越多的安全漏洞，这将间接降低Nacos的安全性。因此，Nacos计划在3.0版本中对JDK和Spring Boot进行升级。这一升级不仅能确保遵循社区的支持，及时修复安全漏洞，而且还能利用Java原生启动来提升性能。

## Nacos 3.X 发展蓝图
在Nacos 3.0的发展蓝图中，我们将继续致力于提升易用性与普适性，以满足用户日益增长的需求。

在引擎自身方面，新版本计划引入了`服务与配置的模糊订阅功能`，使用户能够更灵活地管理服务和配置，简化在网关应用中服务发现和配置订阅的操作流程。此外，我们计划`支持DNS协议`，以进一步拓展Nacos在支持较弱编程语言场景中的适用性。另外对于`服务健康检查体系`，我们将优化相关机制，通过将健康检查与服务类型解耦，提供更多关于服务可用性的判断依据，这将使微服务之间的流量调用更加灵活，同时确保系统的稳定运行。最后对于社区中已经比较成熟的插件，我们会将其纳入Nacos的主干仓库中进行维护，诸如`PostgreSQL插件`、`AES配置加密插件`等，让这些插件在后续版本中随引擎一起发布、不需要再独立构建引入。

在生态建设方面，我们将通过Nacos Controller的快速迭代，实现`Kubernetes服务与配置的同步管理`，从而使云原生环境下的使用变得更加便捷。此外，我们将积极探索`多语言生态与AI大模型的集成`，通过支持多语言应用框架以及Spring AI和其他AI大模型开发框架的动态prompt发现和资源发现，为用户提供更加丰富的功能选择与应用场景，努力构建一个高效、灵活的分布式协调平台。

![](/img/blog/3_0_0-release/3.0_roadMap.svg)

## 关于Nacos3.0的一些投票和讨论
Nacos是一个开放的社区，任何社区参与者和使用者都可以参与Nacos发展的讨论，提供自己的想法和建议。由于Nacos3.0的改动较大，因此社区也发起了一些投票和讨论，希望大家能够积极参与，帮助Nacos社区更好的进行规划。

### 1.X正式EoL(End of Life)
Nacos 2.X 版本经过了近3年的演进和沉淀，无论是从性能、稳定、安全的角度，都比1.X版本优秀太多；而且1.X版本实际上也已经进入了尾声维护阶段（仅修复严重Bug和安全漏洞）近2年，我们希望在Nacos 3.0正式发版之际，正式归档和EoL Nacos 的 1.X版本（不再进行更新）。希望征询社区的意见，大家可以到 [ISSUE#12921](https://github.com/alibaba/nacos/issues/12921) 中进行投票和讨论。

### 3.X 不再支持1.X的客户端
Nacos 的2.X版本 兼容大多数1.X的客户端，这主要是考虑到业务应用升级客户端版本较为谨慎，时间周期较长；随着Nacos 2.X 版本经过了近3年的演进和沉淀，主要的上游应用框架基本都升级到了Nacos 2.X的客户端，因此我们希望在Nacos 3.0或未来的某个3.X版本中，不再支持1.X的客户端，减少Nacos冗余代码。希望征询社区的意见，大家可以到 [ISSUE#12922](https://github.com/alibaba/nacos/issues/12922) 中进行投票和讨论。

### spring boot3 + jdk 17升级
正如前文所提及的，由于spring boot2已经彻底停止了维护， nacos3 升级到spring boot 3势在必行，对应的JDK版本也必须升级到17。而升级JDK版本可能是一个比较大的变动，部分使用者可能由于各种考量无法接受JDK版本的升级。因此我们希望通过社区投票，再决定Nacos 3.0是否升级到JDK17，欢迎大家到[ISSUE#12923](https://github.com/alibaba/nacos/issues/12923)中进行投票和讨论。

除了上述的3个投票，Nacos社区还有更多关于Nacos3.0改动的讨论，也欢迎大家积极参与，比如：[ISSUE#9129](https://github.com/alibaba/nacos/issues/9129)、[ISSUE#9846](https://github.com/alibaba/nacos/issues/9846)等。

## About Nacos

Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。

Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。

最后欢迎大家扫码加入Nacos社区群

![](https://img.alicdn.com/imgextra/i4/O1CN01qOYVoX1DsAKLKzfGh_!!6000000000271-49-tps-1080-1177.webp)
