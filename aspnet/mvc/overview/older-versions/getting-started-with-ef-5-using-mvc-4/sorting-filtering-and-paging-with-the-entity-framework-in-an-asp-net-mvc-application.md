---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Classificação, filtragem e paginação com o Entity Framework em um aplicativo ASP.NET MVC (3 de 10) | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8bea3d4bc19a5a47240abeb2cc015116814a8fdf
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911813"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Classificação, filtragem e paginação com o Entity Framework em um aplicativo ASP.NET MVC (3 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais de início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece por aqui.
> 
> > [!NOTE] 
> > 
> > Se você enfrentar um problema que você não conseguir resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução ao problema comparando seu código com o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


No tutorial anterior, você implementou um conjunto de páginas da web para operações CRUD básicas para `Student` entidades. Neste tutorial você adicionará classificação, filtragem e a funcionalidade de paginação para o **alunos** página de índice. Você também criará uma página que faz um agrupamento simples.

A ilustração a seguir mostra a aparência da página quando você terminar. Os títulos de coluna são links que o usuário pode clicar para classificar por essa coluna. Clicar em um título de coluna alterna repetidamente entre a ordem de classificação ascendente e descendente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Adicionar links de classificação de coluna à página Índice de Alunos

Para adicionar uma classificação à página índice de alunos, você alterará a `Index` método da `Student` controlador e adicione código para o `Student` indexa a exibição.

### <a name="add-sorting-functionality-to-the-index-method"></a>Adicionar funcionalidade ao método Index de classificação

Na *Controllers\StudentController.cs*, substitua o `Index` método com o código a seguir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Esse código recebe um parâmetro `sortOrder` da cadeia de caracteres de consulta na URL. O valor de cadeia de caracteres de consulta é fornecido pelo ASP.NET MVC como um parâmetro para o método de ação. O parâmetro será uma cadeia de caracteres "Name" ou "Date", opcionalmente, seguido de um sublinhado e a cadeia de caracteres "desc" para especificar a ordem descendente. A ordem de classificação crescente é padrão.

Na primeira vez que a página Índice é solicitada, não há nenhuma cadeia de caracteres de consulta. Os alunos são exibidos em ordem crescente pela `LastName`, que é o padrão, conforme estabelecido pelo caso fall-through no `switch` instrução. Quando o usuário clica em um hiperlink de título de coluna, o valor `sortOrder` apropriado é fornecido na cadeia de caracteres de consulta.

Os dois `ViewBag` as variáveis são usadas para que o modo de exibição pode configurar os hiperlinks de título de coluna com os valores de cadeia de caracteres de consulta apropriados:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Essas são instruções ternárias. A primeira delas Especifica que, se o `sortOrder` parâmetro é nulo ou vazio, `ViewBag.NameSortParm` deve ser definido como "nome\_desc"; caso contrário, ele deve ser definido como uma cadeia de caracteres vazia. Essas duas instruções permitem que a exibição defina os hiperlinks de título de coluna da seguinte maneira:

| Ordem de classificação atual | Hiperlink do sobrenome | Hiperlink de data |
| --- | --- | --- |
| Sobrenome ascendente | descending | ascending |
| Sobrenome descendente | ascending | ascending |
| Data ascendente | ascending | descending |
| Data descendente | ascending | ascending |

Usa o método [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) para especificar a coluna para classificar por. O código cria um [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variável antes do `switch` instrução, modifica-o no `switch` instrução e chama o `ToList` método após o `switch` instrução. Quando você cria e modifica variáveis `IQueryable`, nenhuma consulta é enviada para o banco de dados. A consulta não é executada até que você converta a `IQueryable` objeto em uma coleção chamando um método como `ToList`. Portanto, esse código resulta em uma única consulta que não é executada até que o `return View` instrução.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Adicionar hiperlinks a exibição índice de alunos de título de coluna

Na *Views\Student\Index.cshtml*, substitua o `<tr>` e `<th>` elementos para a linha de cabeçalho com o código realçado:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Esse código usa as informações a `ViewBag` valores de cadeia de caracteres de propriedades para configurar hiperlinks com a consulta apropriada.

Execute a página e clique no **Sobrenome** e **data de registro** títulos de coluna para verificar se a classificação funciona.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Depois de clicar na **Sobrenome** título, os alunos são exibidos na ordem do último nome decrescente.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Adicione uma caixa de pesquisa para a página de índice de alunos

Para adicionar a filtragem à página Índice de Alunos, você adicionará uma caixa de texto e um botão Enviar à exibição e fará alterações correspondentes no método `Index`. A caixa de texto permitirá que você insira uma cadeia de caracteres a ser pesquisada nos campos de nome e sobrenome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Adicionar funcionalidade de filtragem a método Index

Na *Controllers\StudentController.cs*, substitua o `Index` método com o código a seguir (as alterações são realçadas):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Você adicionou um parâmetro `searchString` ao método `Index`. Você também adicionou à instrução LINQ uma `where` clausethat seleciona somente os alunos cujo nome ou sobrenome contém a cadeia de caracteres de pesquisa. O valor de cadeia de caracteres de pesquisa é recebido de uma caixa de texto que você adicionará à exibição índice. A instrução que adiciona o [onde](https://msdn.microsoft.com/library/bb535040.aspx) cláusula é executada somente se houver um valor a ser pesquisado.

> [!NOTE]
> Em muitos casos, você pode chamar o mesmo método em um conjunto de entidades do Entity Framework ou como um método de extensão em uma coleção em memória. Os resultados são normalmente os mesmos, mas em alguns casos podem ser diferentes. Por exemplo, a implementação do .NET Framework a `Contains` método retorna todas as linhas quando você passar uma cadeia de caracteres vazia para ele, mas o provedor do Entity Framework para SQL Server Compact 4.0 retorna zero linhas de cadeias de caracteres vazias. Portanto, o código de exemplo (colocando a `Where` instrução dentro de um `if` instrução) torna-se de que você obter os mesmos resultados para todas as versões do SQL Server. Além disso, a implementação do .NET Framework a `Contains` método executa uma comparação que diferencia maiusculas de minúsculas por padrão, mas os provedores SQL Server do Entity Framework executam comparações de maiusculas e minúsculas por padrão. Portanto, chamar o `ToUpper` método para fazer o teste de maiusculas e minúsculas explicitamente garante que eles não são alterados quando você altera o código mais tarde para usar um repositório, que retornará um `IEnumerable` coleção em vez de um `IQueryable` objeto. (Quando você chama o método `Contains` em uma coleção `IEnumerable`, obtém a implementação do .NET Framework; quando chama-o em um objeto `IQueryable`, obtém a implementação do provedor de banco de dados.)


### <a name="add-a-search-box-to-the-student-index-view"></a>Adicionar uma Caixa de Pesquisa à exibição Índice de Alunos

Na *Views\Student\Index.cshtml*, adicione o código realçado imediatamente antes da abertura `table` marca para criar uma legenda, uma caixa de texto e um **pesquisa** botão.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Execute a página, insira uma cadeia de caracteres de pesquisa e clique em **pesquisa** para verificar se a filtragem está funcionando.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Observe que a URL não contiver a "uma" cadeia de pesquisa, que significa que se você marcar essa página, você não obterá a lista filtrada quando você usa o indicador. Você alterará a **pesquisa** botão usar cadeias de caracteres de consulta para os critérios de filtro mais tarde no tutorial.

## <a name="add-paging-to-the-students-index-page"></a>Adicionar paginação à página de índice de alunos

Para adicionar paginação à página índice de alunos, comece instalando o **PagedList.Mvc** pacote do NuGet. Em seguida, você fará alterações adicionais na `Index` método e adicionar links de paginação para o `Index` modo de exibição. **PagedList.Mvc** é um dos muitos paginação boa e classificação pacotes para o ASP.NET MVC e seu uso aqui destina-se apenas como um exemplo, não como uma recomendação para que ele sobre outras opções. A ilustração a seguir mostra os links de paginação.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instale o pacote PagedList.MVC NuGet

O NuGet **PagedList.Mvc** pacote instala automaticamente o **PagedList** pacote como uma dependência. O **PagedList** pacote instala um `PagedList` métodos de tipo e a extensão de coleção para `IQueryable` e `IEnumerable` coleções. Os métodos de extensão criam uma única página de dados em um `PagedList` coleção fora de seu `IQueryable` ou `IEnumerable`e o `PagedList` coleção fornece várias propriedades e métodos que facilitam a paginação. O **PagedList.Mvc** pacote instala um auxiliar de paginação que exibe os botões de paginação.

Dos **ferramentas** menu, selecione **Gerenciador de pacotes NuGet** e, em seguida, **gerenciar pacotes NuGet para solução**.

No **gerenciar pacotes NuGet** caixa de diálogo, clique o **Online** guia à esquerda e, em seguida, insira "paginável" na caixa de pesquisa. Quando você vir a **PagedList.Mvc** do pacote, clique em **instalar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

No **selecionar projetos** , clique em **Okey**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Adicionar a funcionalidade de paginação ao método Index

Na *Controllers\StudentController.cs*, adicione uma `using` instrução para o `PagedList` namespace:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Substitua o método `Index` pelo seguinte código:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Este código adiciona um `page` parâmetro, um parâmetro de ordem de classificação atual e um parâmetro de filtro atual à assinatura do método, conforme mostrado aqui:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Na primeira vez que a página for exibida, ou se o usuário ainda não tiver clicado em um link de paginação ou classificação, todos os parâmetros serão nulos. Se um link de paginação é clicado, o `page` variável conterá o número da página para exibir.

`A ViewBag` propriedade fornece a exibição com a ordem de classificação atual, pois isso precisa ser incluído nos links de paginação para manter a ordem de classificação igual durante a paginação:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Outra propriedade, `ViewBag.CurrentFilter`, fornece a exibição com a cadeia de caracteres de filtro atual. Esse valor precisa ser incluído nos links de paginação para manter as configurações de filtro durante a paginação e precisa ser restaurado para a caixa de texto quando a página é exibida novamente. Se a cadeia de caracteres de pesquisa for alterada durante a paginação, a página precisará ser redefinida como 1, porque o novo filtro pode resultar na exibição de dados diferentes. A cadeia de caracteres de pesquisa é alterada quando um valor é inserido na caixa de texto e o botão Enviar é pressionado. Nesse caso, o `searchString` parâmetro não for nulo.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

No final do método, o `ToPagedList` método de extensão em que os alunos `IQueryable` objeto converte a consulta de alunos em uma única página de alunos em um tipo de coleção que dá suporte à paginação. A única página de alunos é então passada para o modo de exibição:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

O método `ToPagedList` usa um número de página. Os dois pontos de interrogação representam o [operador de coalescência nula](https://msdn.microsoft.com/library/ms173224.aspx). O operador de união de nulo define um valor padrão para um tipo que permite valor nulo; a expressão `(page ?? 1)` significa retornar o valor de `page` se ele tiver um valor ou retornar 1 se `page` for nulo.

### <a name="add-paging-links-to-the-student-index-view"></a>Adicionar Links de paginação à exibição índice de alunos

Na *Views\Student\Index.cshtml*, substitua o código existente pelo código a seguir:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

A instrução `@model` na parte superior da página especifica que a exibição agora obtém um objeto `PagedList`, em vez de um objeto `List`.

O `using` instrução para `PagedList.Mvc` fornece acesso para o auxiliar do MVC para os botões de paginação.

O código usa uma sobrecarga [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que permite que ele especifique [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

O padrão [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) envia dados de formulário com um POST, o que significa que os parâmetros são passados no corpo da mensagem HTTP e não na URL como cadeias de caracteres de consulta. Quando você especifica HTTP GET, os dados de formulário são passados na URL como cadeias de consulta, o que permite aos usuários marcar a URL. O [diretrizes do W3C para o uso de HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) especifique que você deve usar GET quando a ação não resulta em uma atualização.

A caixa de texto é inicializada com a cadeia de caracteres de pesquisa atual quando você clica em uma nova página, você pode ver a cadeia de caracteres de pesquisa atual.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Os links de cabeçalho de coluna usam a cadeia de caracteres de consulta para passar a cadeia de caracteres de pesquisa atual para o controlador, de modo que o usuário possa classificar nos resultados do filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

O número atual de página e total de páginas é exibido.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Se não houver nenhuma página para exibir, "Página 0 de 0" é mostrada. (Nesse caso, o número da página é maior que a contagem de páginas porque `Model.PageNumber` for 1, e `Model.PageCount` é 0.)

Os botões de paginação são exibidos pelo `PagedListPager` auxiliar:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

O `PagedListPager` auxiliar fornece várias opções que você pode personalizar, incluindo URLs e estilo. Para obter mais informações, consulte [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) no site do GitHub.

Execute a página.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Clique nos links de paginação em ordens de classificação diferentes para verificar se a paginação funciona. Em seguida, insira uma cadeia de caracteres de pesquisa e tente fazer a paginação novamente para verificar se ela também funciona corretamente com a classificação e filtragem.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Criar uma página que mostra as estatísticas de alunos sobre

O Contoso University do site sobre a página, você exibirá quantos alunos se registraram para cada data de registro. Isso exige agrupamento e cálculos simples nos grupos. Para fazer isso, você fará o seguinte:

- Criar uma classe de modelo de exibição para os dados que você precisa passar para a exibição.
- Modificar a `About` método no `Home` controlador.
- Modificar o `About` modo de exibição.

### <a name="create-the-view-model"></a>Criar o modelo de exibição

Criar uma *ViewModels* pasta. Nessa pasta, adicione um arquivo de classe *EnrollmentDateGroup.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificar o controlador Home

Na *HomeController.cs*, adicione o seguinte `using` instruções na parte superior do arquivo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Adicione uma variável de classe para o contexto de banco de dados imediatamente após a chave de abertura para a classe:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Substitua o método `About` pelo seguinte código:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

A instrução LINQ agrupa as entidades de alunos por data de registro, calcula o número de entidades em cada grupo e armazena os resultados em uma coleção de objetos de modelo de exibição `EnrollmentDateGroup`.

Adicionar um `Dispose` método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificar a exibição Sobre

Substitua o código na *Views\Home\About.cshtml* arquivo pelo código a seguir:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Execute o aplicativo e clique no **sobre** link. A contagem de alunos para cada data de registro é exibida em uma tabela.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcional: Implantar o aplicativo no Windows Azure

Até agora seu aplicativo ser executado localmente no IIS Express no computador de desenvolvimento. Para torná-lo disponível para que outras pessoas usem a Internet, você precisará implantá-lo em um provedor de hospedagem na web. Nesta seção opcional do tutorial você irá implantá-lo para um Site do Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Usando o Code First Migrations para implantar o banco de dados

Para implantar o banco de dados, você usará migrações do Code First. Quando você cria o perfil de publicação que você usa para definir as configurações para a implantação do Visual Studio, você selecionará uma caixa de seleção rotulada **executar migrações do Code First (executado na inicialização do aplicativo)**. Essa configuração faz com que o processo de implantação configurar automaticamente o aplicativo *Web. config* arquivo no servidor de destino, para que o Code First usa o `MigrateDatabaseToLatestVersion` a classe do inicializador.

Visual Studio não faz nada com o banco de dados durante o processo de implantação. Quando o aplicativo implantado acessa o banco de dados pela primeira vez após a implantação, Code First automaticamente cria o banco de dados ou atualiza o esquema de banco de dados para a versão mais recente. Se o aplicativo implementar um migrações `Seed` método, o método é executado depois que o banco de dados é criado ou o esquema é atualizado.

Suas migrações `Seed` método insere dados de teste. Se você estivesse Implantando um ambiente de produção, você teria que alterar o `Seed` , de modo que ele insere apenas os dados que você deseja a ser inserido no seu banco de dados de produção. Por exemplo, no seu modelo de dados atual você talvez queira ter cursos real, mas os alunos fictícios no banco de dados de desenvolvimento. Você pode escrever um `Seed` método para carregar no desenvolvimento e comente os alunos fictícios antes de implantar em produção. Ou você pode escrever um `Seed` método carregar somente os cursos e insira os alunos fictícios no banco de dados de teste manualmente usando a interface do usuário do aplicativo.

### <a name="get-a-windows-azure-account"></a>Obtenha uma conta do Windows Azure

Você precisará de uma conta do Windows Azure. Se você ainda não tiver uma, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Criar um site da web e um banco de dados SQL no Windows Azure

Seu Site do Windows Azure será executado em um ambiente de hospedagem compartilhado, o que significa que ele é executado em máquinas virtuais (VMs) que são compartilhadas com outros clientes do Windows Azure. Um ambiente de hospedagem compartilhado é uma maneira de baixo custo para começar a usar na nuvem. Posteriormente, se aumentar o tráfego da web, o aplicativo possa ser dimensionado para atender à necessidade executando em VMs dedicadas. Se você precisar de uma arquitetura mais complexa, você pode migrar para um serviço de nuvem do Windows Azure. Serviços de nuvem executados em VMs dedicadas que podem ser configuradas de acordo com suas necessidades.

Banco de dados SQL do Windows Azure é um serviço de banco de dados relacional baseado em nuvem que se baseia em tecnologias do SQL Server. Ferramentas e aplicativos que funcionam com o SQL Server também funcionam com o banco de dados SQL.

1. No [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/), clique em **Sites da Web** na guia à esquerda e depois clique em **New**.

    ![Botão novo no Portal de gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Clique em **criação personalizada**.

    ![Criar com o link do banco de dados no Portal de gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   O **novo Site da Web - criação personalizada** assistente é aberto.
3. No **novo Site da Web** etapa do assistente, insira uma cadeia de caracteres de **URL** caixa para usar como a URL exclusiva para seu aplicativo. A URL completa consistirá em que você inserir aqui mais o sufixo que você vê ao lado da caixa de texto. A ilustração mostra "ConU", mas essa URL provavelmente será executada, portanto, você terá que escolher um diferente.

    ![Criar com o link do banco de dados no Portal de gerenciamento](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. No **região** lista suspensa, escolha uma região perto de você. Essa configuração especifica qual Datacenter seu site da web será executado em.
5. No **banco de dados** lista suspensa, escolha **criar um banco de dados SQL de 20 MB gratuito**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. No **nome de cadeia de conexão de banco de dados**, insira *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Clique na seta que aponta para a direita na parte inferior da caixa. O assistente avança para o **configurações de banco de dados** etapa.
8. No **nome** , digite *ContosoUniversityDB*.
9. No **Server** caixa, selecione **servidor de banco de dados SQL**. Como alternativa, se você tiver criado um servidor, você pode selecionar esse servidor na lista suspensa.
10. Insira um administrador **nome de logon** e **senha**. Se você selecionou **servidor do novo banco de dados SQL** não digitará um nome e senha existentes aqui, você está inserindo um novo nome e senha que você está definindo agora para usar mais tarde, quando você acessa o banco de dados. Se você tiver selecionado um servidor que você criou anteriormente, você vai inserir as credenciais para esse servidor. Para este tutorial, você não selecionar o ***avançado*** caixa de seleção. O ***Advanced*** opções permitem que você defina o banco de dados [agrupamento](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Escolha o mesmo **região** que você escolheu para o site da web.
12. Clique na marca de seleção na parte inferior direita da caixa para indicar que tiver terminado.   
  
    ![Etapa de configurações de banco de dados do novo Site da Web - criar com o Assistente de banco de dados](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    A imagem a seguir mostra o uso de um SQL Server existente e faça logon.   
  
    ![Etapa de configurações de banco de dados do novo Site da Web - criar com o Assistente de banco de dados](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    O Portal de gerenciamento retorna para a página de Sites da Web e o **Status** coluna mostra que o site está sendo criado. Depois de algum tempo (normalmente menos de um minuto), o **Status** coluna mostra o site foi criado com êxito. Na barra de navegação à esquerda, o número de sites que você tem em sua conta é exibido ao lado de **Sites da Web** ícone e o número de bancos de dados é exibido ao lado de **bancos de dados SQL** ícone.

## <a name="deploy-the-application-to-windows-azure"></a>Implantar o aplicativo no Windows Azure

1. No Visual Studio, clique com botão direito no projeto no **Gerenciador de soluções** e selecione **publicar** no menu de contexto.  
  
    ![Publicar no menu de contexto do projeto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. No **perfil** guia da **publicar na Web** assistente, clique em **importação**.  
  
    ![Importar configurações de publicação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Se você não adicionou anteriormente sua assinatura do Windows Azure no Visual Studio, execute as seguintes etapas. Nestas etapas adicionar sua assinatura, de modo que, na lista suspensa **importação de um site do Windows Azure** incluirá o seu site da web.

    a. No **importar perfil de publicação** caixa de diálogo, clique em **importação de um site do Windows Azure**e, em seguida, clique em **assinatura do Windows Azure adicionar**.

    ![Adicionar assinatura do Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. No **importar assinaturas do Windows Azure** caixa de diálogo, clique em **baixar arquivo de assinatura**.

    ![Baixe o arquivo de assinatura](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Na janela do navegador, salve o *. publishsettings* arquivo.

    ![Baixe o arquivo. publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Security - a *publishsettings* arquivo contém suas credenciais (sem codificação) que são usados para administrar suas assinaturas do Windows Azure e serviços. A prática recomendada de segurança para esse arquivo é armazená-lo temporariamente fora dos diretórios de origem (por exemplo, na *Libraries\Documents* pasta) e, em seguida, excluí-lo depois que a importação for concluída. Um usuário mal-intencionado que consiga acessar o `.publishsettings` arquivo pode editar, criar e excluir seus serviços do Windows Azure.

    d. No **importar assinaturas do Windows Azure** caixa de diálogo, clique em **procurar** e navegue até o *. publishsettings* arquivo.

    ![baixar sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Clique em **Importar**.

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. No **importar perfil de publicação** caixa de diálogo, selecione **importação de um site do Windows Azure**, selecione seu site da web na lista suspensa e, em seguida, clique em **Okey**.  
  
    ![Importar o perfil de publicação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. No **Conexão** , clique em **validar Conexão** para certificar-se de que as configurações estão corretas.  
  
    ![Validar a conexão](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Quando a conexão tiver sido validado, uma marca de seleção verde é mostrada ao lado de **validar Conexão** botão. Clique em **Avançar**.  
  
    ![Conexão validada com êxito](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Abra o **cadeia de caracteres de conexão remota** lista suspensa sob **SchoolContext** e selecione a cadeia de caracteres de conexão para o banco de dados que você criou.
8. Selecione **executar migrações do Code First (executado na inicialização do aplicativo)**.
9. Desmarque a opção **Use essa cadeia de caracteres de conexão em tempo de execução** para o **UserContext (DefaultConnection)**, já que este aplicativo não está usando o banco de dados de associação.   
  
    ![Guia Configurações](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Clique em **Avançar**.
11. No **versão prévia** , clique em **iniciar visualização**.  
  
    ![Botão StartPreview na guia de visualização](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    A guia exibe uma lista dos arquivos que serão copiados para o servidor. Exibir a visualização não é necessário para publicar o aplicativo, mas é uma função útil estar atento. Nesse caso, você não precisa fazer nada com a lista de arquivos é exibida. Na próxima vez que você implantar esse aplicativo, somente os arquivos que foram alterados será nessa lista.  
  
    ![Saída do arquivo StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Clique em **Publicar**.  
    Visual Studio inicia o processo de copiar os arquivos para o servidor do Windows Azure.
13. O **saída** janela mostra quais ações de implantação foram executadas e relata a conclusão bem-sucedida da implantação.  
  
    ![Janela de saída do relatório de uma implantação bem-sucedida](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Após a implantação bem-sucedida, o navegador padrão abre automaticamente a URL do site da web implantado.  
    O aplicativo que você criou agora está em execução na nuvem. Clique na guia alunos.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

Neste ponto seu *SchoolContext* banco de dados tenha sido criado no banco de dados do SQL Azure de Windows, porque você selecionou **executar migrações do Code First (executado na inicialização do aplicativo)**. O *Web. config* arquivo no site da web implantado foi alterado para que o [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador seria executado na primeira vez em que seu código lê ou grava dados no banco de dados (que aconteceu quando você selecionou o **alunos** guia):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Além disso, o processo de implantação criada uma nova cadeia de caracteres de conexão *(SchoolContext\_DatabasePublish*) para migrações do Code First a ser usado para atualizar o esquema de banco de dados e a propagação do banco de dados.

![Cadeia de caracteres de conexão Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

O *DefaultConnection* é de cadeia de caracteres de conexão para o banco de dados de associação (que não estamos usando neste tutorial). O *SchoolContext* é de cadeia de caracteres de conexão para o banco de dados ContosoUniversity.

Você pode encontrar a versão implantada do arquivo Web. config em seu próprio computador *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Você pode acessar o implantado *Web. config* próprio arquivo por meio de FTP. Para obter instruções, consulte [implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Siga as instruções que começam com "para usar uma ferramenta FTP, você precisa de três itens: a URL de FTP, o nome de usuário e a senha."

> [!NOTE]
> O aplicativo web não implementa a segurança, portanto, qualquer pessoa que encontre a URL pode alterar os dados. Para obter instruções sobre como proteger o site da web, consulte [implantar um aplicativo ASP.NET MVC seguro com associação, OAuth e banco de dados SQL para um Site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Você pode impedir que outras pessoas usando o site usando o Portal de gerenciamento do Windows Azure ou **Gerenciador de servidores** no Visual Studio para parar o site.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inicializadores de primeiro código

Na seção de implantação que você viu a [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador que está sendo usado. Código pela primeira vez também fornece outros inicializadores de que você pode usar, incluindo [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (o padrão), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) e [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). O `DropCreateAlways` inicializador pode ser útil para configurar condições para testes de unidade. Você também pode escrever seus próprios inicializadores, e você pode chamar um inicializador explicitamente, se você não quiser esperar até que o aplicativo lê ou grava no banco de dados. Para obter uma explicação abrangente de inicializadores, consulte o capítulo 6 do livro [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) por Julie Lerman e Rowan Miller.

## <a name="summary"></a>Resumo

Neste tutorial, você viu como criar um modelo de dados e implementar CRUD básicas, classificação, filtragem, paginação e funcionalidade de agrupamento. O próximo tutorial, você começará examinando tópicos mais avançados, expandindo o modelo de dados.

Links para outros recursos do Entity Framework podem ser encontradas na [mapa de conteúdo de acesso do ASP.NET dados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Próximo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
