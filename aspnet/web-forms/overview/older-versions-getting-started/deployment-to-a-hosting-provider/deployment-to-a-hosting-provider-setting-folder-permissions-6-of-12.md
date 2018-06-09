---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: definindo permissões de pasta - 6 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 573e75221a1c0018bded7544e584b0c75f47d607
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30887259"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: definindo permissões de pasta - 6 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Neste tutorial, defina as permissões de pasta para o *Elmah* pasta na web implantada para que o aplicativo pode criar arquivos de log na pasta do site.

Quando você testar um aplicativo web no Visual Studio usando o Visual Studio Development Server (Cassini), o aplicativo é executado sob sua identidade. Você provavelmente é um administrador no computador de desenvolvimento e tem total autoridade para fazer nada para qualquer arquivo em qualquer pasta. Mas, quando um aplicativo é executado no IIS, ele é executado sob a identidade definida para o pool de aplicativos que o site é atribuído à. Isso normalmente é uma conta definida pelo sistema que tem permissões limitadas. Por padrão ele tem permissões read e execute os arquivos e pastas do seu aplicativo web, mas ele não tem acesso de gravação.

Isso se torna um problema se seu aplicativo cria ou precisam de arquivos de atualizações, que é um comum em aplicativos da web. No aplicativo da Contoso University, Elmah cria arquivos XML no *Elmah* pasta para salvar os detalhes sobre erros. Mesmo se você não usar algo como Elmah, seu site pode permitir que os usuários carreguem arquivos ou executar outras tarefas que gravam dados em uma pasta no seu site.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Teste o log de erros e emissão de relatórios

Para ver como o aplicativo não funciona corretamente no IIS (embora, como fez quando você o testado no Visual Studio), você poderá causar um erro que normalmente é registrado pelo Elmah e, em seguida, abra o log de erros do Elmah para ver os detalhes. Se Elmah não pôde criar um arquivo XML e armazenar os detalhes do erro, você verá um relatório de erros vazio.

Abra um navegador e vá para `http://localhost/ContosoUniversity`, e, em seguida, solicitar uma URL inválida como *Studentsxxx.aspx*. Você verá uma página de erro gerado pelo sistema, em vez do *GenericErrorPage* página porque a `customErrors` configuração no arquivo Web. config é "RemoteOnly" e você estiver executando o IIS localmente:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Agora execute *Elmah.axd* para ver o relatório de erros. Você verá uma página de log de erros vazio porque Elmah não pôde criar um arquivo XML no *Elmah* pasta:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Configuração de permissão de gravação na pasta Elmah

Você pode definir permissões de pasta manualmente, ou você pode fazer parte automática do processo de implantação. Tornando automático requer códigos complexos do MSBuild e desde que você só precisa fazer isso na primeira vez que você implantar, este tutorial mostra apenas como fazê-lo manualmente. (Para obter informações sobre como fazer esta parte do processo de implantação, consulte [definindo permissões de pasta na Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) no blog do Hashimi Sayed.)

Em **Windows Explorer**, navegue até *C:\inetpub\wwwroot\ContosoUniversity*. Clique com botão direito do *Elmah* pasta, selecione **propriedades**e, em seguida, selecione o **segurança** guia.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Se você não vir **DefaultAppPool** no **nomes de grupo ou usuário** lista, você provavelmente usou algum outro método que o especificado neste tutorial para configurar o IIS e ASP.NET 4 no seu computador. Nesse caso, descubra identidade que é usada pelo pool de aplicativos atribuído ao aplicativo Contoso University e conceder permissão de gravação a essa identidade. Consulte os links sobre as identidades do pool de aplicativos no final deste tutorial.)

Clique em **Editar**. No **permissões para Elmah** caixa de diálogo, selecione **DefaultAppPool**e, em seguida, selecione o **gravar** caixa de seleção de **permitir** coluna.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Clique em **Okey** em ambas as caixas de diálogo.

## <a name="retesting-error-logging-and-reporting"></a>Teste o log de erros e emissão de relatórios

Testar, causando um erro novamente da mesma forma (solicitação de uma URL incorreta) e execute o **Log de erros** página. Neste momento o erro é exibido na página.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Você também precisa de permissão de gravação no *aplicativo\_dados* pasta porque você tem arquivos de banco de dados do SQL Server Compact nessa pasta, e você deseja ser capaz de atualizar os dados nesses bancos de dados. Nesse caso, no entanto, você não precisa fazer nada extras porque o processo de implantação define automaticamente a permissão de gravação no *aplicativo\_dados* pasta.

Agora você concluiu todas as tarefas necessárias para que a Contoso University funcionando corretamente no IIS no computador local. No tutorial de Avançar, você fará o site disponível publicamente, implantando-a para um provedor de hospedagem.

## <a name="more-information"></a>Mais informações

Neste exemplo, o motivo por que o Elmah não pôde salvar arquivos de log foi bastante óbvio. Você pode usar o rastreamento do IIS nos casos em que a causa do problema não é tão óbvia; consulte [Solucionando problemas de solicitações falhas usando rastreamento no IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) no site IIS.net.

Para obter mais informações sobre como conceder permissões para identidades do pool de aplicativos, consulte [identidades do Pool de aplicativos](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [conteúdo seguro no IIS através de ACLs de sistema de arquivo](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) no site IIS.net.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
