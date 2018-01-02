---
title: "创建模型 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="creating-a-model"></a>创建模型

Entity Framework 使用一组约定基于实体类的形状构建模型。 可指定其他配置补充和/或替代由约定发现的内容。

本文介绍可应用于面向任何数据存储的模型的配置，以及面向任意关系数据库时可应用的配置。 提供程序还可支持特定于具体数据存储的配置。 有关提供程序特定配置的文档，请参阅[数据库提供程序](../providers/index.md)部分。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples)。

## <a name="methods-of-configuration"></a>配置方法

### <a name="fluent-api"></a>Fluent API

可在派生上下文中替代 `OnModelCreating` 方法，并使用 `ModelBuilder API` 来配置模型。 此配置方法最为有效，并可在不修改实体类的情况下指定配置。 Fluent API 配置具有最高优先级，并将替代约定和数据注释。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a>数据注释

也可将特性（称为数据注释）应用于类和属性。 数据注释会替代约定，但会被 Fluent API 配置覆盖。

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
