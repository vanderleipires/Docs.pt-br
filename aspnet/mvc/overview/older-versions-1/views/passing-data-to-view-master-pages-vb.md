---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Passando dados para exibir páginas mestras (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página de exibição mestre. Examinamos duas estratégias para passar dados para uma exibição m...
ms.author: aspnetcontent
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 2daab1e8596035c1a70fb0f86ba752837d468ef2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822238"
---
<a name="passing-data-to-view-master-pages-vb"></a>Transmitindo dados para exibir páginas mestras (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página de exibição mestre. Examinamos duas estratégias para passar dados para uma página de exibição mestre. Primeiro, discutiremos uma solução fácil que resulta em um aplicativo que é difícil de manter. Em seguida, vamos examinar uma solução muito melhor que requer um pouco mais de trabalho inicial, mas isso resulta em um aplicativo muito mais fácil de manter.


## <a name="passing-data-to-view-master-pages"></a>Transmitindo dados para exibir páginas mestras

O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página de exibição mestre. Examinamos duas estratégias para passar dados para uma página de exibição mestre. Primeiro, discutiremos uma solução fácil que resulta em um aplicativo que é difícil de manter. Em seguida, vamos examinar uma solução muito melhor que requer um pouco mais de trabalho inicial, mas isso resulta em um aplicativo muito mais fácil de manter.

### <a name="the-problem"></a>O problema

Imagine que você está criando um aplicativo de banco de dados de filme e você deseja exibir a lista de categorias de filmes em todas as páginas em seu aplicativo (veja a Figura 1). Além disso, imagine que a lista de categorias de filmes é armazenada em uma tabela de banco de dados. Nesse caso, faria sentido para recuperar as categorias do banco de dados e renderizar a lista de categorias de filmes dentro de uma página de exibição mestre.


[![Exibir categorias de filmes em uma página mestra do modo de exibição](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figura 01**: Exibir categorias de filmes em uma página de exibição mestre ([clique para exibir a imagem em tamanho normal](passing-data-to-view-master-pages-vb/_static/image3.png))


Aqui está o problema. Como você recupera a lista de categorias de filme na página mestra? É tentador para chamar métodos de suas classes de modelo na página mestra diretamente. Em outras palavras, é tentador para incluir o código para recuperar os dados à direita do banco de dados na sua página mestra. No entanto, ignorar os controladores de MVC para acessar o banco de dados violaria a separação limpa de preocupações que é um dos principais benefícios da criação de um aplicativo MVC.

Em um aplicativo MVC, você deseja todas as interações entre seus modos de exibição do MVC e o modelo MVC sejam tratadas por seus controladores MVC. Essa separação de preocupações resulta em um aplicativo que podem ser testado, adaptável e mais sustentável.

Em um aplicativo MVC, todos os dados passados para um modo de exibição, incluindo uma página mestra do modo de exibição, devem ser passados para um modo de exibição por uma ação do controlador. Além disso, os dados devem ser transmitidos, tirando proveito dos dados de exibição. No restante deste tutorial, posso examinar dois métodos de passar dados de exibição para uma página de exibição mestre.

### <a name="the-simple-solution"></a>A solução mais simples

Vamos começar com a solução mais simples para passar os dados de exibição de um controlador para uma página de exibição mestre. A solução mais simples é passar os dados de exibição para a página mestra em cada ação de controlador.

Considere o controlador na listagem 1. Ele expõe duas ações denominadas `Index()` e `Details()`. O `Index()` método de ação retorna todos os filmes na tabela de banco de dados de filmes. O `Details()` método de ação retorna todos os filmes em uma categoria de filme específica.

**Listagem 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Observe que tanto a `Index()` e o `Details()` ações adicionam dois itens para exibir os dados. O `Index()` ação adiciona duas chaves: categorias e filmes. A chave de categorias representa a lista de categorias de filmes exibido pela página modo de exibição mestre. A chave de filmes representa a lista de filmes exibidos por página de exibição de índice.

O `Details()` ação também adiciona duas chaves nomeadas categorias e filmes. A chave de categorias, mais uma vez, representa a lista de categorias de filmes exibido pela página modo de exibição mestre. A chave de filmes representa a lista de filmes em uma determinada categoria exibidos por página de exibição de detalhes (consulte a Figura 2).


[![A exibição de detalhes](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figura 02**: exibir os detalhes ([clique para exibir a imagem em tamanho normal](passing-data-to-view-master-pages-vb/_static/image6.png))


O modo de exibição de índice está contido na listagem 2. Ele simplesmente itera através da lista de filmes representado pelo item de filmes em dados de exibição.

**Listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

A exibição de página mestra está contida na listagem 3. A página de exibição mestre itera e processa todas as categorias de filme representadas pelo item de categorias de dados de exibição.

**Listagem 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Todos os dados são passados para o modo de exibição e a página mestra do modo de exibição por meio de exibir dados. Que é a maneira correta para passar dados para a página mestra.

Portanto, o que está errado com essa solução? O problema é que essa solução viola o princípio DRY (não Repeat Yourself). Cada ação do controlador deve adicionar a mesma lista de categorias de filme para exibir dados. Ter o código duplicado em seu aplicativo torna muito mais difícil de manter, adaptar e modificar seu aplicativo.

### <a name="the-good-solution"></a>A boa solução

Nesta seção, vamos examinar uma solução alternativa e melhor, para passar os dados de uma ação do controlador para uma página de exibição mestre. Em vez de adicionar as categorias de filmes para a página mestra em cada ação de controlador, adicionamos as categorias de filmes para exibir dados apenas uma vez. Todos os dados de exibição usados pela página mestre de modo de exibição é adicionado em um controlador de aplicativo.

A classe ApplicationController está contida na listagem 4.

A classe ApplicationController está contida na listagem 4.

**Listagem 4 – `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Há três coisas que você deve observar sobre o controlador de aplicativo na listagem 4. Primeiro, observe que a classe herda da classe Controller base. O controlador de aplicativo é uma classe de controlador.

Segundo, observe que a classe de controlador do aplicativo é uma classe MustInherit. Uma classe MustInherit é uma classe que deve ser implementada por uma classe concreta. Porque o controlador de aplicativo é uma classe MustInherit, você não é possível não chamar todos os métodos definidos na classe diretamente. Se você tentar chamar a classe do aplicativo diretamente, em seguida, você obterá uma mensagem de erro de recurso não pode ser encontrado.

Em terceiro lugar, observe que o controlador de aplicativo contém um construtor que adiciona a lista de categorias de filmes para exibir os dados. Cada classe de controlador que herda do controlador de aplicativo chama o construtor do controlador do aplicativo automaticamente. Sempre que você chama qualquer ação em qualquer controlador que herda do controlador de aplicativo, as categorias de filmes é incluído nos dados de exibição automaticamente.

O controlador de filmes na listagem 5 herda do controlador de aplicativo.

**Listagem 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

O controlador de filmes, assim como o controlador Home discutido na seção anterior, expõe dois métodos de ação chamados `Index()` e `Details()`. Observe que a lista de categorias de filme exibido pela página mestre de modo de exibição não é adicionada para exibir os dados na `Index()` ou `Details()` método. Como o controlador de filmes herda o controlador de aplicativo, a lista de categorias de filmes é adicionada para exibir os dados automaticamente.

Observe que essa solução à adição de dados de exibição para uma página mestra do modo de exibição não viola o princípio DRY (não Repeat Yourself). O código para adicionar a lista de categorias de filmes para exibir os dados que está contido em um só local: o construtor para o controlador de aplicativo.

### <a name="summary"></a>Resumo

Neste tutorial, discutimos duas abordagens para passar os dados de exibição de um controlador para uma página de exibição mestre. Primeiro, examinamos um simples, mas difícil de manter a abordagem. Na primeira seção, abordamos como você pode adicionar dados de exibição para uma página de exibição mestre em cada cada ação do controlador em seu aplicativo. Podemos concluir que isso foi uma abordagem inválida porque ele viola o princípio DRY (não Repeat Yourself).

Em seguida, examinamos uma estratégia melhor para adicionar os dados necessários para uma página mestra do modo de exibição para exibir os dados. Em vez de adicionar os dados de exibição em cada ação de controlador, adicionamos os dados de exibição apenas uma vez dentro de um controlador de aplicativo. Dessa forma, você pode evitar código duplicado ao passar dados para uma página mestra do modo de exibição em um aplicativo ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](creating-page-layouts-with-view-master-pages-vb.md)
