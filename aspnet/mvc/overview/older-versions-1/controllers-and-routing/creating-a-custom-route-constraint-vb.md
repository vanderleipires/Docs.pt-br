---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Criar uma restrição de rota personalizados (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demonstra como você pode criar uma restrição de rota personalizados. Implementamos um simples personalizada restrição que impede que uma rota correspondente w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 094077fa0cb546f4cc91dbf074f8014e62b3b19c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-vb"></a>Criar uma restrição de rota personalizados (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther demonstra como você pode criar uma restrição de rota personalizados. Implementamos uma restrição personalizada simple que impede que uma rota que está sendo correspondido quando é feita uma solicitação do navegador de um computador remoto.


O objetivo deste tutorial é demonstrar como você pode criar uma restrição de rota personalizados. Uma restrição de rota personalizados permite impedir que uma rota sendo correspondido a menos que alguma condição personalizada for correspondida.

Neste tutorial, podemos criar uma restrição de rota Localhost. A restrição de rota Localhost corresponde apenas a solicitações feitas a partir do computador local. Solicitações remotas de através da Internet não são correspondentes.

Você pode implementar uma restrição de rota personalizados Implementando a interface IRouteConstraint. Esta é uma interface muito simple que descreve um único método:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

O método retorna um valor booliano. Se você retornar False, a rota associada com a restrição não corresponderá a solicitação do navegador.

A restrição Localhost está contida na listagem 1.

**Listando 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

A restrição na listagem 1 tira proveito da propriedade IsLocal exposto pela classe HttpRequest. Essa propriedade retorna true quando o endereço IP da solicitação é o 127.0.0.1 ou quando o IP da solicitação é o mesmo endereço IP do servidor.

Você usar uma restrição personalizada dentro de uma rota definida no arquivo global. asax. O arquivo global. asax na listagem 2 usa a restrição Localhost para impedir que qualquer pessoa que solicita uma página de administração, a menos que eles fazer a solicitação do servidor local. Por exemplo, uma solicitação para /Admin/DeleteAll falhará quando feita de um servidor remoto.

**Listing 2 - Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

A restrição de Localhost é usada na definição da rota Admin. Essa rota não seja correspondida por uma solicitação do navegador remoto. No entanto, observe que outras rotas definidas em global. asax podem corresponder a mesma solicitação. É importante entender que uma restrição impede que uma rota específica de correspondência de uma solicitação e não todas as rotas definidas no arquivo global. asax.

Observe que a rota padrão foi comentada do arquivo global. asax na listagem 2. Se você incluir a rota padrão, a rota padrão corresponderia solicitações para o controlador de administrador. Nesse caso, os usuários remotos ainda podem invocar ações do controlador Admin, embora suas solicitações não correspondem à rota de administrador.

> [!div class="step-by-step"]
> [Anterior](creating-a-route-constraint-vb.md)
