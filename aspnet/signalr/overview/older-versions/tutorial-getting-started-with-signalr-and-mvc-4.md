---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Introdução ao SignalR 1. x e MVC 4 | Microsoft Docs'
author: pfletcher
description: Use o SignalR do ASP.NET e ASP.NET MVC 4 para criar um aplicativo de bate-papo em tempo real.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Tutorial: Introdução ao SignalR 1. x e MVC 4
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Este tutorial mostra como usar o SignalR do ASP.NET para criar um aplicativo de bate-papo em tempo real. Você adicionará o SignalR para um aplicativo MVC 4 e criar uma exibição de bate-papo para enviar e exibir as mensagens.


## <a name="overview"></a>Visão geral

Este tutorial o apresenta ao desenvolvimento de aplicativos web em tempo real com ASP.NET SignalR e ASP.NET MVC 4. O tutorial usa o mesmo código de aplicativo de bate-papo, como o [tutorial de Introdução SignalR](tutorial-getting-started-with-signalr.md), mas mostra como adicioná-lo a um aplicativo MVC 4 com base no modelo de Internet.

Neste tópico, você aprenderá as seguintes tarefas de desenvolvimento SignalR:

- Adicionar a biblioteca de SignalR para um aplicativo MVC 4.
- Criando uma classe de hub para enviar por push o conteúdo aos clientes.
- Usando a biblioteca de jQuery SignalR em uma página da web para enviar mensagens e exibir as atualizações do hub.

A captura de tela a seguir mostra o aplicativo de chat concluído em execução em um navegador.

![Instâncias de bate-papo](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Seções:

- [Configurar o projeto](#setup)
- [Executar o exemplo](#run)
- [Examine o código](#code)
- [Próximas Etapas](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar o projeto

Pré-requisitos:

- Visual Studio 2010 SP1, o Visual Studio 2012 ou Visual Studio 2012 Express. Se você não tiver o Visual Studio, consulte [ASP.NET Downloads](https://www.asp.net/downloads) para obter o Visual Studio 2012 Express desenvolvimento ferramenta gratuita.
- Para o Visual Studio 2010, instale [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

Esta seção mostra como criar um aplicativo ASP.NET MVC 4, adicione a biblioteca de SignalR e criar o aplicativo de bate-papo.

1. 1. No Visual Studio criar um aplicativo ASP.NET MVC 4, nomeie-o SignalRChat e clique em Okey.

        > [!NOTE]
        > No VS 2010, selecione **.NET Framework 4** no controle de lista suspensa de versão do Framework. Código de SignalR é executado em versões do .NET Framework 4 e 4.5.

        ![Criar web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Selecione o modelo de aplicativo de Internet, desmarque a opção de **criar um projeto de teste de unidade**e clique em Okey.

         ![Criar um site da internet mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Abra o **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console** e execute o comando a seguir. Esta etapa adiciona ao projeto um conjunto de arquivos de script e referências de assembly que habilitar a funcionalidade do SignalR.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. Em **Solution Explorer** expanda a pasta de Scripts. Observe que as bibliotecas de scripts para o SignalR foram adicionadas ao projeto.

         ![Referências de biblioteca](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. Em **Solution Explorer**, clique com o botão direito, selecione **adicionar | Nova pasta**, e adicione uma nova pasta chamada **Hubs**.
      6. Clique com botão direito do **Hubs** pasta, clique em **adicionar | Classe**e crie uma nova classe c# chamada **ChatHub.cs**. Você usará essa classe como um hub de servidor do SignalR que envia mensagens para todos os clientes.

> [!NOTE]
> Se você usar o Visual Studio 2012 e tiver instalado o [atualização ASP.NET e Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), você pode usar o novo modelo de item de SignalR para criar a classe de hub. Para fazer isso, clique com botão direito do **Hubs** pasta, clique em **adicionar | Novo Item**, selecione **classe de Hub SignalR (v1)** e nomeie a classe **ChatHub.cs**.


1. Substitua o código no **ChatHub** classe com o código a seguir.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Abra o **global. asax** de arquivos para o projeto e adicionar uma chamada ao método `RouteTable.Routes.MapHubs();` como a primeira linha do código no `Application_Start` método. Esse código registra a rota padrão para hubs de SignalR e deve ser chamado antes de registrar todas as outras rotas. Concluído `Application_Start` método é semelhante ao exemplo a seguir.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Editar o `HomeController` classe encontrada no **Controllers/HomeController.cs** e adicione o seguinte método à classe. Esse método retorna o **bate-papo** modo de exibição que você criará em uma etapa posterior.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Clique dentro do `Chat` método recém-criado e clique em **adicionar exibição** para criar um novo arquivo de exibição.
5. No **adicionar exibição** caixa de diálogo, verifique se a caixa de seleção é selecionada para **usar uma layout ou página mestra** (desmarque as outras caixas de seleção) e, em seguida, clique em **adicionar**.

    ![Adicionar uma exibição](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Edite o novo arquivo de modo de exibição denominado **Chat.cshtml**. Após o &lt;h2&gt; marca, cole o seguinte &lt;div&gt; seção e `@section scripts` bloco de código para a página. Esse script permite que a página enviar mensagens de chat e exibir as mensagens do servidor. O código completo para o modo de exibição de bate-papo aparece no bloco de código a seguir.

    > [!IMPORTANT]
    > Quando você adiciona o SignalR e outras bibliotecas de script ao seu projeto do Visual Studio, o Gerenciador de pacotes pode instalar as versões dos scripts que são mais recentes do que as versões mostradas neste tópico. Certifique-se de que as referências de script em seu código correspondem às versões das bibliotecas de script instaladas em seu projeto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Salvar todos os** para o projeto.

<a id="run"></a>

## <a name="run-the-sample"></a>Executar o exemplo

1. Pressione F5 para executar o projeto no modo de depuração.
2. Na linha de endereço do navegador, acrescente **inicial/bate-papo** para a URL da página padrão para o projeto. A página de bate-papo carrega em uma instância do navegador e solicitará um nome de usuário.

    ![Insira o nome de usuário](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Insira um nome de usuário.
4. Copie a URL da linha de endereço do navegador e usá-lo para abrir duas ou mais instâncias de navegador. Em cada instância do navegador, digite um nome de usuário exclusivo.
5. Em cada instância do navegador, adicione um comentário e clique em **enviar**. Os comentários devem exibir em todas as instâncias do navegador.

    > [!NOTE]
    > Este aplicativo de chat simples não mantém o contexto de discussão no servidor. O hub transmite comentários para todos os usuários atuais. Os usuários que entram no bate-papo posteriormente verá mensagens adicionadas desde o momento que entrarem.
6. A captura de tela a seguir mostra o aplicativo de chat em execução em um navegador.

    ![Navegadores de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. Em **Solution Explorer**, inspecione o **documentos de Script** nó para o aplicativo em execução. Esse nó é visível no modo de depuração, se você estiver usando o Internet Explorer como navegador. Há um arquivo de script denominado **hubs** que a biblioteca de SignalR gera dinamicamente em tempo de execução. Este arquivo gerencia a comunicação entre jQuery script e código do lado do servidor. Se você usar um navegador que não seja o Internet Explorer, você também pode acessar dinâmico **hubs** arquivo navegando até ele diretamente, por exemplo http://mywebsite/signalr/hubs.

    ![Script de hub gerado](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine o código

O aplicativo de chat SignalR demonstra duas tarefas básicas de desenvolvimento SignalR: Criando um hub de como o objeto de coordenação principal no servidor e, usando a biblioteca de jQuery SignalR para enviar e receber mensagens.

### <a name="signalr-hubs"></a>Hubs de SignalR

No exemplo de código a **ChatHub** classe deriva o **Microsoft.AspNet.SignalR.Hub** classe. Derivando de **Hub** classe é uma maneira útil para criar um aplicativo do SignalR. Você pode criar métodos públicos em sua classe de hub e, em seguida, esses métodos de acesso chamando-los a partir de scripts jQuery em uma página da web.

No código de bate-papo, clientes chamar o **ChatHub.Send** para enviar uma nova mensagem. O hub por sua vez envia a mensagem a todos os clientes chamando **Clients.All.addNewMessageToPage**.

O **enviar** método demonstra vários conceitos de hub:

- Declare métodos públicos em um hub para que os clientes podem chamá-los.
- Use o **Microsoft.AspNet.SignalR.Hub.Clients** propriedade para acessar todos os clientes conectados a esse hub.
- Chamar uma função de jQuery no cliente (como o `addNewMessageToPage` função) para atualizar clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

O **Chat.cshtml** exibir o arquivo no exemplo de código mostra como usar a biblioteca de jQuery SignalR para se comunicar com um hub SignalR. As tarefas essenciais no código estão criando uma referência para o proxy geradas automaticamente para o hub, declarar uma função que o servidor pode chamar para conteúdo por push aos clientes e iniciar uma conexão para enviar mensagens para o hub.

O código a seguir declara um proxy para um hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> Em jQuery a referência para a classe de servidor e seus membros está em minúsculas concatenadas. O exemplo de código referencia o c# **ChatHub** classe jQuery como **chatHub**. Se você quiser fazer referência a `ChatHub` classe jQuery com Pascal convencional maiusculas e minúsculas como você faria no c#, editar o arquivo de classe ChatHub.cs. Adicionar um `using` instrução para fazer referência a `Microsoft.AspNet.SignalR.Hubs` namespace. Em seguida, adicione o `HubName` de atributo para o `ChatHub` classe, por exemplo `[HubName("ChatHub")]`. Por fim, atualize sua referência jQuery para o `ChatHub` classe.


O código a seguir mostra como criar uma função de retorno de chamada no script. A classe de hub no servidor chama esta função para enviar atualizações de conteúdo para cada cliente. A chamada opcional para o `htmlEncode` função mostra uma maneira para HTML codifica o conteúdo da mensagem antes de exibi-lo na página, como uma maneira de impedir injeção de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

O código a seguir mostra como abrir uma conexão com o hub. O código começa a conexão e, em seguida, passa uma função para manipular o evento de clique no **enviar** botão na página de bate-papo.

> [!NOTE]
> Essa abordagem garante que a conexão seja estabelecida antes de executa o manipulador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Próximas etapas

Você aprendeu SignalR é uma estrutura para criar aplicativos web em tempo real. Você também aprendeu diversas tarefas de desenvolvimento SignalR: como adicionar SignalR para um aplicativo ASP.NET, como criar uma classe de hub e como enviar e receber mensagens do hub.

Para saber mais avançados conceitos de desenvolvimentos SignalR, visite os seguintes sites para SignalR código-fonte e recursos:

- [Projeto de SignalR](http://signalr.net)
- [SignalR Github e exemplos](https://github.com/SignalR/SignalR)
- [Wiki do SignalR](https://github.com/SignalR/SignalR/wiki)
