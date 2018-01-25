---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usando o Web API 2 com o Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: "Este tutorial irá ensiná-lo a Noções básicas de criação de um aplicativo web com uma API da Web ASP.NET back-end. O tutorial usa o Entity Framework 6 para o layout de dados..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: cceefa128f90b4c3e23dd31119f44e6ffc55f46f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a>Usando o Web API 2 com o Entity Framework 6
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

> Este tutorial irá ensiná-lo a Noções básicas de criação de um aplicativo web com uma API da Web ASP.NET back-end. O tutorial usa o Entity Framework 6 para a camada de dados e Knockout. js para o aplicativo de JavaScript do lado do cliente. O tutorial também mostra como implantar o aplicativo para aplicativos de Web do serviço de aplicativo do Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - 2.1 da API da Web
> - [Visual Studio 2013 Atualização 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout. js](http://knockoutjs.com/) 3.1


Este tutorial usa o ASP.NET Web API 2 com o Entity Framework 6 para criar um aplicativo web que manipula um banco de dados de back-end. Aqui está uma captura de tela do aplicativo que você criará.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

O aplicativo usa um design de aplicativo de página única (SPA). "Aplicativo de página única" é o termo geral para um aplicativo web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar as novas páginas. Após o carregamento de página inicial, o aplicativo se comunica com o servidor por meio de solicitações do AJAX. O AJAX solicitações retornar dados JSON, que o aplicativo usa para atualizar a interface do usuário.

AJAX não é novo, mas atualmente há estruturas de JavaScript que tornam mais fácil de criar e manter um grande aplicativo SPA sofisticado. Este tutorial usa [Knockout. js](http://knockoutjs.com/), mas você pode usar qualquer estrutura de cliente JavaScript.

Aqui estão os principais blocos de construção para este aplicativo:

- ASP.NET MVC cria a página HTML.
- ASP.NET Web API trata as solicitações do AJAX e retorna dados JSON.
- Knockout. js dados-associa os elementos HTML para os dados JSON.
- Entity Framework se comunica com o banco de dados.

## <a name="see-this-app-running-on-azure"></a>Consulte este aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo web em tempo real? Você pode implantar uma versão completa do aplicativo para sua conta do Azure, clicando no botão a seguir.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você não tiver uma conta, você tem as seguintes opções:

- [Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos você pode usar para experimentar os serviços do Azure pagos e mesmo depois que eles são usados até que você pode manter a conta e livre de usar os serviços do Azure.
- [Ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -assinatura do MSDN fornece créditos de cada mês em que você pode usar para os serviços do Azure pagos.

## <a name="create-the-project"></a>Criar o projeto

Abra o Visual Studio. Do **arquivo** menu, selecione **novo**, em seguida, selecione **projeto**. (Ou clique em **novo projeto** na página inicial.)

No **novo projeto** caixa de diálogo, clique em **Web** no painel esquerdo e **aplicativo Web ASP.NET** no painel central. Nomeie o projeto BookService e clique em **Okey**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

No **novo projeto ASP.NET** caixa de diálogo, selecione o **API da Web** modelo.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Se você deseja hospedar o projeto em um serviço de aplicativo do Azure, deixe o **Host na nuvem** caixa marcada.

Clique em **Okey** para criar o projeto.

## <a name="configure-azure-settings-optional"></a>Definir configurações do Azure (opcionais)

Se você deixou o **Host na nuvem** opção marcada, o Visual Studio será solicitado a entrar no Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Depois de entrar Azure, o Visual Studio solicitará que você configurar o aplicativo web. Insira um nome para o site, selecione sua assinatura do Azure e selecione uma região geográfica. Em **o servidor de banco de dados**, selecione **criar novo servidor**. Insira um nome de usuário administrador e uma senha.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[Avançar](part-2.md)
