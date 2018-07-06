---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Criando uma ação (VB) | Microsoft Docs
author: microsoft
description: Saiba como adicionar uma nova ação para um controlador ASP.NET MVC. Saiba mais sobre os requisitos para um método a ser uma ação.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 6ec69f5aa9f7a789cb533a8dc04dca952adf35d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824040"
---
<a name="creating-an-action-vb"></a>Criando uma ação (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como adicionar uma nova ação para um controlador ASP.NET MVC. Saiba mais sobre os requisitos para um método a ser uma ação.


O objetivo deste tutorial é explicar como você pode criar uma nova ação de controlador. Você aprenderá sobre os requisitos de um método de ação. Você também aprenderá a impedir que um método que está sendo exposto como uma ação.

## <a name="adding-an-action-to-a-controller"></a>Adicionando uma ação a um controlador

Adicionar uma nova ação para um controlador, adicionando um novo método ao controlador. Por exemplo, o controlador na listagem 1 contém uma ação chamada index () e uma ação chamada sayHello (). Ambos os métodos são expostos como ações.

**Listagem 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

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

Se você precisa criar um método público em uma classe de controlador e você não deseja expor o método como uma ação do controlador, você pode impedir que o método que está sendo invocado usando o &lt;NonAction&gt; atributo. Por exemplo, o controlador na listagem 2 contém um método público chamado CompanySecrets() é decorado com o &lt;NonAction&gt; atributo.

**Listagem 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Se você tentar invocar a ação do controlador CompanySecrets() digitando /Work/CompanySecrets na barra de endereços do navegador, em seguida, você obterá a mensagem de erro na Figura 1.


[![Invocando um método NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Figura 01**: invocando um método NonAction ([clique para exibir a imagem em tamanho normal](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-controller-vb.md)
> [Próximo](aspnet-mvc-controllers-overview-cs.md)
