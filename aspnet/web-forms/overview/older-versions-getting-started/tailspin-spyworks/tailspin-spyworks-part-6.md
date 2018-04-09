---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: Associação do ASP.NET | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 6 adiciona a associação do ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a>Parte 6: Associação do ASP.NET
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 6 adiciona a associação do ASP.NET.


## <a id="_Toc260221672"></a>  Trabalhando com a associação do ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Clique em segurança

![](tailspin-spyworks-part-6/_static/image1.jpg)

Certifique-se de que estamos usando autenticação de formulários.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Use o link "Create User" para criar alguns usuários.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Quando terminar, consulte a janela Gerenciador de soluções e atualizar a exibição.

![](tailspin-spyworks-part-6/_static/image2.png)

Observe que o ASPNETDB. MDF BOM foi criado. Esse arquivo contém as tabelas para dar suporte os serviços do ASP.NET core como associação.

Agora podemos começar a implementar o processo de check-out.

Comece criando uma página de CheckOut.aspx.

A página CheckOut.aspx deve estar disponível para usuários que estão conectados para nós serão restringir o acesso ao registrado em usuários e redireciona os usuários que não estão conectados a página de logon.

Para fazer isso, adicionaremos o seguinte à seção de configuração de nosso arquivo Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

O modelo para aplicativos ASP.NET Web Forms automaticamente adicionada uma seção de autenticação para nosso arquivo Web. config e estabelecer a página de logon padrão.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Podemos deve modificar aspx arquivo code-behind para migrar um carrinho de compras anônimo quando o usuário fizer logon. Altere a página\_evento de carregamento da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Em seguida, adicione um manipulador de eventos de "Logon efetuado" como esta para definir o nome da sessão para o usuário conectado recentemente e alterar a id de sessão temporária no carrinho de compras para que o usuário chamando o método MigrateCart em nossa classe MyShoppingCart. (Implementado no arquivo. cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementar o método MigrateCart() assim.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Checkout.aspx, usaremos um EntityDataSource e GridView em nossa página de confirmação que fizemos em nossa página de carrinho de compras.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Observe que o nosso controle GridView Especifica um manipulador de eventos "ondatabound" denominado MyList\_RowDataBound Vamos implementar o manipulador de eventos como este.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Isso mantém método total de execução das compras carrinho conforme cada linha está associada e atualiza a linha inferior de GridView.

Neste estágio, implementamos uma apresentação "revisão" do pedido será colocado.

Vamos lidar com um cenário de carrinho vazio adicionando algumas linhas de código para nossa página\_evento de carregamento:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Quando o usuário clica no botão "Enviar", executará o código a seguir no manipulador de eventos de clique de botão Enviar.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

É a parte "principal" do processo de envio da ordem a ser implementada no método SubmitOrder() de nossa classe MyShoppingCart.

SubmitOrder será:

- Coloque todos os itens de linha no carrinho de compras e usá-las para criar um novo registro de ordem e os registros de OrderDetails associados.
- Calcule a data de envio.
- Limpe o carrinho de compras.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Para os fins desse aplicativo de exemplo irá calcular uma data de remessa simplesmente adicionando dois dias para a data atual.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Executando o aplicativo agora também permite que nós para testar o processo de compra do início ao fim.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-5.md)
> [Próximo](tailspin-spyworks-part-7.md)
