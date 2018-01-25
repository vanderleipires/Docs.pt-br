---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Migrando para o SQL Server - 10 12 | Microsoft Docs'
author: tdykstra
description: "Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: b97834e3e287645151bf927996fde63d93ae8356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Migrando para o SQL Server - 10 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como migrar do SQL Server Compact para o SQL Server. Um motivo, que talvez você queira fazer isso é tirar proveito dos recursos do SQL Server que o SQL Server Compact não dá suporte, como procedimentos armazenados, disparadores, exibições ou replicação. Para obter mais informações sobre as diferenças entre o SQL Server Compact e SQL Server, consulte o [Implantando o SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express versus completo do SQL Server para desenvolvimento

Depois que você decidir atualizar para SQL Server, você talvez queira usar o SQL Server ou SQL Server Express em seus ambientes de desenvolvimento e teste. Além das diferenças no suporte a ferramentas e nos recursos do mecanismo de banco de dados, há diferenças em implementações do provedor do SQL Server Compact e outras versões do SQL Server. Essas diferenças podem causar o mesmo código gerar resultados diferentes. Portanto, se você optar por manter o SQL Server Compact como seu banco de dados de desenvolvimento, você deve testar completamente seu site no SQL Server ou SQL Server Express em um ambiente de teste antes de cada implantação de produção.

Ao contrário do SQL Server Compact, SQL Server Express é essencialmente o mesmo mecanismo de banco de dados e usa o mesmo provedor de .NET como completo do SQL Server. Quando você testar com o SQL Server Express, você pode ter certeza de obter os mesmos resultados, assim como você com o SQL Server. Você pode usar a maioria das mesmas ferramentas de banco de dados com o SQL Server Express que você pode usar com o SQL Server (uma exceção notável sendo [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), e oferece suporte a outros recursos do SQL Server, como procedimentos armazenados, exibições, gatilhos, e a replicação. (Você normalmente precisa usar completo do SQL Server em um site de produção, no entanto. SQL Server Express pode ser executado em um ambiente de hospedagem compartilhado, mas não foi projetado para fazer isso e muitos provedores de hospedagem não oferecem suporte a ele.)

Se você estiver usando o Visual Studio 2012, você normalmente escolher SQL Server Express LocalDB para seu ambiente de desenvolvimento porque esse é o que é instalado por padrão com o Visual Studio. No entanto, o LocalDB não funciona no IIS, para seu ambiente de teste, você precisará usar o SQL Server ou SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>A combinação de bancos de dados versus mantê-las separadas

O aplicativo University Contoso tem dois bancos de dados do SQL Server Compact: o banco de dados de associação (*aspnet.sdf*) e o banco de dados do aplicativo (*School.sdf*). Quando você migra, você pode migrar esses bancos de dados para dois bancos de dados separados ou para um único banco de dados. Você talvez queira combiná-las para facilitar as junções de banco de dados entre o banco de dados do aplicativo e o banco de dados de associação. Seu plano de hospedagem também pode fornecer um motivo para combiná-las. Por exemplo, o provedor de hospedagem pode cobrar mais por vários bancos de dados ou talvez não permitam ainda mais de um banco de dados. Que é o caso com a conta que é usada para este tutorial, o que permite que apenas um único banco de dados SQL de hospedagem Cytanium Lite.

Neste tutorial, você vai migrar dois bancos de dados desta forma:

- Migre para dois bancos de dados de LocalDB no ambiente de desenvolvimento.
- Migre para dois bancos de dados SQL Server Express no ambiente de teste.
- Migre para uma combinação do SQL Server banco de dados completo no ambiente de produção.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalando o SQL Server Express

SQL Server Express é instalado automaticamente por padrão com o Visual Studio 2010, mas, por padrão ele não é instalado com o Visual Studio 2012. Para instalar o SQL Server 2012 Express, clique no link a seguir

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Escolha *x64/ENU/SQLEXPR\_x64\_ENU.exe* ou *ENU/x86/SQLEXPR\_x86\_ENU.exe*e no Assistente de instalação, aceite o padrão Configurações. Para obter mais informações sobre opções de instalação, consulte [instalar o SQL Server 2012 do Assistente de instalação (instalação)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Criando bancos de dados do SQL Server Express para o ambiente de teste

A próxima etapa é criar a associação do ASP.NET e bancos de dados de escola.

Do **exibição** menu, selecione **Server Explorer** (**Pesquisador de objetos de banco de dados** no Visual Web Developer) e, em seguida, clique com botão direito **conexões de dados**e selecione **criar novo servidor de banco de dados SQL**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

No **criar novo servidor de banco de dados SQL** caixa de diálogo, digite ". \SQLExpress" no **nome do servidor** caixa e "aspnet-teste" no **novo nome de banco de dados** caixa e, em seguida, clique em **Okey**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Siga o mesmo procedimento para criar um novo banco de dados de escola do SQL Server Express denominado "Teste de escola".

(Você está anexando "Teste" para esses nomes de banco de dados porque mais tarde, você criará uma instância adicional de cada banco de dados para o ambiente de desenvolvimento, e você precisa ser capaz de diferenciar os dois conjuntos de bancos de dados.)

**Gerenciador de servidores** agora mostra dois novos bancos de dados.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Criando um Script de concessão para novos bancos de dados

Quando o aplicativo é executado no IIS no computador de desenvolvimento, o aplicativo acessa o banco de dados usando as credenciais do pool de aplicativos padrão. No entanto, por padrão, a identidade do pool de aplicativos não tem permissão para abrir os bancos de dados. Assim, você precisa executar um script para conceder essa permissão. Nesta seção, você cria o script que será executado posteriormente para certificar-se de que o aplicativo pode abrir os bancos de dados quando ele é executado no IIS.

A solução *SolutionFiles* pasta que você criou no [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, crie um novo arquivo SQL chamado *Grant.sql*. Copie os seguintes comandos SQL para o arquivo e, em seguida, salve e feche o arquivo:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Esse script foi projetado para trabalhar com o SQL Server 2008 e com as configurações do IIS no Windows 7, conforme elas são especificadas neste tutorial. Se você estiver usando uma versão diferente do SQL Server ou do Windows, ou se você configurar o IIS no computador diferente, as alterações a esse script podem ser necessárias. Para obter mais informações sobre os scripts do SQL Server, consulte [Manuais Online do SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Observação de segurança** esse script fornece db\_permissões de proprietário para o usuário que acessa o banco de dados em tempo de execução, o que é o que você terá no ambiente de produção. Em alguns cenários, convém especificar um usuário que tem um esquema de banco de dados completo atualizar permissões apenas para implantação e especifique para tempo de execução de um usuário diferente que tenha permissões apenas para ler e gravar dados. Para obter mais informações, consulte **revisando as alterações automáticas de Web. config para migrações do Code First** na [implantando para o IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>A configuração de implantação de banco de dados para o ambiente de teste

Em seguida, você configurará o Visual Studio para que ele fará as seguintes tarefas para cada banco de dados:

- Gere um script SQL que cria a estrutura do banco de dados de origem (tabelas, colunas, restrições, etc.) no banco de dados de destino.
- Gere um script SQL que insere os dados do banco de dados de origem para as tabelas no banco de dados de destino.
- Execute os scripts gerados e o script de concessão que você criou, no banco de dados de destino.

Abra o **propriedades do projeto** janela e selecione o **pacote/publicar SQL** guia.

Verifique se **ativo (versão)** ou **versão** está selecionado no **configuração** lista suspensa.

Clique em **habilitar esta página**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

O **pacote/publicar SQL** guia normalmente está desabilitada porque especifica um método de implantação herdado. Na maioria dos cenários, você deve configurar a implantação de banco de dados no **Publicar Web** assistente. Migrando do SQL Server Compact para o SQL Server ou SQL Server Express é um caso especial para o qual esse método é uma boa opção.

Clique em **importação de Web. config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

O Visual Studio procura cadeias de caracteres de conexão no *Web. config* arquivo, localiza um para o banco de dados de associação e outro para o banco de dados School e adiciona uma linha correspondente a cada cadeia de caracteres de conexão no **entradas do banco de dados**  tabela. As cadeias de caracteres de conexão encontrar são para os bancos de dados do SQL Server Compact, e a próxima etapa será configurar como e onde implantar esses bancos de dados.

Insira configurações de implantação de banco de dados no **detalhes de entrada do banco de dados** seção abaixo o **entradas do banco de dados** tabela. As configurações exibidas no **detalhes de entrada do banco de dados** seção pertencem a que linha o **entradas do banco de dados** tabela for selecionada, conforme mostrado na ilustração a seguir.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Definindo configurações de implantação para o banco de dados de associação

Selecione o **implantação DefaultConnection** linha o **entradas do banco de dados** tabela para definir as configurações que se aplicam ao banco de dados de associação.

Em **cadeia de caracteres de Conexão de banco de dados de destino**, insira uma cadeia de caracteres de conexão que aponta para o novo SQL Server Express associação banco de dados. Você pode obter a cadeia de caracteres de conexão que você precisa do **Server Explorer**. Em **Server Explorer**, expanda **conexões de dados** e selecione o **aspnetTest** banco de dados, depois do **propriedades** cópia de janela a **Cadeia de caracteres de Conexão** valor.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

A mesma cadeia de conexão é reproduzida aqui:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copie e cole essa cadeia de caracteres de conexão em **cadeia de caracteres de Conexão de banco de dados de destino** no **pacote/publicar SQL** guia.

Verifique se **extrair dados e/ou esquema de banco de dados existente** está selecionado. Este é o que faz com que os scripts SQL a ser gerado automaticamente e executar no banco de dados de destino.

O **cadeia de caracteres de Conexão para o banco de dados de origem** valor é extraído do *Web. config* arquivo e aponta para o banco de dados do SQL Server Compact do desenvolvimento. Este é o banco de dados de origem que será usado para gerar os scripts que serão executados mais tarde no banco de dados de destino. Desde que você deseja implantar a versão de produção do banco de dados, altere "aspnet Dev.sdf" para "aspnet Prod.sdf".

Alterar **opções de script de banco de dados** de **somente esquema** para **esquema e dados**, uma vez que você deseja copiar os dados (contas de usuário e funções), bem como a estrutura de banco de dados.

Para configurar a implantação para executar os scripts de concessão que você criou anteriormente, você precisa adicioná-los para o **Scripts de banco de dados** seção. Clique em **Adicionar Script**e no **adicionar Scripts de SQL** caixa de diálogo, navegue até a pasta onde você armazenou o script de concessão (essa é a pasta que contém o arquivo de solução). Selecione o arquivo chamado *Grant.sql*e clique em **abrir**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

As configurações para o **implantação DefaultConnection** linha **entradas do banco de dados** agora ser semelhante a ilustração a seguir:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Definindo configurações de implantação para o banco de dados de escola

Em seguida, selecione o **SchoolContext implantação** linha o **entradas do banco de dados** tabela para definir as configurações de implantação para o banco de dados de escola.

Você pode usar o mesmo método que você usou anteriormente para obter a cadeia de caracteres de conexão para o novo banco de dados SQL Server Express. Copie esta cadeia de caracteres de conexão em **cadeia de caracteres de Conexão de banco de dados de destino** no **pacote/publicar SQL** guia.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Verifique se **extrair dados e/ou esquema de banco de dados existente** está selecionado.

O **cadeia de caracteres de Conexão para o banco de dados de origem** valor é extraído do *Web. config* arquivo e aponta para o banco de dados do SQL Server Compact do desenvolvimento. Altere "Dev.sdf escola" para "Prod.sdf de escola" para implantar a versão de produção do banco de dados. (Você nunca criou um arquivo de escola Prod.sdf no aplicativo\_pasta de dados, para que você vai copiar esse arquivo do ambiente de teste para o aplicativo\_pasta de dados na pasta de projeto ContosoUniversity posteriormente.)

Alterar **opções de script de banco de dados** para **esquema e dados**.

Você também deseja executar o script para conceder a leitura e a permissão de gravação para esse banco de dados para a identidade do pool de aplicativos, para adicionar o *Grant.sql* arquivo de script, como você fez para o banco de dados de associação.

Quando você terminar, as configurações o **SchoolContext implantação** linha **entradas do banco de dados** semelhante a ilustração a seguir:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Salvar as alterações para o **pacote/publicar SQL** guia.

Cópia a *escola-Prod.sdf* arquivo o *c:\inetpub\wwwroot\ContosoUniversity\App\_dados* pasta para o *aplicativo\_dados* pasta no o projeto ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Especificando o modo de transação para o Script de concessão

O processo de implantação gera scripts que implantar o esquema de banco de dados e os dados. Por padrão, esses scripts são executados em uma transação. No entanto, os scripts personalizados (como os scripts de concessão) por padrão não são executados em uma transação. Se o processo de implantação misturar modos de transação, você poderá receber um erro de tempo limite quando os scripts são executados durante a implantação. Nesta seção, você pode editar o arquivo de projeto para configurar os scripts personalizados para ser executado em uma transação.

Em **Solution Explorer**, com o botão direito do **ContosoUniversity** do projeto e selecione **descarregar projeto**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Clique com o botão direito no projeto novamente e selecione **ContosoUniversity.csproj editar**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Editor do Visual Studio mostra o conteúdo XML do arquivo do projeto. Observe que há vários `PropertyGroup` elementos. (A imagem, o conteúdo do `PropertyGroup` elementos foram omitidos.)

![Janela do editor de arquivo de projeto](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

O primeiro deles, o que não tem nenhum `Condition` atributo, é para configurações que se aplicam independentemente da configuração de compilação. Um `PropertyGroup` elemento só se aplica a configuração de compilação de depuração (Observe o `Condition` atributo), um só se aplica a configuração de compilação de versão e uma só se aplica a configuração de compilação de teste. Dentro de `PropertyGroup` elemento para a configuração de compilação de versão, você verá um `PublishDatabaseSettings` elemento que contém as configurações que você inseriu **pacote/publicar SQL** guia. Há um `Object` elemento que corresponde a cada um dos scripts de concessão especificada (Observe as duas instâncias de "Grant.sql"). Por padrão, o `Transacted` atributo o `Source` elemento para cada script de concessão é `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Alterar o valor da `Transacted` atributo do `Source` elemento `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Salve e feche o arquivo de projeto e, em seguida, clique com botão direito no projeto no **Solution Explorer** e selecione **recarregar projeto**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Configurando transformações de Web. config para as cadeias de caracteres de Conexão

A conexão cadeias de caracteres para o SQL Express novos bancos de dados que você inseriu **pacote/publicar SQL** guia usados pela implantação da Web somente para atualizar o banco de dados de destino durante a implantação. Você ainda precisará configurar *Web. config* transformações para que as cadeias de caracteres de conexão no implantado *Web. config* arquivo de ponto para novos bancos de dados SQL Server Express. (Quando você usa o **pacote/publicar SQL** guia, você não pode configurar cadeias de caracteres de conexão no perfil de publicação.)

Abra *Web.Test.config* e substitua o `connectionStrings` elemento com o `connectionStrings` elemento no exemplo a seguir. (Certifique-se de que copiar apenas o elemento connectionStrings, não o código ao redor é mostrado aqui para fornecer contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Este código faz com que o `connectionString` e `providerName` atributos de cada `add` elemento a ser substituído no implantado *Web. config* arquivo. Essas cadeias de caracteres de conexão não são idênticas aos que você inseriu no **pacote/publicar SQL** guia. A configuração "MultipleActiveResultSets = True" foi adicionada a eles porque ela é necessária para o Entity Framework e o Universal Providers.

## <a name="installing-sql-server-compact"></a>Instalando o SQL Server Compact

O pacote SqlServerCompact NuGet fornece o banco de dados do SQL Server Compact assemblies de mecanismo para o aplicativo da Contoso University. Mas agora não é o aplicativo, mas implantação da Web que deve ser capaz de ler os bancos de dados do SQL Server Compact, para criar scripts para serem executados nos bancos de dados do SQL Server. Para habilitar a implantação da Web para ler os bancos de dados do SQL Server Compact, instalar o SQL Server Compact no computador de desenvolvimento usando o seguinte link: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Implantando para o ambiente de teste

Para publicar o ambiente de teste, você precisa criar um perfil de publicação que está configurado para usar o **pacote/publicar SQL** guia para o banco de dados de publicação em vez das configurações de banco de dados de perfil de publicação.

Primeiro, exclua o perfil de teste existente.

Em **Solution Explorer**, com o botão direito no projeto ContosoUniversity e clique em **publicar**.

Selecione o **perfil** guia.

Clique em **gerenciar perfis**.

Selecione **teste**, clique em **remover**e, em seguida, clique em **fechar**.

Fechar o **Publicar Web** Assistente para salvar essa alteração.

Em seguida, crie um novo perfil de teste e usá-lo para publicar o projeto.

Em **Solution Explorer**, com o botão direito no projeto ContosoUniversity e clique em **publicar**.

Selecione o **perfil** guia.

Selecione  **&lt;novo... &gt;**  na lista suspensa lista e digite "Test" como o nome do perfil.

No **URL do serviço** , digite *localhost*.

No **Site/aplicativo** , digite *Default Web Site/ContosoUniversity*.

No **URL de destino** , digite `http://localhost/ContosoUniversity/`.

Clique em **Avançar**.

O **configurações** guia avisa que o **pacote/publicar SQL** guia foi configurada e fornece uma oportunidade para substituí-las clicando em Habilitar o banco de dados novas melhorias de publicação. Para essa implantação, você não deseja substituir o **pacote/publicar SQL** guia Configurações, portanto, basta clicar em **próximo**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Uma mensagem de **visualização** guia indica que **nenhum banco de dados selecionados para publicação**, mas isso apenas significa que a publicação de banco de dados não está configurada no perfil de publicação.

Clique em **Publicar**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio implanta o aplicativo e abre o navegador para a home page do site no ambiente de teste. Execute a página instrutores para ver se ele exibe os mesmos dados que você viu anteriormente. Execute o **adicionar alunos** página, adicione um novo aluno e, em seguida, exibir o novo aluno no **alunos** página. Isso verifica que você pode atualizar o banco de dados. Selecione o **atualização créditos** página (você precisará fazer logon) para verificar se o banco de dados de associação foi implantado e você tem acesso a ele.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Criando um banco de dados do SQL Server para o ambiente de produção

Agora que você implantou o ambiente de teste, você está pronto para configurar a implantação na produção. Começar como você fez para o ambiente de teste, criando um banco de dados para implantar. Como você se lembra da visão geral, o plano de hospedagem Cytanium Lite permite apenas um único banco de dados de SQL Server, para que você irá configurar somente um banco de dados, não dois. Todas as tabelas e os dados da associação e os bancos de dados School SQL Server Compact serão implantadas em um banco de dados do SQL Server em produção.

Vá até o painel de controle de Cytanium em [http://panel.cytanium.com](http://panel.cytanium.com). Mantenha o mouse sobre **bancos de dados** e, em seguida, clique em **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

No **SQL Server 2008** , clique em **criar banco de dados**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nome do banco de dados "Escola" e clique em **salvar**. (A página automaticamente adiciona o prefixo "contosou", então, o nome efetivo será "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Na mesma página, clique em **Create User**. Em servidores do Cytanium, em vez de usar segurança integrada do Windows e permitindo que a identidade do pool de aplicativos abrir o banco de dados, você criará um usuário que tem autoridade para abrir o banco de dados. Você adicionará as credenciais do usuário para as cadeias de caracteres de conexão que entram na produção *Web. config* arquivo. Nesta etapa, você cria essas credenciais.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Preencha os campos obrigatórios do **propriedades de usuário do SQL** página:

- Insira "ContosoUniversityUser" como o nome.
- Digite uma senha.
- Selecione **contosouSchool** como o banco de dados padrão.
- Selecione o **contosouSchool** caixa de seleção.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>A configuração de implantação de banco de dados para o ambiente de produção

Agora você está pronto para definir as configurações de implantação de banco de dados no **pacote/publicar SQL** guia, como você fez anteriormente para o ambiente de teste.

Abra o **propriedades do projeto** janela, selecione o **pacote/publicar SQL** guia e verifique se **ativo (versão)** ou **versão** é selecionado no **configuração** lista suspensa.

Quando você definir as configurações de implantação para cada banco de dados, a principal diferença entre o que fazer para ambientes de produção e de teste está em como você configura cadeias de caracteres de conexão. Para o ambiente de teste que você inseriu cadeias de conexão de banco de dados de destino diferente, mas o ambiente de produção a cadeia de caracteres de conexão de destino será o mesmo para ambos os bancos de dados. Isso ocorre porque você está implantando bancos de dados para um banco de dados em produção.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Definindo configurações de implantação para o banco de dados de associação

Para definir as configurações que se aplicam ao banco de dados de associação, selecione o **implantação DefaultConnection** linha o **entradas do banco de dados** tabela.

Em **cadeia de caracteres de Conexão de banco de dados de destino**, insira uma cadeia de caracteres de conexão que aponta para o banco de dados de SQL Server produção novo que você acabou de criar. Você pode obter a cadeia de caracteres de conexão do seu email de boas-vinda. A parte relevante do email contém a cadeia de conexão de exemplo a seguir:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Depois que você substitui as três variáveis, a cadeia de caracteres de conexão que você precisa é semelhante a este exemplo:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copie e cole essa cadeia de caracteres de conexão em **cadeia de caracteres de Conexão de banco de dados de destino** no **pacote/publicar SQL** guia.

Verifique se **extrair dados e/ou esquema de banco de dados existente** ainda selecionado, e o **opções de script de banco de dados** ainda é **esquema e dados**.

No **Scripts de banco de dados** caixa, desmarque a caixa de seleção ao lado de script Grant.sql.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Definindo configurações de implantação para o banco de dados de escola

Em seguida, selecione o **SchoolContext implantação** linha o **entradas do banco de dados** tabela para definir as configurações de banco de dados de escola.

Copie a mesma cadeia de conexão em **cadeia de caracteres de Conexão de banco de dados de destino** que você copiou no campo para o banco de dados de associação.

Verifique se **extrair dados e/ou esquema de banco de dados existente** ainda selecionado, e o **opções de script de banco de dados** ainda é **esquema e dados**.

No **Scripts de banco de dados** caixa, desmarque a caixa de seleção ao lado de script Grant.sql.

Salvar as alterações para o **pacote/publicar SQL** guia.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Transformações de configuração Web. config para cadeias de Conexão para bancos de dados de produção

Em seguida, você irá configurar *Web. config* transformações para que as cadeias de caracteres de conexão no implantado *Web. config* arquivo para apontar para o novo banco de dados de produção. A cadeia de caracteres de conexão que você inseriu no **pacote/publicar SQL** guia de implantação da Web para usar é o mesmo que o aplicativo deve usar, exceto pela adição da `MultipleResultSets` opção.

Abra *Web.Production.config* e substitua o `connectionStrings` elemento com um `connectionStrings` elemento parecido com o exemplo a seguir. (Copiar somente o `connectionStrings` elemento, não as marcas ao redor que são fornecidas para mostrar o contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Às vezes você ver o aviso que informa você sempre criptografe cadeias de caracteres de conexão no *Web. config* arquivo. Isso pode ser apropriado se você foram implantando servidores na rede da sua empresa. No entanto, quando você estiver implantando em um ambiente de hospedagem compartilhado, você confia em práticas de segurança do provedor de hospedagem e não é necessário ou prático criptografar as cadeias de caracteres de conexão.

## <a name="deploying-to-the-production-environment"></a>Implantando o ambiente de produção

Agora você está pronto para implantar na produção. A implantação da Web lerá os bancos de dados do SQL Server Compact no seu projeto *aplicativo\_dados* pasta e recrie todas as suas tabelas e os dados do banco de dados do SQL Server de produção. Para publicar usando o **pacote/publicar na Web** configurações da guia, você precisa criar um novo perfil de publicação para produção.

Primeiro, exclua o perfil de produção existente como o perfil de teste anteriores.

Em **Solution Explorer**, com o botão direito no projeto ContosoUniversity e clique em **publicar**.

Selecione o **perfil** guia.

Clique em **gerenciar perfis**.

Selecione **produção**, clique em **remover**e, em seguida, clique em **fechar**.

Fechar o **Publicar Web** Assistente para salvar essa alteração.

Em seguida, crie um novo perfil de produção e usá-lo para publicar o projeto.

Em **Solution Explorer**, com o botão direito no projeto ContosoUniversity e clique em **publicar**.

Selecione o **perfil** guia.

Clique em **importação**e selecione o arquivo. publishsettings que você baixou anteriormente.

Sobre o **Conexão** , altere o **URL de destino** para a URL correta temporária, que neste exemplo é http://contosouniversity.com.vserver01.cytanium.com.

Renomear o perfil para a produção. (Selecione o **perfil** guia e clique em **gerenciar perfis** para fazer isso).

Fechar o **Publicar Web** Assistente para salvar suas alterações.

Em um aplicativo real em que o banco de dados estava sendo atualizado em produção, você faria duas etapas adicionais agora antes de publicar:

1. Carregar *aplicativo\_offline.htm*, conforme mostrado no [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.
2. Use o **Gerenciador de arquivos** recurso do painel de controle Cytanium para copiar o *aspnet Prod.sdf* e *escola-Prod.sdf* arquivos do site de produção para o *Aplicativo\_dados* pasta do projeto ContosoUniversity. Isso garante que os dados que você estiver implantando o novo banco de dados do SQL Server incluem as atualizações mais recentes feitas pelo seu site de produção.

No **Web um clique em publicar** barra de ferramentas, certifique-se de que o **produção** perfil está selecionado e, em seguida, clique em **publicar**.

Se você carregou *aplicativo\_offline.htm* antes de publicar, você deve usar o **Gerenciador de arquivos** utilitário no painel de controle Cytanium excluir *aplicativo\_off-line.* htm antes de testar. Você também pode ao mesmo tempo excluir o *. sdf* arquivos do *aplicativo\_dados* pasta.

Agora você pode abrir um navegador e vá para a URL do seu site público para testar o aplicativo da mesma maneira que você fez após a implantação para o ambiente de teste.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Alternar para o SQL Server Express LocalDB em desenvolvimento

Como foi explicado na visão geral, ele geralmente é melhor usar o mesmo mecanismo de banco de dados em desenvolvimento que você pode usar em teste e produção. (Lembre-se de que a vantagem de usar o SQL Server Express em desenvolvimento é que o banco de dados funcionam da mesma forma em seus ambientes de desenvolvimento, teste e produção.) Nesta seção você configurará o projeto ContosoUniversity para usar o SQL Server Express LocalDB quando você executa o aplicativo do Visual Studio.

A maneira mais simples para realizar essa migração é permitir que o Code First e o sistema de associação cria dois novos bancos de dados de desenvolvimento para você. Usando esse método para migrar requer três etapas:

1. Altere as cadeias de caracteres de conexão para especificar novos bancos de dados do SQL Express LocalDB.
2. Execute a ferramenta de administração de Site para criar um usuário administrador. Isso cria o banco de dados de associação.
3. Use o comando de atualização de banco de dados de migrações do Code First para criar e propagar o banco de dados do aplicativo.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Atualizar cadeias de caracteres de Conexão no arquivo Web. config

Abra o *Web. config* de arquivo e substitua o `connectionStrings` elemento com o código a seguir:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Criando o banco de dados de associação

Em **Solution Explorer**, selecione o projeto ContosoUniversity e, em seguida, clique em **configuração ASP.NET** no **projeto** menu.

Selecione a guia Segurança.

Clique em **criar ou gerenciar funções**e, em seguida, crie um **administrador** função.

Volte para a guia de segurança.

Clique em **criar usuário**e, em seguida, selecione o **administrador** caixa de seleção e criar um usuário denominado Admin.

Fechar o **ferramenta de administração de Site**.

### <a name="creating-the-school-database"></a>Criando o banco de dados de escola

Abra a janela do Console do Gerenciador de pacotes.

No **projeto padrão** lista suspensa, selecione o projeto ContosoUniversity.DAL.

Insira o seguinte comando:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migrações do Code First aplica-se a migração inicial que cria o banco de dados e, em seguida, aplica-se a migração de AddBirthDate, em seguida, ele executa o método de propagação.

Execute o site pressionando F5 do controle. Como você fez para ambientes de teste e produção, execute o **adicionar alunos** página, adicione um novo aluno e, em seguida, exibir o novo aluno no **alunos** página. Isso verifica se o banco de dados School foi criado e inicializado e que você leu e acesso de gravação a ele.

Selecione o **atualização créditos** página e faça logon para verificar se o banco de dados de associação foi implantado e se você tem acesso a ele. Se você não migrar suas contas de usuário, crie uma conta de administrador e, em seguida, selecione o **atualização créditos** página para verificar se ele funciona.

## <a name="cleaning-up-sql-server-compact-files"></a>Limpando arquivos do SQL Server Compact

Você não precisar mais arquivos e pacotes do NuGet que foram incluídos para oferecer suporte a SQL Server Compact. Se você quiser (essa etapa não é necessária), você poderá limpar os arquivos desnecessários e referências.

Em **Solution Explorer**, excluir o *. sdf* arquivos do *aplicativo\_dados* pasta e o *amd64* e *x86* pastas a partir de *bin* pasta.

Em **Solution Explorer**, clique com botão direito a solução (não um dos projetos) e, em seguida, clique em **gerenciar pacotes NuGet para solução**.

No painel esquerdo do **gerenciar pacotes NuGet** caixa de diálogo, selecione **pacotes instalados**.

Selecione o **EntityFramework.SqlServerCompact** de pacote e clique em **gerenciar**.

No **selecione projetos** caixa de diálogo, ambos os projetos selecionados. Para desinstalar o pacote em ambos os projetos, desmarque as duas caixas de seleção e clique em **Okey**.

Na caixa de diálogo que pergunta se você deseja desinstalar os pacotes dependentes também, clique em não. Um deles é o pacote do Entity Framework que você precisa manter.

Siga o mesmo procedimento para desinstalar o **SqlServerCompact** pacote. (Os pacotes devem ser desinstalados nessa ordem porque o **EntityFramework.SqlServerCompact** depende do pacote de **SqlServerCompact** pacote.)

Agora foi migrado com êxito para o SQL Server Express e o SQL Server completo. A próxima tutorial, você criará outra alteração de banco de dados e você verá como implantar as alterações do banco de dados quando os bancos de dados de teste e produção usam o SQL Server Express e o SQL Server completo.

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[Próximo](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
