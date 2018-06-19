---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: implantar o SQL Server Compact bancos de dados - 2 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 7e2d430bd8e07ed7d97d11a00c61d90beeac005f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890077"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: implantar o SQL Server Compact bancos de dados - 2 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão Geral

Este tutorial mostra como configurar dois bancos de dados do SQL Server Compact e o mecanismo de banco de dados para a implantação.

Para acesso ao banco de dados, o aplicativo Contoso University requer o seguinte software deve ser implantado com o aplicativo porque ele não está incluído no .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (o mecanismo de banco de dados).
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (que permitem que o sistema de associação do ASP.NET usar o SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First com migrações).

A estrutura de banco de dados e alguns (não todos) dos dados nas duas do aplicativo também devem ser implantados bancos de dados. Normalmente, à medida que desenvolve um aplicativo, você insere dados de teste em um banco de dados que você não deseja implantar em um site ao vivo. No entanto, você também pode inserir alguns dados de produção que você deseja implantar. Este tutorial, você configurará o projeto University Contoso para que o software necessário e os dados corretos são incluídos quando você implanta.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact e SQL Server Express

O aplicativo de exemplo usa o SQL Server Compact 4.0. Esse mecanismo de banco de dados é uma opção relativamente nova para sites; versões anteriores do SQL Server Compact não funcionam em um ambiente de hospedagem na web. SQL Server Compact oferece alguns benefícios em comparação comparados o cenário mais comum de desenvolvimento com o SQL Server Express e implantação completa do SQL Server. Dependendo do provedor de hospedagem que você escolher, o SQL Server Compact pode ser mais barato de implantar, porque alguns fornecedores cobram extra para oferecer suporte a um banco de dados completo do SQL Server. Não há nenhum custo adicional para o SQL Server Compact porque você pode implantar o mecanismo de banco de dados como parte do seu aplicativo web.

No entanto, você também deve estar atento a suas limitações. SQL Server Compact não suporta procedimentos armazenados, disparadores, exibições ou replicação. (Para obter uma lista completa dos recursos do SQL Server que não são suportados pelo SQL Server Compact, consulte [as diferenças entre o SQL Server Compact e SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Além disso, algumas das ferramentas que você pode usar para manipular esquemas e dados no SQL Server Express e bancos de dados do SQL Server não funcionam com o SQL Server Compact. Por exemplo, você não pode usar o SQL Server Management Studio ou o SQL Server Data Tools no Visual Studio com bancos de dados do SQL Server Compact. Você tem outras opções para trabalhar com bancos de dados do SQL Server Compact:

- Você pode usar o Gerenciador de servidores no Visual Studio, que oferece funcionalidade de manipulação de banco de dados limitada para o SQL Server Compact.
- Você pode usar o recurso de manipulação de banco de dados do [WebMatrix](https://www.microsoft.com/web/webmatrix/), que tem mais recursos do que o Gerenciador de servidores.
- Use relativamente completa de terceiros ou abrir ferramentas de software, como o [caixa de ferramentas do SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) e [utilitário de script de esquema e dados do SQL Compact](https://github.com/ErikEJ/SqlCeToolbox).
- Você pode gravar e executar seus próprios scripts do DDL (linguagem de definição de dados) para manipular o esquema de banco de dados.

Você pode começar com o SQL Server Compact e, em seguida, atualizar posteriormente conforme suas necessidades. Tutoriais posteriores nesta série mostram como migrar do SQL Server Compact para o SQL Server Express e do SQL Server. No entanto, se você estiver criando um novo aplicativo e espera precisar do SQL Server em um futuro próximo, é provavelmente melhor iniciar com o SQL Server ou SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configurando o mecanismo de banco de dados compacto do SQL Server para implantação

O software necessário para acesso a dados no aplicativo Contoso University foi adicionado ao instalar os seguintes pacotes do NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET universais providers)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Os links apontam para as versões atuais desses pacotes, que pode ser mais recente do que o que é instalado no projeto de starter baixado para este tutorial. Para implantação de um provedor de hospedagem, certifique-se de que você use o Entity Framework 5.0 ou posterior. Versões anteriores de migrações do Code First exigem confiança total e muitos provedores de hospedagem de seu aplicativo será executado em confiança média. Para obter mais informações sobre o nível de confiança média, consulte o [implantando para o IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.

Instalação do pacote NuGet geralmente cuida de tudo o que você precisa para implantar esse software com o aplicativo. Em alguns casos, isso envolve tarefas como alterar o arquivo Web. config e adicionar scripts do PowerShell que são executados sempre que você compile a solução. **Se você deseja adicionar suporte para esses recursos (como o SQL Server Compact e o Entity Framework) sem usar o NuGet, certifique-se de que você sabe que a instalação do pacote NuGet faz para que você possa fazer o mesmo trabalho manualmente.**

Há uma exceção em que o NuGet não seja cuidadoso de tudo o que você precisa fazer para garantir uma implantação bem-sucedida. O pacote SqlServerCompact NuGet adiciona um script de pós-compilação ao seu projeto que copia os assemblies nativo para *x86* e *amd64* subpastas no projeto *bin* No entanto, o script não inclui essas pastas no projeto. Como resultado, Web Deploy não copiará-los para o site de destino, a menos que você incluí-las manualmente no projeto. (Esse comportamento resulta da configuração de implantação padrão; outra opção, que você não usar esses tutoriais, é alterar a configuração que controla esse comportamento. É a configuração que você pode alterar **apenas os arquivos necessários para executar o aplicativo** em **itens para implantar** no **pacote/publicar na Web** guia do **projeto Propriedades** janela. A alteração dessa configuração não é geralmente recomendável porque pode resultar na implantação de vários arquivos mais no ambiente de produção que é necessária. Para obter mais informações sobre as alternativas, consulte o [configurando propriedades de projeto](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) tutorial.)

Compilar o projeto e, em seguida, em **Solution Explorer** clique **Mostrar todos os arquivos** se você ainda não tiver feito isso. Talvez você também precise clicar **atualizar**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Expanda o **bin** pasta para ver o **amd64** e **x86** pastas e, em seguida, selecione as pastas, clique com botão direito e selecione **incluir no projeto**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Alteram os ícones de pasta para mostrar que a pasta foi incluída no projeto.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configuração de migrações do Code First para implantação de banco de dados de aplicativo

Quando você implanta um aplicativo de banco de dados, normalmente você não simplesmente implantar seu banco de dados de desenvolvimento com todos os dados nela para produção, porque a maioria dos dados nele há provavelmente apenas para fins de teste. Por exemplo, os nomes dos alunos em um banco de dados de teste são fictícios. Por outro lado, você geralmente não é possível implantar apenas a estrutura de banco de dados sem dados nela em todos os. Alguns dos dados em seu banco de dados de teste podem ser dados reais e devem existir quando os usuários começam a usar o aplicativo. Por exemplo, o banco de dados pode ter uma tabela que contém os valores de classificação válido ou nomes de departamento real.

Para simular um cenário comum, você vai configurar um método de propagação de migrações primeiro código que insere o banco de dados somente os dados que você deseja ser existe em produção. Esse método de propagação não insere dados de teste porque ele será executado em produção depois que o Code First cria o banco de dados em produção.

Em versões anteriores do Code First antes do lançamento migrações, era comum para métodos de propagação para inserir dados de teste também, porque cada alteração de modelo durante o desenvolvimento de banco de dados completamente excluído e recriado do zero. Com migrações do Code First, dados de teste são mantidos após as alterações do banco de dados, incluindo dados de teste no método de propagação não é necessário. O projeto que você baixou usa o método de pré-migrações de inclusão de todos os dados no método de propagação de uma classe de inicializador. Neste tutorial você desabilitar a classe de inicializador e habilitará as migrações. Em seguida, atualizará o método de propagação na classe de configuração de migrações para que ele insira apenas os dados que você deseja ser inserida na produção.

O diagrama a seguir ilustra o esquema do banco de dados do aplicativo:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Para esses tutoriais, você assumirá que o `Student` e `Enrollment` as tabelas devem estar vazias quando o site é implantado pela primeira vez. As outras tabelas contêm dados que precisa ser pré-carregados quando o aplicativo for executado.

Desde que você usará migrações do Code First, você não precisa usar o **DropCreateDatabaseIfModelChanges** inicializador Code First. É o código para este inicializador no arquivo SchoolInitializer.cs no projeto ContosoUniversity.DAL. Uma configuração de **appSettings** elemento do arquivo Web. config faz com que este inicializador ser executado sempre que o aplicativo tentar acessar o banco de dados pela primeira vez:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Abra o arquivo Web. config do aplicativo e remover o elemento que especifica a classe Code First do inicializador de elemento appSettings. O elemento appSettings agora tem esta aparência:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Outra maneira de especificar um inicializador de classe é feito chamando `Database.SetInitializer` no `Application_Start` método o *global. asax* arquivo. Se você estiver habilitando migrações em um projeto que usa esse método para especificar o inicializador, remova essa linha de código.


Em seguida, habilita migrações do Code First.

A primeira etapa é certificar-se de que o projeto de ContosoUniversity é definido como o projeto de inicialização. Em **Solution Explorer**, com o botão direito no projeto ContosoUniversity e selecione **definir como projeto de inicialização**. Migrações do Code First examinará o projeto de inicialização para localizar a cadeia de caracteres de conexão do banco de dados.

Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote** e **Package Manager Console**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Na parte superior do **Package Manager Console** janela Selecionar ContosoUniversity.DAL como o projeto padrão e, em seguida, at a `PM>` prompt insira "enable-migrations".

![Habilitar migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Este comando cria um *Configuration.cs* arquivo em uma nova *migrações* pasta no projeto ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Você selecionou o projeto DAL como o comando "enable-migrations" deve ser executado no projeto que contém a classe de contexto Code First. Quando essa classe está em um projeto de biblioteca de classe, migrações do Code First procura a cadeia de caracteres de conexão de banco de dados no projeto de inicialização para a solução. Na solução ContosoUniversity, o projeto web foi definido como o projeto de inicialização. (Se você não quiser designar o projeto que tem a cadeia de caracteres de conexão como o projeto de inicialização no Visual Studio, você pode especificar o projeto de inicialização no comando do PowerShell. Para ver a sintaxe de comando para o comando enable-migrations, você pode inserir os comando "get-help enable-migrations".)

Abra o arquivo Configuration.cs e substitua os comentários a `Seed` método com o código a seguir:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

As referências a `List` ter linhas curvadas vermelhas sob eles, porque você não tem um `using` instrução ainda para seu namespace. Clique em um das instâncias do `List` e clique em **resolver**e, em seguida, clique em **usando Generic**.

![Resolver com a instrução using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Esta seleção de menu adiciona o código a seguir para o `using` instruções perto da parte superior do arquivo.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Adicionando código para o `Seed` método é uma das várias maneiras que você pode inserir dados fixa no banco de dados. Uma alternativa é adicionar código para o `Up` e `Down` métodos de cada classe de migração. O `Up` e `Down` métodos contêm código que implementa as alterações do banco de dados. Você verá exemplos no [implantar uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.
> 
> Você também pode escrever código que executa instruções SQL usando o `Sql` método. Por exemplo, se você estivesse adicionando uma coluna de orçamento para a tabela de departamento e quiser inicializar todos os orçamentos de departamento para $ 1.000,00 como parte de uma migração, você pode adicionar a seguinte linha de código para o `Up` método para migração:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Este exemplo mostrado para este tutorial usa o `AddOrUpdate` método o `Seed` método as migrações do Code First `Configuration` classe. Code First Migrations chama o `Seed` método após cada migração e este método atualiza linhas que já foram inseridas ou insere-los se eles ainda não existirem. O `AddOrUpdate` método não pode ser a melhor opção para seu cenário. Para obter mais informações, consulte [tome cuidado com EF 4.3 AddOrUpdate método](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie Lerman.


Pressione CTRL-SHIFT-B para compilar o projeto.

A próxima etapa é criar um `DbMigration` classe para a migração inicial. Você deseja que a migração para criar um novo banco de dados, você precisará excluir o banco de dados que já existe. Bancos de dados do SQL Server Compact estão contidos em *. sdf* arquivos de *aplicativo\_dados* pasta. Em **Solution Explorer**, expanda *aplicativo\_dados* no projeto ContosoUniversity para ver dois bancos de dados do SQL Server Compact, que é representado por *. sdf*arquivos.

Clique com botão direito do *School.sdf* de arquivo e clique em **excluir**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

No **Package Manager Console** janela, digite o comando "Adicionar-migração inicial" para criar a migração inicial e nomeie-a como "Initial".

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrações do Code First cria outro arquivo de classe no *migrações* pasta e essa classe contém o código que cria o esquema de banco de dados.

No **Package Manager Console**, digite o comando "update-database" para criar o banco de dados e executar o **semente** método.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Se você receber um erro que indica uma tabela já existe e não pode ser criada, é provável que o aplicativo é executado depois que você excluir o banco de dados e antes da execução `update-database`. Excluir in that case, o *School.sdf* novamente e repita a `update-database` comando.)

Execute o aplicativo. Agora a página de alunos está vazia, mas a página instrutores contém instrutores. Este é o que você obterá em produção depois que você implantar o aplicativo.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

O projeto agora está pronto para implantar o *escola* banco de dados.

## <a name="creating-a-membership-database-for-deployment"></a>Criando um banco de dados de associação para implantação

O aplicativo Contoso University usa a autenticação de formulários e sistema de associação do ASP.NET para autenticar e autorizar usuários. Uma das suas páginas é acessível somente para administradores. Para ver essa página, execute o aplicativo e selecione **atualização créditos** no menu de atalho em **cursos**. O aplicativo exibe o **logon** página, porque apenas os administradores estão autorizados a usar o **atualização créditos** página.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Faça logon como "admin" usando a senha "Pas$ w0rd" (Observe o número zero no lugar da letra "m" em "w0rd"). Depois que você fizer logon, o **atualização créditos** página é exibida.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Quando você implanta um site pela primeira vez, é comum para excluir a maioria ou todas as contas de usuário que você criar para teste. Nesse caso, você implantará uma conta de administrador e não há contas de usuário. Em vez de excluir manualmente as contas de teste, você criará um novo banco de dados membros que tenha somente a conta de usuário de um administrador que você precisa em produção.

> [!NOTE]
> O banco de dados de associação armazena um hash de senhas de conta. Para implantar as contas de um computador para outro, você deve garantir que as rotinas de hash não geram hashes diferentes no servidor de destino do que no computador de origem. Eles irá gerar os hashes mesmo quando você usa o ASP.NET Universal Providers, desde que você não altere o algoritmo padrão. O algoritmo padrão é HMACSHA256 e é especificado no **validação** atributo o **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** elemento no arquivo Web. config.


O banco de dados de associação não é mantido pela migrações do Code First e não há nenhum inicializador automática que propaga o banco de dados com contas de teste (como há para o banco de dados School). Portanto, para manter os dados de teste disponíveis você vai fazer uma cópia do banco de dados de teste antes de criar um novo.

Em **Gerenciador de soluções**, renomear o *aspnet.sdf* arquivo o *aplicativo\_dados* pasta para *aspnet Dev.sdf*. (Não fazer uma cópia, basta renomeá-lo, você criará um novo banco de dados em um momento.)

Em **Solution Explorer**, certifique-se de que o projeto da web (ContosoUniversity, não ContosoUniversity.DAL) está selecionado. Em seguida, no **projeto** menu, selecione **configuração ASP.NET** para executar o **ferramenta de administração de Site**(WAT).

Selecione o **segurança** guia.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Clique em **criar ou gerenciar funções** e adicione um **administrador** função.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Navegue de volta para o **segurança** , clique em **Create User**, e adicionar o usuário "admin" como um administrador. Antes de clicar no **Create User** botão o **Create User** página, certifique-se de que você selecione o **administrador** caixa de seleção. A senha usada neste tutorial é "Pas$ w0rd", e você pode inserir qualquer endereço de email.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Feche o navegador. Em **Solution Explorer**, clique no botão Atualizar para ver o novo *aspnet.sdf* arquivo.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Clique com botão direito **aspnet.sdf** e selecione **incluir no projeto**.

## <a name="distinguishing-development-from-production-databases"></a>Distinção de desenvolvimento de bancos de dados de produção

Nesta seção, você renomeará os bancos de dados para que as versões de desenvolvimento são escola Dev.sdf e Dev.sdf aspnet e as versões de produção são Prod.sdf escola e Prod.sdf aspnet. Isso não é necessário, mas fazer assim ajuda a impedir a obtenção de versões de teste e produção de bancos de dados confusos.

Em **Solution Explorer**, clique em **atualizar** e expanda o aplicativo\_pasta de dados para ver o banco de dados School criado anteriormente; clique duas vezes e selecionar **incluir no projeto** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Renomear *aspnet.sdf* para *Prod.sdf aspnet*.

Renomear *School.sdf* para *escola-Dev.sdf*.

Quando você executar o aplicativo no Visual Studio, você não quiser usar o *-Prod* versões dos arquivos de banco de dados, que você deseja usar *- Dev* versões. Portanto, você precisa alterar as cadeias de caracteres de conexão no arquivo Web. config para que eles apontem para o *- Dev* versões dos bancos de dados. (Você não criou um arquivo de escola Prod.sdf, mas que é Okey porque o Code First criará esse banco de dados no tempo de produção primeiro que executar seu aplicativo existe.)

Abra o arquivo do aplicativo Web. config e localize as cadeias de caracteres de conexão:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Altere "aspnet.sdf" para "aspnet Dev.sdf" e altere "School.sdf" para "Dev.sdf escola":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

O mecanismo de banco de dados do SQL Server Compact e bancos de dados agora estão prontos para ser implantado. O tutorial a seguir você configura automático *Web. config* arquivo transformações para as configurações que devem ser diferentes nos ambientes de desenvolvimento, teste e produção. (Entre as configurações que devem ser alteradas são as cadeias de caracteres de conexão, mas você configurará as alterações posteriormente quando você cria um perfil de publicação).

## <a name="more-information"></a>Mais Informações

Para obter mais informações sobre o NuGet, consulte [gerenciar bibliotecas de projeto com o NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) e [NuGet documentação](http://docs.nuget.org/docs/start-here/overview). Se você não quiser usar o NuGet, você precisará saber como analisar um pacote do NuGet para determinar o que fazer quando ele está instalado. (Por exemplo, ele pode configurar *Web. config* transformações, configurar scripts do PowerShell para executar em tempo de compilação, etc.) Para saber mais sobre como funciona o NuGet, consulte especialmente [criar e publicar um pacote](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e [arquivo de configuração e transformações de código fonte](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
