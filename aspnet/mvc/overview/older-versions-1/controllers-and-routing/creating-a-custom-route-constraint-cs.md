---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Criar uma restrição de rota personalizada (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demonstra como você pode criar uma restrição de rota personalizados. Implementamos um simples restrição personalizada que impede que uma rota que está sendo correspondido w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: a77a25672c94d6b706af0cc36807b11297bc573a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399997"
---
<a name="creating-a-custom-route-constraint-c"></a>Criar uma restrição de rota personalizada (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther demonstra como você pode criar uma restrição de rota personalizados. Implementamos uma restrição personalizada simple que impede que uma rota que está sendo correspondido quando é feita uma solicitação de navegador de um computador remoto.


O objetivo deste tutorial é demonstrar como você pode criar uma restrição de rota personalizados. Uma restrição de rota personalizada permite impedir que uma rota que está sendo correspondido a menos que alguma condição personalizada é correspondida.

Neste tutorial, podemos criar uma restrição de rota do Localhost. A restrição de rota Localhost corresponde apenas a solicitações feitas do computador local. Não correspondem às solicitações remotas pela Internet.

Você pode implementar uma restrição de rota personalizada, Implementando a interface IRouteConstraint. Esta é uma interface muito simple que descreve um único método:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

O método retorna um valor booliano. Se você retornar false, a rota associada com a restrição não corresponde a solicitação do navegador.

A restrição de Localhost está contida na listagem 1.

**Listagem 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

A restrição na listagem 1 tira proveito da propriedade IsLocal exposto pela classe HttpRequest. Essa propriedade retorna true quando o endereço IP da solicitação é qualquer 127.0.0.1 ou quando o IP da solicitação é o mesmo endereço IP do servidor.

Você usar uma restrição personalizada dentro de uma rota definida no arquivo global. asax. O arquivo global asax na listagem 2 usa a restrição de Localhost para evitar que qualquer pessoa que solicite uma página de administração, a menos que eles fazem a solicitação do servidor local. Por exemplo, uma solicitação para /Admin/DeleteAll falhará quando feita de um servidor remoto.

**Listagem 2 - global. asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

A restrição de Localhost é usada na definição de rota de administrador. Essa rota não ser correspondida por uma solicitação remota do navegador. No entanto, observe que outras rotas definidas em global. asax podem corresponder a mesma solicitação. É importante entender que uma restrição impede que uma rota específica de correspondência de uma solicitação e não todas as rotas definidas no arquivo global. asax.

Observe que a rota padrão foi comentada do arquivo global. asax na listagem 2. Se você incluir a rota padrão, a rota padrão corresponderia solicitações para o controlador de administrador. Nesse caso, os usuários remotos ainda pode invocar ações do controlador de administrador, mesmo que suas solicitações não correspondem à rota Admin.

> [!div class="step-by-step"]
> [Anterior](creating-a-route-constraint-cs.md)
> [Próximo](asp-net-mvc-controller-overview-vb.md)
