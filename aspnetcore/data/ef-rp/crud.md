---
title: "Páginas Razor com núcleo EF - CRUD - 2 de 8"
author: rick-anderson
description: "Mostra como criar, ler, atualizar, excluir com núcleo de EF"
keywords: ASP.NET Core, Entity Framework Core, CRUD, criar, ler, atualizar, excluir
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: b4b24c155c29a0ef8ffffda752253f56097e50ed
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>Criar, ler, atualizar e excluir - Core EF com páginas Razor (2 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Neste tutorial, o scaffolding CRUD (criar, ler, atualizar e excluir) código é revisado e personalizado.

Observação: Para minimizar a complexidade e manter esses tutoriais que voltadas EF Core, código de EF principal é usado nos arquivos code-behind páginas Razor. Alguns desenvolvedores usam um padrão de repositório ou camada de serviço em para criar uma camada de abstração entre a interface do usuário (páginas Razor) e a camada de acesso a dados.

Neste tutorial, criar, editar, excluir e páginas Razor detalhes de *aluno* pasta são modificadas.

O código de scaffolding usa o padrão a seguir para criar, editar e excluir páginas:

* Obter e exibir os dados solicitados com o método HTTP GET `OnGetAsync`.
* Salvar as alterações dos dados com o método HTTP POST `OnPostAsync`.

As páginas de índice e detalhes de obtém e exibem os dados solicitados com o método HTTP GET`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Substitua SingleOrDefaultAsync FirstOrDefaultAsync

O código gerado usa [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) para buscar a entidade solicitada. [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) é mais eficiente em busca de uma entidade:

* A menos que o código precisa verificar se não há mais de uma entidade retornada da consulta. 
* `SingleOrDefaultAsync`busca mais dados e funciona o desnecessária.
* `SingleOrDefaultAsync`gera uma exceção se houver mais de uma entidade que se ajusta a parte do filtro.
*  `FirstOrDefaultAsync`não gerar se houver mais de uma entidade que se ajusta a parte do filtro.

Substituir globalmente `SingleOrDefaultAsync` com `FirstOrDefaultAsync`. `SingleOrDefaultAsync`é usado em 5 locais:

* `OnGetAsync`Na página de detalhes.
* `OnGetAsync`e `OnPostAsync` nas páginas de editar e excluir.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

Em grande parte do código scaffolding, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) pode ser usado no lugar de `FirstOrDefaultAsync` ou `SingleOrDefaultAsync`. 

`FindAsync`:

* Localiza uma entidade com a chave primária (PK). Se uma entidade com o PK está sendo controlada pelo contexto, ele é retornado sem uma solicitação para o banco de dados.
* É simples e conciso.
* É otimizado para pesquisar uma única entidade.
* Pode ter benefícios de desempenho em algumas situações, mas eles raramente entram em jogo para cenários web normal.
* Usa implicitamente [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) em vez de [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Mas se você deseja incluir outras entidades, encontrar não é mais apropriado. Isso significa que talvez seja necessário abandoná-localizar e mover para uma consulta à medida que avança seu aplicativo.

## <a name="customize-the-details-page"></a>Personalizar a página de detalhes

Navegue até `Pages/Students` página. O **editar**, **detalhes**, e **excluir** links são gerados pelo [auxiliar de marca de âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) no *páginas/alunos / Cshtml* arquivo.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Selecione um link de detalhes. É a URL do formulário `http://localhost:5000/Students/Details?id=2`. A identificação do aluno é passada usando uma cadeia de caracteres de consulta (`?id=2`).

Atualizar a edição, os detalhes e excluir páginas do Razor para usar o `"{id:int}"` modelo de rota. Altere a diretiva de página de cada uma dessas páginas de `@page` para `@page "{id:int}"`.

Uma solicitação para a página com o modelo de rota "{id: int}" que faz **não** incluem um número inteiro rota valor retorna um HTTP 404 (não encontrado) erro. Por exemplo, `http://localhost:5000/Students/Details` retornará um erro 404. Para tornar a ID opcional, acrescente `?` à restrição de rota:

 ```cshtml
@page "{id:int?}"
```

Executar o aplicativo, clique em um link de detalhes e verifique se a URL está passando a ID como dados de rota (`http://localhost:5000/Students/Details/2`).

Não altere globalmente `@page` para `@page "{id:int}"`, fazendo interrompe os links para a página inicial e criar páginas.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Adicionar dados relacionados

O código de scaffolding para a página de índice de alunos não inclui o `Enrollments` propriedade. Nesta seção, o conteúdo do `Enrollments` coleção é exibida na página de detalhes.

O `OnGetAsync` método *Pages/Students/Details.cshtml.cs* usa o `FirstOrDefaultAsync` método para recuperar um único `Student` entidade. Adicione o seguinte código:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

O `Include` e `ThenInclude` métodos fazer com que o contexto para carregar o `Student.Enrollments` propriedade de navegação e dentro de cada registro de `Enrollment.Course` propriedade de navegação. Esses métodos são examinied em detalhes no tutorial relacionadas à leitura de dados.

O `AsNoTracking` método melhora o desempenho em cenários quando as entidades retornadas não são atualizadas no contexto atual. `AsNoTracking`é abordada posteriormente neste tutorial.

### <a name="display-related-enrollments-on-the-details-page"></a>Exibir registros relacionados na página de detalhes

Abra *Pages/Students/Details.cshtml*. Adicione o seguinte código realçado para exibir uma lista de registros:

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Se o recuo do código está errado depois que o código será colado, pressione CTRL-K-D para corrigi-lo.

O código anterior percorre as entidades de `Enrollments` propriedade de navegação. Para cada registro, ele exibe o nome do curso e a classificação. O título do curso é recuperado da entidade curso que é armazenada no `Course` propriedade de navegação de entidade de registros.

Executar o aplicativo, selecione o **alunos** guia e, em seguida, clique no **detalhes** link para um aluno. A lista de cursos e notas do aluno selecionado é exibida.

## <a name="update-the-create-page"></a>Atualizar a página de criação

Atualização de `OnPostAsync` método *Pages/Students/Create.cshtml.cs* com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Examine o [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) código:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

No código anterior, `TryUpdateModelAsync<Student>` tenta atualizar o `emptyStudent` objeto usando os valores de formulário postado do [conteúdo de página](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) propriedade o [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync`atualiza apenas as propriedades listadas (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

No exemplo anterior:

* O segundo argumento (` "student", // Prefix`) é o prefixo usa para procurar valores. Não diferencia maiusculas de minúsculas.
* Os valores de formulário postado são convertidos para tipos no `Student` modelo usando [associação de modelo](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Mais

Usando `TryUpdateModel` atualizar os campos com valores postados é uma prática recomendada de segurança porque evita que overposting. Por exemplo, suponha que a entidade do aluno inclui um `Secret` que esta página da web não deve atualizar ou adicionar:

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Mesmo que o aplicativo não tiver um `Secret` campo ao criar/atualizar Razor de página, um hacker pode definir o `Secret` valor por mais. Um hacker pode usar uma ferramenta como o Fiddler ou escrever alguns JavaScript, postar um `Secret` formar o valor. O código original não limita os campos que o associador de modelo usa quando ele cria uma instância do aluno.

O valor que o hacker especificado para o `Secret` campo de formulário é atualizado no banco de dados. A imagem a seguir mostra a adição de ferramenta Fiddler o `Secret` campo (com o valor "OverPost") para os valores de formulário postado.

![Campo secreto adição do Fiddler](../ef-mvc/crud/_static/fiddler.png)

O valor "OverPost" foi adicionado com êxito para o `Secret` propriedade da linha inserida. O designer do aplicativo nunca se destina a `Secret` propriedade a ser definida com a página de criação.

<a name="vm"></a>
### <a name="view-model"></a>Modelo de exibição

Um modelo de exibição normalmente contém um subconjunto das propriedades incluídas no modelo usado pelo aplicativo. O modelo de aplicativo é geralmente chamado de modelo de domínio. O modelo de domínio normalmente contém todas as propriedades necessárias pela entidade correspondente no banco de dados. O modelo de exibição contém apenas as propriedades necessárias para a camada de interface do usuário (por exemplo, a página Criar). Além do modelo de exibição, alguns aplicativos usam um modelo de associação ou o modelo de entrada para passar dados entre a classe code-behind páginas Razor e o navegador. Considere o seguinte `Student` modelo de exibição:

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

Exibir modelos fornecem uma maneira alternativa para evitar overposting. O modelo de exibição contém apenas as propriedades para exibir (exibição) ou atualizar.

O código a seguir usa o `StudentVM` modelo de exibição para criar um novo aluno:

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

O [SetValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) método define os valores do objeto lendo os valores de outro [PropertyValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) objeto. `SetValues`usa a correspondência de nomes de propriedade. O tipo de modelo de exibição não precisa estar relacionado ao tipo de modelo, ele só precisa ter as propriedades que correspondem.

Usando `StudentVM` requer [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) ser atualizado para usar `StudentVM` em vez de `Student`.

Nas páginas Razor, o `PageModel` classe derivada é o modelo de exibição. 

## <a name="update-the-edit-page"></a>Atualizar a página de edição

Atualize o arquivo de code-behind de página de edição:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

As alterações de código são semelhantes a página de criação, com algumas exceções:

* `OnPostAsync`tem um `id` parâmetro.
* O aluno atual é obtido do banco de dados, em vez de criar um aluno vazio.
* `FirstOrDefaultAsync`foi substituído por [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync`é uma boa opção, ao selecionar uma entidade da chave primária. Consulte [FindAsync](#FindAsync) para obter mais informações.

### <a name="test-the-edit-and-create-pages"></a>A edição de teste e criar páginas

Criar e editar algumas entidades aluno.

## <a name="entity-states"></a>Estados da entidade

Mantém o controle de contexto de banco de dados se entidades na memória estão em sincronia com suas linhas correspondentes no banco de dados. As informações de sincronização de contexto do banco de dados determinam o que acontece quando `SaveChanges` é chamado. Por exemplo, quando uma nova entidade é passada para o `Add` método, o estado da entidade definida como `Added`. Quando `SaveChanges` é chamado, o banco de dados contexto emite um comando INSERT SQL.

Uma entidade pode estar em um dos seguintes estados:

* `Added`: A entidade ainda não existir no banco de dados. O `SaveChanges` método emite uma instrução INSERT.

* `Unchanged`: Nenhuma alteração precisa ser salva com essa entidade. Uma entidade possui esse status quando ele é lido a partir do banco de dados.

* `Modified`: Foram modificados alguns ou todos os valores de propriedade da entidade. O `SaveChanges` método emite uma instrução UPDATE.

* `Deleted`: A entidade foi marcada para exclusão. O `SaveChanges` método emite uma instrução DELETE.

* `Detached`: A entidade não está sendo controlada pelo contexto de banco de dados.

Em um aplicativo de área de trabalho, as alterações de estado geralmente são definidas automaticamente. Uma entidade é leitura, as alterações são feitas e o estado da entidade a ser alterada automaticamente para `Modified`. Chamando `SaveChanges` gera uma instrução de atualização do SQL que atualiza apenas as propriedades alteradas.

Em um aplicativo web, o `DbContext` que lê uma entidade e exibe os dados for descartados depois que uma página é processada. Quando um páginas `OnPostAsync` método é chamado, é feita uma nova solicitação de web e com uma nova instância do `DbContext`. Ler novamente a entidade no novo contexto simula o processamento de área de trabalho.

## <a name="update-the-delete-page"></a>Atualizar a página de exclusão

Nesta seção, o código é adicionado para implementar um erro personalizado de mensagem quando a chamada para `SaveChanges` falhar. Adicione uma cadeia de caracteres para conter as mensagens de erro de possile:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Substitua o método `OnGetAsync` pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

O código anterior contém o parâmetro opcional `saveChangesError`. `saveChangesError`Indica se o método foi chamado após uma falha ao excluir o objeto do aluno. A operação de exclusão pode falhar devido a problemas de rede temporários. Erros de rede transitório serão mais prováveis na nuvem. `saveChangesError`é falso quando a página de exclusão `OnGetAsync` é chamado a partir da interface do usuário. Quando `OnGetAsync` é chamado pelo `OnPostAsync` (porque a operação de exclusão falhou), o `saveChangesError` parâmetro for true.

### <a name="the-delete-pages-onpostasync-method"></a>O método de OnPostAsync de páginas de exclusão

Substitua o `OnPostAsync` com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

O código anterior recupera a entidade selecionada, em seguida, chama o `Remove` método para definir o status da entidade `Deleted`. Quando `SaveChanges` é chamado, um SQL DELETE comando é gerado. Se `Remove` falhar:

* A exceção de banco de dados é detectada.
* As páginas de exclusão `OnGetAsync` método for chamado com `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Atualizar a página de exclusão Razor

Adicione a seguinte mensagem de erro realçado para excluir a página de Razor.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Exclusão de teste.

## <a name="common-errors"></a>Erros comuns

Aluno/Home ou outros links não funcionam:

Verifique se a página Razor contém corretas `@page` diretiva. Por exemplo, o aluno/Home Page de Razor deve **não** contêm um modelo de rota:

```cshtml
@page "{id:int}"
```

Cada página do Razor deve incluir a `@page` diretiva.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/intro)
[Próximo](xref:data/ef-rp/sort-filter-page)
