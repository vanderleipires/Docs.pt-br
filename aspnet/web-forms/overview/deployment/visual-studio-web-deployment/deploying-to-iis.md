---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Implantando para teste | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: ce361d2826733d5ecb9005713993e5ecf7f11860
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834054"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Implantação de Web do ASP.NET usando o Visual Studio: Implantando para teste
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como implantar um aplicativo da web ASP.NET no IIS no computador local.

Quando você desenvolve um aplicativo, geralmente você testa executá-lo no Visual Studio. Por padrão, os projetos de aplicativos web no Visual Studio 2012 usam o IIS Express como servidor web de desenvolvimento. O IIS Express se comporta como o IIS completo que o Visual Studio Development Server (também conhecido como Cassini), que usa o Visual Studio 2010 por padrão. Mas nenhum servidor web de desenvolvimento funciona exatamente como o IIS. Como resultado, é possível que um aplicativo será executado corretamente quando você testá-lo no Visual Studio, mas falha quando ele é implantado no IIS.

Você pode testar seu aplicativo de maneira mais confiável das seguintes maneiras:

1. Implante o aplicativo ao IIS no computador de desenvolvimento usando o mesmo processo que você usará posteriormente para implantá-lo em seu ambiente de produção. Você pode configurar o Visual Studio para usar o IIS quando você executar um projeto da web, mas isso não seria testar seu processo de implantação. Esse método valida seu processo de implantação além de validar que o aplicativo será executado corretamente no IIS.
2. Implante o aplicativo em um ambiente de teste é quase idêntico ao seu ambiente de produção. Uma vez que o ambiente de produção para esses tutoriais é aplicativos Web no serviço de aplicativo do Azure, o ambiente de teste ideal é um aplicativo web adicional criado no serviço de aplicativo do Azure. Você usaria esse segundo aplicativo web somente para teste, mas ele seria configurado da mesma forma que o aplicativo web de produção.

Opção 2 é a maneira mais confiável para testar e, se você fizer isso, você não precisa necessariamente à opção 1. No entanto, se você estiver implantando em uma opção de provedor hospedagem terceirizados 2 pode não ser viável ou poderá ser cara, portanto, esta série de tutoriais mostra os dois métodos. Orientação para a opção 2 é fornecida na [implantando no ambiente de produção](deploying-to-production.md) tutorial.

Para obter mais informações sobre como usar os servidores web no Visual Studio, consulte [servidores Web no Visual Studio para projetos Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="install-iis"></a>Instalar o IIS

Para implantar em IIS no computador de desenvolvimento, você deve ter o IIS e implantação da Web instalado. A implantação da Web é instalada por padrão com o Visual Studio, mas o IIS não está incluído na configuração padrão Windows 8 ou Windows 7. Se você já tiver instalado o IIS e o pool de aplicativos padrão já está definido para o .NET 4, vá para [a próxima seção](#sqlexpress).

1. Usando o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) é a maneira preferencial para instalar o IIS e a implantação da Web, porque o Web Platform Installer instala uma configuração recomendada para o IIS e instala automaticamente os pré-requisitos para o IIS e da Web Implante se necessário.

    Para executar o Web Platform Installer para instalar o IIS e implantação da Web, use o link a seguir. Se você já tiver instalado o IIS, implantação da Web ou qualquer um de seus componentes obrigatórios, o Web Platform Installer instala apenas o que está faltando.

   - [Instalar o IIS e implantação da Web usando WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     Você verá mensagens indicando que o IIS 7 será instalado. A links funciona para o IIS 8 no Windows 8, mas para o Windows 8 Certifique-se de que o ASP.NET 4.5 está instalado, executando as seguintes etapas:

   - Abra **painel de controle**, **programas e recursos**, **ativar ou desativar recursos do Windows ativar**.
   - Expandir **serviços de informações da Internet**, **serviços da World Wide Web**, e **recursos de desenvolvimento de aplicativo**.
   - Certifique-se de que **ASP.NET 4.5** está selecionado.

      ![Selecione ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Depois de instalar o IIS, execute **o Gerenciador do IIS** para certificar-se de que o .NET Framework versão 4 é atribuído ao pool de aplicativos padrão.

1. Pressione WINDOWS + R para abrir o **executar** caixa de diálogo.

    (Ou no Windows 8 insira "run" **inicie** página ou no Windows 7, selecione **execute** do **iniciar** menu. Se **executados** não está no **iniciar** menu, clique com botão direito a barra de tarefas, clique em **propriedades**, selecione o **Menu Iniciar** , clique em **Personalizar**e selecione **executar comando**.)
2. Insira "inetmgr" e, em seguida, clique em **Okey**.
3. No **conexões** painel, expanda o nó do servidor e selecione **Pools de aplicativos**. No **Pools de aplicativos** painel, se **DefaultAppPool** é atribuído para o .NET framework versão 4, como mostra a ilustração a seguir, pule para a próxima seção.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Se você vir apenas dois pools de aplicativos e os dois são definidos para o .NET Framework 2.0, você precisa instalar o ASP.NET 4 no IIS.

    Para o Windows 8, consulte as instruções anteriormente na seção para garantir que o ASP.NET 4.5 está instalado, ou consulte [neste artigo da KB](https://support.microsoft.com/kb/2736284). Para o Windows 7, abra uma janela de prompt de comando clicando **Prompt de comando** em que o Windows **inicie** menu e selecionando **executar como administrador**. Em seguida, execute [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar o ASP.NET 4 no IIS, usando os comandos a seguir. (Em sistemas de 32 bits, substitua "Framework64" com "Estrutura".)

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Este comando cria novos pools de aplicativos para o .NET Framework 4, mas o pool de aplicativos padrão ainda será definido como 2.0. Você estará Implantando um aplicativo que tem como alvo o .NET 4 para esse pool de aplicativos, portanto, você precisa alterar o pool de aplicativos para o .NET 4.
5. Se você fechou **o Gerenciador do IIS**, execute-o novamente, expanda o nó do servidor e clique em **Pools de aplicativos** para exibir o **Pools de aplicativos** painel novamente.
6. No **Pools de aplicativos** painel, clique em **DefaultAppPool**e, em seguida, no **ações** painel clique **configurações básicas**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. No **Editar Pool de aplicativos** caixa de diálogo alteração **versão do .NET Framework** para **v4.0.30319 do .NET Framework** e clique em **Okey**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

O IIS agora está pronto para publicar um aplicativo web nele, mas antes de fazer isso você precisa criar os bancos de dados que será usada no ambiente de teste.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Instalar o SQL Server Express

LocalDB não foi projetado para funcionar no IIS, portanto, para o seu ambiente de teste, você precisa ter o SQL Server Express instalado. Se você estiver usando o Visual Studio 2010 do SQL Server Express já está instalado por padrão. Se você estiver usando o Visual Studio 2012, você precisa instalá-lo.

Para instalar o SQL Server Express, instale-o de [Centro de Download: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) clicando [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) ou [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Se você escolher o errado para seu sistema não conseguir instalar e você pode tentar outro.

Na primeira página da Central de instalação do SQL Server, clique em **instalação autônoma do novo SQL Server ou adicionar recursos a uma instalação existente**e siga as instruções, aceitando as opções padrão. No Assistente de instalação, aceite as configurações padrão. Para obter mais informações sobre opções de instalação, consulte [instalar o SQL Server 2012 do Assistente de instalação (instalação)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Criar bancos de dados SQL Server Express para o ambiente de teste

O aplicativo Contoso University tem dois bancos de dados: o banco de dados de associação e o banco de dados do aplicativo. Você pode implantar esses bancos de dados para dois bancos de dados separados ou para um banco de dados. Talvez você queira combiná-los a fim de facilitar junções de banco de dados entre seu banco de dados do aplicativo e seu banco de dados de associação. Se você estiver implantando em um provedor de hospedagem de terceiros, o seu plano de hospedagem também pode fornecer um motivo para combiná-los. Por exemplo, o provedor de hospedagem pode cobrar mais para vários bancos de dados ou até mesmo não pode permitir mais de um banco de dados.

Neste tutorial, você implantará os dois bancos de dados no ambiente de teste e um banco de dados em ambientes de preparo e produção.

Dos **modo de exibição** menu no Visual Studio, selecione **Gerenciador de servidores** (**Database Explorer** no Visual Web Developer) e, em seguida, clique com botão direito **conexões de dados**  e selecione **Create New SQL Server Database**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

No **Create New SQL Server Database** caixa de diálogo, digite ". \SQLExpress" no **nome do servidor** caixa e "aspnet-ContosoUniversity" no **novo nome de banco de dados** caixa, em seguida, Clique em **Okey**.

![Criar ContosoUniversity aspnet](deploying-to-iis/_static/image9.png)

Siga o mesmo procedimento para criar um novo banco de dados School do SQL Server Express chamado "ContosoUniversity".

**Gerenciador de servidores** agora mostra dois novos bancos de dados.

![Novos bancos de dados no Gerenciador de servidores](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Criar um script de concessão para novos bancos de dados

Quando o aplicativo é executado no IIS no computador de desenvolvimento, o aplicativo acessa o banco de dados usando credenciais do pool de aplicativos padrão. No entanto, por padrão, a identidade do pool de aplicativos não tem permissão para abrir os bancos de dados. Portanto, você precisa executar um script para conceder essa permissão. Nesta seção, você criará o script que será executado posteriormente para certificar-se de que o aplicativo pode abrir os bancos de dados quando ele é executado no IIS.

A solução (não um dos projetos) com o botão direito e, em seguida, clique em **Adicionar Novo Item**e, em seguida, crie um novo **arquivo SQL** denominada *Grant.sql*. Copie os seguintes comandos SQL para o arquivo e, em seguida, salve e feche o arquivo:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Este script foi desenvolvido para funcionar com o SQL Server Express 2012 e com as configurações do IIS no Windows 8 ou Windows 7, conforme elas são especificadas neste tutorial. Se você estiver usando uma versão diferente do SQL Server ou do Windows, ou se você configurar o IIS no computador de forma diferente, as alterações a esse script podem ser necessárias. Para obter mais informações sobre scripts do SQL Server, consulte [Manuais Online do SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Observação de segurança** este script dá db\_permissões de proprietário para o usuário que acessa o banco de dados em tempo de execução, o que é o que você terá no ambiente de produção. Em alguns cenários, você talvez queira especificar um usuário que tem o esquema de banco de dados completo permissões apenas para a implantação de atualização e especificar para o tempo de execução de um usuário diferente que tenha permissões apenas para ler e gravar dados. Para obter mais informações, consulte [revisar as alterações da Web. config automática para migrações do Code First](#reviewingmigrations) posteriormente neste tutorial.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Execute o script de concessão no banco de dados de aplicativo

Você pode configurar o perfil de publicação para executar o script de concessão no banco de dados de associação durante a implantação, pois essa implantação de banco de dados usa o provedor dbDacFx. Você não pode executar scripts durante a implantação de migrações do Code First, que é como você está implantando o banco de dados do aplicativo. Portanto, é necessário que executar manualmente o script antes da implantação no banco de dados do aplicativo.

1. No Visual Studio, abra o *Grant.sql* arquivo que você criou anteriormente.
2. Clique em **Conectar**. 

    ![Botão conectar](deploying-to-iis/_static/image11.png)
3. No **conectar ao servidor** caixa de diálogo, digite *. \SQLExpress* como o **nome do servidor**e, em seguida, clique em **Connect**.
4. Na lista suspensa de banco de dados, selecione **ContosoUniversity**e, em seguida, clique em **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

A identidade do pool de aplicativos padrão agora tem permissões suficientes no banco de dados de aplicativo para migrações do Code First criar as tabelas de banco de dados quando o aplicativo é executado.

## <a name="publish-to-iis"></a>Publicar no IIS

Há várias maneiras que você pode implantar em IIS usando o Visual Studio e a implantação da Web:

- Use o Visual Studio, um clique para publicar.
- Publica a partir da linha de comando.
- Criar uma *pacote de implantação* e instalá-lo usando a UI do Gerenciador do IIS. O pacote de implantação consiste em uma *. zip* arquivo que contém todos os arquivos e metadados necessários para instalar um site no IIS.
- Criar um pacote de implantação e instalá-lo usando a linha de comando.

O processo que você seguiu nos tutoriais anteriores para configurar o Visual Studio para automatizar tarefas de implantação se aplica a todos esses métodos. Esses tutoriais, você usará as duas primeiras desses métodos. Para obter informações sobre como usar pacotes de implantação, consulte [Implantando um aplicativo da web criando e instalando um pacote de implantação da web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) no mapa de conteúdo de implantação Web para Visual Studio e do ASP.NET.

Antes de publicar, certifique-se de que você está executando o Visual Studio no modo de administrador. Se você não vir **(administrador)** na barra de título, feche o Visual Studio. No Windows 8 **inicie** página ou o Windows 7 **inicie** menu, clique com botão direito no ícone para a versão do Visual Studio que você está usando e selecione **executar como administrador**. Modo de administrador é necessário para publicar somente quando você estiver publicando para IIS no computador local.

### <a name="create-the-publish-profile"></a>Criar o perfil de publicação

1. Na **Gerenciador de soluções**, clique com botão direito do projeto ContosoUniversity (não no projeto ContosoUniversity.DAL) e selecione **publicar**.

    O **publicar na Web** assistente é exibido.

    ![Guia de perfil do Assistente da Web de publicação](deploying-to-iis/_static/image13.png)
2. Na lista suspensa, selecione  **&lt;New... &gt;**. (Com a atualização mais recente Visual Studio instalada, não há nenhuma lista suspensa, e é o botão para clicar para criar um novo perfil a partir do zero **personalizado**.)
3. No **novo perfil** caixa de diálogo, insira "Test" e, em seguida, clique em **Okey**.

    O assistente avançará automaticamente para o **Conexão** guia.
4. No **URL do serviço** , digite *localhost*.
5. No **Site/aplicativo** , digite *Default Web Site/ContosoUniversity*
6. No **URL de destino** , digite `http://localhost/ContosoUniversity`

    O **URL de destino** configuração não é necessária. Quando o Visual Studio terminar de implantar o aplicativo, ele abre automaticamente o navegador padrão para essa URL. Se você não quiser que o navegador para abrir automaticamente após a implantação, deixe essa caixa em branco.
7. Clique em **validar Conexão** para verificar se as configurações estão corretas e se conectar ao IIS no computador local.

    Uma marca de seleção verde verifica se a conexão foi bem-sucedida.

    ![Guia de Conexão do Assistente da Web de publicação](deploying-to-iis/_static/image14.png)
8. Clique em **próxima** para ir para o **configurações** guia.
9. O **configuração** caixa suspensa Especifica a configuração de compilação para implantar. Deixe definido como o valor padrão da versão. Você não implantar compilações de depuração neste tutorial.
10. Expandir **opções de publicação do arquivo**e, em seguida, selecione **excluir arquivos do aplicativo\_pasta de dados**.

    No ambiente de teste o aplicativo irá acessar os bancos de dados que você criou na instância local do SQL Server Express, não os arquivos. mdf arquivos a *App\_dados* pasta.
11. Deixe o **pré-compilar durante a publicação** e **remover arquivos adicionais no destino** caixas de seleção desmarcadas.

    ![Opções de publicação do arquivo na guia Configurações](deploying-to-iis/_static/image15.png)

    Pré-compilação é uma opção que é útil principalmente para sites muito grandes; ela pode reduzir o tempo de inicialização de página pela primeira vez em que uma página é solicitada, depois que o site for publicado.

    Você não precisa remover arquivos adicionais, como este é a primeira implantação e não haverá todos os arquivos na pasta de destino ainda.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Se você selecionar **remover arquivos adicionais** para uma implantação subsequente ao mesmo site, certifique-se de que você use o recurso de visualização para que você veja com antecedência quais arquivos serão excluídos antes de implantar. O comportamento esperado é que a implantação da Web excluirá os arquivos no servidor de destino que você excluiu no seu projeto. No entanto, em comparação com a estrutura de pasta inteira sob as pastas de origem e de destino e, em alguns cenários de implantação da Web pode excluir arquivos que não deseja excluir.
    > 
    > Por exemplo, se você tiver um aplicativo web em uma subpasta no servidor quando você implanta um projeto para a pasta raiz, a subpasta será excluída. Você pode ter um projeto do site principal em contoso.com e o outro projeto para um blog em contoso.com/blog. O aplicativo de blog está em uma subpasta. Se você selecionar remover arquivos adicionais no destino quando você implanta o site principal, o aplicativo de blog será excluído.
    > 
    > Para obter outro exemplo, seu aplicativo\_pasta de dados pode ser excluída de forma inesperada. Alguns bancos de dados, como o SQL Server Compact armazenam arquivos de banco de dados no aplicativo\_pasta de dados. Após a implantação inicial, você não deseja manter copiando os arquivos de banco de dados em implantações subsequentes, para que você selecionar Excluir aplicativo\_dados na guia empacotar/Publicar Web. Depois de fazer isso, se você tiver que remover arquivos adicionais no destino selecionado, os arquivos de banco de dados e o aplicativo\_própria pasta de dados será excluída na próxima vez que você publicar.

### <a name="configure-deployment-for-the-membership-database"></a>Configurar a implantação para o banco de dados de associação

As etapas a seguir se aplicam à **DefaultConnection** banco de dados de **bancos de dados** seção da caixa de diálogo.

1. No **cadeia de caracteres de conexão remota** , digite a seguinte cadeia de conexão que aponta para o novo SQL Server Express associação ao banco de dados.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    O processo de implantação colocará essa cadeia de caracteres de conexão no arquivo Web. config implantado porque **Use essa cadeia de caracteres de conexão em tempo de execução** está selecionado.

    Você também pode obter a cadeia de conexão do **Gerenciador de servidores**. Na **Gerenciador de servidores**, expanda **conexões de dados** e selecione o  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** banco de dados, depois do **propriedades** cópia de janela a **cadeia de caracteres de Conexão** valor. Que a cadeia de caracteres de conexão terá uma configuração adicional que você pode excluir: `Pooling=False`.
2. Selecione **Atualizar banco de dados**.

    Isso fará com que o esquema de banco de dados a ser criado no banco de dados de destino durante a implantação. Nas etapas a seguir, você especificar os scripts adicionais que você precisa executar: uma para conceder acesso de banco de dados para o pool de aplicativos padrão e para implantar dados.
3. Clique em **configurar atualizações de banco de dados**.
4. No **configurar atualizações de banco de dados** caixa de diálogo, clique em **Adicionar Script do SQL** e, em seguida, navegue até o *Grant.sql* script que você salvou anteriormente na pasta da solução.
5. Repita o processo para adicionar o *aspnet-data-dev.sql* script.

    ![Configurar atualizações de banco de dados para o banco de dados de associação](deploying-to-iis/_static/image16.png)
6. Clique em **Fechar**.

### <a name="configure-deployment-for-the-application-database"></a>Configurar a implantação para o banco de dados do aplicativo

Quando o Visual Studio detecta um Entity Framework `DbContext` classe, ele cria uma entrada na **bancos de dados** seção tem um **executar migrações do Code First** caixa de seleção em vez de um  **Atualizar banco de dados** caixa de seleção. Para este tutorial, você usará essa caixa de seleção para especificar a implantação de migrações do Code First.

Em alguns cenários, talvez você esteja usando um `DbContext` banco de dados, mas você deseja usar o provedor dbDacFx em vez de migrações para implantar o banco de dados. Nesse caso, consulte [como implantar um banco de dados Code First sem migrações?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nas perguntas frequentes sobre ASP.NET Web implantação no MSDN.

As etapas a seguir se aplicam à **SchoolContext** banco de dados de **bancos de dados** seção da caixa de diálogo.

1. No **cadeia de caracteres de conexão remota** , digite a seguinte cadeia de conexão que aponta para o novo SQL Server Express aplicativo banco de dados.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    O processo de implantação colocará essa cadeia de caracteres de conexão no arquivo Web. config implantado porque **Use essa cadeia de caracteres de conexão em tempo de execução** está selecionado.

    Você também pode obter a cadeia de caracteres de conexão de banco de dados de aplicativo do **Gerenciador de servidores** da mesma maneira que você obteve a associação de cadeia de caracteres de conexão de banco de dados.
2. Selecione **executar migrações do Code First (executado na inicialização do aplicativo)**.

    Essa opção faz com que o processo de implantação configurar o arquivo Web. config implantado para especificar o `MigrateDatabaseToLatestVersion` inicializador. Esse inicializador atualiza automaticamente o banco de dados para a versão mais recente quando o aplicativo acessa o banco de dados pela primeira vez após a implantação.

### <a name="configure-publish-profile-transforms"></a>Configurar transformações de perfil de publicação

1. Clique em **feche**e, em seguida, clique em **Sim** quando for perguntado se você deseja salvar as alterações.
2. Na **Gerenciador de soluções**, expanda **Properties**, expanda **PublishProfiles**.
3. Clique Rright *Test.pubxml,* e, em seguida, clique em **adicionar Config transformar**.

    ![Adicionar menu de transformação de configuração](deploying-to-iis/_static/image17.png)

    O Visual Studio cria o *Web.Test.config* arquivo de transformação e o abre.
4. No *Web.Test.config* transformar o arquivo, insira o seguinte código imediatamente após a abertura marca de configuração.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Quando você usa o teste de perfil de publicação, essa transformação define o indicador de ambiente para "Test". No site implantado, você verá "(teste)" após o cabeçalho H1 de "Contoso University".
5. Salve e feche o arquivo.
6. Clique com botão direito do *Web.Test.config* de arquivo e clique em **visualização transformar** para certificar-se de que a transformação que você codificou produz as alterações esperadas.

    O **visualização da Web. config** janela mostra o resultado da aplicação de ambos os *Release* transforma e o *Web.Test.config* transforma.

### <a name="preview-the-deployment-updates"></a>Visualize as atualizações de implantação

1. Abra o **Publicar Web** novamente o Assistente (com o botão direito no projeto ContosoUniversity e clique em **publicar**).
2. No **versão prévia** guia, certifique-se de que o **teste** perfil é ainda selecionado e depois clique em **iniciar visualização** para ver uma lista dos arquivos que serão copiados.

    ![Botão de visualização](deploying-to-iis/_static/image18.png)

    ![Visualização de publicação](deploying-to-iis/_static/image19.png)

    Você também pode clicar na **banco de dados de visualização** link para ver os scripts que serão executados no banco de dados de associação. (Nenhum script é executado para a implantação de migrações do Code First, portanto, não há nada a visualização para o banco de dados do aplicativo.)
3. Clique em **Publicar**.

    Se o Visual Studio não está no modo de administrador, você poderá receber uma mensagem de erro que indica um erro de permissões. Nesse caso, feche o Visual Studio, abra-o no modo de administrador e tente publicar novamente.

    Se o Visual Studio está no modo de administrador, o **saída** janela relatórios bem-sucedida compilar e publicar.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Se você digitou a URL na **URL de destino** caixa no perfil de publicação **Conexão** guia, o navegador abre automaticamente para a página inicial do Contoso University em execução no IIS no computador local.

## <a name="test-in-the-test-environment"></a>Teste no ambiente de teste

Observe que o indicador de ambiente mostra "(teste)" em vez de "(desenvolvimento)", que mostra que o *Web. config* transformação para o indicador de ambiente foi bem-sucedida.

Execute o **instrutores** página para verificar se que o Code First propagado o banco de dados do instrutor. Quando você seleciona essa página, ele pode levar alguns minutos para carregar porque o Code First cria o banco de dados e, em seguida, executa o `Seed` método. (Ele não fez isso quando você estava na home page, porque o aplicativo não tente acessar o banco de dados ainda.)

Clique o **alunos** guia para verificar se o banco de dados implantado não tem nenhum aluno.

Selecione **adicionar alunos** da **alunos** menu, adicionar um aluno e, em seguida, exibir o novo aluno no **alunos** página para verificar se que você pode escrever com êxito no banco de dados .

Dos **cursos** menu, selecione **atualização créditos**. O **créditos de atualização** página requer permissões de administrador, portanto, o **Log In** página é exibida. Insira as credenciais de conta de administrador que você criou anteriormente ("admin" e "devpwd"). O **atualização créditos** página for exibida, que verifica que a conta de administrador que você criou no tutorial anterior foi implantada corretamente no ambiente de teste.

Verificar se um *Elmah* pasta existe na *c:\inetpub\wwwroot\ContosoUniversity* pasta com apenas o arquivo de espaço reservado para ele.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Examine as alterações automáticas de Web. config para migrações do Code First

Abra o *Web. config* arquivo no aplicativo implantado na *C:\inetpub\wwwroot\ContosoUniversity* e você pode ver onde o processo de implantação configurado migrações do Code First para automaticamente Atualize o banco de dados para a versão mais recente.

![](deploying-to-iis/_static/image21.png)

O processo de implantação também criou uma nova cadeia de conexão para migrações do Code First exclusivamente atualizar o esquema de banco de dados:

![Cadeia de caracteres de conexão Database_Publish](deploying-to-iis/_static/image22.png)

Essa cadeia de caracteres de conexão adicional permite que você especifique uma conta de usuário para atualizações de esquema de banco de dados e uma conta de usuário diferentes para acesso de dados do aplicativo. Por exemplo, você pode atribuir a **db\_proprietário** função para migrações do Code First, e **db\_datareader** e **db\_datawriter**as funções para o aplicativo. Isso é um padrão comum de defesa em profundidade que impede que o código potencialmente mal-intencionado no aplicativo de alterar o esquema de banco de dados. (Por exemplo, isso pode acontecer em um ataque de injeção de SQL.) Esse padrão não é usado por esses tutoriais. Se você quiser implementar esse padrão em seu cenário, você pode fazer isso executando as seguintes etapas:

1. No **as configurações** guia da **publicar na Web** assistente, insira a cadeia de caracteres de conexão que especifica um usuário com permissões de atualização do esquema de banco de dados completo e desmarque o **usar essa cadeia de caracteres de conexão em tempo de execução** caixa de seleção. No arquivo Web. config implantado, isso se torna o `DatabasePublish` cadeia de caracteres de conexão.
2. Crie uma transformação do arquivo Web. config para a cadeia de caracteres de conexão que você deseja que o aplicativo para usar em tempo de execução.

## <a name="summary"></a>Resumo

Agora você implantou seu aplicativo ao IIS no computador de desenvolvimento e testá-lo lá.

![Home page no teste](deploying-to-iis/_static/image23.png)

Isso verifica se o processo de implantação copiados de conteúdo do aplicativo para o local certo (excluindo os arquivos que você não quiser implantar), e também essa implantação da Web configurado IIS corretamente durante a implantação. No próximo tutorial, você executará mais um teste que localiza uma tarefa de implantação que ainda não foi feita: definindo permissões de pasta na *Elmah* pasta.

## <a name="more-information"></a>Mais informações

Para obter informações sobre como executar o IIS ou IIS Express no Visual Studio, consulte os seguintes recursos:

- [Visão geral do IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) no site IIS.net.
- [Introdução ao IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) no blog de Guthrie.
- [Web servidores no Visual Studio para projetos Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Principais diferenças entre o IIS e o ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) no site do ASP.NET.

Para obter informações sobre quais problemas podem surgir quando seu aplicativo é executado em confiança média, consulte [hospedando aplicativos do ASP.NET em confiança média](http://www.4guysfromrolla.com/articles/100307-1.aspx) em que a equipe de site Rolla de 4.

> [!div class="step-by-step"]
> [Anterior](project-properties.md)
> [Próximo](setting-folder-permissions.md)
