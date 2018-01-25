---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: SignalR auto-host | Microsoft Docs'
author: pfletcher
description: "Este tutorial mostra como criar um servidor do SignalR 2 auto-hospedado e como conectá-la com um cliente JavaScript. Versões de software usadas no tutorial V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-signalr-self-host"></a>Tutorial: SignalR auto-host
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Este tutorial mostra como criar um servidor do SignalR 2 auto-hospedado e como conectá-la com um cliente JavaScript.
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
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Visão geral

Um servidor do SignalR é geralmente hospedado em um aplicativo ASP.NET no IIS, mas também pode ser auto-hospedado (como em um aplicativo de console ou o serviço do Windows) usando a biblioteca de hospedagem interna. Essa biblioteca, como todas SignalR 2, se baseia no OWIN ([Interface Web aberta para .NET](http://owin.org)). OWIN define uma abstração entre os servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, o que torna OWIN ideal para hospedagem interna de um aplicativo da web em seu próprio processo e fora do IIS.

Motivos para não hospedar no IIS:

- Ambientes em que o IIS não está disponível ou desejável, como um farm de servidores existente sem o IIS.
- A sobrecarga de desempenho do IIS deve ser evitado.
- Funcionalidade de SignalR é a ser adicionado a um aplicativo exising que é executado em um serviço do Windows, a função de trabalho do Azure ou outro processo.

Se uma solução está sendo desenvolvida como hospedar internamente por motivos de desempenho, recomenda-se também de teste o aplicativo hospedado no IIS para determinar o benefício de desempenho.

Este tutorial contém as seções a seguir:

- [Criar o servidor](#server)
- [Acessando o servidor com um cliente JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Criar o servidor

Neste tutorial, você criará um servidor que está hospedado em um aplicativo de console, mas o servidor pode ser hospedado em qualquer tipo de processo, como um serviço do Windows ou uma função de trabalho do Azure. Para obter o código de exemplo para hospedar um servidor do SignalR em um serviço do Windows, consulte [Self-Hosting SignalR em um serviço Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Abra o Visual Studio 2013 com privilégios de administrador. Selecione **arquivo**, **novo projeto**. Selecione **Windows** sob o **Visual c#** nó o **modelos** painel e selecione o **aplicativo de Console** modelo. Nomeie o novo projeto "SignalRSelfHost" e clique em **Okey**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Abra o console de Gerenciador de pacote de biblioteca selecionando **ferramentas**, **Gerenciador de biblioteca de pacote**, **Package Manager Console**.
3. No console do Gerenciador de pacote, digite o seguinte comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Este comando adiciona as bibliotecas de SignalR 2 Self-Host ao projeto.
4. No console do Gerenciador de pacote, digite o seguinte comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Este comando adiciona a biblioteca owin para o projeto. Essa biblioteca será usada para suporte de domínio cruzado, que é necessário para aplicativos que hospedam o SignalR e um cliente de página da web em domínios diferentes. Como você vai hospedando o servidor do SignalR e o cliente da web em portas diferentes, isso significa que esse domínio cruzado deve ser habilitado para a comunicação entre esses componentes.
5. Substitua o conteúdo de Program.cs pelo código a seguir.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    O código acima inclui três classes:

    - **Programa**, incluindo o **principal** definindo o caminho primário da execução do método. Nesse método, um aplicativo web do tipo **inicialização** é iniciado na URL especificada (`http://localhost:8080`). Se segurança for necessária no ponto de extremidade, o SSL pode ser implementado. Consulte [como: configurar uma porta com um certificado SSL](https://msdn.microsoft.com/library/ms733791.aspx) para obter mais informações.
    - **Inicialização**, a classe que contém a configuração para o servidor do SignalR (a única configuração este tutorial usa é a chamada ao `UseCors`) e a chamada para `MapSignalR`, que cria rotas para todos os objetos Hub no projeto.
    - **MyHub**, a classe de SignalR Hub que o aplicativo irá fornecer aos clientes. Essa classe tem um único método, **enviar**, que os clientes serão chamada para difundir uma mensagem para todos os outros clientes conectados.
6. Compile e execute o aplicativo. O endereço que o servidor está executando deve mostrar na janela do console.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Se a execução falhará com a exceção `System.Reflection.TargetInvocationException was unhandled`, você precisará reiniciar o Visual Studio com privilégios de administrador.
8. Pare o aplicativo antes de prosseguir para a próxima seção.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Acessando o servidor com um cliente JavaScript

Nesta seção, você usará o mesmo cliente JavaScript do [tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md). Só será feita uma modificação para o cliente, que é definir explicitamente a URL do hub. Com um aplicativo hospedado automaticamente, o servidor não necessariamente estejam no mesmo endereço como a URL de conexão (devido a proxies reversos e balanceadores de carga), então a URL precisa ser definida explicitamente.

1. Em **Solution Explorer**, com o botão direito na solução e selecione **adicionar**, **novo projeto**. Selecione o **Web** nó e selecione o **aplicativo Web ASP.NET** modelo. Nomeie o projeto "JavascriptClient" e clique em **Okey**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Selecione o **vazio** modelo e deixe as opções restantes desmarcadas. Selecione **criar projeto**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. No console do Gerenciador de pacote, selecione o projeto "JavascriptClient" no **projeto padrão** lista suspensa e execute o seguinte comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Esse comando instala as bibliotecas do SignalR e JQuery que será necessário no cliente.
4. Clique com botão direito no projeto e selecione **adicionar**, **Novo Item**. Selecione o **Web** nó e a página de seleção de HTML. Nomeie a página **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Substitua o conteúdo da nova página HTML com o código a seguir. Verifique se as referências de script correspondem os scripts na pasta Scripts do projeto.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    O código a seguir (realçado no exemplo de código acima) é a adição de que você fez para o cliente usado no tutorial obtendo Stared (além de atualizar o código para o SignalR beta de versão 2). Esta linha de código define explicitamente a URL de conexão base para o SignalR no servidor.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Com o botão direito na solução e selecione **definir projetos de inicialização...** . Selecione o **vários projetos de inicialização** botão de opção e defina os dois projetos **ação** para **iniciar**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Clique no "Default" e selecione **definir como página inicial**.
8. Execute o aplicativo. O servidor e a página serão iniciado. Talvez seja necessário recarregar a página da web (ou selecione **continuar** no depurador) se a página for carregada antes do servidor é iniciado.
9. No navegador, forneça um nome de usuário quando solicitado. Copie a URL da página em outra janela ou guia do navegador e fornecer um nome de usuário diferente. Você poderá enviar mensagens do painel de um navegador para outro, como o tutorial de Introdução.
