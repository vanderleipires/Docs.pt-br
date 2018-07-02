---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: eb049dea2800cf2e12c044b88d1664ee80bb95a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296763"
---
# <a name="get-started-with-aspnet-core"></a>Introdução ao ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Instale o [!INCLUDE[](~/includes/2.1-SDK.md)].

2. Crie um projeto do ASP.NET Core. Abra um shell de comando e insira o seguinte comando:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. Confie no certificado de desenvolvimento HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. Execute o aplicativo:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Procure [http://localhost:5001](http://localhost:5001).  Clique em **Aceitar** para aceitar a política de privacidade e cookies. Este aplicativo não armazena informações pessoais.

6. Abra *Pages/About.cshtml* e modifique a página com a seguinte marcação realçada:

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Navegue até [http://localhost:5001/About](http://localhost:5001/About) e verifique as alterações exibidas.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Instale o [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].

2. Crie um novo projeto ASP.NET Core.

   Abra um shell de comando. Insira o seguinte comando:

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Execute o aplicativo com os seguintes comandos:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Procure [http://localhost:5000](http://localhost:5000).

5. Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo! O horário no servidor é @DateTime.Now" :

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Navegue até [ http://localhost:5000/About ](http://localhost:5000/About) e verifique as alterações.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Instale o **Instalador do SDK** do .NET Core do SDK 1.0.4 que se encontra na [página Todos os downloads do .NET Core](https://www.microsoft.com/net/download/all).

2. Crie uma pasta para um novo projeto ASP.NET Core.

   Abra um shell de comando. Insira os seguintes comandos:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Se você instalou uma versão posterior do SDK no computador, crie um arquivo *global.json* para selecionar o SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Crie um novo projeto ASP.NET Core.

   ```console
   dotnet new web
   ```

5. Restaure os pacotes.

    ```console
    dotnet restore
    ```

6. Execute o aplicativo.

   ```console
   dotnet run
   ```

   O comando [dotnet run](/dotnet/core/tools/dotnet-run) cria o aplicativo primeiro, se necessário.

7. Navegue para `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
