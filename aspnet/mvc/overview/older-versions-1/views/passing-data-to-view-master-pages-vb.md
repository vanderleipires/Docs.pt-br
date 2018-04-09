---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Transmitindo dados para páginas de exibição mestre (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página de exibição mestre. Vamos examinar duas estratégias para transferir dados para uma exibição m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: fcd7c5baacc00490720d1f82252d81e40c097c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="passing-data-to-view-master-pages-vb"></a>Transmitindo dados para páginas de exibição mestre (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página de exibição mestre. Examinamos duas estratégias para transferir dados para uma página de exibição mestre. Primeiro, vamos abordar uma solução fácil que resulta em um aplicativo que é difícil de manter. Em seguida, podemos examinar uma solução muito melhor que exige um pouco mais trabalho inicial, mas resulta em um aplicativo muito mais fácil manutenção.


## <a name="passing-data-to-view-master-pages"></a>Transmitindo dados para páginas de exibição mestre

O objetivo deste tutorial é explicar como você pode passar dados de um controlador para uma página de exibição mestre. Examinamos duas estratégias para transferir dados para uma página de exibição mestre. Primeiro, vamos abordar uma solução fácil que resulta em um aplicativo que é difícil de manter. Em seguida, podemos examinar uma solução muito melhor que exige um pouco mais trabalho inicial, mas resulta em um aplicativo muito mais fácil manutenção.

### <a name="the-problem"></a>O problema

Imagine que você está criando um aplicativo de banco de dados do filme e você deseja exibir a lista de categorias de filme em todas as páginas em seu aplicativo (consulte a Figura 1). Além disso, imagine que a lista de categorias de filme é armazenada em uma tabela de banco de dados. Nesse caso, faria sentido para recuperar as categorias do banco de dados e processar a lista de categorias de filme dentro de uma página de exibição mestre.


[![Exibindo as categorias de filmes em uma página de exibição mestre](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figura 01**: exibindo as categorias de filmes em uma página de exibição mestre ([clique para exibir a imagem em tamanho normal](passing-data-to-view-master-pages-vb/_static/image3.png))


Aqui está o problema. Como recuperar a lista de categorias de filme na página mestra? É tentador para chamar diretamente os métodos de suas classes de modelo na página mestra. Em outras palavras, é tentador para incluir o código para recuperar os dados a partir da direita do banco de dados na sua página mestra. No entanto, ignorar os controladores MVC para acessar o banco de dados violaria a separação limpa de preocupações que é um dos principais benefícios da criação de um aplicativo MVC.

Em um aplicativo MVC, você deseja que todas as interações entre as exibições do MVC e seu modelo MVC para ser tratado por seus controladores MVC. Essa separação de preocupações resulta em um aplicativo mais sustentável, adaptável e podem ser testado.

Em um aplicativo MVC, todos os dados passados para um modo de exibição, incluindo uma página de exibição mestre, devem ser passados para um modo de exibição por uma ação do controlador. Além disso, os dados devem ser passados por aproveitando as vantagens de exibir dados. No restante deste tutorial, eu examinar dois métodos de passar os dados de exibição para uma página de exibição mestre.

### <a name="the-simple-solution"></a>A solução mais simples

Vamos começar com a solução mais simples para passar dados de exibição de um controlador para uma página de exibição mestre. A solução mais simples é passar os dados de exibição para a página mestra em cada ação de controlador.

Considere o controlador na listagem 1. Isso expõe duas ações denominadas `Index()` e `Details()`. O `Index()` método de ação retorna cada filme na tabela de banco de dados de filmes. O `Details()` método de ação retorna todos os filmes em uma categoria específica de filme.

**Listando 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Observe que tanto o `Index()` e `Details()` ações adicionam dois itens para exibir os dados. O `Index()` ação adiciona duas chaves: categorias e filmes. A chave de categorias representa a lista de categorias de filme exibida pela página modo de exibição mestre. A chave de filmes representa a lista de filmes exibidos por página de exibição de índice.

O `Details()` ação também adiciona duas chaves denominado categorias e filmes. A chave de categorias, uma vez, representa a lista de categorias de filme exibida pela página modo de exibição mestre. A chave de filmes representa a lista de filmes em uma determinada categoria exibidos por página de exibição de detalhes (consulte a Figura 2).


[![O modo de exibição de detalhes](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figura 02**: exibir os detalhes ([clique para exibir a imagem em tamanho normal](passing-data-to-view-master-pages-vb/_static/image6.png))


O modo de exibição do índice está contido na listagem 2. Ele simplesmente itera através da lista de filmes representado pelo item de filmes nos dados de exibição.

**A listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

A exibição de página mestra está contida na listagem 3. A página de exibição mestre itera e processa todas as categorias de filme representadas pelo item de categorias de dados de exibição.

**A listagem 3 – `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Todos os dados são passados para o modo de exibição e a página de exibição mestre por exibir dados. Que é a maneira correta para passar dados para a página mestra.

Portanto, o que está errado com essa solução? O problema é que essa solução viola o princípio de teste (não repetitivo). Cada ação de controlador deve adicionar a mesma lista de categorias de filme para exibir dados. Com código duplicado em seu aplicativo torna muito mais difícil de manter, adaptar e modificar seu aplicativo.

### <a name="the-good-solution"></a>A solução adequada

Nesta seção, vamos examinar uma solução alternativa e melhor, para passar dados de uma ação do controlador para uma página de exibição mestre. Em vez de adicionar categorias de filme para a página mestra em cada ação de controlador, adicionamos as categorias de filme para exibir dados apenas uma vez. Todos os dados de exibição usados pela página modo de exibição mestre é adicionada no controlador de aplicativo.

A classe ApplicationController está contida na listagem 4.

A classe ApplicationController está contida na listagem 4.

**A listagem 4 – `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Há três coisas que você deve observar sobre o controlador de aplicativo na listagem 4. Primeiro, observe que a classe herda da classe Controller base. O controlador de aplicativo é uma classe de controlador.

Segundo, observe que a classe do controlador de aplicativo é uma classe MustInherit. Uma classe MustInherit é uma classe que deve ser implementada por uma classe concreta. Como o controlador de aplicativo é uma classe MustInherit, você não não é possível invocar métodos definidos na classe diretamente. Se você tentar chamar a classe do aplicativo diretamente obterá uma mensagem de erro de recurso não foi encontrado.

Em terceiro lugar, observe que o controlador de aplicativo contém um construtor que adiciona a lista de categorias de filme para exibir dados. Cada classe de controlador que herda do controlador de aplicativo chama o construtor do controlador de aplicativo automaticamente. Sempre que você chamar qualquer ação em qualquer controlador que herda do controlador de aplicativo, as categorias de filme é incluído nos dados de exibição automaticamente.

O controlador de filmes na listagem 5 herda do controlador de aplicativo.

**Listando 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

O controlador de filmes, assim como o controlador Home discutido na seção anterior, expõe dois métodos de ação denominados `Index()` e `Details()`. Observe que a lista de categorias de filme exibidos por página mestra exibição não é adicionada para exibir os dados no `Index()` ou `Details()` método. Como o controlador de filmes herda do controlador de aplicativo, a lista de categorias de filme é adicionada para exibir os dados automaticamente.

Observe que esta solução para adicionar dados de exibição para uma página de exibição mestre não viola o princípio de teste (não repetitivo). O código para adicionar a lista de categorias de filme para exibir os dados que está contido em um só local: o construtor para o controlador de aplicativo.

### <a name="summary"></a>Resumo

Neste tutorial, discutimos duas abordagens para passar dados de exibição de um controlador para uma página de exibição mestre. Primeiro, examinamos simples, mas difícil de manter a abordagem. A primeira seção, discutimos como você pode adicionar dados de exibição para uma página de exibição mestre em cada cada ação do controlador em seu aplicativo. Podemos concluir que essa foi uma abordagem inválida porque viola o princípio de teste (não repetitivo).

Em seguida, examinamos uma estratégia melhor para adicionar dados requeridos por uma página de exibição mestre para exibir dados. Em vez de adicionar os dados de exibição em cada ação de controlador, adicionamos os exibir dados apenas uma vez dentro de um controlador de aplicativo. Dessa forma, você pode evitar código duplicado ao passar dados para uma página de exibição mestre em um aplicativo ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](creating-page-layouts-with-view-master-pages-vb.md)
