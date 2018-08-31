---
uid: visual-studio/overview/2017/optimize-build-perf
title: Otimizar o desempenho de build de solução
author: AngelosP
description: Otimizar o desempenho de build de solução
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312135"
---
# <a name="optimize-build-performance-for-solution"></a>Otimizar o desempenho de build de solução

15,8 de 2017 do Visual Studio ou posterior inclui um item de menu: **construir** > **compilação ASP.NET** > **otimizar o desempenho de compilação para a solução**.

![captura de tela do novo item de menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

O ASP.NET compila seus modos de exibição em tempo de execução, o que significa que um projeto ASP.NET carrega uma cópia do compilador. No entanto em um computador de desenvolvedor quando a cópia do compilador não corresponde a cópia do Visual Studio, o desempenho de compilação é afetado na ordem de 1 a 3 segundos por compilação incremental. Esse recurso atualiza a cópia do seu projeto do compilador para coincidir com o Visual Studio, que geralmente acelera a compilações incrementais.

**Isso é aplicável ao ASP.NET Framework 4.7.1 ou posterior somente a projetos, não se aplica ao ASP.NET Core.**
