---
title: 计算的列的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052477"
---
# <a name="computed-columns"></a><span data-ttu-id="06982-102">计算列</span><span class="sxs-lookup"><span data-stu-id="06982-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="06982-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="06982-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="06982-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="06982-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="06982-105">计算的列是在数据库中计算其值的列。</span><span class="sxs-lookup"><span data-stu-id="06982-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="06982-106">计算的列可以使用表中其他列，若要计算其值。</span><span class="sxs-lookup"><span data-stu-id="06982-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="06982-107">约定</span><span class="sxs-lookup"><span data-stu-id="06982-107">Conventions</span></span>

<span data-ttu-id="06982-108">按照约定，计算的列不在模型中创建。</span><span class="sxs-lookup"><span data-stu-id="06982-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="06982-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="06982-109">Data Annotations</span></span>

<span data-ttu-id="06982-110">计算的列不可以使用数据注释配置。</span><span class="sxs-lookup"><span data-stu-id="06982-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="06982-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="06982-111">Fluent API</span></span>

<span data-ttu-id="06982-112">可以使用 Fluent API 指定属性应映射到计算列。</span><span class="sxs-lookup"><span data-stu-id="06982-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
