
## <a name="test-the-app"></a>Testar o aplicativo

* Execute o aplicativo e toque no link **Filme Mvc**.
* Toque no link **Criar Novo** e crie um filme.

  ![Criar exibição com campos para título, gênero, preço e data de lançamento](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* Talvez você não possa inserir pontos decimais ou vírgulas no campo `Price`. Para dar suporte à [validação jQuery](https://jqueryvalidation.org/) para localidades de idiomas diferentes do inglês que usam uma vírgula (",") para ponto decimal e formatos de data diferentes do inglês dos EUA, você deve tomar medidas para globalizar seu aplicativo. Consulte https://github.com/aspnet/Docs/issues/4076 e [Recursos adicionais](#additional-resources) para obter mais informações. Por enquanto, digite apenas números inteiros como 10.

<a name="displayformatdatelocal"></a>

* Em algumas localidades, você precisa especificar o formato da data. Consulte o código realçado abaixo.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Falaremos sobre `DataAnnotations` posteriormente no tutorial.

Tocar em **Criar** faz com que o formulário seja enviado para o servidor, no qual as informações do filme são salvas em um banco de dados. O aplicativo redireciona para a URL */Movies*, em que as informações do filme recém-criadas são exibidas.

![Exibição de filme mostrando a listagem de filmes recém-criados](../../tutorials/first-mvc-app/adding-model/_static/h.png)

Crie duas mais entradas de filme adicionais. Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.
