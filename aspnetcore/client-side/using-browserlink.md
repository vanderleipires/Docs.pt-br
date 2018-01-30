---
title: "Link do navegador no núcleo do ASP.NET"
author: ncarandini
description: "Explica como o Link do navegador é um recurso do Visual Studio que vincula o ambiente de desenvolvimento com um ou mais navegadores da web."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: 3e62bdd180bb1f5e2ce0645a8cf13c9ffe76197e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="92f28-103">Link do navegador no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="92f28-103">Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="92f28-104">Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="92f28-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="92f28-105">Link do navegador é um recurso no Visual Studio que cria um canal de comunicação entre o ambiente de desenvolvimento e um ou mais navegadores da web.</span><span class="sxs-lookup"><span data-stu-id="92f28-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="92f28-106">Você pode usar o Link do navegador para atualizar seu aplicativo da web em vários navegadores de uma vez, que é útil para testes entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="92f28-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="92f28-107">Instalação de Link do navegador</span><span class="sxs-lookup"><span data-stu-id="92f28-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="92f28-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="92f28-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="92f28-109">O ASP.NET Core 2. x **aplicativo Web**, **vazio**, e **API da Web** modelo projetos usam o [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) pacote meta, que contém uma referência de pacote para [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="92f28-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="92f28-110">Portanto, usando o `Microsoft.AspNetCore.All` pacote meta não requer nenhuma ação adicional para disponibilizar o Link do navegador para uso.</span><span class="sxs-lookup"><span data-stu-id="92f28-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="92f28-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="92f28-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="92f28-112">O ASP.NET Core 1. x **aplicativo Web** modelo de projeto possui uma referência de pacote para o [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacote.</span><span class="sxs-lookup"><span data-stu-id="92f28-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="92f28-113">O **vazio** ou **API da Web** projetos de modelo exigem que você adicione uma referência de pacote para `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="92f28-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="92f28-114">Como este é um recurso do Visual Studio, a maneira mais fácil para adicionar o pacote a um **vazio** ou **API da Web** projeto modelo é abrir o **Package Manager Console** (**Exibição** > **outras janelas** > **Package Manager Console**) e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="92f28-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="92f28-115">Como alternativa, você pode usar **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="92f28-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="92f28-116">Clique no nome do projeto em **Solution Explorer** e escolha **gerenciar pacotes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="92f28-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Abra NuGet Package Manager](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="92f28-118">Localizar e instalar o pacote:</span><span class="sxs-lookup"><span data-stu-id="92f28-118">Find and install the package:</span></span>

![Adicionar o pacote com o NuGet Package Manager](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="92f28-120">Configuração</span><span class="sxs-lookup"><span data-stu-id="92f28-120">Configuration</span></span>

<span data-ttu-id="92f28-121">No `Configure` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="92f28-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="92f28-122">Geralmente o código está dentro de um `if` bloco que permite apenas o Link de navegador no ambiente de desenvolvimento, como mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="92f28-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="92f28-123">Para obter mais informações, consulte [Trabalhando com vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="92f28-123">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="92f28-124">Como usar o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="92f28-124">How to use Browser Link</span></span>

<span data-ttu-id="92f28-125">Quando você tiver um projeto do ASP.NET Core aberta, o Visual Studio mostra o controle de barra de ferramentas do Link do navegador ao lado de **destino de depuração** controle de barra de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="92f28-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu de lista suspensa de Link do navegador](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="92f28-127">O controle de barra de ferramentas Link do navegador, você pode:</span><span class="sxs-lookup"><span data-stu-id="92f28-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="92f28-128">Atualize o aplicativo web em vários navegadores de uma vez.</span><span class="sxs-lookup"><span data-stu-id="92f28-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="92f28-129">Abra o **painel de Link do navegador**.</span><span class="sxs-lookup"><span data-stu-id="92f28-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="92f28-130">Habilitar ou desabilitar **Link do navegador**.</span><span class="sxs-lookup"><span data-stu-id="92f28-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="92f28-131">Observação: O Link do navegador está desabilitado por padrão no Visual Studio 2017 (15,3).</span><span class="sxs-lookup"><span data-stu-id="92f28-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="92f28-132">Habilitar ou desabilitar [sincronização automática de CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="92f28-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="92f28-133">Alguns plug-ins do Visual Studio, mais notoriamente *Web 2015 de pacote de extensão* e *Web 2017 de pacote de extensão*, oferecem funcionalidade estendida para o Link do navegador, mas alguns dos recursos adicionais não funcionam com o ASP. Projetos de rede principal.</span><span class="sxs-lookup"><span data-stu-id="92f28-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="92f28-134">Atualizar o aplicativo web em vários navegadores de uma vez</span><span class="sxs-lookup"><span data-stu-id="92f28-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="92f28-135">Para escolher um navegador web única para iniciar ao iniciar o projeto, use o menu suspenso no **destino de depuração** controle de barra de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="92f28-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu suspenso de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="92f28-137">Para abrir vários navegadores de uma vez, escolha **procurar com...**  da mesma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="92f28-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="92f28-138">Mantenha pressionada a tecla CTRL para selecionar os navegadores que você deseja e, em seguida, clique em **procurar**:</span><span class="sxs-lookup"><span data-stu-id="92f28-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Abrir vários navegadores de uma vez](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="92f28-140">Aqui está uma captura de tela mostrando o Visual Studio com o modo de exibição do índice aberto e dois navegadores abertas:</span><span class="sxs-lookup"><span data-stu-id="92f28-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Sincronizar com o exemplo de dois navegadores](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="92f28-142">Passe o mouse sobre o controle de barra de ferramentas de Link do navegador para ver os navegadores que estão conectados ao projeto:</span><span class="sxs-lookup"><span data-stu-id="92f28-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Passe o mouse dica](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="92f28-144">Alterar a exibição do índice, e todos os navegadores conectados são atualizados quando você clicar no botão de atualização de Link do navegador:</span><span class="sxs-lookup"><span data-stu-id="92f28-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browsers-sync-to-changes](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="92f28-146">Link do navegador também funciona com navegadores que você inicia a partir de fora do Visual Studio e navegue até a URL do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="92f28-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="92f28-147">O painel de Link do navegador</span><span class="sxs-lookup"><span data-stu-id="92f28-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="92f28-148">Abra o painel de Link do navegador de menu para gerenciar a conexão com o navegador abertas suspenso Link do navegador:</span><span class="sxs-lookup"><span data-stu-id="92f28-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="92f28-150">Se nenhum navegador estiver conectado, você pode iniciar uma sessão de depuração não, selecionando o *exibir no navegador* link:</span><span class="sxs-lookup"><span data-stu-id="92f28-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="92f28-152">Caso contrário, os navegadores conectados são mostrados com o caminho para a página que está mostrando cada navegador:</span><span class="sxs-lookup"><span data-stu-id="92f28-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="92f28-154">Se desejar, você pode clicar em um nome de navegador listados para atualizar o navegador único.</span><span class="sxs-lookup"><span data-stu-id="92f28-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="92f28-155">Habilitar ou desabilitar o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="92f28-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="92f28-156">Quando você habilitar novamente o Link do navegador depois de desabilitá-lo, você deve atualizar os navegadores para reconectar-se-los.</span><span class="sxs-lookup"><span data-stu-id="92f28-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="92f28-157">Habilitar ou desabilitar a sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="92f28-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="92f28-158">Quando a sincronização automática de CSS está habilitada, navegadores conectados são atualizadas automaticamente quando você fizer qualquer alteração nos arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="92f28-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="92f28-159">Como funciona?</span><span class="sxs-lookup"><span data-stu-id="92f28-159">How does it work?</span></span>

<span data-ttu-id="92f28-160">Link do navegador usa SignalR para criar um canal de comunicação entre o navegador e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92f28-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="92f28-161">Quando o Link do navegador é habilitado, o Visual Studio atua como um servidor de SignalR que vários clientes (navegadores) podem se conectar ao.</span><span class="sxs-lookup"><span data-stu-id="92f28-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="92f28-162">Link do navegador também registra um componente de middleware no pipeline de solicitação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="92f28-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="92f28-163">Este componente injeta especial `<script>` referências em cada solicitação de página do servidor.</span><span class="sxs-lookup"><span data-stu-id="92f28-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="92f28-164">Você pode ver as referências de script selecionando **Exibir código-fonte** no navegador e rolar até o final do `<body>` conteúdo de marca:</span><span class="sxs-lookup"><span data-stu-id="92f28-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="92f28-165">Os arquivos de origem não são modificados.</span><span class="sxs-lookup"><span data-stu-id="92f28-165">Your source files aren't modified.</span></span> <span data-ttu-id="92f28-166">O componente de middleware injeta as referências de script dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="92f28-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="92f28-167">Como o código do lado do navegador é todo JavaScript, ele funciona em todos os navegadores que oferece suporte a SignalR sem a necessidade de um plug-in de navegador.</span><span class="sxs-lookup"><span data-stu-id="92f28-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
