---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Adicionar projeto de formulários do ASP.NET Identity a uma Web vazia ou existente | Microsoft Docs
author: raquelsa
description: Este tutorial mostra como adicionar a identidade do ASP.NET (o novo sistema de associação do ASP.NET) para um aplicativo ASP.NET. Quando você cria um novo Web Forms ou MVC...
ms.author: aspnetcontent
ms.date: 10/23/2013
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 1e7508fc2431f4e1e3c4509fbe705daf42686e8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832656"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Projeto de formulários a identidade do ASP.NET adicionando a uma Web vazia ou existente
====================
por [Raquel Soares De Almeida](https://github.com/raquelsa)

> Este tutorial mostra como adicionar [ASP.NET Identity](introduction-to-aspnet-identity.md) (o novo sistema de associação do ASP.NET) para um aplicativo ASP.NET.
> 
> Quando você cria um novo projeto de Web Forms ou MVC no Visual Studio 2013 RTM com contas individuais, Visual Studio instalará todos os pacotes necessários e adicionar todas as classes necessárias para você. Este tutorial irá ilustrar as etapas para adicionar suporte à identidade do ASP.NET ao seu projeto de formulários da Web existente ou um novo projeto vazio. Vou descrever todos os pacotes do NuGet que necessários para instalar e as classes que você precisa adicionar. Entrarei em formulários da Web de exemplo para registrar novos usuários e registro em log enquanto destaca todas as APIs de ponto de entrada principal para autenticação e gerenciamento de usuário. Este exemplo usará a implementação do padrão de identidade do ASP.NET para o armazenamento de dados SQL que é criado no Entity Framework. Neste tutorial, usaremos o LocalDB para o banco de dados SQL.
> 
> Este tutorial foi escrito por Raquel Soares De Almeida e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Introdução ao ASP.NET Identity

1. Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Clique em **novo projeto** desde o início página, ou você pode usar o menu e selecione **arquivo**e então **novo projeto**.
3. Selecione **Visual c# i** n o painel esquerdo, em seguida, **Web** e, em seguida, selecione **aplicativo Web ASP.NET**. Nomeie o projeto "WebFormsIdentity" e, em seguida, clique em **Okey**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Observe que o **alterar autenticação** botão está desabilitado e não há suporte de autenticação é fornecida neste modelo. Os modelos de Web Forms, MVC e API da Web permitem que você selecionar a abordagem de autenticação. Para obter mais informações, consulte [visão geral da autenticação](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Adicionando pacotes de identidade para seu aplicativo

No Gerenciador de soluções, clique em seu projeto e selecione **gerenciar pacotes NuGet**. No diálogo de caixa de texto de pesquisa, digite "*Identity.E*". Clique em instalar para este pacote.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Observe que este pacote irá instalar os pacotes de dependência: EntityFramework e identidade do Microsoft ASP.NET Core.

## <a name="adding-web-forms-to-register-users"></a>A adição de formulários da Web para registrar usuários

1. Na **Gerenciador de soluções**, clique em seu projeto e clique em **Add**e então **Web Form**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. No **especificar nome para o Item** caixa de diálogo, o nome do novo formulário da web **registrar**e, em seguida, clique em **Okey**
3. Substitua a marcação em gerado *Register* arquivo pelo código a seguir. As alterações de código são realçadas.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Isso é apenas uma versão simplificada do *Register* arquivo que é criado quando você cria um novo projeto de Web Forms do ASP.NET. A marcação acima adiciona campos de formulário e um botão para registrar um novo usuário.
4. Abra o *Register.aspx.cs* de arquivo e substitua o conteúdo do arquivo pelo código a seguir:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. O código acima é uma versão simplificada do *Register.aspx.cs* arquivo que é criado quando você cria um novo projeto de Web Forms do ASP.NET.
    > 2. O *IdentityUser* classe é a implementação EntityFramework padrão de *IUser* interface. *IUser* interface é a interface mínima para um usuário no ASP.NET Core de identidade.
    > 3. O *UserStore* classe é a implementação do EntityFramework padrão de um repositório de usuário. Essa classe implementa as interfaces mínimas da identidade do ASP.NET Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* e *IUserRoleStore* .
    > 4. O *UserManager* classe expõe usuário relacionadas a APIs que salvará automaticamente as alterações para o *UserStore*.
    > 5. O *IdentityResult* classe representa o resultado de uma operação de identidade.
5. Na **Gerenciador de soluções**, clique em seu projeto e clique em **Add**, **adicionar pasta ASP.NET** e, em seguida, **aplicativo\_dados**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Abra o *Web. config* arquivo e adicione uma entrada de cadeia de caracteres de conexão para o banco de dados que usaremos para armazenar informações do usuário. O banco de dados será criado no tempo de execução por EntityFramework para as entidades de identidade. A cadeia de caracteres de conexão é semelhante a um criado quando você cria um novo projeto de formulários da Web. O código realçado mostra a marcação, que você deve adicionar:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Para o Visual Studio 2015 ou superior, substitua `(localdb)\v11.0` com `(localdb)\MSSQLLocalDB` em sua cadeia de conexão.
    
7. Clique com botão direito no arquivo *Register* em seu projeto e selecione **definir como página inicial**. Pressione Ctrl + F5 para compilar e executar o aplicativo web. Insira um novo nome de usuário e senha e, em seguida, clique em **registrar**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > Identidade do ASP.NET tem suporte para validação e neste exemplo, você pode verificar o comportamento padrão no usuário e senha validadores que vêm de pacote do núcleo de identidade. O validador padrão para o usuário (`UserValidator`) tem uma propriedade `AllowOnlyAlphanumericUserNames` que tem o valor padrão definido como `true`. O validador de padrão para a senha (`MinimumLengthValidator`) garante que essa senha tem pelo menos 6 caracteres. Esses validadores são propriedades no `UserManager` que pode ser substituído se você quiser ter validação personalizada,

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Verificando o banco de dados de identidade do LocalDb e tabelas geradas pelo Entity Framework

1. No **modo de exibição** menu, clique em **Gerenciador de servidores**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Expandir **DefaultConnection (WebFormsIdentity)**, expanda **tabelas**, clique com botão direito **AspNetUsers** e clique em **Mostrar dados da tabela**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Configurando o aplicativo para autenticação OWIN

Neste ponto, apenas adicionamos suporte para a criação de usuários. Agora, vamos demonstrar como é possível adicionar a autenticação para fazer logon de um usuário. Identidade do ASP.NET usa a autenticação do Microsoft OWIN middleware para autenticação de formulários. A autenticação de Cookie do OWIN é um cookie e declarações de mecanismo de autenticação baseada em que pode ser usado por qualquer estrutura hospedada no [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) ou IIS. Com esse modelo, os mesmos pacotes de autenticação podem ser usados em várias estruturas, incluindo o ASP.NET MVC e Web Forms. Para obter mais informações sobre o projeto Katana e como executar o middleware em uma consulte independente de host [Introdução ao projeto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Instalar os pacotes de autenticação ao seu aplicativo

1. No Gerenciador de soluções, clique em seu projeto e selecione **gerenciar pacotes NuGet**. No diálogo de caixa de texto de pesquisa, digite "*Identity.Owin*". Clique em instalar para este pacote.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Procure o pacote ***systemweb*** e instalá-lo.   

    > [!NOTE]
    > O **Microsoft.Aspnet.Identity.Owin** pacote contém um conjunto de classes de extensão do OWIN para gerenciar e configurar o middleware de autenticação OWIN a ser consumida por pacotes do ASP.NET Core de identidade.  
    > O **systemweb** pacote contém um servidor OWIN que permite que aplicativos baseados no OWIN sejam executados no IIS usando o pipeline de solicitação do ASP.NET. Para obter mais informações, consulte [pipeline integrado do Middleware do OWIN no IIS](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Adicionando Classes de configuração de autenticação e de inicialização OWIN

1. Na **Gerenciador de soluções**, clique em seu projeto, clique em **Add**e então **Add New Item**. No diálogo de caixa de texto de pesquisa, digite "*owin*". Nomeie a classe "*inicialização*" e clique em **Add**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. No arquivo Startup.cs, adicione o código realçado abaixo para configurar a autenticação de cookie do OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Essa classe contém o `OwinStartup` atributo para especificar a classe de inicialização do OWIN. Todos os aplicativos OWIN tem uma classe de inicialização na qual você pode especificar componentes para o pipeline do aplicativo. Ver [detecção de classe de inicialização OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) para obter mais informações sobre esse modelo.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>A adição de formulários da Web para registro e login de usuários

1. Abra o *Register.cs* arquivo e adicione o seguinte código que registrará em log o usuário quando o registro terá êxito. As alterações são realçadas abaixo.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Como o ASP.NET Identity e autenticação de Cookie OWIN são sistema baseada em declarações, o framework exige que o desenvolvedor de aplicativo gerar uma [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) para o usuário. ClaimsIdentity tem informações sobre todas as declarações do usuário, como as funções que o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio.
    > - Você pode conectar o usuário usando o AuthenticationManager do OWIN e chamar `SignIn` e passando o ClaimsIdentity conforme mostrado acima. Esse código será a entrada do usuário e gerar um cookie também. Essa chamada é análoga à [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usado pelas [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
2. Na **Gerenciador de soluções**, seu projeto, clique com o botão direito **Add**e então **Web Form**. Nomeie o formulário da web **Login**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Substitua o conteúdo do *login. aspx* arquivo pelo código a seguir:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Substitua o conteúdo do *Login.aspx.cs* arquivo com o seguinte:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - O `Page_Load` agora verifica o status do usuário atual e entra em ação com base em seu `Context.User.Identity.IsAuthenticated` status.  
    >     **Exibir conectado em nome de usuário** : A estrutura de identidade do Microsoft ASP.NET tenha adicionado os métodos de extensão no [IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) que permite que você obtenha o `UserName` e `UserId` para o Usuário conectado. Esses métodos de extensão são definidos na `Microsoft.AspNet.Identity.Core` assembly. Esses métodos de extensão são a substituição de [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Método SignIn:   
    >     `This` método substitui o anterior `CreateUser_Click` método neste exemplo e agora os sinais do usuário depois de criar com êxito o usuário.   
    >  A estrutura do Microsoft OWIN tenha adicionado os métodos de extensão em `System.Web.HttpContext` que permite que você obtenha uma referência a um `IOwinContext`. Esses métodos de extensão são definidos no `Microsoft.Owin.Host.SystemWeb` assembly. O `OwinContext` classe expõe um `IAuthenticationManager` propriedade que representa a funcionalidade de middleware de autenticação disponível na solicitação atual.  
    >  Você pode conectar o usuário usando o `AuthenticationManager` do OWIN e chamar `SignIn` e passando o `ClaimsIdentity` conforme mostrado acima.   
    >  Como o ASP.NET Identity e autenticação de Cookie OWIN são sistema baseado em declarações, o framework requer que o aplicativo para gerar um `ClaimsIdentity` para o usuário.   
    >  O `ClaimsIdentity` tem informações sobre todas as declarações para o usuário, como as funções que o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio  
    >  Esse código será a entrada do usuário e gerar um cookie também. Essa chamada é análoga à [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usado pelas [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
    > - `SignOut` método:   
    >  Obtém uma referência para o `AuthenticationManager` do OWIN e chamadas `SignOut`. Isso é análogo à [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) método usado pelas [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo.
5. Pressione **Ctrl + F5** para compilar e executar o aplicativo web. Insira um novo nome de usuário e senha e, em seguida, clique em **registrar**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Observação: neste ponto, o novo usuário é criado e conectado.
6. Clique em **fazer logoff** botão. Você será redirecionado para o Log no formulário.
7. Insira um nome de usuário inválido ou a senha e clique **faça logon no** botão.   
   O `UserManager.Find` método retornará null e a mensagem de erro: " *nome de usuário inválido ou a senha* " será exibida.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
