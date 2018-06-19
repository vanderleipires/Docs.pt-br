---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introdução ao Tutorial NerdDinner | Microsoft Docs
author: shanselman
description: É a melhor maneira de aprender uma nova estrutura criar algo com ele. Este tutorial explica como criar um aplicativo pequeno, mas completo usando ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868549"
---
<a name="introducing-the-nerddinner-tutorial"></a>Introdução ao Tutorial NerdDinner
====================
por [Scott Hanselman](https://github.com/shanselman)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> É a melhor maneira de aprender uma nova estrutura criar algo com ele. Este tutorial explica como criar um pequeno, mas concluir, o aplicativo usando o ASP.NET MVC 1 e apresenta alguns dos principais conceitos por trás dele.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-tutorial"></a>Tutorial de NerdDinner

É a melhor maneira de aprender uma nova estrutura criar algo com ele. Este tutorial explica como criar um pequeno, mas concluir, o aplicativo usando o ASP.NET MVC e apresenta alguns dos principais conceitos por trás dele.

O aplicativo, vamos criar é chamado de "NerdDinner". NerdDinner fornece uma maneira fácil para as pessoas a localizar e organizar jantares online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner permite que os usuários registrados criar, editar e excluir jantares. Ele aplica um conjunto de regras de negócio e validação consistente entre o aplicativo:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Visitantes podem usar um mapa de AJAX para pesquisar futuros jantares sendo mantidos próximo a eles:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Clicar em uma refeição vai levá-los a uma página de detalhes onde eles podem aprender mais sobre ele:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Se eles estão interessados em participar refeição que possam fazer logon ou registre-se no site:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Eles, em seguida, podem clicar em um link de RSVP baseados em AJAX para participar do evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementando NerdDinner

Vamos começar nosso aplicativo NerdDinner usando arquivo -&gt;comando novo projeto no Visual Studio para criar um novo projeto do ASP.NET MVC. Em seguida, incrementalmente adicionaremos recursos e funcionalidades. Ao longo do caminho, abordaremos:

1. [Como criar um novo projeto ASP.NET MVC](# "criar um novo projeto ASP.NET MVC")
2. [Como criar um banco de dados](# "criar um banco de dados")
3. [Como criar um modelo de validação de regra de negócios](# "criar um modelo com validações de regra de negócios")
4. [Como usar controladores e exibições para implementar uma interface do usuário de listagem/detalhes](# "usar controladores e exibições para implementar uma interface de usuário de lista-detalhes")
5. [Como fornecer CRUD (criar, ler, atualizar e excluir) suporte de entrada de formulário de dados](# "fornecer CRUD (criar, ler, atualizar, excluir) dados de formulário entrada de suporte")
6. [Como usar ViewData e implementar classes ViewModel](# "usar ViewData e implementar ViewModel Classes")
7. [Como usar novamente a interface do usuário usando páginas mestras e existe meio-termo](# "reutilização da interface do usuário usando páginas mestras e existe meio-termo")
8. [Como implementar a paginação de dados eficiente](# "implementar eficiente dados paginação")
9. [Como proteger aplicativos usando a autenticação e autorização](# "aplicativos usando autenticação e autorização seguras")
10. [Como usar o AJAX para fornecer atualizações dinâmicas](# "usar AJAX para fornecer atualizações dinâmicas")
11. [Como usar o AJAX para implementar cenários de mapeamento](# "usar AJAX para implementar cenários de mapeamento")
12. [Como habilitar o teste de unidade automatizado](# "habilitar teste de unidade automatizado")

Você pode criar sua própria cópia do NerdDinner do zero ao concluir cada etapa, passo a passo neste capítulo. Como alternativa, você pode baixar uma versão completa do código-fonte aqui: [NerdDinner no GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Você também pode também [baixar uma versão gratuita do PDF deste tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) se você deseja ler o tutorial offline.

Você pode usar o Visual Studio 2008 ou o livre Visual Web Developer 2008 Express para compilar o aplicativo. Você pode usar o SQL Server ou o livre SQL Server Express para o banco de dados.

Você pode instalar o ASP.NET MVC, Visual Web Developer 2008 Express e SQL Server Express (gratuitas) o usando V2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Agora vamos começar...

Agora que já abordamos o que é NerdDinner, vamos acumular nosso capas e escrever um código.

Vamos começar usando arquivo -&gt;novo projeto do Visual Studio para criar o aplicativo NerdDinner.

> [!div class="step-by-step"]
> [Avançar](create-a-new-aspnet-mvc-project.md)
