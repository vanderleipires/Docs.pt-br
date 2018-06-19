---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Criar rotas personalizadas (VB) | Microsoft Docs
author: microsoft
description: Saiba como adicionar as rotas personalizadas para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como modificar a tabela de rota padrão no arquivo global. asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: e827725ab7ce54c86ae9f4193d0a8a7ef4af8512
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872319"
---
<a name="creating-custom-routes-vb"></a>Criar rotas personalizadas (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como adicionar as rotas personalizadas para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como modificar a tabela de rota padrão no arquivo global. asax.


Neste tutorial, você aprenderá como adicionar uma rota personalizada a um aplicativo ASP.NET MVC. Você aprenderá a modificar a tabela de rota padrão no arquivo global. asax com uma rota personalizada.

Em aplicativos ASP.NET MVC, a tabela de rota padrão funcionará bem. No entanto, você pode descobrir que você tiver um especializado necessidades de roteamentos. Nesse caso, você pode criar uma rota personalizada.

Por exemplo, imagine que você esteja criando um aplicativo de blog. Você talvez queira manipular solicitações de entrada que ter esta aparência:

/ Arquivamento/12-25-2009

Quando um usuário insere essa solicitação, você quiser retornar a entrada do blog que corresponde à data de 25/12/2009. Para lidar com esse tipo de solicitação, você precisa criar uma rota personalizada.

O arquivo global. asax na listagem 1 contém uma nova rota personalizada, chamada de Blog, que trata as solicitações que parecem /Archive/*data de entrada*.

**Listando 1 - global. asax (com rota personalizados)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

A ordem das rotas que você adicionar à tabela de rotas é importante. Nossa nova rota Blog personalizada é adicionada antes da rota padrão existente. Se você a ordem inversa, em seguida, a rota padrão sempre será chamada em vez de rota personalizados.

Personalizado Blog corresponde a qualquer solicitação que começa com/arquivamento. Portanto, ele corresponde a todas as URLs a seguir:

- / Arquivamento/12-25-2009

- / Arquivamento/10-6-2004

- / Arquivamento/apple

A rota personalizada a solicitação de entrada é mapeado para um controlador chamado arquivamento e invoca a ação Entry(). Quando o método Entry() é chamado, a data de entrada é passada como um parâmetro denominado entryDate.

Você pode usar a rota de Blog personalizada com o controlador na listagem 2.

**A listagem 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Observe que o método Entry() na listagem 2 aceita um parâmetro do tipo DateTime. A estrutura MVC é inteligente o suficiente para converter a data de entrada da URL em um valor DateTime automaticamente. Se o parâmetro de data de entrada da URL não pode ser convertido em uma data e hora, ocorrerá um erro (consulte a Figura 1).

**Figura 1 - erro de conversão de parâmetro**


[![A caixa de diálogo Novo projeto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Figura 01**: erro de conversão de parâmetro ([clique para exibir a imagem em tamanho normal](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Resumo

O objetivo deste tutorial era demonstrar como você pode criar uma rota personalizada. Você aprendeu como adicionar uma rota personalizada para a tabela de rota no arquivo global asax que representa as entradas do blog. Discutimos como mapear solicitações de entradas do blog para um controlador chamado ArchiveController e uma ação de controlador chamada Entry().

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-controller-overview-vb.md)
> [Próximo](creating-a-route-constraint-vb.md)
