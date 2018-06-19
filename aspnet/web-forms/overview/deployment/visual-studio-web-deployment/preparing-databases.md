---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Preparando para implantação de banco de dados | Microsoft Docs'
author: tdykstra
description: Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 61392af322de454687da522055005a670b34f510
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889141"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Implantação de Web do ASP.NET usando o Visual Studio: Preparando para implantação de banco de dados
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão Geral

Este tutorial mostra como obter o projeto pronto para a implantação de banco de dados. A estrutura de banco de dados e alguns (não todos) dos dados nas duas do aplicativo bancos de dados devem ser implantados para teste, preparação e ambientes de produção.

Normalmente, à medida que desenvolve um aplicativo, você insere dados de teste em um banco de dados que você não deseja implantar em um site ao vivo. No entanto, você também pode ter alguns dados de produção que você deseja implantar. Neste tutorial, você configurar o projeto University Contoso e preparar scripts SQL para que os dados corretos são incluídos quando você implanta.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

O aplicativo de exemplo usa o SQL Server Express LocalDB. SQL Server Express é a edição gratuita do SQL Server. Ele costuma ser usado durante o desenvolvimento porque ele se baseia o mesmo mecanismo de banco de dados completo das versões do SQL Server. Você pode testar com o SQL Server Express e ter certeza de que o aplicativo se comporte o mesmo em produção, com algumas exceções para os recursos que variam entre edições do SQL Server.

O LocalDB é um modo especial de execução do SQL Server Express que permite que você trabalhe com bancos de dados como *. mdf* arquivos. Normalmente, os arquivos de banco de dados LocalDB são mantidos no *aplicativo\_dados* pasta de um projeto da web. O recurso de instância de usuário do SQL Server Express também permite que você trabalhe com *. mdf* arquivos, mas o recurso de instância de usuário são preteridos; portanto, o LocalDB é recomendado para trabalhar com *. mdf* arquivos.

Normalmente o SQL Server Express não é usado para aplicativos da web de produção. LocalDB em particular não é recomendável para uso em produção com um aplicativo web porque ele não foi projetado para trabalhar com o IIS.

No Visual Studio 2012, o LocalDB é instalado por padrão com o Visual Studio. No Visual Studio 2010 e versões anteriores, o SQL Server Express (sem LocalDB) é instalado por padrão com o Visual Studio; Isto é porque você instalado como um dos pré-requisitos em [primeiro tutorial nesta série](introduction.md).

Para obter mais informações sobre as edições do SQL Server, incluindo LocalDB, consulte os seguintes recursos [trabalhando com bancos de dados do SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework e Universal Providers

Para acesso ao banco de dados, o aplicativo Contoso University requer o seguinte software deve ser implantado com o aplicativo porque ele não está incluído no .NET Framework:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (permite que o sistema de associação do ASP.NET usar o banco de dados do SQL Azure)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Como este software é incluído em pacotes do NuGet, o projeto já está definido para que os assemblies necessários são implantados com o projeto. (Os links apontam para as versões atuais desses pacotes, que pode ser mais recente do que o que é instalado no projeto de starter baixado para este tutorial.)

Se você estiver implantando em um provedor de hospedagem de terceiros em vez do Azure, certifique-se de que você use o Entity Framework 5.0 ou posterior. Versões anteriores de migrações do Code First exigem confiança total, e a maioria dos provedores de hospedagem executará seu aplicativo no nível de confiança média. Para obter mais informações sobre o nível de confiança média, consulte o [implantar para o IIS como um ambiente de teste](deploying-to-iis.md) tutorial.

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurar as migrações do Code First para implantação de banco de dados de aplicativo

O banco de dados de aplicativo Contoso University é gerenciado pelo Code First e implantará usando migrações do Code First. Para obter uma visão geral da implantação de banco de dados usando migrações do Code First, consulte [primeiro tutorial nesta série](introduction.md).

Quando você implanta um aplicativo de banco de dados, normalmente você não simplesmente implantar seu banco de dados de desenvolvimento com todos os dados nela para produção, porque a maioria dos dados nele há provavelmente apenas para fins de teste. Por exemplo, os nomes dos alunos em um banco de dados de teste são fictícios. Por outro lado, você geralmente não é possível implantar apenas a estrutura de banco de dados sem dados nela em todos os. Alguns dos dados em seu banco de dados de teste podem ser dados reais e devem existir quando os usuários começam a usar o aplicativo. Por exemplo, o banco de dados pode ter uma tabela que contém os valores de classificação válido ou nomes de departamento real.

Para simular um cenário comum, você configurará um migrações do Code First `Seed` método que insere o banco de dados somente os dados que você deseja ser existe em produção. Isso `Seed` método não deve inserir os dados de teste porque ele será executado em produção depois que o Code First cria o banco de dados em produção.

Em versões anteriores do Code First antes do lançamento migrações, era comum `Seed` métodos para inserir dados de teste também, porque cada alteração de modelo durante o desenvolvimento de banco de dados completamente excluído e recriado do zero. Com migrações do Code First, teste os dados são mantidos após as alterações do banco de dados, portanto, incluindo dados de teste no `Seed` método não é necessário. O projeto que você baixou usa o método de todos os dados, incluindo o `Seed` método de uma classe de inicializador. Neste tutorial, você vai desabilitar essa classe de inicializador e `enable Migrations. Then you'll update the `semente ' método na configuração de migrações de classe para que ele insira apenas os dados que você deseja ser inserida na produção.

O diagrama a seguir ilustra o esquema do banco de dados do aplicativo:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Para esses tutoriais, você assumirá que o `Student` e `Enrollment` as tabelas devem estar vazias quando o site é implantado pela primeira vez. As outras tabelas contêm dados que precisa ser pré-carregados quando o aplicativo for executado.

### <a name="disable-the-initializer"></a>Desabilitar o inicializador

Como você usará migrações do Code First, você não precisa usar o `DropCreateDatabaseIfModelChanges` inicializador Code First. O código para este inicializador está no *SchoolInitializer.cs* arquivo no projeto ContosoUniversity.DAL. Uma configuração no `appSettings` elemento o *Web. config* arquivo faz com que este inicializador ser executado sempre que o aplicativo tentar acessar o banco de dados pela primeira vez:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Abra o aplicativo *Web. config* de arquivo e remova ou comente a `add` elemento que especifica a classe de inicializador Code First. O `appSettings` elemento agora esta aparência:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Outra maneira de especificar um inicializador de classe é feito chamando `Database.SetInitializer` no `Application_Start` método o *global. asax* arquivo. Se você estiver habilitando migrações em um projeto que usa esse método para especificar o inicializador, remova essa linha de código.


> [!NOTE]
> Se você estiver usando o Visual Studio 2013, adicionar as seguintes etapas entre as etapas 2 e 3: insira PMC (a) em "pacote de atualização entityframework-versão 6.1.1" para obter a versão atual do EF. Em seguida, compilação (b) o projeto para obter uma lista de erros de compilação e corrigi-los. Excluir usando as instruções para namespaces que já não existem, com o botão direito e clique em resolver para adicionar instruções using onde forem necessários e altere ocorrências de System.Data.EntityState para System.Data.Entity.EntityState.


### <a name="enable-code-first-migrations"></a>Habilitar migrações do Code First

1. Certifique-se de que o projeto de ContosoUniversity (não ContosoUniversity.DAL) é definido como o projeto de inicialização. Em **Solution Explorer**, com o botão direito no projeto ContosoUniversity e selecione **definir como projeto de inicialização**. Migrações do Code First examinará o projeto de inicialização para localizar a cadeia de caracteres de conexão do banco de dados.
2. Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote** (ou **NuGet Package Manager**) e, em seguida, **Package Manager Console**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Na parte superior do **Package Manager Console** janela Selecionar ContosoUniversity.DAL como o projeto padrão e, em seguida, at a `PM>` prompt insira "enable-migrations".

    ![comando enable-migrations](preparing-databases/_static/image4.png)

    (Se você receber uma narração erro o *enable-migrations* comando não for reconhecido, digite o comando *EntityFramework do pacote de atualização-reinstalar* e tente novamente.)

    Este comando cria um *migrações* pasta no projeto ContosoUniversity.DAL e ele coloca na pasta dois arquivos: um *Configuration.cs* arquivo que você pode usar para configurar as migrações e um *InitialCreate.cs* arquivo para a migração primeiro que cria o banco de dados.

    ![Pasta de migrações](preparing-databases/_static/image5.png)

    Você selecionou o projeto DAL no **projeto padrão** lista suspensa do **Package Manager Console** porque o `enable-migrations` comando deve ser executado no projeto que contém o Code First classe de contexto. Quando essa classe está em um projeto de biblioteca de classe, migrações do Code First procura a cadeia de caracteres de conexão de banco de dados no projeto de inicialização para a solução. Na solução ContosoUniversity, o projeto web foi definido como o projeto de inicialização. Se você não quiser designar o projeto que tem a cadeia de caracteres de conexão como o projeto de inicialização no Visual Studio, você pode especificar o projeto de inicialização no comando do PowerShell. Para ver a sintaxe de comando, digite o comando `get-help enable-migrations`.

    O `enable-migrations` comando criado automaticamente a primeira migração porque o banco de dados já existe. Uma alternativa é ter as migrações de criar o banco de dados. Para fazer isso, use **Server Explorer** ou **Pesquisador de objetos do SQL Server** para excluir o banco de dados ContosoUniversity antes de habilitar migrações. Depois de habilitar migrações, crie a primeira migração manualmente digitando o comando "migração adicionar InitialCreate". Você pode criar o banco de dados digitando o comando "update-database".

### <a name="set-up-the-seed-method"></a>Configurar o método de propagação

Para este tutorial, você adicionará o dados fixos, adicionando o código para o `Seed` método as migrações do Code First `Configuration` classe. Code First Migrations chama o `Seed` método após cada migração.

Desde o `Seed` método é executado após a migração de cada, houver dados nas tabelas após a migração primeiro. Para lidar com essa situação, você usará o `AddOrUpdate` para atualizar linhas que já foram inseridas ou inseri-los se eles ainda não existirem. O `AddOrUpdate` método não pode ser a melhor opção para seu cenário. Para obter mais informações, consulte [tome cuidado com EF 4.3 AddOrUpdate método](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie Lerman.

1. Abra o *Configuration.cs* de arquivo e substitua os comentários no `Seed` método com o código a seguir:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. As referências a `List` ter linhas curvadas vermelhas sob eles, porque você não tem um `using` instrução ainda para seu namespace. Clique em um das instâncias do `List` e clique em **resolver**e, em seguida, clique em **usando Generic**.

    ![Resolver com a instrução using](preparing-databases/_static/image6.png)

    Esta seleção de menu adiciona o código a seguir para o `using` instruções perto da parte superior do arquivo.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Pressione CTRL-SHIFT-B para compilar o projeto.

O projeto agora está pronto para implantar o *ContosoUniversity* banco de dados. Depois de implantar o aplicativo, na primeira vez que você executá-lo e navegue até uma página que acessa o banco de dados Code First criará o banco de dados e executar este `Seed` método.

> [!NOTE]
> Adicionando código para o `Seed` método é uma das várias maneiras que você pode inserir dados fixa no banco de dados. Uma alternativa é adicionar código para o `Up` e `Down` métodos de cada classe de migração. O `Up` e `Down` métodos contêm código que implementa as alterações do banco de dados. Você verá exemplos no [implantar uma atualização de banco de dados](deploying-a-database-update.md) tutorial.
> 
> Você também pode escrever código que executa instruções SQL usando o `Sql` método. Por exemplo, se você estivesse adicionando uma coluna de orçamento para a tabela de departamento e quiser inicializar todos os orçamentos de departamento para $ 1.000,00 como parte de uma migração, você pode adicionar a seguinte linha de código para o `Up` método para migração:
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>Criar scripts para implantação de banco de dados de associação

O aplicativo Contoso University usa a autenticação de formulários e sistema de associação do ASP.NET para autenticar e autorizar usuários. O **atualização créditos** está acessível somente a usuários que estão na função de administrador.

Execute o aplicativo e clique em **cursos**e, em seguida, clique em **atualização créditos**.

![Clique em créditos de atualização](preparing-databases/_static/image7.png)

O **login** página aparece porque o **créditos de atualização** página requer privilégios administrativos.

Digite *admin* como o nome de usuário e *devpwd* como a senha e clique em **login**.

![Página de logon](preparing-databases/_static/image8.png)

O **atualização créditos** página será exibida.

![Página de atualização de créditos](preparing-databases/_static/image9.png)

Informações de usuário e a função estão no *aspnet ContosoUniversity* banco de dados que é especificado pelo **DefaultConnection** cadeia de caracteres de conexão no *Web. config* arquivo.

Este banco de dados não é gerenciado pelo Entity Framework Code First, portanto você não pode usar as migrações para implantá-lo. Você usará o provedor dbDacFx para implantar o esquema de banco de dados, e você vai configurar o perfil de publicação para executar um script que irá inserir os dados iniciais para tabelas de banco de dados.

> [!NOTE]
> Um novo sistema de associação do ASP.NET (agora chamado de identidade do ASP.NET) foi introduzido com o Visual Studio 2013. O novo sistema permite que você mantenha o aplicativo e tabelas de associação no mesmo banco de dados, e você pode usar as migrações do Code First para implantar. O aplicativo de exemplo usa o sistema de associação ASP.NET anterior, que não pode ser implantado usando migrações do Code First. Os procedimentos para implantar esse banco de dados de associação também aplicam para qualquer cenário em que seu aplicativo precisa para implantar um banco de dados do SQL Server que não é criado pelo Entity Framework Code First.


Aqui, você normalmente não quer os mesmos dados em produção que você tem em desenvolvimento. Quando você implanta um site pela primeira vez, é comum para excluir a maioria ou todas as contas de usuário que você criar para teste. Portanto, o projeto baixado tem dois bancos de dados de associação: *aspnet ContosoUniversity.mdf* com usuários de desenvolvimento e *aspnet-ContosoUniversity-Prod.mdf* com usuários de produção. Para este tutorial, os nomes de usuário são os mesmos em ambos os bancos de dados: *admin* e *nonadmin*. Ambos os usuários têm a senha *devpwd* no banco de dados de desenvolvimento e *prodpwd* no banco de dados de produção.

Você implantará os usuários de desenvolvimento para o ambiente de teste e os usuários de produção para preparação e produção. Para fazer isso você criará dois scripts SQL neste tutorial, um para o desenvolvimento e outro para produção e tutoriais subsequentes, você configurará o processo de publicação para executá-los.

> [!NOTE]
> O banco de dados de associação armazena um hash de senhas de conta. Para implantar as contas de um computador para outro, você deve garantir que as rotinas de hash não geram hashes diferentes no servidor de destino do que no computador de origem. Eles irá gerar os hashes mesmo quando você usa o ASP.NET Universal Providers, desde que você não altere o algoritmo padrão. O algoritmo padrão é HMACSHA256 e é especificado no **validação** atributo o **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** elemento no arquivo Web. config.


Você pode criar scripts de implantação de dados manualmente, usando o SQL Server Management Studio (SSMS), ou usando uma ferramenta de terceiros. Este restante deste tutorial mostrará como fazer isso no SSMS, mas se você não quiser instalar e usar o SSMS, você pode obter os scripts da versão do projeto concluído e ignorar a seção onde você armazená-los na pasta da solução.

Para instalar o SSMS, instale-o de [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) clicando [ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) ou [ ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Se você escolher a errado para o seu sistema haverá falha na instalação e tente outro.

(Observe que este é um download de 600 megabytes. Pode levar muito tempo para instalar e exigirá uma reinicialização do computador.)

Na primeira página da Central de instalação do SQL Server, clique em **instalação autônoma do novo SQL Server ou adicionar recursos a uma instalação existente**e siga as instruções, aceite as opções padrão.

### <a name="create-the-development-database-script"></a>Criar o script de banco de dados de desenvolvimento

1. Execute o SSMS.
2. No **conectar ao servidor** caixa de diálogo, digite *(localdb) \v11.0* como o **nome do servidor**, deixe **autenticação** definida como **a autenticação do Windows**e, em seguida, clique em **conectar**.

    ![SSMS se conectar ao servidor](preparing-databases/_static/image10.png)
3. No **Pesquisador de objetos** janela, expanda **bancos de dados**, clique com botão direito **aspnet ContosoUniversity**, clique em **tarefas**e, em seguida, clique em **Gerar Scripts**.

    ![SSMS gerar Scripts](preparing-databases/_static/image11.png)
4. No **gerar e publicar Scripts** caixa de diálogo, clique em **definir opções de script**.

    Você pode ignorar o **escolher objetos** etapa porque o padrão é **banco de dados inteiro e todos os objetos de banco de dados de Script** e que é o que você deseja.
5. Clique em **Avançadas**.

    ![Opções de script de SSMS](preparing-databases/_static/image12.png)
6. No **opções de script avançadas** caixa de diálogo, role para baixo para **tipos de dados para script**e clique no **somente dados** opção na lista suspensa.
7. Alterar **banco de dados de uso de Script** para **False**. Instruções de uso não são válido para o banco de dados do SQL Azure e não são necessários para a implantação do SQL Server Express no ambiente de teste.

    ![SSMS Script somente dados, nenhuma instrução USE](preparing-databases/_static/image13.png)
8. Clique em **OK**.
9. No **gerar e publicar Scripts** caixa de diálogo, o **nome de arquivo** Especifica onde o script será criado. Altere o caminho para a pasta de solução (a pasta que contém o arquivo ContosoUniversity.sln) e o nome do arquivo para *aspnet de dados de dev.sql*.
10. Clique em **próximo** para ir para o **resumo** guia e, em seguida, clique em **próximo** novamente para criar o script.

    ![Script SSMS criado](preparing-databases/_static/image14.png)
11. Clique em **Finalizar**.

### <a name="create-the-production-database-script"></a>Criar o script de banco de dados de produção

Desde que você não executar o projeto com o banco de dados de produção, ele não está conectado ainda para a instância de LocalDB. Portanto, você precisa anexar o banco de dados pela primeira vez.

1. No SSMS **Pesquisador de objetos**, clique com botão direito **bancos de dados** e clique em **Attach**.

    ![Anexar SSMS](preparing-databases/_static/image15.png)
2. No **anexar bancos de dados** caixa de diálogo, clique em **adicionar** e, em seguida, navegue até o *aspnet-ContosoUniversity-Prod.mdf* arquivo o *aplicativo\_ Dados* pasta.

     ![SSMS Adicionar arquivo. mdf para anexar](preparing-databases/_static/image16.png)
3. Clique em **OK**.
4. Siga o mesmo procedimento usado anteriormente para criar um script para o arquivo de produção. Nomeie o arquivo de script *aspnet de dados de prod.sql*.

## <a name="summary"></a>Resumo

Os bancos de dados agora estão prontos para ser implantado e você tiver dois scripts de implantação de dados em sua pasta de solução.

![Scripts de implantação de dados](preparing-databases/_static/image17.png)

O tutorial a seguir você define as configurações de projeto que afetam a implantação e configurar automático *Web. config* arquivo transformações para as configurações que devem ser diferentes em um aplicativo implantado.

## <a name="more-information"></a>Mais Informações

Para obter mais informações sobre o NuGet, consulte [gerenciar bibliotecas de projeto com o NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) e [NuGet documentação](http://docs.nuget.org/docs/start-here/overview). Se você não quiser usar o NuGet, você precisará saber como analisar um pacote do NuGet para determinar o que fazer quando ele está instalado. (Por exemplo, ele pode configurar *Web. config* transformações, configurar scripts do PowerShell para executar em tempo de compilação, etc.) Para saber mais sobre como funciona o NuGet, consulte [criar e publicar um pacote](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e [arquivo de configuração e transformações de código fonte](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Próximo](web-config-transformations.md)
