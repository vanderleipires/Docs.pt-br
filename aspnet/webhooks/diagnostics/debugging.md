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
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET WebHooks depuração  

## <a name="debugging-in-azure"></a>Depuração no Azure

Para depurar seu aplicativo Web durante a execução no Azure, consulte o tutorial [solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

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
