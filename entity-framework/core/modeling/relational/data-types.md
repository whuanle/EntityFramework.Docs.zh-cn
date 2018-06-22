---
title: 数据类型的 EF 核心
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
# <a name="data-types"></a><span data-ttu-id="3cb45-102">数据类型</span><span class="sxs-lookup"><span data-stu-id="3cb45-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="3cb45-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="3cb45-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3cb45-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="3cb45-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3cb45-105">数据类型是指数据库特定类型的属性映射的列。</span><span class="sxs-lookup"><span data-stu-id="3cb45-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="3cb45-106">约定</span><span class="sxs-lookup"><span data-stu-id="3cb45-106">Conventions</span></span>

<span data-ttu-id="3cb45-107">按照约定，数据库提供程序选择基于属性的 CLR 类型的数据类型。</span><span class="sxs-lookup"><span data-stu-id="3cb45-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="3cb45-108">它还会考虑其他元数据，例如配置[最大长度](../max-length.md)，无论该属性是一部分的主键，等等。</span><span class="sxs-lookup"><span data-stu-id="3cb45-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="3cb45-109">例如，SQL Server 使用`datetime2(7)`为`DateTime`属性，和`nvarchar(max)`为`string`属性 (或`nvarchar(450)`为`string`用作键的属性)。</span><span class="sxs-lookup"><span data-stu-id="3cb45-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3cb45-110">数据注释</span><span class="sxs-lookup"><span data-stu-id="3cb45-110">Data Annotations</span></span>

<span data-ttu-id="3cb45-111">可以使用数据注释以指定确切的数据类型列。</span><span class="sxs-lookup"><span data-stu-id="3cb45-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="3cb45-112">例如下面的代码配置`Url`为非 unicode 字符串，最大长度为`200`和`Rating`用作小数点的精度`5`和缩放的`2`。</span><span class="sxs-lookup"><span data-stu-id="3cb45-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="3cb45-113">Fluent API</span><span class="sxs-lookup"><span data-stu-id="3cb45-113">Fluent API</span></span>

<span data-ttu-id="3cb45-114">Fluent API 还可用于指定列的相同数据类型。</span><span class="sxs-lookup"><span data-stu-id="3cb45-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
