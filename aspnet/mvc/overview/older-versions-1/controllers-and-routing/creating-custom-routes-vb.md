---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Criação de rotas personalizadas (VB) | Microsoft Docs
author: microsoft
description: Saiba como adicionar rotas personalizadas para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como modificar a tabela de rotas padrão no arquivo global. asax.
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d6125f93d99acc695be319bd000ff65ab4a1159
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829694"
---
<a name="creating-custom-routes-vb"></a>Criação de rotas personalizadas (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Saiba como adicionar rotas personalizadas para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como modificar a tabela de rotas padrão no arquivo global. asax.


Neste tutorial, você aprenderá como adicionar uma rota personalizada a um aplicativo ASP.NET MVC. Você aprenderá a modificar a tabela de rotas padrão no arquivo global. asax com uma rota personalizada.

Em aplicativos ASP.NET MVC, a tabela de rotas padrão funcionará bem. No entanto, talvez você descubra que você tiver um roteamentos necessidades especializado. Nesse caso, você pode criar uma rota personalizada.

Por exemplo, imagine que você esteja criando um aplicativo de blog. Você talvez queira manipular solicitações de entrada que ter esta aparência:

/ Arquivamento/12-25-2009

Quando um usuário insere essa solicitação, você deseja retornar a entrada de blog que corresponde à data 12/25/2009. Para lidar com esse tipo de solicitação, você precisa criar uma rota personalizada.

O arquivo global asax na listagem 1 contém uma nova rota personalizada, chamada de Blog, que trata as solicitações que se parecem com /Archive/*data de entrada*.

**Listagem 1 - global. asax (com rota personalizada)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

A ordem das rotas que você adicionar à tabela de rotas é importante. Nossa nova rota Blog personalizada é adicionada antes da rota padrão existente. Se você invertemos a ordem, em seguida, a rota padrão sempre será chamada em vez de na rota personalizada.

A rota de Blog personalizada corresponde a qualquer solicitação que começa com/arquivamento /. Assim, ele corresponde a todas as URLs a seguir:

- / Arquivamento/12-25-2009

- / Arquivamento/10 6 de 2004

- / Arquivamento/apple

A rota personalizada mapeia a solicitação de entrada para um controlador chamado arquivo morto e invoca a ação Entry(). Quando o método Entry() é chamado, a data de entrada é passada como um parâmetro chamado entryDate.

Você pode usar a rota personalizada do Blog com o controlador na listagem 2.

**Listagem 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Observe que o método Entry() na listagem 2 aceita um parâmetro do tipo DateTime. A estrutura do MVC é inteligente o suficiente para converter a data de entrada da URL em um valor DateTime automaticamente. Se o parâmetro de data de entrada da URL não pode ser convertido em uma data e hora, ocorrerá um erro (consulte a Figura 1).

**Figura 1 – erro de conversão de parâmetro**


[![A caixa de diálogo Novo projeto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Figura 01**: erro de conversão de parâmetro ([clique para exibir a imagem em tamanho normal](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Resumo

O objetivo deste tutorial era demonstrar como você pode criar uma rota personalizada. Você aprendeu como adicionar uma rota personalizada à tabela de rotas no arquivo global. asax que representa as entradas de blog. Discutimos como mapear solicitações para entradas de blog para um controlador chamado ArchiveController e uma ação de controlador chamada Entry().

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-controller-overview-vb.md)
> [Próximo](creating-a-route-constraint-vb.md)
