---
uid: visual-studio/overview/2017/optimize-build-perf
title: Otimizar o desempenho de build de solução
author: tfitzmac
description: Otimizar o desempenho de build de solução
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909940"
---
# <a name="optimize-build-performance-for-solution"></a>Otimizar o desempenho de build de solução
15,8 de 2017 do Visual Studio e posteriormente adicionado um novo item de menu sob **Build > compilação do ASP.NET > otimizar o desempenho de compilação para a solução**.

![captura de tela do novo item de menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

O ASP.NET compila seus modos de exibição em tempo de execução, o que significa que seu projeto ASP.NET carrega uma cópia do compilador. No entanto, em um computador de desenvolvedor quando a cópia do compilador não corresponde à cópia do Visual Studio, o desempenho de compilação é afetado na ordem de 1 a 3 segundos por compilação incremental. Esse recurso atualizará a cópia do seu projeto do compilador para corresponder ao Visual Studio qual devem acelerar as compilações incrementais.

Isso é aplicável a somente projetos de estrutura do ASP.NET, ele não se aplica ao ASP.NET Core.
