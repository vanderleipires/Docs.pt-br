---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: "Introdução ao ASP.NET MVC 3 (VB) | Microsoft Docs"
author: Rick-Anderson
description: "Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f0717e8d635818322be8b3242efe756a20351340
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Introdução ao ASP.NET MVC 3 (VB)
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com VB.NET código-fonte está disponível para acompanhar este tópico. [Baixe a versão VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir c#, alterne para o [versão c#](../cs/intro-to-aspnet-mvc-3.md) deste tutorial.


Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:

- [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)

Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Um projeto do Visual Web Developer ao código-fonte VB está disponível para acompanhar este tópico. [Baixe a versão do VB](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Se você preferir CSharp, alterne para o [versão CSharp](../cs/intro-to-aspnet-mvc-3.md) deste tutorial.

## <a name="what-youll-build"></a>O que você vai criar

Você implementará um simples aplicativo de listagem de filme que dá suporte à criação, edição e listando filmes de um banco de dados. Abaixo estão as duas capturas de tela do aplicativo que você criará. Ele inclui uma página que exibe uma lista de filmes de um banco de dados:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

O aplicativo também permite adicionar, editar e excluir filmes, assim como para ver detalhes sobre os problemas individuais. Todos os cenários de entrada de dados incluem validação para garantir que os dados armazenados no banco de dados estão corretos.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Você vai aprender as habilidades

Aqui está o que você aprenderá:

- Como criar um novo projeto ASP.NET MVC
- Como criar um novo banco de dados usando primeiro código do Entity Framework
- Como criar o ASP.NET MVC controladores e exibições
- Como recuperar e exibir dados
- Como editar dados e habilitar a validação de dados

## <a name="getting-started"></a>Guia de Introdução

Inicie executando o Visual Web Developer 2010 Express ("VWD" em resumo) e selecione **novo projeto** do **iniciar** página.

O Visual Web Developer é um IDE ou ambiente de desenvolvimento integrado. Como usar o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos. No Visual Web Developer, há uma barra de ferramentas na parte superior, mostrando várias opções disponíveis para você. Há também um menu que oferece uma maneira de realizar tarefas no IDE. (Por exemplo, em vez de selecionar **novo projeto** do **iniciar** página, você pode usar o menu e selecione **arquivo** &gt; **denovoprojeto**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Você pode criar aplicativos usando o Visual Basic ou Visual c# como linguagem de programação. Para este tutorial, selecione Visual Basic à esquerda e selecione **aplicativo Web do ASP.NET MVC 3**. Nomeie o projeto "MvcMovie" e, em seguida, clique em **Okey**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

No **novo projeto do ASP.NET MVC 3** caixa de diálogo, selecione **aplicativo de Internet**. Deixe **Razor** como o mecanismo de exibição padrão.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Clique em **OK**. O Visual Web Developer usado um modelo padrão para o projeto ASP.NET MVC que você acabou de criar, para que você tenha um aplicativo em execução no momento sem fazer nada! Este é um simples "Hello World!" projeto e ele á um bom lugar para iniciar seu aplicativo.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

No menu **Depuração**, selecione **Iniciar Depuração**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Observe que o atalho de teclado para iniciar a depuração F5.

F5 faz com que o Visual Web Developer iniciar um servidor de desenvolvimento web e executar seu aplicativo da web. Em seguida, VWD inicia um navegador e abre a página inicial do aplicativo. Observe que a barra de endereço do navegador informa `localhost` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso, é executado o aplicativo que você acabou de criar. Quando VWD é executado em um projeto da web, uma porta aleatória é usada para o projeto. Na imagem abaixo, o número da porta aleatória é 43246. Seu projeto provavelmente usará um número de porta diferente.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Fora da caixa de nesse modelo padrão fornece dois páginas visitar e uma página de logon básica. Vamos alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo. Feche seu navegador e vamos alterar o código.

>[!div class="step-by-step"]
[Avançar](adding-a-controller.md)
