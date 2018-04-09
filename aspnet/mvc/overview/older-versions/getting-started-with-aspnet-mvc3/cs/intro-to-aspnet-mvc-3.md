---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introdução ao ASP.NET MVC 3 (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-c"></a>Introdução ao ASP.NET MVC 3 (c#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.
> 
> 
> Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer ao código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.


## <a name="what-youll-build"></a>O que você vai criar

Você implementará um simples aplicativo de listagem de filme que dá suporte à criação, edição e listando filmes de um banco de dados. Abaixo estão as duas capturas de tela do aplicativo que você criará. Ele inclui uma página que exibe uma lista de filmes de um banco de dados:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

O aplicativo também permite adicionar, editar e excluir filmes, assim como para ver detalhes sobre os problemas individuais. Todos os cenários de entrada de dados incluem validação para garantir que os dados armazenados no banco de dados estão corretos.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Você vai aprender as habilidades

Aqui está o que você aprenderá:

- Como criar um novo projeto ASP.NET MVC.
- Como criar o ASP.NET MVC controladores e exibições.
- Como criar um novo banco de dados usando o paradigma do Entity Framework Code First.
- Como recuperar e exibir dados.
- Como editar os dados e habilitar a validação de dados.

## <a name="getting-started"></a>Guia de Introdução

Inicie executando o Visual Web Developer 2010 Express ("Visual Web Developer" em resumo) e selecione **novo projeto** do **iniciar** página.

O Visual Web Developer é um IDE ou ambiente de desenvolvimento integrado. Como usar o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos. No Visual Web Developer, há uma barra de ferramentas na parte superior, mostrando várias opções disponíveis para você. Há também um menu que oferece uma maneira de realizar tarefas no IDE. (Por exemplo, em vez de selecionar **novo projeto** do **iniciar** página, você pode usar o menu e selecione **arquivo** &gt; **denovoprojeto**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Você pode criar aplicativos usando o Visual Basic ou Visual c# como linguagem de programação. Selecione o Visual C# à esquerda e, em seguida, selecione **aplicativo Web do ASP.NET MVC 3**. Nomeie o projeto "MvcMovie" e, em seguida, clique em **Okey**. (Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

No **novo projeto do ASP.NET MVC 3** caixa de diálogo, selecione **aplicativo de Internet**. Verificar **HTML5 Use marcação** e deixe **Razor** como o mecanismo de exibição padrão.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Clique em **OK**. O Visual Web Developer usado um modelo padrão para o projeto ASP.NET MVC que você acabou de criar, para que você tenha um aplicativo em execução no momento sem fazer nada! Este é um simples "Hello World!" projeto e ele á um bom lugar para iniciar seu aplicativo.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

No menu **Depuração**, selecione **Iniciar Depuração**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Observe que o atalho de teclado para iniciar a depuração F5.

F5 faz com que o Visual Web Developer iniciar um servidor de desenvolvimento web e executar seu aplicativo da web. Visual Web Developer inicia um navegador e abre a página inicial do aplicativo. Observe que a barra de endereço do navegador informa `localhost` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso, é executado o aplicativo que você acabou de criar. Quando o Visual Web Developer executa um projeto da web, uma porta aleatória é usada para o servidor web. Na imagem abaixo, o número da porta aleatória é 43246. Quando você executa o aplicativo, você provavelmente verá um número de porta diferente.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Imediato desse modelo padrão fornece duas páginas para visitar e uma página de logon básica. A próxima etapa é alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo. Feche seu navegador e vamos alterar o código.

> [!div class="step-by-step"]
> [Avançar](adding-a-controller.md)
