---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Criando um controlador (VB) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode adicionar um controlador para um aplicativo ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: e9a2bbcb09672f5247429064908cd4d2ef67f518
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879004"
---
<a name="creating-a-controller-vb"></a>Criando um controlador (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther demonstra como você pode adicionar um controlador para um aplicativo ASP.NET MVC.


O objetivo deste tutorial é explicar como você pode criar novos ASP.NET MVC controladores. Você aprenderá a criar controladores usando a opção de menu do Visual Studio Adicionar controlador e criando um arquivo de classe manualmente.

### <a name="using-the-add-controller-menu-option"></a>Usando o adicionar a opção de Menu do controlador

É a maneira mais fácil de criar um novo controlador para a pasta de controladores na janela do Gerenciador de soluções do Visual Studio e selecione o **adicionar, controlador** opção de menu (consulte a Figura 1). Selecionar essa opção de menu abre o **Adicionar controlador** caixa de diálogo (consulte a Figura 2).


[![A caixa de diálogo Novo projeto](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Figura 01**: adicionando um novo controlador ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image2.png))


[![A caixa de diálogo Novo projeto](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Figura 02**: caixa de diálogo a adicionar controlador ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image4.png))


Observe que a primeira parte do nome do controlador está realçada no **Adicionar controlador** caixa de diálogo. Cada nome de controlador deve terminar com o sufixo *controlador*. Por exemplo, você pode criar um controlador nomeado *ProductController* , mas não um controlador chamado *produto*.


Se você criar um controlador que está faltando o *controlador* sufixo, em seguida, você não poderá invocar o controlador. Não faça isso - desperdício de frustração da minha vida depois de fazer esse erro.


**Listando 1 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Você sempre deve criar os controladores na pasta controladores. Caso contrário, você irá violar as convenções do ASP.NET MVC e outros desenvolvedores terão mais dificuldades Noções básicas sobre o seu aplicativo.

### <a name="scaffolding-action-methods"></a>Métodos de ação de scaffolding

Quando você cria um controlador, você tem a opção para gerar automaticamente o métodos de ação de criar, atualizar e detalhes (consulte a Figura 3). Se você selecionar essa opção, em seguida, a classe do controlador na listagem 2 é gerada.


[![Criação automática de métodos de ação](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Figura 03**: criação automática de métodos de ação ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image6.png))


**A listagem 2 - Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Esses métodos gerados são os métodos de stub. Você deve adicionar a lógica real para criar, atualizar e mostrando detalhes de um cliente. Porém, os métodos de stub fornecem um bom ponto de partida.

### <a name="creating-a-controller-class"></a>Criando uma classe de controlador

Controlador MVC do ASP.NET é apenas uma classe. Se preferir, você pode ignorar o scaffolding de controlador conveniente do Visual Studio e crie uma classe de controlador manualmente. Siga estas etapas:

1. Clique na pasta controladores e selecione a opção de menu **adicionar, o novo Item** e selecione o **classe** modelo (consulte a Figura 4).
2. Nomeie a nova classe PersonController.vb e clique no **adicionar** botão.
3. Modifique o arquivo de classe resultante para que a classe herda da classe Controller base (consulte a listagem 3).


[![Criando uma nova classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Figura 04**: Criando uma nova classe ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image8.png))


**A listagem 3 - Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

O controlador na listagem 3 expõe uma ação chamada index () que retorna a cadeia de caracteres "Hello World!". Você pode chamar a ação de controlador executando seu aplicativo e solicitar uma URL semelhante ao seguinte:

`http://localhost:40071/Person`

> [!NOTE]
> 
> O servidor de desenvolvimento ASP.NET usa um número de porta aleatória (por exemplo, 40071). Ao inserir uma URL para invocar um controlador, você precisará fornecer o número de porta correto. Você pode determinar o número da porta passando o mouse sobre o ícone para o servidor de desenvolvimento ASP.NET na área de notificação do Windows (inferior direito da tela).
> 
> [!div class="step-by-step"]
> [Anterior](adding-dynamic-content-to-a-cached-page-vb.md)
> [Próximo](creating-an-action-vb.md)
