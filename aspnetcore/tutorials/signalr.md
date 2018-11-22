---
title: Introdução ao SignalR para ASP.NET Core
author: tdykstra
description: Neste tutorial, você criará um aplicativo de chat que usa o SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 8916b3659250c1bcbbc2dc9b3d466586f98bcc7e
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818376"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="6d261-103">Tutorial: introdução ao SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d261-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="6d261-104">Este tutorial ensina as noções básicas da criação de um aplicativo em tempo real usando o SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="6d261-105">Você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="6d261-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6d261-106">Crie um projeto Web.</span><span class="sxs-lookup"><span data-stu-id="6d261-106">Create a web project.</span></span>
> * <span data-ttu-id="6d261-107">Adicionar uma biblioteca de clientes do SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="6d261-108">Criar um hub do SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="6d261-109">Configurar o projeto para usar o SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="6d261-110">Adicione o código que envia mensagens de qualquer cliente para todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="6d261-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="6d261-111">No final, você terá um aplicativo de chat funcionando:</span><span class="sxs-lookup"><span data-stu-id="6d261-111">At the end, you'll have a working chat app:</span></span>

![Aplicativo de exemplo do SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="6d261-113">[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([como baixar](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6d261-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d261-114">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="6d261-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d261-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d261-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6d261-116">[Visual Studio 2017 versão 15.8 ou posterior](https://www.visualstudio.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**</span><span class="sxs-lookup"><span data-stu-id="6d261-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6d261-117">SDK do .NET Core 2.1 Core ou posterior</span><span class="sxs-lookup"><span data-stu-id="6d261-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6d261-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d261-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6d261-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d261-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6d261-120">SDK do .NET Core 2.1 Core ou posterior</span><span class="sxs-lookup"><span data-stu-id="6d261-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="6d261-121">C# para Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d261-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6d261-122">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6d261-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="6d261-123">Visual Studio para Mac versão 7.5.4 ou posteriores</span><span class="sxs-lookup"><span data-stu-id="6d261-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="6d261-124">[SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/download/all) (incluído na instalação do Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="6d261-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="6d261-125">Criar um projeto Web</span><span class="sxs-lookup"><span data-stu-id="6d261-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d261-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d261-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="6d261-127">No menu, selecione **Arquivo > Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="6d261-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="6d261-128">Na caixa de diálogo **Novo Projeto**, selecione **Instalado > Visual C# > Web > Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6d261-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6d261-129">Dê ao projeto o nome de *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="6d261-129">Name the project *SignalRChat*.</span></span>

  ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="6d261-131">Selecione **Aplicativo Web** para criar um projeto que usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6d261-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="6d261-132">Selecione uma estrutura de destino do **.NET Core**, selecione **ASP.NET Core 2.1** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="6d261-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Caixa de diálogo Novo Projeto no Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6d261-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d261-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="6d261-135">Abra o [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) para a pasta na qual a nova pasta de projeto será criada.</span><span class="sxs-lookup"><span data-stu-id="6d261-135">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="6d261-136">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="6d261-136">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6d261-137">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6d261-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6d261-138">No menu, selecione **Arquivo > Nova Solução**.</span><span class="sxs-lookup"><span data-stu-id="6d261-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="6d261-139">Selecione **.NET Core > Aplicativo > Aplicativo Web ASP.NET Core** (não selecione **Aplicativo Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="6d261-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="6d261-140">Selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="6d261-140">Select **Next**.</span></span>

* <span data-ttu-id="6d261-141">Nomeie o projeto como *SignalRChat* e, em seguida, selecione **Criar**.</span><span class="sxs-lookup"><span data-stu-id="6d261-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="6d261-142">Adicionar a biblioteca de clientes do SignalR</span><span class="sxs-lookup"><span data-stu-id="6d261-142">Add the SignalR client library</span></span>

<span data-ttu-id="6d261-143">A biblioteca do servidor SignalR está incluída no metapacote `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="6d261-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="6d261-144">A biblioteca de clientes do JavaScript não é incluída automaticamente no projeto.</span><span class="sxs-lookup"><span data-stu-id="6d261-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="6d261-145">Neste tutorial, você usará o LibMan (Library Manager) para obter a biblioteca de clientes de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="6d261-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="6d261-146">unpkg é uma CDN (rede de distribuição de conteúdo) que pode distribuir qualquer conteúdo do npm, o gerenciador de pacotes do Node.js.</span><span class="sxs-lookup"><span data-stu-id="6d261-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d261-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d261-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="6d261-148">No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Biblioteca do Lado do Cliente**.</span><span class="sxs-lookup"><span data-stu-id="6d261-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="6d261-149">Na caixa de diálogo **Adicionar Biblioteca do Lado do Cliente**, para **Provedor**, selecione **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="6d261-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="6d261-150">Para **Biblioteca**, insira `@aspnet/signalr@1` e selecione a versão mais recente que não seja uma versão prévia.</span><span class="sxs-lookup"><span data-stu-id="6d261-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Caixa de diálogo Adicionar Biblioteca do Lado do Cliente – selecionar biblioteca](signalr/_static/libman1.png)

* <span data-ttu-id="6d261-152">Selecione **Escolher arquivos específicos**, expanda a pasta *distribuidor/navegador* e selecione *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="6d261-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="6d261-153">Defina **Localização de Destino** como *wwwroot/lib/signalr/* e selecione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="6d261-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Caixa de diálogo Adicionar Biblioteca do Lado do Cliente – selecionar arquivos e destino](signalr/_static/libman2.png)

  <span data-ttu-id="6d261-155">O LibMan cria uma pasta *wwwroot/lib/signalr* e copia os arquivos selecionados para ela.</span><span class="sxs-lookup"><span data-stu-id="6d261-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6d261-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d261-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="6d261-157">No terminal integrado, execute o seguinte comando para instalar o LibMan.</span><span class="sxs-lookup"><span data-stu-id="6d261-157">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="6d261-158">Execute o comando a seguir para obter a biblioteca de clientes SignalR usando LibMan.</span><span class="sxs-lookup"><span data-stu-id="6d261-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="6d261-159">Talvez seja necessário aguardar alguns segundos antes de ver a saída.</span><span class="sxs-lookup"><span data-stu-id="6d261-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="6d261-160">Os parâmetros especificam as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="6d261-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="6d261-161">Use o provedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="6d261-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="6d261-162">Copie os arquivos para o destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="6d261-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="6d261-163">Copie apenas os arquivos especificados.</span><span class="sxs-lookup"><span data-stu-id="6d261-163">Copy only the specified files.</span></span>

  <span data-ttu-id="6d261-164">A saída tem a aparência do seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="6d261-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6d261-165">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6d261-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6d261-166">No **Terminal**, execute o comando a seguir para instalar o LibMan.</span><span class="sxs-lookup"><span data-stu-id="6d261-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="6d261-167">Navegue até a pasta do projeto, que inclui o arquivo *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="6d261-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="6d261-168">Execute o comando a seguir para obter a biblioteca de clientes SignalR usando LibMan.</span><span class="sxs-lookup"><span data-stu-id="6d261-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="6d261-169">Os parâmetros especificam as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="6d261-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="6d261-170">Use o provedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="6d261-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="6d261-171">Copie os arquivos para o destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="6d261-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="6d261-172">Copie apenas os arquivos especificados.</span><span class="sxs-lookup"><span data-stu-id="6d261-172">Copy only the specified files.</span></span>

  <span data-ttu-id="6d261-173">A saída tem a aparência do seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="6d261-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="6d261-174">Criar um hub do SignalR</span><span class="sxs-lookup"><span data-stu-id="6d261-174">Create a SignalR hub</span></span>

<span data-ttu-id="6d261-175">Um *hub* é uma classe que funciona como um pipeline de alto nível que lida com a comunicação entre cliente e servidor.</span><span class="sxs-lookup"><span data-stu-id="6d261-175">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="6d261-176">Na pasta do projeto SignalRChat, crie uma pasta *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="6d261-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="6d261-177">Na pasta *Hubs*, crie um arquivo *ChatHub.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="6d261-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="6d261-178">A classe `ChatHub` é herda da classe `Hub` do SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-178">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="6d261-179">A classe `Hub` gerencia conexões, grupos e sistemas de mensagens.</span><span class="sxs-lookup"><span data-stu-id="6d261-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="6d261-180">O método `SendMessage` pode ser chamado por qualquer cliente conectado.</span><span class="sxs-lookup"><span data-stu-id="6d261-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="6d261-181">Ele envia a mensagem recebida para todos os clientes.</span><span class="sxs-lookup"><span data-stu-id="6d261-181">It sends the received message to all clients.</span></span> <span data-ttu-id="6d261-182">O código do SignalR é assíncrono para fornecer o máximo de escalabilidade.</span><span class="sxs-lookup"><span data-stu-id="6d261-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="6d261-183">Configurar o SignalR</span><span class="sxs-lookup"><span data-stu-id="6d261-183">Configure SignalR</span></span>

<span data-ttu-id="6d261-184">O servidor do SignalR precisa ser configurado para passar solicitações do SignalR ao SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="6d261-185">Adicione o seguinte código realçado ao arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6d261-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="6d261-186">Essas alterações adicionam o SignalR ao sistema de injeção de dependência e ao pipeline do middleware do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d261-186">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="6d261-187">Adicionar o código de cliente do SignalR</span><span class="sxs-lookup"><span data-stu-id="6d261-187">Add SignalR client code</span></span>

* <span data-ttu-id="6d261-188">Substitua o conteúdo *Pages\Index.cshtml* pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="6d261-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="6d261-189">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="6d261-189">The preceding code:</span></span>

  * <span data-ttu-id="6d261-190">Cria as caixas de texto para o nome e a mensagem de texto e um botão Enviar.</span><span class="sxs-lookup"><span data-stu-id="6d261-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="6d261-191">Cria uma lista com `id="messagesList"` para exibir as mensagens recebidas do hub do SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="6d261-192">Inclui referências de script ao SignalR e ao código do aplicativo *chat.js* que você criará na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="6d261-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="6d261-193">Na pasta *wwwroot/js*, crie um arquivo *chat.js* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="6d261-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="6d261-194">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="6d261-194">The preceding code:</span></span>

  * <span data-ttu-id="6d261-195">Cria e inicia uma conexão.</span><span class="sxs-lookup"><span data-stu-id="6d261-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="6d261-196">Adiciona no botão Enviar um manipulador que envia mensagens ao hub.</span><span class="sxs-lookup"><span data-stu-id="6d261-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="6d261-197">Adiciona no objeto de conexão um manipulador que recebe mensagens do hub e as adiciona à lista.</span><span class="sxs-lookup"><span data-stu-id="6d261-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6d261-198">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="6d261-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d261-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d261-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6d261-200">Pressione **CTRL + F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="6d261-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6d261-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d261-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6d261-202">No terminal integrado, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="6d261-202">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6d261-203">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="6d261-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6d261-204">No menu, selecione **Executar > Iniciar sem Depuração**.</span><span class="sxs-lookup"><span data-stu-id="6d261-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="6d261-205">Copie a URL da barra de endereços, abra outra instância ou guia do navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="6d261-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="6d261-206">Escolha qualquer navegador, insira um nome e uma mensagem e selecione o botão **Enviar Mensagem**.</span><span class="sxs-lookup"><span data-stu-id="6d261-206">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="6d261-207">O nome e a mensagem são exibidos em ambas as páginas instantaneamente.</span><span class="sxs-lookup"><span data-stu-id="6d261-207">The name and message are displayed on both pages instantly.</span></span>

  ![Aplicativo de exemplo do SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="6d261-209">Se o aplicativo não funcionar, abra as ferramentas para desenvolvedores do navegador (F12) e acesse o console.</span><span class="sxs-lookup"><span data-stu-id="6d261-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="6d261-210">Você pode encontrar erros relacionados ao código HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6d261-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="6d261-211">Por exemplo, suponha que você coloque *signalr.js* em uma pasta diferente daquela direcionada.</span><span class="sxs-lookup"><span data-stu-id="6d261-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="6d261-212">Nesse caso, a referência a esse arquivo não funcionará e ocorrerá um erro 404 no console.</span><span class="sxs-lookup"><span data-stu-id="6d261-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="6d261-213">![Erro de signalr.js não encontrado](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="6d261-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d261-214">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="6d261-214">Next steps</span></span>

<span data-ttu-id="6d261-215">Neste tutorial, você aprendeu como:</span><span class="sxs-lookup"><span data-stu-id="6d261-215">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6d261-216">Criar um projeto de aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="6d261-216">Create a web app project.</span></span>
> * <span data-ttu-id="6d261-217">Adicionar uma biblioteca de clientes do SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-217">Add the SignalR client library.</span></span>
> * <span data-ttu-id="6d261-218">Criar um hub do SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-218">Create a SignalR hub.</span></span>
> * <span data-ttu-id="6d261-219">Configurar o projeto para usar o SignalR.</span><span class="sxs-lookup"><span data-stu-id="6d261-219">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="6d261-220">Adicionar o código que usa o hub para enviar mensagens de qualquer cliente para todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="6d261-220">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="6d261-221">Para saber mais sobre o SignalR, confira a introdução:</span><span class="sxs-lookup"><span data-stu-id="6d261-221">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6d261-222">Introdução ao ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6d261-222">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
