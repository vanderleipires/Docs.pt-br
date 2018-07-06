---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Atualizando um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Este documento descreve como atualizar manualmente e com um Assistente de um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2. Este documento também está disponível para d...
ms.author: aspnetcontent
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: fe1696cd8f98f2ff253d385b62a6bcd74b536d33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823865"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Atualizando um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2
====================
> Este documento descreve como atualizar manualmente e com um Assistente de um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2. Este documento também está disponível para [baixar](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Introdução

ASP.NET MVC 2 pode ser instalado lado a lado com o ASP.NET MVC 1.0 no mesmo servidor. Isso fornece flexibilidade de desenvolvedores de aplicativo na escolha quando atualizar um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2.

Visual Studio 2010 inclui um assistente que atualizações de projetos do ASP.NET MVC 1.0 existentes criados com o Visual Studio 2008 para o ASP.NET MVC 2. O Assistente de atualização é iniciado, abrindo um projeto ASP.NET MVC 1.0 no Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Atualizar Assistente para ASP.NET MVC 1.0 no Visual Studio 2008 SP1

Para atualizar um aplicativo ASP.NET MVC 1.0 para o ASP.NET MVC 2 no Visual Studio 2008 SP1, use o aplicativo MvcAppConverter (sem suporte). Você pode baixar este aplicativo da seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Atualizar manualmente um projeto ASP.NET MVC 1.0

Para atualizar manualmente um aplicativo ASP.NET MVC 1.0 existente para a versão 2, siga estas etapas:

1. Faça um backup de um projeto existente.
2. Em um editor de texto, abra o arquivo de projeto (o arquivo com a extensão de arquivo. csproj ou. vbproj) e localize o elemento ProjectTypeGuid. Como o valor desse elemento, substitua o GUID {603c0e0b-db56-11dc-be95-000d561079b0} com {F85E285D-A4E0-4152-9332-AB1D724D3325}. Quando você terminar, o valor desse elemento deve ser da seguinte maneira: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Na pasta raiz do aplicativo Web, edite o arquivo Web. config. Procure System.Web.Mvc, versão = 1.0.0.0 e substitua todas as instâncias com System.Web.Mvc, versão = 2.0.0.0.
4. Repita a etapa anterior para o arquivo Web. config localizado na pasta modos de exibição.
5. Abra o projeto usando o Visual Studio e, na **Gerenciador de soluções**, expanda o **referências** nó. Exclua a referência ao System.Web.Mvc (que aponta para o assembly de versão 1.0). Adicione uma referência ao System.Web.Mvc (v2.0.0.0).
6. Adicione o seguinte elemento de bindingRedirect para o arquivo Web. config na raiz do aplicativo sob a seção de configuração:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Crie um novo aplicativo ASP.NET MVC 2 vazio. Copie os arquivos da pasta Scripts do novo aplicativo para a pasta de Scripts do aplicativo existente.
8. Atualizar o existente â €™ arquivo CSS de s com as definições de estilo CSS no arquivo CSS.
9. Compilar o aplicativo e executá-lo. Se ocorrerem erros, consulte a seção alterações significativas do [o que há de novo no ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) página.
