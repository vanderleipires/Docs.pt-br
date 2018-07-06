---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introdução ao ASP.NET MVC | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: ee39cf78835767d73bd9a1d4c9a2c4ca75aca5fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808856"
---
<a name="intro-to-aspnet-mvc"></a>Introdução ao ASP.NET MVC
====================
por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Uma versão atualizada, se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). O novo tutorial usa o ASP.NET MVC 5, que fornece muitos aprimoramentos ao longo deste tutorial.
> 
> 
> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que lê e grava de um banco de dados. Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Vamos criar nosso primeiro aplicativo Web ASP.NET MVC usando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Vamos fazer um pequeno aplicativo de lista de filmes que vamos criar e lista de filmes.

## <a name="what-youll-build"></a>O que você vai criar

Aqui estão as duas capturas de tela do aplicativo que você criará. Você terá uma tabela simples de filmes com várias colunas.

[![Lista de filmes - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

E você terá um formulário de criação para poder adicionar à lista de filmes.

[![Criar um filme - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Habilidades que você aprenderá

Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web do ASP.NET MVC usando o Visual Studio. Você aprenderá:

- Como criar um novo projeto ASP.NET MVC
- Como criar um novo banco de dados com o SQL Server
- Como criar exibições e controladores de MVC do ASP.NET
- Como recuperar e exibir dados
- Como editar dados e habilitar a validação de dados
- Como atualizar o esquema de banco de dados

## <a name="get-started"></a>Introdução

Comece executando o Visual Web Developer 2010 Express (chamarei de "VWD" de agora em diante) e selecione Novo projeto na tela Iniciar.

O Visual Web Developer é um IDE ou ambiente de desenvolvimento integrado. Assim como usar o Microsoft Word para gravar documentos, você usará um IDE para criar aplicativos. Há uma barra de ferramentas na parte superior mostrando várias opções disponíveis para você, bem como no menu que você também poderia ter usado para selecionar o arquivo | Novo projeto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Você pode criar aplicativos usando o Visual Basic ou Visual c#. Por enquanto, selecione Visual c# à esquerda, em seguida, escolher "Aplicativo de Web do ASP.NET MVC 2". Nomeie o projeto "Filmes" e clique em Okey.

[![Novo projeto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

No lado direito é o Gerenciador de soluções mostrando todos os arquivos e pastas em seu aplicativo. A janela grande no meio é onde você pode editar seu código e passa a maior parte do seu tempo. Visual Studio usou um modelo padrão para o projeto do ASP.NET MVC que você acabou de criar, portanto, você tem agora um aplicativo de trabalho sem fazer nada! Isso é um simples "Hello World! projeto e é um bom lugar para começar nosso aplicativo.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Selecione o botão "reproduzir" na barra de ferramentas.

![Iniciar a depuração](getting-started-with-mvc-part1/_static/image11.png)

É uma seta verde apontando para a direita que será compilado seu programa e iniciar seu aplicativo em um navegador da web.

*Observação: Você pode em vez disso, pressione F5 no teclado, ou selecione Debug -&gt;iniciar depuração no menu "Debug".*

Isso fará com que o Visual Web Developer iniciar um servidor web de desenvolvimento e executar nosso aplicativo web (não há nenhuma configuração ou etapas manuais necessárias para habilitar isso). Em seguida, ele iniciará um navegador e configurá-lo para procurar a home page do aplicativo. Abaixo, observe que a barra de endereços do navegador diz "localhost" e não algo como exemplo.com. Isso ocorre porque o localhost sempre aponta para o seu próprio computador local – que nesse caso está executando o aplicativo que acabou de criar.

[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Fora da caixa desse modelo padrão fornece a você duas páginas para visitar e uma página de logon básica. Vamos alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo. Feche seu navegador e permite alterar o código.

> [!div class="step-by-step"]
> [Avançar](getting-started-with-mvc-part2.md)
