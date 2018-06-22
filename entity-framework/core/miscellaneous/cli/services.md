---
title: 设计时服务的 EF 核心
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
<a name="design-time-services"></a>设计时服务
====================
在设计时仅使用由工具使用某些服务。 这些服务 EF 核心运行时服务，以阻止它们不会与你的应用部署分开管理。 若要覆盖这些服务 （例如服务来生成迁移文件） 之一，添加的实现`IDesignTimeServices`到你的启动项目。

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
