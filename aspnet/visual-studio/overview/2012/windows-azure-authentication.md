---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows autenticação do Azure | Microsoft Docs
author: Rick-Anderson
description: Ferramentas do Microsoft ASP.NET para o Windows Azure Active Directory torna simples para habilitar a autenticação para aplicativos web hospedados no Windows Azure Web Sites...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: d46a673b75eba0e058c7e20b12f44caf4e2f4f50
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379787"
---
<a name="windows-azure-authentication"></a>Windows autenticação do Azure
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Ferramentas do Microsoft ASP.NET para Windows Azure Active Directory torna simples para habilitar a autenticação para aplicativos web hospedados no [Windows Azure Web Sites](https://www.windowsazure.com/home/features/web-sites/). Você pode usar a autenticação do Windows Azure para autenticar usuários do Office 365 da sua organização, sincronizadas do Active Directory no local de contas corporativas ou os usuários criados no seu próprio domínio personalizado do Windows Azure Active Directory. Habilitar a autenticação do Windows Azure configura seu aplicativo para autenticar usuários usando uma única [Active Directory do Windows Azure](https://docs.microsoft.com/azure/active-directory/) locatário.
> 
> Não há suporte para a ferramenta de autenticação do Windows Azure do ASP.NET para funções web em um serviço de nuvem, mas planejamos fazê-lo em uma versão futura. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) é compatível com funções da web do Windows Azure.
> 
> Para obter detalhes sobre como configurar a sincronização entre o Active Directory local e seu locatário do Active Directory do Windows Azure, consulte [usar o AD FS 2.0 para implementar e gerenciar logon único](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory está disponível como um [livre de serviço de visualização](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Requisitos:

- Visual Studio 2012 ou [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Extensões para Visual Studio 2012 de ferramentas da Web](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) ou [extensões de ferramentas da Web do Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Ferramentas do Microsoft ASP.NET para o Windows Active Directory do Azure – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) ou [ferramentas do Microsoft ASP.NET para o Windows Azure Active Directory – Visual Studio Express 2012 para Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Criar um aplicativo Web ASP.NET com o Visual Studio 2012

Você pode criar qualquer aplicativo da Web com o Visual Studio 2012, este tutorial usa o modelo de intranet do ASP.NET MVC.

1. Crie um novo aplicativo ASP.NET MVC 4 Intranet e aceitar todos os padrões. (Ele deve ser um **tra** net e não no **MOS** projeto net).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Habilitar a autenticação do Windows Azure (quando você for um Administrador Global da filosofia)

Se você não tiver um locatário existente do Windows Azure Active Directory (por exemplo, por meio de uma conta existente do Office 365) você pode criar um novo locatário inscrevendo-se para uma [nova conta do Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. No menu projeto, selecione **habilitar a autenticação do Windows Azure**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Digite o domínio para seu locatário do Active Directory do Windows Azure (por exemplo, contoso.onmicrosoft.com) e clique em **habilitar**:

![](windows-azure-authentication/_static/image3.png)

3. Na autenticação da Web caixa de diálogo de entrada como um administrador para seu locatário do Active Directory do Windows Azure:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Habilitar o Azure de janela por um não-administrador do princípio

Se você não tiver privilégios de Administrador Global para seu locatário do Active Directory do Windows Azure, você pode desmarcar a caixa de seleção para o provisionamento de aplicativo.

![](windows-azure-authentication/_static/image6.png)

A caixa de diálogo exibirá o **domínio**, **Id da entidade de aplicativo** e **URL de resposta** que são necessários para o provisionamento de aplicativo com um Azure Active Directory princípio. Você precisa fornecer essas informações para alguém que tem privilégios suficientes para provisionar o aplicativo. Ver[como implementar o logon único com o Azure Active Directory do Windows - aplicativo ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) para obter detalhes sobre como usar o cmdlet para criar a entidade de serviço manualmente.  
Depois que o aplicativo foi configurado com êxito, você pode clicar em **continuar Atualizar Web. config com as configurações selecionadas**. Se você quiser continuar a desenvolver o aplicativo enquanto aguarda para provisionamento acontecer, você pode clicar em **Fechar para memorizar as configurações no arquivo de projeto**. Na próxima vez que você invocar a habilitar a autenticação do Windows Azure e desmarque a caixa de seleção de provisionamento, você verá as mesmas configurações e você pode clicar em **Continue**, em seguida, clique em **aplicar essas configurações no Web. config**.

1. Aguarde enquanto seu aplicativo é configurado para autenticação do Windows Azure e provisionado com o Windows Azure Active Directory.
2. Depois que a autenticação do Windows Azure tiver sido habilitada para seu aplicativo, clique em **fechar:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Pressione F5 para executar seu aplicativo. Você deve obter redirecionado automaticamente para a página de logon. Usar as credenciais de usuário do princípio de diretório para fazer logon no aplicativo...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Porque seu aplicativo está usando um certificado de teste autoassinado, você receberá um aviso do navegador que o certificado não foi emitido por uma autoridade de certificação confiável.

    Esse aviso pode ser ignorado durante o desenvolvimento local, clicando em **continuar neste site:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Você agora fizeram logon com êxito seu aplicativo usando a autenticação do Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Autenticação de habilitação do Windows Azure faz as seguintes alterações ao seu aplicativo:

- Uma falsificação de solicitação de anti-script entre sites ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) classe ( *App\_Start\AntiXsrfConfig.cs* ) é adicionado ao seu projeto.
- Os pacotes do NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` é adicionado ao seu projeto.
- Configurações do Windows Identity Foundation em seu aplicativo serão configuradas para aceitar tokens de segurança de seu locatário do Active Directory do Windows Azure. Clique na imagem abaixo para ver uma exibição expandida das alterações feitas para o *Web. config* arquivo.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Uma entidade de serviço para seu aplicativo em seu locatário do Active Directory do Windows Azure será provisionado.
- O HTTPS será habilitado.

## <a name="deploy-the-application-to-windows-azure"></a>Implantar o aplicativo no Windows Azure

Para obter instruções completas, consulte [Implantando um aplicativo Web do ASP.NET a um Site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Para publicar um aplicativo usando a autenticação do Windows Azure em um Site do Azure:

1. Clique com o botão direito em seu aplicativo e selecione **publicar:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Na caixa de diálogo Publicar na Web, baixar e importar um perfil de publicação para seu Site da Web do Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. O **Conexão** guia mostra os **URL de destino** (a voltado para a URL pública para o seu aplicativo). Clique em **validar Conexão** para testar sua conexão:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Se você publicou para este Site da Web do Azure anteriormente considere verificar a **remover arquivos adicionais no destino** publica de configuração para garantir que seu aplicativo corretamente. Observe que o **habilitar a autenticação do Windows Azure** caixa de seleção é slected.  

    ![](windows-azure-authentication/_static/image10.png)
5. Opcional: Na **versão prévia** guia, clique em **iniciar visualização** para ver os arquivos sejam implantados.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Clique em **publicar.**

    Você será solicitado a habilitar a autenticação do Windows Azure para o host de destino. Clique em **habilitar** para continuar:

    ![](windows-azure-authentication/_static/image11.png)
7. Insira suas credenciais de administrador para seu locatário do Active Directory do Windows Azure:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Depois que seu aplicativo tiver sido publicado com êxito, um navegador abrirá o site publicado.

    > [!NOTE]
    > Ele pode levar até cinco minutos (normalmente deve menor) para o seu aplicativo seja totalmente provisionada com o Windows Azure Active Directory depois de habilitar a autenticação do Windows Azure para o host de destino. Quando você primeiro executar o aplicativo se você receber o erro ACS50001: terceira parte confiável com o nome '[Território]' não foi encontrada, em seguida, aguarde alguns minutos e tente executar o aplicativo novamente.
9. Quando solicitado, faça logon como um usuário em seu diretório:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Agora fazer logon com êxito no Azure usando a autenticação do Windows Azure do aplicativo hospedado.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Problemas Conhecidos

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Autorização baseada em função falhar ao usar a autenticação do Windows Azure < o:p >< / o:p >

Autenticação do Windows Azure atualmente não fornece a declaração de função necessários para que a autorização baseada em função pode ser executada. A função do usuário autenticado deve ser recuperada do Active Directory do Windows Azure. < o:p >< / o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Navegando para um aplicativo com autenticação do Windows Azure resulta no erro "ACS20016 o domínio do usuário conectado (live.com) não corresponde a nenhum permitidos domínio desse STS" < o:p >< / o:p >

Se você já estiver conectado a uma Account da Microsoft (por exemplo, hotmail.com, live.com, outlook.com) e você tentar acessar um aplicativo que tenha habilitado a autenticação do Windows Azure você pode receber uma resposta de 400 erro porque o domínio da sua Account da Microsoft não é reconhecida pelo Windows Azure Active Directory. Para fazer logon no aplicativo, faça logoff do seu Account da Microsoft pela primeira vez. < o:p >< / o:p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Registro em log em um aplicativo com a autenticação do Windows Azure habilitado e um X509CertificateValidationMode diferente de None resulta em erros de validação de certificado para o certificado accounts.accesscontrol.windows.net < o:p >< / o:p >

Validação de certificado não é necessária e deve ser deixada desabilitada. A impressão digital do certificado do emissor é validada pelo WSFederationAuthenticationModule. < o:p >< / o:p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Ao tentar habilitar a autenticação do Windows Azure a caixa de diálogo de autenticação da Web mostra o erro "ACS20016: O domínio do usuário conectado (contoso.onmicrosoft.com) não coincide com nenhum domínio permitido desse STS." < o:p >< / o:p >

Você poderá ver esse erro quando você anteriormente fizeram logon com êxito usando uma conta diferente do Active Directory do Windows Azure de dentro do mesmo processo do Visual Studio. Faça logoff da conta especificada ou reinicie o Visual Studio. Se anteriormente você conectado e selecionado a opção "Manter-me conectado", talvez você precise limpar seus cookies do navegador. < o:p >< / o:p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: A solicitação não é uma mensagem de protocolo do WS-Federation válida < o:p >< / o:p >

Isso pode acontecer se você já estiver conectado com alguns outros ID da Microsoft para um dos serviços do Azure. Janela do navegador privada de uso como InPrivate no Internet Explorer ou anônimo no Chrome ou limpar todos os cookies. < o:p >< / o:p >

## <a name="additional-resources"></a>Recursos adicionais

- [Ferramentas do Microsoft ASP.NET para o Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows recursos do Azure: identidade](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows do Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: desenvolver aplicativos para sua organização](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: desenvolver aplicativos para várias organizações](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Como implementar o logon único com o Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Logon único com o Windows Azure Active Directory: um mergulho profundo](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Usar o AD FS 2.0 para implementar e gerenciar logon único](https://technet.microsoft.com/library/jj205462.aspx)
