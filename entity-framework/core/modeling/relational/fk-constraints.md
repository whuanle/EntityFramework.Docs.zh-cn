---
title: 外键约束的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993983"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="bf640-102">外键约束</span><span class="sxs-lookup"><span data-stu-id="bf640-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="bf640-103">一般而言，本部分中的配置适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="bf640-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="bf640-104">安装关系数据库提供程序时，此处显示的扩展方法将变为可用（原因在于共享的 Microsoft.EntityFrameworkCore.Relational 包）。</span><span class="sxs-lookup"><span data-stu-id="bf640-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="bf640-105">每个关系在模型中引入了外键约束。</span><span class="sxs-lookup"><span data-stu-id="bf640-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="bf640-106">约定</span><span class="sxs-lookup"><span data-stu-id="bf640-106">Conventions</span></span>

<span data-ttu-id="bf640-107">按照约定外, 键约束名为`FK_<dependent type name>_<principal type name>_<foreign key property name>`。</span><span class="sxs-lookup"><span data-stu-id="bf640-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="bf640-108">对于复合外键`<foreign key property name>`变得外键属性名称的下划线分隔列表。</span><span class="sxs-lookup"><span data-stu-id="bf640-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="bf640-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="bf640-109">Data Annotations</span></span>

<span data-ttu-id="bf640-110">外键约束名称不能使用数据批注进行配置。</span><span class="sxs-lookup"><span data-stu-id="bf640-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="bf640-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="bf640-111">Fluent API</span></span>

<span data-ttu-id="bf640-112">可以使用 Fluent API 来配置关系的外键约束名称。</span><span class="sxs-lookup"><span data-stu-id="bf640-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
