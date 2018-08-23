---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Implantação de Web do ASP.NET usando o Visual Studio: implantação em produção | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: f71d8311cbb1131d9c30c0bd9071a1c6c90f9976
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834988"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Implantação de Web do ASP.NET usando o Visual Studio: implantação em produção
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Neste tutorial, você configurar uma conta do Microsoft Azure, cria ambientes de preparo e produção e implanta seu aplicativo web ASP.NET de preparo e recurso de publicação de ambientes de produção usando o Visual Studio em um único clique.

Se você preferir, você pode implantar em um provedor de hospedagem de terceiros. A maioria dos procedimentos descritos neste tutorial é as mesmas para um provedor de hospedagem ou do Azure, exceto que cada provedor tem sua própria interface de usuário para o gerenciamento de conta e o site da web. Você pode encontrar um provedor de hospedagem na [Galeria de provedores de](https://www.microsoft.com/web/hosting) no site da web do Microsoft.com.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, certifique-se de verificar a página de solução de problemas nessa série de tutoriais.

## <a name="get-a-microsoft-azure-account"></a>Obtenha uma conta do Microsoft Azure

Se você ainda não tiver uma conta do Azure, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Criar um ambiente de preparo

> [!NOTE]
> Uma vez que este tutorial foi escrito, o serviço de aplicativo do Azure adicionou um novo recurso para automatizar vários processos para a criação de ambientes de preparo e produção. Ver [configurar ambientes de preparo para aplicativos web no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Conforme explicado a [implantar para o tutorial do ambiente de teste](deploying-to-iis.md), a maioria dos ambiente de teste confiáveis é um site da web no provedor de hospedagem que é exatamente como o site da web de produção. Muitos provedores de hospedagem, seria necessário ponderar os benefícios deste significativo custo adicional, mas no Azure, você pode criar um aplicativo web gratuito adicionais como seu aplicativo de preparo. Você também precisa de um banco de dados, e a despesa adicional para fazer isso pela despesa de seu banco de dados de produção será nenhum ou mínimo. No Azure você paga pela quantidade de armazenamento de banco de dados usado em vez de para cada banco de dados e a quantidade de armazenamento adicional, que você usará no ambiente de preparo será mínima.

Conforme explicado a [implantar para o tutorial do ambiente de teste](deploying-to-iis.md), em preparação e produção que você pretende implantar dois bancos de dados em um banco de dados. Se você quiser mantê-los separados, o processo seria o mesmo, exceto que você deve criar um banco de dados adicional para cada ambiente e selecione a cadeia de caracteres de destino correto para cada banco de dados quando você cria o perfil de publicação.

Nesta seção do tutorial, você vai criar um aplicativo web e o banco de dados a ser usado para o ambiente de preparo, e você vai implantar no preparo e teste existe antes de criar e implantar o ambiente de produção.

> [!NOTE]
> As etapas a seguir mostram como criar um aplicativo web no serviço de aplicativo do Azure usando o portal de gerenciamento do Azure. Na versão mais recente do SDK do Azure, você também pode fazer isso sem sair do Visual Studio, usando o Gerenciador de servidores. No Visual Studio 2013, você também pode criar um aplicativo web diretamente na caixa de diálogo Publicar. Para obter mais informações, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. No [Portal de gerenciamento](https://manage.windowsazure.com/), clique em **sites**e, em seguida, clique em **New**.
2. Clique em **site**e, em seguida, clique em **criação personalizada**.

    O **novo site – criação personalizada** assistente é aberto. O **criação personalizada** assistente permite que você crie um site da web e um banco de dados ao mesmo tempo.
3. No **criar site** etapa do assistente, insira uma cadeia de caracteres de **URL** caixa para usar como a URL exclusiva para seu aplicativo do ambiente de preparo. Por exemplo, digite ContosoUniversity-staging123 (incluindo números aleatórios no final para torná-la exclusiva, no caso de preparo ContosoUniversity é obtido).

    A URL completa consistirá em que você inserir aqui mais o sufixo que você vê ao lado da caixa de texto.
4. No **região** lista suspensa, escolha a região mais próxima a você.

    Essa configuração especifica qual data center que seu aplicativo web será executado em.
5. No **banco de dados** lista suspensa, escolha **criar um novo banco de dados do SQL**.
6. No **nome de cadeia de caracteres de Conexão do BD** caixa, deixe o valor padrão, *DefaultConnection*.
7. Clique na seta que aponta para a direita na parte inferior da caixa.

    A ilustração a seguir mostra a **criar site** caixa de diálogo com os valores de exemplo nele. A URL e a região que você inseriu será diferente.

    ![Criar site etapa](deploying-to-production/_static/image1.png)

    O assistente avança para o **especificar configurações de banco de dados** etapa.
8. No **nome** , digite *ContosoUniversity* além de um número aleatório para torná-la exclusiva, por exemplo *ContosoUniversity123*.
9. No **Server** caixa, selecione **novo servidor de banco de dados SQL**.
10. Insira um nome de administrador e senha.

    Você não inserir um nome e senha existentes aqui. Você está inserindo um novo nome e senha que você está definindo agora para usar mais tarde ao acessar o banco de dados.
11. No **região** , escolha a mesma região que você escolheu para o aplicativo web.

    Manter o servidor web e o servidor de banco de dados na mesma região lhe dá o melhor desempenho e minimiza as despesas.
12. Clique na marca de seleção na parte inferior da caixa para indicar que é concluído.

    A ilustração a seguir mostra a **especificar configurações de banco de dados** caixa de diálogo com os valores de exemplo nele. Os valores inseridos por você podem ser diferentes.

    ![Etapa de configurações de banco de dados do site de New - criar com o Assistente de banco de dados](deploying-to-production/_static/image2.png)

    O Portal de gerenciamento retorna para a página de sites e o **Status** coluna mostra que o aplicativo web está sendo criado. Depois de algum tempo (normalmente menos de um minuto), o **Status** coluna mostra o aplicativo web foi criado com êxito. Na barra de navegação à esquerda, o número de aplicativos web que você tem em sua conta é exibido ao lado de **sites** ícone e o número de bancos de dados é exibido ao lado de **bancos de dados SQL** ícone.

    ![Página de Sites da Web do Portal de gerenciamento do site criado](deploying-to-production/_static/image3.png)

    Nome do aplicativo web será diferente do aplicativo de exemplo na ilustração.

## <a name="deploy-the-application-to-staging"></a>Implantar o aplicativo para o preparo

Agora que você criou um aplicativo web e o banco de dados para o ambiente de preparo, você pode implantar o projeto.

> [!NOTE]
> Estas instruções mostram como criar um perfil de publicação, baixando uma *. publishsettings* arquivo, que funciona não só para o Azure, mas também para provedores de hospedagem de terceiros. A versão mais recente do SDK do Azure também permite que você se conectar diretamente no Azure do Visual Studio e escolha em uma lista de aplicativos web que você tem em sua conta do Azure. No Visual Studio 2013, você pode entrar no Azure a partir de **Web Publish** caixa de diálogo ou do **Gerenciador de servidores** janela. Para obter mais informações, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Baixe o arquivo. publishsettings

1. Clique no nome do aplicativo web que você acabou de criar.

    ![Clique no site para ir para o painel](deploying-to-production/_static/image4.png)
2. Sob **visão geral rápida** na **Dashboard** , clique em **baixar perfil de publicação**.

    ![Baixar o link do perfil de publicação](deploying-to-production/_static/image5.png)

    Esta etapa baixa um arquivo que contém todas as configurações que você precisa para implantar um aplicativo em seu aplicativo web. Você importará esse arquivo para o Visual Studio para que você não precise inserir essas informações manualmente.
3. Salvar a *. publishsettings* arquivo em uma pasta que você pode acessar a partir do Visual Studio.

    ![salvando o arquivo. publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Security - a *. publishsettings* arquivo contém suas credenciais (sem codificação) que são usados para administrar suas assinaturas do Azure e serviços. A prática recomendada de segurança para esse arquivo é armazená-lo temporariamente fora dos diretórios de origem (por exemplo, na pasta Libraries\Documents) e, em seguida, exclua-o após a importação for concluída. Um usuário mal-intencionado que consiga acessar o *. publishsettings* arquivo pode editar, criar e excluir seus serviços do Azure.

### <a name="create-a-publish-profile"></a>Criar um perfil de publicação

1. No Visual Studio, clique com botão direito no projeto ContosoUniversity no **Gerenciador de soluções** e selecione **publicar** no menu de contexto.

    O **publicar na Web** assistente é aberto.
2. Clique o **perfil** guia.
3. Clique em **Importar**.
4. Navegue até a *. publishsettings* arquivo que você baixou anteriormente e, em seguida, clique em **abrir**.

    ![Caixa de diálogo Importar configurações de publicação](deploying-to-production/_static/image7.png)
5. No **Conexão** , clique em **validar Conexão** para certificar-se de que as configurações estão corretas.

    Quando a conexão tiver sido validado, uma marca de seleção verde é mostrada ao lado de **validar Conexão** botão.

    Para alguns provedores de hospedagem, quando você clica **validar Conexão**, você poderá ver uma **erro de certificado** caixa de diálogo. Se você fizer isso, verifique se que o nome do servidor é o esperado. Se o nome do servidor está correto, selecione **salvar este certificado para sessões futuras do Visual Studio** e clique em **Accept**. (Esse erro significa que o provedor de hospedagem tem escolhidos para evitar a despesa de comprar um certificado SSL para a URL que você está implantando. Se você preferir estabelecer uma conexão segura usando um certificado válido, entre em contato com seu provedor de hospedagem.)
6. Clique em **Avançar**.

    ![ícone de conexão bem-sucedida e o botão Avançar na guia Conexão](deploying-to-production/_static/image8.png)
7. No **as configurações** guia, expanda **opções de publicação do arquivo**e, em seguida, selecione **excluir arquivos do aplicativo\_pasta dados**.

    Para obter informações sobre as outras opções sob **opções de publicação do arquivo**, consulte o [implantando no IIS](deploying-to-iis.md) tutorial. A captura de tela que mostra o resultado desta etapa e as seguintes etapas de configuração do banco de dados está no final das etapas de configuração de banco de dados.
8. Sob **DefaultConnection** na **bancos de dados** seção, configura a implantação de banco de dados para o banco de dados de associação.
9. 1. Selecione **Atualizar banco de dados**.

        O **cadeia de caracteres de conexão remota** caixa diretamente abaixo **DefaultConnection** será preenchido com a cadeia de caracteres de conexão do arquivo. publishsettings. A cadeia de caracteres de conexão inclui as credenciais do SQL Server, que são armazenadas em texto sem formatação na *. pubxml* arquivo. Se você preferir não armazená-las permanentemente lá, você pode removê-los do perfil de publicação depois que o banco de dados é implantado e armazená-las no Azure. Para obter mais informações, consulte [como manter seu banco de dados do ASP.NET cadeias de caracteres de conexão segura ao implantar no Azure de origem](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) no blog de hanselman.
      2. Clique em **configurar atualizações de banco de dados**.
      3. No **configurar atualizações de banco de dados** caixa de diálogo, clique em **Adicionar Script do SQL**.
      4. No **Adicionar Script do SQL** , navegue até a *aspnet-data-prod.sql* script que você salvou anteriormente na pasta da solução e, em seguida, clique em **abrir**.
      5. Fechar o **configurar atualizações de banco de dados** caixa de diálogo.
10. Sob **SchoolContext** na **bancos de dados** seção, selecione **executar migrações do Code First (executado na inicialização do aplicativo)**.

    O Visual Studio exibe **executar migrações do Code First** em vez de **Atualizar banco de dados** para `DbContext` classes. Se você deseja usar o provedor dbDacFx em vez de migrações para implantar um banco de dados que você acessa usando um `DbContext` classe, consulte [como implantar um banco de dados Code First sem migrações?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nas perguntas Frequentes de implantação da Web para o Visual Studio e o ASP.NET no MSDN.

    O **configurações** guia agora se parece com o exemplo a seguir:

    ![Guia de configurações para o preparo](deploying-to-production/_static/image9.png)
11. Execute as seguintes etapas para salvar o perfil e renomeie-o para *preparo*:

    1. Clique o **perfil** guia e, em seguida, clique em **gerenciar perfis**.
    2. A importação criados dois novos perfis, um para FTP e outro para a implantação da Web. Você configurou o perfil de implantação da Web: renomear esse perfil seja *preparo*.

        ![Renomear perfil para o preparo](deploying-to-production/_static/image10.png)
    3. Fechar o **editar perfis de publicação de Web** caixa de diálogo.
    4. Fechar o **publicar na Web** assistente.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurar uma transformação de perfil de publicação para o indicador de ambiente

> [!NOTE]
> Esta seção mostra como configurar uma transformação de Web. config para o indicador de ambiente. Como o indicador é no `<appSettings>` elemento, você tem outra alternativa para especificar a transformação quando você estiver implantando no serviço de aplicativo do Azure. Para obter mais informações, consulte [Web. config especificando configurações no Azure](web-config-transformations.md#watransforms).


1. Na **Gerenciador de soluções**, expanda **Properties**e, em seguida, expanda **PublishProfiles**.
2. Clique com botão direito *Staging.pubxml*e, em seguida, clique em **adicionar Config transformar**.

    ![Adicionar transformação de Config para o preparo](deploying-to-production/_static/image11.png)

    O Visual Studio cria o *staging* arquivo de transformação e o abre.
3. No *staging* transformar o arquivo, insira o seguinte código imediatamente após a abertura `configuration` marca.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Quando você usa o perfil de publicação de preparo, essa transformação define o indicador de ambiente para "Prod". No aplicativo web implantado, você não verá nenhum sufixo, como "(desenvolvimento)" ou "(teste)" após o cabeçalho H1 de "Contoso University".
4. Clique com botão direito do *staging* de arquivo e clique em **visualização transformar** para certificar-se de que a transformação que você codificou produz as alterações esperadas.

    O **visualização da Web. config** janela mostra o resultado da aplicação de ambos os *Release* transforma e o *staging* transforma.

### <a name="prevent-public-use-of-the-test-app"></a>Impedir o uso público do aplicativo de teste

Uma consideração importante para o aplicativo de preparo é que ele estará em tempo real na Internet, mas você não deseja usá-lo público. Para minimizar a probabilidade de que as pessoas encontrará e usá-lo, você pode usar um ou mais dos seguintes métodos:

- Definir regras de firewall que permitem o acesso apenas de endereços IP que você pode usar para testar a preparação para o aplicativo de preparo.
- Use uma URL ofuscada que seria impossível de adivinhar.
- Criar uma *robots* arquivo para garantir que os mecanismos de pesquisa serão não rastrear os links de relatório e o aplicativo de teste a ele nos resultados da pesquisa.

O primeiro desses métodos é mais eficiente, mas não é abordado neste tutorial, porque ela requer que você implanta um serviço de nuvem do Azure em vez do serviço de aplicativo do Azure. Para obter mais informações sobre serviços de nuvem e as restrições de IP no Azure, consulte [computação hospedando opções fornecidas pelo Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) e [bloco de endereços IP específicos de acessar uma função Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Se você estiver implantando em um provedor de hospedagem de terceiros, contate o provedor para descobrir como implementar restrições de IP.

Para este tutorial, você criará um *robots* arquivo.

1. Na **Gerenciador de soluções**, clique com botão direito no projeto ContosoUniversity e clique em **Adicionar Novo Item**.
2. Criar um novo **arquivo de texto** denominado *robots*e colocar o seguinte texto nele:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    O `User-agent` linha informa os mecanismos de pesquisa que se aplicam as regras no arquivo para rastreadores de web de mecanismo de pesquisa todos os (robôs), e o `Disallow` linha especifica que nenhuma página no site deve ser rastreada.

    Você deseja que os mecanismos de pesquisa do catálogo seu aplicativo de produção, portanto, você precisará excluir este arquivo de implantação de produção. Para fazer o que, você configurará uma configuração na produção perfil de publicação ao criá-lo.

### <a name="deploy-to-staging"></a>Implantar no preparo

1. Abra o **Publicar Web** assistente clicando com botão direito no projeto do Contoso University e clicando em **publicar**.
2. Certifique-se de que o **preparo** perfil for selecionado.
3. Clique em **Publicar**.

    O **saída** janela mostra quais ações de implantação foram executadas e relata a conclusão bem-sucedida da implantação. O navegador padrão abre automaticamente a URL do aplicativo web implantado.

## <a name="test-in-the-staging-environment"></a>Teste no ambiente de preparo

Observe que o indicador de ambiente esteja ausente (não há nenhum "(teste)" ou "(desenvolvimento)" após o título H1, que mostra que o *Web. config* transformação para o indicador de ambiente foi bem-sucedida.

![Home page preparo](deploying-to-production/_static/image12.png)

Execute o **alunos** página para verificar se o banco de dados implantado não tem nenhum aluno.

Execute o **instrutores** página para verificar se que o Code First propagado o banco de dados do instrutor:

Selecione **adicionar alunos** da **alunos** menu, adicionar um aluno e, em seguida, exibir o novo aluno no **alunos** página para verificar se que você pode escrever com êxito no banco de dados .

Dos **cursos** , clique em **atualização créditos**. O **créditos de atualização** página requer permissões de administrador, portanto, o **Log In** página é exibida. Insira as credenciais de conta de administrador que você criou anteriormente ("admin" e "prodpwd"). O **atualização créditos** página for exibida, que verifica que a conta de administrador que você criou no tutorial anterior foi implantada corretamente no ambiente de teste.

Solicite uma URL inválida para causar um erro que ELMAH rastrear e, em seguida, solicitar o relatório de erros do ELMAH. Se você estiver implantando em um provedor de hospedagem de terceiros, você provavelmente perceberá que o relatório está vazio, pelo mesmo motivo que ele estava vazio no tutorial anterior. Você precisará usar as ferramentas de gerenciamento de conta do provedor de hospedagem para configurar permissões de pasta para habilitar o ELMAH gravar na pasta de log.

O aplicativo que você criou agora está em execução na nuvem em um aplicativo web que é exatamente como o que você usará para a produção. Uma vez que tudo está funcionando corretamente, a próxima etapa é implantar em produção.

## <a name="deploy-to-production"></a>Implantar na produção

O processo para criar um aplicativo web de produção e implantação em produção é da mesma maneira que a preparação, exceto que você precisará excluir o *robots* da implantação. Para fazer isso, você editará o arquivo de perfil de publicação.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Crie o ambiente de produção e a produção de perfil de publicação

1. Crie o aplicativo web de produção e o banco de dados no Azure, seguindo o mesmo procedimento usado para a preparação.

    Quando você cria o banco de dados, você pode optar por colocá-lo no mesmo servidor que você criou anteriormente ou criar um novo servidor.
2. Baixe o *. publishsettings* arquivo.
3. Criar o perfil de publicação com a importação de produção *. publishsettings* arquivo, seguindo o mesmo procedimento usado para a preparação.

    Não se esqueça de configurar o script de implantação de dados sob **DefaultConnection** na **bancos de dados** seção o **configurações** guia.
4. Renomear perfil de publicação para *produção*.
5. Configurar uma transformação de perfil de publicação para o indicador de ambiente, seguindo o mesmo procedimento usado para o preparo...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edite o arquivo. pubxml para excluir robots. txt

Os arquivos são nomeados de perfil de publicação &lt;profilename&gt;*. pubxml* e estão localizados na *PublishProfiles* pasta. O *PublishProfiles* pasta está sob o *propriedades* pasta em um aplicativo web c# do projeto, no *meu projeto* pasta em um projeto de aplicativo da web VB ou em um de *App\_dados* pasta em um projeto de aplicativo web. Cada *. pubxml* arquivo contém as configurações que se aplicam a um perfil de publicação. Os valores inseridos no Assistente de publicação na Web são armazenados nesses arquivos, e você pode editá-los para criar ou alterar as configurações que não estão disponíveis na IU do Visual Studio.

Por padrão, *. pubxml* arquivos são incluídos no projeto quando você criar um perfil de publicação, mas você pode excluí-los do projeto e Visual Studio ainda irá usá-los. O Visual Studio procura *PublishProfiles* pasta *. pubxml* arquivos, independentemente se eles são incluídos no projeto.

Para cada *. pubxml* arquivo, existe uma *. pubxml* arquivo. O *. pubxml* arquivo contém a senha criptografada se você tiver selecionado o **salvar senha** opção e, por padrão, ele é excluído do projeto.

Um *. pubxml* arquivo contém as configurações que pertencem a um perfil de publicação específica. Se você quiser definir as configurações que se aplicam a todos os perfis, você pode criar uma *. wpp.targets* arquivo. O processo de compilação importa esses arquivos para o *. csproj* ou *. vbproj* arquivo de projeto, portanto, a maioria das configurações que você pode configurar no arquivo de projeto podem ser configuradas nesses arquivos. Para obter mais informações sobre *. pubxml* arquivos e *. wpp.targets* arquivos, consulte [como: Editar configurações de implantação nos arquivos de perfil de publicação (. pubxml) e o. wpp.targets arquivo no Visual Studio Projetos da Web](https://msdn.microsoft.com/library/ff398069.aspx).

1. Na **Gerenciador de soluções**, expanda **Properties** e expanda **PublishProfiles**.
2. Clique com botão direito *Production.pubxml* e clique em **abrir**.

    ![Abra o arquivo. pubxml](deploying-to-production/_static/image13.png)
3. Clique com botão direito *Production.pubxml* e clique em **abrir**.
4. Adicione as seguintes linhas imediatamente antes do fechamento `PropertyGroup` elemento:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    O arquivo. pubxml agora se parece com o exemplo a seguir:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Para obter mais informações sobre como excluir arquivos e pastas, consulte [pode posso excluir arquivos ou pastas específicas da implantação?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) na **perguntas frequentes sobre a implantação da Web para Visual Studio e o ASP.NET** no MSDN.

### <a name="deploy-to-production"></a>Implantar na produção

1. Abra o **publicar na Web** Assistente para ter certeza de que o **produção** perfil de publicação está selecionado e, em seguida, clique em **iniciar visualização** no **visualização**guia para verificar se o *robots* arquivo não será copiado para o aplicativo de produção.

    ![Visualização de arquivos a serem publicados para a produção](deploying-to-production/_static/image14.png)

    Examine a lista de arquivos que serão copiados. Você verá que todos os *. CS* arquivos, incluindo *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, e  *Master.Designer.CS* arquivos são omitidos. Todo esse código tiver sido compilado na *contosouniversity. dll* e *ContosUniversity.pdb* arquivos que você encontrará o *bin* pasta. Porque somente o *. dll* é necessária para executar o aplicativo e você especificou anteriormente que devem ser implantados somente os arquivos necessários para executar o aplicativo, não *CS* arquivos foram copiados para o destino ambiente. O *obj* pasta e o *Contosouniversity* e *. csproj* arquivos são omitidos pelo mesmo motivo.

    Clique em **publicar** para implantar o ambiente de produção.
2. Teste em produção, seguindo o mesmo procedimento usado para a preparação.

    Tudo o que é idêntico de preparo, exceto para a URL e a ausência do *robots* arquivo.

## <a name="summary"></a>Resumo

Você agora com êxito implantado e testado seu aplicativo web e ele está disponível publicamente na Internet.

![Home page produção](deploying-to-production/_static/image15.png)

No próximo tutorial, você atualizar o código do aplicativo e implante a alteração para os ambientes de teste, preparação e produção.

> [!NOTE]
> Enquanto seu aplicativo está em uso no ambiente de produção deve implementar um plano de recuperação. Ou seja, você deve ser periodicamente fazendo backup de seus bancos de dados do aplicativo de produção para um local de armazenamento seguro, e você deve manter várias gerações de tais backups. Quando você atualiza o banco de dados, você deve fazer uma cópia de backup de imediatamente antes da alteração. Em seguida, se você comete um erro e não Descubra até depois que você implantou em produção, você ainda poderá recuperar o banco de dados para o estado que estava antes de ele se tornou corrompido. Para obter mais informações, consulte [Backup de banco de dados SQL do Azure e restauração](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> Neste tutorial, o SQL Server edition que você está implantando é banco de dados SQL. Enquanto o processo de implantação é semelhante a outras edições do SQL Server, um aplicativo de produção real pode exigir código especial para o banco de dados SQL em alguns cenários. Para obter mais informações, consulte [trabalhando com o banco de dados SQL](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) e [escolhendo entre o SQL Server e banco de dados SQL](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Anterior](setting-folder-permissions.md)
> [Próximo](deploying-a-code-update.md)
