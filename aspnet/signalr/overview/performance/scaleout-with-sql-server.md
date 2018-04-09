---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Expansão do SignalR com o SQL Server | Microsoft Docs
author: MikeWasson
description: Versões de software usados neste tópico Visual Studio 2013 .NET 4.5 SignalR versões anteriores de versão 2 deste tópico para obter informações sobre versões anteriores do...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="bff3f-103">Expansão do SignalR com o SQL Server</span><span class="sxs-lookup"><span data-stu-id="bff3f-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="bff3f-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="bff3f-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="bff3f-105">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="bff3f-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="bff3f-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bff3f-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="bff3f-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="bff3f-107">.NET 4.5</span></span>
> - <span data-ttu-id="bff3f-108">SignalR versão 2</span><span class="sxs-lookup"><span data-stu-id="bff3f-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="bff3f-109">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="bff3f-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="bff3f-110">Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="bff3f-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="bff3f-111">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="bff3f-111">Questions and comments</span></span>
> 
> <span data-ttu-id="bff3f-112">Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="bff3f-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="bff3f-113">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="bff3f-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="bff3f-114">Neste tutorial, você usará o SQL Server para distribuir mensagens através de um aplicativo de SignalR é implantado em duas instâncias separadas do IIS.</span><span class="sxs-lookup"><span data-stu-id="bff3f-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="bff3f-115">Você também pode executar este tutorial em uma máquina de teste único, mas para obter o efeito completo, você precisa implantar o aplicativo de SignalR em dois ou mais servidores.</span><span class="sxs-lookup"><span data-stu-id="bff3f-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="bff3f-116">Você também deve instalar o SQL Server em um dos servidores, ou em um servidor dedicado separado.</span><span class="sxs-lookup"><span data-stu-id="bff3f-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="bff3f-117">Outra opção é executar o tutorial usando máquinas virtuais no Azure.</span><span class="sxs-lookup"><span data-stu-id="bff3f-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="bff3f-118">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="bff3f-118">Prerequisites</span></span>

<span data-ttu-id="bff3f-119">Microsoft SQL Server 2005 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="bff3f-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="bff3f-120">O backplane dá suporte às edições de desktop e servidor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bff3f-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="bff3f-121">Ele não oferece suporte a SQL Server Compact Edition ou banco de dados do SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="bff3f-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="bff3f-122">(Se o aplicativo está hospedado no Azure, considere o backplane de barramento de serviço.)</span><span class="sxs-lookup"><span data-stu-id="bff3f-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="bff3f-123">Visão geral</span><span class="sxs-lookup"><span data-stu-id="bff3f-123">Overview</span></span>

<span data-ttu-id="bff3f-124">Antes de entrar para o tutorial detalhado, aqui está uma visão geral das tarefas que você executará.</span><span class="sxs-lookup"><span data-stu-id="bff3f-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="bff3f-125">Crie um novo banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="bff3f-125">Create a new empty database.</span></span> <span data-ttu-id="bff3f-126">O backplane criará as tabelas necessárias nesse banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bff3f-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="bff3f-127">Adicione esses pacotes do NuGet ao seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="bff3f-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="bff3f-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="bff3f-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="bff3f-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="bff3f-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="bff3f-130">Crie um aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="bff3f-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="bff3f-131">Adicione o seguinte código ao Startup.cs para configurar o backplane:</span><span class="sxs-lookup"><span data-stu-id="bff3f-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="bff3f-132">Este código configura o backplane com os valores padrão para [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="bff3f-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="bff3f-133">Para obter informações sobre como alterar esses valores, consulte [desempenho SignalR: métricas de expansão](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="bff3f-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="bff3f-134">Configurar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="bff3f-134">Configure the Database</span></span>

<span data-ttu-id="bff3f-135">Decida se o aplicativo usará a autenticação do Windows ou autenticação do SQL Server para acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bff3f-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="bff3f-136">Independente disso, verifique se que o usuário de banco de dados tem permissões para fazer logon, criar esquemas e criar tabelas.</span><span class="sxs-lookup"><span data-stu-id="bff3f-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="bff3f-137">Crie um novo banco de dados para o backplane usar.</span><span class="sxs-lookup"><span data-stu-id="bff3f-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="bff3f-138">Você pode atribuir qualquer nome para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bff3f-138">You can give the database any name.</span></span> <span data-ttu-id="bff3f-139">Você não precisa criar tabelas no banco de dados; o backplane criará as tabelas necessárias.</span><span class="sxs-lookup"><span data-stu-id="bff3f-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="bff3f-140">Habilite o Service Broker</span><span class="sxs-lookup"><span data-stu-id="bff3f-140">Enable Service Broker</span></span>

<span data-ttu-id="bff3f-141">É recomendável habilitar o Service Broker para o banco de dados do backplane.</span><span class="sxs-lookup"><span data-stu-id="bff3f-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="bff3f-142">O Service Broker fornece suporte nativo para mensagens e enfileiramento no SQL Server, que permite que o backplane receber atualizações com mais eficiência.</span><span class="sxs-lookup"><span data-stu-id="bff3f-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="bff3f-143">(No entanto, o backplane também funciona sem o Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="bff3f-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="bff3f-144">Para verificar se o Service Broker está habilitado, consulte o **é\_broker\_habilitado** coluna o **sys. Databases** exibição do catálogo.</span><span class="sxs-lookup"><span data-stu-id="bff3f-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="bff3f-145">Para habilitar o Service Broker, use a seguinte consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="bff3f-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="bff3f-146">Se esta consulta é exibido um deadlock, verifique se não há nenhum aplicativo conectado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bff3f-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="bff3f-147">Se você tiver habilitado o rastreamento, os rastreamentos também mostrará se o Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="bff3f-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="bff3f-148">Criar um aplicativo de SignalR</span><span class="sxs-lookup"><span data-stu-id="bff3f-148">Create a SignalR Application</span></span>

<span data-ttu-id="bff3f-149">Crie um aplicativo de SignalR seguindo um destes tutoriais:</span><span class="sxs-lookup"><span data-stu-id="bff3f-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="bff3f-150">Guia de Introdução ao SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="bff3f-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="bff3f-151">Introdução ao SignalR 2.0 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="bff3f-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="bff3f-152">Em seguida, modificaremos o aplicativo de chat para dar suporte a expansão com o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bff3f-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="bff3f-153">Primeiro, adicione o pacote SignalR.SqlServer NuGet ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="bff3f-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="bff3f-154">No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="bff3f-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bff3f-155">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="bff3f-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="bff3f-156">Em seguida, abra o arquivo Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="bff3f-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="bff3f-157">Adicione o seguinte código para o **configurar** método:</span><span class="sxs-lookup"><span data-stu-id="bff3f-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="bff3f-158">Implantar e executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="bff3f-158">Deploy and Run the Application</span></span>

<span data-ttu-id="bff3f-159">Prepare suas instâncias de servidor do Windows para implantar o aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="bff3f-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="bff3f-160">Adicione a função do IIS.</span><span class="sxs-lookup"><span data-stu-id="bff3f-160">Add the IIS role.</span></span> <span data-ttu-id="bff3f-161">Inclua recursos de "Desenvolvimento de aplicativos", incluindo o protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bff3f-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="bff3f-162">Também inclui o serviço de gerenciamento (listadas em "Ferramentas de gerenciamento").</span><span class="sxs-lookup"><span data-stu-id="bff3f-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="bff3f-163">**Instalar Web implantação 3.0.**</span><span class="sxs-lookup"><span data-stu-id="bff3f-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="bff3f-164">Quando você executar o Gerenciador do IIS, ele solicitará que você instale o Microsoft Web Platform, ou você pode [baixar o intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="bff3f-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="bff3f-165">No instalador de plataforma, pesquisar para implantação da Web e instalar o Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="bff3f-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="bff3f-166">Verifique se o serviço de gerenciamento da Web está em execução.</span><span class="sxs-lookup"><span data-stu-id="bff3f-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="bff3f-167">Caso contrário, inicie o serviço.</span><span class="sxs-lookup"><span data-stu-id="bff3f-167">If not, start the service.</span></span> <span data-ttu-id="bff3f-168">(Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando você adicionou a função do IIS.)</span><span class="sxs-lookup"><span data-stu-id="bff3f-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="bff3f-169">Por fim, abra a porta 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="bff3f-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="bff3f-170">Essa é a porta que usa a ferramenta de implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="bff3f-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="bff3f-171">Agora você está pronto para implantar o projeto do Visual Studio em seu computador de desenvolvimento para o servidor.</span><span class="sxs-lookup"><span data-stu-id="bff3f-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="bff3f-172">No Gerenciador de soluções, clique com botão direito a solução e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="bff3f-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="bff3f-173">Para obter mais documentação sobre a implantação da web, consulte [mapa de conteúdo de implantação da Web do Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="bff3f-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="bff3f-174">Se você implantar o aplicativo em dois servidores, você pode abrir cada instância em uma janela separada do navegador e ver a que cada um receba mensagens do SignalR da outra.</span><span class="sxs-lookup"><span data-stu-id="bff3f-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="bff3f-175">(É claro que, em um ambiente de produção, os dois servidores estaria atrás de um balanceador de carga.)</span><span class="sxs-lookup"><span data-stu-id="bff3f-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="bff3f-176">Depois de executar o aplicativo, você pode ver que o SignalR criou automaticamente tabelas no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="bff3f-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="bff3f-177">SignalR gerencia as tabelas.</span><span class="sxs-lookup"><span data-stu-id="bff3f-177">SignalR manages the tables.</span></span> <span data-ttu-id="bff3f-178">Como o aplicativo é implantado, não excluir linhas, modifique a tabela e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="bff3f-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
