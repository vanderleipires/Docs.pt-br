---
uid: webhooks/diagnostics/debugging
title: "ASP.NET WebHooks depuração | Microsoft Docs"
author: rick-anderson
description: Como depurar WebHooks ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 524cdf0246eda9ef213414923cd23a92a01f211e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET WebHooks depuração  

## <a name="debugging-in-azure"></a>Depuração no Azure

Para depurar seu aplicativo Web durante a execução no Azure, consulte o tutorial [solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Depuração com símbolos e fonte

Além de depuração de seu próprio código, é possível depurar diretamente no Microsoft ASP.NET WebHooks e, na verdade todos do .NET. Isso funciona independentemente de se você depurar localmente ou remotamente. Configurar pela primeira vez, o Visual Studio para localizar a origem e símbolos acessando **depurar** e **opções e configurações**. Defina as opções como este:

![Opções e configurações](_static/SourceSymbols.png)

Em seguida, adicionar um link para [symbolsource.org](http://symbolsource.org) para baixar o código-fonte e símbolos. Vá para o **símbolos** guia do menu acima e adicione o seguinte como um local de símbolo:

```
http://srv.symbolsource.org/pdb/Public
```

Além disso, certifique-se de que o diretório de cache tem um nome curto; Caso contrário, os nomes de arquivo podem obter muito que fará com que os símbolos não carregar. Um exemplo de caminho é:

```
C:\SymCache
```

As configurações devem ser semelhantes a este:

![Exemplo de local de arquivo de símbolo de opções](_static/SymSource.png)
