---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Adicionando uma nova categoria ao DropDownList usando o jQuery UI | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 5d23b1d41834d669bb8006ae0ac963fab60918dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402981"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="86df8-102">Adicionando uma nova categoria ao DropDownList usando o jQuery UI</span><span class="sxs-lookup"><span data-stu-id="86df8-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="86df8-103">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="86df8-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="86df8-104">O HTML `Select` marca é ideal para apresentar uma lista de dados de categoria fixo, mas muitas vezes você precisa adicionar uma nova categoria.</span><span class="sxs-lookup"><span data-stu-id="86df8-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="86df8-105">Suponha que desejamos adicionar o gênero "Opera" para as categorias no nosso banco de dados?</span><span class="sxs-lookup"><span data-stu-id="86df8-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="86df8-106">Nesta seção, usaremos o jQuery UI para adicionar uma caixa de diálogo, que podemos usar para adicionar uma nova categoria.</span><span class="sxs-lookup"><span data-stu-id="86df8-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="86df8-107">A imagem abaixo mostra como a interface do usuário apresentará no navegador.</span><span class="sxs-lookup"><span data-stu-id="86df8-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="86df8-108">Quando um usuário seleciona o **adicionar novo gênero** link, uma caixa de diálogo pop-up solicita ao usuário para um novo nome de gênero (e opcionalmente uma descrição).</span><span class="sxs-lookup"><span data-stu-id="86df8-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="86df8-109">A imagem a seguir mostram os **gênero adicionar** caixa de diálogo pop-up.</span><span class="sxs-lookup"><span data-stu-id="86df8-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="86df8-110">Quando um novo nome de gênero é inserido e o **salvar** botão é pressionado, ocorre o seguinte:</span><span class="sxs-lookup"><span data-stu-id="86df8-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="86df8-111">Uma chamada AJAX publica os dados para o método Create do controlador de gênero, que salva o novo gênero no banco de dados e retorna as novo gênero informações (nome de gênero e ID) como JSON.</span><span class="sxs-lookup"><span data-stu-id="86df8-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="86df8-112">JavaScript adiciona os novos dados de gênero à lista de seleção.</span><span class="sxs-lookup"><span data-stu-id="86df8-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="86df8-113">JavaScript torna o novo gênero o item selecionado.</span><span class="sxs-lookup"><span data-stu-id="86df8-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="86df8-114">Na imagem abaixo, **Opera** foi adicionado ao banco de dados e selecionado na **gênero** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="86df8-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="86df8-115">Abra o *Views\StoreManager\Create.cshtml* de arquivo e substitua a marcação de gênero a seguir o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="86df8-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="86df8-116">O `_ChooseGenre` exibição parcial conterá toda a lógica para ligar o JavaScript e jQuery usado para implementar a funcionalidade de gênero adicionar novo.</span><span class="sxs-lookup"><span data-stu-id="86df8-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="86df8-117">Depois que concluímos o código é simple de fazer o mesmo com o interface do usuário do artista.</span><span class="sxs-lookup"><span data-stu-id="86df8-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="86df8-118">No Gerenciador de soluções, clique com botão direito do *Views\StoreManager* pasta e selecione **Add**, em seguida, **exibição**.</span><span class="sxs-lookup"><span data-stu-id="86df8-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="86df8-119">No **nome da exibição** de entrada, insira `_ChooseGenre` , em seguida, selecione **Add**.</span><span class="sxs-lookup"><span data-stu-id="86df8-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="86df8-120">Substitua a marcação na *Views\StoreManager\\_ChooseGenre.cshtml* arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="86df8-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="86df8-121">A primeira linha declara que estamos transmitindo uma `Album` como nosso modelo, exatamente da mesma instrução encontrado na exibição de criação de modelo.</span><span class="sxs-lookup"><span data-stu-id="86df8-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="86df8-122">As próximas linhas são as **rótulo** marcação do auxiliar.</span><span class="sxs-lookup"><span data-stu-id="86df8-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="86df8-123">A próxima linha é o **DropDownList** chamar do auxiliar, exatamente como no modo de exibição de criação original.</span><span class="sxs-lookup"><span data-stu-id="86df8-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="86df8-124">A próxima linha adiciona um link com o nome `Add New Genre`, e cria o estilo como um botão.</span><span class="sxs-lookup"><span data-stu-id="86df8-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="86df8-125">A linha que contém `ValidationMessageFor` é copiado diretamente do modo de exibição criar.</span><span class="sxs-lookup"><span data-stu-id="86df8-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="86df8-126">As seguintes linhas:</span><span class="sxs-lookup"><span data-stu-id="86df8-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="86df8-127">cria um div oculto, com a ID de `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="86df8-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="86df8-128">Usaremos o jQuery para interligar nosso **Adicionar Gênero** caixa de diálogo com a ID `genreDialog` nessa divisão.</span><span class="sxs-lookup"><span data-stu-id="86df8-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="86df8-129">As últimas duas marcas de script contêm links para os arquivos JavaScript que usaremos para implementar a funcionalidade de gênero adicionar novo.</span><span class="sxs-lookup"><span data-stu-id="86df8-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="86df8-130">O */Scripts/chooseGenre.js* arquivo é fornecido para você no projeto, vamos examiná-la posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="86df8-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="86df8-131">Execute o aplicativo e clique no **adicionar novo gênero** botão.</span><span class="sxs-lookup"><span data-stu-id="86df8-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="86df8-132">No **Adicionar Gênero** caixa de diálogo, digite **Opera** no **nome** caixa de entrada.</span><span class="sxs-lookup"><span data-stu-id="86df8-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="86df8-133">Clique no botão **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="86df8-133">Click the **Save** button.</span></span> <span data-ttu-id="86df8-134">Uma chamada AJAX cria a categoria Opera e, em seguida, preenche a lista suspensa com Opera e define Opera como o gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="86df8-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="86df8-135">Insira um artista, título e o preço e selecione o **criar** botão.</span><span class="sxs-lookup"><span data-stu-id="86df8-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="86df8-136">Se você inserir um preço menor que US $8.99, o novo álbum aparecerá na parte superior da exibição índice.</span><span class="sxs-lookup"><span data-stu-id="86df8-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="86df8-137">Verifique se que a nova entrada de álbum foi salvo no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="86df8-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="86df8-138">Tente criar um novo gênero com apenas uma letra.</span><span class="sxs-lookup"><span data-stu-id="86df8-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="86df8-139">O código a seguir na *Models\Genre.cs* arquivo define o comprimento mínimo e máximo do nome do gênero.</span><span class="sxs-lookup"><span data-stu-id="86df8-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="86df8-140">Validação do lado do cliente relata que você deve inserir uma cadeia de caracteres entre 2 e 20 caracteres.</span><span class="sxs-lookup"><span data-stu-id="86df8-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="86df8-141">Examinar como um novo gênero é adicionado ao banco de dados e a lista Select.</span><span class="sxs-lookup"><span data-stu-id="86df8-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="86df8-142">Abra o *Scripts\chooseGenre.js* de arquivo e examine o código.</span><span class="sxs-lookup"><span data-stu-id="86df8-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="86df8-143">A segunda linha usa a ID `genreDialog` para criar uma caixa de diálogo na marca div na *Views\StoreManager\\_ChooseGenre.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="86df8-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="86df8-144">A maioria dos parâmetros nomeados é auto-explicativa.</span><span class="sxs-lookup"><span data-stu-id="86df8-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="86df8-145">O `autoOpen` parâmetro é definido como false, selecionando os **gênero criar** botão abre a caixa de diálogo explicitamente (Isso é descrito último em).</span><span class="sxs-lookup"><span data-stu-id="86df8-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="86df8-146">A caixa de diálogo tem dois botões, **salve** e **Cancelar**.</span><span class="sxs-lookup"><span data-stu-id="86df8-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="86df8-147">O **Cancelar** botão fecha a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="86df8-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="86df8-148">O código a seguir mostra a **salvar** botão de função.</span><span class="sxs-lookup"><span data-stu-id="86df8-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="86df8-149">O `var createGenreForm` é selecionado do `createGenreForm` ID.</span><span class="sxs-lookup"><span data-stu-id="86df8-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="86df8-150">O `createGenreForm` ID foi definido no código a seguir encontrado na *Views\Genre\\_CreateGenre.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="86df8-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="86df8-151">O [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) sobrecarga auxiliar usada na *Views\Genre\\_CreateGenre.cshtml* arquivo gera HTML com um atributo de ação que contém a URL para enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="86df8-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="86df8-152">Você pode ver isso exibindo a página de criação do álbum em um navegador e selecionando Mostrar fonte no navegador.</span><span class="sxs-lookup"><span data-stu-id="86df8-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="86df8-153">A marcação a seguir mostra o código HTML gerado que contém a marca de formulário.</span><span class="sxs-lookup"><span data-stu-id="86df8-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="86df8-154">O jQuery `$.post` linha faz uma chamada AJAX para o atributo de ação (`/StoreManager/Create`) e passa os dados de **gênero criar** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="86df8-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="86df8-155">Os dados consistem no nome para o novo gênero e uma descrição opcional.</span><span class="sxs-lookup"><span data-stu-id="86df8-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="86df8-156">Se a chamada AJAX for bem-sucedida, o novo nome de gênero e o valor são adicionados para a marcação de seleção, e o novo gênero é definido como o valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="86df8-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="86df8-157">Porque essa é a marcação gerada dinamicamente, você não pode ver a nova opção de selecionar, exibindo o código-fonte no navegador.</span><span class="sxs-lookup"><span data-stu-id="86df8-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="86df8-158">Você pode ver o novo HTML com as ferramentas de desenvolvedor F12 do Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="86df8-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="86df8-159">Para exibir a nova opção de selecionar, no Internet Explorer 9, pressione a tecla F12 para iniciar as ferramentas de desenvolvedor F12.</span><span class="sxs-lookup"><span data-stu-id="86df8-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="86df8-160">Navegue até a página criar e adicionar um novo gênero, portanto o gênero da nova é selecionado na lista de seleção de gênero.</span><span class="sxs-lookup"><span data-stu-id="86df8-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="86df8-161">Nas ferramentas de desenvolvedor F12:</span><span class="sxs-lookup"><span data-stu-id="86df8-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="86df8-162">Selecione a guia HTML.</span><span class="sxs-lookup"><span data-stu-id="86df8-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="86df8-163">Clique no ícone de atualização.</span><span class="sxs-lookup"><span data-stu-id="86df8-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="86df8-164">Na caixa de pesquisa, digite GenreID.</span><span class="sxs-lookup"><span data-stu-id="86df8-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="86df8-165">Usando o ícone próximo</span><span class="sxs-lookup"><span data-stu-id="86df8-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="86df8-166">Navegue até a seguinte marca de seleção:</span><span class="sxs-lookup"><span data-stu-id="86df8-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="86df8-167">Expanda o último valor de opção.</span><span class="sxs-lookup"><span data-stu-id="86df8-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="86df8-168">O código a seguir na *Scripts\chooseGenre.js* arquivo mostra como o **adicionar novo gênero** botão é conectado ao evento de clique e como a **adicionar novo gênero** caixa de diálogo criado.</span><span class="sxs-lookup"><span data-stu-id="86df8-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="86df8-169">A primeira linha cria uma função de clique anexada para o **adicionar novo gênero** botão.</span><span class="sxs-lookup"><span data-stu-id="86df8-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="86df8-170">A seguinte marcação do Views\StoreManager\\_ChooseGenre.cshtml arquivo mostra como o **adicionar novo gênero** botão é criado:</span><span class="sxs-lookup"><span data-stu-id="86df8-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="86df8-171">O método load cria e abre a caixa de diálogo Adicionar gênero e chama o jQuery `parse` , de modo a validação do cliente ocorre em dados inseridos na caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="86df8-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="86df8-172">Nesta seção, você aprendeu como criar uma caixa de diálogo que pode ser usada para adicionar novos dados de categoria a uma lista de seleção.</span><span class="sxs-lookup"><span data-stu-id="86df8-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="86df8-173">Você pode seguir o mesmo procedimento para criar a interface do usuário para adicionar um novo artista à lista de seleção artista.</span><span class="sxs-lookup"><span data-stu-id="86df8-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="86df8-174">Este tutorial tenha fornecido uma visão geral de como trabalhar com o auxiliar HTML do ASP.NET MVC **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="86df8-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="86df8-175">Para obter mais informações sobre como trabalhar com o **DropDownList**, consulte a seção de referências de adição abaixo.</span><span class="sxs-lookup"><span data-stu-id="86df8-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="86df8-176">Informe-nos se este tutorial tenha sido útil.</span><span class="sxs-lookup"><span data-stu-id="86df8-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="86df8-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="86df8-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="86df8-178">Referências adicionais</span><span class="sxs-lookup"><span data-stu-id="86df8-178">Additional References</span></span>

- <span data-ttu-id="86df8-179">[Tutorial de lista suspensa – em cascata MVC do ASP.NET](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) por [visão de Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="86df8-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="86df8-180">[Escolhido](http://harvesthq.github.com/chosen/) JavaScript de um plug-in que dão suporte à filtragem e seleção múltipla.</span><span class="sxs-lookup"><span data-stu-id="86df8-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="86df8-181">Colaboradores</span><span class="sxs-lookup"><span data-stu-id="86df8-181">Contributors</span></span>

- [<span data-ttu-id="86df8-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="86df8-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="86df8-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="86df8-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="86df8-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="86df8-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="86df8-185">Revisores</span><span class="sxs-lookup"><span data-stu-id="86df8-185">Reviewers</span></span>

- <span data-ttu-id="86df8-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="86df8-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="86df8-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="86df8-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="86df8-188">Mike Papa</span><span class="sxs-lookup"><span data-stu-id="86df8-188">Mike Pope</span></span>
- <span data-ttu-id="86df8-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="86df8-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="86df8-190">Anterior</span><span class="sxs-lookup"><span data-stu-id="86df8-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
