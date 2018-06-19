---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: Em tempo real de alta frequência com SignalR 2 | Microsoft Docs'
author: pfletcher
description: Este tutorial mostra como criar um aplicativo web que usa o ASP.NET SignalR para fornecer funcionalidade de mensagens de alta frequência. Alta frequência de mensagens em...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036109"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Tutorial: Em tempo real de alta frequência com SignalR 2
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> Este tutorial mostra como criar um aplicativo web que usa o ASP.NET SignalR 2 para fornecer funcionalidade de mensagens de alta frequência. Nesse caso, mensagens de alta frequência significa atualizações que são enviadas a uma taxa fixa; no caso do aplicativo, até 10 mensagens por segundo.
> 
> O aplicativo que você criará neste tutorial exibe uma forma que os usuários podem arrastar. A posição da forma em todos os outros navegadores conectados será atualizada para corresponder a posição da forma arrastada usando atualizações regulares.
> 
> Conceitos apresentados neste tutorial tem aplicativos em jogos em tempo real e outros aplicativos de simulação.
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

Este tutorial demonstra como criar um aplicativo que compartilha o estado de um objeto com outros navegadores em tempo real. O aplicativo, criaremos é chamado MoveShape. A página de MoveShape exibirá um elemento HTML Div que o usuário pode arrastar; Quando o usuário arrasta a Div, a nova posição será enviada para o servidor, que informará todos os outros clientes conectados para atualizar a posição da forma a corresponder.

![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

O aplicativo criado neste tutorial se baseia em uma demonstração por Damian Edwards. Um vídeo que contém esta demonstração pode ser visto [aqui](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Começar o tutorial demonstra como enviar mensagens do SignalR de cada evento que dispara como a forma é arrastada. Todos os clientes conectados, em seguida, atualizará a posição da versão local da forma cada vez que uma mensagem é recebida.

Enquanto o aplicativo funcionará com esse método, isso não é um modelo de programação recomendado, pois não haverá nenhum limite superior para o número de mensagens obtendo enviadas, portanto, os clientes e o servidor podem obter sobrecarregados com mensagens e deve degradar o desempenho . A animação exibida no cliente também é separado, como a forma deve ser movida instantaneamente por cada método, em vez de mover perfeitamente para cada local. As seções posteriores do tutorial demonstrará como criar uma função de timer que restringe a taxa máxima em que as mensagens são enviadas pelo cliente ou servidor e como mover a forma perfeita entre locais. A versão final do aplicativo criado neste tutorial pode ser baixada do [Galeria de códigos](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Este tutorial contém as seções a seguir:

- [Pré-requisitos](#prerequisites)
- [Criar o projeto e adicionar o pacote de JQuery.UI NuGet SignalR](#createtheproject2013)
- [Criar o aplicativo básico](#baseapp)
- [Iniciando o hub quando o aplicativo for iniciado](#startup2013)
- [Adicionar o loop de cliente](#clientloop)
- [Adicionar o loop do servidor](#serverloop)
- [Adicionar animação smooth no cliente](#animation)
- [Etapas adicionais](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial requer o Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Criar o projeto e adicionar o pacote de JQuery.UI NuGet SignalR

Nesta seção, vamos criar o projeto no Visual Studio 2013.

As etapas a seguir usam o Visual Studio 2013 para criar um aplicativo Web vazio do ASP.NET e adicionar o SignalR e jQuery.UI bibliotecas:

1. No Visual Studio, crie um aplicativo Web ASP.NET.

    ![Criar web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. No **novo projeto ASP.NET** janela, deixe **vazio** selecionado e clique em **criar projeto**.

    ![Criar web vazio](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. Em **Solution Explorer**, clique com o botão direito, selecione **adicionar | Classe de Hub SignalR (v2)**. Nomeie a classe **MoveShapeHub.cs** e adicioná-lo ao projeto. Esta etapa cria a **MoveShapeHub** classe e o adiciona ao projeto um conjunto de arquivos de script e referências de assembly que dão suporte ao SignalR.

    > [!NOTE]
    > Você também pode adicionar o SignalR para um projeto clicando **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console** e executar um comando:

    `install-package Microsoft.AspNet.SignalR`. 

    Se você usar o console para adicionar o SignalR, crie a classe de hub SignalR como uma etapa separada depois de adicionar o SignalR.
4. Clique em **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console**. Na janela do Gerenciador de pacote, execute o seguinte comando:

    `Install-Package jQuery.UI.Combined`

    Isso instala a biblioteca de interface do usuário do jQuery, você usará para animar a forma.
5. Em **Solution Explorer** expanda o nó de Scripts. Bibliotecas de scripts para jQuery, jQueryUI e SignalR são visíveis no projeto.

    ![Referências da biblioteca de script](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Criar o aplicativo básico

Nesta seção, vamos criar um aplicativo de navegador que envia o local da forma para o servidor durante cada evento mouse move. O servidor, em seguida, transmite essas informações para todos os outros clientes conectados conforme são recebido. Vamos expandir esse aplicativo em seções posteriores.

1. Se você ainda não criou a classe MoveShapeHub.cs, em **Solution Explorer**, com o botão direito no projeto e selecione **adicionar**, **classe...** . Nomeie a classe **MoveShapeHub** e clique em **adicionar**.
2. Substitua o código no novo **MoveShapeHub** classe com o código a seguir.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    O `MoveShapeHub` classe acima é uma implementação de um hub SignalR. Como no [guia de Introdução ao SignalR](tutorial-getting-started-with-signalr.md) tutorial, o hub tem um método que os clientes chamará diretamente. Nesse caso, o cliente enviará um objeto que contém o novo coordenadas X e Y da forma para o servidor, que, em seguida, obtém transmitido para todos os outros clientes conectados. SignalR automaticamente serializa o objeto usando JSON.

    O objeto que será enviado ao cliente (`ShapeModel`) contém membros para armazenar a posição da forma. A versão do objeto no servidor também contém um membro para controlar dados do cliente que estão sendo armazenados, para que um determinado cliente não será enviado seus próprios dados. Esse membro usa o `JsonIgnore` atributo para impedir que está sendo serializado e enviado ao cliente.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Iniciando o hub quando o aplicativo for iniciado

1. Em seguida, vamos definir o mapeamento para o hub quando o aplicativo for iniciado. SignalR 2, isso é feito pela adição de uma classe de inicialização OWIN, que chamará `MapSignalR` quando a classe de inicialização `Configuration` método é executado quando inicia do OWIN. A classe é adicionada a inicialização do OWIN processar usando a `OwinStartup` atributo do assembly.

    Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Classe de inicialização OWIN**. Nomeie a classe *inicialização* e clique em **Okey**.
2. Altere o conteúdo de Startup.cs para o seguinte:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Adicionando o cliente

1. Em seguida, vamos adicionar o cliente. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Novo Item**. No **Adicionar Novo Item** caixa de diálogo, selecione **Página Html**. Nomeie a página **Default.html** e clique em **adicionar**.
2. Em **Solution Explorer**, clique com botão direito a página que você acabou de criar e clique em **definir como página inicial**.
3. Substitua o código padrão na página HTML com o seguinte trecho de código.

    > [!NOTE]
    > Verificar se o script faz referência abaixo correspondência os pacotes adicionados ao seu projeto na pasta Scripts.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    O código HTML e JavaScript acima cria um Div vermelho chamado forma, habilita o comportamento de arrastar da forma usando a biblioteca jQuery e usa a forma `drag` evento para enviar a posição da forma para o servidor.
4. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-o em uma segunda janela do navegador. Arraste a forma de uma das janelas do navegador; a forma na janela do navegador deve ser movido.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Adicionar o loop de cliente

Como enviar o local da forma em cada evento mouse move criará uma quantidade desnecessários do tráfego de rede, as mensagens do cliente precisam ser limitadas. Vamos usar o javascript `setInterval` função para configurar um loop que envia informações de posição de novo para o servidor a uma taxa fixa. Esse loop é uma representação muito básica de um "loop de jogo", uma função chamada repetidamente que conduz toda a funcionalidade de um jogo ou outros simulação.

1. Atualize o código de cliente na página HTML para coincidir com o seguinte trecho de código.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    A atualização acima adiciona o `updateServerModel` função, que é chamada em uma frequência fixa. Essa função envia os dados de posição para o servidor sempre que o `moved` sinalizador indica que há novos dados de posição para enviar.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-o em uma segunda janela do navegador. Arraste a forma de uma das janelas do navegador; a forma na janela do navegador deve ser movido. Desde que o número de mensagens que sejam enviados ao servidor será limitado, a animação não será exibido como smooth como na seção anterior.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Adicionar o loop do servidor

No aplicativo atual, as mensagens enviadas do servidor para o cliente sair geralmente são recebidos. Isso apresenta um problema semelhante, como foi visto no cliente; as mensagens podem ser enviadas com mais frequência do que eles são necessários e a conexão pode se tornar inundada como resultado. Esta seção descreve como atualizar o servidor para implementar um temporizador que limita a taxa das mensagens de saída.

1. Substitua o conteúdo do `MoveShapeHub.cs` com o seguinte trecho de código.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    O código acima expande o cliente para adicionar o `Broadcaster` classe, que limita o envio de mensagens usando o `Timer` classe do .NET framework.

    Como o hub em si é transitório (é criada toda vez que for necessário), o `Broadcaster` será criado como um singleton. Inicialização lenta (introduzida no .NET 4) é usada para adiar a criação, até que seja necessário, garantindo que a primeira instância de hub é criada completamente antes do timer é iniciado.

    A chamada para os clientes `UpdateShape` função é tirada do hub `UpdateModel` método, por que ele não é chamado imediatamente sempre que as mensagens de entrada são recebidas. Em vez disso, as mensagens para os clientes serão enviadas a uma taxa de 25 chamadas por segundo, gerenciado pelo `_broadcastLoop` timer de dentro de `Broadcaster` classe.

    Por fim, em vez de chamar o método de cliente do hub diretamente, o `Broadcaster` a classe precisa obter uma referência para o hub de operação no momento (`_hubContext`) usando o `GlobalHost`.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-o em uma segunda janela do navegador. Arraste a forma de uma das janelas do navegador; a forma na janela do navegador deve ser movido. Não haverá uma diferença visível no navegador da seção anterior, mas o número de mensagens que sejam enviados para o cliente será reduzido.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Adicionar animação smooth no cliente

O aplicativo está quase concluído, mas poderíamos criar um mais melhoria, em movimento da forma no cliente quando ele é movida em resposta a mensagens do servidor. Em vez de definir a posição da forma para o novo local fornecido pelo servidor, vamos usar a biblioteca de JQuery UI `animate` função para mover a forma perfeita entre sua posição atual e o nova.

1. Atualizar o cliente `updateShape` como o método para pesquisar o código realçado abaixo:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    O código acima move a forma do local antigo para o novo fornecido pelo servidor ao longo do intervalo de animação (nesse caso, 100 milissegundos). Nenhuma animação anterior em execução na forma é limpo antes do início da animação nova.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-o em uma segunda janela do navegador. Arraste a forma de uma das janelas do navegador; a forma na janela do navegador deve ser movido. A movimentação da forma na janela de deve aparecer em uma menor que o movimento é interpolado ao longo do tempo em vez de ser definida uma vez por mensagem de entrada.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Etapas adicionais

Neste tutorial, você aprendeu como programar um aplicativo SignalR que envia mensagens de alta frequência entre clientes e servidores. Esse paradigma de comunicação é útil para desenvolvimento de jogos online e outros simulações como [o jogo ShootR criado com o SignalR](http://shootr.signalr.net).

O aplicativo completo criado neste tutorial pode ser baixado do [Galeria de códigos](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Para saber mais sobre conceitos de desenvolvimento SignalR, visite os seguintes sites para SignalR código-fonte e recursos:

- [Projeto de SignalR](http://signalr.net)
- [SignalR Github e exemplos](https://github.com/SignalR/SignalR)
- [Wiki do SignalR](https://github.com/SignalR/SignalR/wiki)

Para obter instruções sobre como implantar um aplicativo SignalR no Azure, consulte [usando SignalR com aplicativos Web no serviço de aplicativo do Azure](../deployment/using-signalr-with-azure-web-sites.md). Para obter informações detalhadas sobre como implantar um projeto da web do Visual Studio para um Site do Windows Azure, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
