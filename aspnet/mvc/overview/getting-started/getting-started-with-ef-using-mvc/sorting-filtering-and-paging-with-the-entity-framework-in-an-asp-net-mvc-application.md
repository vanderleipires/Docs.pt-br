---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Classificação, filtragem e paginação com o Entity Framework em um aplicativo ASP.NET MVC | Microsoft Docs"
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8d11bf47f8c43040ef30d7132f0bb756748dbacd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Classificação, filtragem e paginação com o Entity Framework em um aplicativo ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


No tutorial anterior, você implementou um conjunto de páginas da web para operações CRUD básicas para `Student` entidades. Neste tutorial você adicionará a classificação, filtragem e funcionalidade de paginação para o **alunos** página de índice. Você também criará uma página que faz o agrupamento simples.

A ilustração a seguir mostra a aparência de página quando terminar. Os cabeçalhos de coluna são links que o usuário pode clicar para classificar por essa coluna. Clicar em uma coluna de título repetidamente alterna entre crescente e decrescente de classificação.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Adicionar Links de classificação da coluna para a página de índice de alunos

Para adicionar uma classificação para a página de índice do aluno, você alterará a `Index` método o `Student` controlador e adicione código para o `Student` indexa a exibição.

### <a name="add-sorting-functionality-to-the-index-method"></a>Adicionar a funcionalidade para o método de índice de classificação

Em *Controllers\StudentController.cs*, substitua o `Index` método com o código a seguir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Esse código recebe um `sortOrder` parâmetro da cadeia de consulta na URL. O valor de cadeia de caracteres de consulta é fornecido pelo ASP.NET MVC, como um parâmetro para o método de ação. O parâmetro será uma cadeia de caracteres que é o "Nome" ou "Data", opcionalmente seguido por um sublinhado e a cadeia de caracteres "desc" para especificar a ordem decrescente. A ordem de classificação crescente é padrão.

Na primeira vez em que a página de índice é solicitada, não há nenhuma cadeia de caracteres de consulta. Os alunos são exibidos em ordem crescente em `LastName`, que é o padrão, conforme estabelecido pelo caso leva a algo no `switch` instrução. Quando o usuário clica em um hiperlink de título de coluna, apropriado `sortOrder` valor é fornecido na cadeia de caracteres de consulta.

Os dois `ViewBag` variáveis são usadas para que o modo de exibição pode configurar os hiperlinks de título de coluna com os valores de cadeia de caracteres de consulta apropriada:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Essas são instruções ternários. A primeira delas Especifica que o `sortOrder` parâmetro é nulo ou vazio, `ViewBag.NameSortParm` deve ser definido como "nome\_desc"; caso contrário, ele deve ser definido como uma cadeia de caracteres vazia. Essas duas instruções habilitar o modo de exibição definir a coluna de hiperlinks do título da seguinte maneira:

| Ordem de classificação atual | Hiperlink do último nome | Hiperlink de data |
| --- | --- | --- |
| Último nome em ordem crescente | descending | ascending |
| Último nome em ordem decrescente | ascending | ascending |
| Data em ordem crescente | ascending | descending |
| Data em ordem decrescente | ascending | ascending |

Usa o método [LINQ to Entities](https://msdn.microsoft.com/en-us/library/bb386964.aspx) para especificar a coluna para classificar por. O código cria um [IQueryable](https://msdn.microsoft.com/en-us/library/bb351562.aspx) variável antes do `switch` instrução, modifica-lo no `switch` instrução e chama o `ToList` método após o `switch` instrução. Quando você criar e modificar `IQueryable` variáveis, nenhuma consulta é enviada para o banco de dados. A consulta não é executada até que você converta o `IQueryable` objeto em uma coleção, chamando um método como `ToList`. Portanto, esse código resulta em uma única consulta que não é executada até que o `return View` instrução.

Como uma alternativa a escrita de instruções LINQ diferentes para cada ordem de classificação, você pode criar dinamicamente uma instrução LINQ. Para obter informações sobre o LINQ dinâmico, consulte [LINQ dinâmico](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Adicionar cabeçalho hiperlinks para o modo de exibição do aluno índice da coluna

Em *Views\Student\Index.cshtml*, substitua o `<tr>` e `<th>` elementos para a linha de cabeçalho com o código:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Esse código usa as informações de `ViewBag` valores de cadeia de caracteres de propriedades para configurar hiperlinks com a consulta apropriada.

Execute a página e clique no **Sobrenome** e **data de inscrição** títulos de coluna para verificar essa classificação funciona.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Depois de clicar no **Sobrenome** título, alunos são exibidos em decrescente último nome.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Adicione uma caixa de pesquisa para a página de índice de alunos

Para adicionar a filtragem para a página de índice de alunos, você adicionará uma caixa de texto e um botão de envio para o modo de exibição e fazer as alterações correspondentes no `Index` método. A caixa de texto permitirá que você insira uma cadeia de caracteres a ser pesquisado no nome e o último campos de nome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Adicionar a funcionalidade de filtragem para o método de índice

Em *Controllers\StudentController.cs*, substitua o `Index` método com o código a seguir (as alterações são realçadas):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Você adicionou um `searchString` parâmetro para o `Index` método. O valor de cadeia de caracteres de pesquisa é recebido em uma caixa de texto que você adicionará à exibição de índice. Você adicionou à instrução LINQ também um `where` cláusula que seleciona somente os alunos cujo primeiro nome ou sobrenome contém a cadeia de caracteres de pesquisa. A instrução que adiciona o [onde](https://msdn.microsoft.com/en-us/library/bb535040.aspx) cláusula é executada somente se houver um valor de pesquisa.

> [!NOTE]
> Em muitos casos, você pode chamar o mesmo método em um conjunto de entidades do Entity Framework ou como um método de extensão em uma coleção de memória. Os resultados são normalmente o mesmo, mas em alguns casos podem ser diferentes.
> 
> Por exemplo, a implementação do .NET Framework do `Contains` método retorna todas as linhas quando você passar uma cadeia de caracteres vazia para ele, mas o provedor do Entity Framework para SQL Server Compact 4.0 retorna zero linhas de cadeias de caracteres vazias. Portanto o código de exemplo (colocando o `Where` instrução dentro de um `if` instrução) torna-se de que tenha os mesmos resultados para todas as versões do SQL Server. Além disso, a implementação do .NET Framework do `Contains` método executa uma comparação que diferencia maiusculas de minúsculas por padrão, mas provedores do Entity Framework SQL Server executam comparações de maiusculas e minúsculas por padrão. Portanto, ao chamar o `ToUpper` método para fazer o teste de maiusculas e minúsculas explicitamente garante que eles não são alterados quando você alterar o código mais tarde para usar um repositório, que retornará um `IEnumerable` coleção em vez de um `IQueryable` objeto. (Quando você chama o `Contains` método em um `IEnumerable` coleção, você obtém a implementação do .NET Framework; quando chamá-lo em um `IQueryable` do objeto, você obtém a implementação de provedor de banco de dados.)
> 
> Tratamento de nulos também pode ser diferente para provedores de banco de dados diferente ou quando você usa um `IQueryable` objeto comparado a quando você usa um `IEnumerable` coleção. Por exemplo, em alguns cenários um `Where` condição como `table.Column != 0` pode não retornar colunas que tenham `null` como o valor. Para obter mais informações, consulte [tratamento incorreto de nulos variáveis na cláusula 'where'](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Adicione uma caixa de pesquisa para o modo de exibição de índice do aluno

Em *Views\Student\Index.cshtml*, adicione o código realçado imediatamente antes de abertura `table` marca para criar uma legenda, uma caixa de texto e um **pesquisa** botão.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Execute a página, insira uma cadeia de caracteres de pesquisa e clique em **pesquisa** para verificar se a filtragem está funcionando.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Observe que a URL não contém a "uma" cadeia de caracteres, que significa que se você marcar essa página, você não obterá a lista filtrada quando você usa o indicador. Isso se aplica também os links de classificação da coluna, como eles serão classificados toda a lista. Você alterará a **pesquisa** botão usar cadeias de caracteres de consulta para os critérios de filtro no tutorial posteriormente.

## <a name="add-paging-to-the-students-index-page"></a>Adicionar a paginação para a página de índice de alunos

Para adicionar a paginação para a página de índice de alunos, comece instalando o **PagedList.Mvc** pacote NuGet. Em seguida, você poderá fazer alterações adicionais no `Index` método e adicionar links de paginação para o `Index` exibição. **PagedList.Mvc** é um dos muitos paginação boa e a classificação de pacotes para o ASP.NET MVC e seu uso aqui se destina apenas como um exemplo, não como uma recomendação para ele por outras opções. A ilustração a seguir mostra os links de paginação.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instale o pacote PagedList.MVC NuGet

O NuGet **PagedList.Mvc** pacote instala automaticamente o **PagedList** pacote como uma dependência. O **PagedList** pacote instala um `PagedList` métodos de tipo e a extensão de coleção para `IQueryable` e `IEnumerable` coleções. Os métodos de extensão criam uma única página de dados em um `PagedList` coleção fora de sua `IQueryable` ou `IEnumerable`e o `PagedList` coleção fornece várias propriedades e métodos que facilitam a paginação. O **PagedList.Mvc** pacote instala um auxiliar de paginação que exibe os botões de paginação.

Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote** e **Package Manager Console**.

No **Package Manager Console** janela, verifique se o **origem do pacote** é **nuget.org** e o **projeto padrão** é **ContosoUniversity**e, em seguida, digite o seguinte comando:

`Install-Package PagedList.Mvc`

![Instalar PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Compile o projeto. 

### <a name="add-paging-functionality-to-the-index-method"></a>Adicionar a funcionalidade de paginação para o método de índice

Em *Controllers\StudentController.cs*, adicione um `using` instrução para o `PagedList` namespace:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Substitua o método `Index` pelo seguinte código:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Esse código adiciona um `page` parâmetro, um parâmetro de ordem de classificação atual e um parâmetro de filtro atual para a assinatura do método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Na primeira vez em que a página é exibida, ou se o usuário ainda não clicou uma paginação ou link de classificação, todos os parâmetros será nulos. Se um link de paginação é clicado, o `page` variável conterá o número da página para exibir.

Um `ViewBag` propriedade fornece a exibição com a ordem de classificação atual, porque isso deve ser incluído nos links de paginação para manter a ordem de classificação igual de paginação:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Outra propriedade, `ViewBag.CurrentFilter`, fornece a exibição com a cadeia de caracteres do filtro atual. Esse valor deve ser incluído nos links de paginação para manter as configurações de filtro durante a paginação e deve ser restaurado para a caixa de texto quando a página é exibida novamente. Se a cadeia de caracteres de pesquisa for alterada durante a paginação, a página deve ser redefinido como 1, porque o novo filtro pode resultar em dados diferentes a serem exibidos. A cadeia de caracteres de pesquisa é alterada quando um valor é inserido na caixa de texto e o botão de envio é pressionado. Nesse caso, o `searchString` parâmetro não é nulo.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

No final do método, o `ToPagedList` método de extensão no alunos `IQueryable` objeto converte a consulta do aluno em uma única página de alunos em um tipo de coleção que oferece suporte à paginação. Página única de alunos é então passada para o modo de exibição:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

O `ToPagedList` método usa um número de página. Os dois pontos de interrogação representam o [operador união null](https://msdn.microsoft.com/en-us/library/ms173224.aspx). O operador de união null define um valor padrão para um tipo anulável. a expressão `(page ?? 1)` significa retorna o valor de `page` se ele tem um valor ou retornará 1 se `page` é nulo.

### <a name="add-paging-links-to-the-student-index-view"></a>Adicionar Links de paginação para o modo de exibição de índice do aluno

Em *Views\Student\Index.cshtml*, substitua o código existente com o código a seguir. As alterações são realçadas.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

O `@model` instrução na parte superior da página especifica o modo de exibição agora obtém um `PagedList` do objeto, em vez de um `List` objeto.

O `using` instrução `PagedList.Mvc` fornece acesso para o auxiliar do MVC para os botões de paginação.

O código usa uma sobrecarga [BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que permite especificar [FormMethod.Get](https://msdn.microsoft.com/en-us/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

O padrão [BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) envia dados de formulário com uma POSTAGEM, o que significa que parâmetros são passados no corpo da mensagem HTTP e não na URL como cadeias de caracteres de consulta. Quando você especificar HTTP GET, os dados do formulário são passados na URL como cadeias de caracteres de consulta, que permite aos usuários indicar a URL. O [diretrizes do W3C para o uso de HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recomendável que você deve usar um GET quando a ação não resulta em uma atualização.

A caixa de texto é inicializada com a cadeia de caracteres de pesquisa atual para que quando você clica em uma nova página, você pode ver a cadeia de caracteres de pesquisa atual.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Os links de cabeçalho de coluna usam a cadeia de caracteres de consulta para passar a cadeia de caracteres de pesquisa atual para o controlador para que o usuário pode classificar nos resultados do filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

O número atual de página e o total de páginas é exibido.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Se não houver nenhuma página para exibir, "Página 0 de 0" é mostrada. (Nesse caso, o número da página é maior do que a contagem de páginas porque `Model.PageNumber` é 1, e `Model.PageCount` é 0.)

Os botões de paginação são exibidos pelo `PagedListPager` auxiliar:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

O `PagedListPager` auxiliar fornece várias opções que você pode personalizar, incluindo URLs e estilo. Para obter mais informações, consulte [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) no site do GitHub.

Execute a página.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Clique nos links de paginação em ordens de classificação diferente para tornar-se de que funciona de paginação. Em seguida, digite uma cadeia de caracteres de pesquisa e tente paginação novamente para confirmar se paginação também funciona corretamente com a classificação e filtragem.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Criar um sobre a página que mostra as estatísticas do aluno

Universidade de Contoso do site sobre a página, você exibirá quantas alunos registrados para cada data de registro. Isso exige cálculos simples e agrupamento nos grupos. Para fazer isso, você fará o seguinte:

- Crie uma classe de modelo de exibição para os dados que você precisa passar para o modo de exibição.
- Modificar o `About` método o `Home` controlador.
- Modificar o `About` exibição.

### <a name="create-the-view-model"></a>Criar o modelo de exibição

Criar um *ViewModels* pasta na pasta do projeto. Nessa pasta, adicionar um arquivo de classe *EnrollmentDateGroup.cs* e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificar controlador principal

Em *HomeController*, adicione o seguinte `using` instruções na parte superior do arquivo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Adicione uma variável de classe para o contexto do banco de dados imediatamente após a chave de abertura para a classe:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Substitua o método `About` pelo seguinte código:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

A instrução LINQ agrupa as entidades de alunos por data de inscrição, calcula o número de entidades em cada grupo e armazena os resultados em uma coleção de `EnrollmentDateGroup` exibir objetos de modelo.

Adicionar um `Dispose` método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificar a sobre o modo de exibição

Substitua o código no *Views\Home\About.cshtml* arquivo com o código a seguir:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Executar o aplicativo e clique no **sobre** link. A contagem de alunos para cada data de registro é exibida em uma tabela.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Resumo

Neste tutorial, você viu como criar um modelo de dados e implementar CRUD básica, classificação, filtragem, paginação e funcionalidade de agrupamento. O seguinte tutorial, você começará a examinar os tópicos mais avançados, expandindo o modelo de dados.

Deixe comentários em como você gostou neste tutorial e nós poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Próximo](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
