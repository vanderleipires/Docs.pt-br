---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: "Criando uma ação (c#) | Microsoft Docs"
author: microsoft
description: "Saiba como adicionar uma nova ação a um controlador MVC do ASP.NET. Saiba mais sobre os requisitos para um método de uma ação."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b751dc7e34951be33e7c27a3429c383a3e1e1c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-c"></a>Criando uma ação (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como adicionar uma nova ação a um controlador MVC do ASP.NET. Saiba mais sobre os requisitos para um método de uma ação.


O objetivo deste tutorial é explicar como você pode criar uma nova ação de controlador. Você aprenderá sobre os requisitos de um método de ação. Você também aprenderá como impedir que um método que está sendo exposto como uma ação.

## <a name="adding-an-action-to-a-controller"></a>Adicionar uma ação a um controlador

Adicionar uma nova ação a um controlador, adicionando um novo método para o controlador. Por exemplo, o controlador na listagem 1 contém uma ação chamada index () e uma ação chamada sayHello (). Ambos os métodos são expostos como ações.

**Listando 1 - Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Para ser exposto ao universo como uma ação, um método deve atender a certos requisitos:

- O método deve ser público.
- O método não pode ser um método estático.
- O método não pode ser um método de extensão.
- O método não pode ser um construtor, getter ou setter.
- O método não pode ter abertos tipos genéricos.
- O método não é um método da classe base do controlador.
- O método não pode conter **ref** ou **out** parâmetros.

Observe que não existem restrições no tipo de retorno de uma ação do controlador. Uma ação do controlador pode retornar uma cadeia de caracteres, uma data e hora, uma instância da classe Random, nulo ou vazio. A estrutura ASP.NET MVC converterá qualquer tipo de retorno não é o resultado de uma ação em uma cadeia de caracteres e processar a cadeia de caracteres para o navegador.

Quando você adicionar qualquer método que não viole a esses requisitos para um controlador, o método é exposto como uma ação do controlador. Tenha cuidado aqui. Uma ação do controlador pode ser chamada por qualquer usuário conectado à Internet. Não, por exemplo, crie uma ação do controlador DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impedindo que um método público que está sendo invocado

Se você precisa criar um método público em uma classe de controlador e você não deseja expor o método como uma ação do controlador, em seguida, você pode impedir que o método que está sendo invocado usando o atributo [NonAction]. Por exemplo, o controlador na listagem 2 contém um método público chamado CompanySecrets() decorada com o atributo [NonAction].

**A listagem 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Se você tentar chamar a ação de controlador CompanySecrets() digitando /Work/CompanySecrets na barra de endereços do navegador, em seguida, você receberá a mensagem de erro na Figura 1.


[![Invocando um método NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figura 01**: invocando um método NonAction ([clique para exibir a imagem em tamanho normal](creating-an-action-cs/_static/image2.png))

>[!div class="step-by-step"]
[Anterior](creating-a-controller-cs.md)
[Próximo](asp-net-mvc-routing-overview-vb.md)
