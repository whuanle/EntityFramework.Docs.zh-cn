---
title: 自定义迁移操作-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 33f4fb6dca7c0d2e29abd196194a453f2b390b14
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997089"
---
<a name="custom-migrations-operations"></a>自定义迁移操作
============================
MigrationBuilder API，可在迁移过程中执行许多不同类型的操作，但它并不详尽。 但是，该 API 是还可扩展允许您定义自己的操作。 有两种方法来扩展 API： 使用`Sql()`方法，或通过定义自定义`MigrationOperation`对象。

为了演示，让我们看一下实现创建使用每种方法的数据库用户的操作。 在我们迁移中，我们想要启用编写以下代码：

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>使用 MigrationBuilder.Sql()
----------------------------
若要实现自定义操作的最简单方法是定义扩展方法的调用`MigrationBuilder.Sql()`。
下面是生成相应的 TRANSACT-SQL 示例。

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

如果迁移需要支持多个数据库提供程序，则可以使用`MigrationBuilder.ActiveProvider`属性。 下面是支持 Microsoft SQL Server 和 PostgreSQL 示例。

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

此方法仅适用于您知道每个提供程序的应用自定义操作的位置。

<a name="using-a-migrationoperation"></a>使用 MigrationOperation
---------------------------
若要分离 SQL 的自定义操作，你可以定义自己`MigrationOperation`代表它。 该操作然后传递给提供程序以便它可以确定相应的 SQL 生成。

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

使用此方法时，扩展方法只需添加到这些操作之一`MigrationBuilder.Operations`。

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

这种方法需要知道如何在此操作生成的 SQL 每个提供程序其`IMigrationsSqlGenerator`服务。 下面是示例重写以处理新的操作的 SQL Server 的生成器。

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

将更新一个替换为默认迁移 sql 生成器服务。

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
