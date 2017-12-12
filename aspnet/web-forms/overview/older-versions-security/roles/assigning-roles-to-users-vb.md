---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: "Atribuir funções aos usuários (VB) | Microsoft Docs"
author: rick-anderson
description: "Neste tutorial, que vamos criar duas páginas do ASP.NET para ajudá-lo a gerenciar o que os usuários pertencem a quais funções. A primeira página inclui recursos para ver qual..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: 528d6f844c68cc7077a86961f9c432e405f22ee7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="assigning-roles-to-users-vb"></a>Atribuir funções aos usuários (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> Neste tutorial, que vamos criar duas páginas do ASP.NET para ajudá-lo a gerenciar o que os usuários pertencem a quais funções. A primeira página inclui recursos para ver o que os usuários pertencem a uma determinada função, as funções que um usuário específico pertence e a capacidade de atribuir ou remover um usuário específico de uma função específica. Na segunda página, serão aumentar o controle CreateUserWizard para que ele inclui uma etapa para especificar a quais funções pertence o usuário recém-criado. Isso é útil em cenários onde um administrador é capaz de criar novas contas de usuário.


## <a name="introduction"></a>Introdução

O <a id="_msoanchor_1"> </a> [tutorial anterior](creating-and-managing-roles-vb.md) examinou o framework de funções e o `SqlRoleProvider`; vimos como usar o `Roles` classe para criar, recuperar e excluir funções. Além de criar e excluir funções, é preciso ser capaz de atribuir ou remover usuários de uma função. Infelizmente, o ASP.NET não é fornecido com os controles da Web para que os usuários pertencem a quais funções de gerenciamento. Em vez disso, devemos criar nossas própria páginas ASP.NET para gerenciar essas associações. A boa notícia é que a adição e remoção de usuários às funções é muito fácil. O `Roles` classe contém um número de métodos para adicionar um ou mais usuários a uma ou mais funções.

Neste tutorial, que vamos criar duas páginas do ASP.NET para ajudá-lo a gerenciar o que os usuários pertencem a quais funções. A primeira página inclui recursos para ver o que os usuários pertencem a uma determinada função, as funções que um usuário específico pertence e a capacidade de atribuir ou remover um usuário específico de uma função específica. Na segunda página, serão aumentar o controle CreateUserWizard para que ele inclui uma etapa para especificar a quais funções pertence o usuário recém-criado. Isso é útil em cenários onde um administrador é capaz de criar novas contas de usuário.

Vamos começar!

## <a name="listing-what-users-belong-to-what-roles"></a>Listando o que os usuários pertencem a quais funções

A primeira tarefa prioritária para este tutorial é criar uma página da web do qual os usuários podem ser atribuídos a funções. Antes de em nós preocupar com como atribuir usuários a funções, vamos primeiro se concentrem em como determinar o que os usuários pertencem a quais funções. Há duas maneiras de exibir essas informações: "função" ou "por usuário." Pode permitir que o visitante selecionar uma função e, em seguida, mostrá-los todos os usuários que pertencem à função (a exibição "por função"), ou podemos pode solicitar o visitante para selecionar um usuário e mostrá-los as funções atribuídas ao usuário (a exibição "por usuário").

O modo de exibição "por função" é útil em circunstâncias em que o visitante que saber o conjunto de usuários que pertencem a uma função específica; o modo de exibição "por usuário" é ideal quando o visitante precisa saber as funções de um usuário específico. Vamos dar nossa página incluem "pela função" e "por usuário" interfaces.

Vamos começar a criar a interface "pelo usuário". Essa interface consistirá em uma lista suspensa e uma lista de caixas de seleção. A lista suspensa será preenchida com o conjunto de usuários no sistema. as caixas de seleção vai enumerar as funções. Selecionar um usuário na lista suspensa verificará essas funções que o usuário pertence. A pessoa que está visitando a página pode, em seguida, marque ou desmarque as caixas de seleção para adicionar ou remover o usuário selecionado de funções correspondentes.

> [!NOTE]
> Usando uma lista suspensa lista para as contas de usuário não é uma opção ideal para sites onde pode haver centenas de contas de usuário. Uma lista suspensa foi projetada para permitir que um usuário selecione um item de uma lista relativamente curto de opções. Rapidamente torna difícil à medida que aumenta o número de itens da lista. Se você estiver criando um site que terá um grande número de contas de usuário, você talvez queira considerar usando uma interface de usuário alternativas, como prompts de GridView paginável ou uma interface filtrável que lista o visitante para escolher uma letra e somente mostra os usuários cujo nome começa com a letra selecionada.


## <a name="step-1-building-the-by-user-user-interface"></a>Etapa 1: Criar a Interface do usuário "Pelo usuário"

Abra o `UsersAndRoles.aspx` página. Na parte superior da página, adicione um controle de rótulo Web chamado `ActionStatus` e limpar seu `Text` propriedade. Usaremos esse rótulo para fornecer comentários sobre as ações executadas, exibindo mensagens como, "usuário Tito foi adicionado à função administradores do" ou "Jisun de usuário foi removido da função de supervisores." Para fazer essas mensagens se destacam, defina o rótulo `CssClass` propriedade como "Importante".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Em seguida, adicione a seguinte definição de classe CSS para o `Styles.css` folha de estilos:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Esta definição CSS instrui o navegador para exibir o rótulo usando uma fonte grande, vermelha. A Figura 1 mostra esse efeito por meio do Designer do Visual Studio.


[![A propriedade do rótulo CssClass resulta em uma fonte grande, vermelha](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Figura 1**: do rótulo `CssClass` resultados de propriedade em uma grande, fonte vermelha ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image3.png))


Em seguida, adicionar DropDownList para a página, defina seu `ID` propriedade `UserList`e defina seu `AutoPostBack` propriedade como True. Usaremos essa DropDownList para listar todos os usuários no sistema. Este DropDownList será associada a uma coleção de objetos MembershipUser. Como desejamos DropDownList para exibir a propriedade de nome de usuário do objeto MembershipUser (e usá-lo como o valor dos itens de lista), defina a DropDownList `DataTextField` e `DataValueField` propriedades para "UserName".

Abaixo DropDownList, adicione um repetidor denominado `UsersRoleList`. Este repetidor lista todas as funções no sistema como uma série de caixas de seleção. Definir o repetidor `ItemTemplate` usando a seguinte marcação declarativa:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

O `ItemTemplate` marcação inclui um controle de caixa de seleção Web único chamado `RoleCheckBox`. A caixa de seleção `AutoPostBack` propriedade é definida como True e o `Text` propriedade está vinculada a `Container.DataItem`. O motivo pelo qual a sintaxe de associação de dados é simplesmente `Container.DataItem` porque a estrutura de funções retorna a lista de nomes de função como uma matriz de cadeia de caracteres, e é essa matriz de cadeia de caracteres que haverá a ligação para Repetidor. Uma descrição completa de por que essa sintaxe é usada para exibir o conteúdo de uma matriz associada a um controle de Web de dados está além do escopo deste tutorial. Para obter mais informações sobre esse assunto, consulte [uma matriz escalar de associação a um controle de Web dados](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Neste ponto declarativo seu "pelo usuário" da interface deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Agora você está pronto para escrever o código para associar o conjunto de contas de usuário para DropDownList e o conjunto de funções de Repetidor. Na classe de code-behind da página, adicione um método chamado `BindUsersToUserList` e outro chamado `BindRolesList`, usando o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

O `BindUsersToUserList` método recupera todas as contas de usuário no sistema por meio de [ `Membership.GetAllUsers` método](https://msdn.microsoft.com/en-us/library/dy8swhya.aspx). Isso retorna um [ `MembershipUserCollection` objeto](https://msdn.microsoft.com/en-us/library/system.web.security.membershipusercollection.aspx), que é uma coleção de [ `MembershipUser` instâncias](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx). Essa coleção é então associada para o `UserList` DropDownList. O `MembershipUser` instâncias que composição a coleção contém uma variedade de propriedades, como `UserName`, `Email`, `CreationDate`, e `IsOnline`. Para instruir o DropDownList para exibir o valor da `UserName` propriedade, certifique-se de que o `UserList` do DropDownList `DataTextField` e `DataValueField` propriedades foram definidas como "UserName".

> [!NOTE]
> O `Membership.GetAllUsers` método tem duas sobrecargas: um que não aceita nenhum parâmetro de entrada e retorna todos os usuários e outro que usa valores inteiros para o índice de página e tamanho de página e retorna somente o subconjunto especificado dos usuários. Quando houver grandes quantidades de contas de usuário que está sendo exibidas em um elemento de interface do usuário paginável, a segunda sobrecarga pode ser usado para percorrer com mais eficiência os usuários já que ele retorna apenas o subconjunto preciso de contas de usuário em vez de todos eles.


O `BindRolesToList` método inicia chamando o `Roles` da classe [ `GetAllRoles` método](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getallroles.aspx), que retorna uma matriz de cadeia de caracteres que contém as funções do sistema. Esta matriz de cadeia de caracteres é então associado a Repetidor.

Por fim, é preciso chamar esses dois métodos, quando a página for carregada pela primeira vez. Adicione o seguinte código ao manipulador de eventos do `Page_Load`:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Com esse código, reserve um tempo para visitar a página por meio de um navegador. sua tela deve ser semelhante a Figura 2. Todas as contas de usuário são preenchidas na lista suspensa e, abaixo dele, cada função é exibida como uma caixa de seleção. Como definimos o `AutoPostBack` propriedades do DropDownList e caixas de seleção para True, alterando o usuário selecionado ou marcar ou desmarcar uma função causa um postback. Nenhuma ação é executada, no entanto, como temos ainda escrever código para lidar com essas ações. Que abordaremos essas tarefas nas próximas duas seções.


[![A página exibe os usuários e funções](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Figura 2**: A página exibe os usuários e funções ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Verificando as funções do usuário selecionado pertence a

Quando a página for carregada pela primeira vez, ou sempre que o visitante seleciona um novo usuário na lista suspensa, precisamos atualizar o `UsersRoleList`de caixas de seleção para que uma caixa de seleção de função é verificada apenas se o usuário selecionado pertence a essa função. Para fazer isso, crie um método chamado `CheckRolesForSelectedUser` com o código a seguir:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

Inicia o código acima, determinando quem é o usuário selecionado. Ele então usa a classe de funções [ `GetRolesForUser(userName)` método](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getrolesforuser.aspx) para retornar conjunto do usuário especificado de funções como uma matriz de cadeia de caracteres. Em seguida, os itens do repetidor são enumeradas e cada item `RoleCheckBox` caixa de seleção é referenciada por meio de programação. A caixa de seleção estiver marcada, somente se ela corresponde à função está dentro do `selectedUsersRoles` matriz de cadeia de caracteres.

> [!NOTE]
> O `Linq.Enumerable.Contains(Of String)(...)` sintaxe não serão compilados se você estiver usando o ASP.NET versão 2.0. O `Contains(Of String)` método é parte do [biblioteca LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), que é novo no ASP.NET 3.5. Se você ainda estiver usando o ASP.NET versão 2.0, use o [ `Array.IndexOf(Of String)` método](https://msdn.microsoft.com/en-us/library/eha9t187.aspx) em vez disso.


O `CheckRolesForSelectedUser` método precisa ser chamado em dois casos: quando a página for carregada pela primeira vez e sempre que o `UserList` índice selecionado do DropDownList é alterado. Portanto, chamar esse método a partir de `Page_Load` manipulador de eventos (após as chamadas para `BindUsersToUserList` e `BindRolesToList`). Além disso, crie um manipulador de eventos para o DropDownList `SelectedIndexChanged` eventos e chamar esse método de lá.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Com esse código, você pode testar a página por meio do navegador. No entanto, como o `UsersAndRoles.aspx` página atualmente não tem a capacidade de atribuir usuários a funções, nenhum usuário tem funções. Vamos criar a interface para atribuir usuários a funções mais tarde, você pode levar o word que esse código funciona e verificar que ele faz isso mais tarde, ou você pode adicionar manualmente os usuários a funções, inserindo registros no `aspnet_UsersInRoles` tabela para testar este functi onality agora.

### <a name="assigning-and-removing-users-from-roles"></a>Atribuir e remover usuários das funções

Quando o visitante ou desmarca uma caixa de seleção no `UsersRoleList` repetidor precisamos adicionar ou remover o usuário selecionado da função correspondente. A caixa de seleção `AutoPostBack` propriedade atualmente está definida como True, o que causa um postback sempre que uma caixa de seleção no repetidor estiver marcada ou desmarcada. Em resumo, precisamos criar um manipulador de eventos para a caixa de seleção `CheckChanged` eventos. Como a caixa de seleção em um controle repetidor, precisamos adicionar manualmente a estrutura do manipulador de eventos. Comece adicionando o manipulador de eventos para a classe code-behind, como um `Protected` método, da seguinte forma:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Podemos retornará para escrever o código para este manipulador de eventos em breve. Mas primeiro vamos concluir o direcionamento de manipulação de eventos. Na caixa de seleção dentro do repetidor `ItemTemplate`, adicionar `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Essa sintaxe conecta o `RoleCheckBox_CheckChanged` manipulador de eventos para o `RoleCheckBox`do `CheckedChanged` evento.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

É nossa tarefa final concluir o `RoleCheckBox_CheckChanged` manipulador de eventos. É preciso iniciar referenciando o controle de caixa de seleção que gerou o evento porque esta instância de caixa de seleção indica qual função foi marcada ou desmarcada por meio de seu `Text` e `Checked` propriedades. Usando essas informações junto com o nome de usuário do usuário selecionado, podemos adicionar ou remover o usuário da função por meio de `Roles` da classe [ `AddUserToRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.addusertorole.aspx) ou [ `RemoveUserFromRole` método](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

O código acima inicia referenciando programaticamente a caixa de seleção que gerou o evento, que está disponível por meio de `sender` parâmetro de entrada. Se a caixa de seleção estiver marcada, o usuário selecionado é adicionado à função especificada, caso contrário, que eles serão removidos da função. Em ambos os casos, o `ActionStatus` exibirá uma mensagem de resumo a ação que acabou de ser executada.

Dedicar um tempo para testar esta página por meio de um navegador. Selecione o usuário Tito e adicione Tito às funções de administradores e supervisores.


[![Tito foi adicionado para as funções de supervisores e administradores](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Figura 3**: Tito foi adicionado para os administradores e funções de supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image9.png))


Em seguida, selecione o usuário Bruce na lista suspensa. Há um postback e caixas de seleção do repetidor são atualizadas por meio de `CheckRolesForSelectedUser`. Como Bruce ainda não pertence a nenhuma função, as duas caixas de seleção estão desmarcadas. Em seguida, adicione Bruce à função de supervisores.


[![Bruce foi adicionado à função de supervisores](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Figura 4**: Bruce foi adicionado à função de supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image12.png))


Para verificar a funcionalidade do `CheckRolesForSelectedUser` método, selecione um usuário diferente Tito ou Bruce. Observe como as caixas de seleção estão desmarcadas, automaticamente, indicando que não pertence a nenhuma função. Retorne ao Tito. Os administradores e supervisores de caixas de seleção devem ser verificadas.

## <a name="step-2-building-the-by-roles-user-interface"></a>Etapa 2: Criar a Interface do usuário "Pelas funções"

Nesse ponto, podemos concluiu a interface "por"usuários e está prontos para começar a lidar com a interface "pelas funções". A interface "pelas funções" solicita que o usuário selecionar uma função de uma lista suspensa e, em seguida, exibe o conjunto de usuários que pertencem a essa função em um controle GridView.

Adicione outro controle DropDownList para o `UsersAndRoles.aspx page`. Coloque esse um sob o controle repetidor, nomeie-o `RoleList`e defina seu `AutoPostBack` propriedade como True. Abaixo dele, adicione um controle GridView e denomine- `RolesUserList`. Este GridView listará os usuários que pertencem à função selecionada. Definir o GridView `AutoGenerateColumns` a propriedade como False, adicionar um TemplateField à grade `Columns` coleção e defina seu `HeaderText` propriedade como "Usuários". Definir o TemplateField `ItemTemplate` para que ele exibe o valor da expressão de associação de dados `Container.DataItem` no `Text` propriedade de um rótulo denominado `UserNameLabel`.

Depois de adicionar e configurar o GridView, marcação declarativa seu "pela função" da interface deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

É necessário preencher o `RoleList` DropDownList com o conjunto de funções do sistema. Para fazer isso, atualize o `BindRolesToList` método para que seja associado a matriz de cadeia de caracteres retornada pelo `Roles.GetAllRoles` método para o `RolesList` DropDownList (bem como a `UsersRoleList` repetidor).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

As duas últimas linhas no `BindRolesToList` método foram adicionados ao associar o conjunto de funções para o `RoleList` controle DropDownList. A Figura 5 mostra o resultado final quando visualizada através de um navegador – uma lista suspensa preenchida com funções do sistema.


[![As funções são exibidas na RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Figura 5**: as funções são exibidas no `RoleList` DropDownList ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Exibindo os usuários que pertencem à função selecionada

Quando a página for carregada pela primeira vez ou quando uma nova função é selecionada do `RoleList` DropDownList, é necessário exibir a lista de usuários que pertencem a essa função em GridView. Crie um método chamado `DisplayUsersBelongingToRole` usando o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Esse método inicia fazendo com que a função selecionada do `RoleList` DropDownList. Ele usa o [ `Roles.GetUsersInRole(roleName)` método](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getusersinrole.aspx) para recuperar uma matriz de cadeia de caracteres de nomes de usuário dos usuários que pertencem a essa função. Esta matriz é então associada para o `RolesUserList` GridView.

Esse método precisa ser chamado em duas circunstâncias: quando a página for carregada inicialmente e quando a função selecionada no `RoleList` DropDownList alterações. Portanto, atualizar o `Page_Load` manipulador de eventos para que esse método é chamado após a chamada a `CheckRolesForSelectedUser`. Em seguida, crie um manipulador de eventos para o `RoleList`do `SelectedIndexChanged` evento e chame esse método a partir daí, muito.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Com esse código no local, o `RolesUserList` GridView deve exibir os usuários que pertencem à função selecionada. Como mostra a Figura 6, a função de supervisores consiste em dois membros: Bruce e Tito.


[![GridView lista os usuários que pertencem à função selecionada](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Figura 6**: O GridView lista os usuários que pertencem à função selecionada ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Remover usuários da função selecionada

Vamos ampliar o `RolesUserList` GridView para que ele inclui uma coluna de "remover" botões. Clicar no botão "Remover" para um usuário específico removerá dessa função.

Comece adicionando um campo de botão de exclusão para o GridView. Tornar este campo aparecem como arquivada mais à esquerda e alterar seu `DeleteText` propriedade de "Excluir" (o padrão) para "Remover".


[![Adicionar o](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Figura 7**: adicionar o botão "Remover" para o GridView ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image21.png))


Ao clicar no botão "Remover" um postback tem lugar e a GridView `RowDeleting` é gerado. É preciso criar um manipulador de eventos para esse evento e escrever código que remove o usuário da função selecionada. Crie o manipulador de eventos e, em seguida, adicione o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

O código começa determinando o nome da função selecionada. Em seguida, por meio de programação referências a `UserNameLabel` controle da linha cuja "Remover" foi clicada para determinar o nome do usuário a ser removido. O usuário é então removido da função por meio de uma chamada para o `Roles.RemoveUserFromRole` método. O `RolesUserList` GridView é atualizada e será exibida uma mensagem por meio de `ActionStatus` controle de rótulo.

> [!NOTE]
> O botão "Remover" não requer qualquer tipo de confirmação do usuário antes de remover o usuário da função. Eu convido você a adicionar algum nível de confirmação do usuário. Uma das maneiras mais fáceis para confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [' adicionando confirmação do lado do cliente quando excluindo](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Figura 8 mostra a página depois que o usuário Tito foi removido do grupo de supervisores.


[![Infelizmente, Tito não é mais um Supervisor](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Figura 8**: Infelizmente, Tito não é mais um Supervisor ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Adicionar novos usuários à função selecionada

Junto com a remoção de usuários da função selecionada, o visitante a essa página também deve ser capaz de adicionar um usuário à função selecionada. A melhor interface para adicionar um usuário à função selecionada depende do número de contas de usuário que você espera ter. Se seu site conterá apenas algumas contas de usuário dúzias ou menos, você pode usar DropDownList aqui. Se pode haver milhares de contas de usuário, convém inclui uma interface de usuário que permite que o visitante a página por meio de contas, procure uma conta específica, ou filtrar as contas de usuário de alguma outra forma.

Para essa página Vamos usar uma interface muito simples que funciona independentemente do número de contas de usuário no sistema. Ou seja, vamos usar uma caixa de texto, solicitando que o visitante para digitar o nome do usuário que deseja adicionar à função selecionada. Se não existe um usuário com esse nome, ou se o usuário já é um membro da função, exibirá uma mensagem em `ActionStatus` rótulo. Mas, se o usuário existe e não é um membro da função, vamos adicioná-los para a função e atualizar a grade.

Adicione uma caixa de texto e um botão abaixo GridView. Defina a caixa de texto `ID` para `UserNameToAddToRole` e definir o botão `ID` e `Text` propriedades `AddUserToRoleButton` e "Adicionar a função de usuário", respectivamente.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Em seguida, crie um `Click` manipulador de eventos para o `AddUserToRoleButton` e adicione o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

A maior parte do código de `Click` manipulador de eventos executa várias verificações de validação. Garante que o visitante fornecido um nome de usuário no `UserNameToAddToRole` caixa de texto, que o usuário existe no sistema e que eles já não pertencem à função selecionada. Se qualquer uma dessas verificações falhar, uma mensagem apropriada será exibida no `ActionStatus` e o manipulador de eventos é encerrado. Se todas as verificações passarem, o usuário é adicionado à função por meio de `Roles.AddUserToRole` método. Depois disso, a caixa de texto `Text` propriedade é limpo, GridView é atualizada e o `ActionStatus` rótulo exibe uma mensagem indicando que o usuário especificado foi adicionado com êxito à função selecionada.

> [!NOTE]
> Para garantir que o usuário especificado já não pertence à função selecionada, usamos o [ `Roles.IsUserInRole(userName, roleName)` método](https://msdn.microsoft.com/en-us/library/system.web.security.roles.isuserinrole.aspx), que retorna um valor booliano que indica se *userName* é um membro de *roleName*. Usaremos esse método novamente no <a id="_msoanchor_2"> </a> [tutorial próxima](role-based-authorization-vb.md) quando vamos examinar a autorização baseada em função.


Visite a página por meio de um navegador e selecione a função de supervisores do `RoleList` DropDownList. Tente digitar um nome de usuário inválido – você deve ver uma mensagem explicando que o usuário não existe no sistema.


[![Você não pode adicionar um usuário inexistente para uma função](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Figura 9**: você não pode adicionar um usuário inexistente para uma função ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image27.png))


Agora, tente adicionar um usuário válido. Vá em frente e adicione novamente Tito à função de supervisores.


[![Tito é novamente um Supervisor!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Figura 10**: Tito é novamente um Supervisor!  ([Clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Etapa 3: Cross-atualizando o "Pelo usuário" e "função" Interfaces

O `UsersAndRoles.aspx` página oferece duas interfaces diferentes para gerenciar usuários e funções. Atualmente, essas duas interfaces agir independentemente das outras portanto, é possível que uma alteração feita em uma interface não aparecerá imediatamente no outro. Por exemplo, imagine que o visitante para a página seleciona a função de supervisores do `RoleList` DropDownList, que lista Bruce e Tito como seus membros. Em seguida, o visitante seleciona Tito do `UserList` DropDownList, que verifica as caixas de seleção de administradores e supervisores do `UsersRoleList` Repetidor. Se o visitante desmarca, em seguida, a função de Supervisor do repetidor, Tito é removido da função supervisores, mas essa modificação não será refletida na interface "pela função". GridView ainda mostrará Tito como sendo um membro da função de supervisores.

Para corrigir isso, precisamos atualizar o GridView sempre que uma função é marcada ou desmarcada do `UsersRoleList` Repetidor. Da mesma forma, é preciso atualizar repetidor sempre que um usuário for removido ou adicionado a uma função da interface "pela função".

Repetidor na interface "pelo usuário" é atualizada por chamar o `CheckRolesForSelectedUser` método. A interface "pela função" pode ser modificada no `RolesUserList` do GridView `RowDeleting` manipulador de eventos e o `AddUserToRoleButton` do botão `Click` manipulador de eventos. Portanto, precisamos chamar o `CheckRolesForSelectedUser` método de cada um desses métodos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Da mesma forma, o GridView na interface "pela função" é atualizado chamando o `DisplayUsersBelongingToRole` método e a interface "pelo usuário" é modificada por meio de `RoleCheckBox_CheckChanged` manipulador de eventos. Portanto, precisamos chamar o `DisplayUsersBelongingToRole` método este manipulador de eventos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Com essas pequenas alterações de código, o "pelo usuário" e "função" interfaces agora atualização cruzada corretamente. Para verificar isso, visite a página por meio de um navegador e selecione Tito e supervisores do `UserList` e `RoleList` DropDownLists, respectivamente. Observe que que você desmarque a função supervisores para Tito do repetidor na interface "pelo usuário", Tito será removido automaticamente do GridView na interface "pela função". Adicionar Tito volta para a função de supervisores da interface "pela função" automaticamente novamente verifica a caixa de seleção de supervisores na interface "pelo usuário".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Etapa 4: Personalizar o CreateUserWizard para incluir uma etapa "Especificar funções"

No <a id="_msoanchor_3"> </a> [ *criando contas de usuário* ](../membership/creating-user-accounts-vb.md) tutorial vimos como usar o controle CreateUserWizard Web para fornecer uma interface para criar uma nova conta de usuário. O controle CreateUserWizard pode ser usado em uma das duas maneiras:

- Como um meio de visitantes criar sua própria conta de usuário no site, e
- Como um meio para administradores criar novas contas

No primeiro caso de uso, um visitante fornecido para o site e preenche CreateUserWizard, inserir suas informações para registrar no site. No segundo caso, um administrador cria uma nova conta para outra pessoa.

Quando uma conta está sendo criada por um administrador para alguma outra pessoa, pode ser útil permitir que o administrador especifique as funções que a nova conta de usuário pertence. No <a id="_msoanchor_4"> </a> [ *armazenamento* *informações adicionais do usuário* ](../membership/storing-additional-user-information-vb.md) tutorial vimos como personalizar CreateUserWizard adicionando adicionais `WizardSteps`. Vamos ver como adicionar uma etapa adicional para CreateUserWizard para especificar as novas funções do usuário.

Abra o `CreateUserWizardWithRoles.aspx` página e adicione um controle CreateUserWizard chamado `RegisterUserWithRoles`. Definir o controle `ContinueDestinationPageUrl` propriedade como "~ / default. aspx". Como a ideia aqui é que um administrador usará esse controle CreateUserWizard para criar novas contas de usuário, defina o controle `LoginCreatedUser` a propriedade como False. Isso `LoginCreatedUser` propriedade especifica se o visitante é automaticamente conectado como usuário recém-criada e o padrão é True. Vamos configurá-lo como False quando um administrador cria uma nova conta queremos mantê-lo conectado como ele mesmo.

Em seguida, selecione o "Adicionar ou remover `WizardSteps`..." opção de marca inteligente do CreateUserWizard e adicionar um novo `WizardStep`, configuração seu `ID` para `SpecifyRolesStep`. Mover o `SpecifyRolesStep WizardStep` para que ele vem após a etapa de "Logon se sua nova conta", mas antes da etapa "Concluído". Definir o `WizardStep`do `Title` propriedade como "Especificar funções", seu `StepType` propriedade `Step`e seu `AllowReturn` propriedade como False.


[![Adicionar o](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Figura 11**: adicionar "Especificar funções" `WizardStep` para CreateUserWizard ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image33.png))


Depois dessa alteração declarativo do CreateUserWizard deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

Em "Funções especificar" `WizardStep`, adicionar um CheckBoxList denominado `RoleList.` CheckBoxList essa lista as funções disponíveis, permitindo que a pessoa visitar a página verificar se as funções que o usuário recém-criado pertence.

Continuamos com duas tarefas de codificação: primeiro é necessário preencher o `RoleList` CheckBoxList com as funções do sistema; em seguida, precisamos adicionar o usuário criado para as funções selecionadas quando o usuário move da etapa "Especificar funções" para a etapa "Completo". Podemos pode fazer a primeira tarefa no `Page_Load` manipulador de eventos. O código a seguir programaticamente as referências a `RoleList` caixa de seleção na primeira visita à página e uma associação as funções no sistema.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

O código acima deve parecer familiar. No <a id="_msoanchor_5"> </a> [ *armazenamento* *informações adicionais do usuário* ](../membership/storing-additional-user-information-vb.md) tutorial, usamos dois `FindControl` instruções para fazer referência a um controle da Web de dentro de um personalizado `WizardStep`. E o código que associa as funções para o CheckBoxList foi obtido anteriormente neste tutorial.

Para executar a segunda tarefa de programação, precisamos saber quando a etapa "Especificar funções" foi concluída. Lembre-se que o CreateUserWizard tem um `ActiveStepChanged` evento, que é acionado sempre que o visitante navega a partir de uma etapa para outra. Aqui é possível determinar se o usuário atingiu a etapa "Completo". Nesse caso, precisamos adicionar o usuário a funções selecionadas.

Criar um manipulador de eventos para o `ActiveStepChanged` evento e adicione o seguinte código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Se o usuário atingiu apenas a etapa de "Concluído", o manipulador de eventos enumera os itens do `RoleList` CheckBoxList e o usuário recém-criada é atribuído às funções selecionadas.

Visite essa página através de um navegador. A primeira etapa na CreateUserWizard é a etapa de "Logon se sua nova conta" padrão, que solicitará o novo nome do usuário, senha, email e outras informações importantes. Insira as informações para criar um novo usuário chamado Wanda.


[![Criar um novo usuário chamado Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Figura 12**: criar um novo usuário chamado Wanda ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image36.png))


Clique no botão "Criar usuário". CreateUserWizard chama internamente o `Membership.CreateUser` método, criar a nova conta de usuário e, em seguida, passa para a próxima etapa, "Especifica funções." Aqui, as funções do sistema são listadas. Marque a caixa de seleção de supervisores e clique em Avançar.


[![Tornar um membro da função de supervisores de Wanda](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Figura 13**: tornar um membro da função de supervisores de Wanda ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image39.png))


Clicar em Avançar causa um postback e atualizações de `ActiveStep` para a etapa "Completo". No `ActiveStepChanged` manipulador de eventos, a conta de usuário criado recentemente é atribuída à função de supervisores. Para verificar isso, retornar o `UsersAndRoles.aspx` página e selecione supervisores do `RoleList` DropDownList. Como mostra a Figura 14, supervisores agora são compostos de três usuários: Bruce Tito e Wanda.


[![Bruce, Tito e Wanda são todos os supervisores](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Figura 14**: Bruce, Tito e Wanda são todos os supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Resumo

A estrutura de funções oferece métodos para recuperar informações sobre funções e métodos para determinar o que os usuários pertencem a uma função especificada de um usuário específico. Além disso, há vários métodos para adicionar e remover um ou mais usuários para uma ou mais funções. Este tutorial nos concentramos em apenas dois desses métodos: `AddUserToRole` e `RemoveUserFromRole`. Há variantes projetadas para adicionar vários usuários a uma única função e atribuir várias funções a um único usuário.

Este tutorial também incluídos examinar estendendo o controle CreateUserWizard para incluir um `WizardStep` para especificar as funções do usuário recém-criado. Essa é uma etapa pode ajudar um administrador simplificar o processo de criação de contas de usuário para novos usuários.

Neste ponto, vimos como criar e excluir funções e como adicionar e remover usuários das funções. Mas, ainda precisamos examinar aplicar a autorização baseada em função. No <a id="_msoanchor_6"> </a> [seguinte tutorial](role-based-authorization-vb.md) examinaremos definindo regras de autorização de URL em uma base de função por função, bem como limitar a funcionalidade de nível de página com base nas funções do usuário conectado no momento.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Visão geral de ferramenta de administração de Site da Web ASP.NET](https://msdn.microsoft.com/en-us/library/ms228053.aspx)
- [Examinando ASP. Associação, funções e perfil do NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Implantar sua própria ferramenta de administração de site](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](creating-and-managing-roles-vb.md)
[Próximo](role-based-authorization-vb.md)
