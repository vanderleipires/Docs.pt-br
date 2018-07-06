---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Páginas finais, tratamento de exceção e conclusão | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 8 adiciona uma página de contato, sobre a página e a exceção...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 4fb0147a5cd8621e5341e4995960adf2f00fffc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804711"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: Páginas finais, tratamento de exceção e conclusão
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 8 adiciona uma página de contato, sobre a página e manipulação de exceção. Isso é a conclusão da série.


## <a id="_Toc260221680"></a>  Entre em contato com a página (enviar e-mail do ASP.NET)

Criar uma nova página chamada ContactUs.aspx

Usando o designer, crie o seguinte formulário anotar especial para incluir o ToolkitScriptManager e o controle de Editor do AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Clique duas vezes no botão "Enviar" para gerar um manipulador de eventos no arquivo code-behind e implementar um método para enviar as informações de contato como um email.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Esse código requer que seu arquivo Web. config contém uma entrada na seção de configuração que especifica o servidor SMTP a ser usado para enviar um email.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Sobre a página

Crie uma página chamada AboutUs.aspx e adicione qualquer conteúdo que você deseja.

## <a id="_Toc260221682"></a>  Manipulador de exceção global

Por fim, em todo o aplicativo, podemos ter lançada exceções e há circunstâncias imprevistas ou cold também causa sem tratamento de exceções em nosso aplicativo web.

Queremos que nunca uma exceção sem tratamento a ser exibida para um visitante de site da web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Além de ser uma experiência de usuário terrível exceções sem tratamento também podem ser um problema de segurança.

Para resolver esse problema, podemos irá implementar um manipulador de exceção global.

Para fazer isso, abra o arquivo global. asax e observe o seguinte manipulador de eventos gerados previamente.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Adicione código para implementar o aplicativo\_manipulador de erro da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Em seguida, adicione uma página chamada Error.aspx à solução e adicione esse trecho de marcação.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Agora, na página\_carregar extração de manipulador de eventos, as mensagens de erro do objeto de solicitação.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Conclusão

Já vimos que WebForms ASP.NET torna mais fácil para criar um site da Web sofisticado com acesso de banco de dados, associação, AJAX, etc. muito rapidamente.

Espero que este tutorial tenha fornecido as ferramentas que você precisa para começar a criar seu próprio ASP.NET WebForms aplicativos!

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-7.md)
