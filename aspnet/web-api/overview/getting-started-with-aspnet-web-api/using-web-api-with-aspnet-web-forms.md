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
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: 91811031b937d3ca05888ce8d4f28bcc33537c76
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400873"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Usando a API da Web com Web Forms do ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Embora a API Web ASP.NET é empacotada com o ASP.NET MVC, é fácil adicionar a API da Web a um aplicativo Web Forms do ASP.NET tradicional. Este tutorial orienta você pelas etapas.

## <a name="overview"></a>Visão geral

Para usar a API da Web em um aplicativo Web Forms, há duas etapas principais:

- Adicionar um controlador de API da Web que deriva de **ApiController** classe.
- Adicionar uma tabela de rotas para o **Application\_iniciar** método.

## <a name="create-a-web-forms-project"></a>Criar um projeto de formulários da Web

Inicie o Visual Studio e selecione **Novo projeto** na página **Iniciar**. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo do ASP.NET Web Forms**. Insira um nome para o projeto e clique em **Okey**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Criar o modelo e o controlador

Este tutorial usa as mesmas classes de modelo e controlador como o [guia de Introdução](tutorial-your-first-web-api.md) tutorial.

Primeiro, adicione uma classe de modelo. Na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Adicionar classe**. Nomeie a classe Product e, em seguida, adicione a seguinte implementação:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Em seguida, adicione um controlador de API da Web ao projeto., uma *controlador* é o objeto que manipula as solicitações HTTP para a API da Web.

Na **Gerenciador de soluções**, clique com botão direito no projeto. Selecione **Adicionar Novo Item**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

Sob **modelos instalados**, expanda **Visual c#** e selecione **Web**. Em seguida, na lista de modelos, selecione **Web API Controller Class**. Nomeie o controlador "ProductsController" e clique em **adicionar**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

O **Adicionar Novo Item** assistente criará um arquivo chamado ProductsController.cs. Exclua os métodos que o assistente incluído e adicione os seguintes métodos:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Para obter mais informações sobre o código neste controlador, consulte o [guia de Introdução](tutorial-your-first-web-api.md) tutorial.

## <a name="add-routing-information"></a>Adicionar informações de roteamento

Em seguida, vamos adicionar uma rota URI assim que URIs do formulário &quot;/API/produtos/&quot; são roteadas para o controlador.

Na **Gerenciador de soluções**, clique duas vezes em global. asax para abrir o arquivo code-behind Global.asax.cs. Adicione o seguinte **usando** instrução.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Em seguida, adicione o seguinte código para o **Application\_iniciar** método:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Para obter mais informações sobre tabelas de roteamento, consulte [roteamento na API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Adicionar o AJAX do lado do cliente

Isso é tudo o que você precisa criar uma API web que os clientes podem acessar. Agora vamos adicionar uma página HTML que usa o jQuery para chamar a API.

Verifique se a página mestra (por exemplo, *Master*) inclui um `ContentPlaceHolder` com `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Abra o arquivo default. aspx. Substitua o texto clichê que está na seção de conteúdo principal, conforme mostrado:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Em seguida, adicione uma referência para o arquivo de origem do jQuery no `HeaderContent` seção:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Observação: Você pode adicionar facilmente a referência de script arrastando e soltando o arquivo a partir **Gerenciador de soluções** na janela do editor de código.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Abaixo da marca de script do jQuery, adicione o seguinte bloco de script:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Quando o documento for carregado, esse script faz uma solicitação AJAX ao &quot;api/produtos&quot;. A solicitação retorna uma lista de produtos no formato JSON. O script adiciona as informações do produto na tabela de HTML.

Quando você executa o aplicativo, ele deve ser assim:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
