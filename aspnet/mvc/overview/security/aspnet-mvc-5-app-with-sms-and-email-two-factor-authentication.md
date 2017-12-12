---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: "Aplicativo ASP.NET MVC 5 com SMS e o email de autenticação de dois fatores | Microsoft Docs"
author: Rick-Anderson
description: "Este tutorial mostra como criar um aplicativo da web ASP.NET MVC 5 com a autenticação de dois fatores. Você deve concluir o aplicativo web de criar um seguro ASP.NET MVC 5 com..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: db57b8fe44f41d65d27964f45e0884138629f92b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplicativo ASP.NET MVC 5 com SMS e o email de autenticação de dois fatores
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial mostra como criar um aplicativo da web ASP.NET MVC 5 com a autenticação de dois fatores. Você deve concluir [criar um aplicativo web seguro do ASP.NET MVC 5 com logon, redefinição de senha e de confirmação de email](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar. Você pode baixar o aplicativo concluído [aqui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). O download contém os auxiliares de depuração que permitem que você teste confirmação por email e SMS sem configurar um email ou o provedor de SMS.
> 
> Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Criar um aplicativo ASP.NET MVC](#createMvc)
- [Configurar o SMS para autenticação de dois fatores](#SMS)
- [Habilitar a autenticação de dois fatores](#enable2)
- [Recursos adicionais](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Criar um aplicativo ASP.NET MVC

Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.

> [!NOTE]
> Aviso: Você deve concluir [criar um aplicativo web seguro do ASP.NET MVC 5 com logon, redefinição de senha e de confirmação de email](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar. Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.


1. Criar um novo projeto da Web do ASP.NET e selecione o modelo MVC. Formulários da Web também oferece suporte a identidade do ASP.NET, você pode seguir etapas semelhantes em um aplicativo de formulários da web.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Deixe a autenticação padrão como **contas de usuário individuais**. Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada. No tutorial posteriormente, implantará no Azure. Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).
3. Definir o [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Configurar o SMS para autenticação de dois fatores

Este tutorial fornece instruções sobre como usar o Twilio ou ASPSMS, mas você pode usar qualquer outro provedor SMS.

1. **Criando uma conta de usuário com um provedor de SMS**  
  
 Criar um [Twilio](https://www.twilio.com/try-twilio) ou um [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) conta.
2. **Instalando pacotes adicionais ou adicionando referências de serviço**  
  
 Twilio:  
 No Console do Gerenciador de pacotes, digite o seguinte comando:  
    `Install-Package Twilio`  
  
 ASPSMS:  
 A referência de serviço a seguir precisa ser adicionado:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 endereço:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 Namespace:  
    `ASPSMSX2`
3. **Descobrir as credenciais do usuário do provedor de SMS**  
  
 Twilio:  
 Do **painel** guia da sua conta do Twilio, copie o **SID de conta** e **token de autenticação**.  
  
 ASPSMS:  
 De suas configurações de conta, navegue até **chave de usuário confidenciais** e copiá-lo junto com seus Self definido **senha**.  
  
 Mais tarde armazenaremos esses valores no *Web. config* arquivo nas chaves `"SMSAccountIdentification"` e `"SMSAccountPassword"` .
4. **Especificando SenderID / originador**  
  
 Twilio:  
 Do **números** guia, copie o seu número de telefone do Twilio.  
  
 ASPSMS:  
 Dentro de **desbloquear originadores** Menu desbloquear originadores de uma ou mais ou escolha um originador alfanumérico (não é suportado por todas as redes).  
  
 Mais tarde armazenaremos esse valor no *Web. config* arquivo dentro da chave `"SMSAccountFrom"` .
5. **Transferindo as credenciais do provedor SMS em aplicativo**  
  
 Verifique as credenciais e o número de telefone do remetente disponíveis para o aplicativo. Para manter as coisas simples armazenaremos esses valores no *Web. config* arquivo. Quando for implantada no Azure, podemos armazenar os valores de segurança no **configurações do aplicativo** guia Configurar de seção no site da web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Segurança - nunca armazenar os dados confidenciais em seu código-fonte. A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples. Consulte [práticas recomendadas para a implantação de senhas e outros dados confidenciais em ASP.NET e o Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementação de transferência de dados para o provedor de SMS**  
  
 Configurar o `SmsService` classe no *aplicativo\_Start\IdentityConfig.cs* arquivo.  
  
 Dependendo do provedor SMS usado ativar o **Twilio** ou **ASPSMS** seção: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Atualização de *Views\Manage\Index.cshtml* exibição Razor: (Observação: apenas não remova os comentários no código existente, use o código abaixo.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Verifique se o `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` métodos de ação no `ManageController` tem o[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atributo:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Executar o aplicativo e faça logon usando a conta que você registrou anteriormente.
10. Clique em sua ID de usuário, que ativa o `Index` método de ação `Manage` controlador.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Clique em Adicionar.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. O `AddPhoneNumber` método de ação exibe uma caixa de diálogo para inserir um número de telefone que pode receber mensagens SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Em alguns segundos, você receberá uma mensagem de texto com o código de verificação. Insira-o e pressione **enviar**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. O modo de gerenciar mostra que o número de telefone foi adicionado.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Habilitar a autenticação de dois fatores

No aplicativo do modelo gerado, você precisa usar a interface do usuário para habilitar a autenticação de dois fatores (2FA). Para habilitar a 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Clique em Habilitar 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Logoff, em seguida, log novamente. Se você tiver habilitado o email (consulte meu [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), você pode selecionar o email para 2FA ou SMS.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

A página de código verificar é exibida, onde você pode inserir o código (de SMS ou email).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Clicar no **lembrar este navegador** caixa de seleção isentará a necessidade de usar 2FA para fazer logon ao usar o navegador e o dispositivo em que você tiver marcado a caixa. Desde que usuários mal-intencionados não podem acessar seu dispositivo, permitindo que 2FA e clicando no **lembrar este navegador** fornece acesso conveniente de senha um etapa, enquanto ainda retém a proteção forte 2FA para todo o acesso dispositivos não confiáveis. Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.

Este tutorial fornece uma introdução rápida ao habilitar 2FA em um novo aplicativo ASP.NET MVC. O tutorial [autenticação de dois fatores usando SMS e email com o ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) apresenta detalhes sobre o código de exemplo.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Autenticação de dois fatores usando SMS e email com o ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) apresenta detalhes sobre a autenticação de dois fatores
- [Links para a identidade do ASP.NET recomendado recursos](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Conta de confirmação e de recuperação de senha com a identidade do ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) apresenta mais detalhes na confirmação da senha da conta e a recuperação.
- [Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com autorização Facebook e Google OAuth 2. Ele também mostra como adicionar dados adicionais para o banco de dados de identidade.
- [Implantar um aplicativo ASP.NET MVC seguro com associação, OAuth e o banco de dados SQL Web do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com as funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.
- [Criar um aplicativo do Google OAuth 2 e conectar o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Criando o aplicativo no Facebook e conectar o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurar o SSL no projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
