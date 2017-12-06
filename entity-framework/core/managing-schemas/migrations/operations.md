---
title: "自定义迁移操作-EF 核心"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d41409dee034e84d22092a5f9111dd79c87dcec3
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-operations"></a><span data-ttu-id="b3d41-102">自定义迁移操作</span><span class="sxs-lookup"><span data-stu-id="b3d41-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="b3d41-103">MigrationBuilder API 允许你迁移期间，执行许多不同类型的操作，但它并不详尽。</span><span class="sxs-lookup"><span data-stu-id="b3d41-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="b3d41-104">但是，API 还会可扩展它允许你定义自己的操作。</span><span class="sxs-lookup"><span data-stu-id="b3d41-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="b3d41-105">有两种方法来扩展 API： 使用`Sql()`方法，或通过定义自定义`MigrationOperation`对象。</span><span class="sxs-lookup"><span data-stu-id="b3d41-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="b3d41-106">为了说明，让我们看一下实现创建使用每种方法的数据库用户的操作。</span><span class="sxs-lookup"><span data-stu-id="b3d41-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="b3d41-107">在我们迁移中，我们想要启用编写以下代码：</span><span class="sxs-lookup"><span data-stu-id="b3d41-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="b3d41-108">使用 MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="b3d41-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="b3d41-109">实现自定义操作的最简单方法是定义扩展方法的调用`MigrationBuilder.Sql()`。</span><span class="sxs-lookup"><span data-stu-id="b3d41-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="b3d41-110">下面是生成相应的 Transact SQL 示例。</span><span class="sxs-lookup"><span data-stu-id="b3d41-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="b3d41-111">如果你迁移需要支持多个数据库提供程序，则可以使用`MigrationBuilder.ActiveProvider`属性。</span><span class="sxs-lookup"><span data-stu-id="b3d41-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="b3d41-112">下面是支持 Microsoft SQL Server 和 PostreSQL 一个示例。</span><span class="sxs-lookup"><span data-stu-id="b3d41-112">Here's an example supporting both Microsoft SQL Server and PostreSQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }
}
```

<span data-ttu-id="b3d41-113">此方法仅适用如果知道每个提供程序将应用你自定义操作的位置。</span><span class="sxs-lookup"><span data-stu-id="b3d41-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="b3d41-114">使用 MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="b3d41-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="b3d41-115">若要分离 SQL 的自定义操作，你可以定义自己`MigrationOperation`来表示它。</span><span class="sxs-lookup"><span data-stu-id="b3d41-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="b3d41-116">然后，因此它可以确定相应的 SQL 生成，系统会操作传递给提供程序。</span><span class="sxs-lookup"><span data-stu-id="b3d41-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="b3d41-117">使用此方法，该扩展方法只需添加到这些操作之一`MigrationBuilder.Operations`。</span><span class="sxs-lookup"><span data-stu-id="b3d41-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

<span data-ttu-id="b3d41-118">此方法要求每个提供商联系，以了解如何在此操作生成 SQL 其`IMigrationsSqlGenerator`服务。</span><span class="sxs-lookup"><span data-stu-id="b3d41-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="b3d41-119">下面是示例重写以处理新的操作的 SQL Server 的生成器。</span><span class="sxs-lookup"><span data-stu-id="b3d41-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="b3d41-120">将替换为更新默认迁移 sql 生成器服务。</span><span class="sxs-lookup"><span data-stu-id="b3d41-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
