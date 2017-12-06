---
title: "编写数据库提供程序的 EF 核心"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a>编写数据库提供程序

有关编写实体框架核心数据库提供程序的信息，请参阅[因此你想要编写 EF 核心提供程序](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/)通过[Arthur Vickers](https://github.com/ajcvickers)。

基本 EF 核心代码属于开放源代码，包含多个数据库提供程序可以使用作为参考。 你可以在 https://github.com/aspnet/EntityFramework 找到源代码。

## <a name="the-providers-beware-label"></a>提供程序注意标签

一旦开始于提供程序的工作，监视[ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware)在我们的 GitHub 问题和拉取请求的标签。 我们使用此标签来标识可能会影响提供程序编写器的更改。

## <a name="suggested-naming-of-third-party-providers"></a>建议的第三方提供程序命名

我们建议使用 NuGet 包的以下命名。 这是与 EF 核心小组所传递的包名称一致。

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

例如：
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
