---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Páginas finais, tratamento de exceção e conclusão | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 8 adiciona uma página de contato, sobre a página e a exceção...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: f82294aab0616012393cf3e10f932f6d1ad0cdb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886265"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: Páginas finais, tratamento de exceção e conclusão
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET. Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 8 adiciona uma página de contato, sobre a página e manipulação de exceção. Este é o final da série.


## <a id="_Toc260221680"></a>  Entre em contato com a página (envio de email do ASP.NET)

Criar uma nova página chamada ContactUs.aspx

Usando o designer, crie a seguinte forma anotar especial para incluir o ToolkitScriptManager e o controle de Editor do AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Clique duas vezes no botão "Enviar" para gerar um manipulador de eventos no arquivo code-behind e implementar um método para enviar as informações de contato como um email.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Esse código requer que seu arquivo Web. config contém uma entrada na seção de configuração que especifica o servidor SMTP a ser usado para enviar email.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Sobre a página

Crie uma página chamada AboutUs.aspx e adicione qualquer conteúdo que você deseja.

## <a id="_Toc260221682"></a>  Manipulador de exceção global

Por fim, em todo o aplicativo investiram exceções e há circunstâncias imprevistas ou cold também causa sem tratamento de exceções em nosso aplicativo web.

Queremos nunca uma exceção sem tratamento a ser exibida para um visitante de site da web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Além de ser uma experiência de usuário terríveis exceções sem tratamento também podem ser um problema de segurança.

Para resolver esse problema Vamos implementar um manipulador de exceção global.

Para fazer isso, abra o arquivo global. asax e observe o seguinte manipulador de eventos gerados previamente.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Adicione código para implementar o aplicativo\_manipulador de erros da seguinte maneira.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Em seguida, adicione uma página chamada Error.aspx para a solução e adicione este trecho de marcação.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Agora, na página\_carregar extração do manipulador de eventos mensagens de erro do objeto de solicitação.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Conclusão

Já vimos que ASP.NET WebForms torna mais fácil para criar um site sofisticado com acesso de banco de dados, associação, AJAX, etc. muito rapidamente.

Esperamos que este tutorial lhe forneceu as ferramentas que você precisa para começar a criar seus próprios formulários da Web do ASP.NET em aplicativos!

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-7.md)
