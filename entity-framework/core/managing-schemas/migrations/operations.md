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
<a name="custom-migrations-operations"></a>自定义迁移操作
============================
MigrationBuilder API 允许你迁移期间，执行许多不同类型的操作，但它并不详尽。 但是，API 还会可扩展它允许你定义自己的操作。 有两种方法来扩展 API： 使用`Sql()`方法，或通过定义自定义`MigrationOperation`对象。

为了说明，让我们看一下实现创建使用每种方法的数据库用户的操作。 在我们迁移中，我们想要启用编写以下代码：

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>使用 MigrationBuilder.Sql()
----------------------------
实现自定义操作的最简单方法是定义扩展方法的调用`MigrationBuilder.Sql()`。
下面是生成相应的 Transact SQL 示例。

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

如果你迁移需要支持多个数据库提供程序，则可以使用`MigrationBuilder.ActiveProvider`属性。 下面是支持 Microsoft SQL Server 和 PostreSQL 一个示例。

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

此方法仅适用如果知道每个提供程序将应用你自定义操作的位置。

<a name="using-a-migrationoperation"></a>使用 MigrationOperation
---------------------------
若要分离 SQL 的自定义操作，你可以定义自己`MigrationOperation`来表示它。 然后，因此它可以确定相应的 SQL 生成，系统会操作传递给提供程序。

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

使用此方法，该扩展方法只需添加到这些操作之一`MigrationBuilder.Operations`。

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

此方法要求每个提供商联系，以了解如何在此操作生成 SQL 其`IMigrationsSqlGenerator`服务。 下面是示例重写以处理新的操作的 SQL Server 的生成器。

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

将替换为更新默认迁移 sql 生成器服务。

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
