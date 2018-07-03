---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Preparando-se para a implantação de banco de dados | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 21b4a924115daff6ee79ce045330c748b58246ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388526"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Implantação de Web do ASP.NET usando o Visual Studio: Preparando-se para a implantação de banco de dados
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como obter o projeto pronto para implantação de banco de dados. A estrutura de banco de dados e alguns (não todos) dos dados em dois do aplicativo bancos de dados devem ser implantados em ambientes de produção, preparo e teste.

Normalmente, à medida que você desenvolve um aplicativo, você insere dados de teste em um banco de dados que você não deseja implantar em um site ativo. No entanto, você também pode ter alguns dados de produção que você deseja implantar. Neste tutorial, você configurar o projeto do Contoso University e preparar scripts SQL para que os dados corretos serão incluídos quando você implanta.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

O aplicativo de exemplo usa o SQL Server Express LocalDB. SQL Server Express é a edição gratuita do SQL Server. Ele normalmente é usado durante o desenvolvimento, como ele se baseia no mesmo mecanismo de banco de dados a versões completas do SQL Server. Você pode testar com o SQL Server Express e ter certeza de que o aplicativo se comportará em produção, com algumas exceções para recursos que variam entre edições do SQL Server.

O LocalDB é um modo de execução especial do SQL Server Express que permite que você trabalhe com bancos de dados como *mdf* arquivos. Normalmente, os arquivos de banco de dados LocalDB são mantidos na *App\_dados* pasta de um projeto web. O recurso de instância de usuário do SQL Server Express também permite que você trabalhe com *mdf* arquivos, mas o recurso de instância de usuário é preterido; portanto, o LocalDB é recomendado para trabalhar com *mdf* arquivos.

Normalmente o SQL Server Express não é usado para aplicativos da web de produção. LocalDB em particular não é recomendado para uso em produção com um aplicativo web porque ela não foi projetada para trabalhar com o IIS.

No Visual Studio 2012, o LocalDB é instalado por padrão com o Visual Studio. No Visual Studio 2010 e versões anteriores, o SQL Server Express (sem o LocalDB) é instalado por padrão com o Visual Studio; Isto é por que você instalou-lo como um dos pré-requisitos em [o primeiro tutorial nesta série](introduction.md).

Para obter mais informações sobre as edições do SQL Server, incluindo LocalDB, consulte os seguintes recursos [trabalhando com bancos de dados do SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework e os provedores universais

Para acesso de banco de dados, o aplicativo Contoso University exige o seguinte software deve ser implantado com o aplicativo porque ele não está incluído no .NET Framework:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (permite que o sistema de associação do ASP.NET usar o banco de dados SQL)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Como esse software está incluído nos pacotes NuGet, o projeto já estiver configurado para que os assemblies necessários são implantados com o projeto. (Os links apontam para as versões atuais desses pacotes, que pode ser mais recente do que o que é instalado no projeto de início que você baixou para este tutorial.)

Se você estiver implantando em um provedor de hospedagem de terceiros em vez do Azure, certifique-se de que você usa o Entity Framework 5.0 ou posterior. Versões anteriores de migrações do Code First exigem confiança total e a maioria dos provedores de hospedagem executará seu aplicativo de confiança médio. Para obter mais informações sobre o nível de confiança média, consulte o [implantar no IIS como um ambiente de teste](deploying-to-iis.md) tutorial.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurar migrações do Code First para implantação de banco de dados de aplicativo

O banco de dados do aplicativo Contoso University é gerenciado pelo Code First e vai implantá-lo por meio de migrações do Code First. Para uma visão geral da implantação de banco de dados por meio de migrações do Code First, consulte [o primeiro tutorial nesta série](introduction.md).

Quando você implanta um banco de dados do aplicativo, normalmente você não simplesmente implanta seu banco de dados de desenvolvimento com todos os dados contidos nela para produção, porque muitos dos dados em que ele é provavelmente existe apenas para fins de teste. Por exemplo, os nomes de alunos em um banco de dados de teste são fictícios. Por outro lado, você geralmente não é possível implantar apenas a estrutura de banco de dados sem dados nele em todos os. Alguns dos dados no banco de dados de teste podem ser dados reais e devem estar lá quando os usuários começam a usar o aplicativo. Por exemplo, seu banco de dados pode ter uma tabela que contém os valores de classificação válidos ou nomes de departamento real.

Para simular esse cenário comum, você configurará um migrações do Code First `Seed` método que insere no banco de dados, somente os dados que você deseja estar lá em produção. Isso `Seed` método não deve inserir dados de teste porque ele será executado em produção depois que o Code First cria o banco de dados em produção.

Em versões anteriores do Code First antes que as migrações foi lançado, era comum `Seed` métodos para inserir dados de teste também, porque cada alteração de modelo durante o desenvolvimento de banco de dados tinha completamente ser excluído e recriado do zero. Com migrações do Code First, teste os dados são mantidos após as alterações do banco de dados, portanto, incluindo dados de teste no `Seed` método não é necessário. O projeto que você baixou usa o método de todos os dados, incluindo o `Seed` método de uma classe de inicializador. Neste tutorial, você vai desabilitar essa classe de inicializador e `enable Migrations. Then you'll update the `semente ' método na configuração de migrações de classe para que ele insira apenas os dados que você deseja ser inserida em produção.

O diagrama a seguir ilustra o esquema do banco de dados do aplicativo:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Para esses tutoriais, você assumirá que o `Student` e `Enrollment` tabelas devem estar vazias quando o site é implantado pela primeira vez. As outras tabelas contêm dados que deve ser pré-carregado quando o aplicativo for lançado.

### <a name="disable-the-initializer"></a>Desabilitar o inicializador

Uma vez que você usará migrações do Code First, você não precisa usar o `DropCreateDatabaseIfModelChanges` inicializador do Code First. O código para esse inicializador é na *SchoolInitializer.cs* arquivo no projeto ContosoUniversity.DAL. Uma configuração na `appSettings` elemento do *Web. config* arquivo faz com que esse inicializador ser executado sempre que o aplicativo tenta acessar o banco de dados pela primeira vez:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Abra o aplicativo *Web. config* do arquivo e remova ou comente o `add` elemento que especifica a classe de inicializador Code First. O `appSettings` elemento agora tem esta aparência:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Outra maneira de especificar uma classe de inicializador é fazê-lo chamando `Database.SetInitializer` no `Application_Start` método na *global. asax* arquivo. Se você estiver habilitando as migrações em um projeto que usa esse método para especificar o inicializador, remova essa linha de código.


> [!NOTE]
> Se você estiver usando o Visual Studio 2013, adicione as etapas a seguir entre as etapas 2 e 3: (a) no PMC insira "update-package entityframework – versão 6.1.1" para obter a versão atual do EF. Em seguida, a compilação de (b) o projeto para obter uma lista de erros de compilação e corrigi-los. Excluir usando as instruções para namespaces que não existem, clique com botão direito e clique em resolver para adicionar instruções de uso onde eles são necessários e altere as ocorrências de System.Data.EntityState para entitystate.


### <a name="enable-code-first-migrations"></a>Habilitar migrações do Code First

1. Certifique-se de que o projeto de ContosoUniversity (não ContosoUniversity.DAL) é definido como o projeto de inicialização. Na **Gerenciador de soluções**, clique com botão direito no projeto ContosoUniversity e selecione **definir como projeto de inicialização**. Migrações do Code First examinará o projeto de inicialização para localizar a cadeia de caracteres de conexão de banco de dados.
2. Dos **ferramentas** menu, clique em **Gerenciador de pacotes de biblioteca** (ou **Gerenciador de pacotes NuGet**) e, em seguida, **Package Manager Console**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Na parte superior a **Package Manager Console** janela Selecione ContosoUniversity.DAL como o projeto padrão e, em seguida, at a `PM>` prompt insira "enable-migrations".

    ![comando enable-migrations](preparing-databases/_static/image4.png)

    (Se você receber um erro dizendo que o *enable-migrations* comando não for reconhecido, digite o comando *update-package EntityFramework – reinstalar* e tente novamente.)

    Este comando cria uma *migrações* pasta no projeto ContosoUniversity.DAL e ele coloca nessa pasta dois arquivos: um *Configuration.cs* arquivo que você pode usar para configurar migrações e um *InitialCreate.cs* arquivo para a primeira migração que cria o banco de dados.

    ![Pasta Migrations](preparing-databases/_static/image5.png)

    Você selecionou o projeto DAL na **projeto padrão** lista suspensa da **Package Manager Console** porque o `enable-migrations` comando deve ser executado no projeto que contém o Code First classe de contexto. Quando essa classe está em um projeto de biblioteca de classes, as migrações Code First procura a cadeia de caracteres de conexão de banco de dados no projeto de inicialização para a solução. Na solução ContosoUniversity, o projeto da web foi definido como o projeto de inicialização. Se você não quiser designar o projeto que tem a cadeia de caracteres de conexão como o projeto de inicialização no Visual Studio, você pode especificar o projeto de inicialização no comando do PowerShell. Para ver a sintaxe de comando, digite o comando `get-help enable-migrations`.

    O `enable-migrations` comando criada automaticamente a primeira migração porque o banco de dados já existe. Uma alternativa é para migrações de criar o banco de dados. Para fazer isso, use **Gerenciador de servidores** ou **SQL Server Object Explorer** para excluir o banco de dados ContosoUniversity antes de habilitar as migrações. Depois de habilitar migrações, crie a primeira migração manualmente, digitando o comando "add-migration InitialCreate". Em seguida, você pode criar o banco de dados, inserindo o comando "update-database".

### <a name="set-up-the-seed-method"></a>Configurar o método de semente

Para este tutorial você adicionará dados fixos, adicionando código para o `Seed` método das migrações do Code First `Configuration` classe. Código migrações primeiro chama o `Seed` método após cada migração.

Uma vez que o `Seed` método é executado após cada migração, dados já existe nas tabelas após a primeira migração. Para lidar com essa situação, você usará o `AddOrUpdate` método para atualizar linhas que já foram inseridas ou inseri-los se eles ainda não existirem. O `AddOrUpdate` método não pode ser a melhor opção para seu cenário. Para obter mais informações, consulte [tome cuidado com o EF 4.3 AddOrUpdate método](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie.

1. Abra o *Configuration.cs* do arquivo e substitua os comentários no `Seed` método com o código a seguir:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. As referências aos `List` tiver linhas curvadas vermelhas sob eles, porque você não tem um `using` instrução ainda para seu namespace. Clique em um das instâncias do `List` e clique em **resolver**e, em seguida, clique em **usando Collections**.

    ![Resolver com a instrução using](preparing-databases/_static/image6.png)

    Essa seleção de menu adiciona o código a seguir para o `using` instruções na parte superior do arquivo.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Pressione CTRL-SHIFT-B para compilar o projeto.

O projeto agora está pronto para implantar o *ContosoUniversity* banco de dados. Depois de implantar o aplicativo, na primeira vez que você executá-lo e navega até uma página que acessa o banco de dados Code First criará o banco de dados e executá-lo `Seed` método.

> [!NOTE]
> Adicionando código para o `Seed` método é uma das muitas maneiras que você pode inserir dados fixa no banco de dados. Uma alternativa é adicionar código para o `Up` e `Down` métodos de cada classe de migração. O `Up` e `Down` métodos contêm código que implementa as alterações do banco de dados. Você verá exemplos na [Implantando uma atualização de banco de dados](deploying-a-database-update.md) tutorial.
> 
> Você também pode escrever código que executa instruções SQL usando o `Sql` método. Por exemplo, se você estivesse adicionando uma coluna de orçamento para a tabela de departamento e quiser inicializar todos os orçamentos de departamento para US $ 1.000,00 como parte de uma migração, você pode adicionar a seguinte linha de código ao `Up` método para que a migração:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Criar scripts para implantação de banco de dados de associação

O aplicativo Contoso University usa a autenticação de formulários e sistema de associação do ASP.NET para autenticar e autorizar usuários. O **atualização créditos** está acessível somente a usuários que estão na função de administrador.

Execute o aplicativo e clique em **cursos**e, em seguida, clique em **atualização créditos**.

![Clique em créditos de atualização](preparing-databases/_static/image7.png)

O **faça logon no** página aparece porque o **atualização créditos** página requer privilégios administrativos.

Insira *admin* como o nome de usuário e *devpwd* como a senha e clique em **fazer logon no**.

![Página de logon](preparing-databases/_static/image8.png)

O **atualização créditos** página será exibida.

![Página de atualização de créditos](preparing-databases/_static/image9.png)

Informações de usuário e a função estão no *ContosoUniversity aspnet* banco de dados que é especificado pelo **DefaultConnection** cadeia de caracteres de conexão no *Web. config* arquivo.

Este banco de dados não é gerenciado pelo Entity Framework Code First, portanto, você não pode usar as migrações para implantá-lo. Você usará o provedor dbDacFx para implantar o esquema de banco de dados, e você vai configurar o perfil de publicação para executar um script que irá inserir dados iniciais para tabelas de banco de dados.

> [!NOTE]
> Um novo sistema de associação do ASP.NET (agora chamado de identidade do ASP.NET) foi introduzido com o Visual Studio 2013. O novo sistema permite que você mantenha o aplicativo e tabelas de associação no mesmo banco de dados, e você pode usar as migrações Code First para implantar ambas. O aplicativo de exemplo usa o sistema de associação ASP.NET anterior, que não pode ser implantado por meio de migrações do Code First. Os procedimentos para implantar esse banco de dados de associação também se aplicam a qualquer outro cenário em que seu aplicativo precisa para implantar um banco de dados do SQL Server que não é criado pelo Entity Framework Code First.


Aqui também, você normalmente não quer os mesmos dados em produção que você tem em desenvolvimento. Quando você implanta um site pela primeira vez, é comum para excluir a maioria ou todas as contas de usuário que criar para teste. Portanto, o projeto baixado tem dois bancos de dados de associação: *ContosoUniversity.mdf aspnet* com usuários de desenvolvimento e *aspnet-ContosoUniversity-Prod.mdf* com usuários de produção. Para este tutorial, os nomes de usuário são os mesmos em ambos os bancos de dados: *admin* e *nonadmin*. Os dois usuários possuírem a senha *devpwd* no banco de dados de desenvolvimento e *prodpwd* no banco de dados de produção.

Você implantará os usuários de desenvolvimento para o ambiente de teste e os usuários de produção para preparação e produção. Para fazer isso você criará dois scripts SQL neste tutorial, um para desenvolvimento e outra para produção e, em tutoriais posteriores, você vai configurar o processo de publicação para executá-los.

> [!NOTE]
> O banco de dados de associação armazena um hash de senhas de conta. Para implantar contas de um computador para outro, você deve garantir que as rotinas de hash não geram hashes de diferentes no servidor de destino do que em um computador de origem. Eles irá gerar os hashes mesmos quando você usa o ASP.NET Universal Providers, desde que você não altere o algoritmo padrão. O algoritmo padrão é HMACSHA256 e é especificado na **validação** atributo da **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** elemento no arquivo Web. config.


Você pode criar scripts de implantação de dados manualmente, usando o SQL Server Management Studio (SSMS) ou usando uma ferramenta de terceiros. O restante deste tutorial mostrará como fazê-lo no SSMS, mas se você não quiser instalar e usar o SSMS, você pode obter os scripts da versão concluída do projeto e vá para a seção em que você armazená-los na pasta da solução.

Para instalar o SSMS, instalá-lo partir [Centro de Download: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) clicando [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) ou [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Se você escolher o errado para seu sistema não conseguir instalar e você pode tentar outro.

(Observe que se trata de um download de 600 megabytes. Pode levar muito tempo para instalar e exigirá uma reinicialização do computador.)

Na primeira página da Central de instalação do SQL Server, clique em **instalação autônoma do novo SQL Server ou adicionar recursos a uma instalação existente**e siga as instruções, aceitando as opções padrão.

### <a name="create-the-development-database-script"></a>Criar o script de banco de dados de desenvolvimento

1. Execute o SSMS.
2. No **conectar ao servidor** caixa de diálogo, digite *(localdb) \v11.0* como o **nome do servidor**, deixe **autenticação** definido como **Autenticação do Windows**e, em seguida, clique em **Connect**.

    ![O SSMS se conectar ao servidor](preparing-databases/_static/image10.png)
3. No **Pesquisador de objetos** janela, expanda **bancos de dados**, clique com botão direito **aspnet ContosoUniversity**, clique em **tarefas**e, em seguida, clique em **Gerar Scripts**.

    ![SSMS gerar Scripts](preparing-databases/_static/image11.png)
4. No **gerar e publicar Scripts** caixa de diálogo, clique em **definir opções de script**.

    Você pode ignorar a **escolher objetos** etapa porque o padrão é **banco de dados inteiro e todos os objetos de banco de dados de Script** e é o que você deseja.
5. Clique em **Avançadas**.

    ![Opções de script de SSMS](preparing-databases/_static/image12.png)
6. No **opções de script avançadas** caixa de diálogo, role para baixo até **tipos de dados para script**e clique no **somente dados** opção na lista suspensa.
7. Alteração **Gerar Script de USE DATABASE** à **falso**. Instruções de uso não são válidas para o banco de dados SQL e não são necessários para a implantação para o SQL Server Express no ambiente de teste.

    ![SSMS Script somente dados, nenhuma instrução USE](preparing-databases/_static/image13.png)
8. Clique em **OK**.
9. No **gerar e publicar Scripts** caixa de diálogo, o **nome do arquivo** caixa especifica onde o script será criado. Altere o caminho para a sua pasta de solução (a pasta que contém seu arquivo ContosoUniversity.sln) e o nome do arquivo para *aspnet-data-dev.sql*.
10. Clique em **próxima** para ir para o **resumo** guia e, em seguida, clique em **próxima** novamente para criar o script.

    ![Script SSMS criado](preparing-databases/_static/image14.png)
11. Clique em **Finalizar**.

### <a name="create-the-production-database-script"></a>Criar o script de banco de dados de produção

Uma vez que você ainda não executou o projeto com o banco de dados de produção, ele não está anexado ainda para a instância de LocalDB. Portanto, você precisa anexar o banco de dados pela primeira vez.

1. No SSMS **Pesquisador de objetos**, clique com botão direito **bancos de dados** e clique em **Attach**.

    ![Anexar do SSMS](preparing-databases/_static/image15.png)
2. No **anexar bancos de dados** caixa de diálogo, clique em **Add** e, em seguida, navegue até o *aspnet-ContosoUniversity-Prod.mdf* de arquivo no *aplicativo\_ Dados* pasta.

     ![SSMS Adicionar arquivo. mdf a ser anexado](preparing-databases/_static/image16.png)
3. Clique em **OK**.
4. Siga o mesmo procedimento que você usou anteriormente para criar um script para o arquivo de produção. Nomeie o arquivo de script *aspnet-data-prod.sql*.

## <a name="summary"></a>Resumo

Ambos os bancos de dados agora estão prontos para ser implantado e você tem dois scripts de implantação de dados na pasta da solução.

![Scripts de implantação de dados](preparing-databases/_static/image17.png)

O tutorial a seguir você define as configurações de projeto que afetam a implantação, e você configurar automatic *Web. config* arquivo transformações para as configurações que devem ser diferentes no aplicativo implantado.

## <a name="more-information"></a>Mais informações

Para obter mais informações sobre o NuGet, consulte [gerenciar bibliotecas de projeto com o NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) e [documentação do NuGet](http://docs.nuget.org/docs/start-here/overview). Se você não quiser usar o NuGet, você precisará aprender como analisar um pacote do NuGet para determinar o que ele faz quando ele está instalado. (Por exemplo, ele pode configurar *Web. config* transformações, configurar scripts do PowerShell para ser executado em tempo de compilação, etc.) Para saber mais sobre como o NuGet funciona, consulte [criar e publicar um pacote](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e [arquivo de configuração e transformações de código fonte](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Próximo](web-config-transformations.md)
