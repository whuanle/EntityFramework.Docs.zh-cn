---
title: 从 EF6 移植到 EF Core-验证要求
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052857"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>之前从 EF6 到 EF Core 的迁移： 验证应用程序的要求

在开始迁移过程之前很重要，以验证 EF Core 满足你的应用程序的数据访问要求。

## <a name="missing-features"></a>缺少的功能

请确保 EF Core 具有所需应用程序中使用的所有功能。 请参阅[功能比较](../features.md)有关如何在 EF Core 中设置的功能比较到 ef6 更高版本的详细比较。 如果缺少任何所需的功能，请确保，你可以补偿缺少这些功能移植到 EF Core 之前。

## <a name="behavior-changes"></a>行为更改

这是从 EF6 和 EF Core 之间的行为中的某些更改非详尽列表。 请务必记住这几点你端口作为你的应用程序因为它们可能更改你的应用程序的行为，但将不显示为编译错误后交换到 EF Core 的方式。

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach 和 graph 行为

在从 EF6，调用`DbSet.Add()`对实体的所有实体在其导航属性中引用的递归搜索。 发现了，并且不由上下文中，已跟踪的任何实体也是标记为添加。 `DbSet.Attach()`行为相同，但所有实体都标记为不变。

**EF Core 执行类似的递归搜索，但使用一些略有不同的规则。**

*  根实体始终处于请求的状态 (为添加`DbSet.Add`和不变`DbSet.Attach`)。

*  **适用于导航属性在递归搜索过程中找到的实体：**

    *  **如果实体的主键是生成的存储**

        * 如果主键不设置为一个值，则状态将设置为添加。 主键值被视为"未设置"被分配的属性类型的 CLR 默认值 (即`0`为`int`，`null`为`string`等。)。

        * 如果为主键设置为一个值，状态将是设置为不变。

    *  如果主键不是数据库生成，则会将实体放在与根相同的状态。

### <a name="code-first-database-initialization"></a>第一个数据库初始化的代码

**从 EF6 具有大量的幻它围绕选择数据库连接和初始化数据库执行。这些规则包括：**

* 如果不执行任何配置，则从 EF6 将选择 SQL Express 或 LocalDb 上的数据库。

* 如果连接字符串具有相同名称作为上下文是在应用程序`App/Web.config`文件，将使用此连接。

* 如果数据库不存在，则创建它。

* 如果没有任何从模型表的数据库中存在，则会将当前模型的架构添加到数据库。 如果启用迁移后，它们用于创建数据库。

* 如果数据库存在并从 EF6 以前已创建了架构，然后针对与当前模型的兼容性检查架构。 如果自创建架构时，该模型已更改，将引发异常。

**EF Core 不执行任何此幻数。**

* 必须在代码中显式配置数据库连接。

* 不执行初始化。 必须使用`DbContext.Database.Migrate()`将迁移 (或`DbContext.Database.EnsureCreated()`和`EnsureDeleted()`而无需使用迁移的创建或删除数据库)。

### <a name="code-first-table-naming-convention"></a>代码的第一个表命名约定

从 EF6 通过计算该实体映射到的默认表名称的复数形式服务运行的实体类名。

EF Core使用的名称的`DbSet`派生上下文公开了实体的属性。 如果该实体没有`DbSet`使用属性，则类名称。
