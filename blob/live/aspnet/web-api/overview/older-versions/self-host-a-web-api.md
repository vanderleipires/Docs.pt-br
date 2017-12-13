---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "Próprio Host ASP.NET Web API 1 (c#) | Microsoft Docs"
author: MikeWasson
description: "API da Web do ASP.NET não requer o IIS. Automaticamente, você pode hospedar uma API da web em seu próprio processo de host. Este tutorial mostra como hospedar uma API da web dentro de um console applic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: b308ee9ec209ba8bbb021827655c83443dd149e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="18331-105">Próprio Host ASP.NET Web API 1 (c#)</span><span class="sxs-lookup"><span data-stu-id="18331-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="18331-106">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="18331-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="18331-107">API da Web do ASP.NET não requer o IIS.</span><span class="sxs-lookup"><span data-stu-id="18331-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="18331-108">Automaticamente, você pode hospedar uma API da web em seu próprio processo de host.</span><span class="sxs-lookup"><span data-stu-id="18331-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="18331-109">Este tutorial mostra como hospedar uma API da web dentro de um aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="18331-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="18331-110">**Novos aplicativos devem usar OWIN para hospedagem interna de API da Web.**</span><span class="sxs-lookup"><span data-stu-id="18331-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="18331-111">Consulte [usar OWIN para hospedar o ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="18331-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="18331-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="18331-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="18331-113">API Web 1</span><span class="sxs-lookup"><span data-stu-id="18331-113">Web API 1</span></span>
> - <span data-ttu-id="18331-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="18331-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="18331-115">Criar o projeto de aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="18331-115">Create the Console Application Project</span></span>

<span data-ttu-id="18331-116">Inicie o Visual Studio e selecione **novo projeto** do **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="18331-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="18331-117">Ou, do **arquivo** menu, selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="18331-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="18331-118">No **modelos** painel, selecione **modelos instalados** e expanda o **Visual C#** nó.</span><span class="sxs-lookup"><span data-stu-id="18331-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="18331-119">Em **Visual C#**, selecione **Windows**.</span><span class="sxs-lookup"><span data-stu-id="18331-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="18331-120">Na lista de modelos de projeto, selecione **aplicativo de Console**.</span><span class="sxs-lookup"><span data-stu-id="18331-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="18331-121">Nomeie o projeto &quot;SelfHost&quot; e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="18331-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="18331-122">Definir a estrutura de destino (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="18331-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="18331-123">Se você estiver usando o Visual Studio 2010, altere a estrutura de destino para o .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="18331-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="18331-124">(Por padrão, os destinos de modelo de projeto de [perfil do .net Framework Client](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="18331-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="18331-125">No Gerenciador de soluções, clique com o botão direito e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="18331-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="18331-126">No **framework de destino** suspensa lista, altere a estrutura de destino para o .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="18331-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="18331-127">Quando solicitado para aplicar a alteração, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="18331-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="18331-128">Instalar o NuGet Package Manager</span><span class="sxs-lookup"><span data-stu-id="18331-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="18331-129">O NuGet Package Manager é a maneira mais fácil para adicionar os assemblies de API da Web a um projeto de não-ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="18331-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="18331-130">Para verificar se o NuGet Package Manager está instalado, clique o **ferramentas** menu do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18331-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="18331-131">Se você vir um item de menu chamado **Gerenciador de biblioteca de pacote**, em seguida, você tem o NuGet Package Manager.</span><span class="sxs-lookup"><span data-stu-id="18331-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="18331-132">Para instalar o NuGet Package Manager:</span><span class="sxs-lookup"><span data-stu-id="18331-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="18331-133">Inicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18331-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="18331-134">Do **ferramentas** menu, selecione **extensões e atualizações**.</span><span class="sxs-lookup"><span data-stu-id="18331-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="18331-135">No **extensões e atualizações** caixa de diálogo, selecione **Online**.</span><span class="sxs-lookup"><span data-stu-id="18331-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="18331-136">Se você não vir "NuGet Package Manager", digite "nuget package manager" na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="18331-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="18331-137">Selecione o NuGet Package Manager e clique em **baixar**.</span><span class="sxs-lookup"><span data-stu-id="18331-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="18331-138">Após a conclusão do download, você será solicitado a instalar.</span><span class="sxs-lookup"><span data-stu-id="18331-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="18331-139">Após a conclusão da instalação, você pode solicitado a reiniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18331-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="18331-140">Adicionar o pacote de NuGet de API da Web</span><span class="sxs-lookup"><span data-stu-id="18331-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="18331-141">Depois que o NuGet Package Manager está instalado, adicione o pacote de Self-Host de API da Web ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="18331-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="18331-142">Do **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**.</span><span class="sxs-lookup"><span data-stu-id="18331-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="18331-143">*Observação*: se você não vir esse menu item, certifique-se de que NuGet Package Manager está instalado corretamente.</span><span class="sxs-lookup"><span data-stu-id="18331-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="18331-144">Selecione **gerenciar pacotes NuGet para solução...**</span><span class="sxs-lookup"><span data-stu-id="18331-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="18331-145">No **gerenciar preciosidade pacotes** caixa de diálogo, selecione **Online**.</span><span class="sxs-lookup"><span data-stu-id="18331-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="18331-146">Na caixa de pesquisa, digite &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="18331-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="18331-147">Selecione o pacote ASP.NET Web API Self Host e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="18331-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="18331-148">Depois de instala o pacote, clique em **fechar** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="18331-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="18331-149">Certifique-se de instalar o pacote denominado Microsoft.AspNet.WebApi.SelfHost, não AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="18331-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="18331-150">Criar o modelo e o controlador</span><span class="sxs-lookup"><span data-stu-id="18331-150">Create the Model and Controller</span></span>

<span data-ttu-id="18331-151">Este tutorial usa as mesmas classes de modelo e o controlador como o [Introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="18331-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="18331-152">Adicionar uma classe pública chamada `Product`.</span><span class="sxs-lookup"><span data-stu-id="18331-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="18331-153">Adicionar uma classe pública chamada `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="18331-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="18331-154">Derive essa classe de **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="18331-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="18331-155">Para obter mais informações sobre o código neste controlador, consulte o [Introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="18331-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="18331-156">Esse controlador define três ações GET:</span><span class="sxs-lookup"><span data-stu-id="18331-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="18331-157">URI</span><span class="sxs-lookup"><span data-stu-id="18331-157">URI</span></span> | <span data-ttu-id="18331-158">Descrição</span><span class="sxs-lookup"><span data-stu-id="18331-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="18331-159">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="18331-159">/api/products</span></span> | <span data-ttu-id="18331-160">Obter uma lista de todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="18331-160">Get a list of all products.</span></span> |
| <span data-ttu-id="18331-161">/API/produtos/*id*</span><span class="sxs-lookup"><span data-stu-id="18331-161">/api/products/*id*</span></span> | <span data-ttu-id="18331-162">Obter um produto por ID.</span><span class="sxs-lookup"><span data-stu-id="18331-162">Get a product by ID.</span></span> |
| <span data-ttu-id="18331-163">/API/produtos /? categoria =*categoria*</span><span class="sxs-lookup"><span data-stu-id="18331-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="18331-164">Obter uma lista de produtos por categoria.</span><span class="sxs-lookup"><span data-stu-id="18331-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="18331-165">Hospedar a API da Web</span><span class="sxs-lookup"><span data-stu-id="18331-165">Host the Web API</span></span>

<span data-ttu-id="18331-166">Abra o arquivo Program.cs e adicione o seguinte usando instruções:</span><span class="sxs-lookup"><span data-stu-id="18331-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="18331-167">Adicione o seguinte código para o **programa** classe.</span><span class="sxs-lookup"><span data-stu-id="18331-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="18331-168">(Opcional) Adicionar uma reserva de Namespace de URL de HTTP</span><span class="sxs-lookup"><span data-stu-id="18331-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="18331-169">Este aplicativo ouve `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="18331-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="18331-170">Por padrão, escutando em um determinado endereço HTTP requer privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="18331-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="18331-171">Quando você executar o tutorial, portanto, você pode obter esse erro: "HTTP não pôde registrar a URL http://+:8080/" há duas maneiras de evitar esse erro:</span><span class="sxs-lookup"><span data-stu-id="18331-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="18331-172">Execute o Visual Studio com permissões de administrador com privilégios elevados, ou</span><span class="sxs-lookup"><span data-stu-id="18331-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="18331-173">Use Netsh.exe para conceder as permissões de conta para reservar a URL.</span><span class="sxs-lookup"><span data-stu-id="18331-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="18331-174">Para usar Netsh.exe, abra um prompt de comando com privilégios de administrador e digite o seguinte comando: comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="18331-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="18331-175">onde *máquina \ nomedeusuário* é sua conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="18331-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="18331-176">Quando você terminar de hospedagem interna, certifique-se de excluir a reserva:</span><span class="sxs-lookup"><span data-stu-id="18331-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="18331-177">Chame a API da Web de um aplicativo cliente (c#)</span><span class="sxs-lookup"><span data-stu-id="18331-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="18331-178">Vamos escrever um aplicativo de console simples que chama a API da web.</span><span class="sxs-lookup"><span data-stu-id="18331-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="18331-179">Adicione um novo projeto de aplicativo de console à solução:</span><span class="sxs-lookup"><span data-stu-id="18331-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="18331-180">No Gerenciador de soluções, a solução e selecione **adicionar novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="18331-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="18331-181">Criar um novo aplicativo de console chamado &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="18331-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="18331-182">Use o Gerenciador de pacote de NuGet para adicionar o pacote ASP.NET Web API Core Libraries:</span><span class="sxs-lookup"><span data-stu-id="18331-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="18331-183">No menu Ferramentas, selecione **Gerenciador de pacotes de biblioteca**.</span><span class="sxs-lookup"><span data-stu-id="18331-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="18331-184">Selecione **gerenciar pacotes NuGet para solução...**</span><span class="sxs-lookup"><span data-stu-id="18331-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="18331-185">No **gerenciar pacotes NuGet** caixa de diálogo, selecione **Online**.</span><span class="sxs-lookup"><span data-stu-id="18331-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="18331-186">Na caixa de pesquisa, digite &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="18331-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="18331-187">Selecione o pacote de bibliotecas de cliente do Microsoft ASP.NET Web API e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="18331-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="18331-188">Adicione uma referência em ClientApp ao projeto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="18331-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="18331-189">No Gerenciador de soluções, clique com botão direito no projeto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="18331-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="18331-190">Selecione **adicionar referência**.</span><span class="sxs-lookup"><span data-stu-id="18331-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="18331-191">No **Gerenciador de referências** caixa de diálogo, em **solução**, selecione **projetos**.</span><span class="sxs-lookup"><span data-stu-id="18331-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="18331-192">Selecione o projeto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="18331-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="18331-193">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="18331-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="18331-194">Abra o arquivo Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="18331-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="18331-195">Adicione o seguinte **usando** instrução:</span><span class="sxs-lookup"><span data-stu-id="18331-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="18331-196">Adicionar um estático **HttpClient** instância:</span><span class="sxs-lookup"><span data-stu-id="18331-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="18331-197">Adicione os seguintes métodos para listar todos os produtos, um produto por ID de lista e lista produtos por categoria.</span><span class="sxs-lookup"><span data-stu-id="18331-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="18331-198">Cada um desses métodos segue o mesmo padrão:</span><span class="sxs-lookup"><span data-stu-id="18331-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="18331-199">Chamar **HttpClient.GetAsync** para enviar uma solicitação GET para o URI apropriado.</span><span class="sxs-lookup"><span data-stu-id="18331-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="18331-200">Chamar **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="18331-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="18331-201">Este método lança uma exceção se o status de resposta HTTP for um código de erro.</span><span class="sxs-lookup"><span data-stu-id="18331-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="18331-202">Chamar **ReadAsAsync&lt;T&gt;**  para desserializar um tipo CLR da resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="18331-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="18331-203">Esse método é um método de extensão, definido em **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="18331-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="18331-204">O **GetAsync** e **ReadAsAsync** métodos são assíncronas.</span><span class="sxs-lookup"><span data-stu-id="18331-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="18331-205">Elas retornam **tarefa** objetos que representam a operação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="18331-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="18331-206">Obtendo o **resultados** propriedade bloqueia o thread até que a operação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="18331-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="18331-207">Para obter mais informações sobre como usar HttpClient, inclusive como fazer chamadas sem bloqueio, consulte [chamar uma Web API de um cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="18331-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="18331-208">Antes de chamar esses métodos, defina a propriedade BaseAddress na instância do HttpClient para "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="18331-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="18331-209">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="18331-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="18331-210">Esta saída a seguir.</span><span class="sxs-lookup"><span data-stu-id="18331-210">This should output the following.</span></span> <span data-ttu-id="18331-211">(Lembre-se de executar o aplicativo SelfHost primeiro.)</span><span class="sxs-lookup"><span data-stu-id="18331-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
