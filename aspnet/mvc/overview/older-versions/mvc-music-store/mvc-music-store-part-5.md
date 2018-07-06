---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: Formulários de edição e modelagem | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 5 aborda os formulários de edição e modelagem.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: f799c4d492e88f3edcf3800e66e0a1bae3845ba2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395195"
---
<a name="part-5-edit-forms-and-templating"></a>Parte 5: Formulários de edição e modelagem
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 5 aborda os formulários de edição e modelagem.


No capítulo anterior, estávamos Carregando dados de nosso banco de dados e exibi-la. Neste capítulo, também será habilitar edição de dados.

## <a name="creating-the-storemanagercontroller"></a>Criando o StoreManagerController

Vamos começar criando um novo controlador chamado **StoreManagerController**. Para este controlador, Extrairemos proveito dos recursos disponíveis na atualização das ferramentas do ASP.NET MVC 3 Scaffolding. Defina as opções da caixa de diálogo Adicionar controlador, conforme mostrado abaixo.

![](mvc-music-store-part-5/_static/image1.png)

Quando você clica no botão Adicionar, você verá que o mecanismo de scaffolding do ASP.NET MVC 3 faz uma boa quantidade de trabalho para você:

- Ele cria o novo StoreManagerController com uma variável local do Entity Framework
- Ele adiciona uma pasta de StoreManager à pasta de modos de exibição do projeto
- Ele adiciona a exibição Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml e index. cshtml, fortemente tipada para a classe do álbum

![](mvc-music-store-part-5/_static/image2.png)

A nova classe de controlador StoreManager inclui CRUD (criar, ler, atualizar e excluir) ações do controlador que sabe como trabalhar com o álbum de classe de modelo e usar o nosso contexto de Entity Framework para acesso de banco de dados.

## <a name="modifying-a-scaffolded-view"></a>Modificação de uma exibição gerado por scaffolding

É importante lembrar que, embora esse código foi gerado para nós, ele é código ASP.NET MVC padrão, assim como escrevemos ao longo deste tutorial. Ele tem se destina a economizar tempo que gastaria sobre como escrever o código do controlador clichê e criar as exibições fortemente tipadas manualmente, mas isso não é o tipo de código gerado que talvez você tenha visto precedido com avisos terríveis nos comentários sobre como você não deve alterar a código. Este é seu código, e você deverá alterá-la.

Então, vamos começar com uma edição rápida para o modo de exibição de índice StoreManager (/ Views/StoreManager/Index.cshtml). Este modo de exibição exibe uma tabela que lista os álbuns em nosso repositório com editar / detalhes / excluir links e inclui as propriedades públicas do álbum. Vamos remover o campo AlbumArtUrl, pois ela não é muito útil nessa exibição. Na &lt;tabela&gt; seção do código de exibição, remover o &lt;th&gt; e &lt;td&gt; elementos ao redor AlbumArtUrl referências, conforme indicado pelas linhas destacadas abaixo:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

O código de exibição modificado será exibida da seguinte maneira:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Um primeiro olhar o Store Manager

Agora execute o aplicativo e navegue até/StoreManager /. Isso exibe apenas modificamos, mostrando uma lista dos álbuns no repositório com links para obter detalhes, editar e excluir o índice de Gerenciador de Store.

![](mvc-music-store-part-5/_static/image3.png)

Ao clicar no link de edição exibe um formulário de edição com campos para o álbum, incluindo menus suspensos para gênero e artista.

![](mvc-music-store-part-5/_static/image4.png)

Clique no link de "Volta à lista" na parte inferior, clique no link de detalhes para um álbum. Isso exibe informações detalhadas sobre um álbum individual.

![](mvc-music-store-part-5/_static/image5.png)

Novamente, clique o link voltar ao lista, clique em um link de Delete. Isso exibe uma caixa de diálogo de confirmação, mostrando os detalhes de álbum e perguntando se temos certeza de que deseja excluí-lo.

![](mvc-music-store-part-5/_static/image6.png)

Clicando no botão Excluir na parte inferior, exclui o álbum e de volta para a página de índice, que mostra o álbum excluído.

Não terminamos com o Gerenciador de Store, mas temos controlador e o código de exibição para as operações CRUD iniciar a partir de trabalhar.

## <a name="looking-at-the-store-manager-controller-code"></a>Observando o código do controlador do Store Manager

O controlador do Store Manager contém uma boa quantidade de código. Vamos analisar isso de cima para baixo. O controlador inclui alguns namespaces padrão para um controlador MVC, bem como uma referência ao namespace nossos modelos. O controlador tem uma instância particular da MusicStoreEntities, usado por cada uma das ações do controlador para acesso a dados.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Store Manager Index e Details ações

O modo de exibição de índice recupera uma lista dos álbuns, inclusive referenciado gênero artista informações e do cada álbum, como vimos anteriormente ao trabalhar no método Store procurar. O modo de exibição de índice é seguir as referências a objetos vinculados para que ele possa exibir cada álbum de nome de gênero e nome do artista, para que o controlador está sendo eficiente e de consulta para obter essas informações na solicitação original.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Ação do controlador de detalhes do controlador StoreManager funciona exatamente da mesma forma que a ação de detalhes do controlador de Store que escrevemos anteriormente - ele consulta para o álbum por ID usando o método Find (), em seguida, retorna para o modo de exibição.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>A criar métodos de ação

Os métodos de ação Criar são um pouco diferentes daquelas que vimos até agora, porque eles manipulam a entrada de formulário. Quando um usuário visita primeiro /StoreManager/criar/eles serão mostrados um formulário vazio. Essa página HTML conterá uma &lt;formulário&gt; onde eles podem digitar os detalhes do álbum de elementos de entrada do elemento que contém a lista suspensa e caixa de texto.

Depois que o usuário preenche os valores de formulário do álbum, pressione o botão "Salvar" para enviar que essas alterações de volta para o nosso aplicativo para salvar no banco de dados. Quando o usuário pressiona o botão "Salvar" a &lt;formulário&gt; executará um HTTP-POST para a URL de /StoreManager/criar e enviar o &lt;formulário&gt; valores como parte de um HTTP POST.

ASP.NET MVC permite dividir facilmente a lógica desses dois cenários de invocação de URL, permitindo-nos implementar dois métodos de ação "Criar" separados dentro de nossa classe StoreManagerController – uma para tratar de HTTP GET inicial, navegue para o /StoreManager/Create / URL e o outro para lidar com HTTP-POST, as alterações enviadas.

### <a name="passing-information-to-a-view-using-viewbag"></a>Passar informações para uma exibição usando ViewBag

Usei a ViewBag neste tutorial, mas ainda não falamos muito sobre ele. A ViewBag nos permite passar informações para o modo de exibição sem usar um objeto de modelo fortemente tipado. Nesse caso, nossa ação de controlador HTTP-GET Editar precisa passar uma lista de gêneros e artistas ao formulário para preencher os menus suspensos, e a maneira mais simples de fazer isso é para retorná-las como itens ViewBag.

A ViewBag é um objeto dinâmico, que significa que você pode digitar ViewBag.Foo ou ViewBag.YourNameHere sem escrever código para definir essas propriedades. Nesse caso, o código do controlador usa ViewBag.GenreId e ViewBag.ArtistId para que os valores de lista suspensa enviados com o formulário serão GenreId e ArtistId, são eles definirá as propriedades do álbum.

Esses valores de lista suspensa são retornados para o formulário usando o objeto SelectList, que é criado apenas para essa finalidade. Isso é feito usando o código como este:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Como você pode ver no código do método de ação, três parâmetros estão sendo usados para criar esse objeto:

- A lista de itens que deseja exibir a lista suspensa. Observe que isso não é apenas uma cadeia de caracteres - estamos passando uma lista de gêneros.
- O próximo parâmetro que está sendo passado para o SelectList é o valor selecionado. Este como o SelectList sabe como pre-selecionar um item na lista. Isso será mais fácil de entender quando olhamos o formulário de edição, que é muito semelhante.
- O parâmetro final é a propriedade a ser exibido. Nesse caso, isso está indicando que a propriedade Genre.Name é o que será mostrado ao usuário.

Com isso em mente, em seguida, a ação Criar HTTP GET é bastante simple – duas SelectLists são adicionados ao ViewBag e nenhum objeto de modelo é passado para o formulário (já que ele ainda não foi criado ainda).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Auxiliares HTML para exibir a liste Drop no modo de exibição criar

Uma vez que já falamos sobre como a lista suspensa de valores são passados para o modo de exibição, vamos dar uma olhada rápida na exibição para ver como esses valores são exibidos. No código do modo de exibição (/ Views/StoreManager/Create.cshtml), você verá a seguinte chamada é feita para exibir a lista de gênero para baixo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Isso é conhecido como um auxiliar de HTML - um método de utilitário que executa uma tarefa comum de exibição. Os auxiliares HTML são muito úteis em manter nosso código de exibição concisas e legíveis. O auxiliar Html.DropDownList é fornecido pelo ASP.NET MVC, mas, como veremos posteriormente é possível criar nossos própria auxiliares para exibir o código que vai reutilizar em nosso aplicativo.

A chamada Html.DropDownList apenas precisa ser informado duas coisas - onde para obter a lista para exibir e qual valor (se houver) deve estar pré-selecionado. O primeiro parâmetro, GenreId, informa à DropDownList para procurar um valor chamado GenreId no modelo ou ViewBag. O segundo parâmetro é usado para indicar o valor para mostrar como inicialmente selecionado na lista suspensa. Uma vez que esse formulário é uma forma de criar, não há nenhum valor a ser pré-selecionado e String. Empty é passado.

### <a name="handling-the-posted-form-values"></a>Lidar com os valores de formulário postados

Como abordado anteriormente, há dois métodos de ação associados a cada formulário. O primeiro manipula a solicitação HTTP GET e exibe o formulário. O segundo manipula a solicitação HTTP POST, que contém os valores de formulário enviado. Observe que a ação do controlador tem um atributo [HttpPost], que informa ao ASP.NET MVC que ele só deve responder a solicitações HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Essa ação tem quatro responsabilidades:

- 1. Ler os valores de formulário
- 2. Verifique se os valores de formulário passam qualquer regra de validação
- 3. Se o envio do formulário for válido, salve os dados e exibir a lista atualizada
- 4. Se o envio do formulário não é válido, reexiba o formulário com erros de validação

#### <a name="reading-form-values-with-model-binding"></a>Ler valores de formulário com associação de modelos

A ação do controlador está processando o envio de um formulário que inclui valores para GenreId e ArtistId (na lista suspensa) e os valores de caixa de texto de título, preço e AlbumArtUrl. Embora seja possível acessar diretamente os valores de formulário, uma abordagem melhor é usar os recursos de associação de modelos integrados ASP.NET MVC. Quando uma ação do controlador usa um tipo de modelo como um parâmetro, o ASP.NET MVC tentará popular um objeto desse tipo usando entradas de formulário (bem como valores de rota e de cadeia de consulta). Isso é feito procurando valores cujos nomes correspondem a propriedades do objeto de modelo, por exemplo, ao definir o novo álbum valor do objeto GenreId, ele procura uma entrada com o nome GenreId. Ao criar modos de exibição usando os métodos padrão no ASP.NET MVC, os formulários sempre serão processados usando nomes de propriedades como nomes de campo de entrada, portanto, isso os nomes de campo serão apenas corresponder ao.

#### <a name="validating-the-model"></a>Validação do modelo

O modelo é validado com uma chamada simples para o ModelState. Ainda não adicionamos qualquer regra de validação à nossa classe de álbum ainda – vamos fazer isso daqui a pouco – agora essa verificação não têm muita relação. O importante é que essa verificação ModelStat.IsValid adaptará às regras de validação, colocamos em nosso modelo, para que as alterações futuras em regras de validação não exigirá nenhuma atualização para o código de ação do controlador.

#### <a name="saving-the-submitted-values"></a>Salvando os valores enviados

Se o envio do formulário passar na validação, é hora de salvar os valores no banco de dados. Com o Entity Framework, que requer apenas adicionar modelo à coleção de álbuns e chamar SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework gera os comandos SQL apropriados para manter o valor. Se o texto fornecido é menor que o comprimento especificado, o auxiliar de saídas-lo como-está. Se for mais longo, ele trunca o texto e renderiza..."para o restante. Agora podemos usar nosso auxiliar de truncamento para garantir que as propriedades de título do álbum tanto o nome do artista são menos de 25 caracteres.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>O código de exibição completa usando nosso novo auxiliar de truncamento é exibida abaixo.

Agora quando pesquisamos a URL /StoreManager/, os álbuns e títulos são mantidos abaixo nosso comprimentos máximos. Observação: Isso mostra o caso mais simples de criar e usar um auxiliar em um modo de exibição.

#### <a name="testing-the-create-form"></a>Para saber mais sobre a criação de auxiliares que podem ser usados em todo o site, consulte a postagem do meu blog:

Para testar isso, execute o aplicativo e navegue até /StoreManager/criar / - Isso mostrará o formulário em branco que foi retornado pelo método StoreController criar HTTP-GET.

Preencha alguns valores e clique no botão Criar para enviar o formulário.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Tratamento de edições

O par de ação Editar (HTTP-GET e HTTP POST) é muito semelhante aos métodos de ação Criar que acabamos de ver. Uma vez que o cenário de edição envolve o trabalho com um álbum existente, editar HTTP-GET método carrega o álbum de acordo com o parâmetro de "id", passado por meio da rota. Esse código para recuperar um álbum por AlbumId é o mesmo anteriormente examinamos na ação de controlador de detalhes. Assim como acontece com a criação / método HTTP GET, na lista suspensa de valores são retornados por meio de ViewBag. Isso nos permite retornar um álbum como nosso objeto de modelo para o modo de exibição (que é fortemente tipado para a classe de álbum) ao transmitir dados adicionais (por exemplo, uma lista de gêneros) por meio de ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

A ação HTTP-POST editar é muito semelhante à ação HTTP-POST criar. A única diferença é que, em vez de adicionar um novo álbum ao banco de dados. Coleção de álbuns, podemos está localizando a instância atual do álbum usando o banco de dados. Entry(Album) e definindo seu estado para modificado. Isso informa ao Entity Framework que estamos modificando um álbum existente em vez de criar um novo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Podemos testar isso executando o aplicativo e navegando até/StoreManger /, em seguida, clicar no link de edição para um álbum.

![](mvc-music-store-part-5/_static/image9.png)

Isso exibe o formulário de edição, mostrado pelo método HTTP-GET editar. Preencha alguns valores e clique no botão Salvar.

![](mvc-music-store-part-5/_static/image10.png)

Isso envia o formulário, salva os valores e retorna-à lista de álbum, mostrando que os valores foram atualizados.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Tratamento de exclusão

Exclusão segue o mesmo padrão que editar e criar, usando a ação de um controlador para exibir o formulário de confirmação e outra ação do controlador para manipular o envio do formulário.

A ação do controlador GET HTTP Delete é exatamente o mesmo que nossa ação de controlador Store Manager detalhes anterior.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Podemos exibir um formulário que é fortemente tipado a um tipo de álbum, usando o modelo de conteúdo de modo de exibição de exclusão.

![](mvc-music-store-part-5/_static/image12.png)

O modelo de exclusão mostra todos os campos para o modelo, mas podemos simplificar bastante essa busca. Altere o código de exibição no /Views/StoreManager/Delete.cshtml ao seguinte.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Isso exibe uma confirmação de exclusão simplificada.

![](mvc-music-store-part-5/_static/image13.png)

Clicando no botão Excluir faz com que o formulário a ser postada de volta para o servidor que executa a ação DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Nossa ação de controlador HTTP-POST excluir realiza as seguintes ações:

- 1. Carrega o álbum por ID
- 2. Exclui o álbum e salvar as alterações
- 3. Redireciona para o índice, mostrando que o álbum foi removido da lista

Para testar isso, execute o aplicativo e navegue até /StoreManager. Selecione um álbum na lista e clique no link excluir.

![](mvc-music-store-part-5/_static/image14.png)

Isso exibe nossa tela de confirmação de exclusão.

![](mvc-music-store-part-5/_static/image15.png)

Clicando no botão Excluir remove o álbum e retorna-nos para a página de índice do Store Manager, que mostra que o álbum foi excluído.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Usando um auxiliar de HTML personalizados para truncar o texto

Temos um problema potencial com nossa página de índice do Store Manager. Nossas propriedades do título do álbum e o nome do artista podem ambos ser longo o suficiente que eles poderia lançar desativar a formatação de nossa tabela. Vamos criar um auxiliar de HTML personalizados para permitir que nós truncar facilmente essas e outras propriedades em nossos modos de exibição.

![](mvc-music-store-part-5/_static/image17.png)

Do Razor @helper sintaxe tornou muito fácil criar suas próprias funções auxiliares para uso em exibições. Abra o modo de exibição /Views/StoreManager/Index.cshtml e adicione o código a seguir diretamente após o @model linha.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Esse método auxiliar usa uma cadeia de caracteres e um comprimento máximo para permitir. Se o texto fornecido é menor que o comprimento especificado, o auxiliar de saídas-lo como-está. Se for mais longo, ele trunca o texto e renderiza..."para o restante.

Agora podemos usar nosso auxiliar de truncamento para garantir que as propriedades de título do álbum tanto o nome do artista são menos de 25 caracteres. O código de exibição completa usando nosso novo auxiliar de truncamento é exibida abaixo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Agora quando pesquisamos a URL /StoreManager/, os álbuns e títulos são mantidos abaixo nosso comprimentos máximos.

![](mvc-music-store-part-5/_static/image18.png)

Observação: Isso mostra o caso mais simples de criar e usar um auxiliar em um modo de exibição. Para saber mais sobre a criação de auxiliares que podem ser usados em todo o site, consulte a postagem do meu blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-4.md)
> [Próximo](mvc-music-store-part-6.md)
