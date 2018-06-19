---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Atualizando um aplicativo ASP.NET MVC 1.0 para ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Este documento descreve como atualizar manualmente e com um Assistente para um aplicativo ASP.NET MVC 1.0 para ASP.NET MVC 2. Este documento também está disponível para d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530055"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Atualizando um aplicativo ASP.NET MVC 1.0 para ASP.NET MVC 2
====================
> Este documento descreve como atualizar manualmente e com um Assistente para um aplicativo ASP.NET MVC 1.0 para ASP.NET MVC 2. Este documento também está disponível para [baixar](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Introdução

ASP.NET MVC 2 pode ser instalado lado a lado com o ASP.NET MVC 1.0 no mesmo servidor. Isso fornece flexibilidade de desenvolvedores de aplicativos na escolha quando atualizar um aplicativo ASP.NET MVC 1.0 para ASP.NET MVC 2.

Visual Studio 2010 inclui um assistente que atualizações de projetos do ASP.NET MVC 1.0 existentes criados com o Visual Studio 2008 para o ASP.NET MVC 2. O Assistente de atualização é iniciado, abra um projeto ASP.NET MVC 1.0 no Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Atualizar Assistente do ASP.NET MVC 1.0 no Visual Studio 2008 SP1

Para atualizar um aplicativo ASP.NET MVC 1.0 para ASP.NET MVC 2 no Visual Studio 2008 SP1, use o aplicativo MvcAppConverter (sem suporte). Você pode baixar este aplicativo da URL a seguir:

[https://go.microsoft.com/fwlink/?LinkId=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Atualizar manualmente um projeto ASP.NET MVC 1.0

Para atualizar manualmente um aplicativo ASP.NET MVC 1.0 existente para a versão 2, siga estas etapas:

1. Faça um backup de um projeto existente.
2. Em um editor de texto, abra o arquivo de projeto (o arquivo com a extensão de arquivo. csproj ou. vbproj) e localizar o elemento ProjectTypeGuid. Como o valor desse elemento, substitua o GUID {603c0e0b-db56-11dc-be95-000d561079b0} {F85E285D-A4E0-4152-9332-AB1D724D3325}. Quando terminar, o valor do elemento deve ser da seguinte maneira: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Na pasta raiz do aplicativo Web, edite o arquivo Web. config. Pesquise System.Web.Mvc, versão = 1.0.0.0 e substitua todas as instâncias System.Web.Mvc, versão = 2.0.0.0.
4. Repita a etapa anterior para o arquivo Web. config localizado na pasta modos de exibição.
5. Abra o projeto usando o Visual Studio e em **Solution Explorer**, expanda o **referências** nó. Exclua a referência ao System.Web.Mvc (que aponta para o assembly versão 1.0). Adicione uma referência a System.Web.Mvc (v2.0.0.0).
6. Adicione o seguinte elemento bindingRedirect para o arquivo Web. config na raiz do aplicativo na seção de configuração:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Crie um novo aplicativo ASP.NET MVC 2 vazio. Copie os arquivos da pasta de Scripts do novo aplicativo para a pasta de Scripts do aplicativo existente.
8. Atualizar as existentes â €™ o arquivo % s CSS com as definições de estilo CSS no arquivo Site.css.
9. Compilar o aplicativo e executá-lo. Se ocorrer algum erro, consulte a seção alterações significativas do [o que há de novo no ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) página.
