---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: "Tutorial: Introdução ao SignalR 2 | Microsoft Docs"
author: pfletcher
description: "Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real. Você irá adicionar SignalR para um aplicativo de web ASP.NET vazio e criar um pa HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a>Tutorial: Introdução ao SignalR 2
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real. Você adicionará o SignalR para um aplicativo de web ASP.NET vazio e criar uma página HTML para enviar e exibir as mensagens. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versão 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Usando o Visual Studio 2012 com este tutorial
> 
> 
> Para usar o Visual Studio 2012 com este tutorial, faça o seguinte:
> 
> - Atualização de seu [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) para a versão mais recente.
> - Instalar o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - No Web Platform Installer, procurar e instalar **ASP.NET e Web 2013.1 de ferramentas para Visual Studio 2012**. Isso irá instalar os modelos do Visual Studio para SignalR classes como **Hub**.
> - Alguns modelos (como **classe de inicialização OWIN**) não estará disponível; para isso, use um arquivo de classe em vez disso.
> 
> 
> ## <a name="tutorial-versions"></a>Versões do tutoriais
> 
> Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Visão geral

Este tutorial apresenta o desenvolvimento de SignalR, mostrando como criar um aplicativo simples baseado em navegador bate-papo. Você adicionar a biblioteca de SignalR para um aplicativo de web ASP.NET vazio, crie uma classe de hub para enviar mensagens para os clientes e criar uma página HTML que permite aos usuários enviar e receber mensagens de chat. Para obter um tutorial semelhante que mostra como criar um aplicativo de chat no MVC 5 usando um modo de exibição do MVC, consulte [Introdução ao SignalR 2 e 5 MVC](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Este tutorial demonstra como criar aplicativos SignalR na versão 2. Para obter detalhes sobre as alterações entre SignalR 1. x e 2, consulte [SignalR atualização 1. x projetos](../releases/upgrading-signalr-1x-projects-to-20.md) e [notas de versão do Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR é uma biblioteca .NET de código-fonte aberto para a criação de aplicativos web que requerem interação do usuário em tempo real ou atualizações de dados em tempo real. Exemplos incluem aplicativos sociais, jogos multiusuários, clima de colaboração e notícias, business ou financeiro atualizar aplicativos. Esses são chamados de aplicativos em tempo real.

SignalR simplifica o processo de criação de aplicativos em tempo real. Ele inclui uma biblioteca de servidor ASP.NET e uma biblioteca de cliente JavaScript para tornar mais fácil gerenciar conexões de cliente-servidor e enviar as atualizações de conteúdo para clientes. Você pode adicionar a biblioteca de SignalR para um aplicativo ASP.NET existente para obter a funcionalidade em tempo real.

O tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:

- Adicionar a biblioteca de SignalR para um aplicativo web ASP.NET.
- Criando uma classe de hub para enviar por push o conteúdo aos clientes.
- Criando uma classe de inicialização OWIN para configurar o aplicativo.
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

Esta seção mostra como usar o Visual Studio 2013 e SignalR versão 2 para criar um aplicativo web ASP.NET vazio, adicione o SignalR e criar o aplicativo de bate-papo.

Pré-requisitos:

- Visual Studio 2013. Se você não tiver o Visual Studio, consulte [ASP.NET Downloads](https://www.asp.net/downloads) para obter o Visual Studio 2013 Express desenvolvimento ferramenta gratuita.

As etapas a seguir usam o Visual Studio 2013 para criar um aplicativo Web vazio do ASP.NET e adicionar a biblioteca de SignalR:

1. No Visual Studio, crie um aplicativo Web ASP.NET.

    ![Criar web](tutorial-getting-started-with-signalr/_static/image2.png)
2. No **novo projeto ASP.NET** janela, deixe **vazio** selecionado e clique em **criar projeto**.

    ![Criar web vazio](tutorial-getting-started-with-signalr/_static/image3.png)
3. Em **Solution Explorer**, clique com o botão direito, selecione **adicionar | Classe de Hub SignalR (v2)**. Nomeie a classe **ChatHub.cs** e adicioná-lo ao projeto. Esta etapa cria a **ChatHub** classe e o adiciona ao projeto um conjunto de arquivos de script e referências de assembly que dão suporte ao SignalR.

    > [!NOTE]
    > Você também pode adicionar o SignalR para um projeto abrindo o **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console** e executar um comando:

    `install-package Microsoft.AspNet.SignalR`

    Se você usar o console para adicionar o SignalR, crie a classe de hub SignalR como uma etapa separada depois de adicionar o SignalR.

    > [!NOTE]
    > Se você estiver usando o Visual Studio 2012, o **classe de Hub SignalR (v2)** modelo não estará disponível. Você pode adicionar um simples **classe** chamado `ChatHub` em vez disso.
4. Em **Solution Explorer**, expanda o nó de Scripts. Bibliotecas de scripts para jQuery e SignalR são visíveis no projeto.
5. Substitua o código no novo **ChatHub** classe com o código a seguir.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Classe de inicialização OWIN**. Nomeie a nova classe `Startup` e clique em Okey.

    > [!NOTE]
    > Se você estiver usando o Visual Studio 2012, o **classe de inicialização OWIN** modelo não estará disponível. Você pode adicionar um simples **classe** chamado `Startup` em vez disso.
7. Altere o conteúdo da nova classe de inicialização para o seguinte.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Página HTML**. Nomeie a nova página `index.html`.
    >[!NOTE]
    >Talvez seja necessário alterar os números de versão para as referências a bibliotecas JQuery e SignalR
9. Em **Solution Explorer**, clique com botão direito a página HTML que você acabou de criar e clique em **definir como página inicial**.
10. Substitua o código padrão na página HTML com o código a seguir.

    > [!NOTE]
    > Uma versão posterior do SignalR scripts pode ser instalada pelo Gerenciador de pacote. Verifique se as referências de script a seguir correspondem às versões dos arquivos de script no projeto (que será diferente se você adicionou o SignalR usando o NuGet em vez de adicionar um hub.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Salvar todos os** para o projeto.

<a id="run"></a>

## <a name="run-the-sample"></a>Executar o exemplo

1. Pressione F5 para executar o projeto no modo de depuração. Carrega a página HTML em uma instância do navegador e solicitará um nome de usuário.

    ![Insira o nome de usuário](tutorial-getting-started-with-signalr/_static/image4.png)
2. Insira um nome de usuário.
3. Copie a URL da linha de endereço do navegador e usá-lo para abrir duas ou mais instâncias de navegador. Em cada instância do navegador, digite um nome de usuário exclusivo.
4. Em cada instância do navegador, adicione um comentário e clique em **enviar**. Os comentários devem exibir em todas as instâncias do navegador.

    > [!NOTE]
    > Este aplicativo de chat simples não mantém o contexto de discussão no servidor. O hub transmite comentários para todos os usuários atuais. Os usuários que entram no bate-papo posteriormente verá mensagens adicionadas desde o momento que entrarem.

    A captura de tela a seguir mostra o aplicativo de chat em execução em três instâncias do navegador, que são atualizados quando uma instância envia uma mensagem:

    ![Navegadores de chat](tutorial-getting-started-with-signalr/_static/image5.png)
5. Em **Solution Explorer**, inspecione o **documentos de Script** nó para o aplicativo em execução. Há um arquivo de script denominado **hubs** que a biblioteca de SignalR gera dinamicamente em tempo de execução. Este arquivo gerencia a comunicação entre jQuery script e código do lado do servidor.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine o código

O aplicativo de chat SignalR demonstra duas tarefas básicas de desenvolvimento SignalR: Criando um hub de como o objeto de coordenação principal no servidor e, usando a biblioteca de jQuery SignalR para enviar e receber mensagens.

### <a name="signalr-hubs"></a>Hubs de SignalR

No exemplo de código a **ChatHub** classe deriva o **Microsoft.AspNet.SignalR.Hub** classe. Derivando de **Hub** classe é uma maneira útil para criar um aplicativo do SignalR. Você pode criar métodos públicos em sua classe de hub e, em seguida, esses métodos de acesso chamando-los a partir de scripts em uma página da web.

No código de bate-papo, clientes chamar o **ChatHub.Send** para enviar uma nova mensagem. O hub por sua vez envia a mensagem a todos os clientes chamando **Clients.All.broadcastMessage**.

O **enviar** método demonstra vários conceitos de hub:

- Declare métodos públicos em um hub para que os clientes podem chamá-los.
- Use o **Microsoft.AspNet.SignalR.Hub.Clients** propriedade dinâmica para acessar todos os clientes conectados a esse hub.
- Chamar uma função no cliente (como o `broadcastMessage` função) para atualizar clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

A página HTML no exemplo de código mostra como usar a biblioteca de jQuery SignalR para se comunicar com um hub SignalR. As tarefas essenciais no código estão declarando um proxy para referenciar o hub, declarar uma função que o servidor pode chamar para conteúdo por push aos clientes e iniciar uma conexão para enviar mensagens para o hub.

O código a seguir declara uma referência a um proxy do hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> Em JavaScript a referência para a classe de servidor e seus membros está em minúsculas concatenadas. O exemplo de código referencia o c# **ChatHub** classe em JavaScript como **chatHub**.


O código a seguir é como criar uma função de retorno de chamada no script. A classe de hub no servidor chama esta função para enviar atualizações de conteúdo para cada cliente. As duas linhas que o HTML codificar o conteúdo antes de exibi-los são opcionais e mostram uma maneira simples para prevenir injeção de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

O código a seguir mostra como abrir uma conexão com o hub. O código começa a conexão e, em seguida, passa uma função para manipular o evento de clique no **enviar** botão na página HTML.

> [!NOTE]
> Essa abordagem garante que a conexão seja estabelecida antes de executa o manipulador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Próximas etapas

Você aprendeu SignalR é uma estrutura para criar aplicativos web em tempo real. Você também aprendeu diversas tarefas de desenvolvimento SignalR: como adicionar SignalR para um aplicativo ASP.NET, como criar uma classe de hub e como enviar e receber mensagens do hub.

Para obter instruções sobre como implantar o aplicativo de exemplo de SignalR no Azure, consulte [usando SignalR com aplicativos Web no serviço de aplicativo do Azure](../deployment/using-signalr-with-azure-web-sites.md). Para obter informações detalhadas sobre como implantar um projeto da web do Visual Studio para um Site do Windows Azure, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Para saber mais avançados conceitos de desenvolvimentos SignalR, visite os seguintes sites para SignalR código-fonte e recursos:

- [Projeto de SignalR](http://signalr.net)
- [SignalR Github e exemplos](https://github.com/SignalR/SignalR)
- [Wiki do SignalR](https://github.com/SignalR/SignalR/wiki)
