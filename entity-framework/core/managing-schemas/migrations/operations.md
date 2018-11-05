---
title: 自定义迁移操作-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: d715fe0408f25eb75c3160af79bb98fc87e41b17
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284065"
---
<a name="custom-migrations-operations"></a>自定义迁移操作
============================
MigrationBuilder API，可在迁移过程中执行许多不同类型的操作，但它并不全面。 不过，这个可扩展的 API 允许你定义自己的操作。 有两种方法来扩展 API： 使用`Sql()`方法，或通过自定义`MigrationOperation`对象。

出于演示目的，让我们看一下如何通过这两种方案实现新增数据库用户的操作。 在我们的迁移中，我们希望以下代码生效：

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>使用 MigrationBuilder.Sql()
----------------------------
实现自定义操作的最简单方法是通过定义扩展方法来调用`MigrationBuilder.Sql()`。
下面是生成相应 TRANSACT-SQL 的示例。

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

如果迁移需要支持多个数据库提供程序，则可以使用`MigrationBuilder.ActiveProvider`属性。 下面是支持 Microsoft SQL Server 和 PostgreSQL 的示例。

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

此方法仅适用于你了解这些自定义操作所应用的每个具体数据库的提供程序的情况。

<a name="using-a-migrationoperation"></a>使用 MigrationOperation
---------------------------
你也可以定义自己的`MigrationOperation`来解耦自定义操作，以避免对具体 SQL 语句的依赖。 然后该操作被传递给具体的数据库提供程序以便其生成合适的 SQL 语句。

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

通过这个方式，扩展方法内只需要将该自定义操作添加到`MigrationBuilder.Operations`中即可。

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

这种方法需要数据库提供程序知道如何在其`IMigrationsSqlGenerator`服务当中为此操作去生成 SQL 。 下面是一个重写 SQL Server 的生成器以处理新操作的示例。

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

用新的迁移生成器替换默认的 sql 迁移生成器服务。

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
