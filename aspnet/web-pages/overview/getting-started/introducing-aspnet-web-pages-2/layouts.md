---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Introdução ao ASP.NET Web Pages - criando um Layout consistente | Microsoft Docs
author: tfitzmac
description: Este tutorial mostra como usar layouts para criar uma aparência consistente para as páginas em um site que usa as páginas da Web ASP.NET. Ele pressupõe que você tenha concluído a...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 5641b65ab1053ccc039a94f7a591185ff00ff1c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806539"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="17f8f-104">Introdução ao ASP.NET Web Pages - criando um Layout consistente</span><span class="sxs-lookup"><span data-stu-id="17f8f-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="17f8f-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="17f8f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="17f8f-106">Este tutorial mostra como usar *layouts* para criar uma aparência consistente para as páginas em um site que usa as páginas da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="17f8f-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="17f8f-107">Ele pressupõe que você tenha concluído a série por meio [excluir do banco de dados em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="17f8f-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="17f8f-108">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="17f8f-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="17f8f-109">É o que uma página de layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-109">What a layout page is.</span></span>
> - <span data-ttu-id="17f8f-110">Como combinar as páginas de layout com conteúdo dinâmico.</span><span class="sxs-lookup"><span data-stu-id="17f8f-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="17f8f-111">Como passar valores para uma página de layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="17f8f-112">Sobre Layouts</span><span class="sxs-lookup"><span data-stu-id="17f8f-112">About Layouts</span></span>

<span data-ttu-id="17f8f-113">As páginas que você criou até agora todas forem concluídas, páginas autônomas.</span><span class="sxs-lookup"><span data-stu-id="17f8f-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="17f8f-114">Todos eles pertencem ao mesmo site, mas eles não têm uma aparência padrão ou todos os elementos comuns.</span><span class="sxs-lookup"><span data-stu-id="17f8f-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="17f8f-115">A maioria dos sites têm uma aparência consistente e o layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="17f8f-116">Por exemplo, se você for para o [Microsoft.com/web](https://www.microsoft.com/web/) do site e olhar em volta, você verá que as páginas de todas as aderem a um layout geral e um tema visual:</span><span class="sxs-lookup"><span data-stu-id="17f8f-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Página do site Microsoft.com/Web que mostra o layout do cabeçalho, área de navegação, área de conteúdo e do rodapé](layouts/_static/image1.png)

<span data-ttu-id="17f8f-118">Uma *ineficiente* maneira para criar esse layout seria definir um cabeçalho, barra de navegação e rodapé separadamente em cada uma das suas páginas.</span><span class="sxs-lookup"><span data-stu-id="17f8f-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="17f8f-119">Você poderia ser duplicando a mesma marcação cada vez.</span><span class="sxs-lookup"><span data-stu-id="17f8f-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="17f8f-120">Se você quiser alterar algo (por exemplo, atualize o rodapé), você precisaria alterar cada página separadamente.</span><span class="sxs-lookup"><span data-stu-id="17f8f-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="17f8f-121">É aí que estão *páginas de layout* entram.</span><span class="sxs-lookup"><span data-stu-id="17f8f-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="17f8f-122">Em páginas de Web do ASP.NET, você pode definir uma página de layout que fornece um contêiner geral para páginas em seu site.</span><span class="sxs-lookup"><span data-stu-id="17f8f-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="17f8f-123">Por exemplo, a página de layout pode conter o cabeçalho, área de navegação e rodapé.</span><span class="sxs-lookup"><span data-stu-id="17f8f-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="17f8f-124">A página de layout inclui um espaço reservado para onde vai o conteúdo principal.</span><span class="sxs-lookup"><span data-stu-id="17f8f-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="17f8f-125">Em seguida, você pode definir páginas de conteúdo individuais que contêm a marcação e o código para essa página somente.</span><span class="sxs-lookup"><span data-stu-id="17f8f-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="17f8f-126">Páginas de conteúdo não precisam ser completar páginas HTML; eles nem precisam ter um `<body>` elemento.</span><span class="sxs-lookup"><span data-stu-id="17f8f-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="17f8f-127">Eles também têm uma linha de código que diz ao ASP.NET qual página de layout que você deseja exibir o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="17f8f-128">Aqui está uma figura que mostra mais ou menos como funciona essa relação:</span><span class="sxs-lookup"><span data-stu-id="17f8f-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagrama conceitual que mostra duas páginas de conteúdo e uma página de layout no qual eles se ajustam](layouts/_static/image2.png)

<span data-ttu-id="17f8f-130">Essa interação é fácil de entender quando você vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="17f8f-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="17f8f-131">Neste tutorial, você alterará as páginas de filmes para usar um layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="17f8f-132">Adicionando uma página de Layout</span><span class="sxs-lookup"><span data-stu-id="17f8f-132">Adding a Layout Page</span></span>

<span data-ttu-id="17f8f-133">Você começará criando uma página de layout que define um layout de página típico com um cabeçalho, rodapé e uma área para o conteúdo principal.</span><span class="sxs-lookup"><span data-stu-id="17f8f-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="17f8f-134">No site WebPagesMovies, adicionar uma página CSHTML denominada  *\_layout. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="17f8f-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="17f8f-135">O sublinhado à esquerda ( `_` ) caractere é significativo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="17f8f-136">Se o nome de uma página começa com um sublinhado, o ASP.NET não enviará diretamente nessa página para o navegador.</span><span class="sxs-lookup"><span data-stu-id="17f8f-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="17f8f-137">Essa convenção permite definir as páginas que são necessárias para seu site, mas que os usuários não devem ser capazes de solicitar diretamente.</span><span class="sxs-lookup"><span data-stu-id="17f8f-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="17f8f-138">Substitua o conteúdo da página com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="17f8f-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="17f8f-139">Como você pode ver, essa marcação é apenas o HTML que usa `<div>` elementos para definir três seções na página, além do ano mais `<div>` elemento para conter as três seções.</span><span class="sxs-lookup"><span data-stu-id="17f8f-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="17f8f-140">O rodapé contém um pouco de código Razor: `@DateTime.Now.Year`, que será renderizado o ano atual nesse local na página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="17f8f-141">Observe que há um link para uma folha de estilo nomeada *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="17f8f-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="17f8f-142">A folha de estilos é onde os detalhes do layout físico dos elementos serão definidos.</span><span class="sxs-lookup"><span data-stu-id="17f8f-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="17f8f-143">Você vai criar que em breve.</span><span class="sxs-lookup"><span data-stu-id="17f8f-143">You'll create that in a moment.</span></span>

<span data-ttu-id="17f8f-144">O recurso somente incomum desta  *\_layout. cshtml* página é o `@Render.Body()` linha.</span><span class="sxs-lookup"><span data-stu-id="17f8f-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="17f8f-145">Esse é o espaço reservado aonde o conteúdo quando este layout é mesclado com outra página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="17f8f-146">Adicionando um arquivo. CSS</span><span class="sxs-lookup"><span data-stu-id="17f8f-146">Adding a .css File</span></span>

<span data-ttu-id="17f8f-147">A maneira preferencial para definir a organização real (ou seja, aparência) dos elementos na página é usar regras de (CSS) de folha de estilos em cascata.</span><span class="sxs-lookup"><span data-stu-id="17f8f-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="17f8f-148">Portanto, você criará um *. CSS* arquivo com as regras para seu novo layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="17f8f-149">No WebMatrix, selecione a raiz do seu site.</span><span class="sxs-lookup"><span data-stu-id="17f8f-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="17f8f-150">Em seguida, no **arquivos** guia de faixa de opções, clique na seta sob o **New** botão e, em seguida, clique em **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="17f8f-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![A opção 'Nova pasta' em novo na faixa de opções.](layouts/_static/image3.png)

<span data-ttu-id="17f8f-152">Nomeie a nova pasta *estilos*.</span><span class="sxs-lookup"><span data-stu-id="17f8f-152">Name the new folder *Styles*.</span></span>

![Nomear a nova pasta 'Estilos'](layouts/_static/image4.png)

<span data-ttu-id="17f8f-154">Dentro do novo *estilos* pasta, crie um arquivo chamado *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="17f8f-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Criando um novo arquivo Movies.css](layouts/_static/image5.png)

<span data-ttu-id="17f8f-156">Substitua o conteúdo do novo *. CSS* arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="17f8f-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="17f8f-157">Não falaremos muito sobre essas regras CSS, exceto a Observe duas coisas.</span><span class="sxs-lookup"><span data-stu-id="17f8f-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="17f8f-158">Uma é que além de definir fontes e tamanhos, as regras de usam posicionamento absoluto para estabelecer o local do cabeçalho, rodapé e área de conteúdo principal.</span><span class="sxs-lookup"><span data-stu-id="17f8f-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="17f8f-159">Se você for novo no posicionamento no CSS, você pode ler o [posicionamento CSS](http://www.w3schools.com/css/css_positioning.asp) tutorial no site W3Schools.</span><span class="sxs-lookup"><span data-stu-id="17f8f-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="17f8f-160">A outra coisa a observar é que, na parte inferior, podemos ter copiado as regras de estilo que foram originalmente definida individualmente na *Movies.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="17f8f-161">Essas regras foram usadas na [Introdução aos dados exibindo por usando páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para tornar o `WebGrid` auxiliar renderizar a marcação que adicionou faixas à tabela.</span><span class="sxs-lookup"><span data-stu-id="17f8f-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="17f8f-162">(Se você pretende usar um *. CSS* arquivo para definições de estilo, você também pode colocar as regras de estilo para o site inteiro nele.)</span><span class="sxs-lookup"><span data-stu-id="17f8f-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="17f8f-163">Atualizando o arquivo de filmes para usar o Layout</span><span class="sxs-lookup"><span data-stu-id="17f8f-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="17f8f-164">Agora você pode atualizar os arquivos existentes no seu site para usar o novo layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="17f8f-165">Abra o *Movies.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="17f8f-166">Na parte superior, como a primeira linha de código, adicione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="17f8f-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="17f8f-167">Agora começa a página dessa forma:</span><span class="sxs-lookup"><span data-stu-id="17f8f-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="17f8f-168">Esta linha de código diz ao ASP.NET que, quando o *filmes* página é executada, ele deve ser mesclado com o  *\_layout. cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="17f8f-169">Uma vez que o *Movies.cshtml* arquivo agora usa uma página de layout, você pode remover a marcação da *Movies.cshtml* página já foi solucionado pelo  *\_layout. cshtml*arquivo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="17f8f-170">Tire o `<!DOCTYPE>`, `<html>`, e `<body>` de abertura e fechamento de marcas.</span><span class="sxs-lookup"><span data-stu-id="17f8f-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="17f8f-171">Remova todo o `<head>` elemento e seu conteúdo, que inclui as regras de estilo para a grade, uma vez que você já tem essas regras um *. CSS* arquivo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="17f8f-172">Enquanto você faz isso, alterar existente `<h1>` elemento para um `<h2>` elemento; você tem um `<h1>` elemento na página de layout já.</span><span class="sxs-lookup"><span data-stu-id="17f8f-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="17f8f-173">Alterar o `<h2>` texto para "Lista de filmes".</span><span class="sxs-lookup"><span data-stu-id="17f8f-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="17f8f-174">Normalmente você não teria que fazer esses tipos de alterações em uma página de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="17f8f-175">Quando você começar seu site com uma página de layout, criar páginas de conteúdo sem todos esses elementos para começar.</span><span class="sxs-lookup"><span data-stu-id="17f8f-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="17f8f-176">Nesse caso, no entanto, você está convertendo uma página autônoma para uma que usa um layout, portanto, não há um pouco de limpeza.</span><span class="sxs-lookup"><span data-stu-id="17f8f-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="17f8f-177">Quando tiver terminado, o *Movies.cshtml* página ficará semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="17f8f-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="17f8f-178">Teste o Layout</span><span class="sxs-lookup"><span data-stu-id="17f8f-178">Testing the Layout</span></span>

<span data-ttu-id="17f8f-179">Agora você pode ver a aparência do layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="17f8f-180">No WebMatrix, clique com botão direito do *Movies.cshtml* página e selecione **iniciar no navegador**.</span><span class="sxs-lookup"><span data-stu-id="17f8f-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="17f8f-181">Quando o navegador exibe a página, ele se parece com esta página:</span><span class="sxs-lookup"><span data-stu-id="17f8f-181">When the browser displays the page, it looks like this page:</span></span>

![Página de filmes renderizada usando um layout](layouts/_static/image6.png)

<span data-ttu-id="17f8f-183">ASP.NET mesclou o conteúdo da página Movies.cshtml para o  *\_layout. cshtml* página direita, onde o `RenderBody` método é.</span><span class="sxs-lookup"><span data-stu-id="17f8f-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="17f8f-184">E claro o  *\_layout. cshtml* página referências uma *. CSS* arquivo que define a aparência da página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="17f8f-185">Atualizando a página de AddMovie para usar o Layout</span><span class="sxs-lookup"><span data-stu-id="17f8f-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="17f8f-186">O verdadeiro benefício de layouts é que você pode usá-los para todas as páginas em seu site.</span><span class="sxs-lookup"><span data-stu-id="17f8f-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="17f8f-187">Abra o *AddMovie.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="17f8f-188">Você pode se lembrar de que o *AddMovie.cshtml* página originalmente tinha algumas regras CSS nele para definir a aparência das mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="17f8f-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="17f8f-189">Como você tem um *. CSS* arquivo do seu site agora, você pode mover essas regras para o *. CSS* arquivo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="17f8f-190">Removê-los da *AddMovie.cshtml* do arquivo e adicioná-los na parte inferior do *Movies.css* arquivo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="17f8f-191">Você está movendo as regras a seguir:</span><span class="sxs-lookup"><span data-stu-id="17f8f-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="17f8f-192">Agora, verifique os mesmos tipos de alterações no *AddMovie.cshtml* que você fez para *Movies.cshtml* — adicione `Layout="~/_Layout.cshtml;` e remova a marcação HTML que agora é irrelevante.</span><span class="sxs-lookup"><span data-stu-id="17f8f-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="17f8f-193">Alterar o `<h1>` elemento para `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="17f8f-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="17f8f-194">Quando terminar, a página se parecerá com este exemplo:</span><span class="sxs-lookup"><span data-stu-id="17f8f-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="17f8f-195">Execute a página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-195">Run the page.</span></span> <span data-ttu-id="17f8f-196">Agora, ele se parece com esta ilustração:</span><span class="sxs-lookup"><span data-stu-id="17f8f-196">Now it looks like this illustration:</span></span>

![Renderizado usando um layout de página 'Adicionar filmes'](layouts/_static/image7.png)

<span data-ttu-id="17f8f-198">Você deseja fazer alterações semelhantes para as páginas no site — *EditMovie.cshtml* e *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="17f8f-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="17f8f-199">No entanto, antes de fazer, você pode fazer outra alteração no layout que torna um pouco mais flexível.</span><span class="sxs-lookup"><span data-stu-id="17f8f-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="17f8f-200">Passando informações de título para a página de Layout</span><span class="sxs-lookup"><span data-stu-id="17f8f-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="17f8f-201">O  *\_layout. cshtml* página que você criou tem um `<title>` elemento que é definido como "Meu Site de filme".</span><span class="sxs-lookup"><span data-stu-id="17f8f-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="17f8f-202">A maioria dos navegadores exibir o conteúdo desse elemento como o texto em uma guia:</span><span class="sxs-lookup"><span data-stu-id="17f8f-202">Most browsers display the content of this element as the text on a tab:</span></span>

![A página &lt;título&gt; elemento exibido em uma guia do navegador](layouts/_static/image8.png)

<span data-ttu-id="17f8f-204">Essas informações de título são genéricas.</span><span class="sxs-lookup"><span data-stu-id="17f8f-204">This title information is generic.</span></span> <span data-ttu-id="17f8f-205">Suponha que você deseja que o texto do título para ser mais específico para a página atual.</span><span class="sxs-lookup"><span data-stu-id="17f8f-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="17f8f-206">(O texto do título também é usado pelos mecanismos de pesquisa para determinar qual é sua página sobre.) Você pode passar informações de uma página de conteúdo, como *Movies.cshtml* ou *AddMovie.cshtml* para o layout da página e, em seguida, use essas informações para personalizar a página de layout processa.</span><span class="sxs-lookup"><span data-stu-id="17f8f-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="17f8f-207">Abra o *Movies.cshtml* página novamente.</span><span class="sxs-lookup"><span data-stu-id="17f8f-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="17f8f-208">No código na parte superior, adicione a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="17f8f-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="17f8f-209">O `Page` objeto está disponível em todos os *. cshtml* páginas e é para essa finalidade, ou seja, para compartilhar informações entre uma página e seu layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="17f8f-210">Abra o<em>\_layout. cshtml</em> página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="17f8f-211">Alterar o `<title>` , de modo que ele se parece com essa marcação:</span><span class="sxs-lookup"><span data-stu-id="17f8f-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="17f8f-212">Esse código renderiza tudo o que está no `Page.Title` propriedade diretamente nesse local na página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="17f8f-213">Execute o *Movies.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="17f8f-214">Desta vez, a guia do navegador mostra o que é passado como o valor do `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="17f8f-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Uma guia do navegador mostrando o título criado dinamicamente](layouts/_static/image9.png)

<span data-ttu-id="17f8f-216">Se desejar, exiba a origem da página no navegador.</span><span class="sxs-lookup"><span data-stu-id="17f8f-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="17f8f-217">Você pode ver que o `<title>` elemento é renderizado como `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="17f8f-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="17f8f-218">**O objeto de página**</span><span class="sxs-lookup"><span data-stu-id="17f8f-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="17f8f-219">Um recurso útil do `Page` é que ele é um objeto dinâmico — o `Title` propriedade não é um nome reservado ou fixo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="17f8f-220">Você pode usar *qualquer* nome do valor de `Page` objeto.</span><span class="sxs-lookup"><span data-stu-id="17f8f-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="17f8f-221">Por exemplo, você poderia facilmente passar o título usando uma propriedade chamada `Page.CurrentName` ou `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="17f8f-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="17f8f-222">A única restrição é que o nome deve seguir as regras normais de quais propriedades podem ser nomeadas.</span><span class="sxs-lookup"><span data-stu-id="17f8f-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="17f8f-223">(Por exemplo, o nome não pode conter um espaço.)</span><span class="sxs-lookup"><span data-stu-id="17f8f-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="17f8f-224">Você pode passar qualquer número de valores usando o `Page` objeto.</span><span class="sxs-lookup"><span data-stu-id="17f8f-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="17f8f-225">Se você quiser passar informações de filmes para a página de layout, você pode passar valores usando algo como `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="17f8f-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="17f8f-226">(Ou outros nomes inventada para armazenar as informações.) O único requisito — que é provavelmente óbvia — é que você precisa usar os mesmos nomes na página de conteúdo e a página de layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="17f8f-227">As informações transmitidas por meio de `Page` objeto não está limitado a apenas texto a ser exibido na página de layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="17f8f-228">Você pode passar um valor para a página de layout e, em seguida, o código na página de layout pode usar o valor para decidir se deve exibir uma seção da página, o que *. CSS* de arquivos para usar e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="17f8f-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="17f8f-229">Os valores que você passe a `Page` objeto são como quaisquer outros valores que você usar no código.</span><span class="sxs-lookup"><span data-stu-id="17f8f-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="17f8f-230">É assim que os valores são originadas na página de conteúdo e são passados para a página de layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="17f8f-231">Abra o *AddMovie.cshtml* da página e adicione uma linha na parte superior do código que fornece um título para o *AddMovie.cshtml* página:</span><span class="sxs-lookup"><span data-stu-id="17f8f-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="17f8f-232">Execute o *AddMovie.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="17f8f-233">Você verá o novo título lá:</span><span class="sxs-lookup"><span data-stu-id="17f8f-233">You see the new title there:</span></span>

![Uma guia do navegador mostrando o título 'Adicionar filmes' criado dinamicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="17f8f-235">Atualizando as páginas restantes para usar o Layout</span><span class="sxs-lookup"><span data-stu-id="17f8f-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="17f8f-236">Agora você pode concluir as páginas restantes no seu site para que eles usem o novo layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="17f8f-237">Abra *EditMovie.cshtml* e *DeleteMovie.cshtml* por sua vez e fazer as mesmas alterações em cada um.</span><span class="sxs-lookup"><span data-stu-id="17f8f-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="17f8f-238">Adicione a linha de código que vincula a página de layout:</span><span class="sxs-lookup"><span data-stu-id="17f8f-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="17f8f-239">Adicione uma linha para definir o título da página:</span><span class="sxs-lookup"><span data-stu-id="17f8f-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="17f8f-240">ou:</span><span class="sxs-lookup"><span data-stu-id="17f8f-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="17f8f-241">Remover toda a marcação HTML estranhas — basicamente, deixar apenas os bits que estão dentro do `<body>` elemento (mais o bloco de código na parte superior).</span><span class="sxs-lookup"><span data-stu-id="17f8f-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="17f8f-242">Alterar o `<h1>` elemento seja um `<h2>` elemento.</span><span class="sxs-lookup"><span data-stu-id="17f8f-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="17f8f-243">Quando você fez essas alterações, teste cada e certifique-se de que ela está exibindo corretamente e se o título está correto.</span><span class="sxs-lookup"><span data-stu-id="17f8f-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="17f8f-244">Pensamentos para encerrar sobre páginas de Layout</span><span class="sxs-lookup"><span data-stu-id="17f8f-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="17f8f-245">Neste tutorial, você criou uma  *\_layout. cshtml* página e usado o `RenderBody` método para mesclar o conteúdo de outra página.</span><span class="sxs-lookup"><span data-stu-id="17f8f-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="17f8f-246">Que é o padrão básico para usar layouts em páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="17f8f-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="17f8f-247">Páginas de layout têm recursos adicionais que não abordaremos aqui.</span><span class="sxs-lookup"><span data-stu-id="17f8f-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="17f8f-248">Por exemplo, você pode aninhar as páginas de layout — uma página de layout por sua vez pode referenciar outro.</span><span class="sxs-lookup"><span data-stu-id="17f8f-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="17f8f-249">Layouts aninhados podem ser útil se você estiver trabalhando com subseções de um site que exigem diferentes layouts.</span><span class="sxs-lookup"><span data-stu-id="17f8f-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="17f8f-250">Você também pode usar métodos adicionais (por exemplo, `RenderSection`) para configurar a chamada seções na página de layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="17f8f-251">A combinação de páginas de layout e *. CSS* arquivos é poderoso.</span><span class="sxs-lookup"><span data-stu-id="17f8f-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="17f8f-252">Como você verá na próxima série de tutoriais, no WebMatrix, você pode criar um site com base em um *modelo*, que oferece um site que possui funcionalidade predefinidas nele.</span><span class="sxs-lookup"><span data-stu-id="17f8f-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="17f8f-253">Os modelos faça bom uso de CSS para criar sites que combinam bem e que têm recursos como menus e páginas de layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="17f8f-254">Aqui está uma captura de tela da home page de um site com base em um modelo, mostrando os recursos que usam páginas de layout e CSS:</span><span class="sxs-lookup"><span data-stu-id="17f8f-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Layout criado pelo modelo de site do WebMatrix mostrando o cabeçalho, área de navegação, área de conteúdo, seção opcional e links de logon](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="17f8f-256">Listagem completa para a página de filme (atualizada para usar uma página de Layout)</span><span class="sxs-lookup"><span data-stu-id="17f8f-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="17f8f-257">Página concluída listagem para adicionar a página de filme (atualizada para o Layout)</span><span class="sxs-lookup"><span data-stu-id="17f8f-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="17f8f-258">Listagem completa da página para página de exclusão de filme (atualizada para o Layout)</span><span class="sxs-lookup"><span data-stu-id="17f8f-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="17f8f-259">Listagem de página completa para a página de edição do filme (atualizada para o Layout)</span><span class="sxs-lookup"><span data-stu-id="17f8f-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="17f8f-260">Próximo</span><span class="sxs-lookup"><span data-stu-id="17f8f-260">Coming Up Next</span></span>

<span data-ttu-id="17f8f-261">O próximo tutorial, você aprenderá como publicar seu site na Internet para que qualquer pessoa pode vê-lo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17f8f-262">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="17f8f-262">Additional Resources</span></span>

- <span data-ttu-id="17f8f-263">[Criar uma aparência consistente](https://go.microsoft.com/fwlink/?LinkID=202891) — um artigo que fornece mais detalhes sobre como trabalhar com layouts.</span><span class="sxs-lookup"><span data-stu-id="17f8f-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="17f8f-264">Ele também descreve como passar um valor para uma página de layout que mostra ou oculta a parte do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="17f8f-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="17f8f-265">[Páginas de Layout com o Razor aninhadas](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — blogs de Mike Brind um exemplo de como aninhar páginas de layout.</span><span class="sxs-lookup"><span data-stu-id="17f8f-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="17f8f-266">(Inclui um download das páginas).</span><span class="sxs-lookup"><span data-stu-id="17f8f-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="17f8f-267">[Anterior](deleting-data.md)
> [Próximo](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="17f8f-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
