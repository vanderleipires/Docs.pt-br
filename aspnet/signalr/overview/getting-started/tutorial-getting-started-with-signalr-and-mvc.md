---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: "Tutorial: Introdução ao SignalR 2 e MVC 5 | Microsoft Docs"
author: pfletcher
description: "Este tutorial mostra como usar o ASP.NET SignalR 2 para criar um aplicativo de bate-papo em tempo real. Você adicionará o SignalR para um aplicativo MVC 5 e criar uma exibição de bate-papo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 96a6f6446b26d96b2bcffe4354375ab9c444bbbb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Tutorial: Introdução ao SignalR 2 e 5 do MVC
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Este tutorial mostra como usar o ASP.NET SignalR 2 para criar um aplicativo de bate-papo em tempo real. Você adicionará o SignalR para um aplicativo MVC 5 e criar uma exibição de bate-papo para enviar e exibir as mensagens. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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


## <a name="overview"></a>Visão Geral

Este tutorial o apresenta ao desenvolvimento de aplicativos web em tempo real com ASP.NET SignalR 2 e 5 do ASP.NET MVC. O tutorial usa o mesmo código de aplicativo de bate-papo, como o [tutorial de Introdução SignalR](tutorial-getting-started-with-signalr.md), mas mostra como adicioná-lo a um aplicativo MVC 5.

Neste tópico, você aprenderá as seguintes tarefas de desenvolvimento SignalR:

- Adicionar a biblioteca de SignalR para um aplicativo MVC 5.
- Criando hub e classes de inicialização OWIN para transferir conteúdo para clientes.
- Usando a biblioteca de jQuery SignalR em uma página da web para enviar mensagens e exibir as atualizações do hub.

A captura de tela a seguir mostra o aplicativo de chat concluído em execução em um navegador.

![Instâncias de bate-papo](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Seções:

- [Configurar o projeto](#setup)
- [Executar o exemplo](#run)
- [Examine o código](#code)
- [Próximas Etapas](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar o projeto

Pré-requisitos:

- Visual Studio 2013. Se você não tiver o Visual Studio, consulte [ASP.NET Downloads](https://www.asp.net/downloads) para obter o Visual Studio 2013 Express desenvolvimento ferramenta gratuita.

Esta seção mostra como criar um aplicativo ASP.NET MVC 5, adicione a biblioteca de SignalR e criar o aplicativo de bate-papo.

1. No Visual Studio, criar um aplicativo c# ASP.NET que tem como destino o .NET Framework 4.5, nomeie-o SignalRChat e clique em Okey.

    ![Criar web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. No `New ASP.NET Project` caixa de diálogo e selecione **MVC**e clique em **alterar autenticação**.

    ![Criar web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Selecione **sem autenticação** no **alterar autenticação** caixa de diálogo e clique em **Okey**.

    ![Selecione sem autenticação](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Se você selecionar um provedor de autenticação diferentes para o seu aplicativo, um `Startup.cs` classe será criada para você; você não precisará criar seus próprios `Startup.cs` classe na etapa 10 abaixo.
4. Clique em **Okey** no **novo projeto ASP.NET** caixa de diálogo.
5. Abra o **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console** e execute o comando a seguir. Esta etapa adiciona ao projeto um conjunto de arquivos de script e referências de assembly que habilitar a funcionalidade do SignalR.

    `install-package Microsoft.AspNet.SignalR`
6. Em **Solution Explorer**, expanda a pasta de Scripts. Observe que as bibliotecas de scripts para o SignalR foram adicionadas ao projeto.

    ![Pasta de scripts](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. Em **Solution Explorer**, clique com o botão direito, selecione **adicionar | Nova pasta**, e adicione uma nova pasta chamada **Hubs**.
8. Clique com botão direito do **Hubs** pasta, clique em **adicionar | Novo Item**, selecione o **Visual c# | Web | SignalR** nó o **instalado** painel, selecione **classe de Hub SignalR (v2)** no painel central e criar um novo hub denominado **ChatHub.cs**. Você usará essa classe como um hub de servidor do SignalR que envia mensagens para todos os clientes.

    ![Criar novo hub](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Substitua o código no **ChatHub** classe com o código a seguir.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Crie uma nova classe chamada Startup.cs. Altere o conteúdo do arquivo para o seguinte.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Editar o `HomeController` classe encontrada no **Controllers/HomeController.cs** e adicione o seguinte método à classe. Esse método retorna o **bate-papo** modo de exibição que você criará em uma etapa posterior.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Clique com botão direito do **exibições/inicial** pasta e selecione **adicionar.... | Exibição**.
13. No **adicionar exibição** caixa de diálogo, o nome do novo modo de exibição **bate-papo**.

    ![Adicionar uma exibição](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Substitua o conteúdo do **Chat.cshtml** com o código a seguir.

    > [!IMPORTANT]
    > Quando você adiciona o SignalR e outras bibliotecas de script ao seu projeto do Visual Studio, o Gerenciador de pacotes pode instalar uma versão do arquivo de script SignalR é mais recente que a versão mostrada neste tópico. Certifique-se de que a referência de script em seu código corresponde à versão da biblioteca de script instalada em seu projeto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Salvar todos os** para o projeto.

<a id="run"></a>

## <a name="run-the-sample"></a>Executar o exemplo

1. Pressione F5 para executar o projeto no modo de depuração.
2. Na linha de endereço do navegador, acrescente **inicial/bate-papo** para a URL da página padrão para o projeto. A página de bate-papo carrega em uma instância do navegador e solicitará um nome de usuário.

    ![Insira o nome de usuário](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Insira um nome de usuário.
4. Copie a URL da linha de endereço do navegador e usá-lo para abrir duas ou mais instâncias de navegador. Em cada instância do navegador, digite um nome de usuário exclusivo.
5. Em cada instância do navegador, adicione um comentário e clique em **enviar**. Os comentários devem exibir em todas as instâncias do navegador.

    > [!NOTE]
    > Este aplicativo de chat simples não mantém o contexto de discussão no servidor. O hub transmite comentários para todos os usuários atuais. Os usuários que entram no bate-papo posteriormente verá mensagens adicionadas desde o momento que entrarem.
6. A captura de tela a seguir mostra o aplicativo de chat em execução em um navegador.

    ![Navegadores de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. Em **Solution Explorer**, inspecione o **documentos de Script** nó para o aplicativo em execução. Esse nó é visível no modo de depuração, se você estiver usando o Internet Explorer como navegador. Há um arquivo de script denominado **hubs** que a biblioteca de SignalR gera dinamicamente em tempo de execução. Este arquivo gerencia a comunicação entre jQuery script e código do lado do servidor. Se você usar um navegador que não seja o Internet Explorer, você também pode acessar dinâmico **hubs** arquivo navegando até ele diretamente, por exemplo http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Examine o código

O aplicativo de chat SignalR demonstra duas tarefas básicas de desenvolvimento SignalR: Criando um hub de como o objeto de coordenação principal no servidor e, usando a biblioteca de jQuery SignalR para enviar e receber mensagens.

### <a name="signalr-hubs"></a>Hubs de SignalR

No exemplo de código a **ChatHub** classe deriva o **Microsoft.AspNet.SignalR.Hub** classe. Derivando de **Hub** classe é uma maneira útil para criar um aplicativo do SignalR. Você pode criar métodos públicos em sua classe de hub e, em seguida, esses métodos de acesso chamando-los a partir de scripts em uma página da web.

No código de bate-papo, clientes chamar o **ChatHub.Send** para enviar uma nova mensagem. O hub por sua vez envia a mensagem a todos os clientes chamando **Clients.All.addNewMessageToPage**.

O **enviar** método demonstra vários conceitos de hub:

- Declare métodos públicos em um hub para que os clientes podem chamá-los.
- Use o **Microsoft.AspNet.SignalR.Hub.Clients** propriedade para acessar todos os clientes conectados a esse hub.
- Chamar uma função no cliente (como o `addNewMessageToPage` função) para atualizar clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR e jQuery

O **Chat.cshtml** exibir o arquivo no exemplo de código mostra como usar a biblioteca de jQuery SignalR para se comunicar com um hub SignalR. As tarefas essenciais no código estão criando uma referência para o proxy geradas automaticamente para o hub, declarar uma função que o servidor pode chamar para conteúdo por push aos clientes e iniciar uma conexão para enviar mensagens para o hub.

O código a seguir declara uma referência a um proxy do hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> Em JavaScript a referência para a classe de servidor e seus membros está em minúsculas concatenadas. O exemplo de código referencia o c# **ChatHub** classe em JavaScript como **chatHub**. Se você quiser fazer referência a `ChatHub` classe jQuery com Pascal convencional maiusculas e minúsculas como você faria no c#, editar o arquivo de classe ChatHub.cs. Adicionar um `using` instrução para fazer referência a `Microsoft.AspNet.SignalR.Hubs` namespace. Em seguida, adicione o `HubName` de atributo para o `ChatHub` classe, por exemplo `[HubName("ChatHub")]`. Por fim, atualize sua referência jQuery para o `ChatHub` classe.


O código a seguir mostra como criar uma função de retorno de chamada no script. A classe de hub no servidor chama esta função para enviar atualizações de conteúdo para cada cliente. A chamada opcional para o `htmlEncode` função mostra uma maneira para HTML codifica o conteúdo da mensagem antes de exibi-lo na página, como uma maneira de impedir injeção de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

O código a seguir mostra como abrir uma conexão com o hub. O código começa a conexão e, em seguida, passa uma função para manipular o evento de clique no **enviar** botão na página de bate-papo.

> [!NOTE]
> Essa abordagem garante que a conexão seja estabelecida antes de executa o manipulador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Próximas etapas

Você aprendeu SignalR é uma estrutura para criar aplicativos web em tempo real. Você também aprendeu diversas tarefas de desenvolvimento SignalR: como adicionar SignalR para um aplicativo ASP.NET, como criar uma classe de hub e como enviar e receber mensagens do hub.

Para obter instruções sobre como implantar o aplicativo de exemplo de SignalR no Azure, consulte [usando SignalR com aplicativos Web no serviço de aplicativo do Azure](../deployment/using-signalr-with-azure-web-sites.md). Para obter informações detalhadas sobre como implantar um projeto da web do Visual Studio para um Site do Windows Azure, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).

Para saber mais avançados conceitos de desenvolvimentos SignalR, visite os seguintes sites para SignalR código-fonte e recursos:

- [Projeto de SignalR](http://signalr.net)
- [SignalR Github e exemplos](https://github.com/SignalR/SignalR)
- [Wiki do SignalR](https://github.com/SignalR/SignalR/wiki)
