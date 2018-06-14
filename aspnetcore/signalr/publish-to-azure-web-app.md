---
title: Publicar um núcleo de ASP.NET SignalR aplicativo para o aplicativo Web do Azure
author: rachelappel
description: Publicar um núcleo de ASP.NET SignalR aplicativo para o aplicativo Web do Azure
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 8612824cc9d4a9ace1c214411c44754350100f06
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341802"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Publicar um núcleo de ASP.NET SignalR aplicativo para um aplicativo Web do Azure

[Aplicativo Web do Azure](/azure/app-service/app-service-web-overview) é um [a computação em nuvem Microsoft](https://azure.microsoft.com/) serviço de plataforma para hospedar aplicativos web, incluindo o ASP.NET Core.

> [!NOTE]
> Este artigo se refere a publicação de um aplicativo do ASP.NET Core SignalR do Visual Studio. Visite [serviço SignalR para o Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) para obter mais informações sobre como usar o SignalR no Azure.

## <a name="publish-the-app"></a>Publique o aplicativo

Visual Studio fornece ferramentas internas para a publicação para um aplicativo Web do Azure. Usuário do Visual Studio Code pode usar [CLI do Azure](/cli/azure) comandos para publicar aplicativos no Azure. Este artigo aborda a publicação usando as ferramentas do Visual Studio. Para publicar um aplicativo usando a CLI do Azure, consulte [publicar um aplicativo do ASP.NET Core para o Azure com ferramentas de linha de comando](xref:tutorials/publish-to-azure-webapp-using-cli).

Clique com botão direito no projeto no **Solution Explorer** e selecione **publicar**. Confirme se **criar novo** check-in a **escolher um destino de publicação** caixa de diálogo e selecione **publicar**.

![Selecionar destino de publicação](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Insira as seguintes informações no **criar serviço de aplicativo** caixa de diálogo e selecione **criar**.

| Item | Descrição |
| ---- | ----------- |
| **Nome do aplicativo** | Um nome exclusivo do aplicativo. |
| **Assinatura** | A assinatura do Azure que usa o aplicativo. |
| **Grupo de recursos** | O grupo de recursos relacionados ao qual pertence o aplicativo.  |
| **Plano de hospedagem** | O plano de preço para o aplicativo web. |

![Criar serviço de aplicativo](publish-to-azure-web-app/_static/create-app-service-dialog.png)

O Visual Studio executará as seguintes tarefas:

* Cria um perfil de publicação que contém as configurações de publicação.
* Cria ou usa um existente *aplicativo Web do Azure* com os detalhes fornecidos.
* Publica o aplicativo.
* Inicia um navegador, com o aplicativo web publicado carregado.

Observe o formato da URL para o aplicativo é *.azurewebsites {nome do aplicativo} .net*. Por exemplo, um aplicativo chamado `SignalRChattR` tem uma URL semelhante a `https://signalrchattr.azurewebsites.net`.

Se ocorrer um erro de HTTP 502.2, consulte [versão de visualização de implantar o ASP.NET Core para o serviço de aplicativo do Azure](xref:host-and-deploy/azure-apps/index) resolvê-lo.

## <a name="configure-signalr-web-app"></a>Configurar o aplicativo web de SignalR

ASP.NET SignalR Core aplicativos que são publicados como um aplicativo Web do Azure deve ter [ARR afinidade](https://en.wikipedia.org/wiki/Application_Request_Routing) habilitado. [O WebSocket](xref:fundamentals/websockets) deve ser habilitado, para permitir que o transporte de WebSocket à função.

No portal do Azure, navegue até **configurações do aplicativo** para seu aplicativo web. Definir **WebSockets** para **na**e verifique se **ARR afinidade** é **em**.

![Configurações do aplicativo Web do Azure no portal do Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 O WebSocket e outros transportes [são limitados com base no plano de serviço de aplicativo](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Recursos relacionados

* [Publicar um aplicativo do ASP.NET Core para o Azure com ferramentas de linha de comando](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Publicar um aplicativo do ASP.NET Core para o Azure com o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Hospedar e implantar aplicativos de visualização do ASP.NET Core no Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
