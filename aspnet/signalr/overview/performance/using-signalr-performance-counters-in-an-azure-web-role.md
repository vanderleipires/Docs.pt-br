---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Usando contadores de desempenho do SignalR em uma função da Web do Azure | Microsoft Docs
author: guardrex
description: Como instalar e usar contadores de desempenho do SignalR em uma função da Web do Azure.
keywords: ASP.NET,SignalR,Performance contador, a função web do azure
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
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Usando contadores de desempenho do SignalR em uma função da Web do Azure

Por [Luke Latham](https://github.com/guardrex)

Contadores de desempenho do SignalR são usados para monitorar o desempenho do aplicativo em uma função da Web do Azure. Os contadores são capturados pelo diagnóstico do Microsoft Azure. Instalar contadores de desempenho do SignalR no Azure com *signalr.exe*, da mesma ferramenta usada para aplicativos autônomos ou local. Como funções do Azure são transitórias, você pode configurar um aplicativo para instalar e registrar os contadores de desempenho do SignalR durante a inicialização.

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [SDK do Microsoft Azure para Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Observação: reinicie o computador depois de instalar o SDK.**
* Assinatura do Microsoft Azure: para inscrever-se para uma conta de avaliação gratuita do Azure, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Criando um aplicativo de função Web do Azure que expõe contadores de desempenho do SignalR

1. Abra o Visual Studio 2015.

2. No Visual Studio 2015, selecione **arquivo** > **novo** > **projeto**.

3. No **modelos** painel do **novo projeto** janela sob o **Visual c#** nó, selecione o **nuvem** nó e selecione o **Serviço de nuvem do azure** modelo. Nomear o aplicativo **SignalRPerfCounters** e selecione **Okey**.

   ![Novo aplicativo de nuvem](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. No **novo serviço de nuvem do Microsoft Azure** caixa de diálogo, selecione **função Web ASP.NET** e selecione o > botão para adicionar a função ao projeto. Selecione **OK**.

   ![Adicionar função Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. No **novo aplicativo de Web do ASP.NET - WebRole1** caixa de diálogo, selecione o **MVC** modelo e selecione **Okey**.

   ![Adicionar MVC e Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. Em **Solution Explorer**, abra o *wadcfgx* de arquivos em **WebRole1**.

   ![Solution Explorer wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Substitua o conteúdo do arquivo de configuração a seguir e salve o arquivo:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Abra o **Package Manager Console** de **ferramentas** > **NuGet Package Manager**. Digite os comandos a seguir para instalar a versão mais recente do SignalR e o pacote de utilitários SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Configure o aplicativo para instalar os contadores de desempenho do SignalR para a instância de função quando ele é iniciado ou se reciclar. Em **Solution Explorer**, com o botão direito no **WebRole1** do projeto e selecione **adicionar** > **nova pasta**. Nomeie a nova pasta *inicialização*.

   ![Adicionar pasta de inicialização](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Copiar o *signalr.exe* arquivo (adicionado com o **Microsoft.AspNet.SignalR.Utils** pacote) de \<pasta do projeto > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< versão > / Ferramentas para a *inicialização* pasta que você criou na etapa anterior.

11. Em **Solution Explorer**, com o botão direito do *inicialização* pasta e selecione **adicionar** > **Item existente**. Na caixa de diálogo que aparece, selecione *signalr.exe* e selecione **adicionar**.

    ![Adicionar signalr.exe ao projeto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Clique com botão direito no *inicialização* pasta que você criou. Selecione **Adicionar** > **Novo Item**. Selecione o **geral** nó, selecione **arquivo de texto**e o nome do novo item *SignalRPerfCounterInstall.cmd*. Esse arquivo de comando instalará os contadores de desempenho do SignalR para a função da web.

    ![Criar arquivo de lote de instalação do contador de desempenho de SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Quando o Visual Studio cria o *SignalRPerfCounterInstall.cmd* arquivo, ele será aberto automaticamente na janela principal. Substitua o conteúdo do arquivo com o script a seguir, em seguida, salve e feche o arquivo. Esse script executa *signalr.exe*, que adiciona os contadores de desempenho do SignalR para a instância de função.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Selecione o *signalr.exe* arquivo **Gerenciador de soluções**. O arquivo **propriedades**, defina **copiar para diretório de saída** para **copiar sempre**.

    ![Definir copiar para diretório de saída para copiar sempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Repita a etapa anterior para o *SignalRPerfCounterInstall.cmd* arquivo.

    
16. Clique com botão direito no *SignalRPerfCounterInstall.cmd* de arquivo e selecione **abrir com**. Na caixa de diálogo que aparece, selecione **Editor binário** e selecione **Okey**.

    ![Abrir com o Editor binário](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. No editor binário, selecione qualquer principais bytes no arquivo e excluí-los. Salve e feche o arquivo.

    ![Excluir bytes à esquerda](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Abra *servicedefinition. Csdef* e adicione uma tarefa de inicialização que executa o *SignalrPerfCounterInstall.cmd* arquivo quando o serviço é iniciado:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Abra `Views/Shared/_Layout.cshtml` e remover o script de pacote jQuery do final do arquivo.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Adicionar um cliente JavaScript que chama continuamente o `increment` método no servidor. Abra `Views/Home/Index.cshtml` e substitua o conteúdo com o código a seguir:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Criar uma nova pasta de **WebRole1** projeto chamado *Hubs*. Com o botão direito do *Hubs* pasta **Solution Explorer**, selecione **Web** > **SignalR**e selecione  **Classe de Hub SignalR (v2)**. Nomeie o novo hub *MyHub.cs* e selecione **adicionar**.

    ![Adicionar classe de Hub SignalR para a pasta de Hubs na caixa de diálogo Adicionar Novo Item](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* será aberto automaticamente na janela principal. Substitua o conteúdo com o código a seguir, em seguida, salve e feche o arquivo:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  é uma densidade de conexão fornecida com o SignalR codebase de ferramenta de teste. Como pedais requer uma conexão persistente, você adicionar uma ao seu site para uso durante o teste. Adicionar uma nova pasta para o **WebRole1** projeto chamado *PersistentConnections*. Esta pasta e selecione **adicionar** > **classe**. Nomeie o novo arquivo de classe *MyPersistentConnections.cs* e selecione **adicionar**.

24. O Visual Studio abrirá o *MyPersistentConnections.cs* arquivo na janela principal. Substitua o conteúdo com o código a seguir, em seguida, salve e feche o arquivo:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Usando o `Startup` classe, os objetos de SignalR iniciar quando OWIN é iniciado. Abra ou crie *Startup.cs* e substitua o conteúdo com o código a seguir:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    No código acima, o `OwinStartup` atributo marcar esta classe para iniciar o OWIN. O `Configuration` método inicia o SignalR.
    
26. Testar seu aplicativo no emulador do Microsoft Azure pressionando **F5**.

    > [!NOTE]
    > Se você encontrar um **FileLoadException** em **MapSignalR**, altere os redirecionamentos de associação em *Web. config* para o seguinte:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Aguarde cerca de um minuto. Abra a janela de ferramenta do Gerenciador de nuvem no Visual Studio (**exibição** > **Cloud Explorer**) e expanda o caminho `(Local)/Storage Accounts/(Development)/Tables`. Clique duas vezes em **WADPerformanceCountersTable**. Você deve ver os contadores do SignalR nos dados de tabela. Se você não vir a tabela, você precisará inserir novamente suas credenciais de armazenamento do Azure. Talvez seja necessário selecionar o **atualizar** para ver a tabela em **Cloud Explorer** ou selecione o **atualização** botão na janela Abrir tabela para ver os dados na tabela.

    ![Selecionar a tabela de contadores de desempenho do WAD no Gerenciador de nuvem do Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Mostrar os contadores coletados na tabela de contadores de desempenho do WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Para testar seu aplicativo na nuvem, atualizar o **ServiceConfiguration** arquivo e defina o `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` para uma cadeia de conexão de conta de armazenamento do Azure válida.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Implante o aplicativo em sua assinatura do Azure. Para obter detalhes sobre como implantar um aplicativo no Azure, consulte [como criar e implantar um serviço de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Aguarde alguns minutos. Em **Cloud Explorer**, localize a conta de armazenamento configurados acima e localizar o `WADPerformanceCountersTable` a tabela nela. Você deve ver os contadores do SignalR nos dados de tabela. Se você não vir a tabela, você precisará inserir novamente suas credenciais de armazenamento do Azure. Talvez seja necessário selecionar o **atualizar** para ver a tabela em **Cloud Explorer** ou selecione o **atualização** botão na janela Abrir tabela para ver os dados na tabela.

Agradecimentos especiais a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) para o conteúdo original usado neste tutorial.
