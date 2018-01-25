---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "Tutorial: Introdução ao SignalR 1. x | Microsoft Docs"
author: pfletcher
description: "Use o SignalR do ASP.NET para criar um aplicativo de bate-papo em tempo real em uma página HTML."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Tutorial: Introdução ao SignalR 1. x
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real. Você adicionará o SignalR para um aplicativo de web ASP.NET vazio e criar uma página HTML para enviar e exibir as mensagens.


## <a name="overview"></a>Visão geral

Este tutorial apresenta o desenvolvimento de SignalR, mostrando como criar um aplicativo simples baseado em navegador bate-papo. Você adicionar a biblioteca de SignalR para um aplicativo de web ASP.NET vazio, crie uma classe de hub para enviar mensagens para os clientes e criar uma página HTML que permite aos usuários enviar e receber mensagens de chat. Para obter um tutorial semelhante que mostra como criar um aplicativo de chat no MVC 4 usando uma exibição do MVC, consulte [Introdução ao SignalR e MVC 4](index.md).

> [!NOTE]
> Este tutorial usa a versão (1. x) do SignalR. Para obter detalhes sobre as alterações entre SignalR 1. x e 2.0, consulte [SignalR atualização 1. x projetos](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR é uma biblioteca .NET de código-fonte aberto para a criação de aplicativos web que requerem interação do usuário em tempo real ou atualizações de dados em tempo real. Exemplos incluem aplicativos sociais, jogos multiusuários, clima de colaboração e notícias, business ou financeiro atualizar aplicativos. Esses são chamados de aplicativos em tempo real.

SignalR simplifica o processo de criação de aplicativos em tempo real. Ele inclui uma biblioteca de servidor ASP.NET e uma biblioteca de cliente JavaScript para tornar mais fácil gerenciar conexões de cliente-servidor e enviar as atualizações de conteúdo para clientes. Você pode adicionar a biblioteca de SignalR para um aplicativo ASP.NET existente para obter a funcionalidade em tempo real.

O tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:

- Adicionar a biblioteca de SignalR para um aplicativo web ASP.NET.
- Criando uma classe de hub para enviar por push o conteúdo aos clientes.
- Usando a biblioteca de jQuery SignalR em uma página da web para enviar mensagens e exibir as atualizações do hub.

A captura de tela a seguir mostra o aplicativo de chat em execução em um navegador. Cada novo usuário pode enviar comentários e ver comentários adicionados depois que o usuário se associe o bate-papo.

![Instâncias de bate-papo](tutorial-getting-started-with-signalr/_static/image1.png)

Seções:

- [Configurar o projeto](#setup)
- [Executar o exemplo](#run)
- [Examine o código](#code)
- [Próximas Etapas](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar o projeto

Esta seção mostra como criar um aplicativo web ASP.NET vazio, adicione o SignalR e criar o aplicativo de bate-papo.

Pré-requisitos:

- Visual Studio 2010 SP1 ou 2012. Se você não tiver o Visual Studio, consulte [ASP.NET Downloads](https://www.asp.net/downloads) para obter o Visual Studio 2012 Express desenvolvimento ferramenta gratuita.
- [Microsoft ASP.NET e Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Para o Visual Studio 2012, esse instalador adiciona novos recursos do ASP.NET, incluindo modelos de SignalR ao Visual Studio. Para o Visual Studio 2010 SP1, um instalador não está disponível, mas você pode concluir o tutorial instalando o pacote SignalR NuGet conforme descrito nas etapas de instalação.

As etapas a seguir usam o Visual Studio 2012 para criar um aplicativo Web vazio do ASP.NET e adicionar a biblioteca de SignalR:

1. No Visual Studio, crie um aplicativo Web vazio do ASP.NET.

    ![Criar web vazio](tutorial-getting-started-with-signalr/_static/image2.png)
2. Abra o **Package Manager Console** selecionando **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console**. Digite o seguinte comando na janela de console:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Esse comando instala a versão mais recente do SignalR 1. x.
3. Em **Solution Explorer**, clique com o botão direito, selecione **adicionar | Classe**. Nomeie a nova classe **ChatHub**.
4. Em **Solution Explorer** expanda o nó de Scripts. Bibliotecas de scripts para jQuery e SignalR são visíveis no projeto.

    ![Referências de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. Substitua o código no **ChatHub** classe com o código a seguir.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Novo Item**. No **Adicionar Novo Item** caixa de diálogo, selecione **classe de aplicativo Global** e clique em **adicionar**.

    ![Adicionar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. Adicione o seguinte `using` instruções após fornecido `using` instruções na classe asax.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Adicione a seguinte linha de código no `Application_Start` método da classe Global para registrar a rota padrão para hubs de SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Novo Item**. No **Adicionar Novo Item** caixa de diálogo, selecione Página Html e clique em **adicionar**.
10. Em **Solution Explorer**, clique com botão direito a página HTML que você acabou de criar e clique em **definir como página inicial**.
11. Substitua o código padrão na página HTML com o código a seguir.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Salvar todos os** para o projeto.

<a id="run"></a>

## <a name="run-the-sample"></a>Executar o exemplo

1. Pressione F5 para executar o projeto no modo de depuração. Carrega a página HTML em uma instância do navegador e solicitará um nome de usuário.

    ![Insira o nome de usuário](tutorial-getting-started-with-signalr/_static/image5.png)
2. Insira um nome de usuário.
3. Copie a URL da linha de endereço do navegador e usá-lo para abrir duas ou mais instâncias de navegador. Em cada instância do navegador, digite um nome de usuário exclusivo.
4. Em cada instância do navegador, adicione um comentário e clique em **enviar**. Os comentários devem exibir em todas as instâncias do navegador.

    > [!NOTE]
    > Este aplicativo de chat simples não mantém o contexto de discussão no servidor. O hub transmite comentários para todos os usuários atuais. Os usuários que entram no bate-papo posteriormente verá mensagens adicionadas desde o momento que entrarem.

    A captura de tela a seguir mostra o aplicativo de chat em execução em três instâncias do navegador, que são atualizados quando uma instância envia uma mensagem:

    ![Navegadores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. Em **Solution Explorer**, inspecione o **documentos de Script** nó para o aplicativo em execução. Há um arquivo de script denominado **hubs** que a biblioteca de SignalR gera dinamicamente em tempo de execução. Este arquivo gerencia a comunicação entre jQuery script e código do lado do servidor.

    ![Script de hub gerado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine o código

O aplicativo de chat SignalR demonstra duas tarefas básicas de desenvolvimento SignalR: Criando um hub de como o objeto de coordenação principal no servidor e, usando a biblioteca de jQuery SignalR para enviar e receber mensagens.

### <a name="signalr-hubs"></a>Hubs de SignalR

No exemplo de código a **ChatHub** classe deriva o **Microsoft.AspNet.SignalR.Hub** classe. Derivando de **Hub** classe é uma maneira útil para criar um aplicativo do SignalR. Você pode criar métodos públicos em sua classe de hub e, em seguida, esses métodos de acesso chamando-los a partir de scripts jQuery em uma página da web.

No código de bate-papo, clientes chamar o **ChatHub.Send** para enviar uma nova mensagem. O hub por sua vez envia a mensagem a todos os clientes chamando **Clients.All.broadcastMessage**.

O **enviar** método demonstra vários conceitos de hub:

- Declare métodos públicos em um hub para que os clientes podem chamá-los.
- Use o **Microsoft.AspNet.SignalR.Hub.Clients** propriedade dinâmica para acessar todos os clientes conectados a esse hub.
- Chamar uma função de jQuery no cliente (como o `broadcastMessage` função) para atualizar clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

A página HTML no exemplo de código mostra como usar a biblioteca de jQuery SignalR para se comunicar com um hub SignalR. As tarefas essenciais no código estão declarando um proxy para referenciar o hub, declarar uma função que o servidor pode chamar para conteúdo por push aos clientes e iniciar uma conexão para enviar mensagens para o hub.

O código a seguir declara um proxy para um hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> Em jQuery a referência para a classe de servidor e seus membros está em minúsculas concatenadas. O exemplo de código referencia o c# **ChatHub** classe jQuery como **chatHub**.


O código a seguir é como criar uma função de retorno de chamada no script. A classe de hub no servidor chama esta função para enviar atualizações de conteúdo para cada cliente. As duas linhas que o HTML codificar o conteúdo antes de exibi-los são opcionais e mostram uma maneira simples para prevenir injeção de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

O código a seguir mostra como abrir uma conexão com o hub. O código começa a conexão e, em seguida, passa uma função para manipular o evento de clique no **enviar** botão na página HTML.

> [!NOTE]
> Essa abordagem garante que a conexão é estabelecida antes de executa o manipulador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Próximas etapas

Você aprendeu SignalR é uma estrutura para criar aplicativos web em tempo real. Você também aprendeu diversas tarefas de desenvolvimento SignalR: como adicionar SignalR para um aplicativo ASP.NET, como criar uma classe de hub e como enviar e receber mensagens do hub.

Você pode tornar o aplicativo de exemplo neste tutorial ou outros aplicativos SignalR disponíveis pela Internet por implantá-los para um provedor de hospedagem. A Microsoft oferece gratuito da web de hospedagem para até 10 sites da web em um livre [conta de avaliação do Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para obter instruções sobre como implantar o aplicativo de exemplo SignalR, consulte [publicar o SignalR obtendo iniciado exemplo como um Site do Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Para obter informações detalhadas sobre como implantar um projeto da web do Visual Studio para um Site do Windows Azure, consulte [Implantando um aplicativo ASP.NET a um Site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Observação: O transporte de WebSocket não tem suporte para o Windows Azure Web Sites. Transporte de WebSocket quando não estiver disponível, o SignalR usa os outros transportes disponíveis conforme descrito na seção de transportes de [Introdução ao tópico SignalR](index.md).)

Para saber mais avançados conceitos de desenvolvimentos SignalR, visite os seguintes sites para SignalR código-fonte e recursos:

- [Projeto de SignalR](http://signalr.net)
- [SignalR Github e exemplos](https://github.com/SignalR/SignalR)
- [Wiki do SignalR](https://github.com/SignalR/SignalR/wiki)
