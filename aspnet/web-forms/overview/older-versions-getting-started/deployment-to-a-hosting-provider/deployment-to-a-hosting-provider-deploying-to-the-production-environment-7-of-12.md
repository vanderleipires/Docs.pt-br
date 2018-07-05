---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando no ambiente de produção – 7 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383630"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando no ambiente de produção – 7 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Neste tutorial, você pode configurar uma conta com um provedor de hospedagem e implantar seu ASP.NET recurso de publicação do aplicativo web para o ambiente de produção usando o Visual Studio em um único clique.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Selecionando um provedor de hospedagem

Para o aplicativo Contoso University e esta série de tutoriais, você precisa de um provedor que dá suporte à implantação da Web e ASP.NET 4. Uma empresa de hospedagem específica foi escolhida para que os tutoriais poderiam ilustram a experiência completa de implantação em um site ao vivo. Cada empresa de hospedagem fornece recursos diferentes, e a experiência de implantação para seus servidores varia um pouco. No entanto, o processo descrito neste tutorial é típico para o processo geral. O provedor de hospedagem usado para este tutorial, Cytanium.com, é um dos muitos que estão disponíveis e seu uso neste tutorial não constitui um endosso ou recomendação.

Quando você estiver pronto para selecionar seu próprio provedor de hospedagem, você pode comparar os recursos e preços na [Galeria de provedores de](https://www.microsoft.com/web/hosting) no site Microsoft.com/web.

## <a name="creating-an-account"></a>Criar uma conta

Crie uma conta no seu provedor selecionado. Se o suporte para um banco de dados completo do SQL Server é uma adição extra, você precisa selecioná-lo para este tutorial, mas você precisará dela para o [migrando para o SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) tutorial posteriormente nessa série.

Para esses tutoriais, você não precisa registrar um novo nome de domínio. Você pode testar para verificar a implantação bem-sucedida, usando a URL temporária atribuída ao site pelo provedor.

Depois que a conta foi criada, você normalmente recebe um email de boas-vindo que contém todas as informações que você precisa para implantar e gerenciar seu site. As informações que seu provedor de hospedagem envia será semelhantes ao que é mostrado aqui. O email de boas-vindo Cytanium que é enviado para proprietários de conta nova inclui as seguintes informações:

- A URL para o site de painel de controle do provedor, onde você pode gerenciar as configurações para seu site. A ID e a senha que você especificou estão incluídos nesta parte do email de boas vindas para facilitar a referência. (Ambos foram alteradas para um valor de demonstração para esta ilustração.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- A versão padrão do .NET Framework e informações sobre como alterá-la. Muitos hospedagem sites padrão para 2.0, que funciona com aplicativos ASP.NET que direcionam o .NET Framework 2.0, 3.0 ou 3.5. No entanto Contoso University é um aplicativo do .NET Framework 4, portanto, você precisa alterar essa configuração. (Para um aplicativo ASP.NET 4.5 você usaria a configuração do .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- A URL temporária que você pode usar para acessar seu site da web. Quando essa conta foi criada, "contosouniversity.com" foi inserido como nome de domínio existente. Portanto a URL temporária é `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informações sobre como configurar os bancos de dados e as cadeias de caracteres de conexão que você precisa para acessá-los:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informações sobre ferramentas e configurações para implantar seu site. (O email da Cytanium também menciona o WebMatrix, que é omitido aqui).

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Definindo a versão do .NET Framework

O email de boas-vindo Cytanium inclui um link para obter instruções sobre como alterar a versão do .NET Framework. Estas instruções explicam o que isso pode ser feito por meio do painel de controle Cytanium. Outros provedores de tem sites de painel de controle uma aparência diferentes, ou eles podem instruí-lo a fazer isso de maneira diferente.

Vá para a URL do painel de controle. Após fazer logon com seu nome de usuário e senha, você verá o painel de controle.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

No **hospedagem espaços** , mantenha o ponteiro sobre o ícone de Web e selecione **Sites da Web** no menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

No **Sites da Web** , clique em **contosouniversity.com** (o nome do site que você usou quando criou a conta).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

No **propriedades do Site** caixa, selecione a **extensões** guia.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Alterar do ASP.NET **2.0 Pipeline integrado** à **4.0 (Pipeline integrado)** e, em seguida, clique em **atualização**.

## <a name="publishing-to-the-hosting-provider"></a>Publicação para o provedor de hospedagem

O email de boas-vindo do provedor de hospedagem inclui todas as configurações que você precisa para publicar o projeto e insira essas informações manualmente em um perfil de publicação. Mas, você usará uma maneira mais fácil e menos propenso a erro método para configurar a implantação para o provedor: você baixará uma *. publishsettings* de arquivo e importá-lo para um perfil de publicação.

Em seu navegador, vá para painel de controle Cytanium e selecione **Web** e, em seguida, selecione **Sites da Web.**

![Painel de controle, selecionando a Sites da Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Selecione o **contosouniversity.com** site da web.

![Painel de controle selecionando contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Selecione o **Web Publishing** guia.

![Guia de publicação de Web do painel de controle](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Crie credenciais a serem usadas para digitando um nome de usuário e senha de publicação na web. Você pode inserir as mesmas credenciais que você usa para fazer logon no painel de controle. Em seguida, clique em **habilitar**.

![Painel de controle criar credenciais de publicação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Clique em **baixar perfil de publicação para este site**.

![Download do painel de controle de perfil de publicação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Quando você for solicitado a abrir ou salvar o arquivo, salve-o.

![Salve o arquivo de perfil de publicação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

Na **Gerenciador de soluções** no Visual Studio, clique com botão direito no projeto ContosoUniversity e selecione **publicar**. O **Publicar Web** caixa de diálogo é aberta na **visualização** guia com o **teste** perfil selecionado porque esse é o último perfil que você usou.

Selecione o **perfil** guia e, em seguida, clique em **importação**.

![Botão de importação do Assistente da Web de publicação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

No **importar configurações de publicação** caixa de diálogo, selecione o *. publishsettings* do arquivo que você baixou e, em seguida, clique em **abrir**. O assistente avança para a guia Conexão com todos os campos preenchidos.

![Guia de Conexão do Assistente da Web de publicação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

O arquivo. publishsettings coloca a URL permanente planejada para o site na caixa URL de destino, mas se você ainda não adquiriu esse domínio ainda, substitua o valor com a URL temporária. Neste exemplo, a URL é  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* É a única finalidade dessa caixa especificar qual URL do navegador abrirá automaticamente após com êxito após a implantação. Se você deixar em branco, a única consequência é que o navegador não será iniciado automaticamente após a implantação.

Clique em **validar Conexão** para verificar se as configurações estão corretas e você pode se conectar ao servidor. Como você viu anteriormente, uma marca de seleção verde verifica se a conexão foi bem-sucedida.

Quando você clicar em validar Conexão, você poderá ver uma **erro de certificado** caixa de diálogo. Se você fizer isso, verifique se que o nome do servidor é o esperado. Se estiver, selecione **salvar este certificado para sessões futuras do Visual Studio** e clique em **Accept**. (Esse erro significa que o provedor de hospedagem tem escolhidos para evitar a despesa de comprar um certificado SSL para a URL que você está implantando. Se você preferir estabelecer uma conexão segura usando um certificado válido, entre em contato com seu provedor de hospedagem.)

![Erro de certificado](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Clique em **Avançar**.

No **bancos de dados** seção o **configurações** , insira o mesmo perfil de publicação de valores que você inseriu para o teste. Você encontrará as cadeias de conexão que precisa na lista suspensa.

- Na caixa de cadeia de caracteres de conexão para **SchoolContext,** selecione `Data Source=|DataDirectory|School-Prod.sdf`
- Sob **SchoolContext**, selecione **aplicar migrações do Code First**.
- Na caixa de cadeia de caracteres de conexão para **DefaultConnection**, selecione `Data Source=|DataDirectory|aspnet-Prod.sdf`
- Sob **DefaultConnection**, deixe **Atualizar banco de dados** desmarcada.

![Guia de configurações do Assistente da Web de publicação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Clique em **Avançar**.

No **versão prévia** , clique em **iniciar visualização** para ver uma lista dos arquivos que serão copiados. Você verá a mesma lista que vimos anteriormente quando implantado no IIS no computador local.

Antes de publicar, altere o nome do perfil para que seu arquivo de transformação Web.Production.config será aplicado. Selecione o **perfil** guia e clique em **gerenciar perfis**.

![Gerenciar perfis do Assistente da Web de publicação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

No **editar perfis de publicação na Web** diálogo caixa, selecione o perfil de produção, clique em **Renomear**e altere o nome do perfil para produção. Em seguida, clique em **fechar**.

![Editar caixa de diálogo Web publicar perfis](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Clique em **Publicar**.

O aplicativo é publicado para o provedor de hospedagem. O resultado mostra na **saída** janela.

![Janela de saída após a implantação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

O navegador é aberto automaticamente para a URL que você inseriu na **URL de destino** caixa sobre o **Conexão** guia da **publicar na Web** assistente. Consulte a home page mesma como quando você executa o site no Visual Studio, exceto que agora não há nenhum "(teste)" ou "(desenvolvimento)" indicador de ambiente na barra de título. Isso indica que o indicador de ambiente *Web. config* transformação funcionou corretamente.

> [!NOTE]
> Se você vir "(teste)" no título, exclua o *obj* pasta do projeto ContosoUniversity e reimplantação. Nas versões de pré-lançamento do software, o arquivo de transformação aplicadas anteriormente (Web.Test.config) pode ser aplicado novamente Embora você esteja usando o perfil de produção.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Antes de executar uma página que faz com que o acesso de banco de dados, certifique-se de que o Elmah poderão registrar erros que ocorrem.

## <a name="setting-folder-permissions-for-elmah"></a>Definindo permissões de pasta para Elmah

Conforme você se lembra do tutorial anterior desta série, certifique-se de que o aplicativo tenha permissões de gravação para a pasta em seu aplicativo onde o Elmah armazena arquivos de log de erros. Quando você implantou no IIS localmente no seu computador, defina manualmente essas permissões. Nesta seção, você verá como definir permissões em Cytanium. (Alguns provedores de hospedagem podem não permitir a fazer isso, eles podem oferecer uma ou mais pastas predefinidas com permissões de gravação. Nesse caso, você precisaria modificar seu aplicativo para usar as pastas especificadas.)

Você pode definir permissões de pasta no painel de controle Cytanium. Vá para o controle de URL do painel e selecione **Gerenciador de arquivos**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

No **Gerenciador de arquivos** caixa, selecione **contosouniversity.com** e, em seguida, **wwwrooot** para ver a pasta raiz do aplicativo. Clique no ícone de cadeado lado **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

No **arquivo**/**permissões de pasta** janela, selecione o **leitura** e **gravar** caixas de seleção  **contosouniversity.com** e clique em **definir permissões**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Certifique-se de que o Elmah possui acesso de gravação para o *Elmah* pasta, causando um erro e, em seguida, exibindo o relatório de erros do Elmah. Solicitar uma URL inválida, como *Studentsxxx.aspx*. Como antes, você pode ver os *GenericErrorPage* página. Clique o **fazer logoff** vincular e, em seguida, execute *Elmah.axd*. Você obtém a **fazer logon** página pela primeira vez, que valida que o *Web. config* transformação adicionado com êxito o Elmah autorização. Depois de fazer logon, você verá o relatório que mostra o erro que você acabou de causou.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Teste no ambiente de produção

Execute o **alunos** página. O aplicativo tenta acessar o banco de dados School pela primeira vez, o que dispara a migrações do Code First para criar o banco de dados. Quando a página é exibida após o atraso de qualquer momento, ele mostra que não há nenhum aluno.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Execute o **instrutores** página para verificar se os dados de semente inseridos com êxito os dados de instrutor no banco de dados.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Como você fez no ambiente de teste, você deseja verificar se as atualizações de banco de dados funcionam no ambiente de produção, mas você normalmente não se deseja inserir dados de teste no seu banco de dados de produção. Para este tutorial, você usará o mesmo método que você fez no teste. Mas em um aplicativo real, que você talvez queira localizar um método que valida esse banco de dados as atualizações são bem-sucedidas sem introduzir dados de teste para o banco de dados de produção. Em alguns aplicativos, pode ser prático adicionar algo e, em seguida, excluí-lo.

Adicionar um aluno e, em seguida, exibir os dados inseridos na **alunos** página para verificar se que você pode atualizar dados no banco de dados.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Validar que as regras de autorização estão funcionando corretamente, selecionando **atualização créditos** da **cursos** menu. O **fazer logon** página é exibida. Insira suas credenciais de conta de administrador, clique em **fazer logon**e o **atualização créditos** página é exibida.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Se o logon for bem-sucedido, o **atualização créditos** página é exibida. Isso indica que o banco de dados de associação do ASP.NET (com a conta de administrador único) foi implantado com êxito.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Você agora com êxito implantado e testado seu site e ele está disponível publicamente na Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Criando um ambiente de teste mais confiável

Conforme explicado a [implantando no ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial, o ambiente de teste mais confiável seria uma segunda conta no provedor de hospedagem que é exatamente como a conta de produção. Isso seria mais caro do que usar o IIS local como ambiente de teste, como teria se inscrever para uma segunda conta de hospedagem. Mas se ela impedir erros de site de produção ou interrupções, você pode decidir que vale a pena o custo.

A maioria do processo de criação e implantação para uma conta de teste é semelhante ao que você já fez para implantar em produção:

- Criar uma *Web. config* arquivo de transformação.
- Crie uma conta no provedor de hospedagem.
- Criar um novo perfil de publicação e implantar para a conta de teste.

### <a name="preventing-public-access-to-the-test-site"></a>Impedindo o acesso público para o Site de teste

Uma consideração importante para a conta de teste é que ele estará em tempo real na Internet, mas você não deseja usá-lo público. Para manter o site em particular, você pode usar um ou mais dos seguintes métodos:

- Entre em contato com o provedor de hospedagem para definir regras de firewall que permitem o acesso para o site de teste apenas de endereços IP que você pode usar para teste.
- Disfarçar a URL para que não seja semelhante à URL do site público.
- Use uma *robots* arquivo para garantir que os mecanismos de pesquisa serão não rastrear os links de site e o relatório de teste a ele nos resultados da pesquisa.

O primeiro desses métodos é obviamente o mais seguro, mas o procedimento para fazer isso é específico para cada provedor de hospedagem e não será abordado neste tutorial. Se você organizar com seu provedor de hospedagem para permitir que somente o endereço IP navegar até a URL da conta de teste, teoricamente não precisa se preocupar sobre rastreamento ele os mecanismos de pesquisa. Mas mesmo nesse caso, implantando uma *robots* arquivo é uma boa ideia como um backup, caso essa regra de firewall está desativada acidente.

O *robots* arquivo vai para a pasta do projeto e deve ter o seguinte texto nele:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

O `User-agent` linha informa os mecanismos de pesquisa que se aplicam as regras no arquivo para rastreadores de web de mecanismo de pesquisa todos os (robôs), e o `Disallow` linha especifica que nenhuma página no site deve ser rastreada.

Você provavelmente desejará mecanismos de pesquisa do catálogo seu site de produção, portanto, você precisará excluir este arquivo de implantação de produção. Para fazer isso, consulte **pode posso excluir arquivos ou pastas específicas da implantação?** na [perguntas frequentes sobre o ASP.NET Web Application Project implantação](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Certifique-se de que você especificar a exclusão somente para a produção de perfil de publicação.

Criar uma segunda conta de hospedagem é uma abordagem para trabalhar com um ambiente de teste que não é necessária, mas talvez valha a pena a despesa adicionada. Nos tutoriais a seguir, você continuará a usar o IIS como seu ambiente de teste.

No próximo tutorial, você atualizar o código do aplicativo e implantar sua alteração em ambientes de teste e produção.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
