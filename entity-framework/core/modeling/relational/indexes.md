---
title: 索引的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678994"
---
# <a name="indexes"></a>索引

> [!NOTE]  
> 一般而言，本部分中的配置适用于关系数据库。 安装关系数据库提供程序时，此处显示的扩展方法将变为可用（原因在于共享的 Microsoft.EntityFrameworkCore.Relational 包）。

关系数据库中的索引映射到索引中的实体框架的核心概念相同。

## <a name="conventions"></a>约定

按照约定，名为索引`IX_<type name>_<property name>`。 对于组合索引`<property name>`将成为下划线分隔列表的属性名称。

## <a name="data-annotations"></a>数据注释

不能使用数据注释配置索引。

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于配置索引的名称。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

你还可以指定筛选器。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

当使用 SQL Server 提供程序 EF 将添加 IS NOT NULL 筛选所有可以为 null 的列都是唯一索引的一部分。 若要重写你可以提供此约定`null`值。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
