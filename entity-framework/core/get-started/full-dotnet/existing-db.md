---
title: ".NET Framework 入门 - 现有数据库 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>通过现有数据库在 .NET Framework 上开始使用 EF Core

在本演练中，你将构建一个控制台应用程序，该程序使用 Entity Framework 对 Microsoft SQL Server 数据库执行基本数据访问。 你将使用反向工程基于现有数据库创建 Entity Framework 模型。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb)。

## <a name="prerequisites"></a>先决条件

完成本演练需要以下先决条件：

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [最新版本的 NuGet 包管理器](https://dist.nuget.org/index.html)

* [最新版本的 Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [博客数据库](#blogging-database)

### <a name="blogging-database"></a>博客数据库

本教程使用 LocalDb 实例上的博客数据库作为现有数据库。

> [!TIP]  
> 如果你已在其他教程中创建了博客数据库，则可以跳过这些步骤。

* 打开 Visual Studio

* “工具”->“连接到数据库...”

* 选择“Microsoft SQL Server”，然后单击“继续”

* 输入“(localdb)\mssqllocaldb”作为服务器名称

* 输入“master”作为数据库名称，然后单击“确定”

* Master 数据库现在显示在“服务器资源管理器”的“数据连接”中

* 右键单击“服务器资源管理器”中的数据库，然后选择“新建查询”

* 将下面列出的脚本复制到查询编辑器中

* 右键单击查询编辑器，然后选择“执行”

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>创建新项目

* 打开 Visual Studio

* “文件”>“新建”>“项目...”

* 从左侧菜单中选择“模板”>“Visual C#”>“Windows”

* 选择“控制台应用程序”项目模板

* 确保面向 .NET Framework 4.5.1 或更高版本

* 为项目提供名称，然后单击“确定”

## <a name="install-entity-framework"></a>安装 Entity Framework

要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。 本演练使用 SQL Server。 有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。

* “工具”>“NuGet 包管理器”>“包管理器控制台”

* 运行 `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

若要从现有数据库启用反向工程，我们还需要安装几个其他程序包。

* 运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>对模型实施反向工程

现在是时候基于现有数据库创建 EF 模型了。

* “工具”–>“NuGet 包管理器”–>“包管理器控制台”

* 运行以下命令以从现有数据库创建模型

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

反向工程过程基于现有数据库的架构创建实体类和派生上下文。 实体类是简单的 C# 对象，代表要查询和保存的数据。

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

上下文表示与数据库的会话，并允许查询和保存实体类的实例。

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>使用模型

你现在可以使用模型执行数据访问。

* 打开 Program.cs

* 将此文件的内容替换为以下代码

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* “调试”>“开始执行(不调试)”

你将看到，把一个博客保存到数据库后，所有博客的详细信息都将显示在控制台中。

![图像](_static/output-existing-db.png)
