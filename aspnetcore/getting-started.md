---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>Introdução ao ASP.NET Core

> [!NOTE]
> Essas instruções referem-se à última versão do ASP.NET Core. Confira [Introdução ao ASP.NET Core 1.1](xref:getting-started-1.1) para obter a versão 1.1 deste documento.

1. Instale o [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Crie um novo projeto .NET Core.

   No macOS e no Linux, abra uma janela de terminal. No Windows, abra um prompt de comando. Insira o seguinte comando:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Execute o aplicativo.

    Use os seguintes comandos para executar o aplicativo:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Navegue até [http://localhost:5000](http://localhost:5000)

5. Abra <em>Pages/About.cshtml</em> e modifique a página para exibir a mensagem "Olá, mundo! O horário no servidor é @DateTime.Now ":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Navegue até [ http://localhost:5000/About ](http://localhost:5000/About) e verifique as alterações.

### <a name="next-steps"></a>Próximas etapas

Para obter tutoriais de introdução, consulte [Tutoriais do ASP.NET Core](tutorials/index.md)

Para obter uma introdução aos conceitos e à arquitetura do ASP.NET Core, consulte [Introdução ao ASP.NET Core](index.md) e [Conceitos básicos do ASP.NET Core](fundamentals/index.md).

Um aplicativo ASP.NET Core pode usar a Biblioteca de Classes Base e o tempo de execução do .NET Core ou do .NET Framework. Para obter mais informações, consulte [Escolhendo entre o .NET Core e o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
