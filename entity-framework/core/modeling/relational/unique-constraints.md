---
title: "备用键 （唯一约束） 的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="60eed-102">备用键 （唯一约束）</span><span class="sxs-lookup"><span data-stu-id="60eed-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="60eed-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="60eed-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="60eed-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="60eed-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="60eed-105">唯一约束是每个备用键在模型中引入的。</span><span class="sxs-lookup"><span data-stu-id="60eed-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="60eed-106">约定</span><span class="sxs-lookup"><span data-stu-id="60eed-106">Conventions</span></span>

<span data-ttu-id="60eed-107">索引和约束引入了备用密钥相关的命名约定，由`AK_<type name>_<property name>`。</span><span class="sxs-lookup"><span data-stu-id="60eed-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="60eed-108">有关备用组合键`<property name>`将成为下划线分隔列表的属性名称。</span><span class="sxs-lookup"><span data-stu-id="60eed-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="60eed-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="60eed-109">Data Annotations</span></span>

<span data-ttu-id="60eed-110">不能使用数据注释配置唯一约束。</span><span class="sxs-lookup"><span data-stu-id="60eed-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="60eed-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="60eed-111">Fluent API</span></span>

<span data-ttu-id="60eed-112">可以使用 Fluent API 来配置备用项的索引和约束的名称。</span><span class="sxs-lookup"><span data-stu-id="60eed-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
