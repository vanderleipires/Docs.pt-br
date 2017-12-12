---
uid: mvc/overview/getting-started/introduction/getting-started
title: "Introdução ao ASP.NET MVC 5 | Microsoft Docs"
author: Rick-Anderson
description: "Observação: Uma versão atualizada deste tutorial está disponível aqui usando o Visual Studio 2015. O novo tutorial usa o ASP.NET Core MVC 6, que fornece muitos improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 8f2cb9b3f14ad95acd9e4c7a0ed26228d3c480e0
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2017
---
<a name="getting-started-with-aspnet-mvc-5"></a>Introdução ao ASP.NET MVC 5
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 Este tutorial ensina as Noções básicas de criação de um aplicativo de web de ASP.NET MVC 5 usando [2017 do Visual Studio](https://www.visualstudio.com/). Fonte final tutorial localizada no [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)
 
 
 Este tutorial foi escrito por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )
 
 Você precisa de uma conta do Azure para implantar esse aplicativo no Azure:
 
 - Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos você pode usar para experimentar os serviços do Azure pagos e mesmo depois que eles são usados até que você pode manter a conta e livre de usar os serviços do Azure.
 - Você pode [ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -assinatura do MSDN fornece créditos de cada mês em que você pode usar para os serviços do Azure pagos.


## <a name="getting-started"></a>Guia de Introdução

Comece instalando e executando [2017 do Visual Studio](https://www.visualstudio.com/).

O Visual Studio é um IDE ou ambiente de desenvolvimento integrado. Como usar o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos. No Visual Studio, há uma lista na parte inferior mostra várias opções disponíveis para você. Há também um menu que oferece uma maneira de realizar tarefas no IDE. (Por exemplo, em vez de selecionar **novo projeto** do **iniciar** página, você pode usar o menu e selecione **arquivo** &gt; **denovoprojeto**.)

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Clique em **novo projeto**, em seguida, selecione Visual c# à esquerda, em seguida, **Web** e, em seguida, selecione **o aplicativo Web do ASP.NET (.NET Framework)**. Nomeie o projeto "MvcMovie" e, em seguida, clique em **Okey**.

![](getting-started/_static/image2.png)

No **novo projeto ASP.NET** caixa de diálogo, clique em **MVC** e, em seguida, clique em **Okey**.

![](getting-started/_static/image3.png)

O Visual Studio usado um modelo padrão para o projeto ASP.NET MVC que você acabou de criar, para que você tenha um aplicativo em execução no momento sem fazer nada! Este é um simples "Hello World!" projeto e ele á um bom lugar para iniciar seu aplicativo.

![](getting-started/_static/image4.png)

Clique em F5 para iniciar a depuração. F5 faz com que o Visual Studio para iniciar [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) e executar seu aplicativo web. Visual Studio, em seguida, inicia um navegador e abre a página inicial do aplicativo. Observe que a barra de endereço do navegador informa `localhost:port#` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso, é executado o aplicativo que você acabou de criar. Quando o Visual Studio executará um projeto da web, uma porta aleatória é usada para o servidor web. Na imagem abaixo, o número da porta for 1234. Quando você executa o aplicativo, você verá um número de porta diferente.

![](getting-started/_static/image5.png)

Imediatamente esse modelo padrão fornece páginas Home, contatos e sobre. A imagem acima não mostra o **início**, **sobre** e **contato** links. Dependendo do tamanho da janela do navegador, você talvez seja necessário clicar no ícone de navegação para ver esses links.

![](getting-started/_static/image6.png)  

O aplicativo também fornece suporte para registrar e entrar. A próxima etapa é alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC. Feche o aplicativo ASP.NET MVC e vamos alterar o código.

Para obter uma lista de tutoriais atuais, consulte [MVC artigos recomendado](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Consulte este aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo web em tempo real? Você pode implantar uma versão completa do aplicativo para sua conta do Azure, clicando no botão a seguir.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você não tiver uma conta, você tem as seguintes opções:

- [Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos você pode usar para experimentar os serviços do Azure pagos e mesmo depois que eles são usados até que você pode manter a conta e livre de usar os serviços do Azure.
- [Ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -assinatura do MSDN fornece créditos de cada mês em que você pode usar para os serviços do Azure pagos.

>[!div class="step-by-step"]
[Avançar](adding-a-controller.md)
