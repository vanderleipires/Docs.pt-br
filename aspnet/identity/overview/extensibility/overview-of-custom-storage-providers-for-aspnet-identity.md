---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Visão geral dos provedores de armazenamento personalizado para o ASP.NET Identity | Microsoft Docs
author: tfitzmac
description: O ASP.NET Identity é um sistema extensível que permite que você criar seu próprio provedor de armazenamento e conectá-lo ao seu aplicativo sem trabalhar novamente o aplicativo...
ms.author: aspnetcontent
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 35c8986024279bf66f239dbd969fba5ca02f3d01
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831726"
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Visão geral dos provedores de armazenamento personalizados para a identidade do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> O ASP.NET Identity é um sistema extensível que permite que você criar seu próprio provedor de armazenamento e conectá-lo ao seu aplicativo sem trabalhar novamente o aplicativo. Este tópico descreve como criar um provedor de armazenamento personalizado para a identidade do ASP.NET. Ele aborda os conceitos importantes para a criação de seu próprio provedor de armazenamento, mas não é passo a passo sobre como implementar um provedor de armazenamento personalizado.
> 
> Para obter um exemplo de implementação de um provedor de armazenamento personalizado, consulte [implementando um provedor de armazenamento de identidade personalizado MySQL ASP.NET](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Este tópico foi atualizado para o ASP.NET 2.0 de identidade.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Visual Studio 2013 com atualização 2
> - Identidade do ASP.NET 2


## <a name="introduction"></a>Introdução

Por padrão, o sistema de identidade do ASP.NET armazena informações de usuário em um banco de dados do SQL Server e usa o Entity Framework Code First para criar o banco de dados. Para muitos aplicativos, essa abordagem funciona bem. No entanto, talvez você prefira usar um tipo diferente de mecanismo de persistência, como o armazenamento de tabela do Azure, ou você pode já ter tabelas de banco de dados com uma estrutura muito diferente que a implementação do padrão. Em ambos os casos, você pode escrever um provedor personalizado para seu mecanismo de armazenamento e conecte esse provedor no seu aplicativo.

O ASP.NET Identity é incluído por padrão em muitos dos modelos do Visual Studio 2013. Você pode obter atualizações para a identidade do ASP.NET por meio [pacote EntityFramework NuGet do Microsoft AspNet identidade](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Este tópico inclui as seções a seguir:

- [Compreender a arquitetura](#architecture)
- [Compreender os dados armazenados](#data)
- [Criar camada de acesso a dados](#dal)
- [Personalizar a classe de usuário](#user)
- [Personalizar o repositório do usuário](#userstore)
- [Personalizar a classe de função](#role)
- [Personalizar o repositório da função](#rolestore)
- [Reconfigurar o aplicativo para usar o novo provedor de armazenamento](#reconfigure)
- [Outras implementações de provedores de armazenamento personalizado](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Compreender a arquitetura

Identidade do ASP.NET consiste em classes chamado gerentes e repositórios. Os gerentes são classes de alto nível que um desenvolvedor de aplicativo usa para executar operações, como a criação de um usuário no sistema de identidade do ASP.NET. Repositórios são classes de nível inferior que especificam como entidades, como usuários e funções, são persistentes. Lojas estão intimamente acopladas com o mecanismo de persistência, mas os gerentes são separados dos repositórios que significa que você pode substituir o mecanismo de persistência sem interromper todo o aplicativo.

O diagrama a seguir mostra como o seu aplicativo web interage com os gerentes e armazenamentos de interagem com a camada de acesso a dados.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Para criar um provedor de armazenamento personalizado para a identidade do ASP.NET, você precisa criar a fonte de dados, a camada de acesso a dados e as classes de armazenamento que interagem com essa camada de acesso a dados. Você pode continuar usando o mesmo Gerenciador de APIs para executar operações de dados, o usuário, mas agora que os dados sejam salvos em um sistema de armazenamento diferente.

Você não precisa personalizar as classes manager porque, ao criar uma nova instância do UserManager ou RoleManager você fornecer o tipo da classe de usuário e passar uma instância da classe de armazenamento como um argumento. Essa abordagem permite que você conecte suas classes personalizadas a estrutura existente. Você verá como criar uma instância UserManager e RoleManager com suas classes de repositório personalizado na seção [reconfigurar o aplicativo para usar o novo provedor de armazenamento](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Compreender os dados armazenados

Para implementar um provedor de armazenamento personalizado, você deve entender os tipos de dados usados com o ASP.NET Identity e decidir quais recursos são relevantes para seu aplicativo.

| Dados | Descrição |
| --- | --- |
| Usuários | Usuários registrados do seu site da web. Inclui o nome de usuário e Id de usuário. Pode incluir uma senha hash se os usuários fazem logon com credenciais que são específicas para seu site (em vez de usar credenciais de um site externo, como o Facebook) e o carimbo de segurança para indicar se nada foi alterado nas credenciais do usuário. Podem também incluir o endereço de email, número de telefone, se a autenticação de dois fatores é ativada, o número atual de logons que falharam, e se uma conta foi bloqueada. |
| Declarações de usuário | Um conjunto de instruções (ou declarações) sobre o usuário que representam a identidade do usuário. Pode permitir que uma expressão maior da identidade do usuário que pode ser obtido por meio de funções. |
| Logons de usuário | Informações sobre o provedor de autenticação externa (como Facebook) para usar ao fazer logon em um usuário. |
| Funções | Grupos de autorização para seu site. Inclui o nome da função Id e a função (como "Admin" ou "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Criar camada de acesso a dados

Este tópico pressupõe que você esteja familiarizado com o mecanismo de persistência que você pretende usar e como criar entidades para esse mecanismo. Este tópico fornece detalhes sobre como criar os repositórios ou classes de acesso a dados; em vez disso, ele fornece algumas sugestões sobre as decisões de design que você precisa fazer ao trabalhar com o ASP.NET Identity.

Você tem liberdade ao projetar os repositórios para um personalizado no provedor de repositório. Você só precisará criar repositórios para os recursos que você pretende usar em seu aplicativo. Por exemplo, se você não estiver usando as funções em seu aplicativo, você precisa criar armazenamento para funções ou funções de usuário. Sua tecnologia e a infraestrutura existente podem exigir uma estrutura que é muito diferente da implementação do padrão de identidade do ASP.NET. Na camada de acesso a dados, você deve fornecer a lógica para trabalhar com a estrutura de seus repositórios.

Para uma implementação de MySQL de repositórios de dados para o ASP.NET 2.0 de identidade, consulte [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Na camada de acesso a dados, você deve fornecer a lógica para salvar os dados de identidade do ASP.NET em sua fonte de dados. Camada de acesso a dados para o seu provedor de armazenamento personalizado pode incluir as seguintes classes para armazenar informações de usuário e a função.

| Classe | Descrição | Exemplo |
| --- | --- | --- |
| Contexto | Encapsula as informações para conectar-se ao mecanismo de persistência e executar consultas. Essa classe é central para sua camada de acesso a dados. Classes de dados exigirá uma instância dessa classe para executar suas operações. Você também irá inicializar suas classes de armazenamento com uma instância dessa classe. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Armazenamento de usuário | Armazena e recupera informações de usuário (por exemplo, o hash de nome e a senha do usuário). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Armazenamento de função | Armazena e recupera informações de função (como o nome da função). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Armazenamento de UserClaims | Armazena e recupera informações de declaração de usuário (como o tipo de declaração e o valor). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Armazenamento de UserLogins | Armazena e recupera informações de logon do usuário (como um provedor de autenticação externa). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Armazenamento de UserRole | Armazena e recupera a quais funções um usuário é atribuído a. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Novamente, você só precisa implementar as classes que você pretende usar em seu aplicativo.

As classes de acesso a dados, você fornece código para executar operações de dados para o mecanismo de persistência específico. Por exemplo, dentro da implementação do MySQL, a classe UserTable contém um método para inserir um novo registro na tabela de banco de dados de usuários. A variável chamada `_database` é uma instância da classe MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Depois de criar suas classes de acesso a dados, você deve criar classes de repositório que chamam os métodos específicos na camada de acesso a dados.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Personalizar a classe de usuário

Ao implementar seu próprio provedor de armazenamento, você deve criar uma classe de usuário que é equivalente à [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) classe a [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) namespace:

O diagrama a seguir mostra a classe IdentityUser que você deve criar e a interface para implementar nessa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

O [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interface define as propriedades que o UserManager tenta chamar ao executar operações solicitadas. A interface contém duas propriedades - Id e nome de usuário. O [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interface permite que você especifique o tipo da chave para o usuário por meio de genérica **TKey** parâmetro. O tipo da propriedade Id corresponde ao valor do parâmetro TKey.

A estrutura de identidade também fornece o [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) interface (sem o parâmetro genérico) quando você quiser usar um valor de cadeia de caracteres para a chave.

A classe IdentityUser implementa IUser e contém todos os construtores para usuários em seu site da web ou propriedades adicionais. O exemplo a seguir mostra uma classe IdentityUser que usa um número inteiro para a chave. O campo de Id é definido como **int** para corresponder ao valor do parâmetro genérico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Para uma implementação completa, consulte [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Personalizar o repositório do usuário

Você também pode criar uma classe UserStore que fornece os métodos para todas as operações de dados no usuário. Essa classe é equivalente à [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) classe os [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) namespace. Em sua classe UserStore, você implementa o [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) e qualquer uma das interfaces opcionais. Você selecione quais interfaces opcionais para implementar com base na funcionalidade que você deseja fornecer em seu aplicativo.

A imagem a seguir mostra a classe UserStore que você deve criar e as interfaces relevantes.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

O modelo de projeto padrão no Visual Studio contém código que pressupõe que muitas das interfaces opcionais foram implementadas no repositório do usuário. Se você estiver usando o modelo padrão com um repositório de usuário personalizada, implementar interfaces opcionais em seu repositório de usuários ou alterar o código de modelo para não chamar métodos em interfaces que não implementou.

 O exemplo a seguir mostra uma classe de armazenamento do usuário simples. O **TUser** parâmetro genérico usa o tipo de sua classe de usuário que normalmente é a classe IdentityUser que você definiu. O **TKey** parâmetro genérico usa o tipo da sua chave de usuário. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 Neste exemplo, o construtor que aceita um parâmetro denominado *banco de dados* do tipo ExampleDatabase é apenas uma ilustração de como passar em sua classe de acesso de dados. Por exemplo, na implementação do MySQL, este construtor aceita um parâmetro de tipo MySQLDatabase. 

Em sua classe UserStore, você deve usar as classes de acesso de dados que você criou para executar operações. Por exemplo, na implementação do MySQL, a classe UserStore tem o método CreateAsync que usa uma instância de UserTable para inserir um novo registro. O **inserir** método sobre o **userTable** objeto é o mesmo método que foi mostrado na seção anterior. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces a serem implementadas durante a personalização armazenamento de usuário

A imagem a seguir mostra mais detalhes sobre a funcionalidade definida em cada interface. Todas as interfaces opcionais herdam IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  O [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) interface é a única interface, você deve implementar em seu repositório de usuários. Define métodos para criar, atualizar, excluir e recuperar os usuários.
- **IUserClaimStore**  
  O [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) interface define os métodos você deve implementar em seu repositório de usuários para habilitar as declarações do usuário. Ela contém métodos ou adicionando, removendo e recuperando as declarações de usuário.
- **IUserLoginStore**  
  O [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) define os métodos você deve implementar em seu repositório de usuários para permitir que os provedores de autenticação externa. Ela contém métodos para adicionar, remover e recuperar os logons de usuário e um método para recuperar um usuário com base nas informações de logon.
- **IUserRoleStore**  
  O [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) interface define os métodos você deve implementar em seu repositório de usuários para mapear um usuário a uma função. Ela contém métodos para adicionar, remover e recuperar funções de usuário e um método para verificar se um usuário está atribuído a uma função.
- **IUserPasswordStore**  
  O [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) interface define os métodos que você deve implementar em seu repositório de usuários para persistir com hash de senhas. Ela contém métodos para obter e definir a senha de hash e um método que indica se o usuário tiver definido uma senha.
- **IUserSecurityStampStore**  
  O [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) interface define os métodos que você deve implementar em seu repositório de usuários para usar um carimbo de segurança para que indica se as informações da conta do usuário foi alterado . Esse carimbo é atualizado quando um usuário altera a senha ou adiciona ou remove logons. Ela contém métodos para obter e definir o carimbo de segurança.
- **IUserTwoFactorStore**  
  O [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) interface define os métodos que você deve implementar para implementar a autenticação de dois fatores. Ela contém métodos para obter e definir se a autenticação de dois fatores é ativada para um usuário.
- **IUserPhoneNumberStore**  
  O [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) interface define os métodos que você deve implementar para armazenar números de telefone do usuário. Ela contém métodos para obter e definir o número de telefone e se o número de telefone foi confirmado.
- **IUserEmailStore**  
  O [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) interface define os métodos que você deve implementar para armazenar endereços de email do usuário. Ela contém métodos para obter e definir o endereço de email e se o email for confirmado.
- **IUserLockoutStore**  
  O [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) interface define os métodos que você deve implementar para armazenar informações sobre como bloquear uma conta. Ela contém métodos para obter o número atual de tentativas de acesso com falha, obter e definir se a conta pode ser bloqueada, obter e definir a data de término do bloqueio, incrementando o número de tentativas com falha e redefinindo o número de tentativas com falha.
- **IQueryableUserStore**  
  O [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) interface define os membros que você deve implementar para fornecer um repositório de usuários que podem ser consultados. Ele contém uma propriedade que contém os usuários que podem ser consultados.

  Implementar as interfaces que são necessários em seu aplicativo. Por exemplo, o IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore e IUserSecurityStampStore interfaces conforme mostrado abaixo. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Para uma implementação completa (incluindo todas as interfaces), consulte [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin e IdentityUserRole

O namespace do EntityFramework contém implementações do [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), e [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) classes. Se você estiver usando esses recursos, você talvez queira criar suas próprias versões dessas classes e definir as propriedades de seu aplicativo. No entanto, às vezes, é mais eficiente para não carregar essas entidades na memória ao executar operações básicas (como adicionar ou remover a declaração do usuário). Em vez disso, as classes de armazenamento de back-end podem executar essas operações diretamente na fonte de dados. Por exemplo, o método UserStore.GetClaimsAsync() pode chamar o userClaimTable.FindByUserId(user. Método de ID) para executar uma consulta em que diretamente da tabela e retorna uma lista de declarações.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Personalizar a classe de função

Ao implementar seu próprio provedor de armazenamento, você deve criar uma classe de função que é equivalente à [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) classe a [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) namespace:

O diagrama a seguir mostra a classe IdentityRole que você deve criar e a interface para implementar nessa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

O [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interface define as propriedades que o RoleManager tenta chamar ao executar operações solicitadas. A interface contém duas propriedades - Id e nome. O [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interface permite que você especifique o tipo da chave para a função por meio de genérica **TKey** parâmetro. O tipo da propriedade Id corresponde ao valor do parâmetro TKey.

A estrutura de identidade também fornece o [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) interface (sem o parâmetro genérico) quando você quiser usar um valor de cadeia de caracteres para a chave.

O exemplo a seguir mostra uma classe de IdentityRole que usa um número inteiro para a chave. O campo de Id é definido como int para corresponder ao valor do parâmetro genérico. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Para uma implementação completa, consulte [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Personalizar o repositório da função

Você também pode criar uma classe de alteração do RoleStore que fornece os métodos para todas as operações de dados em funções. Essa classe é equivalente a [alteração do RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) classe no namespace Microsoft.ASP.NET.Identity.EntityFramework. Em sua classe de alteração do RoleStore, você implementa o [IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) e, opcionalmente, o [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) interface.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

O exemplo a seguir mostra uma classe de repositório de função. O parâmetro genérico de TRole usa o tipo de sua classe de função que normalmente é a classe de IdentityRole definida por você. O parâmetro genérico de TKey usa o tipo da sua chave de função. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  O [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) interface define os métodos para implementar em sua classe de repositório de função. Ela contém métodos para criar, atualizar, excluir e recuperar funções.
- **RoleStore&lt;TRole&gt;**  
  Para personalizar a alteração do RoleStore, crie uma classe que implementa a interface IRoleStore. Você só precisa implementar essa classe se quiser usar funções em seu sistema. O construtor que usa um parâmetro denominado *banco de dados* do tipo ExampleDatabase é apenas uma ilustração de como passar em sua classe de acesso de dados. Por exemplo, na implementação do MySQL, este construtor aceita um parâmetro de tipo MySQLDatabase.  
  
  Para uma implementação completa, consulte [alteração do RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Reconfigurar o aplicativo para usar o novo provedor de armazenamento

Você implementou o novo provedor de armazenamento. Agora, você deve configurar seu aplicativo para usar esse provedor de armazenamento. Se o provedor de armazenamento padrão foi incluído em seu projeto, você deve remover o provedor padrão e substituí-lo com seu provedor.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Substitua o provedor de armazenamento padrão no projeto do MVC

1. No **gerenciar pacotes NuGet** janela, desinstale o **EntityFramework de identidade do Microsoft ASP.NET** pacote. Você pode encontrar esse pacote, pesquisando Identity.EntityFramework nos pacotes instalados.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Você será solicitado se você também deseja desinstalar o Entity Framework. Se você não precisar dela em outras partes do seu aplicativo, você pode desinstalá-lo.
2. No arquivo IdentityModels.cs na pasta modelos, exclua ou comente o **ApplicationUser** e **ApplicationDbContext** classes. Em um aplicativo MVC, você pode excluir todo o arquivo IdentityModels.cs. Em um aplicativo Web Forms, exclua as duas classes, mas certifique-se de que manter a classe auxiliar que também está localizada no arquivo IdentityModels.cs.
3. Se seu provedor de armazenamento reside em um projeto separado, adicione uma referência a ele em seu aplicativo web.
4. Substitua todas as referências a `using Microsoft.AspNet.Identity.EntityFramework;` com o uso de uma instrução para o namespace do seu provedor de armazenamento.
5. No **Startup.Auth.cs** classe, altere o **ConfigureAuth** método para usar uma única instância do contexto apropriado. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. No aplicativo\_pasta de início, abra **IdentityConfig.cs**. Na classe ApplicationUserManager, alterar o **criar** método retorne um gerente de usuário que usa seu armazenamento de usuário personalizada. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Substitua todas as referências aos **ApplicationUser** com **IdentityUser**.
8. O projeto padrão inclui alguns membros na classe de usuário que não estão definidos na interface IUser; como Email, PasswordHash e GenerateUserIdentityAsync. Se sua classe de usuário não tiver esses membros, você deve implementá-las ou alterar o código que usa esses membros.
9. Se você tiver criado quaisquer instâncias de RoleManager, altere esse código para usar sua nova classe de alteração do RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. O projeto padrão destina-se uma classe de usuário que tem um valor de cadeia de caracteres para a chave. Se sua classe de usuário tem um tipo diferente para a chave (por exemplo, um número inteiro), você deverá alterar o projeto para trabalhar com seu tipo. Ver [alterar a chave primária para os usuários no ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Se necessário, adicione a cadeia de caracteres de conexão no arquivo Web. config.

<a id="other"></a>
## <a name="other-resources"></a>Outros recursos

- Blog: [implementar a identidade do ASP.NET](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Código do tutorial e o GIT: [Simple.Data provedor de identidade do Asp.Net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Tutorial:[Configurando as contas de identidade básicas e apontá-los em um banco de dados externo](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Por [ @xivSolutions ](https://twitter.com/xivSolutions).
- Tutorial[: Implementando um provedor de armazenamento de identidade do ASP.NET personalizado de MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entidades CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) por [SoftFluent](http://www.softfluent.com/)
- [Armazenamento de tabelas do Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) por James Randall.
- Armazenamento de tabelas do Azure: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) pela [ @stuartleeks ](https://twitter.com/stuartleeks).
- [O CouchDB / Cloudant por Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elástico Pesqui[h: identidade Elástico](https://github.com/bmbsqd/elastic-identity) por AB Bombsquad.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) por Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) por Antônio Milesi Bastos.
- [O RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) pela [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) pela [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modelos T4 para gerar o EF de código para um repositório de usuário "primeiro banco de dados": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
