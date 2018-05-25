---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a>Introdução ao ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

1. Instale o [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].

2. Crie um novo projeto .NET Core.

   No macOS e no Linux, abra uma janela de terminal. No Windows, abra um prompt de comando. Insira o seguinte comando:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Execute o aplicativo com os seguintes comandos:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Procure [http://localhost:5000](http://localhost:5000).

5. Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo! O horário no servidor é @DateTime.Now" :

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Navegue até [ http://localhost:5000/About ](http://localhost:5000/About) e verifique as alterações.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Instale o **Instalador do SDK** do .NET Core do SDK 1.0.4 que se encontra na [página Todos os downloads do .NET Core](https://www.microsoft.com/net/download/all).

2. Crie uma pasta para um novo projeto .NET Core.

   No macOS e no Linux, abra uma janela de terminal. No Windows, abra um prompt de comando.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Se você instalou uma versão posterior do SDK no computador, crie um arquivo *global.json* para selecionar o SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Crie um novo projeto .NET Core.

   ```terminal
   dotnet new web
   ```

5. Restaure os pacotes.

    ```terminal
    dotnet restore
    ```

6. Execute o aplicativo.

   ```terminal
   dotnet run
   ```

   O comando [dotnet run](/dotnet/core/tools/dotnet-run) cria o aplicativo primeiro, se necessário.

7. Navegue para `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end