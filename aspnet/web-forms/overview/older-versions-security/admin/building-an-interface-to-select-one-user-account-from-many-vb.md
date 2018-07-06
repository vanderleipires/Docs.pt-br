---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Criação de uma Interface para selecionar uma conta de usuário dentre muitas (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, vamos criar uma interface do usuário com uma grade paginável, que podem ser filtrada. Em particular, nossa interface do usuário consistirá em uma série de botões de link para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: ec257b09209cb1a377f1ae93b58db4469f438a46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373185"
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Criação de uma Interface para selecionar uma conta de usuário dentre muitas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> Neste tutorial, vamos criar uma interface do usuário com uma grade paginável, que podem ser filtrada. Em particular, a nossa interface do usuário consistirá de uma série de botões de link para filtrar os resultados com base na letra inicial do nome de usuário e um controle GridView para mostrar os usuários correspondentes. Vamos começar listando todas as contas de usuário em um GridView. Em seguida, na etapa 3, vamos adicionar o filtro de botões de link. Etapa 4 examina os resultados filtrados de paginação. A interface criada nas etapas 2 a 4 será ser usada nos próximos tutoriais para executar tarefas administrativas para uma determinada conta de usuário.


## <a name="introduction"></a>Introdução

No <a id="_msoanchor_1"> </a> [ *atribuir funções a usuários* ](../roles/assigning-roles-to-users-vb.md) tutorial, criamos uma interface rudimentar para um administrador para selecionar um usuário e gerenciar suas funções. Especificamente, a interface apresentada o administrador com uma lista suspensa de todos os usuários. Tal interface é adequado quando existem, mas o usuário de uma dúzia ou mais contas, mas é complicado para sites com centenas ou milhares de contas. Uma grade paginada, filtrável é a interface do usuário mais adequada para sites com bases de usuários grandes.

Neste tutorial, vamos criar tal uma interface do usuário. Em particular, a nossa interface do usuário consistirá de uma série de botões de link para filtrar os resultados com base na letra inicial do nome de usuário e um controle GridView para mostrar os usuários correspondentes. Vamos começar listando todas as contas de usuário em um GridView. Em seguida, na etapa 3, vamos adicionar o filtro de botões de link. Etapa 4 examina os resultados filtrados de paginação. A interface criada nas etapas 2 a 4 será ser usada nos próximos tutoriais para executar tarefas administrativas para uma determinada conta de usuário.

Vamos começar!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: Adicionar novas páginas do ASP.NET

Este tutorial e as próximas duas podemos será examinando vários recursos e funções relacionadas à administração. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados durante esses tutoriais. Vamos criar essas páginas e atualizar o mapa de site.

Comece criando uma nova pasta no projeto chamado `Administration`. Em seguida, adicione duas novas páginas do ASP.NET para a pasta, vinculando cada página com o `Site.master` página mestra. Nomeie as páginas:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Também adicione duas páginas para o diretório de raiz do site: `ChangePassword.aspx` e `RecoverPassword.aspx`.

Esses quatro páginas neste ponto, devem, tem dois controles de conteúdo, uma para cada um dos ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Queremos mostrar a marcação de padrão da página mestra para o `LoginContent` ContentPlaceHolder para essas páginas. Portanto, remova a marcação declarativa para a `Content2` controle de conteúdo. Depois de fazer isso, a marcação de páginas deve conter apenas um controle de conteúdo.

As páginas do ASP.NET no `Administration` pasta destinam-se exclusivamente para os usuários administrativos. Adicionamos uma função de administradores de sistema na <a id="_msoanchor_2"> </a> [ *criando e gerenciando funções* ](../roles/creating-and-managing-roles-vb.md) tutorial; restringir o acesso a essas duas páginas a essa função. Para fazer isso, adicione uma `Web.config` o arquivo para o `Administration` pasta e configure seu `<authorization>` elemento admitir que os usuários na função de administradores e para negar a todos os outros.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

Neste ponto, Gerenciador de soluções do seu projeto deve ser semelhante à mostrada na Figura 1 de captura de tela.


[![Quatro novas páginas e um arquivo Web. config foram adicionados ao site](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Figura 1**: quatro novas páginas e uma `Web.config` arquivo foram adicionados ao site ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Por fim, atualize o mapa do site (`Web.sitemap`) para incluir uma entrada para o `ManageUsers.aspx` página. Adicione o seguinte XML após o `<siteMapNode>` adicionamos para os tutoriais de funções.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Com o mapa de site atualizado, visite o site por meio de um navegador. Como mostra a Figura 2, a navegação à esquerda agora inclui itens para os tutoriais de administração.


[![O mapa do Site inclui um nó chamado administração de usuários](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Figura 2**: O mapa do Site inclui um nó intitulada administração do usuário ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Etapa 2: Listando todas as contas de usuário em um GridView

Nossa meta final para este tutorial é criar uma grade paginável, que podem ser filtrada por meio do qual um administrador pode selecionar uma conta de usuário para gerenciar. Vamos começar listando *todos os* usuários em um GridView. Quando isso for concluído, vamos adicionar as interfaces e funcionalidade de filtragem e paginação.

Abra o `ManageUsers.aspx` página na `Administration` pasta e adicione um GridView, definindo seu `ID` para `UserAccounts` daqui a pouco, vamos escrever código para associar o conjunto de contas de usuário para o GridView usando o `Membership` da classe `GetAllUsers` método. Conforme discutido nos tutoriais anteriores, o `GetAllUsers` método retorna um `MembershipUserCollection` objeto, que é uma coleção de `MembershipUser` objetos. Cada `MembershipUser` na coleção inclui propriedades como `UserName`, `Email`, `IsApproved`e assim por diante.

Para exibir as informações de conta de usuário desejado em GridView, defina o GridView `AutoGenerateColumns` a propriedade como False e adicionar BoundFields para o `UserName`, `Email`, e `Comment` propriedades e CheckBoxFields para o `IsApproved`, `IsLockedOut`, e `IsOnline` propriedades. Essa configuração pode ser aplicada por meio de marcação declarativa do controle ou da caixa de diálogo de campos. Figura 3 mostra uma captura de tela dos campos de caixa de diálogo depois que a caixa de seleção de campos de geração automática foi desmarcada e o BoundFields e CheckBoxFields foram adicionadas e configuradas.


[![Adicione três BoundFields e três CheckBoxFields a GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Figura 3**: adicionar três BoundFields e três CheckBoxFields a GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


Depois de configurar o GridView, certifique-se de que sua marcação declarativa é semelhante à seguinte:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Em seguida, precisamos escrever código que associa as contas de usuário para o GridView. Crie um método chamado `BindUserAccounts` para executar essa tarefa e, em seguida, chamá-lo no `Page_Load` manipulador de eventos na primeira visita de página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Reserve um tempo para testar a página por meio de um navegador. Como mostra a Figura 4, o `UserAccounts` GridView lista o nome de usuário, endereço de email e outras informações de conta pertinentes para todos os usuários no sistema.


[![As contas de usuário estão listadas em GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Figura 4**: as contas de usuário são listadas no GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Etapa 3: Filtrando os resultados pela primeira letra do nome de usuário

Atualmente, o `UserAccounts` GridView mostra *todos os* das contas de usuário. Para sites com centenas ou milhares de contas de usuário, é imperativo que o usuário seja capaz de reduzir rapidamente as contas exibidas. Isso pode ser feito adicionando-se a filtragem de botões de link para a página. Vamos adicionar botões de 27 link para a página: um denominado tudo junto com um LinkButton para cada letra do alfabeto. Se um visitante clicar no LinkButton todos, GridView mostrará todos os usuários. Se eles clicarem em uma determinada letra, somente os usuários cujo nome de usuário começa com a letra selecionada serão exibidos.

Nossa primeira tarefa é adicionar os controles LinkButton 27. Uma opção seria criar 27 LinkButtons declarativamente, um de cada vez. Uma abordagem mais flexível é usar um controle Repeater com um `ItemTemplate` que renderiza um LinkButton e, em seguida, associa as opções de filtragem para o Repeater como um `String` matriz.

Iniciar com a adição de um controle Repeater para a página acima a `UserAccounts` GridView. Defina o Repeater `ID` propriedade para `FilteringUI` configurar modelos do repetidor para que seus `ItemTemplate` renderiza um LinkButton cujo `Text` e `CommandName` propriedades são associadas ao elemento de matriz atual. Como vimos na <a id="_msoanchor_3"> </a> [ *atribuir funções a usuários* ](../roles/assigning-roles-to-users-vb.md) tutorial, isso pode ser feito usando o `Container.DataItem` sintaxe de associação de dados. Use o Repeater `SeparatorTemplate` para exibir uma linha vertical entre cada link.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Para preencher essa repetidor com as opções de filtragem desejadas, crie um método chamado `BindFilteringUI`. Certifique-se de chamar esse método a partir de `Page_Load` manipulador de eventos no primeiro carregamento de página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Esse método Especifica as opções de filtragem como elementos na `String` array `filterOptions` para cada elemento na matriz, o Repeater renderizará um LinkButton com seus `Text` e `CommandName` atribuídas ao valor da matriz de propriedades elemento.

A Figura 5 mostra o `ManageUsers.aspx` página quando visualizado por meio de um navegador.


[![O Repeater lista LinkButtons filtragem 27](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Figura 5**: O Repeater lista 27 filtragem LinkButtons ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> Os nomes de usuário podem iniciar com qualquer caractere, incluindo números e pontuação. Para exibir essas contas, o administrador terá que usar a opção de todos os LinkButton. Como alternativa, você pode adicionar um botão LinkButton para retornar todas as contas de usuário que começam com um número. Posso deixar isso como um exercício para o leitor.


Clicar em qualquer um de LinkButtons filtragem faz com que um postback e aciona o Repeater `ItemCommand` evento, mas não há nenhuma alteração na grade, porque ainda não encontramos escrever nenhum código para filtrar os resultados. O `Membership` classe inclui um [ `FindUsersByName` método](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) que retorna as contas de usuário cujo nome de usuário corresponde a um padrão de pesquisa especificado. Podemos usar esse método para recuperar apenas as contas de usuário cujos nomes começam com a letra especificada pelo `CommandName` no LinkButton filtrado que foi clicado.

Comece Atualizando as `ManageUser.aspx` de lógica da página de classe para que ele inclua uma propriedade chamada `UsernameToMatch` essa propriedade persiste a cadeia de caracteres de filtro de nome de usuário entre postbacks:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

O `UsernameToMatch` propriedade armazena o valor será atribuído para o `ViewState` coleção usando a chave UsernameToMatch. Quando o valor desta propriedade é lido, ele verifica se existe um valor no `ViewState` coleção; caso contrário, ele retorna o valor padrão, uma cadeia de caracteres vazia. O `UsernameToMatch` propriedade apresenta um padrão comum, ou seja, persistência de um valor para exibir o estado, de modo que todas as alterações à propriedade são persistentes entre postbacks. Para obter mais informações sobre esse padrão, leia [Noções básicas sobre estado de exibição de ASP.NET](https://msdn.microsoftn-us/library/ms972976.aspx).

Em seguida, atualize o `BindUserAccounts` método para que em vez de chamar `Membership.GetAllUsers`, ele chama `Membership.FindUsersByName`, passando o valor da `UsernameToMatch` property acrescentada com o caractere curinga SQL, %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Para exibir apenas os usuários cujo nome de usuário começa com a letra A, defina as `UsernameToMatch` propriedade para um e, em seguida, chame `BindUserAccounts` isso resultaria em uma chamada para `Membership.FindUsersByName("A%")`, que retornará todos os usuários cujo nome de usuário começa com A. da mesma forma, para retornar *todos os* os usuários, atribuir uma cadeia de caracteres vazia para o `UsernameToMatch` propriedade para que o `BindUserAccounts` invocará o método `Membership.FindUsersByName("%")`e, portanto, retornando todas as contas de usuário.

Criar um manipulador de eventos para o Repeater `ItemCommand` eventos. Esse evento é gerado sempre que um dos filtros de botões de link é clicado; ele é passado no LinkButton clicado `CommandName` de valor por meio de `RepeaterCommandEventArgs` objeto. É necessário atribuir o valor apropriado para o `UsernameToMatch` propriedade e, em seguida, chame o `BindUserAccounts` método. Se o `CommandName` é All, atribuir uma cadeia de caracteres vazia para `UsernameToMatch` para que todas as contas de usuário sejam exibidas. Caso contrário, atribui o `CommandName` valor `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Com esse código funcionando, teste a funcionalidade de filtragem. Quando a página é visitada primeiro, todas as contas de usuário são exibidas (consulte novamente a Figura 5). Clicar no LinkButton um faz com que um postback e filtra os resultados, exibindo apenas as contas de usuário que começam com um.


[![Use filtragem LinkButtons para exibir os usuários cujo nome de usuário começa com uma determinada letra](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Figura 6**: Use LinkButtons filtragem para exibir os usuários cujo nome de usuário começa com uma letra de determinados ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Etapa 4: Atualizando o GridView para usar a paginação

O GridView mostrado nas figuras 5 e 6 lista todos os registros retornados do `FindUsersByName` método. Se não houver centenas ou milhares de contas de usuário, isso pode levar a sobrecarga de informações ao exibir todas as contas (como é o caso quando clicar no LinkButton todos ou quando inicialmente visitando a página). Para ajudar a apresentar as contas de usuário em partes mais gerenciáveis, vamos configurar o GridView para exibir os 10 contas de usuário por vez.

O controle GridView oferece dois tipos de paginação:

- **Paginação padrão** – fácil de implementar, mas ineficiente. Em resumo, com a paginação GridView padrão espera *todos os* os registros da fonte de dados. Ele, em seguida, exibe apenas a página apropriada de registros.
- **Paginação personalizada** -requer mais trabalho para implementar, mas é mais eficiente do que o padrão de paginação como com personalizado paginar os dados fonte retorna apenas o conjunto exato de registros a serem exibidos.

A diferença de desempenho entre padrão e a paginação personalizada pode ser muito significativa, quando a paginação por meio de milhares de registros. Porque estamos criando essa interface, supondo que haja pode ter centenas ou milhares de contas de usuário, vamos usar a paginação personalizada.

> [!NOTE]
> Para obter uma discussão mais completa sobre as diferenças entre a paginação personalizada e padrão, bem como os desafios envolvidos na implementação de paginação personalizada, consulte [com eficiência por meio de grandes quantidades de dados de paginação](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Para algumas análises de diferença de desempenho entre padrão e a paginação personalizada, consulte [paginação personalizada no ASP.NET com o SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Para implementar a paginação personalizada, que primeiro precisamos de algum mecanismo pelo qual recuperar o subconjunto exato de registros que estão sendo exibidos pelo GridView. A boa notícia é que o `Membership` da classe `FindUsersByName` método tem uma sobrecarga que nos permite especificar o índice da página e o tamanho de página e retorna apenas as contas de usuário que se enquadram dentro desse intervalo de registros.

Em particular, essa sobrecarga tem a seguinte assinatura: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

O *pageIndex* parâmetro especifica a página de contas de usuário para retornar; *pageSize* indica quantos registros devem ser exibidos por página. O *totalRecords* parâmetro é um `ByRef` parâmetro que retorna o número de contas de usuário total no armazenamento do usuário.

> [!NOTE]
> Os dados retornados pelo `FindUsersByName` é classificada por nome de usuário; os critérios de classificação não podem ser personalizados.


O GridView pode ser configurado para utilizar a paginação personalizada, mas somente quando associado a um controle ObjectDataSource. Para o controle ObjectDataSource implementar a paginação personalizada, ele requer dois métodos: um que é passado um índice de linha de início e o número máximo de registros a serem exibidos, e retornará o subconjunto exato de registros que se enquadram dentro desse período; e um método que retorna o número total de registros por meio de pager. O `FindUsersByName` sobrecarga aceita um índice de página e tamanho de página e retorna o número total de registros por meio de um `ByRef` parâmetro. Portanto, há uma incompatibilidade de interface aqui.

Uma opção seria criar uma classe de proxy que expõe a interface ObjectDataSource espera e, em seguida, chama internamente o `FindUsersByName` método. Outra opção – e aquele que usaremos para este artigo – é criar nossa própria interface de paginação e usá-lo em vez de interface de paginação interna do GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Criando o primeiro, anterior, em seguida, da última Interface de paginação

Vamos criar uma interface de paginação com a primeira, anterior, próximo e último LinkButtons. A primeira LinkButton, quando clicado, levará o usuário para a primeira página de dados, enquanto que o anterior em contato com ele retornará à página anterior. Da mesma forma, próximo e último moverá o usuário para a próxima e última página, respectivamente. Adicione os quatro controles LinkButton sob o `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Em seguida, crie um manipulador de eventos para cada um no LinkButton `Click` eventos.

Figura 7 mostra quatro botões de link quando visualizado por meio da exibição de Design Visual de desenvolvedor da Web.


[![Adicionar o primeiro, anterior, em seguida, e o último LinkButtons abaixo de GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Figura 7**: adicionar primeiro, anterior, próximo e último LinkButtons sob o controle GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Manter o controle do índice da página atual

Quando um usuário visita primeiro o `ManageUsers.aspx` página ou clica em um da filtragem de botões, queremos exibir a primeira página de dados em um GridView. Quando o usuário clica em um dos botões de link de navegação, no entanto, precisamos atualizar o índice de página. Para manter o índice de página e o número de registros a serem exibidos por página, adicione as duas propriedades a seguir à classe de code-behind da página:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Como o `UsernameToMatch` propriedade, o `PageIndex` propriedade persiste seu valor para o estado de exibição. Somente leitura `PageSize` propriedade retorna um valor embutido em código, 10. Os leitores interessados atualizar esta propriedade para usar o mesmo padrão que eu o convido `PageIndex`e, em seguida, para aumentar o `ManageUsers.aspx` página, de modo que a pessoa que visitar a página pode especificar quantas contas de usuário serem exibidos por página.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recuperar apenas os registros da página atual, atualizando o índice de página e habilitando e desabilitando os botões de link de Interface de paginação

Com a interface de paginação no lugar e o `PageIndex` e `PageSize` propriedades adicionadas, estamos prontos para atualizar o `BindUserAccounts` , de modo que ele usa apropriado `FindUsersByName` de sobrecarga. Além disso, é necessário ter esse método habilitar ou desabilitar a interface de paginação, dependendo de qual página está sendo exibida. Ao exibir a primeira página de dados, os links primeiro e anterior deve ser desabilitados; Em seguida e pela última vez deve ser desabilitada ao exibir a última página.

Atualize o método `BindUserAccounts` pelo seguinte código:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Observe que o número total de registros sendo paginado por meio de é determinado pelo último parâmetro do `FindUsersByName` método. Depois que a página especificada de contas de usuário são retornados, os quatro botões de link são habilitados ou desabilitados, dependendo se a primeira ou última página de dados estiver sendo exibida.

A última etapa é escrever o código para 'LinkButtons quatro `Click` manipuladores de eventos. Esses manipuladores de eventos precisam atualizar o `PageIndex` propriedade e, em seguida, associar novamente os dados para o GridView por meio de uma chamada para `BindUserAccounts` a primeira, anterior e próxima manipuladores de eventos são muito simples. O `Click` manipulador de eventos para o último LinkButton, no entanto, é um pouco mais complexo porque precisamos determinar quantos registros estão sendo exibido para determinar o último índice de página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

As figuras 8 e 9 mostram a interface de paginação personalizada em ação. A Figura 8 mostra o `ManageUsers.aspx` página ao exibir a primeira página de dados para todas as contas de usuário. Observe que apenas 10 das contas de 13 são exibidos. Ao clicar no link próximo ou último faz com que um postback, as atualizações a `PageIndex` para 1 e o associa à segunda página do usuário de contas à grade (veja a Figura 9).


[![As contas de usuário de 10 primeiros são exibidas](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Figura 8**: as contas de usuário de 10 primeiros são exibidas ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Clicando no Link seguinte exibe a segunda página de contas de usuário](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Figura 9**: clicando no Link seguinte exibe a segunda página de contas de usuário ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Resumo

Os administradores muitas vezes precisam selecionar um usuário na lista de contas. Nos tutoriais anteriores, nós examinamos usando uma lista suspensa preenchida com os usuários, mas essa abordagem não dimensiona bem. Neste tutorial, exploramos uma alternativa melhor: uma interface que podem ser filtrada cujos resultados são exibidos em um GridView paginado. Com essa interface do usuário, os administradores podem rapidamente e com eficiência Localize e selecione uma conta de usuário entre milhares.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Paginação personalizada no ASP.NET com o SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Paginação de grandes quantidades de dados com eficiência](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Sem interrupção de sua própria ferramenta de administração de site](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Alicja Maziarz. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em

> [!div class="step-by-step"]
> [Anterior](unlocking-and-approving-user-accounts-cs.md)
> [Próximo](recovering-and-changing-passwords-vb.md)
