---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Adicionar projeto de formulários do ASP.NET Identity para uma Web vazio ou já existente | Microsoft Docs
author: raquelsa
description: Este tutorial mostra como adicionar a identidade do ASP.NET (o novo sistema de associação do ASP.NET) para um aplicativo ASP.NET. Quando você cria uma nova Web Forms ou MVC...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8961e596f0d6cc4810e2439be1ec2915bddb8c78
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874649"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Projeto de formulários de identidade do ASP.NET adicionando um Web vazio ou existente
====================
por [Raquel Soares De Almeida](https://github.com/raquelsa)

> Este tutorial mostra como adicionar [ASP.NET Identity](introduction-to-aspnet-identity.md) (o novo sistema de associação do ASP.NET) para um aplicativo ASP.NET.
> 
> Quando você cria um novo projeto MVC ou de Web Forms no Visual Studio 2013 RTM com contas individuais, Visual Studio instalará todos os pacotes necessários e adicionar todas as classes necessárias para você. Este tutorial ilustra as etapas para adicionar suporte a identidade do ASP.NET para o projeto Web Forms existente ou um novo projeto vazio. Vou descrever todos os pacotes do NuGet que é necessário instalar, de classes que você precisa adicionar. Entrarei em formulários da Web de exemplo para registrar novos usuários e fazer logon ao realce todas as APIs de ponto de entrada principal para autenticação e gerenciamento de usuário. Este exemplo usará a implementação do padrão de identidade do ASP.NET para o armazenamento de dados SQL que é criado no Entity Framework. Neste tutorial, vamos usar LocalDB para o banco de dados SQL.
> 
> Este tutorial foi escrito por Raquel Soares De Almeida e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Introdução a identidade do ASP.NET

1. Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Clique em **novo projeto** desde o início página, ou você pode usar o menu e selecione **arquivo**e, em seguida, **novo projeto**.
3. Selecione **Visual C# i** n painel esquerdo, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.NET**. Nomeie o projeto "WebFormsIdentity" e, em seguida, clique em **Okey**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Observe o **alterar autenticação** botão será desabilitado e não há suporte de autenticação é fornecida nesse modelo. Os modelos de Web Forms, MVC e API da Web permitem que você selecione o método de autenticação. Para obter mais informações, consulte [visão geral de autenticação](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Adicionando pacotes de identidade para seu aplicativo

No Gerenciador de soluções, clique com o botão direito e selecione **gerenciar pacotes NuGet**. Na caixa de diálogo de pesquisa texto, digite "*Identity.E*". Clique em instalar para este pacote.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Observe que este pacote irá instalar os pacotes de dependência: EntityFramework e Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Adição de Web Forms para registrar usuários

1. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar**e, em seguida, **formulário da Web**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. No **especificar nome para o Item** caixa de diálogo, o nome do novo formulário da web **registrar**e, em seguida, clique em **Okey**
3. Substitua a marcação em gerado *Register* arquivo com o código a seguir. As alterações de código são realçadas.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Isso é apenas uma versão simplificada do *Register* arquivo criado quando você cria um novo projeto ASP.NET Web Forms. A marcação acima adiciona campos do formulário e um botão para registrar um novo usuário.
4. Abra o *Register.aspx.cs* de arquivo e substituir o conteúdo do arquivo com o código a seguir:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. O código acima é uma versão simplificada do *Register.aspx.cs* arquivo criado quando você cria um novo projeto ASP.NET Web Forms.
    > 2. O *IdentityUser* classe é a implementação EntityFramework padrão a *IUser* interface. *IUser* interface é a interface mínima para um usuário no ASP.NET Identity Core.
    > 3. O *UserStore* classe é a implementação de EntityFramework padrão de um repositório do usuário. Essa classe implementa interfaces mínimas do ASP.NET Identity Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* e *IUserRoleStore* .
    > 4. O *UserManager* APIs que salvará automaticamente as alterações relacionadas ao usuário expõe o *UserStore*.
    > 5. O *IdentityResult* classe representa o resultado de uma operação de identidade.
5. Em **Solution Explorer**, clique com o botão direito e clique em **adicionar**, **adicionar pasta ASP.NET** e **aplicativo\_dados**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Abra o *Web. config* de arquivos e adicionar uma entrada de cadeia de caracteres de conexão para o banco de dados que usaremos para armazenar informações do usuário. O banco de dados será criado no tempo de execução por EntityFramework para as entidades de identidade. A cadeia de caracteres de conexão é semelhante a um criado quando você cria um novo projeto de formulários da Web. O código realçado mostra a marcação, você deve adicionar:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Para o Visual Studio 2015 ou superior, substitua `(localdb)\v11.0` com `(localdb)\MSSQLLocalDB` em sua cadeia de conexão.
    
7. Clique com botão direito arquivo *Register* no seu projeto e selecione **definir como página inicial**. Pressione Ctrl + F5 para compilar e executar o aplicativo web. Digite um novo nome de usuário e senha e, em seguida, clique em **registrar**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > Identidade do ASP.NET tem suporte para validação e neste exemplo, você pode verificar o comportamento padrão de usuário e senha validadores que vêm do pacote principal de identidade. O validador de padrão para o usuário (`UserValidator`) tem uma propriedade `AllowOnlyAlphanumericUserNames` que tem o valor padrão definido como `true`. O validador de padrão para a senha (`MinimumLengthValidator`) garante que a senha tem pelo menos 6 caracteres. Esses validadores são propriedades em `UserManager` que pode ser substituído se você deseja ter a validação personalizada,

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Verificando a identidade de LocalDb banco de dados e tabelas geradas pelo Entity Framework

1. No **exibição** menu, clique em **Server Explorer**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Expanda **DefaultConnection (WebFormsIdentity)**, expanda **tabelas**, clique com botão direito **AspNetUsers** e clique em **Mostrar dados da tabela**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Configurando o aplicativo para autenticação OWIN

Neste ponto, apenas adicionamos suporte para a criação de usuários. Agora, vamos demonstrar como é possível adicionar autenticação de logon de um usuário. Identidade do ASP.NET usa a autenticação do Microsoft OWIN middleware para autenticação de formulários. A autenticação de Cookie OWIN é um cookie e declarações de mecanismo de autenticação com base que pode ser usado por qualquer estrutura hospedada em [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) ou IIS. Com esse modelo, os mesmos pacotes de autenticação podem ser usados em várias estruturas, incluindo ASP.NET MVC e formulários da Web. Para obter mais informações sobre o projeto Katana e a execução de middleware em um consulte independente de host [guia de Introdução ao projeto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Instalando os pacotes de autenticação para seu aplicativo

1. No Gerenciador de soluções, clique com o botão direito e selecione **gerenciar pacotes NuGet**. Na caixa de diálogo de pesquisa texto, digite "*Identity.Owin*". Clique em instalar para este pacote.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Pesquise pacote ***systemweb*** e instalá-lo.   

    > [!NOTE]
    > O **Microsoft.Aspnet.Identity.Owin** pacote contém um conjunto de classes de extensão OWIN para gerenciar e configurar o middleware de autenticação OWIN a ser consumidos por pacotes de ASP.NET Identity Core.  
    > O **systemweb** pacote contém um servidor OWIN que permite que aplicativos baseados no OWIN para executar em IIS usando o pipeline de solicitação do ASP.NET. Para obter mais informações, consulte [Middleware OWIN no IIS integrado pipeline](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Adicionando Classes de configuração de autenticação e de inicialização OWIN

1. Em **Solution Explorer**, clique com o botão direito, clique em **adicionar**e, em seguida, **Adicionar Novo Item**. Na caixa de diálogo de pesquisa texto, digite "*owin*". Nomeie a classe "*inicialização*" e clique em **adicionar**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. No arquivo Startup.cs, adicione o código realçado abaixo para configurar a autenticação de cookie do OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Essa classe contém o `OwinStartup` atributo para especificar a classe de inicialização OWIN. Cada aplicativo OWIN tem uma classe de inicialização em que você especificar componentes para o pipeline do aplicativo. Consulte [detecção de classe de inicialização OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) para obter mais informações sobre esse modelo.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Adicionando formulários da Web para registrar e login de usuários

1. Abra o *Register.cs* de arquivo e adicione o seguinte código que registrará o usuário quando o registro terá êxito. As alterações estão destacadas abaixo.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Como identidade do ASP.NET e autenticação de Cookie OWIN são sistema baseada em declarações, o framework requer o desenvolvedor do aplicativo gerar um [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) para o usuário. ClaimsIdentity tem informações sobre todas as declarações para o usuário, como as funções que o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio.
    > - Você pode entrar o usuário usando o AuthenticationManager do OWIN e chamar `SignIn` e passando o ClaimsIdentity como mostrado acima. Esse código será entre o usuário e gerar um cookie também. Esta chamada é análoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usado pelo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
2. Em **Solution Explorer**, clique o projeto, clique **adicionar**e, em seguida, **formulário da Web**. Nomeie o formulário da web **logon**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Substitua o conteúdo do *Login.aspx* arquivo com o código a seguir:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Substitua o conteúdo do *Login.aspx.cs* arquivo com o seguinte:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - O `Page_Load` agora verifica o status do usuário atual e executará uma ação com base em seu `Context.User.Identity.IsAuthenticated` status.  
    >     **Exibir o log em nome de usuário** : O Microsoft ASP.NET Identity Framework adicionou os métodos de extensão em [IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) que permite que você obtenha o `UserName` e `UserId` para o Usuário conectado. Esses métodos de extensão são definidos no `Microsoft.AspNet.Identity.Core` assembly. Esses métodos de extensão são a substituição de [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Método de entrada:   
    >     `This` método substitui anterior `CreateUser_Click` método neste exemplo e agora conecta o usuário depois de criar com êxito o usuário.   
    >  O Microsoft OWIN Framework adicionou os métodos de extensão em `System.Web.HttpContext` que permite que você obtenha uma referência a um `IOwinContext`. Esses métodos de extensão são definidos no `Microsoft.Owin.Host.SystemWeb` assembly. O `OwinContext` classe expõe um `IAuthenticationManager` propriedade que representa a funcionalidade de middleware de autenticação disponível na solicitação atual.  
    >  Você pode entrar o usuário usando o `AuthenticationManager` do OWIN e chamar `SignIn` e passando o `ClaimsIdentity` como mostrado acima.   
    >  Como identidade do ASP.NET e autenticação de Cookie OWIN são sistema baseado em declarações, o framework requer o aplicativo para gerar um `ClaimsIdentity` para o usuário.   
    >  O `ClaimsIdentity` tem informações sobre todas as declarações para o usuário, como as funções que o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio  
    >  Esse código será entre o usuário e gerar um cookie também. Esta chamada é análoga a [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usado pelo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
    > - `SignOut` método:   
    >  Obtém uma referência para o `AuthenticationManager` do OWIN e chamadas `SignOut`. Isso equivale a [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) método usado pelo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
5. Pressione **Ctrl + F5** para compilar e executar o aplicativo web. Digite um novo nome de usuário e senha e, em seguida, clique em **registrar**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Observação: neste ponto, o novo usuário é criado e conectado.
6. Clique em **logoff** botão. Você será redirecionado para o formulário de login.
7. Insira um nome de usuário inválido ou a senha e clique **login** botão.   
   O `UserManager.Find` método retornará null e a mensagem de erro: " *nome inválido de usuário ou senha* " será exibida.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
