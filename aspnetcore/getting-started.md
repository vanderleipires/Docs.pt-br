---
title: "Introdução ao ASP.NET Core 2.0"
author: rick-anderson
description: "Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core."
keywords: "ASP.NET Core, tutorial, introdução"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a>Introdução ao ASP.NET Core

> [!NOTE]
> Essas instruções referem-se à última versão do ASP.NET Core. Deseja começar com uma versão anterior? Consulte [a versão 1.1 deste tutorial](xref:getting-started-1.1).

1. Instale o [.NET Core](https://microsoft.com/net/core/).

2. Crie um novo projeto .NET Core.

   No macOS e no Linux, abra uma janela de terminal. No Windows, abra um prompt de comando.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. Execute o aplicativo.

   O comando `dotnet run` compila o aplicativo primeiro, se necessário.

   ```terminal
   dotnet run
   ```

5. Navegue para `http://localhost:5000`

### <a name="next-steps"></a>Próximas etapas

Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)

Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).

Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework. Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
