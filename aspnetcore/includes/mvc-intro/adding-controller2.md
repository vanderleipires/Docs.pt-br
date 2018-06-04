Substitua o conteúdo de *Controllers/HelloWorldController.cs* pelo seguinte:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Cada método `public` em um controlador pode ser chamado como um ponto de extremidade HTTP. Na amostra acima, ambos os métodos retornam uma cadeia de caracteres.  Observe os comentários que precedem cada método.

Um ponto de extremidade HTTP é uma URL direcionável no aplicativo Web, como `http://localhost:1234/HelloWorld`, e combina o protocolo usado `HTTP`, o local de rede do servidor Web (incluindo a porta TCP) `localhost:1234` e o URI de destino `HelloWorld`.

O primeiro comentário indica que este é um método [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) invocado por meio do acréscimo de “/HelloWorld/” à URL base. O segundo comentário especifica um método [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) invocado por meio do acréscimo de “/HelloWorld/Welcome/” à URL. Mais adiante no tutorial, você usará o mecanismo de scaffolding para gerar métodos `HTTP POST`.

Execute o aplicativo no modo sem depuração e acrescente “HelloWorld” ao caminho na barra de endereços. O método `Index` retorna uma cadeia de caracteres.

![Janela do navegador mostrando a resposta do aplicativo Esta é minha ação padrão](~/tutorials/first-mvc-app/adding-controller/_static/hell1.png)

O MVC invoca as classes do controlador (e os métodos de ação dentro delas), dependendo da URL de entrada. A [lógica de roteamento de URL](xref:mvc/controllers/routing) padrão usada pelo MVC usa um formato como este para determinar o código a ser invocado:

`/[Controller]/[ActionName]/[Parameters]`

Configure o formato de roteamento no método `Configure` do arquivo *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Quando você executa o aplicativo e não fornece nenhum segmento de URL, ele usa como padrão o controlador “Home” e o método “Index” especificado na linha do modelo realçada acima.

O primeiro segmento de URL determina a classe do controlador a ser executada. Portanto, `localhost:xxxx/HelloWorld` é mapeado para a classe `HelloWorldController`. A segunda parte do segmento de URL determina o método de ação na classe. Portanto, `localhost:xxxx/HelloWorld/Index` fará com que o método `Index` da classe `HelloWorldController` seja executado. Observe que você precisou apenas navegar para `localhost:xxxx/HelloWorld` e o método `Index` foi chamado por padrão. Isso ocorre porque `Index` é o método padrão que será chamado em um controlador se um nome de método não for especificado explicitamente. A terceira parte do segmento de URL (`id`) refere-se aos dados de rota. Você verá os dados de rota mais adiante neste tutorial.

Navegue para `http://localhost:xxxx/HelloWorld/Welcome`. O método `Welcome` é executado e retorna a cadeia de caracteres “Este é o método de ação Boas-vindas...”. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![Janela do navegador mostrando a resposta do aplicativo Este é o método de ação Boas-vindas](~/tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Modifique o código para passar algumas informações de parâmetro da URL para o controlador. Por exemplo, `/HelloWorld/Welcome?name=Rick&numtimes=4`. Altere o método `Welcome` para incluir dois parâmetros, conforme mostrado no código a seguir. 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

O código anterior:

* Usa o recurso de parâmetro opcional do C# para indicar que o parâmetro `numTimes` usa 1 como padrão se nenhum valor é passado para esse parâmetro.
* Usa `HtmlEncoder.Default.Encode` para proteger o aplicativo contra a entrada mal-intencionada (ou seja, JavaScript). 
* Usa [Cadeias de caracteres interpoladas](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Execute o aplicativo e navegue para:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Substitua xxxx pelo número da porta.) Você pode tentar valores diferentes para `name` e `numtimes` na URL. O sistema de [associação de modelos](xref:mvc/models/model-binding) MVC mapeia automaticamente os parâmetros nomeados da cadeia de consulta na barra de endereços para os parâmetros no método. Consulte [Associação de modelos](xref:mvc/models/model-binding) para obter mais informações.

![Janela do navegador mostrando a resposta do aplicativo Olá, Ricardo, NumTimes é: 4](~/tutorials/first-mvc-app/adding-controller/_static/rick4.png)

Na imagem acima, o segmento de URL (`Parameters`) não é usado e os parâmetros `name` e `numTimes` são transmitidos como [cadeias de consulta](https://wikipedia.org/wiki/Query_string). O `?` (ponto de interrogação) na URL acima é um separador seguido pelas cadeias de consulta. O caractere `&` separa as cadeias de consulta.

Substitua o método `Welcome` pelo seguinte código:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Execute o aplicativo e insira a seguinte URL: `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Janela do navegador mostrando a resposta do aplicativo Olá, Ricardo, ID: 3](~/tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

Agora, o terceiro segmento de URL corresponde ao parâmetro de rota `id`. O método `Welcome` contém um parâmetro `id` que correspondeu ao modelo de URL no método `MapRoute`. O `?` à direita (em `id?`) indica que o parâmetro `id` é opcional.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Nestes exemplos, o controlador faz a parte “VC” do MVC – ou seja, o trabalho da exibição e do controlador. O controlador retorna o HTML diretamente. Em geral, você não deseja que os controladores retornem HTML diretamente, pois isso é muito difícil de codificar e manter. Em vez disso, normalmente, você usa um arquivo de modelo de exibição do Razor separado para ajudar a gerar a resposta HTML. Faça isso no próximo tutorial.
