---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'Identidade do ASP.NET: Usar o armazenamento do MySQL com um provedor de MySQL do EntityFramework (c#) | Microsoft Docs'
author: maumar
description: Este tutorial mostra como substituir o mecanismo de armazenamento de dados padrão para a identidade do ASP.NET com EntityFramework (provedor de cliente SQL) por uma ou se contrair MySQL...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6b1d57c65cb4197d1b20175415ee73b3e81cb53f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383033"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>Identidade do ASP.NET: Usar o armazenamento do MySQL com um provedor de MySQL do EntityFramework (c#)
====================
por [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Este tutorial mostra como substituir o mecanismo de armazenamento de dados padrão para [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) com EntityFramework (provedor de cliente SQL) com um provedor de MySQL.


Os seguintes tópicos serão abordados neste tutorial:

- Criação de um banco de dados MySQL no Azure
- Criando um aplicativo MVC usando o modelo MVC do Visual Studio 2013
- Configurando o EntityFramework para trabalhar com um provedor de banco de dados MySQL
- Executando o aplicativo para verificar os resultados

No final deste tutorial, você terá um aplicativo MVC com a identidade do ASP.NET armazene que está usando um banco de dados MySQL que está hospedado no Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Criação de uma instância do banco de dados MySQL no Azure

1. Faça logon no [portal do Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Clique em **NEW** na parte inferior da página e, em seguida, selecione **repositório**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. No **escolher e o complemento** assistente, selecione **banco de dados ClearDB MySQL**e, em seguida, clique no **próxima** seta na parte inferior do quadro:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Mantenha o padrão **livre** planejar, altere o **nome** para **IdentityMySQLDatabase**, selecione a região mais próxima de você e, em seguida, clique no **Avançar** seta na parte inferior do quadro:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Clique o **compra** marca de seleção para concluir a criação do banco de dados.  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Depois que seu banco de dados tiver sido criado, você pode gerenciá-lo na **complementos** guia no portal de gerenciamento. Para recuperar as informações de conexão do banco de dados, clique em **informações de conexão** na parte inferior da página:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copie a cadeia de conexão clicando no botão de cópia, o **CONNECTIONSTRING** campo e salvá-lo; você usará essas informações posteriormente no tutorial para seu aplicativo MVC:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Criando um projeto de aplicativo do MVC

Para concluir as etapas nesta seção do tutorial, você primeiro precisará instalar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Depois que o Visual Studio tiver sido instalado, use as seguintes etapas para criar um novo projeto de aplicativo do MVC:

1. Abra o Visual Studio 2103.
2. Clique em **novo projeto** da **inicie** página, ou você pode clicar na **arquivo** menu e, em seguida, **novo projeto**:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Quando o **novo projeto** caixa de diálogo for exibida, expanda **Visual c#** na lista de modelos, em seguida, clique em **Web**e selecione **ASP.NET Web Application**. Nomeie o projeto **IdentityMySQLDemo** e, em seguida, clique em **Okey**:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. No **novo projeto ASP.NET** caixa de diálogo, selecione o **MVC** templatewith as opções padrão; esse será configurar **contas de usuário individuais** como método de autenticação. Clique em **Okey**:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurar o EntityFramework para trabalhar com um banco de dados MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Atualizar o assembly do Entity Framework para o seu projeto

O aplicativo de MVC que foi criado a partir do modelo do Visual Studio 2013 contém uma referência para o [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) do pacote, mas houve foi atualizações a esse assembly desde seu lançamento, que contêm significativa melhorias de desempenho. Para usar essas atualizações mais recentes em seu aplicativo, use as etapas a seguir.

1. Abra seu projeto do MVC no Visual Studio 2013.
2. Clique em **ferramentas**, em seguida, clique em **Gerenciador de pacotes de biblioteca**e, em seguida, clique em **Package Manager Console**:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. O **Package Manager Console** aparecerá na seção inferior do Visual Studio. Tipo de &quot; **Update-Package EntityFramework** &quot; e pressione Enter:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Instalar o provedor de MySQL para EntityFramework

Para o EntityFramework para se conectar ao banco de dados MySQL, você precisa instalar um provedor de MySQL. Para fazer isso, abra o **Package Manager Console** e digite &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, e pressione Enter.

> [!NOTE]
> Esta é uma versão de pré-lançamento do assembly e como tal, ele pode conter bugs. Você não deve usar uma versão de pré-lançamento do provedor em produção.


[Clique a imagem a seguir para expandi-la.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Fazer alterações de configuração de projeto para o arquivo Web. config para seu aplicativo

Nesta seção que você irá configurar o Entity Framework para usar o provedor de MySQL que acabou de instalar, registrar o alocador de provedores de MySQL e adicione sua cadeia de caracteres de conexão do Azure.

> [!NOTE]
> Os exemplos a seguir contém uma versão do assembly específico para MySql.Data.dll. Se a versão do assembly for alterado, você precisará modificar as definições de configuração apropriado com a versão correta.


1. Abra o arquivo Web. config para seu projeto no Visual Studio 2013.
2. Localize as seguintes definições de configuração, que definem o provedor de banco de dados padrão e a fábrica para o Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Substitua os parâmetros de configuração a seguir, que irá configurar o Entity Framework para usar o provedor de MySQL: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Localize o &lt;connectionStrings&gt; seção e substitua-o pelo código a seguir, que definirá a cadeia de caracteres de conexão do banco de dados MySQL que está hospedado no Azure (Observe que o valor de providerName também foi alterado da original):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Adicionar contexto de MigrationHistory personalizado

Entity Framework Code First usa um **MigrationHistory** tabela para manter controle das alterações de modelo e para garantir a consistência entre o esquema de banco de dados e esquema conceitual. No entanto, essa tabela não funciona para MySQL por padrão porque a chave primária é muito grande. Para corrigir essa situação, você precisa reduzir o tamanho da chave para a tabela. Para fazer isso, use as seguintes etapas:

1. As informações de esquema para essa tabela são capturadas em uma **HistoryContext**, que pode ser modificado como qualquer outro **DbContext**. Para fazer isso, adicione um novo arquivo de classe chamado **MySqlHistoryContext.cs** ao projeto e substitua seu conteúdo pelo seguinte código:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Em seguida, será necessário configurar o Entity Framework para usar o modificada **HistoryContext**, em vez do padrão. Isso pode ser feito, aproveitando os recursos de configuração com base em código. Para fazer isso, adicione o novo arquivo de classe chamado **MySqlConfiguration.cs** ao seu projeto e substitua seu conteúdo com:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Criação de um inicializador personalizado do EntityFramework para ApplicationDbContext

O provedor de MySQL é mostrado neste tutorial não suporta no momento, as migrações do Entity Framework, portanto, será necessário usar os inicializadores de modelo para se conectar ao banco de dados. Como este tutorial está usando uma instância do MySQL no Azure, você precisará criar um inicializador personalizado do Entity Framework.

> [!NOTE]
> Essa etapa não será necessária se você estiver se conectando a uma instância do SQL Server no Azure ou se você estiver usando um banco de dados estiver hospedado localmente.


Para criar um inicializador personalizado do Entity Framework para MySQL, use as seguintes etapas:

1. Adicionar um novo arquivo de classe chamado **MySqlInitializer.cs** para o projeto e substitua é conteúdo com o código a seguir: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Abra o **IdentityModels.cs** arquivo do seu projeto, que está localizado na **modelos** diretório e substitua seu conteúdo pelo seguinte: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Executando o aplicativo e verificar o banco de dados

Depois de concluir as etapas nas seções anteriores, você deve testar seu banco de dados. Para fazer isso, use as seguintes etapas:

1. Pressione **Ctrl + F5** para compilar e executar o aplicativo web.
2. Clique o **registrar** guia na parte superior da página:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Insira um novo nome de usuário e senha e, em seguida, clique em **registrar**:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. Neste ponto, as tabelas de identidade do ASP.NET são criadas no banco de dados MySQL e o usuário está registrado e conectado ao aplicativo:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalando a ferramenta MySQL Workbench para verificar os dados

1. Instalar o **MySQL Workbench** ferramenta do [página de downloads do MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. No Assistente de instalação: **seleção de recursos** guia, selecione **MySQL Workbench** sob **aplicativos** seção.
3. Inicie o aplicativo e adicionar uma nova conexão usando os dados de cadeia de caracteres de conexão do banco de dados MySQL do Azure criada no implorando deste tutorial.
4. Depois de estabelecer a conexão, inspecione a **ASP.NET Identity** tabelas criadas no **IdentityMySQLDatabase.**
5. Você verá que todos os ASP.NET requeridos para identidade tabelas são criadas conforme mostrado na imagem abaixo:  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Inspecione o **aspnetusers** por instância de tabela para verificar as entradas como registrar novos usuários.  
  
   [Clique a imagem a seguir para expandi-la. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
