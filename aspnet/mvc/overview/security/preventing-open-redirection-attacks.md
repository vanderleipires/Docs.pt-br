---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Impedindo ataques de redirecionamento aberto (c#) | Microsoft Docs
author: jongalloway
description: Este tutorial explica como você pode impedir ataques de redirecionamento aberto em seus aplicativos ASP.NET MVC. Este tutorial aborda as alterações que foram feitas...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 767f9c85527fbcdf34e700eb32fe0c6cad30bf0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825423"
---
<a name="preventing-open-redirection-attacks-c"></a>Impedindo ataques de redirecionamento aberto (c#)
====================
por [Jon Galloway](https://github.com/jongalloway)

> Este tutorial explica como você pode impedir ataques de redirecionamento aberto em seus aplicativos ASP.NET MVC. Este tutorial aborda as alterações que foram feitas no controlador no ASP.NET MVC 3 e demonstra como você pode aplicar essas alterações no seu ASP.NET MVC 1.0 existente e 2 aplicativos.


## <a name="what-is-an-open-redirection-attack"></a>O que é um ataque de redirecionamento aberto?

Qualquer aplicativo web que redireciona para uma URL que é especificada por meio de solicitação, como os dados de formulário ou cadeia de consulta potencialmente pode ser violado para redirecionar usuários para uma URL externa e mal-intencionado. Essa violação é chamado de um ataque de redirecionamento aberto.

Sempre que a lógica do aplicativo redireciona para uma URL especificada, verifique se a URL de redirecionamento não foi adulterada. O logon usado no padrão AccountController para ASP.NET MVC 1.0 e ASP.NET MVC 2 é vulnerável a ataques de redirecionamento de abrir. Felizmente, é fácil de atualizar seus aplicativos existentes para usar as correções da ASP.NET MVC 3 versão prévia.

Para entender a vulnerabilidade, vamos examinar como o redirecionamento de login funciona em um projeto de aplicativo Web do ASP.NET MVC 2 padrão. Neste aplicativo, tentando visitar uma ação do controlador que tem o atributo [autorizar] redirecionará usuários não autorizados para o modo de exibição /Account/LogOn. Esse redirecionamento para /Account/LogOn incluirá um parâmetro de cadeia de consulta returnUrl para que o usuário pode ser retornado para a URL solicitada originalmente depois que eles fizeram logon com êxito.

Captura de tela abaixo, podemos ver que uma tentativa de acessar o modo de exibição /Account/ChangePassword quando não se conectaram resulta em um redirecionamento para /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: página de logon com um redirecionamento aberto

Uma vez que o parâmetro de cadeia de consulta ReturnUrl não for validado, um invasor pode modificar para injetar qualquer endereço de URL para o parâmetro para conduzir um ataque de redirecionamento aberto. Para demonstrar isso, podemos modificar o parâmetro ReturnUrl [ http://bing.com ](http://bing.com), portanto, a URL de logon resultante será/conta/LogOn? ReturnUrl =<http://www.bing.com/>. Após fazer logon com êxito no site, podemos são redirecionados para [ http://bing.com ](http://bing.com). Uma vez que esse redirecionamento não for validado, ele em vez disso, pode apontar para um site mal-intencionado que tenta convencer o usuário.

### <a name="a-more-complex-open-redirection-attack"></a>Um ataque de redirecionamento aberto mais complexos

Ataques de redirecionamento aberto são especialmente perigosas porque um invasor saiba que estamos tentando fazer logon em um site específico, o que nos faz vulnerável a um [ataques de phishing](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Por exemplo, um invasor pode enviar emails mal-intencionados para os usuários do site em uma tentativa para capturar suas senhas. Vejamos como isso funcionaria no site do NerdDinner. (Observe que o site ao vivo do NerdDinner foi atualizado para se proteger contra ataques de redirecionamento aberto).

Primeiro, um invasor envia-em um link para a página de logon do NerdDinner que inclui um redirecionamento para suas páginas forjadas:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Observe que a URL de retorno aponta nerddiner.com, que está faltando um "n" de jantar o word. Neste exemplo, este é um domínio que o invasor controla. Quando podemos acessar o link acima, podemos são levados para a página de logon do NerdDinner.com legítimos.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: página de logon do NerdDinner com um redirecionamento aberto

Quando fazemos logon corretamente, ação de LogOn do controlador do ASP.NET MVC redireciona-nos para a URL especificada no parâmetro de cadeia de consulta returnUrl. Nesse caso, é a URL que o invasor tenha inserido, que é [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). A menos que estamos extremamente olhar, é muito provável que não percebemos isso, especialmente porque o invasor tem sido cuidado para ter certeza de que suas páginas forjadas se parece exatamente com a página de logon legítimos. Essa página de logon inclui uma mensagem de erro solicitando que podemos fazer logon novamente. Infeliz conosco, podemos deve ter digitado incorretamente nossa senha.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: tela de logon do NerdDinner forjado

Quando podemos digitar novamente nosso nome de usuário e senha, a página de logon forjados salva as informações e envia-nos de volta para o site legítimo do NerdDinner.com. Neste ponto, o site do NerdDinner.com já tiver sido autenticado nós, portanto, a página de logon forjados pode redirecionar diretamente para a página. O resultado final é que o invasor tem nosso nome de usuário e senha, e estamos cientes de que fornecemos-lo a eles.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Observando o código vulnerável na ação de LogOn AccountController

Abaixo está o código para a ação de LogOn em um aplicativo ASP.NET MVC 2. Observe que, após um logon bem-sucedido, o controlador retorna um redirecionamento para a returnUrl. Você pode ver que nenhuma validação está sendo executada em relação o parâmetro returnUrl.

**Listagem 1 – ação de LogOn do ASP.NET MVC 2 no `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Agora vamos examinar as alterações para a ação de LogOn do ASP.NET MVC 3. Esse código foi alterado para validar o parâmetro returnUrl chamando um novo método na classe auxiliar System.Web.Mvc.Url chamado `IsLocalUrl()`.

**Listagem 2 – ação de LogOn do ASP.NET MVC 3 em `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Isso foi alterado para validar o parâmetro de URL de retorno, chamando um novo método na classe auxiliar System.Web.Mvc.Url, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protegendo o ASP.NET MVC 1.0 e o MVC 2 aplicativos

Podemos pode aproveitar as alterações do ASP.NET MVC 3 em nosso ASP.NET MVC 1.0 existente e 2 aplicativos adicionando o método auxiliar IsLocalUrl() e atualizando a ação de LogOn para validar o parâmetro returnUrl.

O método UrlHelper IsLocalUrl(), na verdade, apenas chamar um método em System.Web.WebPages, como essa validação também é usado por aplicativos de páginas da Web ASP.NET.

**Listagem 3 – o método IsLocalUrl() o UrlHelper do ASP.NET MVC 3 `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

O método IsUrlLocalToHost contém a lógica de validação real, conforme mostrado na listagem 4.

**Listagem 4 – IsUrlLocalToHost() método da classe System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Em nosso ASP.NET MVC 1.0 ou o aplicativo 2, adicionaremos um método IsLocalUrl() o AccountController, mas é incentivado a adicioná-lo a uma classe auxiliar separada se possível. Faremos duas pequenas alterações para a versão do ASP.NET MVC 3 do IsLocalUrl() para que ele funciona dentro do controlador. Primeiro, vamos alterá-lo de um método público para um método privado, já que os métodos públicos em controladores podem ser acessados como ações do controlador. Em segundo lugar, vamos modificar a chamada que verifica o host de URL com o host do aplicativo. Chamada torna o uso de um local RequestContext campo na classe UrlHelper. Em vez de usar isso. RequestContext.HttpContext.Request.Url.Host, usaremos isso. Request.Url.Host. O código a seguir mostra o método IsLocalUrl() modificado para uso com uma classe de controlador no ASP.NET MVC 1.0 e 2 aplicativos.

**Listagem 5 – método IsLocalUrl(), que é modificado para uso com uma classe de controlador MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Agora que o método IsLocalUrl() está em vigor, podemos pode chamá-lo da nossa ação de LogOn para validar o parâmetro returnUrl, conforme mostrado no código a seguir.

**Listagem 6 – método de LogOn atualizado que valida o parâmetro de returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Agora podemos testar um ataque de redirecionamento aberto pela tentativa de fazer logon usando uma URL de retornada externa. Vamos usar /Account/LogOn? ReturnUrl =<http://www.bing.com/> novamente.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: o teste da ação de LogOn atualizado

Depois de fazer logon com êxito, podemos são redirecionados para a ação do controlador Home/Index em vez da URL externa.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: ataques de redirecionamento aberto derrotado

## <a name="summary"></a>Resumo

Ataques de redirecionamento aberto podem ocorrer quando o redirecionamento de URLs são passadas como parâmetros na URL para um aplicativo. Abra o modelo inclui o código para se proteger contra o ASP.NET MVC 3 ataques de redirecionamento. Você pode adicionar esse código com algumas modificações para ASP.NET MVC 1.0 e 2 aplicativos. Para proteger contra ataques de redirecionamento aberto ao fazer logon em aplicativos de 2 e ASP.NET 1.0, adicione um método IsLocalUrl() e validar o parâmetro returnUrl na ação de LogOn.
