---
title: 备用键 （唯一约束） 的 EF 核心
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
ms.locfileid: "26052787"
---
# <a name="alternate-keys-unique-constraints"></a>备用键 （唯一约束）

> [!NOTE]  
> 一般情况下，此部分中的配置是适用于关系数据库。 此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。

唯一约束是每个备用键在模型中引入的。

## <a name="conventions"></a>约定

索引和约束引入了备用密钥相关的命名约定，由`AK_<type name>_<property name>`。 有关备用组合键`<property name>`将成为下划线分隔列表的属性名称。

## <a name="data-annotations"></a>数据注释

不能使用数据注释配置唯一约束。

## <a name="fluent-api"></a>Fluent API

可以使用 Fluent API 来配置备用项的索引和约束的名称。

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
