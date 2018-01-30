---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: "Introdução ao ASP.NET MVC | Microsoft Docs"
author: shanselman
description: "Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que leituras e gravações de banco de dados."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 08c30f4aab77bff64ed3ab874d13cc5dc863fc99
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
<a name="intro-to-aspnet-mvc"></a>Introdução ao ASP.NET MVC
====================
por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Uma versão atualizada se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). O novo tutorial usa ASP.NET MVC 5, que fornece muitas melhorias sobre este tutorial.
> 
> 
> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados. Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Vamos criar nossa primeira usando o aplicativo Web ASP.NET MVC [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Faremos um pequeno aplicativo de lista de filme que vamos criar e lista de filmes.

## <a name="what-youll-build"></a>O que você vai criar

Aqui estão duas capturas de tela do aplicativo que você criará. Você terá uma tabela simples de filmes com várias colunas.

[![Lista de filmes - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

E você terá um formulário de criação para que possa adicionar filmes à lista.

[![Criar um filme - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Você vai aprender as habilidades

Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Visual Studio. Você aprenderá:

- Como criar um novo projeto ASP.NET MVC
- Como criar um novo banco de dados com o SQL Server
- Como criar controladores do ASP.NET MVC e modos de exibição
- Como recuperar e exibir dados
- Como editar dados e habilitar a validação de dados
- Como atualizar o esquema de banco de dados

## <a name="get-started"></a>Introdução

Comece executando o Visual Web Developer 2010 Express (vou chamá-lo "VWD" de agora em diante) e selecione Novo projeto na tela Iniciar.

O Visual Web Developer é um IDE ou ambiente de desenvolvedor integrado. Como usar o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos. Há uma barra de ferramentas na parte superior, mostrando várias opções disponíveis para você, bem como o menu você também poderia ter usado para selecionar arquivo | Novo projeto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Criando seu primeiro aplicativo

Você pode criar aplicativos usando o Visual Basic ou Visual c#. Por enquanto, selecione Visual C# à esquerda, em seguida, escolha "Aplicativo de Web do ASP.NET MVC 2". Nomeie o projeto "Filmes" e clique em Okey.

[![Novo projeto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

No lado direito é o Gerenciador de soluções mostrando todos os arquivos e pastas em seu aplicativo. A janela grande no meio é onde você pode editar o seu código e passa a maior parte do tempo. O Visual Studio usado um modelo padrão para o projeto ASP.NET MVC que você acabou de criar, para que você tenha um aplicativo em execução no momento sem fazer nada! Este é um simples "Hello World! projeto e é um bom ponto de partida para nosso aplicativo.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Selecione o botão "reproduzir" na barra de ferramentas.

![Iniciar a depuração](getting-started-with-mvc-part1/_static/image11.png)

É uma seta verde apontando para a direita que será compilado seu programa e iniciar o aplicativo em um navegador da web.

*Observação: Você pode em vez disso, pressione F5 no teclado, ou selecione Debug -&gt;iniciar depuração no menu "Debug".*

Isso fará com que o Visual Web Developer iniciar um servidor web de desenvolvimento e executar o nosso aplicativo web (não há nenhuma configuração ou etapas manuais necessárias para habilitar isso). Em seguida, ele inicia um navegador e configurá-lo para procurar a home page do aplicativo. Abaixo, observe que a barra de endereços do navegador diz "localhost" e não algo como example.com. Isso ocorre porque o localhost sempre aponta para o seu próprio computador local - que nesse caso, é executado o aplicativo que acabamos de criar.

[![Página inicial](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Fora da caixa de nesse modelo padrão fornece dois páginas visitar e uma página de logon básica. Vamos alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo. Feche seu navegador e permite alterar o código.

>[!div class="step-by-step"]
[Avançar](getting-started-with-mvc-part2.md)
