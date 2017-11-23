---
title: Ferramentas do Visual Studio para Docker com ASP.NET Core
description: "Este artigo explica como usar as ferramentas do Visual Studio 2017 e o Docker para Windows para colocar um aplicativo ASP.NET Core em contêineres."
keywords: "Docker,ASP.NET Core,Visual Studio,contêiner"
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a>Ferramentas do Visual Studio para Docker

O [Microsoft Visual Studio 2017](https://www.visualstudio.com/) com [Docker para Windows](https://docs.docker.com/docker-for-windows/install/) dá suporte ao build, depuração e execução de aplicativos Web e de console do .NET Framework e .NET Core usando contêineres do Windows e do Linux.

## <a name="prerequisites"></a>Pré-requisitos

- [Microsoft Visual Studio 2017](https://www.visualstudio.com/) com carga de trabalho do .NET Core
- [Docker para Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalação e configuração

Instale o [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) com a carga de trabalho do .NET Core. Caso você já tenha o Visual Studio instalado, é possível [modificar a sua instalação do Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) para adicionar a carga de trabalho do .NET Core.

Para a instalação do Docker, examine as informações em [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker para Windows: o que saber antes de instalar) e instale o [Docker para Windows](https://docs.docker.com/docker-for-windows/install/).

É uma configuração necessária para instalação de **[Unidades Compartilhadas](https://docs.docker.com/docker-for-windows/#shared-drives)** no Docker para Windows. A configuração é necessária para o mapeamento do volume e suporte à depuração.

Clique com o botão direito do mouse no ícone do Docker na Bandeja do Sistema, clique em **Configurações** e selecione **Unidades Compartilhadas**. Selecione a unidade em que o Docker armazenará seus arquivos e faça as alterações.

![Unidades Compartilhadas](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a>Criar um Aplicativo Web ASP.NET e adicionar suporte ao Docker

Usando o Visual Studio, crie um novo Aplicativo Web ASP.NET Core. Quando o aplicativo é carregado, selecione **Adicionar Suporte ao Docker** no **Menu Projeto** ou clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Adicionar** > **Suporte ao Docker**.

*Menu Projeto*

![Projeto Adicionar Suporte ao Docker](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

*Menu de Contexto do Projeto*

![Clique com o botão direito do mouse em Adicionar Suporte ao Docker](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

Quando você adiciona suporte do Docker ao seu projeto, é possível escolher entre contêineres do Windows ou Linux. (O host do Docker deve estar executando o mesmo tipo de contêiner. Se for necessário, altere o tipo de contêiner na instância do Docker em execução, clique com o botão direito do mouse no ícone do **Docker** na Bandeja do Sistema e escolha **Alterar para contêineres do Windows** ou **Alterar para contêineres do Linux**.) 

Os seguintes arquivos são adicionados ao projeto:

- **Dockerfile**: o arquivo do Docker para aplicativos do ASP.NET Core se baseia na imagem [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore). Essa imagem inclui os pacotes NuGet do ASP.NET Core, que foram pré-compilados com JIT para melhorar o desempenho de inicialização. Ao criar Aplicativos de Console do .NET Core, o Dockerfile FROM fará referência à imagem [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) mais recente.   
- **docker-compose.yml**: arquivo base do Docker Compose usado para definir o conjunto de imagens que serão compiladas e executadas com o build/execução do docker-compose.   
- **docker-compose.dev.debug.yml**: arquivo docker-compose adicional com alterações iterativas quando a configuração está definida para depuração. O Visual Studio chamará -f docker-compose.yml -f docker-compose.dev.debug.yml para mesclá-los. Esse arquivo de composição é usado pelas ferramentas de desenvolvimento do Visual Studio.   
- **docker-compose.dev.release.yml**: arquivo adicional do Docker Compose para depurar sua definição de versão. Ele montará o volume do depurador, portanto ele não altera o conteúdo da imagem de produção.  

O arquivo *docker-compose.yml* contém o nome da imagem que é criada quando o projeto é executado. 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

Neste exemplo, `image: user/hellodockertools${TAG}` gera a imagem `user/hellodockertools:dev` quando o aplicativo é executado no modo de **Depuração** e `user/hellodockertools:latest` no modo de **Lançamento**, respectivamente. 

Você deve alterar o `user` para seu nome de usuário do [Hub do Docker](https://hub.docker.com/) se planejar enviar a imagem por push para o registro. Por exemplo, `spboyer/hellodockertools`, ou altere para a URL do seu registro privado `privateregistry.domain.com/`, dependendo da configuração.

### <a name="debugging"></a>Depuração

Selecione **Docker** no menu suspenso de depuração na barra de ferramentas e use F5 para iniciar a depuração do aplicativo. 

- A imagem do *microsoft/aspnetcore* é adquirida (se ainda não estiver em seu cache)
- *ASPNETCORE_ENVIRONMENT* é definido como Desenvolvimento dentro do contêiner
- A PORTA 80 é EXPOSTA e mapeada para uma porta atribuída dinamicamente para o localhost. A porta é determinada pelo host do docker e pode ser consultada com o docker ps. 
- Seu aplicativo é copiado para o contêiner
- O navegador padrão é iniciado com o depurador anexado ao contêiner, usando a porta atribuída dinamicamente. 

A imagem criada resultante do Docker é a imagem de *desenvolvimento* do seu aplicativo com as imagens *microsoft/aspnetcore* como a imagem base.

**Observação:** a imagem de desenvolvimento não inclui o conteúdo do aplicativo, pois as configurações de Depuração usam montagem de volume para fornecer uma experiência iterativa. Para enviar uma imagem por push, use a configuração de Lançamento.

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

O aplicativo é executado usando o contêiner que pode ser visto executando o comando `docker ps`.

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a>Editar e continuar

Alterações em arquivos estáticos e/ou arquivos de modelo do razor (*.cshtml*) são atualizadas automaticamente sem a necessidade de uma etapa de compilação. Faça a alteração, salve e toque em Atualizar no navegador para exibir a atualização.  

Modificações em arquivos de código exigem a compilação e a reinicialização do Kestrel dentro do contêiner. Depois de fazer a alteração, use CTRL + F5 para executar o processo e iniciar o aplicativo dentro do contêiner. O contêiner Docker não é recriado ou interrompido; usando `docker ps` na linha de comando, você pode ver que o contêiner original ainda está em execução a partir de 10 minutos atrás. 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a>Publicando imagens do Docker

Depois de ter concluído o ciclo de desenvolvimento e depuração do seu aplicativo, o Ferramentas do Visual Studio para Docker ajudará você a criar a imagem de produção do seu aplicativo. Altere o menu suspenso de depuração para **Lançamento** e compile o aplicativo. A ferramenta produzirá a imagem com o marcador `:latest` que você pode enviar por push para seu Registro privado ou Hub do Docker. 

Usando o comando `docker images`, você pode ver a lista de imagens.

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

Pode haver uma expectativa para a imagem de produção ou de lançamento serem menores em comparação à imagem de **desenvolvimento**; no entanto, com o uso do mapeamento de volume, o depurador e o aplicativo realmente foram executados em seu computador local e não dentro do contêiner. A imagem **mais recente** empacotou todo o código de aplicativo necessário para executar o aplicativo em um computador host, portanto, o delta é do tamanho do código do aplicativo.
