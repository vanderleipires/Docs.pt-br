---
title: "Páginas Razor com núcleo EF - classificação, filtro, paginação - 3 de 8"
author: rick-anderson
description: "Neste tutorial, você adicionará a classificação, filtragem e paginação funcionalidade para a página usando o ASP.NET Core e o Entity Framework Core."
ms.author: riande
ms.date: 10/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 08f00e183dd8a8daa883d0b9ff15698b3a39f625
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-razor-pages-3-of-8"></a>A classificação, filtragem, paginação e agrupando - Core de EF com páginas Razor (3 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Neste tutorial, classificação, filtragem, agrupamento e paginação, a funcionalidade é adicionada.

A ilustração a seguir mostra uma página concluída. Os cabeçalhos de coluna são links clicáveis para classificar a coluna. Clicar em uma coluna de título repetidamente alterna entre crescente e decrescente de classificação.

![Página de índice de alunos](sort-filter-page/_static/paging.png)

Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

## <a name="add-sorting-to-the-index-page"></a>Adicionar uma classificação para a página de índice

Adicionar cadeias de caracteres para o *Students/Index.cshtml.cs* `PageModel` para conter os parâmetros de classificação:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


Atualização de *Students/Index.cshtml.cs* `OnGetAsync` com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

O código anterior recebe um `sortOrder` parâmetro da cadeia de consulta na URL. A URL (inclusive a cadeia de caracteres de consulta) é gerada pelo [auxiliar de marca de âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

O `sortOrder` parâmetro é "Name" ou "Data". O `sortOrder` parâmetro é opcionalmente seguido por "_desc" para especificar a ordem decrescente. A ordem de classificação crescente é padrão.

Quando a página de índice é solicitada do **alunos** link, não há nenhuma cadeia de caracteres de consulta. Os alunos são exibidos em ordem crescente pelo sobrenome. Ordem crescente pelo sobrenome é o padrão (caso leva a algo) no `switch` instrução. Quando o usuário clica em um link de título de coluna, apropriado `sortOrder` valor é fornecido no valor de cadeia de caracteres de consulta.

`NameSort`e `DateSort` são usados pela página Razor para configurar os hiperlinks de título de coluna com os valores de cadeia de caracteres de consulta apropriada:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

O código a seguir contém o c# [?: operador](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

A primeira linha especifica que, quando `sortOrder` é nulo ou vazio, `NameSort` é definido como "name_desc". Se `sortOrder` é **não** nulo ou vazio, `NameSort` é definido como uma cadeia de caracteres vazia.

O `?: operator` também é conhecido como o operador ternário.

Essas duas instruções habilitar o modo de exibição definir a coluna de hiperlinks do título da seguinte maneira:

| Ordem de classificação atual | Hiperlink do último nome | Hiperlink de data |
|:--------------------:|:-------------------:|:--------------:|
| Último nome em ordem crescente | descending        | ascending      |
| Último nome em ordem decrescente | ascending           | ascending      |
| Data em ordem crescente       | ascending           | descending     |
| Data em ordem decrescente      | ascending           | ascending      |

O método usa LINQ to Entities para especificar a coluna para classificar por. O código inicializa um `IQueryable<Student> ` antes da instrução switch e ela modifica a instrução switch:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-)]

 Quando um`IQueryable` é criado ou modificado, nenhuma consulta é enviada para o banco de dados. A consulta não é executada até que o `IQueryable` objeto é convertido em uma coleção. `IQueryable`são convertidos em uma coleção, chamando um método como `ToListAsync`. Portanto, o `IQueryable` código resulta em uma única consulta que não é executada até que a instrução a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync`foi possível obter detalhado com um grande número de colunas.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Adicionar hiperlinks de título de coluna para a exibição do índice do aluno

Substitua o código em *Students/Index.cshtml*, com o seguinte realçado código:

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

O código anterior:

* Adiciona hiperlinks para o `LastName` e `EnrollmentDate` títulos de coluna.
* Usa as informações no `NameSort` e `DateSort` para configurar hiperlinks com os valores de ordem de classificação atual.

Para verificar se a classificação funciona:

* Execute o aplicativo e selecione o **alunos** guia.
* Clique em **Sobrenome**.
* Clique em **data de inscrição**.

Para obter um melhor entendimento do código:

* Em *Student/Index.cshtml.cs*, defina um ponto de interrupção `switch (sortOrder)`.
* Adicione uma inspeção para `NameSort` e `DateSort`.
* Em *Student/Index.cshtml*, defina um ponto de interrupção `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Percorra o depurador.

## <a name="add-a-search-box-to-the-students-index-page"></a>Adicione uma caixa de pesquisa para a página de índice de alunos

Para adicionar a filtragem para a página de índice de alunos:

* Uma caixa de texto e um botão de envio é adicionado à página Razor. A caixa de texto fornece uma cadeia de caracteres de pesquisa no nome do primeiro ou último.
* O arquivo code-behind é atualizado para usar o valor da caixa de texto.

### <a name="add-filtering-functionality-to-the-index-method"></a>Adicionar a funcionalidade de filtragem para o método de índice

Atualização de *Students/Index.cshtml.cs* `OnGetAsync` com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

O código anterior:

* Adiciona o `searchString` parâmetro para o `OnGetAsync` método. O valor de cadeia de caracteres de pesquisa é recebido em uma caixa de texto que é adicionada na próxima seção.
* Adicionado à instrução LINQ um `Where` cláusula. O `Where` cláusula seleciona somente os alunos cujo primeiro nome ou sobrenome contém a cadeia de caracteres de pesquisa. A instrução LINQ é executada somente se houver um valor de pesquisa.

Observação: As código chamadas anteriores a `Where` método em um `IQueryable` objeto e o filtro é processado no servidor. Em alguns cenários, tha aplicativo pode chamar o `Where` método como um método de extensão em uma coleção de memória. Por exemplo, suponha que `_context.Students` muda de núcleo EF `DbSet` para um método de repositório que retorna um `IEnumerable` coleção. O resultado normalmente é o mesmo, mas em alguns casos pode ser diferente.

Por exemplo, a implementação do .NET Framework do `Contains` executa uma comparação diferencia maiusculas de minúsculas por padrão. No SQL Server, `Contains` essa função é determinada pela configuração de agrupamento de instância do SQL Server. SQL Server padrão para maiusculas de minúsculas. `ToUpper`poderia ser chamado para fazer o teste explicitamente maiusculas de minúsculas:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

O código anterior garantiria que resultados diferenciam maiusculas de minúsculas se o código for alterado para usar `IEnumerable`. Quando `Contains` é chamado em um `IEnumerable` coleção, o .NET Core implementação é usada. Quando `Contains` é chamado em um `IQueryable` do objeto, a implementação do banco de dados é usada. Retornando um `IEnumerable` de um repositório pode ter um penality significativos de desempenho:

1. Todas as linhas são retornadas do servidor de banco de dados.
1. O filtro é aplicado a todas as linhas retornadas no aplicativo.

Há uma penalidade de desempenho para chamar `ToUpper`. O `ToUpper` código adiciona uma função na cláusula WHERE da instrução SELECT TSQL. A função adicionada impede que o otimizador de uso de um índice. Considerando que o SQL é instalado como maiusculas e minúsculas, é melhor evitar o `ToUpper` chamar quando ela não for necessária.

### <a name="add-a-search-box-to-the-student-index-view"></a>Adicione uma caixa de pesquisa para o modo de exibição de índice do aluno

Em *Views/Student/Index.cshtml*, adicione o seguinte código realçado para criar um **pesquisa** botão e o chrome variado.

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

O código anterior usa a `<form>` [auxiliar de marca](xref:mvc/views/tag-helpers/intro) para adicionar a caixa de texto de pesquisa e o botão. Por padrão, o `<form>` auxiliar de marca envia dados de formulário com um POST. Com o POST, os parâmetros são passados no corpo da mensagem HTTP e não na URL. Quando o HTTP GET é usado, os dados de formulário são passados na URL como cadeias de caracteres de consulta. Passar os dados com cadeias de caracteres de consulta permite aos usuários indicar a URL. O [diretrizes W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) recomendável que obtenha deve ser usada quando a ação não resulta em uma atualização.

Teste o aplicativo:

* Selecione o **alunos** guia e insira uma cadeia de caracteres de pesquisa.
* Selecione **pesquisa**.

Observe que a URL contém a cadeia de caracteres de pesquisa.

```html
http://localhost:5000/Students?SearchString=an
```

Se a página estiver indicada, o indicador contém a URL para a página e o `SearchString` cadeia de caracteres de consulta. O `method="get"` no `form` marca é o que causou a cadeia de caracteres de consulta a ser gerado.

Atualmente, quando um link de classificação de cabeçalho de coluna for selecionado, o filtro de valor da **pesquisa** caixa é perdida. O valor do filtro perdidas é corrigido na próxima seção.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Adicionar a funcionalidade de paginação para a página de índice de alunos

Nesta seção, um `PaginatedList` classe é criada para dar suporte à paginação. O `PaginatedList` classe usa `Skip` e `Take` instruções para filtrar os dados no servidor em vez de recuperar todas as linhas da tabela. A ilustração a seguir mostra os botões de paginação.

![Os alunos índice página com links de paginação](sort-filter-page/_static/paging.png)

Na pasta do projeto, criar `PaginatedList.cs` com o código a seguir:

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

O `CreateAsync` método no código anterior usa o tamanho da página e o número da página e aplica as `Skip` e `Take` instruções para o `IQueryable`. Quando `ToListAsync` é chamado de `IQueryable`, ele retorna uma lista que contém somente a página solicitada. As propriedades `HasPreviousPage` e `HasNextPage` são usados para habilitar ou desabilitar **anterior** e **próximo** botões de paginação.

O `CreateAsync` método é usado para criar o `PaginatedList<T>`. Um construtor não é possível criar o `PaginatedList<T>` do objeto, construtores não podem executar código assíncrono.

## <a name="add-paging-functionality-to-the-index-method"></a>Adicionar a funcionalidade de paginação para o método de índice

Em *Students/Index.cshtml.cs*, atualize o tipo de `Student` de `IList<Student>` para `PaginatedList<Student>`:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Atualização de *Students/Index.cshtml.cs* `OnGetAsync` com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-)]

O código acima adiciona o índice de página atual `sortOrder`e o `currentFilter` à assinatura do método.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Todos os parâmetros forem nulos quando:

* A página é chamada a partir de **alunos** link.
* O usuário ainda não clicou uma paginação ou link de classificação.

Quando um link de paginação é clicado, a variável de índice da página contém o número da página para exibir.

`CurrentSort`fornece o Razor de página com a ordem de classificação atual. A ordem de classificação atual deve ser incluída nos links de paginação para manter a ordem de classificação durante a paginação.

`CurrentFilter`fornece o Razor de página com a cadeia de caracteres do filtro atual. O `CurrentFilter` valor:

* Deve ser incluído nos links de paginação para manter as configurações de filtro durante a paginação.
* Deve ser restaurado para a caixa de texto quando a página é exibida novamente.

Se a cadeia de caracteres de pesquisa é alterada durante a paginação, a página será redefinida para 1. A página deve ser redefinido para 1, porque o novo filtro pode resultar em dados diferentes a serem exibidos. Quando um valor de pesquisa é inserido e **enviar** é selecionado:

* A cadeia de caracteres de pesquisa é alterada.
* O `searchString` parâmetro não é nulo.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

O `PaginatedList.CreateAsync` método converte a consulta do aluno em uma única página de alunos em um tipo de coleção que oferece suporte à paginação. Página única de alunos é passada para a página do Razor.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

Os dois pontos de interrogação na `PaginatedList.CreateAsync` representar o [operador união null](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator). O operador de união null define um valor padrão para um tipo anulável. A expressão `(pageIndex ?? 1)` significa retorna o valor de `pageIndex` se ele tiver um valor. Se `pageIndex` não tem um valor, retornam 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Adicionar links de paginação ao aluno Razor de página

Atualizar a marcação em *Students/Index.cshtml*. As alterações são realçadas:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-)]

Os links de cabeçalho de coluna usam a cadeia de caracteres de consulta para passar a cadeia de caracteres de pesquisa atual para o `OnGetAsync` método para que o usuário pode classificar nos resultados do filtro:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

Os botões de paginação são exibidos por auxiliares de marca:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

Executar o aplicativo e navegue até a página de alunos.

* Para tornar-se de que funciona de paginação, clique nos links de paginação em diferentes ordens de classificação.
* Para verificar que a paginação funciona corretamente com a classificação e filtragem, insira uma cadeia de caracteres de pesquisa e tente a paginação.

![Os alunos índice página com links de paginação](sort-filter-page/_static/paging.png)

Para obter um melhor entendimento do código:

* Em *Student/Index.cshtml.cs*, defina um ponto de interrupção `switch (sortOrder)`.
* Adicione uma inspeção para `NameSort`, `DateSort`, `CurrentSort`, e `Model.Student.PageIndex`.
* Em *Student/Index.cshtml*, defina um ponto de interrupção `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Percorra o depurador.

## <a name="update-the-about-page-to-show-student-statistics"></a>Atualize a página sobre para exibir as estatísticas do aluno

Nesta etapa, *Pages/About.cshtml* é atualizada para exibir quantos alunos registrados para cada data de registro. A atualização usa o agrupamento e inclui as seguintes etapas:

* Crie uma classe de modelo de exibição para os dados usados pelo **sobre** página.
* Modificar o arquivo code-behind e sobre a página do Razor.

### <a name="create-the-view-model"></a>Criar o modelo de exibição

Criar um *SchoolViewModels* pasta o *modelos* pasta.

No *SchoolViewModels* pasta, adicione um *EnrollmentDateGroup.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-code-behind-page"></a>Atualizar a página de code-behind sobre

Atualização de *Pages/About.cshtml.cs* arquivo com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/About.cshtml.cs)]

A instrução LINQ agrupa as entidades de alunos por data de inscrição, calcula o número de entidades em cada grupo e armazena os resultados em uma coleção de `EnrollmentDateGroup` exibir objetos de modelo.

Observação: O LINQ `group` comando não é suportado atualmente por núcleo EF. No código acima, todos os registros do aluno são retornados do SQL Server. O `group` comando será aplicado no aplicativo páginas Razor, não no SQL Server. EF Core 2.1 oferecerá suporte a este LINQ `group` operador e o agrupamento ocorre no SQL Server. Consulte [relacional: suporte a tradução GroupBy () para o SQL](https://github.com/aspnet/EntityFrameworkCore/issues/2341). [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) será lançada com o .NET Core 2.1. Para obter mais informações, consulte o [.NET Core roteiro](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="modify-the-about-razor-page"></a>Modificar o sobre Razor de página

Substitua o código no *Views/Home/About.cshtml* arquivo com o código a seguir:

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

Execute o aplicativo e navegue até a página sobre. A contagem de alunos para cada data de registro é exibida em uma tabela.

Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Sobre a página](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Recursos adicionais

* [Depuração de origem do ASP.NET Core 2.x](https://github.com/aspnet/Docs/issues/4155)

No tutorial próximo, o aplicativo usa as migrações para atualizar o modelo de dados.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/crud)
[Próximo](xref:data/ef-rp/migrations)
