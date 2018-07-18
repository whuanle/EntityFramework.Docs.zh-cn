---
title: 依赖项解析-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
caps.latest.revision: 3
ms.openlocfilehash: 88fd859b4f9a8069eeb08f32bb1d35ddcd21aec5
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120568"
---
# <a name="dependency-resolution"></a><span data-ttu-id="bb9d9-102">依赖项解析</span><span class="sxs-lookup"><span data-stu-id="bb9d9-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="bb9d9-103">**仅限 EF6 及更高版本** - 此页面中讨论的功能、API 等已引入实体框架 6。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="bb9d9-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="bb9d9-105">从 EF6 开始，实体框架包含用于获取它需要的服务的实现的通用机制。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="bb9d9-106">也就是说，EF 使用一些接口或基类的实例时它将要求提供的接口或基类的类的具体实现使用。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="bb9d9-107">这可以通过使用 IDbDependencyResolver 接口的实现：</span><span class="sxs-lookup"><span data-stu-id="bb9d9-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="bb9d9-108">GetService 方法通常由 EF 调用和由 IDbDependencyResolver EF 或应用程序提供的实现。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="bb9d9-109">类型参数调用时，是所请求的服务的接口或基类的类类型和键的对象为 null 或对象，用于提供有关所请求服务的上下文信息。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="bb9d9-110">这篇文章不包含有关如何实现 IDbDependencyResolver，完整的详细信息，但充当其 EF 调用 GetService 和键的对象的语义的每个服务类型 （即，接口和基类类类型） 的引用调用。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="bb9d9-111">本文档将保持最新状态添加其他服务时。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="bb9d9-112">解析的服务</span><span class="sxs-lookup"><span data-stu-id="bb9d9-112">Services resolved</span></span>  

<span data-ttu-id="bb9d9-113">除非另有说明返回任何对象必须是线程安全，因为它可以用作单一实例。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="bb9d9-114">在许多情况下，在这种情况下是一个工厂返回的对象工厂本身必须是线程安全，但从工厂返回的对象不需要是线程安全，因为从每次使用工厂请求的新实例。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="bb9d9-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="bb9d9-116">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-117">**返回对象**： 给定的上下文类型的数据库初始值设定项</span><span class="sxs-lookup"><span data-stu-id="bb9d9-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="bb9d9-118">**密钥**： 未使用; 将为 null</span><span class="sxs-lookup"><span data-stu-id="bb9d9-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="bb9d9-119">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="bb9d9-120">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="bb9d9-121">**返回对象**： 用于创建可用于迁移和其他操作，会导致创建，例如数据库初始值设定项使用的数据库创建数据库的 SQL 生成器工厂。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="bb9d9-122">**密钥**： 包含指定的类型将为其生成 SQL 数据库的 ADO.NET 提供程序固定名称的字符串。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="bb9d9-123">例如，SQL Server SQL 生成器返回密钥"System.Data.SqlClient"。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="bb9d9-124">有关在 EF6 中与提供程序相关的服务的详细信息，请参阅[EF6 提供程序模型](~/ef6/fundamentals/providers/provider-model.md)部分。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="bb9d9-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="bb9d9-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="bb9d9-126">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-127">**返回对象**： 要用于给定提供程序固定名称的 EF 提供程序</span><span class="sxs-lookup"><span data-stu-id="bb9d9-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="bb9d9-128">**密钥**： 包含指定的类型为其提供程序所需的数据库的 ADO.NET 提供程序固定名称的字符串。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="bb9d9-129">例如，SQL Server 提供程序返回密钥"System.Data.SqlClient"。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="bb9d9-130">有关在 EF6 中与提供程序相关的服务的详细信息，请参阅[EF6 提供程序模型](~/ef6/fundamentals/providers/provider-model.md)部分。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="bb9d9-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="bb9d9-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="bb9d9-132">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-133">**返回对象**： 将按照惯例 EF 创建数据库连接时使用的连接工厂。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="bb9d9-134">也就是说时没有连接或连接字符串被授予对 EF，并且可以在 app.config 或 web.config 中找到任何连接字符串，, 然后此服务用于创建连接按照约定。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="bb9d9-135">默认情况下，更改连接工厂可以允许 EF 使用不同类型的数据库 (例如，SQL Server Compact Edition)。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="bb9d9-136">**密钥**： 未使用; 将为 null</span><span class="sxs-lookup"><span data-stu-id="bb9d9-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="bb9d9-137">有关在 EF6 中与提供程序相关的服务的详细信息，请参阅[EF6 提供程序模型](~/ef6/fundamentals/providers/provider-model.md)部分。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="bb9d9-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="bb9d9-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="bb9d9-139">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-140">**返回对象**： 一种服务，可以从连接生成提供程序清单标记。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="bb9d9-141">通常通过两种方式使用此服务。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-141">This service is typically used in two ways.</span></span> <span data-ttu-id="bb9d9-142">首先，它可以用于避免 Code First 生成模型时，连接到数据库。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="bb9d9-143">其次，它可以用于强制代码优先，若要生成的模型特定的数据库版本--例如，若要强制进行 SQL Server 2005 的一个模型，即使有时使用 SQL Server 2008。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="bb9d9-144">**对象生存期**： 单一实例-相同的对象可能是使用多个时间同时由不同的线程</span><span class="sxs-lookup"><span data-stu-id="bb9d9-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="bb9d9-145">**密钥**： 未使用; 将为 null</span><span class="sxs-lookup"><span data-stu-id="bb9d9-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="bb9d9-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="bb9d9-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="bb9d9-147">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-148">**返回对象**： 一种服务，可以从给定的连接获取提供程序工厂。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="bb9d9-149">在.NET 4.5 提供程序是从连接可公开访问。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="bb9d9-150">.NET 4 上的此服务的默认实现使用某些试探方法查找匹配的提供程序。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="bb9d9-151">如果这些然后失败，此服务的新实现可以注册为提供相应的解决方法。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="bb9d9-152">**密钥**： 未使用; 将为 null</span><span class="sxs-lookup"><span data-stu-id="bb9d9-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="bb9d9-153">Func < DbContext，System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="bb9d9-154">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-155">**返回对象**： 一个工厂，它将生成给定上下文的模型缓存键。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="bb9d9-156">默认情况下，EF 缓存每个 DbContext 类型，每个提供程序的一个模型。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="bb9d9-157">此服务的不同实现可以用于将其他信息，如架构名称添加到缓存键。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="bb9d9-158">**密钥**： 未使用; 将为 null</span><span class="sxs-lookup"><span data-stu-id="bb9d9-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="bb9d9-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="bb9d9-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="bb9d9-160">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-161">**返回对象**： 向 geography 和 geometry 空间类型的基本 EF 提供程序支持的 EF 空间提供程序。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="bb9d9-162">**密钥**: DbSptialServices 要求的两种方式。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="bb9d9-163">首先，提供程序特定的空间服务请求使用 DbProviderInfo 对象 (其中包含固定条件名称和清单标记) 作为键。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="bb9d9-164">其次，DbSpatialServices 可以要求不含键。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="bb9d9-165">这用于解决"全局空间提供程序"在创建独立的 DbGeography 或 DbGeometry 类型时使用。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="bb9d9-166">有关在 EF6 中与提供程序相关的服务的详细信息，请参阅[EF6 提供程序模型](~/ef6/fundamentals/providers/provider-model.md)部分。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="bb9d9-167">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="bb9d9-168">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-169">**返回对象**： 工厂可以创建允许提供程序时对数据库执行查询和命令实现重试或其他行为的服务。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="bb9d9-170">如果未不提供任何实现，然后 EF 将只需执行命令并引发任何异常传播。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="bb9d9-171">SQL Server 的此服务用于提供的重试策略，这是针对基于云的数据库服务器，例如 SQL Azure 运行时特别有用。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="bb9d9-172">**密钥**: ExecutionStrategyKey 对象，其中包含提供程序固定名称和 （可选） 将用于执行策略的服务器名称。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="bb9d9-173">有关在 EF6 中与提供程序相关的服务的详细信息，请参阅[EF6 提供程序模型](~/ef6/fundamentals/providers/provider-model.md)部分。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="bb9d9-174">Func < DbConnection，字符串，System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="bb9d9-175">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-176">**返回对象**： 一个工厂，它允许一个提供程序来配置映射到 HistoryContext`__MigrationHistory`使用 EF 迁移的表。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="bb9d9-177">HistoryContext 是代码的第一个 DbContext，并且可以使用正常的 fluent API 来更改像表和列映射规范的名称进行配置。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="bb9d9-178">**密钥**： 未使用; 将为 null</span><span class="sxs-lookup"><span data-stu-id="bb9d9-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="bb9d9-179">有关在 EF6 中与提供程序相关的服务的详细信息，请参阅[EF6 提供程序模型](~/ef6/fundamentals/providers/provider-model.md)部分。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="bb9d9-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="bb9d9-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="bb9d9-181">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-182">**返回对象**： 要用于给定提供程序固定名称的 ADO.NET 提供程序。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="bb9d9-183">**密钥**： 包含 ADO.NET 提供程序固定名称的字符串</span><span class="sxs-lookup"><span data-stu-id="bb9d9-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="bb9d9-184">此服务不通常会更改直接由于的默认实现使用普通 ADO.NET 提供程序注册。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="bb9d9-185">有关在 EF6 中与提供程序相关的服务的详细信息，请参阅[EF6 提供程序模型](~/ef6/fundamentals/providers/provider-model.md)部分。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="bb9d9-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="bb9d9-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="bb9d9-187">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-188">**返回对象**： 一种服务，用于确定给定类型的 DbProviderFactory 的提供程序固定名称。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="bb9d9-189">此服务的默认实现使用 ADO.NET 提供程序注册。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="bb9d9-190">这意味着，如果因为 DbProviderFactory 正在解析由 EF，ADO.NET 提供程序未注册以正常方式，然后它将还需要解析此服务。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="bb9d9-191">**密钥**: DbProviderFactory 的固定名称是必需的实例。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="bb9d9-192">有关在 EF6 中与提供程序相关的服务的详细信息，请参阅[EF6 提供程序模型](~/ef6/fundamentals/providers/provider-model.md)部分。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="bb9d9-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="bb9d9-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="bb9d9-194">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-195">**返回对象**： 包含预生成的视图的程序集的缓存。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="bb9d9-196">替换通常用于让 EF 知道哪些程序集而无需执行任何发现包含预生成的视图。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="bb9d9-197">**密钥**： 未使用; 将为 null</span><span class="sxs-lookup"><span data-stu-id="bb9d9-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="bb9d9-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="bb9d9-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="bb9d9-199">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-200">**返回对象**： 由 EF 来改为复数形式和所名称的服务。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="bb9d9-201">默认情况下使用英语复数化服务。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="bb9d9-202">**密钥**： 未使用; 将为 null</span><span class="sxs-lookup"><span data-stu-id="bb9d9-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="bb9d9-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="bb9d9-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="bb9d9-204">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="bb9d9-205">**返回的对象**: 应用程序启动时应注册任何侦听器。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="bb9d9-206">请注意，这些对象使用 getservices 进行提取调用请求和返回的任何依赖关系解析程序的所有侦听器都会注册。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="bb9d9-207">**密钥**： 未使用; 将为 null。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="bb9d9-208">Func < System.Data.Entity.DbContext、 操作 < 字符串\>，System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="bb9d9-209">**版本引入了**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="bb9d9-210">**返回对象**： 将用于创建的数据库日志格式化程序将是一个工厂时使用的上下文。Database.Log 属性设置在给定的上下文。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="bb9d9-211">**密钥**： 未使用; 将为 null。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="bb9d9-212">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="bb9d9-213">**版本引入了**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="bb9d9-214">**返回对象**： 将用于当上下文不具有可访问的无参数构造函数时为迁移创建上下文实例的工厂。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="bb9d9-215">**密钥**: 工厂为其所需的派生 dbcontext 类型的类型对象。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="bb9d9-216">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="bb9d9-217">**版本引入了**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="bb9d9-218">**返回对象**： 一个工厂，它将用于创建序列化程序进行序列化强类型化自定义批注，以便它们能够序列化并在 Code First 迁移中使用 desterilized 为 XML。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="bb9d9-219">**密钥**： 正在批注的名称序列化或反序列化。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="bb9d9-220">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="bb9d9-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="bb9d9-221">**版本引入了**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="bb9d9-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="bb9d9-222">**返回对象**： 一个工厂，它将用于创建事务的处理程序，以便可以如处理提交失败的情况下应用特殊处理。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="bb9d9-223">**密钥**: ExecutionStrategyKey 对象，其中包含提供程序固定名称和 （可选） 将使用事务处理程序的服务器名称。</span><span class="sxs-lookup"><span data-stu-id="bb9d9-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
