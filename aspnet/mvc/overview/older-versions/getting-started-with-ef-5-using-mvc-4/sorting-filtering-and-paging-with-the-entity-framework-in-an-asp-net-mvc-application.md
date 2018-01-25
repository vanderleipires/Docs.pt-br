---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Classificação, filtragem e paginação com o Entity Framework em um aplicativo ASP.NET MVC (3 de 10) | Microsoft Docs"
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f9b68abeba19561a327bad5ee4be80d79af1a550
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Classificação, filtragem e paginação com o Entity Framework em um aplicativo ASP.NET MVC (3 de 10)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais do início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece aqui.
> 
> > [!NOTE] 
> > 
> > Se você tiver um problema que não é possível resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tentar reproduzir o problema. Geralmente, você pode encontrar a solução para o problema, comparando o seu código para o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

Usa o método [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) para especificar a coluna para classificar por. O código cria um [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variável antes do `switch` instrução, modifica-lo no `switch` instrução e chama o `ToList` método após o `switch` instrução. Quando você criar e modificar `IQueryable` variáveis, nenhuma consulta é enviada para o banco de dados. A consulta não é executada até que você converta o `IQueryable` objeto em uma coleção, chamando um método como `ToList`. Portanto, esse código resulta em uma única consulta que não é executada até que o `return View` instrução.

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

Você adicionou um `searchString` parâmetro para o `Index` método. Você adicionou à instrução LINQ também um `where` clausethat seleciona somente os alunos cujo primeiro nome ou sobrenome contém a cadeia de caracteres de pesquisa. O valor de cadeia de caracteres de pesquisa é recebido em uma caixa de texto que você adicionará à exibição de índice. A instrução que adiciona o [onde](https://msdn.microsoft.com/library/bb535040.aspx) cláusula é executada somente se houver um valor de pesquisa.

> [!NOTE]
> Em muitos casos, você pode chamar o mesmo método em um conjunto de entidades do Entity Framework ou como um método de extensão em uma coleção de memória. Os resultados são normalmente o mesmo, mas em alguns casos podem ser diferentes. Por exemplo, a implementação do .NET Framework do `Contains` método retorna todas as linhas quando você passar uma cadeia de caracteres vazia para ele, mas o provedor do Entity Framework para SQL Server Compact 4.0 retorna zero linhas de cadeias de caracteres vazias. Portanto o código de exemplo (colocando o `Where` instrução dentro de um `if` instrução) torna-se de que tenha os mesmos resultados para todas as versões do SQL Server. Além disso, a implementação do .NET Framework do `Contains` método executa uma comparação que diferencia maiusculas de minúsculas por padrão, mas provedores do Entity Framework SQL Server executam comparações de maiusculas e minúsculas por padrão. Portanto, ao chamar o `ToUpper` método para fazer o teste de maiusculas e minúsculas explicitamente garante que eles não são alterados quando você alterar o código mais tarde para usar um repositório, que retornará um `IEnumerable` coleção em vez de um `IQueryable` objeto. (Quando você chama o `Contains` método em um `IEnumerable` coleção, você obtém a implementação do .NET Framework; quando chamá-lo em um `IQueryable` do objeto, você obtém a implementação de provedor de banco de dados.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Adicione uma caixa de pesquisa para o modo de exibição de índice do aluno

Em *Views\Student\Index.cshtml*, adicione o código realçado imediatamente antes de abertura `table` marca para criar uma legenda, uma caixa de texto e um **pesquisa** botão.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Execute a página, insira uma cadeia de caracteres de pesquisa e clique em **pesquisa** para verificar se a filtragem está funcionando.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Observe que a URL não contém a "uma" cadeia de caracteres, que significa que se você marcar essa página, você não obterá a lista filtrada quando você usa o indicador. Você alterará a **pesquisa** botão usar cadeias de caracteres de consulta para os critérios de filtro no tutorial posteriormente.

## <a name="add-paging-to-the-students-index-page"></a>Adicionar a paginação para a página de índice de alunos

Para adicionar a paginação para a página de índice de alunos, comece instalando o **PagedList.Mvc** pacote NuGet. Em seguida, você poderá fazer alterações adicionais no `Index` método e adicionar links de paginação para o `Index` exibição. **PagedList.Mvc** é um dos muitos paginação boa e a classificação de pacotes para o ASP.NET MVC e seu uso aqui se destina apenas como um exemplo, não como uma recomendação para ele por outras opções. A ilustração a seguir mostra os links de paginação.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instale o pacote PagedList.MVC NuGet

O NuGet **PagedList.Mvc** pacote instala automaticamente o **PagedList** pacote como uma dependência. O **PagedList** pacote instala um `PagedList` métodos de tipo e a extensão de coleção para `IQueryable` e `IEnumerable` coleções. Os métodos de extensão criam uma única página de dados em um `PagedList` coleção fora de sua `IQueryable` ou `IEnumerable`e o `PagedList` coleção fornece várias propriedades e métodos que facilitam a paginação. O **PagedList.Mvc** pacote instala um auxiliar de paginação que exibe os botões de paginação.

Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote** e **gerenciar pacotes NuGet para solução**.

No **gerenciar pacotes NuGet** caixa de diálogo, clique o **Online** guia à esquerda e, em seguida, digite "paginável" na caixa de pesquisa. Quando você vir o **PagedList.Mvc** do pacote, clique em **instalar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

No **selecione projetos** , clique em **Okey**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Adicionar a funcionalidade de paginação para o método de índice

Em *Controllers\StudentController.cs*, adicione um `using` instrução para o `PagedList` namespace:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Substitua o método `Index` pelo seguinte código:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Esse código adiciona um `page` parâmetro, um parâmetro de ordem de classificação atual e um parâmetro de filtro atual para a assinatura do método, conforme mostrado aqui:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Na primeira vez em que a página é exibida, ou se o usuário ainda não clicou uma paginação ou link de classificação, todos os parâmetros será nulos. Se um link de paginação é clicado, o `page` variável conterá o número da página para exibir.

`A ViewBag`propriedade fornece a exibição com a ordem de classificação atual, porque isso deve ser incluído nos links de paginação para manter a ordem de classificação igual de paginação:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Outra propriedade, `ViewBag.CurrentFilter`, fornece a exibição com a cadeia de caracteres do filtro atual. Esse valor deve ser incluído nos links de paginação para manter as configurações de filtro durante a paginação e deve ser restaurado para a caixa de texto quando a página é exibida novamente. Se a cadeia de caracteres de pesquisa for alterada durante a paginação, a página deve ser redefinido como 1, porque o novo filtro pode resultar em dados diferentes a serem exibidos. A cadeia de caracteres de pesquisa é alterada quando um valor é inserido na caixa de texto e o botão de envio é pressionado. Nesse caso, o `searchString` parâmetro não é nulo.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

No final do método, o `ToPagedList` método de extensão no alunos `IQueryable` objeto converte a consulta do aluno em uma única página de alunos em um tipo de coleção que oferece suporte à paginação. Página única de alunos é então passada para o modo de exibição:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

O `ToPagedList` método usa um número de página. Os dois pontos de interrogação representam o [operador união null](https://msdn.microsoft.com/library/ms173224.aspx). O operador de união null define um valor padrão para um tipo anulável. a expressão `(page ?? 1)` significa retorna o valor de `page` se ele tem um valor ou retornará 1 se `page` é nulo.

### <a name="add-paging-links-to-the-student-index-view"></a>Adicionar Links de paginação para o modo de exibição de índice do aluno

Em *Views\Student\Index.cshtml*, substitua o código existente pelo seguinte código:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

O `@model` instrução na parte superior da página especifica o modo de exibição agora obtém um `PagedList` do objeto, em vez de um `List` objeto.

O `using` instrução `PagedList.Mvc` fornece acesso para o auxiliar do MVC para os botões de paginação.

O código usa uma sobrecarga [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que permite especificar [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

O padrão [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) envia dados de formulário com uma POSTAGEM, o que significa que parâmetros são passados no corpo da mensagem HTTP e não na URL como cadeias de caracteres de consulta. Quando você especificar HTTP GET, os dados do formulário são passados na URL como cadeias de caracteres de consulta, que permite aos usuários indicar a URL. O [diretrizes do W3C para o uso de HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) especifique que você deve usar um GET quando a ação não resulta em uma atualização.

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Clique nos links de paginação em ordens de classificação diferente para tornar-se de que funciona de paginação. Em seguida, digite uma cadeia de caracteres de pesquisa e tente paginação novamente para confirmar se paginação também funciona corretamente com a classificação e filtragem.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Criar um sobre a página que mostra as estatísticas do aluno

Universidade de Contoso do site sobre a página, você exibirá quantas alunos registrados para cada data de registro. Isso exige cálculos simples e agrupamento nos grupos. Para fazer isso, você fará o seguinte:

- Crie uma classe de modelo de exibição para os dados que você precisa passar para o modo de exibição.
- Modificar o `About` método o `Home` controlador.
- Modificar o `About` exibição.

### <a name="create-the-view-model"></a>Criar o modelo de exibição

Criar um *ViewModels* pasta. Nessa pasta, adicionar um arquivo de classe *EnrollmentDateGroup.cs* e substitua o código existente pelo seguinte código:

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcional: Implantar o aplicativo no Windows Azure

Até agora seu aplicativo está sendo executado localmente no IIS Express no computador de desenvolvimento. Para torná-lo disponível para outros usuários na Internet, você precisa implantá-lo em um provedor de hospedagem na web. Nesta seção opcional do tutorial você poderá implantá-lo para um Site do Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Usando migrações do Code First para implantar o banco de dados

Para implantar o banco de dados, você usará migrações do Code First. Quando você cria o perfil de publicação que você usa para definir as configurações de implantação do Visual Studio, você selecionará uma caixa de seleção rotulada **executar migrações do Code First (executado na inicialização do aplicativo)**. Essa configuração faz com que o processo de implantação configurar automaticamente o aplicativo *Web. config* arquivos no servidor de destino, de forma que usa o Code First a `MigrateDatabaseToLatestVersion` classe inicializador.

O Visual Studio não faz nada com o banco de dados durante o processo de implantação. Quando um aplicativo implantado acessa o banco de dados pela primeira vez após a implantação, Code First automaticamente cria o banco de dados ou atualiza o esquema de banco de dados para a versão mais recente. Se o aplicativo implementa uma migrações `Seed` método, o método é executado depois que o banco de dados é criado ou o esquema é atualizado.

Suas migrações `Seed` método insere dados de teste. Se você foram implantando em um ambiente de produção, você precisa alterar o `Seed` método para que ele somente insere dados que você deseja ser inseridos em seu banco de dados de produção. Por exemplo, em seu modelo de dados atual convém ter cursos reais, mas os alunos fictícios no banco de dados de desenvolvimento. Você pode escrever um `Seed` método para carregar no desenvolvimento e comentar os alunos fictícios antes de implantar na produção. Ou você pode escrever um `Seed` método carregar somente os cursos e insira os alunos fictícios no banco de dados de teste manualmente usando a interface do usuário do aplicativo.

### <a name="get-a-windows-azure-account"></a>Obter uma conta do Windows Azure

Você precisará de uma conta do Windows Azure. Se você ainda não tiver um, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Criar um site da web e um banco de dados SQL no Windows Azure

O Site do Windows Azure será executado em um ambiente de hospedagem compartilhado, o que significa que ele é executado em máquinas virtuais (VMs) que são compartilhadas com outros clientes do Windows Azure. Um ambiente de hospedagem compartilhado é uma maneira de baixo custo para começar na nuvem. Posteriormente, se aumenta o tráfego da web, o aplicativo pode ser dimensionado para satisfazer a necessidade, executando em VMs dedicadas. Se você precisar de uma arquitetura mais complexa, você pode migrar para um serviço de nuvem do Windows Azure. Serviços de nuvem executados em VMs dedicados que podem ser configuradas de acordo com suas necessidades.

Banco de dados SQL do Windows Azure é um serviço de banco de dados relacional baseado em nuvem que se baseia em tecnologias do SQL Server. Ferramentas e aplicativos que funcionam com o SQL Server também funcionam com o banco de dados SQL.

1. No [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/), clique em **Sites da Web** na guia à esquerda e clique **novo**.

    ![Novo botão no Portal de gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Clique em **criação personalizada**.

    ![Criar com link do banco de dados no Portal de gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

 O **novo Site da Web - criação personalizada** assistente é aberto.
3. No **novo Site** etapa do assistente, insira uma cadeia de caracteres de **URL** caixa para usar como a URL exclusiva para seu aplicativo. A URL completa consistirá em que você digitar aqui e o sufixo que você vê ao lado da caixa de texto. A ilustração mostra "ConU", mas essa URL provavelmente é feita para que você precisará escolher um diferente.

    ![Criar com link do banco de dados no Portal de gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. No **região** lista suspensa, escolha uma região de perto de você. Essa configuração especifica que seu site da web será executado do data center.
5. No **banco de dados** lista suspensa, escolha **criar um banco de dados SQL de 20 MB gratuito**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. No **nome de cadeia de caracteres de conexão do banco de dados**, digite *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Clique na seta que aponta para a direita na parte inferior da caixa. O assistente avança para o **configurações de banco de dados** etapa.
8. No **nome** , digite *ContosoUniversityDB*.
9. No **servidor** selecione **servidor novo banco de dados do SQL**. Como alternativa, se você criou um servidor, você pode selecionar o servidor na lista suspensa.
10. Insira um administrador **nome de logon** e **senha**. Se você selecionou **servidor novo banco de dados do SQL** você não digitar um nome existente e a senha aqui, você está inserindo um novo nome e uma senha que você está definindo agora para usar mais tarde, quando você acessa o banco de dados. Se você tiver selecionado um servidor que você criou anteriormente, você vai inserir credenciais para o servidor. Para este tutorial, você não selecionar a ***avançado*** caixa de seleção. O ***avançado*** opções permitem que você defina o banco de dados [agrupamento](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Escolha o mesmo **região** que você escolheu para o site da web.
12. Clique na marca de seleção no canto inferior direito da caixa para indicar que você tiver terminado.   
  
    ![Etapa de configurações de banco de dados do novo Site da Web - criar com o Assistente de banco de dados](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

 A imagem a seguir mostra o uso de um logon e o SQL Server existente.   
  
    ![Etapa de configurações de banco de dados do novo Site da Web - criar com o Assistente de banco de dados](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
 O Portal de gerenciamento retorna para a página de Sites da Web e o **Status** coluna mostra que o site está sendo criado. Após alguns instantes (geralmente menor que um minuto), o **Status** coluna mostra que o site foi criado com êxito. Na barra de navegação à esquerda, o número de sites que você tem em sua conta aparece ao lado de **Sites da Web** ícone e o número de bancos de dados aparece ao lado a **bancos de dados SQL** ícone.

## <a name="deploy-the-application-to-windows-azure"></a>Implantar o aplicativo no Windows Azure

1. No Visual Studio, clique com botão direito no projeto no **Solution Explorer** e selecione **publicar** no menu de contexto.  
  
    ![Publicar no menu de contexto do projeto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. No **perfil** guia do **Publicar Web** assistente, clique em **importação**.  
  
    ![Importar configurações de publicação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Se você não adicionou anteriormente sua assinatura do Windows Azure no Visual Studio, execute as etapas a seguir. Nestas etapas adicionar sua assinatura para que a lista suspensa em **importar de um site do Windows Azure** incluirá o seu site da web.

    a. No **perfil de publicação de importação** caixa de diálogo, clique em **importar de um site do Windows Azure**e, em seguida, clique em **assinatura do Windows Azure adicionar**.

    ![Adicionar assinatura do Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. No **importação assinatura do Windows Azure** caixa de diálogo, clique em **arquivo de assinatura de Download**.

    ![Baixe o arquivo de assinatura](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Na janela do navegador, salve o *. publishsettings* arquivo.

    ![Baixe o arquivo. publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Segurança - o *publishsettings* arquivo contém suas credenciais (sem codificação) que são usados para administrar suas assinaturas do Windows Azure e serviços. A prática recomendada de segurança para esse arquivo é armazená-lo temporariamente fora dos diretórios de origem (por exemplo, no *bibliotecas\documentos* pasta) e, em seguida, excluí-lo depois que a importação for concluída. Um usuário mal-intencionado que consiga acessar o `.publishsettings` arquivo pode editar, criar e excluir seus serviços do Windows Azure.

    d. No **importação assinatura do Windows Azure** caixa de diálogo, clique em **procurar** e navegue até o *. publishsettings* arquivo.

    ![baixar sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Clique em **Importar**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. No **perfil de publicação de importação** caixa de diálogo, selecione **importar de um site do Windows Azure**, selecione o site da web da lista suspensa e, em seguida, clique em **Okey**.  
  
    ![Importar o perfil de publicação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. No **Conexão** , clique em **Conexão validar** para certificar-se de que as configurações estão corretas.  
  
    ![Validar a conexão](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Quando a conexão foi validada, uma marca de seleção verde é exibida ao lado de **Conexão validar** botão. Clique em **Avançar**.  
  
    ![Conexão validada com êxito](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Abra o **cadeia de caracteres de conexão remota** lista suspensa em **SchoolContext** e selecione a cadeia de caracteres de conexão para o banco de dados que você criou.
8. Selecione **executar migrações do Code First (executado na inicialização do aplicativo)**.
9. Desmarque **usar essa cadeia de caracteres de conexão em tempo de execução** para o **UserContext (DefaultConnection)**, desde que este aplicativo não está usando o banco de dados de associação.   
  
    ![Guia Configurações](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Clique em **Avançar**.
11. No **visualização** , clique em **visualização iniciar**.  
  
    ![Botão de StartPreview na guia de visualização](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
 A guia exibe uma lista dos arquivos que serão copiados para o servidor. Exibir a visualização não é necessário para publicar o aplicativo, mas é uma função útil estar atento. Nesse caso, você não precisa fazer nada com a lista de arquivos que é exibida. Na próxima vez que você implantar esse aplicativo, somente os arquivos que foram alterados será nesta lista.  
  
    ![Saída de arquivo StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Clique em **Publicar**.  
 O Visual Studio inicia o processo de copiar os arquivos para o servidor Windows Azure.
13. O **saída** janela mostra quais ações de implantação foram realizadas e relata a conclusão com êxito da implantação.  
  
    ![Relatório de implantação bem-sucedida de janela de saída](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Após a implantação bem-sucedida, o navegador padrão é aberto automaticamente para a URL do site da web implantados.  
 O aplicativo que você criou agora está em execução na nuvem. Clique na guia de alunos.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

Neste ponto o *SchoolContext* banco de dados foi criado no Windows Azure SQL Database porque você selecionou **executar migrações do Code First (executado na inicialização do aplicativo)**. O *Web. config* arquivo no site da web implantados foi alterado para que o [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador seria executado na primeira vez em que o código lê ou grava dados no banco de dados (que aconteceu quando você selecionou o **alunos** guia):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

O processo de implantação também criou uma nova cadeia de caracteres de conexão *(SchoolContext\_DatabasePublish*) para migrações do Code First a ser usado para atualizar o esquema de banco de dados e a propagação do banco de dados.

![Cadeia de caracteres de conexão Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

O *DefaultConnection* é de cadeia de caracteres de conexão para o banco de dados de associação (que não estamos usando neste tutorial). O *SchoolContext* é de cadeia de caracteres de conexão para o banco de dados ContosoUniversity.

Você pode encontrar a versão implantada do arquivo Web. config em seu próprio computador *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Você pode acessar o implantado *Web. config* próprio arquivo, usando o FTP. Para obter instruções, consulte [implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Siga as instruções que começam com "para usar uma ferramenta FTP, é necessário que três coisas: a URL de FTP, o nome de usuário e a senha."

> [!NOTE]
> O aplicativo web não implementa a segurança, para que qualquer pessoa que localiza a URL pode alterar os dados. Para obter instruções sobre como proteger o site da web, consulte [implantar um aplicativo ASP.NET MVC seguro com associação, OAuth e o banco de dados SQL para um Site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Você pode impedir que outras pessoas usando o site usando o Portal de gerenciamento do Windows Azure ou **Server Explorer** no Visual Studio para parar o site.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inicializadores de primeiro do código

Na seção de implantação, você viu o [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador que está sendo usado. Código primeiro também fornece outros inicializadores que você pode usar, incluindo [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (o padrão), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) e [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). O `DropCreateAlways` inicializador pode ser útil para configurar condições para testes de unidade. Você também pode escrever seus próprios inicializadores e você pode chamar um inicializador explicitamente se não desejar aguardar até que o aplicativo lê de ou grava no banco de dados. Para obter uma explicação completa de inicializadores, consulte o capítulo 6 do catálogo de [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman e Rowan Miller.

## <a name="summary"></a>Resumo

Neste tutorial, você viu como criar um modelo de dados e implementar CRUD básica, classificação, filtragem, paginação e funcionalidade de agrupamento. O seguinte tutorial, você começará a examinar os tópicos mais avançados, expandindo o modelo de dados.

Links para outros recursos do Entity Framework podem ser encontradas no [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Próximo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
