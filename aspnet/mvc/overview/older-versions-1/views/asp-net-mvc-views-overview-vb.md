---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: O ASP.NET MVC exibições visão geral (VB) | Microsoft Docs
author: StephenWalther
description: O que é um modo de exibição do ASP.NET MVC e como ela difere de uma página HTML? Neste tutorial, Stephen Walther apresenta exibições e demonstra como você pode t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a64c70851d13b923964dfd1cf3bad55612ae0d0f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-views-overview-vb"></a>Visão geral (VB) de exibições do MVC do ASP.NET
====================
por [Stephen Walther](https://github.com/StephenWalther)

> O que é um modo de exibição do ASP.NET MVC e como ela difere de uma página HTML? Neste tutorial, Stephen Walther apresenta a modos de exibição e demonstra como você pode tirar proveito dos dados de exibição e auxiliares HTML em uma exibição.


O objetivo deste tutorial é fornecer uma breve introdução às exibições do ASP.NET MVC, dados de exibição e auxiliares HTML. No final deste tutorial, você deve compreender como criar novos modos de exibição, passar dados de um controlador para um modo de exibição e usar auxiliares HTML para gerar o conteúdo em uma exibição.

## <a name="understanding-views"></a>Noções básicas sobre modos de exibição

Ao contrário do ASP.NET ou Active Server Pages, ASP.NET MVC não inclui tudo o que corresponde diretamente a uma página. Em um aplicativo ASP.NET MVC, não há uma página em disco que corresponde ao caminho na URL que você digita na barra de endereços do navegador. O mais próximo a uma página em um aplicativo ASP.NET MVC é algo chamado um *exibição*.

Em um aplicativo ASP.NET MVC, solicitações de navegador são mapeadas para ações do controlador. Uma ação do controlador pode retornar uma exibição. No entanto, uma ação do controlador pode executar algum outro tipo de ação como redirecionando você para outra ação de controlador.

Listar 1 contém um controlador simple denominado HomeController. HomeController expõe duas ações do controlador denominadas Index () e Details().

**Listando 1 - HomeController.vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

Você pode chamar a primeira ação, ação Index (), digitando a URL a seguir na barra de endereços do navegador:

/ Início/índice

Você pode chamar a segunda ação, a ação de Details(), digitando este endereço no navegador:

/Home/Details

A ação Index () retorna uma exibição. A maioria das ações que você criar retornará modos de exibição. No entanto, uma ação pode retornar outros tipos de resultados da ação. Por exemplo, a ação de Details() retorna um RedirectToActionResult que redireciona a solicitação de entrada para a ação Index ().

A ação Index () contém a única linha de código a seguir:

View()

Esta linha de código retorna uma exibição que deve estar localizada no seguinte caminho no servidor web:

\Views\Home\Index.aspx

O caminho para o modo de exibição é inferido do nome do controlador e o nome da ação do controlador.

Se preferir, você pode ser explícito sobre o modo de exibição. A linha de código a seguir retorna uma exibição chamada Fred:

Modo de exibição (Fred)

Quando esta linha de código é executada, uma exibição é retornada no seguinte caminho:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Se você planeja criar testes de unidade para seu aplicativo ASP.NET MVC é uma boa ideia para ser explícito sobre nomes de exibição. Dessa forma, você pode criar um teste de unidade para verificar se o modo de exibição esperado foi retornado por uma ação do controlador.


## <a name="adding-content-to-a-view"></a>Adicionando conteúdo a um modo de exibição

Uma exibição é um padrão de (documento HTML que pode conter scripts X). Você pode usar scripts para adicionar conteúdo dinâmico para um modo de exibição.

Por exemplo, o modo de exibição na lista 2 exibe a data e hora atuais.

**A listagem 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

Observe que o corpo da página HTML na listagem 2 contém o script a seguir:

&lt;% Response.Write(DateTime.Now)%&gt;

Use os delimitadores de script &lt;% e %&gt; para marcar o início e término de um script. Esse script é escrito em Visual basic. Ele exibe a data e hora atuais chamando o método Response para renderizar o conteúdo para o navegador. Os delimitadores de script &lt;% e %&gt; pode ser usada para executar uma ou mais instruções.

Como chamar Response com frequência, Microsoft fornece um atalho para chamar o método Response. O modo de exibição na listagem 3 usa os delimitadores &lt;% = e %&gt; como um atalho para chamar Response.

**A listagem 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

Você pode usar qualquer linguagem .NET para gerar conteúdo dinâmico em uma exibição. Normalmente, você ll usar Visual Basic .NET ou c# para escrever seus controladores e exibições.

## <a name="using-html-helpers-to-generate-view-content"></a>Usando auxiliares HTML para gerar o conteúdo da exibição

Para tornar mais fácil de adicionar conteúdo a um modo de exibição, você pode tirar proveito de algo chamado de um *auxiliar HTML*. Um auxiliar HTML, normalmente, é um método que gera uma cadeia de caracteres. Você pode usar os auxiliares HTML para gerar elementos HTML padrão, como caixas de texto, links, listas suspensas e caixas de listagem.

Por exemplo, a exibição na listagem 4 aproveita os auxiliares HTML três – os auxiliares BeginForm(), TextBox() e Password() – para gerar um logon de formam (consulte a Figura 1).

**Listing 4 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


[![A caixa de diálogo Novo projeto](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)

**Figura 01**: um formulário de logon padrão ([clique para exibir a imagem em tamanho normal](asp-net-mvc-views-overview-vb/_static/image2.png))


Todos os métodos auxiliares HTML são chamados na propriedade Html da exibição. Por exemplo, você deve processar uma caixa de texto chamando o método Html.TextBox().

Observe que você use os delimitadores de script &lt;% = e %&gt; ao chamar o Html.TextBox() e Html.Password() auxiliares. Esses auxiliares simplesmente retornam uma cadeia de caracteres. Você precisa chamar Response para renderizar a cadeia de caracteres para o navegador.

Usar métodos auxiliares HTML é opcional. Elas facilitam sua vida, reduzindo a quantidade de HTML e script que você precisa para escrever. O modo de exibição na listagem 5 renderiza a mesma forma exata que o modo de exibição na listagem 4 sem usar auxiliares HTML.

**Listing 5 -- \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

Você também tem a opção de criar seu próprio auxiliares HTML. Por exemplo, você pode criar um método auxiliar GridView() que exibe um conjunto de registros do banco de dados em uma tabela HTML automaticamente. Podemos explorar este tópico no tutorial **auxiliares de HTML personalizado criando**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Usando dados de exibição para passar dados para um modo de exibição

Você pode usar dados de exibição para passar dados de um controlador para um modo de exibição. Pense exibir dados como um pacote que você enviar por email. Todos os dados passados de um controlador para um modo de exibição devem ser enviados usando este pacote. Por exemplo, o controlador na listagem 6 adiciona uma mensagem para exibir dados.

**Listando 6 - ProductController.vb**

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

O controlador de propriedade ViewData representa uma coleção de pares de nome e valor. Na listagem 6, o método Index () adiciona um item para a coleta de dados de exibição denominada mensagem com o valor Olá, mundo!. Quando o modo de exibição é retornado pelo método Index (), os dados de exibição são passados para o modo de exibição automaticamente.

O modo de exibição na listagem 7 recupera a mensagem de dados de exibição e processa a mensagem para o navegador.

**Listando 7-- \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

Observe que o modo de exibição aproveita o método auxiliar de HTML Html.Encode() ao processar a mensagem. O auxiliar HTML Html.Encode() codifica caracteres especiais, como &lt; e &gt; em caracteres que são seguros exibir uma página da web. Sempre que você processar o conteúdo que um usuário envia a um site, você deve codificar o conteúdo para evitar ataques de injeção de JavaScript.

(Pois criamos a mensagem de nós no ProductController, podemos don t precisa codificar a mensagem. No entanto, é um bom hábito sempre chamar o método Html.Encode() ao exibir o conteúdo recuperado de exibir dados em uma exibição.)

Na listagem 7, aproveitamos os dados de exibição para passar uma mensagem de cadeia de caracteres simples de um controlador para um modo de exibição. Você também pode usar dados de exibição para passar a outros tipos de dados, como um conjunto de registros do banco de dados, de um controlador para um modo de exibição. Por exemplo, se você deseja exibir o conteúdo da tabela de banco de dados de produtos em uma exibição, em seguida, você passaria a coleção de banco de dados no modo de exibição registra dados.

Você também tem a opção de passar os dados de exibição fortemente tipada de um controlador para um modo de exibição. Podemos explorar este tópico no tutorial **Noções básicas sobre fortemente tipado exibir dados e exibições**.

## <a name="summary"></a>Resumo

Este tutorial fornecida uma breve introdução ao ASP.NET MVC modos de exibição, dados de exibição e auxiliares HTML. A primeira seção, você aprendeu a adicionar novos modos de exibição ao seu projeto. Você aprendeu que você deve adicionar um modo de exibição para a pasta correta para chamá-lo de um controlador específico. Em seguida, abordamos o tópico de auxiliares HTML. Você aprendeu como auxiliares HTML permitem que você gerar facilmente o conteúdo HTML padrão. Por fim, você aprendeu como tirar proveito dos dados de exibição para passar dados de um controlador para um modo de exibição.

> [!div class="step-by-step"]
> [Anterior](passing-data-to-view-master-pages-cs.md)
> [Próximo](creating-custom-html-helpers-vb.md)
