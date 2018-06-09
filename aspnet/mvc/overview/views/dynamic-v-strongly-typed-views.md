---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dinâmico v. Fortemente tipado exibições | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26504085"
---
<a name="dynamic-v-strongly-typed-views"></a>Dinâmico v. Modos de exibição fortemente tipados
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

Há três maneiras de transmitir informações de um controlador em uma exibição no ASP.NET MVC 3:

1. Como um objeto de modelo com rigidez de tipos.
2. Como um tipo dinâmico (usando @model dinâmico)
3. Usando a ViewBag

Eu escrevi um aplicativo MVC 3 superior Blog simples para comparar e contrastar exibições dinâmicas e fortemente tipadas. O controlador começa com uma lista simple de blogs:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Clique com o botão direito no método IndexNotStonglyTyped() e adicionar um modo de exibição do Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Verifique se o **criar uma exibição fortemente tipada** caixa não estiver marcada. A exibição resultante não contêm muito:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Como estamos usando um dinâmico e não uma exibição fortemente tipada, o intellisense não ajude-nos. O código completo é mostrado abaixo:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Agora vamos adicionar uma exibição fortemente tipada. Adicione o seguinte código para o controlador:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Observe que é exatamente é o View(topBlogs) retorno mesmo; chamar como o modo de exibição não fortemente tipado. Clique com o botão direito dentro do *StonglyTypedIndex()* e selecione **adicionar exibição**. Dessa vez, selecione o **Blog** classe de modelo e selecione **lista** como o modelo de Scaffold.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Dentro do novo modelo de exibição, podemos obter suporte do intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

O projeto c# pode ser baixado [aqui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
