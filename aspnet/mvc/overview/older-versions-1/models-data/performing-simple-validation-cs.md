---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: Executar a validação Simple (c#) | Microsoft Docs
author: StephenWalther
description: Saiba como executar a validação em um aplicativo ASP.NET MVC. Neste tutorial, Stephen Walther apresenta a você para o estado de modelo e o auxiliar de validação HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fc1dcc6935841382215f67a519cd241ac68931a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869654"
---
<a name="performing-simple-validation-c"></a>Executar a validação Simple (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Saiba como executar a validação em um aplicativo ASP.NET MVC. Neste tutorial, Stephen Walther apresenta a você para o estado de modelo e os auxiliares de validação HTML.


O objetivo deste tutorial é explicar como você pode executar a validação em um aplicativo ASP.NET MVC. Por exemplo, você aprenderá a impedir que alguém enviar um formulário que não contém um valor para um campo obrigatório. Você aprenderá a usar o estado de modelo e os auxiliares HTML de validação.

## <a name="understanding-model-state"></a>Compreendendo o estado do modelo

Você usa o estado de modelo - ou mais precisamente, dicionário de estado de modelo - para representar os erros de validação. Por exemplo, a ação Create () na listagem 1 valida as propriedades de uma classe de produto antes de adicionar a classe de produto para um banco de dados.


Eu não estou recomendando que você adicione sua lógica de validação ou o banco de dados a um controlador. Um controlador deve conter apenas a lógica relacionada ao controle de fluxo do aplicativo. Estamos realizando um atalho para manter as coisas simples.


**Listando 1 - Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

Na listagem 1, as propriedades de nome, descrição e UnitsInStock da classe de produto são validadas. Se qualquer uma dessas propriedades falhar um teste de validação, um erro será adicionado ao dicionário de estado de modelo (representado pela propriedade ModelState da classe do controlador).

Se houver erros no estado de modelo, em seguida, a propriedade ModelState retorna false. Nesse caso, o formulário HTML para criar um novo produto é exibida novamente. Caso contrário, se não houver nenhum erro de validação, o novo produto é adicionado ao banco de dados.

## <a name="using-the-validation-helpers"></a>Usando os auxiliares de validação

A estrutura ASP.NET MVC inclui dois auxiliares de validação: o auxiliar de Html.ValidationMessage() e o auxiliar Html.ValidationSummary(). Você pode usar esses dois auxiliares em uma exibição para exibir mensagens de erro de validação.

Os auxiliares Html.ValidationMessage() e Html.ValidationSummary() são usados nas exibições de criar e editar que são geradas automaticamente pela estrutura ASP.NET MVC. Siga estas etapas para gerar a exibição de criar:

1. A ação Create () no controlador de produto e selecione a opção de menu **adicionar exibição** (consulte a Figura 1).
2. No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada** (consulte a Figura 2).
3. Do **exibir dados de classe** lista suspensa, selecione a classe do produto.
4. Do **exibir conteúdo** lista suspensa, selecione Criar.
5. Clique no botão **Adicionar**.


Certifique-se de que você criar seu aplicativo antes de adicionar um modo de exibição. Caso contrário, a lista de classes não aparecerá no **exibir dados de classe** lista suspensa.


[![A caixa de diálogo Novo projeto](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**Figura 01**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](performing-simple-validation-cs/_static/image2.png))


[![A caixa de diálogo Novo projeto](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**Figura 02**: Criando uma exibição fortemente tipado ([clique para exibir a imagem em tamanho normal](performing-simple-validation-cs/_static/image4.png))


Depois de concluir essas etapas, você obtém o modo de exibição de criar lista 2.

**A listagem 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

Na listagem 2, o auxiliar Html.ValidationSummary() é chamado imediatamente acima do formulário HTML. Este auxiliar é usado para exibir uma lista das mensagens de erro de validação. O auxiliar Html.ValidationSummary() renderiza os erros em uma lista com marcadores.

O auxiliar Html.ValidationMessage() é chamado ao lado de cada um dos campos de formulário HTML. Este auxiliar é usado para exibir uma mensagem de erro ao lado de um campo de formulário. No caso de listagem 2, o auxiliar Html.ValidationMessage() exibe um asterisco quando há um erro.

A página na Figura 3 ilustra as mensagens de erro processadas pelos auxiliares de validação quando o formulário é enviado com valores inválidos e os campos ausentes.


[![A caixa de diálogo Novo projeto](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**Figura 03**: criar o modo de exibição enviado com problemas ([clique para exibir a imagem em tamanho normal](performing-simple-validation-cs/_static/image6.png))


Observe que a aparência do HTML de entrada campos também são modificados quando há um erro de validação. Os renderizadores de auxiliar Html.TextBox() uma *classe = "Erro de validação de entrada"* atributo quando há um erro de validação associada com a propriedade renderizada pelo auxiliar de Html.TextBox().

Há três classes de folha de estilo em cascata usadas para controlar a aparência de erros de validação:

- entrada erro-validação - aplicado para o &lt;entrada&gt; renderizada pelo Html.TextBox() auxiliar de marca.
- campo erro-validação - aplicado para o &lt;abrangem&gt; marca renderizada pelo auxiliar de Html.ValidationMessage().
- Resumo-validação-erros - aplicado para o &lt;ul&gt; marca renderizada pelo auxiliar de Html.ValidationSumamry().

Você pode modificar essas classes de folha de estilo em cascata e, portanto, modificar a aparência dos erros de validação, modificando o arquivo Site.css localizado na pasta de conteúdo.

> [!NOTE] 
> 
> A classe HtmlHelper inclui propriedades estáticas somente leitura para recuperar os nomes de validação relacionados a CSS classes. Essas propriedades estáticas são nomeadas ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Validação prebinding e validação de Postbinding

Se você enviar o formulário HTML para a criação de um produto, e você inserir um valor inválido para o campo de preço e nenhum valor para o campo UnitsInStock, você receberá as mensagens de validação exibidas na Figura 4. Onde vêm essas mensagens de erro de validação?


[![A caixa de diálogo Novo projeto](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**Figura 04**: erros de validação de Prebinding ([clique para exibir a imagem em tamanho normal](performing-simple-validation-cs/_static/image8.png))


Há realmente dois tipos de mensagens de erro de validação - aqueles gerados antes que os campos de formulário HTML são associados a uma classe e aqueles gerados depois que os campos de formulário são associados à classe. Em outras palavras, há prebinding erros de validação e postbinding erros de validação.

A ação Create () exposta pelo controlador de produto na listagem 1 aceita uma instância da classe de produto. A assinatura do método Create tem esta aparência:

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

Os valores dos campos de formulário HTML do formulário Criar estão associados à classe productToCreate por algo chamado de um associador de modelo. O associador de modelo padrão adiciona uma mensagem de erro para o estado de modelo automaticamente quando ele não é possível associar um campo de formulário a uma propriedade de formulário.

O associador de modelo padrão não é possível associar a cadeia de caracteres "apple" para a propriedade de preço da classe de produto. Você não pode atribuir uma cadeia de caracteres para uma propriedade decimal. Portanto, o associador de modelo adiciona um erro ao estado de modelo.

O associador de modelo padrão também não é possível atribuir um valor nulo em uma propriedade que não aceita valores nulos. Em particular, o associador de modelo não é possível atribuir um valor nulo à propriedade UnitsInStock. Novamente, o associador de modelo desistir e adiciona uma mensagem de erro para o estado de modelo.

Se você quiser personalizar a aparência desses prebinding mensagens de erro, em seguida, você precisa criar cadeias de caracteres de recurso para essas mensagens.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era descrever os mecanismos básicos de validação na estrutura do ASP.NET MVC. Você aprendeu a usar o estado de modelo e os auxiliares HTML de validação. Também discutimos a distinção entre prebinding e postbinding a validação. Outros tutoriais, discutiremos várias estratégias para mover seu código de validação de seus controladores e em suas classes de modelo.

> [!div class="step-by-step"]
> [Anterior](displaying-a-table-of-database-data-cs.md)
> [Próximo](validating-with-the-idataerrorinfo-interface-cs.md)
