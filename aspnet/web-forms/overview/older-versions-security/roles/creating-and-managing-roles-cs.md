---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Criar e gerenciar funções (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, vamos criar páginas da web para criar e excluir funções.
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: 84b9624aaae0cc98f1908b4521cee43bce6e856d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806866"
---
<a name="creating-and-managing-roles-c"></a>Criar e gerenciar funções (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, vamos criar páginas da web para criar e excluir funções.


## <a name="introduction"></a>Introdução

No <a id="_msoanchor_1"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial, examinamos usando a autorização de URL para impedir que determinados usuários de um conjunto de páginas e explorou declarativa e técnicas de programação para ajustar a funcionalidade de uma página ASP.NET com base no usuário visitante. Conceder permissão de acesso à página ou a funcionalidade em uma base por usuário, no entanto, pode se tornar um pesadelo em cenários em que há muitas contas de usuário ou quando os privilégios dos usuários são alterados com frequência. Sempre que um usuário ganha ou perde a autorização para executar uma tarefa específica, o administrador precisa atualizar as regras de autorização de URL apropriadas, marcação declarativa e código.

Normalmente, é útil para classificar usuários em grupos ou *funções* e, em seguida, para aplicar permissões em uma base de função por função. Por exemplo, a maioria dos aplicativos web têm um determinado conjunto de páginas ou tarefas que são reservadas apenas para usuários administrativos. Usando as técnicas aprendidas na *autorização baseada em usuário* tutorial, estamos adicionaria as regras de autorização de URL apropriadas, marcação declarativa e código para permitir que as contas de usuário especificado executar tarefas administrativas. Mas, se um novo administrador foi adicionado ou se um administrador existente precisava ter seus direitos de administração revogados, teríamos que volte e atualize os arquivos de configuração e as páginas da web. Com as funções, no entanto, podemos pode criar uma função chamada de administradores e atribuir esses usuários confiáveis para a função de administradores. Em seguida, podemos adicionaria as regras de autorização de URL apropriadas, marcação declarativa e código para permitir que a função de administradores realizar as diversas tarefas administrativas. Com essa infra-estrutura in-loco, adicionando novos administradores para o site ou removendo existentes é tão simple quanto incluindo ou remover o usuário da função de administradores. Nenhuma configuração, marcação declarativa ou alterações de código são necessárias.

O ASP.NET oferece uma estrutura de funções para definir funções e associá-los a contas de usuário. Com a estrutura de funções, podemos criar e excluir funções, adicione usuários ou remover usuários de uma função, determinar o conjunto de usuários que pertencem a uma função específica e informar se um usuário pertence a uma função específica. Depois que a estrutura de funções tiver sido configurada, podemos pode limitar o acesso a páginas em uma base de função por função por meio de regras de autorização de URL e mostrar ou ocultar informações adicionais ou funcionalidade em uma página com base nas funções do usuário conectado no momento.

Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, vamos criar páginas da web para criar e excluir funções. No <a id="_msoanchor_2"> </a> [ *atribuir funções a usuários* ](assigning-roles-to-users-cs.md) tutorial, veremos como adicionar e remover usuários das funções. E, na <a id="_msoanchor_3"> </a> [ *autorização baseada em função* ](role-based-authorization-cs.md) tutorial, veremos como limitar o acesso a páginas em uma base de função por função, juntamente com como ajustar dependendo da funcionalidade de página na função de usuário visitante. Vamos começar!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: Adicionar novas páginas do ASP.NET

Este tutorial e as próximas duas podemos será examinando várias funções relacionadas a funções e recursos. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados durante esses tutoriais. Vamos criar essas páginas e atualizar o mapa de site.

Comece criando uma nova pasta no projeto chamado `Roles`. Em seguida, adicione quatro novas páginas do ASP.NET para o `Roles` pasta, vinculando cada página com o `Site.master` página mestra. Nomeie as páginas:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Neste ponto, Gerenciador de soluções do seu projeto deve ser semelhante à mostrada na Figura 1 de captura de tela.


[![Quatro novas páginas foram adicionadas à pasta de funções](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figura 1**: quatro novas páginas foram adicionados para o `Roles` pasta ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image3.png))


Cada página, neste ponto, terá dois controles de conteúdo, uma para cada um dos ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Lembre-se de que o `LoginContent` marcação de padrão do ContentPlaceHolder exibe um link para fazer logon ou logoff do site, dependendo se o usuário é autenticado. A presença do `Content2` conteúdo de controle na página ASP.NET, no entanto, substituirá a marcação de padrão da página mestra. Como discutimos no <a id="_msoanchor_4"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, substituindo a marcação padrão é útil para páginas em que não queremos exibir relacionadas a logon Opções da coluna à esquerda.

Para essas quatro páginas, no entanto, queremos mostrar a marcação de padrão da página mestra para o `LoginContent` ContentPlaceHolder. Portanto, remova a marcação declarativa para a `Content2` controle de conteúdo. Depois de fazer isso, cada uma a quatro marcação da página deve conter apenas um controle de conteúdo.

Por fim, vamos atualizar o mapa do site (`Web.sitemap`) para incluir essas novas páginas da web. Adicione o seguinte XML após o `<siteMapNode>` adicionamos para os tutoriais de associação.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Com o mapa de site atualizado, visite o site por meio de um navegador. Como mostra a Figura 2, a navegação à esquerda agora inclui itens para os tutoriais de funções.


[![Quatro novas páginas foram adicionadas à pasta de funções](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figura 2**: quatro novas páginas foram adicionados para o `Roles` pasta ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Etapa 2: Especificar e configurar o provedor de funções do Framework

Como o framework de associação, a estrutura de funções é construída sobre o modelo de provedor. Conforme discutido na <a id="_msoanchor_5"> </a> [ *Noções básicas sobre segurança e suporte do ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) tutorial, o .NET Framework vem com três provedores de funções internos: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), e [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Esta série de tutoriais enfoca o `SqlRoleProvider`, que usa um banco de dados do Microsoft SQL Server como o repositório da função.

Nos bastidores a estrutura de funções e `SqlRoleProvider` funcionam exatamente como a estrutura de associação e `SqlMembershipProvider`. O .NET Framework contém um `Roles` classe que serve como a API para a estrutura de funções. O `Roles` classe possui métodos estáticos, como `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`e assim por diante. Quando um desses métodos é invocado, o `Roles` classe delega a chamada para o provedor configurado. O `SqlRoleProvider` funciona com as tabelas específicas de função (`aspnet_Roles` e `aspnet_UsersInRoles`) em resposta.

Para usar o `SqlRoleProvider` provedor em nosso aplicativo, é necessário especificar qual banco de dados a ser usado como o repositório. O `SqlRoleProvider` espera que o repositório de função especificada para ter determinadas tabelas de banco de dados, exibições e procedimentos armazenados. Esses objetos de banco de dados necessárias podem ser adicionados usando o [ `aspnet_regsql.exe` ferramenta](https://msdn.microsoft.com/library/ms229862.aspx). Neste ponto já temos um banco de dados com o esquema necessário para o `SqlRoleProvider`. Volta a <a id="_msoanchor_6"> </a> [ *criando o esquema de associação no SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) tutorial, criamos um banco de dados denominado `SecurityTutorials.mdf` e usado `aspnet_regsql.exe` para adicionar o aplicativo serviços, que incluía os objetos de banco de dados necessários para o `SqlRoleProvider`. Portanto, só precisamos dizer à estrutura de funções para habilitar o suporte da função e usar o `SqlRoleProvider` com o `SecurityTutorials.mdf` banco de dados como o repositório da função.

A estrutura de funções é configurada por meio de &lt; `roleManager` &gt; elemento na caixa de diálogo `Web.config` arquivo. Por padrão, o suporte à função está desabilitado. Para habilitá-lo, você deve definir a [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) do elemento `enabled` atributo `true` da seguinte forma:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Por padrão, todos os aplicativos web têm um provedor de funções denominado `AspNetSqlRoleProvider` do tipo `SqlRoleProvider`. Esse provedor padrão é registrado no `machine.config` (localizado em `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

O provedor `connectionStringName` atributo especifica o repositório de função que é usado. O `AspNetSqlRoleProvider` provedor define esse atributo para `LocalSqlServer`, que também é definida no `machine.config` pontos, por padrão, para um banco de dados do SQL Server 2005 Express Edition e o `App_Data` pasta chamada `aspnet.mdf`.

Consequentemente, se podemos simplesmente ativar a estrutura de funções sem especificar quaisquer informações de provedor em nosso aplicativo `Web.config` arquivo, o aplicativo usa o provedor de funções padrão registrado, `AspNetSqlRoleProvider`. Se o `~/App_Data/aspnet.mdf` banco de dados não existe, o tempo de execução do ASP.NET automaticamente criá-lo e adicionar o esquema de serviços de aplicativo. No entanto, não queremos usar o `aspnet.mdf` banco de dados; em vez disso, queremos usar o `SecurityTutorials.mdf` banco de dados que já temos criado e adicionado o esquema de serviços de aplicativo para. Essa modificação pode ser feita de duas maneiras:

- <strong>Especifique um valor para o</strong><strong>`LocalSqlServer`</strong><strong>nome de cadeia de caracteres de conexão no</strong><strong>`Web.config`</strong><strong>.</strong> Substituindo o `LocalSqlServer` o valor do nome da cadeia de conexão no `Web.config`, podemos usar o provedor de funções padrão registrado (`AspNetSqlRoleProvider`) e ainda funcionar corretamente com o `SecurityTutorials.mdf` banco de dados. Para obter mais informações sobre essa técnica, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da postagem de blog [Configurando serviços de aplicativos do ASP.NET 2.0 para usar o SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Adicionar um novo provedor registrado do tipo</strong><strong>`SqlRoleProvider`</strong><strong>e configurar seu</strong><strong>`connectionStringName`</strong><strong>configuração para apontar para o</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>banco de dados.</strong> Essa é a abordagem que eu recomendado e usado na <a id="_msoanchor_7"> </a> [ *criando o esquema de associação no SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) tutorial e é a abordagem que eu usarei neste tutorial também.

Adicione a seguinte marcação de configuração de funções para o `Web.config` arquivo. Essa marcação registra um novo provedor chamado `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

A marcação acima define a `SecurityTutorialsSqlRoleProvider` como o provedor padrão (via o `defaultProvider` atributo no `<roleManager>` elemento). Ele também define o `SecurityTutorialsSqlRoleProvider`do `applicationName` definir como `SecurityTutorials`, que é o mesmo `applicationName` configuração usada pelo provedor de associação (`SecurityTutorialsSqlMembershipProvider`). Embora não mostrado aqui, o [ `<add>` elemento](https://msdn.microsoft.com/library/ms164662.aspx) para o `SqlRoleProvider` também pode conter um `commandTimeout` atributo para especificar a duração de tempo limite de banco de dados, em segundos. O valor padrão é 30.

Com essa marcação de configuração em vigor, estamos prontos para começar a usar a funcionalidade de função dentro do nosso aplicativo.

> [!NOTE]
> A marcação de configuração acima ilustra o uso de &lt; `roleManager` &gt; do elemento `enabled` e `defaultProvider` atributos. Há uma série de outros atributos que afetam como o framework de funções associa informações de função em uma base por usuário. Vamos examinar essas configurações na <a id="_msoanchor_8"> </a> [ *autorização baseada em função* ](role-based-authorization-cs.md) tutorial.


## <a name="step-3-examining-the-roles-api"></a>Etapa 3: Examinar as funções de API

Funcionalidade da estrutura de funções é exposta por meio de [ `Roles` classe](https://msdn.microsoft.com/library/system.web.security.roles.aspx), que contém métodos estáticos treze para executar operações com base em função. Quando examinamos criação e exclusão de funções em etapas 4 e 6 nós usaremos o [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) e [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) métodos, que adicionar ou remover uma função do sistema.

Para obter uma lista de todas as funções do sistema, use o [ `GetAllRoles` método](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (consulte a etapa 5). O [ `RoleExists` método](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) retorna um valor booliano que indica se uma função especificada existe.

O próximo tutorial, vamos examinar como associar usuários a funções. O `Roles` da classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), e [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) métodos de adicionam um ou mais usuários a uma ou mais funções. Para remover usuários das funções, use o [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), ou [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) métodos.

No <a id="_msoanchor_9"> </a> [ *autorização baseada em função* ](role-based-authorization-cs.md) tutorial, veremos maneiras para mostrar ou ocultar com base na função do usuário conectado no momento a funcionalidade de programaticamente. Para fazer isso, podemos usar o `Role` da classe [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), ou [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) métodos.

> [!NOTE]
> Tenha em mente que sempre que um desses métodos é invocado, o `Roles` classe delega a chamada para o provedor configurado. Em nosso caso, isso significa que a chamada está sendo enviada para o `SqlRoleProvider`. O `SqlRoleProvider` , em seguida, executa a operação de banco de dados apropriado com base no método invocado. Por exemplo, o código `Roles.CreateRole("Administrators")` resulta nos `SqlRoleProvider` executando o `aspnet_Roles_CreateRole` procedimento armazenado, que insere um novo registro no `aspnet_Roles` tabela chamada Administrators.


O restante deste tutorial examina usando o `Roles` da classe `CreateRole`, `GetAllRoles`, e `DeleteRole` métodos para gerenciar as funções do sistema.

## <a name="step-4-creating-new-roles"></a>Etapa 4: Criando novas funções

Funções oferecem uma maneira arbitrariamente os usuários do grupo e, mais comumente esse agrupamento é usado para uma forma mais conveniente aplicar as regras de autorização. Mas, para usar funções como um mecanismo de autorização, primeiro precisamos definir as funções que existem no aplicativo. Infelizmente, o ASP.NET não inclui um controle CreateRoleWizard. Para adicionar novas funções, é necessário criar uma interface do usuário adequada e chamar a API de funções sozinhos. A boa notícia é que isso é muito fácil de realizar.

> [!NOTE]
> Embora não haja nenhum controle CreateRoleWizard Web, é o [ferramenta de administração de Site da Web do ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), que é um aplicativo ASP.NET local, projetado para ajudar a exibir e gerenciar a configuração do seu aplicativo web. No entanto, eu não sou muito fã da ferramenta de administração de Site da Web do ASP.NET por dois motivos. Primeiro, ele é um pouco com bugs e a experiência do usuário deixa muito a desejar. Em segundo lugar, a ferramenta de administração de Site da Web do ASP.NET foi projetada para apenas trabalhar localmente, o que significa que você precisará criar sua própria função páginas da web de gerenciamento se você precisar gerenciar funções em um site ao vivo remotamente. Para esses dois motivos, este tutorial e o próximo se concentrará em criar a função necessária ferramentas de gerenciamento em uma página da web em vez de contar com a ferramenta de administração de Site da Web do ASP.NET.


Abra o `ManageRoles.aspx` página o `Roles` pasta e adicione uma caixa de texto e um controle da Web de botão para a página. Defina o controle de caixa de texto `ID` propriedade para `RoleName` e o botão `ID` e `Text` propriedades a serem `CreateRoleButton` e criar uma função, respectivamente. Neste ponto, marcação declarativa de sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Em seguida, clique duas vezes o `CreateRoleButton` botão controle no Designer para criar um `Click` manipulador de eventos e, em seguida, adicione o seguinte código:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

O código acima inicia atribuindo o nome da função cortado inserido na `RoleName` caixa de texto para o `newRoleName` variável. Em seguida, o `Roles` da classe `RoleExists` método é chamado para determinar se a função `newRoleName` já existe no sistema. Se a função não existir, ele será criado por meio de uma chamada para o `CreateRole` método. Se o `CreateRole` método recebe um nome de função que já existe no sistema, um `ProviderException` exceção é lançada. Isso ocorre porque o código primeiro verifica para garantir que a função ainda não existir no sistema antes de chamar `CreateRole`. O `Click` manipulador de eventos termina limpando os `RoleName` da caixa de texto `Text` propriedade.

> [!NOTE]
> Você pode estar se perguntando o que acontecerá se o usuário não inserir qualquer valor para o `RoleName` caixa de texto. Se o valor passado para o `CreateRole` método é `null` ou uma cadeia de caracteres vazia, uma exceção será gerada. Da mesma forma, se o nome da função contém uma vírgula uma exceção é gerada. Consequentemente, a página deve conter controles de validação para garantir que o usuário insere uma função e que não contém qualquer vírgulas. Eu deixo como um exercício para o leitor.


Vamos criar uma função chamada Administrators. Visite o `ManageRoles.aspx` através de um navegador, digite Administradores na caixa de texto (veja a Figura 3) e, em seguida, clique no botão Create Role.


[![Criar uma função de administradores](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figura 3**: criar uma função de administradores ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image9.png))


O que acontece? Ocorre um postback, mas não há nenhuma indicação visual que a função, na verdade, foi adicionado ao sistema. Atualizaremos esta página na etapa 5 para incluir comentários visuais. Por enquanto, no entanto, você pode verificar se a função foi criada, vá para o `SecurityTutorials.mdf` banco de dados e exibir os dados do `aspnet_Roles` tabela. Como mostra a Figura 4, o `aspnet_Roles` tabela contém um registro para as funções de administradores just-adicionado.


[![A tabela aspnet_Roles tem uma linha para os administradores](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figura 4**: O `aspnet_Roles` tabela tem uma linha para os administradores ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Etapa 5: Exibir as funções do sistema

Vamos ampliar o `ManageRoles.aspx` página para incluir uma lista das funções atuais no sistema. Para fazer isso, adicione um controle GridView à página e defina suas `ID` propriedade para `RoleList`. Em seguida, adicionar um método à classe de code-behind da página denominada `DisplayRolesInGrid` usando o seguinte código:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

O `Roles` da classe `GetAllRoles` método retorna todas as funções no sistema como uma matriz de cadeias de caracteres. Essa matriz de cadeia de caracteres é então associado ao GridView. Para associar a lista de funções a GridView quando a página é carregada pela primeira vez, precisamos chamar o `DisplayRolesInGrid` método a partir da página `Page_Load` manipulador de eventos. O código a seguir chama esse método quando a página é visitada pela primeira vez, mas não em postbacks subsequentes.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Com esse código, visite a página por meio de um navegador. Como mostra a Figura 5, você deve ver uma grade com uma única coluna rotulada como o Item. A grade inclui uma linha para a função de administradores que adicionamos na etapa 4.


[![O GridView exibe as funções em uma única coluna](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figura 5**: O GridView exibe as funções em uma única coluna ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image15.png))


O GridView exibe uma coluna solitário Item rotulada, pois o GridView `AutoGenerateColumns` estiver definida como True (padrão), que faz com que o GridView criar automaticamente uma coluna para cada propriedade no seu `DataSource`. Uma matriz tem uma única propriedade que representa os elementos na matriz, portanto, a única coluna em GridView.

Ao exibir dados com um GridView, eu prefiro explicitamente definir minhas colunas em vez de tê-los implicitamente gerado pelo GridView. Definindo explicitamente as colunas é muito mais fácil de formatar os dados, reorganizar as colunas e executar outras tarefas comuns. Portanto, vamos atualizar marcação declarativa do GridView para que suas colunas são definidas explicitamente.

Comece definindo o GridView `AutoGenerateColumns` propriedade como False. Em seguida, adicione um TemplateField à grade, defina suas `HeaderText` propriedade às funções e configurar seu `ItemTemplate` para que ele exibe o conteúdo da matriz. Para fazer isso, adicione um controle de rótulo Web chamado `RoleNameLabel` para o `ItemTemplate` e associar seus `Text` propriedade `Container.DataItem`.

Essas propriedades e o `ItemTemplate`do conteúdo pode ser definido de forma declarativa ou por meio de interface de editar modelos e a caixa de diálogo de campos do GridView. Para acessar os campos de caixa de diálogo, clique no link de editar colunas do GridView de marca inteligente. Em seguida, desmarque a opção Gerar automaticamente campos checkbox para definir a `AutoGenerateColumns` a propriedade como False e adicionar um TemplateField para o controle GridView, definindo seu `HeaderText` propriedade à função. Para definir o `ItemTemplate`do conteúdo, escolha a opção de editar modelos na marca inteligente do GridView. Arraste um controle de Web de rótulo para o `ItemTemplate`, defina seu `ID` propriedade a ser `RoleNameLabel`e definir suas configurações de associação de dados, de modo que seu `Text` propriedade é associada a `Container.DataItem`.

Independentemente de qual abordagem você usa, marcação declarativa do resultante do GridView deve ser semelhante à seguinte quando você terminar.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> O conteúdo da matriz é exibido usando a sintaxe de associação de dados `<%# Container.DataItem %>`. Uma descrição completa de por que essa sintaxe é usada quando exibir o conteúdo de uma matriz associada a GridView está além do escopo deste tutorial. Para obter mais informações sobre essa questão, consulte [associando uma matriz de escalar a um controle de Web de dados](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Atualmente, o `RoleList` GridView está associado apenas à lista de funções quando a página é visitada primeiro. É necessário atualizar a grade sempre que uma nova função é adicionada. Para fazer isso, atualize o `CreateRoleButton` do botão `Click` manipulador de eventos para que chame o `DisplayRolesInGrid` método se uma nova função é criada.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Agora quando o usuário adiciona uma nova função de `RoleList` GridView mostra a função adicionada apenas no postback, fornecer comentários visuais que a função foi criada com êxito. Para ilustrar isso, visite o `ManageRoles.aspx` página por meio de um navegador e adicionar uma função chamada supervisores. Ao clicar no botão Create Role, ocorrerá um postback e a grade será atualizada para incluir os administradores, bem como a nova função, os supervisores.


[![A função de supervisores tem foi adicionado](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figura 6**: A função de supervisores tem foram adicionados ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Etapa 6: Excluindo funções

Neste ponto, um usuário pode criar uma nova função e exibir todas as funções existentes do `ManageRoles.aspx` página. Vamos permitir que os usuários também excluir funções. O `Roles.DeleteRole` método tem duas sobrecargas:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Exclui a função *roleName*. Uma exceção é gerada se a função contém um ou mais membros.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Exclui a função *roleName*. Se *throwOnPopulateRole* é `true`, em seguida, uma exceção é lançada se a função contém um ou mais membros. Se *throwOnPopulateRole* é `false`, em seguida, a função será excluída se ele contém todos os membros ou não. Internamente, o `DeleteRole(roleName)` chamadas de método `DeleteRole(roleName, true)`.

O `DeleteRole` método também gerará uma exceção se *roleName* é `null` ou uma cadeia de caracteres vazia ou se *roleName* contém uma vírgula. Se *roleName* não existe no sistema, `DeleteRole` falha silenciosamente, sem gerar uma exceção.

Vamos ampliar o GridView no `ManageRoles.aspx` para incluir a uma exclusão de botão que, quando clicado, exclui a função selecionada. Comece adicionando um botão Excluir ao GridView indo para a caixa de diálogo de campos e adicionar um botão Excluir, que está localizado sob a opção CommandField. Verifique a exclusão de coluna à extrema esquerda de botão e defina seu `DeleteText` propriedade à função de excluir.


[![Adicionar um botão Excluir ao RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figura 7**: adicionar um botão Excluir para o `RoleList` GridView ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image21.png))


Depois de adicionar o botão de exclusão, marcação declarativa do seu GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Em seguida, crie um manipulador de eventos para o GridView `RowDeleting` eventos. Esse é o evento que é gerado em um postback quando o botão de função excluir é clicado. Adicione o seguinte código ao manipulador de eventos.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

O código começa fazendo referência a programaticamente o `RoleNameLabel` controle na linha cuja função excluir foi clicada da Web. O `Roles.DeleteRole` método, em seguida, é chamado, passando a `Text` da `RoleNameLabel` e `false`e, portanto, excluindo a função independentemente se houver usuários associados à função. Por fim, o `RoleList` GridView é atualizado para que a função just-excluído não aparece mais na grade.

> [!NOTE]
> O botão Excluir função não requer qualquer tipo de confirmação do usuário antes de excluir a função. Uma das maneiras mais fáceis para confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [' adicionando confirmação do lado do cliente quando excluindo](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Resumo

Muitos aplicativos web têm certas regras de autorização ou funcionalidade de nível de página que está disponível apenas para determinadas classes de usuários. Por exemplo, pode haver um conjunto de páginas da web que somente os administradores podem acessar. Em vez de definir essas regras de autorização em uma base por usuário, muitas vezes, é mais útil definir as regras com base em uma função. Ou seja, em vez de explicitamente, permitindo que os usuários Scott e Jisun para acessar as páginas da web administrativas, uma abordagem mais sustentável é permitir que os membros da função Administradores para acessar essas páginas e, em seguida, a fim de denotar Scott e Jisun como usuários que pertencem à Função de administradores.

A estrutura de funções facilita criar e gerenciar funções. Neste tutorial, examinamos como configurar a estrutura de funções para usar o `SqlRoleProvider`, que usa um banco de dados do Microsoft SQL Server como o repositório da função. Além disso, criamos uma página da web que lista as funções existentes no sistema e permite que novas funções a serem criados e as existentes a serem excluídos. Nos tutoriais subsequentes veremos como atribuir usuários às funções e como aplicar a autorização baseada em função.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Examinando o ASP.NET da 2.0 associação, funções e perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Como Usar o Gerenciador de funções no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Provedores de função](https://msdn.microsoft.com/library/aa478950.aspx)
- [Sem interrupção de sua própria ferramenta de administração de site](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentação técnica para o `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)
- [Usando a associação e APIs do Gerenciador de função](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial incluem Alicja Maziarz, Suchi Banerjee e Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](assigning-roles-to-users-cs.md)
