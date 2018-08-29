---
title: 备用键 （唯一约束） 的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994186"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="40ddb-102">备用键 （唯一约束）</span><span class="sxs-lookup"><span data-stu-id="40ddb-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="40ddb-103">一般而言，本部分中的配置适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="40ddb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="40ddb-104">安装关系数据库提供程序时，此处显示的扩展方法将变为可用（原因在于共享的 Microsoft.EntityFrameworkCore.Relational 包）。</span><span class="sxs-lookup"><span data-stu-id="40ddb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="40ddb-105">在模型中每个备用键是引入了唯一约束。</span><span class="sxs-lookup"><span data-stu-id="40ddb-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="40ddb-106">约定</span><span class="sxs-lookup"><span data-stu-id="40ddb-106">Conventions</span></span>

<span data-ttu-id="40ddb-107">按照约定，索引和约束，引入了的备用键将被命名为`AK_<type name>_<property name>`。</span><span class="sxs-lookup"><span data-stu-id="40ddb-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="40ddb-108">用于备用组合键`<property name>`变得下划线分隔属性名称的列表。</span><span class="sxs-lookup"><span data-stu-id="40ddb-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="40ddb-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="40ddb-109">Data Annotations</span></span>

<span data-ttu-id="40ddb-110">不能使用数据批注配置唯一约束。</span><span class="sxs-lookup"><span data-stu-id="40ddb-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="40ddb-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="40ddb-111">Fluent API</span></span>

<span data-ttu-id="40ddb-112">可以使用 Fluent API 配置备用键的索引和约束名称。</span><span class="sxs-lookup"><span data-stu-id="40ddb-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
