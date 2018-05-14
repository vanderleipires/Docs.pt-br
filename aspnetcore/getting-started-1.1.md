---
title: Introdução ao ASP.NET Core 1.1
author: rick-anderson
description: Siga este tutorial rápido para criar e executar um aplicativo Olá, Mundo simples usando o ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a>Introdução ao ASP.NET Core 1.1

> [!NOTE]
> Estas instruções referem-se ao ASP.NET Core 1.1. Procurando a última versão? Consulte [a versão atual deste tutorial](xref:getting-started).

1. Instale o **Instalador do SDK** do .NET Core do SDK 1.0.4 que se encontra na [página Todos os downloads do .NET Core](https://www.microsoft.com/net/download/all).

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

   O comando [dotnet run](/dotnet/core/tools/dotnet-run) criará o aplicativo primeiro caso seja necessário.

   ```terminal
   dotnet run
   ```

5. Navegue para `http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Próximas etapas

Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)

Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).

Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework. Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
