---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "Introdução a páginas da Web ASP.NET - excluir dados do banco de dados | Microsoft Docs"
author: tfitzmac
description: "Este tutorial mostra como excluir uma entrada de banco de dados individuais. Ele pressupõe que você tenha concluído a série por meio de atualização de banco de dados no PA da Web do ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 5bc92b5d40e7a55dcd730d552c71031d913b277e
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="1e1cd-104">Introdução a páginas da Web ASP.NET - excluir dados do banco de dados</span><span class="sxs-lookup"><span data-stu-id="1e1cd-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="1e1cd-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1e1cd-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1e1cd-106">Este tutorial mostra como excluir uma entrada de banco de dados individuais.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="1e1cd-107">Ele pressupõe que você tenha concluído a série por meio de [atualização do banco de dados em páginas da Web do ASP.NET](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="1e1cd-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="1e1cd-108">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1e1cd-109">Como selecionar um registro individual de uma lista de registros.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="1e1cd-110">Como excluir um único registro de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="1e1cd-111">Como verificar se um botão específico foi clicado em um formulário.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="1e1cd-112">Recursos/tecnologias abordadas:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="1e1cd-113">O `WebGrid` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="1e1cd-114">O SQL `Delete` comando.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="1e1cd-115">O `Database.Execute` método para executar um SQL `Delete` comando.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="1e1cd-116">O que você vai criar</span><span class="sxs-lookup"><span data-stu-id="1e1cd-116">What You'll Build</span></span>

<span data-ttu-id="1e1cd-117">No tutorial anterior, você aprendeu como atualizar um registro de banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="1e1cd-118">Este tutorial é semelhante, exceto que, em vez de atualizar o registro, você poderá excluí-lo.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="1e1cd-119">Os processos são de modo semelhante, exceto que a exclusão é mais simples, para que seja curto neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="1e1cd-120">No *filmes* página, você poderá atualizar o `WebGrid` auxiliar para que ele exibe um **excluir** link ao lado de cada filme para acompanhar o **editar** link adicionado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Página de filmes mostrando um link de exclusão para cada filme](deleting-data/_static/image1.png)

<span data-ttu-id="1e1cd-122">Com edição, quando você clica o **excluir** link, ele leva a uma página diferente, onde as informações de filme já estão em um formulário:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Excluir a página de filme com um filme exibido](deleting-data/_static/image2.png)

<span data-ttu-id="1e1cd-124">Você pode, em seguida, clique no botão para excluí-lo permanentemente.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="1e1cd-125">Adicionar um Link de exclusão para a listagem de filme</span><span class="sxs-lookup"><span data-stu-id="1e1cd-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="1e1cd-126">Comece adicionando um **excluir** link para o `WebGrid` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="1e1cd-127">Este link é semelhante do **editar** link adicionado em um tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="1e1cd-128">Abra o *Movies.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="1e1cd-129">Alteração de `WebGrid` marcação no corpo da página, adicionando uma coluna.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="1e1cd-130">Aqui está a marcação modificada:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="1e1cd-131">A nova coluna é este:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="1e1cd-132">A maneira como a grade é configurada, o **editar** coluna é mais à esquerda na grade e **excluir** coluna é mais à direita.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="1e1cd-133">(Há uma vírgula após o `Year` coluna agora, no caso de você nem notou que.) Não há nada especial sobre onde ir essas colunas de link, e você pode colocá-los facilmente próximos um do outro.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="1e1cd-134">Nesse caso, eles são separados para torná-los mais difícil obter misturados.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Página de filmes com links de edição e detalhes marcada para mostrar que não estão próximos um do outro](deleting-data/_static/image3.png)

<span data-ttu-id="1e1cd-136">A nova coluna mostra um link (`<a>` elemento) cujo texto é "Excluir".</span><span class="sxs-lookup"><span data-stu-id="1e1cd-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="1e1cd-137">O destino do link (seu `href` atributo) é o código que, por fim, resolve para algo como essa URL com o `id` valor diferente para cada filme:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="1e1cd-138">Este link invocará uma página chamada *DeleteMovie* e passe a ID do filme que você selecionou.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="1e1cd-139">Este tutorial não entrarei em detalhes sobre como esse link é construído, porque ele é quase idêntico de **editar** link do tutorial anterior ([atualização do banco de dados em páginas da Web do ASP.NET](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="1e1cd-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="1e1cd-140">Criando a página de exclusão</span><span class="sxs-lookup"><span data-stu-id="1e1cd-140">Creating the Delete Page</span></span>

<span data-ttu-id="1e1cd-141">Agora você pode criar a página que será o destino para o **excluir** link na grade.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="1e1cd-142">**Importante** a técnica de primeiro selecionar um registro para excluir e, em seguida, usando uma página separada e o botão para confirmar o processo é extremamente importante para a segurança.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="1e1cd-143">Conforme você leu nos tutoriais anteriores, tornando *qualquer* deve do tipo de alteração para seu site *sempre* ser feito usando um formulário &mdash; ou seja, usando uma operação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="1e1cd-144">Se você fez possível alterar o site bastando clicar em um link (ou seja, usando uma operação GET), as pessoas podem fazer solicitações simples para seu site e excluir seus dados.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="1e1cd-145">Até mesmo um rastreador de mecanismo de pesquisa que é a indexação de seu site pode inadvertidamente excluir dados apenas links a seguir.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="1e1cd-146">Quando seu aplicativo permite que pessoas alterar um registro, você precisa apresentar o registro para o usuário para edição mesmo assim.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="1e1cd-147">Mas você poderá ficar tentado para ignorar esta etapa para exclusão de um registro.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="1e1cd-148">Não ignore essa etapa, embora.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-148">Don't skip that step, though.</span></span> <span data-ttu-id="1e1cd-149">(Também é útil para os usuários vejam o registro e confirmar que ele está excluindo o registro que eles se destinam.)</span><span class="sxs-lookup"><span data-stu-id="1e1cd-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="1e1cd-150">Um tutorial subsequente, você verá como adicionar a funcionalidade de logon para que um usuário precisa entrar antes de excluir um registro.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="1e1cd-151">Crie uma página chamada *DeleteMovie.cshtml* e substitua o que está no arquivo com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="1e1cd-152">Essa marcação é como o *EditMovie* páginas, exceto que, em vez de usar as caixas de texto (`<input type="text">`), a marcação inclui `<span>` elementos.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="1e1cd-153">Não há nada aqui para editar.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-153">There's nothing here to edit.</span></span> <span data-ttu-id="1e1cd-154">Tudo o que você precisa fazer é exibir os detalhes do filme, para que os usuários podem Certifique-se de que eles está excluindo o filme à direita.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="1e1cd-155">A marcação já contém um link que permite que o usuário retornar para a página de listagem do filme.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="1e1cd-156">Como no *EditMovie* página, a ID do filme selecionado é armazenada em um campo oculto.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="1e1cd-157">(Ele é passado para a página da primeira vez como um valor de cadeia de caracteres de consulta.) Há um `Html.ValidationSummary` chamada com erros de validação.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="1e1cd-158">Nesse caso, o erro pode ser que nenhuma ID de filme foi passado para a página ou que a ID de filme é inválida.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="1e1cd-159">Essa situação pode ocorrer se alguém tiver executado essa página sem primeiro selecionar um filme no *filmes* página.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="1e1cd-160">A legenda do botão é **excluir filme**, e seu nome de atributo é definido como `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="1e1cd-161">O `name` atributo será usado no código para identificar o botão que enviou o formulário.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="1e1cd-162">Você precisará escrever código para 1) Leia os detalhes do filme quando a página é exibida pela primeira vez e 2) realmente excluir o filme quando o usuário clica no botão.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="1e1cd-163">Adicionando código para ler um filme único</span><span class="sxs-lookup"><span data-stu-id="1e1cd-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="1e1cd-164">Na parte superior do *DeleteMovie.cshtml* página, adicione o seguinte bloco de código:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="1e1cd-165">Essa marcação é o mesmo que o código correspondente no *EditMovie* página.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="1e1cd-166">Ele obtém a ID de filme fora a cadeia de caracteres de consulta e usa a ID para ler um registro do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="1e1cd-167">O código inclui o teste de validação (`IsInt()` e `row != null`) para certificar-se de que a ID de filme que está sendo passada para a página é válida.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="1e1cd-168">Lembre-se de que esse código deve apenas ser executado na primeira vez em que a página é executada.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="1e1cd-169">Você não deseja ler novamente o registro de filme do banco de dados quando o usuário clica o **excluir filme** botão.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="1e1cd-170">Portanto, de código para ler o filme está dentro de um teste que diz `if(!IsPost)` &mdash; ou seja, *se a solicitação não é uma operação post (envio)*.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="1e1cd-171">Adicionando código para excluir o filme selecionado</span><span class="sxs-lookup"><span data-stu-id="1e1cd-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="1e1cd-172">Para excluir o filme quando o usuário clica no botão, adicione o seguinte código dentro da chave de fechamento do `@` bloco:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="1e1cd-173">Esse código é semelhante ao código para atualizar um registro existente, mas é mais simples.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="1e1cd-174">Basicamente, o código executa um SQL `Delete` instrução.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="1e1cd-175">Como no *EditMovie* página, o código está em um `if(IsPost)` bloco.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="1e1cd-176">Neste momento, o `if()` condição é um pouco mais complicada:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="1e1cd-177">Há duas condições aqui.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-177">There are two conditions here.</span></span> <span data-ttu-id="1e1cd-178">A primeira é que a página está sendo enviada, como você viu antes &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="1e1cd-179">A segunda condição for `!Request["buttonDelete"].IsEmpty()`, que significa que a solicitação tem um objeto chamado `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="1e1cd-180">Sem dúvida, é uma maneira indireta de teste que botão enviado o formulário.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="1e1cd-181">Se um formulário contém vários botões de envio, somente o nome do botão que foi clicado aparece na solicitação.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="1e1cd-182">Portanto, logicamente, se o nome de um determinado botão aparece na solicitação &mdash; ou conforme indicado no código, se esse botão não estiver vazio &mdash; que é o botão que enviou o formulário.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="1e1cd-183">O `&&` operador significa "e" (AND lógico).</span><span class="sxs-lookup"><span data-stu-id="1e1cd-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="1e1cd-184">Portanto, toda a `if` condição for...</span><span class="sxs-lookup"><span data-stu-id="1e1cd-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="1e1cd-185">*Essa solicitação é post (não é uma solicitação pela primeira vez)*</span><span class="sxs-lookup"><span data-stu-id="1e1cd-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="1e1cd-186">AND</span><span class="sxs-lookup"><span data-stu-id="1e1cd-186">AND</span></span>  
  
<span data-ttu-id="1e1cd-187">*O* `buttonDelete` *botão foi o botão que enviou o formulário.*</span><span class="sxs-lookup"><span data-stu-id="1e1cd-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="1e1cd-188">Este formulário (na verdade, essa página) contém apenas um botão, portanto o teste adicional para `buttonDelete` tecnicamente não é necessária.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="1e1cd-189">Ainda assim, você está prestes a executar uma operação que irá remover permanentemente os dados.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="1e1cd-190">Portanto, você queira Certifique-se como possíveis que você estiver executando a operação somente quando o usuário tiver solicitado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="1e1cd-191">Por exemplo, suponha que você expandido esta página mais tarde e adicionadas outros botões.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="1e1cd-192">Mesmo assim, o código que exclui o filme será executado somente se o `buttonDelete` botão foi clicado.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="1e1cd-193">Como no *EditMovie* página, obter a ID do campo oculto e, em seguida, execute o comando SQL.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="1e1cd-194">A sintaxe para a `Delete` instrução é:</span><span class="sxs-lookup"><span data-stu-id="1e1cd-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="1e1cd-195">É essencial para incluir o `WHERE` cláusula e a ID.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="1e1cd-196">Se você omitir a cláusula WHERE, *todos os registros na tabela serão excluídos*.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="1e1cd-197">Como você viu, você passar o valor de ID para o comando SQL usando um espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="1e1cd-198">Teste o processo de exclusão de filme</span><span class="sxs-lookup"><span data-stu-id="1e1cd-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="1e1cd-199">Agora você pode testar.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-199">Now you can test.</span></span> <span data-ttu-id="1e1cd-200">Execute o *filmes* página e, em seguida, clique em **excluir** ao lado de um filme.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="1e1cd-201">Quando o *DeleteMovie* página aparece, clique em **excluir filme**.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Excluir a página de filme com o botão Excluir filme realçado](deleting-data/_static/image4.png)

<span data-ttu-id="1e1cd-203">Quando você clica no botão, o código exclui filmes e retorna para a listagem do filme.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="1e1cd-204">Lá você pode procurar o filme excluído e confirme se ele foi excluído.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="1e1cd-205">Próximo</span><span class="sxs-lookup"><span data-stu-id="1e1cd-205">Coming Up Next</span></span>

<span data-ttu-id="1e1cd-206">O seguinte tutorial mostra como fornecer todas as páginas em seu site uma aparência e layout comuns.</span><span class="sxs-lookup"><span data-stu-id="1e1cd-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="1e1cd-207">Listagem completa para a página de filme (atualizada com Links de exclusão)</span><span class="sxs-lookup"><span data-stu-id="1e1cd-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="1e1cd-208">Lista completa de página DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="1e1cd-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="1e1cd-209">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="1e1cd-209">Additional Resources</span></span>

- [<span data-ttu-id="1e1cd-210">Introdução à programação da Web do ASP.NET usando a sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="1e1cd-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="1e1cd-211">[A instrução DELETE do SQL](http://www.w3schools.com/sql/sql_delete.asp) no site W3Schools</span><span class="sxs-lookup"><span data-stu-id="1e1cd-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1e1cd-212">[Anterior](updating-data.md)
[Próximo](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="1e1cd-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
