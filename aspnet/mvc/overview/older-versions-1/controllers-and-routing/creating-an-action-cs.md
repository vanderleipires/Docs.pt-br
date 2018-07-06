---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Criando uma ação (c#) | Microsoft Docs
author: microsoft
description: Saiba como adicionar uma nova ação para um controlador ASP.NET MVC. Saiba mais sobre os requisitos para um método a ser uma ação.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: bc85e1439bfa0a89d60271dda53829e921b26f55
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812219"
---
<a name="creating-an-action-c"></a>Criando uma ação (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como adicionar uma nova ação para um controlador ASP.NET MVC. Saiba mais sobre os requisitos para um método a ser uma ação.


O objetivo deste tutorial é explicar como você pode criar uma nova ação de controlador. Você aprenderá sobre os requisitos de um método de ação. Você também aprenderá a impedir que um método que está sendo exposto como uma ação.

## <a name="adding-an-action-to-a-controller"></a>Adicionando uma ação a um controlador

Adicionar uma nova ação para um controlador, adicionando um novo método ao controlador. Por exemplo, o controlador na listagem 1 contém uma ação chamada index () e uma ação chamada sayHello (). Ambos os métodos são expostos como ações.

**Listagem 1 - Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Para serem expostos para o universo como uma ação, um método deve atender a certos requisitos:

- O método deve ser público.
- O método não pode ser um método estático.
- O método não pode ser um método de extensão.
- O método não pode ser um construtor, getter ou setter.
- O método não pode ter tipos genéricos abertos.
- O método não é um método da classe base do controlador.
- O método não pode conter **ref** ou **out** parâmetros.

Observe que não há nenhuma restrição sobre o tipo de retorno de uma ação do controlador. Uma ação do controlador pode retornar uma cadeia de caracteres, uma data e hora, uma instância da classe Random, ou nulo. O ASP.NET MVC framework converterá qualquer tipo de retorno que não seja o resultado de uma ação em uma cadeia de caracteres e processar a cadeia de caracteres para o navegador.

Quando você adiciona qualquer método que não viole a esses requisitos para um controlador, o método é exposto como uma ação do controlador. Tenha cuidado aqui. Uma ação do controlador pode ser chamada por qualquer pessoa conectado à Internet. Não, por exemplo, crie uma ação do controlador DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impedir que um método público que está sendo invocado

Se você precisar criar um método público em uma classe de controlador e você não deseja expor o método como uma ação do controlador, em seguida, você pode impedir que o método que está sendo invocado usando o atributo [NonAction]. Por exemplo, o controlador na listagem 2 contém um método público chamado CompanySecrets() é decorado com o atributo [NonAction].

**Listagem 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Se você tentar invocar a ação do controlador CompanySecrets() digitando /Work/CompanySecrets na barra de endereços do navegador, em seguida, você obterá a mensagem de erro na Figura 1.


[![Invocando um método NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figura 01**: invocando um método NonAction ([clique para exibir a imagem em tamanho normal](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-controller-cs.md)
> [Próximo](asp-net-mvc-routing-overview-vb.md)
