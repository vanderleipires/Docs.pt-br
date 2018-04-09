---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Examinando os métodos de edição e exibição de edição | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: a3baa8e9af572d4c21813218ba394715a6db65cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Examinando os métodos de edição e exibição de edição
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Nesta seção, você examinará gerado `Edit` métodos de ação e modos de exibição para o controlador do filme. Mas primeiro, levará um desvio curto para tornar a data de lançamento aparência. Abra o *Models\Movie.cs* de arquivos e adicionar as linhas destacadas mostradas abaixo:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Você também pode fazer a cultura de data específico como este:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Abordaremos [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) no próximo tutorial. O atributo [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) especifica o que deve ser exibido no nome de um campo (neste caso, “Release Date” em vez de “ReleaseDate”). O [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo especifica o tipo de dados, nesse caso é uma data, portanto, as informações de hora armazenadas no campo não são exibidas. O [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo é necessário para um bug no navegador Chrome que renderiza os formatos de data incorretamente.

Execute o aplicativo e navegue até o `Movies` controlador. Mantenha o ponteiro do mouse sobre um **editar** link para ver a URL que ela está vinculada.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

O **editar** link foi gerado pelo `Html.ActionLink` método o *Views\Movies\Index.cshtml* exibição:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

O `Html` objeto é um auxiliar que é exposto usando uma propriedade no [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) classe base. O `ActionLink` método do auxiliar facilita gerar dinamicamente os hiperlinks HTML com links para os métodos de ação em controladores. O primeiro argumento para o `ActionLink` método é o texto do link para renderizar (por exemplo, `<a>Edit Me</a>`). O segundo argumento é o nome do método de ação para invocar (nesse caso, o `Edit` ação). O argumento final é um [objeto anônimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que gera os dados de rota (nesse caso, a ID de 4).

O link gerado mostrado na imagem anterior é `http://localhost:1234/Movies/Edit/4`. A rota padrão (estabelecidas na *aplicativo\_Start\RouteConfig.cs*) usa o padrão de URL `{controller}/{action}/{id}`. Portanto, o ASP.NET converte `http://localhost:1234/Movies/Edit/4` em uma solicitação para o `Edit` método de ação a `Movies` controlador com o parâmetro `ID` igual a 4. Examine o código a seguir do *aplicativo\_Start\RouteConfig.cs* arquivo. O [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) método é usado para rotear solicitações HTTP para o método correto de controlador e ação e forneça o parâmetro ID opcional. O [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) método também é usado pelo [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) como `ActionLink` para gerar URLs, considerando o controlador, o método de ação e quaisquer dados de rota.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Você também pode passar parâmetros de método de ação usando uma cadeia de caracteres de consulta. Por exemplo, a URL `http://localhost:1234/Movies/Edit?ID=3` também passa o parâmetro `ID` de 3 para o `Edit` método de ação a `Movies` controlador.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Abra o `Movies` controlador. Os dois `Edit` métodos de ação são mostrados abaixo.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Observe se o segundo método de ação `Edit` é precedido pelo atributo `HttpPost`. Esse atributo especifica que a sobrecarga do `Edit` método pode ser chamado somente para solicitações POST. Você pode aplicar o `HttpGet` editar o atributo para o primeiro método, mas que não é necessário porque é o padrão. (Vamos nos referir a métodos de ação que recebem implicitamente a `HttpGet` como `HttpGet` métodos.) O [associar](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) atributo é outro mecanismo de segurança importante que impede que hackers excesso enviar dados a seu modelo. Você somente deve incluir propriedades do atributo de associação que você deseja alterar. Você pode ler sobre overposting e o atributo de associação no meu [mais Observação de segurança](https://go.microsoft.com/fwlink/?LinkId=317598). No modelo simple usado neste tutorial, haverá a ligação todos os dados no modelo. O [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) atributo é usado para evitar a falsificação de uma solicitação e é associado `@Html.AntiForgeryToken()` no arquivo de exibição de edição (*Views\Movies\Edit.cshtml*), uma parte é mostrada abaixo:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` gera um token antifalsificação oculto do formulário que deve corresponder a `Edit` método o `Movies` controlador. Você pode ler mais sobre sites solicitação forjada (também conhecido como XSRF ou CSRF) meu tutorial [XSRF/CSRF prevenção no MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

O `HttpGet` `Edit` método usa o parâmetro de ID de filme, pesquise o filme usando o Entity Framework `Find` método e retorna o filme selecionado para o modo de exibição de edição. Se não for encontrado um filme, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) é retornado. Quando o sistema de scaffolding criou a exibição de Edição, ele examinou a classe `Movie` e o código criado para renderizar os elementos `<label>` e `<input>` de cada propriedade da classe. O exemplo a seguir mostra a exibição de edição que foi gerada pelo sistema de scaffolding do visual studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Observe como o modelo de exibição tem um `@model MvcMovie.Models.Movie` instrução na parte superior do arquivo – Especifica que o modo de exibição espera que o modelo para o modelo de exibição ser do tipo `Movie`.

O código de scaffolding usa várias *métodos auxiliares* para simplificar a marcação HTML. O [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) auxiliar exibe o nome do campo (&quot;título&quot;, &quot;ReleaseDate&quot;, &quot;gênero&quot;, ou &quot;preço &quot;). O [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) auxiliar renderiza uma marca HTML `<input>` elemento. O [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) auxiliar exibe quaisquer mensagens de validação associadas a essa propriedade.

Execute o aplicativo e navegue até o */Movies* URL. Clique em um link **Editar**. No navegador, exiba a origem da página. O HTML para o elemento de formulário é mostrado abaixo.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

O `<input>` elementos estão em um HTML `<form>` elemento cujo `action` atributo está definido para enviar para o *filmes/editar* URL. Os dados do formulário serão lançados com o servidor quando o **salvar** botão é clicado. A segunda linha mostra oculto [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token gerado pelo `@Html.AntiForgeryToken()` chamar.

## <a name="processing-the-post-request"></a>Processando a solicitação POST

A lista a seguir mostra a versão `HttpPost` do método de ação `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

O [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) atributo valida o [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token gerado pelo `@Html.AntiForgeryToken()` chamar no modo de exibição.

O [associador de modelo do ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) usa os valores de formulário postado e cria um `Movie` objeto que é passado como o `movie` parâmetro. O método `ModelState.IsValid` verifica se os dados enviados no formulário podem ser usados para modificar (editar ou atualizar) um objeto `Movie`. Se os dados são válidos, os dados do filme será salvo o `Movies` coleção da `db(MovieDBContext` instância). Os novos dados de filme é salvo no banco de dados chamando o `SaveChanges` método `MovieDBContext`. Depois de salvar os dados, o código redireciona o usuário para o método de ação `Index` da classe `MoviesController`, que exibe a coleção de filmes, incluindo as alterações feitas recentemente.

Assim que a validação do lado do cliente determina que os valores de um campo não são válidos, uma mensagem de erro é exibida. Se você desabilitar o JavaScript, você não terá a validação do lado do cliente, mas o servidor detectará os valores postados não são válidos e os valores de formulário serão reexibidos com mensagens de erro. Posteriormente no tutorial, podemos examinar mais detalhadamente a validação.

O `Html.ValidationMessageFor` auxiliares no *Edit.cshtml* exibição modelo cuidam da exibição de mensagens de erro apropriado.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Todos os `HttpGet` métodos seguem um padrão semelhante. Eles obtêm um objeto de filme (ou lista de objetos, no caso de `Index`) e passar o modelo para o modo de exibição. O `Create` método passa um objeto de filme vazio para o modo de exibição de criar. Todos os métodos que criam, editam, excluem ou, de outro modo, modificam dados fazem isso na sobrecarga `HttpPost` do método. Modificando dados em um método HTTP GET é um risco de segurança, conforme descrito na entrada de postagem de blog [ASP.NET MVC dica 46 – não use Excluir Links porque eles criar vulnerabilidades de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modificando dados em um método GET também viola as práticas recomendadas HTTP e a arquitetura [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) padrão, que especifica que as solicitações GET não devem alterar o estado do seu aplicativo. Em outras palavras, a execução de uma operação GET deve ser uma operação segura que não tem efeitos colaterais e não modifica os dados persistentes.

Se você estiver usando um computador de inglês dos EUA, você pode ignorar esta seção e vá para o tutorial a próximo. Você pode baixar a versão Globalize deste tutorial [aqui](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Para obter um tutorial de duas partes excelente sobre internacionalização, consulte [ASP.NET MVC 5 internacionalização do Nadeem](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> para dar suporte a validação jQuery para idiomas diferentes do inglês que usam uma vírgula (&quot;,&quot;) para um ponto decimal e formatos de data diferente do inglês dos EUA, você deve incluir *globalize.js* e específicos  *cultures/globalize.cultures.js* arquivo (de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`. Você pode obter a validação do jQuery diferentes do inglês do NuGet. (Não instale Globalize se você estiver usando uma localidade em inglês.)


1. Do **ferramentas** menu clique **NuGetLibrary Package Manager**e, em seguida, clique em **gerenciar pacotes NuGet para solução**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. No painel esquerdo, selecione <strong>procurar*.</strong>* (Consulte a imagem abaixo).
3. Na caixa de entrada, insira * Globalize * *.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Escolha `jQuery.Validation.Globalize`, escolha `MvcMovie` e clique em **instalar**. O *Scripts\jquery.globalize\globalize.js* arquivo será adicionado ao seu projeto. O *Scripts\jquery.globalize\cultures\* conterá muitos arquivos JavaScript de cultura. Observe que pode levar cinco minutos para instalar este pacote.

   O código a seguir mostra as modificações no arquivo Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Para evitar repetir este código em cada modo de exibição de edição, você pode movê-la para o arquivo de layout. Para otimizar o download de script, consulte o tutorial [empacotamento e minimização](../../performance/bundling-and-minification.md).

Para obter mais informações, consulte [internacionalização do ASP.NET MVC 3](http://afana.me/post/aspnet-mvc-internationalization.aspx) e [internacionalização de 3 do ASP.NET MVC - parte 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Como uma correção temporária, se você não conseguir validação trabalhando em sua localidade, você pode forçar o computador para usar inglês (EUA) ou você pode desabilitar o JavaScript no navegador. Para forçar o computador para usar inglês (EUA), você pode adicionar o elemento de globalização para a raiz de projetos *Web. config* arquivo. O código a seguir mostra o elemento de globalização com a cultura definida como inglês dos Estados Unidos.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> O seguinte tutorial, implementaremos a funcionalidade de pesquisa.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Próximo](adding-search.md)
