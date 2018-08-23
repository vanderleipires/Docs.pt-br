---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Criar uma restrição de rota (c#) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita as rotas de correspondência, criando restrições de rota com expressões regulares.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51d76248a59968e1d6befd5d5404f7bf6c2878e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831663"
---
<a name="creating-a-route-constraint-c"></a>Criar uma restrição de rota (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita as rotas de correspondência, criando restrições de rota com expressões regulares.


Você pode usar restrições de rota para restringir as solicitações de navegador que correspondam a uma rota específica. Você pode usar uma expressão regular para especificar uma restrição de rota.

Por exemplo, imagine que você definiu a rota na listagem 1 no arquivo global. asax.

**Listagem 1 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Listagem 1 contém uma rota chamada Product. Você pode usar a rota de produto para mapear as solicitações do navegador para ProductController contida na listagem 2.

**Listagem 2 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Observe que a ação de Details() exposta pelo controlador de produto aceita um parâmetro único chamado productId. Esse parâmetro é um parâmetro de número inteiro.

A rota definida na listagem 1 corresponderá a qualquer uma das seguintes URLs:

- / Produto/23
- / Produto/7

Infelizmente, a rota também serão compatíveis com as seguintes URLs:

- / Produto/blah
- / Produto/apple

Porque a ação Details() espera um parâmetro de número inteiro, fazendo uma solicitação que contém algo diferente de um valor inteiro causará um erro. Por exemplo, se você digitar a URL /Product/apple no seu navegador, em seguida, você obterá a página de erro na Figura 1.


[![A caixa de diálogo Novo projeto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Figura 01**: vendo uma página explodir ([clique para exibir a imagem em tamanho normal](creating-a-route-constraint-cs/_static/image2.png))


O que você realmente deseja fazer é correspondente somente a URLs que contêm um productId inteiro apropriado. Você pode usar uma restrição ao definir uma rota para restringir as URLs que correspondem à rota. A rota de produto modificada na listagem 3 contém uma restrição de expressão regular que corresponde apenas números inteiros.

**Listagem 3 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

A expressão regular \d+ corresponde a um ou mais números inteiros. Essa restrição faz com que a rota de produto coincidir com as seguintes URLs:

- / Produto/3
- / Produto/8999

Mas não as URLs a seguir:

- / Produto/apple
- / Produto

- Essas solicitações do navegador serão tratadas por outra rota ou, se não houver rotas correspondentes, uma *não foi possível encontrar o recurso* erro será retornado.

> [!div class="step-by-step"]
> [Anterior](creating-custom-routes-cs.md)
> [Próximo](creating-a-custom-route-constraint-cs.md)
