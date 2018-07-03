---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Criar o MVC 5 aplicativo com o Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo web ASP.NET MVC 5 que permite que os usuários façam logon usando o OAuth 2.0 com as credenciais de um autenti externo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 8a9528f76b0166175f950543b4b8a7250bdf5100
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388760"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Criar um aplicativo ASP.NET MVC 5 com o Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial mostra como criar um aplicativo web ASP.NET MVC 5 que permite que os usuários façam logon usando [OAuth 2.0](http://oauth.net/2/) com as credenciais de um provedor de autenticação externa, como o Facebook, Twitter, LinkedIn, Microsoft ou Google. Para simplificar, este tutorial concentra-se sobre como trabalhar com as credenciais do Facebook e Google.
> 
> Habilitar essas credenciais em sites da web fornece uma vantagem significativa porque milhões de usuários já têm contas com esses provedores externos. Esses usuários podem ser mais decididos a se inscrever para seu site se eles não precisam criar e lembrar de um novo conjunto de credenciais.
> 
> Consulte também [aplicativo ASP.NET MVC 5 com o SMS e email de autenticação de dois fatores](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> O tutorial também mostra como adicionar dados de perfil do usuário e como usar a API de associação para adicionar funções. Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (siga-me no Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Guia de Introdução

Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar o Visual Studio [2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior. Para obter ajuda com o Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! e muito mais, consulte este [projeto de exemplo](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Você deve instalar o Visual Studio [2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior para usar o Google OAuth 2 e depurar localmente sem avisos de SSL.


Clique em **novo projeto** da **inicie** página, ou você pode usar o menu e selecione **arquivo**e, em seguida, **novo projeto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Clique em **novo projeto**, em seguida, selecione **Visual c#** à esquerda, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.NET**. Nomeie o projeto "MvcAuth" e, em seguida, clique em **Okey**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

No **novo projeto ASP.NET** caixa de diálogo, clique em **MVC**. Se a autenticação não estiver **contas de usuário individuais**, clique no **alterar autenticação** botão e selecione **contas de usuário individuais**. Verificando **hospedar na nuvem**, o aplicativo será muito fácil hospedar no Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Se você selecionou **hospedar na nuvem**, conclua a caixa de diálogo Configurar.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Use o NuGet para atualizar para o middleware OWIN mais recente

Use o Gerenciador de pacotes do NuGet para atualizar o [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Selecione **atualizações** no menu à esquerda. Você pode clicar na **Atualizar tudo** botão ou você pode procurar apenas os pacotes OWIN (mostrados na próxima imagem):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

A imagem abaixo, são mostrados apenas os pacotes OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Do pacote Manager Console (PMC), você pode inserir o `Update-Package` comando, que atualizará todos os pacotes.

Pressione **F5** ou **Ctrl + F5** para executar o aplicativo. Na imagem abaixo, o número da porta for 1234. Quando você executa o aplicativo, você verá um número de porta diferente.

Dependendo do tamanho da janela do navegador, talvez seja necessário clicar no ícone de navegação para ver os **página inicial**, **sobre**, **contato**, **registrar**e **efetuar login** links.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configurar o SSL no projeto

Para se conectar a provedores de autenticação, como Google e Facebook, você precisará configurar o IIS Express para usar SSL. É importante manter usando SSL, após o logon e não descarte volta HTTP, o cookie de logon é apenas como segredo como seu nome de usuário e senha e sem o uso de SSL que você está enviando em texto não criptografado na transmissão. Além disso, você já passou um tempo para executar o handshake e proteja o canal (que é a maior parte do que o torna mais lento do que o HTTP do HTTPS) antes que o pipeline MVC é executado, então redirecionar de volta para o HTTP depois que você está conectado não fará a solicitação atual ou futuro solicitações muito mais rápidas.

1. Na **Gerenciador de soluções**, clique no **MvcAuth** projeto.
2. Pressione a tecla F4 para mostrar as propriedades do projeto. Como alternativa, a partir de **exibição** menu você pode selecionar **janela propriedades**.
3. Alteração **SSL habilitado** como True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copie a URL do SSL (que será `https://localhost:44300/` , a menos que você criou outros projetos SSL).
5. Na **Gerenciador de soluções**, clique com botão direito do **MvcAuth** do projeto e selecione **propriedades**.
6. Selecione o **Web** guia e, em seguida, cole a URL do SSL para o **Url do projeto** caixa. Salve o arquivo (Ctl + S). Você precisará dessa URL para configurar os aplicativos de autenticação do Facebook e Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Adicione a [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atributo para o `Home` controlador para exigir que todas as solicitações deve usar HTTPS. Uma abordagem mais segura é adicionar o [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtro para o aplicativo. Consulte a seção &quot;proteger o aplicativo com SSL e o atributo Authorize&quot; no meu tutoral [criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no serviço de aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Abaixo está uma parte do controlador Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Pressione CTRL+F5 para executar o aplicativo. Se você tiver instalado o certificado no passado, você pode ignorar o restante desta seção e vá para [criando um aplicativo do Google para o OAuth 2 e conectar o aplicativo ao projeto](#goog), caso contrário, siga as instruções para confiar a autoassinado certificado que o IIS Express gerou.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Leia as **aviso de segurança** caixa de diálogo e clique **Sim** se você quiser instalar o certificado que representa o localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE mostra a *Home* página e não há nenhum aviso de SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome também aceita o certificado e mostrará o conteúdo HTTPS sem aviso. O Firefox usa seu próprio repositório de certificados, portanto, ele exibirá um aviso. Para nosso aplicativo você pode clicar com segurança **compreende os riscos**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Criando um aplicativo do Google para o OAuth 2 e conectar o aplicativo ao projeto

> [!WARNING]
> Para instruções de OAuth do Google atuais, consulte [Google Configurando a autenticação no ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Navegue até a [Console de desenvolvedores do Google](https://console.developers.google.com/).
2. Se você não tiver criado um projeto antes, selecione **credenciais** na guia à esquerda e em seguida, selecione **criar**.
3. Na guia à esquerda, clique em **credenciais**.
4. Clique em **criar credenciais** , em seguida, **ID do cliente OAuth**. 

    1. No **criar ID do cliente** caixa de diálogo, mantenha o padrão **aplicativo Web** para o tipo de aplicativo.
    2. Defina as **JavaScript autorizadas** origens para a URL do SSL usado acima (`https://localhost:44300/` , a menos que você criou outros projetos SSL)
    3. Defina as **URI de redirecionamento autorizado** para:  
         `https://localhost:44300/signin-google`
5. Clique no item de menu de tela de consentimento de OAuth e, em seguida, definir seu nome de produto e o endereço de email. Quando você tiver concluído o formulário, clique **salvar**.
6. Clique no item de menu de biblioteca, pesquise **API do Google +**, clique nele e em seguida, pressione Enable.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   A imagem abaixo mostra as APIs habilitadas.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. No Gerenciador de API de APIs do Google, visite o **credenciais** tab para obter o **ID do cliente**. Download para salvar um arquivo JSON com segredos do aplicativo. Copie e cole a **ClientId** e **ClientSecret** no `UseGoogleAuthentication` método encontrado no *Startup.Auth.cs* arquivo no *App_Start* pasta. O **ClientId** e **ClientSecret** valores mostrados abaixo são exemplos e não funcionam.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Security - nunca armazenar os dados confidenciais em seu código-fonte. A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples. Ver [práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.NET e o serviço de aplicativo do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Pressione **CTRL + F5** para compilar e executar o aplicativo. Clique o **faça logon no** link.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Sob **usar outro serviço para fazer logon**, clique em **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Se você perder qualquer uma das etapas acima, você receberá um erro HTTP 401. Verifique novamente as etapas acima. Se você perder uma configuração necessária (por exemplo **nome do produto**), adicione o item ausente e salve; pode levar alguns minutos para que a autenticação funcione.
10. Você será redirecionado ao site do Google, onde você vai inserir suas credenciais.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Depois de inserir suas credenciais, você será solicitado a conceder permissões para o aplicativo web que você acabou de criar:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Clique em **aceitar**. Você será redirecionado para o **registrar** página do aplicativo MvcAuth onde você pode registrar sua conta do Google. Você tem a opção de alterar o nome de registro de email local utilizado para sua conta do Gmail, mas você geralmente deseja manter o alias de email padrão (ou seja, aquele usado para autenticação). Clique em **Registrar**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Criando o aplicativo no Facebook e conectar o aplicativo ao projeto

> [!WARNING]
> Para instruções de autenticação do Facebook OAuth2 atuais, consulte [autenticação do Facebook Configurando](/aspnet/core/security/authentication/social/facebook-logins)

Para a autenticação de OAuth2 do Facebook, você precisará copiar para seu projeto de algumas configurações de um aplicativo que você cria no Facebook.

1. No seu navegador, navegue até [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) e faça logon digitando suas credenciais do Facebook.
2. Se você já não estiver registrado como um desenvolvedor de Facebook, clique em **se registrar como um desenvolvedor** e siga as instruções para registrar.
3. Sobre o **aplicativos** , clique em **criar novo aplicativo**.

    ![Criar novo aplicativo](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Insira um **nome do aplicativo** e **categoria**, em seguida, clique em **criar aplicativo**.

    O <strong>Namespace do aplicativo</strong> é a parte da URL que seu aplicativo usará para acessar o aplicativo do Facebook para autenticação (por exemplo, https\://apps.facebook.com/{App Namespace}). Se você não especificar um <strong>Namespace do aplicativo</strong>, o <strong>ID do aplicativo</strong> será usado para a URL. O <strong>ID do aplicativo</strong> é um número longo gerados pelo sistema que você verá na próxima etapa.

    ![Criar caixa de diálogo Novo aplicativo](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Envie a verificação de segurança padrão.

    ![Verificação de segurança](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Selecione **as configurações** para a barra de menu à esquerda.![ Barra de menus do desenvolvedor do Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. Sobre o **básicas** seção configurações da página selecionar **Adicionar plataforma** para especificar que você está adicionando um aplicativo de site. ![Configurações básicas](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Selecione **site** entre as opções de plataforma.  
  
    ![Opções de plataforma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Anote sua **ID do aplicativo** e seu **segredo do aplicativo** para que você possa adicionar ambos em seu aplicativo MVC posteriormente no tutorial. Além disso, adicione a URL do Site (`https://localhost:44300/`) para testar seu aplicativo MVC. Além disso, adicione uma **Contact Email**. Em seguida, selecione **salvar alterações**.   

    ![Página de detalhes do aplicativo básico](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Observe que você só poderá se autenticar usando o alias de email que você registrou. Outros usuários e contas de teste não poderão se registrar. Você pode conceder outro acesso de contas do Facebook para o aplicativo do Facebook **funções do desenvolvedor** guia.
10. No Visual Studio, abra *App\_Start\Startup.Auth.cs*.
11. Copie e cole a **AppId** e **segredo do aplicativo** para o `UseFacebookAuthentication` método. O **AppId** e **segredo do aplicativo** valores mostrados abaixo são exemplos e não funcionará.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Clique em **salvar alterações**.
13. Pressione **CTRL + F5** para executar o aplicativo.


Selecione **faça logon no** para exibir a página de logon. Clique em **Facebook** sob **Use outro serviço para fazer logon.**

Insira suas credenciais do Facebook.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Você será solicitado a conceder permissão para que o aplicativo acesse seu perfil público e uma lista de amigos.

![Detalhes do aplicativo Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Agora você está conectado. Agora você pode registrar essa conta com o aplicativo.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Quando você se registra, uma entrada é adicionada para o *usuários* tabela do banco de dados de associação.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examinar os dados de associação

No **modo de exibição** menu, clique em **Gerenciador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Expandir **DefaultConnection (MvcAuth)**, expanda **tabelas**, clique com botão direito **AspNetUsers** e clique em **Mostrar dados da tabela**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dados da tabela aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Adicionar dados de perfil para a classe de usuário

Nesta seção você adicionará data de nascimento e cidade para os dados do usuário durante o registro, conforme mostrado na imagem a seguir.

![reg com a cidade e Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Abra o *Models\IdentityModels.cs* arquivo e adicione as propriedades de cidade de data e a página inicial de nascimento:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Abra o *Models\AccountViewModels.cs* de arquivo e o conjunto de propriedades de cidade de data e a página inicial de nascimento `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Abra o *Controllers\AccountController.cs* arquivo e adicione o código para a cidade de data e a página inicial de nascimento do `ExternalLoginConfirmation` método de ação, conforme mostrado:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Adicionar data de nascimento e cidade para os *Views\Account\ExternalLoginConfirmation.cshtml* arquivo:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Exclua o banco de dados de associação para que você pode registrar sua conta do Facebook com seu aplicativo novamente e verifique se que você pode adicionar a nova data de nascimento e informações de perfil de cidade.

Da **Gerenciador de soluções**, clique no **Show All Files** ícone e o botão direito do mouse *Add\_Data\aspnet-MvcAuth -&lt;carimbo de data&gt;. mdf* e clique em **excluir**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Dos **ferramentas** menu, clique em **Gerenciador de pacotes NuGet**, em seguida, clique em **Package Manager Console** (PMC). Digite os seguintes comandos no PMC.

1. Enable-Migrations
2. Add-Migration Init
3. Atualizar banco de dados

Execute o aplicativo e usar o FaceBook e Google para fazer logon e registrar alguns usuários.

## <a name="examine-the-membership-data"></a>Examinar os dados de associação

No **modo de exibição** menu, clique em **Gerenciador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Clique com botão direito **AspNetUsers** e clique em **Mostrar dados da tabela**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

O `HomeTown` e `BirthDate` campos são mostrados abaixo.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Fazer logoff do seu aplicativo e fazendo logon com outra conta

Se fazer logon no seu aplicativo com o Facebook e, em seguida, faça logoff e tente fazer logon novamente com uma conta diferente do Facebook (usando o mesmo navegador), você será imediatamente conectado à conta do Facebook anterior, que você usou. Para usar outra conta, você precisa navegar para o Facebook e faça logoff no Facebook. A mesma regra se aplica a qualquer outro provedor do autenticação de terceiros 3ª. Como alternativa, você pode fazer logon com outra conta usando um navegador diferente.

## <a name="next-steps"></a>Próximas etapas

Ver [apresentando os provedores de segurança do Yahoo e do OAuth do LinkedIn para OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) por Jerrie Pelser para obter instruções do Yahoo e LinkedIn. Consulte do Jerrie praticamente botões de logon social para ASP.NET MVC 5 obter os botões de logon social enable.

Siga o tutorial [criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no serviço de aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que continua a este tutorial e mostra o seguinte:

1. Como implantar seu aplicativo no Azure.
2. Como proteger seu aplicativo com as funções.
3. Como proteger seu aplicativo com o [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [autorizar](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtros.
4. Como usar a API de associação para adicionar usuários e funções.

Deixe comentários sobre como você gostou neste tutorial e o que poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Você pode até mesmo solicitar e votar em novos recursos a serem adicionadas ao ASP.NET. Por exemplo, você poderá votar para uma ferramenta de [criar e gerenciar usuários e funções.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Para uma boa explicação de como funcionam os serviços de autenticação externos do ASP.NET, consulte de Robert McMurray [serviços de autenticação externos](https://asp.net/web-api/overview/security/external-authentication-services). O artigo de Robert também entra em detalhes na habilitação da autenticação da Microsoft e Twitter. Tom Dykstra do excelente [tutorial do EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) mostra como trabalhar com o Entity Framework.
