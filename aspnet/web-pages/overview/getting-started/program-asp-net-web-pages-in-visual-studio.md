---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: "Programação ASP.NET Web Pages (Razor) com o Visual Studio | Microsoft Docs"
author: tfitzmac
description: "Este apêndice explica como você pode usar o Visual Studio 2010 ou Visual Web Developer 2010 Express para programa páginas da Web ASP.NET com a sintaxe do Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programação de páginas da Web do ASP.NET (Razor) usando o Visual Studio
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como você pode usar o Visual Studio ou Visual Web Developer Express para sites de páginas da Web do ASP.NET (Razor) do programa.
> 
> Você aprenderá
> 
> - O que você precisa instalar (se houver alguma) para trabalhar com páginas da Web do ASP.NET na sua versão do Visual Studio.
> - Como adicionar suporte para páginas da Web do ASP.NET para o Visual Web Developer 2010 Express.
> - Como usar recursos do Visual Studio para trabalhar com páginas ASP.NET Razor, incluindo IntelliSense e o depurador.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
> - Visual Studio 2013
> - O WebMatrix 3
>   
> 
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET, o Visual Studio 2012, o Visual Studio 2010 e o WebMatrix 2.


Você pode programar páginas da Web ASP.NET com sintaxe do Razor usando o WebMatrix ou outros editores de códigos. Você também pode usar o Microsoft Visual Studio, que é um ambiente completo de desenvolvimento integrado (IDE) que fornece um conjunto poderoso de ferramentas para criar vários tipos de aplicativos (não apenas sites). Para trabalhar com páginas ASP.NET Razor, você pode usar uma das edições do Visual Studio completas ou livre [Visual Studio Express para Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.

Dois recursos particularmente útil que o Visual Studio fornece para programação com páginas da web de ASP.NET Razor são:

- *IntelliSense*. O recurso IntelliSense embutido no Visual Studio é mais abrangente do que o IntelliSense no WebMatrix.
- *Depurador*. O depurador permite que você solucione problemas de seu código parar um programa enquanto está em execução, examinando variáveis e percorrendo o código linha por linha.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Usando o Visual Studio com versões diferentes de páginas da Web do ASP.NET

Visual Studio 2012 e o Visual Studio 2013 incluem suporte para páginas da Web do ASP.NET. (Os pacotes que são necessários para dar suporte a páginas da Web ASP.NET são instalados quando você instala o Visual Studio).

Visual Studio 2010 não inclui suporte por padrão para páginas da Web do ASP.NET. Para usar as páginas da Web ASP.NET com o Visual Studio 2010, você deve instalar o pacote do ASP.NET MVC. Para obter 2 de páginas da Web do ASP.NET, você instalar o ASP.NET MVC 4.

A tabela a seguir resume o suporte para páginas da Web do ASP.NET em diferentes versões do Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **Páginas da Web do ASP.NET 2** | Instalar o ASP.NET MVC 4 | (Incluído) | (Incluído) |
| **ASP.NET Web Pages 3** |  | 3 NuGet por meio de páginas de atualização da Web do ASP.NET | (Incluído) |

Para trabalhar com o Visual Studio 2010, consulte [instalar suporte para páginas da Web ASP.NET no Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Inicie o Visual Studio do WebMatrix

Se você tiver iniciado um projeto no WebMatrix e deseja alternar para o Visual Studio, o WebMatrix oferece um botão para abrir facilmente o projeto no Visual Studio. Você deve ter o Visual Studio instalado em seu computador para que esse botão seja habilitado. A imagem a seguir mostra o botão no WebMatrix.

![Inicie o Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Quando você clica no botão, o projeto é aberto no Visual Studio. Você pode alternar entre o WebMatrix e Visual Studio sem problemas. Você será notificado se todos os arquivos foram alterados em outro ambiente e precisam ser recarregado para obter as alterações mais recentes.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Criando Site ASP.NET Razor no Visual Studio

Para criar um site ASP.NET Razor no Visual Studio:

1. Inicie o Visual Studio ou Visual Web Developer.
2. No **arquivo** menu, clique em **novo Site**.

    ![Criar novo site](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. No **novo Site** caixa de diálogo, selecione o idioma a ser usado (Visual c# ou Visual Basic).
4. Selecione o **Site da Web do ASP.NET (Razor)** modelo.

    ![site do Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Clique em **OK**.

O novo projeto existe e é populado com algumas páginas da web padrão para ajudá-lo a começar.

### <a name="using-intellisense"></a>Usando IntelliSense

Agora que você criou um site, você pode ver como IntelliSense funciona no Visual Studio.

1. No site que você acabou de criar, abrir o *cshtml* página.
2. Após o `<h3>` marcas da página, digite `@ServerInfo.` (incluindo o ponto final). Observe como IntelliSense exibe os métodos disponíveis para o `ServerInfo` auxiliar em uma lista suspensa. 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Selecione o `GetHtml` método na lista e pressione Enter. IntelliSense preenche automaticamente o método. (Como com qualquer método em c#, você deve adicionar `()` caracteres após o método.)  
 O código completo para o `GetHtml` método é semelhante ao exemplo a seguir:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Pressione Ctrl + F5 para executar a página. Esta é a página de aparência quando exibido em um navegador: 

    ![página padrão no navegador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Feche o navegador.

### <a name="using-the-debugger"></a>Usando o depurador

1. Na parte superior do *cshtml* página após a linha que começa com `Page.Title`, adicione a seguinte linha de código: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Na margem cinza do Editor para a esquerda do código, clique em Avançar essa nova linha para adicionar um *ponto de interrupção*. Um ponto de interrupção é um marcador que informa o depurador para interromper a execução do programa nesse momento para ver o que está acontecendo.

    ![Conjunto de ponto de interrupção](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Remova a chamada para o `ServerInfo.GetHtml` método e adicione uma chamada para o `@myTime` variável em seu lugar. Essa chamada exibe o valor de tempo atual que é retornado pela nova linha de código.
4. Pressione F5 para executar a página no depurador. Interrompe a página no ponto de interrupção que você definir. A imagem a seguir mostra como a página aparece no editor com o ponto de interrupção (amarelo). 

    ![ponto de interrupção de depuração](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Na barra de ferramentas de depuração, clique o **intervir** botão (ou pressione F11) para executar a próxima linha de código. Cada vez que você clicar nesse botão Avançar a execução para a próxima linha de código.

    ![Step-into do botão](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Examinar o valor da `myTime` variável, mantendo o ponteiro do mouse sobre ele ou inspecionando os valores exibidos na **locais** e **pilha de chamadas** windows. O Visual Studio exibe o valor da variável.

    ![mostra o tempo valor](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Quando você terminar de examinar a variável e percorrer o código, pressione F5 para continuar a execução da página sem parar em cada linha. Quando tiver terminado de depurar através de todo o código, o navegador exibe a página.

Para saber mais sobre o depurador e sobre como depurar código no Visual Studio, consulte [passo a passo: Depurando páginas da Web no Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Usando o Razor em projetos do ASP.NET MVC com o Visual Studio

A sintaxe do Razor é também usada extensivamente em projetos do ASP.NET MVC. MVC é uma maneira eficiente com base em padrões para criar sites dinâmicos. Se seu site de páginas da Web ASP.NET torna-se difíceis de manter, convém considerar convertê-la em um aplicativo ASP.NET MVC. Para obter um exemplo de criação de um aplicativo MVC, consulte [guia de Introdução ao ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalando suporte para páginas da Web ASP.NET no Visual Studio 2010

Esta seção mostra como instalar o Visual Web Developer Express 2010 e as ferramentas de páginas da Web do ASP.NET para o Visual Studio.

1. Se você ainda não tiver o Web Platform Installer, baixe-o da URL a seguir:

    [https://www.microsoft.com/Web/downloads/Platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Execute o Web Platform Installer.
3. Clique o **produtos** guia.

    ![Guia de produtos do WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Procurar **ASP.NET MVC 4** (para 2 de páginas da Web do ASP.NET) e, em seguida, clique em **adicionar**. Esses produtos incluem ferramentas do Visual Studio para criar sites ASP.NET Razor.

    ![Opções de instalação do WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Clique em **instalar** para concluir a instalação.
