---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examinando como o ASP.NET MVC faz Scaffold do auxiliar DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 542790b7f475cc641ed26ff3187c25c25118e0ed
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577841"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="5fa1c-102">Examinando como o ASP.NET MVC faz Scaffold do auxiliar DropDownList</span><span class="sxs-lookup"><span data-stu-id="5fa1c-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="5fa1c-103">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="5fa1c-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="5fa1c-104">Na **Gerenciador de soluções**, clique com botão direito do *controladores* pasta e, em seguida, selecione **Adicionar controlador**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="5fa1c-105">Nomeie o controlador **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="5fa1c-106">Defina as opções para o **Adicionar controlador** caixa de diálogo, conforme mostrado na imagem abaixo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="5fa1c-107">Editar o *StoreManager\Index.cshtml* exibir e remover `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="5fa1c-108">Removendo `AlbumArtUrl` tornar a apresentação mais legível.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="5fa1c-109">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="5fa1c-110">Abra o *Controllers\StoreManagerController.cs* do arquivo e encontre o `Index` método.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="5fa1c-111">Adicionar o `OrderBy` cláusula para que os álbuns serão classificados por preço.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="5fa1c-112">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="5fa1c-113">Classificar por preço tornará mais fácil de testar as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="5fa1c-114">Quando você estiver testando o editar e cria métodos, você pode usar um baixo preço, para que os dados salvos aparecerão primeiros.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="5fa1c-115">Abra o *StoreManager\Edit.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="5fa1c-116">Adicione a linha a seguir logo após a marca de legenda.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="5fa1c-117">O código a seguir mostra o contexto dessa alteração:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="5fa1c-118">O `AlbumId` é necessária para fazer alterações em um registro de álbum.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="5fa1c-119">Pressione CTRL+F5 para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="5fa1c-120">Selecione esta opção para o **Admin** vincular e, em seguida, selecione o **criar novo** link para criar um novo álbum.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="5fa1c-121">Verifique se que as informações de álbum foi salvo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-121">Verify the album information was saved.</span></span> <span data-ttu-id="5fa1c-122">Editar um álbum e verifique se as alterações feitas serão mantidas.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="5fa1c-123">O esquema do álbum</span><span class="sxs-lookup"><span data-stu-id="5fa1c-123">The Album Schema</span></span>

<span data-ttu-id="5fa1c-124">O `StoreManager` controlador criado pelo mecanismo de scaffolding MVC permite o acesso CRUD (Create, Read, Update, Delete) aos álbuns no banco de dados de loja de música.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="5fa1c-125">O esquema para obter informações de álbum é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="5fa1c-126">O `Albums` tabela não armazena o gênero de álbum e a descrição, ele armazena uma chave estrangeira para a `Genres` tabela.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="5fa1c-127">O `Genres` tabela contém o nome de gênero e uma descrição.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="5fa1c-128">Da mesma forma, o `Albums` a tabela não contém o nome do álbum artistas, mas uma chave estrangeira para a `Artists` tabela.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="5fa1c-129">O `Artists` tabela contém o nome do artista.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="5fa1c-130">Se você examinar os dados a `Albums` tabela, você pode ver cada linha contém uma chave estrangeira para a `Genres` tabela e uma chave estrangeira para a `Artists` tabela.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="5fa1c-131">A imagem a seguir mostram alguns dados da tabela de `Albums` tabela.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="5fa1c-132">A marca de seleção HTML</span><span class="sxs-lookup"><span data-stu-id="5fa1c-132">The HTML Select Tag</span></span>

<span data-ttu-id="5fa1c-133">O HTML `<select>` elemento (criado pelo HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar) é usado para exibir uma lista completa de valores (como a lista de gêneros).</span><span class="sxs-lookup"><span data-stu-id="5fa1c-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="5fa1c-134">Para formulários de edição, quando o valor atual é conhecido, a lista de seleção pode exibir o valor atual.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="5fa1c-135">Vimos anteriormente nesse quando definimos o valor selecionado **comédia**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="5fa1c-136">A lista de seleção é ideal para a exibição de categoria ou dados de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="5fa1c-137">O `<select>` elemento para a chave estrangeira do gênero exibe a lista de nomes de gênero possíveis, mas quando você salvar o formulário a propriedade de gênero é atualizada com o gênero valor de chave estrangeira, não o nome exibido do gênero.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="5fa1c-138">Na imagem abaixo, é o gênero selecionado **Disco** e é o artista **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="5fa1c-139">Código gerado automaticamente examinar o ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5fa1c-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="5fa1c-140">Abra o *Controllers\StoreManagerController.cs* do arquivo e encontre o `HTTP GET Create` método.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="5fa1c-141">O `Create` método adiciona duas [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objetos para o `ViewBag`, uma para conter as informações de gênero e outra para conter a informações sobre o artista.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="5fa1c-142">O [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) sobrecarga do construtor usada acima usa três argumentos:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="5fa1c-143">*itens*: uma [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contém os itens na lista.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="5fa1c-144">No exemplo acima, a lista de gêneros é retornado por `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="5fa1c-145">*dataValueField*: O nome da propriedade na **IEnumerable** lista que contém o valor da chave.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="5fa1c-146">No exemplo acima, `GenreId` e `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="5fa1c-147">*dataTextField*: O nome da propriedade na **IEnumerable** lista que contém as informações a serem exibidas.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="5fa1c-148">No artistas e tabela de gênero, o `name` campo é usado.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="5fa1c-149">Abra o *Views\StoreManager\Create.cshtml* do arquivo e examine o `Html.DropDownList` marcação do auxiliar para o campo gênero.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="5fa1c-150">A primeira linha mostra que o modo de exibição criar leva um `Album` modelo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="5fa1c-151">No `Create` método mostrado acima, nenhum modelo foi passado, portanto, a exibição obtém um **nulo** `Album` modelo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="5fa1c-152">Agora que estamos criando um novo álbum portanto, não há nenhum `Album` dados para ele.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="5fa1c-153">O [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) sobrecarga mostrada acima usa o nome do campo a ser associado ao modelo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="5fa1c-154">Ele também usa esse nome para procurar uma **ViewBag** objeto que contém uma [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objeto.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="5fa1c-155">Usando essa sobrecarga, é necessário nome de **ViewBag SelectList** objeto `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="5fa1c-156">O segundo parâmetro (`String.Empty`) é o texto a ser exibido quando nenhum item está selecionado.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="5fa1c-157">Isso é exatamente o que queremos ao criar um novo álbum.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="5fa1c-158">Se você remover o segundo parâmetro e usado o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="5fa1c-159">A lista de seleção padrão para o primeiro elemento ou Rock em nosso exemplo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="5fa1c-160">Examinando o `HTTP POST Create` método.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="5fa1c-161">Essa sobrecarga da `Create` leva um `album` objeto, criado pelo sistema de associação de modelo ASP.NET MVC dos valores de formulário postados.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="5fa1c-162">Quando você envia um novo álbum, se o estado do modelo é válido e não há nenhum erro de banco de dados, o novo álbum é adicionado o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="5fa1c-163">A imagem a seguir mostra a criação de um novo álbum.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="5fa1c-164">Você pode usar o [ferramenta fiddler](http://www.fiddler2.com/fiddler2/) para examinar os valores de formulário postados essa associação de modelos do ASP.NET MVC usa para criar o objeto de álbum.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="5fa1c-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="5fa1c-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="5fa1c-166">A criação de ViewBag SelectList de refatoração</span><span class="sxs-lookup"><span data-stu-id="5fa1c-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="5fa1c-167">Os dois o `Edit` métodos e o `HTTP POST Create` método ter código idêntico para configurar a **SelectList** no **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="5fa1c-168">No espírito do [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), podemos será refatorar esse código.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="5fa1c-169">Vamos fazer uso disso refatorado código mais tarde.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="5fa1c-170">Criar um novo método para adicionar um gênero e artista **SelectList** para o **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="5fa1c-171">Substitua as duas linhas definindo a `ViewBag` em cada uma da `Create` e `Edit` métodos com uma chamada para o `SetGenreArtistViewBag` método.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="5fa1c-172">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="5fa1c-173">Criar um novo álbum e editar um álbum para verificar que as alterações funcionem.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="5fa1c-174">Passar explicitamente o SelectList para DropDownList</span><span class="sxs-lookup"><span data-stu-id="5fa1c-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="5fa1c-175">Os modos de exibição criar e editar criado pelo uso de scaffolding do ASP.NET MVC o seguinte **DropDownList** sobrecarga:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="5fa1c-176">O `DropDownList` marcação da exibição create é mostrada abaixo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="5fa1c-177">Porque o `ViewBag` propriedade para o `SelectList` é denominado `GenreId`, o **DropDownList** auxiliar usará o `GenreId` **SelectList** no **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="5fa1c-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="5fa1c-178">A seguir **DropDownList** sobrecarga, o `SelectList` é passado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="5fa1c-179">Abra o *Views\StoreManager\Edit.cshtml* de arquivo e altere o **DropDownList** chamada transmitir explicitamente o **SelectList**, usando a sobrecarga acima.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="5fa1c-180">Faça isso para a categoria de gênero.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-180">Do this for the Genre category.</span></span> <span data-ttu-id="5fa1c-181">O código completo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="5fa1c-182">Execute o aplicativo e clique no **Admin** vincular, em seguida, navegue até um álbum de Jazz e selecione o **editar** link.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="5fa1c-183">Em vez de Mostrar Jazz como o gênero selecionado no momento, Rock é exibida.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="5fa1c-184">Quando o argumento de cadeia de caracteres (a propriedade para associar) e o **SelectList** objeto têm o mesmo nome, o valor selecionado não é usado.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="5fa1c-185">Quando não houver nenhum valor selecionado fornecido, os navegadores padrão para o primeiro elemento na **SelectList**(que é **Rock** no exemplo acima).</span><span class="sxs-lookup"><span data-stu-id="5fa1c-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="5fa1c-186">Essa é uma limitação conhecida do **DropDownList** auxiliar.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="5fa1c-187">Abra o *Controllers\StoreManagerController.cs* do arquivo e altere o **SelectList** objeto nomes a serem `Genres` e `Artists`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="5fa1c-188">O código completo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="5fa1c-189">Os nomes dos gêneros e artistas são nomes melhores para as categorias, pois eles contêm mais do que apenas a ID de cada categoria.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="5fa1c-190">A refatoração que fizemos anteriormente valeu.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="5fa1c-191">Em vez de alterar o **ViewBag** em quatro métodos, nossas alterações sejam isoladas o `SetGenreArtistViewBag` método.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="5fa1c-192">Alterar o **DropDownList** chamar a criar e editar modos de exibição para usar a nova **SelectList** nomes.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="5fa1c-193">A nova marcação para a exibição de edição é mostrada abaixo:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="5fa1c-194">Criar exibição requer uma cadeia de caracteres vazia para impedir que o primeiro item no SelectList que está sendo exibida.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="5fa1c-195">Criar um novo álbum e editar um álbum para verificar que as alterações funcionem.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="5fa1c-196">Teste o código de edição, selecionando um álbum com um gênero diferente de Rock.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="5fa1c-197">Usando um modelo de exibição com o auxiliar do DropDownList</span><span class="sxs-lookup"><span data-stu-id="5fa1c-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="5fa1c-198">Crie uma nova classe na pasta ViewModels chamada `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="5fa1c-199">Substitua o código no `AlbumSelectListViewModel` classe com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="5fa1c-200">O `AlbumSelectListViewModel` construtor usa um álbum, uma lista dos artistas e gêneros e cria um objeto que contém o álbum e um `SelectList` para gêneros e artistas.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="5fa1c-201">Compile o projeto para que o `AlbumSelectListViewModel` está disponível quando criamos uma exibição na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="5fa1c-202">Adicionar um `EditVM` método para o `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="5fa1c-203">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="5fa1c-204">Clique com botão direito `AlbumSelectListViewModel`, selecione **resolver**, em seguida, **usando MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="5fa1c-205">Como alternativa, você pode adicionar a seguinte instrução using:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="5fa1c-206">Clique com botão direito `EditVM` e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="5fa1c-207">Use as opções mostradas abaixo.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="5fa1c-208">Selecione **Add**, em seguida, substitua o conteúdo do *Views\StoreManager\EditVM.cshtml* arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5fa1c-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="5fa1c-209">O `EditVM` marcação é muito semelhante ao original `Edit` marcação com as seguintes exceções.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="5fa1c-210">Propriedades de modelo a `Edit` modo de exibição estão no formato `model.property`(por exemplo, `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="5fa1c-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="5fa1c-211">Propriedades de modelo a `EditVm` modo de exibição estão no formato `model.Album.property`(por exemplo, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="5fa1c-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="5fa1c-212">Isso ocorre porque o `EditVM` exibição é passada a um contêiner um `Album`, e não um `Album` como no `Edit` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="5fa1c-213">O **DropDownList** segundo parâmetro vem do modelo de exibição, não a **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="5fa1c-214">O **BeginForm** auxiliar na `EditVM` modo de exibição explicitamente envia de volta para o `Edit` método de ação.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="5fa1c-215">Ao postar de volta para o `Edit` ação, não temos de escrever um `HTTP POST EditVM` ação e pode reutilizar as `HTTP POST` `Edit` ação.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="5fa1c-216">Execute o aplicativo e editar um álbum.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-216">Run the application and edit an album.</span></span> <span data-ttu-id="5fa1c-217">Altere a URL para usar `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="5fa1c-218">Alterar um campo e pressione a **salvar** botão para verificar o código está funcionando.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="5fa1c-219">A abordagem que você deve usar?</span><span class="sxs-lookup"><span data-stu-id="5fa1c-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="5fa1c-220">Todas as três abordagens mostradas são acceptible.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="5fa1c-221">Muitos desenvolvedores preferem explictily pass a `SelectList` para o `DropDownList` usando o `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="5fa1c-222">Essa abordagem tem a vantagem adicional de oferecer a você a flexibilidade de usar um nome mais apropriado para a coleção.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="5fa1c-223">Uma limitação é que você não pode nomear o `ViewBag SelectList` o mesmo nome que a propriedade do modelo de objeto.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="5fa1c-224">Alguns desenvolvedores preferem a abordagem do ViewModel.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="5fa1c-225">Outros considere a marcação mais detalhada e gerado HTML da abordagem ViewModel uma desvantagem.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="5fa1c-226">Nesta seção aprendemos três abordagens para usar o **DropDownList** com os dados de categoria.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="5fa1c-227">Na próxima seção, mostraremos como adicionar uma nova categoria.</span><span class="sxs-lookup"><span data-stu-id="5fa1c-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5fa1c-228">[Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Próximo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="5fa1c-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
