---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: "Laboratório prático: Aplicativos de Web em tempo real com SignalR | Microsoft Docs"
author: rick-anderson
description: "A capacidade de enviar por push do lado do servidor de conteúdo para os clientes conectados quando isso acontece, em tempo real de recursos de aplicativos Web em tempo real. Para desenvolvedores do ASP.NET, ASP..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 22123a9c61e6830f3f9f66a45182e1e923950341
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Laboratório prático: Aplicativos de Web em tempo real com SignalR
====================
por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](http://aka.ms/webcamps-training-kit)

> A capacidade de enviar por push do lado do servidor de conteúdo para os clientes conectados quando isso acontece, em tempo real de recursos de aplicativos Web em tempo real. Para desenvolvedores do ASP.NET, **ASP.NET SignalR** é uma biblioteca para adicionar a funcionalidade da web em tempo real para seus aplicativos. Tira proveito dos vários transportes, selecionar automaticamente o melhor considerando o cliente e transporte disponível melhor do servidor de transporte disponível. Tira proveito dos **WebSocket**, uma API de HTML5 que habilita comunicação bidirecional entre o navegador e o servidor.
> 
> **SignalR** também fornece uma API simple e de alto nível para fazer o servidor para cliente RPC (chamada de funções JavaScript em navegadores de seus clientes de código do .NET do servidor) no seu aplicativo ASP.NET, bem como adicionar ganchos úteis para o gerenciamento de conexão como conectar/desconectar eventos, agrupamento de conexões e autorização.
> 
> **SignalR** é uma abstração sobre alguns dos transportes que são necessários para fazer o trabalho em tempo real entre cliente e servidor. Um **SignalR** conexão começa como HTTP e, em seguida, é promovido para um **WebSocket** conexão se estiver disponível. **WebSocket** é o transporte ideal para **SignalR**, pois ele torna o uso mais eficiente da memória de servidor, tem a menor latência e os recursos mais subjacentes (como full-duplex comunicação entre cliente e servidor), mas ele também tem os requisitos mais rígidos: **WebSocket** requer que o servidor usando **Windows Server 2012** ou **Windows 8**, juntamente com **Do .NET framework 4.5**. Se esses requisitos não forem atendidos, **SignalR** tentará usar outros transportes para fazer suas conexões (como *Ajax sondagens longa*).
> 
> O **SignalR** API contém dois modelos para comunicação entre clientes e servidores: **conexões persistentes** e **Hubs**. Um **Conexão** representa um ponto de extremidade simple para enviar um único destinatário, agrupados ou mensagens de difusão. Um **Hub** é um pipeline de mais alto nível construído com a API de Conexão que permite que o cliente e o servidor chamar os métodos diretamente entre si.
> 
> ![Arquitetura de SignalR](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Visão geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Envie notificações do servidor para o cliente usando o SignalR.
- Dimensionar seu aplicativo SignalR usando **do SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

O exemplo a seguir é necessário para concluir este laboratório prático:

- [O Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou maior

<a id="Setup"></a>
### <a name="setup"></a>Configuração

Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro.

1. Abra uma janela do Windows Explorer e navegue até o laboratório **fonte** pasta.
2. Clique com botão direito **Setup.cmd** e selecione **executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instale os trechos de código do Visual Studio para este laboratório.
3. Se a caixa de diálogo controle de conta de usuário for exibida, confirme a ação para continuar.

> [!NOTE]
> Verifique se que você selecionou todas as dependências para este laboratório antes de executar a instalação.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando os trechos de código

Em todo o documento de laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maioria do código é fornecido como o Visual Studio trechos de código que pode ser acessada de dentro do Visual Studio 2013 para evitar a necessidade para adicioná-lo manualmente.

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial localizada no **começar** pasta do exercício que permite que você execute cada exercício independentemente de outras pessoas. Esteja ciente de que os trechos de código que são adicionados durante um exercício estão ausentes dos seguintes iniciando soluções e podem não funcionar até que você concluiu o exercício. Dentro do código-fonte para um exercício, você também encontrará um **final** pasta que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente. Você pode usar essas soluções como orientação se você precisar de ajuda adicional usando este laboratório prático.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Trabalhando com dados em tempo real com SignalR](#Exercise1)
2. [Expansão usando o SQL Server](#Exercise2)

Tempo estimado para concluir este laboratório: **60 minutos**

> [!NOTE]
> Quando você inicia o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida foi projetada para coincidir com um estilo de desenvolvimento específico e determina o comportamento do editor, layouts de janela, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma tarefa específica no Visual Studio ao usar o **configurações gerais de desenvolvimento** coleção. Se você escolher uma coleção de configurações diferentes para o seu ambiente de desenvolvimento, pode haver diferenças nas etapas que você deve levar em conta.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Exercício 1: Trabalhando com dados em tempo real com SignalR

Enquanto bate-papo geralmente é usada como um exemplo, você pode fazer um todo muito mais com a funcionalidade da Web em tempo real. Sempre que um usuário atualiza uma página da web para ver os novos dados ou implementa a página Ajax longo de sondagem para recuperar novos dados, você pode usar o SignalR.

Dá suporte ao SignalR **por push do servidor** ou **difusão** funcionalidade; trata o gerenciamento de conexão automaticamente. Em conexões HTTP clássicos para comunicação entre cliente e servidor, conexão for restabelecida para cada solicitação, mas o SignalR fornece uma conexão persistente entre o cliente e o servidor. No SignalR que chama o código do servidor para um código de cliente no navegador usando chamadas de procedimento remoto (RPC), em vez do modelo de solicitação-resposta sabemos hoje.

Neste exercício, você configurará o **nerd teste** aplicativo para usar o SignalR para exibir o painel de estatísticas com as métricas atualizados sem a necessidade de atualizar a página inteira.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Tarefa 1 – Explorando a página de estatísticas de teste nerd

Nesta tarefa, você percorrer o aplicativo e verificar como a página de estatísticas é mostrada e como você pode melhorar a maneira como as informações é atualizado.

1. Abra **Visual Studio Express 2013 para Web** e abra o **GeekQuiz.sln** solução localizada no **Source\Ex1 WorkingWithRealTimeData\Begin** pasta.
2. Pressione **F5** para executar a solução. O **login** página deve ser exibida no navegador.

    ![Executar a solução](real-time-web-applications-with-signalr/_static/image2.png "executando a solução")

    *Executar a solução*
3. Clique em **registrar** no canto superior direito da página para criar um novo usuário do aplicativo.

    ![Registrar o link](real-time-web-applications-with-signalr/_static/image3.png "link Register")

    *Link de registro*
4. No **registrar** , insira um **nome de usuário** e **senha**e, em seguida, clique em **registrar**.

    ![Registrar um usuário](real-time-web-applications-with-signalr/_static/image4.png "registrar um usuário")

    *Registrar um usuário*
5. O aplicativo registra a nova conta e o usuário é autenticado e redirecionado de volta para a home page, mostrando a primeira pergunta de teste.
6. Abra o **estatísticas** página em uma nova janela e colocar o **início** página e **estatísticas** página lado a lado.

    ![Windows lado a lado](real-time-web-applications-with-signalr/_static/image5.png "lado a lado windows lado")

    *Windows lado a lado*
7. No **início** página, responder à pergunta clicando em uma das opções.

    ![Responder a uma pergunta](real-time-web-applications-with-signalr/_static/image6.png "responder a uma pergunta")

    *Responder a uma pergunta*
8. Depois de clicar em um dos botões, a resposta deve aparecer.

    ![Pergunta para a resposta correta](real-time-web-applications-with-signalr/_static/image7.png "pergunta para a resposta correta")

    *Pergunta corretamente*
9. Observe que as informações fornecidas na página de estatísticas estão desatualizadas. Atualize a página para ver os resultados atualizados.

    ![Página de estatísticas](real-time-web-applications-with-signalr/_static/image8.png "página de estatísticas")

    *Página de estatísticas*
10. Volte para o Visual Studio e parar a depuração.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Tarefa 2 – adicionar SignalR para especialista de teste para mostrar gráficos Online

Nesta tarefa, você adicionar SignalR para a solução e enviar atualizações para os clientes automaticamente quando uma nova resposta é enviada ao servidor.

1. Do **ferramentas** menu do Visual Studio, selecione **Gerenciador de biblioteca de pacote**e, em seguida, clique em **Package Manager Console**.
2. No **Package Manager Console** janela, execute o seguinte comando:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Instalação do pacote SignalR](real-time-web-applications-with-signalr/_static/image9.png "instalação do pacote SignalR")

    *Instalação do pacote SignalR*

    > [!NOTE]
    > Ao instalar **SignalR** versão de pacotes do NuGet 2.0.2 de um aplicativo MVC 5 totalmente novato, você precisará atualizar manualmente **OWIN** pacotes para versão 2.0.1 (ou superior) antes de instalar o SignalR. Para fazer isso, você pode executar o script a seguir no **Package Manager Console**:
    > 
    > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
    > 
    > Em uma versão futura do SignalR, dependências OWIN serão atualizadas automaticamente.
3. Em **Solution Explorer**, expanda o **Scripts** pasta e observe que o SignalR *js* arquivos foram adicionados à solução.

    ![SignalR JavaScript referencia](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript faz referência")

    *Referências de SignalR JavaScript*
4. Em **Solution Explorer**, com o botão direito do **GeekQuiz** projeto, selecione **adicionar** | **nova pasta**e nomeie-  **Hubs**.
5. Clique com botão direito do **Hubs** pasta e selecione **adicionar | Novo Item**.

    ![Adicionar novo item](real-time-web-applications-with-signalr/_static/image11.png "adicionar novo item")

    *Adicionar novo item*
6. No **Adicionar Novo Item** caixa de diálogo, selecione o **Visual c# | Web | SignalR** nó no painel esquerdo, selecione **classe de Hub SignalR (v2)** no painel central, nomeie o arquivo **StatisticsHub.cs** e clique em **adicionar**.

    ![Caixa de diálogo Adicionar novo item](real-time-web-applications-with-signalr/_static/image12.png "caixa de diálogo Adicionar novo item")

    *Adicionar caixa de diálogo novo item*
7. Substitua o código no **StatisticsHub** classe com o código a seguir.

    (Código de trecho - *StatisticsHubClass RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Abra **Startup.cs** e adicione a seguinte linha no final de **configuração** método.

    (Código de trecho - *MapSignalR RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Abra o **StatisticsService.cs** página dentro de **serviços** pasta e adicione o seguinte usando diretivas.

    (Código de trecho - *UsingDirectives RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Para notificar clientes conectados de atualizações, você recuperar um **contexto** objeto para a conexão atual. O **Hub** objeto contém métodos para enviar mensagens para um único cliente ou difusão para todos os clientes. Adicione o seguinte método para o **StatisticsService** classe para transmitir os dados de estatísticas.

    (Código de trecho - *NotifyUpdatesMethod RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > No código acima, você está usando um nome de método arbitrário para chamar uma função no cliente (por exemplo: *updateStatistics*). O nome do método que você especificar será interpretado como um objeto dinâmico, o que significa que não há IntelliSense ou validação de tempo de compilação para ele. A expressão é avaliada em tempo de execução. Quando a chamada do método é executado, o SignalR envia o nome do método e os valores de parâmetro para o cliente. Se o cliente tiver um método que corresponde ao nome, esse método é chamado e os valores de parâmetro são passados para ele. Se nenhum método correspondente for encontrado no cliente, nenhum erro será gerado. Para obter mais informações, consulte [ASP.NET SignalR Hubs API guia](../guide-to-the-api/hubs-api-guide-server.md).
11. Abrir o **TriviaController.cs** página dentro de **controladores** pasta e adicione o seguinte usando diretivas.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Adicione o seguinte código realçado para o **Post** método de ação.

    (Código de trecho - *NotifyUpdatesCall RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Abra o **Statistics.cshtml** página dentro de **exibições | Início** pasta. Localize o **Scripts** seção e adicione as seguintes referências de script no início da seção.

    (Código de trecho - *SignalRScriptReferences RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Quando você adiciona o SignalR e outras bibliotecas de script ao seu projeto do Visual Studio, o Gerenciador de pacotes pode instalar uma versão do arquivo de script SignalR é mais recente que a versão mostrada neste tópico. Certifique-se de que a referência de script em seu código corresponde à versão da biblioteca de script instalada em seu projeto.
14. Adicione o seguinte código realçado para conectar o cliente para o hub SignalR e atualizar os dados de estatísticas quando uma nova mensagem é recebida do hub.

    (Código de trecho - *SignalRClientCode RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    Nesse código, você está criando um Proxy do Hub e registrar um manipulador de eventos para escutar mensagens enviadas pelo servidor. Nesse caso, você escuta as mensagens enviadas por meio de *updateStatistics* método.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarefa 3 – executar a solução

Nesta tarefa, você executará a solução para verificar se o modo de exibição de estatísticas é atualizado automaticamente com o SignalR depois de responder a uma nova pergunta.

1. Pressione **F5** para executar a solução.

    > [!NOTE]
    > Se ainda não estiver conectado ao aplicativo, faça logon com o usuário que você criou na tarefa 1.
2. Abra o **estatísticas** página em uma nova janela e colocar o **início** página e **estatísticas** página lado a lado, como você fez na tarefa 1.
3. No **início** página, responder à pergunta clicando em uma das opções.

    ![Responder a pergunta outra](real-time-web-applications-with-signalr/_static/image13.png "responder outra pergunta")

    *Responder a outra pergunta*
4. Depois de clicar em um dos botões, a resposta deve aparecer. Observe que as informações na página de estatísticas são atualizadas automaticamente após responder a pergunta com as informações atualizadas sem a necessidade de atualizar a página inteira.

    ![Página de estatísticas atualizadas após a resposta](real-time-web-applications-with-signalr/_static/image14.png "página estatísticas atualizadas após a resposta")

    *Página de estatísticas atualizada após a resposta*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Exercício 2: Expansão usando o SQL Server

Ao dimensionar um aplicativo web, você geralmente pode escolher entre *expansão* e *expansão* opções. *Dimensionar o* significa usar um servidor maior, com mais recursos (CPU, RAM, etc.) ao *expansão* significa adicionando mais servidores para tratar a carga. O problema com o último é que os clientes podem obter roteados para diferentes servidores. Um cliente que está conectado a um servidor não receberá mensagens enviadas de outro servidor.

Você pode resolver esses problemas usando um componente denominado *backplane*para encaminhar mensagens entre servidores. Com um backplane habilitado, cada instância de aplicativo envia mensagens ao backplane e backplane encaminha-os para as outras instâncias do aplicativo.

Atualmente, há três tipos de painéis posteriores para SignalR:

- **Windows Azure Service Bus**. Barramento de serviço é uma infraestrutura de mensagens que permite que os componentes enviar mensagens acoplados de forma flexível.
- **SQL Server**. O backplane de SQL Server grava mensagens de tabelas SQL. Plano posterior usa o Service Broker para mensagens eficiente. No entanto, também funciona se o Service Broker não está habilitado.
- **Redis**. Redis é um repositório de chave-valor na memória. Redis oferece suporte a um padrão de publicação/assinatura ("pub/sub") para enviar mensagens.

Cada mensagem é enviada por meio de um barramento de mensagem. Implementa um barramento de mensagem a [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, que fornece uma abstração de publicação/assinatura. Os painéis posteriores trabalham substituindo o padrão **IMessageBus** com barramento projetado para que backplane.

Cada instância de servidor se conecta ao backplane por meio do barramento. Quando uma mensagem é enviada, ele passa para o backplane e backplane envia para todos os servidores. Quando um servidor recebe uma mensagem do backplane, ele armazena a mensagem em seu cache local. O servidor, em seguida, entrega mensagens para os clientes de seu cache local.

Para obter mais informações sobre como funciona o backplane SignalR, leia este [artigo](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Existem alguns cenários onde um backplane pode se tornar um afunilamento. Aqui estão alguns cenários típicos de SignalR:
> 
> - [Difusão de servidor](tutorial-server-broadcast-with-signalr.md) (por exemplo, cotações da bolsa): painéis posteriores funcionam bem para este cenário, porque o servidor controla a taxa na qual as mensagens são enviadas.
> - [Cliente-para-cliente](tutorial-getting-started-with-signalr.md) (por exemplo, bate-papo): nesse cenário, backplane pode ser um gargalo se o número de mensagens pode ser dimensionado com o número de clientes, ou seja, se aumentar a taxa de mensagens de clientes mais proporcionalmente como ingressar.
> - [Em tempo real de alta frequência](tutorial-high-frequency-realtime-with-signalr.md) (por exemplo, jogos em tempo real): um backplane não é recomendado para este cenário.


Neste exercício, você usará **do SQL Server** para distribuir mensagens entre o **nerd teste** aplicativo. Você executará essas tarefas em um computador de teste para saber como configurar a configuração, mas para obter o efeito completo, você precisará implantar o aplicativo de SignalR em dois ou mais servidores. Você também deve instalar o SQL Server em um dos servidores, ou em um servidor dedicado separado.

![Expansão usando o SQL Server diagrama](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Tarefa 1: Noções básicas sobre o cenário

Nesta tarefa, você executará 2 instâncias de **nerd teste** simular IIS várias instâncias em sua máquina local. Nesse cenário, ao responder perguntas trívia em um aplicativo, a atualização não será notificada na página de estatísticas da segunda instância. Esta simulação é semelhante a um ambiente em que o aplicativo for implantado em várias instâncias e usando um balanceador de carga para comunicar-se com eles.

1. Abra o **Begin.sln** solução localizada no **fonte/o Ex2-ScalingOutWithSQLServer/início** pasta. Uma vez carregado, você observará no **Server Explorer** a solução tem dois projetos com idênticos estruturas nomes diferentes mas. Isso simulará que executa duas instâncias do mesmo aplicativo em seu computador local.

    ![Começar a solução simulando 2 instâncias de teste nerd](real-time-web-applications-with-signalr/_static/image16.png "começar com uma simulação de 2 instâncias de teste nerd de solução")

    *Começar com uma simulação de 2 instâncias de teste nerd de solução*
2. Abra a página de propriedades da solução clicando duas vezes no nó da solução e selecionando **propriedades**. Em **projeto de inicialização**, selecione **vários projetos de inicialização** e altere o **ação** valor para ambos os projetos para *iniciar*.

    ![Iniciar vários projetos](real-time-web-applications-with-signalr/_static/image17.png "iniciar vários projetos")

    *Iniciar vários projetos*
3. Pressione **F5** para executar a solução. O aplicativo será iniciado de duas instâncias de **nerd teste** em portas diferentes, com uma simulação de várias instâncias do mesmo aplicativo. Fixar um dos navegadores à esquerda e o outro à direita da tela. Faça logon com suas credenciais ou registrar um novo usuário. Depois de conectado, manter a página Trívia à esquerda e vá para o **estatísticas** página no navegador à direita.

    ![Teste nerd lado a lado](real-time-web-applications-with-signalr/_static/image18.png)

    *Teste nerd lado a lado*

    ![Teste de especialista em portas diferentes](real-time-web-applications-with-signalr/_static/image19.png)

    *Teste de especialista em portas diferentes*
4. Iniciar respondendo perguntas no navegador à esquerda e você observará que o **estatísticas** page no navegador da direita não está sendo atualizado. Isso ocorre porque **SignalR** usa um local de cache para distribuir mensagens através de seus clientes e esse cenário está simulando várias instâncias, portanto, o cache não é compartilhado entre elas. Você pode verificar se **SignalR** está funcionando, teste as mesmas etapas, mas usando um único aplicativo. As tarefas a seguir, você configurará um backplane para replicar as mensagens entre instâncias.
5. Volte para o Visual Studio e parar a depuração.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Tarefa 2 – criar o Backplane do SQL Server

Nesta tarefa, você criará um banco de dados que servirá como um backplane para o **nerd teste** aplicativo. Você usará **Pesquisador de objetos do SQL Server** procurar seu servidor e inicializar o banco de dados. Além disso, você habilitará o **Service Broker**.

1. Em **Visual Studio**, abrir menu **exibição** e selecione **Pesquisador de objetos do SQL Server**.
2. Conecte-se à instância do LocalDB clicando com o **do SQL Server** nó e selecionando **adicionar SQL Server...**  opção.

    ![Adicionar uma instância do SQL Server](real-time-web-applications-with-signalr/_static/image20.png "adicionar uma instância do SQL Server")

    *A adição de uma instância do SQL Server para o Pesquisador de objetos do SQL Server*
3. Definir o **nome do servidor** para *(localdb) \v11.0* e deixe **autenticação do Windows** como o modo de autenticação. Clique em **conectar** para continuar.

    ![Conectar-se ao LocalDB](real-time-web-applications-with-signalr/_static/image21.png "conectar-se ao LocalDB")

    *Conectar-se ao LocalDB*
4. Agora que você está conectado à instância do LocalDB, você precisará criar um banco de dados que representa o backplane do SQL Server para o SignalR. Para fazer isso, clique com botão direito do **bancos de dados** nó e selecione **adicionar novo banco de dados**.

    ![Adicionando um novo banco de dados](real-time-web-applications-with-signalr/_static/image22.png "adicionando um novo banco de dados")

    *Adicionando um novo banco de dados*
5. Defina o nome do banco de dados como *SignalR* e clique em **Okey** para criá-lo.

    ![Criando o banco de dados do SignalR](real-time-web-applications-with-signalr/_static/image23.png "criando o banco de dados do SignalR")

    *Criando o banco de dados do SignalR*

    > [!NOTE]
    > Você pode escolher qualquer nome para o banco de dados.
6. Para receber atualizações com mais eficiência do backplane, é recomendável habilitar o Service Broker para o banco de dados. O Service Broker fornece suporte nativo para mensagens e enfileiramento de mensagens no SQL Server. O backplane também funciona sem o Service Broker. Abra uma nova consulta clicando com o banco de dados e selecione **nova consulta**.

    ![Abrir uma nova consulta](real-time-web-applications-with-signalr/_static/image24.png "abrir uma nova consulta")

    *Abrir uma nova consulta*
7. Para verificar se o Service Broker está habilitado, consulte o **é\_broker\_habilitado** coluna o **sys. Databases** exibição do catálogo. Execute o script a seguir na janela de consulta abertos recentemente.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Consultar o Status do agente de serviço](real-time-web-applications-with-signalr/_static/image25.png "consultar o Status do agente de serviço")

    *Consultar o Status do agente de serviço*
8. Se o valor da **é\_broker\_habilitado** coluna no banco de dados é &quot;0&quot;, use o seguinte comando para habilitá-lo. Substituir  **&lt;seu banco de dados&gt;**  com o nome definido ao criar o banco de dados (por exemplo: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Habilitando o Service Broker](real-time-web-applications-with-signalr/_static/image26.png "habilitando o Service Broker")

    *Habilitando o Service Broker*

    > [!NOTE]
    > Se esta consulta é exibido um deadlock, verifique se não há nenhum aplicativo conectado ao banco de dados.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Tarefa 3 – Configurando o aplicativo de SignalR

Nesta tarefa, você irá configurar **nerd teste** para se conectar ao backplane do SQL Server. Primeiro, você irá adicionar o **SignalR.SqlServer** pacote NuGet e defina a conexão da cadeia de caracteres para o banco de dados do backplane.

1. Abra o **Package Manager Console** de **ferramentas** | **Gerenciador de pacotes de biblioteca**. Verifique se **GeekQuiz** projeto for selecionado no **projeto padrão** lista suspensa. Digite o seguinte comando para instalar o **Microsoft.AspNet.SignalR.SqlServer** pacote NuGet.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Repita a etapa anterior, mas desta vez para o projeto **GeekQuiz2**.
3. Para configurar o backplane do SQL Server, abra o **Startup.cs** arquivo o **GeekQuiz** do projeto e adicione o seguinte código para o **configurar** método. Substituir  **&lt;seu banco de dados&gt;**  com seu nome de banco de dados usado durante a criação de backplane do SQL Server. Repita essa etapa para o **GeekQuiz2** projeto.

    (Código de trecho - *StartupConfiguration RealTimeSignalR - o Ex2 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Agora que os dois projetos estão configurados para usar o backplane do SQL Server, pressione **F5** para executá-los simultaneamente.
5. Novamente, **Visual Studio** iniciará duas instâncias do **nerd teste** em portas diferentes. Fixar um dos navegadores à esquerda e o outro à direita da tela e faça logon com suas credenciais. Manter a página Trívia à esquerda e vá para **estatísticas** o navegador à direita da página.
6. Inicie a responder perguntas no navegador à esquerda. Neste momento, o **estatísticas** página é atualizada graças ao backplane. Alternar entre aplicativos (**estatísticas** agora está à esquerda, e **Trívia** está à direita) e repita o teste para validar que ele está funcionando para ambas as instâncias. O backplane serve como um *compartilhados de* de mensagens para cada servidor conectado e cada servidor armazena as mensagens em seu próprio cache local para distribuir aos clientes conectados.
7. Volte para o Visual Studio e parar a depuração.
8. O componente de backplane do SQL Server gera automaticamente as tabelas necessárias no banco de dados especificado. No **Pesquisador de objetos do SQL Server** painel, abra o banco de dados que você criou para o backplane (por exemplo: SignalR) e expanda suas tabelas. Você deve ver as tabelas a seguir:

    ![Backplane gerado tabelas](real-time-web-applications-with-signalr/_static/image27.png)

    *Backplane gerado tabelas*
9. Clique com botão direito do **SignalR.Messages\_0** de tabela e selecione **exibir dados**.

    ![Exibir tabela de mensagens do SignalR Backplane](real-time-web-applications-with-signalr/_static/image28.png)

    *Exibir tabela de mensagens do SignalR Backplane*
10. Você pode ver diferentes mensagens enviadas para o **Hub** ao responder as perguntas trívia. O backplane distribui essas mensagens para qualquer instância conectada.

    ![Tabela de mensagens do backplane](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabela de mensagens do backplane*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste laboratório prático, você aprendeu como adicionar **SignalR** seu aplicativo e enviar as notificações do servidor para os clientes conectados usando **Hubs**. Além disso, você aprendeu como dimensionar seu aplicativo usando um *backplane* componente quando o aplicativo for implantado em várias instâncias do IIS.
