---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Registro em log usando Sites externos em uma Web ASP.NET páginas Site (Razor) | Microsoft Docs
author: tfitzmac
description: Este artigo explica como fazer logon no seu site de páginas da Web do ASP.NET (Razor) usando o Google, Facebook, Twitter, Yahoo e outros sites — ou seja, como dar suporte...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530165"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Registro em log usando Sites externos em um Site de páginas (Razor) da Web do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como fazer logon no seu site de páginas da Web do ASP.NET (Razor) usando o Google, Facebook, Twitter, Yahoo e outros sites — ou seja, como dar suporte a OpenID e OAuth em seu site.
> 
> O que você aprenderá:
> 
> - Como habilitar o logon de outros sites quando você usa o modelo de Site do WebMatrix inicial.
> 
> Este é o recurso ASP.NET introduzido no artigo:
> 
> - O `OAuthWebSecurity` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - O WebMatrix 3

Páginas da Web ASP.NET inclui suporte para [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provedores. Usando esses provedores, você pode permitir que os usuários façam logon em seu site usando suas credenciais existentes do Facebook, Twitter, Microsoft e Google. Por exemplo, para efetuar login usando uma conta do Facebook, os usuários podem escolher apenas um ícone do Facebook, que redireciona para a página de logon do Facebook onde digite suas informações de usuário. Em seguida, podem associar o logon do Facebook com suas contas no seu site. Um aprimoramento relacionado aos recursos de associação de páginas da Web é que os usuários podem associar vários logons (incluindo logons de sites de rede social) com uma única conta no seu site.

Esta imagem mostra a página de logon a partir de **Site inicial** modelo, onde um usuário pode escolher um ícone do Facebook, Twitter, Google ou Microsoft para habilitar o registro em log com uma conta externa:

![provedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Você pode habilitar associação OpenID e OAuth, uncommenting algumas linhas de código a **Site inicial** modelo. Os métodos e propriedades que você usar para trabalhar com o OAuth e provedores de OpenID estão no `WebMatrix.Security.OAuthWebSecurity` classe. O **Site inicial** modelo inclui uma infraestrutura completa de associação, completo com uma página de logon, um banco de dados de associação e todo o código necessário permitir que os usuários façam logon em seu site usando credenciais locais ou outro site .

Esta seção fornece um exemplo de como permitir aos usuários fazer logon usando sites externos a um site que se baseia o **Site inicial** modelo. Depois de criar um site inicial, você tanto (detalhes):

- Para os sites que usam um provedor OAuth (Facebook, Twitter e Microsoft), você pode criar um aplicativo no site externo. Isso fornece chaves do aplicativo que você vai precisar para invocar o recurso de logon para esses sites.
- Para sites que usam um provedor OpenID (Google), você não precisa criar um aplicativo. Para todos os sites, você deve ter uma conta para efetuar login e criar aplicativos de desenvolvedor.

    > [!NOTE]
    > Aplicativos da Microsoft aceitam somente uma URL ao vivo para um site de trabalho, portanto você não pode usar uma URL do site local para logons de teste.
- Edite alguns arquivos em seu site para especificar o provedor de autenticação apropriado e enviar um logon para o site que você deseja usar.

Este artigo fornece instruções separadas para as seguintes tarefas:

- [Habilitar os logons do Google](#To_enable_Google_logins)
- [Habilitar os logons do Facebook](#To_enable_Facebook_logins)
- [Habilitar os logons do Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Habilitar os logons do Google

1. Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.
2. Abra o  *\_AppStart.cshtml* página e remova a linha de código a seguir. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Teste o logon do Google

1. Execute o *cshtml* página do seu site e escolha o **login** botão.
2. No *logon* página, o **utilize outro serviço para fazer logon no** seção, escolha o o **Google** ou **Yahoo** enviar botão. Este exemplo usa o logon do Google. 

    A página da web redireciona a solicitação para a página de logon do Google.

    ![Google entrar](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Insira as credenciais para uma conta existente do Google.
4. Se o Google pergunta se você deseja permitir *Localhost* para usar as informações da conta, clique em **permitir**.

    O código usa o token do Google para autenticar o usuário e, em seguida, retorna a esta página no seu site. Esta página permite que os usuários associar seu logon do Google com uma conta existente no seu site, ou eles podem registrar uma nova conta no seu site para associar o logon externo com.

    ![5 de OAuth](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Escolha o **associar** botão. O navegador retorna à página inicial do aplicativo.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Habilitar os logons do Facebook

1. Vá para o [site de desenvolvedores do Facebook](https://developers.facebook.com/apps) (entrar se você ainda não estiver conectado).
2. Escolha o **criar novo aplicativo** botão e, em seguida, siga os prompts para nomear e criar o novo aplicativo.
3. Na seção **selecione como o aplicativo integra-se ao Facebook**, escolha o **site** seção.
4. Preencha o **URL do Site** campo com a URL do seu site (por exemplo, `http://www.example.com`). O **domínio** campo é opcional; você pode usar isso para fornecer autenticação para um domínio inteiro (como *example.com*). 

    > [!NOTE]
    > Se você estiver executando um site no computador local com uma URL como `http://localhost:12345` (onde o número é um número de porta local), você pode adicionar esse valor para o **URL do Site** campo testando o seu site. No entanto, sempre que o número de porta de suas alterações do site local, você precisará atualizar o **URL do Site** campo do seu aplicativo.
5. Escolha o **salvar alterações** botão.
6. Escolha o **aplicativos** guia novamente e, em seguida, exibir a página inicial do seu aplicativo.
7. Copie o **ID do aplicativo** e **segredo do aplicativo** valores para o seu aplicativo e colá-los em um arquivo de texto temporário. Você passará esses valores para o provedor do Facebook no código do site.
8. Saia do site do desenvolvedor de Facebook.

Agora você fazer alterações para duas páginas em seu site para que os usuários poderão fazer logon no site usando suas contas do Facebook.

1. Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.
2. Abra o  *\_AppStart.cshtml* página e descomente o código para o provedor OAuth do Facebook. O bloco de código sem marca de comentário é semelhante ao seguinte: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copiar o **ID do aplicativo** valor do aplicativo Facebook como o valor da `appId` parâmetro (entre aspas).
4. Cópia **segredo do aplicativo** valor do aplicativo Facebook como o `appSecret` o valor do parâmetro.
5. Salve e feche o arquivo.

### <a name="testing-facebook-login"></a>Teste o logon do Facebook

1. Executar o site *cshtml* página e selecione o **Login** botão.
2. No *logon* página, o **utilize outro serviço para fazer logon no** , escolha o **Facebook** ícone. 

    A página da web redireciona a solicitação para a página de logon do Facebook.

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Faça logon em uma conta do Facebook. 

    O código usa o token do Facebook para autenticação e, em seguida, retorna a uma página onde você pode associar o logon do Facebook com o logon do seu site. Seu endereço de email ou nome de usuário é preenchido para o **Email** campo no formulário.

    ![5 de OAuth](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Escolha o **associar** botão. 

    Retorna o navegador para a home page e você está conectado.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Habilitar os logons do Twitter

1. Navegue até o [site de desenvolvedores do Twitter](https://dev.twitter.com/).
2. Escolha o **criar um aplicativo** link e, em seguida, faça logon no site.
3. Sobre o **criar um aplicativo** formulário, preencha o **nome** e **descrição** campos.
4. No **site** campo, insira a URL do seu site (por exemplo, `http://www.example.com`). 

    > [!NOTE]
    > Se você estiver testando o seu site localmente (usando uma URL como `http://localhost:12345`), Twitter pode não aceitar a URL. No entanto, você poderá usar o endereço IP de loopback local (por exemplo `http://127.0.0.1:12345`). Isso simplifica o processo de testar seu aplicativo localmente. No entanto, sempre que altera o número da porta do site local, você precisará atualizar o **site** campo do seu aplicativo.
5. No **URL de retorno de chamada** campo, digite uma URL para a página no seu site que você deseja que os usuários para retornar ao depois de fazer logon no Twitter. Por exemplo, para enviar aos usuários para a home page do Site inicial (que reconheça seu status conectado), digite a mesma URL que você inseriu no **site** campo.
6. Aceite os termos e escolha o **criar seu aplicativo Twitter** botão.
7. Sobre o **meus aplicativos** inicial da página, escolha o aplicativo que você criou.
8. Sobre o **detalhes** guia, role até a parte inferior e escolha o **criar meu Token de acesso** botão.
9. No **detalhes** guia, copie o **chave do consumidor** e **segredo do consumidor** valores para o seu aplicativo e colá-los em um arquivo de texto temporário. Você irá passar esses valores para o provedor do Twitter em seu código de site.
10. Saia do site do Twitter.

Agora você fazer alterações para duas páginas em seu site para que os usuários poderão fazer logon no site usando suas contas do Twitter.

1. Criar ou abrir um site de páginas da Web ASP.NET com base no modelo de Site do WebMatrix inicial.
2. Abra o  *\_AppStart.cshtml* página e descomente o código para o provedor OAuth do Twitter. O bloco de código sem marca de comentário tem esta aparência: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copiar o **chave do consumidor** valor do aplicativo como o valor do Twitter o `consumerKey` parâmetro (entre aspas).
4. Copiar o **segredo do consumidor** valor do aplicativo como o valor do Twitter o `consumerSecret` parâmetro.
5. Salve e feche o arquivo.

### <a name="testing-twitter-login"></a>Teste o logon do Twitter

1. Execute o *cshtml* página do seu site e escolha o **Login** botão.
2. No *logon* página, o **utilize outro serviço para fazer logon no** , escolha o **Twitter** ícone. 

    A página da web redireciona a solicitação para uma página de logon do Twitter para o aplicativo que você criou.

    ![4 de OAuth](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Faça logon em uma conta do Twitter.
4. O código usa o token do Twitter para autenticar o usuário e, em seguida, retorna a uma página onde você pode associar seu logon com sua conta do site. O nome ou endereço de email é preenchido para o **Email** campo no formulário.

    ![5 de OAuth](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Escolha o **associar** botão. 

    Retorna o navegador para a home page e você está conectado.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


- [Personalizar o comportamento de todo o Site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Adicionando a segurança e a associação a um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
