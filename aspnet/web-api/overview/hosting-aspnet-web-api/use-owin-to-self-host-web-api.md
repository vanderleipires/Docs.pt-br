---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usar o OWIN para auto-hospedar a API Web ASP.NET 2 | Microsoft Docs
author: rick-anderson
description: Este tutorial mostra como hospedar a API Web ASP.NET em um aplicativo de console, usando o OWIN para auto-hospedar a estrutura da API Web. Open Web Interface para .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 06fd13fe9b12d172d615ae76a71d246a89f5386d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910480"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Usar o OWIN para auto-hospedar a API Web ASP.NET 2
====================
por [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Este tutorial mostra como hospedar a API Web ASP.NET em um aplicativo de console, usando o OWIN para auto-hospedar a estrutura da API Web.
>
> [Open Web Interface para .NET](http://owin.org) (OWIN) define uma abstração entre servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, o que torna o OWIN ideal para auto-hospedagem em um aplicativo web em seu próprio processo, fora do IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (também funciona com o Visual Studio 2012)
> - API Web 2


> [!NOTE]
> Você pode encontrar o código-fonte completo para este tutorial em [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Criar um aplicativo de Console

No **arquivo** menu, clique em **New**, em seguida, clique em **projeto**. Partir **modelos instalados**, no Visual c#, clique em **Windows** e, em seguida, clique em **aplicativo de Console**. Nomeie o projeto "OwinSelfhostSample" e clique em **Okey**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Adicionar a API da Web e pacotes do OWIN

Do **ferramentas** menu, clique em **Gerenciador de pacotes NuGet**, em seguida, clique em **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Isso instalará o pacote de selfhost WebAPI OWIN e todos os pacotes necessários do OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurar API da Web para hospedar internamente

No Gerenciador de soluções, com o botão direito do mouse no projeto e selecione **Add** / **classe** para adicionar uma nova classe. Nomeie a classe `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Adicionar um controlador de API da Web

Em seguida, adicione uma classe de controlador de API da Web. No Gerenciador de soluções, com o botão direito do mouse no projeto e selecione **Add** / **classe** para adicionar uma nova classe. Nomeie a classe `ValuesController`.

Substitua todo o código clichê nesse arquivo com o seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Iniciar o Host do OWIN e fazer uma solicitação usando HttpClient

Substitua todo o código clichê no arquivo Program.cs pelo seguinte:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Executando o aplicativo

Para executar o aplicativo, pressione F5 no Visual Studio. A saída deve se parecer com o seguinte:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Recursos adicionais

[Uma visão geral do projeto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hospedar a API da Web ASP.NET em uma função de trabalho do Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
