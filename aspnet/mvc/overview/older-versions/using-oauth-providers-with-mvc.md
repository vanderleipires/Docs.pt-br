---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Usando provedores OAuth com o MVC 4 | Microsoft Docs
author: tfitzmac
description: Este tutorial mostra como criar um aplicativo web ASP.NET MVC 4 que permite que os usuários façam logon com credenciais de um provedor externo, como Facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 9b0db2775db5c74762bdc55328ad44ef7ebe75ce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831646"
---
<a name="using-oauth-providers-with-mvc-4"></a>Usando provedores OAuth com o MVC 4
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um aplicativo web ASP.NET MVC 4 que permite aos usuários fazer logon com credenciais de um provedor externo, como o Facebook, Twitter, Microsoft ou Google e, em seguida, integre algumas das funcionalidades desses provedores em seu aplicativo Web. Para simplificar, este tutorial concentra-se sobre como trabalhar com as credenciais do Facebook.
> 
> Para usar credenciais externas em um aplicativo ASP.NET MVC 5, consulte [criar um aplicativo do ASP.NET MVC 5 com o Facebook e Google OAuth2 e OpenID Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Habilitar essas credenciais em sites da web fornece uma vantagem significativa porque milhões de usuários já têm contas com esses provedores externos. Esses usuários podem ser mais decididos a se inscrever para seu site se eles não precisam criar e lembrar de um novo conjunto de credenciais. Além disso, depois que um usuário tiver conectado por meio de um desses provedores, você pode incorporar operações sociais do provedor.


## <a name="what-youll-build"></a>O que você vai criar

Há duas metas principais neste tutorial:

1. Permitir que um usuário fazer logon com credenciais de um provedor de OAuth.
2. Recuperar informações da conta do provedor e integrar essas informações com o registro de conta para seu site.

Embora os exemplos neste tutorial se concentrar em usando o Facebook como provedor de autenticação, você pode modificar o código para usar qualquer um dos provedores. As etapas para implementar qualquer provedor são muito semelhantes às etapas que você verá neste tutorial. Você só verá diferenças significativas ao fazer chamadas diretas para a API do provedor definida.

## <a name="prerequisites"></a>Pré-requisitos

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) ou [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Ou

- Microsoft Visual Studio 2010 SP1 ou [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Além disso, este tópico pressupõe que você tenha um conhecimento básico sobre o ASP.NET MVC e o Visual Studio. Se você precisar de uma introdução ao ASP.NET MVC 4, consulte [Introdução ao ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Criar o projeto

No Visual Studio, crie um novo aplicativo ASP.NET MVC 4 da Web e nomeie- &quot;OAuthMVC&quot;. Você pode direcionar o .NET Framework 4.5 ou 4.

![Criar projeto](using-oauth-providers-with-mvc/_static/image1.png)

Na janela novo projeto ASP.NET MVC 4, selecione **aplicativo de Internet** e deixe **Razor** como o mecanismo de exibição.

![Selecione o aplicativo de Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Habilite um provedor

Quando você cria um aplicativo web MVC 4 com o modelo de aplicativo de Internet, o projeto é criado com um arquivo chamado AuthConfig.cs no aplicativo\_pasta inicial.

![Arquivo AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

O arquivo AuthConfig contém código para registrar clientes para provedores de autenticação externa. Por padrão, esse código é comentado, portanto, nenhum dos provedores externos estão habilitados.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Você deve remover o comentário este código para usar o cliente de autenticação externa. Remova o comentário do somente os provedores que você deseja incluir em seu site. Para este tutorial, você habilitará apenas as credenciais do Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Observe no exemplo acima que o método inclui cadeias de caracteres vazias para os parâmetros de registro. Se você tentar executar o aplicativo agora, o aplicativo gerará uma exceção de argumento, como cadeias de caracteres vazias não são permitidas para os parâmetros. Para fornecer valores válidos, você deve registrar seu site da web com provedores externos, conforme mostrado na próxima seção.

## <a name="registering-with-an-external-provider"></a>Registrando um provedor externo

Para autenticar usuários com credenciais de um provedor externo, você deve registrar seu site da web com o provedor. Quando você registra seu site, você receberá os parâmetros (como chave ou id e segredo) para incluir ao registrar o cliente. Você deve ter uma conta com os provedores que você deseja usar.

Este tutorial não mostra todas as etapas que você deve executar para registrar com esses provedores. Normalmente, as etapas não são difíceis. Para registrar o seu site com êxito, siga as instruções fornecidas nesses sites. Para começar a registrar seu site, consulte o site de desenvolvedor para:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Ao registrar o seu site com o Facebook, você pode fornecer &quot;localhost&quot; para o domínio do site e `&quot;http://localhost/&quot;` para a URL, conforme mostrado na imagem abaixo. Usando localhost funciona com a maioria dos provedores, mas atualmente não funciona com o provedor Microsoft. Para o provedor da Microsoft, você deve incluir uma URL de site válido.

![Registre o site](using-oauth-providers-with-mvc/_static/image4.png)

Na imagem anterior, os valores para a id do aplicativo, o segredo do aplicativo e o email de contato foram removidos. Ao registrar o seu site na verdade, esses valores estará presentes. Você desejará observar os valores de id do aplicativo e segredo do aplicativo porque você os adicionará ao seu aplicativo.

## <a name="creating-test-users"></a>Criar usuários de teste

Se você não se importar usando uma conta existente do Facebook para testar seu site, você pode ignorar esta seção.

Você pode criar facilmente os usuários de teste para seu aplicativo na página de gerenciamento de aplicativo do Facebook. Você pode usar essas contas para fazer logon em seu site de teste. Criar usuários de teste clicando o **funções** link no painel de navegação esquerdo e clicando o **criar** link.

![criar usuários de teste](using-oauth-providers-with-mvc/_static/image5.png)

O site do Facebook cria automaticamente o número de contas de teste que você solicitar.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Adicionando a id do aplicativo e o segredo do provedor

Agora que você recebeu o segredo e id do Facebook, volte para o arquivo AuthConfig e adicioná-los como os valores de parâmetro. Os valores mostrados abaixo não são valores reais.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Faça logon com credenciais externas

Isso é tudo o que você precisa fazer para habilitar credenciais externas no seu site. Execute seu aplicativo e clique no link de logon no canto superior direito. O modelo reconhece automaticamente que você registrou o Facebook como um provedor e inclui um botão para o provedor. Se você registrar vários provedores, um botão para cada uma delas é incluído automaticamente.

![logon externo](using-oauth-providers-with-mvc/_static/image6.png)

Este tutorial não abrange como personalizar o log em botões para provedores externos. Para obter essas informações, consulte [Personalizando o interface do usuário de logon ao usar OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Clique no botão Facebook para fazer logon com as credenciais do Facebook. Quando você seleciona um dos provedores externos, você é redirecionado para o site e solicitado por esse serviço a fazer logon.

A imagem a seguir mostra a tela de logon do Facebook. Ele observa que você estiver usando sua conta do Facebook para fazer logon em um site chamado oauthmvcexample.

![autenticação do Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Depois de fazer logon com as credenciais do Facebook, uma página informa ao usuário que o site terá acesso a informações básicas.

![solicitar permissão](using-oauth-providers-with-mvc/_static/image8.png)

Depois de selecionar **ir para o aplicativo**, o usuário deve se registrar para o site. A imagem a seguir mostra a página de registro depois que um usuário fez logon com as credenciais do Facebook. O nome de usuário é normalmente previamente preenchido com um nome do provedor.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Clique em **registrar** para concluir o registro. Feche o navegador.

Você pode ver que a nova conta foi adicionada ao banco de dados. No Gerenciador de servidores, abra o **DefaultConnection** do banco de dados e abra o **tabelas** pasta.

![tabelas de banco de dados](using-oauth-providers-with-mvc/_static/image10.png)

Clique com botão direito do **UserProfile** de tabela e selecione **Mostrar dados da tabela**.

![Mostrar dados](using-oauth-providers-with-mvc/_static/image11.png)

Você verá a nova conta que você adicionou. Examinar os dados no **página da Web\_OAuthMembership** tabela. Você verá mais dados relacionados ao provedor externo para a conta que você acabou de adicionar.

Se você quiser habilitar autenticação externa, você terminar. No entanto, você pode integrar ainda mais informações do provedor ao novo processo de registro do usuário, conforme mostrado nas seções a seguir.

## <a name="create-models-for-additional-user-information"></a>Criar modelos para informações adicionais do usuário

Como observado nas seções anteriores, você não precisará recuperar quaisquer informações adicionais para o registro de conta interna trabalhar. No entanto, os provedores externos mais passam informações adicionais sobre o usuário. As seções a seguir mostram como reter essa informação e salvá-lo em um banco de dados. Especificamente, você manterá os valores para nome completo do usuário, o URI da página de web pessoal do usuário e um valor que indica se o Facebook confirmou a conta.

Você usará [migrações do Code First](https://msdn.microsoft.com/data/jj591621) para adicionar uma tabela para armazenar informações adicionais do usuário. Você está adicionando a tabela ao banco de dados existente, portanto, primeiro você precisará criar um instantâneo do banco de dados atual. Criando um instantâneo do banco de dados atual, você pode criar posteriormente uma migração que contém somente a nova tabela. Para criar um instantâneo do banco de dados atual:

1. Abra o **Package Manager Console**
2. Execute o comando **enable-migrations**
3. Execute o comando **adicionar – migração inicial – IgnoreChanges**
4. Execute o comando **Atualizar banco de dados**

Agora, você adicionará as novas propriedades. Na pasta modelos, abra o arquivo AccountModels.cs e localizar a classe RegisterExternalLoginModel. A classe RegisterExternalLoginModel contém os valores retornados do provedor de autenticação. Adicione propriedades chamadas FullName e o Link, como destacado abaixo.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Também no AccountModels.cs, adicione uma nova classe chamada ExtraUserInformation. Essa classe representa a nova tabela que será criada no banco de dados.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Na classe UsersContext, adicione o código realçado abaixo para criar uma propriedade DbSet para a nova classe.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Agora você está pronto para criar uma nova tabela. Abra o Console do Gerenciador de pacotes novamente e, desta vez:

1. Execute o comando **AddExtraUserInformation adicionar-migração**
2. Execute o comando **Atualizar banco de dados**

Agora existe a nova tabela no banco de dados.

## <a name="retrieve-the-additional-data"></a>Recuperar os dados adicionais

Há duas maneiras de recuperar dados de usuário adicionais. A primeira maneira é manter os dados de usuário que são passados de volta, por padrão, durante a solicitação de autenticação. A segunda maneira é chamar a API de provedor especificamente e solicitar mais informações. Valores de FullName e Link são passados automaticamente volta pelo Facebook. Um valor que indica se o Facebook confirmou a conta é recuperado por meio de uma chamada à API do Facebook. Primeiro, você preencherá os valores de FullName e o Link e, em seguida, posteriormente, você obterá o valor verificado.

Para recuperar os dados de usuário adicional, abra o **AccountController.cs** arquivo na **controladores** pasta.

Esse arquivo contém a lógica para registro em log, registrar e gerenciar contas. Em particular, observe os métodos chamados **ExternalLoginCallback** e **ExternalLoginConfirmation**. Dentro desses métodos, você pode adicionar código para personalizar as operações de logon externo para seu aplicativo. A primeira linha do **ExternalLoginCallback** método contém:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Dados adicionais do usuário são repassados na **ExtraData** propriedade do **AuthenticationResult** objeto que é retornado o **VerifyAuthentication** método. O cliente do Facebook contém os seguintes valores na **ExtraData** propriedade:

- id
- name
- link
- sexo
- accesstoken

Outros provedores terá dados semelhantes, mas ligeiramente diferentes na propriedade ExtraData.

Se o usuário é novo no seu site, você recuperar alguns dados adicionais e passá-los para o modo de exibição de confirmação. O último bloco de código no método é executado somente se o usuário é novo no seu site. Substitua a linha a seguir:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Com esta linha:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Essa alteração simplesmente inclui valores para as propriedades de FullName e Link.

No **ExternalLoginConfirmation** método, modifique o código conforme realçado abaixo para salvar as informações de usuário adicionais.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Ajustando o modo de exibição

Os dados de usuário adicionais que podem ser recuperadas do provedor aparecerão na página de registro.

No **modos de exibição**/**conta** pasta, abra **ExternalLoginConfirmation.cshtml**. Abaixo do campo existente para o nome de usuário, adicione campos para FullName, Link e PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Agora você está quase pronto para executar o aplicativo e registrar um novo usuário com as informações adicionais salvas. Você deve ter uma conta que já não está registrada com o site. Você pode usar uma conta de teste diferente ou exclui as linhas as **UserProfile** e **páginas da Web\_OAuthMembership** tabelas para a conta que você deseja reutilizar. Excluindo as linhas, você terá de garantir que a conta está registrada novamente.

Execute o aplicativo e registrar o novo usuário. Observe que dessa vez a página de confirmação contém mais valores.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Depois de concluir o registro, feche o navegador. Examinar o banco de dados, observe os novos valores na **ExtraUserInformation** tabela.

## <a name="install-nuget-package-for-facebook-api"></a>Instalar o pacote do NuGet para a API do Facebook

O Facebook fornece um [API](https://developers.facebook.com/docs/reference/apis/) que você pode chamar para executar operações. Você pode chamar a API do Facebook, direcionando a enviar solicitações HTTP ou usando a instalação de um pacote do NuGet que facilita o envio dessas solicitações. Usar um pacote do NuGet é mostrado neste tutorial, mas instalar o NuGet pacote não é essencial. Este tutorial mostra como usar o pacote C# SDK do Facebook. Há outros pacotes do NuGet que ajudam com a chamada da API do Facebook.

Dos **gerenciar pacotes NuGet** windows, selecione o pacote C# SDK do Facebook.

![Instalar pacote](using-oauth-providers-with-mvc/_static/image13.png)

Você usará o SDK do Facebook C# para chamar uma operação que exige o token de acesso do usuário. A próxima seção mostra como obter o token de acesso.

## <a name="retrieve-access-token"></a>Recuperar o token de acesso

Provedores externos mais passam de volta um token de acesso depois que as credenciais do usuário são verificadas. Esse token de acesso é muito importante porque permite que você chame as operações que estão disponíveis somente para usuários autenticados. Portanto, recuperar e armazenar o token de acesso é essencial quando você deseja fornecer mais funcionalidade.

Dependendo do provedor externo, o token de acesso pode ser válido para apenas uma quantidade limitada de tempo. Para garantir que você tenha um token de acesso válido, você irá recuperá-lo sempre que o usuário fizer logon e armazená-lo como um valor de sessão em vez de salvá-lo em um banco de dados.

No **ExternalLoginCallback** método, o token de acesso também é repassado na **ExtraData** propriedade do **AuthenticationResult** objeto. Adicione o código realçado ao **ExternalLoginCallback** para salvar o token de acesso na **sessão** objeto. Esse código é executado sempre que o usuário faz logon com uma conta do Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Embora este exemplo recupera um token de acesso do Facebook, você pode recuperar o token de acesso de qualquer provedor externo por meio da mesma chave nomeada &quot;accesstoken&quot;.

## <a name="logging-off"></a>Logoff

O padrão **LogOff** método registra o usuário do seu aplicativo, mas não registra o usuário do provedor externo. Para fazer logon do usuário do Facebook e impedir que o token de persistência depois que o usuário fez logoff, você pode adicionar o seguinte código realçado para o **LogOff** método no controlador.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

O valor que você fornece no `next` parâmetro é a URL a ser usada depois que o usuário tiver feito fora do Facebook. Durante o teste no computador local, você forneceria o número de porta correto para seu site local. Em produção, você forneceria uma página padrão, como contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Recuperar informações do usuário que requer o token de acesso

Agora que você tiver armazenado o token de acesso e o pacote do Facebook C# SDK instalado, você pode usá-los juntos para solicitar informações adicionais do usuário do Facebook. No **ExternalLoginConfirmation** método, crie uma instância das **FacebookClient** classe, passando o valor do token de acesso. O valor de solicitação a **verificado** propriedade para o usuário atual autenticado. O **verificado** propriedade indica se o Facebook validou a conta por outros meios, como enviar uma mensagem para um telefone celular. Salve esse valor no banco de dados.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Novamente, você precisará excluir os registros no banco de dados para o usuário, ou usar uma conta diferente do Facebook.

Execute o aplicativo e registrar o novo usuário. Examine os **ExtraUserInformation** tabela para ver o valor da propriedade verificado.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você criou um site que esteja integrado com o Facebook para autenticação de usuário e dados de registro. Você aprendeu sobre o comportamento padrão que é configurado para o aplicativo web MVC 4 e como personalizar o comportamento padrão.

## <a name="related-topics"></a>Tópicos relacionados

- [Criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no serviço de aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
