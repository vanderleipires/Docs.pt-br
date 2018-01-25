---
title: "Núcleo do ASP.NET MVC com núcleo EF - classificação, filtro, paginação - 3 de 10"
author: tdykstra
description: "Neste tutorial, você adicionará a classificação, filtragem e paginação funcionalidade para a página usando o ASP.NET Core e o Entity Framework Core."
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 60ac1844e7747002d72aa892a47490cb7a416359
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>A classificação, filtragem, paginação e agrupando - Core de EF com o tutorial do MVC do ASP.NET Core (3 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

No tutorial anterior, você implementou um conjunto de páginas da web para operações CRUD básicas para entidades do aluno. Neste tutorial você adicionará a classificação, filtragem e funcionalidade de paginação para a página de índice de alunos. Você também criará uma página que faz o agrupamento simples.

A ilustração a seguir mostra a aparência de página quando terminar. Os cabeçalhos de coluna são links que o usuário pode clicar para classificar por essa coluna. Clicar em uma coluna de título repetidamente alterna entre crescente e decrescente de classificação.

![Página de índice de alunos](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Adicionar Links de classificação da coluna para a página de índice de alunos

Para adicionar uma classificação para a página de índice do aluno, você alterará o `Index` método do controlador alunos e adicione o código para o modo de exibição do índice do aluno.

### <a name="add-sorting-functionality-to-the-index-method"></a>Adicionar a funcionalidade de classificação para o método de índice

Em *StudentsController.cs*, substitua o `Index` método com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Esse código recebe um `sortOrder` parâmetro da cadeia de consulta na URL. O valor de cadeia de caracteres de consulta é fornecido pelo MVC do ASP.NET Core como um parâmetro para o método de ação. O parâmetro será uma cadeia de caracteres que é o "Nome" ou "Data", opcionalmente seguido por um sublinhado e a cadeia de caracteres "desc" para especificar a ordem decrescente. A ordem de classificação crescente é padrão.

Na primeira vez em que a página de índice é solicitada, não há nenhuma cadeia de caracteres de consulta. Os alunos são exibidos em ordem crescente pelo sobrenome, o que é o padrão, conforme estabelecido pelo caso leva a algo no `switch` instrução. Quando o usuário clica em um hiperlink de título de coluna, apropriado `sortOrder` valor é fornecido na cadeia de caracteres de consulta.

Os dois `ViewData` elementos (NameSortParm e DateSortParm) são usados pelo modo de exibição para configurar os hiperlinks de título de coluna com os valores de cadeia de caracteres de consulta apropriada.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Essas são instruções ternários. A primeira delas Especifica que o `sortOrder` parâmetro é nulo ou vazio, NameSortParm deve ser definido como "name_desc"; caso contrário, ele deve ser definido como uma cadeia de caracteres vazia. Essas duas instruções habilitar o modo de exibição definir a coluna de hiperlinks do título da seguinte maneira:

|  Ordem de classificação atual  | Hiperlink do último nome | Hiperlink de data |
|:--------------------:|:-------------------:|:--------------:|
| Último nome em ordem crescente  | descending          | ascending      |
| Último nome em ordem decrescente | ascending           | ascending      |
| Data em ordem crescente       | ascending           | descending     |
| Data em ordem decrescente      | ascending           | ascending      |

O método usa LINQ to Entities para especificar a coluna para classificar por. O código cria um `IQueryable` variável antes da instrução switch, modifica-lo a instrução switch e chama o `ToListAsync` método após o `switch` instrução. Quando você criar e modificar `IQueryable` variáveis, nenhuma consulta é enviada para o banco de dados. A consulta não é executada até que você converta o `IQueryable` objeto em uma coleção, chamando um método como `ToListAsync`. Portanto, esse código resulta em uma única consulta que não é executada até que o `return View` instrução.

Este código poderia ficar detalhado com um grande número de colunas. [O último tutorial nesta série](advanced.md#dynamic-linq) mostra como escrever código que permite que você passe o nome do `OrderBy` coluna em uma variável de cadeia de caracteres.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Adicionar hiperlinks de título de coluna para a exibição do índice do aluno

Substitua o código em *Views/Students/Index.cshtml*, com o código a seguir para adicionar hiperlinks de título de coluna. As linhas alteradas são realçadas.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Esse código usa as informações no `ViewData` valores de cadeia de caracteres de propriedades para configurar hiperlinks com a consulta apropriada.

Executar o aplicativo, selecione o **alunos** guia e, em seguida, clique no **Sobrenome** e **data de inscrição** títulos de coluna para verificar essa classificação funciona.

![Página de índice de alunos na ordem do nome](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Adicione uma caixa de pesquisa para a página de índice de alunos

Para adicionar a filtragem para a página de índice de alunos, você adicionará uma caixa de texto e um botão de envio para o modo de exibição e fazer as alterações correspondentes no `Index` método. A caixa de texto permitirá que você insira uma cadeia de caracteres a ser pesquisado no nome e o último campos de nome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Adicionar a funcionalidade de filtragem para o método de índice

Em *StudentsController.cs*, substitua o `Index` método com o código a seguir (as alterações são realçadas).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Você adicionou um `searchString` parâmetro para o `Index` método. O valor de cadeia de caracteres de pesquisa é recebido em uma caixa de texto que você adicionará à exibição de índice. Você também adicionou à instrução LINQ um onde cláusula que seleciona somente os alunos cujo primeiro nome ou sobrenome contém a cadeia de caracteres de pesquisa. A instrução que adiciona onde cláusula é executada somente se houver um valor de pesquisa.

> [!NOTE]
> Aqui você estiver chamando o `Where` método em um `IQueryable` objeto e o filtro serão processado no servidor. Em alguns cenários você pode chamar o `Where` método como um método de extensão em uma coleção de memória. (Por exemplo, suponha que você alterar a referência ao `_context.Students` para que em vez de um EF `DbSet` faz referência a um método de repositório que retorna um `IEnumerable` coleção.) O resultado normalmente é o mesmo, mas em alguns casos pode ser diferente.
>
>Por exemplo, a implementação do .NET Framework do `Contains` método executa uma comparação que diferencia maiusculas de minúsculas por padrão, mas no SQL Server, isso é determinado pela configuração de agrupamento de instância do SQL Server. Essa configuração padrão para maiusculas de minúsculas. Você poderia chamar o `ToUpper` método para fazer o teste explicitamente maiusculas de minúsculas: *onde (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Garantiria que resultados permaneçam o mesmo se você alterar o código mais tarde para usar um repositório que retorna um `IEnumerable` coleção em vez de um `IQueryable` objeto. (Quando você chama o `Contains` método em um `IEnumerable` coleção, você obtém a implementação do .NET Framework; quando chamá-lo em um `IQueryable` do objeto, você obtém a implementação de provedor de banco de dados.) No entanto, há uma penalidade de desempenho para essa solução. O `ToUpper` código colocar uma função na cláusula WHERE da instrução SELECT TSQL. Que possam impedir que o otimizador de uso de um índice. Considerando que o SQL geralmente é instalado como maiusculas e minúsculas, é melhor evitar o `ToUpper` código até você migrar para um repositório de dados diferencia maiusculas de minúsculas.

### <a name="add-a-search-box-to-the-student-index-view"></a>Adicione uma caixa de pesquisa para o modo de exibição de índice do aluno

Em *Views/Student/Index.cshtml*, adicione o código realçado imediatamente antes da abertura tag da tabela para criar uma legenda, uma caixa de texto e um **pesquisa** botão.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Esse código usa o `<form>` [auxiliar de marca](xref:mvc/views/tag-helpers/intro) para adicionar a caixa de texto de pesquisa e o botão. Por padrão, o `<form>` auxiliar de marca envia dados de formulário com uma POSTAGEM, o que significa que parâmetros são passados no corpo da mensagem HTTP e não na URL como cadeias de caracteres de consulta. Quando você especificar HTTP GET, os dados do formulário são passados na URL como cadeias de caracteres de consulta, que permite aos usuários indicar a URL. É recomendável o W3C diretrizes que você deve usar obter quando a ação não resulta em uma atualização.

Executar o aplicativo, selecione o **alunos** guia, insira uma cadeia de caracteres de pesquisa e clique em Pesquisar para verificar se a filtragem está funcionando.

![Página de índice de alunos com filtragem](sort-filter-page/_static/filtering.png)

Observe que a URL contém a cadeia de caracteres de pesquisa.

```html
http://localhost:5813/Students?SearchString=an
```

Se você marcar essa página, você obterá a lista filtrada quando você usa o indicador. Adicionando `method="get"` para o `form` marca é o que causou a cadeia de caracteres de consulta a ser gerado.

Neste estágio, se você clicar em um link de classificação de cabeçalho de coluna perderá o valor do filtro que você inseriu no **pesquisa** caixa. Isso será corrigido na próxima seção.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Adicionar a funcionalidade de paginação para a página de índice de alunos

Para adicionar a paginação para a página de índice de alunos, você criará uma `PaginatedList` classe que usa `Skip` e `Take` instruções para filtrar os dados no servidor em vez de recuperar sempre todas as linhas da tabela. Em seguida, você poderá fazer alterações adicionais no `Index` método e adicione botões de paginação para o `Index` exibição. A ilustração a seguir mostra os botões de paginação.

![Os alunos índice página com links de paginação](sort-filter-page/_static/paging.png)

Na pasta do projeto, criar `PaginatedList.cs`e, em seguida, substitua o código de modelo com o código a seguir.

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

O `CreateAsync` método nesse código usa o tamanho da página e o número da página e aplica as `Skip` e `Take` instruções para o `IQueryable`. Quando `ToListAsync` é chamado de `IQueryable`, ela retornará uma lista que contém somente a página solicitada. As propriedades `HasPreviousPage` e `HasNextPage` pode ser usado para habilitar ou desabilitar **anterior** e **próximo** botões de paginação.

Um `CreateAsync` método é usado em vez de um construtor para criar o `PaginatedList<T>` porque construtores não podem executar código assíncrono do objeto.

## <a name="add-paging-functionality-to-the-index-method"></a>Adicionar a funcionalidade de paginação para o método de índice

Em *StudentsController.cs*, substitua o `Index` método com o código a seguir.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Esse código adiciona um parâmetro de número de página, um parâmetro de ordem de classificação atual e um parâmetro de filtro atual para a assinatura do método.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

Na primeira vez em que a página é exibida, ou se o usuário ainda não clicou uma paginação ou link de classificação, todos os parâmetros será nulos.  Se um link de paginação é clicado, a variável de página conterá o número da página para exibir.

O `ViewData` elemento chamado CurrentSort fornece a exibição com a ordem de classificação atual, porque isso deve ser incluído nos links de paginação para manter a ordem de classificação igual de paginação.

O `ViewData` elemento chamado FiltroAtual fornece a exibição com a cadeia de caracteres do filtro atual. Esse valor deve ser incluído nos links de paginação para manter as configurações de filtro durante a paginação e deve ser restaurado para a caixa de texto quando a página é exibida novamente.

Se a cadeia de caracteres de pesquisa for alterada durante a paginação, a página deve ser redefinido como 1, porque o novo filtro pode resultar em dados diferentes a serem exibidos. A cadeia de caracteres de pesquisa é alterada quando um valor é inserido na caixa de texto e o botão de envio é pressionado. Nesse caso, o `searchString` parâmetro não é nulo.

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

No final do `Index` método, o `PaginatedList.CreateAsync` método converte a consulta do aluno em uma única página de alunos em um tipo de coleção que oferece suporte à paginação. Página única de alunos é então passada para o modo de exibição.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

O `PaginatedList.CreateAsync` método usa um número de página. Os dois pontos de interrogação representar o operador de união de null. O operador de união null define um valor padrão para um tipo anulável. a expressão `(page ?? 1)` significa retorna o valor de `page` se ele tem um valor ou retornará 1 se `page` é nulo.

## <a name="add-paging-links-to-the-student-index-view"></a>Adicionar links de paginação para o modo de exibição do índice do aluno

Em *Views/Students/Index.cshtml*, substitua o código existente com o código a seguir. As alterações são realçadas.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

O `@model` instrução na parte superior da página especifica o modo de exibição agora obtém um `PaginatedList<T>` do objeto, em vez de um `List<T>` objeto.

Os links de cabeçalho de coluna usam a cadeia de caracteres de consulta para passar a cadeia de caracteres de pesquisa atual para o controlador para que o usuário pode classificar nos resultados do filtro:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Os botões de paginação são exibidos por auxiliares de marca:

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Execute o aplicativo e vá para a página de alunos.

![Os alunos índice página com links de paginação](sort-filter-page/_static/paging.png)

Clique nos links de paginação em ordens de classificação diferente para tornar-se de que funciona de paginação. Em seguida, digite uma cadeia de caracteres de pesquisa e tente paginação novamente para confirmar se paginação também funciona corretamente com a classificação e filtragem.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Criar uma página sobre que mostra as estatísticas do aluno

Para o site do Contoso University **sobre** página, você exibirá quantas alunos registrados para cada data de registro. Isso exige cálculos simples e agrupamento nos grupos. Para fazer isso, você fará o seguinte:

* Crie uma classe de modelo de exibição para os dados que você precisa passar para o modo de exibição.

* Modifique o método sobre no controlador Home.

* Modificar o modo de exibição sobre.

### <a name="create-the-view-model"></a>Criar o modelo de exibição

Criar um *SchoolViewModels* pasta o *modelos* pasta.

Na nova pasta, adicionar um arquivo de classe *EnrollmentDateGroup.cs* e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Modificar controlador principal

Em *HomeController*, adicione o seguinte usando instruções na parte superior do arquivo:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Adicionar uma variável de classe para o contexto do banco de dados imediatamente após a chave de abertura para a classe e obter uma instância do contexto do ASP.NET Core DI:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Substitua o método `About` pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

A instrução LINQ agrupa as entidades de alunos por data de inscrição, calcula o número de entidades em cada grupo e armazena os resultados em uma coleção de `EnrollmentDateGroup` exibir objetos de modelo.
> [!NOTE] 
> Na 1.0 versão do Entity Framework Core, o conjunto de resultados inteiro será retornado ao cliente e agrupamento é feito no cliente. Em alguns cenários, isso pode criar problemas de desempenho. Certifique-se de testar o desempenho com os volumes de dados de produção e, se for necessário usar SQL bruto para fazer o agrupamento do servidor. Para obter informações sobre como usar o SQL não processada, consulte [o último tutorial nesta série](advanced.md).

### <a name="modify-the-about-view"></a>Modificar a sobre o modo de exibição

Substitua o código no *Views/Home/About.cshtml* arquivo com o código a seguir:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Execute o aplicativo e vá para a página sobre. A contagem de alunos para cada data de registro é exibida em uma tabela.

![Sobre a página](sort-filter-page/_static/about.png)

## <a name="summary"></a>Resumo

Neste tutorial, você viu como realizar a classificação, filtragem, paginação e agrupamento. O seguinte tutorial, você aprenderá como lidar com as alterações do modelo de dados usando as migrações.

>[!div class="step-by-step"]
[Anterior](crud.md)
[Próximo](migrations.md)  
