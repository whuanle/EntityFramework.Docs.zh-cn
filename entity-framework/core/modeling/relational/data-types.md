---
title: 数据类型的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053497"
---
# <a name="data-types"></a>数据类型

> [!NOTE]  
> 一般情况下，此部分中的配置是适用于关系数据库。 此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。

数据类型是指数据库特定类型的属性映射的列。

## <a name="conventions"></a>约定

按照约定，数据库提供程序选择基于属性的 CLR 类型的数据类型。 它还会考虑其他元数据，例如配置[最大长度](../max-length.md)，无论该属性是一部分的主键，等等。

例如，SQL Server 使用`datetime2(7)`为`DateTime`属性，和`nvarchar(max)`为`string`属性 (或`nvarchar(450)`为`string`用作键的属性)。

## <a name="data-annotations"></a>数据注释

可以使用数据注释以指定确切的数据类型列。

例如下面的代码配置`Url`为非 unicode 字符串，最大长度为`200`和`Rating`用作小数点的精度`5`和缩放的`2`。

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent API

Fluent API 还可用于指定列的相同数据类型。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
