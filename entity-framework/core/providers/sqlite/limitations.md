---
title: "SQLite 数据库提供程序的限制的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="472cb-102">SQLite EF 核心数据库提供程序限制</span><span class="sxs-lookup"><span data-stu-id="472cb-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="472cb-103">SQLite 提供程序具有许多的迁移限制。</span><span class="sxs-lookup"><span data-stu-id="472cb-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="472cb-104">这些限制的大多数是基础的 SQLite 数据库引擎中的限制的结果，并不特定于 EF。</span><span class="sxs-lookup"><span data-stu-id="472cb-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="472cb-105">建模限制</span><span class="sxs-lookup"><span data-stu-id="472cb-105">Modeling limitations</span></span>

<span data-ttu-id="472cb-106">（实体框架关系数据库提供程序通过共享） 的公共关系库定义建模概念所共有的大多数关系数据库引擎的 Api。</span><span class="sxs-lookup"><span data-stu-id="472cb-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="472cb-107">SQLite 提供程序不支持几个这些概念。</span><span class="sxs-lookup"><span data-stu-id="472cb-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="472cb-108">架构</span><span class="sxs-lookup"><span data-stu-id="472cb-108">Schemas</span></span>
* <span data-ttu-id="472cb-109">序列</span><span class="sxs-lookup"><span data-stu-id="472cb-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="472cb-110">迁移限制</span><span class="sxs-lookup"><span data-stu-id="472cb-110">Migrations limitations</span></span>

<span data-ttu-id="472cb-111">SQLite 数据库引擎不支持大量的大部分其他关系数据库支持的架构操作。</span><span class="sxs-lookup"><span data-stu-id="472cb-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="472cb-112">如果你尝试将不受支持的操作之一应用于一个 SQLite 数据库则`NotSupportedException`将引发。</span><span class="sxs-lookup"><span data-stu-id="472cb-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="472cb-113">操作</span><span class="sxs-lookup"><span data-stu-id="472cb-113">Operation</span></span>            | <span data-ttu-id="472cb-114">支持？</span><span class="sxs-lookup"><span data-stu-id="472cb-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="472cb-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="472cb-115">AddColumn</span></span>            | <span data-ttu-id="472cb-116">✔</span><span class="sxs-lookup"><span data-stu-id="472cb-116">✔</span></span>          |
| <span data-ttu-id="472cb-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="472cb-117">AddForeignKey</span></span>        | <span data-ttu-id="472cb-118">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-118">✗</span></span>          |
| <span data-ttu-id="472cb-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="472cb-119">AddPrimaryKey</span></span>        | <span data-ttu-id="472cb-120">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-120">✗</span></span>          |
| <span data-ttu-id="472cb-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="472cb-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="472cb-122">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-122">✗</span></span>          |
| <span data-ttu-id="472cb-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="472cb-123">AlterColumn</span></span>          | <span data-ttu-id="472cb-124">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-124">✗</span></span>          |
| <span data-ttu-id="472cb-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="472cb-125">CreateIndex</span></span>          | <span data-ttu-id="472cb-126">✔</span><span class="sxs-lookup"><span data-stu-id="472cb-126">✔</span></span>          |
| <span data-ttu-id="472cb-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="472cb-127">CreateTable</span></span>          | <span data-ttu-id="472cb-128">✔</span><span class="sxs-lookup"><span data-stu-id="472cb-128">✔</span></span>          |
| <span data-ttu-id="472cb-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="472cb-129">DropColumn</span></span>           | <span data-ttu-id="472cb-130">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-130">✗</span></span>          |
| <span data-ttu-id="472cb-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="472cb-131">DropForeignKey</span></span>       | <span data-ttu-id="472cb-132">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-132">✗</span></span>          |
| <span data-ttu-id="472cb-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="472cb-133">DropIndex</span></span>            | <span data-ttu-id="472cb-134">✔</span><span class="sxs-lookup"><span data-stu-id="472cb-134">✔</span></span>          |
| <span data-ttu-id="472cb-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="472cb-135">DropPrimaryKey</span></span>       | <span data-ttu-id="472cb-136">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-136">✗</span></span>          |
| <span data-ttu-id="472cb-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="472cb-137">DropTable</span></span>            | <span data-ttu-id="472cb-138">✔</span><span class="sxs-lookup"><span data-stu-id="472cb-138">✔</span></span>          |
| <span data-ttu-id="472cb-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="472cb-139">DropUniqueConstraint</span></span> | <span data-ttu-id="472cb-140">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-140">✗</span></span>          |
| <span data-ttu-id="472cb-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="472cb-141">RenameColumn</span></span>         | <span data-ttu-id="472cb-142">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-142">✗</span></span>          |
| <span data-ttu-id="472cb-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="472cb-143">RenameIndex</span></span>          | <span data-ttu-id="472cb-144">✗</span><span class="sxs-lookup"><span data-stu-id="472cb-144">✗</span></span>          |
| <span data-ttu-id="472cb-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="472cb-145">RenameTable</span></span>          | <span data-ttu-id="472cb-146">✔</span><span class="sxs-lookup"><span data-stu-id="472cb-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="472cb-147">迁移限制解决方法</span><span class="sxs-lookup"><span data-stu-id="472cb-147">Migrations limitations workaround</span></span>

<span data-ttu-id="472cb-148">一些可以解决方法通过手动编写代码，在你迁移执行表中这些限制的重新生成。</span><span class="sxs-lookup"><span data-stu-id="472cb-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="472cb-149">表重新生成涉及重命名现有表、 创建新表、 将数据复制到新的表中，和删除旧表。</span><span class="sxs-lookup"><span data-stu-id="472cb-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="472cb-150">你将需要使用`Sql(string)`方法来执行这些步骤中的一部分。</span><span class="sxs-lookup"><span data-stu-id="472cb-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="472cb-151">请参阅[进行其他类型的表架构更改](http://sqlite.org/lang_altertable.html#otheralter)有关更多详细信息的 SQLite 文档中。</span><span class="sxs-lookup"><span data-stu-id="472cb-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="472cb-152">将来，EF 可能支持某些操作通过使用下的表重新生成方法。</span><span class="sxs-lookup"><span data-stu-id="472cb-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="472cb-153">你可以[此功能跟踪我们的 GitHub 项目](https://github.com/aspnet/EntityFramework/issues/329)。</span><span class="sxs-lookup"><span data-stu-id="472cb-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
