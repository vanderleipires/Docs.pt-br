---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "Usando o SignalR com aplicativos Web no serviço de aplicativo do Azure | Microsoft Docs"
author: pfletcher
description: "Este documento descreve como configurar um aplicativo de SignalR é executado no Microsoft Azure. Versões de software usado no tutorial do Visual Studio 2013 ou Vis.."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Usando o SignalR com aplicativos Web no serviço de aplicativo do Azure
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este documento descreve como configurar um aplicativo de SignalR é executado no Microsoft Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) ou o Visual Studio 2012
> - .NET 4.5
> - SignalR versão 2
> - SDK 2.3 do Azure para Visual Studio 2013 ou 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), ou o [fóruns do Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Sumário

- [Introdução](#introduction)
- [Implantando um aplicativo Web de SignalR ao serviço de aplicativo do Azure](#deploying)
- [Habilitar WebSockets no serviço de aplicativo do Azure](#websocket)
- [Usando o Backplane de Cache Redis do Azure](#backplane)
- [Próximas Etapas](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introdução

O SignalR do ASP.NET pode ser usado para colocar um novo nível de interatividade entre servidores e web ou de clientes .NET. Quando hospedado no Azure, os aplicativos SignalR podem aproveitar altamente disponível, escalonável e ambiente de alto desempenho que em execução na nuvem fornece.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Implantando um aplicativo Web de SignalR ao serviço de aplicativo do Azure

SignalR não adiciona qualquer complicações específicas para implantar um aplicativo do Azure versus implantando em um servidor local. Um aplicativo que usa o SignalR pode ser hospedado no Azure sem alterações na configuração ou outras configurações (embora para suporte a WebSockets, consulte [habilitar WebSockets no serviço de aplicativo do Azure](#websocket) abaixo.) Para este tutorial, você implantará o aplicativo criado no [Tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md) no Azure.

**Pré-requisitos**

- Visual Studio 2013. Se você não tiver o Visual Studio, o Visual Studio 2013 Express para Web está incluído na instalação do SDK do Azure.
- [SDK 2.3 do Azure para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) ou [SDK 2.3 do Azure para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Para concluir este tutorial, você precisará de uma assinatura do Azure. Você pode [ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), ou [inscrever-se para uma assinatura de avaliação](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Implantar um aplicativo web do SignalR no Azure

1. Concluir o [Tutorial de Introdução](../getting-started/tutorial-getting-started-with-signalr.md), ou baixe o projeto concluído de [Galeria de códigos](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. No Visual Studio, selecione **criar**, **publicar SignalR bate-papo**.
3. Na caixa de diálogo "Publicar na Web", selecione "Windows Azure Web Sites".

    ![Selecione os Sites do Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Se você não tiver entrado sua conta da Microsoft, clique em **entrar...**  na caixa de diálogo "Selecionar Site existente" e de entrada.

    ![Selecione o Site da Web existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Entrar no Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Na caixa de diálogo "Selecionar Site existente", clique em **novo**.

    ![Novo Site](using-signalr-with-azure-web-sites/_static/image4.png)
6. Na caixa de diálogo "Criar o site no Windows Azure", digite um nome exclusivo do aplicativo. Selecione a região mais próxima de você no menu suspenso de região. Clique em **Criar**.

    ![Criar o site no Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. Na caixa de diálogo "Publicar na Web", clique em **publicar**.

    ![publicar o site](using-signalr-with-azure-web-sites/_static/image6.png)
8. Quando o aplicativo for concluída publicação, o aplicativo de SignalR Chat hospedado em aplicativos de Web do serviço de aplicativo do Azure será aberto em um navegador.

    ![Abrir em um navegador de site](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Habilitar WebSockets em aplicativos Web do serviço de aplicativo do Azure

O WebSocket precisa ser habilitado explicitamente em seu aplicativo web a ser usado em um aplicativo do SignalR; Caso contrário, serão usados outros protocolos (consulte [transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports) para obter detalhes).

Para usar o WebSocket em aplicativos de Web do serviço de aplicativo do Azure, habilitá-la na seção de configuração do aplicativo web. Para fazer isso, abra o seu aplicativo web a [Portal de gerenciamento](https://manage.windowsazure.com/)e selecione Configurar.

![Guia Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

Na parte superior da página de configuração, certifique-se de que o .NET 4.5 é usado para seu aplicativo web.

![Configuração do .NET framework versão 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

Na página de configuração, no **WebSockets** configuração, selecione **em**.

![Configuração de WebSockets: no](using-signalr-with-azure-web-sites/_static/image10.png)

Na parte inferior da página de configuração, selecione **salvar** para salvar suas alterações.

![Salvar configurações](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Usando o Backplane de Cache Redis do Azure

Se você usar várias instâncias do seu aplicativo web, e os usuários dessas instâncias precisam interagir com um outro (de forma que, por exemplo, mensagens de chat criadas em uma instância podem alcançar os usuários conectados a outras instâncias), o [Cache Redis do Azure backplane](../performance/scaleout-with-redis.md) devem ser implementados em seu aplicativo.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre aplicativos Web no serviço de aplicativo do Azure, consulte [visão geral de aplicativos Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
