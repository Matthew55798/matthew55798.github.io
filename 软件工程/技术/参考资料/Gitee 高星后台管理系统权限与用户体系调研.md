# Gitee 高星后台管理系统权限与用户体系调研

> 调研日期：2026-06-30
> 调研范围：17 个 Gitee 上较高关注度、且至少带后端的后台管理/权限/IAM 类项目。
> 调研边界：优先阅读 Gitee README、仓库简介、README 链出的官方文档或官网；README 信息足够时不阅读源码。本文不凭代码目录外推架构能力；官方资料没有明确说明的能力，统一标为“未看到明确证据”。

## 调研结论索引

这 17 个项目都不是纯前端项目，至少能从 README、仓库说明或官方文档中看到后端、服务端或完整 IAM 服务的明确证据。

从“权限与用户体系”的写作主线看，可以先按下面四类阅读：

| 主线 | 重点项目 | 可以观察的内容 |
|---|---|---|
| 基础后台权限 | RuoYi、RuoYi-Vue、renren-fast、mall、ELADMIN、Guns、SmartAdmin | 用户、角色、菜单、按钮、部门、岗位、在线用户、登录日志等后台管理系统的常规权限模型。 |
| 企业级权限 | ruoyi-vue-pro、JeecgBoot、JeeSite/JeeSite5、SpringBlade、RuoYi-Vue-Plus、lamp-cloud | RBAC、数据权限、多租户、组织机构、岗位、部门、表单权限、租户隔离等更完整的企业应用能力。 |
| 统一登录网关鉴权 | RuoYi-Cloud、pig、SpringBlade、lamp-cloud、MaxKey | 微服务网关、认证中心、OAuth2/OIDC、SSO、JWT、Token、统一认证等能力。 |
| 数据权限治理闭环 | ruoyi-vue-pro、RuoYi 系列、JeecgBoot、JeeSite5、RuoYi-Vue-Plus、ELADMIN、SmartAdmin、lamp-cloud | 数据范围、部门数据权限、行级数据权限、字段权限、MyBatis 插件或注解式数据范围控制。 |

上表是基于官方资料的阅读归纳，不代表项目代码实现质量排序。

## 项目速览

| 项目 | Gitee | 后端证据 | 权限与用户体系要点 | 数据权限/多租户证据 |
|---|---|---|---|---|
| ruoyi-vue-pro | [zhijiantianya/ruoyi-vue-pro](https://gitee.com/zhijiantianya/ruoyi-vue-pro) | 官方简介称其基于 Spring Boot、MyBatis Plus、Vue、Element、UniApp。 | 官方文档明确支持 RBAC 动态权限、Spring Security、Token、Redis、SSO、动态权限菜单、按钮级权限。 | 官方简介和功能列表明确支持数据权限、SaaS 多租户。来源：[简介](https://doc.iocoder.cn/intro/)、[功能列表](https://doc.iocoder.cn/feature/)、[技术选型](https://doc.iocoder.cn/technology/)。 |
| RuoYi | [y_project/RuoYi](https://gitee.com/y_project/RuoYi) | 官方文档称其基于 Spring Boot、Apache Shiro、MyBatis、Thymeleaf、Bootstrap。 | 内置用户、部门、岗位、菜单、角色；支持菜单及按钮授权；认证授权核心是 Shiro。 | 官方文档明确包含数据权限；未看到多租户明确证据。来源：[快速了解](https://doc.ruoyi.vip/ruoyi/document/kslj.html)。 |
| RuoYi-Vue | [y_project/RuoYi-Vue](https://gitee.com/y_project/RuoYi-Vue) | 仓库简介称其基于 Spring Boot、Spring Security、JWT、Vue、Element。 | 内置用户、部门、岗位、菜单、角色；支持动态权限菜单、多方式权限控制。 | 官方文档明确包含数据权限；未看到多租户明确证据。来源：[快速了解](https://doc.ruoyi.vip/ruoyi-vue/document/kslj.html)、[环境部署](https://doc.ruoyi.vip/ruoyi-vue/document/hjbs.html)。 |
| RuoYi-Cloud | [y_project/RuoYi-Cloud](https://gitee.com/y_project/RuoYi-Cloud) | README 列出 `ruoyi-gateway`、`ruoyi-auth`、`ruoyi-api`、`ruoyi-modules`、`ruoyi-system` 等模块。 | 官方文档称微服务版是 Spring Cloud & Alibaba 微服务权限管理系统；README 有认证中心、网关、用户、角色、菜单等模块。 | README 提到部门树结构支持数据权限、角色按机构划分数据范围；未看到多租户明确证据。来源：[RuoYi 官方文档](https://doc.ruoyi.vip/)、[Gitee README](https://gitee.com/y_project/RuoYi-Cloud)。 |
| pig | [log4j/pig](https://gitee.com/log4j/pig) | README 列出 `pig-auth`、`pig-gateway`、`pig-upms`、`pig-visual`、`pig-boot`，并说明支持微服务和单体架构。 | README 定位为 RBAC 企业级快速开发平台；认证中心基于 Spring Authorization Server。 | README 明确开源版移除了多租户、数据权限等商业扩展模块。来源：[Gitee README](https://gitee.com/log4j/pig)。 |
| SpringBlade | [smallc/SpringBlade](https://gitee.com/smallc/SpringBlade) | README 有后端 Gitee 地址、后端业务模块和 Spring Cloud 技术栈说明。 | README 写到借鉴 OAuth2 自研多终端认证系统、借鉴 Security 自研 Secure 模块、JWT Token 认证；官网写到“五维权限管理”。 | README 明确 SaaS 多租户；官网说明多租户隔离模式，且“五维权限管理”包含数据维度。来源：[Gitee README](https://gitee.com/smallc/SpringBlade)、[官网](https://bladex.cn/)。 |
| JeeSite | [thinkgem/jeesite](https://gitee.com/thinkgem/jeesite) | 当前 README 标题偏 Vue3 前端源码，但平台介绍说明后端基于 Spring Boot、Shiro、MyBatis，并要求安装 JeeSite v5.x 后台服务。 | README 明确组织机构、用户、角色、岗位、管理员、权限审计、菜单及按钮权限。 | README 明确数据权限；扩展能力包含单点登录、第三方登录、统一认证服务，并提到可扩展 SaaS 架构。来源：[Gitee README](https://gitee.com/thinkgem/jeesite)。 |
| JeeSite V5.x | [thinkgem/jeesite5](https://gitee.com/thinkgem/jeesite5) | README 写明主框架 Spring Boot、Spring Framework、Apache Shiro、J2Cache，持久层 MyBatis、Druid 等。 | README 明确组织机构、用户、角色、管理员、权限审计、菜单及按钮权限；官方 Shiro 文档说明实现用户认证、用户授权、菜单授权、按钮授权。 | README 明确数据权限；官方数据权限文档说明支持行级数据权限和字段权限；更多文档列出 SaaS 多租户架构。来源：[Gitee README](https://gitee.com/thinkgem/jeesite5)、[Shiro 权限文档](https://jeesite.com/docs/permi-shiro/)、[数据权限文档](https://jeesite.com/docs/service-datascope/)。 |
| JeecgBoot | [jeecg/JeecgBoot](https://gitee.com/jeecg/JeecgBoot) | README 写明 `jeecg-boot` 是后端 Java 源码项目，技术栈包含 SpringBoot3、Shiro、MyBatis、SpringCloudAlibaba。 | README 明确颗粒化权限控制，支持按钮权限和数据权限；系统管理含用户、角色、菜单、权限设置、表单权限、部门、多租户管理。 | 官方文档说明数据权限用于“让不同的人看不同的数据”；多租户文档说明通过租户 ID 进行数据隔离。来源：[Gitee README](https://gitee.com/jeecg/JeecgBoot)、[官方文档](https://help.jeecg.com/java/)、[数据权限](https://help.jeecg.com/java/system/dataauth/use/)、[多租户](https://help.jeecg.com/java/saas/open)。 |
| RuoYi-Vue-Plus | [dromara/RuoYi-Vue-Plus](https://gitee.com/dromara/RuoYi-Vue-Plus) | README 有后端项目结构、Web 容器、ORM、权限认证等说明，前端项目另链到 plus-ui。 | README 写明 Sa-Token、JWT、权限注解、角色校验、权限校验、二级认证、HttpBasic、JustAuth 第三方鉴权。 | README 明确面向分布式集群与多租户，数据权限采用 MyBatis-Plus 插件，并列出租户管理、租户套餐等业务表。来源：[Gitee README](https://gitee.com/dromara/RuoYi-Vue-Plus)。 |
| Guns | [stylefeng/guns](https://gitee.com/stylefeng/guns) | README 分前端启动和后端启动，后端通过 `ProjectStartApplication` 启动。 | README 功能列表包含用户管理、机构管理、应用管理、角色管理、菜单管理、资源查看、在线用户、登录日志；插件列表包含 jwt 插件。 | README 更新日志提到数据范围升级、`@DataScope` 注解、多机构切换、角色区分和权限绑定；未看到多租户明确证据。来源：[Gitee README](https://gitee.com/stylefeng/guns)。 |
| SmartAdmin | [lab1024/smart-admin](https://gitee.com/lab1024/smart-admin) | README 明确后端提供 Java8 + SpringBoot2.x 和 Java17 + SpringBoot3.x 双版本。 | README 写到 Sa-Token、登录限制、双因子登录、密码复杂度、错误次数锁定、员工、部门、角色、权限、菜单。 | 官方数据范围文档说明用自定义 MyBatis 插件在 SQL 查询时动态拼接 SQL；未看到多租户明确证据。来源：[Gitee README](https://gitee.com/lab1024/smart-admin)、[数据范围文档](https://www.smartadmin.vip/views/doc/back/DataScope.html)。 |
| ELADMIN | [elunez/eladmin](https://gitee.com/elunez/eladmin) | 官方文档称其为前后端分离后台管理系统，技术栈含 Spring Boot、JPA/MyBatis-Plus、Spring Security、Redis、Vue。 | 官方权限文档说明用户-角色-菜单模型，认证使用 Spring Security + JWT Token，并支持接口权限、匿名接口。 | 官方数据权限文档说明基于部门控制，支持全部、本级、自定义；未看到多租户明确证据。来源：[官方介绍](https://eladmin.vip/pages/010101/)、[权限文档](https://eladmin.vip/pages/010202/)、[数据权限](https://eladmin.vip/pages/010207/)。 |
| lamp-cloud | [dromara/lamp-cloud](https://gitee.com/dromara/lamp-cloud) | README 称 `lamp-cloud` 为微服务和单体模式融合版，官方文档写明 Java、Spring Cloud Alibaba、Spring Cloud、Spring Boot、MyBatis/MyBatisPlus 等。 | 官方文档说明自研 RBAC、网关统一鉴权、数据权限。 | 项目定位为多租户/SaaS 方案；但官方文档对版本口径有区分，开源 `lamp-cloud` 对应 NONE 非租户模式，数据源/字段租户模式在赞助版中。来源：[Gitee README](https://gitee.com/dromara/lamp-cloud)、[官方简介](https://tangyh.top/doc/%E7%AE%80%E4%BB%8B.html)。 |
| mall | [macrozheng/mall](https://gitee.com/macrozheng/mall) | README 写明 `master` 为 Spring Boot 3.5 + JDK17，模块含 `mall-admin`、`mall-portal`、`mall-security`。 | 官方权限文档说明后台权限管理包括菜单管理、资源管理、角色管理、后台用户管理；认证授权使用 SpringSecurity + JWT。 | 本次官方资料中未看到数据权限或多租户明确能力。来源：[Gitee README](https://gitee.com/macrozheng/mall)、[权限表文档](https://www.macrozheng.com/mall/database/mall_permission.html)、[SpringSecurity + JWT 文档](https://www.macrozheng.com/mall/architect/mall_arch_04.html)。 |
| renren-fast | [renrenio/renren-fast](https://gitee.com/renrenio/renren-fast) | README 写明后端部署运行 `RenrenApplication.java`，并称其为前后端分离 Java 快速开发平台。 | README 明确管理员列表、角色管理、菜单管理、`sys` 权限模块，支持通过 token 进行数据交互，权限可控制到页面或按钮。 | README 本身未直接声明数据权限；README 链出的官方文档页有数据权限设计目录，但该页同时标注 `renren-security v5.0 renren-fast v3.0`，此处不强行等同为当前 Gitee README 的直接声明；未看到多租户明确证据。来源：[Gitee README](https://gitee.com/renrenio/renren-fast)、[官方文档](https://www.renren.io/guide)。 |
| MaxKey | [dromara/MaxKey](https://gitee.com/dromara/MaxKey) | README/官网说明其基于 Java EE、微服务架构、Spring、MySQL、Tomcat、Redis、MQ。 | README 明确提供 IDM、AM、SSO、RBAC 权限管理、资源管理，支持 OAuth2.x、OIDC、SAML2.0、JWT、CAS、SCIM 等协议。 | README 明确 IDaaS 多租户认证平台，支持集团多企业独立管理或企业内不同部门数据隔离；“数据权限”未作为独立功能出现。来源：[Gitee README](https://gitee.com/dromara/MaxKey)、[官网](https://www.maxkey.top/)。 |

## 按主线展开阅读

### 基础后台权限

RuoYi、RuoYi-Vue、renren-fast、mall、ELADMIN、Guns、SmartAdmin 都可以作为“后台管理系统权限模型”的入门样本。它们的官方资料中都能看到用户、角色、菜单、按钮、部门或机构等基础对象。RuoYi 使用 Shiro；RuoYi-Vue、mall、ELADMIN 更明确地走 Spring Security/JWT 路线；SmartAdmin 和 RuoYi-Vue-Plus 明确使用 Sa-Token。

写文档时，可以先把基础后台权限抽象成这些对象：用户、角色、菜单、按钮、部门/机构、岗位、资源/API、登录态。再说明不同项目如何把“菜单权限”“按钮权限”“接口权限”分开表达。

### 企业级权限

ruoyi-vue-pro、JeecgBoot、JeeSite5、SpringBlade、RuoYi-Vue-Plus、lamp-cloud 更适合作为企业级样本。它们的官方资料中不仅有基础 RBAC，还出现了多租户、数据权限、组织机构、岗位、表单权限、统一认证、租户隔离等能力。

写文档时，可以把“企业级权限”从基础 RBAC 往外扩一层：组织模型、岗位模型、租户模型、数据范围、审计、第三方登录、单点登录。这里要注意区分“项目宣称支持”和“开源版明确包含”，例如 pig 的 README 明确说明开源版移除了多租户、数据权限扩展，lamp-cloud 的官方文档也区分了开源非租户模式和赞助版租户模式。

### 统一登录网关鉴权

RuoYi-Cloud、pig、SpringBlade、lamp-cloud、MaxKey 可以作为统一登录、认证中心、网关鉴权的重点阅读对象。RuoYi-Cloud 有 `ruoyi-gateway` 和 `ruoyi-auth`；pig 有 `pig-auth` 和 `pig-gateway`，并基于 Spring Authorization Server；MaxKey 是更纯粹的 IAM/SSO 产品，官方资料明确列出 OAuth2.x、OIDC、SAML2.0、JWT、CAS、SCIM 等协议。

写文档时，可以把这条主线写成：单体后台的登录态，到前后端分离的 JWT/Token，再到微服务网关统一鉴权，最后到 IAM/IDaaS 与标准协议。

### 数据权限治理闭环

RuoYi 系列、ruoyi-vue-pro、JeecgBoot、JeeSite5、RuoYi-Vue-Plus、ELADMIN、SmartAdmin、lamp-cloud 都有数据权限或数据范围的官方证据。常见实现口径包括：部门数据权限、角色数据范围、行级数据权限、字段权限、MyBatis 插件、注解式数据范围控制。

写文档时，可以先区分功能权限和数据权限：功能权限回答“能不能进入页面、点击按钮、调用接口”；数据权限回答“能看哪些行、哪些部门、哪些字段、哪些租户的数据”。再把官方项目中的实现口径归纳为部门树、角色数据范围、SQL 拼接/插件、注解和字段权限。

## 本地图片归档

图片统一放在 `/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/`。本次只保存 README 或官方资料中与架构、系统界面、权限体系阅读有直接帮助的代表图；徽章、二维码、课程广告类图片未归档。

| 项目 | 本地图片 |
|---|---|
| ruoyi-vue-pro | ![ruoyi-vue-pro architecture](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/ruoyi-vue-pro-architecture.png) |
| ruoyi-vue-pro | ![ruoyi-vue-pro system feature](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/ruoyi-vue-pro-system-feature.png) |
| RuoYi | ![ruoyi admin preview](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/ruoyi-admin-preview-1.png) |
| RuoYi-Vue | ![ruoyi vue preview](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/ruoyi-vue-preview-1.png) |
| RuoYi-Cloud | ![ruoyi cloud preview](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/ruoyi-cloud-preview-1.png) |
| JeeSite | ![jeesite preview](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/jeesite-preview-1.png) |
| JeecgBoot | ![jeecgboot preview](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/jeecgboot-preview-1.png) |
| SpringBlade | ![springblade framework](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/springblade-framework.png) |
| RuoYi-Vue-Plus | ![ruoyi vue plus preview](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/ruoyi-vue-plus-preview-1.png) |
| Guns | ![guns preview](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/guns-preview-1.png) |
| SmartAdmin | ![smart admin home](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/smart-admin-home.webp) |
| lamp-cloud | ![lamp cloud architecture](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/lamp-cloud-architecture.png) |
| mall | ![mall system architecture](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/mall-system-architecture.jpg) |
| mall | ![mall admin show](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/mall-admin-show.png) |
| renren-fast | ![renren fast preview](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/renren-fast-preview-1.jpeg) |
| MaxKey | ![maxkey index](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/maxkey-index.png) |
| MaxKey | ![maxkey users](/img/技术/系统设计基础/权限与用户体系/gitee-admin-research/maxkey-users.png) |

## 后续建议

下一步如果要进入正式文档编写，可以先只深读 6 个代表项目：

1. RuoYi：基础后台权限和 Shiro 样本。
2. RuoYi-Vue：前后端分离、Spring Security、JWT 样本。
3. ruoyi-vue-pro 或 RuoYi-Vue-Plus：多租户、数据权限、现代 Spring Boot 后台样本。
4. JeecgBoot：低代码平台中的细粒度权限、多租户、数据权限样本。
5. pig 或 RuoYi-Cloud：微服务认证中心、网关鉴权样本。
6. MaxKey：IAM、SSO、标准协议样本。

正式写正文时，不建议把 17 个项目都平均展开；更适合把它们作为证据池，按“基础后台权限、企业级权限、统一登录网关鉴权、数据权限治理闭环”四条主线提炼。
