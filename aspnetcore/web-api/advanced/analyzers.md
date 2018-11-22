---
title: Usar os analisadores da API Web
author: pranavkm
description: Saiba mais sobre os analisadores da API Web em Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 89424d89ec2b3125fd3c6b7c86fed2d292b153e6
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635374"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="37b42-103">Usar os analisadores da API Web</span><span class="sxs-lookup"><span data-stu-id="37b42-103">Use web API analyzers</span></span>

<span data-ttu-id="37b42-104">O ASP.NET Core 2.2 apresenta o pacote NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) contendo analisadores para APIs Web.</span><span class="sxs-lookup"><span data-stu-id="37b42-104">ASP.NET Core 2.2 introduces the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package containing analyzers for web APIs.</span></span> <span data-ttu-id="37b42-105">Os analisadores trabalham com controladores anotados com <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> enquanto fazem a compilação em [convenções de API](xref:web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="37b42-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [API conventions](xref:web-api/advanced/conventions).</span></span>

## <a name="package-installation"></a><span data-ttu-id="37b42-106">Instalação do pacote</span><span class="sxs-lookup"><span data-stu-id="37b42-106">Package installation</span></span>

<span data-ttu-id="37b42-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` pode ser adicionado com uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="37b42-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` can be added with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="37b42-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37b42-108">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="37b42-109">Da janela **Console do Gerenciador de Pacotes**:</span><span class="sxs-lookup"><span data-stu-id="37b42-109">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="37b42-110">Acesse **Exibição** > **Outras Janelas** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="37b42-110">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="37b42-111">Navegue até o diretório no qual o arquivo *ApiConventions.csproj* está localizado.</span><span class="sxs-lookup"><span data-stu-id="37b42-111">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="37b42-112">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="37b42-112">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* <span data-ttu-id="37b42-113">Da caixa de diálogo **Gerenciar Pacotes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="37b42-113">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="37b42-114">Clique com o botão direito do mouse no projeto em **Gerenciador de Soluções** > **Gerenciar Pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="37b42-114">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**.</span></span>
  * <span data-ttu-id="37b42-115">Defina a **Origem do pacote** como "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="37b42-115">Set the **Package source** to "nuget.org".</span></span>
  * <span data-ttu-id="37b42-116">Insira "Microsoft.AspNetCore.Mvc.Api.Analyzers" na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="37b42-116">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
  * <span data-ttu-id="37b42-117">Selecione o pacote "Microsoft.AspNetCore.Mvc.Api.Analyzers" na guia **Procurar** e clique em **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="37b42-117">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the **Browse** tab and click **Install**.</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="37b42-118">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="37b42-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="37b42-119">Clique com o botão direito do mouse na pasta *Pacotes* em **Painel de Soluções** > **Adicionar Pacotes...**.</span><span class="sxs-lookup"><span data-stu-id="37b42-119">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="37b42-120">Defina a lista suspensa **Origem** da janela **Adicionar Pacotes** como "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="37b42-120">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="37b42-121">Insira "Microsoft.AspNetCore.Mvc.Api.Analyzers" na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="37b42-121">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="37b42-122">Selecione o pacote "Microsoft.AspNetCore.Mvc.Api.Analyzers" do painel de resultados e clique em **Adicionar Pacote**.</span><span class="sxs-lookup"><span data-stu-id="37b42-122">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="37b42-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37b42-123">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="37b42-124">Execute o comando a seguir do **Terminal Integrado**:</span><span class="sxs-lookup"><span data-stu-id="37b42-124">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="37b42-125">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="37b42-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="37b42-126">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="37b42-126">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a><span data-ttu-id="37b42-127">Analisadores para convenções de API</span><span class="sxs-lookup"><span data-stu-id="37b42-127">Analyzers for API conventions</span></span>

<span data-ttu-id="37b42-128">Documentos de API aberta contêm códigos de status e tipos de resposta que uma ação pode retornar.</span><span class="sxs-lookup"><span data-stu-id="37b42-128">Open API documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="37b42-129">No ASP.NET Core MVC, atributos como <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> e <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> são usados para documentar uma ação.</span><span class="sxs-lookup"><span data-stu-id="37b42-129">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="37b42-130"><xref:tutorials/web-api-help-pages-using-swagger> apresenta mais detalhes sobre como documentar sua API.</span><span class="sxs-lookup"><span data-stu-id="37b42-130"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your API.</span></span>

<span data-ttu-id="37b42-131">Um dos analisadores no pacote inspeciona controladores anotados com <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> e identifica ações que não documentam totalmente as respostas.</span><span class="sxs-lookup"><span data-stu-id="37b42-131">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="37b42-132">Considere o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="37b42-132">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

<span data-ttu-id="37b42-133">A ação precedente documenta o tipo de retorno com êxito do HTTP 200, mas não documenta o código de status com falha do HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="37b42-133">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="37b42-134">O analisador relata a documentação ausente para o código de status HTTP 404 como um aviso.</span><span class="sxs-lookup"><span data-stu-id="37b42-134">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="37b42-135">É fornecida uma opção para consertar o problema.</span><span class="sxs-lookup"><span data-stu-id="37b42-135">An option to fix the problem is provided.</span></span>
