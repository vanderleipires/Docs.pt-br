---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Criar um aplicativo de cliente do OData v4 (c#) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: daa39fbbb4ff17d61f71bf2a642a9c2260b353e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="97f34-102">Criar um aplicativo de cliente do OData v4 (c#)</span><span class="sxs-lookup"><span data-stu-id="97f34-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="97f34-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="97f34-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="97f34-104">No tutorial anterior, você criou um serviço OData básico que suporta operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="97f34-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="97f34-105">Agora vamos criar um cliente para o serviço.</span><span class="sxs-lookup"><span data-stu-id="97f34-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="97f34-106">Inicie uma nova instância do Visual Studio e crie um novo projeto de aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="97f34-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="97f34-107">No **novo projeto** caixa de diálogo, selecione **instalado** &gt; **modelos** &gt; **Visual C#** &gt; **Windows Desktop**e selecione o **aplicativo de Console** modelo.</span><span class="sxs-lookup"><span data-stu-id="97f34-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="97f34-108">Nomeie o projeto &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="97f34-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="97f34-109">Você também pode adicionar o aplicativo de console para a mesma solução do Visual Studio que contém o serviço OData.</span><span class="sxs-lookup"><span data-stu-id="97f34-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="97f34-110">Instalar o gerador de código de cliente OData</span><span class="sxs-lookup"><span data-stu-id="97f34-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="97f34-111">Do **ferramentas** menu, selecione **extensões e atualizações**.</span><span class="sxs-lookup"><span data-stu-id="97f34-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="97f34-112">Selecione **Online** &gt; **Galeria do Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="97f34-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="97f34-113">Na caixa Pesquisar, procure &quot;gerador de código de cliente OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="97f34-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="97f34-114">Clique em **baixar** para instalar o VSIX.</span><span class="sxs-lookup"><span data-stu-id="97f34-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="97f34-115">Você pode ser solicitado a reiniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97f34-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="97f34-116">Executar o serviço OData localmente</span><span class="sxs-lookup"><span data-stu-id="97f34-116">Run the OData Service Locally</span></span>

<span data-ttu-id="97f34-117">Execute o projeto ProductService do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97f34-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="97f34-118">Por padrão, o Visual Studio inicia um navegador para a raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="97f34-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="97f34-119">Observe o URI. Isso será necessário na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="97f34-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="97f34-120">Deixe o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="97f34-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="97f34-121">Se você colocar os dois projetos na mesma solução, certifique-se de executar o projeto ProductService sem depuração.</span><span class="sxs-lookup"><span data-stu-id="97f34-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="97f34-122">Na próxima etapa, você precisará manter o serviço em execução enquanto você modificar o projeto de aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="97f34-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="97f34-123">Gerar o Proxy de serviço</span><span class="sxs-lookup"><span data-stu-id="97f34-123">Generate the Service Proxy</span></span>

<span data-ttu-id="97f34-124">O proxy de serviço é uma classe .NET que define métodos para acessar o serviço OData.</span><span class="sxs-lookup"><span data-stu-id="97f34-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="97f34-125">O proxy converte chamadas de método em solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="97f34-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="97f34-126">Você criará a classe proxy executando um [modelo T4](https://msdn.microsoft.com/en-us/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="97f34-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/en-us/library/bb126445.aspx).</span></span>

<span data-ttu-id="97f34-127">Clique com botão direito no projeto.</span><span class="sxs-lookup"><span data-stu-id="97f34-127">Right-click the project.</span></span> <span data-ttu-id="97f34-128">Selecione **adicionar** &gt; **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="97f34-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="97f34-129">No **Adicionar Novo Item** caixa de diálogo, selecione **itens do Visual C#** &gt; **código** &gt; **cliente OData**.</span><span class="sxs-lookup"><span data-stu-id="97f34-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="97f34-130">Nomear o modelo &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="97f34-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="97f34-131">Clique em **adicionar** e clique no aviso de segurança.</span><span class="sxs-lookup"><span data-stu-id="97f34-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="97f34-132">Neste ponto, você obterá um erro, que pode ser ignorada.</span><span class="sxs-lookup"><span data-stu-id="97f34-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="97f34-133">O Visual Studio executa automaticamente o modelo, mas o modelo precisa ser algumas configurações primeiro.</span><span class="sxs-lookup"><span data-stu-id="97f34-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="97f34-134">Abra o arquivo ProductClient.odata.config. No `Parameter` elemento, cole no URI do projeto ProductService (etapa anterior).</span><span class="sxs-lookup"><span data-stu-id="97f34-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="97f34-135">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="97f34-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="97f34-136">Execute novamente o modelo.</span><span class="sxs-lookup"><span data-stu-id="97f34-136">Run the template again.</span></span> <span data-ttu-id="97f34-137">No Gerenciador de soluções, clique com botão direito do arquivo ProductClient.tt e selecione **executar personalizado ferramenta**.</span><span class="sxs-lookup"><span data-stu-id="97f34-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="97f34-138">O modelo cria um arquivo de código chamado ProductClient.cs que define o proxy.</span><span class="sxs-lookup"><span data-stu-id="97f34-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="97f34-139">À medida que desenvolve seu aplicativo, se você alterar o ponto de extremidade OData, execute o modelo novamente para atualizar o proxy.</span><span class="sxs-lookup"><span data-stu-id="97f34-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="97f34-140">Use o Proxy de serviço para chamar o serviço OData</span><span class="sxs-lookup"><span data-stu-id="97f34-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="97f34-141">Abra o arquivo Program.cs e substitua o código clichê pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="97f34-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="97f34-142">Substitua o valor de *serviceUri* com o URI do serviço anteriores.</span><span class="sxs-lookup"><span data-stu-id="97f34-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="97f34-143">Quando você executa o aplicativo, ele deve gerar o seguinte:</span><span class="sxs-lookup"><span data-stu-id="97f34-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
