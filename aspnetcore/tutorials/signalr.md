---
title: Introdução ao SignalR para ASP.NET Core
author: tdykstra
description: Neste tutorial, você criará um aplicativo de chat que usa o SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 190717dc6e6f9f2766ba92aa7472f4cdea9b6827
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458524"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="5ecd6-103">Tutorial: introdução ao SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5ecd6-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="5ecd6-104">Este tutorial ensina as noções básicas da criação de um aplicativo em tempo real usando o SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="5ecd6-105">Você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5ecd6-106">Crie um projeto Web.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-106">Create a web project.</span></span>
> * <span data-ttu-id="5ecd6-107">Adicionar uma biblioteca de clientes do SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="5ecd6-108">Criar um hub do SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="5ecd6-109">Configurar o projeto para usar o SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="5ecd6-110">Adicione o código que envia mensagens de qualquer cliente para todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="5ecd6-111">No final, você terá um aplicativo de chat funcionando:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-111">At the end, you'll have a working chat app:</span></span>

![Aplicativo de exemplo do SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="5ecd6-113">[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([como baixar](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5ecd6-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="5ecd6-114">Estamos testando a usabilidade de uma nova estrutura proposta para o sumário do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="5ecd6-115">Se você tiver alguns minutos para experimentar um exercício de localização de sete tópicos diferentes no sumário atual ou proposto, [clique aqui para participar do estudo](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="5ecd6-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ecd6-116">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="5ecd6-116">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ecd6-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ecd6-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5ecd6-118">[Visual Studio 2017 versão 15.8 ou posterior](https://www.visualstudio.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**</span><span class="sxs-lookup"><span data-stu-id="5ecd6-118">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="5ecd6-119">SDK do .NET Core 2.1 Core ou posterior</span><span class="sxs-lookup"><span data-stu-id="5ecd6-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5ecd6-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ecd6-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="5ecd6-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ecd6-121">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="5ecd6-122">SDK do .NET Core 2.1 Core ou posterior</span><span class="sxs-lookup"><span data-stu-id="5ecd6-122">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="5ecd6-123">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ecd6-123">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5ecd6-124">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5ecd6-124">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="5ecd6-125">Visual Studio para Mac versão 7.5.4 ou posteriores</span><span class="sxs-lookup"><span data-stu-id="5ecd6-125">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="5ecd6-126">[SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/download/all) (incluído na instalação do Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="5ecd6-126">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="5ecd6-127">Criar um projeto Web</span><span class="sxs-lookup"><span data-stu-id="5ecd6-127">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ecd6-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ecd6-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="5ecd6-129">No menu, selecione **Arquivo > Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="5ecd6-130">Na caixa de diálogo **Novo Projeto**, selecione **Instalado > Visual C# > Web > Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="5ecd6-131">Dê ao projeto o nome de *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-131">Name the project *SignalRChat*.</span></span>

  ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="5ecd6-133">Selecione **Aplicativo Web** para criar um projeto que usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="5ecd6-134">Selecione uma estrutura de destino do **.NET Core**, selecione **ASP.NET Core 2.1** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5ecd6-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ecd6-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="5ecd6-137">Abra o [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) para a pasta na qual a nova pasta de projeto será criada.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="5ecd6-138">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-138">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5ecd6-139">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5ecd6-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5ecd6-140">No menu, selecione **Arquivo > Nova Solução**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="5ecd6-141">Selecione **.NET Core > Aplicativo > Aplicativo Web ASP.NET Core** (não selecione **Aplicativo Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="5ecd6-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="5ecd6-142">Selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-142">Select **Next**.</span></span>

* <span data-ttu-id="5ecd6-143">Nomeie o projeto como *SignalRChat* e, em seguida, selecione **Criar**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="5ecd6-144">Adicionar a biblioteca de clientes do SignalR</span><span class="sxs-lookup"><span data-stu-id="5ecd6-144">Add the SignalR client library</span></span>

<span data-ttu-id="5ecd6-145">A biblioteca do servidor SignalR está incluída no metapacote `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-145">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="5ecd6-146">A biblioteca de clientes do JavaScript não é incluída automaticamente no projeto.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-146">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="5ecd6-147">Neste tutorial, você usará o LibMan (Library Manager) para obter a biblioteca de clientes de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-147">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="5ecd6-148">unpkg é uma CDN (rede de distribuição de conteúdo) que pode distribuir qualquer conteúdo do npm, o gerenciador de pacotes do Node.js.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-148">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ecd6-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ecd6-149">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="5ecd6-150">No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Biblioteca do Lado do Cliente**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-150">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="5ecd6-151">Na caixa de diálogo **Adicionar Biblioteca do Lado do Cliente**, para **Provedor**, selecione **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-151">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="5ecd6-152">Para **Biblioteca**, insira `@aspnet/signalr@1` e selecione a versão mais recente que não seja uma versão prévia.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-152">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Caixa de diálogo Adicionar Biblioteca do Lado do Cliente – selecionar biblioteca](signalr/_static/libman1.png)

* <span data-ttu-id="5ecd6-154">Selecione **Escolher arquivos específicos**, expanda a pasta *distribuidor/navegador* e selecione *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-154">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="5ecd6-155">Defina **Localização de Destino** como *wwwroot/lib/signalr/* e selecione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-155">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Caixa de diálogo Adicionar Biblioteca do Lado do Cliente – selecionar arquivos e destino](signalr/_static/libman2.png)

  <span data-ttu-id="5ecd6-157">O LibMan cria uma pasta *wwwroot/lib/signalr* e copia os arquivos selecionados para ela.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-157">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5ecd6-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ecd6-158">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="5ecd6-159">No terminal integrado, execute o seguinte comando para instalar o LibMan.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-159">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="5ecd6-160">Execute o comando a seguir para obter a biblioteca de clientes SignalR usando LibMan.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-160">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="5ecd6-161">Talvez seja necessário aguardar alguns segundos antes de ver a saída.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-161">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="5ecd6-162">Os parâmetros especificam as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="5ecd6-163">Use o provedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="5ecd6-164">Copie os arquivos para o destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="5ecd6-165">Copie apenas os arquivos especificados.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-165">Copy only the specified files.</span></span>

  <span data-ttu-id="5ecd6-166">A saída tem a aparência do seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5ecd6-167">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5ecd6-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5ecd6-168">No **Terminal**, execute o comando a seguir para instalar o LibMan.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-168">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="5ecd6-169">Navegue até a pasta do projeto, que inclui o arquivo *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-169">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="5ecd6-170">Execute o comando a seguir para obter a biblioteca de clientes SignalR usando LibMan.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-170">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="5ecd6-171">Os parâmetros especificam as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-171">The parameters specify the following options:</span></span>
  * <span data-ttu-id="5ecd6-172">Use o provedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-172">Use the unpkg provider.</span></span>
  * <span data-ttu-id="5ecd6-173">Copie os arquivos para o destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-173">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="5ecd6-174">Copie apenas os arquivos especificados.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-174">Copy only the specified files.</span></span>

  <span data-ttu-id="5ecd6-175">A saída tem a aparência do seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-175">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="5ecd6-176">Criar um hub do SignalR</span><span class="sxs-lookup"><span data-stu-id="5ecd6-176">Create a SignalR hub</span></span>

<span data-ttu-id="5ecd6-177">Um *hub* é uma classe que funciona como um pipeline de alto nível que lida com a comunicação entre cliente e servidor.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-177">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="5ecd6-178">Na pasta do projeto SignalRChat, crie uma pasta *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-178">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="5ecd6-179">Na pasta *Hubs*, crie um arquivo *ChatHub.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-179">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="5ecd6-180">A classe `ChatHub` é herda da classe `Hub` do SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-180">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="5ecd6-181">A classe `Hub` gerencia conexões, grupos e sistemas de mensagens.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-181">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="5ecd6-182">O método `SendMessage` pode ser chamado por qualquer cliente conectado.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-182">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="5ecd6-183">Ele envia a mensagem recebida para todos os clientes.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-183">It sends the received message to all clients.</span></span> <span data-ttu-id="5ecd6-184">O código do SignalR é assíncrono para fornecer o máximo de escalabilidade.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-184">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="5ecd6-185">Configurar o SignalR</span><span class="sxs-lookup"><span data-stu-id="5ecd6-185">Configure SignalR</span></span>

<span data-ttu-id="5ecd6-186">O servidor do SignalR precisa ser configurado para passar solicitações do SignalR ao SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-186">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="5ecd6-187">Adicione o seguinte código realçado ao arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-187">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="5ecd6-188">Essas alterações adicionam o SignalR ao sistema de injeção de dependência e ao pipeline do middleware do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-188">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="5ecd6-189">Adicionar o código de cliente do SignalR</span><span class="sxs-lookup"><span data-stu-id="5ecd6-189">Add SignalR client code</span></span>

* <span data-ttu-id="5ecd6-190">Substitua o conteúdo *Pages\Index.cshtml* pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-190">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="5ecd6-191">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-191">The preceding code:</span></span>

  * <span data-ttu-id="5ecd6-192">Cria as caixas de texto para o nome e a mensagem de texto e um botão Enviar.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-192">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="5ecd6-193">Cria uma lista com `id="messagesList"` para exibir as mensagens recebidas do hub do SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-193">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="5ecd6-194">Inclui referências de script ao SignalR e ao código do aplicativo *chat.js* que você criará na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-194">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="5ecd6-195">Na pasta *wwwroot/js*, crie um arquivo *chat.js* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-195">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="5ecd6-196">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-196">The preceding code:</span></span>

  * <span data-ttu-id="5ecd6-197">Cria e inicia uma conexão.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-197">Creates and starts a connection.</span></span>
  * <span data-ttu-id="5ecd6-198">Adiciona no botão Enviar um manipulador que envia mensagens ao hub.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-198">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="5ecd6-199">Adiciona no objeto de conexão um manipulador que recebe mensagens do hub e as adiciona à lista.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-199">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="5ecd6-200">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="5ecd6-200">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5ecd6-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5ecd6-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5ecd6-202">Pressione **CTRL + F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5ecd6-203">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5ecd6-203">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5ecd6-204">No terminal integrado, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-204">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5ecd6-205">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5ecd6-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="5ecd6-206">No menu, selecione **Executar > Iniciar sem Depuração**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-206">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="5ecd6-207">Copie a URL da barra de endereços, abra outra instância ou guia do navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-207">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="5ecd6-208">Escolha qualquer navegador, insira um nome e uma mensagem e selecione o botão **Enviar Mensagem**.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-208">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="5ecd6-209">O nome e a mensagem são exibidos em ambas as páginas instantaneamente.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-209">The name and message are displayed on both pages instantly.</span></span>

  ![Aplicativo de exemplo do SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="5ecd6-211">Se o aplicativo não funcionar, abra as ferramentas para desenvolvedores do navegador (F12) e acesse o console.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-211">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="5ecd6-212">Você pode encontrar erros relacionados ao código HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-212">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="5ecd6-213">Por exemplo, suponha que você coloque *signalr.js* em uma pasta diferente daquela direcionada.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-213">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="5ecd6-214">Nesse caso, a referência a esse arquivo não funcionará e ocorrerá um erro 404 no console.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-214">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="5ecd6-215">![Erro de signalr.js não encontrado](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="5ecd6-215">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ecd6-216">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="5ecd6-216">Next steps</span></span>

<span data-ttu-id="5ecd6-217">Neste tutorial, você aprendeu como:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-217">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5ecd6-218">Criar um projeto de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-218">Create a web app project.</span></span>
> * <span data-ttu-id="5ecd6-219">Adicionar uma biblioteca de clientes do SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-219">Add the SignalR client library.</span></span>
> * <span data-ttu-id="5ecd6-220">Criar um hub do SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-220">Create a SignalR hub.</span></span>
> * <span data-ttu-id="5ecd6-221">Configurar o projeto para usar o SignalR.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-221">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="5ecd6-222">Adicionar o código que usa o hub para enviar mensagens de qualquer cliente para todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="5ecd6-222">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="5ecd6-223">Para saber mais sobre o SignalR, confira a introdução:</span><span class="sxs-lookup"><span data-stu-id="5ecd6-223">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5ecd6-224">Introdução ao ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="5ecd6-224">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
