---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Detecção de classe de inicialização OWIN | Microsoft Docs
author: Praburaj
description: Este tutorial mostra como configurar a classe de inicialização que OWIN é carregado. Para obter mais informações sobre OWIN, consulte uma visão geral de projeto Katana. Este tutorial foi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 33d2745b24387419e5614c62c2d46948427b242a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="2c137-105">Detecção de classe de inicialização OWIN</span><span class="sxs-lookup"><span data-stu-id="2c137-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="2c137-106">por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2c137-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="2c137-107">Este tutorial mostra como configurar a classe de inicialização que OWIN é carregado.</span><span class="sxs-lookup"><span data-stu-id="2c137-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="2c137-108">Para obter mais informações sobre OWIN, consulte [uma visão geral de projeto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="2c137-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="2c137-109">Este tutorial foi escrito de Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan e Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="2c137-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="2c137-110">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2c137-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="2c137-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2c137-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="2c137-112">Detecção de classe de inicialização OWIN</span><span class="sxs-lookup"><span data-stu-id="2c137-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="2c137-113">Cada aplicativo OWIN tem uma classe de inicialização em que você especificar componentes para o pipeline do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2c137-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="2c137-114">Você pode se conectar a sua classe de inicialização com o tempo de execução de maneiras diferentes, dependendo do modelo de hospedagem que você escolher (OwinHost, IIS e o IIS Express).</span><span class="sxs-lookup"><span data-stu-id="2c137-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="2c137-115">A classe de inicialização mostrada neste tutorial pode ser usada em todos os aplicativos de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="2c137-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="2c137-116">Você se conectar a classe de inicialização com o tempo de execução hospedagem usando uma dessas abordagens:</span><span class="sxs-lookup"><span data-stu-id="2c137-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="2c137-117">**Convenção de nomenclatura**: Katana procura uma classe denominada `Startup` no namespace correspondentes ao nome de assembly ou o namespace global.</span><span class="sxs-lookup"><span data-stu-id="2c137-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="2c137-118">**Atributo OwinStartup**: essa é a abordagem a maioria dos desenvolvedores levará para especificar a classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="2c137-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="2c137-119">O seguinte atributo definirá a classe de inicialização o `TestStartup` classe no `StartupDemo` namespace.</span><span class="sxs-lookup"><span data-stu-id="2c137-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="2c137-120">O `OwinStartup` atributo substitui a convenção de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="2c137-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="2c137-121">Você também pode especificar um nome amigável com esse atributo, no entanto, usar um nome amigável requer que você também use o `appSetting` elemento no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="2c137-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="2c137-122">**O elemento appSetting no arquivo de configuração**: O `appSetting` elemento substitui o `OwinStartup` atributo e a convenção de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="2c137-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="2c137-123">Você pode ter várias classes de inicialização (cada usando um `OwinStartup` atributo) e configurar qual classe de inicialização será carregado em um arquivo de configuração usando marcação semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="2c137-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="2c137-124">A seguinte chave, que especifica explicitamente a classe de inicialização e o assembly também pode ser usada:</span><span class="sxs-lookup"><span data-stu-id="2c137-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="2c137-125">O XML no arquivo de configuração a seguir especifica um nome de classe de inicialização amigável do `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="2c137-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="2c137-126">A marcação acima deve ser usada com o seguinte `OwinStartup` atributo que especifica um nome amigável e faz com que o `ProductionStartup2` classe a ser executada.</span><span class="sxs-lookup"><span data-stu-id="2c137-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="2c137-127">Para desabilitar a descoberta de inicialização OWIN, adicione o `appSetting owin:AutomaticAppStartup` com um valor de `"false"` no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="2c137-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="2c137-128">Criar um aplicativo Web do ASP.NET usando a inicialização de OWIN</span><span class="sxs-lookup"><span data-stu-id="2c137-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="2c137-129">Criar um aplicativo de web Asp.Net vazio e nomeie- **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="2c137-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="2c137-130">-Instalar `Microsoft.Owin.Host.SystemWeb` usando o NuGet package manager.</span><span class="sxs-lookup"><span data-stu-id="2c137-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="2c137-131">Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**e, em seguida, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="2c137-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="2c137-132">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="2c137-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="2c137-133">Adicione uma classe de inicialização OWIN.</span><span class="sxs-lookup"><span data-stu-id="2c137-133">Add an OWIN startup class.</span></span> <span data-ttu-id="2c137-134">No Visual Studio 2013 clique com botão direito no projeto e selecione **Adicionar classe**. - na **Adicionar Novo Item** caixa de diálogo, digite *OWIN* no campo de pesquisa e altere o nome para Startup.cs, e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2c137-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   <span data-ttu-id="2c137-135">Na próxima vez em que você deseja adicionar um *classe de inicialização Owin*, para que esteja disponível no **adicionar** menu.</span><span class="sxs-lookup"><span data-stu-id="2c137-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   <span data-ttu-id="2c137-136">Como alternativa, você pode clique com botão direito no projeto e selecione **adicionar**, em seguida, selecione **Novo Item**e, em seguida, selecione o **classe de inicialização Owin**.</span><span class="sxs-lookup"><span data-stu-id="2c137-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="2c137-137">Substitua o código gerado no *Startup.cs* arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="2c137-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  <span data-ttu-id="2c137-138">O `app.Use` expressão lambda é usada para registrar o componente de middleware especificado para o pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="2c137-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="2c137-139">Nesse caso estamos configurando registro em log de solicitações de entrada antes de responder à solicitação de entrada.</span><span class="sxs-lookup"><span data-stu-id="2c137-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="2c137-140">O `next` parâmetro é o delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [tarefa](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) para o próximo componente no pipeline.</span><span class="sxs-lookup"><span data-stu-id="2c137-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="2c137-141">O `app.Run` expressão lambda conecta o solicitações de entrada do pipeline e fornece o mecanismo de resposta.</span><span class="sxs-lookup"><span data-stu-id="2c137-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="2c137-142">No código acima é ter comentada o `OwinStartup` atributo e nós depende da convenção de executar a classe denominada `Startup` .-pressione ***F5*** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2c137-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="2c137-143">Clique em atualizar algumas vezes.</span><span class="sxs-lookup"><span data-stu-id="2c137-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  <span data-ttu-id="2c137-144">Observação: O número mostrado nas imagens neste tutorial não corresponderá o número exibido.</span><span class="sxs-lookup"><span data-stu-id="2c137-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="2c137-145">A cadeia de caracteres de milissegundos é usada para mostrar uma nova resposta quando você atualizar a página.</span><span class="sxs-lookup"><span data-stu-id="2c137-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
  <span data-ttu-id="2c137-146">Você pode ver as informações de rastreamento de **saída** janela.</span><span class="sxs-lookup"><span data-stu-id="2c137-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="2c137-147">Adicionar mais Classes de inicialização</span><span class="sxs-lookup"><span data-stu-id="2c137-147">Add More Startup Classes</span></span>

<span data-ttu-id="2c137-148">Nesta seção, vamos adicionar de outra classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="2c137-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="2c137-149">Você pode adicionar vários classe de inicialização OWIN para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2c137-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="2c137-150">Por exemplo, você talvez queira criar classes de inicialização para o desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="2c137-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="2c137-151">Crie uma nova classe de inicialização OWIN e nomeie- `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="2c137-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="2c137-152">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="2c137-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="2c137-153">Pressione F5 de controle para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2c137-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="2c137-154">O `OwinStartup` atributo especifica a classe de inicialização de produção é executada.</span><span class="sxs-lookup"><span data-stu-id="2c137-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="2c137-155">Crie outra classe de inicialização OWIN e nomeie- `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="2c137-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="2c137-156">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="2c137-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="2c137-157">O `OwinStartup` acima de sobrecarga de atributo especifica `TestingConfiguration` como o *amigável* nome da classe de inicialização.</span><span class="sxs-lookup"><span data-stu-id="2c137-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="2c137-158">Abra o *Web. config* de arquivos e adicionar a chave de inicialização do aplicativo OWIN que especifica o nome amigável da classe de inicialização:</span><span class="sxs-lookup"><span data-stu-id="2c137-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="2c137-159">Pressione F5 de controle para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2c137-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="2c137-160">O elemento de configurações do aplicativo usa precedência e o teste de configuração é executada.</span><span class="sxs-lookup"><span data-stu-id="2c137-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="2c137-161">Remover o *amigável* nome do `OwinStartup` atributo o `TestStartup` classe.</span><span class="sxs-lookup"><span data-stu-id="2c137-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="2c137-162">Substitua a chave de inicialização do aplicativo OWIN no *Web. config* arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="2c137-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="2c137-163">Reverter o `OwinStartup` atributo em cada classe para o código do atributo padrão gerado pelo Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2c137-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="2c137-164">Cada uma das chaves de inicialização do aplicativo OWIN abaixo fará com que a classe de produção executar.</span><span class="sxs-lookup"><span data-stu-id="2c137-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="2c137-165">A última chave de inicialização especifica o método de configuração de inicialização.</span><span class="sxs-lookup"><span data-stu-id="2c137-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="2c137-166">A seguinte chave de inicialização do aplicativo OWIN permite que você altere o nome da classe de configuração para `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="2c137-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="2c137-167">Usando Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="2c137-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="2c137-168">Substitua o arquivo Web. config com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="2c137-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="2c137-169">A última chave wins, isso neste caso `TestStartup` for especificado.</span><span class="sxs-lookup"><span data-stu-id="2c137-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="2c137-170">Instale o Owinhost do PMC:</span><span class="sxs-lookup"><span data-stu-id="2c137-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="2c137-171">Navegue até a pasta do aplicativo (a pasta que contém o *Web. config* arquivo) e em um prompt de comando e digite:</span><span class="sxs-lookup"><span data-stu-id="2c137-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="2c137-172">Mostra a janela de comando:</span><span class="sxs-lookup"><span data-stu-id="2c137-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="2c137-173">Iniciar um navegador com a URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="2c137-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   <span data-ttu-id="2c137-174">OwinHost cumprido as convenções de inicialização listadas acima.</span><span class="sxs-lookup"><span data-stu-id="2c137-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="2c137-175">Na janela de comando, pressione Enter para sair do OwinHost.</span><span class="sxs-lookup"><span data-stu-id="2c137-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="2c137-176">No `ProductionStartup` da classe, adicione o seguinte atributo OwinStartup que especifica um nome amigável do *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="2c137-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="2c137-177">No prompt de comando e tipo:</span><span class="sxs-lookup"><span data-stu-id="2c137-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="2c137-178">A classe de inicialização de produção é carregada.</span><span class="sxs-lookup"><span data-stu-id="2c137-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
   <span data-ttu-id="2c137-179">Nosso aplicativo tem várias classes de inicialização e, neste exemplo, podemos adiaram qual classe de inicialização para carregar até o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="2c137-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="2c137-180">As seguintes opções de inicialização do tempo de execução de teste:</span><span class="sxs-lookup"><span data-stu-id="2c137-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
