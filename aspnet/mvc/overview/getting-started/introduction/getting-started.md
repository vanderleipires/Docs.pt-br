---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introdução ao ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2cc9364b815cae0207fc59784303c6a0906f1b94
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578439"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Introdução ao ASP.NET MVC 5
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Este tutorial ensina as Noções básicas da criação de um aplicativo de web de ASP.NET MVC 5 usando [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). O código-fonte final para o tutorial está localizado em [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Este tutorial foi escrito por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

Você precisa de uma conta do Azure para implantar esse aplicativo no Azure:

- Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.
- Você pode [ativar benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN concede créditos todos os meses que você pode usar para serviços pagos do Azure.

## <a name="get-started"></a>Introdução

Comece [instalar o Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Em seguida, abra o Visual Studio.

Visual Studio é um IDE ou ambiente de desenvolvimento integrado. Assim como usar o Microsoft Word para gravar documentos, você usará um IDE para criar aplicativos. No Visual Studio, há uma lista na parte inferior, mostrando várias opções disponíveis para você. Há também um menu que fornece outra maneira de executar tarefas no IDE. Por exemplo, em vez de selecionar **novo projeto** sobre o **página inicial**, você pode usar a barra de menus e selecione **arquivo** > **denovoprojeto**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Criar seu primeiro aplicativo

Sobre o **página inicial**, selecione **novo projeto**. No **novo projeto** caixa de diálogo, selecione o **Visual c#** categoria à esquerda, em seguida, **Web**e, em seguida, selecione o **ASP.NET Web Application (.NET Framework)**  modelo de projeto. Nomeie o projeto "como MvcMovie" e, em seguida, escolha **Okey**.

![](getting-started/_static/image2.png)

No **novo aplicativo Web ASP.NET** caixa de diálogo, escolha **MVC** e, em seguida, escolha **Okey**.

![](getting-started/_static/image3.png)

Visual Studio usou um modelo padrão para o projeto do ASP.NET MVC que você acabou de criar, portanto, você tem agora um aplicativo de trabalho sem fazer nada! Isso é um simples "Hello World!" projeto e ele á um bom lugar para iniciar o aplicativo.

![](getting-started/_static/image4.png)

Pressione **F5** para iniciar a depuração. Quando você pressiona **F5**, o Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) e executa o aplicativo web. Em seguida, o Visual Studio inicia um navegador e abre a página inicial do aplicativo. Observe que a barra de endereços do navegador diz `localhost:port#` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso está executando o aplicativo que você acabou de criar. Quando o Visual Studio executa um projeto da web, uma porta aleatória é usada para o servidor web. Na imagem abaixo, o número da porta for 1234. Quando você executa o aplicativo, você verá um número de porta diferente.

![](getting-started/_static/image5.png)

Prontos neste modelo padrão fornece `Home`, `Contact`, e `About` páginas. A imagem acima não mostra a **página inicial**, **sobre**, e **contato** links. Dependendo do tamanho da janela do navegador, você talvez seja necessário clicar no ícone de navegação para ver esses links.

![](getting-started/_static/image6.png)

O aplicativo também fornece suporte para se registrar e fazer logon. A próxima etapa é alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC. Feche o aplicativo ASP.NET MVC e vamos alterar algum código.

Para obter uma lista de tutoriais atuais, consulte [MVC artigos recomendado](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Consulte esse aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo web ao vivo? Você pode implantar uma versão completa do aplicativo para sua conta do Azure, simplesmente clicando no botão a seguir.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você ainda não tiver uma conta, use uma das opções a seguir para criar um:

- [Abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.
- [Ativar benefícios de assinante do Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) -assinatura do seu Visual Studio concede créditos todos os meses que você pode usar para serviços pagos do Azure.

> [!div class="step-by-step"]
> [Avançar](adding-a-controller.md)
