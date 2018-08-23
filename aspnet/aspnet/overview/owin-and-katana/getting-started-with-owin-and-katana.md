---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introdução ao OWIN e Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: f400ec887bdee123084f582d18016fb5ed3f2165
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825380"
---
<a name="getting-started-with-owin-and-katana"></a>Introdução ao OWIN e Katana
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Open Web Interface para .NET (OWIN)](http://owin.org/) define uma abstração entre servidores de web do .NET e aplicativos da web. Desacoplando o servidor web do aplicativo, OWIN torna mais fácil criar middleware para desenvolvimento de web do .NET. Além disso, o OWIN torna mais fácil de portar aplicativos da web para outros hosts&#8212;por exemplo, a hospedagem interna em um serviço do Windows ou outro processo.

OWIN é uma especificação de propriedade da comunidade, não uma implementação. O projeto Katana é um conjunto de componentes do OWIN de código-fonte aberto desenvolvida pela Microsoft. Para obter uma visão geral do OWIN e Katana, consulte [uma visão geral do projeto Katana](an-overview-of-project-katana.md). Neste artigo, eu será ir diretamente para um código para começar.

Este tutorial usa [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mas você também pode usar o Visual Studio 2012. Algumas das etapas são diferentes no Visual Studio 2012, o que eu observação abaixo.

## <a name="host-owin-in-iis"></a>Hospedar OWIN em IIS

Nesta seção, hospedaremos OWIN no IIS. Essa opção lhe dá a flexibilidade e capacidade de composição de um pipeline do OWIN junto com o conjunto maduro de recursos do IIS. Usando essa opção, o aplicativo OWIN executa no pipeline de solicitação do ASP.NET.

Primeiro, crie um novo projeto de aplicativo Web ASP.NET. (No Visual Studio 2012, use o tipo de projeto de aplicativo Web vazio ASP.NET).

![](getting-started-with-owin-and-katana/_static/image1.png)

Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Adicionar pacotes NuGet

Em seguida, adicione os pacotes NuGet necessários. Dos **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Adicione uma classe de inicialização

Em seguida, adicione uma classe de inicialização do OWIN. No Gerenciador de Soluções, clique com o botão direito e selecione **adicionar**, em seguida, selecione **Novo Item**. No **Adicionar Novo Item** caixa de diálogo, selecione **classe de inicialização Owin**. Para obter mais informações sobre como configurar a classe de inicialização, consulte [detecção de classe de inicialização OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Adicione o seguinte código ao método `Startup1.Configuration`:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Este código adiciona uma simple peça de middleware ao pipeline do OWIN, implementado como uma função que recebe um **Microsoft.Owin.IOwinContext** instância. Quando o servidor recebe uma solicitação HTTP, o pipeline do OWIN invoca o middleware. O middleware define o tipo de conteúdo para a resposta e grava o corpo da resposta.

> [!NOTE]
> O modelo de classe de inicialização OWIN está disponível no Visual Studio 2013. Se você estiver usando o Visual Studio 2012, basta adicionar uma nova classe vazia chamada `Startup1`e cole no código a seguir:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Executar o aplicativo

Pressione F5 para iniciar a depuração. Visual Studio abrirá uma janela do navegador para `http://localhost:*port*/`. A página deve ser semelhante ao seguinte:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Auto-hospedar OWIN em um aplicativo de Console

É fácil converter esse aplicativo de hospedagem do IIS para hospedagem interna em um processo personalizado. Com a hospedagem do IIS, IIS atua como o servidor HTTP e como o processo que hospeda o serviço. Com a hospedagem interna, seu aplicativo cria o processo e usa o **HttpListener** classe como o servidor HTTP.

No Visual Studio, crie um novo aplicativo de console. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Adicionar um `Startup1` classe da parte 1 deste tutorial ao projeto. Você não precisa modificar essa classe.

Implementar o aplicativo `Main` método da seguinte maneira.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Quando você executa o aplicativo de console, o servidor começa a escutar `http://localhost:9000`. Se você navegar até esse endereço em um navegador da web, você verá a página "Hello world".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Adicionar o diagnóstico OWIN

O pacote de Microsoft.Owin.Diagnostics contém middleware que captura exceções sem tratamento e exibe uma página HTML com detalhes do erro. Essa funções de página muito como a página de erro do ASP.NET que às vezes é chamada de "[tela amarela morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Como o YSOD, a página de erro do Katana é útil durante o desenvolvimento, mas é uma boa prática para desabilitá-lo no modo de produção.

Para instalar o pacote de diagnóstico em seu projeto, digite o seguinte comando na janela do Console do Gerenciador de pacotes:

`install-package Microsoft.Owin.Diagnostics –Pre`

Altere o código no seu `Startup1.Configuration` método da seguinte maneira:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Agora, use CTRL + F5 para executar o aplicativo sem depuração, para que o Visual Studio não interromperá na exceção. O aplicativo se comporta da mesma como antes, até que você navegue até `http://localhost/fail`, no ponto em que o aplicativo gera a exceção. O middleware de página de erro será capturar a exceção e exibirá uma página HTML com informações sobre o erro. Você pode clicar nas guias para ver a pilha de cadeia de caracteres de consulta, cookies, cabeçalho de solicitação e as variáveis de ambiente do OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Próximas etapas

- [Detecção de classe de inicialização OWIN](owin-startup-class-detection.md)
- [Usar o OWIN para auto-hospedar a API Web ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Usar o OWIN para auto-hospedar SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
