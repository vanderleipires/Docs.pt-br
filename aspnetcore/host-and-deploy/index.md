---
title: Hospedar e implantar o ASP.NET Core
author: tdykstra
description: Aprenda como configurar ambientes de hospedagem e implantar aplicativos ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 515589e38a1ba121d365427b5fddac1b0e845b1f
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/30/2018
---
# <a name="host-and-deploy-aspnet-core"></a>Hospedar e implantar o ASP.NET Core

Em geral, implantar um aplicativo ASP.NET Core em um ambiente de hospedagem:

* Publique o aplicativo em uma pasta no servidor de hospedagem.
* Configure um gerenciador de processo que inicia o aplicativo quando a solicitação chega e reinicia-o depois que ele falha ou que o servidor é reinicializado.
* Configure um proxy reverso que encaminha solicitações para o aplicativo.

## <a name="publish-to-a-folder"></a>Publicar em uma pasta 

O comando [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) da CLI compila o código do aplicativo e copia os arquivos necessários para executar o aplicativo em uma pasta *publish*. Ao implantar usando o Visual Studio, a etapa [dotnet publish](/dotnet/core/tools/dotnet-publish) é executada automaticamente antes de os arquivos serem copiados para o destino da implantação.

### <a name="folder-contents"></a>Conteúdo da pasta

A pasta *publish* contém arquivos *.exe* e *.dll* para o aplicativo, as respectivas dependências e, opcionalmente, o tempo de execução do .NET.

Um aplicativo .NET Core pode ser publicado como *autocontido* ou *dependente de estrutura*. Se o aplicativo é autocontido, os arquivos *.dll* que contêm o tempo de execução do .NET são incluídos na pasta *publish*. Se o aplicativo depender da estrutura, os arquivos de tempo de execução do .NET não serão incluídos porque o aplicativo tem uma referência para uma versão do .NET que está instalada no servidor. O modelo de implantação padrão é dependente da estrutura. Para obter mais informações, consulte [Implantação de aplicativos .NET Core](/dotnet/articles/core/deploying/index).

Além de arquivos *.exe* e *.dll*, a pasta *publish* para um aplicativo ASP.NET Core normalmente contém arquivos de configuração, ativos estáticos e exibições do MVC. Para obter mais informações, consulte [Estrutura de diretórios](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Configure um gerenciador de processo

Um aplicativo ASP.NET Core é um aplicativo de console que deve ser iniciado quando um servidor é inicializado e reiniciado após falhas. Para automatizar inicializações e reinicializações, um gerenciador de processo é necessário. Os gerenciadores de processo mais comuns para o ASP.NET Core são:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Serviço Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Configurar um proxy reverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se o aplicativo usar o servidor Web [Kestrel](xref:fundamentals/servers/kestrel), você poderá usar [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) ou [IIS](xref:host-and-deploy/iis/index) como um servidor proxy reverso. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar. Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se o aplicativo usar o servidor Web [Kestrel](xref:fundamentals/servers/kestrel) e for exposto à Internet, você deverá usar [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) ou [IIS](xref:host-and-deploy/iis/index) como um servidor proxy reverso. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar. O principal motivo para usar um proxy reverso é a segurança. Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de balanceador de carga

Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy e balanceadores de carga. Sem configuração adicional, um aplicativo pode não ter acesso ao esquema (HTTP/HTTPS) e ao endereço IP remoto em que uma solicitação foi originada. Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Usando o Visual Studio e o MSBuild para automatizar a implantação

A implantação muitas vezes requer tarefas adicionais além de copiar a saída da [dotnet publish](/dotnet/core/tools/dotnet-publish) para um servidor. Por exemplo, arquivos extras podem ser necessários ou excluídos da pasta *publish*. O MSBuild, que é usado pelo Visual Studio para implantação da Web, pode ser personalizado para fazer muitas outras tarefas durante a implantação. Para obter mais informações, consulte [Perfis de publicação no Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) e o livro [Usando MSBuild e o Team Foundation Build](http://msbuildbook.com/).

Você pode implantar diretamente do Visual Studio para o Serviço de Aplicativo do Azure usando [o recurso Publicar na Web](xref:tutorials/publish-to-azure-webapp-using-vs) ou usando o [suporte ao Git interno](xref:host-and-deploy/azure-apps/azure-continuous-deployment). O Visual Studio Team Services dá suporte à [implantação contínua para o Serviço de Aplicativo do Azure](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Publicação no Azure

Confira as instruções sobre como publicar um aplicativo no Azure usando o Visual Studio em [Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs). O aplicativo também pode ser publicado no Azure através da [linha de comando](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Recursos adicionais

Para obter informações sobre como usar o Docker como um ambiente de hospedagem, consulte [Hospedar Aplicativos ASP.NET Core no Docker](xref:host-and-deploy/docker/index).
