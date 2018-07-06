---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usar o OWIN para auto-hospedar a API Web ASP.NET 2 | Microsoft Docs
author: rick-anderson
description: Este tutorial mostra como hospedar a API Web ASP.NET em um aplicativo de console, usando o OWIN para auto-hospedar a estrutura da API Web. Open Web Interface para .NET (OWIN) d...
ms.author: aspnetcontent
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9fba2774e3873d32115a14fa0c84b99466eda04f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830905"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="13960-104">Usar o OWIN para auto-hospedar a API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="13960-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="13960-105">por [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="13960-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="13960-106">Este tutorial mostra como hospedar a API Web ASP.NET em um aplicativo de console, usando o OWIN para auto-hospedar a estrutura da API Web.</span><span class="sxs-lookup"><span data-stu-id="13960-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="13960-107">[Open Web Interface para .NET](http://owin.org) (OWIN) define uma abstração entre servidores de web do .NET e aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="13960-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="13960-108">OWIN separa o aplicativo web do servidor, o que torna o OWIN ideal para auto-hospedagem em um aplicativo web em seu próprio processo, fora do IIS.</span><span class="sxs-lookup"><span data-stu-id="13960-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="13960-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="13960-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="13960-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (também funciona com o Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="13960-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="13960-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="13960-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="13960-112">Você pode encontrar o código-fonte completo para este tutorial em [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="13960-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="13960-113">Criar um aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="13960-113">Create a Console Application</span></span>

<span data-ttu-id="13960-114">No **arquivo** menu, clique em **New**, em seguida, clique em **projeto**.</span><span class="sxs-lookup"><span data-stu-id="13960-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="13960-115">Partir **modelos instalados**, no Visual c#, clique em **Windows** e, em seguida, clique em **aplicativo de Console**.</span><span class="sxs-lookup"><span data-stu-id="13960-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="13960-116">Nomeie o projeto "OwinSelfhostSample" e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="13960-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="13960-117">Adicionar a API da Web e pacotes do OWIN</span><span class="sxs-lookup"><span data-stu-id="13960-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="13960-118">Do **ferramentas** menu, clique em **Gerenciador de pacotes de biblioteca**, em seguida, clique em **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="13960-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="13960-119">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="13960-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="13960-120">Isso instalará o pacote de selfhost WebAPI OWIN e todos os pacotes necessários do OWIN.</span><span class="sxs-lookup"><span data-stu-id="13960-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="13960-121">Configurar API da Web para hospedar internamente</span><span class="sxs-lookup"><span data-stu-id="13960-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="13960-122">No Gerenciador de soluções, com o botão direito do mouse no projeto e selecione **Add** / **classe** para adicionar uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="13960-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="13960-123">Nomeie a classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="13960-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="13960-124">Substitua todo o código clichê nesse arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="13960-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="13960-125">Adicionar um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="13960-125">Add a Web API Controller</span></span>

<span data-ttu-id="13960-126">Em seguida, adicione uma classe de controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="13960-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="13960-127">No Gerenciador de soluções, com o botão direito do mouse no projeto e selecione **Add** / **classe** para adicionar uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="13960-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="13960-128">Nomeie a classe `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="13960-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="13960-129">Substitua todo o código clichê nesse arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="13960-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="13960-130">Iniciar o Host do OWIN e fazer uma solicitação usando HttpClient</span><span class="sxs-lookup"><span data-stu-id="13960-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="13960-131">Substitua todo o código clichê no arquivo Program.cs pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="13960-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="13960-132">Executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="13960-132">Running the Application</span></span>

<span data-ttu-id="13960-133">Para executar o aplicativo, pressione F5 no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13960-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="13960-134">A saída deve se parecer com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="13960-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="13960-135">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="13960-135">Additional Resources</span></span>

[<span data-ttu-id="13960-136">Uma visão geral do projeto Katana</span><span class="sxs-lookup"><span data-stu-id="13960-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="13960-137">Hospedar a API da Web ASP.NET em uma função de trabalho do Azure</span><span class="sxs-lookup"><span data-stu-id="13960-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
