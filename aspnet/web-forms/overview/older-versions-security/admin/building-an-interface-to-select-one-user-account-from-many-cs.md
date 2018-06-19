---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: Criando uma Interface para selecionar uma conta de usuário de muitos (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial, vamos criar uma interface de usuário com uma grade paginável, podem ser filtrada. Em particular, nossa interface do usuário será composto de uma série de botões de link para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 304505b18e330425ea1dc8df87a552f3d8cd15f3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890665"
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Criando uma Interface para selecionar uma conta de usuário de muitos (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> Este tutorial, vamos criar uma interface de usuário com uma grade paginável, podem ser filtrada. Em particular, nossa interface do usuário será composto de uma série de botões de link para filtrar os resultados com base na letra inicial do nome de usuário e um controle GridView para mostrar os usuários correspondentes. Vamos começar listando todas as contas de usuário em um controle GridView. Em seguida, na etapa 3, vamos adicionar o filtro de botões de link. Etapa 4 examina os resultados filtrados de paginação. A interface criada nas etapas 2 a 4 será usada nos tutoriais subsequentes para executar tarefas administrativas para uma determinada conta de usuário.


## <a name="introduction"></a>Introdução

No <a id="_msoanchor_1"> </a> [ *atribuir funções aos usuários* ](../roles/assigning-roles-to-users-cs.md) tutorial, criamos uma interface rudimentar de um administrador para selecionar um usuário e gerenciar suas funções. Especificamente, a interface de conhecer o administrador uma lista suspensa de todos os usuários. Tal interface é adequado quando existem, mas o usuário uma dúzia ou mais contas, mas é difícil para os sites com centenas ou milhares de contas. Uma grade paginável, podem ser filtrada é mais adequada interface do usuário para sites com o usuário grandes bases de dados.

Este tutorial, vamos criar cada uma interface do usuário. Em particular, nossa interface do usuário será composto de uma série de botões de link para filtrar os resultados com base na letra inicial do nome de usuário e um controle GridView para mostrar os usuários correspondentes. Vamos começar listando todas as contas de usuário em um controle GridView. Em seguida, na etapa 3, vamos adicionar o filtro de botões de link. Etapa 4 examina os resultados filtrados de paginação. A interface criada nas etapas 2 a 4 será usada nos tutoriais subsequentes para executar tarefas administrativas para uma determinada conta de usuário.

Vamos começar!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: Adicionar novas páginas ASP.NET

Este tutorial e as próximas duas nós serão examinando diversos recursos e funções relacionadas à administração. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados durante esses tutoriais. Vamos criar essas páginas e atualizar o mapa do site.

Comece criando uma nova pasta no projeto chamado `Administration`. Em seguida, adicione duas novas páginas ASP.NET para a pasta, vinculando cada página com o `Site.master` página mestra. Nomeie as páginas:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Também adicionar duas páginas para o diretório de raiz do site: `ChangePassword.aspx` e `RecoverPassword.aspx`.

Essas quatro páginas, neste momento, terá dois controles de conteúdo, um para cada um dos ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Nós queremos mostrar uma marcação padrão da página mestra para o `LoginContent` ContentPlaceHolder essas páginas. Portanto, remova a marcação declarativa para o `Content2` controle de conteúdo. Depois de fazer isso, a marcação das páginas deve conter somente um controle de conteúdo.

Páginas ASP.NET o `Administration` pasta destinam-se somente para usuários administrativos. Adicionamos uma função de administradores ao sistema no <a id="_msoanchor_2"> </a> [ *criando e gerenciando funções* ](../roles/creating-and-managing-roles-cs.md) tutorial; restringir o acesso a essas duas páginas para essa função. Para fazer isso, adicione um `Web.config` o arquivo para o `Administration` pasta e configure seu `<authorization>` elemento admitir usuários na função de administradores e recusar todos os outros.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

Neste ponto Gerenciador de soluções do projeto deve ser semelhante à mostrada na Figura 1 de captura de tela.


[![Quatro novas páginas e um arquivo Web. config foram adicionados para o site](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Figura 1**: quatro novas páginas e um `Web.config` arquivos foram adicionados para o site ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


Por fim, atualize o mapa de site (`Web.sitemap`) para incluir uma entrada para o `ManageUsers.aspx` página. Adicione o seguinte XML após o `<siteMapNode>` adicionamos para os tutoriais de funções.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Com o mapa de site atualizado, visite o site por meio de um navegador. Como mostra a Figura 2, a navegação à esquerda inclui itens para os tutoriais de administração.


[![O mapa do Site inclui um nó chamado administração de usuários](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Figura 2**: O mapa do Site inclui uma administração de usuário denominada do nó ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Etapa 2: Listando todas as contas de usuário em um GridView

Nosso objetivo final para este tutorial é criar uma grade paginável, podem ser filtrada por meio do qual um administrador pode selecionar uma conta de usuário para gerenciar. Vamos começar listagem *todos os* usuários em um controle GridView. Quando isso for concluído, vamos adicionar as interfaces e funcionalidade de paginação e filtragem.

Abra o `ManageUsers.aspx` página o `Administration` pasta e adicionar um controle GridView, definindo seu `ID` para `UserAccounts`. Em alguns momentos, podemos escreverá código para associar o conjunto de contas de usuário para o GridView usando o `Membership` da classe `GetAllUsers` método. Como discutido nos tutoriais anteriores, o método GetAllUsers retorna um `MembershipUserCollection` objeto, que é uma coleção de `MembershipUser` objetos. Cada `MembershipUser` na coleção inclui propriedades como `UserName`, `Email`, `IsApproved`e assim por diante.

Para exibir as informações de conta de usuário desejado em GridView, defina o GridView `AutoGenerateColumns` a propriedade como False e adicionar BoundFields para o `UserName`, `Email`, e `Comment` propriedades e CheckBoxFields para o `IsApproved`, `IsLockedOut`, e `IsOnline` propriedades. Essa configuração pode ser aplicada por meio da marcação declarativa de controle ou a caixa de diálogo de campos. A Figura 3 mostra uma captura de tela dos campos de caixa de diálogo depois que a caixa de seleção de campos de geração automática foi desmarcada e o BoundFields e CheckBoxFields foram adicionados e configurados.


[![Adicionar três BoundFields e três CheckBoxFields a GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Figura 3**: adicionar três BoundFields e três CheckBoxFields a GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


Depois de configurar o GridView, certifique-se de que sua marcação declarativa é semelhante à seguinte:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

Em seguida, é preciso escrever código que associa as contas de usuário a GridView. Crie um método chamado `BindUserAccounts` para executar esta tarefa e, em seguida, chamá-lo no `Page_Load` manipulador de eventos na primeira visita de página.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

Dedicar um tempo para testar a página por meio de um navegador. Como mostra a Figura 4, o `UserAccounts` GridView lista o nome de usuário, endereço de email e outras informações de conta pertinentes para todos os usuários no sistema.


[![As contas de usuário estão listadas em GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Figura 4**: as contas de usuário são listadas em GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Etapa 3: Filtrar os resultados pela primeira letra do nome de usuário

Atualmente o `UserAccounts` GridView mostra *todas as* das contas de usuário. Para sites com centenas ou milhares de contas de usuário, é essencial que o usuário poderá reduzir rapidamente as contas exibidas. Isso pode ser feito adicionando a filtragem de botões de link para a página. Vamos adicionar botões de 27 link à página: uma denominada tudo junto com um LinkButton para cada letra do alfabeto. Se um visitante clica no LinkButton todos, GridView mostrará todos os usuários. Se clicar uma letra específica, somente os usuários cujo nome começa com a letra selecionada serão exibidos.

A primeira tarefa é adicionar os controles LinkButton 27. Seria uma opção criar os botões de 27 link declarativamente, um de cada vez. Uma abordagem mais flexível é usar um controle repetidor com um `ItemTemplate` que renderiza um LinkButton e, em seguida, associa as opções de filtragem para repetidor como um `string` matriz.

Comece a adicionar um controle repetidor a página acima do `UserAccounts` GridView. Definir o repetidor `ID` propriedade `FilteringUI`. Configurar modelos do repetidor para que seu `ItemTemplate` renderiza um LinkButton cujo `Text` e `CommandName` propriedades associadas ao elemento de matriz atual. Como vimos no <a id="_msoanchor_3"> </a> [ *atribuir funções aos usuários* ](../roles/assigning-roles-to-users-cs.md) tutorial, isso pode ser feito usando o `Container.DataItem` sintaxe de associação de dados. Use o repetidor `SeparatorTemplate` para exibir uma linha vertical entre cada link.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Para preencher essa repetidor com as opções de filtragem desejadas, crie um método chamado `BindFilteringUI`. Certifique-se de chamar esse método a partir de `Page_Load` manipulador de eventos para o primeiro carregamento de página.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Esse método Especifica as opções de filtragem como elementos no `string` matriz `filterOptions`. Para cada elemento na matriz, repetidor processará LinkButton com seu `Text` e `CommandName` propriedades atribuídas ao valor do elemento da matriz.

A Figura 5 mostra o `ManageUsers.aspx` página quando visualizada através de um navegador.


[![Repetidor lista 27 botões de link de filtragem](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Figura 5**: O repetidor lista 27 filtragem botões de link ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> Nomes de usuário podem iniciar com qualquer caractere, incluindo números e pontuação. Para exibir essas contas, o administrador terá que usar a opção LinkButton todos. Como alternativa, você pode adicionar um LinkButton para retornar todas as contas de usuário que começam com um número. Posso deixar isso como um exercício para o leitor.


Clicar em qualquer um dos botões de link a filtragem causa um postback e gera o repetidor `ItemCommand` evento, mas não há nenhuma alteração na grade porque temos ainda para escrever nenhum código para filtrar os resultados. O `Membership` classe inclui um [ `FindUsersByName` método](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) que retorna as contas de usuário cujo nome de usuário corresponde a um padrão de pesquisa especificados. Podemos usar esse método para recuperar apenas as contas de usuário cujos nomes começam com a letra especificada pelo `CommandName` no LinkButton filtrado que foi clicado.

Inicie atualizando o `ManageUser.aspx` por trás do código da página de classe para que ele inclui uma propriedade chamada `UsernameToMatch`. Essa propriedade persiste a cadeia de caracteres de filtro de nome de usuário em postagens:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

O `UsernameToMatch` propriedade armazena seu valor é atribuído para o `ViewState` coleção usando a chave UsernameToMatch. Quando o valor desta propriedade é lida, ela verifica se existe um valor na `ViewState` coleção; caso contrário, ele retorna o valor padrão, uma cadeia de caracteres vazia. O `UsernameToMatch` propriedade apresenta um padrão comum, isto é manter um valor para o estado de exibição para que as alterações à propriedade são persistentes entre postbacks. Para obter mais informações sobre esse padrão, leia [Noções básicas sobre estado de exibição de ASP.NET](https://msdn.microsoft.com/library/ms972976.aspx).

Em seguida, atualize o `BindUserAccounts` método para que em vez de chamar `Membership.GetAllUsers`, ele chama `Membership.FindUsersByName`, passando o valor da `UsernameToMatch` propriedade anexada com o caractere curinga SQL, %.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Para exibir apenas os usuários cujo nome começa com a letra A, defina o `UsernameToMatch` propriedade para um e, em seguida, chame `BindUserAccounts`. Isso resulta em uma chamada para `Membership.FindUsersByName("A%")`, que retorna todos os usuários cujo nome começa com A. da mesma forma, para retornar *todos os* usuários, atribuir uma cadeia de caracteres vazia para o `UsernameToMatch` propriedade para que o `BindUserAccounts` método será invocar `Membership.FindUsersByName("%")`e, portanto, retornando todas as contas de usuário.

Criar um manipulador de eventos para o repetidor `ItemCommand` eventos. Esse evento é gerado sempre que um filtro de botões de link é clicado; ele é passado no LinkButton clicado `CommandName` valor por meio de `RepeaterCommandEventArgs` objeto. É necessário atribuir o valor apropriado para o `UsernameToMatch` propriedade e em seguida, chame o `BindUserAccounts` método. Se o `CommandName` é All, atribuir uma cadeia de caracteres vazia para `UsernameToMatch` para que todas as contas de usuário são exibidas. Caso contrário, atribua o `CommandName` valor `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Com esse código, teste a funcionalidade de filtragem. Quando a página for visitada primeiro, todas as contas de usuário são exibidas (consulte novamente a Figura 5). Clicar no LinkButton A causa um postback e filtra os resultados, exibindo apenas as contas de usuário que começam com um.


[![Use os botões de link de filtragem para exibir os usuários cujo nome começa com uma letra determinados](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Figura 6**: Use os botões de filtragem de link para exibir os usuários cujo nome de usuário começa com uma letra determinados ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Etapa 4: Atualizar o GridView para usar a paginação

O GridView mostrado nas figuras 5 e 6 lista todos os registros retornados do `FindUsersByName` método. Se não houver centenas ou milhares de contas de usuário, isso pode levar a sobrecarga de informações ao exibir todas as contas (como é o caso quando clicar no LinkButton todos ou quando inicialmente visitar a página). Para ajudar a apresentar as contas de usuário em partes mais gerenciáveis, vamos configurar o GridView para exibir 10 contas de usuário de cada vez.

O controle GridView oferece dois tipos de paginação:

- **Paginação padrão** - fácil de implementar, mas ineficiente. Em resumo, com padrão paginação GridView espera *todos os* os registros da fonte de dados. Ele apenas exibe a página apropriada de registros.
- **Paginação personalizada** -exige mais trabalho para implementar, mas é mais eficiente do que o padrão de paginação porque com personalizado paginar os dados fonte retorna apenas o conjunto exato de registros a serem exibidos.

A diferença de desempenho entre padrão e paginação personalizada pode ser bastante significativa quando a paginação milhares de registros. Como estamos criando esta interface, supondo que haja pode ser centenas ou milhares de contas de usuário, vamos usar a paginação personalizada.

> [!NOTE]
> Para obter uma discussão mais completa sobre as diferenças entre padrão e paginação personalizada, bem como os desafios envolvidos na implementação de paginação personalizada, consulte [com eficiência por meio de grandes quantidades de dados de paginação](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Para algumas análises de diferença de desempenho entre padrão e paginação personalizada, consulte [paginação personalizada no ASP.NET com o SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Para implementar a paginação personalizada, que primeiro, precisamos algum mecanismo pelo qual recuperar o subconjunto preciso de registros que está sendo exibido pelo GridView. A boa notícia é que o `Membership` da classe `FindUsersByName` método tem uma sobrecarga que nos permite especificar o índice de página e o tamanho de página e retorna apenas as contas de usuário que se enquadram dentro desse intervalo de registros.

Em particular, essa sobrecarga tem a seguinte assinatura: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

O *pageIndex* parâmetro especifica a página de contas de usuário para retornar; *pageSize* indica quantos registros devem ser exibidos por página. O *totalRecords* parâmetro é um `out` parâmetro que retorna o número de contas de usuário total no repositório do usuário.

> [!NOTE]
> Os dados retornados por `FindUsersByName` são classificados por nome de usuário; os critérios de classificação não podem ser personalizados.


GridView pode ser configurada para utilizar a paginação personalizada, mas somente quando associado a um controle ObjectDataSource. Para o controle ObjectDataSource implementar a paginação personalizada, ela requer dois métodos: um que é passado um índice de linha inicial e o número máximo de registros a serem exibidos, e retorna o subconjunto preciso de registros que se enquadram que abrangem; e pager por meio de um método que retorna o número total de registros. O `FindUsersByName` sobrecarga aceita um índice de página e tamanho de página e retorna o número total de registros por meio de um `out` parâmetro. Portanto, há uma incompatibilidade de interface aqui.

Uma opção é criar uma classe de proxy que expõe a interface ObjectDataSource espera e, em seguida, chama internamente o `FindUsersByName` método. Outra opção - e um que usaremos para esse artigo - é criar nossa própria interface de paginação e usá-lo em vez de interface de paginação interna do GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Criando um primeiro, anterior, em seguida, última Interface de paginação

Vamos criar uma interface de paginação com botões de link do último, anterior, Avançar e primeiro. O primeiro LinkButton, quando clicado, levará o usuário para a primeira página de dados, enquanto anterior que ele retornará à página anterior. Da mesma forma, próximo e último moverá o usuário para a próxima e a última página, respectivamente. Adicionar os quatro controles LinkButton sob o `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

Em seguida, crie um manipulador de eventos para cada no LinkButton `Click` eventos.

A Figura 7 mostra quatro botões de link quando exibidas por meio da exibição de Design Visual de desenvolvedor da Web.


[![Adicionar o primeiro, anterior, em seguida, e o último botões de link abaixo GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Figura 7**: adicionar primeiro, anterior, Avançar e botões de link última abaixo GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Manter o controle do índice de página atual

Quando um usuário acessa primeiro o `ManageUsers.aspx` página ou clica em uma da filtragem botões, desejamos exibir a primeira página de dados em GridView. Quando o usuário clica em um dos botões de link do painel de navegação, no entanto, precisamos atualizar o índice de página. Para manter o índice de página e o número de registros a serem exibidos por página, adicione as duas propriedades a seguir para a classe de code-behind da página:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

Como o `UsernameToMatch` propriedade, o `PageIndex` propriedade persiste o valor para o estado de exibição. Somente leitura `PageSize` propriedade retorna um valor embutido, 10. Convidar o leitor de interesse para atualizar esta propriedade para usar o mesmo padrão `PageIndex`e, em seguida, aumentar o `ManageUsers.aspx` página, de modo que a pessoa que está visitando a página pode especificar quantas contas de usuário serão exibidos por página.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recuperar apenas os registros da página atual, atualizando o índice de página e habilitar e desabilitar os botões de link de Interface de paginação

Com a interface de paginação no local e o `PageIndex` e `PageSize` propriedades adicionadas, você está pronto para atualizar o `BindUserAccounts` método assim que ele usa apropriada `FindUsersByName` sobrecarga. Além disso, é preciso ter esse método habilitar ou desabilitar a interface de paginação, dependendo de qual página está sendo exibida. Ao exibir a primeira página de dados, os links primeiro e anterior deve ser desabilitados; Em seguida e última deve ser desabilitado ao exibir a última página.

Atualize o método `BindUserAccounts` pelo seguinte código:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Observe que o número total de registros por meio de pager é determinado pelo último parâmetro do `FindUsersByName` método. Este é um `out` parâmetro, portanto, precisamos primeiro declarar uma variável para manter esse valor (`totalRecords`) e, em seguida, um prefixo com o `out` palavra-chave.

Depois que a página especificada de contas de usuário são retornados, os quatro botões de link estão habilitados ou desabilitados, dependendo se a primeira ou última página de dados estiver sendo exibida.

A última etapa é escrever o código para quatro dos botões de link `Click` manipuladores de eventos. Esses manipuladores de eventos precisam atualizar o `PageIndex` propriedade e, em seguida, associar novamente os dados do GridView por meio de uma chamada para `BindUserAccounts`. A primeira, anterior e próximos manipuladores de eventos são muito simples. O `Click` manipulador de eventos para o último LinkButton, porém, é um pouco mais complexo porque é preciso determinar quantos registros estão sendo exibidos para determinar o último índice de página.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Figuras 8 e 9 mostrar a interface de paginação personalizada em ação. A Figura 8 mostra a `ManageUsers.aspx` página ao exibir a primeira página de dados para todas as contas de usuário. Observe que apenas 10 das contas de 13 são exibidos. Clicando no link próximo ou última causa um postback, atualizações de `PageIndex` para 1 e associa contas à segunda página do usuário à grade (consulte a Figura 9).


[![As contas de usuário de 10 primeiro são exibidas](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Figura 8**: O primeiro contas de usuário de 10 são exibidas ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![Clique no Link próximo exibe a segunda página de contas de usuário](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Figura 9**: clique no Link próximo exibe a segunda página de contas de usuário ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>Resumo

Geralmente, os administradores devem selecionar um usuário na lista de contas. Nos tutoriais anteriores analisamos usando uma lista suspensa preenchida com os usuários, mas essa abordagem não escala bem. Neste tutorial, estamos explorados uma alternativa melhor: uma interface filtrável cujos resultados são exibidos em um GridView paginado. Com essa interface do usuário, os administradores podem rápida e eficaz Localize e selecione uma conta de usuário entre milhares.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Paginação personalizada no ASP.NET com o SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Paginação grandes quantidades de dados com eficiência](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Implantar sua própria ferramenta de administração de site](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Alicja Maziarz. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](recovering-and-changing-passwords-cs.md)
