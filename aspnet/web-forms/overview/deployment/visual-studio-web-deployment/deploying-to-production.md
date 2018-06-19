---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Implantação de Web do ASP.NET usando o Visual Studio: implantação em produção | Microsoft Docs'
author: tdykstra
description: Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: f3b3898bd003ace100ba05619f2c45ca808462df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889801"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Implantação de Web do ASP.NET usando o Visual Studio: implantação em produção
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão Geral

Neste tutorial, você configurar uma conta do Microsoft Azure, cria ambientes de preparo e produção e implanta seu aplicativo web ASP.NET de preparo e ambientes de produção usando o Visual Studio clique publicar o recurso.

Se preferir, você pode implantar em um provedor de hospedagem de terceiros. A maioria dos procedimentos descritos neste tutorial é as mesmas para um provedor de hospedagem ou Azure, exceto pelo fato de cada provedor tem sua própria interface de usuário para o gerenciamento de conta e o site da web. Você pode encontrar um provedor de hospedagem de [Galeria de provedores de](https://www.microsoft.com/web/hosting) no site Microsoft.com.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se de verificar a página de solução de problemas nesta série tutorial.

## <a name="get-a-microsoft-azure-account"></a>Obter uma conta do Microsoft Azure

Se você ainda não tiver uma conta do Azure, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Criar um ambiente de preparo

> [!NOTE]
> Como este tutorial foi escrito, o serviço de aplicativo do Azure adicionado um novo recurso para automatizar muitos processos para a criação de ambientes de preparo e produção. Consulte [configurar ambientes de preparo para aplicativos web no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Conforme explicado no [implantar para o tutorial do ambiente de teste](deploying-to-iis.md), mais o ambiente de teste confiável é um site da web no provedor de hospedagem que é exatamente como o site de produção. Muitos provedores de hospedagem de você precisa ponderar os benefícios disso e custo adicional significativo, mas no Azure, você pode criar um aplicativo web gratuita adicionais como seu aplicativo de teste. Você também precisa de um banco de dados, e a despesa adicional para que sobre a despesa de seu banco de dados de produção será nenhum ou mínimo. No Azure, você paga para a quantidade de armazenamento de banco de dados usado em vez de para cada banco de dados e a quantidade de armazenamento adicional que você usará em preparo será mínima.

Conforme explicado no [implantar para o tutorial do ambiente de teste](deploying-to-iis.md), em preparação e produção que você pretende implantar dois bancos de dados em um banco de dados. Se você quiser mantê-los separados, o processo será a mesma exceto que você deve criar um banco de dados adicional para cada ambiente, e você deve selecionar a cadeia de caracteres de destino correto para cada banco de dados quando você cria o perfil de publicação.

Esta seção do tutorial você criará um aplicativo web e o banco de dados a ser usado para o ambiente de preparo, e você implantar a preparação e teste existe antes de criar e implantar o ambiente de produção.

> [!NOTE]
> As etapas a seguir mostram como criar um aplicativo web no serviço de aplicativo do Azure usando o portal de gerenciamento do Azure. A versão mais recente do SDK do Azure, você também pode fazer isso sem sair do Visual Studio, usando o Gerenciador de servidores. No Visual Studio 2013, você também pode criar um aplicativo web diretamente na caixa de diálogo Publicar. Para obter mais informações, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. No [Portal de gerenciamento](https://manage.windowsazure.com/), clique em **sites**e, em seguida, clique em **novo**.
2. Clique em **site**e, em seguida, clique em **criação personalizada**.

    O **novo site da Web - criação personalizada** assistente é aberto. O **criação personalizada** assistente permite que você crie um site da web e um banco de dados ao mesmo tempo.
3. No **criar site** etapa do assistente, insira uma cadeia de caracteres de **URL** caixa para usar como a URL exclusiva para seu aplicativo do ambiente de preparo. Por exemplo, digite ContosoUniversity-staging123 (incluindo números aleatórios no final para torná-lo exclusivo no caso de preparo ContosoUniversity é obtido).

    A URL completa consistirá em que você digitar aqui e o sufixo que você vê ao lado da caixa de texto.
4. No **região** lista suspensa, escolha a região que está mais próxima de você.

    Essa configuração especifica que o aplicativo web será executado do data center.
5. No **banco de dados** lista suspensa, escolha **criar um novo banco de dados do SQL**.
6. No **nome de cadeia de caracteres de Conexão do banco de dados** caixa, deixe o valor padrão, *DefaultConnection*.
7. Clique na seta que aponta para a direita na parte inferior da caixa.

    A ilustração a seguir mostra o **criar site** caixa de diálogo com valores de exemplo nela. A URL e a região que você inseriu será diferente.

    ![Criar site etapa](deploying-to-production/_static/image1.png)

    O assistente avança para o **especificar configurações de banco de dados** etapa.
8. No **nome** , digite *ContosoUniversity* mais um número aleatório para torná-lo exclusivo, por exemplo *ContosoUniversity123*.
9. No **servidor** selecione **novo servidor de banco de dados do SQL**.
10. Insira um nome de administrador e senha.

    Você não digitar um nome existente e a senha aqui. Você está inserindo um novo nome e uma senha que você está definindo agora para usar mais tarde, quando você acessa o banco de dados.
11. No **região** caixa, escolha a mesma região que você escolheu para o aplicativo web.

    Manter o servidor web e o servidor de banco de dados na mesma região oferece o melhor desempenho e reduz as despesas.
12. Clique na marca de seleção na parte inferior da caixa para indicar que você tiver terminado.

    A ilustração a seguir mostra o **especificar configurações de banco de dados** caixa de diálogo com valores de exemplo nela. Os valores que você inseriu podem ser diferentes.

    ![Etapa de configurações de banco de dados do site de New - criar com o Assistente de banco de dados](deploying-to-production/_static/image2.png)

    O Portal de gerenciamento retorna para a página de sites e o **Status** coluna mostra que o aplicativo web está sendo criado. Após alguns instantes (geralmente menor que um minuto), o **Status** coluna mostra que o aplicativo web foi criado com êxito. Na barra de navegação à esquerda, o número de aplicativos da web que você tem em sua conta aparece ao lado a **sites** ícone e o número de bancos de dados aparece ao lado de **bancos de dados SQL** ícone.

    ![Página de Sites da Web do Portal de gerenciamento, o site da web criada](deploying-to-production/_static/image3.png)

    O nome do aplicativo web será diferente do aplicativo de exemplo na ilustração.

## <a name="deploy-the-application-to-staging"></a>Implantar o aplicativo em teste

Agora que você criou um aplicativo web e o banco de dados para o ambiente de preparo, você pode implantar o projeto.

> [!NOTE]
> Estas instruções mostram como criar um perfil de publicação, baixando um *. publishsettings* arquivo, que não só funciona para Azure mas também para provedores de hospedagem de terceiros. A versão mais recente do SDK do Azure também permite que você se conectar diretamente ao Azure do Visual Studio e escolha em uma lista de aplicativos web que você tem em sua conta do Azure. No Visual Studio 2013, você pode entrar Azure a partir de **Publicar Web** caixa de diálogo ou do **Server Explorer** janela. Para obter mais informações, consulte [criar um aplicativo web ASP.NET no serviço de aplicativo do Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Baixe o arquivo. publishsettings

1. Clique no nome do aplicativo web que você acabou de criar.

    ![Clique no site para ir para o painel](deploying-to-production/_static/image4.png)
2. Em **visão rápida** no **painel** , clique em **baixar perfil de publicação**.

    ![Perfil de publicação de link de download](deploying-to-production/_static/image5.png)

    Esta etapa baixa um arquivo que contém todas as configurações que você precisa para implantar um aplicativo em seu aplicativo web. Você vai importar esse arquivo para o Visual Studio para que você não precisa inserir essas informações manualmente.
3. Salve o *. publishsettings* arquivo em uma pasta que você pode acessar no Visual Studio.

    ![Salvar o arquivo. publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Segurança - o *. publishsettings* arquivo contém suas credenciais (sem codificação) que são usados para administrar suas assinaturas do Azure e serviços. A prática recomendada de segurança para esse arquivo é armazená-lo temporariamente fora dos diretórios de origem (por exemplo, na pasta bibliotecas\documentos) e, em seguida, excluí-la depois que a importação for concluída. Um usuário mal-intencionado que consiga acessar o *. publishsettings* arquivo pode editar, criar e excluir seus serviços do Azure.

### <a name="create-a-publish-profile"></a>Criar um perfil de publicação

1. No Visual Studio, clique com botão direito no projeto ContosoUniversity no **Solution Explorer** e selecione **publicar** no menu de contexto.

    O **Publicar Web** assistente é aberto.
2. Clique o **perfil** guia.
3. Clique em **Importar**.
4. Navegue até o *. publishsettings* arquivo que você baixou anteriormente e, em seguida, clique em **abrir**.

    ![Caixa de diálogo Importar configurações de publicação](deploying-to-production/_static/image7.png)
5. No **Conexão** , clique em **Conexão validar** para certificar-se de que as configurações estão corretas.

    Quando a conexão foi validada, uma marca de seleção verde é exibida ao lado de **Conexão validar** botão.

    Para alguns provedores de hospedagem, quando você clica em **Conexão validar**, talvez você veja uma **erro de certificado** caixa de diálogo. Se você fizer isso, verifique se o nome do servidor que o esperado. Se o nome do servidor está correto, selecione **salvar este certificado para sessões futuras do Visual Studio** e clique em **aceitar**. (Esse erro significa que o provedor de hospedagem tem escolhidos para evitar a despesa de comprar um certificado SSL para a URL que você está implantando. Se você preferir estabelecer uma conexão segura usando um certificado válido, entre em contato com seu provedor de hospedagem.)
6. Clique em **Avançar**.

    ![ícone de conexão bem-sucedida e o botão Avançar na guia Conexão](deploying-to-production/_static/image8.png)
7. No **configurações** guia, expanda **opções de publicação do arquivo**e, em seguida, selecione **excluir arquivos do aplicativo\_pasta dados**.

    Para obter informações sobre as outras opções sob **opções de publicação do arquivo**, consulte o [Implantando ao IIS](deploying-to-iis.md) tutorial. A captura de tela que mostra o resultado desta etapa e as seguintes etapas de configuração de banco de dados está no final das etapas de configuração de banco de dados.
8. Em **DefaultConnection** no **bancos de dados** seção, configurar a implantação de banco de dados para o banco de dados de associação.
9. 1. Selecione **Atualizar banco de dados**.

        O **cadeia de caracteres de conexão remota** caixa diretamente abaixo **DefaultConnection** é preenchido com a cadeia de caracteres de conexão do arquivo. publishsettings. A cadeia de caracteres de conexão contém credenciais do SQL Server, que são armazenadas em texto sem formatação no *. pubxml* arquivo. Se você preferir não armazená-las permanentemente lá, você pode removê-los do perfil de publicação após a implantação do banco de dados e armazená-las no Azure. Para obter mais informações, consulte [como proteger seu banco de dados do ASP.NET cadeias de caracteres de conexão durante a implantação no Azure de origem](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) no blog de Scott Hanselman.
      2. Clique em **configurar atualizações de banco de dados**.
      3. No **configurar atualizações de banco de dados** caixa de diálogo, clique em **Adicionar Script do SQL**.
      4. No **Adicionar Script do SQL** , navegue até o *aspnet de dados de prod.sql* script que você salvou anteriormente na pasta da solução e, em seguida, clique em **abrir**.
      5. Fechar o **configurar atualizações de banco de dados** caixa de diálogo.
10. Em **SchoolContext** no **bancos de dados** seção, selecione **executar migrações do Code First (executado na inicialização do aplicativo)**.

    O Visual Studio exibe **executar migrações do Code First** em vez de **Atualizar banco de dados** para `DbContext` classes. Se você deseja usar o provedor dbDacFx em vez de migrações para implantar um banco de dados que você acessa usando um `DbContext` de classe, consulte [como implantar a um banco de dados Code First sem migrações?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nas perguntas Frequentes de implantação da Web para o Visual Studio e o ASP.NET no MSDN.

    O **configurações** guia agora é semelhante ao exemplo a seguir:

    ![Guia de configurações de preparação](deploying-to-production/_static/image9.png)
11. Execute as seguintes etapas para salvar o perfil e renomeá-lo como *preparo*:

    1. Clique o **perfil** guia e, em seguida, clique em **gerenciar perfis**.
    2. A importação criados dois novos perfis, uma para o FTP e outra para a implantação da Web. Você configurou o perfil de implantação da Web: renomear esse perfil para *preparo*.

        ![Renomear o perfil de preparo](deploying-to-production/_static/image10.png)
    3. Fechar o **editar perfis de publicação da Web** caixa de diálogo.
    4. Fechar o **Publicar Web** assistente.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurar uma transformação de perfil de publicação para o indicador de ambiente

> [!NOTE]
> Esta seção mostra como configurar uma transformação do Web. config para o indicador de ambiente. Como o indicador está no `<appSettings>` elemento, você tem outra alternativa para especificar a transformação quando você estiver implantando o serviço de aplicativo do Azure. Para obter mais informações, consulte [Web. config especificando configurações no Azure](web-config-transformations.md#watransforms).


1. Em **Solution Explorer**, expanda **propriedades**e, em seguida, expanda **PublishProfiles**.
2. Clique com botão direito *Staging.pubxml*e, em seguida, clique em **Adicionar configuração transformar**.

    ![Adicionar a transformação de configuração de preparo](deploying-to-production/_static/image11.png)

    O Visual Studio cria o *Web.Staging.config* arquivo de transformação e abri-lo.
3. No *Web.Staging.config* transformar o arquivo, insira o seguinte código imediatamente após a abertura `configuration` marca.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Quando você usa o perfil de publicação de preparação, essa transformação define o indicador de ambiente para "Produção". No aplicativo da web implantados, você não verá nenhum sufixo, como "(desenvolvimento)" ou "(teste)" após o cabeçalho H1 "Contoso University".
4. Com o botão direito do *Web.Staging.config* de arquivo e clique em **visualização transformar** para certificar-se de que a transformação que você codificou produz alterações esperadas.

    O **visualização de Web. config** janela mostra o resultado da aplicação de ambos os *Web.Release.config* transforma e *Web.Staging.config* transforma.

### <a name="prevent-public-use-of-the-test-app"></a>Impedir o uso público do aplicativo de teste

Uma consideração importante para o aplicativo de preparo é que ela será ao vivo na Internet, mas você não deseja usá-lo público. Para minimizar a probabilidade de que as pessoas encontrará e usá-lo, você pode usar um ou mais dos seguintes métodos:

- Definir regras de firewall que permitam o acesso apenas de endereços IP que você pode usar para testar a preparação para o aplicativo de teste.
- Use uma URL ofuscada que seja impossível adivinhar.
- Criar um *robots* arquivo para garantir que os mecanismos de pesquisa não rastreará o os links de relatório e o aplicativo de teste para ele nos resultados da pesquisa.

O primeiro desses métodos é mais eficiente, mas não é abordado neste tutorial, porque ela requer que você implanta um serviço de nuvem do Azure em vez do serviço de aplicativo do Azure. Para obter mais informações sobre serviços de nuvem e as restrições de IP no Azure, consulte [de computação hospedagem opções fornecidas pelo Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) e [blocos de endereços IP específicos de acessar uma função Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Se você estiver implantando em um provedor de hospedagem de terceiros, entre em contato com o provedor para saber como implementar restrições de IP.

Para este tutorial, você criará uma *robots* arquivo.

1. Em **Solution Explorer**, com o botão direito no projeto ContosoUniversity e clique em **Adicionar Novo Item**.
2. Criar um novo **arquivo de texto** chamado *robots*e coloque o seguinte texto nele:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    O `User-agent` linha informa os mecanismos de pesquisa que as regras no arquivo se aplicam a todos os pesquisa mecanismo são (robôs), e o `Disallow` linha especifica que nenhuma página no site deve ser rastreada.

    Você deseja que os mecanismos de pesquisa para seu aplicativo de produção do catálogo, você precisa excluir este arquivo de implantação de produção. Para fazer o que, você vai configurar uma configuração na produção perfil de publicação ao criá-lo.

### <a name="deploy-to-staging"></a>Implantar o preparo

1. Abra o **Publicar Web** assistente clicando duas vezes o projeto University Contoso e clicando em **publicar**.
2. Verifique se o **preparo** perfil for selecionado.
3. Clique em **Publicar**.

    O **saída** janela mostra quais ações de implantação foram realizadas e relata a conclusão com êxito da implantação. O navegador padrão é aberto automaticamente para a URL do aplicativo da web implantados.

## <a name="test-in-the-staging-environment"></a>Teste no ambiente de preparo

Observe que o indicador de ambiente esteja ausente (não há nenhuma "(teste)" ou "(desenvolvimento)" após o cabeçalho H1, que mostra que o *Web. config* transformação para o indicador de ambiente foi bem-sucedida.

![Preparação da home page](deploying-to-production/_static/image12.png)

Execute o **alunos** página para verificar se o banco de dados implantado não tem nenhum alunos.

Execute o **instrutores** página para verificar que o Code First propagado o banco de dados do instrutor:

Selecione **adicionar alunos** do **alunos** menu, adicionar um aluno e, em seguida, exibir o novo aluno no **alunos** página para verificar que você pode escrever com êxito para o banco de dados .

Do **cursos** , clique em **atualização créditos**. O **atualização créditos** página requer permissões de administrador, para que o **logon** página é exibida. Insira as credenciais de conta de administrador que você criou anteriormente ("admin" e "prodpwd"). O **atualização créditos** página é exibida, que verifica se a conta de administrador que você criou no tutorial anterior foi implantada corretamente para o ambiente de teste.

Uma URL inválida para causar um erro que ELMAH rastrear e, em seguida, solicitar o relatório de erros do ELMAH da solicitação. Se você estiver implantando em um provedor de hospedagem de terceiros, você provavelmente perceberá que o relatório está vazio pelo mesmo motivo que estava vazio no tutorial anterior. Você precisará usar ferramentas de gerenciamento de conta do provedor de hospedagem para configurar permissões de pasta para habilitar ELMAH gravar na pasta de log.

O aplicativo que você criou agora está em execução na nuvem em um aplicativo web que é como o que você usará para produção. Como tudo está funcionando corretamente, a próxima etapa é implantar na produção.

## <a name="deploy-to-production"></a>Implantar na produção

O processo para criar um aplicativo da web de produção e implantar na produção é igual de preparo, exceto que você precisa excluir o *robots* da implantação. Para fazer isso, você editará o arquivo de perfil de publicação.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Criar o ambiente de produção e a produção de perfil de publicação

1. Crie o aplicativo web de produção e o banco de dados no Azure, siga o mesmo procedimento usado para a preparação.

    Quando você cria o banco de dados, você pode escolher para colocá-lo no mesmo servidor em que você criou anteriormente ou criar um novo servidor.
2. Baixe o *. publishsettings* arquivo.
3. Criar o perfil de publicação com a importação de produção *. publishsettings* arquivo, seguindo o mesmo procedimento usado para a preparação.

    Não se esqueça de configurar o script de implantação de dados em **DefaultConnection** no **bancos de dados** seção o **configurações** guia.
4. Renomear o perfil de publicação para *produção*.
5. Configurar uma transformação de perfil de publicação para o indicador de ambiente, siga o mesmo procedimento usado para a preparação.

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edite o arquivo. pubxml para excluir robots

Os arquivos são nomeados de perfil de publicação &lt;profilename&gt;*. pubxml* e estão localizados no *PublishProfiles* pasta. O *PublishProfiles* pasta está localizada no *propriedades* pasta em um aplicativo c# da web do projeto, no *meu projeto* pasta em um projeto de aplicativo web VB ou sob a *Aplicativo\_dados* pasta em um projeto de aplicativo web. Cada *. pubxml* arquivo contém configurações que se aplicam a um perfil de publicação. Os valores que você inserir no assistente Publicar Web são armazenados nesses arquivos, e você pode editá-los para criar ou alterar as configurações que não estão disponíveis na interface do usuário do Visual Studio.

Por padrão, *. pubxml* arquivos são incluídos no projeto quando você criar um perfil de publicação, mas você pode excluí-los do projeto e Visual Studio ainda será usá-los. O Visual Studio examina o *PublishProfiles* pasta *. pubxml* arquivos, independentemente se eles estão incluídos no projeto.

Para cada *. pubxml* arquivo há um *. pubxml.user* arquivo. O *. pubxml.user* arquivo contém a senha criptografada se você tiver selecionado o **salvar senha** opção e, por padrão, ele é excluído do projeto.

Um *. pubxml* arquivo contém as configurações que pertencem a um perfil de publicação específica. Se você quiser definir as configurações que se aplicam a todos os perfis, você pode criar um *. wpp.targets* arquivo. O processo de compilação importa esses arquivos para o *. csproj* ou *. vbproj* arquivo de projeto, portanto, a maioria das configurações que você pode configurar o arquivo de projeto pode ser configurada nesses arquivos. Para obter mais informações sobre *. pubxml* arquivos e *. wpp.targets* arquivos, consulte [como: Editar configurações de implantação nos arquivos de perfil de publicação (. pubxml) e o. wpp.targets arquivo no Visual Studio Projetos da Web](https://msdn.microsoft.com/library/ff398069.aspx).

1. Em **Solution Explorer**, expanda **propriedades** e expanda **PublishProfiles**.
2. Clique com botão direito *Production.pubxml* e clique em **abrir**.

    ![Abra o arquivo. pubxml](deploying-to-production/_static/image13.png)
3. Clique com botão direito *Production.pubxml* e clique em **abrir**.
4. Adicione as seguintes linhas imediatamente antes do fechamento `PropertyGroup` elemento:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    O arquivo. pubxml agora se parece com o exemplo a seguir:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Para obter mais informações sobre como excluir arquivos e pastas, consulte [pode, excluir arquivos ou pastas específicas de implantação?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) no **perguntas frequentes sobre a implantação da Web do Visual Studio e ASP.NET** no MSDN.

### <a name="deploy-to-production"></a>Implantar na produção

1. Abra o **publicar na Web** Assistente Certifique-se de que o **produção** perfil de publicação está selecionada e, em seguida, clique em **iniciar visualização** no **visualização**guia para verificar se o *robots* arquivo não será copiado para o aplicativo de produção.

    ![Visualização de arquivos a ser publicado para produção](deploying-to-production/_static/image14.png)

    Examine a lista de arquivos que serão copiados. Você verá que todos os *. CS* arquivos, incluindo *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, e  *Master.Designer.CS* arquivos são omitidos. Todo esse código foi compilado para o *ContosoUniversity.dll* e *ContosUniversity.pdb* arquivos que você encontrará o *bin* pasta. Porque somente o *. dll* é necessário para executar o aplicativo e você especificou anteriormente que devem ser implantados somente os arquivos necessários para executar o aplicativo, não *. CS* arquivos foram copiados para o destino ambiente. O *obj* pasta e o *ContosoUniversity.csproj* e *. csproj.user* arquivos são omitidos pela mesma razão.

    Clique em **publicar** para implantar o ambiente de produção.
2. Teste em produção, siga o mesmo procedimento usado para a preparação.

    Tudo o que é idêntico à preparação, exceto a URL e a ausência de *robots* arquivo.

## <a name="summary"></a>Resumo

Você tem agora implantados e testados com êxito seu aplicativo web e está disponível publicamente na Internet.

![Página inicial de produção](deploying-to-production/_static/image15.png)

O seguinte tutorial, você atualizar o código do aplicativo e implante a alteração para os ambientes de teste, preparação e produção.

> [!NOTE]
> Enquanto seu aplicativo estiver em uso no ambiente de produção deve implementar um plano de recuperação. Ou seja, você deve ser periodicamente backup de seus bancos de dados do aplicativo de produção para um local de armazenamento seguro, e você deve manter várias gerações de backups desse tipo. Quando você atualizar o banco de dados, você deve fazer uma cópia de backup de imediatamente antes da alteração. Em seguida, se você comete um erro e não Descubra até depois de implantá-lo em produção, você ainda poderá recuperar o banco de dados para o estado em que estava antes que ele se tornou corrompido. Para obter mais informações, consulte [Backup de banco de dados do SQL Azure e restauração](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> Neste tutorial, o SQL Server edition que você está implantando é banco de dados do SQL Azure. Enquanto o processo de implantação é semelhante a outras edições do SQL Server, um aplicativo de produção real pode exigir código especial para o banco de dados do SQL Azure em alguns cenários. Para obter mais informações, consulte [trabalhando com o banco de dados do Azure SQL](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) e [escolhendo entre o SQL Server e banco de dados do SQL Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Anterior](setting-folder-permissions.md)
> [Próximo](deploying-a-code-update.md)
