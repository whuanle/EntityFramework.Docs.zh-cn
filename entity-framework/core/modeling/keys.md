---
title: 密钥 （主） 的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052567"
---
# <a name="keys-primary"></a><span data-ttu-id="44dd5-102">密钥 （主）</span><span class="sxs-lookup"><span data-stu-id="44dd5-102">Keys (primary)</span></span>

<span data-ttu-id="44dd5-103">一个键用作每个实体实例的主唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="44dd5-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="44dd5-104">使用关系数据库时此方法映射到的概念*主键*。</span><span class="sxs-lookup"><span data-stu-id="44dd5-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="44dd5-105">你还可以配置不是主键的唯一标识符 (请参阅[备用键](alternate-keys.md)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="44dd5-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="44dd5-106">约定</span><span class="sxs-lookup"><span data-stu-id="44dd5-106">Conventions</span></span>

<span data-ttu-id="44dd5-107">按照约定，属性名为`Id`或`<type name>Id`将配置为实体的键。</span><span class="sxs-lookup"><span data-stu-id="44dd5-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="44dd5-108">数据注释</span><span class="sxs-lookup"><span data-stu-id="44dd5-108">Data Annotations</span></span>

<span data-ttu-id="44dd5-109">数据注释可用于配置要作为键的实体的单个属性。</span><span class="sxs-lookup"><span data-stu-id="44dd5-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="44dd5-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="44dd5-110">Fluent API</span></span>

<span data-ttu-id="44dd5-111">Fluent API 可用于配置要作为键的实体的单个属性。</span><span class="sxs-lookup"><span data-stu-id="44dd5-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="44dd5-112">Fluent API 还可用于配置多个属性，以作为键的实体 （称为复合键）。</span><span class="sxs-lookup"><span data-stu-id="44dd5-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="44dd5-113">复合键仅可以配置即可使用 Fluent API-约定将永远不会在安装程序组合键，不可以使用数据注释若要配置一个。</span><span class="sxs-lookup"><span data-stu-id="44dd5-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
