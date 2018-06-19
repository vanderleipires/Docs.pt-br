---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introdução ao ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Uma versão atualizada se este tutorial está disponível aqui usando o Visual Studio 2013. O novo tutorial usa o ASP.NET MVC 5, que fornece muitas melhorias em t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 519bac22ba2607931c5f3123b9b567859a2b3d1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869043"
---
<a name="intro-to-aspnet-mvc-4"></a>Introdução ao ASP.NET MVC 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Uma versão atualizada se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). O novo tutorial usa ASP.NET MVC 5, que fornece muitas melhorias sobre este tutorial.
> 
> Este tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo Web do ASP.NET MVC 4 usando o Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou Visual Web Developer 2010 Express Service Pack 1. O Visual Studio 2012 é recomendada, você não precisará instalar nada para concluir o tutorial. Se você estiver usando o Visual Studio 2010, você deve instalar os componentes abaixo. Você pode instalar todos eles clicando nos links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalador WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale o [instalador WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) e: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Um projeto do Visual Web Developer ao código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> No tutorial você executar o aplicativo no Visual Studio. Você também pode tornar o aplicativo disponível pela Internet por implantá-lo em um provedor de hospedagem. A Microsoft oferece gratuito da web de hospedagem para até 10 sites da web em um [conta de avaliação do Windows Azure gratuita](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para obter informações sobre como implantar um projeto da web do Visual Studio para um Site do Windows Azure, consulte [criar e implantar um site da web ASP.NET e o banco de dados SQL com o Visual Studio](https://docs.microsoft.com/dotnet/azure/). Esse tutorial também mostra como usar o Entity Framework Code First Migrations para implantar seu banco de dados do SQL Server para o banco de dados de SQL do Windows Azure (antigo SQL Azure).
> 
> Este tutorial foi escrito de Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>O que você vai criar

> [!NOTE]
> Uma versão atualizada se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). O novo tutorial usa ASP.NET MVC 5, que fornece muitas melhorias sobre este tutorial.


Você implementará um simples aplicativo de listagem de filme que dá suporte à criação, edição, pesquisa e listando filmes de um banco de dados. Abaixo estão as duas capturas de tela do aplicativo que você criará. Ele inclui uma página que exibe uma lista de filmes de um banco de dados:

![](intro-to-aspnet-mvc-4/_static/image1.png)

O aplicativo também permite adicionar, editar e excluir filmes, assim como para ver detalhes sobre os problemas individuais. Todos os cenários de entrada de dados incluem validação para garantir que os dados armazenados no banco de dados estão corretos.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Guia de Introdução

Comece executando o Visual Studio Express 2012 ou Visual Web Developer 2010 Express. A maioria das capturas de tela esse uso de série do Visual Studio Express 2012, mas você pode concluir este tutorial com Visual Studio 2010 SP1, o Visual Studio 2012, o Visual Studio Express 2012 ou Visual Web Developer 2010 Express. Selecione **novo projeto** do **iniciar** página.

O Visual Studio é um IDE ou ambiente de desenvolvimento integrado. Como usar o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos. No Visual Studio, há uma barra de ferramentas na parte superior, mostrando várias opções disponíveis para você. Há também um menu que oferece uma maneira de realizar tarefas no IDE. (Por exemplo, em vez de selecionar **novo projeto** do **iniciar** página, você pode usar o menu e selecione **arquivo** &gt; **denovoprojeto**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Você pode criar aplicativos usando o Visual Basic ou Visual c# como linguagem de programação. Selecione o Visual C# à esquerda e, em seguida, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto &quot;MvcMovie&quot; e, em seguida, clique em **Okey**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **aplicativo de Internet**. Deixe **Razor** como o mecanismo de exibição padrão.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Clique em **OK**. O Visual Studio usado um modelo padrão para o projeto ASP.NET MVC que você acabou de criar, para que você tenha um aplicativo em execução no momento sem fazer nada! Este é um simples &quot;Olá, mundo!&quot; projeto e é de um bom lugar para iniciar seu aplicativo.

![](intro-to-aspnet-mvc-4/_static/image6.png)

No menu **Depuração**, selecione **Iniciar Depuração**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Observe que o atalho de teclado para iniciar a depuração F5.

F5 faz com que o Visual Studio para iniciar o IIS Express e execute seu aplicativo da web. Visual Studio, em seguida, inicia um navegador e abre a página inicial do aplicativo. Observe que a barra de endereço do navegador informa `localhost` e não algo como `example.com`. Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso, é executado o aplicativo que você acabou de criar. Quando o Visual Studio executará um projeto da web, uma porta aleatória é usada para o servidor web. Na imagem abaixo, o número da porta é 41788. Quando você executa o aplicativo, você provavelmente verá um número de porta diferente.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Imediatamente esse modelo padrão fornece páginas Home, contatos e sobre. Ele também fornece suporte para registrar e fazer logon no e links para Facebook e Twitter. A próxima etapa é alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC. Feche seu navegador e vamos alterar o código.

> [!div class="step-by-step"]
> [Avançar](adding-a-controller.md)
