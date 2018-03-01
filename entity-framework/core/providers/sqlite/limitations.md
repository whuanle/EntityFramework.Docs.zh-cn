---
title: "SQLite 数据库提供程序的限制的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 3e0f375fa3e01747565cc158af02f6d21f6ae898
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF 核心数据库提供程序限制

SQLite 提供程序具有许多的迁移限制。 这些限制的大多数是基础的 SQLite 数据库引擎中的限制的结果，并不特定于 EF。

## <a name="modeling-limitations"></a>建模限制

（实体框架关系数据库提供程序通过共享） 的公共关系库定义建模概念所共有的大多数关系数据库引擎的 Api。 SQLite 提供程序不支持几个这些概念。

* 架构
* 序列

## <a name="migrations-limitations"></a>迁移限制

SQLite 数据库引擎不支持大量的大部分其他关系数据库支持的架构操作。 如果你尝试将不受支持的操作之一应用于一个 SQLite 数据库则`NotSupportedException`将引发。

| 操作            | 支持？ | 需要版本 |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.1              |
| RenameIndex          | ✔          | 1.0              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ （无操作）  | 2.0              |
| DropSchema           | ✔ （无操作）  | 2.0              |
| Insert               | ✔          | 2.0              |
| 更新               | ✔          | 2.0              |
| 删除               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>迁移限制解决方法

一些可以解决方法通过手动编写代码，在你迁移执行表中这些限制的重新生成。 表重新生成涉及重命名现有表、 创建新表、 将数据复制到新的表中，和删除旧表。 你将需要使用`Sql(string)`方法来执行这些步骤中的一部分。

请参阅[进行其他类型的表架构更改](http://sqlite.org/lang_altertable.html#otheralter)有关更多详细信息的 SQLite 文档中。

将来，EF 可能支持某些操作通过使用下的表重新生成方法。 你可以[此功能跟踪我们的 GitHub 项目](https://github.com/aspnet/EntityFrameworkCore/issues/329)。
