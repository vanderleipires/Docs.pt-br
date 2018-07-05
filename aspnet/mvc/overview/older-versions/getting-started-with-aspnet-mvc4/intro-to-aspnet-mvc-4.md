---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introdução ao ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Uma versão atualizada, se este tutorial está disponível aqui usando o Visual Studio 2013. O novo tutorial usa o ASP.NET MVC 5, que fornece muitos aprimoramentos em t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51e469e131b083325bc565530d91173887769ab0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395808"
---
<a name="intro-to-aspnet-mvc-4"></a>Introdução ao ASP.NET MVC 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Uma versão atualizada, se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). O novo tutorial usa o ASP.NET MVC 5, que fornece muitos aprimoramentos ao longo deste tutorial.
> 
> Este tutorial lhe ensinará os conceitos básicos da criação de um aplicativo Web do ASP.NET MVC 4 usando o Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou Visual Web Developer 2010 Express Service Pack 1. Visual Studio 2012 é recomendado, você não precisará instalar qualquer coisa para concluir o tutorial. Se você estiver usando o Visual Studio 2010, você deve instalar componentes abaixo. Você pode instalar todos eles clicando nos links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalador WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale o [installer WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) e o: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Um projeto do Visual Web Developer com código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> No tutorial, você executar o aplicativo no Visual Studio. Você também pode fazer o aplicativo disponível na Internet, implantá-lo em um provedor de hospedagem. A Microsoft oferece a hospedagem de web gratuita para até 10 sites em uma [conta gratuita do Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para obter informações sobre como implantar um projeto de web do Visual Studio para um Site do Windows Azure, consulte [criar e implantar um site da web ASP.NET e o banco de dados SQL com o Visual Studio](https://docs.microsoft.com/dotnet/azure/). Esse tutorial também mostra como usar o Entity Framework Code First Migrations para implantar seu banco de dados do SQL Server no Windows Azure SQL Database (anteriormente conhecido como SQL Azure).
> 
> Este tutorial foi escrito por Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>O que você vai criar

> [!NOTE]
> Uma versão atualizada, se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). O novo tutorial usa o ASP.NET MVC 5, que fornece muitos aprimoramentos ao longo deste tutorial.


Você implementará um simples aplicativo de listagem de filmes que dá suporte à criação, edição, pesquisando e listagem de filmes de um banco de dados. Abaixo estão as duas capturas de tela do aplicativo que você criará. Ele inclui uma página que exibe uma lista de filmes de um banco de dados:

![](intro-to-aspnet-mvc-4/_static/image1.png)

O aplicativo também permite adicionar, editar e excluir filmes, bem como ver detalhes sobre itens individuais. Todos os cenários de entrada de dados incluem a validação para garantir que os dados armazenados no banco de dados estão corretos.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Guia de Introdução

Comece executando o Visual Studio Express 2012 ou Visual Web Developer 2010 Express. A maioria das capturas de tela do uso dessa série Visual Studio Express 2012, mas você pode concluir este tutorial com o Visual Studio 2010 SP1, o Visual Studio 2012, Visual Studio Express 2012 ou Visual Web Developer 2010 Express. Selecione **novo projeto** da **iniciar** página.

Visual Studio é um IDE ou ambiente de desenvolvimento integrado. Assim como usar o Microsoft Word para gravar documentos, você usará um IDE para criar aplicativos. No Visual Studio, há uma barra de ferramentas na parte superior mostrando várias opções disponíveis para você. Há também um menu que fornece outra maneira de executar tarefas no IDE. (Por exemplo, em vez de selecionar **novo projeto** da **inicie** página, você pode usar o menu e selecione **arquivo** &gt; **denovoprojeto**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Você pode criar aplicativos usando o Visual Basic ou Visual c# como linguagem de programação. Selecione Visual c# à esquerda e, em seguida, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto &quot;MvcMovie&quot; e, em seguida, clique em **Okey**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **aplicativo de Internet**. Deixe **Razor** como o mecanismo de exibição padrão.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Clique em **OK**. Visual Studio usou um modelo padrão para o projeto do ASP.NET MVC que você acabou de criar, portanto, você tem agora um aplicativo de trabalho sem fazer nada! Isso é uma simples &quot;Olá, mundo!&quot; projeto e é de um bom lugar para iniciar o aplicativo.

![](intro-to-aspnet-mvc-4/_static/image6.png)

No menu **Depuração**, selecione **Iniciar Depuração**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Observe que o atalho de teclado para iniciar a depuração F5.

F5 faz com que o Visual Studio iniciar o IIS Express e executar seu aplicativo web. Em seguida, o Visual Studio inicia um navegador e abre a página inicial do aplicativo. Observe que a barra de endereços do navegador diz `localhost` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso está executando o aplicativo que você acabou de criar. Quando o Visual Studio executa um projeto da web, uma porta aleatória é usada para o servidor web. Na imagem abaixo, o número da porta é 41788. Quando você executa o aplicativo, você provavelmente verá um número de porta diferente.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Prontos nesse modelo padrão fornece páginas Home, contato e sobre. Ele também fornece suporte para se registrar e faça logon no e vincula-se ao Facebook e Twitter. A próxima etapa é alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC. Feche seu navegador e vamos alterar algum código.

> [!div class="step-by-step"]
> [Avançar](adding-a-controller.md)
