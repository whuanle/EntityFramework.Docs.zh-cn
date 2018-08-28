---
title: 设计时服务的 EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997526"
---
<a name="design-time-services"></a><span data-ttu-id="946d1-102">设计时服务</span><span class="sxs-lookup"><span data-stu-id="946d1-102">Design-time services</span></span>
====================
<span data-ttu-id="946d1-103">工具使用某些服务仅在设计时使用。</span><span class="sxs-lookup"><span data-stu-id="946d1-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="946d1-104">这些服务 EF Core运行时服务，以阻止它们不会与你的应用部署分开管理。</span><span class="sxs-lookup"><span data-stu-id="946d1-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="946d1-105">若要重写其中一种服务 （例如服务来生成迁移文件），将添加的实现`IDesignTimeServices`到启动项目。</span><span class="sxs-lookup"><span data-stu-id="946d1-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
