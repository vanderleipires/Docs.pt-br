---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: MVC recomendado artigos e tutoriais | Microsoft Docs
author: Rick-Anderson
description: "Esta página contém links para tutoriais do ASP.NET MVC e uma sequência sugerida para segui-las."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: b6cc785a5ddaf156f49b15897577e733fb67736a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="mvc-recommended-tutorials-and-articles"></a>MVC recomendado artigos e tutoriais
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

<a id="pwd"></a>
## <a name="getting-started"></a>Guia de Introdução

- [Introdução ao ASP.NET MVC 5](introduction/getting-started.md) série 11 este é um bom lugar para começar.
- [Conceitos básicos do Pluralsight ASP.NET MVC 5](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (curso vídeo)
- [Introdução ao ASP.NET MVC](https://www.microsoftvirtualacademy.com/training-courses/introduction-to-asp-net-mvc) Jon Galloway e Christopher Harrison
- [Ciclo de vida de um aplicativo ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) documento PDF que gráficos de ciclo de vida de um aplicativo ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Trabalhando com os dados

- [Guia de Introdução ao EF 6 Code First usando MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) premiado de Tom Dykstra com série examina profundamente EF.

<a id="wj"></a>
## <a name="security"></a>Segurança

- [Criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) este tutorial popular orienta você pela criação de um aplicativo simples e adicionar membros e funções.
- [Criar um aplicativo do ASP.NET MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial mostra como criar um aplicativo web ASP.NET MVC 5 que permite aos usuários fazer logon usando OAuth 2.0 com as credenciais de uma autenticação externa provedor, como Facebook, Twitter, LinkedIn, Microsoft ou Google.
- [Criar um aplicativo web seguro do ASP.NET MVC 5 com logon, redefinição de senha e de confirmação de email](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) primeiro em uma série de identidade, inclui o código para [reenviar um link de confirmação](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [Aplicativo ASP.NET MVC 5 com SMS e o email de autenticação de dois fatores](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) segunda série de identidade.
- [Práticas recomendadas para implantar as senhas e outros dados confidenciais para ASP.NET e o serviço de aplicativo do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Autenticação de dois fatores usando SMS e email com o ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` e o cookie de segurança, o código para exigir que um usuário tenha uma conta de email validado antes que eles possam fazer logon, como SignInManager procura 2FA requisito e muito mais.
- [Conta de confirmação e de recuperação de senha com a identidade do ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) fornece detalhes sobre a identidade não foi encontrado no [criar um aplicativo web seguro do ASP.NET MVC 5 com logon, redefinição de senha e de confirmação de email](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) por exemplo, como permitir que os usuários redefinir sua senha.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Criar um aplicativo web ASP.NET no Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/) tutorial curto e simple para implantação no Azure.
- [Criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Desempenho e depuração

- [Criar o perfil e depurar seu aplicativo ASP.NET MVC com prévia](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)
