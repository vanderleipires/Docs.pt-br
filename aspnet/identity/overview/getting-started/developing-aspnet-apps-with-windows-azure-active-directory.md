---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Desenvolvendo aplicativos ASP.NET com o Active Directory do Azure | Microsoft Docs
author: Rick-Anderson
description: Ferramentas do Microsoft ASP.NET para o Azure Active Directory torna simples para habilitar a autenticação para aplicativos web hospedados no Azure. Você pode usar o Azure Autenti...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 7f0e569458c9a294cc281b86e731c2fda48768be
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912861"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Desenvolvendo aplicativos ASP.NET com o Azure Active Directory
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

Ferramentas do Microsoft ASP.NET para simplifica a habilitar a autenticação para aplicativos web hospedados no Azure Active Directory [Azure](https://www.windowsazure.com/home/features/web-sites/). Você pode usar a autenticação do Azure para autenticar usuários do Office 365 da sua organização, sincronizadas do Active Directory no local de contas corporativas ou os usuários criados no seu próprio domínio personalizado do Azure Active Directory. Habilitar a autenticação do Windows Azure configura seu aplicativo para autenticar usuários usando uma única [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) locatário.

Este tutorial mostra como criar um aplicativo ASP.NET que está configurado para logon com o [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Você também aprenderá como chamar a API do Graph para obter informações sobre o usuário conectado no momento e como implantar o aplicativo no Azure.

## <a name="prerequisites"></a>Pré-requisitos

1. [Visual Studio Express 2013 para Web](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express) ou [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -atualização 3 ou superior é necessária.
3. Uma conta do Azure. [Clique aqui](https://azure.microsoft.com/pricing/free-trial/) para uma avaliação gratuita se você ainda não tiver uma conta.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Adicionar um Administrador Global para seu Active Directory

1. Entrar para o [Portal de gerenciamento do Azure](https://manage.windowsazure.com/).
2. Todas as contas do Azure contêm uma **diretório padrão** – clique nele e clique no **usuários** guia na parte superior da página (consulte a imagem abaixo).
3. Clique em Adicionar usuário.
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Criar um novo usuário com o **Administrador Global** função. Clique em **os usuários** do menu superior e, em seguida, clique o **adicionar usuário** botão na barra de comandos.
5. No **adicionar usuário** caixa de diálogo, insira um nome para o novo usuário e, em seguida, clique na seta à direita.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Insira o nome de usuário e defina a função como **Administrador Global**. Os administradores globais exigem um endereço de email alternativo para fins de recuperação de senha. Depois que tiver terminado, clique na seta à direita.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Na próxima página da caixa de diálogo, clique em **criar**. Uma senha temporária será criada para o novo usuário e exibida na caixa de diálogo.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)

   Salvar a senha, será necessário alterar a senha após o primeiro logon. A imagem a seguir mostra a nova conta de administrador. Você deve usar o Azure Active Directory façam logon no seu aplicativo, não a conta da Microsoft também mostrado nesta página.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Criar um aplicativo ASP.NET

Use as etapas a seguir [Visual Studio Express 2013 para Web](https://www.microsoft.com/download/details.aspx?id=40747)e requer [Visual Studio 2013 atualização 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. No Visual Studio, clique em **arquivo** e, em seguida **novo projeto**. Sobre o **novo projeto** caixa de diálogo, selecione Visual c# Web do projeto no menu à esquerda e clique em **Okey**. Você também deseja desmarcar as **adicionar Application Insights ao projeto** se não quiser a funcionalidade para seu aplicativo.
2. No **novo projeto ASP.NET** caixa de diálogo, selecione **MVC**e, em seguida, clique em **alterar autenticação**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Sobre o **alterar autenticação** caixa de diálogo, selecione **contas organizacionais**. Essas opções podem ser usadas para registrar automaticamente seu aplicativo com o Azure AD, bem como configurar automaticamente seu aplicativo para integrar com o Azure AD. Você não precisa usar o **alterar autenticação** caixa de diálogo para registrar e configurar seu aplicativo, mas ele torna muito mais fácil. Se você estiver usando Visual Studio 2012, por exemplo, você pode manualmente registrar o aplicativo no Portal de gerenciamento do Azure e atualizar sua configuração para integrar com o Azure AD.
   Nos menus suspensos, selecione **nuvem - organização única** e **logon único, ler dados do diretório**. Digite o domínio para seu diretório do AD do Azure, por exemplo (nas imagens abaixo) *aricka0yahoo.onmicrosoft.com*e, em seguida, clique em **Okey**. Você pode obter o nome de domínio a partir da guia domínios para o diretório padrão no portal do azure (consulte a próxima imagem para baixo).

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)

   A imagem a seguir mostra o nome de domínio do portal do Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)

    > [!NOTE]
    > Opcionalmente, você pode configurar o URI da ID do aplicativo que será registrado no AD do Azure clicando **mais opções**. O URI da ID do aplicativo é o identificador exclusivo para um aplicativo, que é registrado no Azure AD e usado pelo aplicativo para se identificar ao se comunicar com o Azure AD. Para obter mais informações sobre o URI da ID do aplicativo e outras propriedades de aplicativos registrados, consulte [neste tópico](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Ao clicar na caixa de seleção abaixo do campo de URI da ID do aplicativo, também é possível substituir um registro existente no AD do Azure que usa o mesmo URI de ID do aplicativo.
4. Depois de clicar em **Okey**, aparecerá uma caixa de diálogo entrar e você precisará entrar usando uma conta de Administrador Global (não a conta Microsoft associada à sua assinatura). Se você criou uma nova conta de administrador, você será solicitado a alterar a senha e, em seguida, entrar novamente usando a nova senha.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Depois de autenticado com êxito, o **novo projeto ASP.NET** caixa de diálogo mostrará sua opção de autenticação (**organizacional** ) e o diretório onde o novo aplicativo será registrado (*aricka0yahoo.onmicrosoft.com* na imagem abaixo). Abaixo essas informações, selecione a caixa de seleção rotulada **hospedar na nuvem**. Se essa caixa de seleção estiver marcada, o projeto será provisionado como um aplicativo web do Azure e será habilitado para publicação fácil mais tarde. Clique em **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. O **configurar site do Azure** caixa de diálogo será exibida, usando um nome de site gerado automaticamente e a região. Observe também a conta que você está conectado no momento na caixa de diálogo. Você deseja certificar-se de que essa conta é aquela que sua assinatura do Azure está anexada, normalmente uma conta da Microsoft.

    > [!NOTE]
    > Este projeto requer um banco de dados. Você precisa selecionar um dos seus bancos de dados existentes ou crie um novo. Um banco de dados é necessário porque o projeto já usa um arquivo de banco de dados local para armazenar uma pequena quantidade de dados de configuração de autenticação. Quando você implanta o aplicativo em um site do Azure, esse banco de dados não é empacotado com a implantação, portanto, você precisa escolher um que seja acessível na nuvem. Clique em **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. O projeto será criado e suas opções de autenticação e opções de aplicativo da web serão configuradas automaticamente com o projeto. Quando esse processo for concluído, execute o projeto localmente pressionando **^ F5**. Você será solicitado a entrar usando sua conta organizacional. Forneça o nome de usuário e a senha para a conta que você criou anteriormente e clique em **entrar**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Após a entrada bem-sucedida, o site do ASP.NET mostrará que você foi autenticado, exibindo o nome de usuário no canto superior direito da página.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)

   Se você receber o erro: valor não pode ser nulo ou vazio. Nome do parâmetro: linkText ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)

   Consulte a [depurar](#dbg) seção no final do tutorial.

## <a name="basics-of-the-graph-api"></a>Noções básicas sobre a API do Graph

[A API do Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) é a interface programática usada para executar o CRUD e outras operações em objetos no diretório do AD do Azure. Se você selecionar uma opção de conta organizacional para autenticação ao criar um novo projeto no Visual Studio 2013, seu aplicativo já será configurado para chamar a API do Graph. Esta seção mostra rapidamente como funciona a API do Graph.

1. Em seu aplicativo em execução, clique no nome do usuário conectado na parte superior direita da página. Você será levado para a página de perfil do usuário, que é uma ação no controlador Home. Você observará que a tabela contém informações de usuário sobre a conta de administrador que você criou anteriormente. Essas informações são armazenadas em seu diretório e a API do Graph é chamada para recuperar essas informações quando a página for carregada.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Volte para o Visual Studio e expanda o **controladores** pasta e, em seguida, abra o **HomeController.cs** arquivo. Você verá uma **UserProfile()** ação que contém o código para recuperar um token e, em seguida, chame a API do Graph. Esse código é duplicado abaixo:

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Para chamar a API do Graph, primeiro você precisa recuperar um token. Quando o token é recuperado, seu valor de cadeia de caracteres deve ser anexado no cabeçalho de autorização para todas as solicitações subsequentes para a API do Graph. A maioria do código acima trata os detalhes de autenticar no Azure AD para obter um token, usando o token para fazer uma chamada à API do Graph e, em seguida, transformar a resposta para que ele pode ser apresentado no modo de exibição.

    A parte mais relevante para a discussão é a seguinte linha realçada: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Essa linha representa o nome do usuário, o que foi desserializado da resposta JSON e é apresentado no modo de exibição.

    Você pode chamar a API do Graph usando HttpClient e manipular os dados brutos por conta própria, mas uma maneira mais fácil é usar o [grafo de biblioteca de cliente que está disponível via NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). A biblioteca de cliente lida com as solicitações HTTP brutas e a transformação dos dados retornados para você e torna muito mais fácil trabalhar com a API do Graph em um ambiente do .NET. Consulte os exemplos de código de API do Graph relacionados no [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Implantar o aplicativo no Azure

As etapas a seguir mostram como implantar o aplicativo no Azure. Nas etapas anteriores, você conectou seu projeto novo com um aplicativo web no Azure, para que ele está pronto para ser publicado em apenas algumas etapas.

1. No Visual Studio, clique com botão direito no projeto e selecione **publicar**. O **publicar na Web** caixa de diálogo será exibida com cada configuração já foi configurada. Clique no **próxima** botão para ir para o **configurações** página. Você pode ser solicitado a autenticar; Verifique se que você autentica usando sua conta de assinatura do Azure (normalmente uma conta da Microsoft) e não a conta institucional que você criou anteriormente.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Verifique as **habilitar autenticação organizacional** opção. No **domínio** , insira o domínio para seu diretório. Dos **nível de acesso** lista suspensa, selecione **logon único, ler dados do diretório**. Você observará que o banco de dados anterior, você usou já está preenchido na **bancos de dados** seção. Clique em **Publicar**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio começará a implantar seu site e, em seguida, uma nova janela do navegador será exibida. Você será solicitado a autenticar novamente ao seu diretório. Depois de autenticado, você será redirecionado para seu site recém-publicado no Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depuração do aplicativo

Se você receber o seguinte erro: valor não pode ser nulo ou vazio. Nome do parâmetro: linkText

![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)


Substitua o código na *Views\Shared\\loginpartial* arquivo com o seguinte:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Depois de executar o aplicativo, se o usuário conectado mostra "Usuário nulo", saia e entre novamente com a conta do Active Directory que você criou anteriormente.

Um excelente tutorial a seguir é de Rick Rainey [Deep Dive: sites do Azure e autenticação organizacional usando o Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Mais informações

- [Deep Dive: Os sites do Azure e autenticação organizacional usando o Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Visão geral da API do Graph do AD do Azure](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Cenários de autenticação do AD do Azure](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Exemplos de código do AD do Azure no GitHub](https://github.com/AzureADSamples)
