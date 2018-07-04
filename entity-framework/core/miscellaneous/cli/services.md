---
title: 设计时服务的 EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053687"
---
<a name="design-time-services"></a><span data-ttu-id="50cf5-102">设计时服务</span><span class="sxs-lookup"><span data-stu-id="50cf5-102">Design-time services</span></span>
====================
<span data-ttu-id="50cf5-103">在设计时仅使用由工具使用某些服务。</span><span class="sxs-lookup"><span data-stu-id="50cf5-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="50cf5-104">这些服务 EF Core运行时服务，以阻止它们不会与你的应用部署分开管理。</span><span class="sxs-lookup"><span data-stu-id="50cf5-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="50cf5-105">若要覆盖这些服务 （例如服务来生成迁移文件） 之一，添加的实现`IDesignTimeServices`到你的启动项目。</span><span class="sxs-lookup"><span data-stu-id="50cf5-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
