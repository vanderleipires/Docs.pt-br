<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

O método `Index` anterior:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

O método `Index` atualizado com o parâmetro `id`:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Agora você pode passar o título de pesquisa como dados de rota (um segmento de URL), em vez de como um valor de cadeia de consulta.

![Exibição de índice com a palavra “ghost” adicionada à URL e uma lista de filmes retornados com dois filmes, Ghostbusters e Ghostbusters 2](../../tutorials/first-mvc-app/search/_static/g2.png)

No entanto, você não pode esperar que os usuários modifiquem a URL sempre que desejarem pesquisar um filme. Portanto, agora você adicionará uma interface do usuário para ajudá-los a filtrar os filmes. Se você tiver alterado a assinatura do método `Index` para testar como passar o parâmetro `ID` associado à rota, altere-o novamente para que ele use um parâmetro chamado `searchString`:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Abra o arquivo *Views/Movies/Index.cshtml* e, em seguida, adicione a marcação `<form>` realçada abaixo:

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

A marcação `<form>` HTML usa o [Auxiliar de Marcação de Formulário](../../mvc/views/working-with-forms.md). Portanto, quando você enviar o formulário, a cadeia de caracteres de filtro será enviada para a ação `Index` do controlador de filmes. Salve as alterações e, em seguida, teste o filtro.

![Exibição de índice com a palavra “ghost” digitada na caixa de texto Filtro de título](../../tutorials/first-mvc-app/search/_static/filter.png)

Não há nenhuma sobrecarga `[HttpPost]` do método `Index` que poderia ser esperada. Isso não é necessário, porque o método não está alterando o estado do aplicativo, apenas filtrando os dados.

Você poderá adicionar o método `[HttpPost] Index` a seguir.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

O parâmetro `notUsed` é usado para criar uma sobrecarga para o método `Index`. Falaremos sobre isso mais adiante no tutorial.

Se você adicionar esse método, o chamador de ação fará uma correspondência com o método `[HttpPost] Index` e o método `[HttpPost] Index` será executado conforme mostrado na imagem abaixo.

![Janela do navegador com a resposta do aplicativo Do Índice HttpPost: filtrar ghost](../../tutorials/first-mvc-app/search/_static/fo.png)

No entanto, mesmo se você adicionar esta versão `[HttpPost]` do método `Index`, haverá uma limitação na forma de como isso tudo foi implementado. Imagine que você deseja adicionar uma pesquisa específica como Favoritos ou enviar um link para seus amigos para que eles possam clicar para ver a mesma lista filtrada de filmes. Observe que a URL da solicitação HTTP POST é a mesma que a URL da solicitação GET (localhost:xxxxx/Movies/Index) – não há nenhuma informação de pesquisa na URL. As informações de cadeia de caracteres de pesquisa são enviadas ao servidor como um [valor de campo de formulário](https://developer.mozilla.org/docs/Web/Guide/HTML/Forms/Sending_and_retrieving_form_data). Verifique isso com as ferramentas do Desenvolvedor do navegador ou a excelente [ferramenta Fiddler](http://www.telerik.com/fiddler). A imagem abaixo mostra as ferramentas do Desenvolvedor do navegador Chrome:

![Guia Rede das Ferramentas do Desenvolvedor no Microsoft Edge mostrando o corpo de uma solicitação com o valor de searchString “ghost”](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

Veja o parâmetro de pesquisa e o token [XSRF](../../security/anti-request-forgery.md) no corpo da solicitação. Observe, conforme mencionado no tutorial anterior, que o [Auxiliar de Marcação de Formulário](../../mvc/views/working-with-forms.md) gera um token antifalsificação [XSRF](../../security/anti-request-forgery.md). Não modificaremos os dados e, portanto, não precisamos validar o token no método do controlador.

Como o parâmetro de pesquisa está no corpo da solicitação e não na URL, não é possível capturar essas informações de pesquisa para adicionar como Favoritos ou compartilhar com outras pessoas. Corrigiremos isso especificando que a solicitação deve ser `HTTP GET`.