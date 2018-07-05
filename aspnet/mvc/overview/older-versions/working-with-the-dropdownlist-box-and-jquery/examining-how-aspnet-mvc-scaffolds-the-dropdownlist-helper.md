---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examinando como o ASP.NET MVC faz Scaffold do auxiliar DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: ece3645f2b37550058a5c93bdd9badbc088ae11c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382959"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Examinando como o ASP.NET MVC faz Scaffold do auxiliar DropDownList
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

Na **Gerenciador de soluções**, clique com botão direito do *controladores* pasta e, em seguida, selecione **Adicionar controlador**. Nomeie o controlador **StoreManagerController**. Defina as opções para o **Adicionar controlador** caixa de diálogo, conforme mostrado na imagem abaixo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Editar o *StoreManager\Index.cshtml* exibir e remover `AlbumArtUrl`. Removendo `AlbumArtUrl` tornar a apresentação mais legível. O código completo é mostrado abaixo.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Abra o *Controllers\StoreManagerController.cs* do arquivo e encontre o `Index` método. Adicionar o `OrderBy` cláusula para que os álbuns serão classificados por preço. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Classificar por preço tornará mais fácil de testar as alterações no banco de dados. Quando você estiver testando o editar e cria métodos, você pode usar um baixo preço, para que os dados salvos aparecerão primeiros.

Abra o *StoreManager\Edit.cshtml* arquivo. Adicione a linha a seguir logo após a marca de legenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

O código a seguir mostra o contexto dessa alteração:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

O `AlbumId` é necessária para fazer alterações em um registro de álbum.

Pressione CTRL+F5 para executar o aplicativo. Selecione esta opção para o **Admin** vincular e, em seguida, selecione o **criar novo** link para criar um novo álbum. Verifique se que as informações de álbum foi salvo. Editar um álbum e verifique se as alterações feitas serão mantidas.

### <a name="the-album-schema"></a>O esquema do álbum

O `StoreManager` controlador criado pelo mecanismo de scaffolding MVC permite o acesso CRUD (Create, Read, Update, Delete) aos álbuns no banco de dados de loja de música. O esquema para obter informações de álbum é mostrado abaixo:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

O `Albums` tabela não armazena o gênero de álbum e a descrição, ele armazena uma chave estrangeira para a `Genres` tabela. O `Genres` tabela contém o nome de gênero e uma descrição. Da mesma forma, o `Albums` a tabela não contém o nome do álbum artistas, mas uma chave estrangeira para a `Artists` tabela. O `Artists` tabela contém o nome do artista. Se você examinar os dados a `Albums` tabela, você pode ver cada linha contém uma chave estrangeira para a `Genres` tabela e uma chave estrangeira para a `Artists` tabela. A imagem a seguir mostram alguns dados da tabela de `Albums` tabela.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>A marca de seleção HTML

O HTML `<select>` elemento (criado pelo HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar) é usado para exibir uma lista completa de valores (como a lista de gêneros). Para formulários de edição, quando o valor atual é conhecido, a lista de seleção pode exibir o valor atual. Vimos anteriormente nesse quando definimos o valor selecionado **comédia**. A lista de seleção é ideal para a exibição de categoria ou dados de chave estrangeira. O `<select>` elemento para a chave estrangeira do gênero exibe a lista de nomes de gênero possíveis, mas quando você salvar o formulário a propriedade de gênero é atualizada com o gênero valor de chave estrangeira, não o nome exibido do gênero. Na imagem abaixo, é o gênero selecionado **Disco** e é o artista **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Código gerado automaticamente examinar o ASP.NET MVC

Abra o *Controllers\StoreManagerController.cs* do arquivo e encontre o `HTTP GET Create` método.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

O `Create` método adiciona duas [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objetos para o `ViewBag`, uma para conter as informações de gênero e outra para conter a informações sobre o artista. O [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) sobrecarga do construtor usada acima usa três argumentos:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *itens*: uma [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contém os itens na lista. No exemplo acima, a lista de gêneros é retornado por `db.Genres`.
2. *dataValueField*: O nome da propriedade na **IEnumerable** lista que contém o valor da chave. No exemplo acima, `GenreId` e `ArtistId`.
3. *dataTextField*: O nome da propriedade na **IEnumerable** lista que contém as informações a serem exibidas. No artistas e tabela de gênero, o `name` campo é usado.

Abra o *Views\StoreManager\Create.cshtml* do arquivo e examine o `Html.DropDownList` marcação do auxiliar para o campo gênero.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

A primeira linha mostra que o modo de exibição criar leva um `Album` modelo. No `Create` método mostrado acima, nenhum modelo foi passado, portanto, a exibição obtém um **nulo** `Album` modelo. Agora que estamos criando um novo álbum portanto, não há nenhum `Album` dados para ele.

O [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) sobrecarga mostrada acima usa o nome do campo a ser associado ao modelo. Ele também usa esse nome para procurar uma **ViewBag** objeto que contém uma [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objeto. Usando essa sobrecarga, é necessário nome de **ViewBag SelectList** objeto `GenreId`. O segundo parâmetro (`String.Empty`) é o texto a ser exibido quando nenhum item está selecionado. Isso é exatamente o que queremos ao criar um novo álbum. Se você remover o segundo parâmetro e usado o código a seguir:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

A lista de seleção padrão para o primeiro elemento ou Rock em nosso exemplo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Examinando o `HTTP POST Create` método.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Essa sobrecarga da `Create` leva um `album` objeto, criado pelo sistema de associação de modelo ASP.NET MVC dos valores de formulário postados. Quando você envia um novo álbum, se o estado do modelo é válido e não há nenhum erro de banco de dados, o novo álbum é adicionado o banco de dados. A imagem a seguir mostra a criação de um novo álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Você pode usar o [ferramenta fiddler](http://www.fiddler2.com/fiddler2/) para examinar os valores de formulário postados essa associação de modelos do ASP.NET MVC usa para criar o objeto de álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>A criação de ViewBag SelectList de refatoração

Os dois o `Edit` métodos e o `HTTP POST Create` método ter código idêntico para configurar a **SelectList** no **ViewBag**. No espírito do [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), podemos será refatorar esse código. Vamos fazer uso disso refatorado código mais tarde.

Criar um novo método para adicionar um gênero e artista **SelectList** para o **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Substitua as duas linhas definindo a `ViewBag` em cada uma da `Create` e `Edit` métodos com uma chamada para o `SetGenreArtistViewBag` método. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Criar um novo álbum e editar um álbum para verificar que as alterações funcionem.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Passar explicitamente o SelectList para DropDownList

Os modos de exibição criar e editar criado pelo uso de scaffolding do ASP.NET MVC o seguinte **DropDownList** sobrecarga:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

O `DropDownList` marcação da exibição create é mostrada abaixo.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Porque o `ViewBag` propriedade para o `SelectList` é denominado `GenreId`, o **DropDownList** auxiliar usará o `GenreId` **SelectList** no **ViewBag** . A seguir **DropDownList** sobrecarga, o `SelectList` é passado explicitamente.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Abra o *Views\StoreManager\Edit.cshtml* de arquivo e altere o **DropDownList** chamada transmitir explicitamente o **SelectList**, usando a sobrecarga acima. Faça isso para a categoria de gênero. O código completo é mostrado abaixo:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Execute o aplicativo e clique no **Admin** vincular, em seguida, navegue até um álbum de Jazz e selecione o **editar** link.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Em vez de Mostrar Jazz como o gênero selecionado no momento, Rock é exibida. Quando o argumento de cadeia de caracteres (a propriedade para associar) e o **SelectList** objeto têm o mesmo nome, o valor selecionado não é usado. Quando não houver nenhum valor selecionado fornecido, os navegadores padrão para o primeiro elemento na **SelectList**(que é **Rock** no exemplo acima). Essa é uma limitação conhecida do **DropDownList** auxiliar.

Abra o *Controllers\StoreManagerController.cs* do arquivo e altere o **SelectList** objeto nomes a serem `Genres` e `Artists`. O código completo é mostrado abaixo:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Os nomes dos gêneros e artistas são nomes melhores para as categorias, pois eles contêm mais do que apenas a ID de cada categoria. A refatoração que fizemos anteriormente valeu. Em vez de alterar o **ViewBag** em quatro métodos, nossas alterações sejam isoladas o `SetGenreArtistViewBag` método.

Alterar o **DropDownList** chamar a criar e editar modos de exibição para usar a nova **SelectList** nomes. A nova marcação para a exibição de edição é mostrada abaixo:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Criar exibição requer uma cadeia de caracteres vazia para impedir que o primeiro item no SelectList que está sendo exibida.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Criar um novo álbum e editar um álbum para verificar que as alterações funcionem. Teste o código de edição, selecionando um álbum com um gênero diferente de Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Usando um modelo de exibição com o auxiliar do DropDownList

Crie uma nova classe na pasta ViewModels chamada `AlbumSelectListViewModel`. Substitua o código no `AlbumSelectListViewModel` classe com o seguinte:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

O `AlbumSelectListViewModel` construtor usa um álbum, uma lista dos artistas e gêneros e cria um objeto que contém o álbum e um `SelectList` para gêneros e artistas.

Compile o projeto para que o `AlbumSelectListViewModel` está disponível quando criamos uma exibição na próxima etapa.

Adicionar um `EditVM` método para o `StoreManagerController`. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Clique com botão direito `AlbumSelectListViewModel`, selecione **resolver**, em seguida, **usando MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Como alternativa, você pode adicionar a seguinte instrução using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Clique com botão direito `EditVM` e selecione **adicionar exibição**. Use as opções mostradas abaixo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Selecione **Add**, em seguida, substitua o conteúdo do *Views\StoreManager\EditVM.cshtml* arquivo com o seguinte:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

O `EditVM` marcação é muito semelhante ao original `Edit` marcação com as seguintes exceções.

- Propriedades de modelo a `Edit` modo de exibição estão no formato `model.property`(por exemplo, `model.Title` ). Propriedades de modelo a `EditVm` modo de exibição estão no formato `model.Album.property`(por exemplo, `model.Album.Title`). Isso ocorre porque o `EditVM` exibição é passada a um contêiner um `Album`, e não um `Album` como no `Edit` modo de exibição.
- O **DropDownList** segundo parâmetro vem do modelo de exibição, não a **ViewBag**.
- O **BeginForm** auxiliar na `EditVM` modo de exibição explicitamente envia de volta para o `Edit` método de ação. Ao postar de volta para o `Edit` ação, não temos de escrever um `HTTP POST EditVM` ação e pode reutilizar as `HTTP POST` `Edit` ação.

Execute o aplicativo e editar um álbum. Altere a URL para usar `EditVM`. Alterar um campo e pressione a **salvar** botão para verificar o código está funcionando.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>A abordagem que você deve usar?

Todas as três abordagens mostradas são acceptible. Muitos desenvolvedores preferem explictily pass a `SelectList` para o `DropDownList` usando o `ViewBag`. Essa abordagem tem a vantagem adicional de oferecer a você a flexibilidade de usar um nome mais apropriado para a coleção. Uma limitação é que você não pode nomear o `ViewBag SelectList` o mesmo nome que a propriedade do modelo de objeto.

Alguns desenvolvedores preferem a abordagem do ViewModel. Outros considere a marcação mais detalhada e gerado HTML da abordagem ViewModel uma desvantagem.

Nesta seção aprendemos três abordagens para usar o **DropDownList** com os dados de categoria. Na próxima seção, mostraremos como adicionar uma nova categoria.

> [!div class="step-by-step"]
> [Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Próximo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
