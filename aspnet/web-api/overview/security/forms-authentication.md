---
uid: web-api/overview/security/forms-authentication
title: "Autenticação de formulários ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: "Descreve como usar a autenticação de formulários na API da Web do ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="forms-authentication-in-aspnet-web-api"></a>Autenticação de formulários na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticação de formulários usa um formulário HTML para enviar as credenciais do usuário para o servidor. Não é um padrão da Internet. Autenticação de formulários só é adequada para APIs da web que são chamados de um aplicativo web, para que o usuário pode interagir com o formulário HTML.

| Vantagens | Desvantagens |
| --- | --- |
| -Fácil de implementar: internos do ASP.NET. -Usa o provedor de associação do ASP.NET, o que torna mais fácil gerenciar contas de usuário. | -Não um padrão mecanismo de autenticação HTTP; usa cookies HTTP em vez de cabeçalho de autorização padrão. -Requer um cliente de navegador. -Credenciais são enviadas como texto sem formatação. -Vulnerável a falsificação de solicitação entre sites (CSRF); requer medidas anti-CSRF. -Difícil de usar de nonbrowser clientes. Logon requer um navegador. -As credenciais do usuário são enviadas na solicitação. -Alguns usuários desabilitam cookies. |

Em resumo, a autenticação de formulários do ASP.NET funciona da seguinte maneira:

1. O cliente solicita um recurso que exige autenticação.
2. Se o usuário não é autenticado, o servidor retorna HTTP 302 (não encontrado) e redireciona para uma página de logon.
3. O usuário insere as credenciais e envia o formulário.
4. O servidor retorna outro HTTP 302 redireciona de volta para o URI original. Essa resposta inclui um cookie de autenticação.
5. O cliente solicita o recurso novamente. A solicitação inclui o cookie de autenticação para que o servidor concede a solicitação.

![](forms-authentication/_static/image1.png)

Para obter mais informações, consulte [uma visão geral de formulários de autenticação.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Usando a autenticação de formulários com a API da Web

Para criar um aplicativo que usa autenticação de formulários, selecione o modelo de "Aplicativo de Internet" no Assistente de projeto MVC 4. Este modelo cria controladores MVC para gerenciamento de conta. Você também pode usar o modelo de "Aplicativo de página única", disponível no ASP.NET estão 2012 Update.

Em seus controladores de API da web, você pode restringir o acesso por meio de `[Authorize]` de atributo, conforme descrito em [usando o atributo [autorizar]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Autenticação de formulários usa um cookie de sessão para autenticar solicitações. Navegadores enviam automaticamente todos os cookies relevantes para o site de destino. Esse recurso facilita a autenticação de formulários vulnerável a ver os ataques CSRF (falsificação) de solicitação intersite [ataques de falsificação de solicitação intersite impedindo (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

Autenticação de formulários não criptografa as credenciais do usuário. Portanto, a autenticação de formulários não é segura, a menos que usado com SSL. Consulte [trabalhar com SSL na API da Web](working-with-ssl-in-web-api.md).
