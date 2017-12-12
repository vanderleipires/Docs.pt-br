---
title: "Visão de geral de implantação e hospedagem – ASP.NET Core"
author: tdykstra
description: "Visão geral de como configurar ambientes de hospedagem e implantar aplicativos ASP.NET Core para eles."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: 0de459128426c4d027606951592b1fe3fdd24fd9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>Visão de geral de implantação e hospedagem para aplicativos ASP.NET Core

Aqui estão as principais etapas executadas para implantar um aplicativo ASP.NET Core em um ambiente de hospedagem:

* Publique o aplicativo em uma pasta no servidor de hospedagem.
* Configure um gerenciador de processo que inicia o aplicativo quando a solicitação chega e reinicia-o depois que ele falha ou que o servidor é reinicializado.
* Configure um proxy reverso que encaminha solicitações para o aplicativo.

## <a name="publish-to-a-folder"></a>Publicar em uma pasta 

O comando [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) da CLI compila o código do aplicativo e copia os arquivos necessários para executar o aplicativo em uma pasta *publish*. Quando você implanta do Visual Studio, a etapa `dotnet publish` é feita para você automaticamente antes que os arquivos sejam copiados para o destino da implantação.

### <a name="folder-contents"></a>Conteúdo da pasta

A pasta *publicar* contém arquivos *.exe* e *.dll* para o aplicativo, as respectivas dependências e, opcionalmente, o tempo de execução do .NET.

Um aplicativo .NET Core pode ser publicado como *autocontido* ou *dependente de estrutura*. Se o aplicativo é autocontido, os arquivos *.dll* que contêm o tempo de execução do .NET são incluídos na pasta *publish*.  Se o aplicativo depende da estrutura, os arquivos de tempo de execução do .NET não são incluídos porque o aplicativo tem uma referência para uma versão do .NET que está instalada no computador. O modelo de implantação padrão é dependente da estrutura. Para obter mais informações, consulte [Implantação de aplicativos .NET Core](https://docs.microsoft.com/dotnet/articles/core/deploying/index).

Além de arquivos *.exe* e *.dll*, a pasta *publish* para um aplicativo ASP.NET Core normalmente contém arquivos de configuração, ativos estáticos e exibições do MVC.  Para obter mais informações, consulte [Estrutura de diretórios](xref:hosting/directory-structure).

## <a name="set-up-a-process-manager"></a>Configure um gerenciador de processo

Um aplicativo ASP.NET Core é um aplicativo de console que deve ser iniciado quando um servidor é inicializado e reiniciado após falhas. Para automatizar inicializações e reinicializações, você precisa de um gerenciador de processo. Os gerenciadores de processo mais comuns para o ASP.NET Core são [Nginx](xref:publishing/linuxproduction) e [Apache](xref:publishing/apache-proxy) no Linux e [IIS](xref:publishing/iis) e [Serviço Windows](xref:hosting/windows-service) no Windows.

## <a name="set-up-a-reverse-proxy"></a>Configurar um proxy reverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se seu aplicativo usa o servidor Web [Kestrel](xref:fundamentals/servers/kestrel), você pode usar [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) ou [IIS](xref:publishing/iis) como um servidor proxy reverso. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar. Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se seu aplicativo usa o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) e será exposto à Internet, você deve usar [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) ou [IIS](xref:publishing/iis) como um servidor proxy reverso. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar. O principal motivo para usar um proxy reverso é a segurança. Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Usando o Visual Studio e o MSBuild para automatizar a implantação

A implantação muitas vezes requer tarefas adicionais além de copiar a saída do `dotnet publish` para um servidor. Por exemplo, você talvez queira incluir arquivos extras na pasta *publish* ou excluir arquivos dela. O Visual Studio usa o MSBuild para implantação da Web e você pode personalizar o MSBuild para fazer muitas outras tarefas durante a implantação. Para obter mais informações, consulte [Perfis de publicação no Visual Studio](xref:publishing/web-publishing-vs) e o livro [Usando MSBuild e o Team Foundation Build](http://msbuildbook.com/).

Você pode implantar diretamente do Visual Studio para o Serviço de Aplicativo do Azure usando [o recurso Publicar na Web](xref:tutorials/publish-to-azure-webapp-using-vs) ou usando o [suporte ao Git interno](xref:publishing/azure-continuous-deployment). O Visual Studio Team Services dá suporte à [implantação contínua para o Serviço de Aplicativo do Azure](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).

## <a name="publishing-to-azure"></a>Publicação no Azure

Confira as instruções sobre como publicar este aplicativo no Azure usando o Visual Studio em [Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).  O aplicativo também pode ser publicado no Azure através da [linha de comando](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Recursos adicionais

Para obter informações sobre como usar o Docker como um ambiente de hospedagem, consulte [Hospedar Aplicativos ASP.NET Core no Docker](xref:publishing/docker).
