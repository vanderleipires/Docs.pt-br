---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando no IIS como um ambiente de teste – 5 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: ba939012e8fb11a50992eeaef70e8ebf61cea851
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833702"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando no IIS como um ambiente de teste – 5 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como implantar um aplicativo da web ASP.NET no IIS no computador local.

Quando você desenvolve um aplicativo, geralmente você testa executá-lo no Visual Studio. Por padrão, isso significa que você estiver usando o Visual Studio Development Server (também conhecido como Cassini). O Visual Studio Development Server torna mais fácil de testar durante o desenvolvimento no Visual Studio, mas ele não funciona exatamente como o IIS. Como resultado, é possível que um aplicativo será executado corretamente quando você testá-lo no Visual Studio, mas falha quando ele é implantado no IIS em um ambiente de hospedagem.

Você pode testar seu aplicativo de maneira mais confiável das seguintes maneiras:

1. Usar o IIS Express ou o IIS completo em vez do Visual Studio Development Server quando você testar no Visual Studio durante o desenvolvimento. Esse método geralmente emula mais precisamente como seu site será executado no IIS. No entanto, esse método não testa seu processo de implantação ou validar que o resultado do processo de implantação será executado corretamente.
2. Implante o aplicativo ao IIS no computador de desenvolvimento usando o mesmo processo que você usará posteriormente para implantá-lo em seu ambiente de produção. Esse método valida seu processo de implantação além de validar que o aplicativo será executado corretamente no IIS.
3. Implante o aplicativo em um ambiente de teste que é o mais próximo possível para seu ambiente de produção. Como o ambiente de produção para esses tutoriais é um provedor de hospedagem de terceiros, o ambiente de teste ideal seria uma segunda conta com o provedor de hospedagem. Você usaria essa segunda conta somente para teste, mas ele seria configurado da mesma forma que a conta de produção.

Este tutorial mostra as etapas para a opção 2. Diretrizes para a opção 3 é fornecido no final o [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, e há links para recursos para a opção 1 no final deste tutorial.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configurando o aplicativo para execução em confiança média

Antes de instalar o IIS e implantando a ele, você vai alterar uma configuração do arquivo Web. config para fazer com que o site executado mais como ele será em um ambiente típico de hospedagem compartilhado.

Provedores de hospedagem normalmente executar seu site da web no *confiança média*, que significa que há algumas coisas que ele não tem permissão para fazer. Por exemplo, o código do aplicativo não pode acessar o registro do Windows e não é possível ler ou gravar arquivos que estão fora da hierarquia de pastas do seu aplicativo. Por padrão, seu aplicativo é executado *alta confiança* no computador local, que significa que o aplicativo pode ser capaz de fazer coisas que falharem ao implantá-lo para a produção. Portanto, para tornar o ambiente de teste que mais reflitam com precisão o ambiente de produção, você configurará o aplicativo seja executado em confiança média.

No arquivo Web. config do aplicativo, adicione uma **relação de confiança** elemento o **System. Web** elemento, conforme mostrado neste exemplo.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Agora, o aplicativo será executado em confiança média no IIS, mesmo em seu computador local. Essa configuração permite que você mais cedo possível capturar todas as tentativas por código do aplicativo para fazer algo que falharia na produção.

> [!NOTE]
> Se você estiver usando o Entity Framework Code First Migrations, certifique-se de que você tenha a versão 5.0 ou posterior instalado. No Entity Framework versão 4.3, migrações requer confiança total para atualizar o esquema de banco de dados.


## <a name="installing-iis-and-web-deploy"></a>Instalando o IIS e Web implantar

Para implantar em IIS no computador de desenvolvimento, você deve ter o IIS e implantação da Web instalado. Eles não estão incluídos na configuração padrão Windows 7. Se você já tiver instalado o IIS e a implantação da Web, vá para a próxima seção.

Usando o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) é a maneira preferencial para instalar o IIS e a implantação da Web, porque o Web Platform Installer instala uma configuração recomendada para o IIS e instala automaticamente os pré-requisitos para o IIS e da Web Implante se necessário.

Para executar o Web Platform Installer para instalar o IIS e implantação da Web, use o link a seguir. Se você já tiver instalado o IIS, implantação da Web ou qualquer um de seus componentes obrigatórios, o Web Platform Installer instala apenas o que está faltando.

- [Instalar o IIS e implantação da Web usando WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Configuração do Pool de aplicativos padrão para o .NET 4

Depois de instalar o IIS, execute **o Gerenciador do IIS** para certificar-se de que o .NET Framework versão 4 é atribuído ao pool de aplicativos padrão.

De que o Windows **inicie** menu, selecione **execute**, insira "inetmgr" e, em seguida, clique em **Okey**. (Se o **executados** comando não está no seu **iniciar** menu, você pode pressionar a tecla Windows e o R para abri-lo. Ou clique com botão direito a barra de tarefas, clique em **propriedades**, selecione o **Menu Iniciar** , clique em **personalizar**e selecione **executar comando**.)

No **conexões** painel, expanda o nó do servidor e selecione **Pools de aplicativos**. No **Pools de aplicativos** painel, se **DefaultAppPool** é atribuído para o .NET framework versão 4, como mostra a ilustração a seguir, pule para a próxima seção.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Se você vir apenas dois pools de aplicativos e os dois são definidos para o .NET Framework 2.0, você precisa instalar o ASP.NET 4 no IIS:

- Abra uma janela de prompt de comando clicando **Prompt de comando** em que o Windows **inicie** menu e selecionando **executar como administrador**. Em seguida, execute [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar o ASP.NET 4 no IIS, usando os comandos a seguir. (Em sistemas de 64 bits, substitua "Estrutura" com "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Este comando cria novos pools de aplicativos para o .NET Framework 4, mas o pool de aplicativos padrão ainda será definido como 2.0. Você estará Implantando um aplicativo que tem como alvo o .NET 4 para esse pool de aplicativos, portanto, você precisa alterar o pool de aplicativos para o .NET 4.

Se você fechou **o Gerenciador do IIS**, execute-o novamente, expanda o nó do servidor e clique em **Pools de aplicativos** para exibir o **Pools de aplicativos** painel novamente.

No **Pools de aplicativos** painel, clique em **DefaultAppPool**e, em seguida, no **ações** painel clique **configurações básicas**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

No **Editar Pool de aplicativos** caixa de diálogo alteração **versão do .NET Framework** para **v4.0.30319 do .NET Framework** e clique em **Okey**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Agora você está pronto para publicar no IIS.

## <a name="publishing-to-iis"></a>Publicação para IIS

Há várias maneiras que você pode implantar usando o Visual Studio 2010 e a implantação da Web:

- Use o Visual Studio, um clique para publicar.
- Criar uma *pacote de implantação* e instalá-lo usando a UI do Gerenciador do IIS. O pacote de implantação consiste em uma *. zip* arquivo que contém todos os arquivos e metadados necessários para instalar um site no IIS.
- Criar um pacote de implantação e instalá-lo usando a linha de comando.

O processo que você seguiu nos tutoriais anteriores para configurar o Visual Studio para automatizar tarefas de implantação se aplica a todos esses três métodos. Esses tutoriais, você usará o primeiro desses métodos. Para obter informações sobre como usar pacotes de implantação, consulte [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

Antes de publicar, certifique-se de que você está executando o Visual Studio no modo de administrador. (No Windows 7 **inicie** menu, clique com botão direito no ícone para a versão do Visual Studio que você está usando e selecione **executar como administrador**.) Modo de administrador é necessário para publicar somente quando você estiver publicando para IIS no computador local.

Na **Gerenciador de soluções**, clique com botão direito do projeto ContosoUniversity (não no projeto ContosoUniversity.DAL) e selecione **publicar**.

O **publicar na Web** assistente é exibido.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Na lista suspensa, selecione  **&lt;New... &gt;**.

No **novo perfil** caixa de diálogo, insira "Test" e, em seguida, clique em **Okey**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Esse nome é que o mesmo que o nó central do Web.Test.config transformar o arquivo que você criou anteriormente. Essa correspondência é o que faz com que as transformações Web.Test.config a ser aplicada quando você publica por meio desse perfil.

O assistente avançará automaticamente para o **Conexão** guia.

No **URL do serviço** , digite *localhost*.

No **Site/aplicativo** , digite *Default Web Site/ContosoUniversity*.

No **URL de destino** , digite `http://localhost/ContosoUniversity`.

O **URL de destino** configuração não é necessária. Quando o Visual Studio terminar de implantar o aplicativo, ele abre automaticamente o navegador padrão para essa URL. Se você não quiser que o navegador para abrir automaticamente após a implantação, deixe essa caixa em branco.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Clique em **validar Conexão** para verificar se as configurações estão corretas e se conectar ao IIS no computador local.

Uma marca de seleção verde verifica se a conexão foi bem-sucedida.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Clique em **próxima** para ir para o **configurações** guia.

O **configuração** caixa suspensa Especifica a configuração de compilação para implantar. O valor padrão é a versão, o que é o que você deseja.

Deixe o **remover arquivos adicionais no destino** caixa de seleção desmarcada. Como essa é a primeira implantação, haverá todos os arquivos na pasta de destino ainda.

No **bancos de dados** , digite o seguinte valor na caixa de cadeia de caracteres de conexão para **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

O processo de implantação colocará essa cadeia de caracteres de conexão no arquivo Web. config implantado porque **Use essa cadeia de caracteres de conexão em tempo de execução** está selecionado.

Também no **SchoolContext**, selecione **aplicar migrações do Code First**. Essa opção faz com que o processo de implantação configurar o arquivo Web. config implantado para especificar o `MigrateDatabaseToLatestVersion` inicializador. Esse inicializador atualiza automaticamente o banco de dados para a versão mais recente quando o aplicativo acessa o banco de dados pela primeira vez após a implantação.

Na caixa de cadeia de caracteres de conexão para **DefaultConnection**, digite o seguinte valor:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Deixe **Atualizar banco de dados** desmarcada. O banco de dados de associação será implantado copiando o arquivo. sdf no aplicativo\_dados e você não deseja que o processo de implantação faça qualquer coisa com esse banco de dados.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Clique em **próxima** para ir para o **visualização** guia.

No **versão prévia** , clique em **iniciar visualização** para ver uma lista dos arquivos que serão copiados.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Clique em **Publicar**.

Se o Visual Studio não está no modo de administrador, você poderá receber uma mensagem de erro que indica um erro de permissões. Nesse caso, feche o Visual Studio, abra-o no modo de administrador e tente publicar novamente.

Se o Visual Studio está no modo de administrador, o **saída** janela relatórios bem-sucedida compilar e publicar.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

O navegador é aberto automaticamente para a página inicial do Contoso University em execução no IIS no computador local.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Teste no ambiente de teste

Observe que o indicador de ambiente mostra "(teste)" em vez de "(desenvolvimento)", que mostra que o *Web. config* transformação para o indicador de ambiente foi bem-sucedida.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Execute o **alunos** página para verificar se o banco de dados implantado não tem nenhum aluno. Quando você seleciona essa página, ele pode levar alguns minutos para carregar porque o Code First cria o banco de dados e, em seguida, executa o `Seed` método. (Ele não fez isso quando você estava na home page, porque o aplicativo não tente acessar o banco de dados ainda.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Execute o **instrutores** página para verificar se que o Code First propagado o banco de dados do instrutor:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Selecione **adicionar alunos** da **alunos** menu, adicionar um aluno e, em seguida, exibir o novo aluno no **alunos** página para verificar se que você pode escrever com êxito no banco de dados :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Dos **cursos** menu, selecione **atualização créditos**. O **créditos de atualização** página requer permissões de administrador, portanto, o **Log In** página é exibida. Insira as credenciais da conta de administrador que você criou anteriormente ("admin" e "Pas$ w0rd"). O **atualização créditos** página for exibida, que verifica que a conta de administrador que você criou no tutorial anterior foi implantada corretamente no ambiente de teste.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Verificar se um *Elmah* pasta existe com apenas o arquivo de espaço reservado para ele.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisar as alterações da Web. config automática para migrações do Code First

Abra o *Web. config* arquivo no aplicativo implantado na *C:\inetpub\wwwroot\ContosoUniversity* e você pode ver onde o processo de implantação configurado migrações do Code First para automaticamente Atualize o banco de dados para a versão mais recente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

O processo de implantação também criou uma nova cadeia de conexão para migrações do Code First exclusivamente atualizar o esquema de banco de dados:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Essa cadeia de caracteres de conexão adicional permite que você especifique uma conta de usuário para atualizações de esquema de banco de dados e uma conta de usuário diferentes para acesso de dados do aplicativo. Por exemplo, você pode atribuir o BD\_função de proprietário para migrações do Code First e o banco de dados\_datareader e o banco de dados\_datawriter funções para o aplicativo. Isso é um padrão comum de defesa em profundidade que impede que o código potencialmente mal-intencionado no aplicativo de alterar o esquema de banco de dados. (Por exemplo, isso pode acontecer em um ataque de injeção de SQL.) Esse padrão não é usado por esses tutoriais. Não se aplica ao SQL Server Compact, e não se aplica quando você migra para o SQL Server em um tutorial posterior nesta série. O site de Cytanium oferece apenas uma conta de usuário para acessar o banco de dados do SQL Server que você criou em Cytanium. Se você for capaz de implementar esse padrão em seu cenário, você pode fazer isso executando as seguintes etapas:

1. No **as configurações** guia da **publicar na Web** assistente, insira a cadeia de caracteres de conexão que especifica um usuário com permissões de atualização do esquema de banco de dados completo e desmarque o **usar essa cadeia de caracteres de conexão em tempo de execução** caixa de seleção. No arquivo Web. config implantado, isso se torna o `DatabasePublish` cadeia de caracteres de conexão.
2. Crie uma transformação do arquivo Web. config para a cadeia de caracteres de conexão que você deseja que o aplicativo para usar em tempo de execução.

Agora você implantou seu aplicativo ao IIS no computador de desenvolvimento e testá-lo lá. Isso verifica se o processo de implantação copiados de conteúdo do aplicativo para o local certo (excluindo os arquivos que você não quiser implantar), e também essa implantação da Web configurado IIS corretamente durante a implantação. No próximo tutorial, você executará mais um teste que localiza uma tarefa de implantação que ainda não foi feita: definindo permissões de pasta na *Elmah* pasta.

## <a name="more-information"></a>Mais informações

Para obter informações sobre como executar o IIS ou IIS Express no Visual Studio, consulte os seguintes recursos:

- [Visão geral do IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) no site IIS.net.
- [Introdução ao IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) no blog de Guthrie.
- [Como: especificar o servidor Web para projetos Web no Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Principais diferenças entre o IIS e o ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) no site do ASP.NET.
- [Testar a ASP.NET MVC ou o aplicativo de formulários da Web no IIS 7 em 30 segundos](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) no blog de Rick Anderson. Essa entrada fornece exemplos de por que testar com o Visual Studio Development Server (Cassini) não é tão confiável quanto o teste no IIS Express e por testes no IIS Express não é tão confiável quanto o teste no IIS.

Para obter informações sobre quais problemas podem surgir quando seu aplicativo é executado em confiança média, consulte [hospedando aplicativos do ASP.NET em confiança média](http://www.4guysfromrolla.com/articles/100307-1.aspx) em que a equipe de site Rolla de 4.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
