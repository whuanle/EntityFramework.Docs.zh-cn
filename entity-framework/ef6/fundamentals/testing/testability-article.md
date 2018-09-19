---
title: 可测试性和 Entity Framework 4.0
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: aec177438004fd255bef85a5e5047cf6b5a6f782
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284039"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="401c6-102">可测试性和 Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="401c6-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="401c6-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="401c6-103">Scott Allen</span></span>

<span data-ttu-id="401c6-104">发布日期： 2010 年 5 月</span><span class="sxs-lookup"><span data-stu-id="401c6-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="401c6-105">介绍</span><span class="sxs-lookup"><span data-stu-id="401c6-105">Introduction</span></span>

<span data-ttu-id="401c6-106">本白皮书主要介绍并演示如何编写使用 ADO.NET Entity Framework 4.0 和 Visual Studio 2010 的可测试代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="401c6-107">本文不会尝试专注于特定的测试方法，如测试驱动设计 (TDD) 或行为驱动设计 (BDD)。</span><span class="sxs-lookup"><span data-stu-id="401c6-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="401c6-108">而是本白皮书将重点介绍如何编写使用 ADO.NET 实体框架，但就一直都易于找出并以自动方式测试的代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="401c6-109">我们将介绍便于测试中的数据访问方案，请参阅如何使用框架时应用这些模式的常见设计模式。</span><span class="sxs-lookup"><span data-stu-id="401c6-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="401c6-110">我们还将介绍框架，以了解这些功能可以在可测试代码中的工作方式的特定功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="401c6-111">可测试代码是什么？</span><span class="sxs-lookup"><span data-stu-id="401c6-111">What Is Testable Code?</span></span>

<span data-ttu-id="401c6-112">若要验证的组成部分软件使用自动化的单元测试的功能可使许多需要优势。</span><span class="sxs-lookup"><span data-stu-id="401c6-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="401c6-113">每个人都知道良好测试将减少在应用程序中，然后增加应用程序的质量-但将单元测试在位置远不止仅查找 bug 的软件缺陷的数量。</span><span class="sxs-lookup"><span data-stu-id="401c6-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="401c6-114">优良单元测试套件使开发团队可以节省时间并控制其创建的软件。</span><span class="sxs-lookup"><span data-stu-id="401c6-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="401c6-115">团队可以对现有代码、 重构、 重新设计，进行更改，并重组软件以满足新需求，并将新组件添加到应用程序中同时了解测试套件可以验证应用程序的行为。</span><span class="sxs-lookup"><span data-stu-id="401c6-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="401c6-116">单元测试是软件的快速反馈周期以推动变革和保留的复杂性增加时的可维护性的一部分。</span><span class="sxs-lookup"><span data-stu-id="401c6-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="401c6-117">单元测试但是造成一个弊端。</span><span class="sxs-lookup"><span data-stu-id="401c6-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="401c6-118">团队必须花些时间来创建和维护单元测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="401c6-119">与直接相关的创建这些测试所需的工作量**可测试性**的底层的软件。</span><span class="sxs-lookup"><span data-stu-id="401c6-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="401c6-120">若要测试的软件是如何简单？</span><span class="sxs-lookup"><span data-stu-id="401c6-120">How easy is the software to test?</span></span> <span data-ttu-id="401c6-121">使用可测试性记住设计软件的团队将比不可测试的软件的团队更快地创建有效的测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="401c6-122">Microsoft 设计了可测试性记住 ADO.NET Entity Framework 4.0 (EF4)。</span><span class="sxs-lookup"><span data-stu-id="401c6-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="401c6-123">这并不意味着开发人员将编写单元测试框架代码本身。</span><span class="sxs-lookup"><span data-stu-id="401c6-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="401c6-124">相反，EF4 的可测试性目标更轻松创建可测试框架的基础上构建的代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="401c6-125">我们查看具体示例之前，因此值得了解可测试代码的质量。</span><span class="sxs-lookup"><span data-stu-id="401c6-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="401c6-126">可测试代码的质量</span><span class="sxs-lookup"><span data-stu-id="401c6-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="401c6-127">可以轻松测试的代码将始终具有至少两个对特性。</span><span class="sxs-lookup"><span data-stu-id="401c6-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="401c6-128">首先，可测试代码很容易**观察**。</span><span class="sxs-lookup"><span data-stu-id="401c6-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="401c6-129">给定输入某些组，它应该很容易观察代码的输出。</span><span class="sxs-lookup"><span data-stu-id="401c6-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="401c6-130">例如，测试下面的方法非常简单的因为该方法直接返回的计算结果。</span><span class="sxs-lookup"><span data-stu-id="401c6-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="401c6-131">测试一种方法是很困难，如果该方法将计算出的值写入到网络套接字、 数据库表或类似下面的代码文件。</span><span class="sxs-lookup"><span data-stu-id="401c6-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="401c6-132">测试必须执行额外的工作，检索的值。</span><span class="sxs-lookup"><span data-stu-id="401c6-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="401c6-133">其次，可测试代码很容易**隔离**。</span><span class="sxs-lookup"><span data-stu-id="401c6-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="401c6-134">让我们使用下面的伪代码作为可测试代码的错误示例。</span><span class="sxs-lookup"><span data-stu-id="401c6-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

<span data-ttu-id="401c6-135">该方法很容易注意 – 我们可以传入保险政策并验证返回值与预期的结果相匹配。</span><span class="sxs-lookup"><span data-stu-id="401c6-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="401c6-136">但是，若要测试的方法我们将需要使用正确的架构中，安装数据库和配置 SMTP 服务器，如果该方法尝试将发送一封电子邮件。</span><span class="sxs-lookup"><span data-stu-id="401c6-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="401c6-137">单元测试仅想要验证在方法中，计算逻辑，但测试可能会失败，因为电子邮件服务器处于脱机状态，或数据库服务器移动的。</span><span class="sxs-lookup"><span data-stu-id="401c6-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="401c6-138">这些故障的这两个测试要验证的行为与不相关。</span><span class="sxs-lookup"><span data-stu-id="401c6-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="401c6-139">行为是难以隔离。</span><span class="sxs-lookup"><span data-stu-id="401c6-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="401c6-140">软件开发人员努力编写可测试的代码通常致力于保持的它们编写代码中的关注点分离。</span><span class="sxs-lookup"><span data-stu-id="401c6-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="401c6-141">上述方法应专注于业务计算并委托给其他组件的数据库和电子邮件实现详细信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="401c6-142">Robert C.Martin 调用此单一责任原则。</span><span class="sxs-lookup"><span data-stu-id="401c6-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="401c6-143">对象应封装单个、 有限的责任，如计算策略的值。</span><span class="sxs-lookup"><span data-stu-id="401c6-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="401c6-144">所有其他数据库和通知的工作应该是其他某个对象的责任。</span><span class="sxs-lookup"><span data-stu-id="401c6-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="401c6-145">以这种方式编写的代码很容易隔离因为它侧重于单个任务。</span><span class="sxs-lookup"><span data-stu-id="401c6-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="401c6-146">在.NET 中我们需要遵循单一责任原则，并实现隔离的抽象。</span><span class="sxs-lookup"><span data-stu-id="401c6-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="401c6-147">我们可以使用接口定义并强制代码以使用接口抽象，而不是具体的类型。</span><span class="sxs-lookup"><span data-stu-id="401c6-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="401c6-148">在本文中我们将看到如何像上面的错误示例可以使用的方法的接口*查找*像它们将与数据库进行通信。</span><span class="sxs-lookup"><span data-stu-id="401c6-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="401c6-149">在测试时，但是，我们可以替换虚拟实现，不会与数据库通信，但改为在内存中保存数据。</span><span class="sxs-lookup"><span data-stu-id="401c6-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="401c6-150">此虚拟实现将隔离不相关的问题中的数据访问代码或数据库配置中的代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="401c6-151">还有其他优点到隔离。</span><span class="sxs-lookup"><span data-stu-id="401c6-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="401c6-152">中的最后一个方法的商业计算应只需几毫秒，若要执行，但测试本身可能会运行几秒钟作为代码周围的网络和访谈的跳转到各种服务器。</span><span class="sxs-lookup"><span data-stu-id="401c6-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="401c6-153">运行单元测试应快速以便于较小的更改。</span><span class="sxs-lookup"><span data-stu-id="401c6-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="401c6-154">单元测试应也是可重复和不失败，因为测试与无关的组件有问题。</span><span class="sxs-lookup"><span data-stu-id="401c6-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="401c6-155">编写易于观察和隔离的代码意味着，开发人员需要更轻松地编写代码的测试，减少测试执行，正在等待的时间和更多重要的是，花费更少时间来跟踪不存在的 bug。</span><span class="sxs-lookup"><span data-stu-id="401c6-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="401c6-156">希望你可以有哪些优势测试并了解可测试代码表现出的质量。</span><span class="sxs-lookup"><span data-stu-id="401c6-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="401c6-157">我们正准备解决如何编写代码，它使用 EF4 若要将数据保存到数据库，同时保持可观察和可以轻松隔离，但首先我们将缩小范围关注讨论数据访问的可测试的设计。</span><span class="sxs-lookup"><span data-stu-id="401c6-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="401c6-158">数据暂留的设计模式</span><span class="sxs-lookup"><span data-stu-id="401c6-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="401c6-159">两个错误示例前面提供了太多责任。</span><span class="sxs-lookup"><span data-stu-id="401c6-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="401c6-160">第一个错误示例必须执行一个计算*和*写入文件。</span><span class="sxs-lookup"><span data-stu-id="401c6-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="401c6-161">第二个错误示例不得不从数据库中读取数据*并*执行业务计算*和*发送电子邮件。</span><span class="sxs-lookup"><span data-stu-id="401c6-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="401c6-162">通过设计的分离关注点和责任委托给其他组件的较小方法将使向编写可测试代码的巨大进步。</span><span class="sxs-lookup"><span data-stu-id="401c6-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="401c6-163">目标是生成通过编写从小且集中的抽象操作的功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="401c6-164">当它涉及数据持久性较小，我们正在寻找的已设定焦点的抽象是很少它们已记录了作为设计模式。</span><span class="sxs-lookup"><span data-stu-id="401c6-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="401c6-165">Martin Fowler 书模式的企业应用程序体系结构已介绍了这些模式中打印的第一个工作。</span><span class="sxs-lookup"><span data-stu-id="401c6-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="401c6-166">之前我们介绍这些 ADO.NET 实体框架如何实现和使用这些模式，我们将提供以下各节中的这些模式的简短说明。</span><span class="sxs-lookup"><span data-stu-id="401c6-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="401c6-167">存储库模式</span><span class="sxs-lookup"><span data-stu-id="401c6-167">The Repository Pattern</span></span>

<span data-ttu-id="401c6-168">Fowler 说，一个存储库"之间进行调停的域和数据访问域对象使用类似于集合的接口映射层"。</span><span class="sxs-lookup"><span data-stu-id="401c6-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="401c6-169">存储库模式的目标是找出代码中的数据访问的繁琐细节上，正如我们看到的更早的隔离可测试性所需的特征。</span><span class="sxs-lookup"><span data-stu-id="401c6-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="401c6-170">隔离的关键是如何存储库中公开使用类似于集合的接口的对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="401c6-171">为使用存储库并不知道如何编写的逻辑存储库中将具体化你请求的对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="401c6-172">存储库可能会联系到数据库，或者它可能只是返回从内存中集合的对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="401c6-173">你的代码需要知道的就是存储库中显示维护集合，并且可以检索、 添加和从集合中删除对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="401c6-174">在现有.NET 应用程序中的具体的存储库通常继承自泛型接口如下所示：</span><span class="sxs-lookup"><span data-stu-id="401c6-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="401c6-175">当我们为 ef4 仍提供实现，但基本概念保持不变时，我们将进行少量更改的接口定义。</span><span class="sxs-lookup"><span data-stu-id="401c6-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="401c6-176">代码可以使用实现此接口的具体存储库来通过其主键值，若要检索的实体基于谓词的计算结果集合中检索实体，或只需检索所有可用的实体。</span><span class="sxs-lookup"><span data-stu-id="401c6-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="401c6-177">该代码还可以添加和删除通过存储库接口的实体。</span><span class="sxs-lookup"><span data-stu-id="401c6-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="401c6-178">给定的员工 IRepository 对象，代码可以执行以下操作。</span><span class="sxs-lookup"><span data-stu-id="401c6-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="401c6-179">因为代码使用接口 （IRepository 的员工），我们可以具有不同的接口的实现提供的代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="401c6-180">一种实现可能是由 EF4 和保持对象到 Microsoft SQL Server 数据库实现。</span><span class="sxs-lookup"><span data-stu-id="401c6-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="401c6-181">（一个我们在测试期间使用） 的不同实现可能受内存中的员工列表对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="401c6-182">该接口将有助于实现代码中的隔离。</span><span class="sxs-lookup"><span data-stu-id="401c6-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="401c6-183">请注意 IRepository&lt;T&gt;接口不会公开保存操作。</span><span class="sxs-lookup"><span data-stu-id="401c6-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="401c6-184">我们如何更新现有对象？</span><span class="sxs-lookup"><span data-stu-id="401c6-184">How do we update existing objects?</span></span> <span data-ttu-id="401c6-185">您可能会遇到 IRepository 定义包含保存操作，这些存储库的实现需要立即将对象保存到数据库。</span><span class="sxs-lookup"><span data-stu-id="401c6-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="401c6-186">但是，许多应用程序中我们不希望单独保存对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="401c6-187">相反，我们想要将对象，可能是从不同的存储库，修改一部分的业务活动，这些对象并稍后作为一个原子操作的一部分保存的所有对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="401c6-188">幸运的是，是一种模式以允许这种行为。</span><span class="sxs-lookup"><span data-stu-id="401c6-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="401c6-189">工作单元模式</span><span class="sxs-lookup"><span data-stu-id="401c6-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="401c6-190">Fowler 说的工作单元将"维护的对象列表受业务事务和协调弃用更改写入和并发问题的解决方法"。</span><span class="sxs-lookup"><span data-stu-id="401c6-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="401c6-191">它负责的工作要跟踪的对象，我们将从存储库并将我们对这些对象时的工作单元提交所做的更改，我们告诉任何更改持久保存的更改的单元。</span><span class="sxs-lookup"><span data-stu-id="401c6-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="401c6-192">它也是工作的要提取的新对象，我们已添加到所有存储库，并将对象插入到数据库，以及管理删除单元的责任。</span><span class="sxs-lookup"><span data-stu-id="401c6-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="401c6-193">如果你曾经使用 ADO.NET 数据集做过任何工作然后您将已经熟悉的工作模式单元。</span><span class="sxs-lookup"><span data-stu-id="401c6-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="401c6-194">ADO.NET 数据集必须能够跟踪我们的更新、 删除和插入的 DataRow 对象，可以 （使用 TableAdapter 的帮助） 协调所有我们更改到的数据库。</span><span class="sxs-lookup"><span data-stu-id="401c6-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="401c6-195">但是，数据集对象的模型已断开连接基础数据库的子集。</span><span class="sxs-lookup"><span data-stu-id="401c6-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="401c6-196">工作模式单元表现出相同的行为，但可处理业务对象，并独立于数据访问代码和不识别实例的数据库的域对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="401c6-197">一种抽象模型中的.NET 代码的工作单元可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="401c6-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="401c6-198">通过公开来自我们可以确保单个工作单元的工作单元的存储库引用对象具有跟踪业务事务期间实例化的所有实体的功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="401c6-199">实际单元提交方法的实现是工作的所有奇迹的地方来协调与数据库的内存中的更改。</span><span class="sxs-lookup"><span data-stu-id="401c6-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="401c6-200">给定 IUnitOfWork 引用，代码可以检索到的业务对象从一个或多个存储库进行更改并保存使用原子提交操作的所有更改。</span><span class="sxs-lookup"><span data-stu-id="401c6-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="401c6-201">延迟加载模式</span><span class="sxs-lookup"><span data-stu-id="401c6-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="401c6-202">Fowler 使用名称延迟加载来描述"一个对象不包含的所有数据，您需要但知道如何获取它"。</span><span class="sxs-lookup"><span data-stu-id="401c6-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="401c6-203">透明延迟加载是重要的功能时编写可测试性的业务代码和使用关系数据库。</span><span class="sxs-lookup"><span data-stu-id="401c6-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="401c6-204">作为示例，请考虑下面的代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="401c6-205">如何填充时间卡片集合？</span><span class="sxs-lookup"><span data-stu-id="401c6-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="401c6-206">有两个可能的答案。</span><span class="sxs-lookup"><span data-stu-id="401c6-206">There are two possible answers.</span></span> <span data-ttu-id="401c6-207">一个答案是员工存储库，当系统询问提取一名员工，发出一个查询来检索员工以及员工的相关联的考勤卡信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="401c6-208">这通常需要具有对 JOIN 子句的查询，并可能会导致比应用程序的详细信息中检索的关系数据库中需要。</span><span class="sxs-lookup"><span data-stu-id="401c6-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="401c6-209">如果应用程序永远不需要接触时间卡片属性？</span><span class="sxs-lookup"><span data-stu-id="401c6-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="401c6-210">第二个答案是加载时间卡片属性"按需"。</span><span class="sxs-lookup"><span data-stu-id="401c6-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="401c6-211">此延迟加载是隐式和透明的业务逻辑，因为该代码不会调用特殊的 Api 来检索考勤卡信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="401c6-212">该代码假定在需要时存在考勤卡信息时。</span><span class="sxs-lookup"><span data-stu-id="401c6-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="401c6-213">所涉及的通常涉及到运行时拦截的方法调用的延迟加载是一些奇妙的功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="401c6-214">截取代码负责和数据库通信并检索考勤卡信息而使可用来为业务逻辑的业务逻辑。</span><span class="sxs-lookup"><span data-stu-id="401c6-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="401c6-215">这种延迟加载神奇允许从数据检索操作，并生成更多可测试代码中隔离本身业务代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="401c6-216">延迟加载的缺点是，当应用程序*does*需要该代码将执行一个额外查询的考勤卡信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="401c6-217">这不是问题的许多应用程序，但对于性能敏感型应用程序或应用程序循环遍历多个员工的对象并执行查询以在每次迭代循环 （问题通常称为 N + 1 的过程中检索时间卡片查询问题），延迟加载是拖动。</span><span class="sxs-lookup"><span data-stu-id="401c6-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="401c6-218">在这些情况下应用程序可能想要预先加载考勤卡信息可能最高效的方式。</span><span class="sxs-lookup"><span data-stu-id="401c6-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="401c6-219">幸运的是，我们将了解如何 EF4 支持这两个隐式延迟加载和高效的预先加载，因为我们将移动到下一节，并实现这些模式。</span><span class="sxs-lookup"><span data-stu-id="401c6-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="401c6-220">使用实体框架的实现模式</span><span class="sxs-lookup"><span data-stu-id="401c6-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="401c6-221">好消息是所有我们在上一部分中所述的设计模式是直接使用 EF4 时实现。</span><span class="sxs-lookup"><span data-stu-id="401c6-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="401c6-222">为了演示，我们将要使用一个简单的 ASP.NET MVC 应用程序来编辑和显示员工和其关联的考勤卡信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="401c6-223">我们首先使用以下"plain old CLR objects"(Poco)。</span><span class="sxs-lookup"><span data-stu-id="401c6-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

<span data-ttu-id="401c6-224">我们将介绍不同的方法和功能的 EF4，但其目的是为持久性未知 (PI) 尽可能保留以下类，这些类定义将略有变化。</span><span class="sxs-lookup"><span data-stu-id="401c6-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="401c6-225">PI 对象并不知道*如何*，或甚至*如果*，它包含的状态存在于数据库内。</span><span class="sxs-lookup"><span data-stu-id="401c6-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="401c6-226">PI 和 Poco 来了解一下通过可测试软件。</span><span class="sxs-lookup"><span data-stu-id="401c6-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="401c6-227">使用 POCO 方法的对象是不太受约束、 更灵活和易于测试，因为这些服务器可以运行不含数据库存在。</span><span class="sxs-lookup"><span data-stu-id="401c6-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="401c6-228">使用就地 Poco 我们可以创建实体数据模型 (EDM) 在 Visual Studio 中 （请参阅图 1）。</span><span class="sxs-lookup"><span data-stu-id="401c6-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="401c6-229">我们不会使用 EDM 来为实体生成代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="401c6-230">相反，我们想要使用我们 lovingly 手动创建的实体。</span><span class="sxs-lookup"><span data-stu-id="401c6-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="401c6-231">我们仅将使用 EDM 生成数据库架构，并提供 EF4 需要将对象映射到数据库的元数据。</span><span class="sxs-lookup"><span data-stu-id="401c6-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![ef test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="401c6-233">**图 1**</span><span class="sxs-lookup"><span data-stu-id="401c6-233">**Figure 1**</span></span>

<span data-ttu-id="401c6-234">注意： 如果你想要首先开发的 EDM 模型，就可以以干净的生成 POCO 代码从 EDM。</span><span class="sxs-lookup"><span data-stu-id="401c6-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="401c6-235">可以使用数据可编程性团队提供的 Visual Studio 2010 扩展来执行此操作。</span><span class="sxs-lookup"><span data-stu-id="401c6-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="401c6-236">若要下载该扩展，启动 Visual Studio 中的工具菜单中的扩展管理器，并搜索"POCO"（请参阅图 2） 的模板的联机库。</span><span class="sxs-lookup"><span data-stu-id="401c6-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="401c6-237">有几个 POCO 模板可用于 EF。</span><span class="sxs-lookup"><span data-stu-id="401c6-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="401c6-238">有关使用模板的详细信息，请参阅"[演练： POCO 实体框架的模板](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)"。</span><span class="sxs-lookup"><span data-stu-id="401c6-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![ef test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="401c6-240">**图 2**</span><span class="sxs-lookup"><span data-stu-id="401c6-240">**Figure 2**</span></span>

<span data-ttu-id="401c6-241">从起始点此 POCO 会探讨两个不同的方法可测试的代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="401c6-242">第一种方法，我调用 EF 方法因为它充分利用从 Entity Framework API 来实现工作和存储库的单位的抽象。</span><span class="sxs-lookup"><span data-stu-id="401c6-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="401c6-243">在第二种方法中我们将创建我们自己自定义存储库的抽象，并查看每种方法的优缺点。</span><span class="sxs-lookup"><span data-stu-id="401c6-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="401c6-244">我们将开始通过探索 EF 方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="401c6-245">EF 为中心实现</span><span class="sxs-lookup"><span data-stu-id="401c6-245">An EF Centric Implementation</span></span>

<span data-ttu-id="401c6-246">请考虑以下控制器操作从 ASP.NET MVC 项目。</span><span class="sxs-lookup"><span data-stu-id="401c6-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="401c6-247">操作检索 Employee 对象并返回要显示的员工的详细的视图的结果。</span><span class="sxs-lookup"><span data-stu-id="401c6-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="401c6-248">是可测试的代码？</span><span class="sxs-lookup"><span data-stu-id="401c6-248">Is the code testable?</span></span> <span data-ttu-id="401c6-249">有至少两个测试，我们需要验证操作的行为。</span><span class="sxs-lookup"><span data-stu-id="401c6-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="401c6-250">首先，我们想要验证操作会返回正确的视图中 – 轻松进行测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="401c6-251">我们还想要编写一个测试，以验证操作中检索正确的员工，以及我们想要执行此操作无需执行代码以查询数据库。</span><span class="sxs-lookup"><span data-stu-id="401c6-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="401c6-252">请记住我们想要隔离受测代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="401c6-253">隔离可确保测试不会因数据访问代码或数据库配置中的 bug 而失败。</span><span class="sxs-lookup"><span data-stu-id="401c6-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="401c6-254">如果测试失败，我们将知道我们有一个 bug，在控制器逻辑，而不是在一些较低级别系统组件。</span><span class="sxs-lookup"><span data-stu-id="401c6-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="401c6-255">若要实现的隔离，我们将存储库和工作单元的前面需要一些抽象，例如，我们演示了接口。</span><span class="sxs-lookup"><span data-stu-id="401c6-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="401c6-256">请记住存储库模式的设计域对象和数据映射层之间进行协调。</span><span class="sxs-lookup"><span data-stu-id="401c6-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="401c6-257">在此方案中 EF4*是*映射的数据层，并已经提供了一个名为 IObjectSet 的类似于存储库的抽象&lt;T&gt; （从 System.Data.Objects 命名空间）。</span><span class="sxs-lookup"><span data-stu-id="401c6-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="401c6-258">接口定义如下所示。</span><span class="sxs-lookup"><span data-stu-id="401c6-258">The interface definition looks like the following.</span></span>

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

<span data-ttu-id="401c6-259">IObjectSet&lt;T&gt;符合要求的存储库，因为它类似于对象的集合 (通过 IEnumerable&lt;T&gt;) 并提供方法来添加和删除对象从模拟的集合。</span><span class="sxs-lookup"><span data-stu-id="401c6-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="401c6-260">附加和分离方法公开 EF4 API 的其他的功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="401c6-261">若要使用 IObjectSet&lt;T&gt;我们需要工作抽象单位以将存储库绑定在一起作为存储库的接口。</span><span class="sxs-lookup"><span data-stu-id="401c6-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="401c6-262">此接口的一个具体实现将与 SQL Server 进行通信，并可以轻松地创建使用从 EF4 的 ObjectContext 类。</span><span class="sxs-lookup"><span data-stu-id="401c6-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="401c6-263">ObjectContext 类是工作的真正的 EF4 API 中单元。</span><span class="sxs-lookup"><span data-stu-id="401c6-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

<span data-ttu-id="401c6-264">将 IObjectSet&lt;T&gt;生动非常简单，只调用 CreateObjectSet ObjectContext 对象的方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="401c6-265">在后台，框架将使用元数据，我们提供在 EDM 中生成具体 ObjectSet&lt;T&gt;。</span><span class="sxs-lookup"><span data-stu-id="401c6-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="401c6-266">我们只讨论与返回 IObjectSet&lt;T&gt;接口，因为它有助于保持客户端代码中的可测试性。</span><span class="sxs-lookup"><span data-stu-id="401c6-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="401c6-267">此具体实现可在生产中，但我们需要关注如何我们将使用我们 IUnitOfWork 抽象以便进行测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="401c6-268">Test Double</span><span class="sxs-lookup"><span data-stu-id="401c6-268">The Test Doubles</span></span>

<span data-ttu-id="401c6-269">若要隔离的控制器操作我们将需要在实际工作单元中 （由 ObjectContext） 和一个测试双精度或"假"工作单元 （执行内存中操作） 之间进行切换的功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="401c6-270">若要执行此类型的切换的常用方法是工作的工作的不让 MVC 控制器实例单元，而是工作的工作的化插入控制器作为构造函数参数传递单元。</span><span class="sxs-lookup"><span data-stu-id="401c6-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="401c6-271">上面的代码是依赖关系注入的示例。</span><span class="sxs-lookup"><span data-stu-id="401c6-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="401c6-272">我们不允许创建它的依赖项 （工作单元），但将依赖项注入到控制器的控制器。</span><span class="sxs-lookup"><span data-stu-id="401c6-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="401c6-273">在 MVC 项目中是通常使用自定义控制器工厂结合使用控制反转 (IoC) 容器来自动执行依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="401c6-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="401c6-274">这些主题不在本文中，范围，但可以在本文末尾的引用以下更通过读取。</span><span class="sxs-lookup"><span data-stu-id="401c6-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="401c6-275">我们可用于测试的工作实现一个虚设单元可能如下所示。</span><span class="sxs-lookup"><span data-stu-id="401c6-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

<span data-ttu-id="401c6-276">请注意虚设的工作单元公开提交属性。</span><span class="sxs-lookup"><span data-stu-id="401c6-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="401c6-277">有时它可用于将功能添加到便于测试虚设类。</span><span class="sxs-lookup"><span data-stu-id="401c6-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="401c6-278">在这种情况下很容易注意是否代码通过检查已提交属性提交的工作单元。</span><span class="sxs-lookup"><span data-stu-id="401c6-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="401c6-279">我们还需要假 IObjectSet&lt;T&gt;在内存中保留对象的员工和时间卡片。</span><span class="sxs-lookup"><span data-stu-id="401c6-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="401c6-280">我们可以提供一个实现代码使用泛型。</span><span class="sxs-lookup"><span data-stu-id="401c6-280">We can provide a single implementation using generics.</span></span>

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

<span data-ttu-id="401c6-281">这个 test double 委托其大部分的工作为基础的 HashSet&lt;T&gt;对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="401c6-282">请注意该 IObjectSet&lt;T&gt;需要作为类 （引用类型），强制 T 的泛型约束，并且还会强制实现 IQueryable&lt;T&gt;。</span><span class="sxs-lookup"><span data-stu-id="401c6-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="401c6-283">很容易地使内存中集合显示为 IQueryable&lt;T&gt;使用标准 LINQ 运算符 AsQueryable。</span><span class="sxs-lookup"><span data-stu-id="401c6-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="401c6-284">测试</span><span class="sxs-lookup"><span data-stu-id="401c6-284">The Tests</span></span>

<span data-ttu-id="401c6-285">传统单元测试将使用单个测试类在一个 MVC 控制器中保存的所有测试的所有操作。</span><span class="sxs-lookup"><span data-stu-id="401c6-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="401c6-286">我们可以编写这些测试或任何类型的单元测试时，使用内存中 fakes 我们构建了。</span><span class="sxs-lookup"><span data-stu-id="401c6-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="401c6-287">但是，本文中我们将避免将单一式应用测试类的方式并改为组我们的测试可以专注于特定的一项功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="401c6-288">例如，"创建新员工"可能是我们要测试，因此我们将使用单个测试类若要验证单个控制器操作负责创建新的员工的功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="401c6-289">没有需要所有这些能够进行细化管理的测试类的一些常见设置代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="401c6-290">例如，我们始终需要创建我们的内存中存储库和虚设工作单元。</span><span class="sxs-lookup"><span data-stu-id="401c6-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="401c6-291">我们还需要使用虚设的注入的工作单元的员工控制器的实例。</span><span class="sxs-lookup"><span data-stu-id="401c6-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="401c6-292">通过使用基类，我们将在测试类之间分享此常见设置代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-292">We’ll share this common setup code across test classes by using a base class.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

<span data-ttu-id="401c6-293">我们在基类中使用"对象母亲"是用于创建测试数据的一种常见模式。</span><span class="sxs-lookup"><span data-stu-id="401c6-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="401c6-294">对象的母亲包含实例使用的测试实体化跨多个测试装置的工厂方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

<span data-ttu-id="401c6-295">我们可以使用 EmployeeControllerTestBase 作为基类的几个测试装置 （见图 3）。</span><span class="sxs-lookup"><span data-stu-id="401c6-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="401c6-296">每个测试固定例程将测试的特定控制器操作。</span><span class="sxs-lookup"><span data-stu-id="401c6-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="401c6-297">例如，一个测试固定例程将重点介绍测试期间 HTTP GET 请求 （若要显示用于创建员工视图），使用创建操作和不同装置将重点介绍使用 HTTP POST 请求中的创建操作 (才能提交的信息要为用户创建员工）。</span><span class="sxs-lookup"><span data-stu-id="401c6-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="401c6-298">每个派生的类只是负责在其特定上下文中，并提供验证其特定的测试上下文的结果所需的断言所需的设置。</span><span class="sxs-lookup"><span data-stu-id="401c6-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![ef test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="401c6-300">**图 3**</span><span class="sxs-lookup"><span data-stu-id="401c6-300">**Figure 3**</span></span>

<span data-ttu-id="401c6-301">此处提供的命名约定和测试样式不是必需的可测试代码，它是一种方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="401c6-302">图 4 显示了在 Jet 大脑 Resharper 中运行的测试用于 Visual Studio 2010 的测试运行程序插件。</span><span class="sxs-lookup"><span data-stu-id="401c6-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![ef test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="401c6-304">**图 4**</span><span class="sxs-lookup"><span data-stu-id="401c6-304">**Figure 4**</span></span>

<span data-ttu-id="401c6-305">有了基类来处理共享的设置代码，针对每个控制器操作的单元测试是尽可能小并且易于编写。</span><span class="sxs-lookup"><span data-stu-id="401c6-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="401c6-306">测试将会执行快速 （因为我们在执行内存中操作），并且不应因不相关的基础结构或环境问题而失败，（因为我们已独立测试的单元）。</span><span class="sxs-lookup"><span data-stu-id="401c6-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

<span data-ttu-id="401c6-307">在这些测试中的基类执行大部分设置工作。</span><span class="sxs-lookup"><span data-stu-id="401c6-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="401c6-308">请记住基类构造函数创建内存中存储库、 虚设的工作，单元和 EmployeeController 类的实例。</span><span class="sxs-lookup"><span data-stu-id="401c6-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="401c6-309">测试类从此基类派生，而侧重于测试的 Create 方法的详细信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="401c6-310">在这种情况下细节上看，这会看到任何单元测试过程中的"排列，操作和断言"步骤：</span><span class="sxs-lookup"><span data-stu-id="401c6-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="401c6-311">创建 newEmployee 对象，用于模拟传入的数据。</span><span class="sxs-lookup"><span data-stu-id="401c6-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="401c6-312">调用 EmployeeController 的 Create 操作，并传入 newEmployee。</span><span class="sxs-lookup"><span data-stu-id="401c6-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="401c6-313">验证创建操作产生预期的结果 （存储库中显示员工）。</span><span class="sxs-lookup"><span data-stu-id="401c6-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="401c6-314">我们已构建，我们可以测试任何 EmployeeController 操作。</span><span class="sxs-lookup"><span data-stu-id="401c6-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="401c6-315">例如，当我们编写测试员工控制器的 Index 操作时我们可以继承测试基类来建立我们的测试中的相同基本设置。</span><span class="sxs-lookup"><span data-stu-id="401c6-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="401c6-316">再次类的基类将创建内存中存储库、 虚设的工作单元，以及 EmployeeController 的一个实例。</span><span class="sxs-lookup"><span data-stu-id="401c6-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="401c6-317">索引操作的测试只需专注于调用 Index 操作并测试操作的模型的质量返回。</span><span class="sxs-lookup"><span data-stu-id="401c6-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

<span data-ttu-id="401c6-318">我们将使用内存中 fakes 创建的测试都是面向测试*状态*的软件。</span><span class="sxs-lookup"><span data-stu-id="401c6-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="401c6-319">例如，测试我们想要检查的存储库的状态后，将执行创建操作 – 创建操作时没有存储库中保存新员工？</span><span class="sxs-lookup"><span data-stu-id="401c6-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="401c6-320">稍后我们将介绍基于交互测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="401c6-321">如果待测试的代码调用我们的对象上的适当方法并传递正确的参数，将要求交互基于测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="401c6-322">现在我们来看看在封面上另一种设计模式 – 延迟加载。</span><span class="sxs-lookup"><span data-stu-id="401c6-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="401c6-323">预先加载和延迟加载</span><span class="sxs-lookup"><span data-stu-id="401c6-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="401c6-324">在某个时间点在 ASP.NET MVC web 应用程序，我们可能想要显示员工的信息，包括员工的关联时间卡片。</span><span class="sxs-lookup"><span data-stu-id="401c6-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="401c6-325">例如，我们可能会有考勤卡摘要显示在系统中显示该员工的姓名和总时间卡片。</span><span class="sxs-lookup"><span data-stu-id="401c6-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="401c6-326">有几种方法，我们可以实现此功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="401c6-327">投影</span><span class="sxs-lookup"><span data-stu-id="401c6-327">Projection</span></span>

<span data-ttu-id="401c6-328">一个简单的方法来创建摘要是以构造模型专用于我们想要在视图中显示的信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="401c6-329">在此方案中模型可能如下所示。</span><span class="sxs-lookup"><span data-stu-id="401c6-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="401c6-330">请注意，EmployeeSummaryViewModel 不是一个实体即它不是我们想要保留在数据库中的内容。</span><span class="sxs-lookup"><span data-stu-id="401c6-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="401c6-331">我们只会使用此类数据进行随机排列到视图中以强类型化方式。</span><span class="sxs-lookup"><span data-stu-id="401c6-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="401c6-332">视图模型是将数据传输对象 (DTO)，因为它不包含的任何行为 （无方法） – 仅属性等。</span><span class="sxs-lookup"><span data-stu-id="401c6-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="401c6-333">属性将保存，需要移动数据。</span><span class="sxs-lookup"><span data-stu-id="401c6-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="401c6-334">很容易地实例化此视图模型，使用 LINQ 的标准投影运算符 – 选择运算符。</span><span class="sxs-lookup"><span data-stu-id="401c6-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

<span data-ttu-id="401c6-335">有两个重要功能上面的代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-335">There are two notable features to the above code.</span></span> <span data-ttu-id="401c6-336">第一次 – 代码很容易地测试，因为仍然很容易观察和隔离。</span><span class="sxs-lookup"><span data-stu-id="401c6-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="401c6-337">Select 运算符对我们的内存中 fakes 有效也很好用，针对实际的工作单元一样。</span><span class="sxs-lookup"><span data-stu-id="401c6-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

<span data-ttu-id="401c6-338">第二个值得注意的功能是代码如何允许 EF4 生成单一、 高效的查询来一起组合员工和考勤卡信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="401c6-339">我们已加载员工信息和考勤卡信息到同一个对象而无需使用任何特殊的 Api。</span><span class="sxs-lookup"><span data-stu-id="401c6-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="401c6-340">该代码只被表示它需要使用标准 LINQ 运算符的工作内存中数据源以及为远程数据源的信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="401c6-341">EF4 无法转换表达式树由 LINQ 查询和 C 生成\#编译器转换为单个和高效的 T-SQL 查询。</span><span class="sxs-lookup"><span data-stu-id="401c6-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="401c6-342">有些的时候时我们不想要使用的视图模型或 DTO 对象，但真正的实体。</span><span class="sxs-lookup"><span data-stu-id="401c6-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="401c6-343">当我们知道我们需要某位员工*和*员工的时间卡片，我们可以自愿加载相关的数据中有非介入式且高效的方式。</span><span class="sxs-lookup"><span data-stu-id="401c6-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="401c6-344">显式预先加载</span><span class="sxs-lookup"><span data-stu-id="401c6-344">Explicit Eager Loading</span></span>

<span data-ttu-id="401c6-345">如果我们想要预先加载相关的实体信息的业务逻辑 （或在此方案中，控制器操作逻辑），我们需要某种途径来表达其需要向存储库。</span><span class="sxs-lookup"><span data-stu-id="401c6-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="401c6-346">EF4 ObjectQuery&lt;T&gt;类定义包含方法以指定要在查询过程中检索的相关的对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="401c6-347">请记住 EF4 ObjectContext 公开实体通过具体的 ObjectSet&lt;T&gt;类继承自 ObjectQuery&lt;T&gt;。</span><span class="sxs-lookup"><span data-stu-id="401c6-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="401c6-348">如果我们使用 ObjectSet&lt;T&gt;中我们可以编写以下代码到我们控制器操作的引用指定每个雇员的考勤卡信息的预先加载。</span><span class="sxs-lookup"><span data-stu-id="401c6-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="401c6-349">但是，由于我们正在尝试保持我们的代码可测试性我们不公开 ObjectSet&lt;T&gt;从外部工作类的实际单元。</span><span class="sxs-lookup"><span data-stu-id="401c6-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="401c6-350">相反，我们依赖 IObjectSet&lt;T&gt;接口的虚设，更容易，但 IObjectSet&lt;T&gt;不会定义 Include 方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="401c6-351">LINQ 的优点是，我们可以创建我们自己 Include 运算符。</span><span class="sxs-lookup"><span data-stu-id="401c6-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

<span data-ttu-id="401c6-352">请注意，此 Include 运算符定义为扩展方法的 IQueryable&lt;T&gt;而不是 IObjectSet&lt;T&gt;。</span><span class="sxs-lookup"><span data-stu-id="401c6-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="401c6-353">这使我们能够使用方法和广泛的可能的类型，包括 IQueryable&lt;T&gt;，IObjectSet&lt;T&gt;，ObjectQuery&lt;T&gt;，和 ObjectSet&lt;T&gt;。</span><span class="sxs-lookup"><span data-stu-id="401c6-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="401c6-354">如果基础序列不是正版 EF4 ObjectQuery&lt;T&gt;，则没有完成任何损害，Include 运算符不执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="401c6-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="401c6-355">如果基础序列*是*ObjectQuery&lt;T&gt; (或派生自 ObjectQuery&lt;T&gt;)，EF4 会请参阅我们的其他数据的要求并制定正确的 SQL查询。</span><span class="sxs-lookup"><span data-stu-id="401c6-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="401c6-356">此位置中的新运算符与我们可以显式请求考勤卡信息的预先加载从存储库。</span><span class="sxs-lookup"><span data-stu-id="401c6-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="401c6-357">针对实际 ObjectContext 运行，则代码将产生以下单个查询。</span><span class="sxs-lookup"><span data-stu-id="401c6-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="401c6-358">该查询从数据库中一次是将 employee 对象具体化和完全填充其时间卡片属性收集足够的信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="401c6-359">好消息是在操作方法中的代码仍可完全测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="401c6-360">我们不需要提供我们 fakes 以支持，Include 运算符的任何其他功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="401c6-361">不幸的是我们需要使用我们想要保留持久性未知的代码内，Include 运算符。</span><span class="sxs-lookup"><span data-stu-id="401c6-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="401c6-362">这是类型的你将需要评估构建可测试代码时的权衡取舍的一个典型示例。</span><span class="sxs-lookup"><span data-stu-id="401c6-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="401c6-363">有些的时候您需要让外部存储库抽象以满足性能目标的持久性问题泄漏。</span><span class="sxs-lookup"><span data-stu-id="401c6-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="401c6-364">预先加载的替代方法是延迟加载。</span><span class="sxs-lookup"><span data-stu-id="401c6-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="401c6-365">延迟加载意味着我们*不*需要我们业务的代码以显式宣布关联的数据的要求。</span><span class="sxs-lookup"><span data-stu-id="401c6-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="401c6-366">相反，我们在应用程序中使用我们的实体和其他数据是否需要实体框架会按需将数据加载。</span><span class="sxs-lookup"><span data-stu-id="401c6-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="401c6-367">延迟加载</span><span class="sxs-lookup"><span data-stu-id="401c6-367">Lazy Loading</span></span>

<span data-ttu-id="401c6-368">它很容易想象我们不知道数据的业务逻辑将需要一个场景。</span><span class="sxs-lookup"><span data-stu-id="401c6-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="401c6-369">我们可能会知道逻辑需要 employee 对象，但我们可能会转换为不同的执行路径，其中某些这些路径需要从该员工的考勤卡信息而有些则不进行分支。</span><span class="sxs-lookup"><span data-stu-id="401c6-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="401c6-370">类似上述的情形是隐式延迟加载，因为在按需奇迹般地显示数据的最佳选择。</span><span class="sxs-lookup"><span data-stu-id="401c6-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="401c6-371">在我们的实体对象上，延迟加载，也称为延迟加载，does 放置一些要求。</span><span class="sxs-lookup"><span data-stu-id="401c6-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="401c6-372">使用 true 持久性无感知的 Poco 不将会面临暂留层中，从任何要求，但 true 持久性无感知几乎不可能实现。</span><span class="sxs-lookup"><span data-stu-id="401c6-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="401c6-373">而是我们会度量持久化透明的相对度。</span><span class="sxs-lookup"><span data-stu-id="401c6-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="401c6-374">如果我们需要从持久性面向基类继承，或使用专用的集合来实现延迟加载在 Poco，它会令人遗憾。</span><span class="sxs-lookup"><span data-stu-id="401c6-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="401c6-375">幸运的是，EF4 具有较少受侵入的解决方案。</span><span class="sxs-lookup"><span data-stu-id="401c6-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="401c6-376">几乎不可检测</span><span class="sxs-lookup"><span data-stu-id="401c6-376">Virtually Undetectable</span></span>

<span data-ttu-id="401c6-377">在使用 POCO 对象时，EF4 可以动态生成实体的运行时的代理。</span><span class="sxs-lookup"><span data-stu-id="401c6-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="401c6-378">这些代理不可见的方式包装实例化的 Poco 和提供通过截获每个属性的其他服务获取和设置操作以执行额外工作。</span><span class="sxs-lookup"><span data-stu-id="401c6-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="401c6-379">一个此类服务就是我们所期望的延迟加载功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="401c6-380">另一个服务是有效的更改跟踪机制可以记录程序更改实体的属性值时。</span><span class="sxs-lookup"><span data-stu-id="401c6-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="401c6-381">更改的列表使用将 objectcontext 在 SaveChanges 方法期间保留任何已修改的实体使用 UPDATE 命令。</span><span class="sxs-lookup"><span data-stu-id="401c6-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="401c6-382">这些代理才能起作用，但是，他们需要一种方法要挂钩到属性 get 和实体，并在代理上的设置操作通过重写虚拟成员实现此目标。</span><span class="sxs-lookup"><span data-stu-id="401c6-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="401c6-383">因此，如果我们想要具有隐式延迟加载和有效的更改跟踪我们需要返回到我们的 POCO 类定义并将属性标记为虚拟。</span><span class="sxs-lookup"><span data-stu-id="401c6-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="401c6-384">我们仍可以说雇员实体是主要是持久性未知。</span><span class="sxs-lookup"><span data-stu-id="401c6-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="401c6-385">唯一要求是使用虚拟成员，这不会影响代码的可测试性。</span><span class="sxs-lookup"><span data-stu-id="401c6-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="401c6-386">我们不需要从任何特殊的基类派生，或甚至使用专用于延迟加载的特殊集合。</span><span class="sxs-lookup"><span data-stu-id="401c6-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="401c6-387">如代码所示，任何实现 ICollection 的类&lt;T&gt;可用来保存相关的实体。</span><span class="sxs-lookup"><span data-stu-id="401c6-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="401c6-388">此外，还有一个细微的改动，我们需要使我们的工作单元内。</span><span class="sxs-lookup"><span data-stu-id="401c6-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="401c6-389">延迟加载是*关闭*默认情况下，当直接使用 ObjectContext 对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="401c6-390">没有一个我们可以设置的属性 ContextOptions 属性上以启用延迟加载，并且我们可以在我们真正的工作单元内设置此属性，如果我们想要启用延迟加载所有位置。</span><span class="sxs-lookup"><span data-stu-id="401c6-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

<span data-ttu-id="401c6-391">使用隐式启用延迟加载后，应用程序代码可以使用员工和员工的关联时间卡片，剩余根本不用考虑 EF 将额外的数据加载所需的工作时。</span><span class="sxs-lookup"><span data-stu-id="401c6-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="401c6-392">延迟加载可以使应用程序代码容易编写，并与代理神奇代码保持完全可测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="401c6-393">内存中的工作单元的 fakes 只可以预加载关联的数据时需要在测试期间具有伪实体。</span><span class="sxs-lookup"><span data-stu-id="401c6-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="401c6-394">此时我们将我们把注意力从构建存储库使用 IObjectSet&lt;T&gt;并查看的抽象，隐藏的持久性框架的所有符号。</span><span class="sxs-lookup"><span data-stu-id="401c6-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="401c6-395">自定义存储库</span><span class="sxs-lookup"><span data-stu-id="401c6-395">Custom Repositories</span></span>

<span data-ttu-id="401c6-396">当我们首先会看到工作设计模式的单元在本文中我们提供一些示例代码的工作单元可能如下所示。</span><span class="sxs-lookup"><span data-stu-id="401c6-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="401c6-397">让我们重新显示使用中的雇员和员工考勤卡方案，我们一直使用此最初的思路。</span><span class="sxs-lookup"><span data-stu-id="401c6-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="401c6-398">此工作单位的值和我们在上一部分中创建的工作单位的主要区别是如何工作此单元不使用 EF4 框架中的任何抽象概念 (没有任何 IObjectSet&lt;T&gt;)。</span><span class="sxs-lookup"><span data-stu-id="401c6-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="401c6-399">IObjectSet&lt;T&gt;适用于的存储库接口，但它公开的 API 可能不与我们的应用程序的需求而完全对齐。</span><span class="sxs-lookup"><span data-stu-id="401c6-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="401c6-400">在此方法中即将推出，我们将表示存储库使用自定义 IRepository&lt;T&gt;抽象。</span><span class="sxs-lookup"><span data-stu-id="401c6-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="401c6-401">采用测试驱动的设计、 行为驱动设计和域驱动的方法设计的许多开发人员更喜欢 IRepository&lt;T&gt;方法的几个原因。</span><span class="sxs-lookup"><span data-stu-id="401c6-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="401c6-402">首先，IRepository&lt;T&gt;接口表示"反损坏"层。</span><span class="sxs-lookup"><span data-stu-id="401c6-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="401c6-403">中所述由 Eric Evans 著作域驱动设计防损层可保持基础结构 Api，例如持久性 API 离开域代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="401c6-404">其次，开发人员可以构建到满足应用程序的确切需求 （如发现编写测试时） 的存储库的方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="401c6-405">例如，我们可能经常需要查找单个实体使用的 ID 值，因此我们可以将一个 FindById 方法添加到存储库接口。</span><span class="sxs-lookup"><span data-stu-id="401c6-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="401c6-406">我们 IRepository&lt;T&gt;定义将如下所示。</span><span class="sxs-lookup"><span data-stu-id="401c6-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="401c6-407">请注意，我们将使用 IQueryable 重新放置&lt;T&gt;接口，以公开实体集合。</span><span class="sxs-lookup"><span data-stu-id="401c6-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="401c6-408">IQueryable&lt;T&gt;允许 LINQ 表达式树排入 EF4 提供程序，并为该提供程序提供查询的整体视图。</span><span class="sxs-lookup"><span data-stu-id="401c6-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="401c6-409">第二个选项是返回 IEnumerable&lt;T&gt;，这意味着 EF4 LINQ 提供程序将只能看到在存储库内生成的表达式。</span><span class="sxs-lookup"><span data-stu-id="401c6-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="401c6-410">不到 SQL 命令发送到数据库，可能会损害性能将撰写任何分组、 排序和外部存储库完成的投影。</span><span class="sxs-lookup"><span data-stu-id="401c6-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="401c6-411">另一方面，存储库返回仅 IEnumerable&lt;T&gt;结果将永远不会让人感到惊讶与新的 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="401c6-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="401c6-412">这两种方法将起作用，且这两种方法仍可测试性。</span><span class="sxs-lookup"><span data-stu-id="401c6-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="401c6-413">直接提供的单一实现 IRepository&lt;T&gt;接口使用泛型和 EF4 ObjectContext API。</span><span class="sxs-lookup"><span data-stu-id="401c6-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

<span data-ttu-id="401c6-414">IRepository&lt;T&gt;方法为我们提供一些附加我们的查询控制因为客户端必须调用一个方法来获取到的实体。</span><span class="sxs-lookup"><span data-stu-id="401c6-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="401c6-415">在方法内，我们可以提供其他检查和强制实施应用程序约束的 LINQ 运算符。</span><span class="sxs-lookup"><span data-stu-id="401c6-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="401c6-416">请注意，此接口具有两个约束的泛型类型参数。</span><span class="sxs-lookup"><span data-stu-id="401c6-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="401c6-417">第一个约束是不所需的 ObjectSet 类缺点&lt;T&gt;，并在第二个约束强制实施我们实体来实现 IEntity – 创建应用程序的抽象。</span><span class="sxs-lookup"><span data-stu-id="401c6-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="401c6-418">IEntity 接口强制实体具有可读的 Id 属性，以及如何我们 FindById 方法中可以将此属性。</span><span class="sxs-lookup"><span data-stu-id="401c6-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="401c6-419">IEntity 是使用以下代码定义。</span><span class="sxs-lookup"><span data-stu-id="401c6-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="401c6-420">IEntity 无法被视为小型违规持久性无感知的因为我们实体都必须实现此接口。</span><span class="sxs-lookup"><span data-stu-id="401c6-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="401c6-421">请记住持久性无感知非常权衡，以及许多 FindById 功能将大于所施加的接口约束。</span><span class="sxs-lookup"><span data-stu-id="401c6-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="401c6-422">该接口上可测试性没有任何影响。</span><span class="sxs-lookup"><span data-stu-id="401c6-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="401c6-423">实例化实时 IRepository&lt;T&gt;需要 EF4 ObjectContext，因此工作实现的具体单元应管理实例化。</span><span class="sxs-lookup"><span data-stu-id="401c6-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a><span data-ttu-id="401c6-424">使用自定义存储库</span><span class="sxs-lookup"><span data-stu-id="401c6-424">Using the Custom Repository</span></span>

<span data-ttu-id="401c6-425">使用我们自定义的存储库并不明显不同于使用存储库基于 IObjectSet&lt;T&gt;。</span><span class="sxs-lookup"><span data-stu-id="401c6-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="401c6-426">而不是直接向属性应用 LINQ 运算符，我们将首先需要调用其中一个存储库的方法来获取 IQueryable&lt;T&gt;引用。</span><span class="sxs-lookup"><span data-stu-id="401c6-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="401c6-427">请注意，我们实现了以前的自定义 Include 运算符需要进行任何修改。</span><span class="sxs-lookup"><span data-stu-id="401c6-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="401c6-428">存储库的 FindById 方法从操作尝试检索单个实体中删除重复的逻辑。</span><span class="sxs-lookup"><span data-stu-id="401c6-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="401c6-429">可测试性的我们探讨了这两种方法没有显著差异。</span><span class="sxs-lookup"><span data-stu-id="401c6-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="401c6-430">我们无法提供假实现 IRepository&lt;T&gt;通过构建由 HashSet 提供支持的具体类&lt;员工&gt;-就像我们在上一部分中所做。</span><span class="sxs-lookup"><span data-stu-id="401c6-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="401c6-431">但是，一些开发人员更愿意使用模拟对象和模拟对象而不是生成 fakes 框架。</span><span class="sxs-lookup"><span data-stu-id="401c6-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="401c6-432">我们将介绍使用模拟来测试我们的实现，并讨论下一节中的模拟和 fakes 之间的差异。</span><span class="sxs-lookup"><span data-stu-id="401c6-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="401c6-433">使用模拟测试</span><span class="sxs-lookup"><span data-stu-id="401c6-433">Testing with Mocks</span></span>

<span data-ttu-id="401c6-434">有不同的方法生成"test double"的哪些 Martin Fowler 的调用。</span><span class="sxs-lookup"><span data-stu-id="401c6-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="401c6-435">Test double （如电影 stunt double) 是一个对象生成的用于"突出显示"为实际生产测试期间的对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="401c6-436">我们创建内存中存储库是与 SQL Server 的存储库的 test double。</span><span class="sxs-lookup"><span data-stu-id="401c6-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="401c6-437">我们已了解如何在单元测试过程中使用这些 test double 来隔离代码并使快速运行的测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="401c6-438">我们构建了 test double 都具有实际的工作实现。</span><span class="sxs-lookup"><span data-stu-id="401c6-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="401c6-439">在后台每个存储的对象，具体集合，并且它们将添加并从此集合中移除对象，如我们在测试期间操作在存储库。</span><span class="sxs-lookup"><span data-stu-id="401c6-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="401c6-440">一些开发人员要构建其 test double 这种方式 – 的实际代码和工作实现。</span><span class="sxs-lookup"><span data-stu-id="401c6-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="401c6-441">这些测试双精度型值是我们所说*fakes*。</span><span class="sxs-lookup"><span data-stu-id="401c6-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="401c6-442">它们的工作实现，但它们不是实际生产环境中使用。</span><span class="sxs-lookup"><span data-stu-id="401c6-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="401c6-443">虚设储存库实际上不会写入数据库。</span><span class="sxs-lookup"><span data-stu-id="401c6-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="401c6-444">虚设的 SMTP 服务器实际上不会通过网络发送一封电子邮件。</span><span class="sxs-lookup"><span data-stu-id="401c6-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="401c6-445">Fakes 与模拟</span><span class="sxs-lookup"><span data-stu-id="401c6-445">Mocks versus Fakes</span></span>

<span data-ttu-id="401c6-446">还有另一种类型的双精度称为测试*模拟*。</span><span class="sxs-lookup"><span data-stu-id="401c6-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="401c6-447">Fakes 有工作实现，而模拟就派上没有实现。</span><span class="sxs-lookup"><span data-stu-id="401c6-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="401c6-448">Mock 对象框架帮助我们在运行时构造这些模拟对象，并将它们用作 test double。</span><span class="sxs-lookup"><span data-stu-id="401c6-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="401c6-449">在本部分中我们将使用模拟框架 Moq 开放源代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="401c6-450">下面是使用 Moq 动态创建一个测试双员工存储库的简单示例。</span><span class="sxs-lookup"><span data-stu-id="401c6-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="401c6-451">我们请求 Moq 是 IRepository&lt;员工&gt;实现和它生成一个动态。</span><span class="sxs-lookup"><span data-stu-id="401c6-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="401c6-452">我们可以处理该对象实现 IRepository&lt;员工&gt;通过访问对象属性的模拟&lt;T&gt;对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="401c6-453">我们可以将它们传递给我们的控制器，此内部对象，并且它们不会知道如果此参数的 test double 或实际的存储库。</span><span class="sxs-lookup"><span data-stu-id="401c6-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="401c6-454">就像我们将调用具有真正的实现的对象的方法一样，我们可以调用对象上的方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="401c6-455">您必须想知道 mock 存储库时我们调用 Add 方法将执行的操作。</span><span class="sxs-lookup"><span data-stu-id="401c6-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="401c6-456">由于没有实现 mock 对象后的，添加将任何影响。</span><span class="sxs-lookup"><span data-stu-id="401c6-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="401c6-457">正如我们已经有了使用 fakes，我们编写了，因此员工将被丢弃，没有在后台具体集合。</span><span class="sxs-lookup"><span data-stu-id="401c6-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="401c6-458">FindById 的返回值呢？</span><span class="sxs-lookup"><span data-stu-id="401c6-458">What about the return value of FindById?</span></span> <span data-ttu-id="401c6-459">在这种情况下该模拟对象的功能的唯一可以执行，这是返回默认值。</span><span class="sxs-lookup"><span data-stu-id="401c6-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="401c6-460">由于我们要返回引用类型 （员工），则返回值是 null 值。</span><span class="sxs-lookup"><span data-stu-id="401c6-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="401c6-461">模拟可能听起来毫无价值;但是，还有两个模拟我们没有讨论过的功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="401c6-462">首先，Moq framework 记录所做的是模拟对象上的所有调用。</span><span class="sxs-lookup"><span data-stu-id="401c6-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="401c6-463">以后在代码中我们可以要求 Moq，如果任何人都调用 Add 方法，或者任何人都调用 FindById 方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="401c6-464">我们将看到更高版本如何在测试中使用此"黑盒"录制功能。</span><span class="sxs-lookup"><span data-stu-id="401c6-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="401c6-465">第二个重要功能是我们可以如何使用 Moq 进行编程的 mock 对象*期望*。</span><span class="sxs-lookup"><span data-stu-id="401c6-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="401c6-466">期望告知 mock 对象如何对任何给定交互作出响应。</span><span class="sxs-lookup"><span data-stu-id="401c6-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="401c6-467">例如，我们可以到我们模拟程序期望并告诉它要有人调用 FindById 时返回 employee 对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="401c6-468">Moq 框架将使用安装程序 API 和 lambda 表达式进行编程这些预期提高。</span><span class="sxs-lookup"><span data-stu-id="401c6-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

<span data-ttu-id="401c6-469">我们要求 Moq 来动态创建一个存储库，在此示例中，然后我们编程的假定条件下的存储库。</span><span class="sxs-lookup"><span data-stu-id="401c6-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="401c6-470">预期结果指示要返回具有 Id 值为 5 的新员工对象，当有人调用 FindById 方法传递的值为 5 时的模拟对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="401c6-471">此测试通过，并且我们不需要生成完整的实现为假 IRepository&lt;T&gt;。</span><span class="sxs-lookup"><span data-stu-id="401c6-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="401c6-472">让我们回顾我们在前面编写的测试，并对它们而不是 fakes 使用模拟进行返工。</span><span class="sxs-lookup"><span data-stu-id="401c6-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="401c6-473">就像之前，我们将使用基类来设置我们需要为所有控制器的测试的基础结构的常见组成部分。</span><span class="sxs-lookup"><span data-stu-id="401c6-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

<span data-ttu-id="401c6-474">安装程序代码主要是保持相同。</span><span class="sxs-lookup"><span data-stu-id="401c6-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="401c6-475">而不是使用 fakes，我们将使用 Moq 构造模拟对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="401c6-476">要返回模拟存储库，当代码调用员工属性时的模拟工作单元安排的基类。</span><span class="sxs-lookup"><span data-stu-id="401c6-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="401c6-477">模拟的安装程序的其余部分会在专用于每个特定的方案测试装置内进行。</span><span class="sxs-lookup"><span data-stu-id="401c6-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="401c6-478">例如，索引操作测试固定例程将设置模拟存储库时要返回的员工列表操作都调用 FindAll 方法的模拟存储库。</span><span class="sxs-lookup"><span data-stu-id="401c6-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

<span data-ttu-id="401c6-479">除预期值，我们的测试看起来类似于一些前面用过的测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="401c6-480">但是，一个模拟框架的录制功能我们可以着手测试从不同角度。</span><span class="sxs-lookup"><span data-stu-id="401c6-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="401c6-481">我们来看看下一节中此新的透视。</span><span class="sxs-lookup"><span data-stu-id="401c6-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="401c6-482">与测试交互状态</span><span class="sxs-lookup"><span data-stu-id="401c6-482">State versus Interaction Testing</span></span>

<span data-ttu-id="401c6-483">有不同的技术可用于测试和模拟对象的软件。</span><span class="sxs-lookup"><span data-stu-id="401c6-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="401c6-484">一种方法是使用状态基于测试，这是我们做了什么本白皮书中到目前为止。</span><span class="sxs-lookup"><span data-stu-id="401c6-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="401c6-485">基于状态的有关软件的状态的测试进行断言。</span><span class="sxs-lookup"><span data-stu-id="401c6-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="401c6-486">中的最后一个测试我们调用操作方法的控制器上做，并且它应生成有关模型的断言。</span><span class="sxs-lookup"><span data-stu-id="401c6-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="401c6-487">下面是一些其他示例的测试状态：</span><span class="sxs-lookup"><span data-stu-id="401c6-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="401c6-488">验证存储库创建在执行后包含新的 employee 对象。</span><span class="sxs-lookup"><span data-stu-id="401c6-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="401c6-489">验证索引执行后，模型包含的所有员工的列表。</span><span class="sxs-lookup"><span data-stu-id="401c6-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="401c6-490">验证删除执行后，存储库不包含给定的员工。</span><span class="sxs-lookup"><span data-stu-id="401c6-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="401c6-491">你将看到使用模拟对象的另一种方法是验证*交互*。</span><span class="sxs-lookup"><span data-stu-id="401c6-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="401c6-492">虽然基于状态的有关对象的状态的测试可以断言，交互将基于对象之间如何交互的测试进行断言。</span><span class="sxs-lookup"><span data-stu-id="401c6-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="401c6-493">例如：</span><span class="sxs-lookup"><span data-stu-id="401c6-493">For example:</span></span>

-   <span data-ttu-id="401c6-494">验证控制器创建执行时调用该存储库的 Add 方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="401c6-495">验证控制器索引执行时调用该存储库的 FindAll 方法。</span><span class="sxs-lookup"><span data-stu-id="401c6-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="401c6-496">验证控制器调用要保存更改时编辑执行工作的 Commit 方法的单元。</span><span class="sxs-lookup"><span data-stu-id="401c6-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="401c6-497">因为我们不在集合内四处闲逛和验证计数，交互测试通常需要较少的测试数据。</span><span class="sxs-lookup"><span data-stu-id="401c6-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="401c6-498">例如，如果我们知道的详细信息操作调用使用正确的值的存储库的 FindById 方法然后操作可能运行正常。</span><span class="sxs-lookup"><span data-stu-id="401c6-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="401c6-499">我们可以验证此行为，而无需设置 FindById 从返回的任何测试数据。</span><span class="sxs-lookup"><span data-stu-id="401c6-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

<span data-ttu-id="401c6-500">在上面测试固定例程所需的唯一设置是由基类提供的安装程序。</span><span class="sxs-lookup"><span data-stu-id="401c6-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="401c6-501">当我们调用控制器操作时，Moq 将记录与模拟存储库之间的交互。</span><span class="sxs-lookup"><span data-stu-id="401c6-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="401c6-502">使用验证 Moq API，我们可以要求 Moq 是否控制器调用 FindById 具有正确的 ID 值。</span><span class="sxs-lookup"><span data-stu-id="401c6-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="401c6-503">如果控制器未调用方法，或调用带有意外的参数值的方法，验证方法将引发异常，则测试将失败。</span><span class="sxs-lookup"><span data-stu-id="401c6-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="401c6-504">下面是工作的另一个示例，验证创建操作调用上当前单元提交。</span><span class="sxs-lookup"><span data-stu-id="401c6-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="401c6-505">使用交互测试的一个危险是往往会通过指定的交互。</span><span class="sxs-lookup"><span data-stu-id="401c6-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="401c6-506">要记录并验证使用模拟对象的每个交互并不意味着该测试的模拟对象的功能应尝试验证的每个交互。</span><span class="sxs-lookup"><span data-stu-id="401c6-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="401c6-507">一些交互是实现详细信息，你应仅验证交互*需*以满足当前的测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="401c6-508">选择模拟或虚设之间很大程度上取决于要测试的系统和你的个人 （或团队） 首选项。</span><span class="sxs-lookup"><span data-stu-id="401c6-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="401c6-509">模拟对象可以显著减少您需要实现 test double 的代码量，但不是每个人都能轻松地编程的期望和验证交互。</span><span class="sxs-lookup"><span data-stu-id="401c6-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="401c6-510">结论</span><span class="sxs-lookup"><span data-stu-id="401c6-510">Conclusions</span></span>

<span data-ttu-id="401c6-511">在本文中我们演示了几种方法使用 ADO.NET 实体框架实现数据持久性的同时创建可测试代码。</span><span class="sxs-lookup"><span data-stu-id="401c6-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="401c6-512">我们可以利用内置的抽象，例如 IObjectSet&lt;T&gt;，或创建我们自己抽象，例如 IRepository&lt;T&gt;。</span><span class="sxs-lookup"><span data-stu-id="401c6-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="401c6-513">在这两种情况下，ADO.NET Entity Framework 4.0 中的 POCO 支持允许这些抽象的使用者保持持久性未知和高度可测试性。</span><span class="sxs-lookup"><span data-stu-id="401c6-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="401c6-514">其他 EF4 功能，例如隐式延迟加载允许业务和应用程序的服务代码工作而无需担心关系数据存储的详细信息。</span><span class="sxs-lookup"><span data-stu-id="401c6-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="401c6-515">最后，我们创建的抽象可以方便地进行模拟或虚设内部单元测试，并且我们可以使用这些 test double，以实现快速运行，高度隔离和可靠的测试。</span><span class="sxs-lookup"><span data-stu-id="401c6-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="401c6-516">其他资源</span><span class="sxs-lookup"><span data-stu-id="401c6-516">Additional Resources</span></span>

-   <span data-ttu-id="401c6-517">Robert C.Martin"[单一责任原则](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="401c6-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="401c6-518">Martin Fowler[模式目录](http://www.martinfowler.com/eaaCatalog/index.html)从*的企业应用程序体系结构模式*</span><span class="sxs-lookup"><span data-stu-id="401c6-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="401c6-519">Griffin Caprio"[依赖关系注入](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="401c6-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="401c6-520">数据可编程性博客"[演练： 测试驱动开发使用 Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)"。</span><span class="sxs-lookup"><span data-stu-id="401c6-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="401c6-521">数据可编程性博客"[结合使用 Entity Framework 4.0 使用存储库和工作单元模式](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="401c6-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="401c6-522">Dave Astels" [BDD 简介](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="401c6-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="401c6-523">Aaron Jensen"[简介机规范](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="401c6-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="401c6-524">Eric Lee"[使用 MSTest BDD](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="401c6-524">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="401c6-525">Eric Evans"[域驱动设计](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="401c6-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="401c6-526">Martin Fowler" [Mock 不是存根](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="401c6-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="401c6-527">Martin Fowler"[测试双](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="401c6-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="401c6-528">Jeremy Miller"[状态和交互测试](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="401c6-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   [<span data-ttu-id="401c6-529">Moq</span><span class="sxs-lookup"><span data-stu-id="401c6-529">Moq</span></span>](http://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="401c6-530">作者简介</span><span class="sxs-lookup"><span data-stu-id="401c6-530">Biography</span></span>

<span data-ttu-id="401c6-531">Scott Allen 是 Pluralsight 技术团队的成员和 OdeToCode.com 的创始人。</span><span class="sxs-lookup"><span data-stu-id="401c6-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="401c6-532">在 15 年的商业软件开发中，Scott 曾解决方案从 8 位到高度可扩展的 ASP.NET web 应用程序的嵌入式设备的一切事项。</span><span class="sxs-lookup"><span data-stu-id="401c6-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="401c6-533">您可以在他的博客上是 OdeToCode，或在 Twitter 上通过 Scott [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode)。</span><span class="sxs-lookup"><span data-stu-id="401c6-533">You can reach Scott on his blog at OdeToCode, or on Twitter at [http://twitter.com/OdeToCode](http://twitter.com/OdeToCode).</span></span>
