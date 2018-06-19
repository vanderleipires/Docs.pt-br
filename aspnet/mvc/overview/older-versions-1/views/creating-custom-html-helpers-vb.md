---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Criando auxiliares HTML personalizado (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizado que você pode usar em suas exibições MVC. Ao aproveitar o auxiliar HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 6980026e2653eacb71697f9b34def9bc38638726
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871500"
---
<a name="creating-custom-html-helpers-vb"></a>Criando auxiliares HTML personalizado (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizado que você pode usar em suas exibições MVC. Aproveitando auxiliares HTML, você pode reduzir a quantidade de digitação entediante de marcas HTML que você deve executar para criar uma página HTML padrão.


O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizado que você pode usar em suas exibições MVC. Aproveitando auxiliares HTML, você pode reduzir a quantidade de digitação entediante de marcas HTML que você deve executar para criar uma página HTML padrão.

A primeira parte deste tutorial, descrevo alguns os auxiliares HTML existente incluído com a estrutura ASP.NET MVC. Em seguida, descrevo dois métodos de criação de auxiliares HTML personalizados: explicam como criar auxiliares HTML personalizados ao criar um método compartilhado e criar um método de extensão.

## <a name="understanding-html-helpers"></a>Noções básicas sobre auxiliares HTML

Um auxiliar HTML é apenas um método que retorna uma cadeia de caracteres. A cadeia de caracteres pode representar qualquer tipo de conteúdo que você deseja. Por exemplo, você pode usar os auxiliares HTML para processar as marcas HTML padrão como HTML `<input>` e `<img>` marcas. Você também pode usar os auxiliares HTML para renderizar o conteúdo mais complexo, como uma faixa da guia ou uma tabela HTML do banco de dados.

A estrutura ASP.NET MVC inclui o seguinte conjunto de auxiliares de HTML padrão (não é uma lista completa):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Por exemplo, considere o formulário na listagem 1. Este formulário é renderizado com a Ajuda de dois os auxiliares HTML padrão (consulte a Figura 1). Este formulário usa o `Html.BeginForm()` e `Html.TextBox()` métodos auxiliares.


[![Página renderizada com auxiliares HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**Figura 01**: página renderizada com auxiliares HTML ([clique para exibir a imagem em tamanho normal](creating-custom-html-helpers-vb/_static/image3.png))


**Listando 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

O `Html.BeginForm()` método auxiliar é usado para criar o HTML de abertura e fechamento `<form>` marcas. Observe que o `Html.BeginForm()` método é chamado no uso instrução. O uso da instrução garante que o `<form>` marca é fechada no final do bloco.

Se preferir, em vez de criar um usando o bloco, você pode chamar o método auxiliar Html.EndForm() para fechar o `<form>` marca. Use a abordagem para criar uma abertura e fechamento `<form>` marca parece mais intuitiva para você.

O `Html.TextBox()` métodos auxiliares usados na listagem 1 para renderizar HTML `<input>` marcas. Se você selecionar Exibir código-fonte em seu navegador, você verá o código-fonte HTML na listagem 2. Observe que a fonte contém marcas HTML padrão.

> [!IMPORTANT]
> Observe que o `Html.TextBox()`-HTML auxiliar é renderizado com `<%= %>` marcas em vez de `<% %>` marcas. Se você não incluir o sinal de igual, nada é renderizado no navegador.

A estrutura ASP.NET MVC contém um conjunto pequeno de auxiliares. Provavelmente, você precisará estender a estrutura do MVC com auxiliares HTML personalizados. O restante deste tutorial, você aprenderá dois métodos de criação de auxiliares HTML personalizados.

**A listagem 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>Criando auxiliares HTML com métodos compartilhados

A maneira mais fácil de criar um novo auxiliar HTML é criar um método compartilhado que retorna uma cadeia de caracteres. Por exemplo, imagine que você optar por criar um novo auxiliar HTML que renderiza uma marca HTML `<label>` marca. Você pode usar a classe na lista 2 para renderizar um `<label>`.

**A listagem 2 – `Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

Não há nada de especial sobre a classe na listagem 2. O `Label()` método simplesmente retorna uma cadeia de caracteres.

Usa o modo de exibição do índice modificado na listagem 3 o `LabelHelper` para renderizar HTML `<label>` marcas. Observe que o modo de exibição inclui um `<%@ imports %>` diretiva que importa o namespace Application1.Helpers.

**A listagem 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Criando auxiliares HTML com os métodos de extensão

Se você deseja criar auxiliares HTML que funcionam apenas como os auxiliares HTML padrão incluído na estrutura do ASP.NET MVC, em seguida, você precisa criar métodos de extensão. Métodos de extensão permitem adicionar novos métodos para uma classe existente. Ao criar um método auxiliar HTML, você adicionar novos métodos para o `HtmlHelper` representado pela propriedade de Html do modo de exibição de classe.

O módulo do Visual Basic na listagem 3 adiciona um método de extensão denominado `Label()` para o `HtmlHelper` classe. Há algumas coisas que você deve observar sobre esse módulo. Primeiro, observe que o módulo está decorado com o `<Extension()>` atributo. Para usar esse atributo, você deve importar o `System.Runtime.CompilerServices` namespace

Segundo, observe que o primeiro parâmetro do `Label()` método representa o `HtmlHelper` classe. O primeiro parâmetro de um método de extensão indica a classe que estende o método de extensão.

**A listagem 3 – `Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

Depois de criar um método de extensão e crie seu aplicativo com êxito, o método de extensão é exibida no Visual Studio Intellisense, como todos os outros métodos de uma classe (consulte a Figura 2). A única diferença é a extensão métodos são exibidos com um símbolo especial ao lado deles (um ícone de uma seta para baixo).


[![Usando o método de extensão Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**Figura 02**: usando o método de extensão Html.Label() ([clique para exibir a imagem em tamanho normal](creating-custom-html-helpers-vb/_static/image6.png))


O modo de exibição do índice modificado na listagem 4 usa o método de extensão Html.Label() para processar todos os seus &lt;rótulo&gt; marcas.

**A listagem 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu dois métodos de criação de auxiliares HTML personalizados. Primeiro, você aprendeu a criar um personalizado `Label()` auxiliar HTML por meio da criação de um método compartilhado que retorna uma cadeia de caracteres. Em seguida, você aprendeu a criar um personalizado `Label()` método auxiliar HTML, criando um método de extensão no `HtmlHelper` classe.

Neste tutorial, eu se concentra na criação de um método auxiliar HTML extremamente simple. Observe que um auxiliar HTML pode ser mais complicado do que você desejar. Você pode criar auxiliares HTML que processam conteúdo formatado como modos de exibição de árvore, menus ou tabelas de banco de dados.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-views-overview-vb.md)
> [Próximo](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
