---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Criando personalizado do AJAX controle de extensor de controle do Kit de ferramentas (VB) | Microsoft Docs
author: microsoft
description: Extensores personalizados permitem personalizar e estender os recursos dos controles do ASP.NET sem precisar criar novas classes.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f0cbee47b541e31f3e9f01e42afeabcd7b9769f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833764"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a><span data-ttu-id="ddaed-103">Criar um personalizado extensor AJAX Control Toolkit (VB)</span><span class="sxs-lookup"><span data-stu-id="ddaed-103">Creating a Custom AJAX Control Toolkit Control Extender (VB)</span></span>
====================
<span data-ttu-id="ddaed-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ddaed-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ddaed-105">Extensores personalizados permitem personalizar e estender os recursos dos controles do ASP.NET sem precisar criar novas classes.</span><span class="sxs-lookup"><span data-stu-id="ddaed-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="ddaed-106">Neste tutorial, você aprenderá como criar um extensor de controle personalizado do AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="ddaed-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="ddaed-107">Podemos criar um simples, mas útil, novo extensor que altera o estado de um botão de desabilitado para habilitado quando você digita texto em uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="ddaed-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="ddaed-108">Depois de ler este tutorial, você poderá estender o ASP.NET AJAX Toolkit com seus próprio extensores de controle.</span><span class="sxs-lookup"><span data-stu-id="ddaed-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="ddaed-109">Você pode criar extensores de controle personalizado usando o Visual Studio ou Visual Web Developer (certifique-se de que você tenha a versão mais recente do Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="ddaed-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="ddaed-110">Visão geral do extensor DisabledButton</span><span class="sxs-lookup"><span data-stu-id="ddaed-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="ddaed-111">O extensor DisabledButton é chamado de nosso novo extensor de controle.</span><span class="sxs-lookup"><span data-stu-id="ddaed-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="ddaed-112">Desse extensor terá três propriedades:</span><span class="sxs-lookup"><span data-stu-id="ddaed-112">This extender will have three properties:</span></span>

- <span data-ttu-id="ddaed-113">TargetControlID - caixa de texto que estende o controle.</span><span class="sxs-lookup"><span data-stu-id="ddaed-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="ddaed-114">TargetButtonIID - o botão que é habilitado ou desabilitado.</span><span class="sxs-lookup"><span data-stu-id="ddaed-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="ddaed-115">DisabledText - o texto que é exibido inicialmente no botão.</span><span class="sxs-lookup"><span data-stu-id="ddaed-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="ddaed-116">Quando você começa a digitar, o botão exibe o valor da propriedade de texto do botão.</span><span class="sxs-lookup"><span data-stu-id="ddaed-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="ddaed-117">Você vincular o extensor DisabledButton a um controle TextBox e Button.</span><span class="sxs-lookup"><span data-stu-id="ddaed-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="ddaed-118">Antes de digitar qualquer texto, o botão está desabilitado e a caixa de texto e o botão ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="ddaed-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

<span data-ttu-id="ddaed-119">([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span></span>


<span data-ttu-id="ddaed-120">Depois de começar a digitar texto, o botão está habilitado e a caixa de texto e o botão ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="ddaed-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

<span data-ttu-id="ddaed-121">([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="ddaed-122">Para criar nosso extensor de controle, é preciso criar três arquivos a seguir:</span><span class="sxs-lookup"><span data-stu-id="ddaed-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="ddaed-123">DisabledButtonExtender.vb - esse arquivo é a classe de controle de servidor que gerenciará a criação de seu extensor e permitem que você defina as propriedades em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="ddaed-123">DisabledButtonExtender.vb - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="ddaed-124">Ele também define as propriedades que podem ser definidas em seu extensor.</span><span class="sxs-lookup"><span data-stu-id="ddaed-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="ddaed-125">Essas propriedades são acessíveis por meio de código e em tempo de design e corresponde às propriedades definidas no arquivo DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="ddaed-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="ddaed-126">DisabledButtonBehavior.js – Esse arquivo é onde você adicionará todos de sua lógica de script de cliente.</span><span class="sxs-lookup"><span data-stu-id="ddaed-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="ddaed-127">DisabledButtonDesigner.vb - essa classe habilita a funcionalidade de tempo de design.</span><span class="sxs-lookup"><span data-stu-id="ddaed-127">DisabledButtonDesigner.vb - This class enables design-time functionality.</span></span> <span data-ttu-id="ddaed-128">Se você quiser que o extensor do controle para funcionar corretamente com o Designer do Visual Studio/Visual Web Developer, você precisa dessa classe.</span><span class="sxs-lookup"><span data-stu-id="ddaed-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="ddaed-129">Portanto, um extensor de controle consiste em um controle de servidor, um comportamento do lado do cliente e uma classe de designer do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="ddaed-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="ddaed-130">Você aprenderá a criar todos os três desses arquivos nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="ddaed-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="ddaed-131">Criando o projeto e o site do extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="ddaed-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="ddaed-132">A primeira etapa é criar um site da Web e um projeto de biblioteca de classes no Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="ddaed-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="ddaed-133">Podemos ll criar o extensor personalizado no projeto de biblioteca de classes e testar o extensor personalizado no site.</span><span class="sxs-lookup"><span data-stu-id="ddaed-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="ddaed-134">Permitir que o s comece com o site.</span><span class="sxs-lookup"><span data-stu-id="ddaed-134">Let�s start with the website.</span></span> <span data-ttu-id="ddaed-135">Siga estas etapas para criar o site:</span><span class="sxs-lookup"><span data-stu-id="ddaed-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="ddaed-136">Selecione a opção de menu **arquivo, novo Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="ddaed-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="ddaed-137">Selecione o **Site da Web ASP.NET** modelo.</span><span class="sxs-lookup"><span data-stu-id="ddaed-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="ddaed-138">Nomeie o novo site *Website1*.</span><span class="sxs-lookup"><span data-stu-id="ddaed-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="ddaed-139">Clique o **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="ddaed-139">Click the **OK** button.</span></span>

<span data-ttu-id="ddaed-140">Em seguida, precisamos criar o projeto de biblioteca de classes que contém o código para o extensor do controle:</span><span class="sxs-lookup"><span data-stu-id="ddaed-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="ddaed-141">Selecione a opção de menu **novo projeto do arquivo, adicionar,**.</span><span class="sxs-lookup"><span data-stu-id="ddaed-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="ddaed-142">Selecione o **biblioteca de classes** modelo.</span><span class="sxs-lookup"><span data-stu-id="ddaed-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="ddaed-143">Nomeie a nova biblioteca de classes com o nome **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="ddaed-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="ddaed-144">Clique o **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="ddaed-144">Click the **OK** button.</span></span>

<span data-ttu-id="ddaed-145">Depois de concluir essas etapas, a janela do Gerenciador de soluções deve ser semelhante a Figura 1.</span><span class="sxs-lookup"><span data-stu-id="ddaed-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="ddaed-146">[![Solução de projeto de biblioteca de classe e de site](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ddaed-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="ddaed-147">**Figura 01**: a solução com o projeto de biblioteca de classe e de site ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span></span>


<span data-ttu-id="ddaed-148">Em seguida, você precisará adicionar todas as referências de assembly necessárias para o projeto de biblioteca de classes:</span><span class="sxs-lookup"><span data-stu-id="ddaed-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="ddaed-149">Clique com botão direito no projeto CustomExtenders e selecione a opção de menu **adicionar referência**.</span><span class="sxs-lookup"><span data-stu-id="ddaed-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="ddaed-150">Selecione a guia do .NET.</span><span class="sxs-lookup"><span data-stu-id="ddaed-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="ddaed-151">Adicione referências aos assemblies a seguir:</span><span class="sxs-lookup"><span data-stu-id="ddaed-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="ddaed-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="ddaed-152">System.Web.dll</span></span>
    2. <span data-ttu-id="ddaed-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="ddaed-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="ddaed-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="ddaed-154">System.Design.dll</span></span>
    4. <span data-ttu-id="ddaed-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="ddaed-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="ddaed-156">Selecione a guia Browse.</span><span class="sxs-lookup"><span data-stu-id="ddaed-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="ddaed-157">Adicione uma referência ao assembly AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="ddaed-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="ddaed-158">Esse assembly está localizado na pasta onde você baixou o AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="ddaed-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="ddaed-159">Você pode verificar que você tenha adicionado todas as referências corretas clicando duas vezes o seu projeto, selecionando as propriedades, e clicando na guia referências (veja a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="ddaed-159">You can verify that you have added all of the right references by right-clicking your project, selecting Properties, and clicking the References tab (see Figure 2).</span></span>


<span data-ttu-id="ddaed-160">[![Pasta de referências com as referências necessárias](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="ddaed-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span></span>

<span data-ttu-id="ddaed-161">**Figura 02**: a pasta de referências com as referências necessárias ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="ddaed-162">Criando o extensor do controle personalizado</span><span class="sxs-lookup"><span data-stu-id="ddaed-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="ddaed-163">Agora que temos nossa biblioteca de classes, podemos começar a criação de nosso controle de extensor.</span><span class="sxs-lookup"><span data-stu-id="ddaed-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="ddaed-164">Permitir que o s comece com o esqueleto de uma classe de controle de extensor personalizado (consulte a listagem 1).</span><span class="sxs-lookup"><span data-stu-id="ddaed-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="ddaed-165">**Listagem 1 - MyCustomExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="ddaed-165">**Listing 1 - MyCustomExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

<span data-ttu-id="ddaed-166">Há várias coisas que você observar sobre a classe de controle de extensor na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="ddaed-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="ddaed-167">Primeiro, observe que a classe herda da classe base ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="ddaed-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="ddaed-168">Todos os controles de extensor do AJAX Control Toolkit derivam dessa classe base.</span><span class="sxs-lookup"><span data-stu-id="ddaed-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="ddaed-169">Por exemplo, a classe base inclui a propriedade TargetID que é uma propriedade obrigatória do extensor cada controle.</span><span class="sxs-lookup"><span data-stu-id="ddaed-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="ddaed-170">Em seguida, observe que a classe inclui os seguintes dois atributos relacionados ao script de cliente:</span><span class="sxs-lookup"><span data-stu-id="ddaed-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="ddaed-171">WebResource - faz com que um arquivo a ser incluído como um recurso inserido em um assembly.</span><span class="sxs-lookup"><span data-stu-id="ddaed-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="ddaed-172">ClientScriptResource - faz com que um recurso de script a ser recuperado de um assembly.</span><span class="sxs-lookup"><span data-stu-id="ddaed-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="ddaed-173">O atributo WebResource é usado para inserir o arquivo MyControlBehavior.js JavaScript no assembly quando o extensor personalizado é compilado.</span><span class="sxs-lookup"><span data-stu-id="ddaed-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="ddaed-174">O atributo ClientScriptResource é usado para recuperar o script MyControlBehavior.js do assembly quando o extensor personalizado é usado em uma página da web.</span><span class="sxs-lookup"><span data-stu-id="ddaed-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="ddaed-175">Em ordem para os atributos WebResource e ClientScriptResource funcione, você deve compilar o arquivo JavaScript como um recurso inserido.</span><span class="sxs-lookup"><span data-stu-id="ddaed-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="ddaed-176">Selecione o arquivo na janela do Gerenciador de soluções, abra a folha de propriedades e atribua o valor *Embedded Resource* para o **Build Action** propriedade.</span><span class="sxs-lookup"><span data-stu-id="ddaed-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="ddaed-177">Observe que o extensor do controle também inclui um atributo TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="ddaed-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="ddaed-178">Esse atributo é usado para especificar o tipo de controle que será estendido para o extensor do controle.</span><span class="sxs-lookup"><span data-stu-id="ddaed-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="ddaed-179">No caso da listagem 1, o extensor do controle é usado para estender uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="ddaed-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="ddaed-180">Finalmente, observe que o extensor personalizado inclui uma propriedade chamada MyProperty.</span><span class="sxs-lookup"><span data-stu-id="ddaed-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="ddaed-181">A propriedade é marcada com o atributo ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="ddaed-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="ddaed-182">Os métodos GetPropertyValue() e SetPropertyValue() são usados para passar o valor da propriedade do extensor do controle do lado do servidor para o comportamento do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="ddaed-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="ddaed-183">Vá em frente e implementar o código para nosso extensor DisabledButton, permitem que s.</span><span class="sxs-lookup"><span data-stu-id="ddaed-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="ddaed-184">O código para este extensor pode ser encontrado na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="ddaed-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="ddaed-185">**Listagem 2 - DisabledButtonExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="ddaed-185">**Listing 2 - DisabledButtonExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

<span data-ttu-id="ddaed-186">O extensor DisabledButton na listagem 2 tem duas propriedades chamadas TargetButtonID e DisabledText.</span><span class="sxs-lookup"><span data-stu-id="ddaed-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="ddaed-187">O IDReferenceProperty aplicado à propriedade TargetButtonID impede que você atribuir qualquer coisa diferente da ID de um controle de botão para essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="ddaed-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="ddaed-188">Os atributos WebResource e ClientScriptResource associar um comportamento do lado do cliente localizado em um arquivo chamado DisabledButtonBehavior.js com desse extensor.</span><span class="sxs-lookup"><span data-stu-id="ddaed-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="ddaed-189">Discutiremos este arquivo JavaScript na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="ddaed-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="ddaed-190">Criando o comportamento de extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="ddaed-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="ddaed-191">O componente do lado do cliente de um extensor de controle é chamado um comportamento.</span><span class="sxs-lookup"><span data-stu-id="ddaed-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="ddaed-192">A lógica real para desabilitar e habilitar o botão está contida no comportamento DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="ddaed-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="ddaed-193">O código de JavaScript para o comportamento está incluído na listagem 3.</span><span class="sxs-lookup"><span data-stu-id="ddaed-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="ddaed-194">**Listagem 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="ddaed-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

<span data-ttu-id="ddaed-195">O arquivo JavaScript na listagem 3 contém uma classe de cliente chamada DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="ddaed-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="ddaed-196">Essa classe, como seu gêmeo do lado do servidor, inclui duas propriedades chamadas TargetButtonID e DisabledText que você pode acessar usando obter\_TargetButtonID/set\_TargetButtonID e obtenha\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="ddaed-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="ddaed-197">O método Initialize () associa um manipulador de evento keyup com o elemento de destino para o comportamento.</span><span class="sxs-lookup"><span data-stu-id="ddaed-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="ddaed-198">Cada vez que você digitar uma letra na caixa de texto associada a esse comportamento, o manipulador de keyup executa.</span><span class="sxs-lookup"><span data-stu-id="ddaed-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="ddaed-199">O manipulador de keyup habilita ou desabilita o botão dependendo se a caixa de texto associada com o comportamento contém qualquer texto.</span><span class="sxs-lookup"><span data-stu-id="ddaed-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="ddaed-200">Lembre-se de que você deve compilar o arquivo JavaScript na listagem 3 como um recurso inserido.</span><span class="sxs-lookup"><span data-stu-id="ddaed-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="ddaed-201">Selecione o arquivo na janela do Gerenciador de soluções, abra a folha de propriedades e atribua o valor *Embedded Resource* para o **Build Action** propriedade (veja a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="ddaed-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="ddaed-202">Essa opção está disponível no Visual Studio e Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="ddaed-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="ddaed-203">[![Adicionando um arquivo JavaScript como um recurso inserido](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="ddaed-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span></span>

<span data-ttu-id="ddaed-204">**Figura 03**: adicionando um arquivo JavaScript como um recurso inserido ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="ddaed-205">Criando o Designer personalizado de extensão</span><span class="sxs-lookup"><span data-stu-id="ddaed-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="ddaed-206">Há uma última classe precisamos criar para concluir nosso extensor.</span><span class="sxs-lookup"><span data-stu-id="ddaed-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="ddaed-207">Precisamos criar a classe de designer na listagem 4.</span><span class="sxs-lookup"><span data-stu-id="ddaed-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="ddaed-208">Essa classe é necessária para tornar o extensor se comportar corretamente com o Designer do Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="ddaed-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="ddaed-209">**Listagem 4 - DisabledButtonDesigner.vb**</span><span class="sxs-lookup"><span data-stu-id="ddaed-209">**Listing 4 - DisabledButtonDesigner.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

<span data-ttu-id="ddaed-210">Você associa o designer na listagem 4 com o extensor DisabledButton com o atributo de Designer. Você precisa aplicar o atributo de Designer para a classe DisabledButtonExtender como este:</span><span class="sxs-lookup"><span data-stu-id="ddaed-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="ddaed-211">Usar o extensor personalizado</span><span class="sxs-lookup"><span data-stu-id="ddaed-211">Using the Custom Extender</span></span>

<span data-ttu-id="ddaed-212">Agora que podemos terminar de criar o extensor do controle DisabledButton, é hora de usá-lo em nosso site do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ddaed-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="ddaed-213">Primeiro, precisamos adicionar o extensor personalizado à caixa de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="ddaed-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="ddaed-214">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="ddaed-214">Follow these steps:</span></span>

1. <span data-ttu-id="ddaed-215">Abra uma página ASP.NET clicando duas vezes a página na janela do Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="ddaed-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="ddaed-216">A caixa de ferramentas com o botão direito e selecione a opção de menu **escolher itens**.</span><span class="sxs-lookup"><span data-stu-id="ddaed-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="ddaed-217">Na caixa de diálogo Escolher itens da caixa de ferramentas, navegue até o assembly CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="ddaed-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="ddaed-218">Clique o **Okey** botão para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="ddaed-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="ddaed-219">Depois de concluir essas etapas, o extensor do controle DisabledButton deve aparecer na caixa de ferramentas (veja a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="ddaed-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="ddaed-220">[![DisabledButton na caixa de ferramentas](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="ddaed-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span></span>

<span data-ttu-id="ddaed-221">**Figura 04**: DisabledButton na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span></span>


<span data-ttu-id="ddaed-222">Em seguida, precisamos criar uma nova página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ddaed-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="ddaed-223">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="ddaed-223">Follow these steps:</span></span>

1. <span data-ttu-id="ddaed-224">Crie uma nova página do ASP.NET chamada ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="ddaed-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="ddaed-225">Arraste um ScriptManager à página.</span><span class="sxs-lookup"><span data-stu-id="ddaed-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="ddaed-226">Arraste um controle de caixa de texto para a página.</span><span class="sxs-lookup"><span data-stu-id="ddaed-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="ddaed-227">Arraste um controle de botão para a página.</span><span class="sxs-lookup"><span data-stu-id="ddaed-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="ddaed-228">Na janela Propriedades, altere a propriedade ID do botão para o valor <em>btnSave</em> e a propriedade de texto como o valor *salve\**.</span><span class="sxs-lookup"><span data-stu-id="ddaed-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="ddaed-229">Criamos uma página com um controle TextBox do ASP.NET e o botão padrão.</span><span class="sxs-lookup"><span data-stu-id="ddaed-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="ddaed-230">Em seguida, precisamos estender o controle de caixa de texto com o extensor DisabledButton:</span><span class="sxs-lookup"><span data-stu-id="ddaed-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="ddaed-231">Selecione o **adicionar extensor** opção para abrir a caixa de diálogo do Assistente de extensor de tarefa (consulte a Figura 5).</span><span class="sxs-lookup"><span data-stu-id="ddaed-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="ddaed-232">Observe que a caixa de diálogo inclui nosso extensor DisabledButton personalizado.</span><span class="sxs-lookup"><span data-stu-id="ddaed-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="ddaed-233">Selecione o extensor DisabledButton e clique no **Okey** botão.</span><span class="sxs-lookup"><span data-stu-id="ddaed-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="ddaed-234">[![A caixa de diálogo do Assistente de extensor](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="ddaed-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span></span>

<span data-ttu-id="ddaed-235">**Figura 05**: caixa de diálogo Assistente de extensor a ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span></span>


<span data-ttu-id="ddaed-236">Por fim, podemos pode definir as propriedades do extensor DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="ddaed-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="ddaed-237">Você pode modificar as propriedades do extensor DisabledButton modificando as propriedades do controle de caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="ddaed-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="ddaed-238">Selecione a caixa de texto no Designer.</span><span class="sxs-lookup"><span data-stu-id="ddaed-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="ddaed-239">Na janela Propriedades, expanda o nó de extensores (veja a Figura 6).</span><span class="sxs-lookup"><span data-stu-id="ddaed-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="ddaed-240">Atribua o valor *salve* para a propriedade DisabledText e o valor *btnSave* à propriedade TargetButtonID.</span><span class="sxs-lookup"><span data-stu-id="ddaed-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="ddaed-241">[![Definindo propriedades do extensor](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="ddaed-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span></span>

<span data-ttu-id="ddaed-242">**Figura 06**: definindo propriedades do extensor ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span></span>


<span data-ttu-id="ddaed-243">Quando você executa a página (pressionando F5), o controle de botão é inicialmente desabilitado.</span><span class="sxs-lookup"><span data-stu-id="ddaed-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="ddaed-244">Assim que você começar a inserir texto na caixa de texto, o botão de controle está habilitado (veja a Figura 7).</span><span class="sxs-lookup"><span data-stu-id="ddaed-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="ddaed-245">[![O extensor DisabledButton em ação](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="ddaed-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span></span>

<span data-ttu-id="ddaed-246">**Figura 07**: DisabledButton o extensor em ação ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="ddaed-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="ddaed-247">Resumo</span><span class="sxs-lookup"><span data-stu-id="ddaed-247">Summary</span></span>

<span data-ttu-id="ddaed-248">O objetivo deste tutorial foi explicar como você pode estender o AJAX Control Toolkit com controles do extensor personalizado.</span><span class="sxs-lookup"><span data-stu-id="ddaed-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="ddaed-249">Neste tutorial, criamos um extensor de controle DisabledButton simple.</span><span class="sxs-lookup"><span data-stu-id="ddaed-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="ddaed-250">Implementamos desse extensor, criando uma classe DisabledButtonExtender, um comportamento DisabledButtonBehavior JavaScript e uma classe DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="ddaed-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="ddaed-251">Você seguir um conjunto semelhante de etapas sempre que você criar um extensor de controle personalizado.</span><span class="sxs-lookup"><span data-stu-id="ddaed-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ddaed-252">Anterior</span><span class="sxs-lookup"><span data-stu-id="ddaed-252">Previous</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
