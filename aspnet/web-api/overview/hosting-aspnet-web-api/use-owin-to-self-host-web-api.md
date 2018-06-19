---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Use OWIN para hospedar o ASP.NET Web API 2 | Microsoft Docs
author: rick-anderson
description: Este tutorial mostra como hospedar API Web do ASP.NET em um aplicativo de console, usando OWIN para a estrutura da API Web de hospedagem interna. Abra a Interface da Web para .NET (OWIN) d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506965"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Use OWIN para hospedar o ASP.NET Web API 2
====================
por [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Este tutorial mostra como hospedar API Web do ASP.NET em um aplicativo de console, usando OWIN para a estrutura da API Web de hospedagem interna.
> 
> [Abra a Interface da Web para .NET](http://owin.org) (OWIN) define uma abstração entre os servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, o que torna OWIN ideal para hospedagem interna de um aplicativo da web em seu próprio processo e fora do IIS.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (também funciona com o Visual Studio 2012)
> - Web API 2


> [!NOTE]
> Você pode encontrar o código-fonte completo para este tutorial em [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Criar um aplicativo de Console

No **arquivo** menu, clique em **novo**, em seguida, clique em **projeto**. De **modelos instalados**, em Visual c#, clique em **Windows** e, em seguida, clique em **aplicativo de Console**. Nomeie o projeto "OwinSelfhostSample" e clique em **Okey**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Adicionar a API da Web e pacotes OWIN

Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote**, em seguida, clique em **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Isso instalará o pacote de selfhost WebAPI OWIN e todos os pacotes necessários do OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurar a API da Web para hospedar internamente

No Gerenciador de soluções, clique com botão direito no projeto e selecione **adicionar** / **classe** para adicionar uma nova classe. Nomeie a classe `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Adicionar um controlador de API da Web

Em seguida, adicione uma classe de controlador de API da Web. No Gerenciador de soluções, clique com botão direito no projeto e selecione **adicionar** / **classe** para adicionar uma nova classe. Nomeie a classe `ValuesController`.

Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Iniciar o Host OWIN e fazer uma solicitação usando HttpClient

Substitua todo o código clichê no arquivo Program.cs pelo seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Executando o aplicativo

Para executar o aplicativo, pressione F5 no Visual Studio. A saída deve se parecer com o seguinte:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Recursos adicionais

[Uma visão geral do projeto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hospedar a API da Web ASP.NET em uma função de trabalho do Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
