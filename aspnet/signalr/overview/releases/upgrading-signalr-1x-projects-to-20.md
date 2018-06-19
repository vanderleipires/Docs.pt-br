---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Atualizando projetos do SignalR 1. x para a versão 2 | Microsoft Docs
author: pfletcher
description: Este tópico descreve como atualizar um projeto existente de 1. x do SignalR para SignalR 2. x e como solucionar problemas que podem surgir durante o processo de atualização...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505735"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Atualizando projetos do SignalR 1. x para a versão 2
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este tópico descreve como atualizar um projeto existente de 1. x do SignalR para SignalR 2. x e como solucionar problemas que podem surgir durante o processo de atualização.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Versões 1 e 2 do SignalR
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Usando o Visual Studio 2012 com este tutorial
> 
> 
> Para usar o Visual Studio 2012 com este tutorial, faça o seguinte:
> 
> - Atualização de seu [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) para a versão mais recente.
> - Instalar o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - No Web Platform Installer, procurar e instalar **ASP.NET e Web 2013.1 de ferramentas para Visual Studio 2012**. Isso irá instalar os modelos do Visual Studio para SignalR classes como **Hub**.
> - Alguns modelos (como **classe de inicialização OWIN**) não estará disponível; para isso, use um arquivo de classe em vez disso.
> 
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 oferece uma experiência de desenvolvimento consistente entre as plataformas de servidor usando [OWIN](http://owin.org). Este artigo descreve as etapas necessárias para atualizar um aplicativo de 1. x SignalR para a versão 2.

Enquanto ele é recomendável atualizar aplicativos para 2 SignalR, SignalR 1. x ainda suporte.

Este tutorial descreve como atualizar um aplicativo web hospedado para o SignalR 2. Agora há suporte para aplicativos de hospedagem interna (aquelas que hospedam um servidor em um aplicativo de console, o serviço do Windows ou outro processo) em 2 de SignalR. Para obter informações sobre como começar a criar um aplicativo hospedado automaticamente com o SignalR 2, consulte [Tutorial: SignalR auto-host](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Conteúdo

As seções a seguir descrevem as tarefas envolvidas na atualização de projetos de SignalR e como solucionar problemas que podem surgir.

- [Exemplo: Atualizar o tutorial de Introdução para o SignalR 2](#example)
- [Solucionando problemas de erros encontrados durante a atualização](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Exemplo: Atualizar o aplicativo tutorial de Introdução para SignalR 2

Nesta seção, você atualizará o aplicativo criado no [SignalR versão 1. x do Tutorial de Introdução](../older-versions/index.md) usar 2 SignalR.

1. Depois de concluir o tutorial de Introdução, clique com botão direito no projeto e selecione **propriedades**. Verifique o **framework de destino** é definido como **.NET Framework 4.5.**
2. Abra o Console do Gerenciador de pacotes. Remover SignalR 1. x do projeto usando o seguinte comando:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Instale o SignalR 2 usando o seguinte comando:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. Na página HTML, atualize a referência de script para o SignalR corresponder à versão do script agora está incluído no projeto.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Na classe de aplicativo global, remova a chamada para MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. A solução e selecione **adicionar**, **Novo Item...** . Na caixa de diálogo, selecione **classe de inicialização Owin**. Nomeie a nova classe **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Substitua o conteúdo do Startup.cs com o código a seguir:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    O atributo do assembly adiciona a classe para o processo de inicialização do Owin, que executa o `Configuration` método quando Owin é iniciado. Por sua vez chama o `MapSignalR` método, que cria rotas para todos os hubs de SignalR no aplicativo.
8. Execute o projeto e copie a URL da página principal para outro navegador ou no painel do navegador, como antes. Cada página solicitará um nome de usuário e as mensagens enviadas de cada página devem estar visíveis em ambos os painéis do navegador.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Solucionando problemas de erros encontrados durante a atualização

Esta seção descreve problemas que podem surgir durante a atualização. Para obter uma lista mais completa de erros e problemas que podem ocorrer com um aplicativo SignalR, consulte [solução de problemas do SignalR](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>'A chamada é ambígua entre os seguintes métodos ou propriedades'

Este erro ocorrerá se uma referência a `Microsoft.AspNet.SignalR.Owin` não é removido. Este pacote é substituído; a referência deve ser removida e a versão 1. x do pacote SelfHost deve ser desinstalada.

### <a name="hub-methods-fail-silently"></a>Métodos de Hub falharem em modo silencioso

Verifique se as referências de script no seu cliente até a data e que o `OwinStartup` atributo para sua classe de inicialização tem a classe correta e nomes de assembly para seu projeto. Além disso, tente abrir o endereço de hubs (hubs de signalr /) em seu navegador; qualquer erro que aparece oferecem mais informações sobre o que está acontecendo errado.
