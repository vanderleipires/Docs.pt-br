---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Fazer logon usando Sites externos em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: Rick-Anderson
description: Este artigo explica como fazer logon seu site de páginas da Web do ASP.NET (Razor) usando o Facebook, Google, Twitter, Yahoo e outros sites — ou seja, como dar suporte a...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 188a9203ba7b04f5a88d0f802f1a05bf35d58d8c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021125"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Fazer logon usando Sites externos em um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como fazer logon seu site de páginas da Web do ASP.NET (Razor) usando o Facebook, Google, Twitter, Yahoo e outros sites — ou seja, como dar suporte a OAuth e OpenID no seu site.
> 
> O que você aprenderá:
> 
> - Como habilitar o logon de outros sites quando você usa o modelo de Site do WebMatrix inicial.
> 
> Esse é o recurso ASP.NET introduzido no artigo:
> 
> - O `OAuthWebSecurity` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - O WebMatrix 3

Páginas da Web ASP.NET inclui suporte para [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provedores. Usando esses provedores, você pode permitir que os usuários façam logon em seu site usando as credenciais existentes do Facebook, Twitter, Microsoft e Google. Por exemplo, para fazer logon usando uma conta do Facebook, os usuários podem escolher apenas um ícone de Facebook, que redireciona para a página de logon do Facebook em que eles inserirem suas informações de usuário. Em seguida, podem associar o logon do Facebook com sua conta em seu site. Um aprimoramento relacionado aos recursos de associação de páginas da Web é que os usuários podem associar vários logons (incluindo logons de sites de rede social) com uma única conta em seu site.

Esta imagem mostra a página de logon a partir de **Starter Site** modelo, em que um usuário pode escolher um ícone do Facebook, Twitter, Google ou Microsoft para habilitar o registro em log com uma conta externa:

![provedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Você pode habilitar a associação de OAuth e OpenID removendo algumas linhas de código na **Starter Site** modelo. Os métodos e propriedades que você usa para trabalhar com o OAuth e OpenID provedores estão no `WebMatrix.Security.OAuthWebSecurity` classe. O **Starter Site** modelo inclui uma infraestrutura completa de associação, completo com uma página de logon, um banco de dados de associação e todo o código necessário permitir que os usuários façam logon em seu site usando credenciais locais ou aqueles de outro site .

Esta seção fornece um exemplo de como permitir que usuários logon de sites externos a um site que se baseia o **Starter Site** modelo. Depois de criar um site inicial, você tanto (detalhes):

- Para os sites que usam um provedor de OAuth (Facebook, Twitter e Microsoft), você pode criar um aplicativo no site externo. Isso lhe dá as chaves de aplicativo que são necessários para invocar o recurso de logon para esses sites.
- Para sites que usam um provedor do OpenID (Google), você não precisa criar um aplicativo. Todos esses sites, você deve ter uma conta para fazer logon e para criar aplicativos de desenvolvedor.

    > [!NOTE]
    > Aplicativos da Microsoft só aceitam uma URL ao vivo para um site de trabalho, portanto você não pode usar uma URL do site local para logons de teste.
- Edite alguns arquivos no seu site para especificar o provedor de autenticação apropriado e enviar um logon para o site que você deseja usar.

Este artigo fornece instruções separadas para as seguintes tarefas:

- [Habilitar os logons do Google](#To_enable_Google_logins)
- [Habilitar os logons do Facebook](#To_enable_Facebook_logins)
- [Habilitar os logons do Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Habilitar os logons do Google

1. Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.
2. Abra o  *\_AppStart.cshtml* página e remova a seguinte linha de código. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testar o logon do Google

1. Execute o *default. cshtml* página do seu site e escolha o **faça logon no** botão.
2. No *logon* página, o **Use outro serviço para fazer logon** , escolha qualquer um o **Google** ou **Yahoo** botão Enviar. Este exemplo usa o logon do Google. 

    A página da web redireciona a solicitação para a página de logon do Google.

    ![Entrar do Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Insira as credenciais para uma conta existente do Google.
4. Se o Google pede que você queira permitir *Localhost* para usar as informações da conta, clique em **permitir**.

    O código usa o token do Google para autenticar o usuário e, em seguida, retorna a esta página em seu site. Esta página permite que os usuários a associar seu logon do Google com uma conta existente no seu site, ou eles podem registrar uma nova conta no seu site para associar o logon externo com.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Escolha o **associar** botão. O navegador retorna à página inicial do seu aplicativo.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Habilitar os logons do Facebook

1. Vá para o [site de desenvolvedores do Facebook](https://developers.facebook.com/apps) (faça logon se você ainda não esteja conectado).
2. Escolha o **criar novo aplicativo** botão e, em seguida, siga os prompts para nomear e criar o novo aplicativo.
3. Na seção **selecione como o seu aplicativo se integrará ao Facebook**, escolha o **site** seção.
4. Preencha a **URL do Site** campo com a URL do seu site (por exemplo, `http://www.example.com`). O **domínio** campo é opcional; você pode usar isso para fornecer autenticação para um domínio inteiro (como *exemplo.com*). 

    > [!NOTE]
    > Se você estiver executando um site no computador local com uma URL como `http://localhost:12345` (em que o número é um número de porta local), você pode adicionar esse valor para o **URL do Site** field para testar seu site. No entanto, o número da porta de suas alterações de site local a qualquer momento, você precisará atualizar o **URL do Site** campo do seu aplicativo.
5. Escolha o **salvar alterações** botão.
6. Escolha o **aplicativos** guia novamente e, em seguida, exibir a página inicial do seu aplicativo.
7. Cópia de **ID do aplicativo** e **segredo do aplicativo** valores para seu aplicativo e colá-los em um arquivo de texto temporário. Você irá passar esses valores para o provedor do Facebook no código do site.
8. Saia do site do desenvolvedor do Facebook.

Agora você fazer alterações para duas páginas em seu site para que os usuários serão capazes de fazer logon no site usando suas contas do Facebook.

1. Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.
2. Abra o  *\_AppStart.cshtml* página e remova o código para o provedor de OAuth do Facebook. O bloco de código sem marca de comentário é semelhante ao seguinte: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Cópia de **ID do aplicativo** o valor do aplicativo Facebook como o valor do `appId` parâmetro (entre aspas).
4. Cópia **segredo do aplicativo** o valor do aplicativo Facebook como o `appSecret` valor do parâmetro.
5. Salve e feche o arquivo.

### <a name="testing-facebook-login"></a>Testar o logon do Facebook

1. Executar o site *default. cshtml* da página e escolher o **Login** botão.
2. No *logon* página, o **Use outro serviço para fazer logon** , escolha o **Facebook** ícone. 

    A página da web redireciona a solicitação para a página de logon do Facebook.

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Faça logon em uma conta do Facebook. 

    O código usa o token do Facebook para autenticá-lo e, em seguida, retorna a uma página onde você pode associar seu logon do Facebook com o logon do seu site. Seu endereço de email ou nome de usuário é preenchido para o **Email** campo no formulário.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Escolha o **associar** botão. 

    O navegador retorna à home page e você está conectado.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Habilitar os logons do Twitter

1. Navegue até a [site de desenvolvedores do Twitter](https://dev.twitter.com/).
2. Escolha o **criar um aplicativo** vincular e, em seguida, faça logon no site.
3. Sobre o **criar um aplicativo** formar, preencha o **nome** e **descrição** campos.
4. No **site** , insira a URL do seu site (por exemplo, `http://www.example.com`). 

    > [!NOTE]
    > Se você estiver testando o seu site localmente (usando uma URL como `http://localhost:12345`), Twitter pode não aceitar a URL. No entanto, você poderá usar o endereço IP de loopback local (por exemplo `http://127.0.0.1:12345`). Isso simplifica o processo de testar seu aplicativo localmente. No entanto, sempre que o número da porta do seu site local for alterada, você precisará atualizar o **site** campo do seu aplicativo.
5. No **URL de retorno de chamada** , insira uma URL para a página em seu site que você deseja que os usuários retornem ao depois de fazer logon no Twitter. Por exemplo, para enviar aos usuários para a home page do Site inicial (que reconheça seu status conectado), insira a mesma URL que você inseriu na **site** campo.
6. Aceite os termos e escolha o **criar seu aplicativo Twitter** botão.
7. Sobre o **meus aplicativos** inicial da página, escolha o aplicativo que você criou.
8. Sobre o **detalhes** guia, role para baixo e escolha o **criar meu Token de acesso** botão.
9. Sobre o **detalhes** guia, copie o **chave do consumidor** e **segredo do consumidor** valores para seu aplicativo e colá-los em um arquivo de texto temporário. Você irá passar esses valores para o provedor do Twitter no código do site.
10. Saia do site do Twitter.

Agora você alterar duas páginas em seu site para que os usuários poderão fazer logon no site usando suas contas do Twitter.

1. Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.
2. Abra o  *\_AppStart.cshtml* página e remova o código para o provedor de OAuth do Twitter. O bloco de código sem marca de comentário tem esta aparência: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Cópia de **chave do consumidor** o valor do aplicativo Twitter como o valor do `consumerKey` parâmetro (entre aspas).
4. Cópia de **segredo do consumidor** o valor do aplicativo Twitter como o valor do `consumerSecret` parâmetro.
5. Salve e feche o arquivo.

### <a name="testing-twitter-login"></a>Testar o logon do Twitter

1. Execute o *default. cshtml* página do seu site e escolha o **Login** botão.
2. No *logon* página, o **Use outro serviço para fazer logon** , escolha o **Twitter** ícone. 

    A página da web redireciona a solicitação para uma página de logon do Twitter para o aplicativo que você criou.

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Faça logon em uma conta do Twitter.
4. O código usa o token do Twitter para autenticar o usuário e, em seguida, retorna a uma página onde você pode associar seu logon com sua conta do site. Seu nome ou endereço de email está preenchido para o **Email** campo no formulário.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Escolha o **associar** botão. 

    O navegador retorna à home page e você está conectado.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


- [Personalizar o comportamento de todo o site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Adicionar segurança e associação a um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
