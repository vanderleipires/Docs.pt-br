---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: "Parte 5: Formulários de edição e modelagem | Microsoft Docs"
author: jongalloway
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 5 abrange formulários de edição e modelagem."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: cde6fe133291254531a797a434a4b2cdd226dd5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-5-edit-forms-and-templating"></a>Parte 5: Formulários de edição e modelagem
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.
> 
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 5 abrange formulários de edição e modelagem.


O capítulo anterior, nós foram carregamento de dados de nosso banco de dados e exibi-lo. Neste capítulo, também deverá habilitar edição dos dados.

## <a name="creating-the-storemanagercontroller"></a>Criando o StoreManagerController

Vamos começar criando um novo controlador chamado **StoreManagerController**. Para este controlador, faremos aproveitar os recursos de Scaffolding disponíveis na atualização das ferramentas do ASP.NET MVC 3. Defina as opções para a caixa de diálogo Adicionar controlador, conforme mostrado abaixo.

![](mvc-music-store-part-5/_static/image1.png)

Quando você clica no botão Adicionar, você verá que o mecanismo do ASP.NET MVC 3 scaffolding faz uma boa quantidade de trabalho para você:

- Cria o novo StoreManagerController com uma variável local do Entity Framework
- Ele adiciona uma pasta StoreManager para a pasta de modos de exibição do projeto
- Ele adiciona exibição Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml e cshtml, fortemente tipada para a classe álbum

![](mvc-music-store-part-5/_static/image2.png)

A nova classe de controlador StoreManager inclui CRUD (criar, ler, atualizar e excluir) ações do controlador que sabem como trabalhar com o álbum de classe de modelo e usar o nosso contexto do Entity Framework para acesso ao banco de dados.

## <a name="modifying-a-scaffolded-view"></a>Modificação de uma exibição de scaffolding

É importante lembrar-se de que, embora este código foi gerado para nós, é código padrão do ASP.NET MVC, assim como o você escreve em todo este tutorial. Ele tem se destina a economizar tempo você levaria em escrever o código do controlador clichê e criar os modos de exibição fortemente tipados manualmente, mas isso não é do tipo de código gerado que você pode ter visto precedido de avisos terríveis nos comentários sobre como você não deve alterar o código. Este é seu código, e você deverá alterá-lo.

Portanto, vamos começar com uma edição rápida para o modo de exibição do índice StoreManager (/ Views/StoreManager/Index.cshtml). Essa exibição mostrará uma tabela que lista os álbuns em nosso repositório com editar / detalhes / excluir links e inclui as propriedades públicas do álbum. Vamos removê campo AlbumArtUrl, pois não é muito útil nessa exibição. Em &lt;tabela&gt; seção do código de exibição, remova o &lt;th&gt; e &lt;td&gt; elementos ao redor AlbumArtUrl referências, como indicado pelas linhas realçadas abaixo:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

O código de modo de exibição modificado será exibida da seguinte maneira:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Um primeiro olhar o Gerenciador de armazenamento

Agora execute o aplicativo e navegue até/StoreManager /. Isso exibe apenas modificamos, mostrando uma lista dos álbuns no repositório com links para obter detalhes, editar e excluir o índice de Gerenciador de repositório.

![](mvc-music-store-part-5/_static/image3.png)

Clicar no link de edição exibe um formulário de edição com campos por álbum, incluindo listas suspensas para gênero e artista.

![](mvc-music-store-part-5/_static/image4.png)

Clique no link "Voltar à lista" na parte inferior, clique no link de detalhes para um álbum. Isso exibe informações detalhadas sobre um álbum individual.

![](mvc-music-store-part-5/_static/image5.png)

Novamente, clique em Voltar para o link de lista, clique no link excluir. Isso exibe uma caixa de diálogo de confirmação, mostrando os detalhes do álbum e perguntando se podemos tem certeza de que deseja excluí-lo.

![](mvc-music-store-part-5/_static/image6.png)

Clicando no botão Excluir na parte inferior excluirá o álbum e você retorna à página de índice, que mostra o álbum excluído.

Não terminamos com o Gerenciador de armazenamento, mas é necessário trabalhar controlador e o código de exibição para as operações CRUD Iniciar em.

## <a name="looking-at-the-store-manager-controller-code"></a>Examinar o código do controlador do Gerenciador de armazenamento

O controlador do Gerenciador de armazenamento contém uma boa quantidade de código. Vamos por meio de cima para baixo. O controlador inclui alguns namespaces padrão para um controlador MVC, bem como uma referência ao namespace nossos modelos. O controlador tem uma instância particular da MusicStoreEntities, usado por cada uma das ações do controlador para acesso a dados.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Armazenar ações do Gerenciador de índice e detalhes

A exibição índice recupera uma lista de álbuns, inclusive do cada álbum referenciadas gênero artista informações e, como visto anteriormente ao trabalhar no método Store procurar. O modo de exibição do índice está seguindo as referências a objetos vinculados para que ele possa exibir cada álbum gênero e artista nome, para que o controlador está sendo eficiente e consultar para obter essas informações na solicitação original.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Ação de controlador de detalhes do controlador StoreManager funciona exatamente como a ação de detalhes do controlador de armazenamento escrevemos anteriormente - ele consulta para o álbum por ID usando o método Find(), em seguida, retorna ao modo de exibição.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>A criar métodos de ação

Os métodos de ação Criar são um pouco diferentes daquelas que vimos até agora, porque eles tratam de entrada de formulário. Quando um usuário acessa primeiro /StoreManager/criação/eles serão mostrados um formulário vazio. Esta página HTML conterá um &lt;formulário&gt; onde eles podem digitar os detalhes do álbum de elementos de entrada do elemento que contém a lista suspensa e caixa de texto.

Depois que o usuário preenche os valores de formulário álbum, pressione o botão "Salvar" para enviar que essas alterações de volta para o nosso aplicativo ao salvar no banco de dados. Quando o usuário pressiona o botão "Salvar" o &lt;formulário&gt; executará um HTTP POST para a URL de /StoreManager/criação/e enviar o &lt;formulário&gt; valores como parte do POST HTTP.

ASP.NET MVC permite dividir facilmente a lógica desses dois cenários de invocação de URL, permitindo que nós a implementação de dois métodos de ação "Criar" separados em nossa classe StoreManagerController – um para tratar de HTTP GET inicial, navegue para o /StoreManager/Create / URL e a outra para manipular o HTTP POST das alterações enviadas.

### <a name="passing-information-to-a-view-using-viewbag"></a>Passar informações para uma exibição usando ViewBag

Usei a ViewBag anteriormente neste tutorial, mas não falamos muito sobre ele. A ViewBag nos permite passar informações para o modo de exibição sem usar um objeto de modelo com rigidez de tipos. Nesse caso, nossa ação do controlador Editar HTTP GET precisa passar uma lista de artistas e gêneros para o formulário para preencher as listas suspensas e a maneira mais simples de fazer isso é para retorná-los como itens ViewBag.

A ViewBag é um objeto dinâmico, o que significa que você pode digitar ViewBag.Foo ou ViewBag.YourNameHere sem escrever código para definir essas propriedades. Nesse caso, o código do controlador usa ViewBag.GenreId e ViewBag.ArtistId para que os valores de lista suspensa enviados com o formulário será GenreId e ArtistId, que são as propriedades de álbum estiver configurando.

Esses valores de lista suspensa são retornados para o formulário usando o objeto SelectList, que é criado apenas para essa finalidade. Isso é feito usando código como este:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Como você pode ver do código do método de ação, três parâmetros são sendo usados para criar este objeto:

- A lista de itens que deseja exibir a lista suspensa. Observe que isso não é apenas uma cadeia de caracteres – estamos passando uma lista de gêneros.
- O próximo parâmetro que está sendo passado para o SelectList é o valor selecionado. Este como o SelectList sabe como pre-selecionar um item na lista. Isso será mais fácil de entender quando vamos examinar o formulário de edição, o que é bastante semelhante.
- O parâmetro final é a propriedade a ser exibida. Nesse caso, isso é que indica que a propriedade Genre.Name é o que será mostrado ao usuário.

Com isso em mente, em seguida, a ação Criar HTTP GET é bastante simple - dois SelectLists são adicionados a ViewBag e nenhum objeto de modelo é passado para o formulário (desde que ele não tenha sido criado).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Auxiliares HTML para exibir a liste Drop em Create View

Como falamos sobre como a lista suspensa de valores são passados para o modo de exibição, vamos dar uma olhada rápida no modo de exibição para ver como esses valores são exibidos. No código de exibição (/ Views/StoreManager/Create.cshtml), você verá a seguinte chamada é feita para exibir o descarte de gênero para baixo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Isso é conhecido como um auxiliar HTML - um método de utilitário que executa uma tarefa comum do modo de exibição. Auxiliares HTML são muito úteis para manter nossos Exibir código conciso e legível. O auxiliar Html.DropDownList é fornecido pelo ASP.NET MVC, mas como você verá posteriormente é possível criar nossos própria auxiliares para exibir o código que reutilizará em nosso aplicativo.

A chamada de Html.DropDownList só precisa ser informado que duas coisas - onde obter a lista para exibir e qual é o valor (se houver) deve ser pré-selecionada. O primeiro parâmetro, GenreId, informa DropDownList para procurar um valor chamado GenreId no modelo ou ViewBag. O segundo parâmetro é usado para indicar o valor para mostrar como inicialmente selecionado na lista suspensa. Como este formulário é um formulário de criação, não há nenhum valor seja pré-selecionados e Empty é passado.

### <a name="handling-the-posted-form-values"></a>Tratamento de valores de formulário postado

Conforme abordado anteriormente, há dois métodos de ação associados a cada formulário. O primeiro manipula a solicitação HTTP GET e exibe o formulário. O segundo manipula a solicitação HTTP POST, que contém os valores de formulário enviado. Observe que a ação do controlador tem um atributo [HttpPost], que faz com que o ASP.NET MVC que ele só deve responder a solicitações HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Essa ação tem quatro responsabilidades:

- 1. Ler os valores de formulário
- 2. Verifique se os valores de formulário passam todas as regras de validação
- 3. Se o envio do formulário é válido, salve os dados e exibir a lista atualizada
- 4. Se o envio do formulário não é válido, reexiba o formulário com erros de validação

#### <a name="reading-form-values-with-model-binding"></a>Valores de formulário de leitura com associação de modelo

Ação do controlador está processando uma submissão de formulário que inclui valores de GenreId e ArtistId (na lista suspensa) e os valores de caixa de texto de título, preço e AlbumArtUrl. Embora seja possível acessar diretamente os valores de formulário, uma abordagem melhor é usar os recursos de associação de modelo criados no ASP.NET MVC. Quando uma ação do controlador tem um tipo de modelo como um parâmetro, o ASP.NET MVC tentará preencher um objeto desse tipo usando entradas de formulário (bem como valores de rota e querystring). Isso é feito procurando valores cujos nomes correspondem a propriedades do objeto de modelo, por exemplo, ao configurar o novo álbum valor do objeto GenreId, ele procura uma entrada com o nome GenreId. Quando você criar modos de exibição usando os métodos padrão do ASP.NET MVC, os formulários sempre serão processados usando nomes de propriedades como nomes de campo de entrada, para que isso os nomes de campo apenas coincidirá.

#### <a name="validating-the-model"></a>Validação do modelo

O modelo é validado com uma chamada simple para o ModelState. Ainda não adicionamos quaisquer regras de validação para nossa classe álbum ainda - faremos isso um pouco - direito agora essa verificação não tem muito o que fazer. O importante é que essa verificação ModelStat.IsValid adaptará às regras de validação que colocamos em nosso modelo, para que as alterações futuras para regras de validação não necessitam de atualizações para o código de ação do controlador.

#### <a name="saving-the-submitted-values"></a>Salvar os valores enviados

Se o envio do formulário passar na validação, é hora de salvar os valores para o banco de dados. Com o Entity Framework, que exige apenas adicionar modelo à coleção álbuns e chamar SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework gera os comandos SQL apropriados para manter o valor. Depois de salvar os dados, podemos redirecionar voltar à lista de álbuns então podemos ver nossa atualização. Isso é feito retornando RedirectToAction com o nome da ação do controlador que deseja exibir. Nesse caso, que é o método de índice.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Exibindo os envios de formulário inválido com erros de validação

No caso de entrada de forma inválida, os valores de lista suspensa são adicionados ao ViewBag (como no caso de HTTP GET) e os valores do modelo associado são retransmitidos para o modo de exibição para exibição. Erros de validação são exibidos automaticamente usando o @Html.ValidationMessageFor auxiliar HTML.

#### <a name="testing-the-create-form"></a>O formulário de criação de teste

Para testar isso, execute o aplicativo e navegue até /StoreManager/criação / - Isso mostrará o formulário em branco que foi retornado pelo método StoreController criar HTTP GET.

Preencha alguns valores e clique no botão Criar para enviar o formulário.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Tratamento de edições

O par de ação de edição (HTTP GET e POST HTTP) é muito semelhante aos métodos de ação de criar que acabamos de ver. Como o cenário de edição envolve trabalhar com um álbum existente, editar HTTP-GET método carrega o álbum com base no parâmetro "id", passado por meio de rota. Esse código para recuperar um álbum por AlbumId é o mesmo anteriormente vimos na ação de controlador de detalhes. Assim como acontece com a criação / método HTTP GET, na lista suspensa de valores são retornados por meio de ViewBag. Isso nos permite retornar um álbum como nosso objeto de modelo para o modo de exibição (que é fortemente tipado para a classe álbum) passando dados adicionais (por exemplo, uma lista de gêneros) via ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

A ação de editar HTTP POST é muito semelhante à ação Criar HTTP POST. A única diferença é que, em vez de adicionar um novo álbum ao banco de dados. Coleção de álbuns, podemos está localizando a instância atual do álbum usando o banco de dados. Entry(Album) e definir seu estado como modificado. Isso informa o Entity Framework que estamos modificando um álbum existente em vez de criar um novo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Podemos testar isso executando o aplicativo e navegando até StoreManger /, em seguida, clique no link de edição para um álbum.

![](mvc-music-store-part-5/_static/image9.png)

Exibe o formulário de edição mostrado pelo método Editar HTTP GET. Preencha alguns valores e clique no botão Salvar.

![](mvc-music-store-part-5/_static/image10.png)

Isso envia o formulário, salva os valores e retorna à lista de álbum, mostrando que os valores foram atualizados.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Tratamento de exclusão

Exclusão segue o mesmo padrão como editar e criar, usando a ação de um controlador para exibir o formulário de confirmação e outra ação de controlador para lidar com o envio do formulário.

A ação de controlador HTTP GET e Delete é exatamente o nosso ação de controlador de detalhes do Gerenciador de armazenamento anterior.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Podemos exibir um formulário que é fortemente tipado para um tipo de álbum, usando o modelo de conteúdo de modo de exibição de exclusão.

![](mvc-music-store-part-5/_static/image12.png)

O modelo de exclusão mostra todos os campos para o modelo, mas podemos simplificar que busca um pouco. Altere o código de exibição no /Views/StoreManager/Delete.cshtml para o seguinte.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Isso exibirá uma confirmação de exclusão simplificada.

![](mvc-music-store-part-5/_static/image13.png)

Clicando no botão Excluir faz com que o formulário a ser lançado para o servidor que executa a ação de DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Ação do controlador excluir nosso HTTP POST executa as seguintes ações:

- 1. Carrega o álbum por ID
- 2. Exclui o álbum e salvar as alterações
- 3. Redireciona para o índice, mostrando que o álbum foi removido da lista

Para testar isso, execute o aplicativo e navegue até /StoreManager. Selecione um álbum na lista e clique no link excluir.

![](mvc-music-store-part-5/_static/image14.png)

Isso exibe a nossa tela de confirmação de exclusão.

![](mvc-music-store-part-5/_static/image15.png)

Clicando no botão Excluir remove o álbum e retorna à página de índice de Gerenciador de armazenamento, que mostra que o álbum foi excluído.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Usando um auxiliar HTML personalizado para truncar o texto

Temos um possível problema com nossa página de índice do Gerenciador de armazenamento. Nosso propriedades álbum e artista nome podem ambos ser suficientemente longa que eles podem acionar desativar a formatação de nossa tabela. Vamos criar um auxiliar HTML personalizado para permitir que nós truncar facilmente essas e outras propriedades no nosso modos de exibição.

![](mvc-music-store-part-5/_static/image17.png)

Do Razor @helper sintaxe tornou muito fácil criar suas próprias funções auxiliares para uso em exibições. Abra o modo de exibição /Views/StoreManager/Index.cshtml e adicione o código a seguir diretamente após o @model linha.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Esse método auxiliar usa uma cadeia de caracteres e um comprimento máximo para permitir. Se o texto fornecido é menor que o comprimento especificado, o auxiliar gera como-é. Se for mais longo, ele truncará o texto e renderiza "..." para o restante.

Agora podemos usar nossa Truncate auxiliar para garantir que o título do álbum e o nome de artista propriedades menos de 25 caracteres. O código de exibição completa usando nosso novo auxiliar de truncamento é exibida abaixo.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Agora quando pesquisamos a URL /StoreManager/, álbuns de títulos são mantidos abaixo os comprimentos máximos.

![](mvc-music-store-part-5/_static/image18.png)

Observação: Isso mostra o caso mais simples de criar e usar um auxiliar em uma exibição. Para saber mais sobre a criação de auxiliares que podem ser usados em todo o site, consulte o postagem de blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


>[!div class="step-by-step"]
[Anterior](mvc-music-store-part-4.md)
[Próximo](mvc-music-store-part-6.md)
