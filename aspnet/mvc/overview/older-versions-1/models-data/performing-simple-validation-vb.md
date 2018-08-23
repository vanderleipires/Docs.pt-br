---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Realizar validação simples (VB) | Microsoft Docs
author: StephenWalther
description: Saiba como executar a validação em um aplicativo ASP.NET MVC. Neste tutorial, Stephen Walther apresenta você ao estado de modelo e o auxiliar de validação HTML...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d0bd6917bab61b17d1cafcf0cd9eb1983275dc8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833766"
---
<a name="performing-simple-validation-vb"></a>Realizar validação simples (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Saiba como executar a validação em um aplicativo ASP.NET MVC. Neste tutorial, Stephen Walther apresenta você ao estado de modelo e os auxiliares de validação HTML.


O objetivo deste tutorial é explicar como você pode executar a validação dentro de um aplicativo ASP.NET MVC. Por exemplo, você aprenderá a evitar que alguém envie um formulário que não contém um valor para um campo obrigatório. Você aprenderá a usar o estado de modelo e os auxiliares HTML de validação.

## <a name="understanding-model-state"></a>Compreendendo o estado do modelo

Use o estado do modelo - ou mais precisamente, o dicionário de estado de modelo - para representar erros de validação. Por exemplo, a ação Create () na listagem 1 valida as propriedades de uma classe de produto antes de adicionar a classe de produto a um banco de dados.


Eu não estou recomendando que você adicione sua lógica de validação ou o banco de dados a um controlador. Um controlador deve conter apenas a lógica relacionada ao controle de fluxo do aplicativo. Estamos realizando um atalho para manter as coisas simples.


**Listagem 1 - Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Na listagem 1, as propriedades de nome, descrição e UnitsInStock da classe Product são validadas. Se qualquer uma dessas propriedades realizar um teste de validação um erro será adicionado ao dicionário de estado de modelo (representado pela propriedade ModelState da classe Controller).

Se houver erros no estado de modelo, em seguida, a propriedade ModelState retornará false. Nesse caso, o formulário HTML para a criação de um novo produto é exibida novamente. Caso contrário, se não houver nenhum erro de validação, o novo produto é adicionado ao banco de dados.

## <a name="using-the-validation-helpers"></a>Usando os auxiliares de validação

O ASP.NET MVC framework inclui dois auxiliares de validação: o auxiliar de Html.ValidationMessage() e o auxiliar Html.ValidationSummary(). Você pode usar esses dois auxiliares em uma exibição para exibir mensagens de erro de validação.

Os auxiliares Html.ValidationMessage() e Html.ValidationSummary() são usados nas exibições de criar e editar que são geradas automaticamente pelo scaffolding MVC do ASP.NET. Siga estas etapas para gerar a exibição Create:

1. A ação Create () no controlador de produto com o botão direito e selecione a opção de menu **adicionar exibição** (veja a Figura 1).
2. No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada** (veja a Figura 2).
3. Dos **exibir dados de classe** lista suspensa, selecione a classe Product.
4. Dos **exibir o conteúdo** lista suspensa, selecione Criar.
5. Clique no botão **Adicionar**.


Certifique-se de que você compila seu aplicativo antes de adicionar um modo de exibição. Caso contrário, a lista de classes não aparecerá na **exibir dados de classe** lista suspensa.


[![A caixa de diálogo Novo projeto](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Figura 01**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](performing-simple-validation-vb/_static/image2.png))


[![A caixa de diálogo Novo projeto](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Figura 02**: criar uma exibição fortemente tipada ([clique para exibir a imagem em tamanho normal](performing-simple-validation-vb/_static/image4.png))


Depois de concluir essas etapas, você obtém o modo de exibição criar na listagem 2.

**Listagem 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Na listagem 2, o auxiliar Html.ValidationSummary() é chamado imediatamente acima do formulário HTML. Esse auxiliar é usado para exibir uma lista de mensagens de erro de validação. O auxiliar Html.ValidationSummary() renderiza os erros em uma lista com marcadores.

O auxiliar Html.ValidationMessage() é chamado ao lado de cada um dos campos de formulário HTML. Esse auxiliar é usado para exibir uma mensagem de erro ao lado de um campo de formulário. No caso da listagem 2, o auxiliar Html.ValidationMessage() exibe um asterisco, quando ocorre um erro.

A página na Figura 3 ilustra as mensagens de erro renderizadas pelos auxiliares de validação quando o formulário é enviado com campos ausentes e valores inválidos.


[![A caixa de diálogo Novo projeto](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Figura 03**: criar o modo de exibição enviado com problemas ([clique para exibir a imagem em tamanho normal](performing-simple-validation-vb/_static/image6.png))


Observe que a aparência do HTML entrada campos também são modificados quando há um erro de validação. Os renderizadores de auxiliar Html.TextBox() uma *classe = "Erro de validação de entrada"* atributo quando há um erro de validação associado à propriedade renderizada pelo auxiliar de Html.TextBox().

Há três classes de folha de estilo em cascata usadas para controlar a aparência de erros de validação:

- entrada-erro de validação - aplicado para o &lt;entrada&gt; renderizada pelo Html.TextBox() auxiliar de marca.
- campo--erro de validação - aplicado à &lt;span&gt; renderizada pelo Html.ValidationMessage() auxiliar de marca.
- – Resumo – erros de validação - aplicado para o &lt;ul&gt; renderizada pelo Html.ValidationSumamry() auxiliar de marca.

Você pode modificar essas classes de folha de estilo em cascata e, portanto, modificar a aparência dos erros de validação, modificando o arquivo CSS localizado na pasta de conteúdo.

> [!NOTE] 
> 
> A classe HtmlHelper inclui propriedades estáticas somente leitura para recuperar os nomes de validação relacionados a CSS classes. Essas propriedades estáticas são nomeadas ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding validação e a validação de Postbinding

Se você enviar o formulário HTML para a criação de um produto e insira um valor inválido para o campo de preço e nenhum valor para o campo UnitsInStock, em seguida, você receberá as mensagens de validação exibidas na Figura 4. Onde vêm essas mensagens de erro de validação?


[![A caixa de diálogo Novo projeto](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Figura 04**: erros de validação Prebinding ([clique para exibir a imagem em tamanho normal](performing-simple-validation-vb/_static/image8.png))


Há realmente dois tipos de mensagens de erro de validação - aqueles gerados antes que os campos de formulário HTML são associados a uma classe e aqueles gerados depois que os campos de formulário são associados à classe. Em outras palavras, há prebinding erros de validação e postbinding erros de validação.

A ação Create () exposta pelo controlador de produto na listagem 1 aceita uma instância da classe Product. A assinatura do método Create tem esta aparência:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Os valores dos campos de formulário HTML do formulário Criar são associados à classe productToCreate por algo chamado de um associador de modelo. O associador de modelo padrão adiciona uma mensagem de erro para o estado de modelo automaticamente quando ele não é possível associar um campo de formulário a uma propriedade de formulário.

O associador de modelo padrão não é possível associar a cadeia de caracteres "apple" para a propriedade de preço da classe Product. É possível atribuir uma cadeia de caracteres a uma propriedade decimal. Portanto, o associador de modelo adiciona um erro para o estado de modelo.

O associador de modelo padrão também não é possível atribuir o valor Nothing a uma propriedade que não aceita o valor Nothing. Em particular, o associador de modelo não é possível atribuir o valor Nothing para a propriedade UnitsInStock. Mais uma vez, o associador de modelo desiste e adiciona uma mensagem de erro para o estado de modelo.

Se você quiser personalizar a aparência desses prebinding mensagens de erro, em seguida, você precisa criar cadeias de caracteres de recurso para essas mensagens.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era descrever a mecânica básica de validação na estrutura do ASP.NET MVC. Você aprendeu a usar o estado de modelo e os auxiliares HTML de validação. Também discutimos a distinção entre prebinding e postbinding validação. Outros tutoriais, discutiremos várias estratégias para mover o código de validação para fora de seus controladores de e para suas classes de modelo.

> [!div class="step-by-step"]
> [Anterior](displaying-a-table-of-database-data-vb.md)
> [Próximo](validating-with-the-idataerrorinfo-interface-vb.md)
