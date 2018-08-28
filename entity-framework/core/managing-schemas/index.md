---
title: 管理数据库架构 - EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994380"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="062e6-102">管理数据库架构</span><span class="sxs-lookup"><span data-stu-id="062e6-102">Managing Database Schemas</span></span>
<span data-ttu-id="062e6-103">EF Core 提供两种主要方法来保持 EF Core 模型和数据库架构同步。若要二者择一，请确定真实源是 EF Core 模型还是数据库架构。</span><span class="sxs-lookup"><span data-stu-id="062e6-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="062e6-104">如果希望 EF Core 模型为真实源，请使用[迁移][1]。</span><span class="sxs-lookup"><span data-stu-id="062e6-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="062e6-105">对 EF Core 模型进行更改时，此方法会以增量方式将相应架构更改应用到数据库，以使数据库保持与 EF Core 模型兼容。</span><span class="sxs-lookup"><span data-stu-id="062e6-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="062e6-106">如果希望数据库架构为真实源，请使用[反向工程][2]。</span><span class="sxs-lookup"><span data-stu-id="062e6-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="062e6-107">使用此方法，可通过将数据库架构反向工程到 EF Core 模型来建立 DbContext 和实体类型类的基架。</span><span class="sxs-lookup"><span data-stu-id="062e6-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="062e6-108">[创建和删除 API][3] 也可从 EF Core 模型创建数据库架构。</span><span class="sxs-lookup"><span data-stu-id="062e6-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="062e6-109">但是，它们主要用于测试、原型制作以及可接受删除数据库的其他方案。</span><span class="sxs-lookup"><span data-stu-id="062e6-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
