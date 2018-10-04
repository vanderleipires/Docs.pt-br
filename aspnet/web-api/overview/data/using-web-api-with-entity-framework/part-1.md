---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usando a API Web 2 com o Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial ensinará os fundamentos da criação de um aplicativo web com uma API Web ASP.NET de back-end. O tutorial usa o Entity Framework 6 para o layout de dados...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: d65c0ea35ec766ef9d9093c6502230f9de72a3f3
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795202"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Usando a API Web 2 com o Entity Framework 6
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](https://github.com/MikeWasson/BookService)

> Este tutorial ensinará os fundamentos da criação de um aplicativo web com uma API Web ASP.NET de back-end. O tutorial usa o Entity Framework 6 para a camada de dados e o Knockout. js para o aplicativo de JavaScript do lado do cliente. O tutorial também mostra como implantar o aplicativo em aplicativos de Web do serviço de aplicativo do Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - API Web 2.1
> - Visual Studio 2013 (Baixe o Visual Studio 2017 [aqui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
> - [Knockout. js](http://knockoutjs.com/) 3.1

Este tutorial usa a API Web ASP.NET 2 com o Entity Framework 6 para criar um aplicativo web que manipula um banco de dados de back-end. Aqui está uma captura de tela do aplicativo que você criará.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

O aplicativo usa um design de aplicativo de página única (SPA). "Aplicativo de página única" é o termo geral para um aplicativo web que carrega uma página HTML única e, em seguida, atualiza a página dinamicamente, em vez de carregar novas páginas. Após o carregamento de página inicial, o aplicativo se comunica com o servidor por meio de solicitações AJAX. O AJAX solicita dados JSON retornados, o que o aplicativo usa para atualizar a interface do usuário.

AJAX não é novo, mas hoje, há estruturas de JavaScript que facilitam criar e manter um grande aplicativo SPA sofisticado. Este tutorial usa [Knockout. js](http://knockoutjs.com/), mas você pode usar qualquer estrutura de cliente JavaScript.

Aqui estão os principais blocos de construção para este aplicativo:

- ASP.NET MVC cria a página HTML.
- API Web ASP.NET manipula as solicitações AJAX e retorna dados JSON.
- Knockout. js-associa dados os elementos HTML para os dados JSON.
- Entity Framework se comunica com o banco de dados.

## <a name="see-this-app-running-on-azure"></a>Consulte esse aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo web ao vivo? Você pode implantar uma versão completa do aplicativo para sua conta do Azure, simplesmente clicando no botão a seguir.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você ainda não tiver uma conta, você tem as seguintes opções:

- [Abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.
- [Ativar benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN concede créditos todos os meses que você pode usar para serviços pagos do Azure.

## <a name="create-the-project"></a>Criar o projeto

Abra o Visual Studio. Dos **arquivo** menu, selecione **New**, em seguida, selecione **projeto**. (Ou clique em **novo projeto** na página inicial.)

No **novo projeto** caixa de diálogo, clique em **Web** no painel esquerdo e **aplicativo Web ASP.NET** no painel central. Nomeie o projeto BookService e clique em **Okey**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

No **novo projeto ASP.NET** caixa de diálogo, selecione o **API da Web** modelo.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Se você quiser hospedar o projeto em um serviço de aplicativo do Azure, deixe o **hospedar na nuvem** caixa marcada.

Clique em **OK** para criar o projeto.

## <a name="configure-azure-settings-optional"></a>Definir configurações do Azure (opcional)

Se você deixou a **Host na nuvem** opção marcada, o Visual Studio solicitará que você entrar no Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Depois de entrar Azure, o Visual Studio solicita que você configurar o aplicativo web. Insira um nome para o site, selecione sua assinatura do Azure e selecione uma região geográfica. Sob **servidor de banco de dados**, selecione **criar novo servidor**. Insira um nome de usuário administrador e senha.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Avançar](part-2.md)
