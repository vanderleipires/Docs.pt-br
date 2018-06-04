<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="073cf-101">O método `Index` anterior:</span><span class="sxs-lookup"><span data-stu-id="073cf-101">The previous `Index` method:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="073cf-102">O método `Index` atualizado com o parâmetro `id`:</span><span class="sxs-lookup"><span data-stu-id="073cf-102">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="073cf-103">Agora você pode passar o título de pesquisa como dados de rota (um segmento de URL), em vez de como um valor de cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="073cf-103">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Exibição de índice com a palavra “ghost” adicionada à URL e uma lista de filmes retornados com dois filmes, Ghostbusters e Ghostbusters 2](~/tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="073cf-105">No entanto, você não pode esperar que os usuários modifiquem a URL sempre que desejarem pesquisar um filme.</span><span class="sxs-lookup"><span data-stu-id="073cf-105">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="073cf-106">Agora você adicionará os elementos da interface do usuário para ajudá-los a filtrar filmes.</span><span class="sxs-lookup"><span data-stu-id="073cf-106">So now you'll add UI elements to help them filter movies.</span></span> <span data-ttu-id="073cf-107">Se você tiver alterado a assinatura do método `Index` para testar como passar o parâmetro `ID` associado à rota, altere-o novamente para que ele use um parâmetro chamado `searchString`:</span><span class="sxs-lookup"><span data-stu-id="073cf-107">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

<span data-ttu-id="073cf-108">Abra o arquivo *Views/Movies/Index.cshtml* e, em seguida, adicione a marcação `<form>` realçada abaixo:</span><span class="sxs-lookup"><span data-stu-id="073cf-108">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="073cf-109">A marcação `<form>` HTML usa o [Auxiliar de Marcação de Formulário](xref:mvc/views/working-with-forms). Portanto, quando você enviar o formulário, a cadeia de caracteres de filtro será enviada para a ação `Index` do controlador de filmes.</span><span class="sxs-lookup"><span data-stu-id="073cf-109">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="073cf-110">Salve as alterações e, em seguida, teste o filtro.</span><span class="sxs-lookup"><span data-stu-id="073cf-110">Save your changes and then test the filter.</span></span>

![Exibição de índice com a palavra “ghost” digitada na caixa de texto Filtro de título](~/tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="073cf-112">Não há nenhuma sobrecarga `[HttpPost]` do método `Index` que poderia ser esperada.</span><span class="sxs-lookup"><span data-stu-id="073cf-112">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="073cf-113">Isso não é necessário, porque o método não está alterando o estado do aplicativo, apenas filtrando os dados.</span><span class="sxs-lookup"><span data-stu-id="073cf-113">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="073cf-114">Você poderá adicionar o método `[HttpPost] Index` a seguir.</span><span class="sxs-lookup"><span data-stu-id="073cf-114">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="073cf-115">O parâmetro `notUsed` é usado para criar uma sobrecarga para o método `Index`.</span><span class="sxs-lookup"><span data-stu-id="073cf-115">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="073cf-116">Falaremos sobre isso mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="073cf-116">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="073cf-117">Se você adicionar esse método, o chamador de ação fará uma correspondência com o método `[HttpPost] Index` e o método `[HttpPost] Index` será executado conforme mostrado na imagem abaixo.</span><span class="sxs-lookup"><span data-stu-id="073cf-117">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Janela do navegador com a resposta do aplicativo Do Índice HttpPost: filtrar ghost](~/tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="073cf-119">No entanto, mesmo se você adicionar esta versão `[HttpPost]` do método `Index`, haverá uma limitação na forma de como isso tudo foi implementado.</span><span class="sxs-lookup"><span data-stu-id="073cf-119">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="073cf-120">Imagine que você deseja adicionar uma pesquisa específica como Favoritos ou enviar um link para seus amigos para que eles possam clicar para ver a mesma lista filtrada de filmes.</span><span class="sxs-lookup"><span data-stu-id="073cf-120">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="073cf-121">Observe que a URL da solicitação HTTP POST é a mesma que a URL da solicitação GET (localhost:xxxxx/Movies/Index) – não há nenhuma informação de pesquisa na URL.</span><span class="sxs-lookup"><span data-stu-id="073cf-121">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="073cf-122">As informações de cadeia de caracteres de pesquisa são enviadas ao servidor como um [valor de campo de formulário](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="073cf-122">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="073cf-123">Verifique isso com as ferramentas do Desenvolvedor do navegador ou a excelente [ferramenta Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="073cf-123">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="073cf-124">A imagem abaixo mostra as ferramentas do Desenvolvedor do navegador Chrome:</span><span class="sxs-lookup"><span data-stu-id="073cf-124">The image below shows the Chrome browser Developer tools:</span></span>

![Guia Rede das Ferramentas do Desenvolvedor no Microsoft Edge mostrando o corpo de uma solicitação com o valor de searchString “ghost”](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="073cf-126">Veja o parâmetro de pesquisa e o token [XSRF](xref:security/anti-request-forgery) no corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="073cf-126">You can see the search parameter and [XSRF](xref:security/anti-request-forgery) token in the request body.</span></span> <span data-ttu-id="073cf-127">Observe, conforme mencionado no tutorial anterior, que o [Auxiliar de Marcação de Formulário](xref:mvc/views/working-with-forms) gera um token antifalsificação [XSRF](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="073cf-127">Note, as mentioned in the previous tutorial, the [Form Tag Helper](xref:mvc/views/working-with-forms) generates an [XSRF](xref:security/anti-request-forgery) anti-forgery token.</span></span> <span data-ttu-id="073cf-128">Não modificaremos os dados e, portanto, não precisamos validar o token no método do controlador.</span><span class="sxs-lookup"><span data-stu-id="073cf-128">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="073cf-129">Como o parâmetro de pesquisa está no corpo da solicitação e não na URL, não é possível capturar essas informações de pesquisa para adicionar como Favoritos ou compartilhar com outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="073cf-129">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="073cf-130">Corrigiremos isso especificando que a solicitação deve ser `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="073cf-130">We'll fix this by specifying the request should be `HTTP GET`.</span></span>
