---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: O que há de novo no ASP.NET Web páginas 3.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 80421018e0508d430b6142cd3cee1727d1d17b7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-web-pages-32"></a>O que há de novo no ASP.NET Web Pages 3.2
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para 3.2 de páginas da Web do ASP.NET, páginas da Web 3.2.2 e [páginas da Web 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>Páginas da Web do ASP.NET 3.2

Esta versão corrige um bug e apresenta um novo recurso.

## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes do NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote 3.2 de páginas da Web do ASP.NET tem a seguinte versão: &ldquo;3.2.0&rdquo;. Você pode instalar ou atualizar esses pacotes por meio de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados usando o NuGet Package Manager Console:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Novo recurso e correção de Bug

Estamos corrigido um bug e fez um aprimoramento de recursos secundários nesta versão. Você pode encontrar a consulta para o mesmo [aqui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>Páginas da Web do ASP.NET 3.2.2

Esta versão reverte o a alteração [versão Beta do ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) que fornece uma melhoria significativa de desempenho na renderização de páginas grandes razor. Consulte[Codeplex problema 585](https://aspnetwebstack.codeplex.com/workitem/585). Essa versão se alinha com o MVC 5.2.2 pacotes que agora dependem desta versão.

Trabalhamos com a equipe do MSN renderizar páginas grandes. Quando páginas renderizadas em 80 Kilobytes de dados, podemos acabar com objetos no heap de objeto grande. Quando várias camadas de layouts são usadas pode ser multiplicado esse efeito.

O resultado no servidor é adicional da CPU, de memória e até mesmo tempo de retenção mais longa pausa durante [Gen 2 limpeza](https://msdn.microsoft.com/en-us/library/ms973837.aspx) no coletor de lixo.

Abaixo está uma tabela que demonstram os resultados da análise de um [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) para uma execução. A CPU é mantida constante em aproximadamente 68%, enquanto estão sendo renderizadas páginas grandes. A tabela mostra que o número de coleções de geração 2 foi eliminado quase completamente, e o resultado é a maior taxa de solicitação e uma considerável redução em pausa devido a coleta de lixo.

| **Área** | **Antes de (3.2)** | **Depois de (3.2.1)** | **% De delta** |
| --- | --- | --- | --- |
| Solicitação total (contagem) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Rastreamento duração (em segundos) | 196.20 | 198.60 | 1.20% |
| Solicitação/segundo | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Carga de CPU | 68.80% | 68.50% |  -0.40% |
| Exemplos de CPU de GC | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Total de alocações (contagem) | 55,357,146 | 57,222,949 | 3.40% |
| Pausar de GC total (exemplos) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (contagem) | 403 | 1,216 | 201.70% |
| Gen1 GC (contagem) | 290 | 367 | 26.60% |
| Gen2 GC (contagem) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / solicitação (exemplos/NEC) | 19.73 | 16.47 | -16.50% |

| Codificação de cores: | <font style="background-color: #00ff00">Aperfeiçoamento de núcleo</font> | <font style="background-color: #4bacc6">Impacto positivo no desempenho</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET páginas da Web 3.2.3 beta1

Esta versão contém somente as correções de bugs. Você pode usar [esta consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver a lista de problemas corrigidos nesta versão.
