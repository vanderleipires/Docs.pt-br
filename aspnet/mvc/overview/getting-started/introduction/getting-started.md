---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introdução ao ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui usando o Visual Studio 2015. O novo tutorial usa o ASP.NET Core MVC 6, que fornece muitos improvem...'
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211742"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Introdução ao ASP.NET MVC 5
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Este tutorial lhe ensinará os conceitos básicos da criação de um aplicativo de web de ASP.NET MVC 5 usando [Visual Studio 2017](https://www.visualstudio.com/). Fonte final para o tutorial localizada [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 Este tutorial foi escrito por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Você precisa de uma conta do Azure para implantar esse aplicativo no Azure:

 - Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.
 - Você pode [ativar benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN concede créditos todos os meses que você pode usar para serviços pagos do Azure.


## <a name="getting-started"></a>Guia de Introdução

Comece instalando e executando [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio é um IDE ou ambiente de desenvolvimento integrado. Assim como usar o Microsoft Word para gravar documentos, você usará um IDE para criar aplicativos. No Visual Studio, há uma lista na parte inferior, mostrando várias opções disponíveis para você. Há também um menu que fornece outra maneira de executar tarefas no IDE. (Por exemplo, em vez de selecionar **novo projeto** da **inicie** página, você pode usar o menu e selecione **arquivo** &gt; **denovoprojeto**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Clique em **novo projeto**, em seguida, selecione Visual c# à esquerda, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.NET (.NET Framework)**. Nomeie o projeto "como MvcMovie" e, em seguida, clique em **Okey**.

![](getting-started/_static/image2.png)

No **novo projeto ASP.NET** caixa de diálogo, clique em **MVC** e, em seguida, clique em **Okey**.

![](getting-started/_static/image3.png)

Visual Studio usou um modelo padrão para o projeto do ASP.NET MVC que você acabou de criar, portanto, você tem agora um aplicativo de trabalho sem fazer nada! Isso é um simples "Hello World!" projeto e ele á um bom lugar para iniciar o aplicativo.

![](getting-started/_static/image4.png)

Clique em F5 para iniciar a depuração. F5 faz com que o Visual Studio para iniciar [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) e executar seu aplicativo web. Em seguida, o Visual Studio inicia um navegador e abre a página inicial do aplicativo. Observe que a barra de endereços do navegador diz `localhost:port#` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso está executando o aplicativo que você acabou de criar. Quando o Visual Studio executa um projeto da web, uma porta aleatória é usada para o servidor web. Na imagem abaixo, o número da porta for 1234. Quando você executa o aplicativo, você verá um número de porta diferente.

![](getting-started/_static/image5.png)

Prontos nesse modelo padrão fornece páginas Home, contato e sobre. A imagem acima não mostra a **página inicial**, **sobre** e **contato** links. Dependendo do tamanho da janela do navegador, você talvez seja necessário clicar no ícone de navegação para ver esses links.

![](getting-started/_static/image6.png)  

O aplicativo também fornece suporte para se registrar e fazer logon. A próxima etapa é alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC. Feche o aplicativo ASP.NET MVC e vamos alterar algum código.

Para obter uma lista de tutoriais atuais, consulte [MVC artigos recomendado](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Consulte esse aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo web ao vivo? Você pode implantar uma versão completa do aplicativo para sua conta do Azure, simplesmente clicando no botão a seguir.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você ainda não tiver uma conta, você tem as seguintes opções:

- [Abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.
- [Ativar benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN concede créditos todos os meses que você pode usar para serviços pagos do Azure.

> [!div class="step-by-step"]
> [Avançar](adding-a-controller.md)
