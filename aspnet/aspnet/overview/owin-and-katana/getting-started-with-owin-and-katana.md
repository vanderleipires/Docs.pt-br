---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introdução ao OWIN e Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
<a name="getting-started-with-owin-and-katana"></a>Introdução ao OWIN e Katana
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Abra a Interface da Web para .NET (OWIN)](http://owin.org/) define uma abstração entre os servidores de web do .NET e aplicativos da web. Separando o servidor web do aplicativo, OWIN torna mais fácil criar middleware para o desenvolvimento de web .NET. Além disso, o OWIN torna mais fácil a aplicativos da web de porta para outros hosts&#8212;por exemplo, hospedagem interna em um serviço do Windows ou outro processo.

OWIN é uma especificação de propriedade da comunidade, não uma implementação. O projeto Katana é um conjunto de componentes do código-fonte aberto OWIN desenvolvida pela Microsoft. Para obter uma visão geral do OWIN e Katana, consulte [uma visão geral de projeto Katana](an-overview-of-project-katana.md). Neste artigo, eu será ir diretamente no código para começar.

Este tutorial usa [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mas você também pode usar o Visual Studio 2012. Algumas das etapas são diferentes no Visual Studio 2012, o que eu observação abaixo.

## <a name="host-owin-in-iis"></a>Host OWIN no IIS

Nesta seção, vamos vai hospedar OWIN no IIS. Essa opção fornece a flexibilidade e capacidade de combinação de um pipeline OWIN junto com o conjunto de recursos madura do IIS. Usando essa opção, o aplicativo OWIN é executado no pipeline de solicitação do ASP.NET.

Primeiro, crie um novo projeto de aplicativo Web ASP.NET. (No Visual Studio 2012, use o tipo de projeto de aplicativo Web vazio ASP.NET).

![](getting-started-with-owin-and-katana/_static/image1.png)

Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Adicione pacotes NuGet

Em seguida, adicione os pacotes do NuGet necessários. Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Adicionar uma classe de inicialização

Em seguida, adicione uma classe de inicialização OWIN. No Gerenciador de Soluções, clique com o botão direito e selecione **adicionar**, em seguida, selecione **Novo Item**. No **Adicionar Novo Item** caixa de diálogo, selecione **classe de inicialização Owin**. Para obter mais informações sobre como configurar a classe de inicialização, consulte [detecção de classe de inicialização OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Adicione o seguinte código ao método `Startup1.Configuration`:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Esse código adiciona uma parte simple de middleware no pipeline OWIN, implementado como uma função que recebe um **Microsoft.Owin.IOwinContext** instância. Quando o servidor recebe uma solicitação HTTP, o pipeline OWIN invoca o middleware. O middleware define o tipo de conteúdo para a resposta e grava o corpo da resposta.

> [!NOTE]
> O modelo de classe de inicialização OWIN está disponível no Visual Studio 2013. Se você estiver usando o Visual Studio 2012, basta adicionar uma nova classe vazia denominada `Startup1`e cole o código a seguir:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Executar o aplicativo

Pressione F5 para iniciar a depuração. O Visual Studio abrirá uma janela do navegador para `http://localhost:*port*/`. A página deve ser semelhante ao seguinte:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>OWIN auto-host em um aplicativo de Console

É fácil converter este aplicativo de hospedagem do IIS para hospedagem interna em um processo personalizado. Com a hospedagem do IIS, IIS atua como o servidor HTTP e o processo que hospeda o serviço. Com hospedagem própria, seu aplicativo cria o processo e usa o **HttpListener** classe como o servidor HTTP.

No Visual Studio, crie um novo aplicativo de console. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Adicionar um `Startup1` classe da parte 1 deste tutorial ao projeto. Você não precisa modificar essa classe.

Implementar o aplicativo `Main` método da seguinte maneira.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Quando você executa o aplicativo de console, o servidor começa a escutar `http://localhost:9000`. Se você navegar para esse endereço em um navegador da web, você verá a página "Olá, mundo".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Adicionar OWIN Diagnostics

O pacote de pt contém middleware que captura exceções sem tratamento e exibe uma página HTML com os detalhes do erro. Essas funções de página muito como a página de erro do ASP.NET que às vezes é chamada de "[amarela tela de morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Como o YSOD, a página de erro Katana é útil durante o desenvolvimento, mas é uma boa prática para desabilitá-lo no modo de produção.

Para instalar o pacote de diagnóstico em seu projeto, digite o seguinte comando na janela do Console do Gerenciador de pacotes:

`install-package Microsoft.Owin.Diagnostics –Pre`

Altere o código no seu `Startup1.Configuration` método da seguinte maneira:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Agora use CTRL + F5 para executar o aplicativo sem depuração, para que o Visual Studio não será interrompido na exceção. O aplicativo se comporta como antes, até que você navegue até `http://localhost/fail`, no ponto em que o aplicativo lança a exceção. O middleware de página de erro será capturar a exceção e exibir uma página HTML com informações sobre o erro. Você pode clicar nas guias para ver a pilha de cadeia de caracteres de consulta, cookies, cabeçalho de solicitação e variáveis de ambiente OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Próximas etapas

- [Detecção de classe de inicialização OWIN](owin-startup-class-detection.md)
- [Use OWIN para auto-host API da Web do ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Use OWIN para o próprio Host SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
