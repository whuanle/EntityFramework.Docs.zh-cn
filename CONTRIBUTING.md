# <a name="contributing-to-the-entity-framework-documentation"></a><span data-ttu-id="1278e-101">参与实体框架文档</span><span class="sxs-lookup"><span data-stu-id="1278e-101">Contributing to the Entity Framework documentation</span></span>

<span data-ttu-id="1278e-102">本文档介绍了参与[实体框架文档站点](https://docs.microsoft.com/ef)上托管的文章和代码示例的过程。</span><span class="sxs-lookup"><span data-stu-id="1278e-102">The document covers the process for contributing to the articles and code samples that are hosted on the [Entity Framework documentation site](https://docs.microsoft.com/ef).</span></span> <span data-ttu-id="1278e-103">参与内容可简（更正拼写错误），可繁（编写新文章）。</span><span class="sxs-lookup"><span data-stu-id="1278e-103">Contributions may be as simple as typo corrections or as complex as new articles.</span></span>

## <a name="how-to-make-a-simple-correction-or-suggestion"></a><span data-ttu-id="1278e-104">如何提出简单的更正或建议</span><span class="sxs-lookup"><span data-stu-id="1278e-104">How to make a simple correction or suggestion</span></span>

<span data-ttu-id="1278e-105">文章作为 Markdown 文件存储在存储库中。</span><span class="sxs-lookup"><span data-stu-id="1278e-105">Articles are stored in the repository as Markdown files.</span></span> <span data-ttu-id="1278e-106">通过点击浏览器窗口右上角的“编辑”链接，可以在浏览器中对 Markdown 文件的内容进行简单更改。</span><span class="sxs-lookup"><span data-stu-id="1278e-106">Simple changes to the content of a Markdown file can be made in the browser by tapping the **Edit** link in the upper right corner of the browser window.</span></span> <span data-ttu-id="1278e-107">（在狭窄的浏览器窗口中，可能需要展开“选项”栏，以查看“编辑”链接。）按照说明创建拉取请求 (PR)。</span><span class="sxs-lookup"><span data-stu-id="1278e-107">(In narrow browser windows you might need to expand the **options** bar to see the **Edit** link.) Follow the directions to create a pull request (PR).</span></span> <span data-ttu-id="1278e-108">EF 团队将对拉取请求进行审核并接受相关请求或提出更改建议。</span><span class="sxs-lookup"><span data-stu-id="1278e-108">The EF team will review the PR and accept it or suggest changes.</span></span>

## <a name="how-to-make-a-more-complex-submission"></a><span data-ttu-id="1278e-109">如何提出更复杂的提交</span><span class="sxs-lookup"><span data-stu-id="1278e-109">How to make a more complex submission</span></span>

<span data-ttu-id="1278e-110">需要对 [Git 和 GitHub.com](https://guides.github.com/activities/hello-world/) 有基本的理解。</span><span class="sxs-lookup"><span data-stu-id="1278e-110">You'll need a basic understanding of [Git and GitHub.com](https://guides.github.com/activities/hello-world/).</span></span>

* <span data-ttu-id="1278e-111">打开[问题](https://github.com/aspnet/EntityFramework.Docs/issues/new)，描述想要执行的操作，例如更改现有文章或创建一个新文章。</span><span class="sxs-lookup"><span data-stu-id="1278e-111">Open an [issue](https://github.com/aspnet/EntityFramework.Docs/issues/new) describing what you want to do, such as change an existing article or create a new one.</span></span> <span data-ttu-id="1278e-112">请等待 EF 团队批准后再投入时间参与进来。</span><span class="sxs-lookup"><span data-stu-id="1278e-112">Wait for approval from the EF team before you invest much time.</span></span>
* <span data-ttu-id="1278e-113">对 [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) 存储库进行分叉，并为所作更改创建分支。</span><span class="sxs-lookup"><span data-stu-id="1278e-113">Fork the [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) repo and create a branch for your changes.</span></span>
* <span data-ttu-id="1278e-114">将拉取请求 (PR) 与更改一起提交到主干。</span><span class="sxs-lookup"><span data-stu-id="1278e-114">Submit a pull request (PR) to master with your changes.</span></span>
* <span data-ttu-id="1278e-115">对 PR 请求反馈进行响应。</span><span class="sxs-lookup"><span data-stu-id="1278e-115">Respond to PR feedback.</span></span>

## <a name="markdown-syntax"></a><span data-ttu-id="1278e-116">Markdown 语法</span><span class="sxs-lookup"><span data-stu-id="1278e-116">Markdown syntax</span></span>

<span data-ttu-id="1278e-117">文章采用 [DocFx 风格的 Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html) 编写，它是 [GitHub 风格的 Markdown (GFM)](https://guides.github.com/features/mastering-markdown/) 的超集。</span><span class="sxs-lookup"><span data-stu-id="1278e-117">Articles are written in [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), which is a superset of [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span></span> <span data-ttu-id="1278e-118">有关 EF 文档中常用的 UI 功能的 DFM 语法示例，请参阅 .NET Core 存储库风格指南中的[元数据和 Markdown 模板](https://github.com/dotnet/docs/blob/master/styleguide/template.md)。</span><span class="sxs-lookup"><span data-stu-id="1278e-118">For examples of DFM syntax for UI features commonly used in the EF documentation, see [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) in the .NET Core repo style guide.</span></span> 

## <a name="folder-structure-conventions"></a><span data-ttu-id="1278e-119">文件夹结构约定</span><span class="sxs-lookup"><span data-stu-id="1278e-119">Folder structure conventions</span></span>

<span data-ttu-id="1278e-120">图像和其他静态内容存储在站点的每个区域/文件夹中的 `_static` 文件夹。</span><span class="sxs-lookup"><span data-stu-id="1278e-120">Images, and other static content, are stored in an `_static` folder within each area/folder of the site.</span></span>

<span data-ttu-id="1278e-121">代码示例存储在 `samples` 根文件夹。</span><span class="sxs-lookup"><span data-stu-id="1278e-121">Code samples are stored in the `samples` root folder.</span></span> <span data-ttu-id="1278e-122">它们采用模仿文档结构（位于 `entity-framework` 根文件夹下）的文件夹结构。</span><span class="sxs-lookup"><span data-stu-id="1278e-122">They are organized into a folder structure that mimics the documentation structure (found under the `entity-framework` root folder).</span></span>

## <a name="code-snippets"></a><span data-ttu-id="1278e-123">代码片段</span><span class="sxs-lookup"><span data-stu-id="1278e-123">Code snippets</span></span>

<span data-ttu-id="1278e-124">文章经常使用代码片段来说明要点。</span><span class="sxs-lookup"><span data-stu-id="1278e-124">Articles frequently contain code snippets to illustrate points.</span></span> <span data-ttu-id="1278e-125">DFM 允许将代码复制到 Markdown 文件或引用单独的代码文件。</span><span class="sxs-lookup"><span data-stu-id="1278e-125">DFM lets you copy code into the Markdown file or refer to a separate code file.</span></span> <span data-ttu-id="1278e-126">请尽可能使用单独的代码文件，以最大限度地减少代码中出错的可能性。</span><span class="sxs-lookup"><span data-stu-id="1278e-126">We prefer to use separate code files whenever possible, to minimize the chance of errors in the code.</span></span> <span data-ttu-id="1278e-127">代码文件应存储在使用示例项目的上面所述的文件夹结构的存储库中。</span><span class="sxs-lookup"><span data-stu-id="1278e-127">The code files should be stored in the repo using the folder structure described above for sample projects.</span></span>

<span data-ttu-id="1278e-128">下面是一些 [DFM 代码片段语法](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet)示例。</span><span class="sxs-lookup"><span data-stu-id="1278e-128">Here are some examples of [DFM code snippet syntax](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span></span>

<span data-ttu-id="1278e-129">将整个代码文件呈现为代码片段：</span><span class="sxs-lookup"><span data-stu-id="1278e-129">To render an entire code file as a snippet:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

<span data-ttu-id="1278e-130">使用行号将文件的一部分呈现为代码片段：</span><span class="sxs-lookup"><span data-stu-id="1278e-130">To render a portion of a file as a snippet by using line numbers:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

<span data-ttu-id="1278e-131">有关 C# 代码片段，请参阅 [C# 区域](https://msdn.microsoft.com/library/9a1ybwek.aspx)。</span><span class="sxs-lookup"><span data-stu-id="1278e-131">For C# snippets, you can reference a [C# region](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span></span> <span data-ttu-id="1278e-132">请尽可能使用区域而不是行号，因为代码文件中的行号往往会更改，并与 Markdown 中引用的行号不同步。</span><span class="sxs-lookup"><span data-stu-id="1278e-132">Whenever possible, use regions rather than line numbers, because line numbers in a code file tend to change and get out of sync with line number references in Markdown.</span></span> <span data-ttu-id="1278e-133">可以嵌套 C# 区域，如果引用外部区域，内部 `#region` 和 `#endregion` 指令不会在代码片段中呈现。</span><span class="sxs-lookup"><span data-stu-id="1278e-133">C# regions can be nested, and if you reference the outer region, the inner `#region` and `#endregion` directives are not rendered in a snippet.</span></span>

<span data-ttu-id="1278e-134">呈现名为 “snippet_Example” 的 C# 区域：</span><span class="sxs-lookup"><span data-stu-id="1278e-134">To render a C# region named "snippet_Example":</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

<span data-ttu-id="1278e-135">突出显示呈现的代码段中选定的行（通常呈现为黄色背景）：</span><span class="sxs-lookup"><span data-stu-id="1278e-135">To highlight selected lines in a rendered snippet (usually renders as yellow background color):</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a><span data-ttu-id="1278e-136">使用 DocFX 测试更改</span><span class="sxs-lookup"><span data-stu-id="1278e-136">Test your changes with DocFX</span></span>

<span data-ttu-id="1278e-137">使用 [DocFX 命令行工具](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool)测试更改，这将会创建站点的本地托管版本。</span><span class="sxs-lookup"><span data-stu-id="1278e-137">Test your changes with the [DocFX command line tool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), which creates a locally hosted version of the site.</span></span> <span data-ttu-id="1278e-138">DocFX 不呈现为 docs.microsoft.com 创建的样式和站点扩展。</span><span class="sxs-lookup"><span data-stu-id="1278e-138">DocFX doesn't render style and site extensions created for docs.microsoft.com.</span></span>

<span data-ttu-id="1278e-139">DocFX 需要使用 .NET Framework（对于 Windows）或 Mono（对于 Linux 或 macOS）。</span><span class="sxs-lookup"><span data-stu-id="1278e-139">DocFX requires the .NET Framework on Windows, or Mono for Linux or macOS.</span></span>

### <a name="windows-instructions"></a><span data-ttu-id="1278e-140">Windows 说明</span><span class="sxs-lookup"><span data-stu-id="1278e-140">Windows instructions</span></span>

* <span data-ttu-id="1278e-141">从 [DocFX 发布](https://github.com/dotnet/docfx/releases)下载并解压缩 “docfx.zip”。</span><span class="sxs-lookup"><span data-stu-id="1278e-141">Download and unzip *docfx.zip* from [DocFX releases](https://github.com/dotnet/docfx/releases).</span></span>
* <span data-ttu-id="1278e-142">将 DocFX 添加到路径。</span><span class="sxs-lookup"><span data-stu-id="1278e-142">Add DocFX to your PATH.</span></span>
* <span data-ttu-id="1278e-143">在命令行窗口中，导航到克隆存储库（其中包含“docfx.json”文件），然后运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="1278e-143">In a command line window, navigate to the cloned repository (which contains the *docfx.json* file) and run the following command:</span></span>

   ``` console
   docfx -t default --serve
   ```

* <span data-ttu-id="1278e-144">在浏览器中导航到 `http://localhost:8080`。</span><span class="sxs-lookup"><span data-stu-id="1278e-144">In a browser, navigate to `http://localhost:8080`.</span></span>

### <a name="mono-instructions"></a><span data-ttu-id="1278e-145">Mono 说明</span><span class="sxs-lookup"><span data-stu-id="1278e-145">Mono instructions</span></span>

* <span data-ttu-id="1278e-146">使用 Homebrew 安装 Mono - `brew install mono`。</span><span class="sxs-lookup"><span data-stu-id="1278e-146">Install Mono via Homebrew - `brew install mono`.</span></span>
* <span data-ttu-id="1278e-147">下载 [DocFX 的最新版本](https://github.com/dotnet/docfx/releases/tag/v2.7.2)。</span><span class="sxs-lookup"><span data-stu-id="1278e-147">Download the [latest version of DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span></span>
* <span data-ttu-id="1278e-148">提取到 `\bin\docfx`。</span><span class="sxs-lookup"><span data-stu-id="1278e-148">Extract to `\bin\docfx`.</span></span>
* <span data-ttu-id="1278e-149">为 “docfx” 创建一个别名：</span><span class="sxs-lookup"><span data-stu-id="1278e-149">Create an alias for **docfx**:</span></span>

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* <span data-ttu-id="1278e-150">在克隆存储库中运行“docfx”以生成站点，并在 `http://localhost:8080` 上运行“docfx-serve”以查看站点。</span><span class="sxs-lookup"><span data-stu-id="1278e-150">Run **docfx** in the cloned repository to build the site, and **docfx-serve** to view the site at `http://localhost:8080`.</span></span>

## <a name="voice-and-tone"></a><span data-ttu-id="1278e-151">语气和语调</span><span class="sxs-lookup"><span data-stu-id="1278e-151">Voice and tone</span></span>

<span data-ttu-id="1278e-152">我们的目标是编写被广泛受众所理解的易懂文档。</span><span class="sxs-lookup"><span data-stu-id="1278e-152">Our goal is to write documentation that is easily understandable by the widest possible audience.</span></span> <span data-ttu-id="1278e-153">为此，我们编写了写作风格指南，请参与者遵守。</span><span class="sxs-lookup"><span data-stu-id="1278e-153">To that end we have established guidelines for writing style that we ask our contributors to follow.</span></span> <span data-ttu-id="1278e-154">有关详细信息，请参阅 .NET Core 存储库中的 [语气和语调指南](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md)。</span><span class="sxs-lookup"><span data-stu-id="1278e-154">For more information, see [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) in the .NET Core repo.</span></span>
