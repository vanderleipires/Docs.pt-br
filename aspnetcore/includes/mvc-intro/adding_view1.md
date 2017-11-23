# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>Adicionando uma exibição a um aplicativo ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Nesta seção, você modifica a classe `HelloWorldController` para que ela use os arquivos de modelo de exibição do Razor para encapsular corretamente o processo de geração de respostas HTML para um cliente.

Crie um arquivo de modelo de exibição usando o Razor. Os modelos de exibição baseados no Razor têm uma extensão de arquivo *.cshtml*. Eles fornecem uma maneira elegante de criar a saída HTML usando o C#.

Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Na classe `HelloWorldController`, substitua o método `Index` pelo seguinte código:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

O código anterior retorna um objeto `View`. Ele usa um modelo de exibição para gerar uma resposta HTML para o navegador. Métodos do controlador (também conhecidos como métodos de ação), como o método `Index` acima, geralmente retornam um [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) (ou uma classe derivada de `ActionResult`), não um tipo como cadeia de caracteres.
