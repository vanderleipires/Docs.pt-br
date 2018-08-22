---
uid: web-api/overview/security/external-authentication-services
title: Serviços de autenticação externa com a API da Web ASP.NET (c#) | Microsoft Docs
author: rmcmurray
description: Descreve como usar os serviços de autenticação externos na API Web ASP.NET.
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 0b23baac7eca0297e063c682a8ae199f9543d75e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824174"
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>Serviços de autenticação externa com a API da Web ASP.NET (c#)
====================
por [Robert McMurray](https://github.com/rmcmurray)

Visual Studio 2013 e no ASP.NET 4.5.1 expanda as opções de segurança [aplicativos de página única](../../../single-page-application/index.md) (SPA) e [API da Web](../../index.md) serviços para integrar com serviços de autenticação externa, que incluem vários OAuth/OpenID e serviços de autenticação de mídia social: Accounts da Microsoft, Twitter, Facebook e Google.

### <a name="in-this-walkthrough"></a>Neste passo a passo

- [Usando os serviços de autenticação externa](#USING)
- [Criando o aplicativo Web de exemplo](#SAMPLE)
- [Habilitando a autenticação do Facebook](#FACEBOOK)
- [Habilitando a autenticação do Google](#GOOGLE)
- [Habilitando a autenticação da Microsoft](#MICROSOFT)
- [Habilitando a autenticação do Twitter](#TWITTER)
- [Informações adicionais](#MOREINFO)

    - [Combinando os serviços de autenticação externa](#COMBINE)
    - [Configuração do IIS Express para usar um nome de domínio totalmente qualificado](#FQDN)
    - [Como obter as configurações do aplicativo para autenticação da Microsoft](#OBTAIN)
    - [Opcional: Desabilitar o registro do Local](#DISABLE)

### <a name="prerequisites"></a>Pré-requisitos

Para seguir os exemplos neste passo a passo, você precisa ter o seguinte:

- Visual Studio 2013
- Uma conta para pelo menos um dos seguintes serviços de autenticação externa:

    - Uma conta de usuário do Google
    - Uma conta de desenvolvedor com o identificador do aplicativo e a chave secreta para um dos seguintes serviços de autenticação de mídia social:

        - Contas da Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Usando os serviços de autenticação externa

A abundância de serviços de autenticação externos que estão atualmente disponíveis para ajuda de que os desenvolvedores da web para reduzir o desenvolvimento de tempo durante a criação de novos aplicativos web. Usuários da Web normalmente têm várias contas existentes para serviços populares da web e sites de mídia social, portanto, quando um implementa de aplicativo web de serviços de autenticação de um serviço web externo ou um site de mídia social, ele salva o tempo de desenvolvimento que teria sido gasto criando uma implementação de autenticação. Usando um serviço de autenticação externa salva os usuários finais de ter que criar outra conta para seu aplicativo web e também precise se lembrar de outro nome de usuário e senha.

No passado, os desenvolvedores têm enfrentado duas opções: criar sua própria implementação de autenticação ou Saiba como integrar um serviço de autenticação externa em seus aplicativos. Nível mais básico, o diagrama a seguir ilustra um fluxo de solicitação simples para um agente do usuário (navegador da web) que está solicitando informações de um aplicativo web que está configurado para usar um serviço de autenticação externa:

[![](external-authentication-services/_static/image2.png "Clique para expandir a imagem")](external-authentication-services/_static/image1.png)

No diagrama anterior, o agente do usuário (ou navegador da web neste exemplo) faz uma solicitação para um aplicativo web, que redireciona o navegador da web para um serviço de autenticação externa. O agente do usuário envia as credenciais para o serviço de autenticação externa e se o agente do usuário foi autenticado com êxito, o serviço de autenticação externa redirecionará o agente do usuário para o aplicativo web original com alguma forma de token que o agente do usuário enviará ao aplicativo web. O aplicativo web usará o token para verificar se o agente do usuário foi autenticado com êxito pelo serviço de autenticação externa e o aplicativo web pode usar o token para obter mais informações sobre o agente do usuário. Quando o aplicativo terminar processamento de informações do agente do usuário, o aplicativo da web retornará a resposta apropriada para o agente do usuário com base em suas configurações de autorização.

No segundo exemplo, o agente do usuário negocia com o aplicativo web e o servidor de autorização externa e o aplicativo web executa a comunicação adicional com o servidor de autorização externa para recuperar informações adicionais sobre o usuário agente:

[![](external-authentication-services/_static/image4.png "Clique para expandir a imagem")](external-authentication-services/_static/image3.png)

Visual Studio 2013 e no ASP.NET 4.5.1 simplificar a integração com serviços de autenticação externa para os desenvolvedores fornecendo integração interna para os serviços de autenticação a seguir:

- Facebook
- Google
- Accounts (contas do Windows Live ID) da Microsoft
- Twitter

Os exemplos neste passo a passo demonstrará como configurar cada um dos serviços com suporte de autenticação externa usando o novo modelo de aplicativo Web ASP.NET que é fornecido com o Visual Studio 2013.

> [!NOTE]
> Se necessário, você talvez precise adicionar o FQDN para as configurações para seu serviço de autenticação externa. Esse requisito é com base nas restrições de segurança para alguns serviços de autenticação externa que exigem o FQDN nas configurações do aplicativo para coincidir com o FQDN que é usado por seus clientes. (As etapas para isso irá variar muito, para cada serviço de autenticação externa; você precisará consultar a documentação para cada serviço de autenticação externa para ver se isso é necessário e como definir essas configurações.) Se você precisar configurar o IIS Express para usar um FQDN para esse ambiente de teste, consulte o [configuração do IIS Express para usar um nome de domínio totalmente qualificado](#FQDN) seção mais adiante neste passo a passo.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Criando um aplicativo Web de exemplo

As etapas a seguir levarão você pela criação de um aplicativo de exemplo usando o modelo de aplicativo Web ASP.NET, e você usará este aplicativo de exemplo para cada um dos serviços de autenticação externa posteriormente neste passo a passo.

Inicie o Visual Studio 2013 select **novo projeto** na página inicial. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

[![](external-authentication-services/_static/image6.png "Clique para expandir a imagem")](external-authentication-services/_static/image5.png)

Quando o **novo projeto** caixa de diálogo for exibida, selecione **instalado** **modelos** e expanda **Visual c#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **Aplicativo Web ASP.NET**. Insira um nome para seu projeto e clique em **Okey**.

[![](external-authentication-services/_static/image8.png "Clique para expandir a imagem")](external-authentication-services/_static/image7.png)

Quando o **novo projeto ASP.NET** é exibida, selecione o **SPA** modelo e clique em **criar projeto**.

[![](external-authentication-services/_static/image10.png "Clique para expandir a imagem")](external-authentication-services/_static/image9.png)

Espera que o Visual Studio 2013 cria seu projeto.

[![](external-authentication-services/_static/image12.png "Clique para expandir a imagem")](external-authentication-services/_static/image11.png)

Quando o Visual Studio 2013 terminou de criar seu projeto, abra o *Startup.Auth.cs* arquivo está localizado na **App\_iniciar** pasta.

[![](external-authentication-services/_static/image14.png "Clique para expandir a imagem")](external-authentication-services/_static/image13.png)

Quando você cria o projeto, nenhum dos serviços de autenticação externos estão habilitados no *Startup.Auth.cs* file; a seguir ilustra o que seu código pode ser semelhante, com as seções realçadas para onde você habilitaria um serviço de autenticação externa e todas as configurações relevantes para usar a autenticação de Accounts da Microsoft, Twitter, Facebook ou Google com o aplicativo ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Quando você pressiona F5 para compilar e depurar seu aplicativo web, ele exibirá uma tela de logon onde você verá que não há serviços de autenticação externa foram definidos.

[![](external-authentication-services/_static/image16.png "Clique para expandir a imagem")](external-authentication-services/_static/image15.png)

As seções a seguir, você aprenderá como habilitar cada um dos serviços de autenticação externa que são fornecidos com o ASP.NET no Visual Studio 2013.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Habilitando a autenticação do Facebook

Usando o Facebook autenticação exige que você criar uma conta de desenvolvedor do Facebook, e seu projeto exigirá uma ID de aplicativo e a chave secreta do Facebook para funcionar. Para obter informações sobre como criar uma conta de desenvolvedor do Facebook e como obter a ID do aplicativo e a chave secreta, consulte [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Uma vez você obteve a ID do aplicativo e a chave secreta, use as etapas a seguir para habilitar a autenticação do Facebook para seu aplicativo web:

1. Quando seu projeto é aberto no Visual Studio 2013, abra o *Startup.Auth.cs* arquivo:

    [![](external-authentication-services/_static/image18.png "Clique para expandir a imagem")](external-authentication-services/_static/image17.png)
2. Localize a seção realçada de código:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Remover o &quot; // &quot; caracteres não comentar as linhas realçadas de código e, em seguida, adicione a ID do aplicativo e a chave secreta. Depois de adicionar esses parâmetros, você pode recompilar o projeto:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Quando você pressiona F5 para abrir o aplicativo web no navegador da web, você verá que o Facebook foi definido como um serviço de autenticação externa:

    [![](external-authentication-services/_static/image20.png "Clique para expandir a imagem")](external-authentication-services/_static/image19.png)
5. Quando você clica o **Facebook** botão, o navegador será redirecionado para a página de logon do Facebook:

    [![](external-authentication-services/_static/image22.png "Clique para expandir a imagem")](external-authentication-services/_static/image21.png)
6. Depois que você insira suas credenciais do Facebook e clique em **faça logon no**, seu navegador da web será redirecionado de volta para seu aplicativo web, que solicitará a você para o **nome de usuário** que você deseja associar ao seu Conta do Facebook:

    [![](external-authentication-services/_static/image24.png "Clique para expandir a imagem")](external-authentication-services/_static/image23.png)
7. Depois de inserir seu nome de usuário e clicado a **Inscreva-se** botão, seu aplicativo web exibirá o padrão **home page do** para sua conta do Facebook:

    [![](external-authentication-services/_static/image26.png "Clique para expandir a imagem")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Habilitando a autenticação do Google

Google é de longe o mais fácil dos serviços de autenticação externa para habilitar a porque ele não requer uma conta de desenvolvedor, nem requer informações adicionais, como sua ID do aplicativo ou a chave secreta que outros serviços de autenticação externa exigir.

Para habilitar a autenticação do Google para seu aplicativo web, use as seguintes etapas:

1. Quando seu projeto é aberto no Visual Studio 2013, abra o *Startup.Auth.cs* arquivo:

    [![](external-authentication-services/_static/image28.png "Clique para expandir a imagem")](external-authentication-services/_static/image27.png)
2. Localize a seção realçada de código:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Remover o &quot; // &quot; caracteres não comentar a linha de código realçada e, em seguida, recompile o projeto:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Quando você pressiona F5 para abrir o aplicativo web no navegador da web, você verá que o Google foi definida como um serviço de autenticação externa:

    [![](external-authentication-services/_static/image30.png "Clique para expandir a imagem")](external-authentication-services/_static/image29.png)
5. Quando você clica o **Google** botão, o navegador será redirecionado para a página de logon do Google:

    [![](external-authentication-services/_static/image32.png "Clique para expandir a imagem")](external-authentication-services/_static/image31.png)
6. Depois que você insira suas credenciais do Google e clique em **entrar**, Google solicitará que você verifique se seu aplicativo web tem permissões para acessar sua conta do Google:

    [![](external-authentication-services/_static/image34.png "Clique para expandir a imagem")](external-authentication-services/_static/image33.png)
7. Quando você clica em **Accept**, seu navegador da web será redirecionado de volta para seu aplicativo web, que solicitará a você para o **nome de usuário** que você deseja associar à sua conta do Google:

    [![](external-authentication-services/_static/image36.png "Clique para expandir a imagem")](external-authentication-services/_static/image35.png)
8. Depois de inserir seu nome de usuário e clicado a **Inscreva-se** botão, seu aplicativo web exibirá o padrão **home page do** para sua conta do Google:

    [![](external-authentication-services/_static/image38.png "Clique para expandir a imagem")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Habilitando a autenticação da Microsoft

Autenticação da Microsoft exige que você criar uma conta de desenvolvedor, e ela requer uma ID do cliente e segredo do cliente para funcionar. Para obter informações sobre como criar uma conta de desenvolvedor da Microsoft e obter sua ID do cliente e segredo do cliente, consulte [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Uma vez você obteve a chave do consumidor e o segredo do consumidor, use as etapas a seguir para habilitar a autenticação da Microsoft para seu aplicativo web:

1. Quando seu projeto é aberto no Visual Studio 2013, abra o *Startup.Auth.cs* arquivo:

    [![](external-authentication-services/_static/image40.png "Clique para expandir a imagem")](external-authentication-services/_static/image39.png)
2. Localize a seção realçada de código:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Remover o &quot; // &quot; caracteres não comentar as linhas realçadas de código e, em seguida, adicione sua ID do cliente e segredo do cliente. Depois de adicionar esses parâmetros, você pode recompilar o projeto:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Quando você pressiona F5 para abrir o aplicativo web no navegador da web, você verá que a Microsoft tenha sido definida como um serviço de autenticação externa:

    [![](external-authentication-services/_static/image42.png "Clique para expandir a imagem")](external-authentication-services/_static/image41.png)
5. Quando você clica o **Microsoft** botão, o navegador será redirecionado à página de logon da Microsoft:

    [![](external-authentication-services/_static/image44.png "Clique para expandir a imagem")](external-authentication-services/_static/image43.png)
6. Depois que você insira suas credenciais da Microsoft e clique em **entrar**, você será solicitado a verificar se seu aplicativo web tem permissões para acessar sua conta da Microsoft:

    [![](external-authentication-services/_static/image46.png "Clique para expandir a imagem")](external-authentication-services/_static/image45.png)
7. Quando você clica em **Yes**, seu navegador da web será redirecionado de volta para seu aplicativo web, que solicitará a você para o **nome de usuário** que você deseja associar à sua conta da Microsoft:

    [![](external-authentication-services/_static/image48.png "Clique para expandir a imagem")](external-authentication-services/_static/image47.png)
8. Depois de inserir seu nome de usuário e clicado a **Inscreva-se** botão, seu aplicativo web exibirá o padrão **home page do** para sua conta da Microsoft:

    [![](external-authentication-services/_static/image50.png "Clique para expandir a imagem")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Habilitando a autenticação do Twitter

Autenticação exige que você criar uma conta de desenvolvedor e requer uma chave do consumidor e o segredo do consumidor para que funcionem do Twitter. Para obter informações sobre como criar uma conta de desenvolvedor do Twitter e obter sua chave do consumidor e o segredo do consumidor, consulte [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Uma vez você obteve a chave do consumidor e o segredo do consumidor, use as etapas a seguir para habilitar a autenticação do Twitter para seu aplicativo web:

1. Quando seu projeto é aberto no Visual Studio 2013, abra o *Startup.Auth.cs* arquivo:

    [![](external-authentication-services/_static/image52.png "Clique para expandir a imagem")](external-authentication-services/_static/image51.png)
2. Localize a seção realçada de código:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Remover o &quot; // &quot; caracteres não comentar as linhas realçadas de código e, em seguida, adicione sua chave do consumidor e o segredo do consumidor. Depois de adicionar esses parâmetros, você pode recompilar o projeto:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Quando você pressiona F5 para abrir o aplicativo web no navegador da web, você verá que o Twitter foi definida como um serviço de autenticação externa:

    [![](external-authentication-services/_static/image54.png "Clique para expandir a imagem")](external-authentication-services/_static/image53.png)
5. Quando você clica o **Twitter** botão, o navegador será redirecionado para a página de logon do Twitter:

    [![](external-authentication-services/_static/image56.png "Clique para expandir a imagem")](external-authentication-services/_static/image55.png)
6. Depois que você insira suas credenciais do Twitter e clique em **autorizar aplicativo**, seu navegador da web será redirecionado de volta para seu aplicativo web, que solicitará a você para o **nome de usuário** que você deseja associar ao sua conta do Twitter:

    [![](external-authentication-services/_static/image58.png "Clique para expandir a imagem")](external-authentication-services/_static/image57.png)
7. Depois de inserir seu nome de usuário e clicado a **Inscreva-se** botão, seu aplicativo web exibirá o padrão **home page do** para sua conta do Twitter:

    [![](external-authentication-services/_static/image60.png "Clique para expandir a imagem")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Informações adicionais

Para obter informações adicionais sobre como criar aplicativos que usam OAuth e OpenID, consulte as seguintes URLs:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Combinando os serviços de autenticação externa

Para obter maior flexibilidade, você pode definir vários serviços de autenticação externa ao mesmo tempo – isso permite que sua web usuários do aplicativo usar uma conta de qualquer um dos serviços de autenticação externa habilitadas:

[![](external-authentication-services/_static/image62.png "Clique para expandir a imagem")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Configuração do IIS Express para usar um nome de domínio totalmente qualificado

Alguns provedores de autenticação externa não dão suporte para a testar seu aplicativo por meio de um endereço HTTP como `http://localhost:port/`. Para contornar esse problema, você pode adicionar um mapeamento estático de totalmente o nome de domínio qualificado (FQDN) ao seu arquivo de HOSTS e configurar as opções de projeto no Visual Studio 2013 para usar o FQDN para teste/depuração. Para fazer isso, use as seguintes etapas:

- Adicione um FQDN estático seu arquivo de HOSTS de mapeamento:

  1. Abra um prompt de comando com privilégios elevados no Windows.
  2. Digite o seguinte comando:

      <kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd>
  3. Adicione uma entrada semelhante à seguinte ao arquivo HOSTS:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Salve e feche o arquivo de HOSTS.

- Configure seu projeto do Visual Studio para usar o FQDN:

  1. Quando o projeto está aberto no Visual Studio 2013, clique no **projeto** menu e, em seguida, selecione as propriedades do projeto. Por exemplo, você pode selecionar **WebApplication1 propriedades**.
  2. Selecione o **Web** guia.
  3. Digite o FQDN para o <strong>Url do projeto</strong>. Por exemplo, você digitaria <kbd> <http://www.wingtiptoys.com> </kbd> se isso foi o mapeamento de FQDN que você adicionou ao arquivo HOSTS.

- Configure o IIS Express para usar o FQDN para o seu aplicativo:

    1. Abra um prompt de comando com privilégios elevados no Windows.
    2. Digite o seguinte comando para alterar para a pasta IIS Express:

        <kbd>CD /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Digite o seguinte comando para adicionar o FQDN para o seu aplicativo:

        <kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  Em que **WebApplication1** é o nome do seu projeto e **bindingInformation** contém o número da porta e o FQDN que você deseja usar para seus testes.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Como obter as configurações do aplicativo para autenticação da Microsoft

A vinculação de um aplicativo para o Windows Live para Microsoft Authentication é um processo simples. Se você já não tiver vinculado um aplicativo para o Windows Live, você pode usar as seguintes etapas:

1. Navegue até [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) e insira seu nome de conta da Microsoft e a senha quando solicitado, clique em **entrar**:

    [![](external-authentication-services/_static/image64.png "Clique para expandir a imagem")](external-authentication-services/_static/image63.png)
2. Insira o nome e o idioma de seu aplicativo quando solicitado e, em seguida, clique em **aceito**:

    [![](external-authentication-services/_static/image66.png "Clique para expandir a imagem")](external-authentication-services/_static/image65.png)
3. Sobre o **configurações de API** para seu aplicativo, insira o domínio de redirecionamento para seu aplicativo e a cópia a **ID do cliente** e **segredo do cliente** para seu projeto e, em seguida, Clique em **salvar**:

    [![](external-authentication-services/_static/image68.png "Clique para expandir a imagem")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Opcional: Desabilitar o registro do Local

A funcionalidade de registro do local atual do ASP.NET não impedir que programas automatizados (bots) criar membro contas; Por exemplo, usando uma tecnologia de prevenção de bot e a validação como [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Por isso, você deve remover o link de registro e o formulário de logon local na página de logon. Para fazer isso, abra o  *\_cshtml* página em seu projeto e, em seguida, comente as linhas para o painel de logon local e o link de registro. A página resultante deve parecer com o exemplo de código a seguir:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Depois que o painel de logon local e o link de registro foram desabilitadas, a página de logon só exibirá os provedores de autenticação externa que você tiver habilitado:

[![](external-authentication-services/_static/image70.png "Clique para expandir a imagem")](external-authentication-services/_static/image69.png)
