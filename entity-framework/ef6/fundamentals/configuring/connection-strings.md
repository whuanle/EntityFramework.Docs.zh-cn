---
title: 连接字符串和模型的 EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120556"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="d087c-102">连接字符串和模型</span><span class="sxs-lookup"><span data-stu-id="d087c-102">Connection strings and models</span></span>
<span data-ttu-id="d087c-103">本主题介绍实体框架如何发现哪个数据库连接，若要使用，以及如何更改它。</span><span class="sxs-lookup"><span data-stu-id="d087c-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="d087c-104">同时涉及本主题中使用 Code First 和 EF 设计器创建的模型。</span><span class="sxs-lookup"><span data-stu-id="d087c-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="d087c-105">通常的实体框架应用程序使用从 DbContext 派生的类。</span><span class="sxs-lookup"><span data-stu-id="d087c-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="d087c-106">此派生的类将调用的构造函数之一来控制 DbContext 基类上：</span><span class="sxs-lookup"><span data-stu-id="d087c-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="d087c-107">上下文将如何连接到数据库，也就是说，连接字符串的方式找到使用</span><span class="sxs-lookup"><span data-stu-id="d087c-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="d087c-108">是否将使用上下文计算使用 Code First 的模型或加载使用 EF 设计器创建模型</span><span class="sxs-lookup"><span data-stu-id="d087c-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="d087c-109">其他高级的选项</span><span class="sxs-lookup"><span data-stu-id="d087c-109">Additional advanced options</span></span>  

<span data-ttu-id="d087c-110">以下片段显示可以使用几种方式的 DbContext 构造函数。</span><span class="sxs-lookup"><span data-stu-id="d087c-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="d087c-111">按照约定使用 Code First 与连接</span><span class="sxs-lookup"><span data-stu-id="d087c-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="d087c-112">如果你尚未在应用程序中做任何其他配置，然后对 DbContext 调用无参数构造函数将导致 DbContext 在 Code First 模式下运行与创建的约定的数据库连接。</span><span class="sxs-lookup"><span data-stu-id="d087c-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="d087c-113">例如：</span><span class="sxs-lookup"><span data-stu-id="d087c-113">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="d087c-114">在此示例中 DbContext 使用你派生的上下文 class—Demo.EF.BloggingContext—as 数据库名称的命名空间限定的名称，并创建使用 SQL Express 或 LocalDB 此数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="d087c-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="d087c-115">如果安装了，则将使用 SQL Express。</span><span class="sxs-lookup"><span data-stu-id="d087c-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="d087c-116">Visual Studio 2010 包括 SQL Express 默认情况下和 Visual Studio 2012 和更高版本包含 LocalDB。</span><span class="sxs-lookup"><span data-stu-id="d087c-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="d087c-117">在安装期间，EntityFramework NuGet 包检查的数据库服务器可用。</span><span class="sxs-lookup"><span data-stu-id="d087c-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="d087c-118">NuGet 包然后将更新的配置文件，通过设置时创建连接的约定，代码优先使用的默认数据库服务器。</span><span class="sxs-lookup"><span data-stu-id="d087c-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="d087c-119">如果正在运行 SQL Express，将使用它。</span><span class="sxs-lookup"><span data-stu-id="d087c-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="d087c-120">如果不可用 SQL Express 然后 LocalDB 将注册为默认值改为。</span><span class="sxs-lookup"><span data-stu-id="d087c-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="d087c-121">如果它已包含的默认连接工厂的设置，将配置文件不进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="d087c-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="d087c-122">按约定和指定的数据库名称使用 Code First 与连接</span><span class="sxs-lookup"><span data-stu-id="d087c-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="d087c-123">如果你尚未在应用程序中做任何其他配置，使用你想要使用的数据库名称，然后调用 DbContext 上字符串构造函数将导致 DbContext 在 Code First 模式下运行与创建的约定的数据库的数据库连接该名称。</span><span class="sxs-lookup"><span data-stu-id="d087c-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="d087c-124">例如：</span><span class="sxs-lookup"><span data-stu-id="d087c-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="d087c-125">在此示例中 DbContext"BloggingDatabase"用作数据库名称，并创建此数据库使用 SQL Express （随 Visual Studio 2010 一起安装） 或 LocalDB （随 Visual Studio 2012 一起安装） 的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="d087c-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="d087c-126">如果安装了，则将使用 SQL Express。</span><span class="sxs-lookup"><span data-stu-id="d087c-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="d087c-127">使用 Code First 与 app.config/web.config 文件中的连接字符串</span><span class="sxs-lookup"><span data-stu-id="d087c-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="d087c-128">您可以选择将连接字符串放在你的 app.config 或 web.config 文件中。</span><span class="sxs-lookup"><span data-stu-id="d087c-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="d087c-129">例如：</span><span class="sxs-lookup"><span data-stu-id="d087c-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="d087c-130">这是告知 DbContext，若要使用 SQL Express 或 LocalDB 以外的数据库服务器的简单办法，上面的示例中指定的 SQL Server Compact Edition 数据库。</span><span class="sxs-lookup"><span data-stu-id="d087c-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="d087c-131">如果连接字符串的名称匹配的上下文 （有或没有命名空间限定） 名称然后它将找到通过 DbContext 时，使用无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="d087c-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="d087c-132">如果连接字符串名称不同于您的上下文的名称，然后可以告知 DbContext 通过将连接字符串名称传递给 DbContext 构造函数在代码优先模式下使用此连接。</span><span class="sxs-lookup"><span data-stu-id="d087c-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="d087c-133">例如：</span><span class="sxs-lookup"><span data-stu-id="d087c-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="d087c-134">或者，可以使用窗体"名称 =\<连接字符串名称\>"字符串传递给 DbContext 构造函数。</span><span class="sxs-lookup"><span data-stu-id="d087c-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="d087c-135">例如：</span><span class="sxs-lookup"><span data-stu-id="d087c-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="d087c-136">这种形式使显式预期要在配置文件中找到的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="d087c-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="d087c-137">如果找不到具有给定名称的连接字符串，将引发异常。</span><span class="sxs-lookup"><span data-stu-id="d087c-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="d087c-138">Database first 或 Model First app.config/web.config 文件中的连接字符串</span><span class="sxs-lookup"><span data-stu-id="d087c-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="d087c-139">使用 EF 设计器创建的模型的不同于 Code First，在于您的模型已存在且应用程序运行时不生成代码。</span><span class="sxs-lookup"><span data-stu-id="d087c-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="d087c-140">模型通常作为你的项目中的 EDMX 文件存在。</span><span class="sxs-lookup"><span data-stu-id="d087c-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="d087c-141">在设计器会将 EF 连接字符串添加到 app.config 或 web.config 文件。</span><span class="sxs-lookup"><span data-stu-id="d087c-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="d087c-142">此连接字符串很特殊，因为它包含有关如何在 EDMX 文件中查找信息的信息。</span><span class="sxs-lookup"><span data-stu-id="d087c-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="d087c-143">例如：</span><span class="sxs-lookup"><span data-stu-id="d087c-143">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="d087c-144">在 EF 设计器还会生成告知 DbContext 通过将连接字符串名称传递给 DbContext 构造函数使用此连接的代码。</span><span class="sxs-lookup"><span data-stu-id="d087c-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="d087c-145">例如：</span><span class="sxs-lookup"><span data-stu-id="d087c-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="d087c-146">DbContext 知道要加载现有模型 （而不是使用 Code First 计算从代码） 的连接字符串是包含要使用的模型的详细信息的 EF 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="d087c-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="d087c-147">DbContext 构造函数的其他选项</span><span class="sxs-lookup"><span data-stu-id="d087c-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="d087c-148">DbContext 类包含其他构造函数和启用一些更高级的方案的使用模式。</span><span class="sxs-lookup"><span data-stu-id="d087c-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="d087c-149">其中一些是：</span><span class="sxs-lookup"><span data-stu-id="d087c-149">Some of these are:</span></span>  

- <span data-ttu-id="d087c-150">DbModelBuilder 类可用于生成 Code First 模型，而实例化 DbContext 实例。</span><span class="sxs-lookup"><span data-stu-id="d087c-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="d087c-151">此操作的结果是 DbModel 对象。</span><span class="sxs-lookup"><span data-stu-id="d087c-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="d087c-152">然后可以将此 DbModel 对象传递给 DbContext 构造函数之一时已准备好创建 DbContext 实例。</span><span class="sxs-lookup"><span data-stu-id="d087c-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="d087c-153">而不是只是数据库或连接字符串名称，可以将完整的连接字符串传递到 DbContext。</span><span class="sxs-lookup"><span data-stu-id="d087c-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="d087c-154">默认情况下使用 System.Data.SqlClient 的提供程序; 使用此连接字符串这可以通过设置 IConnectionFactory 到上下文上的不同实现更改。Database.DefaultConnectionFactory。</span><span class="sxs-lookup"><span data-stu-id="d087c-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="d087c-155">通过将它传递到 DbContext 构造函数，可以使用现有的 DbConnection 对象。</span><span class="sxs-lookup"><span data-stu-id="d087c-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="d087c-156">如果连接对象实例的 EntityConnection，则在连接中指定的模型，会使用，而不是计算模型使用代码优先。</span><span class="sxs-lookup"><span data-stu-id="d087c-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="d087c-157">如果对象是其他类型的实例 — 例如，SqlConnection，则上下文将使用它的 Code First 模式。</span><span class="sxs-lookup"><span data-stu-id="d087c-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="d087c-158">可以将现有的 ObjectContext 传递给 DbContext 构造函数创建包装现有上下文的 DbContext。</span><span class="sxs-lookup"><span data-stu-id="d087c-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="d087c-159">这可以用于现有应用程序使用 ObjectContext，但其想要利用应用程序的某些部分中的 DbContext。</span><span class="sxs-lookup"><span data-stu-id="d087c-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
