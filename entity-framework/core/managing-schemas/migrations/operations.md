---
title: 自定义迁移操作-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: 93de6ee1b2eda1875188ace6eda299260fbcc1fe
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028078"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="a0676-102">自定义迁移操作</span><span class="sxs-lookup"><span data-stu-id="a0676-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="a0676-103">MigrationBuilder API，可在迁移过程中执行许多不同类型的操作，但它并不详尽。</span><span class="sxs-lookup"><span data-stu-id="a0676-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="a0676-104">但是，该 API 是还可扩展允许您定义自己的操作。</span><span class="sxs-lookup"><span data-stu-id="a0676-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="a0676-105">有两种方法来扩展 API： 使用`Sql()`方法，或通过定义自定义`MigrationOperation`对象。</span><span class="sxs-lookup"><span data-stu-id="a0676-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="a0676-106">为了演示，让我们看一下实现创建使用每种方法的数据库用户的操作。</span><span class="sxs-lookup"><span data-stu-id="a0676-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="a0676-107">在我们迁移中，我们想要启用编写以下代码：</span><span class="sxs-lookup"><span data-stu-id="a0676-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="a0676-108">使用 MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="a0676-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="a0676-109">若要实现自定义操作的最简单方法是定义扩展方法的调用`MigrationBuilder.Sql()`。</span><span class="sxs-lookup"><span data-stu-id="a0676-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="a0676-110">下面是生成相应的 TRANSACT-SQL 示例。</span><span class="sxs-lookup"><span data-stu-id="a0676-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="a0676-111">如果迁移需要支持多个数据库提供程序，则可以使用`MigrationBuilder.ActiveProvider`属性。</span><span class="sxs-lookup"><span data-stu-id="a0676-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="a0676-112">下面是支持 Microsoft SQL Server 和 PostgreSQL 示例。</span><span class="sxs-lookup"><span data-stu-id="a0676-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

    return migrationBuilder;
}
```

<span data-ttu-id="a0676-113">此方法仅适用于您知道每个提供程序的应用自定义操作的位置。</span><span class="sxs-lookup"><span data-stu-id="a0676-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="a0676-114">使用 MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="a0676-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="a0676-115">若要分离 SQL 的自定义操作，你可以定义自己`MigrationOperation`代表它。</span><span class="sxs-lookup"><span data-stu-id="a0676-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="a0676-116">该操作然后传递给提供程序以便它可以确定相应的 SQL 生成。</span><span class="sxs-lookup"><span data-stu-id="a0676-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="a0676-117">使用此方法时，扩展方法只需添加到这些操作之一`MigrationBuilder.Operations`。</span><span class="sxs-lookup"><span data-stu-id="a0676-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="a0676-118">这种方法需要知道如何在此操作生成的 SQL 每个提供程序其`IMigrationsSqlGenerator`服务。</span><span class="sxs-lookup"><span data-stu-id="a0676-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="a0676-119">下面是示例重写以处理新的操作的 SQL Server 的生成器。</span><span class="sxs-lookup"><span data-stu-id="a0676-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="a0676-120">将更新一个替换为默认迁移 sql 生成器服务。</span><span class="sxs-lookup"><span data-stu-id="a0676-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
