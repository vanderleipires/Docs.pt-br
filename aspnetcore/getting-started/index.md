---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860934"
---
# <a name="get-started-with-aspnet-core"></a>Introdução ao ASP.NET Core

Este documento fornece etapas para criar e executar um aplicativo ASP.NET Core.

::: moniker range=">= aspnetcore-2.1"

1. Instale o [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Crie um projeto do ASP.NET Core. Abra um shell de comando e insira o seguinte comando:

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. Confie no certificado de desenvolvimento HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  O comando anterior exibe a caixa de diálogo a seguir:

  ![Caixa de diálogo de aviso de segurança](_static/cert.png)

  Selecione **Sim** se você concordar com confiar no certificado de desenvolvimento.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  O comando anterior exibe a mensagem a seguir:

  *Foi solicitada confiança no certificado de desenvolvimento HTTPS. Se o certificado ainda não for confiável, executaremos o seguinte comando:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.  
  *Esse comando pode solicitar que você insira sua senha para instalar o certificado no conjunto de chaves do sistema.
  
  Senha:*

  Insira sua senha se você concordar em confiar no certificado de desenvolvimento.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  Consulte a documentação para sua distribuição do Linux sobre como confiar no certificado de desenvolvimento HTTPS.
   
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

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Instale o [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

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

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

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

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
