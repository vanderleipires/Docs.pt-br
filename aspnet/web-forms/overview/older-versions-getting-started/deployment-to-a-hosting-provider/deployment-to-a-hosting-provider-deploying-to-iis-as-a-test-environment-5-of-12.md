---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando o IIS como um ambiente de teste - 5 de 12 | Microsoft Docs'
author: tdykstra
description: "Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: a7995844ee6ed19efa130c4f6c019214d6652ea7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando o IIS como um ambiente de teste - 5 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como implantar um aplicativo da web ASP.NET para o IIS no computador local.

Quando você desenvolve um aplicativo, você geralmente teste executando-o no Visual Studio. Por padrão, isso significa que você está usando o Visual Studio Development Server (também conhecido como Cassini). O servidor de desenvolvimento do Visual Studio facilita o teste durante o desenvolvimento no Visual Studio, mas ele não funciona exatamente como o IIS. Como resultado, é possível que um aplicativo serão executados corretamente quando você testá-lo no Visual Studio, mas falha quando é implantado no IIS em um ambiente de hospedagem.

Você pode testar seu aplicativo com mais confiança das seguintes maneiras:

1. Use IIS Express ou o IIS completo em vez do servidor de desenvolvimento do Visual Studio quando o teste no Visual Studio durante o desenvolvimento. Este método geralmente emula com mais precisão como seu site será executado no IIS. No entanto, esse método não seu processo de implantação de teste ou validar que o resultado do processo de implantação será executado corretamente.
2. Implante o aplicativo ao IIS no computador de desenvolvimento usando o mesmo processo que você usará mais tarde para implantá-lo em seu ambiente de produção. Esse método valida o processo de implantação além de validar que o aplicativo será executado corretamente no IIS.
3. Implante o aplicativo em um ambiente de teste que seja o mais próximo possível do seu ambiente de produção. Como o ambiente de produção para esses tutoriais é um provedor de hospedagem de terceiros, o ambiente de teste ideal seria uma segunda conta com o provedor de hospedagem. Você usaria essa segunda conta somente para teste, mas ela deve ser configurada da mesma forma que a conta de produção.

Este tutorial mostra as etapas para a opção 2. Orientação para a opção 3 é fornecida no final do [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, e há links para recursos para a opção 1 no final deste tutorial.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configurando o aplicativo para execução no nível de confiança médio

Antes de instalar o IIS e implantar a ele, você altera uma configuração do arquivo Web. config para tornar o site de execução mais que ele irá em um ambiente típico de hospedagem compartilhado.

Provedores de hospedagem normalmente executam seu site da web em *confiança média*, que significa que há algumas coisas que não é permitido fazer. Por exemplo, o código do aplicativo não pode acessar o registro do Windows e não é possível leitura ou gravação arquivos que estão fora da hierarquia de pastas do seu aplicativo. Por padrão, o aplicativo é executado *alta confiança* no computador local, o que significa que o aplicativo poderá fazer coisas que falharem ao implantar na produção. Portanto, para tornar o ambiente de teste que mais reflitam com precisão o ambiente de produção, você configurará o aplicativo seja executado em confiança média.

No arquivo Web. config do aplicativo, adicione um **confiança** elemento o **System. Web** elemento, conforme mostrado neste exemplo.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Agora, o aplicativo será executado em confiança média no IIS mesmo no computador local. Essa configuração permite que você mais cedo possível capturar todas as tentativas pelo código do aplicativo para fazer algo que falharia na produção.

> [!NOTE]
> Se você estiver usando migrações do Entity Framework Code First, verifique se você tem a versão 5.0 ou posterior instalado. Na versão do Entity Framework 4.3, migrações requer confiança total para atualizar o esquema de banco de dados.


## <a name="installing-iis-and-web-deploy"></a>Instalar o IIS e implantação da Web

Para implantar o IIS no computador de desenvolvimento, você deve ter o IIS e a implantação da Web instalado. Eles não estão incluídos na configuração padrão de Windows 7. Se você já tiver instalado o IIS e a implantação da Web, vá para a próxima seção.

Usando o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) é a melhor maneira de instalar o IIS e a implantação da Web, como o Web Platform Installer instala uma configuração recomendada para o IIS e instala automaticamente os pré-requisitos para o IIS e Web Implante se necessário.

Para executar o Web Platform Installer para instalar o IIS e a implantação da Web, use o link a seguir. Se você já tiver instalado o IIS, implantação da Web ou qualquer um de seus componentes necessários, o Web Platform Installer instala apenas o que está faltando.

- [Instalar o IIS e a implantação da Web usando WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Configuração do Pool de aplicativos padrão para o .NET 4

Depois de instalar o IIS, execute **Gerenciador do IIS** para certificar-se de que o .NET Framework versão 4 é atribuído ao pool de aplicativos padrão.

Do Windows **iniciar** menu, selecione **executar**, digite "inetmgr" e, em seguida, clique em **Okey**. (Se o **executar** comando não está no seu **iniciar** menu, você pode pressionar a tecla Windows e R para abri-lo. Ou a barra de tarefas, clique **propriedades**, selecione o **Menu Iniciar** , clique em **personalizar**e selecione **executar o comando**.)

No **conexões** painel, expanda o nó do servidor e selecione **Pools de aplicativos**. No **Pools de aplicativos** painel, se **DefaultAppPool** é atribuído para o .NET framework versão 4 como a ilustração a seguir, vá para a próxima seção.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Se você vir apenas dois pools de aplicativos, e ambos os parâmetros são definidos para o .NET Framework 2.0, você precisa instalar o ASP.NET 4 no IIS:

- Abra uma janela de prompt de comando clicando **Prompt de comando** nas janelas **iniciar** menu e selecionando **executar como administrador**. Em seguida, execute [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar o ASP.NET 4 no IIS, usando os seguintes comandos. (Em sistemas de 64 bits, substitua "Estrutura" com "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Este comando cria novos pools de aplicativos para o .NET Framework 4, mas o pool de aplicativos padrão ainda será definido como 2.0. Você estará Implantando um aplicativo que tem como destino .NET 4 para o pool de aplicativos, você precisará alterar o pool de aplicativos para o .NET 4.

Se você fechou **Gerenciador do IIS**, executá-lo novamente, expanda o nó do servidor e clique em **Pools de aplicativos** para exibir o **Pools de aplicativos** painel novamente.

No **Pools de aplicativos** painel, clique em **DefaultAppPool**e, em seguida, no **ações** painel clique **configurações básicas**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

No **Editar Pool de aplicativos** caixa de diálogo Alterar **versão do .NET Framework** para **v 4.0.30319 do .NET Framework** e clique em **Okey**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Agora você está pronto para publicar no IIS.

## <a name="publishing-to-iis"></a>Publicação para o IIS

Há várias maneiras que você pode implantar usando o Visual Studio 2010 e a implantação da Web:

- Use o Visual Studio, um clique para publicar.
- Criar um *pacote de implantação* e instalá-lo usando a UI Gerenciador do IIS. O pacote de implantação consiste em uma *. zip* arquivo que contém todos os arquivos e metadados necessários para instalar um site no IIS.
- Criar um pacote de implantação e instalá-lo usando a linha de comando.

O processo que você seguiu os tutoriais anterior para configurar o Visual Studio para automatizar tarefas de implantação se aplica a todos os três métodos. Esses tutoriais, você usará o primeiro desses métodos. Para obter informações sobre como usar pacotes de implantação, consulte [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

Antes de publicar, certifique-se de que você está executando o Visual Studio no modo de administrador. (No Windows 7 **iniciar** menu, clique no ícone para a versão do Visual Studio que você está usando e selecione **executar como administrador**.) Modo de administrador é necessária para publicar somente quando você estiver publicando ao IIS no computador local.

Em **Solution Explorer**, clique com botão direito do ContosoUniversity (não no projeto ContosoUniversity.DAL) e selecione **publicar**.

O **Publicar Web** assistente é exibido.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Na lista suspensa, selecione  **&lt;New... &gt;**.

No **novo perfil** caixa de diálogo, digite "Teste" e, em seguida, clique em **Okey**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Esse nome é que o mesmo que o nó intermediário do Web.Test.config transformar o arquivo que você criou anteriormente. Essa correspondência é o que faz com que as transformações Web.Test.config a serem aplicadas quando você publicar usando este perfil.

O assistente avançará automaticamente para o **Conexão** guia.

No **URL do serviço** , digite *localhost*.

No **Site/aplicativo** , digite *Default Web Site/ContosoUniversity*.

No **URL de destino** , digite `http://localhost/ContosoUniversity`.

O **URL de destino** configuração não é necessária. Quando o Visual Studio terminar de implantar o aplicativo, ele abre automaticamente o navegador padrão para essa URL. Se você não deseja que o navegador para abrir automaticamente após a implantação, deixe essa caixa em branco.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Clique em **Conexão validar** para verificar se as configurações estão corretas e você pode se conectar ao IIS no computador local.

Uma marca de seleção verde verifica se a conexão foi bem-sucedida.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Clique em **próximo** para ir para o **configurações** guia.

O **configuração** caixa suspensa Especifica a configuração de compilação para implantar. O valor padrão é a versão, o que é o que você deseja.

Deixe o **remover arquivos adicionais no destino** caixa de seleção está desmarcada. Como essa é a primeira implantação, haverá todos os arquivos na pasta de destino ainda.

No **bancos de dados** seção, digite o seguinte valor na caixa de cadeia de caracteres de conexão do **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

O processo de implantação colocar essa cadeia de caracteres de conexão no arquivo Web. config implantado porque **usar essa cadeia de caracteres de conexão em tempo de execução** está selecionado.

Também em **SchoolContext**, selecione **aplicar migrações do Code First**. Esta opção faz com que o processo de implantação configurar o arquivo Web. config implantado para especificar o `MigrateDatabaseToLatestVersion` inicializador. Este inicializador atualiza automaticamente o banco de dados para a versão mais recente quando o aplicativo acessa o banco de dados pela primeira vez após a implantação.

Na caixa de cadeia de caracteres de conexão do **DefaultConnection**, digite o seguinte valor:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Deixe **Atualizar banco de dados** desmarcada. O banco de dados de associação será implantado, copiando o arquivo. sdf no aplicativo\_dados e você não desejar que o processo de implantação fazer nada com esse banco de dados.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Clique em **próximo** para ir para o **visualização** guia.

No **visualização** , clique em **iniciar visualização** para ver uma lista dos arquivos que serão copiados.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Clique em **Publicar**.

Se o Visual Studio não está no modo de administrador, você poderá receber uma mensagem de erro que indica um erro de permissões. Nesse caso, feche o Visual Studio, abra-o no modo de administrador e tente publicar novamente.

Se o Visual Studio está no modo de administrador, o **saída** janela relatórios bem-sucedida compila e publica.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

O navegador é aberto automaticamente para a página inicial do Contoso University em execução no IIS no computador local.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Teste no ambiente de teste

Observe que o indicador de ambiente mostra "(teste)" em vez de "(desenvolvimento)", que mostra que o *Web. config* transformação para o indicador de ambiente foi bem-sucedida.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Execute o **alunos** página para verificar se o banco de dados implantado não tem nenhum alunos. Quando você seleciona essa página pode levar alguns minutos para carregar porque o Code First cria o banco de dados e, em seguida, executa o `Seed` método. (Ele não fez isso quando estiver na página inicial porque o aplicativo não tente acessar o banco de dados ainda.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Execute o **instrutores** página para verificar que o Code First propagado o banco de dados do instrutor:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Selecione **adicionar alunos** do **alunos** menu, adicionar um aluno e, em seguida, exibir o novo aluno no **alunos** página para verificar que você pode escrever com êxito para o banco de dados :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Do **cursos** menu, selecione **atualização créditos**. O **atualização créditos** página requer permissões de administrador, para que o **logon** página é exibida. Digite as credenciais de conta de administrador que você criou anteriormente ("admin" e "Pas$ w0rd"). O **atualização créditos** página é exibida, que verifica se a conta de administrador que você criou no tutorial anterior foi implantada corretamente para o ambiente de teste.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Verificar se um *Elmah* pasta existe com apenas o arquivo de espaço reservado nele.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisar as alterações da Web. config automática para migrações do Code First

Abra o *Web. config* arquivo em um aplicativo implantado em *C:\inetpub\wwwroot\ContosoUniversity* e você pode ver onde o processo de implantação configurado migrações do Code First para automaticamente Atualize o banco de dados para a versão mais recente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Além disso, o processo de implantação criada uma nova cadeia de conexão para migrações do Code First exclusivamente atualizar o esquema de banco de dados:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Essa cadeia de caracteres de conexão adicionais permite que você especifique uma conta de usuário para atualizações de esquema de banco de dados e uma conta de usuário diferente para acesso de dados do aplicativo. Por exemplo, você pode atribuir o banco de dados\_a função de proprietário para migrações do Code First e banco de dados\_datareader e o banco de dados\_datawriter funções para o aplicativo. Este é um padrão comum de defesa em profundidade que impede que potencialmente mal-intencionados no aplicativo de alterar o esquema de banco de dados. (Por exemplo, isso pode ocorrer em um ataque de injeção de SQL.) Esse padrão não é usado por esses tutoriais. Não é aplicável para SQL Server Compact, e não se aplica quando você migrar para o SQL Server em um tutorial posterior nesta série. O site Cytanium oferece apenas uma conta de usuário para acessar o banco de dados do SQL Server que você criou em Cytanium. Se você é capaz de implementar esse padrão em seu cenário, você poderá fazê-lo executando as seguintes etapas:

1. No **configurações** guia do **Publicar Web** assistente, insira a cadeia de conexão que especifica um usuário com permissões de atualização de esquema de banco de dados completo e desmarque o **usar essa cadeia de caracteres de conexão em tempo de execução** caixa de seleção. O arquivo Web. config implantado, isso se torna o `DatabasePublish` cadeia de caracteres de conexão.
2. Crie uma transformação do arquivo Web. config para a cadeia de caracteres de conexão que você deseja que o aplicativo para usar em tempo de execução.

Agora você implantou o aplicativo ao IIS no computador de desenvolvimento e testado ele existe. Isso verifica se o processo de implantação copiou o conteúdo do aplicativo para o local correto (excluindo os arquivos que você não quiser implantar), e também essa implantação da Web IIS foi configurado corretamente durante a implantação. O seguinte tutorial, você executará mais um teste que localiza uma tarefa de implantação que ainda não foi feita: definindo permissões de pasta no *Elmah* pasta.

## <a name="more-information"></a>Mais informações

Para obter informações sobre como executar o IIS ou IIS Express no Visual Studio, consulte os seguintes recursos:

- [Visão geral do IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) no site IIS.net.
- [Introdução ao IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) no blog de Scott Guthrie.
- [Como: especificar o servidor Web para projetos Web no Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Principais diferenças entre o IIS e o ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) no site do ASP.NET.
- [Testar o ASP.NET MVC ou Web Forms aplicativo no IIS 7 em 30 segundos](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) no blog de Rick Anderson. Essa entrada fornece exemplos de por que teste com o servidor de desenvolvimento do Visual Studio (Cassini) não é tão confiável quanto teste no IIS Express e por que teste no IIS Express não é tão confiável quanto teste no IIS.

Para obter informações sobre quais problemas podem surgir quando seu aplicativo é executado em confiança média, consulte [hospedar aplicativos ASP.NET na relação de confiança médio](http://www.4guysfromrolla.com/articles/100307-1.aspx) na equipe de 4 do site Rolla.

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[Próximo](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
