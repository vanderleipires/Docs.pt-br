---
title: "Introdução ao ASP.NET Core 1.1"
author: rick-anderson
description: "Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core 1.1."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: 895e91efbba931923540e4cd182862cbc1851585
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-11"></a>Introdução ao ASP.NET Core 1.1

> [!NOTE]
> Estas instruções referem-se ao ASP.NET Core 1.1. Procurando a última versão? Consulte [a versão atual deste tutorial](xref:getting-started).

1. Instale o **Instalador do SDK** do .NET Core para o SDK 1.0.4 na [página de downloads do SDK 1.0.4 do .NET Core 1.0.5 e 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).

2. Crie uma pasta para um novo projeto .NET Core.

   No macOS e no Linux, abra uma janela de terminal. No Windows, abra um prompt de comando.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Se você instalou uma versão posterior do SDK no computador, crie um arquivo *global.json* para selecionar o SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Crie um novo projeto .NET Core.

   ```terminal
   dotnet new web
   ```
   
3.  Restaure os pacotes.

    ```terminal
    dotnet restore
    ```

4. Execute o aplicativo.

   O comando `dotnet run` compila o aplicativo primeiro, se necessário.

   ```terminal
   dotnet run
   ```

5. Navegue para `http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Próximas etapas

Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)

Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).

Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework. Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
