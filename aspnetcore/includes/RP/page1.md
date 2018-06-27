# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Páginas do Razor geradas por scaffolding no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial examina as Páginas do Razor criadas por scaffolding no tutorial anterior. 

[Exiba ou baixe](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) a amostra.

## <a name="the-create-delete-details-and-edit-pages"></a>As páginas Criar, Excluir, Detalhes e Editar.

Examine o Modelo de Página, *Pages/Movies/Index.cshtml.cs*:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

As Páginas do Razor são derivadas de `PageModel`. Por convenção, a classe derivada de `PageModel` é chamada de `<PageName>Model`. O construtor usa [injeção de dependência](xref:fundamentals/dependency-injection) para adicionar o `MovieContext` à página. Todas as páginas geradas por scaffolding seguem esse padrão. Consulte [Código assíncrono](xref:data/ef-rp/intro#asynchronous-code) para obter mais informações sobre a programação assíncrona com o Entity Framework.

Quando uma solicitação é feita à página, o método `OnGetAsync` retorna uma lista de filmes para a Página do Razor. `OnGetAsync` ou `OnGet` é chamado em uma Página do Razor para inicializar o estado da página. Nesse caso, `OnGetAsync` obtém uma lista de filmes e os exibe. 

Quando `OnGet` retorna `void` ou `OnGetAsync` retorna `Task`, então nenhum método de retorno é usado. Quando o tipo de retorno for `IActionResult` ou `Task<IActionResult>`, é necessário fornecer uma instrução de retorno. Por exemplo, o método `OnPostAsync` do arquivo *Pages/Movies/Create.cshtml.cs*:

<!-- TODO - replace with snippet
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```

Examine a Página do Razor *Pages/Movies/Index.cshtml*:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

O Razor pode fazer a transição do HTML em C# ou em marcação específica do Razor. Quando um símbolo `@` é seguido por uma [palavra-chave reservada do Razor](xref:mvc/views/razor#razor-reserved-keywords), ele faz a transição para marcação específica do Razor, caso contrário, ele faz a transição para C#.

A diretiva do Razor `@page` transforma o arquivo em uma ação do MVC &mdash;, o que significa que ele pode manipular as solicitações. `@page` deve ser a primeira diretiva do Razor em uma página. `@page` é um exemplo de transição para a marcação específica do Razor. Consulte [Sintaxe Razor](xref:mvc/views/razor#razor-syntax) para obter mais informações.

Examine a expressão lambda usada no auxiliar HTML a seguir:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

O auxiliar HTML `DisplayNameFor` inspeciona a propriedade `Title` referenciada na expressão lambda para determinar o nome de exibição. A expressão lambda é inspecionada em vez de avaliada. Isso significa que não há nenhuma violação de acesso quando `model`, `model.Movie` ou `model.Movie[0]` são `null` ou vazios. Quando a expressão lambda é avaliada (por exemplo, com `@Html.DisplayFor(modelItem => item.Title)`), os valores de propriedade do modelo são avaliados.

<a name="md"></a>
### <a name="the-model-directive"></a>A diretiva @model

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

A diretiva `@model` especifica o tipo de modelo passado para a Página do Razor. No exemplo anterior, a linha `@model` torna a classe derivada de `PageModel` disponível para a Página do Razor. O modelo é usado nos [auxiliares HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` e `@Html.DisplayName` na página.

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData e layout

Considere o código a seguir:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

O código realçado anterior é um exemplo de transição do Razor para C#. Os caracteres `{` e `}` circunscrevem um bloco de código C#.

A classe base `PageModel` tem uma propriedade de dicionário `ViewData` que pode ser usada para adicionar os dados que você deseja passar para uma exibição. Você adiciona objetos ao dicionário `ViewData` usando um padrão de chave/valor. No exemplo anterior, a propriedade "Title" é adicionada ao dicionário `ViewData`. 

::: moniker range="= aspnetcore-2.0"

A propriedade "Título" é usada no arquivo *Pages/_Layout.cshtml*. A marcação a seguir mostra as primeiras linhas do arquivo *Pages/_Layout.cshtml*.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

A propriedade "Título" é usada no arquivo *Pages/_Layout.cshtml*. A marcação a seguir mostra as primeiras linhas do arquivo *Pages/_Layout.cshtml*.

::: moniker-end

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

A linha `@*Markup removed for brevity.*@` é um comentário do Razor. Ao contrário de comentários HTML (`<!-- -->`), comentários do Razor não são enviados ao cliente.

Execute o aplicativo e teste os links no projeto (**Início**, **Sobre**, **Contato**, **Criar**, **Editar** e **Excluir**). Cada página define o título, que pode ser visto na guia do navegador. Quando você coloca um indicador em uma página, o título é usado para o indicador. *Pages/Index.cshtml* e *Pages/Movies/Index.cshtml* atualmente têm o mesmo título, mas você pode modificá-los para terem valores diferentes.

> [!NOTE]
> Talvez você não consiga inserir casas decimais ou vírgulas no campo `Price`. Para dar suporte à [validação do jQuery](https://jqueryvalidation.org/) para localidades de idiomas diferentes do inglês que usam uma vírgula (“,”) para um ponto decimal e formatos de data diferentes do inglês dos EUA, você deve tomar medidas para globalizar o aplicativo. Veja [Problema 4076 do GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) para obter instruções sobre como adicionar casas decimais.

A propriedade `Layout` é definida no arquivo *Pages/_ViewStart.cshtml*:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

A marcação anterior define o arquivo de layout *Pages/_Layout.cshtml* para todos os arquivos do Razor na pasta *Pages*. Veja [Layout](xref:mvc/razor-pages/index#layout) para obter mais informações.

### <a name="update-the-layout"></a>Atualizar o layout

Altere o elemento `<title>` no arquivo *Pages/_Layout.cshtml* para usar uma cadeia de caracteres mais curta.

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

Localizar o elemento de âncora a seguir no arquivo *Pages/_Layout.cshtml*.

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
Substitua o elemento anterior pela marcação a seguir.

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

O elemento de âncora anterior é um [Auxiliar de Marcas](xref:mvc/views/tag-helpers/intro). Nesse caso, ele é o [Auxiliar de Marcas de Âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). O atributo e valor do auxiliar de marcas `asp-page="/Movies/Index"` cria um link para a Página do Razor `/Movies/Index`.

Salve suas alterações e teste o aplicativo clicando no link **RpMovie**. Consulte o arquivo [cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) no GitHub.

### <a name="the-create-page-model"></a>O modelo Criar página

Examine o modelo de página *Pages/Movies/Create.cshtml.cs*:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]
::: moniker-end


O método `OnGet` inicializa qualquer estado necessário para a página. A página Criar não tem nenhum estado para inicializar, assim, `Page` é retornado. Mais adiante no tutorial, você verá o estado de inicialização do método `OnGet`. O método `Page` cria um objeto `PageResult` que renderiza a página *Create.cshtml*.

A propriedade `Movie` usa o atributo `[BindProperty]` para aceitar a [associação de modelos](xref:mvc/models/model-binding). Quando o formulário Criar posta os valores de formulário, o tempo de execução do ASP.NET Core associa os valores postados ao modelo `Movie`.

O método `OnPostAsync` é executado quando a página posta dados de formulário:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Se há algum erro de modelo, o formulário é reexibido juntamente com quaisquer dados de formulário postados. A maioria dos erros de modelo podem ser capturados no lado do cliente antes do formulário ser enviado. Um exemplo de um erro de modelo é postar, para o campo de data, um valor que não pode ser convertido em uma data. Falaremos sobre a validação do lado do cliente e a validação de modelo posteriormente no tutorial.

Se não há nenhum erro de modelo, os dados são salvos e o navegador é redirecionado à página Índice.

### <a name="the-create-razor-page"></a>A Página do Razor Criar

Examine o arquivo na Página do Razor *Pages/Movies/Create.cshtml*:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
