Substitua o conteúdo do arquivo de exibição *Views/HelloWorld/Index.cshtml* do Razor pelo seguinte:

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

Navegue para `http://localhost:xxxx/HelloWorld`. O método `Index` no `HelloWorldController` não fez muita coisa: ele executou a instrução `return View();`, que especificou que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta para o navegador. Como você não especificou explicitamente o nome do arquivo do modelo de exibição, o MVC usou como padrão o arquivo de exibição *Index.cshtml* na pasta */Views/HelloWorld*. A imagem abaixo mostra a cadeia de caracteres “Olá de nosso modelo de exibição!” embutida em código na exibição.

![Janela do navegador](../../tutorials/first-mvc-app/adding-view/_static/hell_template.png)

Se a janela do navegador for pequena (por exemplo, em um dispositivo móvel), talvez você precise alternar (tocar) no [botão de navegação do Bootstrap](http://getbootstrap.com/components/#navbar) no canto superior direito para ver os links **Página Inicial**, **Sobre** e **Contato**.

![Janela do navegador realçando o botão de navegação do Bootstrap](../../tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de layout

Toque os links de menu (**MvcMovie**, **Página Inicial** e **Sobre**). Cada página mostra o mesmo layout de menu. O layout de menu é implementado no arquivo *Views/Shared/_Layout.cshtml*. Abra o arquivo *Views/Shared/_Layout.cshtml*.

Os modelos de [layout](xref:mvc/views/layout) permitem especificar o layout de contêiner HTML do site em um lugar e, em seguida, aplicá-lo a várias páginas do site. Localize a linha `@RenderBody()`. `RenderBody` é um espaço reservado em que todas as páginas específicas à exibição criadas são mostradas, *encapsuladas* na página de layout. Por exemplo, se você selecionar o link **Sobre**, a exibição **Views/Home/About.cshtml** será renderizada dentro do método `RenderBody`.

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a>Alterar o título e o link de menu no arquivo de layout

No elemento de título, altere `MvcMovie` para `Movie App`. Altere o texto de âncora no modelo de layout de `MvcMovie` para `Mvc Movie` e o controlador de `Home` para `Movies`, conforme realçado abaixo:

Observação: a versão do ASP.NET Core 2.0 é ligeiramente diferente. Ela não contém `@inject ApplicationInsights` nem `@Html.Raw(JavaScriptSnippet.FullScript)`.

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

>[!WARNING]
> Ainda não implementamos o controlador `Movies` e, portanto, se você clicar nesse link, obterá um erro 404 (Não encontrado).

Salve as alterações e toque no link **Sobre**. Observe como o título na guia do navegador agora exibe **Sobre – Aplicativo de Filme**, em vez de **Sobre – Filme Mvc**: 

![Sobre a guia](../../tutorials/first-mvc-app/adding-view/_static/about2.png)

Toque no link **Contato** e observe que o texto do título e de âncora também exibem **Aplicativo de Filme**. Conseguimos fazer a alteração uma vez no modelo de layout e fazer com que todas as páginas no site refletissem o novo texto do link e o novo título.

Examine o arquivo *Views/_ViewStart.cshtml*:


```HTML
@{
    Layout = "_Layout";
}
```

O arquivo *Views/_ViewStart.cshtml* mostra o arquivo *Views/Shared/_Layout.cshtml* em cada exibição. Use a propriedade `Layout` para definir outra exibição de layout ou defina-a como `null` para que nenhum arquivo de layout seja usado.

Altere o título da exibição `Index`.

Abra *Views/HelloWorld/Index.cshtml*. Há dois lugares para fazer uma alteração:

   * O texto que é exibido no título do navegador.
   * O cabeçalho secundário (elemento `<h2>`).

Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

`ViewData["Title"] = "Movie List";` no código acima define a propriedade `Title` do dicionário `ViewData` como “Lista de Filmes”. A propriedade `Title` é usada no elemento HTML `<title>` na página de layout:


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Salve as alterações e navegue para `http://localhost:xxxx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.) O título do navegador é criado com `ViewData["Title"]` que definimos no modelo de exibição *Index.cshtml* e o “– Aplicativo de Filme” adicional adicionado no arquivo de layout.

Observe também como o conteúdo no modelo de exibição *Index.cshtml* foi mesclado com o modelo de exibição *Views/Shared/_Layout.cshtml* e uma única resposta HTML foi enviada para o navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo. Para saber mais, consulte [Layout](../../mvc/views/layout.md).

![Exibição de Lista de Filmes](../../tutorials/first-mvc-app/adding-view/_static/hell3.png)

Apesar disso, nossos poucos “dados” (nesse caso, a mensagem “Olá de nosso modelo de exibição!”) são embutidos em código. O aplicativo MVC tem um “V” (exibição) e você tem um “C” (controlador), mas ainda nenhum “M” (modelo).

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

As ações do controlador são invocadas em resposta a uma solicitação de URL de entrada. Uma classe de controlador é o local em que você escreve o código que manipula as solicitações recebidas do navegador. O controlador recupera dados de uma fonte de dados e decide qual tipo de resposta será enviada novamente para o navegador. Modelos de exibição podem ser usados em um controlador para gerar e formatar uma resposta HTML para o navegador.

Os controladores são responsáveis por fornecer os dados necessários para que um modelo de exibição renderize uma resposta. Uma melhor prática: modelos de exibição **não** devem executar a lógica de negócios nem interagir diretamente com um banco de dados. Em vez disso, um modelo de exibição deve funcionar somente com os dados fornecidos pelo controlador. Manter essa “separação de preocupações” ajuda a manter o código limpo, testável e com capacidade de manutenção.

Atualmente, o método `Welcome` na classe `HelloWorldController` usa um parâmetro `name` e um `ID` e, em seguida, gera os valores diretamente no navegador. Em vez de fazer com que o controlador renderize a resposta como uma cadeia de caracteres, vamos alterar o controlador para que ele use um modelo de exibição. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Faça isso fazendo com que o controlador coloque os dados dinâmicos (parâmetros) que o modelo de exibição precisa em um dicionário `ViewData` que pode ser acessado em seguida pelo modelo de exibição.

Retorne ao arquivo *HelloWorldController.cs* e altere o método `Welcome` para adicionar um valor `Message` e `NumTimes` ao dicionário `ViewData`. O dicionário `ViewData` é um objeto dinâmico, o que significa que você pode colocar tudo o que deseja nele; o objeto `ViewData` não tem nenhuma propriedade definida até que você insira algo nele. O sistema de [associação de modelos](xref:mvc/models/model-binding) MVC mapeia automaticamente os parâmetros nomeados (`name` e `numTimes`) da cadeia de consulta na barra de endereços para os parâmetros no método. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

O objeto de dicionário `ViewData` contém dados que serão passados para a exibição. 

Crie um modelo de exibição Boas-vindas chamado *Views/HelloWorld/Welcome.cshtml*.

Você criará um loop no modelo de exibição *Welcome.cshtml* que exibe “Olá” `NumTimes`. Substitua o conteúdo de *Views/HelloWorld/Welcome.cshtml* pelo seguinte:

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Salve as alterações e navegue para a seguinte URL:

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Os dados são obtidos da URL e passados para o controlador usando o [associador de modelo MVC](xref:mvc/models/model-binding). O controlador empacota os dados em um dicionário `ViewData` e passa esse objeto para a exibição. Em seguida, a exibição renderiza os dados como HTML para o navegador.

![Sobre uma exibição que mostra um rótulo Boas-vindas e a frase Olá, Ricardo mostrada quatro vezes](../../tutorials/first-mvc-app/adding-view/_static/rick2.png)

Na amostra acima, usamos o dicionário `ViewData` para passar dados do controlador para uma exibição. Mais adiante no tutorial, usaremos um modelo de exibição para passar dados de um controlador para uma exibição. A abordagem de modelo de exibição para passar dados é geralmente a preferida em relação à abordagem do dicionário `ViewData`. Consulte [ViewModel vs. ViewData vs. ViewBag vs. TempData vs. Session no MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) para obter mais informações.

Bem, isso foi um tipo de “M” de modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.