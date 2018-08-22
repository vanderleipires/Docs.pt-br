---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Scaffolding do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: tfitzmac
description: Scaffolding do ASP.NET é um novo recurso que está incluído no Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: c554f592e57f2e6017f7fcfcc9b4c98051e21b37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830428"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Scaffolding do ASP.NET no Visual Studio 2013
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Scaffolding do ASP.NET é um novo recurso que está incluído no Visual Studio 2013.


## <a name="overview"></a>Visão geral

Scaffolding do ASP.NET é uma estrutura de geração de código para aplicativos Web ASP.NET. Visual Studio 2013 inclui geradores de código previamente instalados para projetos MVC e API da Web. Adicionar o scaffolding ao projeto quando você deseja adicionar rapidamente o código que interage com os modelos de dados. Usando o scaffolding pode reduzir a quantidade de tempo para desenvolver as operações de dados padrão em seu projeto.

Por padrão, o Visual Studio 2013 não oferece suporte a geração de código para um projeto de Web Forms, mas você pode usar o scaffolding com Web Forms Adicionando dependências MVC ao projeto ou instalar uma extensão. Ambas as abordagens são mostradas abaixo.

Visual Studio 2013 atualização 2 (atualmente RC) fornece a capacidade de estender o Scaffolding do ASP.NET para atender aos requisitos do seu cenário. Com essa funcionalidade, você pode criar um modelo de scaffolding personalizado e adicioná-lo à caixa de diálogo Adicionar Scaffold novo. Dentro do modelo personalizado, você deve especificar o código que é gerado ao adicionar um item de scaffolding. Para obter mais informações, consulte [criando um Scaffolder personalizado para o Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Pré-requisitos

Para usar o Scaffolding do ASP.NET, você deve ter:

- Microsoft Visual Studio 2013
- Ferramentas de desenvolvedor da Web (parte da instalação padrão do Visual Studio 2013)
- Estruturas da Web do ASP.NET e Tools 2013 (parte da instalação padrão do Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Adicionar um item de scaffolding MVC ou API da Web

Para adicionar um scaffold, clique com botão direito no projeto ou uma pasta dentro do projeto e selecione **Add** – **Novo Item de Scaffold**, conforme mostrado na imagem a seguir.

![Adicionar item andaime](aspnet-scaffolding-overview/_static/image1.png)

Dos **adicionar Scaffold** janela, selecione o tipo de scaffold para adicionar.

![Selecione o tipo de scaffold](aspnet-scaffolding-overview/_static/image2.png)

O **Adicionar controlador** janela lhe dá a oportunidade de selecionar opções para gerar o controlador, incluindo se você deseja usar os novos recursos assíncronos do Entity Framework 6.

![Adicionar controlador](aspnet-scaffolding-overview/_static/image3.png)

As classes relevantes e as páginas são criadas para seu cenário. Por exemplo, a imagem a seguir mostra o controlador MVC e exibições que foram criadas por meio de scaffolding para uma classe de modelo chamada filmes.

![Os arquivos criados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Adicionar um item de scaffolding para Web Forms

Para adicionar o scaffolding que gera o código de Web Forms, você deve instalar uma extensão para o Visual Studio ou adicionar dependências do MVC. Ambas as abordagens são mostradas abaixo, mas você só precisa fazer uma dessas abordagens.

### <a name="web-forms-scaffolding-extension"></a>Extensão de Scaffolding de Web Forms

Você pode instalar uma extensão do Visual Studio que permitem que você usar o scaffolding com um projeto de Web Forms. No Visual Studio, selecione **ferramentas** e, em seguida **extensões e atualizações**. Nessa caixa de diálogo Pesquisar Galeria do Visual Studio para **Scaffolding dos Web Forms**.

![instalar o web forms do scaffolding](aspnet-scaffolding-overview/_static/image5.png)

Para obter mais informações, consulte [Scaffolding dos Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dependências do MVC

Para adicionar dependências do MVC, selecione **Add** - **Novo Item de Scaffold**. Na janela Adicionar Scaffold, selecione **dependências MVC**, conforme mostrado abaixo.

![Adicionar dependências MVC](aspnet-scaffolding-overview/_static/image6.png)

Há duas opções para scaffolding MVC; Mínimo e completo. Se você selecionar no mínimo, apenas os pacotes do NuGet e referências para o ASP.NET MVC são adicionadas ao seu projeto. Se você selecionar a opção completa, as dependências mínimas são adicionadas, bem como os arquivos de conteúdo necessários para um projeto do MVC. Para usar facilmente o scaffolding, selecione dependências completas.

![Selecionar dependências completas](aspnet-scaffolding-overview/_static/image7.png)

Depois de adicionar as dependências, você verá uma **Readme. txt** arquivo. Cuidadosamente, siga as instruções nesse arquivo para garantir que seu projeto funcione corretamente.

Quando você tiver concluído as etapas no arquivo readme. txt, você pode adicionar um novo item com Scaffold, conforme mostrado na seção anterior sobre o MVC e API da Web. Os modos de exibição gerados automaticamente e o controlador funcionará corretamente dentro de seu projeto.

## <a name="tutorials"></a>Tutoriais

Para criar um scaffolder personalizado, consulte [criando um Scaffolder personalizado para o Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Para personalizar os arquivos gerados, consulte [como personalizar os arquivos gerados na caixa de diálogo Novo Item de Scaffold](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Para obter um exemplo de como usar o scaffolding com **desenvolvimento de banco de dados primeiro**, consulte [First do EF banco de dados com o ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Para obter um exemplo de como usar o scaffolding em uma **MVC** de projeto, consulte [Introdução ao ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obter um exemplo de como usar o scaffolding em uma **API Web** de projeto, consulte [criar uma API REST com roteamento de atributo na API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
