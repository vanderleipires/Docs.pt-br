---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: Visão geral e criando o projeto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-creating-the-project"></a>Parte 1: Visão geral e criando o projeto
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework é uma estrutura de mapeamento relacional/objeto. Ele mapeia os objetos de domínio no seu código para entidades em um banco de dados relacional. A maior parte do tempo, você não precisa se preocupar sobre a camada de banco de dados, como o Entity Framework cuida para você. Seu código manipula os objetos e as alterações são mantidas para um banco de dados.

## <a name="about-the-tutorial"></a>Sobre o Tutorial

Neste tutorial, você criará um aplicativo simples de armazenamento. Há duas partes principais para o aplicativo. Usuários normais podem exibir produtos e criar pedidos:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Os administradores podem criar, excluir ou editar produtos:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Você vai aprender as habilidades

Aqui está o que você aprenderá:

- Como usar o Entity Framework com a API da Web do ASP.NET.
- Como usar Knockout. js para criar um interface de usuário do cliente dinâmico.
- Como usar a autenticação de formulários com a API da Web para autenticar usuários.

Embora este tutorial é independente, você pode desejar ler os tutoriais a seguir primeiro:

- [Sua primeira API da Web ASP.NET](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Criando uma API da Web que suporta operações CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Algum conhecimento dos [ASP.NET MVC](../../../../mvc/index.md) também é útil.

## <a name="overview"></a>Visão geral

Em um nível alto, aqui está a arquitetura do aplicativo:

- ASP.NET MVC gera páginas HTML para o cliente.
- ASP.NET Web API expõe operações CRUD nos dados (products e orders).
- Entity Framework converte os modelos c# usados pela API da Web em entidades de banco de dados.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

O diagrama a seguir mostra como os objetos de domínio são representados em várias camadas do aplicativo: A camada de banco de dados, o modelo de objeto e, finalmente, o formato de conexão, que é usado para transmitir dados para o cliente por meio de HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Você pode criar o projeto do tutorial usando o Visual Web Developer Express ou a versão completa do Visual Studio.

Do **iniciar** , clique em **novo projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto "ProductStore" e clique em **Okey**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **aplicativo de Internet** e clique em **Okey**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

O modelo de "Aplicativo de Internet" cria um aplicativo ASP.NET MVC que dá suporte à autenticação de formulários. Se você executar o aplicativo agora, ele já tem alguns recursos:

- Novos usuários podem registrar clicando no link "Register" no canto superior direito.
- Usuários registrados podem efetuar login clicando no link "Entrar".

Informações de associação são mantidas em um banco de dados é criado automaticamente. Para obter mais informações sobre autenticação de formulários do ASP.NET MVC, consulte [passo a passo: usando a autenticação de formulários do ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Atualizar o arquivo CSS

Esta etapa é superficial, mas isso tornará as páginas renderizar como as capturas de tela anteriores.

No Gerenciador de soluções, expanda a pasta de conteúdo e abra o arquivo chamado Site.css. Adicione os seguintes estilos CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Avançar](using-web-api-with-entity-framework-part-2.md)
