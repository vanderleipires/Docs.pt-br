---
title: Link do navegador no ASP.NET Core
author: ncarandini
description: Explica como o Link do navegador é um recurso do Visual Studio que vincula o ambiente de desenvolvimento com um ou mais navegadores da web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 8808dc705ec87ebf6e7874ad69616ed5bbf61576
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274086"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="6e007-103">Link do navegador no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e007-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="6e007-104">Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6e007-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6e007-105">Link do navegador é um recurso no Visual Studio que cria um canal de comunicação entre o ambiente de desenvolvimento e um ou mais navegadores da web.</span><span class="sxs-lookup"><span data-stu-id="6e007-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="6e007-106">Você pode usar o Link do navegador para atualizar seu aplicativo da web em vários navegadores de uma vez, que é útil para testes entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="6e007-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="6e007-107">Instalação de Link do navegador</span><span class="sxs-lookup"><span data-stu-id="6e007-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6e007-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6e007-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6e007-109">O ASP.NET 2.0 de núcleo **aplicativo Web**, **vazio**, e **API da Web** modelo projetos usam o [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage , que contém uma referência de pacote para [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="6e007-109">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="6e007-110">Portanto, usando o `Microsoft.AspNetCore.All` metapackage não requer nenhuma ação adicional para disponibilizar o Link do navegador para uso.</span><span class="sxs-lookup"><span data-stu-id="6e007-110">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6e007-111">Ao converter um projeto do ASP.NET Core 2.0 em ASP.NET Core 2.1 e fazer a transição para o [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, você deve instalar o [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacote manualmente para a funcionalidade de BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="6e007-111">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, you must install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package manually for BrowserLink functionality.</span></span>

::: moniker-end

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6e007-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6e007-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6e007-113">O ASP.NET Core 1. x **aplicativo Web** modelo de projeto possui uma referência de pacote para o [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacote.</span><span class="sxs-lookup"><span data-stu-id="6e007-113">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="6e007-114">O **vazio** ou **API da Web** projetos de modelo exigem que você adicione uma referência de pacote para `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="6e007-114">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="6e007-115">Como este é um recurso do Visual Studio, a maneira mais fácil para adicionar o pacote a um **vazio** ou **API da Web** projeto modelo é abrir o **Package Manager Console** (**Exibição** > **outras janelas** > **Package Manager Console**) e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="6e007-115">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="6e007-116">Como alternativa, você pode usar **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="6e007-116">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="6e007-117">Clique no nome do projeto em **Solution Explorer** e escolha **gerenciar pacotes NuGet**:</span><span class="sxs-lookup"><span data-stu-id="6e007-117">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Abra NuGet Package Manager](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="6e007-119">Localizar e instalar o pacote:</span><span class="sxs-lookup"><span data-stu-id="6e007-119">Find and install the package:</span></span>

![Adicionar o pacote com o NuGet Package Manager](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="6e007-121">Configuração</span><span class="sxs-lookup"><span data-stu-id="6e007-121">Configuration</span></span>

<span data-ttu-id="6e007-122">No `Configure` método o *Startup.cs* arquivo:</span><span class="sxs-lookup"><span data-stu-id="6e007-122">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="6e007-123">Geralmente o código está dentro de um `if` bloco que permite apenas o Link de navegador no ambiente de desenvolvimento, como mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="6e007-123">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="6e007-124">Para obter mais informações, veja [Usar vários ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6e007-124">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="6e007-125">Como usar o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="6e007-125">How to use Browser Link</span></span>

<span data-ttu-id="6e007-126">Quando um projeto do ASP.NET Core está aberto, o Visual Studio mostra o controle de barra de ferramentas do Link do navegador ao lado do controle de barra de ferramentas do **Destino de Depuração**:</span><span class="sxs-lookup"><span data-stu-id="6e007-126">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu de lista suspensa de Link do navegador](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="6e007-128">O controle de barra de ferramentas Link do navegador, você pode:</span><span class="sxs-lookup"><span data-stu-id="6e007-128">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="6e007-129">Atualizar o aplicativo Web em vários navegadores de uma vez.</span><span class="sxs-lookup"><span data-stu-id="6e007-129">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="6e007-130">Abrir o **Painel de link do navegador**.</span><span class="sxs-lookup"><span data-stu-id="6e007-130">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="6e007-131">Habilitar ou desabilitar **Link do navegador**.</span><span class="sxs-lookup"><span data-stu-id="6e007-131">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="6e007-132">Observação: O Link do navegador está desabilitado por padrão no Visual Studio 2017 (15,3).</span><span class="sxs-lookup"><span data-stu-id="6e007-132">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="6e007-133">Habilitar ou desabilitar [sincronização automática de CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="6e007-133">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="6e007-134">Alguns plug-ins do Visual Studio, mais notoriamente *Web 2015 de pacote de extensão* e *Web 2017 de pacote de extensão*, oferecem funcionalidade estendida para o Link do navegador, mas alguns dos recursos adicionais não funcionam com o ASP. Projetos de rede principal.</span><span class="sxs-lookup"><span data-stu-id="6e007-134">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="6e007-135">Atualizar o aplicativo web em vários navegadores de uma vez</span><span class="sxs-lookup"><span data-stu-id="6e007-135">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="6e007-136">Para escolher um único navegador Web para abrir ao iniciar o projeto, use o menu suspenso do controle de barra de ferramentas do **Destino de depuração**:</span><span class="sxs-lookup"><span data-stu-id="6e007-136">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu suspenso de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="6e007-138">Para abrir vários navegadores de uma vez, escolha **procurar com...**  da mesma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="6e007-138">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="6e007-139">Mantenha pressionada a tecla CTRL para selecionar os navegadores que você deseja e, em seguida, clique em **procurar**:</span><span class="sxs-lookup"><span data-stu-id="6e007-139">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Abrir vários navegadores de uma vez](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="6e007-141">Aqui está uma captura de tela mostrando o Visual Studio com o modo de exibição do índice aberto e dois navegadores abertas:</span><span class="sxs-lookup"><span data-stu-id="6e007-141">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Sincronizar com o exemplo de dois navegadores](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="6e007-143">Passe o mouse sobre o controle de barra de ferramentas de Link do navegador para ver os navegadores que estão conectados ao projeto:</span><span class="sxs-lookup"><span data-stu-id="6e007-143">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Passe o mouse dica](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="6e007-145">Alterar a exibição do índice, e todos os navegadores conectados são atualizados quando você clicar no botão de atualização de Link do navegador:</span><span class="sxs-lookup"><span data-stu-id="6e007-145">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![sincronização de navegadores de alterações](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="6e007-147">Link do navegador também funciona com navegadores que você inicia a partir de fora do Visual Studio e navegue até a URL do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6e007-147">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="6e007-148">O painel de Link do navegador</span><span class="sxs-lookup"><span data-stu-id="6e007-148">The Browser Link Dashboard</span></span>

<span data-ttu-id="6e007-149">Abra o painel de Link do navegador de menu para gerenciar a conexão com o navegador abertas suspenso Link do navegador:</span><span class="sxs-lookup"><span data-stu-id="6e007-149">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Painel de browserslink abrir](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="6e007-151">Se nenhum navegador estiver conectado, você pode iniciar uma sessão de depuração não, selecionando Se nenhum navegador estiver conectado, você poderá iniciar uma sessão de não depuração selecionando o link *exibir no navegador*:</span><span class="sxs-lookup"><span data-stu-id="6e007-151">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="6e007-153">Caso contrário, os navegadores conectados são mostrados com o caminho para a página que está mostrando cada navegador:</span><span class="sxs-lookup"><span data-stu-id="6e007-153">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="6e007-155">Se desejar, você pode clicar em um nome de navegador listados para atualizar o navegador único.</span><span class="sxs-lookup"><span data-stu-id="6e007-155">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="6e007-156">Habilitar ou desabilitar o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="6e007-156">Enable or disable Browser Link</span></span>

<span data-ttu-id="6e007-157">Quando você habilitar novamente o Link do navegador depois de desabilitá-lo, você deve atualizar os navegadores para reconectar-se-los.</span><span class="sxs-lookup"><span data-stu-id="6e007-157">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="6e007-158">Habilitar ou desabilitar a sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="6e007-158">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="6e007-159">Quando a sincronização automática de CSS está habilitada, navegadores conectados são atualizadas automaticamente quando você fizer qualquer alteração nos arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="6e007-159">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="6e007-160">Como funciona?</span><span class="sxs-lookup"><span data-stu-id="6e007-160">How does it work?</span></span>

<span data-ttu-id="6e007-161">Link do navegador usa SignalR para criar um canal de comunicação entre o navegador e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e007-161">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="6e007-162">Quando o Link do navegador é habilitado, o Visual Studio atua como um servidor de SignalR que vários clientes (navegadores) podem se conectar ao.</span><span class="sxs-lookup"><span data-stu-id="6e007-162">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="6e007-163">Link do navegador também registra um componente de middleware no pipeline de solicitação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6e007-163">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="6e007-164">Este componente injeta especial `<script>` referências em cada solicitação de página do servidor.</span><span class="sxs-lookup"><span data-stu-id="6e007-164">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="6e007-165">Você pode ver as referências de script selecionando **Exibir código-fonte** no navegador e rolar até o final do `<body>` conteúdo de marca:</span><span class="sxs-lookup"><span data-stu-id="6e007-165">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="6e007-166">Os arquivos de origem não são modificados.</span><span class="sxs-lookup"><span data-stu-id="6e007-166">Your source files aren't modified.</span></span> <span data-ttu-id="6e007-167">O componente de middleware injeta as referências de script dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="6e007-167">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="6e007-168">Como o código do lado do navegador é todo JavaScript, ele funciona em todos os navegadores que oferece suporte a SignalR sem a necessidade de um plug-in de navegador.</span><span class="sxs-lookup"><span data-stu-id="6e007-168">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
