---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementando um provedor de armazenamento de identidade do ASP.NET personalizado de MySQL | Microsoft Docs
author: raquelsa
description: O ASP.NET Identity é um sistema extensível que permite que você criar seu próprio provedor de armazenamento e conectá-lo ao seu aplicativo sem trabalhar novamente o aplicativo...
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: e5784991a95dcff0db38d87707bc83555da716df
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805669"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementando um provedor de armazenamento de identidade do ASP.NET personalizado de MySQL
====================
por [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> O ASP.NET Identity é um sistema extensível que permite que você criar seu próprio provedor de armazenamento e conectá-lo ao seu aplicativo sem trabalhar novamente o aplicativo. Este tópico descreve como criar um provedor de armazenamento do MySQL para o ASP.NET Identity. Para uma visão geral da criação de provedores de armazenamento personalizados, consulte [visão geral do armazenamento provedores personalizados para o ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Para concluir este tutorial, você deve ter o Visual Studio 2013 com atualização 2.
> 
> Este tutorial vai:
> 
> - Mostre como criar uma instância do banco de dados MySQL no Azure.
> - Mostre como usar uma ferramenta de cliente do MySQL (MySQL Workbench) para criar tabelas e gerenciar seu banco de dados remoto no Azure.
> - Mostra como substituir o padrão de implementação de armazenamento de identidade do ASP.NET com nossa implementação personalizada em um projeto de aplicativo do MVC.
> 
> Este tutorial foi originalmente escrito por Raquel Soares De Almeida e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). O projeto de exemplo foi atualizado para a identidade 2.0 por Suhas Joshi. O tópico foi atualizado para a identidade 2.0 por Tom FitzMacken.


## <a name="download-completed-project"></a>Projeto de download concluído

No final deste tutorial, você terá um projeto de aplicativo do MVC com o ASP.NET Identity trabalhando com um banco de dados MySQL hospedado no Azure.

Você pode baixar o provedor de armazenamento do MySQL concluído em [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>As etapas que serão executadas

Neste tutorial, você irá:

1. Criar um banco de dados MySQL no Azure
2. Criar a identidade do ASP.NET tabelas no MySQL
3. Criar um aplicativo MVC e configurá-lo para usar o provedor de MySQL
4. Executar o aplicativo

Este tópico não aborda a arquitetura do ASP.NET Identity e as decisões que você deve fazer ao implementar um provedor de armazenamento do cliente. Para obter essas informações, consulte [visão geral dos provedores personalizados de armazenamento para o ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Examine as classes de provedor de armazenamento do MySQL

Antes de pular para as etapas para criar o provedor de armazenamento do MySQL, vamos examinar as classes que compõem o provedor de armazenamento. Você precisará de classes que gerenciam as operações de banco de dados e classes que são chamadas do aplicativo para gerenciar usuários e funções.

### <a name="storage-classes"></a>Classes de armazenamento

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -contém propriedades para o usuário.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -contém operações para adicionar, atualizar ou recuperar os usuários.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -contém propriedades para as funções.
- [Alteração do RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -contém operações para adicionar, excluir, atualizar e recuperar funções.

### <a name="data-access-layer-classes"></a>Classes de camada de acesso de dados

Neste exemplo, as classes de camada de acesso de dados contêm instruções SQL para trabalhar com tabelas; No entanto, em seu código você talvez queira usar mapeamento relacional de objeto (ORM) como o Entity Framework ou NHibernate. Em particular, seu aplicativo pode apresentar um desempenho ruim sem um ORM que inclui o carregamento lento e o cache de objeto. Para obter mais informações, consulte [ASP.NET 2.0 de identidade sem Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -contém a conexão de banco de dados MySQL e métodos para executar operações de banco de dados. UserStore e alteração do RoleStore são instanciados com uma instância dessa classe.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -contém operações de banco de dados para a tabela que armazena as funções.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -contém operações de banco de dados para a tabela que armazena as declarações de usuário.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -contém operações de banco de dados para a tabela que armazena informações de logon do usuário.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -contém operações de banco de dados para a tabela que armazena a quais usuários são atribuídos a quais funções.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -contém operações de banco de dados para a tabela que armazena os usuários.

## <a name="create-a-mysql-database-instance-on-azure"></a>Criar uma instância do banco de dados MySQL no Azure

1. Faça logon no [portal do Azure](https://manage.windowsazure.com/).
2. Clique em **+ novo** na parte inferior da página e, em seguida, selecione **repositório**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. No **escolher e o complemento** assistente, selecione **banco de dados ClearDB MySQL** e clique na seta Avançar na parte inferior direita da caixa de diálogo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Mantenha o padrão **livre** planejar e alterar as **nome** para **IdentityMySQLDatabase**. Selecione a região mais próximo de você e, em seguida, clique na seta Avançar.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Clique na marca de seleção para concluir a criação do banco de dados.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Depois que seu banco de dados tiver sido criado, você pode gerenciá-lo na **complementos** guia no portal de gerenciamento.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Você pode obter as informações de conexão de banco de dados clicando em **informações de conexão** na parte inferior da página.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copie a cadeia de caracteres de conexão clicando no botão de cópia e salve-o para que você possa usar posteriormente em seu aplicativo MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Criar a identidade do ASP.NET tabelas no banco de dados MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Instalar a ferramenta MySQL Workbench para se conectar e gerenciar o banco de dados MySQL

1. Instalar o **MySQL Workbench** ferramenta do [página de downloads do MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Inicie o aplicativo e clique em Adicionar a **MySQLConnections +** botão para adicionar uma nova conexão. Use os dados de cadeia de caracteres de conexão copiados do banco de dados MySQL do Azure criado anteriormente neste tutorial.
3. Depois de estabelecer a conexão, abra uma nova **consulta** guia; cole os comandos a partir [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) na consulta e executá-lo para criar as tabelas de banco de dados.
4. Agora você tem todas as ASP.NET Identity tabelas necessárias criadas em um banco de dados MySQL hospedado no Azure, conforme mostrado abaixo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Criar um projeto de aplicativo do MVC de modelo e configurá-lo para usar o provedor de MySQL

Se necessário, instale o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) com atualização 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Baixe o projeto ASP.NET.Identity.MySQL do CodePlex

1. Navegue até a URL do repositório no [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Baixe o código-fonte.
3. Extraia o arquivo. zip em uma pasta local.
4. Abra a solução AspNet.Identity.MySQL e compilá-lo.

### <a name="create-a-new-mvc-application-project-from-template"></a>Criar um novo projeto de aplicativo do MVC de modelo

1. Clique com botão direito do **AspNet.Identity.MySQL** solução e **Add**, **novo projeto**
2. No **adicionar novo projeto** diálogo, selecione **Visual c#** à esquerda, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.NET**. Nomeie o projeto **IdentityMySQLDemo**; e, em seguida, clique em Okey.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. No **novo projeto ASP.NET** caixa de diálogo, selecione o modelo MVC com as opções padrão (que inclui **contas de usuário individuais** como método de autenticação) e clique em **Okey** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. No Gerenciador de soluções, clique em seu projeto IdentityMySQLDemo e selecione **gerenciar pacotes NuGet**. No diálogo de caixa de texto de pesquisa, digite **Identity.EntityFramework**. Selecione este pacote na lista de resultados e clique em **desinstalação**. Você será solicitado a desinstalar o pacote de dependência EntityFramework. Clique em Sim como não faremos esse pacote neste aplicativo.
5. Com o botão direito do mouse no projeto IdentityMySQLDemo, selecione **Add**, **referência, soluções, projetos;** selecione o projeto AspNet.Identity.MySQL e clique em **Okey**.
6. No projeto IdentityMySQLDemo, substitua todas as referências a  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   with  
     `using AspNet.Identity.MySQL;`
7. No IdentityModels.cs, defina **ApplicationDbContext** derivar **MySqlDatabase** e incluir um construtor que utilizam um parâmetro único com o nome da conexão.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Abra o arquivo IdentityConfig.cs. No **ApplicationUserManager.Create** método, substitua instanciar UserManager com o código a seguir:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Abra o arquivo Web. config e substitua a cadeia de caracteres DefaultConnection com essa entrada substituindo os valores realçados com a cadeia de caracteres de conexão do banco de dados MySQL criado nas etapas anteriores:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Execute o aplicativo e conectar-se ao banco de dados MySQL

1. Clique com botão direito do **IdentityMySQLDemo** do projeto e selecione **definir como projeto de inicialização**
2. Pressione **Ctrl + F5** para compilar e executar o aplicativo.
3. Clique em **registrar** guia na parte superior da página.
4. Insira um novo nome de usuário e senha e, em seguida, clique em **registrar**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. O novo usuário agora está registrado e conectado.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Volte para a ferramenta do MySQL Workbench e inspecionar os **IdentityMySQLDatabase** conteúdo da tabela. Inspecione a tabela de usuários para as entradas como registrar novos usuários.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre como habilitar outros métodos de autenticação no aplicativo, consulte [criar um aplicativo do ASP.NET MVC 5 com o Facebook e Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Para saber como integrar o seu banco de dados com o OAuth e configurar funções para limitar o acesso de usuários ao seu aplicativo, consulte [implantar um aplicativo ASP.NET MVC 5 seguro com associação, OAuth e banco de dados SQL no Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
