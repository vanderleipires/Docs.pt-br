---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Associação do ASP.NET | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 6 adiciona a associação do ASP.NET.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 303c1edf548db1da9ef61d94bdd8157d6afbb2e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804414"
---
<a name="part-6-aspnet-membership"></a>Parte 6: Associação do ASP.NET
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 6 adiciona a associação do ASP.NET.


## <a id="_Toc260221672"></a>  Trabalhando com a associação do ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Clique em segurança

![](tailspin-spyworks-part-6/_static/image1.jpg)

Certifique-se de que estamos usando autenticação de formulários.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Use o link "Criar usuário" para criar alguns usuários.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Quando terminar, consulte a janela Gerenciador de soluções e atualize a exibição.

![](tailspin-spyworks-part-6/_static/image2.png)

Observe que o ASPNETDB. Arquivos MDF bem foi criado. Esse arquivo contém as tabelas para dar suporte aos serviços do ASP.NET core, como a associação.

Agora podemos começar a implementar o processo de check-out.

Comece criando uma página CheckOut.aspx.

A página CheckOut.aspx deve estar disponível para usuários que estão conectados no, portanto, nós iremos restringir acesso a registrado em usuários e redireciona os usuários que não estão conectados à página de logon.

Para fazer isso, adicionaremos o seguinte à seção de configuração de nosso arquivo Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

O modelo para aplicativos ASP.NET Web Forms automaticamente adicionado a uma seção de autenticação ao nosso arquivo Web. config e estabelecida a página de logon padrão.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Podemos deve modificar o login. aspx arquivo code-behind para migrar um carrinho de compras anônimo quando o usuário fizer logon. Alterar a página\_evento de carregamento da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Em seguida, adicione um manipulador de eventos de "Logon efetuado" como este para definir o nome de sessão para o usuário conectado recentemente e altere a id de sessão temporária no carrinho de compras para o usuário chamando o método MigrateCart em nossa classe MyShoppingCart. (Implementado no arquivo. cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementa o método MigrateCart() como esse.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Checkout.aspx, usaremos um EntityDataSource e GridView em nossa página de confirmação que fizemos em nossa página de carrinho de compras.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Observe que o nosso controle GridView Especifica um manipulador de eventos de "ondatabound" chamado MyList\_RowDataBound Vamos implementar o manipulador de eventos como este.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Isso mantém método total de execução de compras do carrinho conforme cada linha está associada e atualiza a linha inferior de GridView.

Neste estágio, implementamos uma apresentação "review" do pedido a ser colocado.

Vamos lidar com um cenário de carrinho vazio adicionando algumas linhas de código à nossa página\_evento Load:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Quando o usuário clica no botão "Enviar" Estamos executará o código a seguir no manipulador de eventos de clique de botão Enviar.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

"Cerne" do processo de envio de pedido é para ser implementada no método SubmitOrder() de nossa classe MyShoppingCart.

SubmitOrder será:

- Pegar todos os itens de linha no carrinho de compras e usá-los para criar um novo registro de ordem e os registros de OrderDetails associados.
- Calcule a data de envio.
- Desmarque o carrinho de compras.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Para os fins deste aplicativo de exemplo calcularemos uma data de remessa simplesmente adicionando dois dias à data atual.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Executando o aplicativo agora permitirá que nós para testar o processo de compra do início ao fim.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-5.md)
> [Próximo](tailspin-spyworks-part-7.md)
