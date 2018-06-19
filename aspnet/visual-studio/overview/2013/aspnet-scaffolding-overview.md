---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Estrutura do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: tfitzmac
description: Estrutura do ASP.NET é um novo recurso que está incluído no Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506725"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Estrutura do ASP.NET no Visual Studio 2013
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Estrutura do ASP.NET é um novo recurso que está incluído no Visual Studio 2013.


## <a name="overview"></a>Visão Geral

Estrutura do ASP.NET é uma estrutura de geração de código para aplicativos Web ASP.NET. Visual Studio 2013 inclui geradores de código instalado previamente para projetos MVC e a API da Web. Você pode adicionar scaffolding ao projeto quando você deseja adicionar o código que interage com os modelos de dados rapidamente. Usando o scaffolding pode reduzir a quantidade de tempo para desenvolver as operações de dados padrão em seu projeto.

Por padrão, o Visual Studio 2013 não dá suporte a geração de código para um projeto de Web Forms, mas você pode usar o scaffolding com formulários da Web, adicionando dependências MVC ao projeto ou instalar uma extensão. Ambas as abordagens são mostradas abaixo.

Visual Studio 2013 Update 2 (atualmente RC) fornece a capacidade de estender a estrutura do ASP.NET para atender aos requisitos do seu cenário. Com essa funcionalidade, você pode criar um modelo de scaffolding personalizados e adicioná-lo à caixa de diálogo Adicionar novo Scaffold. Dentro do modelo personalizado, você deve especificar o código que é gerado durante a adição de um item de scaffolding. Para obter mais informações, consulte [criando um Scaffolder personalizado para o Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Pré-requisitos

Para usar a estrutura do ASP.NET, você deve ter:

- Microsoft Visual Studio 2013
- Ferramentas de desenvolvedor da Web (parte da instalação padrão do Visual Studio 2013)
- Estruturas de Web do ASP.NET e ferramentas 2013 (parte da instalação padrão do Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Adicionar um item de scaffolding MVC ou Web API

Para adicionar um scaffold, o projeto ou uma pasta dentro do projeto e selecione **adicionar** – **Novo Item de Scaffold**, conforme mostrado na imagem a seguir.

![Adicionar item de scaffold](aspnet-scaffolding-overview/_static/image1.png)

Do **adicionar Scaffold** janela, selecione o tipo de scaffold para adicionar.

![Selecione o tipo de scaffold](aspnet-scaffolding-overview/_static/image2.png)

O **Adicionar controlador** janela dá a oportunidade de selecionar opções para gerar o controlador, incluindo se deseja usar os novos recursos de assíncronas do Entity Framework 6.

![Adicionar controlador](aspnet-scaffolding-overview/_static/image3.png)

As classes relevantes e as páginas são criadas para seu cenário. Por exemplo, a imagem a seguir mostra as exibições que foram criadas por meio de scaffolding para uma classe de modelo chamada filmes e o controlador MVC.

![Os arquivos criados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Adicionar um item de scaffolding para formulários da Web

Para adicionar o scaffolding que gera o código de formulários da Web, você deve instalar uma extensão para o Visual Studio ou adicione dependências MVC. Ambas as abordagens são mostradas abaixo, mas você só precisa fazer uma dessas abordagens.

### <a name="web-forms-scaffolding-extension"></a>Estrutura de extensão de formulários da Web

Você pode instalar uma extensão do Visual Studio que permitem que você use scaffolding com um projeto Web Forms. No Visual Studio, selecione **ferramentas** e **extensões e atualizações**. Nessa caixa de diálogo Pesquisar na Galeria do Visual Studio para **Web Forms Scaffolding**.

![instalar o web forms do estrutura](aspnet-scaffolding-overview/_static/image5.png)

Para obter mais informações, consulte [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dependências do MVC

Para adicionar dependências MVC, selecione **adicionar** - **Novo Item de Scaffold**. Na janela Adicionar Scaffold, selecione **dependências MVC**, conforme mostrado abaixo.

![Adicione dependências MVC](aspnet-scaffolding-overview/_static/image6.png)

Há duas opções para a estrutura MVC; Mínimo e completo. Se você selecionar no mínimo, somente os pacotes NuGet e referências para o ASP.NET MVC são adicionadas ao seu projeto. Se você selecionar a opção completo, as dependências mínimo são adicionadas, bem como os arquivos de conteúdo necessários para um projeto MVC. Para usar facilmente scaffolding, selecione dependências completas.

![Selecione dependências completas](aspnet-scaffolding-overview/_static/image7.png)

Depois de adicionar as dependências, você verá um **readme.txt** arquivo. Com cuidado, siga as instruções nesse arquivo para garantir que seu projeto está funcionando corretamente.

Quando você tiver concluído as etapas no arquivo Leiame. txt, você pode adicionar um novo item de scaffolding conforme mostrado na seção anterior sobre o MVC e a API da Web. Os modos de exibição gerado automaticamente e o controlador funcionará corretamente em seu projeto.

## <a name="tutorials"></a>Tutoriais

Para criar um scaffolder personalizado, consulte [criando um Scaffolder personalizado para o Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Para personalizar os arquivos gerados, consulte [como personalizar os arquivos gerados na caixa de diálogo Novo Item de Scaffold](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Para obter um exemplo do uso de scaffolding com **desenvolvimento de banco de dados primeiro**, consulte [EF Database First com o ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Para obter um exemplo usando o scaffolding de uma **MVC** de projeto, consulte [guia de Introdução ao ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obter um exemplo usando o scaffolding de uma **API da Web** de projeto, consulte [criar uma API REST com atributo roteamento na API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
