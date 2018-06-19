---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[Como fazer]: manter o estado de um controle de usuário durante um Postback | Microsoft Docs'
author: rick-anderson
description: Este Chris Pels vídeo mostra como manter o estado de um ou mais objetos em um controle de usuário. Primeiro, é criado um controle de usuário que representa o abilit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525965"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Como fazer]: manter o estado de um controle de usuário durante um Postback
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="2279a-105">por [Carlos Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="2279a-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="2279a-106">Este Chris Pels vídeo mostra como manter o estado de um ou mais objetos em um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="2279a-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="2279a-107">Primeiro, é criado um controle de usuário que representa a capacidade de um usuário especificar critérios de filtro de uma pesquisa.</span><span class="sxs-lookup"><span data-stu-id="2279a-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="2279a-108">Além disso, um classe de filtro complementar é criado para armazenar as informações de filtro.</span><span class="sxs-lookup"><span data-stu-id="2279a-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="2279a-109">Vários elementos de interface do usuário são adicionados ao controle de filtro, juntamente com alguns métodos e propriedades para armazenar as informações de filtro atual na instância de classe do filtro.</span><span class="sxs-lookup"><span data-stu-id="2279a-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="2279a-110">Em seguida, a persistência de controle de usuário é implementada usando o método RegisterRequiresControlState e métodos de salvar/restaurar associados.</span><span class="sxs-lookup"><span data-stu-id="2279a-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="2279a-111">Esses métodos armazenam a instância da classe de filtro e seus dados durante postbacks de página.</span><span class="sxs-lookup"><span data-stu-id="2279a-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="2279a-112">Por fim, há uma discussão sobre como armazenar vários objetos na implementação de estado do controle.</span><span class="sxs-lookup"><span data-stu-id="2279a-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="2279a-113">&#9654; Assista ao vídeo (minutos 23)</span><span class="sxs-lookup"><span data-stu-id="2279a-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
