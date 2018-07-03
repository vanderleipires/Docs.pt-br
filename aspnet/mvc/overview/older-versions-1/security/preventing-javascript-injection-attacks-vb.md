---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Impedindo ataques de injeção de JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Impedir ataques de injeção de JavaScript e ataques de scripts entre sites está acontecendo para você. Neste tutorial, Stephen Walther explica como você pode facilmente de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c022847462239624d5b84816999918ec77f8337
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402335"
---
<a name="preventing-javascript-injection-attacks-vb"></a>Impedindo ataques de injeção de JavaScript (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Baixar PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Impedir ataques de injeção de JavaScript e ataques de scripts entre sites está acontecendo para você. Neste tutorial, Stephen Walther explica como você pode facilmente anular esses tipos de ataques por seu conteúdo de codificação HTML.


O objetivo deste tutorial é explicar como você pode impedir ataques de injeção de JavaScript em aplicativos ASP.NET MVC. Este tutorial aborda duas abordagens para proteger seu site contra um ataque de injeção de JavaScript. Você aprenderá a evitar ataques de injeção de JavaScript codificação dos dados que você exibir. Você também aprenderá a evitar ataques de injeção de JavaScript codificação dos dados que você aceite.

## <a name="what-is-a-javascript-injection-attack"></a>O que é um ataque de injeção de JavaScript?

Sempre que você aceita a entrada do usuário e exibir novamente a entrada do usuário, você pode abrir seu site a ataques de injeção de JavaScript. Vamos examinar um aplicativo concreto que está aberto a ataques de injeção de JavaScript.

Imagine que você tenha criado um site de comentários do cliente (veja a Figura 1). Os clientes podem visitar o site e inserir comentários em sua experiência usando seus produtos. Quando um cliente envia seus comentários, comentários é exibida novamente na página de comentários.


[![Site de comentários do cliente](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figura 01**: o site de comentários do cliente ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image3.png))


O site de comentários do cliente usa o `controller` na listagem 1. Isso `controller` contém duas ações denominadas `Index()` e `Create()`.

**Listagem 1 – `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

O `Index()` método exibe a `Index` modo de exibição. Esse método passa todos os comentários do cliente anterior para o `Index` exibição, recuperando os comentários do banco de dados (usando uma consulta LINQ to SQL).

O `Create()` método cria um novo item de comentários e o adiciona ao banco de dados. A mensagem que o cliente insere no formulário é passada para o `Create()` método no parâmetro de mensagem. Um item de comentário é criado e é atribuída à mensagem para o item de comentário `Message` propriedade. O item de comentário é enviado para o banco de dados com o `DataContext.SubmitChanges()` chamada de método. Por fim, o visitante é redirecionado para o `Index` exibição em que todos os comentários são exibidos.

O `Index` modo de exibição estiver contido na listagem 2.

**Listagem 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

O `Index` exibição tem duas seções. A seção superior contém o formulário de comentários de clientes reais. A seção inferior contém um loop For... Cada loop que executa um loop em todos os itens de comentários do cliente anterior e exibe as propriedades de mensagem e EntryDate para cada item de comentários.

O site de comentários do cliente é um site simples. Infelizmente, o site é aberto a ataques de injeção de JavaScript.

Imagine que você insira o texto a seguir no formulário de comentários do cliente:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Esse texto representa um script JavaScript que exibe uma caixa de mensagem de alerta. Depois que alguém envia esse script em comentários de formulário, a mensagem <em>Boo!</em> será exibida sempre que qualquer pessoa que visita o site de comentários do cliente no futuro (veja a Figura 2).


[![Injeção de JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figura 02**: a injeção de JavaScript ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image6.png))


Agora, sua resposta inicial a ataques de injeção de JavaScript pode ser apatia. Você pode pensar que os ataques de injeção de JavaScript são simplesmente um tipo de *desfiguração* ataque. Você pode achar que ninguém pode fazer algo realmente ruim ao confirmar um ataque de injeção de JavaScript.

Infelizmente, um hacker possa fazer alguns, na verdade, as coisas realmente ruim por injeção de JavaScript em um site. Você pode usar um ataque de injeção de JavaScript para executar um ataque de script entre sites (XSS). Em um ataque de script entre sites, roubar informações confidenciais do usuário e enviar as informações para outro site.

Por exemplo, um hacker pode usar um ataque de injeção de JavaScript para roubar os valores dos cookies do navegador de outros usuários. Se informações confidenciais, como senhas, números de cartão de crédito ou números de previdência social – são armazenadas nos cookies do navegador, um hacker pode usar um ataque de injeção de JavaScript para roubar informações. Ou, se um usuário insere informações confidenciais em um campo de formulário contido em uma página que tenha sido comprometida com um ataque de JavaScript, o hacker pode usar o JavaScript injetado para capturar os dados de formulário e enviá-lo a outro site.

*Por favor, tenha medo*. Levar a ataques de injeção de JavaScript sério e proteger as informações confidenciais do usuário. Nas próximas duas seções, discutimos duas técnicas que você pode usar para se defender contra ataques de injeção de JavaScript aplicativos ASP.NET MVC.

## <a name="approach-1-html-encode-in-the-view"></a>Abordagem 1 #: Codificação de HTML no modo de exibição

Um método fácil de impedir ataques de injeção de JavaScript é HTML codificar todos os dados inseridos pelos usuários do site quando você exibir novamente os dados em um modo de exibição. Atualizado `Index` exibição na listagem 3 segue essa abordagem.

**Listagem 3 – `Index.aspx` (codificado em HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Observe que o valor de `feedback.Message` é codificado em HTML antes que o valor é exibido com o código a seguir:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

O que significa para o HTML codificar uma cadeia de caracteres? Quando o HTML codificar uma cadeia de caracteres, perigoso caracteres, como `<` e `>` são substituídos por referências a entidades, como HTML `&lt;` e `&gt;`. Portanto, quando a cadeia de caracteres `<script>alert("Boo!")</script>` é o HTML codificado, eles serão convertidos em `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. A cadeia de caracteres codificada não executa como um script JavaScript quando interpretada por um navegador. Em vez disso, você obtém a página inofensiva na Figura 3.


[![Ataque de JavaScript derrotada](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figura 03**: derrotada ataque de JavaScript ([clique para exibir a imagem em tamanho normal](preventing-javascript-injection-attacks-vb/_static/image9.png))


Observe que, nos `Index` exibir na listagem 3 apenas o valor de `feedback.Message` é codificado. O valor de `feedback.EntryDate` não é codificado. Você somente precisa codificar dados inseridos por um usuário. Como o valor de EntryDate foi gerado no controlador, você não precisa para HTML codificar esse valor.

## <a name="approach-2-html-encode-in-the-controller"></a>Abordagem #2: Codificação de HTML no controlador

Em vez de codificação de dados quando você exibe os dados em um modo de exibição HTML, é possível que o HTML codificar os dados apenas antes de enviar os dados no banco de dados. Essa segunda abordagem é obtida no caso do `controller` na listagem 4.

**Listagem 4 – `HomeController.cs` (codificado em HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Observe que o valor da mensagem é codificado em HTML antes que o valor é enviado para o banco de dados dentro de `Create()` ação. Quando a mensagem é exibida novamente no modo de exibição, a mensagem é codificada em HTML e qualquer JavaScript injetado na mensagem não é executado.

Normalmente, você deve favorecer a primeira abordagem discutida neste tutorial sobre essa segunda abordagem. O problema com essa segunda abordagem é que você terminar com data de codificado em HTML no seu banco de dados. Em outras palavras, seu banco de dados é sujas com caracteres engraçadas.

Por que isso é ruim? Se você alguma vez precisar exibir os dados do banco de dados em algo diferente de uma página da web, você terá problemas. Por exemplo, você pode exibir não são mais facilmente os dados em um aplicativo Windows Forms.

## <a name="summary"></a>Resumo

A finalidade deste tutorial era assustar você sobre a perspectiva de um ataque de injeção de JavaScript. Este tutorial discutidas duas abordagens para proteger seus aplicativos ASP.NET MVC contra ataques de injeção de JavaScript: qualquer HTML que você pode codificar usuário enviado dados no modo de exibição, ou você podem HTML codificar usuário enviado dados no controlador.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-windows-authentication-vb.md)
