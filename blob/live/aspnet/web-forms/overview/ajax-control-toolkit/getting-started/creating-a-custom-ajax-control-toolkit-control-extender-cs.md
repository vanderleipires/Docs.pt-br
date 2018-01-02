---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Criando um AJAX personalizado controle extensor de controle do Kit de ferramentas (c#) | Microsoft Docs
author: microsoft
description: Extensores personalizados permitem personalizar e estender os recursos dos controles do ASP.NET sem a necessidade de criar novas classes.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 2ae03484dd1161c65b77f4718bb8cedb5abfdd82
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="adfcb-103">Criando um extensor de controle do Kit de ferramentas de controle AJAX personalizados (c#)</span><span class="sxs-lookup"><span data-stu-id="adfcb-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="adfcb-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="adfcb-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="adfcb-105">Extensores personalizados permitem personalizar e estender os recursos dos controles do ASP.NET sem a necessidade de criar novas classes.</span><span class="sxs-lookup"><span data-stu-id="adfcb-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="adfcb-106">Neste tutorial, você aprenderá como criar um extensor de controle personalizado do AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="adfcb-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="adfcb-107">Podemos criar um simples, mas útil, novo extensor que altera o estado de um botão de desabilitado para habilitado quando você digita texto em uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="adfcb-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="adfcb-108">Depois de ler este tutorial, você poderá estender o Kit de ferramentas ASP.NET AJAX com seus próprio extensores de controle.</span><span class="sxs-lookup"><span data-stu-id="adfcb-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="adfcb-109">Você pode criar os Extensores do controle personalizado usando o Visual Studio ou Visual Web Developer (certifique-se de que você tem a versão mais recente do Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="adfcb-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="adfcb-110">Visão geral do extensor DisabledButton</span><span class="sxs-lookup"><span data-stu-id="adfcb-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="adfcb-111">Nosso novo extensor de controle é chamado de extensor DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="adfcb-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="adfcb-112">Este extensor tem três propriedades:</span><span class="sxs-lookup"><span data-stu-id="adfcb-112">This extender will have three properties:</span></span>

- <span data-ttu-id="adfcb-113">TargetControlID - caixa de texto que o controle estende.</span><span class="sxs-lookup"><span data-stu-id="adfcb-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="adfcb-114">TargetButtonIID - botão é habilitado ou desabilitado.</span><span class="sxs-lookup"><span data-stu-id="adfcb-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="adfcb-115">DisabledText - o texto que é exibido inicialmente no botão.</span><span class="sxs-lookup"><span data-stu-id="adfcb-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="adfcb-116">Quando você começa a digitar, o botão exibe o valor da propriedade de texto do botão.</span><span class="sxs-lookup"><span data-stu-id="adfcb-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="adfcb-117">Você conecta o extensor DisabledButton a um controle de botão e caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="adfcb-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="adfcb-118">Antes de digitar qualquer texto, o botão está desabilitado e a caixa de texto e o botão ter esta aparecem:</span><span class="sxs-lookup"><span data-stu-id="adfcb-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="adfcb-119">([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="adfcb-120">Depois que você começa a digitar o texto, o botão é habilitado e a caixa de texto e o botão ter esta aparecem:</span><span class="sxs-lookup"><span data-stu-id="adfcb-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="adfcb-121">([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="adfcb-122">Para criar nossa extensor de controle, precisamos criar três arquivos a seguir:</span><span class="sxs-lookup"><span data-stu-id="adfcb-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="adfcb-123">DisabledButtonExtender.cs - este arquivo é a classe de controle de servidor que irá gerenciar criando extender e permitem que você defina as propriedades em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="adfcb-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="adfcb-124">Ele também define as propriedades que podem ser definidas no extender.</span><span class="sxs-lookup"><span data-stu-id="adfcb-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="adfcb-125">Essas propriedades são acessíveis por meio de código e em tempo de design e correspondem a propriedades definidas no arquivo DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="adfcb-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="adfcb-126">DisabledButtonBehavior.js – Este arquivo é onde você adicionará todos sua lógica de script de cliente.</span><span class="sxs-lookup"><span data-stu-id="adfcb-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="adfcb-127">DisabledButtonDesigner.cs - essa classe permite que a funcionalidade de tempo de design.</span><span class="sxs-lookup"><span data-stu-id="adfcb-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="adfcb-128">Você precisa desta classe se você quiser que o extensor de controle para funcionar corretamente com o Designer do Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="adfcb-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="adfcb-129">Portanto um extensor de controle consiste em um controle de servidor, um comportamento do lado do cliente e uma classe do designer do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="adfcb-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="adfcb-130">Você aprenderá a criar todos os três desses arquivos nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="adfcb-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="adfcb-131">Criando o projeto e o site da Web personalizado extensor</span><span class="sxs-lookup"><span data-stu-id="adfcb-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="adfcb-132">A primeira etapa é criar um projeto de biblioteca de classe e um site no Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="adfcb-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="adfcb-133">Podemos ll criar o extensor personalizado no projeto de biblioteca de classe e testar o extensor personalizado no site.</span><span class="sxs-lookup"><span data-stu-id="adfcb-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="adfcb-134">Permitir que o s iniciar com o site.</span><span class="sxs-lookup"><span data-stu-id="adfcb-134">Let�s start with the website.</span></span> <span data-ttu-id="adfcb-135">Siga estas etapas para criar o site:</span><span class="sxs-lookup"><span data-stu-id="adfcb-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="adfcb-136">Selecione a opção de menu **arquivo, o novo Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="adfcb-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="adfcb-137">Selecione o **Site da Web ASP.NET** modelo.</span><span class="sxs-lookup"><span data-stu-id="adfcb-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="adfcb-138">Nomeie o novo site *Website1*.</span><span class="sxs-lookup"><span data-stu-id="adfcb-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="adfcb-139">Clique o **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="adfcb-139">Click the **OK** button.</span></span>

<span data-ttu-id="adfcb-140">Em seguida, precisamos criar o projeto de biblioteca de classe que contém o código para o extensor de controle:</span><span class="sxs-lookup"><span data-stu-id="adfcb-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="adfcb-141">Selecione a opção de menu **arquivo, adicionar novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="adfcb-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="adfcb-142">Selecione o **biblioteca de classes** modelo.</span><span class="sxs-lookup"><span data-stu-id="adfcb-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="adfcb-143">Nomear a nova biblioteca de classe com o nome **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="adfcb-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="adfcb-144">Clique o **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="adfcb-144">Click the **OK** button.</span></span>

<span data-ttu-id="adfcb-145">Depois de concluir essas etapas, a janela do Gerenciador de soluções deve parecer com a Figura 1.</span><span class="sxs-lookup"><span data-stu-id="adfcb-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="adfcb-146">[![Solução de projeto de biblioteca de classe e de site](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="adfcb-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="adfcb-147">**Figura 01**: solução com o projeto de biblioteca de classe e de site ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="adfcb-148">Em seguida, você precisa adicionar todas as referências de assembly necessárias para o projeto de biblioteca de classe:</span><span class="sxs-lookup"><span data-stu-id="adfcb-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="adfcb-149">O projeto CustomExtenders e selecione a opção de menu **adicionar referência**.</span><span class="sxs-lookup"><span data-stu-id="adfcb-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="adfcb-150">Selecione a guia .NET.</span><span class="sxs-lookup"><span data-stu-id="adfcb-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="adfcb-151">Adicione referências aos assemblies a seguir:</span><span class="sxs-lookup"><span data-stu-id="adfcb-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="adfcb-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="adfcb-152">System.Web.dll</span></span>
    2. <span data-ttu-id="adfcb-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="adfcb-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="adfcb-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="adfcb-154">System.Design.dll</span></span>
    4. <span data-ttu-id="adfcb-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="adfcb-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="adfcb-156">Selecione a guia do navegador.</span><span class="sxs-lookup"><span data-stu-id="adfcb-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="adfcb-157">Adicione uma referência ao assembly AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="adfcb-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="adfcb-158">Este assembly está localizado na pasta onde você baixou o Kit de ferramentas de controle AJAX.</span><span class="sxs-lookup"><span data-stu-id="adfcb-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="adfcb-159">Depois de concluir essas etapas, sua pasta de referências de projeto de biblioteca de classe deve parecer com a Figura 2.</span><span class="sxs-lookup"><span data-stu-id="adfcb-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="adfcb-160">[![Pasta de referências com as referências necessárias](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="adfcb-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="adfcb-161">**Figura 02**: pasta de referências com as referências necessárias ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="adfcb-162">Criando o extensor de controle personalizado</span><span class="sxs-lookup"><span data-stu-id="adfcb-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="adfcb-163">Agora que temos nossa biblioteca de classe, podemos começar a criação de nosso controle do extensor.</span><span class="sxs-lookup"><span data-stu-id="adfcb-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="adfcb-164">Permitir que o s começar com o esqueleto de uma classe de controle do extensor personalizado (consulte a listagem 1).</span><span class="sxs-lookup"><span data-stu-id="adfcb-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="adfcb-165">**Listando 1 - MyCustomExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="adfcb-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="adfcb-166">Há várias coisas a observar sobre a classe de extensor de controle na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="adfcb-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="adfcb-167">Primeiro, observe que a classe herda da classe base ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="adfcb-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="adfcb-168">Todos os controles do extensor AJAX Control Toolkit derivam desta classe base.</span><span class="sxs-lookup"><span data-stu-id="adfcb-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="adfcb-169">Por exemplo, a classe base inclui a propriedade TargetID que é uma propriedade necessária de cada extensor de controle.</span><span class="sxs-lookup"><span data-stu-id="adfcb-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="adfcb-170">Em seguida, observe que a classe inclui os seguintes dois atributos relacionados ao script de cliente:</span><span class="sxs-lookup"><span data-stu-id="adfcb-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="adfcb-171">WebResource - faz com que um arquivo a ser incluído como um recurso inserido em um assembly.</span><span class="sxs-lookup"><span data-stu-id="adfcb-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="adfcb-172">ClientScriptResource - faz com que um recurso de script a ser recuperado de um assembly.</span><span class="sxs-lookup"><span data-stu-id="adfcb-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="adfcb-173">O atributo WebResource é usado para inserir o arquivo MyControlBehavior.js JavaScript no assembly quando o extensor personalizado é compilado.</span><span class="sxs-lookup"><span data-stu-id="adfcb-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="adfcb-174">O atributo ClientScriptResource é usado para recuperar o script MyControlBehavior.js do assembly quando o extensor personalizado for usado em uma página da web.</span><span class="sxs-lookup"><span data-stu-id="adfcb-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="adfcb-175">Para os atributos WebResource e ClientScriptResource trabalhar, você deve compilar o arquivo JavaScript como um recurso incorporado.</span><span class="sxs-lookup"><span data-stu-id="adfcb-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="adfcb-176">Selecione o arquivo na janela do Gerenciador de soluções, abra a folha de propriedades e atribuir o valor *recurso incorporado* para o **ação de compilação** propriedade.</span><span class="sxs-lookup"><span data-stu-id="adfcb-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="adfcb-177">Observe que o extensor de controle também inclui um atributo TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="adfcb-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="adfcb-178">Este atributo é usado para especificar o tipo de controle que será estendido o extensor do controle.</span><span class="sxs-lookup"><span data-stu-id="adfcb-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="adfcb-179">No caso de listagem 1, o extensor do controle é usado para estender uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="adfcb-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="adfcb-180">Finalmente, observe que o extensor personalizado inclui uma propriedade chamada MyProperty.</span><span class="sxs-lookup"><span data-stu-id="adfcb-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="adfcb-181">A propriedade é marcada com o atributo ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="adfcb-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="adfcb-182">Os métodos GetPropertyValue() e SetPropertyValue() são usados para passar o valor da propriedade de extensor de controle de servidor para o comportamento do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="adfcb-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="adfcb-183">Permitem s vá em frente e implementar o código para nosso extensor DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="adfcb-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="adfcb-184">O código para este extensor pode ser encontrado na lista 2.</span><span class="sxs-lookup"><span data-stu-id="adfcb-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="adfcb-185">**A listagem 2 - DisabledButtonExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="adfcb-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="adfcb-186">O extensor DisabledButton na listagem 2 tem duas propriedades chamadas TargetButtonID e DisabledText.</span><span class="sxs-lookup"><span data-stu-id="adfcb-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="adfcb-187">O IDReferenceProperty aplicado à propriedade TargetButtonID impede que você atribuir qualquer coisa que não seja a ID de um controle de botão para essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="adfcb-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="adfcb-188">Os atributos WebResource e ClientScriptResource associar um comportamento do cliente localizado em um arquivo chamado DisabledButtonBehavior.js com este extensor.</span><span class="sxs-lookup"><span data-stu-id="adfcb-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="adfcb-189">Abordaremos esse arquivo JavaScript na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="adfcb-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="adfcb-190">Criando o comportamento de extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="adfcb-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="adfcb-191">O componente do lado do cliente de um extensor de controle é chamado um comportamento.</span><span class="sxs-lookup"><span data-stu-id="adfcb-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="adfcb-192">A lógica real para desabilitar e habilitar o botão está contida no comportamento DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="adfcb-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="adfcb-193">O código JavaScript para o comportamento está incluído na listagem 3.</span><span class="sxs-lookup"><span data-stu-id="adfcb-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="adfcb-194">**A listagem 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="adfcb-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="adfcb-195">O arquivo JavaScript na listagem 3 contém uma classe de cliente chamada DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="adfcb-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="adfcb-196">Essa classe, como duas seu lado do servidor, inclui duas propriedades chamadas TargetButtonID e DisabledText que você pode acessar usando obter\_TargetButtonID/set\_TargetButtonID e obter\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="adfcb-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="adfcb-197">O método Initialize associa o elemento de destino para o comportamento de um manipulador de evento keyup.</span><span class="sxs-lookup"><span data-stu-id="adfcb-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="adfcb-198">Cada vez que você digitar uma letra na caixa de texto associada a esse comportamento, o manipulador keyup executa.</span><span class="sxs-lookup"><span data-stu-id="adfcb-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="adfcb-199">O manipulador keyup habilita ou desabilita o botão dependendo se a caixa de texto associada ao comportamento contém qualquer texto.</span><span class="sxs-lookup"><span data-stu-id="adfcb-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="adfcb-200">Lembre-se de que você deve compilar o arquivo JavaScript na listagem 3 como um recurso inserido.</span><span class="sxs-lookup"><span data-stu-id="adfcb-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="adfcb-201">Selecione o arquivo na janela do Gerenciador de soluções, abra a folha de propriedades e atribuir o valor *recurso incorporado* para o **ação de compilação** propriedade (consulte a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="adfcb-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="adfcb-202">Essa opção está disponível no Visual Studio e o Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="adfcb-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="adfcb-203">[![Adicionando um arquivo JavaScript como um recurso inserido](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="adfcb-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="adfcb-204">**Figura 03**: adicionando um arquivo JavaScript como um recurso incorporado ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="adfcb-205">Criando o Designer de extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="adfcb-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="adfcb-206">Há uma classe última que precisamos criar para concluir nosso extensor.</span><span class="sxs-lookup"><span data-stu-id="adfcb-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="adfcb-207">É preciso criar a classe do designer na listagem 4.</span><span class="sxs-lookup"><span data-stu-id="adfcb-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="adfcb-208">Essa classe é necessária para tornar o extensor se comportem corretamente com o Designer do Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="adfcb-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="adfcb-209">**A listagem 4 - DisabledButtonDesigner.cs**</span><span class="sxs-lookup"><span data-stu-id="adfcb-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="adfcb-210">Associe o designer na listagem 4 o extensor DisabledButton com o atributo do Designer. Você precisa aplicar o atributo de Designer para a classe DisabledButtonExtender como este:</span><span class="sxs-lookup"><span data-stu-id="adfcb-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="adfcb-211">Usando o extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="adfcb-211">Using the Custom Extender</span></span>

<span data-ttu-id="adfcb-212">Agora que estamos terminar de criar o extensor do controle DisabledButton, é hora de usá-lo em nosso site da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="adfcb-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="adfcb-213">Primeiro, precisamos adicionar o extensor personalizado à caixa de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="adfcb-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="adfcb-214">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="adfcb-214">Follow these steps:</span></span>

1. <span data-ttu-id="adfcb-215">Abra uma página ASP.NET clicando duas vezes a página na janela do Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="adfcb-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="adfcb-216">A caixa de ferramentas e selecione a opção de menu **escolher itens**.</span><span class="sxs-lookup"><span data-stu-id="adfcb-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="adfcb-217">Na caixa de diálogo Escolher itens da caixa de ferramentas, navegue até o assembly CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="adfcb-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="adfcb-218">Clique o **Okey** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="adfcb-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="adfcb-219">Depois de concluir essas etapas, o extensor do controle DisabledButton deve aparecer na caixa de ferramentas (consulte a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="adfcb-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="adfcb-220">[![DisabledButton na caixa de ferramentas](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="adfcb-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="adfcb-221">**Figura 04**: DisabledButton na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="adfcb-222">Em seguida, é preciso criar uma nova página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="adfcb-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="adfcb-223">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="adfcb-223">Follow these steps:</span></span>

1. <span data-ttu-id="adfcb-224">Crie uma nova página ASP.NET chamada ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="adfcb-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="adfcb-225">Arraste um ScriptManager na página.</span><span class="sxs-lookup"><span data-stu-id="adfcb-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="adfcb-226">Arraste um controle de caixa de texto para a página.</span><span class="sxs-lookup"><span data-stu-id="adfcb-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="adfcb-227">Arraste um controle de botão para a página.</span><span class="sxs-lookup"><span data-stu-id="adfcb-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="adfcb-228">Na janela Propriedades, altere a propriedade de ID de botões para o valor *btnSave* e a propriedade de texto como o valor *salvar\**.</span><span class="sxs-lookup"><span data-stu-id="adfcb-228">In the Properties window, change the Button ID property to the value *btnSave* and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="adfcb-229">Nós criamos uma página com um controle de caixa de texto do ASP.NET e o botão padrão.</span><span class="sxs-lookup"><span data-stu-id="adfcb-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="adfcb-230">Em seguida, é necessário estender o controle de caixa de texto com o extensor DisabledButton:</span><span class="sxs-lookup"><span data-stu-id="adfcb-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="adfcb-231">Selecione o **adicionar extensor** opção para abrir a caixa de diálogo do Assistente de extensor de tarefa (consulte a Figura 5).</span><span class="sxs-lookup"><span data-stu-id="adfcb-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="adfcb-232">Observe que a caixa de diálogo inclui nosso extensor DisabledButton personalizado.</span><span class="sxs-lookup"><span data-stu-id="adfcb-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="adfcb-233">Selecione o extensor DisabledButton e clique no **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="adfcb-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="adfcb-234">[![A caixa de diálogo do Assistente de extensor](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="adfcb-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="adfcb-235">**Figura 05**: caixa de diálogo do Assistente de extensor ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="adfcb-236">Por fim, podemos definir as propriedades do extensor DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="adfcb-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="adfcb-237">Você pode modificar as propriedades do extensor DisabledButton, modificando as propriedades do controle de caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="adfcb-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="adfcb-238">Selecione a caixa de texto no Designer.</span><span class="sxs-lookup"><span data-stu-id="adfcb-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="adfcb-239">Na janela Propriedades, expanda o nó de extensores (veja a Figura 6).</span><span class="sxs-lookup"><span data-stu-id="adfcb-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="adfcb-240">Atribuir o valor *salvar* para a propriedade DisabledText e o valor *btnSave* à propriedade TargetButtonID.</span><span class="sxs-lookup"><span data-stu-id="adfcb-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="adfcb-241">[![Definindo propriedades estendidas](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="adfcb-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="adfcb-242">**Figura 06**: definindo as propriedades estendidas ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="adfcb-243">Quando você executa a página (pressionando F5), o controle de botão é inicialmente desabilitado.</span><span class="sxs-lookup"><span data-stu-id="adfcb-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="adfcb-244">Como começar a inserir o texto na caixa de texto, o botão de controle está habilitado (veja a Figura 7).</span><span class="sxs-lookup"><span data-stu-id="adfcb-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="adfcb-245">[![O extensor DisabledButton em ação](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="adfcb-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="adfcb-246">**Figura 07**: DisabledButton o extensor em ação ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="adfcb-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="adfcb-247">Resumo</span><span class="sxs-lookup"><span data-stu-id="adfcb-247">Summary</span></span>

<span data-ttu-id="adfcb-248">O objetivo deste tutorial era explicam como você pode estender o Kit de ferramentas de controle AJAX com controles do extensor personalizado.</span><span class="sxs-lookup"><span data-stu-id="adfcb-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="adfcb-249">Neste tutorial, criamos um extensor de controle de DisabledButton simple.</span><span class="sxs-lookup"><span data-stu-id="adfcb-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="adfcb-250">Implementamos desse extensor, criando uma classe DisabledButtonExtender, um comportamento DisabledButtonBehavior JavaScript e uma classe DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="adfcb-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="adfcb-251">Você seguir um conjunto similar de etapas sempre que você criar um extensor de controle personalizado.</span><span class="sxs-lookup"><span data-stu-id="adfcb-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="adfcb-252">[Anterior](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Próximo](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="adfcb-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
