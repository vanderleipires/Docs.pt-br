---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Evitando ataques de injeção de JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Evitar ataques de injeção de JavaScript e ataques de Cross Site Scripting aconteça para você. Neste tutorial, Stephen Walther explica como você pode facilmente de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: cb19236b22abd455472621ce74a8cddf9752d6c5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871227"
---
<a name="preventing-javascript-injection-attacks-vb"></a>Evitando ataques de injeção de JavaScript (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Baixar PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Evitar ataques de injeção de JavaScript e ataques de Cross Site Scripting aconteça para você. Neste tutorial, Stephen Walther explica como você pode facilmente anular esses tipos de ataques por seu conteúdo de codificação HTML.


O objetivo deste tutorial é explicar como você pode impedir ataques de injeção de JavaScript em seus aplicativos ASP.NET MVC. Este tutorial aborda duas abordagens para proteger o site contra um ataque de injeção de JavaScript. Você aprenderá a impedir ataques de injeção de JavaScript, a codificação dos dados que você exibir. Você também aprenderá como evitar ataques de injeção de JavaScript codificação dos dados que você aceite.

## <a name="what-is-a-javascript-injection-attack"></a>O que é um ataque de injeção de JavaScript?

Sempre que você aceita a entrada do usuário e exibir novamente a entrada do usuário, você pode abrir seu site a ataques de injeção de JavaScript. Vamos examinar um aplicativo concreto aberto a ataques de injeção de JavaScript.

Imagine que você tenha criado um site de comentários do cliente (consulte a Figura 1). Os clientes podem visitar o site e inserir comentários em sua experiência usando seus produtos. Quando um cliente envia seus comentários, os comentários é exibida na página de comentários novamente.


[![Site de comentários do cliente](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figura 01**: site de comentários do cliente ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image3.png))


O site de comentários do cliente usa o `controller` na listagem 1. Isso `controller` contém duas ações denominadas `Index()` e `Create()`.

**Listando 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

O `Index()` método exibe o `Index` exibição. Esse método passa todos os comentários do cliente anterior para o `Index` exibição recuperando os comentários do banco de dados (usando uma consulta LINQ to SQL).

O `Create()` método cria um novo item de comentários e o adiciona ao banco de dados. A mensagem de que o cliente insere o formulário é passada para o `Create()` método no parâmetro de mensagem. Um item de comentários é criado e a mensagem é atribuída para o item de comentário `Message` propriedade. O item de comentário é enviado para o banco de dados com o `DataContext.SubmitChanges()` chamada de método. Por fim, o visitante é redirecionado para o `Index` exibição onde todos os comentários são exibidos.

O `Index` exibição está contida na listagem 2.

**A listagem 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

O `Index` exibição tem duas seções. A seção superior contém o formulário de comentários do cliente real. A seção inferior contém um loop For... Cada loop que percorre todos os itens de comentários do cliente anterior e exibe as propriedades de mensagem e EntryDate para cada item de comentário.

O site de comentários do cliente é um site simples. Infelizmente, o site está aberto a ataques de injeção de JavaScript.

Imagine que você insira o seguinte texto no formulário de comentários do cliente:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Esse texto representa um script de JavaScript que exibe uma caixa de mensagem de alerta. Depois que alguém envia esse script em comentários do formulário, a mensagem <em>surpresas!</em> será exibida sempre que alguém visita o site de comentários do cliente no futuro (consulte a Figura 2).


[![Injeção de JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figura 02**: injeção de JavaScript ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image6.png))


Agora, sua resposta inicial a ataques de injeção de JavaScript pode ser apatia. Você pode achar que os ataques de injeção de JavaScript são simplesmente um tipo de *desfiguração* ataque. Você pode achar que ninguém pode fazer qualquer coisa realmente nocivo ao confirmar um ataque de injeção de JavaScript.

Infelizmente, um hacker pode fazer alguns realmente, coisas realmente nocivo injetando JavaScript em um site. Você pode usar um ataque de injeção de JavaScript para executar um ataque de scripts entre sites (XSS). Em um ataque de scripts entre sites, roubar informações confidenciais do usuário e enviar as informações para outro site.

Por exemplo, um hacker pode usar um ataque de injeção de JavaScript para roubar os valores de cookies do navegador de outros usuários. Se informações confidenciais — como senhas, números de cartão de crédito ou números de previdência social – são armazenadas nos cookies do navegador, um hacker pode usar um ataque de injeção de JavaScript para roubar informações. Ou, se um usuário insere informações confidenciais em um campo de formulário contido em uma página que foi comprometida com um ataque de JavaScript, o hacker pode usar o JavaScript injetado para capturar os dados de formulário e enviá-lo para outro site.

*Esteja assustados*. Levar a ataques de injeção de JavaScript sério e proteger informações confidenciais do usuário. Nas próximas duas seções, discutiremos duas técnicas que você pode usar para proteger seus aplicativos ASP.NET MVC contra ataques de injeção de JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Abordagem 1 #: Codificação de HTML no modo de exibição

Um método fácil de evitar ataques de injeção de JavaScript é HTML codificar todos os dados inseridos por usuários do site quando você exibir novamente os dados em uma exibição. A atualização `Index` exibição na listagem 3 segue essa abordagem.

**A listagem 3 – `Index.aspx` (codificada em HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Observe que o valor de `feedback.Message` é HTML codificado antes que o valor é exibido com o código a seguir:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

O que significa para HTML codificar uma cadeia de caracteres? Quando você HTML codifica uma cadeia de caracteres, perigosas caracteres como `<` e `>` são substituídos por referências de entidade HTML como `&lt;` e `&gt;`. Portanto, quando a cadeia de caracteres `<script>alert("Boo!")</script>` é HTML codificado, ele é convertido em `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. A cadeia de caracteres codificada não executa como um script de JavaScript interpretado pelo navegador. Em vez disso, você deve obter a página inofensiva na Figura 3.


[![Ataque de JavaScript desfeito](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figura 03**: desfeito ataque de JavaScript ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image9.png))


Observe que o `Index` exibir na listagem 3 apenas o valor de `feedback.Message` é codificado. O valor de `feedback.EntryDate` não é codificado. Você só precisa codificar dados inseridos por um usuário. Como o valor de EntryDate foi gerado no controlador, você não precisa para HTML codifica esse valor.

## <a name="approach-2-html-encode-in-the-controller"></a>Abordagem #2: Codificação de HTML no controlador

Em vez de codificação de dados quando você exibir os dados em um modo de exibição HTML, você pode HTML codificar os dados antes de enviar os dados para o banco de dados. Essa segunda abordagem é realizada no caso do `controller` na listagem 4.

**A listagem 4 – `HomeController.cs` (codificada em HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Observe que o valor da mensagem é HTML codificado antes que o valor é enviado para o banco de dados dentro de `Create()` ação. Quando a mensagem é exibida novamente no modo de exibição, a mensagem é codificada em HTML e qualquer JavaScript injetado na mensagem não será executado.

Normalmente, você deve preferir a primeira abordagem, discutida neste tutorial sobre essa segunda abordagem. O problema com essa segunda abordagem é que você terminar com dados codificada em HTML no banco de dados. Em outras palavras, os dados do banco de dados é sujas com engraçadas caracteres.

Por que isso é ruim? Se você precisar exibir os dados do banco de dados em algo diferente de uma página da web, você terá problemas. Por exemplo, você poderá exibir não são mais facilmente os dados em um aplicativo do Windows Forms.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era assustar você sobre a possibilidade de um ataque de injeção de JavaScript. Este tutorial discutidas duas abordagens para proteger seus aplicativos ASP.NET MVC contra ataques de injeção de JavaScript: pode ou HTML codificar usuário enviado dados no modo de exibição, ou você podem HTML codificar usuário enviado dados no controlador.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-windows-authentication-vb.md)
