---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: "Introdução ao OWIN e Katana | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 8922aada723da9b149ec111902fcd883c8241dfb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="958da-102">Introdução ao OWIN e Katana</span><span class="sxs-lookup"><span data-stu-id="958da-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="958da-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="958da-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="958da-104">[Abra a Interface da Web para .NET (OWIN)](http://owin.org/) define uma abstração entre os servidores de web do .NET e aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="958da-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="958da-105">Separando o servidor web do aplicativo, OWIN torna mais fácil criar middleware para o desenvolvimento de web .NET.</span><span class="sxs-lookup"><span data-stu-id="958da-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="958da-106">Além disso, o OWIN torna mais fácil a aplicativos da web de porta para outros hosts &#8212; por exemplo, hospedagem interna em um serviço do Windows ou outro processo.</span><span class="sxs-lookup"><span data-stu-id="958da-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="958da-107">OWIN é uma especificação de propriedade da comunidade, não uma implementação.</span><span class="sxs-lookup"><span data-stu-id="958da-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="958da-108">O projeto Katana é um conjunto de componentes do código-fonte aberto OWIN desenvolvida pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="958da-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="958da-109">Para obter uma visão geral do OWIN e Katana, consulte [uma visão geral de projeto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="958da-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="958da-110">Neste artigo, eu será ir diretamente no código para começar.</span><span class="sxs-lookup"><span data-stu-id="958da-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="958da-111">Este tutorial usa [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mas você também pode usar o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="958da-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="958da-112">Algumas das etapas são diferentes no Visual Studio 2012, o que eu observação abaixo.</span><span class="sxs-lookup"><span data-stu-id="958da-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="958da-113">Host OWIN no IIS</span><span class="sxs-lookup"><span data-stu-id="958da-113">Host OWIN in IIS</span></span>

<span data-ttu-id="958da-114">Nesta seção, vamos vai hospedar OWIN no IIS.</span><span class="sxs-lookup"><span data-stu-id="958da-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="958da-115">Essa opção fornece a flexibilidade e capacidade de combinação de um pipeline OWIN junto com o conjunto de recursos madura do IIS.</span><span class="sxs-lookup"><span data-stu-id="958da-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="958da-116">Usando essa opção, o aplicativo OWIN é executado no pipeline de solicitação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="958da-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="958da-117">Primeiro, crie um novo projeto de aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="958da-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="958da-118">(No Visual Studio 2012, use o tipo de projeto de aplicativo Web vazio ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="958da-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="958da-119">No **novo projeto ASP.NET** caixa de diálogo, selecione o **vazio** modelo.</span><span class="sxs-lookup"><span data-stu-id="958da-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="958da-120">Adicione pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="958da-120">Add NuGet Packages</span></span>

<span data-ttu-id="958da-121">Em seguida, adicione os pacotes do NuGet necessários.</span><span class="sxs-lookup"><span data-stu-id="958da-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="958da-122">Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="958da-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="958da-123">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="958da-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="958da-124">Adicionar uma classe de inicialização</span><span class="sxs-lookup"><span data-stu-id="958da-124">Add a Startup Class</span></span>

<span data-ttu-id="958da-125">Em seguida, adicione uma classe de inicialização OWIN.</span><span class="sxs-lookup"><span data-stu-id="958da-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="958da-126">No Gerenciador de soluções, clique com o botão direito e selecione **adicionar**, em seguida, selecione **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="958da-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="958da-127">No **Adicionar Novo Item** caixa de diálogo, selecione **classe de inicialização Owin**.</span><span class="sxs-lookup"><span data-stu-id="958da-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="958da-128">Para obter mais informações sobre como configurar a classe de inicialização, consulte [detecção de classe de inicialização OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="958da-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="958da-129">Adicione o seguinte código ao método `Startup1.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="958da-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="958da-130">Esse código adiciona uma parte simple de middleware no pipeline OWIN, implementado como uma função que recebe um **Microsoft.Owin.IOwinContext** instância.</span><span class="sxs-lookup"><span data-stu-id="958da-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="958da-131">Quando o servidor recebe uma solicitação HTTP, o pipeline OWIN invoca o middleware.</span><span class="sxs-lookup"><span data-stu-id="958da-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="958da-132">O middleware define o tipo de conteúdo para a resposta e grava o corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="958da-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="958da-133">O modelo de classe de inicialização OWIN está disponível no Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="958da-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="958da-134">Se você estiver usando o Visual Studio 2012, basta adicionar uma nova classe vazia denominada `Startup1`e cole o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="958da-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="958da-135">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="958da-135">Run the Application</span></span>

<span data-ttu-id="958da-136">Pressione F5 para iniciar a depuração.</span><span class="sxs-lookup"><span data-stu-id="958da-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="958da-137">O Visual Studio abrirá uma janela do navegador para `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="958da-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="958da-138">A página deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="958da-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="958da-139">OWIN auto-host em um aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="958da-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="958da-140">É fácil converter este aplicativo de hospedagem do IIS para hospedagem interna em um processo personalizado.</span><span class="sxs-lookup"><span data-stu-id="958da-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="958da-141">Com hospedagem do IIS, IIS atua como o servidor HTTP e o processo de host do servidor.</span><span class="sxs-lookup"><span data-stu-id="958da-141">With IIS hosting, IIS acts as both the HTTP server and as the process that host the sever.</span></span> <span data-ttu-id="958da-142">Com hospedagem própria, seu aplicativo cria o processo e usa o **HttpListener** classe como o servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="958da-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="958da-143">No Visual Studio, crie um novo aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="958da-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="958da-144">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="958da-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="958da-145">Adicionar um `Startup1` classe da parte 1 deste tutorial ao projeto.</span><span class="sxs-lookup"><span data-stu-id="958da-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="958da-146">Você não precisa modificar essa classe.</span><span class="sxs-lookup"><span data-stu-id="958da-146">You don't need to modify this class.</span></span>

<span data-ttu-id="958da-147">Implementar o aplicativo `Main` método da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="958da-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="958da-148">Quando você executa o aplicativo de console, o servidor começa a escutar `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="958da-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="958da-149">Se você navegar para esse endereço em um navegador da web, você verá a página "Olá, mundo".</span><span class="sxs-lookup"><span data-stu-id="958da-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="958da-150">Adicionar OWIN Diagnostics</span><span class="sxs-lookup"><span data-stu-id="958da-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="958da-151">O pacote de pt contém middleware que captura exceções sem tratamento e exibe uma página HTML com os detalhes do erro.</span><span class="sxs-lookup"><span data-stu-id="958da-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="958da-152">Essas funções de página muito como a página de erro do ASP.NET que às vezes é chamada de "[amarela tela de morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="958da-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="958da-153">Como o YSOD, a página de erro Katana é útil durante o desenvolvimento, mas é uma boa prática para desabilitá-lo no modo de produção.</span><span class="sxs-lookup"><span data-stu-id="958da-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="958da-154">Para instalar o pacote de diagnóstico em seu projeto, digite o seguinte comando na janela do Console do Gerenciador de pacotes:</span><span class="sxs-lookup"><span data-stu-id="958da-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="958da-155">Altere o código no seu `Startup1.Configuration` método da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="958da-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="958da-156">Agora use CTRL + F5 para executar o aplicativo sem depuração, para que o Visual Studio não será interrompido na exceção.</span><span class="sxs-lookup"><span data-stu-id="958da-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="958da-157">O aplicativo se comporta como antes, até que você navegue até `http://localhost/fail`, no ponto em que o aplicativo lança a exceção.</span><span class="sxs-lookup"><span data-stu-id="958da-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="958da-158">O middleware de página de erro será capturar a exceção e exibir uma página HTML com informações sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="958da-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="958da-159">Você pode clicar nas guias para ver a pilha de cadeia de caracteres de consulta, cookies, cabeçalho de solicitação e variáveis de ambiente OWIN.</span><span class="sxs-lookup"><span data-stu-id="958da-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="958da-160">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="958da-160">Next Steps</span></span>

- [<span data-ttu-id="958da-161">Detecção de classe de inicialização OWIN</span><span class="sxs-lookup"><span data-stu-id="958da-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="958da-162">Use OWIN para auto-host API da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="958da-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="958da-163">Use OWIN para o próprio Host SignalR</span><span class="sxs-lookup"><span data-stu-id="958da-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
