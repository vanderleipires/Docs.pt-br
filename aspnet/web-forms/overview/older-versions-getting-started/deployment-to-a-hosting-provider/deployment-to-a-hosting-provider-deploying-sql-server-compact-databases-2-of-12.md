---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando SQL Server Compact bancos de dados – 2 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9b3d47c3c8fe5f0b37f1d45e19341df3f91a5bb0
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911182"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando SQL Server Compact bancos de dados – 2 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como configurar os dois bancos de dados do SQL Server Compact e o mecanismo de banco de dados para a implantação.

Para acesso de banco de dados, o aplicativo Contoso University exige o seguinte software deve ser implantado com o aplicativo porque ele não está incluído no .NET Framework:

- [O SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (o mecanismo de banco de dados).
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (que permitem que o sistema de associação do ASP.NET usar o SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First com migrações).

A estrutura de banco de dados e alguns (não todos) dos dados em dois do aplicativo também devem ser implantados a bancos de dados. Normalmente, à medida que você desenvolve um aplicativo, você insere dados de teste em um banco de dados que você não deseja implantar em um site ativo. No entanto, você também pode inserir alguns dados de produção que você deseja implantar. Neste tutorial, você vai configurar o projeto do Contoso University para que o software necessário e os dados corretos sejam incluídos quando você implanta.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact versus SQL Server Express

O aplicativo de exemplo usa o SQL Server Compact 4.0. Esse mecanismo de banco de dados é uma opção relativamente nova para sites; versões anteriores do SQL Server Compact não funcionam em um ambiente de hospedagem na web. O SQL Server Compact oferece alguns benefícios em comparação comparados o cenário mais comum de desenvolvimento com o SQL Server Express e implantação para o SQL Server completo. Dependendo do provedor de hospedagem escolhido, o SQL Server Compact pode ser mais barato de implantar, porque alguns provedores cobram extra para oferecer suporte a um banco de dados completo do SQL Server. Não há nenhum custo adicional para o SQL Server Compact como é possível implantar o próprio mecanismo de banco de dados como parte do seu aplicativo web.

No entanto, você também deve estar ciente das limitações. O SQL Server Compact não dar suporte a procedimentos armazenados, disparadores, exibições ou replicação. (Para obter uma lista completa dos recursos do SQL Server que não são suportados pelo SQL Server Compact, consulte [diferenças entre o SQL Server Compact e SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Além disso, algumas das ferramentas que você pode usar para manipular esquemas e dados no SQL Server Express e bancos de dados do SQL Server não funcionam com o SQL Server Compact. Por exemplo, você não pode usar o SQL Server Management Studio ou o SQL Server Data Tools no Visual Studio com bancos de dados do SQL Server Compact. Você tem outras opções para trabalhar com bancos de dados do SQL Server Compact:

- Você pode usar o Gerenciador de servidores no Visual Studio, que oferece funcionalidade de manipulação de banco de dados limitados para o SQL Server Compact.
- Você pode usar o recurso de manipulação de banco de dados do [WebMatrix](https://www.microsoft.com/web/webmatrix/), que tem mais recursos do que o Gerenciador de servidores.
- Use terceiros relativamente completa ou abrir ferramentas de código-fonte, como o [SQL Server Compact Toolbox](https://github.com/ErikEJ/SqlCeToolbox) e [utilitário de script de esquema e dados do SQL Compact](https://github.com/ErikEJ/SqlCeToolbox).
- Você pode escrever e executar seus próprios scripts do DDL (linguagem de definição de dados) para manipular o esquema de banco de dados.

Você pode começar com o SQL Server Compact e, em seguida, atualizar mais tarde, conforme suas necessidades evoluem. Tutoriais mais adiante nesta série mostram como migrar do SQL Server Compact para o SQL Server Express e do SQL Server. No entanto, se você estiver criando um novo aplicativo e espera precisar do SQL Server em um futuro próximo, é provavelmente melhor começar com o SQL Server ou SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configuração do mecanismo de banco de dados compacto do SQL Server para a implantação

O software necessário para acesso a dados no aplicativo Contoso University foi adicionado ao instalar os seguintes pacotes NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET universais providers)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Os links apontam para as versões atuais desses pacotes, que pode ser mais recente do que o que é instalado no projeto de início que você baixou para este tutorial. Para implantação em um provedor de hospedagem, certifique-se de que você usa o Entity Framework 5.0 ou posterior. Versões anteriores de migrações do Code First exigem confiança total e, em muitos provedores de hospedagem, o aplicativo será executado em confiança média. Para obter mais informações sobre o nível de confiança média, consulte o [implantando no IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

Instalação do pacote NuGet geralmente cuida de tudo o que você precisa para implantar esse software com o aplicativo. Em alguns casos, isso envolve tarefas como alterar o arquivo Web. config e adicionar scripts do PowerShell que são executados sempre que você compilar a solução. **Se você quiser adicionar suporte para qualquer um desses recursos (como o SQL Server Compact e Entity Framework) sem usar o NuGet, certifique-se de que você sabe o que a instalação do pacote NuGet faz para que você possa fazer o mesmo trabalho manualmente.**

Há uma exceção em que o NuGet não tomar cuidado de tudo o que você precisa fazer para garantir uma implantação bem-sucedida. O pacote do SqlServerCompact NuGet adiciona um script de pós-compilação ao seu projeto que copia os assemblies nativos *x86* e *amd64* subpastas sob o projeto *bin* pasta, mas o script não inclui essas pastas no projeto. Como resultado, implantação da Web não copiará-los para o site de destino, a menos que você incluí-los manualmente no projeto. (Esse comportamento resulta da configuração de implantação padrão; outra opção, que você não vai usar esses tutoriais, é alterar a configuração que controla esse comportamento. É a configuração que você pode alterar **somente os arquivos necessários para executar o aplicativo** sob **itens para implantar** no **pacote/Publicar Web** guia do **projeto Propriedades** janela. A alteração dessa configuração não é geralmente recomendável porque pode resultar na implantação de vários arquivos de mais ao ambiente de produção, que é necessária. Para obter mais informações sobre as alternativas, consulte o [configurando propriedades de projeto](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) tutorial.)

Compile o projeto e, em seguida, na **Gerenciador de soluções** clique em **Mostrar todos os arquivos** se você ainda não tiver feito isso. Talvez você também precise clicar **Refresh**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Expanda o **bin** pasta para ver as **amd64** e **x86** pastas e, em seguida, selecione a essas pastas, clique com botão direito e selecione **Include in Project**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Alteram os ícones de pasta para mostrar que a pasta foi incluída no projeto.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configurando as migrações do Code First para implantação de banco de dados de aplicativo

Quando você implanta um banco de dados do aplicativo, normalmente você não simplesmente implanta seu banco de dados de desenvolvimento com todos os dados contidos nela para produção, porque muitos dos dados em que ele é provavelmente existe apenas para fins de teste. Por exemplo, os nomes de alunos em um banco de dados de teste são fictícios. Por outro lado, você geralmente não é possível implantar apenas a estrutura de banco de dados sem dados nele em todos os. Alguns dos dados no banco de dados de teste podem ser dados reais e devem estar lá quando os usuários começam a usar o aplicativo. Por exemplo, seu banco de dados pode ter uma tabela que contém os valores de classificação válidos ou nomes de departamento real.

Para simular esse cenário comum, você vai configurar um método de semente de migrações primeiro código que insere o banco de dados somente os dados que você deseja estar lá em produção. Esse método de propagação não insere dados de teste porque ele será executado em produção depois que o Code First cria o banco de dados em produção.

Em versões anteriores do Code First antes do lançamento, as migrações era comum para métodos de propagação para inserir dados de teste também, porque cada alteração de modelo durante o desenvolvimento de banco de dados tinha completamente ser excluído e recriado do zero. Com migrações do Code First, dados de teste são mantidos após alterações de banco de dados, portanto, incluindo dados de teste no método de propagação não é necessária. O projeto que você baixou usa o método de pré-migrações de inclusão de todos os dados no método de semente de uma classe de inicializador. Neste tutorial, você desativar a classe de inicializador e habilitar migrações. Em seguida, você atualizará o método de propagação na classe de configuração de migrações para que ele insira apenas os dados que você deseja a ser inserido na produção.

O diagrama a seguir ilustra o esquema do banco de dados do aplicativo:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Para esses tutoriais, você assumirá que o `Student` e `Enrollment` tabelas devem estar vazias quando o site é implantado pela primeira vez. As outras tabelas contêm dados que deve ser pré-carregado quando o aplicativo for lançado.

Uma vez que você usará migrações do Code First, você não precisa usar o **DropCreateDatabaseIfModelChanges** inicializador do Code First. O código para esse inicializador é no arquivo no projeto ContosoUniversity.DAL SchoolInitializer.cs. Uma configuração na **appSettings** elemento do arquivo Web. config faz com que esse inicializador ser executado sempre que o aplicativo tenta acessar o banco de dados pela primeira vez:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Abra o arquivo Web. config do aplicativo e remova o elemento que especifica a classe de inicializador Code First do elemento appSettings. O elemento de appSettings agora fica assim:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Outra maneira de especificar uma classe de inicializador é fazê-lo chamando `Database.SetInitializer` no `Application_Start` método na *global. asax* arquivo. Se você estiver habilitando as migrações em um projeto que usa esse método para especificar o inicializador, remova essa linha de código.

Em seguida, habilite migrações do Code First.

A primeira etapa é certificar-se de que o projeto ContosoUniversity é definido como o projeto de inicialização. Na **Gerenciador de soluções**, clique com botão direito no projeto ContosoUniversity e selecione **definir como projeto de inicialização**. Migrações do Code First examinará o projeto de inicialização para localizar a cadeia de caracteres de conexão de banco de dados.

Dos **ferramentas** menu, clique em **Gerenciador de pacotes NuGet** e, em seguida, **Package Manager Console**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Na parte superior a **Package Manager Console** janela Selecione ContosoUniversity.DAL como o projeto padrão e, em seguida, at a `PM>` prompt insira "enable-migrations".

![Habilitar migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Este comando cria uma *Configuration.cs* arquivo em uma nova *migrações* pasta do projeto ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Você selecionou o projeto da DAL, como o comando "enable-migrations" deve ser executado no projeto que contém a classe de contexto de Code First. Quando essa classe está em um projeto de biblioteca de classes, as migrações Code First procura a cadeia de caracteres de conexão de banco de dados no projeto de inicialização para a solução. Na solução ContosoUniversity, o projeto da web foi definido como o projeto de inicialização. (Se você não quiser designar o projeto que tem a cadeia de caracteres de conexão como o projeto de inicialização no Visual Studio, você pode especificar o projeto de inicialização no comando do PowerShell. Para ver a sintaxe de comando para o comando enable-migrations, você pode inserir os comando "get-help enable-migrations".)

Abra o arquivo Configuration.cs e substitua os comentários no `Seed` método com o código a seguir:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

As referências aos `List` tiver linhas curvadas vermelhas sob eles, porque você não tem um `using` instrução ainda para seu namespace. Clique em um das instâncias do `List` e clique em **resolver**e, em seguida, clique em **usando Collections**.

![Resolver com a instrução using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Essa seleção de menu adiciona o código a seguir para o `using` instruções na parte superior do arquivo.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Adicionando código para o `Seed` método é uma das muitas maneiras que você pode inserir dados fixa no banco de dados. Uma alternativa é adicionar código para o `Up` e `Down` métodos de cada classe de migração. O `Up` e `Down` métodos contêm código que implementa as alterações do banco de dados. Você verá exemplos na [Implantando uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.
> 
> Você também pode escrever código que executa instruções SQL usando o `Sql` método. Por exemplo, se você estivesse adicionando uma coluna de orçamento para a tabela de departamento e quiser inicializar todos os orçamentos de departamento para US $ 1.000,00 como parte de uma migração, você pode adicionar a seguinte linha de código ao `Up` método para que a migração:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Este exemplo mostrado para este tutorial usa o `AddOrUpdate` método na `Seed` método das migrações do Code First `Configuration` classe. Código migrações primeiro chama o `Seed` método após cada migração e esse método atualiza as linhas que já foram inseridas ou insere-os se ainda não existirem. O `AddOrUpdate` método não pode ser a melhor opção para seu cenário. Para obter mais informações, consulte [tome cuidado com o EF 4.3 AddOrUpdate método](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie.


Pressione CTRL-SHIFT-B para compilar o projeto.

A próxima etapa é criar um `DbMigration` classe para a migração inicial. Você deseja que essa migração para criar um novo banco de dados, portanto, você precisa excluir o banco de dados que já existe. Bancos de dados do SQL Server Compact estão contidos em *sdf* arquivos na *App\_dados* pasta. Na **Gerenciador de soluções**, expanda *App\_Data* no projeto ContosoUniversity para ver dois bancos de dados do SQL Server Compact, que é representado por *. sdf*arquivos.

Clique com botão direito do *School.sdf* do arquivo e clique em **excluir**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

No **Package Manager Console** janela, digite o comando "add-migration Initial" para criar a migração inicial e nomeie-a como "Inicial".

![Adicionar-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrações do Code First cria outro arquivo de classe na *migrações* pasta e essa classe contém o código que cria o esquema de banco de dados.

No **Package Manager Console**, insira o comando "update-database" para criar o banco de dados e executar o **semente** método.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Se você receber um erro que indica uma tabela já existe e não pode ser criada, provavelmente é porque você executou o aplicativo depois que você excluiu o banco de dados e antes da execução `update-database`. Nesse caso, exclua o *School.sdf* novamente e repita a `update-database` comando.)

Execute o aplicativo. Agora a página alunos está vazia, mas a página instrutores contém instrutores. Isso é o que você obterá em produção depois que você implantar o aplicativo.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

O projeto agora está pronto para implantar o *School* banco de dados.

## <a name="creating-a-membership-database-for-deployment"></a>Criando um banco de dados de associação para implantação

O aplicativo Contoso University usa a autenticação de formulários e sistema de associação do ASP.NET para autenticar e autorizar usuários. Uma das suas páginas é acessível somente para administradores. Para ver essa página, execute o aplicativo e selecione **atualização créditos** no menu do submenu em **cursos**. O aplicativo exibe a **fazer logon** página, porque somente os administradores estão autorizados a usar o **atualização créditos** página.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Faça logon como "admin" usando a senha "Pas$ w0rd" (Observe o número zero no lugar da letra "m" em "w0rd"). Depois que você fizer logon, o **atualização créditos** página é exibida.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Quando você implanta um site pela primeira vez, é comum para excluir a maioria ou todas as contas de usuário que criar para teste. Nesse caso, você implantará uma conta de administrador e nenhuma conta de usuário. Em vez de excluir manualmente as contas de teste, você criará um novo banco de dados associação que tenha apenas a conta de usuário de um administrador que você precisa em produção.

> [!NOTE]
> O banco de dados de associação armazena um hash de senhas de conta. Para implantar contas de um computador para outro, você deve garantir que as rotinas de hash não geram hashes de diferentes no servidor de destino do que em um computador de origem. Eles irá gerar os hashes mesmos quando você usa o ASP.NET Universal Providers, desde que você não altere o algoritmo padrão. O algoritmo padrão é HMACSHA256 e é especificado na **validação** atributo da **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** elemento no arquivo Web. config.


O banco de dados de associação não é mantido por migrações do Code First, e não há nenhum automático inicializador que propaga o banco de dados com as contas de teste (que existe para o banco de dados de estudante). Portanto, para manter dados de teste disponíveis você vai fazer uma cópia do banco de dados de teste antes de criar um novo.

No **Gerenciador de soluções**, renomeie o *aspnet.sdf* de arquivo no *aplicativo\_dados* pasta a ser *Dev.sdf aspnet*. (Não faça uma cópia, apenas renomeá-la, você criará um novo banco de dados em alguns instantes.)

Na **Gerenciador de soluções**, certifique-se de que o projeto da web (ContosoUniversity, não ContosoUniversity.DAL) está selecionado. Em seguida, no **Project** menu, selecione **configuração do ASP.NET** para executar o **ferramenta Web Site Administration**(WAT).

Selecione o **segurança** guia.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Clique em **criar ou gerenciar funções** e adicione um **administrador** função.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Navegue de volta para o **segurança** , clique em **Create User**, e adicione o usuário "admin" como um administrador. Antes de clicar na **criar usuário** botão a **Create User** página, certifique-se de que você selecione o **administrador** caixa de seleção. A senha usada neste tutorial é "$w0rd Pas", e você pode inserir qualquer endereço de email.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Feche o navegador. Na **Gerenciador de soluções**, clique no botão Atualizar para ver a nova *aspnet.sdf* arquivo.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Clique com botão direito **aspnet.sdf** e selecione **Include in Project**.

## <a name="distinguishing-development-from-production-databases"></a>Distinção entre o desenvolvimento de bancos de dados de produção

Nesta seção, você irá renomear os bancos de dados para que as versões de desenvolvimento são Dev.sdf escola e aspnet Dev.sdf e as versões de produção são Prod.sdf escola e Prod.sdf aspnet. Isso não é necessário, mas fazer então ajudará a impedir a obtenção de versões de teste e produção dos bancos de dados confuso.

Na **Gerenciador de soluções**, clique em **atualize** e expanda o aplicativo\_pasta de dados para ver o banco de dados de escola que você criou anteriormente; clique duas vezes e selecione **incluir no projeto** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Renomeie *aspnet.sdf* à *aspnet Prod.sdf*.

Renomeie *School.sdf* à *School-Dev.sdf*.

Quando você executa o aplicativo no Visual Studio, você não quiser usar o *-Prod* as versões dos arquivos de banco de dados, que você deseja usar *- Dev* versões. Portanto você precisa alterar as cadeias de caracteres de conexão no arquivo Web. config para que eles apontem para o *- Dev* versões dos bancos de dados. (Você ainda não criou um arquivo de School Prod.sdf, mas o que é Okey porque o Code First criará esse banco de dados no momento da produção primeiro que executar o seu aplicativo lá).

Abra o arquivo Web. config do aplicativo e localizar as cadeias de caracteres de conexão:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Altere "aspnet.sdf" para "aspnet-Dev.sdf" e altere "School.sdf" para "Dev.sdf escola":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

O mecanismo de banco de dados do SQL Server Compact e ambos os bancos de dados agora estão prontos para ser implantado. O tutorial a seguir você vai configurar automatic *Web. config* transformações para as configurações que devem ser diferentes nos ambientes de desenvolvimento, teste e produção de arquivo. (Entre as configurações que devem ser alteradas são as cadeias de caracteres de conexão, mas você vai configurar essas alterações mais tarde quando você cria um perfil de publicação).

## <a name="more-information"></a>Mais informações

Para obter mais informações sobre o NuGet, consulte [gerenciar bibliotecas de projeto com o NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) e [documentação do NuGet](http://docs.nuget.org/docs/start-here/overview). Se você não quiser usar o NuGet, você precisará aprender como analisar um pacote do NuGet para determinar o que ele faz quando ele está instalado. (Por exemplo, ele pode configurar *Web. config* transformações, configurar scripts do PowerShell para ser executado em tempo de compilação, etc.) Para saber mais sobre como o NuGet funciona, consulte especialmente [criar e publicar um pacote](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e [arquivo de configuração e transformações de código fonte](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
