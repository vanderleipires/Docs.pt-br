---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Implantação de Web do ASP.NET usando o Visual Studio: definindo permissões de pasta | Microsoft Docs'
author: tdykstra
description: Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 7efe267975835e889950983126088f1b637c28fb
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890392"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Implantação de Web do ASP.NET usando o Visual Studio: definindo permissões de pasta
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Neste tutorial, defina as permissões de pasta para o *Elmah* pasta na web implantada para que o aplicativo pode criar arquivos de log na pasta do site.

Quando você testar um aplicativo web no Visual Studio usando o servidor de desenvolvimento do Visual Studio (Cassini) ou IIS Express, o aplicativo é executado sob sua identidade. Você provavelmente é um administrador no computador de desenvolvimento e tem total autoridade para fazer nada para qualquer arquivo em qualquer pasta. Mas, quando um aplicativo é executado no IIS, ele é executado sob a identidade definida para o pool de aplicativos que o site é atribuído à. Isso normalmente é uma conta definida pelo sistema que tem permissões limitadas. Por padrão ele tem permissões read e execute os arquivos e pastas do seu aplicativo web, mas ele não tem acesso de gravação.

Isso se torna um problema se seu aplicativo cria ou precisam de arquivos de atualizações, que é um comum em aplicativos da web. No aplicativo da Contoso University, Elmah cria arquivos XML no *Elmah* pasta para salvar os detalhes sobre erros. Mesmo se você não usar algo como Elmah, seu site pode permitir que os usuários carreguem arquivos ou executar outras tarefas que gravam dados em uma pasta no seu site.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Log de erros de teste e emissão de relatórios

Para ver como o aplicativo não funciona corretamente no IIS (embora, como fez quando você o testado no Visual Studio), você poderá causar um erro que normalmente é registrado pelo Elmah e, em seguida, abra o log de erros do Elmah para ver os detalhes. Se Elmah não pôde criar um arquivo XML e armazenar os detalhes do erro, você verá um relatório de erros vazio.

Abra um navegador e vá para `http://localhost/ContosoUniversity`, e, em seguida, solicitar uma URL inválida como *Studentsxxx.aspx*. Você verá uma página de erro gerado pelo sistema, em vez do *GenericErrorPage* página porque a `customErrors` configuração no arquivo Web. config é "RemoteOnly" e você estiver executando o IIS localmente:

![Página de erro 404 de HTTP](setting-folder-permissions/_static/image1.png)

Agora execute *Elmah.axd* para ver o relatório de erros. Depois de você fazer logon com as credenciais da conta de administrador (&quot;admin&quot; e &quot;devpwd&quot;), você verá uma página de log de erros vazio porque Elmah não pôde criar um arquivo XML no *Elmah*pasta:

![Erro log vazio](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Definir permissão de gravação na pasta Elmah

Você pode definir permissões de pasta manualmente, ou você pode fazer parte automática do processo de implantação. Tornando automático requer códigos complexos do MSBuild, e desde que você só precisa fazer isso na primeira vez que você implanta, as seguintes etapas como fazê-lo manualmente. (Para obter informações sobre como fazer esta parte do processo de implantação, consulte [definindo permissões de pasta na Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) no blog do Hashimi Sayed.)

1. Em **Explorador de arquivos**, navegue até *C:\inetpub\wwwroot\ContosoUniversity*. Clique com botão direito do *Elmah* pasta, selecione **propriedades**e, em seguida, selecione o **segurança** guia.
2. Clique em **Editar**.
3. No **permissões para Elmah** caixa de diálogo, selecione **DefaultAppPool**e, em seguida, selecione o **gravar** caixa de seleção de **permitir** coluna.

    ![Permissões para pasta ELMAH](setting-folder-permissions/_static/image3.png)

    (Se você não vir **DefaultAppPool** no **nomes de grupo ou usuário** lista, você provavelmente usou algum outro método que o especificado neste tutorial para configurar o IIS e ASP.NET 4 no seu computador. Nesse caso, descubra identidade que é usada pelo pool de aplicativos atribuído ao aplicativo Contoso University e conceder permissão de gravação a essa identidade. Consulte os links sobre as identidades do pool de aplicativos no final deste tutorial.) Clique em **Okey** em ambas as caixas de diálogo.

## <a name="retest-error-logging-and-reporting"></a>Testar novamente o relatório e log de erros

Testar, causando um erro novamente da mesma forma (solicitação de uma URL incorreta) e execute o **Log de erros** página. Neste momento o erro é exibido na página.

![Página de Log de erros do ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Resumo

Agora você concluiu todas as tarefas necessárias para que a Contoso University funcionando corretamente no IIS no computador local. No tutorial de Avançar, você fará o site disponível publicamente, implantando-a para o Azure.

## <a name="more-information"></a>Mais informações

Neste exemplo, o motivo por que o Elmah não pôde salvar arquivos de log foi bastante óbvio. Você pode usar o rastreamento do IIS nos casos em que a causa do problema não é tão óbvia; consulte [Solucionando problemas de solicitações falhas usando rastreamento no IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) no site IIS.net.

Para obter mais informações sobre como conceder permissões para identidades do pool de aplicativos, consulte [identidades do Pool de aplicativos](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [conteúdo seguro no IIS através de ACLs de sistema de arquivo](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) no site IIS.net.

> [!div class="step-by-step"]
> [Anterior](deploying-to-iis.md)
> [Próximo](deploying-to-production.md)
