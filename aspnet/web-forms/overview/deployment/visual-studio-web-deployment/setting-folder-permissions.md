---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Implantação de Web do ASP.NET usando o Visual Studio: definindo permissões de pasta | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 930f46c0ddb0b77525098291393e526107a542d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824775"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Implantação de Web do ASP.NET usando o Visual Studio: definindo permissões de pasta
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Neste tutorial, você define permissões de pasta para o *Elmah* pasta na web implantada do site para que o aplicativo pode criar arquivos de log nessa pasta.

Quando você testa um aplicativo web no Visual Studio usando o Visual Studio Development Server (Cassini) ou IIS Express, o aplicativo é executado sob sua identidade. Você provavelmente é um administrador no computador de desenvolvimento e tem total autoridade para fazer qualquer coisa em qualquer arquivo em qualquer pasta. Mas, quando um aplicativo é executado no IIS, ele é executado sob a identidade definida para o pool de aplicativos que o site é atribuído a. Isso costuma ser uma conta definida pelo sistema que tem permissões limitadas. Por padrão ele tem permissões read e execute os arquivos e pastas do seu aplicativo web, mas ele não tem acesso de gravação.

Isso se torna um problema se seu aplicativo cria ou precisam de arquivos de atualizações, que é um comum em aplicativos da web. No aplicativo Contoso University, o Elmah cria arquivos XML na *Elmah* pasta para salvar os detalhes sobre os erros. Mesmo se você não usar algo parecido com o Elmah, seu site pode permitir que os usuários carregar arquivos ou executar outras tarefas que gravam dados em uma pasta no seu site.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Log de erros de teste e a emissão de relatórios

Para ver como o aplicativo não funciona corretamente no IIS (embora ele fez quando você o testou no Visual Studio), você pode fazer com que um erro que normalmente é registrado pelo Elmah e, em seguida, abra o log de erros do Elmah para ver os detalhes. Se o Elmah não pôde criar um arquivo XML e armazenar os detalhes do erro, você verá um relatório de erros vazio.

Abra um navegador e vá para `http://localhost/ContosoUniversity`, e, em seguida, solicite uma URL inválida, como *Studentsxxx.aspx*. Você verá uma página de erro gerado pelo sistema em vez do *GenericErrorPage* página porque o `customErrors` configuração no arquivo Web. config é "RemoteOnly" e você estiver executando o IIS localmente:

![Página de erro 404 de HTTP](setting-folder-permissions/_static/image1.png)

Agora, execute *Elmah.axd* para ver o relatório de erro. Depois de fazer logon com as credenciais de conta de administrador (&quot;admin&quot; e &quot;devpwd&quot;), você verá uma página de log de erros vazio porque o Elmah não pôde criar um arquivo XML no *Elmah*pasta:

![Erro log vazio](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Definir permissão de gravação na pasta Elmah

Você pode definir permissões de pasta manualmente, ou você pode torná-lo em uma parte automático do processo de implantação. Tornando automático requer um código complexo MSBuild e como você só precisa fazer isso na primeira vez que você implanta, as etapas a seguir mostram como fazer isso manualmente. (Para obter informações sobre como tornar essa parte do processo de implantação, consulte [definição de permissões de pasta na Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) no blog do Sayed Hashimi.)

1. Na **Explorador de arquivos**, navegue até *C:\inetpub\wwwroot\ContosoUniversity*. Clique com botão direito do *Elmah* pasta, selecione **propriedades**e, em seguida, selecione o **segurança** guia.
2. Clique em **Editar**.
3. No **permissões para Elmah** caixa de diálogo, selecione **DefaultAppPool**e, em seguida, selecione o **gravar** caixa de seleção o **permitir** coluna.

    ![Permissões para ELMAH pasta](setting-folder-permissions/_static/image3.png)

    (Se você não vir **DefaultAppPool** na **nomes de grupo ou usuário** lista, você provavelmente usou algum outro método àquela especificada neste tutorial para configurar o IIS e ASP.NET 4 em seu computador. Nesse caso, descubra como a identidade é usada pelo pool de aplicativos atribuído ao aplicativo Contoso University e conceder permissão de gravação a essa identidade. Consulte os links sobre identidades do pool de aplicativos no final deste tutorial.) Clique em **Okey** em ambas as caixas de diálogo.

## <a name="retest-error-logging-and-reporting"></a>Testar novamente o log de erros e emissão de relatórios

Teste, causando um erro novamente da mesma forma (uma URL incorreta de solicitação) e execute o **Log de erros** página. Desta vez o erro é exibido na página.

![Página de Log de erros do ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Resumo

Agora você concluiu todas as tarefas necessárias para que Contoso University funcionando corretamente no IIS no computador local. No próximo tutorial, você fará o site disponível publicamente, implantando-o para o Azure.

## <a name="more-information"></a>Mais informações

Neste exemplo, foi bastante óbvio o motivo por que o Elmah não pôde salvar arquivos de log. Você pode usar o rastreamento do IIS em casos em que a causa do problema não é tão óbvia; ver [solução de problemas de solicitações falhas usando rastreamento no IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) no site IIS.net.

Para obter mais informações sobre como conceder permissões para identidades do pool de aplicativos, consulte [identidades do Pool de aplicativos](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [conteúdo seguro no IIS por meio de ACLs de sistema de arquivo](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) no site IIS.net.

> [!div class="step-by-step"]
> [Anterior](deploying-to-iis.md)
> [Próximo](deploying-to-production.md)
