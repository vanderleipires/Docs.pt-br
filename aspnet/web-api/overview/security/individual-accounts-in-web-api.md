---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Proteger uma API Web com contas individuais e logon Local na API Web ASP.NET 2.2 | Microsoft Docs
author: MikeWasson
description: Este tópico mostra como proteger uma API web usando OAuth2 para autenticar em relação a um banco de dados de associação. Versões de software usadas no tutorial 201 Visual do Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: f37cb206b78fd46835bb59bede5920f8e5acd3fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399932"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Proteger uma API Web com contas individuais e logon Local na API Web ASP.NET 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o aplicativo de exemplo](https://github.com/MikeWasson/LocalAccountsApp)

> Este tópico mostra como proteger uma API web usando OAuth2 para autenticar em relação a um banco de dados de associação.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013 Atualização 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [API Web 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [Identidade do ASP.NET 2.1](../../../identity/index.md)


No Visual Studio 2013, o modelo de projeto de API Web fornece três opções para autenticação:

- **Contas individuais.** O aplicativo usa um banco de dados de associação.
- **Contas organizacionais.** Os usuários entram com suas do Active Directory do Azure, Office 365 ou as credenciais do Active Directory local.
- **Autenticação do Windows.** Essa opção destina-se a aplicativos de Intranet e usa o módulo do IIS de autenticação do Windows.

Para obter mais detalhes sobre essas opções, consulte [Criando projetos Web do ASP.NET no Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Contas individuais fornecem duas maneiras para um usuário faça logon:

- **Logon local**. O usuário se registra no site, inserir um nome de usuário e senha. O aplicativo armazena o hash de senha no banco de dados de associação. Quando o usuário fizer logon, o sistema ASP.NET Identity verificará a senha.
- **Logon social**. O usuário entra com um serviço externo, como Facebook, Microsoft ou Google. O aplicativo ainda cria uma entrada para o usuário no banco de dados de associação, mas não armazena credenciais. O usuário é autenticado depois de entrar em um serviço externo.

Este artigo aborda o cenário de logon local. Para logon local e social, API Web usa OAuth2 para autenticar solicitações. No entanto, os fluxos de credencial são diferentes para logon social e local.

Neste artigo, demonstrarei um aplicativo simples que permite que o usuário faça logon e enviar chamadas AJAX autenticadas para uma API da web. Você pode baixar o código de exemplo [aqui](https://github.com/MikeWasson/LocalAccountsApp). O arquivo leiame descreve como criar o exemplo do zero no Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

O aplicativo de exemplo usa o Knockout. js para vinculação de dados e jQuery para enviar solicitações AJAX. O foco será nas chamadas AJAX, portanto você não precisa saber o Knockout. js para este artigo.

Ao longo do caminho, que descreverei:

- O que o aplicativo está fazendo no lado do cliente.
- O que está acontecendo no servidor.
- O tráfego HTTP no meio.

Primeiro, precisamos definir algumas terminologias OAuth2.

- *Recurso*. Alguma parte dos dados que podem ser protegidos.
- *Servidor de recurso*. O servidor que hospeda o recurso.
- *Proprietário do recurso*. A entidade que pode conceder permissão para acessar um recurso. (Normalmente o usuário.)
- *Cliente*: O aplicativo que deseja acessar o recurso. Neste artigo, o cliente é um navegador da web.
- *Token de acesso*. Um token que concede acesso a um recurso.
- *Token de portador*. Um determinado tipo de token de acesso, com a propriedade que qualquer pessoa pode usar o token. Em outras palavras, um cliente não precisa de uma chave de criptografia ou outro segredo para usar um token de portador. Por esse motivo, os tokens de portador só devem ser usados por um HTTPS e devem ter tempos de expiração relativamente curto.
- *Servidor de autorização*. Um servidor que fornece tokens de acesso.

Um aplicativo pode agir como servidor de autorização e o servidor de recursos. O modelo de projeto de Web API segue esse padrão.

## <a name="local-login-credential-flow"></a>Fluxo de credenciais de logon local

Para logon local, API Web usa a [fluxo de senha de proprietário do recurso](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definidos no OAuth2.

1. O usuário insere um nome e uma senha para o cliente.
2. O cliente envia essas credenciais para o servidor de autorização.
3. O servidor de autorização autentica as credenciais e retorna um token de acesso.
4. Para acessar um recurso protegido, o cliente inclui o token de acesso no cabeçalho de autorização da solicitação HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Quando você seleciona **contas individuais** no modelo de projeto de API da Web, o projeto inclui um servidor de autorização que valida as credenciais do usuário e emite tokens. O diagrama a seguir mostra o mesmo fluxo de credenciais em termos de componentes de API da Web.

![](individual-accounts-in-web-api/_static/image4.png)

Nesse cenário, os controladores de API Web atuam como servidores de recurso. Um filtro de autenticação valida os tokens de acesso e o **[autorizar]** atributo é usado para proteger um recurso. Quando um controlador ou ação tem o **[autorizar]** de atributo, todas as solicitações para esse controlador ou ação deve ser autenticada. Caso contrário, a autorização é negada e API Web retorna um erro de 401 (não autorizado).

O servidor de autorização e o filtro de autenticação que chamam em uma [middleware OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) componente que manipula os detalhes do OAuth2. Descreverei o design em mais detalhes posteriormente neste tutorial.

## <a name="sending-an-unauthorized-request"></a>Enviando uma solicitação não autorizada

Para começar, execute o aplicativo e clique no **chamar a API** botão. Quando a solicitação for concluída, você verá uma mensagem de erro no **resultado** caixa. Isso ocorre porque a solicitação não contém um token de acesso, portanto, a solicitação é autorizada.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

O **chamar a API** botão envia uma solicitação AJAX ao ~/api/valores, que invoca uma ação do controlador de API da Web. Aqui está a seção de código JavaScript que envia a solicitação AJAX. No aplicativo de exemplo, todo o código de aplicativo do JavaScript está localizado no arquivo Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Até que o usuário fizer logon, há nenhum token de portador e, portanto, nenhum cabeçalho de autorização na solicitação. Isso faz com que a solicitação para retornar um erro 401.

Aqui está a solicitação HTTP. (Eu usei [Fiddler](http://www.telerik.com/fiddler) para capturar o tráfego HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Resposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Observe que a resposta inclui um cabeçalho Www-Authenticate com o desafio definido como portador. Que indica que o servidor espera que um token de portador.

## <a name="register-a-user"></a>Registrar um usuário

No **registre** seção do aplicativo, insira um email e senha e clique no **registrar** botão.

Você não precisa usar um endereço de email válido para este exemplo, mas um aplicativo real deve confirmar o endereço. (Consulte [criar um aplicativo de web do ASP.NET MVC 5 seguro com logon, redefinição de senha e de confirmação de email](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) A senha, use algo como "Password1!", com uma letra maiuscula, letra minúscula, número e caractere não alfanumérico. Para manter o aplicativo simples, eu deixei de fora a validação do lado do cliente, portanto, se houver um problema com o formato da senha, você obterá um erro 400 (solicitação incorreta).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

O **registrar** botão envia uma solicitação POST para ~/api/Account/Register /. O corpo da solicitação é um objeto JSON que contém o nome e senha. Aqui está o código JavaScript que envia a solicitação:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Solicitação HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Resposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Essa solicitação é tratada pelo `AccountController` classe. Internamente, `AccountController` usa a identidade do ASP.NET para gerenciar o banco de dados de associação.

Se você executar o aplicativo localmente no Visual Studio, as contas de usuário são armazenadas no LocalDB, na tabela AspNetUsers. Para exibir as tabelas no Visual Studio, clique o **modo de exibição** menu, selecione **Gerenciador de servidores**, em seguida, expanda **conexões de dados**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Obter um acesso de Token

Até agora não fizemos nenhum OAuth, mas agora veremos o servidor de autorização do OAuth em ação, quando solicitamos um token de acesso. No **fazer logon** área do aplicativo de exemplo, insira o email e senha e clique em **Log In**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

O **fazer logon** botão envia uma solicitação para o ponto de extremidade de token. O corpo da solicitação contém os dados codificados de url do formulário a seguir:

- conceder\_tipo: "senha"
- nome de usuário: &lt;o email do usuário&gt;
- senha: &lt;senha&gt;

Aqui está o código JavaScript que envia a solicitação AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Se a solicitação for bem-sucedida, o servidor de autorização retorna um token de acesso no corpo da resposta. Observe que podemos armazenar o token no armazenamento de sessão, para usar mais tarde ao enviar solicitações para a API. Ao contrário de algumas formas de autenticação (como a autenticação baseada em cookie), o navegador não incluirá automaticamente o token de acesso em solicitações subsequentes. O aplicativo deve fazer isso explicitamente. Isso é algo bom, porque limita [vulnerabilidades CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Solicitação HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Você pode ver que a solicitação contém as credenciais do usuário. Você *deve* usar HTTPS para fornecer segurança de camada de transporte.

Resposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Para facilitar a leitura, eu recuada o JSON e truncado o token de acesso, que é muito longo.

O `access_token`, `token_type`, e `expires_in` propriedades são definidas pela especificação OAuth2. As outras propriedades (`userName`, `.issued`, e `.expires`) são apenas para fins informativos. Você pode encontrar o código que adiciona essas propriedades adicionais no `TokenEndpoint` método, no arquivo /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Enviar uma solicitação autenticada

Agora que temos um token de portador, fazemos uma solicitação autenticada para a API. Isso é feito definindo o cabeçalho de autorização na solicitação. Clique o **chamar a API** botão novamente para vê-lo.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Solicitação HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Resposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Fazer logoff

Porque o navegador não armazena em cache as credenciais ou o token de acesso, fazer logoff é simplesmente uma questão de "esquecer" o token, removendo-o do armazenamento de sessão:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Noções básicas sobre o modelo de projeto contas individuais

Quando você seleciona **contas individuais** no modelo de projeto de aplicativo Web ASP.NET, o projeto inclui:

- Um servidor de autorização OAuth2.
- Um ponto de extremidade de API da Web para gerenciar contas de usuário
- Um modelo do EF para armazenar contas de usuário.

Aqui estão as classes de aplicativo principal que implementam esses recursos:

- `AccountController`. Fornece um ponto de extremidade de API da Web para gerenciar contas de usuário. O `Register` ação é a única que usamos neste tutorial. Outros métodos na classe dão suporte a redefinição de senha, os logons sociais e outras funcionalidades.
- `ApplicationUser`, definido em /Models/IdentityModels.cs. Essa classe é o modelo do EF para contas de usuário no banco de dados de associação.
- `ApplicationUserManager`, definido no /App\_Start/IdentityConfig.cs essa classe deriva [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) e executa operações em contas de usuário, como a criação de um novo usuário, verificando as senhas e assim por diante e persiste automaticamente alterações no banco de dados.
- `ApplicationOAuthProvider`. Esse objeto se vincula ao middleware OWIN e processa os eventos gerados pelo middleware. Ele deriva [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configurando o servidor de autorização

StartupAuth.cs, o código a seguir configura o servidor de autorização OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

O `TokenEndpointPath` propriedade é o caminho de URL para o ponto de extremidade do servidor de autorização. Essa é a URL que o aplicativo usa para obter os tokens de portador.

O `Provider` propriedade especifica um provedor que se vincula ao middleware OWIN e processa os eventos gerados pelo middleware.

Aqui está o fluxo básico quando o aplicativo deseja obter um token:

1. Para obter um token de acesso, o aplicativo envia uma solicitação para ~ / Token.
2. As chamadas de middleware OAuth `GrantResourceOwnerCredentials` no provedor.
3. O provedor chamar o `ApplicationUserManager` para validar as credenciais e criar uma identidade baseada em declarações.
4. Se isso ocorrer, o provedor cria um tíquete de autenticação, que é usado para gerar o token.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

O middleware OAuth não sabe nada sobre as contas de usuário. O provedor se comunica entre a identidade do ASP.NET e middleware. Para obter mais informações sobre como implementar o servidor de autorização, consulte [servidor de autorização OAuth 2.0 OWIN](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configurando a API da Web para usar os Tokens de portador

No `WebApiConfig.Register` método, o código a seguir configura a autenticação para o pipeline da API da Web:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

O **HostAuthenticationFilter** classe permite a autenticação usando tokens de portador.

O **SuppressDefaultHostAuthentication** método informa a API da Web para ignorar qualquer autenticação que acontece antes que a solicitação atinja o pipeline da API Web por IIS ou o middleware do OWIN. Dessa forma, podemos pode restringir a API da Web para autenticar usando apenas tokens de portador.

> [!NOTE]
> Em particular, a parte MVC do seu aplicativo pode usar a autenticação de formulários, que armazena as credenciais em um cookie. Autenticação baseada em cookie requer o uso de tokens antifalsificação, para evitar ataques de CSRF. Isso é um problema para APIs web, porque não há nenhuma maneira conveniente para a API da web para enviar o token antifalsificação ao cliente. (Para obter mais informações sobre esse problema, consulte [impedindo ataques de CSRF na API Web](preventing-cross-site-request-forgery-csrf-attacks.md).) Chamando **SuppressDefaultHostAuthentication** garante que a API da Web não está vulnerável a ataques de CSRF de credenciais armazenadas em cookies.


Quando o cliente solicita um recurso protegido, aqui está o que acontece no pipeline da API Web:

1. O **HostAuthentication** filtro chama o middleware de OAuth para validar o token.
2. O middleware converte o token em uma identidade baseada em declarações.
3. Neste ponto, a solicitação é *autenticado* , mas não *autorizado*.
4. O filtro de autorização examina a identidade baseada em declarações. Se as declarações de autorizam o usuário para esse recurso, a solicitação é autorizada. Por padrão, o **[autorizar]** atributo autorizará qualquer solicitação que é autenticada. No entanto, você pode autorizar por função ou por outras declarações. Para obter mais informações, consulte [autenticação e autorização na API Web](authentication-and-authorization-in-aspnet-web-api.md).
5. Se as etapas anteriores forem bem-sucedidas, o controlador retorna o recurso protegido. Caso contrário, o cliente recebe um erro de 401 (não autorizado).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Recursos adicionais

- [Identidade do ASP.NET](../../../identity/index.md)
- [Noções básicas sobre os recursos de segurança no modelo do SPA para VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Postagem do blog do MSDN por Hongye Sun.
- [Dissecando o indivíduo de API da Web contas modelo – parte 2: contas locais](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Postagem de blog de Dominick Baier.
- [Hospedar a autenticação e a API Web com OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Uma boa explicação sobre `SuppressDefaultHostAuthentication` e `HostAuthenticationFilter` por Brock Allen.
- [Personalizando informações de perfil no ASP.NET Identity em modelos do VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Postagem de blog do MSDN de Pranav Rastogi.
- [Por gerenciamento de tempo de vida de solicitação para a classe UserManager no ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Postagem de blog do MSDN de Suhas Joshi, com uma boa explicação sobre o `UserManager` classe.
