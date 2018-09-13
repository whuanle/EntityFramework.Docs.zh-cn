---
title: 使用 NGen-EF6 以提高启动性能
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 2e9a93c7741046da25b890fed084a6b280bf5643
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489955"
---
# <a name="improving-startup-performance-with-ngen"></a><span data-ttu-id="b8b54-102">使用 NGen 以提高启动性能</span><span class="sxs-lookup"><span data-stu-id="b8b54-102">Improving startup performance with NGen</span></span>
> [!NOTE]
> <span data-ttu-id="b8b54-103">**仅限 EF6 及更高版本** - 此页面中讨论的功能、API 等已引入实体框架 6。</span><span class="sxs-lookup"><span data-stu-id="b8b54-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b8b54-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="b8b54-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="b8b54-105">.NET Framework 的托管应用程序支持的本机映像生成和库帮助应用程序启动速度更快的途径，而且还在某些情况下使用更少的内存。</span><span class="sxs-lookup"><span data-stu-id="b8b54-105">The .NET Framework supports the generation of native images for managed applications and libraries as a way to help applications start faster and also in some cases use less memory.</span></span> <span data-ttu-id="b8b54-106">将转换到之前执行该应用程序时，包含本机指令的文件的托管的代码程序集从而减轻无需生成的本机说明.NET JIT （实时） 编译器创建本机映像应用程序运行时。</span><span class="sxs-lookup"><span data-stu-id="b8b54-106">Native images are created by translating managed code assemblies into files containing native machine instructions before the application is executed, relieving the .NET JIT (Just-In-Time) compiler from having to generate the native instructions at application runtime.</span></span>  

<span data-ttu-id="b8b54-107">之前的版本 6，EF 运行时的核心库是.NET Framework 的一部分，并为其自动生成本机映像。</span><span class="sxs-lookup"><span data-stu-id="b8b54-107">Prior to version 6, the EF runtime’s core libraries were part of the .NET Framework and native images were generated automatically for them.</span></span> <span data-ttu-id="b8b54-108">从版本 6 开始整个 EF 运行时已合并到 EntityFramework NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="b8b54-108">Starting with version 6 the whole EF runtime has been combined into the EntityFramework NuGet package.</span></span> <span data-ttu-id="b8b54-109">生成本机映像已从现在将使用 NGen.exe 命令行工具来获得类似结果。</span><span class="sxs-lookup"><span data-stu-id="b8b54-109">Native images have to now be generated using the NGen.exe command line tool to obtain similar results.</span></span>  

<span data-ttu-id="b8b54-110">经验的观测值显示的 EF 运行时程序集的本机映像可以剪切的应用程序启动时间的 1 到 3 秒之间。</span><span class="sxs-lookup"><span data-stu-id="b8b54-110">Empirical observations show that native images of the EF runtime assemblies can cut between 1 and 3 seconds of application startup time.</span></span>  

## <a name="how-to-use-ngenexe"></a><span data-ttu-id="b8b54-111">如何使用 NGen.exe</span><span class="sxs-lookup"><span data-stu-id="b8b54-111">How to use NGen.exe</span></span>  

<span data-ttu-id="b8b54-112">NGen.exe 工具的最基本功能是"安装"(即，创建并保存到磁盘) 的程序集和其直接依赖项的本机映像。</span><span class="sxs-lookup"><span data-stu-id="b8b54-112">The most basic function of the NGen.exe tool is to “install” (that is, to create and persist to disk) native images for an assembly and all its direct dependencies.</span></span> <span data-ttu-id="b8b54-113">下面是如何实现此目的：</span><span class="sxs-lookup"><span data-stu-id="b8b54-113">Here is how you can achieve that:</span></span>  

1. <span data-ttu-id="b8b54-114">以管理员身份打开命令提示符窗口</span><span class="sxs-lookup"><span data-stu-id="b8b54-114">Open a Command Prompt window as an administrator</span></span>  
2. <span data-ttu-id="b8b54-115">将当前工作目录更改为你想要生成本机映像程序集的位置：</span><span class="sxs-lookup"><span data-stu-id="b8b54-115">Change the current working directory to the location of the assemblies you want to generate native images for:</span></span>  

  ``` console
    cd <*Assemblies location*>  
  ```
3. <span data-ttu-id="b8b54-116">具体取决于你的操作系统和应用程序的配置可能需要生成本机映像用于 32 位体系结构，64 位体系结构或同时用于两者。</span><span class="sxs-lookup"><span data-stu-id="b8b54-116">Depending on your operating system and the application’s configuration you might need to generate native images for 32 bit architecture, 64 bit architecture or for both.</span></span>  

    <span data-ttu-id="b8b54-117">适用于运行的 32 位：</span><span class="sxs-lookup"><span data-stu-id="b8b54-117">For 32 bit run:</span></span>  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    <span data-ttu-id="b8b54-118">适用于运行 64 位：</span><span class="sxs-lookup"><span data-stu-id="b8b54-118">For 64 bit run:</span></span>
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> <span data-ttu-id="b8b54-119">正在生成本机映像的体系结构错误是一个非常常见的错误。</span><span class="sxs-lookup"><span data-stu-id="b8b54-119">Generating native images for the wrong architecture is a very common mistake.</span></span> <span data-ttu-id="b8b54-120">可以只需生成本机映像应用到计算机中安装操作系统的所有体系结构的有疑问。</span><span class="sxs-lookup"><span data-stu-id="b8b54-120">When in doubt you can simply generate native images for all the architectures that apply to the operating system installed in the machine.</span></span>  

<span data-ttu-id="b8b54-121">NGen.exe 还支持其他功能，如卸载和显示的已安装的本机映像，队列生成的多个图像，等等。有关使用情况的更多详细信息请阅读[NGen.exe 文档](https://msdn.microsoft.com/library/6t9t5wcf.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b8b54-121">NGen.exe also supports other functions such as uninstalling and displaying the installed native images, queuing the generation of multiple images, etc. For more details of usage read the [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx).</span></span>  

## <a name="when-to-use-ngenexe"></a><span data-ttu-id="b8b54-122">何时使用 NGen.exe</span><span class="sxs-lookup"><span data-stu-id="b8b54-122">When to use NGen.exe</span></span>  

<span data-ttu-id="b8b54-123">就决定要在基于 EF 6 或更高版本的应用程序中生成本机映像的程序集，应考虑以下选项：</span><span class="sxs-lookup"><span data-stu-id="b8b54-123">When it comes to decide which assemblies to generate native images for in an application based on EF version 6 or greater, you should consider the following options:</span></span>  

- <span data-ttu-id="b8b54-124">**主要 EF 运行时程序集，EntityFramework.dll**： 典型的基于 EF 应用程序到数据库从在启动时或在其首次访问此程序集执行大量的代码。</span><span class="sxs-lookup"><span data-stu-id="b8b54-124">**The main EF runtime assembly, EntityFramework.dll**: A typical EF based application executes a significant amount of code from this assembly on startup or on its first access to the database.</span></span> <span data-ttu-id="b8b54-125">因此，创建此程序集的本机映像将生成的最大提高启动性能。</span><span class="sxs-lookup"><span data-stu-id="b8b54-125">Consequently, creating native images of this assembly will produce the biggest gains in startup performance.</span></span>  
- <span data-ttu-id="b8b54-126">**应用程序使用的任何 EF 提供程序程序集**： 启动时间也可稍有受益于正在生成的这些本机映像。</span><span class="sxs-lookup"><span data-stu-id="b8b54-126">**Any EF provider assembly used by your application**: Startup time can also benefit slightly from generating native images of these.</span></span> <span data-ttu-id="b8b54-127">例如，如果应用程序使用 SQL Server EF 提供程序将想要为 EntityFramework.SqlServer.dll 生成本机映像。</span><span class="sxs-lookup"><span data-stu-id="b8b54-127">For example, if the application uses the EF provider for SQL Server you will want to generate a native image for EntityFramework.SqlServer.dll.</span></span>  
- <span data-ttu-id="b8b54-128">**应用程序的程序集和其他依赖项**: [NGen.exe 文档](https://msdn.microsoft.com/library/6t9t5wcf.aspx)介绍了有关选择要生成本机映像和安全性，本机映像的影响的程序集的一般准则高级选项，例如"硬绑定"，例如，使用本机映像中调试和分析方案等方案。</span><span class="sxs-lookup"><span data-stu-id="b8b54-128">**Your application’s assemblies and other dependencies**: The [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx) covers general criteria for choosing which assemblies to generate native images for and the impact of native images on security, advanced options such as “hard binding”, scenarios such as using native images in debugging and profiling scenarios, etc.</span></span>  

> [!TIP]
> <span data-ttu-id="b8b54-129">请确保仔细测量使用本机映像上的启动性能和应用程序的整体性能的影响，并针对实际要求对它们进行比较。</span><span class="sxs-lookup"><span data-stu-id="b8b54-129">Make sure you carefully measure the impact of using native images on both the startup performance and the overall performance of your application and compare them against actual requirements.</span></span> <span data-ttu-id="b8b54-130">本机映像通常会有帮助提高启动性能并在某些情况下降低内存使用情况，而不是所有情况下将同样受益。</span><span class="sxs-lookup"><span data-stu-id="b8b54-130">While native images will generally help improve startup up performance and in some cases reduce memory usage, not all scenarios will benefit equally.</span></span> <span data-ttu-id="b8b54-131">例如，在稳定状态执行 （也就是说，一旦调用至少一次的应用程序正在使用的所有方法） 由 JIT 编译器生成的代码可以实际产生略有更好的性能比本机映像。</span><span class="sxs-lookup"><span data-stu-id="b8b54-131">For instance, on steady state execution (that is, once all the methods being used by the application have been invoked at least once) code generated by the JIT compiler can in fact yield slightly better performance than native images.</span></span>  

## <a name="using-ngenexe-in-a-development-machine"></a><span data-ttu-id="b8b54-132">在开发计算机中使用 NGen.exe</span><span class="sxs-lookup"><span data-stu-id="b8b54-132">Using NGen.exe in a development machine</span></span>  

<span data-ttu-id="b8b54-133">在开发.NET JIT 编译器将提供频繁更改的代码的最佳整体代价。</span><span class="sxs-lookup"><span data-stu-id="b8b54-133">During development the .NET JIT compiler will offer the best overall tradeoff for code that is changing frequently.</span></span> <span data-ttu-id="b8b54-134">正在生成的已编译依赖项，如 EF 运行时程序集的本机映像可帮助加速开发和测试通过削减几秒钟，在每次执行的开始处。</span><span class="sxs-lookup"><span data-stu-id="b8b54-134">Generating native images for compiled dependencies such as the EF runtime assemblies can help accelerate development and testing by cutting a few seconds out at the beginning of each execution.</span></span>  

<span data-ttu-id="b8b54-135">查找 EF 运行时程序集的好时机是解决方案的 NuGet 包位置。</span><span class="sxs-lookup"><span data-stu-id="b8b54-135">A good place to find the EF runtime assemblies is the NuGet package location for the solution.</span></span> <span data-ttu-id="b8b54-136">例如，SQL Server 中使用 EF 6.0.2 和面向.NET 4.5 或更高版本的应用程序您可以键入以下命令提示符窗口中 （请记住将其打开以管理员身份）：</span><span class="sxs-lookup"><span data-stu-id="b8b54-136">For example, for an application using EF 6.0.2 with SQL Server and targeting .NET 4.5 or greater you can type the following in a Command Prompt window (remember to open it as an administrator):</span></span>  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> <span data-ttu-id="b8b54-137">这会利用这一事实，安装 SQL Server 的 EF 提供程序的本机映像还会默认情况下安装的本机映像的主要 EF 运行时程序集。</span><span class="sxs-lookup"><span data-stu-id="b8b54-137">This takes advantage of the fact that installing the native images for EF provider for SQL Server will also by default install the native images for the main EF runtime assembly.</span></span> <span data-ttu-id="b8b54-138">这样做的原因 NGen.exe 可以检测到 EntityFramework.dll 是 EntityFramework.SqlServer.dll 程序集位于同一目录中的直接依赖项。</span><span class="sxs-lookup"><span data-stu-id="b8b54-138">This works because NGen.exe can detect that EntityFramework.dll is a direct dependency of the EntityFramework.SqlServer.dll assembly located in the same directory.</span></span>  

## <a name="creating-native-images-during-setup"></a><span data-ttu-id="b8b54-139">在安装过程中创建本机映像</span><span class="sxs-lookup"><span data-stu-id="b8b54-139">Creating native images during setup</span></span>  

<span data-ttu-id="b8b54-140">WiX 工具包支持队列的本机映像生成托管程序集安装过程中，在此所述[操作方法指南](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html)。</span><span class="sxs-lookup"><span data-stu-id="b8b54-140">The WiX Toolkit supports queuing the generation of native images for managed assemblies during setup, as explained in this [how-to guide](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html).</span></span> <span data-ttu-id="b8b54-141">另一种方法是创建一个自定义安装程序任务，执行 NGen.exe 命令。</span><span class="sxs-lookup"><span data-stu-id="b8b54-141">Another alternative is to create a custom setup task that execute the NGen.exe command.</span></span>  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a><span data-ttu-id="b8b54-142">正在验证的 EF 使用本机映像</span><span class="sxs-lookup"><span data-stu-id="b8b54-142">Verifying that native images are being used for EF</span></span>  

<span data-ttu-id="b8b54-143">你可以验证特定的应用程序通过查找具有扩展的加载程序集来使用本机程序集"。 ni.dll"。 ni.exe"。</span><span class="sxs-lookup"><span data-stu-id="b8b54-143">You can verify that a specific application is using a native assembly by looking for loaded assemblies that have the extension “.ni.dll” or “.ni.exe”.</span></span> <span data-ttu-id="b8b54-144">例如，EF 的主运行时程序集的本机映像，将调用 EntityFramework.ni.dll。</span><span class="sxs-lookup"><span data-stu-id="b8b54-144">For example, a native image for the EF’s main runtime assembly will be called EntityFramework.ni.dll.</span></span> <span data-ttu-id="b8b54-145">若要检查的进程加载的.NET 程序集的简单方法是使用[Process Explorer](https://technet.microsoft.com/sysinternals/bb896653)。</span><span class="sxs-lookup"><span data-stu-id="b8b54-145">An easy way to inspect the loaded .NET assemblies of a process is to use [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).</span></span>  

## <a name="other-things-to-be-aware-of"></a><span data-ttu-id="b8b54-146">需要注意的其他事项</span><span class="sxs-lookup"><span data-stu-id="b8b54-146">Other things to be aware of</span></span>  

<span data-ttu-id="b8b54-147">**创建本机映像的程序集不应混淆注册中的程序集[GAC （全局程序集缓存）](https://msdn.microsoft.com/library/yf1d93sz.aspx)**。</span><span class="sxs-lookup"><span data-stu-id="b8b54-147">**Creating a native image of an assembly should not be confused with registering the assembly in the [GAC (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**.</span></span> <span data-ttu-id="b8b54-148">NGen.exe 允许创建不在 GAC 中的程序集的映像和事实上，使用 EF 的特定版本的多个应用程序可以共享相同的本机映像。</span><span class="sxs-lookup"><span data-stu-id="b8b54-148">NGen.exe allows creating images of assemblies that are not in the GAC, and in fact, multiple applications that use a specific version of EF can share the same native image.</span></span> <span data-ttu-id="b8b54-149">虽然 Windows 8 可以自动创建的程序集放置在 GAC 中本机映像，EF 运行时优化以与你的应用程序一起部署，建议不注册它在 GAC 中，因为这对程序集解析有负面影响和为您在其他方面之间的应用程序提供服务。</span><span class="sxs-lookup"><span data-stu-id="b8b54-149">While Windows 8 can automatically create native images for assemblies placed in the GAC, the EF runtime is optimized to be deployed alongside your application and we do not recommend registering it in the GAC as this has a negative impact on assembly resolution and servicing your applications among other aspects.</span></span>  
