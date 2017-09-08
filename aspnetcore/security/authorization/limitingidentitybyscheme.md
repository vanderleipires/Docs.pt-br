---
title: "Limitação de identidade pelo esquema"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a>Limitação de identidade pelo esquema

<a name=security-authorization-limiting-by-scheme></a>

Em alguns cenários, como aplicativos de página única é possível acabar com vários métodos de autenticação. Por exemplo, seu aplicativo pode usar autenticação baseada em cookie para fazer logon e autenticação do portador para solicitações de JavaScript. Em alguns casos, você pode ter várias instâncias de um middleware de autenticação. Por exemplo, dois cookies middlewares onde um contém uma identidade básica e um é criado quando uma autenticação multifator foi disparado porque o usuário solicitou uma operação que requer segurança adicional.

Esquemas de autenticação são nomeados ao middleware de autenticação é configurado durante a autenticação, por exemplo

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

Nessa configuração middlewares de autenticação de dois foram adicionados, uma para cookies e outra para transmissão.

>[!NOTE]
>Ao adicionar vários middleware de autenticação, que você deve garantir que nenhum middleware é configurado para ser executado automaticamente. Você pode fazer isso definindo o `AutomaticAuthenticate` opções de propriedade como false. Se você não conseguir fazer a filtragem por esquema não funcionará.

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Selecionar o esquema com o atributo de autorização

Como nenhuma middleware de autenticação está configurado para executar automaticamente e criar uma identidade, que você deve, no ponto de autorização escolha qual middleware será usado. Selecione o middleware que deseja autorizar com a maneira mais simples é usar o `ActiveAuthenticationSchemes` propriedade. Essa propriedade aceita uma lista delimitada por vírgulas de esquemas de autenticação a ser usado. Por exemplo,

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

No exemplo acima de cookie e o portador middlewares será executado e tenha a oportunidade de criar e acrescentar uma identidade para o usuário atual. Especificando um único esquema somente o middleware especificado será executado;

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

Nesse caso somente o middleware com o esquema de portador executaria e identidades baseada em cookie serão ignoradas.

## <a name="selecting-the-scheme-with-policies"></a>Selecionar o esquema com as políticas

Se você preferir especificar os esquemas de desejado [política](policies.md#security-authorization-policies-based) você pode definir o `AuthenticationSchemes` coleção ao adicionar sua política.

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

Neste exemplo o Over18 política só será executada em relação a identidade criada pelo `Bearer` middleware.
