---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Criar uma restrição de rota (VB) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita as rotas de correspondência, criando restrições de rota com expressões regulares.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 74afb5653f01a5291b77da1bc672b362fec118a0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364025"
---
<a name="creating-a-route-constraint-vb"></a>Criar uma restrição de rota (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita as rotas de correspondência, criando restrições de rota com expressões regulares.


Você pode usar restrições de rota para restringir as solicitações de navegador que correspondam a uma rota específica. Você pode usar uma expressão regular para especificar uma restrição de rota.

Por exemplo, imagine que você definiu a rota na listagem 1 no arquivo global. asax.

**Listagem 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Listagem 1 contém uma rota chamada Product. Você pode usar a rota de produto para mapear as solicitações do navegador para ProductController contida na listagem 2.

**Listagem 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Observe que a ação de Details() exposta pelo controlador de produto aceita um parâmetro único chamado productId. Esse parâmetro é um parâmetro de número inteiro.

A rota definida na listagem 1 corresponderá a qualquer uma das seguintes URLs:

- / Produto/23
- / Produto/7

Infelizmente, a rota também serão compatíveis com as seguintes URLs:

- / Produto/blah
- / Produto/apple

Porque a ação Details() espera um parâmetro de número inteiro, fazendo uma solicitação que contém algo diferente de um valor inteiro causará um erro. Por exemplo, se você digitar a URL /Product/apple no seu navegador, em seguida, você obterá a página de erro na Figura 1.


[![A caixa de diálogo Novo projeto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figura 01**: vendo uma página explodir ([clique para exibir a imagem em tamanho normal](creating-a-route-constraint-vb/_static/image2.png))


O que você realmente deseja fazer é correspondente somente a URLs que contêm um productId inteiro apropriado. Você pode usar uma restrição ao definir uma rota para restringir as URLs que correspondem à rota. A rota de produto modificada na listagem 3 contém uma restrição de expressão regular que corresponde apenas números inteiros.

**Listagem 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

A expressão regular \d+ corresponde a um ou mais números inteiros. Essa restrição faz com que a rota de produto coincidir com as seguintes URLs:

- / Produto/3
- / Produto/8999

Mas não as URLs a seguir:

- / Produto/apple
- / Produto

Essas solicitações do navegador serão tratadas por outra rota ou, se não houver rotas correspondentes, uma *não foi possível encontrar o recurso* erro será retornado.

> [!div class="step-by-step"]
> [Anterior](creating-custom-routes-vb.md)
> [Próximo](creating-a-custom-route-constraint-vb.md)
