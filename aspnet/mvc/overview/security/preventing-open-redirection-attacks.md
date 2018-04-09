---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Evitando ataques de redirecionamento aberto (c#) | Microsoft Docs
author: jongalloway
description: Este tutorial explica como você pode impedir ataques de redirecionamento abertos em seus aplicativos ASP.NET MVC. Este tutorial aborda as alterações que foram feitas...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: ec1cd1791eb6d32e7c1ea50bc6626929cad2960e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="preventing-open-redirection-attacks-c"></a>Evitando ataques de redirecionamento aberto (c#)
====================
por [Jon Galloway](https://github.com/jongalloway)

> Este tutorial explica como você pode impedir ataques de redirecionamento abertos em seus aplicativos ASP.NET MVC. Este tutorial aborda as alterações que foram feitas no AccountController no ASP.NET MVC 3 e demonstra como você pode aplicar essas alterações em aplicativos de 2 e o ASP.NET MVC 1.0 existente.


## <a name="what-is-an-open-redirection-attack"></a>O que é um ataque de redirecionamento aberto?

Qualquer aplicativo web que redireciona para uma URL que é especificada por meio de solicitação, como os dados de formulário ou querystring potencialmente pode ser violado para redirecionar usuários para uma URL externa e mal-intencionados. Essa violação é chamado de um ataque de redirecionamento aberto.

Sempre que a lógica do aplicativo redireciona para uma URL específica, você deve verificar se a URL de redirecionamento não foi adulterada. O logon usado no padrão AccountController para ASP.NET MVC 1.0 e o ASP.NET MVC 2 é vulnerável a ataques de redirecionamento de abrir. Felizmente, é fácil de atualizar seus aplicativos existentes para usar as correções do ASP.NET MVC 3 Preview.

Para entender a vulnerabilidade, vamos examinar como o redirecionamento de logon funciona em um projeto de aplicativo Web do ASP.NET MVC 2 padrão. Neste aplicativo, tentar visitar uma ação do controlador que possui o atributo [autorizar] redirecionará usuários não autorizados para a exibição de /Account/LogOn. Esse redirecionamento /Account/LogOn incluirá um parâmetro de querystring returnUrl para que o usuário pode ser retornado para a URL solicitada originalmente depois que eles efetuou com êxito.

Captura de tela abaixo, podemos ver que uma tentativa de acessar o modo de exibição /Account/ChangePassword quando não conectado resulta em um redirecionamento para /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: página de logon com um redirecionamento aberto

Como o parâmetro de querystring ReturnUrl não for validado, um invasor pode modificar para inserir qualquer endereço de URL para o parâmetro para realizar um ataque de redirecionamento aberto. Para demonstrar isso, podemos modificar o parâmetro ReturnUrl [ http://bing.com ](http://bing.com), portanto, a URL de logon resultante será/conta/LogOn? ReturnUrl =<http://www.bing.com/>. Após fazer logon com êxito no site, são redirecionadas para [ http://bing.com ](http://bing.com). Como esse redirecionamento não for validado, ele em vez disso, pode apontar para um site mal-intencionado que tenta fazer com que o usuário.

### <a name="a-more-complex-open-redirection-attack"></a>Um ataque de redirecionamento aberto mais complexos

Ataques de redirecionamento aberto são especialmente perigosos porque um invasor sabe que estamos tentando fazer logon em um site específico, o que torna nos vulnerável a um [ataques de phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Por exemplo, um invasor pode enviar emails mal-intencionados para usuários do site em uma tentativa para capturar suas senhas. Vamos ver como isso funciona em site NerdDinner. (Observe que o site NerdDinner foi atualizado para se proteger contra ataques de redirecionamento aberto).

Primeiro, um invasor envia em um link para a página de logon NerdDinner que inclui um redirecionamento para suas páginas forjadas:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Observe que a URL de retorno aponta nerddiner.com, que está faltando um "n" de uma refeição o word. Neste exemplo, este é um domínio que o invasor controla. Quando podemos acessar no link acima, são levados para a página de logon NerdDinner.com legítima.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: NerdDinner a página de logon com um redirecionamento aberto

Quando registramos corretamente, ação de LogOn do AccountController do ASP.NET MVC redireciona nós para a URL especificada no parâmetro querystring returnUrl. Nesse caso, é a URL que o invasor tenha inserido, que é [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). A menos que estamos muito cuidados, é muito provável que não irá notar que isso, especialmente porque o invasor tenha sido cuidado para garantir que suas páginas forjadas parece exatamente com a página de logon legítimo. Essa página de logon inclui uma mensagem de erro solicitando que podemos fazer logon novamente. Trabalhosa nós, é necessário ter digitado sua senha.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: tela de logon de NerdDinner forjado

Quando é novamente em nosso nome de usuário e senha, a página de logon forjado salva as informações e nos envia para o site NerdDinner.com legítimo. Neste ponto, o site NerdDinner.com já autenticado, para a página de logon forjado pode redirecionar diretamente para a página. O resultado final é que o invasor terá nosso nome de usuário e senha, e estamos não reconhecem que fornecemos-lo para eles.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Olhando para o código vulnerável na ação AccountController LogOn

O código para a ação de LogOn em um aplicativo ASP.NET MVC 2 é mostrado abaixo. Observe que, após um logon bem-sucedido, o controlador retorna um redirecionamento para a returnUrl. Você pode ver que nenhuma validação está sendo executada com o parâmetro returnUrl.

**Listando 1 – a ação de LogOn do ASP.NET MVC 2 `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Agora vamos examinar as alterações para a ação de LogOn do ASP.NET MVC 3. Este código foi alterado para validar o parâmetro returnUrl chamando um novo método na classe auxiliar System.Web.Mvc.Url chamada `IsLocalUrl()`.

**Listando 2 – a ação de LogOn do ASP.NET MVC 3 `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Isso foi alterado para validar o parâmetro de URL de retorno chamando um novo método na classe auxiliar System.Web.Mvc.Url, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protegendo o ASP.NET MVC 1.0 e MVC 2 aplicativos

Podemos pode aproveitar as alterações do ASP.NET MVC 3 em nosso ASP.NET MVC 1.0 existentes e 2 aplicativos adicionando o método auxiliar IsLocalUrl() e atualizar a ação de LogOn para validar o parâmetro returnUrl.

O método UrlHelper IsLocalUrl() apenas chamando um método em System.Web.WebPages, como essa validação também é usado por aplicativos de páginas da Web ASP.NET.

**A listagem 3 – o método IsLocalUrl() do UrlHelper do ASP.NET MVC 3 `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

O método IsUrlLocalToHost contém a lógica de validação real, conforme mostrado na listagem 4.

**A listagem 4 – IsUrlLocalToHost() método da classe System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Em nosso ASP.NET MVC 1.0 ou o aplicativo 2, adicionaremos um método IsLocalUrl() o AccountController, mas é recomendável para adicioná-lo a uma classe auxiliar separado se possível. Faremos duas pequenas alterações para a versão do ASP.NET MVC 3 do IsLocalUrl() para que ele seja executado dentro do AccountController. Primeiro, vamos alterá-lo de um método público para um método privado, desde que os métodos públicos em controladores podem ser acessados como ações do controlador. Em segundo lugar, modificaremos a chamada que verifica o host de URL com o host de aplicativo. Essa chamada faz o uso de um local RequestContext campo na classe UrlHelper. Em vez de usar isso. RequestContext.HttpContext.Request.Url.Host, usaremos isso. Request.Url.Host. O código a seguir mostra o método IsLocalUrl() modificado para uso com uma classe de controlador no ASP.NET MVC 1.0 e 2 aplicativos.

**Listando 5 – o método IsLocalUrl(), que é modificado para usar com uma classe do controlador MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Agora que o método IsLocalUrl() está em vigor, vamos pode chamá-la de nossa ação de LogOn para validar o parâmetro returnUrl, conforme mostrado no código a seguir.

**Listando 6 – o método de LogOn atualizadas que valida o parâmetro returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Agora podemos testar um ataque de redirecionamento aberto, na tentativa de fazer logon usando uma URL externa de retorno. Vamos usar /Account/LogOn? ReturnUrl =<http://www.bing.com/> novamente.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: o teste da ação de LogOn atualizado

Depois de fazer logon com êxito, podemos são redirecionados para a ação de controlador Home/índice em vez da URL externa.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: ataques de redirecionamento aberto vencido

## <a name="summary"></a>Resumo

Ataques de redirecionamento aberto podem ocorrer quando o redirecionamento de URLs são passadas como parâmetros na URL para um aplicativo. O ASP.NET MVC 3 inclui o modelo de código para proteger contra abrir ataques de redirecionamento. Você pode adicionar esse código com algumas modificações para ASP.NET MVC 1.0 e 2 aplicativos. Para proteger contra ataques de redirecionamento aberta ao fazer logon no ASP.NET 1.0 e 2 aplicativos, adicione um método IsLocalUrl() e validar o parâmetro returnUrl na ação de LogOn.
