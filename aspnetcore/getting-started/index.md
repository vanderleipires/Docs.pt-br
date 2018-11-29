---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 5b5384b0bfa933f40f82513b02f7a14367fbef76
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549083"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Tutorial: introdução ao ASP.NET Core

Este tutorial mostra como usar a interface de linha de comando do .NET Core para criar um aplicativo Web ASP.NET Core. Você aprenderá como:

> [!div class="checklist"]
> * Criar um projeto de aplicativo Web.
> * Habilitar o HTTPS local.
> * Execute o aplicativo.
> * Editar uma página do Razor.

No final, você terá um aplicativo Web de trabalho em execução no seu computador local.

![Página inicial do aplicativo Web](_static/home-page.png)


## <a name="prerequisites"></a>Pré-requisitos

* Instale o [!INCLUDE [](~/includes/2.1-SDK.md)].

## <a name="create-a-web-app-project"></a>Criar um projeto de aplicativo Web

* Abra um shell de comando e insira o seguinte comando:

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a>Habilitar o HTTPS local

* Confie no certificado de desenvolvimento HTTPS:

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

## <a name="run-the-app"></a>Executar o aplicativo

* Execute os seguintes comandos:

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* Procure [https://localhost:5001](https://localhost:5001). Clique em **Aceitar** para aceitar a política de privacidade e cookies. Este aplicativo não armazena informações pessoais.

## <a name="edit-a-razor-page"></a>Editar uma página do Razor

* Abra *Pages/About.cshtml* e modifique a página com a seguinte marcação realçada:

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* Navegue até [https://localhost:5001/About](https://localhost:5001/About) e verifique as alterações exibidas.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Criar um projeto de aplicativo Web.
> * Habilitar o HTTPS local.
> * Execute o projeto.
> * Faça uma alteração.

Para saber mais sobre o ASP.NET Core, confira a introdução:

> [!div class="nextstepaction"]
> <xref:index>



> [!NOTE]
> Estamos testando a usabilidade de uma nova estrutura proposta para o sumário do ASP.NET Core.  Se você tiver alguns minutos para experimentar um exercício de localização de sete tópicos diferentes no sumário atual ou proposto, [clique aqui para participar do estudo](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).