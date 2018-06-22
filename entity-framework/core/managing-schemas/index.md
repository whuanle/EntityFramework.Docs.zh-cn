---
title: 管理数据库架构 - EF Core
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
ms.locfileid: "26049379"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="16c6b-102">管理数据库架构</span><span class="sxs-lookup"><span data-stu-id="16c6b-102">Managing Database Schemas</span></span>
<span data-ttu-id="16c6b-103">EF Core 提供两种主要方法来保持 EF Core 模型和数据库架构同步。若要二者择一，请确定真实源是 EF Core 模型还是数据库架构。</span><span class="sxs-lookup"><span data-stu-id="16c6b-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="16c6b-104">如果希望 EF Core 模型为真实源，请使用[迁移][1]。</span><span class="sxs-lookup"><span data-stu-id="16c6b-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="16c6b-105">对 EF Core 模型进行更改时，此方法会以增量方式将相应架构更改应用到数据库，以使数据库保持与 EF Core 模型兼容。</span><span class="sxs-lookup"><span data-stu-id="16c6b-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="16c6b-106">如果希望数据库架构为真实源，请使用[反向工程][2]。</span><span class="sxs-lookup"><span data-stu-id="16c6b-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="16c6b-107">使用此方法，可通过将数据库架构反向工程到 EF Core 模型来建立 DbContext 和实体类型类的基架。</span><span class="sxs-lookup"><span data-stu-id="16c6b-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="16c6b-108">[创建和删除 API][3] 也可从 EF Core 模型创建数据库架构。</span><span class="sxs-lookup"><span data-stu-id="16c6b-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="16c6b-109">但是，它们主要用于测试、原型制作以及可接受删除数据库的其他方案。</span><span class="sxs-lookup"><span data-stu-id="16c6b-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
