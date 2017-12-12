---
uid: web-api/overview/security/integrated-windows-authentication
title: "Autenticação integrada do Windows | Microsoft Docs"
author: MikeWasson
description: "Descreve como usar a autenticação integrada do Windows na API da Web do ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a>Autenticação integrada do Windows
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticação integrada do Windows permite que os usuários façam logon com suas credenciais do Windows, usando Kerberos ou NTLM. O cliente envia as credenciais no cabeçalho de autorização. Autenticação do Windows é mais adequada para um ambiente de intranet. Para obter mais informações, consulte [autenticação do Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Vantagens | Desvantagens |
| --- | --- |
| -Integradas no IIS. -Não envia as credenciais do usuário na solicitação. -Se o computador cliente pertence ao domínio (por exemplo, o aplicativo intranet), o usuário não precisa inserir credenciais. | -Não é recomendado para aplicativos da Internet. -Requer suporte a Kerberos ou NTLM no cliente. -O cliente deve estar no domínio do Active Directory. |

> [!NOTE]
> Se seu aplicativo é hospedado no Azure e você tiver um domínio do Active Directory local, considere a possibilidade de Federação do AD local com o Azure Active Directory. Dessa forma, os usuários podem fazer com suas credenciais locais, mas a autenticação é realizada pelo AD do Azure. Para obter mais informações, consulte [autenticação Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Para criar um aplicativo que usa a autenticação integrada do Windows, selecione o modelo de "Aplicativo Intranet" no Assistente de projeto MVC 4. Este modelo de projeto coloca a configuração a seguir no arquivo Web. config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

No lado do cliente, a autenticação integrada do Windows funciona com qualquer navegador que ofereça suporte a [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) esquema de autenticação, que inclui a maioria dos navegadores principais. Para aplicativos de cliente .NET, o **HttpClient** classe dá suporte à autenticação do Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Autenticação do Windows é vulnerável a ataques CSRF (falsificação) de solicitação entre sites. Consulte [impedindo ataques CSRF (falsificação) de solicitação entre sites](preventing-cross-site-request-forgery-csrf-attacks.md).
