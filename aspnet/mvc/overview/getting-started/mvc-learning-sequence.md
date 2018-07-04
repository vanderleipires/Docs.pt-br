---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: Artigos e tutoriais de MVC recomendados | Microsoft Docs
author: Rick-Anderson
description: Esta página contém links para tutoriais do ASP.NET MVC e uma sequência sugerida para segui-las.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: fd972549fd4c1f46f5bdeafbb466baab7a21e9df
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363983"
---
<a name="mvc-recommended-tutorials-and-articles"></a>Artigos e tutoriais de MVC recomendados
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

<a id="pwd"></a>
## <a name="getting-started"></a>Guia de Introdução

- [Introdução ao ASP.NET MVC 5](introduction/getting-started.md) 11 esta série é um bom lugar para começar.
- [Conceitos básicos do Pluralsight ASP.NET MVC 5](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (curso em vídeo)
- [Introdução ao ASP.NET MVC](https://www.microsoftvirtualacademy.com/training-courses/introduction-to-asp-net-mvc) por Jon Galloway e Christopher Harrison
- [Ciclo de vida de um aplicativo ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) documento PDF que gráficos de ciclo de vida de um aplicativo ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Trabalhando com os dados

- [Introdução ao EF 6 Code First usando MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) o prêmio de Tom Dykstra série mergulha aprofundada do EF.

<a id="wj"></a>
## <a name="security"></a>Segurança

- [Criar um aplicativo ASP.NET MVC com autenticação e banco de dados SQL e implantar no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) esse tutorial popular orienta você pela criação de um aplicativo simple e adicionar membros e funções.
- [Criar um aplicativo do ASP.NET MVC 5 com o Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial mostra como criar um aplicativo web ASP.NET MVC 5 que permite que os usuários façam logon usando o OAuth 2.0 com as credenciais de uma autenticação externa provedor, como Facebook, Twitter, LinkedIn, Microsoft ou Google.
- [Criar um aplicativo de web do ASP.NET MVC 5 seguro com logon, redefinição de senha e de confirmação de email](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de uma série sobre identidade, inclui código para [reenviar um link de confirmação](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [Aplicativo ASP.NET MVC 5 com o SMS e email de autenticação de dois fatores](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) segundo na série de identidade.
- [Melhores práticas para implantar senhas e outros dados confidenciais no ASP.NET e no Serviço de Aplicativo do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Autenticação de dois fatores usando SMS e email com o ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` e o cookie de segurança, o código para o usuário deverá ter uma conta de email validada antes que eles possam fazer logon, como o SignInManager procura requisito 2FA e muito mais.
- [Confirmação de conta e recuperação de senha com o ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) fornece detalhes sobre a identidade não foi encontrada no [criar um aplicativo de web do ASP.NET MVC 5 seguro com logon, redefinição de senha e de confirmação de email](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) por exemplo, como permitir que o os usuários redefinir suas senhas esquecidas.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Criar um aplicativo web ASP.NET no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/) tutorial curto e simple para implantação no Azure.
- [Criar um aplicativo ASP.NET MVC com autenticação e banco de dados SQL e implantar no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Desempenho e depuração

- [Analisar e depurar seu aplicativo do ASP.NET MVC com Glimpse](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)
