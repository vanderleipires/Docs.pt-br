---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Criando um MVC 3 aplicativo com Razor e o JavaScript discreto | Microsoft Docs
author: microsoft
description: "O aplicativo de web de exemplo da lista de usuários demonstra como é simples para criar aplicativos ASP.NET MVC 3 usando o mecanismo de exibição Razor. S de aplicativo de exemplo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 29b45c07b5498542abbf22c4c3001b1cee41edc9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="d8100-104">Criando um MVC 3 aplicativo com Razor e o JavaScript discreto</span><span class="sxs-lookup"><span data-stu-id="d8100-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="d8100-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d8100-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d8100-106">O aplicativo de web de exemplo da lista de usuários demonstra como é simples para criar aplicativos ASP.NET MVC 3 usando o mecanismo de exibição Razor.</span><span class="sxs-lookup"><span data-stu-id="d8100-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="d8100-107">O aplicativo de exemplo mostra como usar o novo mecanismo de exibição Razor com o ASP.NET MVC versão 3 e o Visual Studio 2010 para criar um site de lista de usuários fictício que inclui a funcionalidade de como criar, exibir, editar e excluir usuários.</span><span class="sxs-lookup"><span data-stu-id="d8100-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="d8100-108">Este tutorial descreve as etapas que foram usadas para criar o aplicativo do ASP.NET MVC 3 de exemplo lista de usuários.</span><span class="sxs-lookup"><span data-stu-id="d8100-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="d8100-109">Um projeto do Visual Studio com o código-fonte c# e VB está disponível para acompanhar este tópico: [baixar](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="d8100-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="d8100-110">Se você tiver dúvidas sobre este tutorial, poste-os para o [Fórum do MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8100-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="d8100-111">Visão geral</span><span class="sxs-lookup"><span data-stu-id="d8100-111">Overview</span></span>

<span data-ttu-id="d8100-112">O aplicativo que você criará é um site da lista de usuário simples.</span><span class="sxs-lookup"><span data-stu-id="d8100-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="d8100-113">Os usuários podem inserir, exibir e atualizar informações de usuário.</span><span class="sxs-lookup"><span data-stu-id="d8100-113">Users can enter, view, and update user information.</span></span>

![Site de exemplo](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="d8100-115">Você pode baixar o projeto concluído VB e c# [aqui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="d8100-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="d8100-116">Criando o aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="d8100-116">Creating the Web Application</span></span>

<span data-ttu-id="d8100-117">Para iniciar o tutorial, abra o Visual Studio 2010 e crie um novo projeto usando o *aplicativo Web do ASP.NET MVC 3* modelo.</span><span class="sxs-lookup"><span data-stu-id="d8100-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="d8100-118">Nomeie o aplicativo &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="d8100-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="d8100-119">[![Novo projeto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="d8100-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="d8100-120">No **novo projeto do ASP.NET MVC 3** caixa de diálogo, selecione **aplicativo de Internet**, selecione o mecanismo de exibição Razor e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="d8100-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Caixa de diálogo Nova projeto do ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="d8100-122">Neste tutorial você não usará o provedor de associação do ASP.NET, portanto você pode excluir todos os arquivos associados ao logon e associação.</span><span class="sxs-lookup"><span data-stu-id="d8100-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="d8100-123">Em **Solution Explorer**, remova os arquivos e diretórios a seguir:</span><span class="sxs-lookup"><span data-stu-id="d8100-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="d8100-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="d8100-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="d8100-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="d8100-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="d8100-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="d8100-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="d8100-127">*Views\Account* (e todos os arquivos neste diretório)</span><span class="sxs-lookup"><span data-stu-id="d8100-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="d8100-129">Editar o  *\_cshtml* de arquivo e substitua a marcação dentro de `<div>` elemento chamado `logindisplay` com a mensagem  *&quot;* logon desabilitado&quot;.</span><span class="sxs-lookup"><span data-stu-id="d8100-129">Edit the *\_Layout.cshtml* file and replace the markup inside the `<div>` element named `logindisplay` with the message *&quot;*Login Disabled&quot;.</span></span> <span data-ttu-id="d8100-130">O exemplo a seguir mostra a marcação novo:</span><span class="sxs-lookup"><span data-stu-id="d8100-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="d8100-131">Adicionar modelo</span><span class="sxs-lookup"><span data-stu-id="d8100-131">Adding the Model</span></span>

<span data-ttu-id="d8100-132">Em **Solution Explorer**, com o botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, clique em **classe**.</span><span class="sxs-lookup"><span data-stu-id="d8100-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nova classe de usuário Mdl](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="d8100-134">Nomeie a classe `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="d8100-134">Name the class `UserModel`.</span></span> <span data-ttu-id="d8100-135">Substitua o conteúdo do *UserModel* arquivo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d8100-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="d8100-136">O `UserModel` classe representa os usuários.</span><span class="sxs-lookup"><span data-stu-id="d8100-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="d8100-137">Cada membro da classe é anotado com a [necessário](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) de atributo do [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span><span class="sxs-lookup"><span data-stu-id="d8100-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="d8100-138">Os atributos de [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace fornecem validação automática de lado cliente e servidor para aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="d8100-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="d8100-139">Abra o `HomeController` classe e adicione um `using` diretiva para que você possa acessar o `UserModel` e `Users` classes:</span><span class="sxs-lookup"><span data-stu-id="d8100-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="d8100-140">Logo após o `HomeController` declaração, adicione o seguinte comentário e a referência a um `Users` classe:</span><span class="sxs-lookup"><span data-stu-id="d8100-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="d8100-141">O `Users` classe é um repositório de dados simplificada, de memória que você usará neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="d8100-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="d8100-142">Em um aplicativo real, você usaria um banco de dados para armazenar informações do usuário.</span><span class="sxs-lookup"><span data-stu-id="d8100-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="d8100-143">As primeiras linhas do `HomeController` arquivo são mostradas no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d8100-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="d8100-144">Compile o aplicativo para que o modelo de usuário estarão disponível para o Assistente de scaffolding na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="d8100-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="d8100-145">Criar modo de exibição padrão</span><span class="sxs-lookup"><span data-stu-id="d8100-145">Creating the Default View</span></span>

<span data-ttu-id="d8100-146">A próxima etapa é adicionar um método de ação e o modo de exibição para exibir os usuários.</span><span class="sxs-lookup"><span data-stu-id="d8100-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="d8100-147">Exclua o existente *Views\Home\Index* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d8100-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="d8100-148">Você criará um novo *índice* arquivo para exibir os usuários.</span><span class="sxs-lookup"><span data-stu-id="d8100-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="d8100-149">No `HomeController` classe, substitua o conteúdo do `Index` método com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d8100-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="d8100-150">Clique dentro do `Index` método e depois clique em **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="d8100-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Adicionar modo de exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="d8100-152">Selecione o **criar uma exibição fortemente tipada** opção.</span><span class="sxs-lookup"><span data-stu-id="d8100-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="d8100-153">Para **exibir dados de classe**, selecione **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="d8100-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="d8100-154">(Se você não vir **Mvc3Razor.Models.UserModel** no **exibir dados de classe** caixa, você precisa compilar o projeto.) Certifique-se de que o mecanismo de exibição está definido como **Razor**.</span><span class="sxs-lookup"><span data-stu-id="d8100-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="d8100-155">Definir **exibir conteúdo** para **lista** e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="d8100-155">Set **View content** to **List** and then click **Add**.</span></span>

![Adicionar exibição de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="d8100-157">A nova exibição scaffolds automaticamente os dados do usuário que são passados para o `Index` exibição.</span><span class="sxs-lookup"><span data-stu-id="d8100-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="d8100-158">Examine o recém-gerado *Views\Home\Index* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d8100-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="d8100-159">O **criar novo**, **editar**, **detalhes**, e **excluir** links não funcionam, mas o restante da página é funcional.</span><span class="sxs-lookup"><span data-stu-id="d8100-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="d8100-160">Execute a página.</span><span class="sxs-lookup"><span data-stu-id="d8100-160">Run the page.</span></span> <span data-ttu-id="d8100-161">Você verá uma lista de usuários.</span><span class="sxs-lookup"><span data-stu-id="d8100-161">You see a list of users.</span></span>

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="d8100-163">Abra o *cshtml* de arquivo e substitua o `ActionLink` marcação para **editar**, **detalhes**, e **excluir** com o código a seguir :</span><span class="sxs-lookup"><span data-stu-id="d8100-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="d8100-164">O nome de usuário é usado como a ID para localizar o registro selecionado no **editar**, **detalhes**, e **excluir** links.</span><span class="sxs-lookup"><span data-stu-id="d8100-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="d8100-165">Criar a exibição de detalhes</span><span class="sxs-lookup"><span data-stu-id="d8100-165">Creating the Details View</span></span>

<span data-ttu-id="d8100-166">A próxima etapa é adicionar um `Details` método de ação e o modo de exibição para exibir detalhes do usuário.</span><span class="sxs-lookup"><span data-stu-id="d8100-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="d8100-168">Adicione o seguinte `Details` método para o controlador principal:</span><span class="sxs-lookup"><span data-stu-id="d8100-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="d8100-169">Clique dentro do `Details` método e, em seguida, selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="d8100-169">Right-click inside the `Details` method and then select **Add View**.</span></span> <span data-ttu-id="d8100-170">Verifique o **exibir dados de classe** caixa contém **Mvc3Razor.Models.UserModel***.*</span><span class="sxs-lookup"><span data-stu-id="d8100-170">Verify that the **View data class** box contains **Mvc3Razor.Models.UserModel***.*</span></span> <span data-ttu-id="d8100-171">Definir **exibir conteúdo** para **detalhes** e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="d8100-171">Set **View content** to **Details** and then click **Add**.</span></span>

![Adicionar exibição de detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="d8100-173">Execute o aplicativo e selecione um link de detalhes.</span><span class="sxs-lookup"><span data-stu-id="d8100-173">Run the application and select a details link.</span></span> <span data-ttu-id="d8100-174">O scaffolding automática mostra cada propriedade no modelo.</span><span class="sxs-lookup"><span data-stu-id="d8100-174">The automatic scaffolding shows each property in the model.</span></span>

![Detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="d8100-176">Criar a exibição de edição</span><span class="sxs-lookup"><span data-stu-id="d8100-176">Creating the Edit View</span></span>

<span data-ttu-id="d8100-177">Adicione o seguinte `Edit` método para o controlador principal.</span><span class="sxs-lookup"><span data-stu-id="d8100-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="d8100-178">Adicionar um modo de exibição como as etapas anteriores, mas definir **exibir conteúdo** para **editar**.</span><span class="sxs-lookup"><span data-stu-id="d8100-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Adicionar exibição de edição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="d8100-180">Execute o aplicativo e edite o nome e sobrenome de um dos usuários.</span><span class="sxs-lookup"><span data-stu-id="d8100-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="d8100-181">Se você violar qualquer `DataAnnotation` restrições que foram aplicadas a `UserModel` classe, ao enviar o formulário, você verá erros de validação que são produzidos pelo código do servidor.</span><span class="sxs-lookup"><span data-stu-id="d8100-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="d8100-182">Por exemplo, se você alterar o nome &quot;Ann&quot; para &quot;um&quot;, quando você enviar o formulário, o seguinte erro é exibido no formulário:</span><span class="sxs-lookup"><span data-stu-id="d8100-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="d8100-183">Neste tutorial, você está tratando o nome de usuário como a chave primária.</span><span class="sxs-lookup"><span data-stu-id="d8100-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="d8100-184">Portanto, a propriedade de nome de usuário não pode ser alterada.</span><span class="sxs-lookup"><span data-stu-id="d8100-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="d8100-185">No *Edit.cshtml* arquivo, logo após o `Html.BeginForm` instrução, defina o nome de usuário para ser um campo oculto.</span><span class="sxs-lookup"><span data-stu-id="d8100-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="d8100-186">Isso faz com que a propriedade a ser passado no modelo.</span><span class="sxs-lookup"><span data-stu-id="d8100-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="d8100-187">O fragmento de código a seguir mostra a colocação do `Hidden` instrução:</span><span class="sxs-lookup"><span data-stu-id="d8100-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="d8100-188">Substitua o `TextBoxFor` e `ValidationMessageFor` marcação para o nome de usuário com um `DisplayFor` chamar.</span><span class="sxs-lookup"><span data-stu-id="d8100-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="d8100-189">O `DisplayFor` método exibe a propriedade como um elemento somente leitura.</span><span class="sxs-lookup"><span data-stu-id="d8100-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="d8100-190">O exemplo a seguir mostra a marcação concluída.</span><span class="sxs-lookup"><span data-stu-id="d8100-190">The following example shows the completed markup.</span></span> <span data-ttu-id="d8100-191">O original `TextBoxFor` e `ValidationMessageFor` chamadas são comentadas com os caracteres de comentário begin e end comentário do Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="d8100-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="d8100-192">Habilitar a validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="d8100-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="d8100-193">Para habilitar a validação do lado do cliente no ASP.NET MVC 3, você deve definir dois sinalizadores e você deve incluir três arquivos JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d8100-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="d8100-194">Abra o aplicativo *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d8100-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="d8100-195">Verifique se `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` são definidas como true nas configurações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d8100-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="d8100-196">O seguinte fragmento de raiz *Web. config* arquivo mostra as configurações corretas:</span><span class="sxs-lookup"><span data-stu-id="d8100-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="d8100-197">Definindo `UnobtrusiveJavaScriptEnabled` como true Ajax discreto habilita e validação de cliente discreto.</span><span class="sxs-lookup"><span data-stu-id="d8100-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="d8100-198">Quando você usar a validação discreta, as regras de validação são transformadas em atributos do HTML5.</span><span class="sxs-lookup"><span data-stu-id="d8100-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="d8100-199">Nomes de atributo do HTML5 podem consistir apenas letras minúsculas, números e traços.</span><span class="sxs-lookup"><span data-stu-id="d8100-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="d8100-200">Definindo `ClientValidationEnabled` para true habilita a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="d8100-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="d8100-201">Definindo essas chaves no aplicativo *Web. config* arquivo, habilitar a validação do cliente e o JavaScript discreto para todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d8100-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="d8100-202">Você também pode habilitar ou desabilitar essas configurações em exibições individuais ou em métodos do controlador usando o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="d8100-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="d8100-203">Você também precisa incluir vários arquivos JavaScript no modo de exibição renderizado.</span><span class="sxs-lookup"><span data-stu-id="d8100-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="d8100-204">É uma maneira fácil de incluir o JavaScript em todas as exibições para adicioná-los para o *exibições \ compartilhadas\\cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d8100-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="d8100-205">Substitua o `<head>` elemento o  *\_cshtml* arquivo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d8100-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="d8100-206">Os primeiros dois scripts de jQuery são hospedados do Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="d8100-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="d8100-207">Aproveitando a CDN do Microsoft Ajax, você pode melhorar significativamente o desempenho de primeira visita de seus aplicativos.</span><span class="sxs-lookup"><span data-stu-id="d8100-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="d8100-208">Execute o aplicativo e clique em um link de edição.</span><span class="sxs-lookup"><span data-stu-id="d8100-208">Run the application and click an edit link.</span></span> <span data-ttu-id="d8100-209">Exiba código-fonte da página no navegador.</span><span class="sxs-lookup"><span data-stu-id="d8100-209">View the page's source in the browser.</span></span> <span data-ttu-id="d8100-210">A origem do navegador mostra muitos atributos do formulário `data-val` (para validação de dados).</span><span class="sxs-lookup"><span data-stu-id="d8100-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="d8100-211">Quando a validação do cliente e o JavaScript discreto está habilitado, os campos de entrada com uma regra de validação do cliente contém o `data-val="true"` atributo para disparar a validação do cliente discreto.</span><span class="sxs-lookup"><span data-stu-id="d8100-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="d8100-212">Por exemplo, o `City` campo no modelo foi decorado com o [necessário](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributo, o que resulta em HTML mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d8100-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="d8100-213">Para cada regra de validação do cliente, um atributo é adicionado, que tem a forma `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="d8100-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="d8100-214">Usando o `City` campo exemplo mostrado anteriormente, a regra de validação de cliente necessário gera o `data-val-required` atributo e a mensagem &quot;cidade o campo é obrigatório&quot;.</span><span class="sxs-lookup"><span data-stu-id="d8100-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="d8100-215">Executar o aplicativo, edite os usuários e limpar o `City` campo.</span><span class="sxs-lookup"><span data-stu-id="d8100-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="d8100-216">Ao pressionar a tecla tab para fora do campo, você vê uma mensagem de erro de validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="d8100-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Cidade necessária](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="d8100-218">Da mesma forma, para cada parâmetro na regra de validação do cliente, um atributo é adicionado que tem o formato `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="d8100-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="d8100-219">Por exemplo, o `FirstName` propriedade está anotada com o [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) de atributo e especifica um comprimento mínimo de 3 e um comprimento máximo de 8.</span><span class="sxs-lookup"><span data-stu-id="d8100-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="d8100-220">A regra de validação de dados denominada `length` tem o nome do parâmetro `max` e o valor do parâmetro 8.</span><span class="sxs-lookup"><span data-stu-id="d8100-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="d8100-221">A seguir mostra o HTML que é gerado para o `FirstName` campo quando você edita um dos usuários:</span><span class="sxs-lookup"><span data-stu-id="d8100-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="d8100-222">Para obter mais informações sobre a validação do cliente discreto, consulte a entrada [discreto validação do cliente no ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) no blog de Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="d8100-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="d8100-223">No ASP.NET MVC 3 Beta, às vezes você precisa enviar o formulário para iniciar a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="d8100-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="d8100-224">Isso pode ser alterado para a versão final.</span><span class="sxs-lookup"><span data-stu-id="d8100-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="d8100-225">Criando o Create View</span><span class="sxs-lookup"><span data-stu-id="d8100-225">Creating the Create View</span></span>

<span data-ttu-id="d8100-226">A próxima etapa é adicionar um `Create` método de ação e o modo de exibição para permitir que o usuário crie um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="d8100-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="d8100-227">Adicione o seguinte `Create` método para o controlador principal:</span><span class="sxs-lookup"><span data-stu-id="d8100-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="d8100-228">Adicionar um modo de exibição como as etapas anteriores, mas definir **exibir conteúdo** para **criar**.</span><span class="sxs-lookup"><span data-stu-id="d8100-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Criar modo de exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="d8100-230">Executar o aplicativo, selecione o **criar** link e adicionar um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="d8100-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="d8100-231">O `Create` método automaticamente tira proveito da validação do lado do cliente e do servidor.</span><span class="sxs-lookup"><span data-stu-id="d8100-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="d8100-232">Tente digitar um nome de usuário que contém espaço em branco, como &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="d8100-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="d8100-233">Quando você guia fora do campo de nome de usuário, um erro de validação do lado do cliente (`White space is not allowed`) é exibido.</span><span class="sxs-lookup"><span data-stu-id="d8100-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="d8100-234">Adicione o método Delete</span><span class="sxs-lookup"><span data-stu-id="d8100-234">Add the Delete method</span></span>

<span data-ttu-id="d8100-235">Para concluir o tutorial, adicione o seguinte `Delete` método para o controlador principal:</span><span class="sxs-lookup"><span data-stu-id="d8100-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="d8100-236">Adicionar um `Delete` exibição como as etapas anteriores, definindo **exibir conteúdo** para **excluir**.</span><span class="sxs-lookup"><span data-stu-id="d8100-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Excluir exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="d8100-238">Agora você tem um aplicativo ASP.NET MVC 3 simple, mas totalmente funcional com a validação.</span><span class="sxs-lookup"><span data-stu-id="d8100-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
