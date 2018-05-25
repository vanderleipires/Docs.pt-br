---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Tutorial: Introdução ao SignalR 1. x | Microsoft Docs'
author: pfletcher
description: Use o SignalR do ASP.NET para criar um aplicativo de bate-papo em tempo real em uma página HTML.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="b22ce-103">Tutorial: Introdução ao SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="b22ce-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="b22ce-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="b22ce-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="b22ce-105">Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real.</span><span class="sxs-lookup"><span data-stu-id="b22ce-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="b22ce-106">Você adicionará o SignalR para um aplicativo de web ASP.NET vazio e criar uma página HTML para enviar e exibir as mensagens.</span><span class="sxs-lookup"><span data-stu-id="b22ce-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="b22ce-107">Visão geral</span><span class="sxs-lookup"><span data-stu-id="b22ce-107">Overview</span></span>

<span data-ttu-id="b22ce-108">Este tutorial apresenta o desenvolvimento de SignalR, mostrando como criar um aplicativo simples baseado em navegador bate-papo.</span><span class="sxs-lookup"><span data-stu-id="b22ce-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="b22ce-109">Você adicionar a biblioteca de SignalR para um aplicativo de web ASP.NET vazio, crie uma classe de hub para enviar mensagens para os clientes e criar uma página HTML que permite aos usuários enviar e receber mensagens de chat.</span><span class="sxs-lookup"><span data-stu-id="b22ce-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="b22ce-110">Para obter um tutorial semelhante que mostra como criar um aplicativo de chat no MVC 4 usando uma exibição do MVC, consulte [Introdução ao SignalR e MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="b22ce-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b22ce-111">Este tutorial usa a versão (1. x) do SignalR.</span><span class="sxs-lookup"><span data-stu-id="b22ce-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="b22ce-112">Para obter detalhes sobre as alterações entre SignalR 1. x e 2.0, consulte [SignalR atualização 1. x projetos](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="b22ce-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="b22ce-113">SignalR é uma biblioteca .NET de código-fonte aberto para a criação de aplicativos web que requerem interação do usuário em tempo real ou atualizações de dados em tempo real.</span><span class="sxs-lookup"><span data-stu-id="b22ce-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="b22ce-114">Exemplos incluem aplicativos sociais, jogos multiusuários, clima de colaboração e notícias, business ou financeiro atualizar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="b22ce-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="b22ce-115">Esses são chamados de aplicativos em tempo real.</span><span class="sxs-lookup"><span data-stu-id="b22ce-115">These are often called real-time applications.</span></span>

<span data-ttu-id="b22ce-116">SignalR simplifica o processo de criação de aplicativos em tempo real.</span><span class="sxs-lookup"><span data-stu-id="b22ce-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="b22ce-117">Ele inclui uma biblioteca de servidor ASP.NET e uma biblioteca de cliente JavaScript para tornar mais fácil gerenciar conexões de cliente-servidor e enviar as atualizações de conteúdo para clientes.</span><span class="sxs-lookup"><span data-stu-id="b22ce-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="b22ce-118">Você pode adicionar a biblioteca de SignalR para um aplicativo ASP.NET existente para obter a funcionalidade em tempo real.</span><span class="sxs-lookup"><span data-stu-id="b22ce-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="b22ce-119">O tutorial demonstra as seguintes tarefas de desenvolvimento SignalR:</span><span class="sxs-lookup"><span data-stu-id="b22ce-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="b22ce-120">Adicionar a biblioteca de SignalR para um aplicativo web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b22ce-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="b22ce-121">Criando uma classe de hub para enviar por push o conteúdo aos clientes.</span><span class="sxs-lookup"><span data-stu-id="b22ce-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="b22ce-122">Usando a biblioteca de jQuery SignalR em uma página da web para enviar mensagens e exibir as atualizações do hub.</span><span class="sxs-lookup"><span data-stu-id="b22ce-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="b22ce-123">A captura de tela a seguir mostra o aplicativo de chat em execução em um navegador.</span><span class="sxs-lookup"><span data-stu-id="b22ce-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="b22ce-124">Cada novo usuário pode enviar comentários e ver comentários adicionados depois que o usuário se associe o bate-papo.</span><span class="sxs-lookup"><span data-stu-id="b22ce-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instâncias de bate-papo](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="b22ce-126">Seções:</span><span class="sxs-lookup"><span data-stu-id="b22ce-126">Sections:</span></span>

- [<span data-ttu-id="b22ce-127">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="b22ce-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="b22ce-128">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="b22ce-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="b22ce-129">Examine o código</span><span class="sxs-lookup"><span data-stu-id="b22ce-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="b22ce-130">Próximas Etapas</span><span class="sxs-lookup"><span data-stu-id="b22ce-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="b22ce-131">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="b22ce-131">Set up the Project</span></span>

<span data-ttu-id="b22ce-132">Esta seção mostra como criar um aplicativo web ASP.NET vazio, adicione o SignalR e criar o aplicativo de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="b22ce-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="b22ce-133">Pré-requisitos:</span><span class="sxs-lookup"><span data-stu-id="b22ce-133">Prerequisites:</span></span>

- <span data-ttu-id="b22ce-134">Visual Studio 2010 SP1 ou 2012.</span><span class="sxs-lookup"><span data-stu-id="b22ce-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="b22ce-135">Se você não tiver o Visual Studio, consulte [ASP.NET Downloads](https://www.asp.net/downloads) para obter o Visual Studio 2012 Express desenvolvimento ferramenta gratuita.</span><span class="sxs-lookup"><span data-stu-id="b22ce-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="b22ce-136">[Microsoft ASP.NET e Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="b22ce-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="b22ce-137">Para o Visual Studio 2012, esse instalador adiciona novos recursos do ASP.NET, incluindo modelos de SignalR ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b22ce-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="b22ce-138">Para o Visual Studio 2010 SP1, um instalador não está disponível, mas você pode concluir o tutorial instalando o pacote SignalR NuGet conforme descrito nas etapas de instalação.</span><span class="sxs-lookup"><span data-stu-id="b22ce-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="b22ce-139">As etapas a seguir usam o Visual Studio 2012 para criar um aplicativo Web vazio do ASP.NET e adicionar a biblioteca de SignalR:</span><span class="sxs-lookup"><span data-stu-id="b22ce-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="b22ce-140">No Visual Studio, crie um aplicativo Web vazio do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b22ce-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Criar web vazio](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="b22ce-142">Abra o **Package Manager Console** selecionando **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="b22ce-143">Digite o seguinte comando na janela de console:</span><span class="sxs-lookup"><span data-stu-id="b22ce-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="b22ce-144">Esse comando instala a versão mais recente do SignalR 1. x.</span><span class="sxs-lookup"><span data-stu-id="b22ce-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="b22ce-145">Em **Solution Explorer**, clique com o botão direito, selecione **adicionar | Classe**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="b22ce-146">Nomeie a nova classe **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="b22ce-147">Em **Solution Explorer** expanda o nó de Scripts.</span><span class="sxs-lookup"><span data-stu-id="b22ce-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="b22ce-148">Bibliotecas de scripts para jQuery e SignalR são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="b22ce-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Referências de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="b22ce-150">Substitua o código no **ChatHub** classe com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="b22ce-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="b22ce-151">Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="b22ce-152">No **Adicionar Novo Item** caixa de diálogo, selecione **classe de aplicativo Global** e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Adicionar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="b22ce-154">Adicione o seguinte `using` instruções após fornecido `using` instruções na classe asax.</span><span class="sxs-lookup"><span data-stu-id="b22ce-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="b22ce-155">Adicione a seguinte linha de código no `Application_Start` método da classe Global para registrar a rota padrão para hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="b22ce-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="b22ce-156">Em **Solution Explorer**, clique com o botão direito e clique em **adicionar | Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="b22ce-157">No **Adicionar Novo Item** caixa de diálogo, selecione Página Html e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="b22ce-158">Em **Solution Explorer**, clique com botão direito a página HTML que você acabou de criar e clique em **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="b22ce-159">Substitua o código padrão na página HTML com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="b22ce-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="b22ce-160">**Salvar todos os** para o projeto.</span><span class="sxs-lookup"><span data-stu-id="b22ce-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="b22ce-161">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="b22ce-161">Run the Sample</span></span>

1. <span data-ttu-id="b22ce-162">Pressione F5 para executar o projeto no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="b22ce-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="b22ce-163">Carrega a página HTML em uma instância do navegador e solicitará um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="b22ce-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Insira o nome de usuário](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="b22ce-165">Insira um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="b22ce-165">Enter a user name.</span></span>
3. <span data-ttu-id="b22ce-166">Copie a URL da linha de endereço do navegador e usá-lo para abrir duas ou mais instâncias de navegador.</span><span class="sxs-lookup"><span data-stu-id="b22ce-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="b22ce-167">Em cada instância do navegador, digite um nome de usuário exclusivo.</span><span class="sxs-lookup"><span data-stu-id="b22ce-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="b22ce-168">Em cada instância do navegador, adicione um comentário e clique em **enviar**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="b22ce-169">Os comentários devem exibir em todas as instâncias do navegador.</span><span class="sxs-lookup"><span data-stu-id="b22ce-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b22ce-170">Este aplicativo de chat simples não mantém o contexto de discussão no servidor.</span><span class="sxs-lookup"><span data-stu-id="b22ce-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="b22ce-171">O hub transmite comentários para todos os usuários atuais.</span><span class="sxs-lookup"><span data-stu-id="b22ce-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="b22ce-172">Os usuários que entram no bate-papo posteriormente verá mensagens adicionadas desde o momento que entrarem.</span><span class="sxs-lookup"><span data-stu-id="b22ce-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="b22ce-173">A captura de tela a seguir mostra o aplicativo de chat em execução em três instâncias do navegador, que são atualizados quando uma instância envia uma mensagem:</span><span class="sxs-lookup"><span data-stu-id="b22ce-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Navegadores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="b22ce-175">Em **Solution Explorer**, inspecione o **documentos de Script** nó para o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="b22ce-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="b22ce-176">Há um arquivo de script denominado **hubs** que a biblioteca de SignalR gera dinamicamente em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="b22ce-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="b22ce-177">Este arquivo gerencia a comunicação entre jQuery script e código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="b22ce-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script de hub gerado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="b22ce-179">Examine o código</span><span class="sxs-lookup"><span data-stu-id="b22ce-179">Examine the Code</span></span>

<span data-ttu-id="b22ce-180">O aplicativo de chat SignalR demonstra duas tarefas básicas de desenvolvimento SignalR: Criando um hub de como o objeto de coordenação principal no servidor e, usando a biblioteca de jQuery SignalR para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="b22ce-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="b22ce-181">Hubs de SignalR</span><span class="sxs-lookup"><span data-stu-id="b22ce-181">SignalR Hubs</span></span>

<span data-ttu-id="b22ce-182">No exemplo de código a **ChatHub** classe deriva o **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="b22ce-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="b22ce-183">Derivando de **Hub** classe é uma maneira útil para criar um aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="b22ce-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="b22ce-184">Você pode criar métodos públicos em sua classe de hub e, em seguida, esses métodos de acesso chamando-los a partir de scripts jQuery em uma página da web.</span><span class="sxs-lookup"><span data-stu-id="b22ce-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="b22ce-185">No código de bate-papo, clientes chamar o **ChatHub.Send** para enviar uma nova mensagem.</span><span class="sxs-lookup"><span data-stu-id="b22ce-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="b22ce-186">O hub por sua vez envia a mensagem a todos os clientes chamando **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="b22ce-187">O **enviar** método demonstra vários conceitos de hub:</span><span class="sxs-lookup"><span data-stu-id="b22ce-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="b22ce-188">Declare métodos públicos em um hub para que os clientes podem chamá-los.</span><span class="sxs-lookup"><span data-stu-id="b22ce-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="b22ce-189">Use o **Microsoft.AspNet.SignalR.Hub.Clients** propriedade dinâmica para acessar todos os clientes conectados a esse hub.</span><span class="sxs-lookup"><span data-stu-id="b22ce-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="b22ce-190">Chamar uma função de jQuery no cliente (como o `broadcastMessage` função) para atualizar clientes.</span><span class="sxs-lookup"><span data-stu-id="b22ce-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="b22ce-191">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="b22ce-191">SignalR and jQuery</span></span>

<span data-ttu-id="b22ce-192">A página HTML no exemplo de código mostra como usar a biblioteca de jQuery SignalR para se comunicar com um hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="b22ce-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="b22ce-193">As tarefas essenciais no código estão declarando um proxy para referenciar o hub, declarar uma função que o servidor pode chamar para conteúdo por push aos clientes e iniciar uma conexão para enviar mensagens para o hub.</span><span class="sxs-lookup"><span data-stu-id="b22ce-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="b22ce-194">O código a seguir declara um proxy para um hub.</span><span class="sxs-lookup"><span data-stu-id="b22ce-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="b22ce-195">Em jQuery a referência para a classe de servidor e seus membros está em minúsculas concatenadas.</span><span class="sxs-lookup"><span data-stu-id="b22ce-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="b22ce-196">O exemplo de código referencia o c# **ChatHub** classe jQuery como **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="b22ce-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="b22ce-197">O código a seguir é como criar uma função de retorno de chamada no script.</span><span class="sxs-lookup"><span data-stu-id="b22ce-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="b22ce-198">A classe de hub no servidor chama esta função para enviar atualizações de conteúdo para cada cliente.</span><span class="sxs-lookup"><span data-stu-id="b22ce-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="b22ce-199">As duas linhas que o HTML codificar o conteúdo antes de exibi-los são opcionais e mostram uma maneira simples para prevenir injeção de script.</span><span class="sxs-lookup"><span data-stu-id="b22ce-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="b22ce-200">O código a seguir mostra como abrir uma conexão com o hub.</span><span class="sxs-lookup"><span data-stu-id="b22ce-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="b22ce-201">O código começa a conexão e, em seguida, passa uma função para manipular o evento de clique no **enviar** botão na página HTML.</span><span class="sxs-lookup"><span data-stu-id="b22ce-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="b22ce-202">Essa abordagem garante que a conexão é estabelecida antes de executa o manipulador de eventos.</span><span class="sxs-lookup"><span data-stu-id="b22ce-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="b22ce-203">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="b22ce-203">Next Steps</span></span>

<span data-ttu-id="b22ce-204">Você aprendeu SignalR é uma estrutura para criar aplicativos web em tempo real.</span><span class="sxs-lookup"><span data-stu-id="b22ce-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="b22ce-205">Você também aprendeu diversas tarefas de desenvolvimento SignalR: como adicionar SignalR para um aplicativo ASP.NET, como criar uma classe de hub e como enviar e receber mensagens do hub.</span><span class="sxs-lookup"><span data-stu-id="b22ce-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="b22ce-206">Você pode tornar o aplicativo de exemplo neste tutorial ou outros aplicativos SignalR disponíveis pela Internet por implantá-los para um provedor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="b22ce-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="b22ce-207">A Microsoft oferece gratuito da web de hospedagem para até 10 sites da web em um livre [conta de avaliação do Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="b22ce-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="b22ce-208">Para obter instruções sobre como implantar o aplicativo de exemplo SignalR, consulte [publicar o SignalR obtendo iniciado exemplo como um Site do Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="b22ce-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="b22ce-209">Para obter informações detalhadas sobre como implantar um projeto da web do Visual Studio para um Site do Windows Azure, consulte [Implantando um aplicativo ASP.NET a um Site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="b22ce-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="b22ce-210">(Observação: O transporte de WebSocket não tem suporte para o Windows Azure Web Sites.</span><span class="sxs-lookup"><span data-stu-id="b22ce-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="b22ce-211">Transporte de WebSocket quando não estiver disponível, o SignalR usa os outros transportes disponíveis conforme descrito na seção de transportes de [Introdução ao tópico SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="b22ce-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="b22ce-212">Para saber mais avançados conceitos de desenvolvimentos SignalR, visite os seguintes sites para SignalR código-fonte e recursos:</span><span class="sxs-lookup"><span data-stu-id="b22ce-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="b22ce-213">Projeto de SignalR</span><span class="sxs-lookup"><span data-stu-id="b22ce-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="b22ce-214">SignalR Github e exemplos</span><span class="sxs-lookup"><span data-stu-id="b22ce-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="b22ce-215">Wiki do SignalR</span><span class="sxs-lookup"><span data-stu-id="b22ce-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
