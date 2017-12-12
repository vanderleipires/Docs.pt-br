---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Habilitando a autenticação do Windows no Katana | Microsoft Docs"
author: MikeWasson
description: "Este artigo mostra como habilitar a autenticação do Windows no Katana. Ele aborda os dois cenários: usando o IIS para hospedar Katana e usando HttpListener para auto-host Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: cc23a053fb1ba60ea84eca59e99f0e375fefc4cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="4397d-104">Habilitando a autenticação do Windows no Katana</span><span class="sxs-lookup"><span data-stu-id="4397d-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="4397d-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4397d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="4397d-106">Este artigo mostra como habilitar a autenticação do Windows no Katana.</span><span class="sxs-lookup"><span data-stu-id="4397d-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="4397d-107">Ele aborda os dois cenários: usando o IIS para hospedar Katana e usando HttpListener hospedar Katana internamente em um processo personalizado.</span><span class="sxs-lookup"><span data-stu-id="4397d-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="4397d-108">Agradecemos Barry Dorrans, David Matson e Chris Ross revisão deste artigo.</span><span class="sxs-lookup"><span data-stu-id="4397d-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="4397d-109">Katana é a implementação da Microsoft [OWIN](http://owin.org/), a Interface da Web aberta para .NET.</span><span class="sxs-lookup"><span data-stu-id="4397d-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="4397d-110">Você pode ler uma introdução ao OWIN e Katana [aqui](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="4397d-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="4397d-111">A arquitetura OWIN tem várias camadas:</span><span class="sxs-lookup"><span data-stu-id="4397d-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="4397d-112">Host: Gerencia o processo no qual é executado o pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="4397d-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="4397d-113">Servidor: Abre um soquete de rede e de escuta solicitações.</span><span class="sxs-lookup"><span data-stu-id="4397d-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="4397d-114">Middleware: Processa a solicitação e resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="4397d-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="4397d-115">Katana atualmente fornece dois servidores, que oferecem suporte a autenticação integrada do Windows:</span><span class="sxs-lookup"><span data-stu-id="4397d-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="4397d-116">**Systemweb**.</span><span class="sxs-lookup"><span data-stu-id="4397d-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="4397d-117">Usa o IIS com o pipeline do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4397d-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="4397d-118">**HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="4397d-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="4397d-119">Usa [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="4397d-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="4397d-120">Este servidor é atualmente a opção padrão quando auto-hospedagem Katana.</span><span class="sxs-lookup"><span data-stu-id="4397d-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="4397d-121">Katana atualmente não fornece middleware OWIN para a autenticação do Windows, porque essa funcionalidade já está disponível nos servidores.</span><span class="sxs-lookup"><span data-stu-id="4397d-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="4397d-122">Autenticação do Windows no IIS</span><span class="sxs-lookup"><span data-stu-id="4397d-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="4397d-123">Usando systemweb, você pode simplesmente habilitar autenticação do Windows no IIS.</span><span class="sxs-lookup"><span data-stu-id="4397d-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="4397d-124">Vamos começar criando um novo aplicativo ASP.NET, usando o modelo de projeto "Aplicativo Web vazio ASP.NET".</span><span class="sxs-lookup"><span data-stu-id="4397d-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="4397d-125">Em seguida, adicione pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="4397d-125">Next, add NuGet packages.</span></span> <span data-ttu-id="4397d-126">Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="4397d-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4397d-127">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="4397d-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="4397d-128">Agora, adicione uma classe denominada `Startup` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4397d-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="4397d-129">Isso é tudo o que você precisa criar um aplicativo "Hello world" para OWIN, em execução no IIS.</span><span class="sxs-lookup"><span data-stu-id="4397d-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="4397d-130">Pressione F5 para depurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4397d-130">Press F5 to debug the application.</span></span> <span data-ttu-id="4397d-131">Você deverá ver "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4397d-131">You should see "Hello World!"</span></span> <span data-ttu-id="4397d-132">Na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="4397d-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="4397d-133">Em seguida, vamos habilitar autenticação do Windows no IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4397d-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="4397d-134">Do **exibição** menu, selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="4397d-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="4397d-135">Clique no nome do projeto no Gerenciador de soluções para exibir as propriedades do projeto.</span><span class="sxs-lookup"><span data-stu-id="4397d-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="4397d-136">No **propriedades** janela, defina **autenticação anônima** para **desabilitado** e defina **autenticação do Windows** para  **Habilitado**.</span><span class="sxs-lookup"><span data-stu-id="4397d-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="4397d-137">Quando você executa o aplicativo do Visual Studio, o IIS Express exigirá credenciais do Windows do usuário.</span><span class="sxs-lookup"><span data-stu-id="4397d-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="4397d-138">Você pode ver isso usando [Fiddler](http://fiddler2.com/home) ou HTTP de outra ferramenta de depuração.</span><span class="sxs-lookup"><span data-stu-id="4397d-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="4397d-139">Aqui está um exemplo de resposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="4397d-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="4397d-140">Os cabeçalhos de WWW-Authenticate nesta resposta indicam que o servidor oferece suporte a [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocolo, que usa o Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="4397d-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="4397d-141">Posteriormente, quando você implanta o aplicativo em um servidor, execute [essas etapas](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar a autenticação do Windows no IIS no servidor.</span><span class="sxs-lookup"><span data-stu-id="4397d-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="4397d-142">Autenticação do Windows no HttpListener</span><span class="sxs-lookup"><span data-stu-id="4397d-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="4397d-143">Se você estiver usando HttpListener hospedar internamente Katana, você pode habilitar a autenticação do Windows diretamente no **HttpListener** instância.</span><span class="sxs-lookup"><span data-stu-id="4397d-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="4397d-144">Primeiro, crie um novo aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="4397d-144">First, create a new console application.</span></span> <span data-ttu-id="4397d-145">Em seguida, adicione pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="4397d-145">Next, add NuGet packages.</span></span> <span data-ttu-id="4397d-146">Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="4397d-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4397d-147">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="4397d-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="4397d-148">Agora, adicione uma classe denominada `Startup` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4397d-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="4397d-149">Essa classe implementa o mesmo exemplo "Olá, mundo" do antes, mas também define a autenticação do Windows como o esquema de autenticação.</span><span class="sxs-lookup"><span data-stu-id="4397d-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="4397d-150">Dentro de `Main` funcionar, iniciar o pipeline OWIN:</span><span class="sxs-lookup"><span data-stu-id="4397d-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="4397d-151">Você pode enviar uma solicitação de Fiddler para confirmar que o aplicativo está usando a autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="4397d-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="4397d-152">Tópicos relacionados</span><span class="sxs-lookup"><span data-stu-id="4397d-152">Related Topics</span></span>

[<span data-ttu-id="4397d-153">Uma visão geral do projeto Katana</span><span class="sxs-lookup"><span data-stu-id="4397d-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="4397d-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="4397d-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[<span data-ttu-id="4397d-155">Noções básicas sobre autenticação de formulários OWIN no MVC 5</span><span class="sxs-lookup"><span data-stu-id="4397d-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
