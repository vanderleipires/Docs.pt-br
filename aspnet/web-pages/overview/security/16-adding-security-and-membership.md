---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Adição de segurança e associação a uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: Rick-Anderson
description: Este capítulo mostra como proteger seu site para que algumas páginas estão disponíveis somente para as pessoas que fazem logon no. (Você também verá como criar páginas tha...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 1c36adf23f3b53e4fbf3dbdce7ca85664b32c975
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021589"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Adição de segurança e associação a um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como proteger um site de páginas da Web do ASP.NET (Razor) para que algumas páginas estão disponíveis somente para as pessoas que fazem logon no. (Você também verá como criar páginas que qualquer pessoa pode acessar.)
> 
> **O que você aprenderá:** 
> 
> - Como criar um site que tem uma página de registro e uma página de logon de forma que para algumas páginas, você pode limitar o acesso a somente os membros.
> - Como criar páginas públicas e somente a membros.
> - Como definir funções, que são grupos que têm permissões de segurança diferentes em seu site, e como atribuir usuários a uma função.
> - Como usar o CAPTCHA para impedir que programas automatizados (bots) desde a criação de contas de membro.
>   
> 
> Estes são os recursos do ASP.NET introduzidos no artigo:
> 
> - O WebMatrix **Starter Site** modelo.
> - O `WebSecurity` auxiliar e `Roles` classe.
> - O `ReCaptcha` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - O WebMatrix 3
> - ASP.NET Web Helpers Library


Você pode configurar seu site para que os usuários podem fazer nele &#8212; ou seja, para que o site suporta *associação*. Isso pode ser útil por vários motivos. Por exemplo, seu site pode ter páginas que devem estar disponíveis apenas para membros. Em alguns casos, você pode exigir que os usuários façam logon no fim de enviar comentários ou deixe um comentário.

Mesmo se seu site dá suporte à associação, os usuários não são necessariamente necessários para fazer logon antes de usar algumas das páginas no site. Usuários que não estão conectados são conhecidos como *os usuários anônimos*.

Um usuário pode registrar no seu site e, em seguida, pode fazer logon site. O site requer um nome de usuário (um endereço de email) e uma senha para confirmar que os usuários são quem dizem ser. Esse processo de registro em log no e confirmar a identidade do usuário é conhecido como *autenticação*.

Você pode configurar segurança e associação de maneiras diferentes:

- Se você estiver usando o WebMatrix, uma maneira fácil é criar como novo site com base nas **Starter Site** modelo. Este modelo já está configurado para segurança e a associação e já tem uma página de registro, uma página de logon e assim por diante.

    O site criado pelo modelo também tem uma opção para permitir aos usuários fazer logon usando um site externo, como Facebook, Google ou Twitter.
- Se você quiser adicionar segurança para um site existente, ou se você não quiser usar o **Starter Site** modelo, você pode criar sua própria página de registro, página de logon e assim por diante.

Este artigo enfoca a primeira opção &mdash; como adicionar segurança usando o **Starter Site** modelo. Ele também fornece algumas informações básicas sobre como implementar sua própria segurança e, em seguida, fornece links para obter mais informações sobre como fazer isso. Há também informações sobre como habilitar os logons externos, que é descrito mais detalhadamente em outro artigo.

## <a name="creating-website-security-using-the-starter-site-template"></a>Criação de segurança de site usando o modelo de Site inicial

No WebMatrix, você pode usar o **Starter Site** modelo para criar um site que contém o seguinte:

- Um banco de dados que é usado para armazenar nomes de usuário e senhas para seus membros.
- Uma página de registro em que os usuários anônimos de (novos) podem registrar.
- Uma página de logon e logoff.
- Uma página de recuperação e redefinição de senha.

O procedimento a seguir descreve como criar o site e configurá-lo.

1. Inicie o WebMatrix e, na **início rápido** página, selecione **Site From Template**.
2. Selecione o **Starter Site** modelo e clique **Okey**. O WebMatrix cria um novo site.
3. No painel esquerdo, clique no **arquivos** seletor de espaço de trabalho.
4. Na pasta raiz do seu site, abra o  *\_AppStart.cshtml* arquivo, que é um arquivo especial que é usado para conter as configurações globais. Ele contém algumas instruções que são comentadas usando o `//` caracteres:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Configurar essas instruções o `WebMail` auxiliar, que pode ser usado para enviar email. O sistema de associação pode usar o email para enviar mensagens de confirmação quando os usuários registram ou quando desejarem alterar suas senhas. (Por exemplo, depois que os usuários se registrarem, elas recebem um email que inclui um link que eles possam clicar para concluir o processo de registro.)

    Envio de email requer acesso a um servidor SMTP, conforme descrito em [adicionando o Email para um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202899). Você armazenará as configurações de email nesse central  *\_AppStart.cshtml* arquivo para que você não tiver de codificá-los várias vezes em cada página que pode enviar um email. (Você não precisa definir as configurações de SMTP para configurar um banco de dados do registro; você só precisa configurações de SMTP para validar os usuários de seu alias de email e permitir que os usuários redefinir uma senha esquecida.)
5. Remova as instruções, removendo `//` from in front of cada uma delas.

    Se você não quiser configurar o email de confirmação, você pode ignorar esta etapa e a próxima etapa. Se os valores de SMTP não forem definidos, a nova conta está imediatamente disponível sem um email de confirmação.
6. Modifique as seguintes configurações de email no código:

   - Definir `WebMail.SmtpServer` como o nome do servidor SMTP que você tem acesso.
   - Deixe `WebMail.EnableSsl` definido como `true`. Essa configuração protege as credenciais que são enviadas para o servidor SMTP, criptografando-os.
   - Definir `WebMail.UserName` ao nome de usuário para sua conta do servidor SMTP.
   - Definir `WebMail.Password` para a senha de sua conta do servidor SMTP.
   - Definir `WebMail.From` para seu próprio endereço de email. Esse é o endereço de email que a mensagem é enviada.

     > [!NOTE] 
     > 
     > **Dica** para obter informações adicionais sobre os valores para essas propriedades, consulte [definindo as configurações de Email](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) na [Personalizando o comportamento de todo o Site do ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Salve e feche  *\_AppStart.cshtml*.
8. Execute o *cshtml* página em um navegador.

    ![segurança de associação de 2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Se você vir um erro informando que uma propriedade deve ser uma instância de `ExtendedMembershipProvider`, o site não pode ser configurado para usar o sistema de associação do ASP.NET Web Pages (SimpleMembership). Às vezes, isso pode ocorrer se o servidor de um provedor de hospedagem está configurado de maneira diferente de seu servidor local. Para corrigir isso, adicione o seguinte elemento para o site *Web. config* arquivo:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Adicionar esse elemento como um filho de `<configuration>` elemento e como um par da `<system.web>` elemento.
9. No canto superior direito da página, clique no **registrar** link. O *Register. cshtml* página é exibida.
10. Insira um nome de usuário e senha e, em seguida, clique em **registrar**.

    ![segurança de associação de 3](16-adding-security-and-membership/_static/image2.png)

    Quando você criou o site a partir o **Starter Site** modelo, um banco de dados denominado *StarterSite.sdf* foi criado no site do *aplicativo\_dados* pasta. Durante o registro, suas informações de usuário são adicionadas ao banco de dados. Se você definir os valores de SMTP, uma mensagem é enviada para o endereço de email usado para concluir o registro.

    ![segurança de associação de 4](16-adding-security-and-membership/_static/image3.png)
11. Vá para o seu programa de email e localizar a mensagem, que terá seu código de confirmação e um hiperlink para o site.
12. Clique no hiperlink para ativar sua conta. O hiperlink de confirmação é aberto em uma página de confirmação do registro.

    ![segurança de associação de 5](16-adding-security-and-membership/_static/image4.png)
13. Clique o **Login** vincular e, em seguida, faça logon usando a conta que você registrou.

      Depois que você fizer logon, o **Login** e **registrar** links são substituídos por um **Logout** link. Seu nome de logon é exibido como um link. (O link permite que você acesse uma página onde você pode alterar sua senha.)

      ![segurança de associação de 6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Por padrão, o páginas da web ASP.NET enviar credenciais ao servidor em texto não criptografado (como texto legível por humanos). Um site de produção deve usar o HTTP seguro (https://, também conhecido como o *SSL* ou SSL) para criptografar informações confidenciais que são trocadas com o servidor. Você pode email necessário as mensagens sejam enviadas usando SSL definindo `WebMail.EnableSsl=true` do exemplo anterior. Para obter mais informações sobre SSL, consulte [Protegendo comunicações da Web: https://, SSL e certificados](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funcionalidade de associação adicionais no Site

Seu site contém outra funcionalidade que permite aos usuários gerenciar suas contas. Os usuários podem fazer o seguinte:

- Altere suas senhas. Depois de entrar, eles podem clicar o nome de usuário (que é um link). Isso leva a uma página em que eles podem criar uma nova senha (*Account/ChangePassword.cshtml*).
- Recupere uma senha esquecida. Na página de logon, há um link (**esqueceu sua senha?**) que leva os usuários para uma página (*Account/ForgotPassword.cshtml*) no qual eles podem digitar um endereço de email. O site envia uma mensagem de email que tem um link que eles possam clicar para definir uma nova senha (*Account/PasswordReset.cshtml*).

Você também pode permitir que os usuários podem também o logon usando um site externo, conforme explicado mais tarde.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Criando uma página exclusiva

Por enquanto, qualquer pessoa pode navegar para qualquer página no seu site. Mas você talvez queira ter páginas que estão disponíveis somente para as pessoas que fizeram logon (ou seja, aos membros). ASP.NET permite que você crie páginas que podem ser acessadas somente por membros de entrar. Normalmente, se os usuários anônimos tentarem acessar uma página exclusiva, você redirecioná-las para a página de logon.

Neste procedimento, você criará uma pasta que contém as páginas que estão disponíveis somente para os usuários conectados.

1. Na raiz do site, crie uma nova pasta. (Na faixa de opções, clique na seta sob **New** e, em seguida, escolha **nova pasta**.)
2. Nomeie a nova pasta *membros*.
3. Dentro de *membros* pasta, crie uma nova página e o renomeie *MembersInformation.cshtml*.
4. Substitua o conteúdo existente com o código e marcação a seguir:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Esse código testa os `IsAuthenticated` propriedade do `WebSecurity` objeto, que retorna `true` se o usuário fez login. Se o usuário não estiver conectado, o código chama `Response.Redirect` para enviar o usuário para o *cshtml* página a *conta* pasta.

    A URL de redirecionamento inclui um `returnUrl` consultar o valor de cadeia de caracteres que usa `Request.Url.LocalPath` para definir o caminho da página atual. Se você definir o `returnUrl` valor na cadeia de caracteres de consulta como esta (e se a URL de retorno é um caminho local), a página de logon retornará os usuários a esta página depois que ele fizer logon.

    O código também define  *\_SiteLayout.cshtml* página como sua página de layout. (Para obter mais informações sobre páginas de layout, consulte [criando um Layout consistente em Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Execute o site. Se você ainda estiver conectado, clique no **Logout** botão na parte superior da página.
6. No navegador, solicite a página */membros/MembersInformation*. Por exemplo, a URL pode ser assim:

    `http://localhost:38366/Members/MembersInformation`

    (O número da porta (38366) provavelmente será diferente em sua URL.)

    Você será redirecionado para o *cshtml* página, porque você não estiver conectado.
7. Faça logon usando a conta que você criou anteriormente. Você será redirecionado para o *MembersInformation* página. Porque você está conectado, neste momento você ver o conteúdo da página.

Para proteger o acesso a várias páginas, você pode fazer isso:

- Adicione a verificação de segurança para cada página.
- Criar uma  *\_Pagestart* página na pasta onde você manter páginas protegidas e adicionar a verificação de segurança. O  *\_Pagestart* página atua como um tipo de página global para todas as páginas na pasta. Essa técnica é explicada mais detalhadamente [Personalizando o comportamento de todo o Site do ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Criação de segurança para grupos de usuários (funções)

Se seu site possui muitos membros, não é eficiente para verificar as permissões para cada usuário individualmente antes de deixá-los ver uma página. Em vez disso, o que você pode fazer é criar grupos, ou *funções*, que os membros individuais pertencem. Em seguida, você pode verificar permissões com base em função. Nesta seção, você criará um &quot;admin&quot; função e, em seguida, crie uma página que está acessível a usuários que estão em (que pertencem ao) dessa função.

Configurar o sistema de associação do ASP.NET para dar suporte a funções. No entanto, ao contrário de registro de associação e login, o **Starter Site** modelo não contém páginas que ajudam você a gerenciar funções. (Gerenciamento de funções é uma tarefa administrativa em vez de uma tarefa de usuário). No entanto, você pode adicionar grupos diretamente no banco de dados de associação no WebMatrix.

1. No WebMatrix, clique o **bancos de dados** seletor de espaço de trabalho.
2. No painel esquerdo, abra o *StarterSite.sdf* nó, abra o **tabelas** nó e, em seguida, clique duas vezes o *páginas da Web\_funções* tabela.

    ![segurança de associação de 7](16-adding-security-and-membership/_static/image6.png)
3. Adicionar uma função denominada &quot;admin&quot;. O *RoleId* campo é preenchido automaticamente. (Ele é a chave primária e foi definido para ser um campo de identificação, conforme explicado em [Introdução ao trabalho com um banco de dados em Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Anote o que é o valor para o *RoleId* campo. (Se esta for a primeira função em que você está definindo, ele será 1.)

    ![segurança de associação de 8](16-adding-security-and-membership/_static/image7.png)
5. Fechar o *páginas da Web\_funções* tabela.
6. Abra o *UserProfile* tabela.
7. Anote o *UserId* valor de um ou mais dos usuários na tabela e em seguida, feche a tabela.
8. Abra o *páginas da Web\_UserInRoles* de tabela e insira um *UserID* e uma *RoleID* valor na tabela. Por exemplo, para colocar o usuário 2 para o &quot;admin&quot; função, você poderia inserir esses valores:

    ![segurança de associação de 9](16-adding-security-and-membership/_static/image8.png)
9. Fechar o *páginas da Web\_UsersInRoles* tabela.

    Agora que você tem funções definidas, você pode configurar uma página que está acessível a usuários que estão nessa função.
10. Na pasta raiz do site, crie uma nova página chamada *AdminError.cshtml* e substitua o conteúdo existente pelo código a seguir. Isso será a página que os usuários são redirecionados para se eles não tiver permissão de acesso a uma página.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Na pasta raiz do site, crie uma nova página chamada *AdminOnly.cshtml* e substitua o código existente pelo código a seguir:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    O `Roles.IsUserInRole` método retorna `true` se o usuário atual é um membro da função especificada (nesse caso, a função "admin").
12. Execute *cshtml* em um navegador, mas não fazer logon. (Se você já estiver conectado, faça logoff.)
13. Na barra de endereços do navegador, adicione *AdminOnly* na URL. (Em outras palavras, solicitar a *AdminOnly.cshtml* arquivo.) Você será redirecionado para o *AdminError.cshtml* página, porque você não estiver atualmente conectado como um usuário na &quot;admin&quot; função.
14. Retorne ao *default. cshtml* e faça logon como o usuário adicionado à &quot;admin&quot; função.
15. Navegue até *AdminOnly.cshtml* página. Neste momento, você verá a página.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Impedir que programas automatizados ingressando em seu site

A página de logon não irá parar programas automatizados (também conhecido como *web robôs* ou *bots*) ao registrar com o seu site. Este procedimento descreve como habilitar um teste de ReCaptcha para a página de registro.

![/Media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrar seu site em ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Quando você concluir o registro, você obterá uma chave pública e uma chave privada.
2. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não fez isso.
3. No *conta* pasta, abra o arquivo denominado *Register. cshtml*.
4. No código na parte superior da página, localize as seguintes linhas e remova os comentários, removendo o `//` caracteres de comentário:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Substitua `PRIVATE_KEY` com suas próprias chaves ReCaptcha.
6. Na marcação da página, remova os `@*` e `*@` caracteres de comentário da alternativa para as seguintes linhas na marcação da página:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Substitua `PUBLIC_KEY` com sua chave.
8. Se você ainda não o removemos já, remova o `<div>` elemento que contém o texto que começa com "Para habilitar a verificação CAPTCHA...". (Remova toda a `<div>` elemento e seu conteúdo.)

9. Execute *cshtml* em um navegador. Se você estiver conectado ao site, clique no **Logout** link.
10. Clique o **registrar** vinculação e teste o registro usando o teste CAPTCHA.

     ![segurança de associação de 10](16-adding-security-and-membership/_static/image9.png)

Para obter mais informações sobre o `ReCaptcha` auxiliar, consulte [usando um CATPCHA para evitar a programas automatizados (Bots) do usando o Site da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Permitindo que os usuários fazer logon usando um Site externo

O **Starter Site** modelo inclui o código e a marcação que permite aos usuários fazer logon usando o Facebook, Windows Live, Twitter, Google ou Yahoo. Por padrão, essa funcionalidade não está habilitada. O procedimento geral para uso de permitindo que usuários logon usando esses provedores externos é isso:

- Decida quais você deseja dar suporte dos sites externos.
- Se necessário, vá para o site e configurar um aplicativo de logon. (Por exemplo, você precisa fazer isso para permitir logons do Facebook.)
- No seu site, configure o provedor. Na maioria dos casos, você só precisa Remova alguns códigos na  *\_AppStart.cshtml* arquivo.
- Adicione a marcação para a página de registro que permite que as pessoas link para o site externo para fazer logon. Normalmente, você pode copiar a marcação que você precisa e altere o texto um pouco.

Você pode encontrar instruções passo a passo no tópico [habilitando o logon de Sites externos em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969).

Depois que um usuário fizer logon de outro site, o usuário retorna ao seu site e *associa* esse logon com seu site. Na verdade, isso cria uma entrada de associação do site para o logon do usuário externo. Isso permite que você use os recursos normais de associação (por exemplo, funções) com o logon externo.

## <a name="adding-security-to-an-existing-website"></a>Adição de segurança a um site existente

O procedimento neste artigo se baseia em usando o **Starter Site** modelo como base para a segurança de site. Se não for prático para você iniciar a partir de **Starter Site** modelo ou para copiar as páginas relevantes de um site com base nesse modelo, você pode implementar o mesmo tipo de segurança em seu próprio site por meio de codificação por conta própria. Você cria os mesmos tipos de páginas — registro, logon e assim por diante — e, em seguida, usar classes e os auxiliares para configurar a associação.

O processo básico é descrito na postagem do blog [a maneira mais simples para implementar a segurança do ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). A maioria do trabalho é feito usando os seguintes métodos e propriedades do `WebSecurity` auxiliar:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Esses métodos permitem que você determine se alguém já está registrado e registrá-los.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Essa propriedade permite determinar se o usuário atual está conectado. Isso é útil para redirecionar usuários para uma página de logon, se eles ainda não tiverem feito logon.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Esses métodos de logon de um usuário ou de saída.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Esta propriedade é útil para exibir o atual nome do usuário conectado (se o usuário estiver conectado).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Esse método é útil se você configurar o email de confirmação para o registro. (Detalhes são descritos na postagem do blog [usando o recurso de confirmação para a segurança de páginas da Web ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Para gerenciar funções, você pode usar o [funções](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) e [associação](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) classes, conforme descrito na entrada de blog.

## <a name="additional-resources"></a>Recursos adicionais

- [Personalizar o comportamento de todo o site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Proteção de comunicações da Web: Https://, SSL e certificados](https://go.microsoft.com/fwlink/?LinkId=208660)
- [A maneira mais simples para implementar a segurança do ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) e [usando o recurso de confirmação para a segurança de páginas da Web ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Essas são as postagens de blog que descrevem como implementar os recursos de associação do ASP.NET sem usar o **Starter Site** modelo.
- [Habilitar logon de sites externos em um site de Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Referência da API de classe WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Referência da API de classe SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Referência da API de classe SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
