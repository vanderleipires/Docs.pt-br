---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: "Usando contadores de desempenho do SignalR em uma função da Web do Azure | Microsoft Docs"
author: guardrex
description: "Como instalar e usar contadores de desempenho do SignalR em uma função da Web do Azure."
keywords: "ASP.NET,SignalR,Performance contador, a função web do azure"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2018
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="c657f-104">Usando contadores de desempenho do SignalR em uma função da Web do Azure</span><span class="sxs-lookup"><span data-stu-id="c657f-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="c657f-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c657f-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c657f-106">Contadores de desempenho do SignalR são usados para monitorar o desempenho do aplicativo em uma função da Web do Azure.</span><span class="sxs-lookup"><span data-stu-id="c657f-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="c657f-107">Os contadores são capturados pelo diagnóstico do Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c657f-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="c657f-108">Instalar contadores de desempenho do SignalR no Azure com *signalr.exe*, da mesma ferramenta usada para aplicativos autônomos ou local.</span><span class="sxs-lookup"><span data-stu-id="c657f-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="c657f-109">Como funções do Azure são transitórias, você pode configurar um aplicativo para instalar e registrar os contadores de desempenho do SignalR durante a inicialização.</span><span class="sxs-lookup"><span data-stu-id="c657f-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c657f-110">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c657f-110">Prerequisites</span></span>

* [<span data-ttu-id="c657f-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="c657f-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="c657f-112">[SDK do Microsoft Azure para Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Observação: reinicie o computador depois de instalar o SDK.**</span><span class="sxs-lookup"><span data-stu-id="c657f-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="c657f-113">Assinatura do Microsoft Azure: para inscrever-se para uma conta de avaliação gratuita do Azure, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="c657f-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="c657f-114">Criando um aplicativo de função Web do Azure que expõe contadores de desempenho do SignalR</span><span class="sxs-lookup"><span data-stu-id="c657f-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="c657f-115">Abra o Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="c657f-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="c657f-116">No Visual Studio 2015, selecione **arquivo** > **novo** > **projeto**.</span><span class="sxs-lookup"><span data-stu-id="c657f-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="c657f-117">No **modelos** painel do **novo projeto** janela sob o **Visual c#** nó, selecione o **nuvem** nó e selecione o **Serviço de nuvem do azure** modelo.</span><span class="sxs-lookup"><span data-stu-id="c657f-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="c657f-118">Nomear o aplicativo **SignalRPerfCounters** e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="c657f-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Novo aplicativo de nuvem](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="c657f-120">No **novo serviço de nuvem do Microsoft Azure** caixa de diálogo, selecione **função Web ASP.NET** e selecione o > botão para adicionar a função ao projeto.</span><span class="sxs-lookup"><span data-stu-id="c657f-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="c657f-121">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c657f-121">Select **OK**.</span></span>

   ![Adicionar função Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="c657f-123">No **novo aplicativo de Web do ASP.NET - WebRole1** caixa de diálogo, selecione o **MVC** modelo e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="c657f-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Adicionar MVC e Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="c657f-125">Em **Solution Explorer**, abra o *wadcfgx* de arquivos em **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="c657f-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Solution Explorer wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="c657f-127">Substitua o conteúdo do arquivo de configuração a seguir e salve o arquivo:</span><span class="sxs-lookup"><span data-stu-id="c657f-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="c657f-128">Abra o **Package Manager Console** de **ferramentas** > **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="c657f-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="c657f-129">Digite os comandos a seguir para instalar a versão mais recente do SignalR e o pacote de utilitários SignalR:</span><span class="sxs-lookup"><span data-stu-id="c657f-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="c657f-130">Configure o aplicativo para instalar os contadores de desempenho do SignalR para a instância de função quando ele é iniciado ou se reciclar.</span><span class="sxs-lookup"><span data-stu-id="c657f-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="c657f-131">Em **Solution Explorer**, com o botão direito no **WebRole1** do projeto e selecione **adicionar** > **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="c657f-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="c657f-132">Nomeie a nova pasta *inicialização*.</span><span class="sxs-lookup"><span data-stu-id="c657f-132">Name the new folder *Startup*.</span></span>

   ![Adicionar pasta de inicialização](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="c657f-134">Copiar o *signalr.exe* arquivo (adicionado com o **Microsoft.AspNet.SignalR.Utils** pacote) de \<pasta do projeto > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< versão > / Ferramentas para a *inicialização* pasta que você criou na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="c657f-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="c657f-135">Em **Solution Explorer**, com o botão direito do *inicialização* pasta e selecione **adicionar** > **Item existente**.</span><span class="sxs-lookup"><span data-stu-id="c657f-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="c657f-136">Na caixa de diálogo que aparece, selecione *signalr.exe* e selecione **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c657f-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Adicionar signalr.exe ao projeto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="c657f-138">Clique com botão direito no *inicialização* pasta que você criou.</span><span class="sxs-lookup"><span data-stu-id="c657f-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="c657f-139">Selecione **Adicionar** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="c657f-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="c657f-140">Selecione o **geral** nó, selecione **arquivo de texto**e o nome do novo item *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="c657f-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="c657f-141">Esse arquivo de comando instalará os contadores de desempenho do SignalR para a função da web.</span><span class="sxs-lookup"><span data-stu-id="c657f-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Criar arquivo de lote de instalação do contador de desempenho de SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="c657f-143">Quando o Visual Studio cria o *SignalRPerfCounterInstall.cmd* arquivo, ele será aberto automaticamente na janela principal.</span><span class="sxs-lookup"><span data-stu-id="c657f-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="c657f-144">Substitua o conteúdo do arquivo com o script a seguir, em seguida, salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="c657f-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="c657f-145">Esse script executa *signalr.exe*, que adiciona os contadores de desempenho do SignalR para a instância de função.</span><span class="sxs-lookup"><span data-stu-id="c657f-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="c657f-146">Selecione o *signalr.exe* arquivo **Gerenciador de soluções**.</span><span class="sxs-lookup"><span data-stu-id="c657f-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="c657f-147">O arquivo **propriedades**, defina **copiar para diretório de saída** para **copiar sempre**.</span><span class="sxs-lookup"><span data-stu-id="c657f-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Definir copiar para diretório de saída para copiar sempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="c657f-149">Repita a etapa anterior para o *SignalRPerfCounterInstall.cmd* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c657f-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="c657f-150">Clique com botão direito no *SignalRPerfCounterInstall.cmd* de arquivo e selecione **abrir com**.</span><span class="sxs-lookup"><span data-stu-id="c657f-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="c657f-151">Na caixa de diálogo que aparece, selecione **Editor binário** e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="c657f-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Abrir com o Editor binário](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="c657f-153">No editor binário, selecione qualquer principais bytes no arquivo e excluí-los.</span><span class="sxs-lookup"><span data-stu-id="c657f-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="c657f-154">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="c657f-154">Save and close the file.</span></span>

    ![Excluir bytes à esquerda](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="c657f-156">Abra *servicedefinition. Csdef* e adicione uma tarefa de inicialização que executa o *SignalrPerfCounterInstall.cmd* arquivo quando o serviço é iniciado:</span><span class="sxs-lookup"><span data-stu-id="c657f-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="c657f-157">Abra `Views/Shared/_Layout.cshtml` e remover o script de pacote jQuery do final do arquivo.</span><span class="sxs-lookup"><span data-stu-id="c657f-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="c657f-158">Adicionar um cliente JavaScript que chama continuamente o `increment` método no servidor.</span><span class="sxs-lookup"><span data-stu-id="c657f-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="c657f-159">Abra `Views/Home/Index.cshtml` e substitua o conteúdo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c657f-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="c657f-160">Criar uma nova pasta de **WebRole1** projeto chamado *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="c657f-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="c657f-161">Com o botão direito do *Hubs* pasta **Solution Explorer**, selecione **Web** > **SignalR**e selecione  **Classe de Hub SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="c657f-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="c657f-162">Nomeie o novo hub *MyHub.cs* e selecione **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c657f-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Adicionar classe de Hub SignalR para a pasta de Hubs na caixa de diálogo Adicionar Novo Item](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="c657f-164">*MyHub.cs* será aberto automaticamente na janela principal.</span><span class="sxs-lookup"><span data-stu-id="c657f-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="c657f-165">Substitua o conteúdo com o código a seguir, em seguida, salve e feche o arquivo:</span><span class="sxs-lookup"><span data-stu-id="c657f-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="c657f-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  é uma densidade de conexão fornecida com o SignalR codebase de ferramenta de teste.</span><span class="sxs-lookup"><span data-stu-id="c657f-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="c657f-167">Como pedais requer uma conexão persistente, você adicionar uma ao seu site para uso durante o teste.</span><span class="sxs-lookup"><span data-stu-id="c657f-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="c657f-168">Adicionar uma nova pasta para o **WebRole1** projeto chamado *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="c657f-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="c657f-169">Esta pasta e selecione **adicionar** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="c657f-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="c657f-170">Nomeie o novo arquivo de classe *MyPersistentConnections.cs* e selecione **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c657f-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="c657f-171">O Visual Studio abrirá o *MyPersistentConnections.cs* arquivo na janela principal.</span><span class="sxs-lookup"><span data-stu-id="c657f-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="c657f-172">Substitua o conteúdo com o código a seguir, em seguida, salve e feche o arquivo:</span><span class="sxs-lookup"><span data-stu-id="c657f-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="c657f-173">Usando o `Startup` classe, os objetos de SignalR iniciar quando OWIN é iniciado.</span><span class="sxs-lookup"><span data-stu-id="c657f-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="c657f-174">Abra ou crie *Startup.cs* e substitua o conteúdo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c657f-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="c657f-175">No código acima, o `OwinStartup` atributo marcar esta classe para iniciar o OWIN.</span><span class="sxs-lookup"><span data-stu-id="c657f-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="c657f-176">O `Configuration` método inicia o SignalR.</span><span class="sxs-lookup"><span data-stu-id="c657f-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="c657f-177">Testar seu aplicativo no emulador do Microsoft Azure pressionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="c657f-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c657f-178">Se você encontrar um **FileLoadException** em **MapSignalR**, altere os redirecionamentos de associação em *Web. config* para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c657f-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="c657f-179">Aguarde cerca de um minuto.</span><span class="sxs-lookup"><span data-stu-id="c657f-179">Wait about one minute.</span></span> <span data-ttu-id="c657f-180">Abra a janela de ferramenta do Gerenciador de nuvem no Visual Studio (**exibição** > **Cloud Explorer**) e expanda o caminho `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="c657f-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="c657f-181">Clique duas vezes em **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="c657f-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="c657f-182">Você deve ver os contadores do SignalR nos dados de tabela.</span><span class="sxs-lookup"><span data-stu-id="c657f-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="c657f-183">Se você não vir a tabela, você precisará inserir novamente suas credenciais de armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="c657f-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="c657f-184">Talvez seja necessário selecionar o **atualizar** para ver a tabela em **Cloud Explorer** ou selecione o **atualização** botão na janela Abrir tabela para ver os dados na tabela.</span><span class="sxs-lookup"><span data-stu-id="c657f-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Selecionar a tabela de contadores de desempenho do WAD no Gerenciador de nuvem do Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Mostrar os contadores coletados na tabela de contadores de desempenho do WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="c657f-187">Para testar seu aplicativo na nuvem, atualizar o **ServiceConfiguration** arquivo e defina o `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` para uma cadeia de conexão de conta de armazenamento do Azure válida.</span><span class="sxs-lookup"><span data-stu-id="c657f-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="c657f-188">Implante o aplicativo em sua assinatura do Azure.</span><span class="sxs-lookup"><span data-stu-id="c657f-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="c657f-189">Para obter detalhes sobre como implantar um aplicativo no Azure, consulte [como criar e implantar um serviço de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="c657f-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="c657f-190">Aguarde alguns minutos.</span><span class="sxs-lookup"><span data-stu-id="c657f-190">Wait a few minutes.</span></span> <span data-ttu-id="c657f-191">Em **Cloud Explorer**, localize a conta de armazenamento configurados acima e localizar o `WADPerformanceCountersTable` a tabela nela.</span><span class="sxs-lookup"><span data-stu-id="c657f-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="c657f-192">Você deve ver os contadores do SignalR nos dados de tabela.</span><span class="sxs-lookup"><span data-stu-id="c657f-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="c657f-193">Se você não vir a tabela, você precisará inserir novamente suas credenciais de armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="c657f-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="c657f-194">Talvez seja necessário selecionar o **atualizar** para ver a tabela em **Cloud Explorer** ou selecione o **atualização** botão na janela Abrir tabela para ver os dados na tabela.</span><span class="sxs-lookup"><span data-stu-id="c657f-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="c657f-195">Agradecimentos especiais a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) para o conteúdo original usado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="c657f-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
