---
title: Ferramentas do Visual Studio para Docker com ASP.NET Core
author: spboyer
description: Saiba como usar as ferramentas do Visual Studio 2017 e o Docker para o Windows para colocar um aplicativo ASP.NET Core em um contêiner.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: fd485416ff0fab2508ab8ffd3f0ad309be338723
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276848"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Ferramentas do Visual Studio para Docker com ASP.NET Core

O [Visual Studio 2017](https://www.visualstudio.com/) dá suporte à criação, depuração e execução de aplicativos do ASP.NET Core em contêineres direcionados ao .NET Core. Contêineres do Windows e do Linux são compatíveis.

## <a name="prerequisites"></a>Pré-requisitos

* O [Visual Studio 2017](https://www.visualstudio.com/) com a carga de trabalho **Desenvolvimento de multiplataforma do .NET Core**
* [Docker para Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalação e configuração

Para a instalação do Docker, examine as informações em [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker para Windows: o que saber antes de instalar) e instale o [Docker para Windows](https://docs.docker.com/docker-for-windows/install/).

As **[Unidades Compartilhadas](https://docs.docker.com/docker-for-windows/#shared-drives)** do Docker para Windows devem ser configuradas para dar suporte ao mapeamento do volume e à depuração. Clique com o botão direito do mouse no ícone do Docker na Bandeja do Sistema, selecione **Configurações...** e selecione **Unidades Compartilhadas**. Selecione a unidade em que o Docker armazena arquivos. Selecione **Aplicar**.

![Unidades Compartilhadas](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> A versão 15.6 e as versões posteriores do Visual Studio 2017 exibem um aviso quando as **Unidades Compartilhadas** não estão configuradas.

## <a name="add-docker-support-to-an-app"></a>Adicionar suporte do Docker a um aplicativo

Para adicionar suporte do Docker para um projeto do ASP.NET Core, o projeto deve ser destinado ao .NET Core. Contêineres do Linux e Windows são ambos compatíveis.

Ao adicionar o suporte do Docker a um projeto, escolha um contêiner do Windows ou Linux. O host do Docker deve estar executando o mesmo tipo de contêiner. Para alterar o tipo de contêiner na instância do Docker em execução, clique com o botão direito do mouse no ícone do Docker na Bandeja do Sistema e escolha **Alternar para contêineres do Windows...** ou **Alternar para contêineres do Linux...**.

### <a name="new-app"></a>Novo aplicativo

Ao criar um novo aplicativo com os modelos de projeto do **Aplicativo Web ASP.NET Core**, marque a caixa de seleção **Habilitar Suporte do Docker**:

![Caixa de seleção Habilitar Suporte do Docker](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

Se a estrutura de destino for o .NET Core, a lista suspensa **SO** permitirá a seleção de um tipo de contêiner.

### <a name="existing-app"></a>Aplicativo existente

As Ferramentas do Visual Studio para Docker não dão suporte à adição do Docker a um projeto ASP.NET Core existente direcionado ao .NET Framework. Para projetos do ASP.NET Core direcionados ao .NET Core, há duas opções para adicionar o suporte do Docker por meio das ferramentas. Abra o projeto no Visual Studio e escolha uma das seguintes opções:

* Selecione **Suporte do Docker** no menu **Projeto**.
* Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Adicionar** > **Suporte do Docker**.

## <a name="docker-assets-overview"></a>Visão geral de ativos do Docker

As Ferramentas do Visual Studio para Docker adicionam um projeto *docker-compose* à solução, que contém o seguinte:

* *.dockerignore*: contém uma lista de padrões de arquivo e diretório a serem excluídos ao gerar um contexto de build.
* *docker-compose.yml*: o arquivo base do [Docker Compose](https://docs.docker.com/compose/overview/) usado para definir a coleção de imagens que serão compiladas e executadas com o `docker-compose build` e `docker-compose run`, respectivamente.
* *docker-compose.override.yml*: um arquivo opcional, lido pelo Docker Compose, que contém as substituições de configuração para os serviços. O Visual Studio executa `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` para mesclar esses arquivos.

Um *Dockerfile*, a receita para criar uma imagem final do Docker, é adicionado à raiz do projeto. Consulte [Referência do Dockerfile](https://docs.docker.com/engine/reference/builder/) para compreender seus comandos. Esse *Dockerfile* específico usa um [build de vários estágios](https://docs.docker.com/engine/userguide/eng-image/multistage-build/), que contém quatro estágios de build nomeados distintos:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

O *Dockerfile* se baseia na imagem [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore). Essa imagem base inclui os pacotes NuGet do ASP.NET Core, que foram pré-compilados com JIT para melhorar o desempenho de inicialização.

O arquivo *docker-compose.yml* contém o nome da imagem que é criada quando o projeto é executado:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

No exemplo anterior, `image: hellodockertools` gera a imagem `hellodockertools:dev` quando o aplicativo é executado no modo **Depuração**. A imagem `hellodockertools:latest` é gerada quando o aplicativo é executado no modo **Versão**.

Prefixe o nome da imagem com o nome de usuário do [Hub do Docker](https://hub.docker.com/) (por exemplo, `dockerhubusername/hellodockertools`) se a imagem será enviada por push para o registro. Como alternativa, altere o nome da imagem para incluir a URL do registro privado (por exemplo, `privateregistry.domain.com/hellodockertools`), dependendo da configuração.

## <a name="debug"></a>Depurar

Selecione **Docker** no menu suspenso de depuração na barra de ferramentas e inicie a depuração do aplicativo. A exibição **Docker** da janela **Saída** mostra as seguintes ações em andamento:

* A imagem em tempo de execução *microsoft/aspnetcore* é adquirida (se ainda não está no cache).
* A imagem de compilação/publicação *microsoft/aspnetcore-build* é adquirida (se ainda não está no cache).
* A variável de ambiente *ASPNETCORE_ENVIRONMENT* é definida como `Development` dentro do contêiner.
* A porta 80 é exposta e mapeada para uma porta atribuída dinamicamente para o localhost. A porta é determinada pelo host do Docker e pode ser consultada com o comando `docker ps`.
* O aplicativo é copiado para o contêiner.
* O navegador padrão é iniciado com o depurador anexado ao contêiner, usando a porta atribuída dinamicamente. 

A imagem resultante do Docker é a imagem *dev* do aplicativo, com as imagens *microsoft/aspnetcore* como a imagem base. Execute o comando `docker images` na janela do PMC **(Console do Gerenciador de Pacotes)**. As imagens no computador são exibidas:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> A imagem dev não inclui o conteúdo do aplicativo, pois as configurações de **Depuração** usam a montagem de volume para fornecer uma experiência iterativa. Para enviar uma imagem por push, use a configuração **Versão**.

Execute o comando `docker ps` no PMC. Observe que o aplicativo está em execução usando o contêiner:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Editar e continuar

As alterações em arquivos estáticos e exibições do Razor são atualizadas automaticamente sem a necessidade de uma etapa de compilação. Faça a alteração, salve e atualize o navegador para exibir a atualização.  

As modificações em arquivos de código exigem a compilação e a reinicialização do Kestrel dentro do contêiner. Depois de fazer a alteração, use CTRL + F5 para executar o processo e iniciar o aplicativo dentro do contêiner. O contêiner do Docker não é recompilado nem interrompido. Execute o comando `docker ps` no PMC. Observe que o contêiner original ainda está em execução como há 10 minutos:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Publicar imagens do Docker

Depois de concluir o ciclo de desenvolvimento e depuração de seu aplicativo, as Ferramentas do Visual Studio para Docker ajudarão você a criar a imagem de produção do aplicativo. Altere a lista suspensa de configuração para **Versão** e compile o aplicativo. A ferramenta produz a imagem com a marcação *latest*, que você pode enviar por push para seu registro privado ou para o Hub do Docker. 

Execute o comando `docker images` no PMC para ver a lista de imagens:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> O comando `docker images` retorna imagens intermediárias com nomes de repositório e marcas identificadas como *\<none>* (não listadas acima). Essas imagens sem nome são produzidas pelo *Dockerfile* no [build de vários estágios](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Elas melhoram a eficiência da compilação da imagem final &mdash; apenas as camadas necessárias são recompiladas quando ocorrem alterações. Quando você não precisar mais das imagens intermediárias, exclua-as usando o comando [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/).

Pode haver uma expectativa de que a imagem de produção ou versão seja menor em comparação com a imagem *dev*. Devido ao mapeamento do volume, o depurador e o aplicativo estavam em execução no computador local e não dentro do contêiner. A *última* imagem empacotou o código do aplicativo necessário para executar o aplicativo em um computador host. Portanto, o delta é o tamanho do código do aplicativo.
