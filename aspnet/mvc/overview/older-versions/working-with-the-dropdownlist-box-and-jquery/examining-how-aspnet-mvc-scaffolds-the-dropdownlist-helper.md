---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examinar como o ASP.NET MVC scaffolds o auxiliar DropDownList | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 737773ab424b3ec3b6139b8c238a60ca23de2e69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Examinar como o ASP.NET MVC scaffolds o auxiliar DropDownList
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

Em **Solution Explorer**, com o botão direito do *controladores* pasta e, em seguida, selecione **Adicionar controlador**. Nome do controlador **StoreManagerController**. Definir as opções para o **Adicionar controlador** diálogo conforme mostrado na imagem abaixo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Editar o *StoreManager\Index.cshtml* exibir e remover `AlbumArtUrl`. Removendo `AlbumArtUrl` tornar a apresentação mais legível. O código completo é mostrado abaixo.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Abra o *Controllers\StoreManagerController.cs* de arquivos e localizar o `Index` método. Adicionar o `OrderBy` cláusula para álbuns serão classificados pelo preço. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

A classificação por preço tornará mais fácil de testar as alterações no banco de dados. Quando você estiver testando o editar e cria métodos, você pode usar um baixo preço para que os dados salvos aparecerão primeiro.

Abra o *StoreManager\Edit.cshtml* arquivo. Adicione a linha a seguir logo após a marca de legenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

O código a seguir mostra o contexto dessa alteração:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

O `AlbumId` é necessária para fazer alterações em um registro de álbum.

Pressione CTRL+F5 para executar o aplicativo. Selecione esta opção para o **Admin** link e, em seguida, selecione o **criar novo** link para criar um novo álbum. Verifique se que as informações de álbum foi salvo. Editar um álbum e verifique se as alterações feitas são persistentes.

### <a name="the-album-schema"></a>O esquema de álbum

O `StoreManager` controlador criado pelo mecanismo de scaffolding MVC permite acesso CRUD (criar, ler, atualizar, excluir) em álbuns no banco de dados de repositório de música. O esquema para obter informações de álbum é mostrado abaixo:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

O `Albums` tabela não armazena o gênero álbum e a descrição, ele armazena uma chave estrangeira para a `Genres` tabela. O `Genres` tabela contém o nome de gênero e descrição. Da mesma forma, o `Albums` tabela não contém o nome do álbum artistas, mas uma chave estrangeira para a `Artists` tabela. O `Artists` tabela contém o nome do artista. Se você examinar os dados de `Albums` tabela, você pode ver cada linha contém uma chave estrangeira para a `Genres` tabela e uma chave estrangeira para a `Artists` tabela. A imagem a seguir mostram alguns dados da tabela de `Albums` tabela.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>A marca de seleção HTML

O HTML `<select>` elemento (criado pelo HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar) é usado para exibir uma lista completa de valores (como a lista de gêneros). Para editar formulários, quando o valor atual é conhecido, a lista de seleção pode exibir o valor atual. Vimos anteriormente nesse quando definimos o valor selecionado **comédia**. A lista de seleção é ideal para a exibição de categoria ou dados de chave estrangeira. O `<select>` elemento para a chave estrangeira gênero exibe a lista de nomes de gênero possíveis, mas quando você salvar o formulário a propriedade de gênero é atualizada com o gênero valor de chave estrangeira, não o nome exibido gênero. Na imagem abaixo, o gênero selecionado é **Disco** e artista **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Examinar o ASP.NET MVC Scaffold código

Abra o *Controllers\StoreManagerController.cs* de arquivos e localizar o `HTTP GET Create` método.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

O `Create` método adiciona dois [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objetos para o `ViewBag`, uma para conter as informações de gênero e outra para conter as informações de artista. O [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) sobrecarga de construtor usada acima usa três argumentos:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *itens*: um [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contendo os itens na lista. No exemplo acima, a lista de gêneros retornado por `db.Genres`.
2. *dataValueField*: O nome da propriedade no **IEnumerable** lista que contém o valor da chave. No exemplo acima, `GenreId` e `ArtistId`.
3. *dataTextField*: O nome da propriedade no **IEnumerable** lista que contém as informações a serem exibidas. No artistas e tabela de gênero, o `name` campo será usado.

Abra o *Views\StoreManager\Create.cshtml* de arquivo e examine o `Html.DropDownList` marcação de auxiliar para o campo de gênero.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

A primeira linha mostra que usa o modo de exibição de criar um `Album` modelo. No `Create` método mostrado acima, nenhum modelo foi passado, para o modo de exibição obtém uma **nulo** `Album` modelo. Neste momento, estamos criando um novo álbum para que não temos nenhum `Album` dados para ele.

O [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) sobrecarga mostrada acima obtém o nome do campo para associar ao modelo. Ele também usa esse nome para procurar um **ViewBag** objeto contendo um [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objeto. Usando essa sobrecarga, é necessário o nome de **ViewBag SelectList** objeto `GenreId`. O segundo parâmetro (`String.Empty`) é o texto a ser exibido quando nenhum item selecionado. Isso é exatamente o que queremos ao criar um novo álbum. Se você remover o segundo parâmetro e usou o código a seguir:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

A lista de seleção padrão para o primeiro elemento ou Rock em nosso exemplo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Examinando o `HTTP POST Create` método.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Esta sobrecarga do `Create` leva um `album` objeto, criado pelo sistema de associação do ASP.NET MVC modelo entre os valores de formulário postado. Quando você envia um novo álbum, se o estado do modelo é válido e não há erros de banco de dados, o novo álbum é adicionado o banco de dados. A imagem a seguir mostra a criação de um novo álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Você pode usar o [ferramenta fiddler](http://www.fiddler2.com/fiddler2/) para examinar os valores de formulário postado essa associação de modelo do ASP.NET MVC usa para criar o objeto álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>A criação de ViewBag SelectList de refatoração

Ambos os o `Edit` métodos e `HTTP POST Create` método tem código idêntico para configurar o **SelectList** no **ViewBag**. No espírito do [SECA](http://en.wikipedia.org/wiki/Don't_repeat_yourself), podemos refatorar o esse código. Verifique o uso deste refatorado código mais tarde.

Criar um novo método para adicionar um gênero e artista **SelectList** para o **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Substitua as duas linhas definindo o `ViewBag` em cada uma da `Create` e `Edit` métodos com uma chamada para o `SetGenreArtistViewBag` método. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Criar um novo álbum e editar um álbum para verificar que as alterações estão funcionando.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Explicitamente passando o SelectList para DropDownList

As exibições de criar e editar criadas com o uso do ASP.NET MVC scaffolding seguintes **DropDownList** sobrecarga:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

O `DropDownList` marcação para o modo de exibição de criação é mostrada abaixo.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Porque o `ViewBag` propriedade para o `SelectList` chamado `GenreId`, o **DropDownList** usará o `GenreId` **SelectList** no **ViewBag** . A seguir **DropDownList** sobrecarga, o `SelectList` é passado explicitamente.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Abra o *Views\StoreManager\Edit.cshtml* de arquivo e altere o **DropDownList** chamada explicitamente passe a **SelectList**, usando a sobrecarga acima. Faça isso para a categoria de gênero. O código completo é mostrado abaixo:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Execute o aplicativo e clique no **Admin** link, em seguida, navegue até um álbum Jazz e selecione o **editar** link.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Em vez de Mostrar Jazz como o gênero selecionado no momento, Rock é exibida. Quando o argumento de cadeia de caracteres (a propriedade para associar) e o **SelectList** objeto têm o mesmo nome, o valor selecionado não é usado. Quando não houver nenhum valor selecionado fornecido, os navegadores padrão para o primeiro elemento no **SelectList**(que é **Rock** no exemplo acima). Essa é uma limitação conhecida do **DropDownList** auxiliar.

Abra o *Controllers\StoreManagerController.cs* de arquivo e altere o **SelectList** para os nomes de objeto `Genres` e `Artists`. O código completo é mostrado abaixo:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Os nomes de artistas e gêneros são melhor nomes para as categorias, pois eles contêm mais do que apenas a ID de cada categoria. A refatoração que foi anteriormente paga. Em vez de alterar o **ViewBag** em quatro métodos, nossas alterações sejam isoladas de `SetGenreArtistViewBag` método.

Alterar o **DropDownList** chamar a criar e editar modos de exibição para usar o novo **SelectList** nomes. A marcação de novo para o modo de exibição de edição é mostrada abaixo:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Criar exibição requer uma cadeia de caracteres vazia para impedir que o primeiro item a SelectList que está sendo exibido.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Criar um novo álbum e editar um álbum para verificar que as alterações estão funcionando. Teste o código de edição, selecionando um álbum com um gênero diferente Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Usando um modelo de exibição com o auxiliar DropDownList

Criar uma nova classe na pasta ViewModels denominada `AlbumSelectListViewModel`. Substitua o código no `AlbumSelectListViewModel` classe com o seguinte:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

O `AlbumSelectListViewModel` construtor usa um álbum, uma lista de artistas e gêneros e cria um objeto que contém o álbum e um `SelectList` para artistas e gêneros.

Compilar o projeto para que o `AlbumSelectListViewModel` está disponível quando criamos uma exibição na próxima etapa.

Adicionar uma `EditVM` método para o `StoreManagerController`. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Clique com botão direito `AlbumSelectListViewModel`, selecione **resolver**, em seguida, **usando MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Como alternativa, você pode adicionar a seguinte instrução using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Clique com botão direito `EditVM` e selecione **adicionar exibição**. Use as opções abaixo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Selecione **adicionar**, em seguida, substitua o conteúdo do *Views\StoreManager\EditVM.cshtml* arquivo com o seguinte:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

O `EditVM` marcação é muito semelhante ao valor original `Edit` marcação com as seguintes exceções.

- Propriedades de modelo de `Edit` exibição têm o formato `model.property`(por exemplo, `model.Title` ). Propriedades de modelo de `EditVm` exibição têm o formato `model.Album.property`(por exemplo, `model.Album.Title`). Isso ocorre porque o `EditVM` exibição é passada um contêiner um `Album`, não um `Album` como no `Edit` exibição.
- O **DropDownList** segundo parâmetro é proveniente do modelo de exibição, não o **ViewBag**.
- O **BeginForm** auxiliar a `EditVM` exibição explicitamente envia de volta para o `Edit` método de ação. Postando de volta para o `Edit` ação, não temos que gravar um `HTTP POST EditVM` ação e pode reutilizar o `HTTP POST` `Edit` ação.

Execute o aplicativo e editar um álbum. Altere a URL para usar `EditVM`. Alterar um campo e pressione a **salvar** botão para verificar o código está funcionando.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Qual abordagem você deve usar?

Todas as três abordagens mostradas são acceptible. Muitos desenvolvedores preferem passagem explictily o `SelectList` para o `DropDownList` usando o `ViewBag`. Essa abordagem tem a vantagem adicional de fornecer a flexibilidade de usar um nome mais apropriado para a coleção. Uma limitação é que você não pode nomear o `ViewBag SelectList` o mesmo nome que a propriedade do modelo de objeto.

Alguns desenvolvedores preferem a abordagem de ViewModel. Outros considere mais detalhado marcação e o código HTML gerado do ViewModel abordagem uma desvantagem.

Nesta seção aprendemos três abordagens para usar o **DropDownList** com dados de categoria. Na próxima seção, mostraremos como adicionar uma nova categoria.

>[!div class="step-by-step"]
[Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Próximo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
