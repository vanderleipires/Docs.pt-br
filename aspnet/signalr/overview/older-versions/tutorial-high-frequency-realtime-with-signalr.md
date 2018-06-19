---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Alta frequência em tempo real com SignalR 1. x | Microsoft Docs
author: pfletcher
description: Este tutorial mostra como criar um aplicativo web que usa o ASP.NET SignalR para fornecer funcionalidade de mensagens de alta frequência. Alta frequência de mensagens em...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505835"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>Alta frequência em tempo real com SignalR 1. x
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este tutorial mostra como criar um aplicativo web que usa o ASP.NET SignalR para fornecer funcionalidade de mensagens de alta frequência. Nesse caso, mensagens de alta frequência significa atualizações que são enviadas a uma taxa fixa; no caso do aplicativo, até 10 mensagens por segundo.
> 
> O aplicativo que você criará neste tutorial exibe uma forma que os usuários podem arrastar. A posição da forma em todos os outros navegadores conectados será atualizada para corresponder a posição da forma arrastada usando atualizações regulares.
> 
> Conceitos apresentados neste tutorial tem aplicativos em jogos em tempo real e outros aplicativos de simulação.
> 
> Comentários sobre o tutorial são boas-vindas. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Visão Geral

Este tutorial demonstra como criar um aplicativo que compartilha o estado de um objeto com outros navegadores em tempo real. O aplicativo, criaremos é chamado MoveShape. A página de MoveShape exibirá um elemento HTML Div que o usuário pode arrastar; Quando o usuário arrasta a Div, a nova posição será enviada para o servidor, que informará todos os outros clientes conectados para atualizar a posição da forma a corresponder.

![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

O aplicativo criado neste tutorial se baseia em uma demonstração por Damian Edwards. Um vídeo que contém esta demonstração pode ser visto [aqui](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Começar o tutorial demonstra como enviar mensagens do SignalR de cada evento que dispara como a forma é arrastada. Todos os clientes conectados, em seguida, atualizará a posição da versão local da forma cada vez que uma mensagem é recebida.

Enquanto o aplicativo funcionará com esse método, isso não é um modelo de programação recomendado, pois não haverá nenhum limite superior para o número de mensagens obtendo enviadas, portanto, os clientes e o servidor podem obter sobrecarregados com mensagens e deve degradar o desempenho . A animação exibida no cliente também é separado, como a forma deve ser movida instantaneamente por cada método, em vez de mover perfeitamente para cada local. As seções posteriores do tutorial demonstrará como criar uma função de timer que restringe a taxa máxima em que as mensagens são enviadas pelo cliente ou servidor e como mover a forma perfeita entre locais. A versão final do aplicativo criado neste tutorial pode ser baixada do [Galeria de códigos](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Este tutorial contém as seções a seguir:

- [Pré-requisitos](#prerequisites)
- [Criar o projeto](#createtheproject)
- [Adicionar os pacotes de JQuery.UI NuGet ASP.NET SignalR](#nugetpackages)
- [Criar o aplicativo básico](#baseapp)
- [Adicionar o loop de cliente](#clientloop)
- [Adicionar o loop do servidor](#serverloop)
- [Adicionar animação smooth no cliente](#animation)
- [Etapas adicionais](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial requer o Visual Studio 2012 ou Visual Studio 2010. Se o Visual Studio 2010 for usado, o projeto usará o .NET Framework 4, em vez de .NET Framework 4.5.

Se você estiver usando o Visual Studio 2012, é recomendável que você instale o [atualização ASP.NET e Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Esta atualização contém novos recursos, como aprimoramentos à funcionalidade de publicação, novo e novos modelos.

Se você tiver o Visual Studio 2010, verifique se [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) está instalado.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Criar o projeto

Nesta seção, vamos criar o projeto no Visual Studio.

1. Do **arquivo** menu clique **novo projeto**.
2. No **novo projeto** caixa de diálogo caixa, expanda **c#** em **modelos** e selecione **Web**.
3. Selecione o **aplicativo Web vazio ASP.NET** modelo, nomeie o projeto *MoveShapeDemo*e clique em **Okey**.

    ![Criar novo projeto](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Adicione o SignalR e pacotes do JQuery.UI NuGet

Você pode adicionar funcionalidade SignalR para um projeto ao instalar um pacote do NuGet. Este tutorial também usará o pacote de JQuery.UI para permitir que o formato a ser arrastado e animado.

1. Clique em **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console**.
2. Digite o seguinte comando no Gerenciador de pacote.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    O pacote de SignalR instala um número de outros pacotes do NuGet como dependências. Quando a instalação for concluída você tem todos os componentes de servidor e cliente necessários para usar o SignalR em um aplicativo ASP.NET.
3. Digite o seguinte comando no console do Gerenciador de pacote para instalar os pacotes JQuery e JQuery.UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Criar o aplicativo básico

Nesta seção, vamos criar um aplicativo de navegador que envia o local da forma para o servidor durante cada evento mouse move. O servidor, em seguida, transmite essas informações para todos os outros clientes conectados conforme são recebido. Vamos expandir esse aplicativo em seções posteriores.

1. Em **Solution Explorer**, com o botão direito no projeto e selecione **adicionar**, **classe...** . Nomeie a classe **MoveShapeHub** e clique em **adicionar**.
2. Substitua o código no novo **MoveShapeHub** classe com o código a seguir.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    O `MoveShapeHub` classe acima é uma implementação de um hub SignalR. Como no [guia de Introdução ao SignalR](index.md) tutorial, o hub tem um método que os clientes chamará diretamente. Nesse caso, o cliente enviará um objeto que contém o novo coordenadas X e Y da forma para o servidor, que, em seguida, obtém transmitido para todos os outros clientes conectados. SignalR automaticamente serializa o objeto usando JSON.

    O objeto que será enviado ao cliente (`ShapeModel`) contém membros para armazenar a posição da forma. A versão do objeto no servidor também contém um membro para controlar dados do cliente que estão sendo armazenados, para que um determinado cliente não será enviado seus próprios dados. Esse membro usa o `JsonIgnore` atributo para impedir que está sendo serializado e enviado ao cliente.
3. Em seguida, vamos definir hub quando o aplicativo for iniciado. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Classe de aplicativo global**. Aceite o nome padrão de *Global* e clique em **Okey**.

    ![Adicionar classe de aplicativo Global](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Adicione o seguinte `using` instrução depois fornecido **usando** instruções na classe asax.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Adicione a seguinte linha de código no `Application_Start` método da classe Global para registrar a rota padrão para o SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    O arquivo global. asax deve parecer com o seguinte:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Em seguida, vamos adicionar o cliente. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Novo Item**. No **Adicionar Novo Item** caixa de diálogo, selecione **Página Html**. Dê um nome apropriado de página (como **Default.html**) e clique em **adicionar**.
7. Em **Solution Explorer**, clique com botão direito a página que você acabou de criar e clique em **definir como página inicial**.
8. Substitua o código padrão na página HTML com o seguinte trecho de código.

    > [!NOTE]
    > Verificar se o script faz referência abaixo correspondência os pacotes adicionados ao seu projeto na pasta Scripts. No Visual Studio 2010, a versão do JQuery e SignalR adicionado ao projeto pode não corresponder os números de versão abaixo.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    O código HTML e JavaScript acima cria um Div vermelho chamado forma, habilita o comportamento de arrastar da forma usando a biblioteca jQuery e usa a forma `drag` evento para enviar a posição da forma para o servidor.
9. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-o em uma segunda janela do navegador. Arraste a forma de uma das janelas do navegador; a forma na janela do navegador deve ser movido.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Adicionar o loop de cliente

Como enviar o local da forma em cada evento mouse move criará uma quantidade desnecessários do tráfego de rede, as mensagens do cliente precisam ser limitadas. Vamos usar o javascript `setInterval` função para configurar um loop que envia informações de posição de novo para o servidor a uma taxa fixa. Esse loop é uma representação muito básica de um "loop de jogo", uma função chamada repetidamente que conduz toda a funcionalidade de um jogo ou outros simulação.

1. Atualize o código de cliente na página HTML para coincidir com o seguinte trecho de código.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    A atualização acima adiciona o `updateServerModel` função, que é chamada em uma frequência fixa. Essa função envia os dados de posição para o servidor sempre que o `moved` sinalizador indica que há novos dados de posição para enviar.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-o em uma segunda janela do navegador. Arraste a forma de uma das janelas do navegador; a forma na janela do navegador deve ser movido. Desde que o número de mensagens que sejam enviados ao servidor será limitado, a animação não será exibido como smooth como na seção anterior.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Adicionar o loop do servidor

No aplicativo atual, as mensagens enviadas do servidor para o cliente sair geralmente são recebidos. Isso apresenta um problema semelhante, como foi visto no cliente; as mensagens podem ser enviadas com mais frequência do que eles são necessários e a conexão pode se tornar inundada como resultado. Esta seção descreve como atualizar o servidor para implementar um temporizador que limita a taxa das mensagens de saída.

1. Substitua o conteúdo do `MoveShapeHub.cs` com o seguinte trecho de código.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    O código acima expande o cliente para adicionar o `Broadcaster` classe, que limita o envio de mensagens usando o `Timer` classe do .NET framework.

    Como o hub em si é transitório (é criada toda vez que for necessário), o `Broadcaster` será criado como um singleton. Inicialização lenta (introduzida no .NET 4) é usada para adiar a criação, até que seja necessário, garantindo que a primeira instância de hub é criada completamente antes do timer é iniciado.

    A chamada para os clientes `UpdateShape` função é tirada do hub `UpdateModel` método, por que ele não é chamado imediatamente sempre que as mensagens de entrada são recebidas. Em vez disso, as mensagens para os clientes serão enviadas a uma taxa de 25 chamadas por segundo, gerenciado pelo `_broadcastLoop` timer de dentro de `Broadcaster` classe.

    Por fim, em vez de chamar o método de cliente do hub diretamente, o `Broadcaster` a classe precisa obter uma referência para o hub de operação no momento (`_hubContext`) usando o `GlobalHost`.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-o em uma segunda janela do navegador. Arraste a forma de uma das janelas do navegador; a forma na janela do navegador deve ser movido. Não haverá uma diferença visível no navegador da seção anterior, mas o número de mensagens que sejam enviados para o cliente será reduzido.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Adicionar animação smooth no cliente

O aplicativo está quase concluído, mas poderíamos criar um mais melhoria, em movimento da forma no cliente quando ele é movida em resposta a mensagens do servidor. Em vez de definir a posição da forma para o novo local fornecido pelo servidor, vamos usar a biblioteca de JQuery UI `animate` função para mover a forma perfeita entre sua posição atual e o nova.

1. Atualizar o cliente `updateShape` como o método para pesquisar o código realçado abaixo:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    O código acima move a forma do local antigo para o novo fornecido pelo servidor ao longo do intervalo de animação (nesse caso, 100 milissegundos). Nenhuma animação anterior em execução na forma é limpo antes do início da animação nova.
2. Inicie o aplicativo pressionando F5. Copie a URL da página e cole-o em uma segunda janela do navegador. Arraste a forma de uma das janelas do navegador; a forma na janela do navegador deve ser movido. A movimentação da forma na janela de deve aparecer em uma menor que o movimento é interpolado ao longo do tempo em vez de ser definida uma vez por mensagem de entrada.

    ![A janela do aplicativo](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Etapas adicionais

Neste tutorial, você aprendeu como programar um aplicativo SignalR que envia mensagens de alta frequência entre clientes e servidores. Esse paradigma de comunicação é útil para desenvolvimento de jogos online e outros simulações como [o jogo ShootR criado com o SignalR](http://shootr.signalr.net).

O aplicativo completo criado neste tutorial pode ser baixado do [Galeria de códigos](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Para saber mais sobre conceitos de desenvolvimento SignalR, visite os seguintes sites para SignalR código-fonte e recursos:

- [Projeto de SignalR](http://signalr.net)
- [SignalR Github e exemplos](https://github.com/SignalR/SignalR)
- [Wiki do SignalR](https://github.com/SignalR/SignalR/wiki)
