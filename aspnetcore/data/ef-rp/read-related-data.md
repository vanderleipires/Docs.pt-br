---
title: "Páginas Razor com núcleo EF - ler dados relacionados - 6 de 8"
author: rick-anderson
description: "Neste tutorial você ler e exibe dados relacionados – ou seja, os dados que o Entity Framework carrega em Propriedades de navegação."
keywords: Une ASP.NET Core, Entity Framework Core, os dados relacionados
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ba9b17ecdcb605d39117d03230b1db37e8e4d0dd
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2017
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>Leitura relacionadas a dados - Core de EF com páginas Razor (6 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Neste tutorial, os dados relacionados é lido e exibidos. Dados relacionados são dados EF Core carrega em Propriedades de navegação.

Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

As ilustrações a seguir mostram as páginas concluídas para este tutorial:

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice instrutores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Carregamento de dados relacionados lento eager e explícita

Há várias maneiras que o EF Core pode carregar dados relacionados para as propriedades de navegação de uma entidade:

* [Carregamento adiantado](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Carregamento adiantado é quando uma consulta para um tipo de entidade também carrega entidades relacionadas. Quando a entidade é lida, seus dados relacionados são recuperados. Normalmente, isso resulta em uma consulta de junção único que recupera todos os dados que é necessário. Núcleo EF emitirá várias consultas para alguns tipos de carregamento rápido. Emitir várias consultas pode ser mais eficiente do que era o caso para algumas consultas em EF6 qual havia uma única consulta. Carregamento adiantado é especificado com o `Include` e `ThenInclude` métodos.

 ![Exemplo de carregamento rápido](read-related-data/_static/eager-loading.png)
 
 Carregamento adiantado envia várias consultas quando uma coleção nvavigation é incluído:

 * Uma consulta para a consulta principal 
 * Uma consulta para cada coleção "borda", na árvore de carga.

* Separar consultas com `Load`: os dados podem ser recuperados em consultas separadas e Core EF "correções de" as propriedades de navegação. significa "correções para cima" que o EF Core preenche automaticamente as propriedades de navegação. Separar consultas com `Load` é mais parecida com explícito de carregamento de carregamento rápido.

 ![Exemplo de consultas separadas](read-related-data/_static/separate-queries.png)

 Observação: EF Core corrige automaticamente as propriedades de navegação para outras entidades que foram previamente carregadas para a instância de contexto. Mesmo se os dados de uma propriedade de navegação são *não* explicitamente incluído, a propriedade ainda pode ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.

* [Carregamento explícito](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados. Código deve ser escrito para recuperar os dados relacionados quando ele é necessário. Carregamento explícito com consultas separadas resulta em várias consultas enviadas ao banco de dados. Carregamento explícito, o código especifica as propriedades de navegação para ser carregado. Use o `Load` método de fazer o carregamento explícito. Por exemplo:

 ![Exemplo de carregamento explícito](read-related-data/_static/explicit-loading.png)

* [Carregamento preguiçoso](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [Núcleo EF não oferece suporte a carregamento lento](https://github.com/aspnet/EntityFrameworkCore/issues/3797). Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados. Na primeira vez que uma propriedade de navegação é acessada, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Uma consulta é enviada para o banco de dados sempre que uma propriedade de navegação seja acessada pela primeira vez.

* O `Select` operador carrega somente os dados relacionados necessários.

## <a name="create-a-courses-page-that-displays-department-name"></a>Criar uma página de cursos que exibe o nome do departamento

A entidade de curso inclui uma propriedade de navegação que contém o `Department` entidade. O `Department` entidade contém o departamento de curso é atribuído a.

Para exibir o nome do departamento atribuído em uma lista de cursos:

* Obter o `Name` propriedade o `Department` entidade.
* O `Department` entidade vem do `Course.Department` propriedade de navegação.

![ourse. Departamento](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>O modelo de curso o Scaffold

* Sair do Visual Studio.
* Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).
* Execute o seguinte comando:

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

Os scaffolds comando anterior a `Course` modelo. Abra o projeto no Visual Studio.

Compile o projeto. A compilação gera erros, como o seguinte:

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Globalmente altere `_context.Course` para `_context.Courses` (ou seja, adicionar um "s" para `Course`). 7 ocorrências forem encontradas e atualizadas.

Abra *Pages/Courses/Index.cshtml.cs* e examine o `OnGetAsync` método. O mecanismo de scaffolding especificado carregamento rápido para o `Department` propriedade de navegação. O `Include` método especifica carregamento rápido.

Execute o aplicativo e selecione o **cursos** link. Exibe a coluna do departamento a `DepartmentID`, que não é útil.

Atualize o método `OnGetAsync` pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Adiciona o código anterior `AsNoTracking`. `AsNoTracking`melhora o desempenho porque as entidades retornadas não são rastreadas. As entidades não são rastreadas porque eles não são atualizados no contexto atual.

Atualização *Views/Courses/Index.cshtml* com a seguinte marcação realçada:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

As seguintes alterações foram feitas para o código de scaffolding:

* Alterado o título do índice para cursos.
* Adicionado um **número** coluna mostra o `CourseID` o valor da propriedade. Por padrão, as chaves primárias não são Scaffold porque normalmente eles não fazem sentidos para os usuários finais. No entanto, nesse caso a chave primária é significativa.
* Alterado o **departamento** coluna para exibir o nome de departamento. Exibe o código de `Name` propriedade do `Department` entidade que é carregada no `Department` propriedade de navegação:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Execute o aplicativo e selecione o **cursos** guia para ver a lista com nomes de departamento.

![Página de índice de cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Ao carregar os dados com Select relacionados

O `OnGetAsync` método carrega dados relacionados com o `Include` método:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

O `Select` operador carrega somente os dados relacionados necessários. Para itens únicos, como o `Department.Name` usa uma junção interna do SQL. Para coleções ele usa outro acesso de banco de dados, mas também o.`Include` operador em coleções.

O código a seguir carrega os dados relacionados com o `Select` método:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

O `CourseViewModel`:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Consulte [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obter um exemplo completo.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Criar uma página de instrutores que mostra os cursos e registros

Nesta seção, a página de instrutores é criada.

<a name="IP"></a>
![Página de índice instrutores](read-related-data/_static/instructors-index.png)

Essa página lê e exibe dados relacionados das seguintes maneiras:

* A lista de instrutores exibe dados relacionados do `OfficeAssignment` entidade (Office na imagem anterior). O `Instructor` e `OfficeAssignment` são entidades em uma relação um-para-zero-ou-um. Carregamento adiantado é usado para o `OfficeAssignment` entidades. Carregamento adiantado é geralmente mais eficiente quando os dados relacionados que precise ser exibido. Nesse caso, as atribuições do office para os instrutores são exibidas.
* Quando o usuário seleciona um instrutor (Harui na imagem anterior), relacionado `Course` as entidades são exibidas. O `Instructor` e `Course` são entidades em uma relação muitos-para-muitos. Carregamento para rápido o `Course` entidades e suas relacionados `Department` entidades é usado. Nesse caso, consultas separadas podem ser mais eficientes porque são necessários somente o cursos para o instrutor selecionado. Este exemplo mostra como usar o carregamento rápido para propriedades de navegação em entidades que estão nas propriedades de navegação.
* Quando o usuário seleciona um curso (química na imagem anterior), dados de relacionados a `Enrollments` entidade é exibida. A imagem anterior, classificação e nome do aluno são exibidos. O `Course` e `Enrollment` são entidades em uma relação um-para-muitos.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Criar um modelo de exibição para o modo de exibição do índice do instrutor

A página de instrutores mostra dados de três tabelas diferentes. Um modelo de exibição é criado que inclui as três entidades que representam as três tabelas.

No *SchoolViewModels* pasta, criar *InstructorIndexData.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Scaffold modelo instrutor

* Sair do Visual Studio.
* Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).
* Execute o seguinte comando:

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

Os scaffolds comando anterior a `Instructor` modelo. Abra o projeto no Visual Studio.

Compile o projeto. A compilação gera erros.

Globalmente altere `_context.Instructor` para `_context.Instructors` (ou seja, adicionar um "s" para `Instructor`). 7 ocorrências forem encontradas e atualizadas.

Executar o aplicativo e navegue até a página de professores.

Substituir *Pages/Instructors/Index.cshtml.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

O `OnGetAsync` método aceita dados de rota opcional para a ID do instrutor selecionado.

Examine a consulta no *Pages/Instructors/Index.cshtml* página:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

A consulta tem duas inclui:

* `OfficeAssignment`: Exibido no [exibição instrutores](#IP).
* `CourseAssignments`: O que leva os cursos ministrada.


### <a name="update-the-instructors-index-page"></a>Atualizar a página de índice instrutores

Atualização *Pages/Instructors/Index.cshtml* com a seguinte marcação:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

A marcação anterior faz as seguintes alterações:

* Atualizações de `page` diretiva de `@page` para `@page "{id:int?}"`. `"{id:int?}"`é um modelo de rota. O modelo de rota altera cadeias de caracteres de consulta de inteiro na URL para dados de rota. Por exemplo, clicando no **selecione** link para o instrutor quando a diretiva de página produz uma URL semelhante à seguinte:

    `http://localhost:1234/Instructors?id=2`

    Quando a diretiva de página é `@page "{id:int?}"`, a URL do anterior é:

    `http://localhost:1234/Instructors/2`

* Título da página é **instrutores**.
* Adicionado um **Office** coluna que exibe `item.OfficeAssignment.Location` somente se `item.OfficeAssignment` não for nulo. Como esta é uma relação um-para-zero-ou-um, pode não haver uma entidade OfficeAssignment relacionada.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Adicionado um **cursos** coluna que exibe os cursos ministrada por cada instrutor. Consulte [explícita transição de linha com `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) para obter mais informações sobre essa sintaxe do razor.

* Código adicionado dinamicamente adiciona `class="success"` para o `tr` elemento do instrutor selecionado. Isso define uma cor de plano de fundo para a linha selecionada usando uma classe de inicialização.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Adicionado um novo hiperlink rotulado **selecione**. Este link envia a ID do instrutor selecionado para o `Index` método e define uma cor de plano de fundo.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Execute o aplicativo e selecione o **instrutores** guia. A página exibe o `Location` (office) de relacionado `OfficeAssignment` entidade. Se OfficeAssignment' é nulo, uma célula de tabela vazia será exibida.

![Página de índice instrutores que nada selecionado](read-related-data/_static/instructors-index-no-selection.png)

Clique no **selecione** link. As alterações de estilo de linha.

### <a name="add-courses-taught-by-selected-instructor"></a>Adicionar cursos ministrados por instrutor selecionado

Atualização de `OnGetAsync` método *Pages/Instructors/Index.cshtml.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

Examine a consulta atualizada:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

A consulta anterior adiciona o `Department` entidades.

O código a seguir executa quando instrutor é selecionado (`id != null`). O instrutor selecionado é recuperado da lista de instrutores no modelo de exibição. O modelo de exibição `Courses` propriedade é carregada com o `Course` entidades que instrutor `CourseAssignments` propriedade de navegação.

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

O `Where` método retorna uma coleção. Na anterior `Where` método, um único `Instructor` entidade for retornada. O `Single` método converte a coleção em uma única `Instructor` entidade. O `Instructor` entidade fornece acesso para o `CourseAssignments` propriedade. `CourseAssignments`fornece acesso a relacionado `Course` entidades.

![M:M instrutor para cursos](complex-data-model/_static/courseassignment.png)

O `Single` método é usado em uma coleção quando a coleção tem apenas um item. O `Single` método lançará uma exceção se a coleção está vazia ou se houver mais de um item. Uma alternativa é `SingleOrDefault`, que retorna um valor padrão (null nesse caso) se a coleção está vazia. Usando `SingleOrDefault` em uma coleção vazia:

* Resulta em uma exceção (de tentativa de encontrar um `Courses` propriedade em uma referência nula).
* A mensagem de exceção indicaria menos claramente a causa do problema.

O código a seguir preenche o modelo de exibição `Enrollments` propriedade quando um curso é selecionado:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Adicione a seguinte marcação para o fim do *Pages/Courses/Index.cshtml* Razor de página:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

A marcação anterior exibe uma lista de cursos relacionados ao instrutor ao instrutor está selecionado.

Teste o aplicativo. Clique em uma **selecione** link na página de professores.

![Instrutor de página de índice instrutores selecionado](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Mostrar dados do aluno

Nesta seção, o aplicativo é atualizado para mostrar os dados de aluno para um curso selecionado.

Atualizar a consulta a `OnGetAsync` método *Pages/Instructors/Index.cshtml.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Atualização *Pages/Instructors/Index.cshtml*. Adicione a seguinte marcação para o final do arquivo:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

A marcação anterior exibe uma lista dos alunos que são registrados no curso selecionado.

Atualize a página e selecionar um instrutor. Selecione um curso para ver a lista de estudantes registrados e suas classificações.

![Instrutor de página de índice professores e curso selecionado](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Usando um único

O `Single` método pode passar o `Where` condição em vez de chamar o `Where` método separadamente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

Anterior `Single` abordagem não fornece nenhuma benefícios com o uso `Where`. Alguns desenvolvedores preferem o `Single` abordagem estilo.

## <a name="explicit-loading"></a>Carregamento explícito

O código atual Especifica o carregamento rápido para `Enrollments` e `Students`:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Suponha que os usuários desejam ver registros em um curso raramente. Nesse caso, uma otimização seria carregar apenas os dados de registro se solicitado. Nesta seção, o `OnGetAsync` é atualizado para usar o carregamento explícito de `Enrollments` e `Students`.

Atualização de `OnGetAsync` com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

O código anterior elimina o *ThenInclude* chamadas de método para dados de registro e do aluno. Se um curso for selecionado, o código realçado recupera:

* O `Enrollment` entidades para o curso selecionado.
* O `Student` entidades para cada `Enrollment`.

Observe anterior código comenta `.AsNoTracking()`. Propriedades de navegação só podem ser carregadas explicitamente para entidades controladas.

Teste o aplicativo. De uma perspectiva de usuários, o aplicativo se comporta de forma idêntica para a versão anterior.

O seguinte tutorial mostra como atualizar os dados relacionados.

>[!div class="step-by-step"]
>[Anterior](xref:data/ef-rp/complex-data-model)
>[Próximo](xref:data/ef-rp/update-related-data)