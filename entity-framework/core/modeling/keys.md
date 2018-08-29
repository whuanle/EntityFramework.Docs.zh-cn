---
title: 密钥 （主） 的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 9e6946100ebabc6ba57cb792b3672219098b1e21
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994016"
---
# <a name="keys-primary"></a><span data-ttu-id="f2181-102">键 （主要）</span><span class="sxs-lookup"><span data-stu-id="f2181-102">Keys (primary)</span></span>

<span data-ttu-id="f2181-103">关键字可作为每个实体实例的主唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="f2181-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="f2181-104">使用关系数据库时此值映射到的概念*主键*。</span><span class="sxs-lookup"><span data-stu-id="f2181-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="f2181-105">你还可以配置不是主键的唯一标识符 (请参阅[备用键](alternate-keys.md)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="f2181-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="f2181-106">约定</span><span class="sxs-lookup"><span data-stu-id="f2181-106">Conventions</span></span>

<span data-ttu-id="f2181-107">按照约定，属性名为`Id`或`<type name>Id`将配置为实体的键。</span><span class="sxs-lookup"><span data-stu-id="f2181-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="f2181-108">数据注释</span><span class="sxs-lookup"><span data-stu-id="f2181-108">Data Annotations</span></span>

<span data-ttu-id="f2181-109">可以使用数据注释来配置要作为键的实体的单个属性。</span><span class="sxs-lookup"><span data-stu-id="f2181-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="f2181-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="f2181-110">Fluent API</span></span>

<span data-ttu-id="f2181-111">Fluent API 可用于配置要作为键的实体的单个属性。</span><span class="sxs-lookup"><span data-stu-id="f2181-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="f2181-112">Fluent API 还可用于配置多个属性，以作为键的实体 （称为复合键）。</span><span class="sxs-lookup"><span data-stu-id="f2181-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="f2181-113">仅可以使用 Fluent API 配置的复合键-约定将永远不会在安装程序的复合键和不可以使用数据注释来配置一个。</span><span class="sxs-lookup"><span data-stu-id="f2181-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
