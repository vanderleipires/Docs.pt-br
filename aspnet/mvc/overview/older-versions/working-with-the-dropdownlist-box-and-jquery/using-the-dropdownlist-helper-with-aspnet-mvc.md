---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Usando o auxiliar DropDownList com ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 21373deeded801c5cea9e89f6dac0f3542a55ca5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875585"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Usando o auxiliar DropDownList com ASP.NET MVC
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

Este tutorial ensina os fundamentos de trabalhar com o [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar e [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) auxiliar em um aplicativo ASP.NET MVC. Você pode usar o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio para acompanhar o tutorial. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:

- [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)

Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). Este tutorial presume que você tenha concluído o [Introdução ao ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial ou[repositório de música do ASP.NET MVC](../mvc-music-store/mvc-music-store-part-1.md) tutorial ou você estiver familiarizado com o desenvolvimento do ASP.NET MVC. Este tutorial começa com um projeto modificado a partir de [repositório de música do ASP.NET MVC](../mvc-music-store/mvc-music-store-part-1.md) tutorial. Você pode baixar o projeto inicial com o seguinte link [baixar a versão c#](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Um projeto do Visual Web Developer com o código-fonte c# de tutorial concluído está disponível para acompanhar este tópico. [Baixar](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>O que você vai criar

Você criará os métodos de ação e exibições que usam o [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) auxiliar para selecionar uma categoria. Você também usará **jQuery** para adicionar uma caixa de diálogo de categoria de inserção que pode ser usada quando uma nova categoria (como gênero ou artista) é necessária. Abaixo está uma captura de tela de criar exibição links para adicionar um novo gênero e adicionar um novo artista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Você vai aprender as habilidades

Aqui está o que você aprenderá:

- Como usar o [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) auxiliar para selecionar dados de categoria.
- Como adicionar um **jQuery** caixa de diálogo para adicionar novas categorias.

### <a name="getting-started"></a>Guia de Introdução

Iniciar baixando o projeto inicial com o link a seguir, [baixar](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). No Windows Explorer, clique com botão direito do *DDL\_Starter.zip* de arquivo e selecione Propriedades. No **DDL\_Starter.zip propriedades** caixa de diálogo, selecione Desbloquear.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Clique com botão direito DDL\_Starter.zip arquivo e selecione **extrair tudo** para descompactar o arquivo. Abra o *StartMusicStore.sln* arquivo com o Visual Web Developer 2010 Express ("Visual Web Developer" ou "VWD" em resumo) ou o Visual Studio 2010.

Pressione CTRL + F5 para executar o aplicativo e clique no **teste** link.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Selecione o **Selecionar categoria de filme (simples)** link. Uma lista de filmes tipo selecionado é exibida, com comédia o valor selecionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Clique com botão direito no navegador e selecione Exibir código-fonte. O HTML para a página é exibido. O código a seguir mostra o HTML para o elemento select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Você pode ver que cada item na lista de seleção tem um valor (0 para ação, 1 para Drama, 2 para comédia e 3 para ficção científica) e um nome para exibição (ação, Drama, comédia e ficção científica). O código acima é HTML padrão para uma lista de seleção.

Alterar a lista de seleção para Drama e pressione a **enviar** botão. A URL no navegador é `http://localhost:2468/Home/CategoryChosen?MovieType=1` e exibe a página **selecionado: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Abra o *Controllers\HomeController.cs* de arquivo e examine o `SelectCategory` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

O [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) auxiliar usado para criar uma lista de seleção HTML requer um **IEnumerable&lt;SelectListItem &gt;** , explícita ou implicitamente. Ou seja, você pode passar o **IEnumerable&lt;SelectListItem &gt;**  explicitamente para o [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) auxiliar ou você pode adicionar o **IEnumerable&lt; SelectListItem &gt;**  para o [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) usando o mesmo nome para o **SelectListItem** como a propriedade de modelo. Passando o **SelectListItem** implícita e explicitamente é abordado na próxima parte do tutorial. O código anterior mostra a maneira mais simples possível para criar um **IEnumerable&lt;SelectListItem &gt;**  e preenchê-lo com o texto e valores. Observe o `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) tem o [selecionados](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) propriedade definida como **true;** isso fará com que a lista de seleção renderizada para mostrar **comédia** como o item selecionado na lista.

O **IEnumerable&lt;SelectListItem &gt;**  criado acima é adicionada para o [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) com o nome MovieType. Isso é como passamos o **IEnumerable&lt;SelectListItem &gt;**  implicitamente para o [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) auxiliar mostrado abaixo.

Abra o *Views\Home\SelectCategory.cshtml* de arquivo e verifique se a marcação.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

Na terceira linha, definimos o layout para exibições/compartilhadas/\_simples\_cshtml, que é uma versão simplificada do arquivo layout padrão. Podemos fazer isso para manter a exibição e renderizado o HTML simples.

Neste exemplo, não estiver alterando o estado do aplicativo, para que irá enviar os dados usando um **HTTP GET**, não **HTTP POST**. Consulte a seção de W3C [a lista de verificação rápida para escolher HTTP GET ou POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Porque não podemos alterar o aplicativo e o formulário de lançamento, usamos o [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) sobrecarga que nos permite especificar o método de ação, o método de controlador e forma (**HTTP POST** ou **HTTP GET**). Normalmente contêm modos de exibição de [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) sobrecarga que não usa nenhum parâmetro. Nenhuma versão de parâmetro padrão para os dados do formulário para a versão POST do mesmo método de ação e controlador de lançamento.

A linha a seguir

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

passa um argumento de cadeia de caracteres para o **DropDownList** auxiliar. Essa cadeia de caracteres, em nosso exemplo, "MovieType" faz duas coisas:

- Ele fornece a chave para o **DropDownList** auxiliar para localizar um **IEnumerable&lt;SelectListItem &gt;**  no **ViewBag**.
- Ele é associado a dados para o elemento de formulário MovieType. Se o método de envio é **HTTP GET**, `MovieType` será uma cadeia de caracteres de consulta. Se o método de envio é **HTTP POST**, `MovieType` será adicionado no corpo da mensagem. A imagem a seguir mostra a cadeia de caracteres de consulta com o valor de 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

O código a seguir mostra o `CategoryChosen` o formulário foi enviado do método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Navegue de volta para a página de teste e selecione o **HTML SelectList** link. A página HTML renderiza um elemento select semelhante para a página de teste simple do ASP.NET MVC. Clique com botão direito a janela do navegador e selecione **Exibir código-fonte**. A marcação HTML para a lista de seleção é essencialmente idêntica. Página de teste HTML, ele funciona como o método de ação do ASP.NET MVC e a exibição que testamos anteriormente.

### <a name="improving-the-movie-select-list-with-enums"></a>Melhorando a lista de seleção de filme com Enums

Se as categorias em seu aplicativo são fixas e não serão alterado, você pode tirar proveito de enums para tornar o código mais robusto e muito mais simples de estender. Quando você adiciona uma nova categoria, o valor de categoria correta é gerado. O evita erros de copiar e colar, ao adicionar uma nova categoria, mas se esquecer de atualizar o valor da categoria.

Abra o *Controllers\HomeController.cs* de arquivo e examine o código a seguir:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

O [enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` captura os tipos de quatro filme. O `SetViewBagMovieType` método cria a **IEnumerable&lt;SelectListItem &gt;**  do `eMovieCategories` **enum**e define o `Selected` propriedade a partir de `selectedMovie` parâmetro. O `SelectCategoryEnum` método de ação usa a mesma exibição como o `SelectCategory` método de ação.

Navegue até a página de teste e clique no `Select Movie Category (Enum)` link. Desta vez, em vez de um valor (número) que está sendo exibido, uma cadeia de caracteres que representa o enum é exibida.

### <a name="posting-enum-values"></a>Valores de enumeração de lançamento

Formulários HTML normalmente são usados para enviar dados para o servidor. O código a seguir mostra o `HTTP GET` e `HTTP POST` versões do `SelectCategoryEnumPost` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Passando um `eMovieCategories` enum para o `POST` método, é possível extrair o valor de enumeração e a cadeia de caracteres de enum. Executar o exemplo e navegue até a página de teste. Clique no `Select Movie Category(Enum Post)` link. Selecione um tipo de filme e, em seguida, clique no botão de envio. A exibição mostra o valor e o nome do tipo de filme.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Criar um elemento Select de seção vários

O [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) auxiliar HTML renderiza HTML `<select>` elemento com o `multiple` atributo, que permite que os usuários façam várias seleções. Navegue até o link de teste, selecione o **várias selecionar país** link. A interface do usuário renderizado permite que você selecione vários países. Na imagem abaixo, Canadá e China são selecionadas.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Examinar o código de MultiSelectCountry

Examine o código a seguir do *Controllers\HomeController.cs* arquivo.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

O `GetCountries` método cria uma lista de países, em seguida, passa para o `MultiSelectList` construtor. O `MultiSelectList` sobrecarga de construtor usada no `GetCountries` método acima usa quatro parâmetros:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *itens*: um [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contendo os itens na lista. No exemplo acima, a lista de países.
2. *dataValueField*: O nome da propriedade no **IEnumerable** lista que contém o valor. No exemplo acima, o `ID` propriedade.
3. *dataTextField*: O nome da propriedade no **IEnumerable** lista que contém as informações a serem exibidas. No exemplo acima, o `name` propriedade.
4. *selectedValues*: A lista de valores selecionados.

No exemplo acima, o `MultiSelectCountry` método passa um `null` valor de alguns países, para que nenhum países seja selecionadas quando a interface do usuário é exibido. O código a seguir mostra a marcação Razor usada para renderizar o `MultiSelectCountry` exibição.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

O auxiliar HTML [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) método usado acima têm dois parâmetros, o nome da propriedade a associação de modelo e o [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) que contém as opções select e valores. O `ViewBag.YouSelected` código acima é usado para exibir os valores dos países em que você selecionou ao enviar o formulário. Examine a sobrecarga de HTTP POST do `MultiSelectCountry` método.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

O `ViewBag.YouSelected` propriedades dinâmicas contém países selecionados, obtidos para o `Countries` entrada na coleção de formulário. Nesta versão, o método GetCountries é passado uma lista dos países selecionados, portanto, quando o `MultiSelectCountry` de exibição, países selecionados sejam selecionados na interface do usuário.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Fazer um selecionar elemento amigável com o jQuery coleta escolhido plug-in

A coleta [escolhido](http://harvesthq.github.com/chosen/) jQuery plug-in pode ser adicionado a um HTML &lt;selecione&gt; elemento para criar um usuário de interface de usuário amigável. As imagens a seguir demonstram a coleta [escolhido](http://harvesthq.github.com/chosen/) plug-in jQuery com `MultiSelectCountry` exibição.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

Nas duas imagens abaixo, **Canadá** está selecionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

Na imagem acima, Canadá é selecionado e contém um **x** você pode clicar para remover a seleção. A imagem abaixo mostra Canadá, China, e Japão selecionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>O jQuery coleta escolhido plug-in de gancho

A seção a seguir é mais fácil a seguir se você tem alguma experiência com o jQuery. Se você nunca usou jQuery antes, você talvez queira tente um dos seguintes tutoriais jQuery.

- [Como funciona o jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) por [John Resig](http://ejohn.org/)
- [Guia de Introdução ao jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) por [Jörn Zaefferer](http://bassistance.de/)
- [Live exemplos de jQuery](http://codylindley.com/blogstuff/js/jquery/#) por [Cody Lindley](http://codylindley.com/)

O plug-in escolhido está incluído no início e projetos de exemplo completo que acompanham este tutorial. Para este tutorial você somente precisa usar jQuery para vinculá-lo a interface do usuário. Para usar o plug-in jQuery de coleta escolhido em um projeto do ASP.NET MVC, você deve:

1. Baixar o plug-in escolhido de [github](https://github.com/harvesthq/chosen/). Esta etapa foi feita para você.
2. Adicione a pasta escolhida para seu projeto ASP.NET MVC. Adicione os ativos de plug-in do escolhido, que você baixou na etapa anterior para a pasta escolhida. Esta etapa foi feita para você.
3. Conectar o plug-in escolhido para o **DropDownList** ou **ListBox** auxiliar HTML.

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Conectar o plug-in de escolhido para o modo de exibição MultiSelectCountry.

Abra o *Views\Home\MultiSelectCountry.cshtml* e adicione um `htmlAttributes` parâmetro para o `Html.ListBox`. O parâmetro que você irá adicionar contém um nome de classe para a lista de seleção (`@class = "chzn-select"`). O código completo é mostrado abaixo:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

No código acima, estamos adicionando o atributo HTML e o valor do atributo `class = "chzn-select"`. O caractere @ anterior a classe não tem nada a fazer com que o mecanismo de exibição Razor. `class` é um [palavra-chave c#](https://msdn.microsoft.com/library/x53a06bb.aspx). Palavras-chave c# não podem ser usadas como identificadores, a menos que elas incluem como prefixo. No exemplo acima, `@class` é um identificador válido, mas **classe** porque não é **classe** é uma palavra-chave.

Adicione referências para o *Chosen/chosen.jquery.js* e *Chosen/chosen.css* arquivos. O *Chosen/chosen.jquery.js* e implementa a funcionalidade de plug-in escolhido. O *Chosen/chosen.css* arquivo fornece o estilo. Adicione estas referências à parte inferior do *Views\Home\MultiSelectCountry.cshtml* arquivo. O código a seguir mostra como referenciar o plug-in escolhido.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Ativar o plug-in selecionado usando o nome de classe usado no **Html.ListBox** código. No exemplo acima, o nome da classe é `chzn-select`. Adicione a seguinte linha à parte inferior do *Views\Home\MultiSelectCountry.cshtml* o arquivo de exibição. Esta linha ativa o plug-in escolhido.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

A linha a seguir é a sintaxe para chamar a função pronto jQuery, que seleciona o elemento DOM com o nome da classe `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

Encapsulado conjunto retornado da chamada acima, em seguida, aplica o método escolhido (`.chosen();`), que conecta o plug-in escolhido.

O código a seguir mostra concluído *Views\Home\MultiSelectCountry.cshtml* o arquivo de exibição.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Execute o aplicativo e navegue até o `MultiSelectCountry` exibição. Tente adicionando e excluindo países. O download de exemplo fornecido contém também um `MultiCountryVM` método e o modo de exibição que implementa a funcionalidade de MultiSelectCountry usando um modo de exibição de modelo em vez de um **ViewBag**.

Na próxima seção você verá como o mecanismo do ASP.NET MVC scaffolding funciona com o **DropDownList** auxiliar.

> [!div class="step-by-step"]
> [Avançar](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
