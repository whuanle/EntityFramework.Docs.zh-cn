---
title: "EF Core 2.1 中的新增功能 - EF Core"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: bb1e691e0f22bd36467d58c02bde22c63067207e
ms.sourcegitcommit: fcaeaf085171dfb5c080ec42df1d1df8dfe204fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="c7b36-102">EF Core 2.1 中的新增功能</span><span class="sxs-lookup"><span data-stu-id="c7b36-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="c7b36-103">此版本仍处于预览版。</span><span class="sxs-lookup"><span data-stu-id="c7b36-103">This release is still in preview.</span></span>

<span data-ttu-id="c7b36-104">除了做了许多小改进，修复了超过 100 个产品 bug 外，EF Core 2.1 还包含几项新功能：</span><span class="sxs-lookup"><span data-stu-id="c7b36-104">Besides numerous small improvements and more than a hundred product bug fixes, EF Core 2.1 includes several new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="c7b36-105">延迟加载</span><span class="sxs-lookup"><span data-stu-id="c7b36-105">Lazy loading</span></span>
<span data-ttu-id="c7b36-106">EF Core 现包含创作可按需加载导航属性的实体类所必需的构建基块。</span><span class="sxs-lookup"><span data-stu-id="c7b36-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="c7b36-107">我们还创建了一个新包 - Microsoft.EntityFrameworkCore.Proxies，它可利用这些构建基块基于最小修改实体类（例如具有虚拟导航属性的类）来生成延迟加载代理类。</span><span class="sxs-lookup"><span data-stu-id="c7b36-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="c7b36-108">阅读[有关延迟加载的部分](xref:core/querying/related-data#lazy-loading)详细了解本主题。</span><span class="sxs-lookup"><span data-stu-id="c7b36-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="c7b36-109">实体构造函数中的参数</span><span class="sxs-lookup"><span data-stu-id="c7b36-109">Parameters in entity constructors</span></span>
<span data-ttu-id="c7b36-110">作为延迟加载所必需的一个构建基块，我们启用了实体创建，实体可将参数纳入其构造函数中。</span><span class="sxs-lookup"><span data-stu-id="c7b36-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="c7b36-111">可使用参数来注入属性值、延迟加载委托和服务。</span><span class="sxs-lookup"><span data-stu-id="c7b36-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="c7b36-112">阅读[有关含参数的实体构造函数部分](xref:core/modeling/constructors)详细了解本主题。</span><span class="sxs-lookup"><span data-stu-id="c7b36-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="c7b36-113">值转换</span><span class="sxs-lookup"><span data-stu-id="c7b36-113">Value conversions</span></span>
<span data-ttu-id="c7b36-114">到目前为止，EF Core 只能映射底层数据库提供程序本机支持的属性类型。</span><span class="sxs-lookup"><span data-stu-id="c7b36-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="c7b36-115">在列和属性之间来回复制值，无需进行任何转换。</span><span class="sxs-lookup"><span data-stu-id="c7b36-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="c7b36-116">自 EF Core 2.1 起，可通过值转换来转换从列获取的值，之后再将其应用到属性，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="c7b36-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="c7b36-117">我们具有可以根据约定按需应用的大量转换，以及显式配置 API，后者允许在列和属性之间注册自定义转换。</span><span class="sxs-lookup"><span data-stu-id="c7b36-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="c7b36-118">此功能有如下一些用法：</span><span class="sxs-lookup"><span data-stu-id="c7b36-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="c7b36-119">将枚举存储为字符串</span><span class="sxs-lookup"><span data-stu-id="c7b36-119">Storing enums as strings</span></span>
- <span data-ttu-id="c7b36-120">使用 SQL Server 映射无符号整数</span><span class="sxs-lookup"><span data-stu-id="c7b36-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="c7b36-121">自动加密和解密属性值</span><span class="sxs-lookup"><span data-stu-id="c7b36-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="c7b36-122">阅读[有关值转换的部分](xref:core/modeling/value-conversions)详细了解本主题。</span><span class="sxs-lookup"><span data-stu-id="c7b36-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="c7b36-123">LINQ GroupBy 转换</span><span class="sxs-lookup"><span data-stu-id="c7b36-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="c7b36-124">在 2.1 版之前，EF Core 中的 GroupBy LINQ 运算符始终是在内存中进行计算。</span><span class="sxs-lookup"><span data-stu-id="c7b36-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="c7b36-125">在大多数情况下，我们现在支持将其转换为 SQL GROUP BY 子句。</span><span class="sxs-lookup"><span data-stu-id="c7b36-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="c7b36-126">此示例显示了一个用 GroupBy 来计算各种聚合函数的查询：</span><span class="sxs-lookup"><span data-stu-id="c7b36-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

<span data-ttu-id="c7b36-127">相应的 SQL 转化如下所示：</span><span class="sxs-lookup"><span data-stu-id="c7b36-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="c7b36-128">数据种子设定</span><span class="sxs-lookup"><span data-stu-id="c7b36-128">Data Seeding</span></span>
<span data-ttu-id="c7b36-129">新版本可提供初始数据来填充数据库。</span><span class="sxs-lookup"><span data-stu-id="c7b36-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="c7b36-130">与 EF6 不同，种子设定数据作为模型配置的一部分与实体类型相关联。</span><span class="sxs-lookup"><span data-stu-id="c7b36-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="c7b36-131">随后将数据库升级为新版本模型时，EF Core 迁移会自动计算需要应用的插入、更新或删除操作。</span><span class="sxs-lookup"><span data-stu-id="c7b36-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="c7b36-132">如示例所示，可使用它在 `OnModelCreating` 中为 Post 配置种子数据：</span><span class="sxs-lookup"><span data-stu-id="c7b36-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="c7b36-133">阅读[有关数据种子设定的部分](xref:core/modeling/data-seeding)详细了解本主题。</span><span class="sxs-lookup"><span data-stu-id="c7b36-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="c7b36-134">查询类型</span><span class="sxs-lookup"><span data-stu-id="c7b36-134">Query types</span></span>
<span data-ttu-id="c7b36-135">EF Core 模型现可包含查询类型。</span><span class="sxs-lookup"><span data-stu-id="c7b36-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="c7b36-136">与实体类型不同，查询类型上未定义键，也不能插入、删除或更新查询类型（即它们为只读），但查询可直接返回查询类型。</span><span class="sxs-lookup"><span data-stu-id="c7b36-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="c7b36-137">以下是查询类型的一些用法：</span><span class="sxs-lookup"><span data-stu-id="c7b36-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="c7b36-138">映射到没有主键的视图</span><span class="sxs-lookup"><span data-stu-id="c7b36-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="c7b36-139">映射到没有主键的表</span><span class="sxs-lookup"><span data-stu-id="c7b36-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="c7b36-140">映射到模型中定义的查询</span><span class="sxs-lookup"><span data-stu-id="c7b36-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="c7b36-141">用作 `FromSql()` 查询的返回类型</span><span class="sxs-lookup"><span data-stu-id="c7b36-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="c7b36-142">阅读[有关查询类型的部分](xref:core/modeling/query-types)详细了解本主题。</span><span class="sxs-lookup"><span data-stu-id="c7b36-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="c7b36-143">针对派生类型的 Include</span><span class="sxs-lookup"><span data-stu-id="c7b36-143">Include for derived types</span></span>
<span data-ttu-id="c7b36-144">现可在编写 `Include` 方法的表达式时指定仅在派生类型上定义的导航属性。</span><span class="sxs-lookup"><span data-stu-id="c7b36-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="c7b36-145">对于 `Include` 的强类型版本，我们支持使用显式强制转换或 `as` 运算符。</span><span class="sxs-lookup"><span data-stu-id="c7b36-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="c7b36-146">我们现在还支持在 `Include` 的字符串版本中引用在派生类型上定义的导航属性的名称：</span><span class="sxs-lookup"><span data-stu-id="c7b36-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="c7b36-147">阅读[有关派生类型的 Include 部分](xref:core/querying/related-data#include-on-derived-types)详细了解本主题。</span><span class="sxs-lookup"><span data-stu-id="c7b36-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="c7b36-148">System.Transactions 支持</span><span class="sxs-lookup"><span data-stu-id="c7b36-148">System.Transactions support</span></span>
<span data-ttu-id="c7b36-149">我们增加了对 System.Transactions 功能（如 TransactionScope）的应用。</span><span class="sxs-lookup"><span data-stu-id="c7b36-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="c7b36-150">使用支持该功能的数据库提供程序时，这将适用于 .NET Framework 和 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="c7b36-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="c7b36-151">阅读[有关 System.Transactions 的部分](xref:core/saving/transactions#using-systemtransactions)详细了解本主题。</span><span class="sxs-lookup"><span data-stu-id="c7b36-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="c7b36-152">初始迁移时生成更好的列顺序</span><span class="sxs-lookup"><span data-stu-id="c7b36-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="c7b36-153">根据客户反馈，我们对迁移进行了更新，使得先以与类中声明的属性相同的顺序为表生成列。</span><span class="sxs-lookup"><span data-stu-id="c7b36-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="c7b36-154">请注意，在创建初始表后，添加新成员时，EF Core 不能更改顺序。</span><span class="sxs-lookup"><span data-stu-id="c7b36-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="c7b36-155">相关子查询优化</span><span class="sxs-lookup"><span data-stu-id="c7b36-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="c7b36-156">我们改进了查询转换，避免在许多常见情况下执行“N + 1”SQL 查询，一般情况下，在投影中使用导航属性后，来自根查询的数据会与来自相关子查询的数据相连接。</span><span class="sxs-lookup"><span data-stu-id="c7b36-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="c7b36-157">进行优化需要缓冲子查询的结果，且我们要求修改查询，选择新行为。</span><span class="sxs-lookup"><span data-stu-id="c7b36-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="c7b36-158">例如，以下查询通常会转换为：一个“客户”查询，加上 N（其中“N”是返回的客户数量）个单独的“订单”查询：</span><span class="sxs-lookup"><span data-stu-id="c7b36-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="c7b36-159">将 `ToList()` 放入正确的位置，指示缓冲适用于订单，即可启用优化：</span><span class="sxs-lookup"><span data-stu-id="c7b36-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="c7b36-160">请注意，此查询只会被转换为两个 SQL 查询：一个“客户”查询，一个“订单”查询。</span><span class="sxs-lookup"><span data-stu-id="c7b36-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="ownedattribute"></a><span data-ttu-id="c7b36-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="c7b36-161">OwnedAttribute</span></span>

<span data-ttu-id="c7b36-162">现只需使用 `[Owned]` 注释类型，并确保所有者实体添加到了模型中，即可配置[固有实体类型](xref:core/modeling/owned-entities)：</span><span class="sxs-lookup"><span data-stu-id="c7b36-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="c7b36-163">数据库提供程序兼容性</span><span class="sxs-lookup"><span data-stu-id="c7b36-163">Database provider compatibility</span></span>

<span data-ttu-id="c7b36-164">设计 EF Core 2.1 的目的是为了与为 EF Core 2.0 创建的数据库提供提供程序兼容。</span><span class="sxs-lookup"><span data-stu-id="c7b36-164">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0.</span></span> <span data-ttu-id="c7b36-165">虽然需要最新提供程序方可使用上述某些功能（如值转换），但现有提供程序可使用其他一些功能（例如延迟加载）。</span><span class="sxs-lookup"><span data-stu-id="c7b36-165">While some of the features described above (e.g. value conversions) require an updated provider, others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="c7b36-166">如果新功能出现任何意外的不兼容或问题，或你有任何相关反馈，请使用[我们的问题跟踪器](https://github.com/aspnet/EntityFrameworkCore/issues/new)进行报告。</span><span class="sxs-lookup"><span data-stu-id="c7b36-166">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
