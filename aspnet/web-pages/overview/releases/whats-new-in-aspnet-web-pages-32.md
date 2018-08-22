---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: O que há de novo no ASP.NET Web de páginas 3.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 06/30/2014
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 31b462a3116b29770f534fb95b69a29ae665487c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825615"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>O que há de novo no ASP.NET Web Pages 3.2
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para ASP.NET Web Pages 3.2, páginas da Web 3.2.2 e [Web Pages 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

Esta versão corrige um bug e apresenta um novo recurso.

## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote de ASP.NET Web Pages 3.2 tem a seguinte versão: &ldquo;3.2.0&rdquo;. Você pode instalar ou atualizar esses pacotes por meio [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados por meio do Console do Gerenciador de pacotes NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Novo recurso e correção de Bug

Podemos foi corrigido um bug e uma melhoria de recursos secundários feitas nesta versão. Você pode encontrar a consulta para o mesmo [aqui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>3.2.2 de páginas da Web ASP.NET

Esta versão faz o Rollup a alteração [versão Beta do ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) que fornece uma melhoria significativa de desempenho na renderização de páginas do razor grandes. Ver[problema de Codeplex 585](https://aspnetwebstack.codeplex.com/workitem/585). Esta versão se alinha com o MVC 5.2.2 pacotes que serão agora dependem desta versão.

Trabalhamos com a equipe do MSN na renderização de páginas grandes. Quando páginas renderizam mais de 80 quilobytes de dados, podemos acabar com objetos na heap de objeto grande. Quando várias camadas de layouts são usadas, esse efeito pode ser multiplicado.

O resultado no servidor é o uso de CPU adicional, maior retenção de memória e até mesmo longa pausa durante [Gen 2 limpeza](https://msdn.microsoft.com/en-us/library/ms973837.aspx) no coletor de lixo.

Abaixo está uma tabela que demonstra os resultados de análise de um [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) para uma execução. A CPU é mantida constante em cerca de 68%, enquanto estão sendo renderizadas páginas grandes. A tabela mostra que o número de coletas da geração 2 foi eliminado quase que totalmente, e o resultado é a maior taxa de solicitação e uma redução considerável em pausa devido a coleta de lixo.

| **Área** | **Antes (3.2)** | **Depois de (3.2.1)** | **% De delta** |
| --- | --- | --- | --- |
| Solicitação total (contagem) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Rastreamento duração (segundos) | 196.20 | 198.60 | 1.20% |
| Solicitação/segundo | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Carga de CPU | 68.80% | 68.50% |  -0.40% |
| Exemplos de CPU de GC | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Total de alocações (contagem) | 55,357,146 | 57,222,949 | 3.40% |
| Total de GC pausar (exemplos) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (contagem) | 403 | 1,216 | 201.70% |
| GC Gen1 (contagem) | 290 | 367 | 26.60% |
| GC Gen2 (contagem) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / solicitação (amostras/req) | 19.73 | 16.47 | -16.50% |

| Codificação de cores: | <font style="background-color: #00ff00">Aperfeiçoamento de núcleo</font> | <font style="background-color: #4bacc6">Impacto positivo no desempenho</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Esta versão contém apenas as correções de bugs. Você pode usar [essa consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver a lista dos problemas corrigidos nesta versão.
