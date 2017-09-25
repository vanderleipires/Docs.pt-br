---
title: "Habilitando a autenticação usando o Facebook, o Google e outros provedores externos"
author: rick-anderson
description: 
keywords: "ASP.NET Core, autenticação, social, provedores de autenticação, Google, Facebook, Twitter, conta da Microsoft"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: b561dcee5435dfc34cfa0b9b15babf75ca8f3508
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a>Habilitando a autenticação usando o Facebook, o Google e outros provedores externos

<a name=security-authentication-social-logins></a>

Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial demonstra como criar um aplicativo ASP.NET Core 2.x que permite aos usuários fazer logon usando o OAuth 2.0 com as credenciais de provedores de autenticação externa.

Os provedores [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md) e [Microsoft](microsoft-logins.md) são abordados nas seções a seguir. Outros provedores estão disponíveis em pacotes de terceiros, como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Ícones de mídia social do Facebook, Twitter, Google+ e Windows](index/_static/social.png)

Permitir que os usuários entrem com suas credenciais existentes é conveniente para os usuários e transfere muitas das complexidades de gerenciar o processo de entrada para um terceiro. Para obter exemplos de como os logons sociais podem impulsionar o tráfego e as conversões de clientes, consulte os estudos de caso do [Facebook](https://www.facebook.com/unsupportedbrowser) e do [Twitter](https://dev.twitter.com/resources/case-studies).

Observação: os pacotes apresentados aqui eliminam uma grande parte da complexidade do fluxo de autenticação OAuth, mas o entendimento dos detalhes pode ser necessário durante a solução de problemas. Muitos recursos estão disponíveis; por exemplo, consulte [Introdução ao OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) ou [Noções básicas do OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/). Alguns problemas podem ser resolvidos examinando o [código-fonte do ASP.NET Core para os pacotes de provedores](https://github.com/aspnet/Security/tree/dev/src).

## <a name="create-a-new-aspnet-core-project"></a>Criar um novo projeto ASP.NET Core

* No Visual Studio 2017, crie um novo projeto na Página Inicial ou por meio de **Arquivo > Novo > Projeto**.

* Selecione o modelo **Aplicativo Web ASP.NET Core** disponível na categoria **Visual C# > .NET Core**:

![Caixa de diálogo Novo Projeto](index/_static/new-project.png)

* Toque em **Aplicativo Web** e verifique se a opção **Autenticação** está definida como **Contas de Usuário Individuais**:

![Caixa de diálogo Novo Aplicativo Web](index/_static/select-project.png)

Observação: este tutorial aplica-se à versão do SDK do ASP.NET Core 2.0 que pode ser selecionada na parte superior do assistente.

## <a name="require-ssl"></a>Exigir SSL

O OAuth 2.0 exige o uso de SSL para autenticação por meio do protocolo HTTPS.

Observação: os projetos criados com o uso de modelos de projeto **Aplicativo Web** ou **API Web** para o ASP.NET Core 2.x são automaticamente configurados para habilitar o SSL e iniciados com a URL HTTPS se a opção **Contas de Usuário Individuais** foi selecionada na **caixa de diálogo Alterar Autenticação** no assistente do projeto, conforme mostrado acima.

* Saiba como habilitar o SSL manualmente seguindo as etapas do tópico [Configurando o HTTPS para desenvolvimento no ASP.NET Core](xref:security/https).

* Em seguida, exija o SSL em seu site seguindo as etapas do tópico [Impondo o SSL em um aplicativo ASP.NET Core](xref:security/enforcing-ssl).

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Usar o SecretManager para armazenar os tokens atribuídos por provedores de logon

Os provedores de logon social atribuem tokens de **ID do Aplicativo** e **Segredo do Aplicativo** durante o processo de registro (a nomenclatura exata varia de acordo com o provedor).

Esses valores são efetivamente o *nome de usuário* e a *senha* que o aplicativo usa para acessar sua API e constituem os “segredos” que podem ser vinculados à configuração do aplicativo com a ajuda do **Secret Manager**, em vez de armazená-los em arquivos de configuração diretamente ou embuti-los em código.

Siga as etapas do tópico [Armazenamento seguro de segredos do aplicativo durante o desenvolvimento no ASP.NET Core](xref:security/app-secrets) para que você possa armazenar os tokens atribuídos por cada provedor de logon abaixo.

## <a name="setup-login-providers-required-by-your-application"></a>Configurar os provedores de logon necessários para o aplicativo

Use os seguintes tópicos para configurar seu aplicativo para usar os respectivos provedores:

* Instruções do [Facebook](facebook-logins.md)
* Instruções do [Twitter](twitter-logins.md)
* Instruções do [Google](google-logins.md)
* Instruções da [Microsoft](microsoft-logins.md)
* [Outras](other-logins.md) instruções do provedor

## <a name="optionally-set-password"></a>Definir a senha opcionalmente

Ao registrar um provedor de logon externo, você não precisa ter uma senha registrada no aplicativo. Isso o alivia da tarefa de criar e lembrar de uma senha para o site, mas também o torna dependente do provedor de logon externo. Se o provedor de logon externo não estiver disponível, você não poderá fazer logon no site.

Para criar uma senha e entrar usando seu email definido durante o processo de entrada com provedores externos:

* Toque no link **Olá <email alias>** na parte superior direita para navegar para a exibição **Gerenciar**.

![Exibição Gerenciar do Aplicativo Web](index/_static/pass1a.png)

* Toque em **Criar**

![Definir a página de senha](index/_static/pass2a.png)

* Defina uma senha válida e use-a para entrar com seu email.

## <a name="next-steps"></a>Próximas etapas

* Este artigo apresentou a autenticação externa e explicou os pré-requisitos necessários para adicionar logons externos ao aplicativo ASP.NET Core.

* Páginas de referência específicas ao provedor para configurar logons para os provedores necessários para o aplicativo.
