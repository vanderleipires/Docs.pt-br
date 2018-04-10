---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Criar e gerenciar funções (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, vamos criar páginas da web para criar e excluir funções.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: a4ea7e76e023cd436d1d8ac52307a3ac17267fef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="creating-and-managing-roles-c"></a>Criar e gerenciar funções (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) ou [baixar PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, vamos criar páginas da web para criar e excluir funções.


## <a name="introduction"></a>Introdução

No <a id="_msoanchor_1"> </a> [ *autorização baseada em usuário* ](../membership/user-based-authorization-cs.md) tutorial é visto usando a autorização de URL para restringir a determinados usuários de um conjunto de páginas e explorou declarativa e técnicas de programação para ajustar a funcionalidade de uma página ASP.NET com base no usuário visitante. Conceder permissão de acesso de página ou funcionalidade em uma base por usuário, no entanto, pode se tornar um pesadelo em cenários em que há muitas contas de usuário ou quando os privilégios dos usuários são alterados com frequência. Sempre que um usuário recebe ou perde a autorização para executar uma tarefa específica, o administrador precisa atualizar as regras de autorização de URL apropriadas, marcação declarativa e código.

Geral, é útil para classificar os usuários em grupos ou *funções* e, em seguida, aplicar permissões a cada função pela função. Por exemplo, a maioria dos aplicativos web têm um determinado conjunto de páginas ou tarefas que são reservadas apenas para usuários administrativos. Usando as técnicas aprendeu o *autorização baseada em usuário* tutorial, estamos adicionaria a regras de autorização de URL apropriadas, marcação declarativa e código para permitir que as contas de usuário especificado executar tarefas administrativas. Mas se foi adicionado um novo administrador, ou se for necessário um administrador existente com seus direitos de administração revogados, podemos deverá retornar e atualizar os arquivos de configuração e páginas da web. Com funções, no entanto, podemos pode criar uma função chamada de administradores e atribuir esses usuários confiáveis para a função de administradores. Em seguida, vamos adicionaria a regras de autorização de URL apropriadas, marcação declarativa e código para permitir que a função de administradores executar as diversas tarefas administrativas. Com essa infra-estrutura, adicionar novos administradores ao site ou removendo existentes é tão simple quanto incluindo ou remover o usuário da função de administradores. Nenhuma configuração, marcação declarativa ou alterações de código são necessárias.

O ASP.NET oferece uma estrutura de funções para definir funções e associá-los a contas de usuário. Com a estrutura de funções, podemos criar e excluir funções, adicione usuários ou remover usuários de uma função, determinar o conjunto de usuários que pertencem a uma função específica e informar se um usuário pertencer a uma função específica. Uma vez configurado o framework de funções, podemos pode limitar o acesso a páginas em uma base de função pela função por meio de regras de autorização de URL e mostrar ou ocultar informações adicionais ou funcionalidade em uma página com base nas funções do usuário conectado no momento.

Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, vamos criar páginas da web para criar e excluir funções. No <a id="_msoanchor_2"> </a> [ *atribuir funções aos usuários* ](assigning-roles-to-users-cs.md) tutorial, examinaremos como adicionar e remover usuários das funções. E, no <a id="_msoanchor_3"> </a> [ *autorização baseada em função* ](role-based-authorization-cs.md) tutorial veremos como limitar o acesso a páginas em uma base de função pela função junto com a ajustar dependendo da funcionalidade de página na função do usuário visita. Vamos começar!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: Adicionar novas páginas ASP.NET

Este tutorial e as próximas duas nós serão examinando várias funções relacionadas a funções e recursos. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados durante esses tutoriais. Vamos criar essas páginas e atualizar o mapa do site.

Comece criando uma nova pasta no projeto chamado `Roles`. Em seguida, adicione quatro novas páginas ASP.NET para o `Roles` vinculação de pastas, cada página com o `Site.master` página mestra. Nomeie as páginas:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Neste ponto Gerenciador de soluções do projeto deve ser semelhante à mostrada na Figura 1 de captura de tela.


[![Quatro novas páginas foram adicionadas para a pasta de funções](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figura 1**: quatro novas páginas foram adicionados para o `Roles` pasta ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image3.png))


Cada página neste ponto, deve ter os dois controles de conteúdo, uma para cada ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Lembre-se de que o `LoginContent` marcação de padrão do ContentPlaceHolder exibe um link para fazer logon ou logoff do site, dependendo se o usuário é autenticado. A presença de `Content2` conteúdo de controle na página do ASP.NET, no entanto, substitui a marcação de padrão da página mestra. Conforme abordado em <a id="_msoanchor_4"> </a> [ *uma visão geral de formulários de autenticação* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, substituindo a marcação padrão é útil nas páginas onde não desejamos exibir relacionadas a logon opções na coluna esquerda.

Essas quatro páginas, no entanto, queremos mostrar uma marcação padrão da página mestra para o `LoginContent` ContentPlaceHolder. Portanto, remova a marcação declarativa para o `Content2` controle de conteúdo. Depois de fazer isso, cada marcação da página quatro deve conter somente um controle de conteúdo.

Por fim, vamos atualizar o mapa de site (`Web.sitemap`) para incluir essas novas páginas da web. Adicione o seguinte XML após o `<siteMapNode>` adicionamos para os tutoriais de associação.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Com o mapa de site atualizado, visite o site por meio de um navegador. Como mostra a Figura 2, a navegação à esquerda inclui itens para os tutoriais de funções.


[![Quatro novas páginas foram adicionadas para a pasta de funções](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figura 2**: quatro novas páginas foram adicionados para o `Roles` pasta ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Etapa 2: Especificar e configurar o provedor de funções do Framework

Como o framework de associação, estrutura de funções é criada sobre o modelo de provedor. Como discutido o <a id="_msoanchor_5"> </a> [ *Noções básicas sobre segurança e suporte ao ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) tutorial, o .NET Framework vem com três provedores de funções internas: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), e [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Esta série de tutoriais enfoca a `SqlRoleProvider`, que usa um banco de dados do Microsoft SQL Server como repositório de função.

Nos bastidores a estrutura de funções e `SqlRoleProvider` trabalho assim como a estrutura de associação e `SqlMembershipProvider`. O .NET Framework contém um `Roles` classe que serve como a API para a estrutura de funções. O `Roles` classe tem métodos estáticos como `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`e assim por diante. Quando um desses métodos é chamado, o `Roles` classe delega a chamada para o provedor configurado. O `SqlRoleProvider` funciona com as tabelas específicas de função (`aspnet_Roles` e `aspnet_UsersInRoles`) em resposta.

Para usar o `SqlRoleProvider` provedor em nosso aplicativo, é necessário especificar o banco de dados para usar como o repositório. O `SqlRoleProvider` espera que o repositório de função especificada com determinadas tabelas de banco de dados, exibições e procedimentos armazenados. Esses objetos de banco de dados necessários podem ser adicionados usando o [ `aspnet_regsql.exe` ferramenta](https://msdn.microsoft.com/library/ms229862.aspx). Neste ponto já existe um banco de dados com o esquema necessário para o `SqlRoleProvider`. Volta o <a id="_msoanchor_6"> </a> [ *criar o esquema de associação no SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) tutorial criamos um banco de dados denominado `SecurityTutorials.mdf` e usado `aspnet_regsql.exe` para adicionar o aplicativo serviços, que incluía os objetos de banco de dados necessários para o `SqlRoleProvider`. Portanto, é necessário informar o framework de funções para habilitar o suporte de função e usar o `SqlRoleProvider` com o `SecurityTutorials.mdf` banco de dados como o repositório da função.

A estrutura de funções é configurada por meio de &lt; `roleManager` &gt; elemento do aplicativo `Web.config` arquivo. Por padrão, o suporte da função está desabilitado. Para habilitá-lo, você deve definir o [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) do elemento `enabled` atributo `true` da seguinte forma:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Por padrão, todos os aplicativos web têm um provedor de funções chamado `AspNetSqlRoleProvider` do tipo `SqlRoleProvider`. Este provedor padrão é registrado no `machine.config` (localizado em `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

O provedor `connectionStringName` atributo especifica o repositório de função que é usado. O `AspNetSqlRoleProvider` provedor define esse atributo `LocalSqlServer`, que também é definido em `machine.config` pontos, por padrão, para um banco de dados do SQL Server 2005 Express Edition e o `App_Data` pasta denominada `aspnet.mdf`.

Consequentemente, se podemos simplesmente ativar a estrutura de funções sem especificar nenhuma informação de provedor em nosso aplicativo `Web.config` arquivo, o aplicativo usa o provedor de funções padrão registrado, `AspNetSqlRoleProvider`. Se o `~/App_Data/aspnet.mdf` banco de dados não existe, o tempo de execução do ASP.NET automaticamente criá-lo e adicionar o esquema do serviços de aplicativo. No entanto, nós não desejamos usar o `aspnet.mdf` banco de dados; em vez disso, queremos usar o `SecurityTutorials.mdf` banco de dados que nós já criou e adicionou o esquema do serviços de aplicativo para. Essa modificação pode ser feita de duas maneiras:

- <strong>Especifique um valor para o</strong><strong>`LocalSqlServer`</strong><strong>nome de cadeia de caracteres de conexão em</strong><strong>`Web.config`</strong><strong>.</strong> Substituindo o `LocalSqlServer` o valor do nome da cadeia de conexão no `Web.config`, podemos usar o provedor de funções padrão registrado (`AspNetSqlRoleProvider`) e ainda funcionar corretamente com o `SecurityTutorials.mdf` banco de dados. Para obter mais informações sobre essa técnica, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da postagem de blog, [Configurando serviços de aplicativos do ASP.NET 2.0 para uso do SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Adicionar um novo provedor registrado do tipo</strong><strong>`SqlRoleProvider`</strong><strong>e configurar seu</strong><strong>`connectionStringName`</strong><strong>configuração para apontar para o</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>banco de dados.</strong> Essa é a abordagem que recomendado e usado no <a id="_msoanchor_7"> </a> [ *criar o esquema de associação no SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) tutorial e é a abordagem que eu usarei neste tutorial também.

Adicione a seguinte marcação de configuração de funções para o `Web.config` arquivo. Essa marcação registra um novo provedor denominado `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Define a marcação acima a `SecurityTutorialsSqlRoleProvider` como o provedor padrão (por meio de `defaultProvider` atributo no `<roleManager>` elemento). Também define o `SecurityTutorialsSqlRoleProvider`do `applicationName` definindo como `SecurityTutorials`, que é o mesmo `applicationName` configuração usada pelo provedor de associação (`SecurityTutorialsSqlMembershipProvider`). Enquanto não mostrados aqui, o [ `<add>` elemento](https://msdn.microsoft.com/library/ms164662.aspx) para o `SqlRoleProvider` também pode conter um `commandTimeout` atributo para especificar a duração de tempo limite de banco de dados, em segundos. O valor padrão é 30.

Com essa marcação de configuração em vigor, você está pronto para começar a usar a funcionalidade de função em nosso aplicativo.

> [!NOTE]
> A marcação de configuração acima ilustra o uso de &lt; `roleManager` &gt; do elemento `enabled` e `defaultProvider` atributos. Há vários outros atributos que afetam como o framework de funções associa informações de função em uma base por usuário. Vamos examinar essas configurações no <a id="_msoanchor_8"> </a> [ *autorização baseada em função* ](role-based-authorization-cs.md) tutorial.


## <a name="step-3-examining-the-roles-api"></a>Etapa 3: Examinando as funções de API

Funcionalidade do framework de funções é exposta por meio de [ `Roles` classe](https://msdn.microsoft.com/library/system.web.security.roles.aspx), que contém métodos estáticos treze para executar operações baseadas em função. Quando vamos examinar criação e exclusão de funções em etapas 4 e 6 nós usaremos a [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) e [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) métodos que adicionar ou remover uma função do sistema.

Para obter uma lista de todas as funções do sistema, use o [ `GetAllRoles` método](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (consulte a etapa 5). O [ `RoleExists` método](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) retorna um valor booliano que indica se existe uma função especificada.

O seguinte tutorial, examinaremos como associar usuários a funções. O `Roles` da classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), e [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) métodos de adicionam um ou mais usuários a uma ou mais funções. Para remover usuários das funções, use o [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), ou [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) métodos.

No <a id="_msoanchor_9"> </a> [ *autorização baseada em função* ](role-based-authorization-cs.md) tutorial, examinaremos como mostrar ou ocultar com base na função do usuário conectado no momento a funcionalidade de programaticamente. Para fazer isso, podemos usar o `Role` da classe [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), ou [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) métodos.

> [!NOTE]
> Tenha em mente que sempre um desses métodos é invocado, o `Roles` classe delega a chamada para o provedor configurado. Em nosso caso, isso significa que a chamada está sendo enviada para o `SqlRoleProvider`. O `SqlRoleProvider` , em seguida, executa a operação de banco de dados apropriado com base no método invocado. Por exemplo, o código `Roles.CreateRole("Administrators")` resulta no `SqlRoleProvider` executando o `aspnet_Roles_CreateRole` procedimento armazenado, que insere um novo registro para o `aspnet_Roles` tabela chamada Administrators.


O restante deste tutorial examina usando o `Roles` da classe `CreateRole`, `GetAllRoles`, e `DeleteRole` métodos para gerenciar as funções do sistema.

## <a name="step-4-creating-new-roles"></a>Etapa 4: Criando novas funções

Funções oferecem uma maneira de agrupar arbitrariamente os usuários, e esse agrupamento é usado geralmente para uma maneira mais conveniente de aplicar regras de autorização. Mas para usar funções como um mecanismo de autorização que primeiro, precisamos definir as funções que existem no aplicativo. Infelizmente, o ASP.NET não inclui um controle CreateRoleWizard. Para adicionar novas funções, que é preciso criar uma interface de usuário adequado e chamar a API de funções de nós. A boa notícia é que isso é muito fácil de realizar.

> [!NOTE]
> Embora não haja nenhum controle de CreateRoleWizard Web, há o [ferramenta de administração de Site da Web do ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), que é um aplicativo ASP.NET local, projetado para ajudá-lo a exibir e gerenciar a configuração do seu aplicativo web. No entanto, não sou um ventilador grande da ferramenta de administração de Site da Web ASP.NET por dois motivos. Primeiro, é um bit com bugs e a experiência do usuário deixa muito a desejar. Em segundo lugar, a ferramenta de administração de Site da Web do ASP.NET foi projetada para funcionar apenas localmente, o que significa que você precisa criar sua própria função de páginas da web de gerenciamento, se você precisar gerenciar funções em um site ao vivo remotamente. Para esses dois motivos, este tutorial e a próxima se concentrará em compilar a função necessária ferramentas de gerenciamento em uma página da web em vez de contar com a ferramenta de administração de Site da Web do ASP.NET.


Abra o `ManageRoles.aspx` página o `Roles` pasta e adicione uma caixa de texto e um controle de botão Web para a página. Definir o controle de caixa de texto `ID` propriedade `RoleName` e o botão `ID` e `Text` propriedades `CreateRoleButton` e Create Role, respectivamente. Neste ponto, marcação declarativa da página deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Em seguida, clique duas vezes o `CreateRoleButton` botão controle no Designer para criar um `Click` manipulador de eventos e, em seguida, adicione o seguinte código:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Inicia o código acima, atribuindo o nome da função cortados inserido no `RoleName` caixa de texto para o `newRoleName` variável. Em seguida, o `Roles` da classe `RoleExists` método é chamado para determinar se a função `newRoleName` já existe no sistema. Se a função não existir, ele será criado por meio de uma chamada para o `CreateRole` método. Se o `CreateRole` método recebe um nome de função que já existe no sistema, um `ProviderException` exceção será lançada. Isso é porque o código primeiro verifica para garantir que a função ainda não existir no sistema antes de chamar `CreateRole`. O `Click` manipulador de eventos concluído limpando o `RoleName` da caixa de texto `Text` propriedade.

> [!NOTE]
> Você deve estar se perguntando o que acontecerá se o usuário não inserir qualquer valor para o `RoleName` caixa de texto. Se o valor passado para o `CreateRole` método é `null` ou uma cadeia de caracteres vazia, uma exceção é gerada. Da mesma forma, se o nome da função contém uma vírgula é gerada uma exceção. Consequentemente, a página deve conter controles de validação para garantir que o usuário insira uma função e que não contém qualquer vírgulas. Eu deixe como um exercício para o leitor.


Vamos criar uma função chamada administradores. Visite o `ManageRoles.aspx` página através de um navegador, digite Administradores na caixa de texto (consulte a Figura 3) e, em seguida, clique no botão Criar função.


[![Criar uma função de administradores](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figura 3**: criar uma função de administradores ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image9.png))


O que acontece? Ocorre um postback, mas não há nenhuma indicação visual que a função, na verdade, foi adicionado ao sistema. Atualizaremos esta página na etapa 5 para incluir comentários visuais. Agora, no entanto, você pode verificar se a função foi criada, vá para o `SecurityTutorials.mdf` banco de dados e exibir os dados do `aspnet_Roles` tabela. Como mostra a Figura 4, o `aspnet_Roles` tabela contém um registro para as funções de administradores adicionados apenas.


[![A tabela aspnet_Roles tem uma linha para os administradores](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figura 4**: O `aspnet_Roles` tabela tem uma linha para os administradores ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Etapa 5: Exibindo as funções do sistema

Vamos ampliar o `ManageRoles.aspx` página para incluir uma lista das funções atuais no sistema. Para fazer isso, adicione um controle GridView à página e defina seu `ID` propriedade `RoleList`. Em seguida, adicione um método para a classe de code-behind da página chamada `DisplayRolesInGrid` usando o seguinte código:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

O `Roles` da classe `GetAllRoles` método retorna todas as funções do sistema como uma matriz de cadeias de caracteres. Esta matriz de cadeia de caracteres é então associado a GridView. Para associar a lista de funções a GridView quando a página for carregada pela primeira vez, precisamos chamar o `DisplayRolesInGrid` método a partir da página `Page_Load` manipulador de eventos. O código a seguir chama este método quando a página for visitada pela primeira vez, mas não em postagens subsequentes.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Com esse código, visite a página por meio de um navegador. Como mostra a Figura 5, você verá uma grade com uma única coluna rotulada Item. A grade contém uma linha para a função de administradores que são adicionados na etapa 4.


[![O GridView exibe as funções em uma única coluna](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figura 5**: GridView exibe as funções em uma única coluna ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image15.png))


O GridView exibe uma única coluna Item porque o GridView `AutoGenerateColumns` propriedade é definida como True (o padrão), que faz com que o GridView criar automaticamente uma coluna para cada propriedade no seu `DataSource`. Uma matriz tem uma única propriedade que representa os elementos na matriz, portanto, a única coluna em GridView.

Ao exibir dados com um controle GridView, prefiro explicitamente definir minhas colunas em vez de tê-los implicitamente gerado pelo GridView. Definindo explicitamente as colunas é muito mais fácil formatar os dados, reorganizar as colunas e executar outras tarefas comuns. Portanto, vamos atualizar declarativo do GridView para que suas colunas são definidas explicitamente.

Comece definindo o GridView `AutoGenerateColumns` a propriedade como False. Em seguida, adicione um TemplateField à grade, defina seu `HeaderText` propriedade às funções e configurar seu `ItemTemplate` para que ele exibe o conteúdo da matriz. Para fazer isso, adicione um controle de rótulo Web chamado `RoleNameLabel` para o `ItemTemplate` e associar seu `Text` propriedade `Container.DataItem`.

Essas propriedades e o `ItemTemplate`do conteúdo pode ser definido declarativamente ou por meio de interface de editar modelos e a caixa de diálogo de campos do GridView. Para acessar os campos de caixa de diálogo, clique no link Editar colunas marca inteligente do GridView. Em seguida, desmarque a caixa de campos de geração automática de seleção para definir o `AutoGenerateColumns` a propriedade como False e adicionar um TemplateField GridView, definindo seu `HeaderText` propriedade à função. Para definir o `ItemTemplate`do conteúdo, escolha a opção de editar modelos de marca inteligente do GridView. Arraste um controle da Web de rótulo a `ItemTemplate`, defina seu `ID` propriedade `RoleNameLabel`e definir suas configurações de associação de dados para que seu `Text` propriedade está vinculada a `Container.DataItem`.

Independentemente de qual abordagem usada, marcação de declarativa resultante do GridView deve ser semelhante à seguinte quando terminar.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Conteúdo da matriz é exibido usando a sintaxe de associação de dados `<%# Container.DataItem %>`. Uma descrição completa de por que essa sintaxe é usada ao exibir o conteúdo de uma matriz associada a GridView está além do escopo deste tutorial. Para obter mais informações sobre esse assunto, consulte [uma matriz escalar de associação a um controle de Web dados](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Atualmente, o `RoleList` GridView só é associado à lista de funções quando a página for visitada primeiro. É necessário atualizar a grade sempre que uma nova função é adicionada. Para fazer isso, atualize o `CreateRoleButton` do botão `Click` manipulador de eventos para que ele chama o `DisplayRolesInGrid` método se uma nova função é criada.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Agora quando o usuário adiciona uma nova função de `RoleList` GridView mostra a função adicionada apenas em um postback, fornecendo comentários visuais que a função foi criada com êxito. Para ilustrar isso, visite o `ManageRoles.aspx` página através de um navegador e adicionar uma função chamada supervisores. Ao clicar no botão Create Role, ocorrerá um postback e a grade será atualizada para incluir administradores, bem como a nova função, supervisores.


[![A função de supervisores tem foi adicionado](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figura 6**: A função de supervisores tem foi adicionado ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Etapa 6: Excluindo funções

Neste ponto, um usuário pode criar uma nova função e exibir todas as funções existentes da `ManageRoles.aspx` página. Vamos permitir que os usuários também excluir funções. O `Roles.DeleteRole` método tem duas sobrecargas:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Exclui a função *roleName*. Uma exceção é gerada se a função contém um ou mais membros.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Exclui a função *roleName*. Se *throwOnPopulateRole* é `true`, em seguida, uma exceção é gerada se a função contém um ou mais membros. Se *throwOnPopulateRole* é `false`, em seguida, a função será excluída se ele contém todos os membros ou não. Internamente, o `DeleteRole(roleName)` chamadas de método `DeleteRole(roleName, true)`.

O `DeleteRole` método também lançará uma exceção se *roleName* é `null` ou uma cadeia de caracteres vazia ou se *roleName* contém uma vírgula. Se *roleName* não existe no sistema, `DeleteRole` falha silenciosamente, sem gerar uma exceção.

Vamos ampliar o GridView no `ManageRoles.aspx` para incluir a exclusão botão que, quando clicado, exclui a função selecionada. Comece adicionando um botão de exclusão a GridView indo para a caixa de diálogo de campos e adicionar um botão de exclusão, que está localizado sob a opção CommandField. Verifique a exclusão botão coluna à extrema esquerda e defina seu `DeleteText` propriedade excluir função.


[![Adicionar um botão de exclusão a RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figura 7**: adicionar um botão Excluir para o `RoleList` GridView ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image21.png))


Depois de adicionar o botão de exclusão, declarativo do GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Em seguida, crie um manipulador de eventos do GridView `RowDeleting` eventos. Este é o evento que é gerado em um postback quando clicar no botão Excluir função. Adicione o seguinte código ao manipulador de eventos.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

O código começa consultando programaticamente o `RoleNameLabel` Web controle na linha cuja função Excluir botão foi clicado. O `Roles.DeleteRole` método é invocado em seguida, passando o `Text` do `RoleNameLabel` e `false`e, portanto, excluir a função independentemente se há quaisquer usuários associados à função. Por fim, o `RoleList` GridView é atualizado para que a função excluída apenas não aparece mais na grade.

> [!NOTE]
> Botão Excluir função não requer qualquer tipo de confirmação do usuário antes de excluir a função. Uma das maneiras mais fáceis para confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [' adicionando confirmação do lado do cliente quando excluindo](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Resumo

Muitos aplicativos web têm certas regras de autorização ou funcionalidade de nível de página que está disponível apenas para determinadas classes de usuários. Por exemplo, pode haver um conjunto de páginas da web que somente os administradores podem acessar. Em vez de definir essas regras de autorização em uma base por usuário, muitas vezes, é mais útil definir as regras com base em uma função. Ou seja, em vez de explicitamente, permitindo que os usuários Scott e Jisun para acessar as páginas da web administrativo, uma abordagem mais passível de manutenção é permitir que os membros da função de administradores para acessar essas páginas e, em seguida, para indicar Scott e Jisun como os usuários pertencentes ao Função de administradores.

A estrutura de funções torna fácil criar e gerenciar funções. Neste tutorial, examinamos como configurar a estrutura de funções para usar o `SqlRoleProvider`, que usa um banco de dados do Microsoft SQL Server como repositório de função. Além disso, criamos uma página da web que lista as funções existentes no sistema e permite novas funções a serem criados e existentes a serem excluídos. Tutoriais subsequentes veremos como atribuir usuários às funções e como aplicar a autorização baseada em função.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Examinando o ASP.NET de 2.0 associação, funções e perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Como Usar o Gerenciador de funções no ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Provedores de função](https://msdn.microsoft.com/library/aa478950.aspx)
- [Implantar sua própria ferramenta de administração de site](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentação técnica para o `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)
- [Usando a associação e APIs do Gerenciador de função](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial incluem Alicja Maziarz Suchi Banerjee e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](assigning-roles-to-users-cs.md)
