---
title: Ferramentas do Visual Studio para Docker com ASP.NET Core
author: spboyer
description: "Saiba como usar as ferramentas do Visual Studio 2017 e o Docker para o Windows para colocar um aplicativo ASP.NET Core em um contêiner."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="2ac6c-103">Ferramentas do Visual Studio para Docker com ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ac6c-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="2ac6c-104">[Visual Studio de 2017](https://www.visualstudio.com/) dá suporte à criação, depuração e execução em contêineres ASP.NET Core aplicativos voltados para o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="2ac6c-105">Contêineres do Windows e do Linux são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ac6c-106">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2ac6c-106">Prerequisites</span></span>

* <span data-ttu-id="2ac6c-107">O [Visual Studio 2017](https://www.visualstudio.com/) com a carga de trabalho **Desenvolvimento de multiplataforma do .NET Core**</span><span class="sxs-lookup"><span data-stu-id="2ac6c-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="2ac6c-108">Docker para Windows</span><span class="sxs-lookup"><span data-stu-id="2ac6c-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="2ac6c-109">Instalação e configuração</span><span class="sxs-lookup"><span data-stu-id="2ac6c-109">Installation and setup</span></span>

<span data-ttu-id="2ac6c-110">Para a instalação do Docker, examine as informações em [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker para Windows: o que saber antes de instalar) e instale o [Docker para Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="2ac6c-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="2ac6c-111">As **[Unidades Compartilhadas](https://docs.docker.com/docker-for-windows/#shared-drives)** do Docker para Windows devem ser configuradas para dar suporte ao mapeamento do volume e à depuração.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="2ac6c-112">Clique duas vezes o ícone de Docker da bandeja do sistema, selecione **configurações...** e selecione **unidades compartilhadas**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="2ac6c-113">Selecione a unidade em que o Docker armazena arquivos.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="2ac6c-114">Selecione **aplicar**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-114">Select **Apply**.</span></span>

![Unidades Compartilhadas](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="2ac6c-116">Visual Studio 2017 versões 15.6 e posteriores do prompt quando **unidades compartilhadas** não estão configurados.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="2ac6c-117">Adicionar suporte do Docker a um aplicativo</span><span class="sxs-lookup"><span data-stu-id="2ac6c-117">Add Docker support to an app</span></span>

<span data-ttu-id="2ac6c-118">Para adicionar suporte de Docker para um projeto do ASP.NET Core, o projeto deve usar o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-118">In order to add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="2ac6c-119">Há suporte para contêineres do Linux e Windows.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="2ac6c-120">Ao adicionar o suporte de Docker para um projeto, escolha Windows ou um contêiner do Linux.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="2ac6c-121">O host do Docker deve estar executando o mesmo tipo de contêiner.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="2ac6c-122">Para alterar o tipo de contêiner na instância do Docker em execução, clique com o botão direito do mouse no ícone do Docker na Bandeja do Sistema e escolha **Alternar para contêineres do Windows...** ou **Alternar para contêineres do Linux...**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="2ac6c-123">Novo aplicativo</span><span class="sxs-lookup"><span data-stu-id="2ac6c-123">New app</span></span>

<span data-ttu-id="2ac6c-124">Ao criar um novo aplicativo com os modelos de projeto do **Aplicativo Web ASP.NET Core**, marque a caixa de seleção **Habilitar Suporte do Docker**:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** checkbox:</span></span>

![Caixa de seleção Habilitar Suporte do Docker](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

<span data-ttu-id="2ac6c-126">Se a estrutura de destino é o núcleo do .NET, o **OS** suspensa permite a seleção de um tipo de contêiner.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="2ac6c-127">Aplicativo existente</span><span class="sxs-lookup"><span data-stu-id="2ac6c-127">Existing app</span></span>

<span data-ttu-id="2ac6c-128">As Ferramentas do Visual Studio para Docker não dão suporte à adição do Docker a um projeto ASP.NET Core existente direcionado ao .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="2ac6c-129">Para projetos do ASP.NET Core direcionados ao .NET Core, há duas opções para adicionar o suporte do Docker por meio das ferramentas.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="2ac6c-130">Abra o projeto no Visual Studio e escolha uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="2ac6c-131">Selecione **Suporte do Docker** no menu **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="2ac6c-132">Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Adicionar** > **Suporte do Docker**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="2ac6c-133">Visão geral de ativos do Docker</span><span class="sxs-lookup"><span data-stu-id="2ac6c-133">Docker assets overview</span></span>

<span data-ttu-id="2ac6c-134">As Ferramentas do Visual Studio para Docker adicionam um projeto *docker-compose* à solução, que contém o seguinte:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution, containing the following:</span></span>

* <span data-ttu-id="2ac6c-135">*.dockerignore*: contém uma lista de padrões de arquivo e diretório a serem excluídos ao gerar um contexto de build.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="2ac6c-136">*docker-compose.yml*: o arquivo base do [Docker Compose](https://docs.docker.com/compose/overview/) usado para definir a coleção de imagens que serão compiladas e executadas com o `docker-compose build` e `docker-compose run`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="2ac6c-137">*docker-compose.override.yml*: um arquivo opcional, lido pelo Docker Compose, que contém as substituições de configuração para os serviços.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="2ac6c-138">O Visual Studio executa `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` para mesclar esses arquivos.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="2ac6c-139">Um *Dockerfile*, a receita para criar uma imagem final do Docker, é adicionado à raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="2ac6c-140">Consulte [Referência do Dockerfile](https://docs.docker.com/engine/reference/builder/) para compreender seus comandos.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="2ac6c-141">Esse *Dockerfile* específico usa um [build de vários estágios](https://docs.docker.com/engine/userguide/eng-image/multistage-build/), que contém quatro estágios de build nomeados distintos:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="2ac6c-142">O *Dockerfile* se baseia na imagem [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="2ac6c-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="2ac6c-143">Essa imagem base inclui os pacotes NuGet do ASP.NET Core, que foram pré-compilados com JIT para melhorar o desempenho de inicialização.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="2ac6c-144">O *docker compose.yml* arquivo contém o nome da imagem que é criado quando o projeto é executado:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="2ac6c-145">No exemplo anterior, `image: hellodockertools` gera a imagem `hellodockertools:dev` quando o aplicativo é executado no modo **Depuração**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="2ac6c-146">A imagem `hellodockertools:latest` é gerada quando o aplicativo é executado no modo **Versão**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="2ac6c-147">Prefixo do nome de imagem com o [Hub do Docker](https://hub.docker.com/) nome de usuário (por exemplo, `dockerhubusername/hellodockertools`) se a imagem será enviada para o registro.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="2ac6c-148">Como alternativa, alterar o nome da imagem para incluir a URL de registro particular (por exemplo, `privateregistry.domain.com/hellodockertools`) dependendo da configuração.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="2ac6c-149">Depurar</span><span class="sxs-lookup"><span data-stu-id="2ac6c-149">Debug</span></span>

<span data-ttu-id="2ac6c-150">Selecione **Docker** no menu suspenso de depuração na barra de ferramentas e inicie a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="2ac6c-151">A exibição **Docker** da janela **Saída** mostra as seguintes ações em andamento:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="2ac6c-152">O *microsoft/aspnetcore* imagem de tempo de execução é adquirida (se ainda não estiver no cache).</span><span class="sxs-lookup"><span data-stu-id="2ac6c-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="2ac6c-153">O *aspnetcore/microsoft-compilação* imagem/publicação de compilação é adquirida (se ainda não estiver no cache).</span><span class="sxs-lookup"><span data-stu-id="2ac6c-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="2ac6c-154">A variável de ambiente *ASPNETCORE_ENVIRONMENT* é definida como `Development` dentro do contêiner.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="2ac6c-155">A porta 80 é exposta e mapeada para uma porta atribuída dinamicamente para o localhost.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-155">Port 80 is exposed and mapped to a dynamically-assigned port for localhost.</span></span> <span data-ttu-id="2ac6c-156">A porta é determinada pelo host do Docker e pode ser consultada com o comando `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="2ac6c-157">O aplicativo é copiado para o contêiner.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-157">The app is copied to the container.</span></span>
* <span data-ttu-id="2ac6c-158">O navegador padrão é iniciado com o depurador anexado ao contêiner usando a porta atribuída dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-158">The default browser is launched with the debugger attached to the container using the dynamically-assigned port.</span></span> 

<span data-ttu-id="2ac6c-159">A imagem resultante do Docker é o *desenvolvimento* imagem do aplicativo com o *microsoft/aspnetcore* imagens como a imagem base.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="2ac6c-160">Execute o comando `docker images` na janela do PMC **(Console do Gerenciador de Pacotes)**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="2ac6c-161">As imagens no computador são exibidas:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="2ac6c-162">A imagem de desenvolvimento não tem o conteúdo do aplicativo, como **depurar** configurações usam a montagem de volume para fornecer uma experiência interativa.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="2ac6c-163">Para enviar uma imagem por push, use a configuração **Versão**.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="2ac6c-164">Execute o comando `docker ps` no PMC.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="2ac6c-165">Observe que o aplicativo está em execução usando o contêiner:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="2ac6c-166">Editar e continuar</span><span class="sxs-lookup"><span data-stu-id="2ac6c-166">Edit and continue</span></span>

<span data-ttu-id="2ac6c-167">As alterações em arquivos estáticos e exibições do Razor são atualizadas automaticamente sem a necessidade de uma etapa de compilação.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="2ac6c-168">Faça a alteração, salve e atualize o navegador para exibir a atualização.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-168">Make the change, save, and refresh the browser to view the update.</span></span>  

<span data-ttu-id="2ac6c-169">As modificações em arquivos de código exigem a compilação e a reinicialização do Kestrel dentro do contêiner.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-169">Modifications to code files requires compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="2ac6c-170">Depois de fazer a alteração, use CTRL + F5 para executar o processo e iniciar o aplicativo dentro do contêiner.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-170">After making the change, use CTRL + F5 to perform the process and start the app within the container.</span></span> <span data-ttu-id="2ac6c-171">O contêiner do Docker não é recriado ou interrompido.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="2ac6c-172">Execute o comando `docker ps` no PMC.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="2ac6c-173">Observe que o contêiner original ainda está em execução como há 10 minutos:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="2ac6c-174">Publicar imagens do Docker</span><span class="sxs-lookup"><span data-stu-id="2ac6c-174">Publish Docker images</span></span>

<span data-ttu-id="2ac6c-175">Depois que o ciclo de desenvolver e depurar o aplicativo for concluído, o Visual Studio Tools para Docker ajudar a criar a imagem de produção do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="2ac6c-176">Altere a lista suspensa de configuração para **Versão** e compile o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="2ac6c-177">A ferramenta gera a imagem com o *mais recente* marca, que pode ser enviada para o registro particular ou o Hub do Docker.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span> 

<span data-ttu-id="2ac6c-178">Execute o comando `docker images` no PMC para ver a lista de imagens:</span><span class="sxs-lookup"><span data-stu-id="2ac6c-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="2ac6c-179">O comando `docker images` retorna imagens intermediárias com nomes de repositório e marcas identificadas como *\<none>* (não listadas acima).</span><span class="sxs-lookup"><span data-stu-id="2ac6c-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="2ac6c-180">Essas imagens sem nome são produzidas pelo *Dockerfile* no [build de vários estágios](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="2ac6c-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="2ac6c-181">Elas melhoram a eficiência da compilação da imagem final &mdash; apenas as camadas necessárias são recompiladas quando ocorrem alterações.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="2ac6c-182">Quando as imagens intermediárias não são mais necessários, excluí-los usando o [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) comando.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="2ac6c-183">Pode haver uma expectativa de que a imagem de produção ou versão seja menor em comparação com a imagem *dev*.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="2ac6c-184">O mapeamento do volume, devido a estavam em execução o depurador e o aplicativo no computador local e não dentro do contêiner.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="2ac6c-185">A *última* imagem empacotou o código do aplicativo necessário para executar o aplicativo em um computador host.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="2ac6c-186">Portanto, o delta é o tamanho do código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2ac6c-186">Therefore, the delta is the size of the app code.</span></span>
