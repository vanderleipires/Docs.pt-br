---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Os aplicativos SignalR de teste de unidade | Microsoft Docs
author: pfletcher
description: Este artigo descreve como usar os recursos de teste de unidade do SignalR 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: e55efd644dd4b6fb57061ffb89a5c041136c7b5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-signalr-applications"></a>Teste de unidade e os aplicativos SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este artigo descreve como usar os recursos de teste de unidade do SignalR 2. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versão 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Os aplicativos SignalR de teste de unidade

Você pode usar os recursos de teste de unidade no SignalR 2 para criar testes de unidade para o seu aplicativo SignalR. SignalR 2 inclui a [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, que pode ser usado para criar um objeto de simulação para simular seus métodos de hub para teste.

Nesta seção, você adicionará os testes de unidade para o aplicativo criado no [tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md) usando [XUnit.net](https://github.com/xunit/xunit) e [Moq](https://github.com/Moq/moq4).

XUnit.net será usado para controlar o teste; Moq será usado para criar um [simular](http://en.wikipedia.org/wiki/Mock_object) objeto para teste. Outras estruturas fictícias podem ser usadas se quiser; [NSubstitute](http://nsubstitute.github.io/) também é uma boa opção. Este tutorial demonstra como configurar o objeto de simulação de duas maneiras: primeiro, usando um `dynamic` objeto (introduzido no .NET Framework 4) e, segundo, usando uma interface.

### <a name="contents"></a>Conteúdo

Este tutorial contém as seções a seguir.

- [Testes de unidade com dinâmico](#dynamic)
- [Teste pelo tipo de unidade](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Testes de unidade com dinâmico

Nesta seção, você adicionará um teste de unidade para o aplicativo criado no [tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md) usando um objeto dinâmico.

1. Instalar o [extensão XUnit executor](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) para Visual Studio 2013.
2. Completar o [tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md), ou baixe o aplicativo concluído de [Galeria de códigos do MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Se você estiver usando a versão de download do aplicativo Introdução, abra **Package Manager Console** e clique em **restaurar** para adicionar o pacote de SignalR ao projeto.

    ![Restaurar os pacotes](unit-testing-signalr-applications/_static/image1.png)
4. Adicione um projeto à solução para o teste de unidade. Clique com botão direito em sua solução de **Solution Explorer** e selecione **adicionar**, **novo projeto...** . Sob o **c#** nó, selecione o **Windows** nó. Selecione **a biblioteca de classe**. Nomeie o novo projeto **TestLibrary** e clique em **Okey**.

    ![Criar a biblioteca de teste](unit-testing-signalr-applications/_static/image2.png)
5. Adicione uma referência no projeto de biblioteca de teste ao projeto SignalRChat. Clique com botão direito do **TestLibrary** do projeto e selecione **adicionar**, **referência...** . Selecione o **projetos** nó sob o **solução** nó e seleção **SignalRChat**. Clique em **OK**.

    ![Adicionar referência de projeto](unit-testing-signalr-applications/_static/image3.png)
6. Adicionar os pacotes SignalR, Moq e XUnit o **TestLibrary** projeto. No **Package Manager Console**, defina o **projeto padrão** lista suspensa para **TestLibrary**. Execute os comandos a seguir na janela do console:

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Instalar pacotes](unit-testing-signalr-applications/_static/image4.png)
7. Crie o arquivo de teste. Clique com botão direito do **TestLibrary** do projeto e clique em **adicionar...** , **Classe**. Nomeie a nova classe **Tests.cs**.
8. Substitua o conteúdo de Tests.cs com o código a seguir.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    No código acima, um cliente de teste é criado usando o `Mock` de objeto o [Moq](https://github.com/Moq/moq4) biblioteca de tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (2.1 SignalR, atribuir `dynamic` para o tipo parâmetro). O `IHubCallerConnectionContext` interface é o objeto de proxy com o qual você chamar métodos no cliente. O `broadcastMessage` função é definida para o cliente fictício para que ele possa ser chamado `ChatHub` classe. O mecanismo de teste, em seguida, chama o `Send` método o `ChatHub` classe, que por sua vez, chama o fictícios `broadcastMessage` função.
9. Compile a solução pressionando **F6**.
10. Execute o teste de unidade. No Visual Studio, selecione **teste**, **Windows**, **Gerenciador de testes**. Na janela do Gerenciador de testes, clique com botão direito **HubsAreMockableViaDynamic** e selecione **executar testes selecionados**.

    ![Gerenciador de Testes](unit-testing-signalr-applications/_static/image5.png)
11. Verifique se que o teste foi bem-sucedido, verificando o painel inferior na janela do Gerenciador de testes. A janela mostrará que o teste foi bem-sucedido.

    ![Teste aprovado](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Teste pelo tipo de unidade

Nesta seção, você adicionará um teste para o aplicativo criado no [tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md) usando uma interface que contém o método a ser testada.

1. Conclua as etapas 1 a 7 no [testes de unidade com dinâmico](#dynamic) tutorial acima.
2. Substitua o conteúdo de Tests.cs com o código a seguir.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    No código acima, uma interface é criada definindo a assinatura do `broadcastMessage` método para o qual o mecanismo de teste irá criar um cliente de simulação. Um cliente de simulação é criado usando o `Mock` objeto do tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (2.1 SignalR, atribuir `dynamic` para o parâmetro de tipo.) O `IHubCallerConnectionContext` interface é o objeto de proxy com o qual você chamar métodos no cliente.

    O teste, em seguida, cria uma instância de `ChatHub`e, em seguida, cria uma versão fictícia do `broadcastMessage` método, que por sua vez é invocado por chamar o `Send` método no hub.
3. Compile a solução pressionando **F6**.
4. Execute o teste de unidade. No Visual Studio, selecione **teste**, **Windows**, **Gerenciador de testes**. Na janela do Gerenciador de testes, clique com botão direito **HubsAreMockableViaDynamic** e selecione **executar testes selecionados**.

    ![Gerenciador de Testes](unit-testing-signalr-applications/_static/image7.png)
5. Verifique se que o teste foi bem-sucedido, verificando o painel inferior na janela do Gerenciador de testes. A janela mostrará que o teste foi bem-sucedido.

    ![Teste aprovado](unit-testing-signalr-applications/_static/image8.png)
