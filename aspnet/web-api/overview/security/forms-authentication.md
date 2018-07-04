---
uid: web-api/overview/security/forms-authentication
title: A autenticação de formulários na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Descreve como usar a autenticação de formulários na API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7642a30816a04a88a25ef8bf4f01e766fda362ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365065"
---
<a name="forms-authentication-in-aspnet-web-api"></a>Autenticação de formulários na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticação de formulários usa um formulário HTML para enviar as credenciais do usuário para o servidor. Não é um padrão da Internet. Autenticação de formulários é apropriada para APIs da web que são chamadas de um aplicativo web, somente para que o usuário pode interagir com o formulário HTML.

| Vantagens | Desvantagens |
| --- | --- |
| -Fácil de implementar: incorporado ao ASP.NET. – Usa o provedor de associação do ASP.NET, o que torna mais fácil de gerenciar contas de usuário. | -Não um padrão HTTP mecanismo de autenticação; usa cookies HTTP em vez do cabeçalho de autorização padrão. -Requer um cliente de navegador. -Credenciais são enviadas como texto sem formatação. -Vulnerável a falsificação de solicitação intersite forjada (CSRF); requer que medidas anti-CSRF. -Essa opção pode ser difícil de usar em clientes nonbrowser. Logon requer um navegador. -As credenciais do usuário são enviadas na solicitação. -Alguns usuários desabilitam cookies. |

Em resumo, a autenticação de formulários no ASP.NET funciona da seguinte maneira:

1. O cliente solicita um recurso que requer autenticação.
2. Se o usuário não for autenticado, o servidor retorna HTTP 302 (encontrado) e redireciona para uma página de logon.
3. O usuário insere as credenciais e envia o formulário.
4. O servidor retorna outro HTTP 302 redireciona de volta para o URI original. Essa resposta inclui um cookie de autenticação.
5. O cliente solicita o recurso novamente. A solicitação inclui o cookie de autenticação, portanto, o servidor atribui a solicitação.

![](forms-authentication/_static/image1.png)

Para obter mais informações, consulte [uma visão geral de formulários de autenticação.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Usando a autenticação de formulários com a API Web

Para criar um aplicativo que usa a autenticação de formulários, selecione o modelo "Aplicativo de Internet" no Assistente de projeto MVC 4. Este modelo cria os controladores do MVC para gerenciamento de conta. Você também pode usar o modelo "Aplicativo de página única", disponível na atualização de 2012 do outono de ASP.NET.

Em seus controladores de API da web, você pode restringir o acesso usando o `[Authorize]` de atributo, conforme descrito em [usando o atributo [autorizar]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Autenticação de formulários usa um cookie de sessão para autenticar solicitações. Os navegadores enviam automaticamente todos os cookies relevantes para o site de destino. Esse recurso torna a autenticação de formulários potencialmente vulnerável a ataques CSRF () consulte de solicitação intersite [ataques de falsificação de solicitação entre sites impedindo (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

Autenticação de formulários não criptografa as credenciais do usuário. Portanto, a autenticação de formulários não é segura, a menos que usado com SSL. Ver [trabalhando com SSL na API Web](working-with-ssl-in-web-api.md).
