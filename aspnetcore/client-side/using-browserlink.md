---
title: "Link do navegador no núcleo do ASP.NET"
author: ncarandini
description: Um recurso do Visual Studio que vincula o ambiente de desenvolvimento com um ou mais navegadores da web
keywords: "ASP.NET Core, o link do navegador, a sincronização de CSS"
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 211dd5d03e6b8414e0b2ed3234d8970c92f72452
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a><span data-ttu-id="c9cc2-104">Introdução ao Link de navegador no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c9cc2-104">Introduction to Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="c9cc2-105">Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c9cc2-105">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c9cc2-106">Link do navegador é um recurso no Visual Studio que cria um canal de comunicação entre o ambiente de desenvolvimento e um ou mais navegadores da web.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-106">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="c9cc2-107">Você pode usar o Link do navegador para atualizar seu aplicativo da web em vários navegadores de uma vez, que é útil para testes entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-107">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="c9cc2-108">Instalação de Link do navegador</span><span class="sxs-lookup"><span data-stu-id="c9cc2-108">Browser Link setup</span></span>

<span data-ttu-id="c9cc2-109">O ASP.NET Core **aplicativo Web** projeto modelos no Visual Studio 2015 e posterior incluem todo o necessário para o Link do navegador.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-109">The ASP.NET Core **Web Application** project templates in Visual Studio 2015 and later include everything needed for Browser Link.</span></span>

<span data-ttu-id="c9cc2-110">Para adicionar o Link do navegador para um projeto que você criou usando o ASP.NET Core **vazio** ou **API da Web** modelo, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-110">To add Browser Link to a project that you created by using the ASP.NET Core **Empty** or **Web API** template, follow these steps:</span></span>

1. <span data-ttu-id="c9cc2-111">Adicionar o *Microsoft.VisualStudio.Web.BrowserLink.Loader* pacote</span><span class="sxs-lookup"><span data-stu-id="c9cc2-111">Add the *Microsoft.VisualStudio.Web.BrowserLink.Loader* package</span></span> 
2. <span data-ttu-id="c9cc2-112">Adicione código de configuração de *Startup.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-112">Add configuration code in the *Startup.cs* file.</span></span>

### <a name="add-the-package"></a><span data-ttu-id="c9cc2-113">Adicionar o pacote</span><span class="sxs-lookup"><span data-stu-id="c9cc2-113">Add the package</span></span>

<span data-ttu-id="c9cc2-114">Como esse é um recurso do Visual Studio, a maneira mais fácil para adicionar o pacote é abrir o **Package Manager Console** (**exibição > outras janelas > Package Manager Console**) e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-114">Since this is a Visual Studio feature, the easiest way to add the package is to open the **Package Manager Console** (**View > Other Windows > Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

<span data-ttu-id="c9cc2-115">Como alternativa, você pode usar **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-115">Alternatively, you can use **NuGet Package Manager**.</span></span>  <span data-ttu-id="c9cc2-116">Clique no nome do projeto em **Solution Explorer**e escolha **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-116">Right-click the project name in **Solution Explorer**, and choose **Manage NuGet Packages**.</span></span> 

![Abra NuGet Package Manager](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="c9cc2-118">Em seguida, localizar e instalar o pacote.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-118">Then find and install the package.</span></span>

![Adicionar o pacote com o NuGet Package Manager](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a><span data-ttu-id="c9cc2-120">Adicione código de configuração</span><span class="sxs-lookup"><span data-stu-id="c9cc2-120">Add configuration code</span></span>

<span data-ttu-id="c9cc2-121">Abra o *Startup.cs* arquivo e no `Configure` método adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-121">Open the *Startup.cs* file, and in the `Configure` method add the following code:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="c9cc2-122">Normalmente esse código está dentro de um `if` bloco que permite que o Link do navegador somente no ambiente de desenvolvimento, como mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-122">Usually that code is inside an `if` block that enables Browser Link only in the Development environment, as shown here:</span></span>

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

<span data-ttu-id="c9cc2-123">Para obter mais informações, consulte [Trabalhando com vários ambientes](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="c9cc2-123">For more information, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="c9cc2-124">Como usar o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="c9cc2-124">How to use Browser Link</span></span>

<span data-ttu-id="c9cc2-125">Quando você tiver um projeto do ASP.NET Core aberta, o Visual Studio mostra o controle de barra de ferramentas do Link do navegador ao lado de **destino de depuração** controle de barra de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu de lista suspensa de Link do navegador](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="c9cc2-127">O controle de barra de ferramentas Link do navegador, você pode:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-127">From the Browser Link toolbar control, you can:</span></span>

- <span data-ttu-id="c9cc2-128">Atualizar o aplicativo web em vários navegadores de uma vez</span><span class="sxs-lookup"><span data-stu-id="c9cc2-128">Refresh the web application in several browsers at once</span></span>
- <span data-ttu-id="c9cc2-129">Abra o **painel de Link do navegador**</span><span class="sxs-lookup"><span data-stu-id="c9cc2-129">Open the **Browser Link Dashboard**</span></span>
- <span data-ttu-id="c9cc2-130">Habilitar ou desabilitar **Link do navegador**</span><span class="sxs-lookup"><span data-stu-id="c9cc2-130">Enable or disable **Browser Link**</span></span>
- <span data-ttu-id="c9cc2-131">Habilitar ou desabilitar a sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="c9cc2-131">Enable or disable CSS Auto-Sync</span></span>

> [!NOTE]
> <span data-ttu-id="c9cc2-132">Alguns plug-ins do Visual Studio, mais notoriamente *Web 2015 de pacote de extensão* e *Web 2017 de pacote de extensão*, oferecem funcionalidade estendida para o Link do navegador, mas alguns dos recursos adicionais não funcionam com o ASP. Projetos de rede principal.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-132">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="c9cc2-133">Atualizar o aplicativo web em vários navegadores de uma vez</span><span class="sxs-lookup"><span data-stu-id="c9cc2-133">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="c9cc2-134">Para escolher um navegador web única para iniciar ao iniciar o projeto, use o menu suspenso no **destino de depuração** controle de barra de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-134">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu suspenso de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="c9cc2-136">Para abrir vários navegadores de uma vez, escolha **procurar com... ** da mesma lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-136">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span>  <span data-ttu-id="c9cc2-137">Mantenha pressionada a tecla CTRL para selecionar os navegadores que você deseja e, em seguida, clique em **procurar**:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-137">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Abrir vários navegadores de uma vez](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="c9cc2-139">Aqui está uma captura de tela de exemplo mostrando o Visual Studio com o modo de exibição do índice aberto e dois navegadores abertas:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-139">Here's a sample screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Sincronizar com o exemplo de dois navegadores](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="c9cc2-141">Passe o mouse sobre o controle de barra de ferramentas de Link do navegador para ver os navegadores que estão conectados ao projeto:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-141">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Passe o mouse dica](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="c9cc2-143">Alterar a exibição do índice, e todos os navegadores conectados são atualizados quando você clicar no botão de atualização de Link do navegador:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-143">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![sincronização de navegadores de alterações](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="c9cc2-145">Link do navegador também funciona com navegadores que você inicia a partir de fora do Visual Studio e navegue até a URL do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-145">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="c9cc2-146">O painel de Link do navegador</span><span class="sxs-lookup"><span data-stu-id="c9cc2-146">The Browser Link Dashboard</span></span>

<span data-ttu-id="c9cc2-147">Abra o painel de Link do navegador de menu para gerenciar a conexão com o navegador abertas suspenso Link do navegador:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-147">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Painel de browserslink abrir](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="c9cc2-149">Se nenhum navegador estiver conectado, você pode iniciar a sessão não depuração clicando o _exibir no navegador_ link:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-149">If no browser is connected, you can start a non debugging session clicking the _View in Browser_ link:</span></span>

![painel browserlink-conexões não](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="c9cc2-151">Caso contrário, os navegadores conectados são mostrados, com o caminho para a página que está mostrando cada navegador:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-151">Otherwise, the connected browsers are shown, with the path to the page that each browser is showing:</span></span>

![browserlink-painel duas conexões](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="c9cc2-153">Se desejar, você pode clicar em um nome de navegador listados para atualizar o navegador único.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-153">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="c9cc2-154">Habilitar ou desabilitar o Link do navegador</span><span class="sxs-lookup"><span data-stu-id="c9cc2-154">Enable or disable Browser Link</span></span>

<span data-ttu-id="c9cc2-155">Quando você habilitar novamente o Link do navegador depois de desabilitá-lo, você precisa atualizar os navegadores para reconectar-se-los.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-155">When you re-enable Browser Link after disabling it, you have to refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="c9cc2-156">Habilitar ou desabilitar a sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="c9cc2-156">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="c9cc2-157">Quando a sincronização automática de CSS está habilitada, navegadores conectados são atualizadas automaticamente quando você fizer qualquer alteração nos arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-157">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="c9cc2-158">Como funciona?</span><span class="sxs-lookup"><span data-stu-id="c9cc2-158">How does it work?</span></span>

<span data-ttu-id="c9cc2-159">Link do navegador usa SignalR para criar um canal de comunicação entre o navegador e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-159">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="c9cc2-160">Quando o Link do navegador é habilitado, o Visual Studio atua como um servidor de SignalR que vários clientes (navegadores) podem se conectar ao.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-160">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="c9cc2-161">Link do navegador também registra um componente de middleware no pipeline de solicitação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-161">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="c9cc2-162">Este componente injeta especial `<script>` referências em cada solicitação de página do servidor.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-162">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="c9cc2-163">Você pode ver as referências de script selecionando **Exibir código-fonte** no navegador e rolar até o final do `<body>` conteúdo de marca:</span><span class="sxs-lookup"><span data-stu-id="c9cc2-163">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="c9cc2-164">Os arquivos de origem não são modificados.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-164">Your source files are not modified.</span></span> <span data-ttu-id="c9cc2-165">O componente de middleware injeta as referências de script dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-165">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="c9cc2-166">Como o código do lado do navegador é todo JavaScript, ele funciona em todos os navegadores com suporte SignalR, sem a necessidade de qualquer plug-in de navegador.</span><span class="sxs-lookup"><span data-stu-id="c9cc2-166">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports, without requiring any browser plug-in.</span></span>
