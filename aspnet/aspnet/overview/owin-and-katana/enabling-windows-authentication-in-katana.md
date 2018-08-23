---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Habilitando a autenticação do Windows no Katana | Microsoft Docs
author: MikeWasson
description: 'Este artigo mostra como habilitar a autenticação do Windows no Katana. Ele aborda os dois cenários: usando o IIS para hospedar Katana e usando HttpListener para hospedar internamente Kat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 0aa578020a1f02fa68c74e758014c642219b4265
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834479"
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="d74fd-104">Habilitando a autenticação do Windows no Katana</span><span class="sxs-lookup"><span data-stu-id="d74fd-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="d74fd-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d74fd-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="d74fd-106">Este artigo mostra como habilitar a autenticação do Windows no Katana.</span><span class="sxs-lookup"><span data-stu-id="d74fd-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="d74fd-107">Ele aborda os dois cenários: usando o IIS para hospedar Katana e usando HttpListener para hospedar internamente o Katana em um processo personalizado.</span><span class="sxs-lookup"><span data-stu-id="d74fd-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="d74fd-108">Graças ao Barry Dorrans, David Matson e Chris Ross pela revisão deste artigo.</span><span class="sxs-lookup"><span data-stu-id="d74fd-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="d74fd-109">Katana é a implementação da Microsoft [OWIN](http://owin.org/), Open Web Interface para .NET.</span><span class="sxs-lookup"><span data-stu-id="d74fd-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="d74fd-110">Você pode ler uma introdução ao OWIN e Katana [aqui](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="d74fd-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="d74fd-111">A arquitetura OWIN tem várias camadas:</span><span class="sxs-lookup"><span data-stu-id="d74fd-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="d74fd-112">Host: Gerencia o processo no qual o pipeline do OWIN é executado.</span><span class="sxs-lookup"><span data-stu-id="d74fd-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="d74fd-113">Servidor: Abre um soquete de rede e de escuta as solicitações.</span><span class="sxs-lookup"><span data-stu-id="d74fd-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="d74fd-114">Middleware: Processa a solicitação e resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="d74fd-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="d74fd-115">Atualmente, o Katana fornece dois servidores, que dão suporte a autenticação integrada do Windows:</span><span class="sxs-lookup"><span data-stu-id="d74fd-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="d74fd-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="d74fd-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="d74fd-117">Usa o IIS com o pipeline do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d74fd-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="d74fd-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="d74fd-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="d74fd-119">Usa [HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="d74fd-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="d74fd-120">Este servidor é atualmente a opção padrão quando a hospedagem interna Katana.</span><span class="sxs-lookup"><span data-stu-id="d74fd-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="d74fd-121">Katana atualmente não fornece o middleware OWIN para a autenticação do Windows, porque essa funcionalidade já está disponível nos servidores.</span><span class="sxs-lookup"><span data-stu-id="d74fd-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="d74fd-122">Autenticação do Windows no IIS</span><span class="sxs-lookup"><span data-stu-id="d74fd-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="d74fd-123">Usando systemweb, você pode simplesmente habilitar autenticação do Windows no IIS.</span><span class="sxs-lookup"><span data-stu-id="d74fd-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="d74fd-124">Vamos começar criando um novo aplicativo ASP.NET, usando o modelo de projeto "Aplicativo Web vazio ASP.NET".</span><span class="sxs-lookup"><span data-stu-id="d74fd-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="d74fd-125">Em seguida, adicione pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="d74fd-125">Next, add NuGet packages.</span></span> <span data-ttu-id="d74fd-126">Dos **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d74fd-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d74fd-127">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="d74fd-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="d74fd-128">Agora, adicione uma classe chamada `Startup` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d74fd-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="d74fd-129">Isso é tudo o que você precisa criar um aplicativo "Hello world" para OWIN, em execução no IIS.</span><span class="sxs-lookup"><span data-stu-id="d74fd-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="d74fd-130">Pressione F5 para depurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d74fd-130">Press F5 to debug the application.</span></span> <span data-ttu-id="d74fd-131">Você deve ver "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d74fd-131">You should see "Hello World!"</span></span> <span data-ttu-id="d74fd-132">Na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="d74fd-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="d74fd-133">Em seguida, podemos habilitar autenticação do Windows no IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d74fd-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="d74fd-134">Dos **modo de exibição** menu, selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="d74fd-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="d74fd-135">Clique no nome do projeto no Gerenciador de soluções para exibir as propriedades do projeto.</span><span class="sxs-lookup"><span data-stu-id="d74fd-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="d74fd-136">No **propriedades** janela, defina **autenticação anônima** para **desabilitado** e defina **autenticação do Windows** para  **Habilitado**.</span><span class="sxs-lookup"><span data-stu-id="d74fd-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="d74fd-137">Quando você executa o aplicativo do Visual Studio, o IIS Express exigirá as credenciais do usuário Windows.</span><span class="sxs-lookup"><span data-stu-id="d74fd-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="d74fd-138">Você pode ver isso por meio [Fiddler](http://fiddler2.com/home) ou HTTP de outra ferramenta de depuração.</span><span class="sxs-lookup"><span data-stu-id="d74fd-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="d74fd-139">Aqui está um exemplo de resposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="d74fd-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="d74fd-140">Os cabeçalhos de WWW-Authenticate nesta resposta indicam que o servidor oferece suporte a [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocolo, que usa o Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="d74fd-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="d74fd-141">Posteriormente, quando você implanta o aplicativo em um servidor, siga [essas etapas](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar a autenticação do Windows no IIS nesse servidor.</span><span class="sxs-lookup"><span data-stu-id="d74fd-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="d74fd-142">Autenticação do Windows no HttpListener</span><span class="sxs-lookup"><span data-stu-id="d74fd-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="d74fd-143">Se você estiver usando Microsoft.Owin.Host.HttpListener para hospedar internamente o Katana, você pode habilitar a autenticação do Windows diretamente na **HttpListener** instância.</span><span class="sxs-lookup"><span data-stu-id="d74fd-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="d74fd-144">Primeiro, crie um novo aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="d74fd-144">First, create a new console application.</span></span> <span data-ttu-id="d74fd-145">Em seguida, adicione pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="d74fd-145">Next, add NuGet packages.</span></span> <span data-ttu-id="d74fd-146">Dos **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d74fd-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d74fd-147">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="d74fd-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="d74fd-148">Agora, adicione uma classe chamada `Startup` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d74fd-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="d74fd-149">Essa classe implementa o mesmo exemplo "Hello world" de antes, mas ele também define a autenticação do Windows como o esquema de autenticação.</span><span class="sxs-lookup"><span data-stu-id="d74fd-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="d74fd-150">Dentro de `Main` funcionar, iniciar o pipeline do OWIN:</span><span class="sxs-lookup"><span data-stu-id="d74fd-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="d74fd-151">Você pode enviar uma solicitação no Fiddler para confirmar que o aplicativo está usando a autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="d74fd-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="d74fd-152">Tópicos relacionados</span><span class="sxs-lookup"><span data-stu-id="d74fd-152">Related Topics</span></span>

[<span data-ttu-id="d74fd-153">Uma visão geral do projeto Katana</span><span class="sxs-lookup"><span data-stu-id="d74fd-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="d74fd-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="d74fd-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="d74fd-155">Noções básicas sobre a autenticação de formulários do OWIN no MVC 5</span><span class="sxs-lookup"><span data-stu-id="d74fd-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
