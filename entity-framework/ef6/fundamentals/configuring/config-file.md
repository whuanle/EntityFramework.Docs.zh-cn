---
title: 配置文件设置的 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 949ad4f030205753c5fbf9b95f4d183d8c0d1fe7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490865"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="78f32-102">配置文件设置</span><span class="sxs-lookup"><span data-stu-id="78f32-102">Configuration File Settings</span></span>
<span data-ttu-id="78f32-103">实体框架允许要从配置文件指定设置的数量。</span><span class="sxs-lookup"><span data-stu-id="78f32-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="78f32-104">一般情况下 EF 遵循惯例优先于配置原则： 此帖子中讨论的所有设置都具有默认行为，只需担心如何更改设置时默认值就不再满足您的要求。</span><span class="sxs-lookup"><span data-stu-id="78f32-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="78f32-105">基于代码的替代方法</span><span class="sxs-lookup"><span data-stu-id="78f32-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="78f32-106">所有这些设置还可使用代码。</span><span class="sxs-lookup"><span data-stu-id="78f32-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="78f32-107">从我们引入了的 EF6[基于代码的配置](code-based.md)，其中提供了一种重要方法的应用代码中的配置。</span><span class="sxs-lookup"><span data-stu-id="78f32-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="78f32-108">在 EF6 之前仍可以从代码应用配置，但需要使用各种 Api 来配置不同的区域。</span><span class="sxs-lookup"><span data-stu-id="78f32-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="78f32-109">配置文件选项，而无需更新代码，在部署过程中轻松地更改这些设置。</span><span class="sxs-lookup"><span data-stu-id="78f32-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="78f32-110">实体框架配置部分</span><span class="sxs-lookup"><span data-stu-id="78f32-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="78f32-111">启动 EF4.1 后，可以设置为上下文使用的数据库初始值设定项**appSettings**配置文件的部分。</span><span class="sxs-lookup"><span data-stu-id="78f32-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="78f32-112">在 EF 4.3 我们引入了自定义**entityFramework**部分，以处理新的设置。</span><span class="sxs-lookup"><span data-stu-id="78f32-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="78f32-113">实体框架仍将识别数据库初始值设定项使用旧格式设置，但我们建议移到新的格式，在可能的情况。</span><span class="sxs-lookup"><span data-stu-id="78f32-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="78f32-114">**EntityFramework**部分为自动添加到你的项目的配置文件时安装 EntityFramework NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="78f32-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a><span data-ttu-id="78f32-115">连接字符串</span><span class="sxs-lookup"><span data-stu-id="78f32-115">Connection Strings</span></span>  

<span data-ttu-id="78f32-116">[此页](~/ef6/fundamentals/configuring/connection-strings.md)实体框架如何确定要使用的数据库提供更多详细信息包括在配置文件中的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="78f32-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="78f32-117">标准中的连接字符串转**connectionStrings**元素，并且不需要**entityFramework**部分。</span><span class="sxs-lookup"><span data-stu-id="78f32-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="78f32-118">代码首先基于模型使用普通 ADO.NET 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="78f32-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="78f32-119">例如：</span><span class="sxs-lookup"><span data-stu-id="78f32-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="78f32-120">EF 设计器基于模型使用特殊 EF 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="78f32-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="78f32-121">例如：</span><span class="sxs-lookup"><span data-stu-id="78f32-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="78f32-122">基于代码的配置类型 (EF6 及更高版本)</span><span class="sxs-lookup"><span data-stu-id="78f32-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="78f32-123">从 EF6 开始，可以指定要用于 ef DbConfiguration[基于代码的配置](code-based.md)在应用程序中。</span><span class="sxs-lookup"><span data-stu-id="78f32-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="78f32-124">在大多数情况下不需要指定此设置，如 EF 将自动发现你 DbConfiguration。</span><span class="sxs-lookup"><span data-stu-id="78f32-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="78f32-125">有关你可能需要在配置文件中指定 DbConfiguration 的详细信息，请参阅**移动 DbConfiguration**一部分[基于代码的配置](code-based.md)。</span><span class="sxs-lookup"><span data-stu-id="78f32-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="78f32-126">若要设置 DbConfiguration 类型，您指定的程序集限定的类型名称在**codeConfigurationType**元素。</span><span class="sxs-lookup"><span data-stu-id="78f32-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="78f32-127">程序集限定的名是命名空间限定的名称后, 跟一个逗号，然后该类型所在的程序集。</span><span class="sxs-lookup"><span data-stu-id="78f32-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="78f32-128">您可以选择指定程序集版本、 区域性和公钥标记。</span><span class="sxs-lookup"><span data-stu-id="78f32-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="78f32-129">EF 数据库提供程序 (EF6 及更高版本)</span><span class="sxs-lookup"><span data-stu-id="78f32-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="78f32-130">在 EF6 之前，数据库提供程序的特定于实体框架的部分必须是作为核心 ADO.NET 提供程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="78f32-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="78f32-131">从 EF6 开始，EF 特定部分现在管理，并单独注册。</span><span class="sxs-lookup"><span data-stu-id="78f32-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="78f32-132">通常情况下不需要将自己注册提供程序。</span><span class="sxs-lookup"><span data-stu-id="78f32-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="78f32-133">通常这会在提供程序时安装它。</span><span class="sxs-lookup"><span data-stu-id="78f32-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="78f32-134">通过包括注册提供程序**提供程序**元素下的**提供程序**的子部分**entityFramework**部分。</span><span class="sxs-lookup"><span data-stu-id="78f32-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="78f32-135">有两个所需的属性提供程序条目：</span><span class="sxs-lookup"><span data-stu-id="78f32-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="78f32-136">**invariantName**标识核心 ADO.NET 提供程序，此 EF 提供程序目标</span><span class="sxs-lookup"><span data-stu-id="78f32-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="78f32-137">**类型**是 EF 提供程序实现的程序集限定的类型名称</span><span class="sxs-lookup"><span data-stu-id="78f32-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="78f32-138">程序集限定的名是命名空间限定的名称后, 跟一个逗号，然后该类型所在的程序集。</span><span class="sxs-lookup"><span data-stu-id="78f32-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="78f32-139">您可以选择指定程序集版本、 区域性和公钥标记。</span><span class="sxs-lookup"><span data-stu-id="78f32-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="78f32-140">例如，下面是创建安装实体框架时注册的默认 SQL Server 提供程序的条目。</span><span class="sxs-lookup"><span data-stu-id="78f32-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="78f32-141">侦听器 (EF6.1 及更高版本)</span><span class="sxs-lookup"><span data-stu-id="78f32-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="78f32-142">启动与 ef6.1 结合使用可以在配置文件中注册侦听器。</span><span class="sxs-lookup"><span data-stu-id="78f32-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="78f32-143">拦截器，可以运行其他逻辑，当 EF 执行某些操作，如执行数据库查询打开连接，等等。</span><span class="sxs-lookup"><span data-stu-id="78f32-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="78f32-144">通过包括注册侦听器**侦听器**元素下的**侦听器**的子部分**entityFramework**部分。</span><span class="sxs-lookup"><span data-stu-id="78f32-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="78f32-145">例如，下面的配置注册内置**DatabaseLogger**将记录到控制台的所有数据库操作的侦听器。</span><span class="sxs-lookup"><span data-stu-id="78f32-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="78f32-146">日志记录到文件 (及更高版本 EF6.1) 的数据库操作</span><span class="sxs-lookup"><span data-stu-id="78f32-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="78f32-147">注册侦听器通过配置文件时，尤其是你想要将日志记录添加到现有应用程序来帮助调试问题。</span><span class="sxs-lookup"><span data-stu-id="78f32-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="78f32-148">**DatabaseLogger**支持通过提供作为构造函数参数的文件名称的文件的日志记录。</span><span class="sxs-lookup"><span data-stu-id="78f32-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="78f32-149">默认情况下，这将导致日志文件以启动该应用程序每次用新文件覆盖。</span><span class="sxs-lookup"><span data-stu-id="78f32-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="78f32-150">若要改为追加到日志文件已存在使用类似于：</span><span class="sxs-lookup"><span data-stu-id="78f32-150">To instead append to the log file if it already exists use something like:</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="78f32-151">有关其他信息**DatabaseLogger**和注册侦听器，请参阅博客文章[EF 6.1： 打开日志记录，无需重新编译](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)。</span><span class="sxs-lookup"><span data-stu-id="78f32-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="78f32-152">代码第一个默认连接工厂</span><span class="sxs-lookup"><span data-stu-id="78f32-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="78f32-153">配置部分，可指定代码优先应用来定位要用于上下文的数据库的默认连接工厂。</span><span class="sxs-lookup"><span data-stu-id="78f32-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="78f32-154">没有连接字符串已添加到上下文的配置文件时，才使用默认连接工厂。</span><span class="sxs-lookup"><span data-stu-id="78f32-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="78f32-155">安装 EF NuGet 包时默认连接工厂注册了指向 SQL Express 或 LocalDB，具体取决于哪一个已安装。</span><span class="sxs-lookup"><span data-stu-id="78f32-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="78f32-156">若要设置一个连接工厂，你指定的程序集限定的类型名称在**deafultConnectionFactory**元素。</span><span class="sxs-lookup"><span data-stu-id="78f32-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="78f32-157">程序集限定的名是命名空间限定的名称后, 跟一个逗号，然后该类型所在的程序集。</span><span class="sxs-lookup"><span data-stu-id="78f32-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="78f32-158">您可以选择指定程序集版本、 区域性和公钥标记。</span><span class="sxs-lookup"><span data-stu-id="78f32-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="78f32-159">下面是设置自己的默认连接工厂的示例：</span><span class="sxs-lookup"><span data-stu-id="78f32-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="78f32-160">上面的示例要求具有无参数构造函数的自定义工厂。</span><span class="sxs-lookup"><span data-stu-id="78f32-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="78f32-161">如果需要可以指定使用的构造函数参数**参数**元素。</span><span class="sxs-lookup"><span data-stu-id="78f32-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="78f32-162">例如，SqlCeConnectionFactory，包含在实体框架中，要求您提供给构造函数提供程序固定名称。</span><span class="sxs-lookup"><span data-stu-id="78f32-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="78f32-163">提供程序固定名称标识的 SQL Compact 你想要使用的版本。</span><span class="sxs-lookup"><span data-stu-id="78f32-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="78f32-164">下面的配置将导致要使用默认的 SQL Compact 版本 4.0 的上下文。</span><span class="sxs-lookup"><span data-stu-id="78f32-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="78f32-165">如果未设置默认连接工厂，Code First 将使用指向 SqlConnectionFactory `.\SQLEXPRESS`。</span><span class="sxs-lookup"><span data-stu-id="78f32-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="78f32-166">SqlConnectionFactory 还具有构造函数，您可以重写连接字符串的部分。</span><span class="sxs-lookup"><span data-stu-id="78f32-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="78f32-167">如果你想要使用的 SQL Server 实例而不`.\SQLEXPRESS`可以使用此构造函数来设置服务器。</span><span class="sxs-lookup"><span data-stu-id="78f32-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="78f32-168">下面的配置将导致代码优先，使用**我**没有明确的连接字符串设置的上下文。</span><span class="sxs-lookup"><span data-stu-id="78f32-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="78f32-169">默认情况下，假定构造函数自变量是字符串类型。</span><span class="sxs-lookup"><span data-stu-id="78f32-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="78f32-170">类型属性可用于更改此设置。</span><span class="sxs-lookup"><span data-stu-id="78f32-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="78f32-171">数据库初始值设定项</span><span class="sxs-lookup"><span data-stu-id="78f32-171">Database Initializers</span></span>  

<span data-ttu-id="78f32-172">在每个上下文的基础上配置数据库初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="78f32-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="78f32-173">可以在该配置文件中设置它们**上下文**元素。</span><span class="sxs-lookup"><span data-stu-id="78f32-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="78f32-174">此元素使用的程序集限定的名称来标识正在配置的上下文。</span><span class="sxs-lookup"><span data-stu-id="78f32-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="78f32-175">默认情况下，Code First 上下文配置为使用 CreateDatabaseIfNotExists 初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="78f32-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="78f32-176">没有**disableDatabaseInitialization**特性，可以在**上下文**可用于禁用数据库初始化的元素。</span><span class="sxs-lookup"><span data-stu-id="78f32-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="78f32-177">例如，下面的配置禁用数据库初始化 Blogging.BlogContext 上下文 MyAssembly.dll 中定义。</span><span class="sxs-lookup"><span data-stu-id="78f32-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="78f32-178">可以使用**databaseInitializer**要设置自定义初始值设定项元素。</span><span class="sxs-lookup"><span data-stu-id="78f32-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="78f32-179">构造函数参数作为默认连接工厂使用相同的语法。</span><span class="sxs-lookup"><span data-stu-id="78f32-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

<span data-ttu-id="78f32-180">您可以配置一个包含实体框架中的通用数据库初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="78f32-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="78f32-181">**类型**属性为泛型类型使用.NET Framework 格式。</span><span class="sxs-lookup"><span data-stu-id="78f32-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="78f32-182">例如，如果使用 Code First 迁移，你可以配置要使用自动迁移的数据库`MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>`初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="78f32-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
