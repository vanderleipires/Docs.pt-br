---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Usando a API da Web com Web Forms do ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26536075"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Usando a API da Web com Web Forms do ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Embora a API da Web do ASP.NET é empacotado com o ASP.NET MVC, é fácil adicionar API da Web a um aplicativo de Web Forms do ASP.NET tradicional. Este tutorial orienta você pelas etapas.

## <a name="overview"></a>Visão Geral

Para usar a API da Web em um aplicativo de Web Forms, há duas etapas principais:

- Adicionar um controlador de API da Web que deriva de **ApiController** classe.
- Adicionar uma tabela de rota para o **aplicativo\_iniciar** método.

## <a name="create-a-web-forms-project"></a>Criar um projeto de formulários da Web

Inicie o Visual Studio e selecione **novo projeto** do **iniciar** página. Ou, do **arquivo** menu, selecione **novo** e **projeto**.

No **modelos** painel, selecione **modelos instalados** e expanda o **Visual C#** nó. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo ASP.NET Web Forms**. Insira um nome para o projeto e clique em **Okey**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Criar o modelo e o controlador

Este tutorial usa as mesmas classes de modelo e o controlador como o [Introdução](tutorial-your-first-web-api.md) tutorial.

Primeiro, adicione uma classe de modelo. Em **Solution Explorer**, clique com o botão direito e selecione **Adicionar classe**. Nomeie a classe de produto e, em seguida, adicione a implementação a seguir:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Em seguida, adicione um controlador Web API para o projeto, um *controlador* é o objeto que manipula solicitações HTTP para a API da Web.

Em **Solution Explorer**, clique com o botão direito. Selecione **Adicionar Novo Item**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

Em **modelos instalados**, expanda **Visual C#** e selecione **Web**. Em seguida, na lista de modelos, selecione **classe do controlador Web API**. Nome do controlador "ProductsController" e clique em **adicionar**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

O **Adicionar Novo Item** assistente criará um arquivo chamado ProductsController.cs. Excluir os métodos que o assistente incluído e adicione os seguintes métodos:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Para obter mais informações sobre o código neste controlador, consulte o [Introdução](tutorial-your-first-web-api.md) tutorial.

## <a name="add-routing-information"></a>Adicionar informações de roteamento

Em seguida, vamos adicionar uma rota URI assim que URIs do formulário &quot;/api/produtos/&quot; são roteadas para o controlador.

Em **Solution Explorer**, clique duas vezes em global. asax para abrir o arquivo de code-behind asax. Adicione o seguinte **usando** instrução.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Em seguida, adicione o seguinte código para o **aplicativo\_iniciar** método:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Para obter mais informações sobre tabelas de roteamento, consulte [roteamento na API da Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Adicionar cliente AJAX

Isso é tudo o que você precisa criar uma API da web do que os clientes podem acessar. Agora vamos adicionar uma página HTML que usa o jQuery para chamar a API.

Verifique se a página mestra (por exemplo, *Site.Master*) inclui um `ContentPlaceHolder` com `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Abra o arquivo default. aspx. Substitua o texto de boilerplate que está na seção de conteúdo principal, conforme mostrado:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Em seguida, adicione uma referência ao arquivo de origem jQuery no `HeaderContent` seção:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Observação: Você pode facilmente adicionar referência de script arrastando e soltando o arquivo de **Solution Explorer** na janela do editor de código.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Abaixo da marca de script jQuery, adicione o seguinte bloco de script:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Quando o documento for carregado, esse script faz uma solicitação AJAX para &quot;api/produtos&quot;. A solicitação retorna uma lista de produtos no formato JSON. O script adiciona as informações de produto para a tabela HTML.

Quando você executa o aplicativo, ele deve ser assim:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
