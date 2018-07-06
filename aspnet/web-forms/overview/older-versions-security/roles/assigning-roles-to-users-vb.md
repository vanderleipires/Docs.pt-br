---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Atribuindo funções aos usuários (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, criaremos duas páginas do ASP.NET para ajudá-lo com o que os usuários pertencem a quais funções de gerenciamento. A primeira página será incluem recursos para ver o que...
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: 41b73ec54ec2f174bca1d0ee2965ff8e0438ccba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839765"
---
<a name="assigning-roles-to-users-vb"></a>Atribuindo funções aos usuários (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> Neste tutorial, criaremos duas páginas do ASP.NET para ajudá-lo com o que os usuários pertencem a quais funções de gerenciamento. A primeira página será incluem recursos para ver o que os usuários pertencem a uma determinada função, as funções que um determinado usuário pertence e a capacidade de atribuir ou remover um usuário específico de uma função específica. Na segunda página nós será ampliamos o controle CreateUserWizard, para que ele inclui uma etapa para especificar as funções que pertence o usuário recém-criado. Isso é útil em cenários em que um administrador é capaz de criar novas contas de usuário.


## <a name="introduction"></a>Introdução

O <a id="_msoanchor_1"> </a> [tutorial anterior](creating-and-managing-roles-vb.md) examinado o framework de funções e o `SqlRoleProvider`; vimos como usar o `Roles` classe para criar, recuperar e excluir funções. Além de criar e excluir funções, é preciso ser capaz de atribuir ou remover usuários de uma função. Infelizmente, o ASP.NET não é fornecido com todos os controles da Web para gerenciar o que os usuários pertencem a quais funções. Em vez disso, devemos criar nossas próprios páginas do ASP.NET para gerenciar essas associações. A boa notícia é que a adição e remoção de usuários às funções é muito fácil. O `Roles` classe contém um número de métodos para adicionar um ou mais usuários a uma ou mais funções.

Neste tutorial, criaremos duas páginas do ASP.NET para ajudá-lo com o que os usuários pertencem a quais funções de gerenciamento. A primeira página será incluem recursos para ver o que os usuários pertencem a uma determinada função, as funções que um determinado usuário pertence e a capacidade de atribuir ou remover um usuário específico de uma função específica. Na segunda página nós será ampliamos o controle CreateUserWizard, para que ele inclui uma etapa para especificar as funções que pertence o usuário recém-criado. Isso é útil em cenários em que um administrador é capaz de criar novas contas de usuário.

Vamos começar!

## <a name="listing-what-users-belong-to-what-roles"></a>Listando o que os usuários pertencem a quais funções

A ordem do dia para este tutorial é criar uma página da web do qual os usuários podem ser atribuídos a funções. Antes que se preocupar nós mesmos como atribuir usuários às funções, vamos primeiro se concentrar em como determinar o que os usuários pertencem a quais funções. Há duas maneiras de exibir essas informações: "por função" ou "por"usuário. Poderíamos permitir que o visitante selecionar uma função e, em seguida, mostrá-los todos os usuários que pertencem à função (a exibição "por função"), ou podemos pode solicitar que o visitante a selecionar um usuário e, em seguida, mostre a eles as funções atribuídas a esse usuário (a exibição "por usuário").

O modo de exibição "por"função é útil em circunstâncias em que deseja saber o conjunto de usuários que pertencem a uma função específica, o visitante o modo de exibição "por usuário" é ideal quando o visitante precisa saber a função (ões) de um usuário específico. Vamos ter nossa página incluem "por função" e "por usuário" interfaces.

Vamos começar com a criação da interface "por usuário". Essa interface consistirá em uma lista suspensa e uma lista de caixas de seleção. A lista suspensa será preenchida com o conjunto de usuários no sistema; as caixas de seleção irá enumerar as funções. Seleção de um usuário na lista suspensa verificará essas funções que o usuário pertence. A pessoa que visitando a página pode e em seguida, marque ou desmarque as caixas de seleção para adicionar ou remover o usuário selecionado das funções correspondentes.

> [!NOTE]
> Usando uma lista suspensa lista para as contas de usuário não é uma opção ideal para sites onde pode haver centenas de contas de usuário. Uma lista suspensa foi projetada para permitir que um usuário selecionar um item em uma lista relativamente curto de opções. Ele rapidamente se torna complicado à medida que cresce o número de itens de lista. Se você estiver criando um site que terá um grande número de contas de usuário, você talvez queira considerar o uso de uma interface do usuário alternativa, como um GridView paginável ou uma interface filtrável que lista prompts de visitante para escolher uma letra e, em seguida, apenas mostra os usuários cujo nome de usuário começa com a letra selecionada.


## <a name="step-1-building-the-by-user-user-interface"></a>Etapa 1: Criar a Interface do usuário "Por usuário"

Abra o `UsersAndRoles.aspx` página. Na parte superior da página, adicione um controle de rótulo Web chamado `ActionStatus` e limpe seu `Text` propriedade. Usaremos esse rótulo para fornecer comentários sobre as ações executadas, exibindo mensagens como, "Tito do usuário foi adicionado à função administradores do" ou "Usuário Jisun foi removido da função de supervisores." Para tornar essas mensagens se destacam, defina o rótulo `CssClass` propriedade como "Importante".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Em seguida, adicione a seguinte definição de classe CSS para o `Styles.css` folha de estilos:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Essa definição de CSS instrui o navegador para exibir o rótulo usando uma grande fonte vermelha. Figura 1 mostra esse efeito por meio do Designer do Visual Studio.


[![A propriedade do rótulo CssClass resulta em uma grande fonte vermelha](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Figura 1**: O rótulo `CssClass` resultados da propriedade em uma grande, fonte vermelha ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image3.png))


Em seguida, adicione uma DropDownList para a página, defina suas `ID` propriedade para `UserList`e defina seu `AutoPostBack` propriedade como True. Usaremos esse DropDownList para listar todos os usuários no sistema. Este DropDownList será associado a uma coleção de objetos MembershipUser. Como queremos DropDownList para exibir a propriedade de nome de usuário do objeto MembershipUser (e usá-lo como o valor dos itens da lista), defina a DropDownList `DataTextField` e `DataValueField` propriedades para "UserName".

Abaixo DropDownList, adicione um repetidor chamado `UsersRoleList`. Este Repeater lista todas as funções no sistema como uma série de caixas de seleção. Definir o Repeater `ItemTemplate` usando a seguinte marcação declarativa:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

O `ItemTemplate` marcação inclui um único controle de caixa de seleção Web denominado `RoleCheckBox`. A caixa de seleção `AutoPostBack` estiver definida como True e o `Text` propriedade é associada a `Container.DataItem`. O motivo pelo qual a sintaxe de associação de dados é simplesmente `Container.DataItem` é porque a estrutura de funções retorna a lista de nomes de função como uma matriz de cadeia de caracteres e é essa matriz de cadeia de caracteres que haverá a ligação para o Repeater. Uma descrição completa de por que essa sintaxe é usada para exibir o conteúdo de uma matriz associado a um controle de Web de dados está além do escopo deste tutorial. Para obter mais informações sobre essa questão, consulte [associando uma matriz de escalar a um controle de Web de dados](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Neste ponto, marcação declarativa de seu "por usuário" da interface deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Agora estamos prontos para escrever o código para associar o conjunto de contas de usuário para a DropDownList e o conjunto de funções para o Repeater. Na classe de code-behind da página, adicione um método chamado `BindUsersToUserList` e outro chamado `BindRolesList`, usando o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

O `BindUsersToUserList` método recupera todas as contas de usuário no sistema por meio de [ `Membership.GetAllUsers` método](https://msdn.microsoft.com/library/dy8swhya.aspx). Isso retorna um [ `MembershipUserCollection` objeto](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), que é uma coleção de [ `MembershipUser` instâncias](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Essa coleção é então associada para o `UserList` DropDownList. O `MembershipUser` instâncias que a coleção contém uma variedade de propriedades, como de composição `UserName`, `Email`, `CreationDate`, e `IsOnline`. Para instruir a DropDownList para exibir o valor da `UserName` propriedade, certifique-se de que o `UserList` do DropDownList `DataTextField` e `DataValueField` propriedades foram definidas como "UserName".

> [!NOTE]
> O `Membership.GetAllUsers` método tem duas sobrecargas: uma que não aceita a nenhum parâmetro de entrada e retorna todos os usuários e outra que usa valores de inteiro para o índice da página e o tamanho de página e retorna somente o subconjunto especificado dos usuários. Quando houver grandes quantidades de contas de usuário que está sendo exibidas em um elemento de interface do usuário paginável, a segunda sobrecarga pode ser usado para percorrer com mais eficiência os usuários, pois ele retorna apenas o subconjunto preciso de contas de usuário em vez de todos eles.


O `BindRolesToList` método começa com a chamada a `Roles` da classe [ `GetAllRoles` método](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), que retorna uma matriz de cadeia de caracteres que contém as funções do sistema. Essa matriz de cadeia de caracteres é então associado a Repetidor.

Por fim, precisamos chamar esses dois métodos, quando a página é carregada pela primeira vez. Adicione o seguinte código ao manipulador de eventos do `Page_Load`:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Com esse código funcionando, reserve um tempo para visitar a página por meio de um navegador. sua tela deve ser semelhante da Figura 2. Todas as contas de usuário são preenchidas na lista suspensa e, abaixo desse, cada função é exibida como uma caixa de seleção. Porque definimos o `AutoPostBack` propriedades do DropDownList e caixas de seleção como True, alterando o usuário selecionado ou verificando ou desmarcar uma função causa um postback. Nenhuma ação é executada, no entanto, porque ainda temos de escrever código para lidar com essas ações. Que abordaremos essas tarefas nas próximas duas seções.


[![A página exibe os usuários e funções](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Figura 2**: A página exibe os usuários e funções ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Verificando as funções de usuário selecionado pertence a

Quando a página é carregada pela primeira vez ou sempre que o visitante seleciona um novo usuário na lista suspensa, precisamos atualizar o `UsersRoleList`do caixas de seleção para que uma determinada função de caixa de seleção é verificada somente se o usuário selecionado pertence a essa função. Para fazer isso, crie um método chamado `CheckRolesForSelectedUser` com o código a seguir:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

O código acima inicia com a determinação de quem é o usuário selecionado. Em seguida, usa a classe de funções [ `GetRolesForUser(userName)` método](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) para retornar conjunto do usuário especificado de funções como uma matriz de cadeia de caracteres. Em seguida, os itens do repetidor são enumerados e cada item `RoleCheckBox` caixa de seleção é referenciada por meio de programação. A caixa de seleção é marcada, somente se a função que ele corresponde ao que está contida dentro de `selectedUsersRoles` matriz de cadeia de caracteres.

> [!NOTE]
> O `Linq.Enumerable.Contains(Of String)(...)` sintaxe não será compilado se você estiver usando o ASP.NET versão 2.0. O `Contains(Of String)` método faz parte do [biblioteca LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), que é novo no ASP.NET 3.5. Se você ainda estiver usando o ASP.NET versão 2.0, use o [ `Array.IndexOf(Of String)` método](https://msdn.microsoft.com/library/eha9t187.aspx) em vez disso.


O `CheckRolesForSelectedUser` método precisa ser chamado em dois casos: quando a página é carregada pela primeira vez e sempre que o `UserList` índice de selecionado da DropDownList é alterado. Portanto, chamar esse método a partir de `Page_Load` manipulador de eventos (após as chamadas para `BindUsersToUserList` e `BindRolesToList`). Além disso, criar um manipulador de eventos para a DropDownList `SelectedIndexChanged` eventos e chamar esse método a partir daí.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Com esse código em vigor, você pode testar a página por meio do navegador. No entanto, como o `UsersAndRoles.aspx` página atualmente não tem a capacidade de atribuir usuários às funções, nenhum usuário tem funções. Vamos criar a interface para atribuir usuários a funções em breve, portanto, você pode tirar o word que esse código funciona e verifique se que ele faz isso mais tarde, ou você pode adicionar manualmente os usuários a funções, inserindo registros no `aspnet_UsersInRoles` tabela para testar este functi onality agora.

### <a name="assigning-and-removing-users-from-roles"></a>Atribuir e remover usuários das funções

Quando o visitante ou desmarca uma caixa de seleção no `UsersRoleList` Repeater, precisamos adicionar ou remover o usuário selecionado da função correspondente. A caixa de seleção `AutoPostBack` propriedade atualmente é definida como True, o que faz com que um postback, sempre que uma caixa de seleção no repetidor está marcado ou desmarcado. Em resumo, precisamos criar um manipulador de eventos para a caixa de seleção `CheckChanged` eventos. Como a caixa de seleção estiver em um controle Repeater, precisamos adicionar manualmente o direcionamento de manipulador de eventos. Comece adicionando o manipulador de eventos para a classe code-behind, como um `Protected` método, da seguinte forma:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Voltaremos para escrever o código para este manipulador de eventos em alguns instantes. Mas primeiro vamos concluir o direcionamento de manipulação de eventos. Na caixa de seleção dentro do repetidor `ItemTemplate`, adicione `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Essa sintaxe conecta o `RoleCheckBox_CheckChanged` manipulador de eventos para o `RoleCheckBox`do `CheckedChanged` eventos.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

Nossa tarefa final é concluir o `RoleCheckBox_CheckChanged` manipulador de eventos. É preciso começar referenciando o controle de caixa de seleção que acionou o evento porque esta instância de caixa de seleção nos informa qual a função foi marcada ou desmarcada por meio de seu `Text` e `Checked` propriedades. Usando essas informações junto com o nome de usuário do usuário selecionado, podemos adicionar ou remover o usuário da função por meio de `Roles` da classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) ou [ `RemoveUserFromRole` método](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

O código acima inicia referenciando programaticamente a caixa de seleção que acionou o evento, que está disponível via o `sender` parâmetro de entrada. Se a caixa de seleção estiver marcada, o usuário selecionado é adicionado à função especificada, caso contrário, que eles serão removidos da função. Em ambos os casos, o `ActionStatus` rótulo exibe uma mensagem resumindo a ação que acabou de ser executada.

Reserve um tempo para testar esta página por meio de um navegador. Selecione o usuário Tito e, em seguida, adicionar Tito para os administradores e supervisores de funções.


[![Tito foi adicionado para os administradores e funções de supervisores](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Figura 3**: Tito foi adicionado para os administradores e funções de supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image9.png))


Em seguida, selecione o usuário Bruce na lista suspensa. Há um postback e caixas de seleção do repetidor serão atualizadas por meio de `CheckRolesForSelectedUser`. Já que Bruce ainda não pertence a qualquer função, duas caixas de seleção estão desmarcadas. Em seguida, adicione Bruce à função de supervisores.


[![Bruce foi adicionado à função de supervisores](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Figura 4**: Bruce foi adicionado à função de supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image12.png))


Para verificar a funcionalidade do `CheckRolesForSelectedUser` método, selecione um usuário diferente Tito ou Bruce. Observe como as caixas de seleção estão desmarcadas, automaticamente, indicando que eles não pertencem a todas as funções. Retorne ao Tito. Os administradores e supervisores de caixas de seleção devem ser verificadas.

## <a name="step-2-building-the-by-roles-user-interface"></a>Etapa 2: Criar a Interface do usuário "Por funções"

Neste ponto, podemos concluiu a interface "por"usuários e está prontos para começar a lidar com a interface "por funções". A interface "por funções" solicita que o usuário selecionar uma função de uma lista suspensa e, em seguida, exibe o conjunto de usuários que pertencem a essa função em um GridView.

Adicione outro controle DropDownList para a `UsersAndRoles.aspx page`. Colocar esse um sob o controle Repeater, nomeie- `RoleList`e defina seu `AutoPostBack` propriedade como True. Abaixo desse, adicione um controle GridView e denomine- `RolesUserList`. Este GridView listará os usuários que pertencem à função selecionada. Defina o GridView `AutoGenerateColumns` a propriedade como False, adicione um TemplateField para a grade `Columns` coleção e o conjunto seu `HeaderText` propriedade para "Usuários". Definir o TemplateField `ItemTemplate` para que ele exibe o valor da expressão de associação de dados `Container.DataItem` na `Text` propriedade de um rótulo denominado `UserNameLabel`.

Depois de adicionar e configurar o GridView, marcação declarativa de seu "por função" da interface deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Precisamos preencher o `RoleList` DropDownList com o conjunto de funções no sistema. Para fazer isso, atualize o `BindRolesToList` , de modo que é o associa a matriz de cadeia de caracteres retornada pela `Roles.GetAllRoles` método para o `RolesList` DropDownList (bem como o `UsersRoleList` Repeater).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

As duas últimas linhas na `BindRolesToList` método foram adicionados ao associar o conjunto de funções para o `RoleList` controle DropDownList. Figura 5 mostra o resultado final quando visualizado por meio de um navegador – uma lista suspensa preenchida com as funções do sistema.


[![As funções são exibidas na RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Figura 5**: as funções são exibidas na `RoleList` DropDownList ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Exibindo os usuários que pertencem à função selecionada

Quando a página é carregada pela primeira vez ou quando uma nova função é selecionada do `RoleList` DropDownList, é necessário exibir a lista de usuários que pertencem a essa função em GridView. Crie um método chamado `DisplayUsersBelongingToRole` usando o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Esse método começa Obtendo a função selecionada do `RoleList` DropDownList. Ele usa o [ `Roles.GetUsersInRole(roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) para recuperar uma matriz de cadeia de caracteres dos nomes de usuário dos usuários que pertencem a essa função. Essa matriz é então associado para o `RolesUserList` GridView.

Esse método precisa ser chamado em duas circunstâncias: quando a página é inicialmente carregada e quando a função selecionada no `RoleList` DropDownList alterações. Portanto, atualizar o `Page_Load` manipulador de eventos para que esse método é invocado após a chamada para `CheckRolesForSelectedUser`. Em seguida, crie um manipulador de eventos para o `RoleList`do `SelectedIndexChanged` evento e chamar esse método a partir daí, muito.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Com esse código funcionando, o `RolesUserList` GridView deve exibir aos usuários que pertencem à função selecionada. Como mostra a Figura 6, a função de supervisores consiste em dois membros: Bruce e Tito.


[![O GridView lista os usuários que pertencem à função selecionada](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Figura 6**: O GridView lista aqueles usuários que pertencem à função selecionada ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Removendo usuários da função selecionada

Vamos ampliar o `RolesUserList` GridView para que ele inclua uma coluna de "remover" botões. Clicar no botão "Remover" para um determinado usuário irá removê-los dessa função.

Comece adicionando um campo de botão de exclusão para o GridView. Tornar este campo são exibidos como esquerda mais arquivada e alterar seu `DeleteText` propriedade de "Excluir" (o padrão) para "Remover".


[![Adicionar o](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Figura 7**: adicionar o botão "Remover" para o GridView ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image21.png))


Quando se clica no botão "Remover" massacre de um postback e o GridView `RowDeleting` é gerado. É necessário criar um manipulador de eventos para esse evento e escrever um código que remove o usuário da função selecionada. Crie o manipulador de eventos e, em seguida, adicione o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

O código começa pela determinação do nome da função selecionada. Em seguida, ele programaticamente as referências a `UserNameLabel` controle da linha cujo "Remover" botão foi clicado para determinar o nome de usuário a ser removido. O usuário, em seguida, é removido da função por meio de uma chamada para o `Roles.RemoveUserFromRole` método. O `RolesUserList` GridView é atualizada e uma mensagem é exibida por meio de `ActionStatus` controle de rótulo.

> [!NOTE]
> O botão "Remover" não exige qualquer tipo de confirmação do usuário antes de remover o usuário da função. Convido você a adicionar algum nível de confirmação do usuário. Uma das maneiras mais fáceis para confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [' adicionando confirmação do lado do cliente quando excluindo](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Figura 8 mostra a página depois que o usuário Tito tiver sido removido do grupo de supervisores.


[![Infelizmente, Tito não é mais um Supervisor](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Figura 8**: Infelizmente, Tito não é mais um Supervisor ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Adicionar novos usuários à função selecionada

Junto com a remoção de usuários da função selecionada, o visitante a esta página também deve ser capaz de adicionar um usuário à função selecionada. A melhor interface para adicionar um usuário à função selecionada depende do número de contas de usuário que você espera ter. Se seu site conterá apenas algumas contas de usuário dezenas ou menos, você pode usar uma DropDownList aqui. Se pode haver milhares de contas de usuário, convém incluir uma interface do usuário que permite que o visitante a página por meio das contas, procure uma conta específica, ou filtrar as contas de usuário de alguma outra forma.

Para esta página Vamos usar uma interface muito simples que funciona independentemente do número de contas de usuário no sistema. Ou seja, usaremos uma caixa de texto, solicitando que o visitante digitar o nome de usuário do usuário, que ela deseja adicionar à função selecionada. Se nenhum usuário com esse nome existe ou se o usuário já for um membro da função, vamos exibir uma mensagem em `ActionStatus` rótulo. Mas, se o usuário existe e não é um membro da função, vamos adicioná-los à função e atualizar a grade.

Adicione uma caixa de texto e um botão abaixo o GridView. Defina a caixa de texto `ID` ao `UserNameToAddToRole` e defina o botão `ID` e `Text` propriedades a serem `AddUserToRoleButton` e "Adicionar usuário à função", respectivamente.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Em seguida, crie uma `Click` manipulador de eventos para o `AddUserToRoleButton` e adicione o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

A maioria do código no `Click` manipulador de eventos executa várias verificações de validação. Isso garante que o visitante fornecido um nome de usuário na `UserNameToAddToRole` TextBox, que o usuário existe no sistema e que eles já não pertencem à função selecionada. Se qualquer uma dessas verificações falhar, uma mensagem apropriada é exibida no `ActionStatus` e o manipulador de eventos é encerrado. Se passarem por todas as verificações, o usuário é adicionado à função por meio de `Roles.AddUserToRole` método. Depois disso, a caixa de texto `Text` propriedade é limpo, GridView é atualizado e o `ActionStatus` rótulo exibe uma mensagem indicando que o usuário especificado foi adicionado com êxito à função selecionada.

> [!NOTE]
> Para garantir que o usuário especificado já não pertence à função selecionada, podemos usar o [ `Roles.IsUserInRole(userName, roleName)` método](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), que retorna um valor booliano que indica se *userName* é um membro da *roleName*. Usaremos esse método novamente na <a id="_msoanchor_2"> </a> [próximo tutorial](role-based-authorization-vb.md) quando examinarmos a autorização baseada em função.


Visite a página por meio de um navegador e selecione a função de supervisores do `RoleList` DropDownList. Tente inserir um nome de usuário inválido – você deve ver uma mensagem explicando que o usuário não existe no sistema.


[![Você não pode adicionar um usuário não existente a uma função](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Figura 9**: você não pode adicionar um usuário não existente a uma função ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image27.png))


Agora, tente adicionar um usuário válido. Vá em frente e adicione novamente Tito à função de supervisores.


[![Tito mais uma vez é um Supervisor!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Figura 10**: Tito mais uma vez é um Supervisor!  ([Clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Etapa 3: Cross-atualizando o "Por usuário" e "Por função" Interfaces

O `UsersAndRoles.aspx` página oferece duas interfaces distintas para gerenciar usuários e funções. No momento, essas duas interfaces atuam independentemente um do outro, portanto, é possível que uma alteração feita em uma interface não será refletida imediatamente no outro. Por exemplo, imagine que o visitante para a página, selecione a função de supervisores do `RoleList` DropDownList, que lista Bruce e Tito como seus membros. Em seguida, o visitante seleciona Tito do `UserList` DropDownList, que verifica as caixas de seleção de administradores e supervisores no `UsersRoleList` Repeater. Se o visitante desmarca, em seguida, a função de Supervisor do repetidor, Tito é removida da função supervisores, mas essa modificação não será refletida na interface "por função". O GridView ainda mostrará Tito como sendo um membro da função de supervisores.

Para corrigir isso, precisamos atualizar o GridView, sempre que uma função é marcado ou desmarcado do `UsersRoleList` Repeater. Da mesma forma, precisamos atualizar o Repeater, sempre que um usuário for removido ou adicionado a uma função da interface "por função".

O Repeater na interface "por usuário" é atualizado chamando o `CheckRolesForSelectedUser` método. A interface "por função" pode ser modificada na `RolesUserList` do GridView `RowDeleting` manipulador de eventos e o `AddUserToRoleButton` do botão `Click` manipulador de eventos. Portanto, precisamos chamar o `CheckRolesForSelectedUser` método de cada um desses métodos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Da mesma forma, o GridView na interface "por função" é atualizado chamando o `DisplayUsersBelongingToRole` método e a interface "por usuário" é modificado por meio de `RoleCheckBox_CheckChanged` manipulador de eventos. Portanto, precisamos chamar o `DisplayUsersBelongingToRole` método do manipulador de eventos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Com essas alterações de código secundária, o "por usuário" e "por função" interfaces agora corretamente entre a atualização. Para verificar isso, visite a página por meio de um navegador e selecione Tito e supervisores do `UserList` e `RoleList` DropDownLists, respectivamente. Observe que conforme você desmarque a função de supervisores de Tito do repetidor na interface "por usuário", Tito é automaticamente removido do GridView na interface "por função". Novamente adicionando Tito volta para a função de supervisores da interface "por"função automaticamente verifica se a caixa de seleção de supervisores na interface "por usuário".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Etapa 4: Personalizar o CreateUserWizard para incluir uma etapa "Especificar funções"

No <a id="_msoanchor_3"> </a> [ *criando contas de usuário* ](../membership/creating-user-accounts-vb.md) tutorial vimos como usar o controle CreateUserWizard Web para fornecer uma interface para criar uma nova conta de usuário. O controle CreateUserWizard pode ser usado em uma das duas maneiras:

- Como um meio para os visitantes criar sua própria conta de usuário no site, e
- Como um meio para os administradores criar novas contas

No primeiro caso de uso, um visitante vem para o site e preenche o CreateUserWizard, inserir suas informações para se registrar no site. No segundo caso, um administrador cria uma nova conta para outra pessoa.

Quando uma conta está sendo criada por um administrador para alguma outra pessoa, ele pode ser útil permitir que o administrador especifique as funções que pertence a nova conta de usuário. No <a id="_msoanchor_4"> </a> [ *armazenando* *informações adicionais do usuário* ](../membership/storing-additional-user-information-vb.md) tutorial vimos como personalizar o CreateUserWizard, adicionando adicionais `WizardSteps`. Vamos examinar como adicionar uma etapa adicional para o CreateUserWizard para especificar as novas funções do usuário.

Abra o `CreateUserWizardWithRoles.aspx` da página e adicione um controle CreateUserWizard chamado `RegisterUserWithRoles`. Defina o controle `ContinueDestinationPageUrl` propriedade como "~ / default. aspx". Como a ideia aqui é que um administrador usará esse controle CreateUserWizard para criar novas contas de usuário, defina o controle `LoginCreatedUser` a propriedade como False. Isso `LoginCreatedUser` propriedade especifica se o visitante é automaticamente conectado como usuário recém-criada, e ele assume True como padrão. Podemos configurá-lo como False quando um administrador cria uma nova conta de nós queremos manter em contato com ele entrou como si próprio.

Em seguida, selecione o "Adicionar/remover `WizardSteps`..." opção de marca inteligente do CreateUserWizard e adicione um novo `WizardStep`, definindo suas `ID` para `SpecifyRolesStep`. Mover o `SpecifyRolesStep WizardStep` para que ele vem após a etapa de "Sign Up for Your New Account", mas antes da etapa de "Concluído". Defina a `WizardStep`do `Title` propriedade como "Especificar funções", seu `StepType` propriedade a ser `Step`e seu `AllowReturn` propriedade como False.


[![Adicionar o](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Figura 11**: adicione "Especificar funções" `WizardStep` para o CreateUserWizard ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image33.png))


Após essa alteração marcação declarativa do seu CreateUserWizard deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

Em "Especificar funções" `WizardStep`, adicione um CheckBoxList chamado `RoleList.` CheckBoxList este listará as funções disponíveis, permitindo que a pessoa que visitar a página verificar as funções que o usuário recém-criado pertence.

Continuamos com duas tarefas de codificação: primeiro, deve preencher o `RoleList` CheckBoxList com as funções do sistema; em segundo lugar, precisamos adicionar o usuário criado para as funções selecionadas, quando o usuário move da etapa "Especificar funções" para a etapa de "Concluído". Realizamos a primeira tarefa no `Page_Load` manipulador de eventos. O código a seguir por meio de programação referências a `RoleList` caixa de seleção na primeira visite a página e associa as funções do sistema a ele.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

O código acima deve parecer familiar. No <a id="_msoanchor_5"> </a> [ *armazenando* *informações adicionais do usuário* ](../membership/storing-additional-user-information-vb.md) tutorial, usamos dois `FindControl` instruções para fazer referência a um controle da Web de dentro de um personalizado `WizardStep`. E o código que associa as funções a CheckBoxList foi retirado neste tutorial.

Para executar a segunda tarefa de programação, precisamos saber quando a etapa "Especificar funções" foi concluída. Lembre-se que o CreateUserWizard tem um `ActiveStepChanged` evento, que é acionado sempre que o visitante navega de uma etapa para outro. Aqui podemos determinar se o usuário atingiu a etapa de "Concluído". Nesse caso, é necessário adicionar o usuário a funções selecionadas.

Crie um manipulador de eventos para o `ActiveStepChanged` evento e adicione o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Se o usuário apenas atingiu a etapa de "Concluído", o manipulador de eventos enumera os itens do `RoleList` CheckBoxList e o usuário recém-criada é atribuído a funções selecionadas.

Visite esta página por meio de um navegador. A primeira etapa na CreateUserWizard é a etapa de "Sign Up for Your New Account" padrão, que solicitará o novo nome de usuário, senha, email e outras informações importantes. Insira as informações para criar um novo usuário chamado Wanda.


[![Criar um novo usuário denominado Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Figura 12**: criar um novo usuário denominado Wanda ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image36.png))


Clique no botão "Criar usuário". O CreateUserWizard chama internamente o `Membership.CreateUser` método, criando a nova conta de usuário e, em seguida, avança para a próxima etapa, "Especifica funções." Aqui, as funções do sistema são listadas. Marque a caixa de seleção de supervisores e clique em Avançar.


[![Tornar um membro da função de supervisores de Wanda](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Figura 13**: tornar um membro da função de supervisores de Wanda ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image39.png))


Clicar em Avançar faz com que um postback e atualizações de `ActiveStep` para a etapa de "Concluído". No `ActiveStepChanged` manipulador de eventos, a conta de usuário criado recentemente é atribuída à função de supervisores. Para verificar isso e retornar para o `UsersAndRoles.aspx` página e selecione os supervisores do `RoleList` DropDownList. Como mostra a Figura 14, supervisores agora são compostos de três usuários: Bruce, Tito e Wanda.


[![Bruce, Tito e Wanda são todos os supervisores](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Figura 14**: Bruce, Tito e Wanda são todos os supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Resumo

A estrutura de funções oferece métodos para recuperar informações sobre as funções e métodos para determinar o que os usuários pertencem a uma função especificada de um usuário específico. Além disso, existem vários métodos para adicionar e remover um ou mais usuários a uma ou mais funções. Neste tutorial, nos concentramos em apenas dois desses métodos: `AddUserToRole` e `RemoveUserFromRole`. Há outras variantes projetadas para adicionar vários usuários a uma única função e atribuir várias funções para um único usuário.

Este tutorial também incluído uma olhada no estendendo o controle CreateUserWizard para incluir um `WizardStep` para especificar as funções do usuário recém-criado. Uma etapa como essa poderia ajudar um administrador a simplificar o processo de criação de contas de usuário para novos usuários.

Neste ponto, nós vimos como criar e excluir funções e como adicionar e remover usuários das funções. Mas, ainda precisamos examinar aplicar a autorização baseada em função. No <a id="_msoanchor_6"> </a> [seguinte tutorial](role-based-authorization-vb.md) examinaremos definindo regras de autorização de URL em uma base de função por função, bem como limitar a funcionalidade de nível de página com base nas funções do usuário conectado no momento.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Visão geral da ferramenta de administração Site da Web do ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Examinando o ASP. Associações, funções e perfis do NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Sem interrupção de sua própria ferramenta de administração de site](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-and-managing-roles-vb.md)
> [Próximo](role-based-authorization-vb.md)
