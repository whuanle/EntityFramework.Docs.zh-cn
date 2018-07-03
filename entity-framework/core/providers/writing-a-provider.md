---
title: 编写数据库提供程序的 EF Core
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 4bddf5858ab2c6b2fd22571a20edb3f7c85e2853
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678955"
---
# <a name="writing-a-database-provider"></a>编写数据库提供程序

有关编写实体框架核心数据库提供程序的信息，请参阅[因此你想要编写 EF Core 提供程序](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/)通过[Arthur Vickers](https://github.com/ajcvickers)。

基本 EF Core 代码属于开放源代码，包含多个数据库提供程序可以使用作为参考。 你可以在 https://github.com/aspnet/EntityFrameworkCore 找到源代码。

## <a name="the-providers-beware-label"></a>提供程序注意标签

一旦开始于提供程序的工作，监视[ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware)在我们的 GitHub 问题和拉取请求的标签。 我们使用此标签来标识可能会影响提供程序编写器的更改。

## <a name="suggested-naming-of-third-party-providers"></a>建议的第三方提供程序命名

我们建议使用 NuGet 包的以下命名。 这是与 EF Core 小组所传递的包名称一致。

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

例如:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
