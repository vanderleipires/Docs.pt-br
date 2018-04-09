---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Desenvolvimento de aplicativos ASP.NET com o Active Directory do Azure | Microsoft Docs
author: Rick-Anderson
description: Ferramentas do Microsoft ASP.NET para Active Directory do Azure simplifica a habilitar a autenticação para aplicativos web hospedados no Azure. Você pode usar o Azure Autenti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 44bf29e099583bf9d49f2715d3ff4f748728ad8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Desenvolvimento de aplicativos ASP.NET com o Active Directory do Azure
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET ferramentas para Active Directory do Azure torna simples para habilitar a autenticação para aplicativos web hospedados no [Azure](https://www.windowsazure.com/home/features/web-sites/). Você pode usar a autenticação do Azure para autenticar usuários do Office 365 da sua organização, as contas corporativas sincronizadas do Active Directory local ou os usuários criados no seu próprio domínio personalizado do Active Directory do Azure. Habilitar a autenticação do Windows Azure configura seu aplicativo para autenticar usuários usando uma única [Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/) locatário.
> 
>  Este tutorial foi escrito de Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)


Este tutorial mostra como criar um aplicativo ASP.NET que está configurado para logon com [Active Directory do Azure](https://msdn.microsoft.com/library/azure/mt168838.aspx) (AD do Azure). Você também aprenderá como chamar a API do Graph para obter informações sobre o usuário conectado no momento e como implantar o aplicativo no Azure.

## <a name="prerequisites"></a>Pré-requisitos

1. [O Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) ou [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 ou posterior é necessário.
3. Uma conta do Azure. [Clique aqui](https://azure.microsoft.com/pricing/free-trial/) para uma avaliação gratuita, se você ainda não tiver uma conta.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Adicionar um Administrador Global no Active Directory

1. Entrar para o [Portal de gerenciamento do Azure](https://manage.windowsazure.com/).
2. Todas as contas do Azure contêm um **diretório padrão** - clique nela, clique o **usuários** guia na parte superior da página (consulte a imagem abaixo).
3. Clique em Adicionar usuário.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Criar um novo usuário com o **Administrador Global** função. Clique em **usuários** no menu superior e, em seguida, clique o **adicionar usuário** botão na barra de comandos.
5. No **adicionar usuário** caixa de diálogo, digite um nome para o novo usuário e, em seguida, clique na seta à direita.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Insira o nome de usuário e defina a função para **Administrador Global**. Os administradores globais exigem um endereço de email alternativo para fins de recuperação de senha. Após terminar, clique na seta à direita.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Na próxima página da caixa de diálogo, clique em **criar**. Uma senha temporária será criada para o novo usuário e exibida na caixa de diálogo.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   Salvar a senha, será necessário alterar a senha após o primeiro logon. A imagem a seguir mostra a nova conta de administrador. Você deve usar o Active Directory do Azure para fazer logon em seu aplicativo, não a conta da Microsoft também mostrado nesta página.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Criar um aplicativo ASP.NET

Use as etapas a seguir [Visual Studio Express 2013 para Web](https://www.microsoft.com/download/details.aspx?id=40747)e requer [Visual Studio 2013 atualização 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. No Visual Studio, clique em **arquivo** e **novo projeto**. Sobre o **novo projeto** caixa de diálogo, selecione Visual C# Web do projeto no menu à esquerda e clique em **Okey**. Você também deseja desmarcar o **adicionar Application Insights ao projeto** se você não quiser a funcionalidade para seu aplicativo.
2. No **novo projeto ASP.NET** caixa de diálogo, selecione **MVC**e, em seguida, clique em **alterar autenticação**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Sobre o **alterar autenticação** caixa de diálogo, selecione **contas organizacionais**. Essas opções podem ser usadas para registrar automaticamente o aplicativo no AD do Azure, bem como configurar automaticamente seu aplicativo para integrar com o AD do Azure. Você não precisa usar o **alterar autenticação** caixa de diálogo para registrar e configurar seu aplicativo, mas ele torna muito mais fácil. Se você estiver usando o Visual Studio 2012, por exemplo, você pode manualmente registrar o aplicativo no Portal de gerenciamento do Azure e atualize sua configuração para integrar com o Azure AD.  
   No menu suspenso, selecione **nuvem - única organização** e **logon único, ler dados do diretório**. Digite o domínio do seu diretório do AD do Azure, por exemplo (nas imagens abaixo) *aricka0yahoo.onmicrosoft.com*e, em seguida, clique em **Okey**. Você pode obter o nome de domínio da guia domínios para o diretório padrão no portal do azure (consulte a próxima imagem para baixo).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   A imagem a seguir mostra o nome de domínio do portal do Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Opcionalmente, você pode configurar o URI de ID do aplicativo que será registrado no AD do Azure clicando em **mais opções**. O URI de ID do aplicativo é o identificador exclusivo para um aplicativo, que é registrado no Azure AD e usado pelo aplicativo para se identificar ao se comunicar com o Azure AD. Para obter mais informações sobre o URI de ID do aplicativo e outras propriedades de aplicativos registrados, consulte [neste tópico](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Ao clicar na caixa de seleção abaixo do campo de URI de ID de aplicativo, também é possível substituir um registro existente no AD do Azure que usa o mesmo URI de ID do aplicativo.
4. Depois de clicar em **Okey**, aparecerá uma caixa de diálogo de entrada, e você precisará entrar usando uma conta de Administrador Global (não a conta da Microsoft associada à sua assinatura). Se você criou uma nova conta de administrador anteriormente, você precisará alterar a senha e entrar novamente usando a nova senha.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Depois que você foi autenticado com êxito, o **novo projeto ASP.NET** caixa de diálogo mostrará a opção de autenticação (**organizacional** ) e o diretório onde o novo aplicativo será registrado (*aricka0yahoo.onmicrosoft.com* na imagem abaixo). Seguir essas informações, selecione a caixa de seleção **Host na nuvem**. Se essa caixa de seleção estiver marcada, o projeto será configurado como um aplicativo web do Azure e será habilitado para publicação fácil mais tarde. Clique em **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. O **configurar site do Azure** caixa de diálogo aparecerá, usando um nome de site gerado automaticamente e região. Observe também a conta que você está conectado no momento na caixa de diálogo. Você deseja certificar-se de que essa conta é a sua assinatura do Azure está anexada, normalmente uma conta da Microsoft.

    > [!NOTE]
    > Este projeto requer um banco de dados. Você precisa selecionar um dos bancos de dados existentes ou criar um novo. Um banco de dados é necessário porque o projeto já utiliza um arquivo de banco de dados local para armazenar uma pequena quantidade de dados de configuração de autenticação. Quando você implanta o aplicativo em um site do Azure, este banco de dados não é empacotado com a implantação, portanto você precisa escolher um que seja acessível na nuvem. Clique em **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. O projeto será criado e suas opções de autenticação e opções de aplicativo da web serão automaticamente configuradas com o projeto. Depois que esse processo for concluído, execute o projeto localmente pressionando **^ F5**. Você precisará entrar usando sua conta organizacional. Forneça o nome de usuário e a senha para a conta que você criou anteriormente e clique em **entrar**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Após a conexão bem-sucedida, o site ASP.NET mostrará que você foi autenticado, exibindo o nome de usuário no canto superior direito da página.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Se você obtiver o erro:  
   Valor não pode ser nulo ou vazio. Nome do parâmetro: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   Consulte o [depurar](#dbg) seção no final do tutorial.

## <a name="basics-of-the-graph-api"></a>Noções básicas sobre a API do Graph

[A API do Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) é a interface de programação usada para realizar CRUD e outras operações em objetos em seu diretório do AD do Azure. Se você selecionar uma opção de conta organizacional para autenticação ao criar um novo projeto no Visual Studio 2013, o aplicativo já será configurado para chamar a API do Graph. Esta seção brevemente mostra como funciona a API do Graph.

1. Em seu aplicativo em execução, clique no nome do usuário conectado na parte superior direita da página. Isso levará para a página de perfil de usuário, que é uma ação no controlador de início. Você observará que a tabela contém informações de usuário sobre a conta de administrador que você criou anteriormente. Essas informações são armazenadas em seu diretório, e a API do Graph é chamada para recuperar essas informações quando a página for carregada.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Volte para o Visual Studio e expanda o **controladores** pasta e, em seguida, abra o **HomeController** arquivo. Você verá um **UserProfile()** que contém o código para recuperar um token e, em seguida, chame a API do Graph. Esse código é duplicado abaixo: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Para chamar a API do Graph, primeiro você precisa recuperar um token. Quando o token é recuperado, seu valor de cadeia de caracteres deve ser anexado no cabeçalho de autorização para todas as solicitações subsequentes para a API do Graph. A maioria do código acima trata os detalhes de autenticação no AD do Azure para obter um token, usando o token para fazer uma chamada à API do Graph e, em seguida, transformar a resposta para que ele pode ser apresentado no modo de exibição.

    A parte mais relevante para discussão é a seguinte linha realçada: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Essa linha representa o nome do usuário que tiver sido desserializado da resposta JSON e é apresentado no modo de exibição.

    Você pode chamar a API do Graph usando HttpClient e manipular os dados brutos por conta própria, mas uma maneira mais fácil é usar o [gráfico biblioteca de cliente que está disponível por meio do NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). A biblioteca de cliente trata as solicitações HTTP brutas e a transformação dos dados retornados para você e torna muito mais fácil trabalhar com a API do Graph em um ambiente de .NET. Consulte os exemplos de código relacionados do API do Graph em [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Implantar o aplicativo no Azure

As etapas a seguir mostram como implantar o aplicativo no Azure. Nas etapas anteriores, você conectado seu novo projeto com um aplicativo web no Azure, para que esteja pronto para ser publicado em apenas algumas etapas.

1. No Visual Studio, clique com botão direito no projeto e selecione **publicar**. O **Publicar Web** caixa de diálogo aparecerá com cada configuração já configurada. Clique no **próximo** botão para ir para o **configurações** página. Você pode ser solicitado a autenticar; Verifique se que você se autenticar usando sua conta de assinatura do Azure (normalmente uma conta da Microsoft) e não a conta institucional que você criou anteriormente.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Verifique o **habilitar a autenticação organizacional** opção. No **domínio** campo, digite o domínio do seu diretório. Do **nível de acesso** lista suspensa, selecione **logon único, ler dados do diretório**. Você observará que o banco de dados anterior usado já está preenchido no **bancos de dados** seção. Clique em **Publicar**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio iniciará a implantação de seu site, e, em seguida, uma nova janela do navegador será exibida. Você precisará autenticar novamente o seu diretório. Depois de autenticado, você será redirecionado ao seu site recém-publicado no Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depurar o aplicativo

Se você receber o seguinte erro:   
 Valor não pode ser nulo ou vazio. Nome do parâmetro: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Substitua o código no *exibições \ compartilhadas\\loginpartial. cshtml* arquivo com o seguinte:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Depois de executar o aplicativo, se o usuário conectado mostra "Usuário nulo", saia e entre novamente com a conta do Active Directory que você criou anteriormente.

Um excelente tutorial a seguir é de Rick Rainey [mergulho profundo: sites do Azure e a autenticação organizacional usando o Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Mais informações

- [Mergulho profundo: Sites do Azure e a autenticação organizacional usando o Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Visão geral da API do Azure AD Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Cenários de autenticação no AD do Azure](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Exemplos de código do AD do Azure no GitHub](https://github.com/AzureADSamples)
