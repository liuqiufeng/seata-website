---
title: Nacos 2.0.1 + 1.4.2 Release正式发布，业界率先支持MCP-OVER-XDS协议
keywords: [2.0.1, 1.4.2]
description: Nacos 2.0.1 + 1.4.2 同时正式发布，业界率先支持MCP-OVER-XDS协议。
date: "2021-05-10"
category: announcement
---

# Nacos 2.0.1 + 1.4.2 Release正式发布，业界率先支持MCP-OVER-XDS协议

Nacos是阿里巴巴开源的服务发现与配置管理项目，本次同时发布两个版本：

1. 发布2.0.1版本，主要致力于支持MCP-OVER-XDS协议，解决Nacos与Istio数据服务同步问题。
1. 发布1.4.2版本，极大增强k8s环境中JRaft集群Leader选举的稳定性。

## Nacos 2.0.1

2.0.1主要变更为：

1. 在nacos-istio插件及模块中，支持MCP-OVER-XDS协议，解决Nacos与Istio数据服务同步问题。
2. 增强了Jraft协议在k8s环境中的Leader选举的稳定性。
3. 修复了频繁抛出Server is Down错误的问题。

具体变更为：

```
[#3484] Support ldap login.
[#4856] Support mcp over xds.
[#5137] Support service list add view subscriber.
[#5367] Support client encryption plugin for nacos 2.0.
[#5307] Push support config some parameters
[#5334] Fix Server is Down problem in k8s environment.
[#5361] Check isUseGrpcFeatures() when register instance using GRPC protocol.
[#5486] Refactor Distro Config as singleton and replace GlobalConfig.
[#5169] Fix instance beat run only by responsible server.
[#5175] Fix publishConfig lost type.
[#5178] Fix NPE when init server list failed.
[#5182] Fix throw NoSuchFieldException in ConfigController when service connect to nacos.
[#5204] Fix query error when pageNo is larger than service number.
[#5268] Fix subscriber app unknown
[#5327] Fix ThreadPool usage problem and add some monitor for distro.
[#5384] Fix the problem of unable to shutdown NacosConfigService.
[#5404] Fix frequently udp push for client 1.X.
[#5419] Fix Nacos 2.0 client auth may invalid for non public namespace.
[#5442] change state to UP when received status form old version server.
[#5096] Add unit tests in nacos 2.0.
[#5171][#5421][#5436][#5450][#5464] Fix IT for nacos 2.0.
```

## Nacos 1.4.2

1. 该版本优化了JRaft模块，与最新的nacos-k8s项目配合使用，极大增强集群选主的稳定性。
2. 另外，该版本了修复有关“Server is Down”问题的提示及众多1.4.1版本中的Bug。

具体变更为：

```
[#4452] Add config compare features.
[#4602] Add new way for export config.
[#4996] Make log level changeable for nacos-core module.
[#5367] Add pre-plugin in client for encrypting config.
[#3922] Method createServiceIfAbsent in ServiceManager require sync.
[#4274] skip master-select task when db.num is 1.
[#4753] Use SafeConstructor to parse yaml configuration.
[#4762] Naming health check thread num support user define it by self.
[#4770] Beta publish: change the way of select betaIps, from input to select.
[#4778] Make SecurityProxy.accessToken threadsafe in single writer multi reader.
[#4903] Add securuty hint for login page.
[#4917] Raft ops interface add auth.
[#4980] Log4J2NacosLogging.loadConfiguration() return directly When location is blank.
[#5010] Fix the usage of TemplateUtils.
[#5190] Add some hint log when login failed.
[#5234] Solve the problem that page can be edited while publishing-config request is processing.
[#5331] Fix the mouse hovers over the margin in a pointer state and cannot be clicked.
[#5350] Add hint and detail reason for consistence status Down.
[#5439] Support specified naming UDP push port for client.
[#5434] Optimize the ConfigType.isValidType method.
[#3779] Check groupName can't be empty.
[#4661] ConfigServletInner#doGetConfig code optimization.
[#3610] Fix the press F1 to full screen issue in new config page.
[#3876] Fix push empty service name.
[#4306] Fix search service by group error problem.
[#4573,#4629] Jraft leader status check error.
[#4672] Fix cloning configuration lost description.
[#4699] Fix metadata batch operation may delete instance problem.
[#4756] Fix config list sort and search problem.
[#4787] Losting member if parsing host throw UnknownHostException.
[#4806] Fix addListener method comment.
[#4829] Remove instance when distro and raft remove instances data.
[#4852] Fix main.js is too large problem.
[#4854] Modify Header to support Keys Ignore Case.
[#4898] Fix instance list page bug.
[#4925] Fix member list change will cover member status and metadata problem.
[#5078] Fix the problem of inconsistent results for querying subscriber list data multiple times.
[#5026] Fix MetricsHttpAgent metrics twice.
[#5018] Check group and dataId in groupKey.
[#5114] ConcurrentHashSet.java is not compatible with jdk1.6 or 1.7.
[#5253] Fix missing auth identity header error.
[#5291] Fix Beat task will stop when throw unexpected exception.
[#5301] Respond all kinds of collections for istio's request.
[#5351] Fix Consistence status can't switch to UP after Jraft election.
[#5390] Fix ip verify error.
[#5427] Fix NPE if Jraft leader is null in CurcuitFilter.
[#5437] Fix config beta feature will lost dump event problem.
[#5451] Fix the tag can't be removed problem.
[#4822][#4823][#4824][#4825][#4979][#5506] Fix dependency security problem.
[#5277] Subscriber list servername add required.
[#5380][#5418] Add and enhance unit test.
```

## About Nacos

Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。

Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。
