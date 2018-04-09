---
title: Evitar ataques de redirecionamento aberto no núcleo do ASP.NET
author: ardalis
description: Mostra como evitar ataques de redirecionamento aberto em um aplicativo do ASP.NET Core
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/preventing-open-redirects
ms.openlocfilehash: 4a210b8bb8091e7c036d4bc98306e3b3f90d7d46
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Evitar ataques de redirecionamento aberto no núcleo do ASP.NET

Um aplicativo web que redireciona para uma URL que é especificada por meio de solicitação, como os dados de formulário ou querystring potencialmente pode ser violado para redirecionar usuários para uma URL externa e mal-intencionados. Essa violação é chamado de um ataque de redirecionamento aberto.

Sempre que a lógica do aplicativo redireciona para uma URL específica, você deve verificar se a URL de redirecionamento não foi adulterada. Núcleo do ASP.NET tem funcionalidade interna para ajudar a proteger aplicativos contra ataques de redirecionamento aberto (também conhecido como open redirecionamento).

## <a name="what-is-an-open-redirect-attack"></a>O que é um ataque de redirecionamento aberto?

Aplicativos da Web com frequência redirecionam os usuários para uma página de logon quando acessarem os recursos que exigem autenticação. O redirecionamento typlically inclui um `returnUrl` parâmetro querystring para que o usuário pode ser retornado para a URL solicitada originalmente depois que eles efetuou com êxito. Depois que o usuário é autenticado, eles serão redirecionados para a URL que tinha originalmente solicitada.

Como a URL de destino é especificada na querystring da solicitação, um usuário mal-intencionado pode violar querystring. Uma querystring violado pode permitir que o site redirecionar o usuário a um site externo, mal-intencionado. Essa técnica é chamada de um ataque de redirecionamento (ou redirecionamento) aberto.

### <a name="an-example-attack"></a>Um ataque de exemplo

Um usuário mal-intencionado poderá desenvolver um ataque de objetivo de permitir que o usuário mal-intencionado acesso a informações confidenciais em seu aplicativo ou as credenciais de um usuário. Para iniciar o ataque, eles convencer o usuário clicar em um link para a página de logon do site, com um `returnUrl` valor de querystring adicionada à URL. Por exemplo, o [NerdDinner.com](http://nerddinner.com) (gravado para o ASP.NET MVC) do aplicativo de exemplo inclui tal uma página de logon aqui: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``. O ataque, em seguida, siga estas etapas:

1. Usuário clica em um link para ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Observe que a URL segundo é nerddi**n**er, não nerddi**nn**er).
2. O usuário fizer logon com êxito.
3. O usuário é redirecionado (pelo site) para ``http://nerddiner.com/Account/LogOn`` (site mal-intencionado que se parece com um site real).
4. O usuário fizer logon novamente (fornecendo mal-intencionado suas credenciais do site) e é redirecionado para o site real.

O usuário provavelmente vai achar sua primeira tentativa de logon falha, e sua segunda foi bem-sucedida. Provavelmente permanecerá não reconhecem as credenciais foram comprometidas.

![Processo de ataques de redirecionamento aberto](preventing-open-redirects/_static/open-redirection-attack-process.png)

Além das páginas de logon, alguns sites fornecem redirecionamento páginas ou pontos de extremidade. Imagine que seu aplicativo tem uma página com um redirecionamento aberto, ``/Home/Redirect``. Um invasor pode criar, por exemplo, um link em um email que vão para ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``. Um usuário normal na URL e ver começa com o nome do site. Confiar em que, eles clicarem no link. O redirecionamento aberto, em seguida, envia o usuário para o site de phishing, que é idêntico ao seu, e o usuário provavelmente é de logon para o que eles acreditam seu site.

## <a name="protecting-against-open-redirect-attacks"></a>Proteção contra ataques de redirecionamento aberto

Ao desenvolver aplicativos da web, trate todos os dados fornecidos pelo usuário como não confiável. Se seu aplicativo tem funcionalidade que redireciona o usuário com base no conteúdo da URL, certifique-se de que tais redirecionamentos só são feitos localmente em seu aplicativo (ou uma URL conhecida, e não qualquer URL que pode ser fornecido na querystring).

### <a name="localredirect"></a>LocalRedirect

Use o ``LocalRedirect`` método auxiliar de base de `Controller` classe:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

``LocalRedirect`` lançará uma exceção se uma URL de local não for especificada. Caso contrário, ele se comporta exatamente como o ``Redirect`` método.

### <a name="islocalurl"></a>IsLocalUrl

Use o [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) método de teste URLs antes de redirecionar:

O exemplo a seguir mostra como verificar se uma URL é local antes de redirecionar.

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

O `IsLocalUrl` método impede que os usuários inadvertidamente sendo redirecionado para um site mal-intencionado. Você pode registrar os detalhes da URL que foi fornecida uma URL de local não é fornecida em uma situação em que você esperava um URL local. URLs de redirecionamento de log podem ajudar no diagnóstico de ataques de redirecionamento.
