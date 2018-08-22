---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hospedar a API Web ASP.NET 2 em uma função de trabalho do Azure | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como hospedar a API Web do ASP.NET em uma função de trabalho do Azure, usar o OWIN para auto-hospedar a estrutura da API Web. Open Web Interface para de .NET (OWIN)...
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: cabf88e4e6c946f92a9e4534a4db5ae15dd8cae5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825424"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="f77b6-104">Hospedar a API Web ASP.NET 2 em uma função de trabalho do Azure</span><span class="sxs-lookup"><span data-stu-id="f77b6-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="f77b6-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f77b6-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f77b6-106">Este tutorial mostra como hospedar a API Web do ASP.NET em uma função de trabalho do Azure, usar o OWIN para auto-hospedar a estrutura da API Web.</span><span class="sxs-lookup"><span data-stu-id="f77b6-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="f77b6-107">[Open Web Interface para .NET](http://owin.org/) (OWIN) define uma abstração entre servidores de web do .NET e aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="f77b6-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="f77b6-108">OWIN separa o aplicativo web do servidor, o que torna o OWIN ideal para auto-hospedagem em um aplicativo web em seu próprio processo, fora do IIS – por exemplo, dentro de uma função de trabalho do Azure.</span><span class="sxs-lookup"><span data-stu-id="f77b6-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="f77b6-109">Neste tutorial, você usará o pacote Microsoft.Owin.Host.HttpListener, que fornece um servidor HTTP que ser usado para hospedar internamente os aplicativos do OWIN.</span><span class="sxs-lookup"><span data-stu-id="f77b6-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f77b6-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="f77b6-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f77b6-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f77b6-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f77b6-112">API Web 2</span><span class="sxs-lookup"><span data-stu-id="f77b6-112">Web API 2</span></span>
> - [<span data-ttu-id="f77b6-113">SDK do Azure para .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="f77b6-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="f77b6-114">Criar um projeto do Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f77b6-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="f77b6-115">Inicie o Visual Studio com privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="f77b6-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="f77b6-116">Privilégios de administrador são necessárias para depurar o aplicativo localmente, usando o emulador de computação do Azure.</span><span class="sxs-lookup"><span data-stu-id="f77b6-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="f77b6-117">No **arquivo** menu, clique em **New**, em seguida, clique em **projeto**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="f77b6-118">Partir **modelos instalados**, no Visual c#, clique em **nuvem** e, em seguida, clique em **serviço de nuvem do Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="f77b6-119">Nomeie o projeto "AzureApp" e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="f77b6-120">No **novo serviço de nuvem do Windows Azure** caixa de diálogo, clique duas vezes em **função de trabalho**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="f77b6-121">Deixe o nome padrão ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="f77b6-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="f77b6-122">Esta etapa adiciona uma função de trabalho à solução.</span><span class="sxs-lookup"><span data-stu-id="f77b6-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="f77b6-123">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="f77b6-124">A solução do Visual Studio criado contém dois projetos:</span><span class="sxs-lookup"><span data-stu-id="f77b6-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="f77b6-125">&quot;AzureApp&quot; define as funções e a configuração para o aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="f77b6-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="f77b6-126">&quot;WorkerRole1&quot; contém o código da função de trabalho.</span><span class="sxs-lookup"><span data-stu-id="f77b6-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="f77b6-127">Em geral, um aplicativo do Azure pode conter várias funções, embora este tutorial usa uma única função.</span><span class="sxs-lookup"><span data-stu-id="f77b6-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="f77b6-128">Adicionar a API da Web e pacotes do OWIN</span><span class="sxs-lookup"><span data-stu-id="f77b6-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="f77b6-129">Do **ferramentas** menu, clique em **Gerenciador de pacotes de biblioteca**, em seguida, clique em **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="f77b6-130">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="f77b6-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="f77b6-131">Adicionar um ponto de extremidade HTTP</span><span class="sxs-lookup"><span data-stu-id="f77b6-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="f77b6-132">No Gerenciador de soluções, expanda o projeto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="f77b6-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="f77b6-133">Expanda o nó funções, WorkerRole1 com o botão direito e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="f77b6-134">Clique em **pontos de extremidade**e, em seguida, clique em **Adicionar ponto de extremidade**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="f77b6-135">No **protocolo** lista suspensa, selecione "http".</span><span class="sxs-lookup"><span data-stu-id="f77b6-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="f77b6-136">Na **porta pública** e **porta privada**, digite 80.</span><span class="sxs-lookup"><span data-stu-id="f77b6-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="f77b6-137">Esses números de porta podem ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="f77b6-137">These port numbers can be different.</span></span> <span data-ttu-id="f77b6-138">A porta pública é o que os clientes usam quando eles enviam uma solicitação para a função.</span><span class="sxs-lookup"><span data-stu-id="f77b6-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="f77b6-139">Configurar API da Web para hospedar internamente</span><span class="sxs-lookup"><span data-stu-id="f77b6-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="f77b6-140">No Gerenciador de soluções, com o botão direito do mouse no projeto WorkerRole1 e selecione **Add** / **classe** para adicionar uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="f77b6-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="f77b6-141">Nomeie a classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f77b6-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="f77b6-142">Substitua todo o código clichê nesse arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f77b6-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="f77b6-143">Adicionar um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="f77b6-143">Add a Web API Controller</span></span>

<span data-ttu-id="f77b6-144">Em seguida, adicione uma classe de controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="f77b6-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="f77b6-145">Clique com botão direito no projeto WorkerRole1 e selecione **Add** / **classe**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="f77b6-146">Nomeie a classe TestController.</span><span class="sxs-lookup"><span data-stu-id="f77b6-146">Name the class TestController.</span></span> <span data-ttu-id="f77b6-147">Substitua todo o código clichê nesse arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f77b6-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="f77b6-148">Para simplificar, este controlador apenas define dois métodos GET que retornam texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="f77b6-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="f77b6-149">Iniciar o Host OWIN</span><span class="sxs-lookup"><span data-stu-id="f77b6-149">Start the OWIN Host</span></span>

<span data-ttu-id="f77b6-150">Abra o arquivo WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="f77b6-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="f77b6-151">Essa classe define o código que é executado quando a função de trabalho é iniciada e parada.</span><span class="sxs-lookup"><span data-stu-id="f77b6-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="f77b6-152">Adicione a seguinte instrução using:</span><span class="sxs-lookup"><span data-stu-id="f77b6-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="f77b6-153">Adicionar um **IDisposable** membro para o `WorkerRole` classe:</span><span class="sxs-lookup"><span data-stu-id="f77b6-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="f77b6-154">No `OnStart` método, adicione o seguinte código para iniciar o host:</span><span class="sxs-lookup"><span data-stu-id="f77b6-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="f77b6-155">O **WebApp.Start** método inicia o host do OWIN.</span><span class="sxs-lookup"><span data-stu-id="f77b6-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="f77b6-156">O nome da `Startup` classe é um parâmetro de tipo para o método.</span><span class="sxs-lookup"><span data-stu-id="f77b6-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="f77b6-157">Por convenção, o host chamará o `Configure` método dessa classe.</span><span class="sxs-lookup"><span data-stu-id="f77b6-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="f77b6-158">Substituir a `OnStop` descartar o  *\_aplicativo* instância:</span><span class="sxs-lookup"><span data-stu-id="f77b6-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="f77b6-159">Aqui está o código completo para WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="f77b6-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="f77b6-160">Compile a solução e pressione F5 para executar o aplicativo localmente no emulador de computação do Azure.</span><span class="sxs-lookup"><span data-stu-id="f77b6-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="f77b6-161">Dependendo de suas configurações de firewall, você precisa permitir que o emulador através do firewall.</span><span class="sxs-lookup"><span data-stu-id="f77b6-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="f77b6-162">Se você receber uma exceção semelhante à seguinte, consulte [esta postagem de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) para uma solução alternativa.</span><span class="sxs-lookup"><span data-stu-id="f77b6-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="f77b6-163">"Não foi possível carregar arquivo ou assembly ' Microsoft. owin, versão = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou uma de suas dependências.</span><span class="sxs-lookup"><span data-stu-id="f77b6-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="f77b6-164">Definição do manifesto do assembly localizado não coincide com a referência de assembly.</span><span class="sxs-lookup"><span data-stu-id="f77b6-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="f77b6-165">(Exceção de HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="f77b6-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="f77b6-166">O emulador de computação atribui um endereço IP local para o ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="f77b6-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="f77b6-167">Você pode encontrar o endereço IP, exibindo a interface de usuário do emulador de computação.</span><span class="sxs-lookup"><span data-stu-id="f77b6-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="f77b6-168">Clique com botão direito no ícone do emulador na tarefa de área de notificação da barra e, em seguida, selecione **mostrar IU do emulador de computação**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="f77b6-169">Localize o endereço IP em implantações de serviços de implantação [id], detalhes do serviço.</span><span class="sxs-lookup"><span data-stu-id="f77b6-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="f77b6-170">Abra um navegador da web e navegue até http://<em>endereço</em>/teste/1, onde <em>endereço</em> é o endereço IP atribuído pelo emulador de computação; por exemplo, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="f77b6-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="f77b6-171">Você deve ver a resposta do controlador de API da Web:</span><span class="sxs-lookup"><span data-stu-id="f77b6-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="f77b6-172">Implantar no Azure</span><span class="sxs-lookup"><span data-stu-id="f77b6-172">Deploy to Azure</span></span>

<span data-ttu-id="f77b6-173">Para esta etapa, você deve ter uma conta do Azure.</span><span class="sxs-lookup"><span data-stu-id="f77b6-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="f77b6-174">Se você ainda não tiver uma, você pode criar uma conta de avaliação gratuita em apenas alguns minutos.</span><span class="sxs-lookup"><span data-stu-id="f77b6-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f77b6-175">Para obter detalhes, consulte [avaliação gratuita do Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="f77b6-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="f77b6-176">No Gerenciador de soluções, clique com botão direito no projeto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="f77b6-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="f77b6-177">Selecione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="f77b6-178">Se você não estiver conectado à sua conta do Azure, clique em **Sign In**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="f77b6-179">Depois que você está conectado, escolha uma assinatura e clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="f77b6-180">Insira um nome para o serviço de nuvem e escolha uma região.</span><span class="sxs-lookup"><span data-stu-id="f77b6-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="f77b6-181">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="f77b6-182">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f77b6-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="f77b6-183">A janela de Log de atividades do Azure mostra o progresso da implantação.</span><span class="sxs-lookup"><span data-stu-id="f77b6-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="f77b6-184">Quando o aplicativo é implantado, navegue até http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="f77b6-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="f77b6-185">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="f77b6-185">Additional Resources</span></span>

- [<span data-ttu-id="f77b6-186">Uma visão geral do projeto Katana</span><span class="sxs-lookup"><span data-stu-id="f77b6-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="f77b6-187">Projeto Katana no GitHub</span><span class="sxs-lookup"><span data-stu-id="f77b6-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
