---
uid: signalr/overview/performance/scaleout-with-sql-server
title: "Expansão do SignalR com o SQL Server | Microsoft Docs"
author: MikeWasson
description: "Versões de software usados neste tópico Visual Studio 2013 .NET 4.5 SignalR versões anteriores de versão 2 deste tópico para obter informações sobre versões anteriores do..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 5bf625a1ef8cc8ceab0014fadfab0c8a23dbc8da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server"></a>Expansão do SignalR com o SQL Server
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versão 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
> 
> Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


Neste tutorial, você usará o SQL Server para distribuir mensagens através de um aplicativo de SignalR é implantado em duas instâncias separadas do IIS. Você também pode executar este tutorial em uma máquina de teste único, mas para obter o efeito completo, você precisa implantar o aplicativo de SignalR em dois ou mais servidores. Você também deve instalar o SQL Server em um dos servidores, ou em um servidor dedicado separado. Outra opção é executar o tutorial usando máquinas virtuais no Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Pré-requisitos

Microsoft SQL Server 2005 ou posterior. O backplane dá suporte às edições de desktop e servidor do SQL Server. Ele não oferece suporte a SQL Server Compact Edition ou banco de dados do SQL Azure. (Se o aplicativo está hospedado no Azure, considere o backplane de barramento de serviço.)

## <a name="overview"></a>Visão Geral

Antes de entrar para o tutorial detalhado, aqui está uma visão geral das tarefas que você executará.

1. Crie um novo banco de dados vazio. O backplane criará as tabelas necessárias nesse banco de dados.
2. Adicione esses pacotes do NuGet ao seu aplicativo: 

    - [SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Crie um aplicativo do SignalR.
4. Adicione o seguinte código ao Startup.cs para configurar o backplane: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

 Este código configura o backplane com os valores padrão para [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Para obter informações sobre como alterar esses valores, consulte [desempenho SignalR: métricas de expansão](signalr-performance.md#scaleout_metrics). 

## <a name="configure-the-database"></a>Configurar o banco de dados

Decida se o aplicativo usará a autenticação do Windows ou autenticação do SQL Server para acessar o banco de dados. Independente disso, verifique se que o usuário de banco de dados tem permissões para fazer logon, criar esquemas e criar tabelas.

Crie um novo banco de dados para o backplane usar. Você pode atribuir qualquer nome para o banco de dados. Você não precisa criar tabelas no banco de dados; o backplane criará as tabelas necessárias.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Habilite o Service Broker

É recomendável habilitar o Service Broker para o banco de dados do backplane. O Service Broker fornece suporte nativo para mensagens e enfileiramento no SQL Server, que permite que o backplane receber atualizações com mais eficiência. (No entanto, o backplane também funciona sem o Service Broker.)

Para verificar se o Service Broker está habilitado, consulte o **é\_broker\_habilitado** coluna o **sys. Databases** exibição do catálogo.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Para habilitar o Service Broker, use a seguinte consulta SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Se esta consulta é exibido um deadlock, verifique se não há nenhum aplicativo conectado ao banco de dados.


Se você tiver habilitado o rastreamento, os rastreamentos também mostrará se o Service Broker está habilitado.

## <a name="create-a-signalr-application"></a>Criar um aplicativo de SignalR

Crie um aplicativo de SignalR seguindo um destes tutoriais:

- [Guia de Introdução ao SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introdução ao SignalR 2.0 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Em seguida, modificaremos o aplicativo de chat para dar suporte a expansão com o SQL Server. Primeiro, adicione o pacote SignalR.SqlServer NuGet ao seu projeto. No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Em seguida, abra o arquivo Startup.cs. Adicione o seguinte código para o **configurar** método:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Implantar e executar o aplicativo

Prepare suas instâncias de servidor do Windows para implantar o aplicativo do SignalR.

Adicione a função do IIS. Inclua recursos de "Desenvolvimento de aplicativos", incluindo o protocolo WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Também inclui o serviço de gerenciamento (listadas em "Ferramentas de gerenciamento").

![](scaleout-with-sql-server/_static/image5.png)

**Instalar Web implantação 3.0.** Quando você executar o Gerenciador do IIS, ele solicitará que você instale o Microsoft Web Platform, ou você pode [baixar o intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). No instalador de plataforma, pesquisar para implantação da Web e instalar o Web Deploy 3.0

![](scaleout-with-sql-server/_static/image6.png)

Verifique se o serviço de gerenciamento da Web está em execução. Caso contrário, inicie o serviço. (Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando você adicionou a função do IIS.)

Por fim, abra a porta 8172 para TCP. Essa é a porta que usa a ferramenta de implantação da Web.

Agora você está pronto para implantar o projeto do Visual Studio em seu computador de desenvolvimento para o servidor. No Gerenciador de soluções, clique com botão direito a solução e clique em **publicar**.

Para obter mais documentação sobre a implantação da web, consulte [mapa de conteúdo de implantação da Web do Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Se você implantar o aplicativo em dois servidores, você pode abrir cada instância em uma janela separada do navegador e ver a que cada um receba mensagens do SignalR da outra. (É claro que, em um ambiente de produção, os dois servidores estaria atrás de um balanceador de carga.)

![](scaleout-with-sql-server/_static/image7.png)

Depois de executar o aplicativo, você pode ver que o SignalR criou automaticamente tabelas no banco de dados:

![](scaleout-with-sql-server/_static/image8.png)

SignalR gerencia as tabelas. Como o aplicativo é implantado, não excluir linhas, modifique a tabela e assim por diante.
