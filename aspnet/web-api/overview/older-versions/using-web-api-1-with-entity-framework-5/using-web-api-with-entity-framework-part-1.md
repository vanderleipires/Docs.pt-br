---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: Visão geral e criação do projeto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f0616383fce2e92f7d1a0b63bf840208f7327bf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394055"
---
<a name="part-1-overview-and-creating-the-project"></a>Parte 1: Visão geral e criação do projeto
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework é uma estrutura de mapeamento relacional/objeto. Ele mapeia os objetos de domínio em seu código para entidades em um banco de dados relacional. Geralmente, você não precisa se preocupar sobre a camada de banco de dados, pois o Entity Framework cuida para você. O código manipula os objetos e as alterações são persistidas para um banco de dados.

## <a name="about-the-tutorial"></a>Sobre o Tutorial

Neste tutorial, você criará um aplicativo simples do repositório. Há duas partes principais para o aplicativo. Usuários normais podem visualizar os produtos e criam pedidos:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Os administradores podem criar, excluir ou editar produtos:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Habilidades que você aprenderá

Aqui está o que você aprenderá:

- Como usar o Entity Framework com a API Web do ASP.NET.
- Como usar Knockout. js para criar um cliente dinâmico da interface do usuário.
- Como usar a autenticação de formulários com a API Web para autenticar usuários.

Embora este tutorial é independente, você talvez queira ler os tutoriais a seguir primeiro:

- [Sua primeira API de Web ASP.NET](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Criar uma API da Web que suporta operações CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Algum conhecimento dos [ASP.NET MVC](../../../../mvc/index.md) também é útil.

## <a name="overview"></a>Visão geral

Em um alto nível, aqui está a arquitetura do aplicativo:

- ASP.NET MVC gera as páginas HTML para o cliente.
- API Web ASP.NET expõe operações CRUD nos dados (produtos e pedidos).
- Entity Framework converte os modelos de c# usados pela API da Web em entidades de banco de dados.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

O diagrama a seguir mostra como os objetos de domínio são representados em várias camadas do aplicativo: A camada de banco de dados, o modelo de objeto e, finalmente, o formato de transmissão, que é usado para transmitir dados para o cliente por meio de HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Você pode criar o projeto do tutorial usando o Visual Web Developer Express ou a versão completa do Visual Studio.

Dos **inicie** , clique em **novo projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto "ProductStore" e clique em **Okey**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **aplicativo de Internet** e clique em **Okey**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

O modelo "Aplicativo de Internet" cria um aplicativo ASP.NET MVC que dá suporte à autenticação de formulários. Se você executar o aplicativo agora, ele já tem alguns recursos:

- Novos usuários podem registrar clicando no link "Registrar" no canto superior direito.
- Os usuários registrados podem fazer logon, clicando no link "Entrar".

Informações de associação são mantidas em um banco de dados que é criado automaticamente. Para obter mais informações sobre a autenticação de formulários no ASP.NET MVC, consulte [instruções passo a passo: usando a autenticação de formulários no ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Atualizar o arquivo CSS

Esta etapa é superficial, mas isso tornará as páginas renderizado da seguinte forma as capturas de tela anteriores.

No Gerenciador de soluções, expanda a pasta de conteúdo e abra o arquivo chamado CSS. Adicione os seguintes estilos CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Avançar](using-web-api-with-entity-framework-part-2.md)
