---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Criando um controlador (VB) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode adicionar um controlador a um aplicativo ASP.NET MVC.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: b24aeabc6fd4192f44f83e0c0b2150e87ef6f501
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833057"
---
<a name="creating-a-controller-vb"></a>Criando um controlador (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther demonstra como você pode adicionar um controlador a um aplicativo ASP.NET MVC.


O objetivo deste tutorial é explicar como você pode criar novas ASP.NET MVC controladores. Você aprenderá a criar controladores usando a opção de menu do Visual Studio Adicionar controlador e criando um arquivo de classe manualmente.

### <a name="using-the-add-controller-menu-option"></a>Usando o adicionar a opção de Menu do controlador

A maneira mais fácil de criar um novo controlador é com o botão direito na pasta controladores na janela do Gerenciador de soluções do Visual Studio e selecione o **Add, controlador** opção de menu (veja a Figura 1). Selecionar essa opção de menu abre a **Adicionar controlador** caixa de diálogo (consulte a Figura 2).


[![A caixa de diálogo Novo projeto](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Figura 01**: adicionar um novo controlador ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image2.png))


[![A caixa de diálogo Novo projeto](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Figura 02**: caixa de diálogo o adicionar controlador ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image4.png))


Observe que a primeira parte do nome do controlador está realçada na **Adicionar controlador** caixa de diálogo. Cada nome de controlador deve terminar com o sufixo *controlador*. Por exemplo, você pode criar um controlador chamado *ProductController* , mas não um controlador chamado *produto*.


Se você criar um controlador que está faltando a *controlador* sufixo e em seguida, você não poderá invocar o controlador. Não fizer isso, eu já desperdiçado incontáveis horas da minha vida depois de cometer esse erro.


**Listagem 1 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Você deve sempre criar controladores na pasta controladores. Caso contrário, você vai ser violar as convenções do ASP.NET MVC e outros desenvolvedores terá mais dificuldade Noções básicas sobre seu aplicativo.

### <a name="scaffolding-action-methods"></a>Métodos de ação de scaffolding

Quando você cria um controlador, você tem a opção para gerar os métodos de ação de criar, atualizar e detalhes automaticamente (veja a Figura 3). Se você selecionar essa opção, em seguida, a classe do controlador na listagem 2 é gerada.


[![Criação automática de métodos de ação](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Figura 03**: a criação automática de métodos de ação ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image6.png))


**Listagem 2 - Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Esses métodos gerados são métodos stub. Você deve adicionar a lógica real para criar, atualizar e mostrando detalhes de um cliente. Porém, os métodos stub fornecem um bom ponto de partida.

### <a name="creating-a-controller-class"></a>Criando uma classe de controlador

O controlador MVC do ASP.NET é apenas uma classe. Se você preferir, você pode ignorar o scaffolding de controlador conveniente do Visual Studio e criar uma classe de controlador manualmente. Siga estas etapas:

1. Clique com botão direito na pasta controladores e selecione a opção de menu **Add, o novo Item** e selecione o **classe** modelo (consulte a Figura 4).
2. Nomeie a nova classe PersonController.vb e clique no **adicionar** botão.
3. Modifique o arquivo de classe resultante para que a classe herda da classe Controller base (consulte a listagem 3).


[![Criando uma nova classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Figura 04**: criar uma nova classe ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image8.png))


**Listagem 3 - Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

O controlador na listagem 3 expõe uma ação chamada index () que retorna a cadeia de caracteres "Hello World!". Você pode chamar essa ação de controlador, executando o aplicativo e solicitar uma URL semelhante à seguinte:

`http://localhost:40071/Person`

> [!NOTE]
> 
> O ASP.NET Development Server usa um número de porta aleatória (por exemplo, 40071). Ao inserir uma URL para invocar um controlador, você precisará fornecer o número da porta à direita. Você pode passar o mouse sobre o ícone para o ASP.NET Development Server na área de notificação do Windows (canto inferior direito da tela) para determinar o número da porta.
> 
> [!div class="step-by-step"]
> [Anterior](adding-dynamic-content-to-a-cached-page-vb.md)
> [Próximo](creating-an-action-vb.md)
