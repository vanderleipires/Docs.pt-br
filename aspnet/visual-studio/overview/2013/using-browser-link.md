---
uid: visual-studio/overview/2013/using-browser-link
title: Usando o Link de navegador no Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 14f67d81a5b460da591b8fb27fedf53d228e7717
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-browser-link-in-visual-studio-2013"></a>Usando o Link de navegador no Visual Studio 2013
====================
por [Mike Wasson](https://github.com/MikeWasson)

Link do navegador é um novo recurso no Visual Studio 2013 que cria um canal de comunicação entre o ambiente de desenvolvimento e um ou mais navegadores da web. Você pode usar o Link do navegador para atualizar seu aplicativo da web em vários navegadores de uma vez, que é útil para testes entre navegadores.

- [Atualizar do navegador](#browser-refresh)
- [Exibir o painel de Link do navegador](#dashboard)
- [Habilitando o Link do navegador para arquivos HTML estáticos](#static-html)
- [Desabilitando o Link do navegador](#disabling)
- [Como funciona?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Atualizar do navegador

Com a atualização de navegador, você pode atualizar vários navegadores que estão conectados ao Visual Studio por meio do Link do navegador.

Para usar a atualização do navegador, primeiro crie um aplicativo ASP.NET, usando qualquer um dos modelos de projeto. Depure o aplicativo pressionando F5 ou clicando no ícone de seta na barra de ferramentas:

![](using-browser-link/_static/image1.png)

Você também pode usar a lista suspensa para selecionar um navegador específico para depuração.

![](using-browser-link/_static/image2.png)

Para depurar com vários navegadores, selecione **procurar com**. No **procurar com** caixa de diálogo, mantenha pressionada a tecla CTRL para selecionar mais de um navegador. Clique em **procurar** para depurar com os navegadores selecionados. Link do navegador também funcionará se você inicia um navegador de fora do Visual Studio e navegue até a URL do aplicativo.

![](using-browser-link/_static/image3.png)

Os controles de Link de navegador estão localizados no menu suspenso com o ícone de seta circular. O ícone de seta é o **atualização** botão.

![](using-browser-link/_static/image4.png)

Para ver quais navegadores conectados, passe o mouse sobre o **atualização** botão durante a depuração. Os navegadores conectados são mostrados em uma janela de dica de ferramenta.

![](using-browser-link/_static/image5.png)

Para atualizar os navegadores conectados, clique o **atualização** botão ou pressione CTRL + ALT + ENTER. Por exemplo, a captura de tela a seguir mostra um projeto do ASP.NET, criado com o modelo de projeto MVC 5. Você pode ver o aplicativo em execução em duas navegadores na parte superior. Na parte inferior, o projeto é aberto no Visual Studio.

![](using-browser-link/_static/image6.png)

No Visual Studio, eu alterei a &lt;h1&gt; título da home page:

![](using-browser-link/_static/image7.png)

Quando cliquei o **atualização** botão, a alteração é exibida em ambas as janelas do navegador:

![](using-browser-link/_static/image8.png)

**Observações**

- Para habilitar o Link do navegador, defina `debug=true` no [ &lt;compilação&gt; ](https://msdn.microsoft.com/en-us/library/s10awwz0(v=vs.85).aspx) elemento no arquivo de Web. config do projeto.
- O aplicativo deve estar em execução no host local.
- O aplicativo deve direcionar o .NET 4.0 ou posterior.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Exibir o painel de Link do navegador

O painel de Link do navegador mostra informações sobre as conexões de Link do navegador. Para exibir o painel de controle, selecione o menu suspenso de Link do navegador (a pequena seta ao lado de **atualização** botão). Em seguida, clique em **painel de Link do navegador**.

![](using-browser-link/_static/image9.png)

O painel lista os navegadores conectados e a URL para o qual cada navegador navegou.

![](using-browser-link/_static/image10.png)

O **pré-requisitos** seção mostra as etapas necessárias para habilitar o Link do navegador para o projeto. Por exemplo, a captura de tela a seguir mostra um projeto em que "debug" é definida como false no arquivo Web. config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Habilitando o Link do navegador para arquivos HTML estáticos

Para habilitar o Link do navegador para arquivos HTML estáticos, adicione o seguinte ao arquivo Web. config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Por motivos de desempenho, remova essa configuração quando você publicar seu projeto.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Desabilitando o Link do navegador

Link do navegador é habilitado por padrão. Há várias maneiras para desabilitá-lo:

- No menu suspenso de Link do navegador, desmarque a opção **habilitar o Link do navegador**. 

    ![](using-browser-link/_static/image12.png)
- No arquivo Web. config, adicione uma chave chamada "vs: EnableBrowserLink" com o valor "false" na seção appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- No arquivo Web. config, defina a depuração como false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Como funciona?

Link do navegador usa [SignalR](../../../signalr/index.md) para criar um canal de comunicação entre o navegador e o Visual Studio. Quando o Link do navegador é habilitado, o Visual Studio atua como um servidor de SignalR que vários clientes (navegadores) podem se conectar ao. Link do navegador também registra um módulo HTTP com o ASP.NET. Esse módulo injeta especial &lt;script&gt; referências em cada solicitação de página do servidor. Você pode ver as referências de script selecionando "Exibir fonte" no navegador.

![](using-browser-link/_static/image13.png)

Os arquivos de origem não são modificados. O módulo HTTP injeta as referências de script dinamicamente.

Como o código do lado do navegador é todo JavaScript, ele funciona em todos os navegadores que [SignalR dá suporte a](../../../signalr/overview/getting-started/supported-platforms.md), sem a necessidade de qualquer plug-in de navegador.
