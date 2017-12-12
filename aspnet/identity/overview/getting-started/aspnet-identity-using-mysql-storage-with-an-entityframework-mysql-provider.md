---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'Identidade do ASP.NET: Usando o armazenamento de MySQL com um provedor de MySQL EntityFramework (c#) | Microsoft Docs'
author: maumar
description: "Este tutorial mostra como substituir o mecanismo de armazenamento de dados padrão para a identidade do ASP.NET com EntityFramework (provedor de cliente SQL) por fornecer um MySQL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: ac254abcb756d048d159a9b67967a581f35ac871
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>Identidade do ASP.NET: Usando o armazenamento de MySQL com um provedor de EntityFramework MySQL (c#)
====================
por [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Este tutorial mostra como substituir o mecanismo de armazenamento de dados padrão para [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) com EntityFramework (provedor de cliente SQL) com um provedor MySQL.


Os tópicos a seguir serão abordados neste tutorial:

- Criando um banco de dados MySQL no Azure
- Criando um aplicativo MVC usando o modelo MVC de 2013 do Visual Studio
- Configurando EntityFramework para trabalhar com um provedor de banco de dados MySQL
- Executando o aplicativo para verificar os resultados

No final deste tutorial, você terá um aplicativo MVC com a identidade do ASP.NET armazenar que está usando um banco de dados MySQL é hospedado no Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Criando uma instância de banco de dados MySQL no Azure

1. Faça logon na [Portal do Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Clique em **novo** na parte inferior da página e, em seguida, selecione **repositório**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. No **escolha e o complemento** assistente, selecione **ClearDB banco de dados MySQL**e, em seguida, clique no **próximo** seta na parte inferior do quadro:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Mantenha o padrão **livre** planejar, altere o **nome** para **IdentityMySQLDatabase**, selecione a região mais próxima a você e, em seguida, clique no **Avançar** seta na parte inferior do quadro:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Clique o **compra** marca de seleção para concluir a criação do banco de dados.  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Depois que o banco de dados tiver sido criado, você pode gerenciá-lo do **complementos** guia no portal de gerenciamento. Para recuperar as informações de conexão do banco de dados, clique em **informações de conexão** na parte inferior da página:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copie a cadeia de caracteres de conexão, clicando no botão Copiar pelo **CONNECTIONSTRING** campo e salvá-lo; você usará essas informações posteriormente neste tutorial para seu aplicativo MVC:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Criando um projeto de aplicativo MVC

Para concluir as etapas nesta seção do tutorial, você primeiro precisará instalar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Depois que o Visual Studio foi instalado, use as seguintes etapas para criar um novo projeto de aplicativo MVC:

1. Abra o Visual Studio 2103.
2. Clique em **novo projeto** do **iniciar** página, ou você pode clicar no **arquivo** menu e, em seguida, **novo projeto**:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Quando o **novo projeto** caixa de diálogo é exibida, expanda **Visual C#** na lista de modelos, em seguida, clique em **Web**e selecione **doaplicativodeWebdoASP.NET**. Nomeie o projeto **IdentityMySQLDemo** e, em seguida, clique em **Okey**:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. No **novo projeto ASP.NET** caixa de diálogo, selecione o **MVC** templatewith as opções padrão; isto configurar **contas de usuário individuais** como o método de autenticação. Clique em **Okey**:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurar EntityFramework para trabalhar com um banco de dados MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Atualizar o assembly do Entity Framework para o seu projeto

O aplicativo MVC que foi criado a partir do modelo do Visual Studio 2013 contém uma referência para o [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) do pacote, mas há têm foi atualizações a esse assembly desde seu lançamento que contêm significativa melhorias de desempenho. Para usar essas atualizações mais recentes em seu aplicativo, use as etapas a seguir.

1. Abra seu projeto MVC no Visual Studio 2013.
2. Clique em **ferramentas**, em seguida, clique em **Gerenciador de biblioteca de pacote**e, em seguida, clique em **Package Manager Console**:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. O **Package Manager Console** aparecerá na seção inferior do Visual Studio. Tipo &quot; **EntityFramework do pacote de atualização** &quot; e pressione Enter:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Instalar o provedor do MySQL para EntityFramework

Em ordem para EntityFramework para se conectar ao banco de dados MySQL, você precisa instalar um provedor MySQL. Para fazer isso, abra o **Package Manager Console** e tipo &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, e pressione Enter.

> [!NOTE]
> Esta é uma versão de pré-lançamento do assembly e como tal, ele pode conter bugs. Você não deve usar uma versão de pré-lançamento do provedor em produção.


[Clique a imagem a seguir para expandi-lo.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Fazer alterações de configuração de projeto para o arquivo Web. config para seu aplicativo

Nesta seção, que você configurará o Entity Framework para usar o provedor do MySQL que você acabou de instalar, registrar a fábrica de provedor MySQL e adicione sua cadeia de caracteres de conexão do Azure.

> [!NOTE]
> Os exemplos a seguir contêm uma versão específica de assembly para MySql.Data.dll. Se a versão do assembly é alterado, você precisará modificar as definições de configuração apropriado com a versão correta.


1. Abra o arquivo Web. config para seu projeto no Visual Studio 2013.
2. Localize as seguintes definições de configuração, que define o provedor de banco de dados padrão e a fábrica para o Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Substitua os parâmetros de configuração com o seguinte, que irá configurar o Entity Framework para usar o provedor de MySQL: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Localize o &lt;connectionStrings&gt; seção e substituí-lo com o código a seguir, que define a cadeia de caracteres de conexão do banco de dados MySQL que é hospedado no Azure (Observe que o valor de providerName também foi alterado da original):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Adicionando contexto MigrationHistory personalizado

Entity Framework Code First usa um **MigrationHistory** tabela para manter controle das alterações de modelo e para garantir a consistência entre o esquema de banco de dados e esquema conceitual. No entanto, essa tabela não funciona para MySQL por padrão porque a chave primária é muito grande. Para corrigir essa situação, você precisa reduzir o tamanho da chave para essa tabela. Para fazer isso, use as seguintes etapas:

1. As informações de esquema para essa tabela são capturadas em uma **HistoryContext**, que pode ser modificada como qualquer outro **DbContext**. Para fazer isso, adicione um novo arquivo de classe chamado **MySqlHistoryContext.cs** para o projeto e substitua o seu conteúdo com o código a seguir:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Em seguida, será necessário configurar o Entity Framework para usar o **HistoryContext**, em vez do padrão. Isso pode ser feito aproveitando os recursos de configuração baseada em código. Para fazer isso, adicione o novo arquivo de classe chamado **MySqlConfiguration.cs** ao seu projeto e substitua o seu conteúdo com:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Criando um inicializador de EntityFramework personalizado para ApplicationDbContext

O provedor do MySQL é caracterizado neste tutorial não suporta atualmente migrações do Entity Framework, portanto, será necessário usar inicializadores de modelo para se conectar ao banco de dados. Como este tutorial está usando uma instância do MySQL no Azure, será necessário necessário criar um inicializador personalizado do Entity Framework.

> [!NOTE]
> Essa etapa não será necessária se você estiver se conectando a uma instância do SQL Server no Azure ou se você estiver usando um banco de dados que é hospedado no local.


Para criar um inicializador personalizado do Entity Framework para MySQL, use as seguintes etapas:

1. Adicionar um novo arquivo de classe chamado **MySqlInitializer.cs** para o projeto e substituir é conteúdo com o código a seguir: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Abra o **IdentityModels.cs** arquivo para seu projeto, que está localizado no **modelos** directory e substitua o conteúdo do arquivo com o seguinte: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Executar o aplicativo e verificar o banco de dados

Depois de concluir as etapas nas seções anteriores, você deve testar o banco de dados. Para fazer isso, use as seguintes etapas:

1. Pressione **Ctrl + F5** para compilar e executar o aplicativo web.
2. Clique o **registrar** guia no topo da página:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Digite um novo nome de usuário e senha e, em seguida, clique em **registrar**:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. Agora as tabelas de identidade do ASP.NET são criadas no banco de dados MySQL, e o usuário está registrado e conectado ao aplicativo:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalando a ferramenta MySQL Workbench para verificar os dados

1. Instalar o **MySQL Workbench** ferramenta do [página downloads do MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. No Assistente de instalação: **seleção de recursos** guia, selecione **MySQL Workbench** em **aplicativos** seção.
3. Inicie o aplicativo e adicione uma nova conexão usando os dados de cadeia de caracteres de conexão do banco de dados MySQL Azure criada no implorando deste tutorial.
4. Depois de estabelecer a conexão, inspecione o **ASP.NET Identity** tabelas criadas no **IdentityMySQLDatabase.**
5. Você verá que todos os ASP.NET Identity necessário tabelas são criadas, conforme mostrado na imagem a seguir:  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Inspecione o **aspnetusers** para a instância de tabela para verificar as entradas como registrar novos usuários.  
  
 [Clique a imagem a seguir para expandi-lo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
