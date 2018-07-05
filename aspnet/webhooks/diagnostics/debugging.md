---
uid: webhooks/diagnostics/debugging
title: WebHooks do ASP.NET depuração | Microsoft Docs
author: rick-anderson
description: Como depurar a WebHooks do ASP.NET.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801982"
---
# <a name="aspnet-webhooks-debugging"></a>Depuração de WebHooks do ASP.NET  

## <a name="debugging-in-azure"></a>Depuração no Azure

Para depurar seu aplicativo Web durante a execução no Azure, consulte o tutorial [solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Com o código-fonte e símbolos de depuração

Além de seu próprio código de depuração, é possível depurar diretamente no Microsoft ASP.NET WebHooks e, de fato todos os .NET. Isso funciona independentemente de se você depura localmente ou remotamente. Configurar pela primeira vez, o Visual Studio para localizar o código-fonte e os símbolos acessando **Debug** e, em seguida **opções e configurações**. Defina as opções como este:

![Opções e configurações](_static/SourceSymbols.png)

Em seguida, adicione um link para [symbolsource.org](http://symbolsource.org) para baixar o código-fonte e símbolos. Vá para o **símbolos** guia do menu acima e adicione o seguinte como um local do símbolo:

```
http://srv.symbolsource.org/pdb/Public
```

Além disso, certifique-se de que o diretório de cache tem um nome curto; Caso contrário, os nomes de arquivo podem obter muito longos que fará com que os símbolos para não carregar. Um exemplo de caminho é:

```
C:\SymCache
```

As configurações devem ser semelhantes a este:

![Exemplo de local do arquivo de símbolo de opções](_static/SymSource.png)
