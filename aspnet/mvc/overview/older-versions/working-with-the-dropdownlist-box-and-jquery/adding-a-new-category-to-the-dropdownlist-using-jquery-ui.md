---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Adicionando uma nova categoria a DropDownList usando jQuery UI | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 16f7af1d679aace24fff86abb19740beebafe785
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871929"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Adicionando uma nova categoria a DropDownList usando jQuery UI
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

O HTML `Select` marca é ideal para apresentar uma lista de dados de categoria fixo, mas muitas vezes você precisa adicionar uma nova categoria. Vamos supor que queremos adicionar o gênero "Opera" às categorias em nosso banco de dados? Nesta seção, usaremos o jQuery UI para adicionar uma caixa de diálogo que podemos usar para adicionar uma nova categoria. A imagem abaixo mostra como a interface do usuário será apresentado no navegador.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Quando um usuário seleciona o **adicionar novo gênero** link, uma caixa de diálogo pop-up solicita ao usuário para um novo nome de gênero (e, opcionalmente, uma descrição). A imagem a seguir mostram o **Adicionar Gênero** caixa de diálogo pop-up.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Quando um novo nome de gênero é inserido e o **salvar** botão é pressionado, ocorre o seguinte:

1. Uma chamada AJAX envia os dados para o método de criação do controlador de gênero, que salva o gênero novo ao banco de dados e retorna as novo gênero informações (nome de gênero e ID) como JSON.
2. JavaScript adiciona os novos dados de gênero à lista de seleção.
3. JavaScript torna o novo gênero o item selecionado.

   Na imagem abaixo, **Opera** foi adicionada ao banco de dados e selecionado no **gênero** lista suspensa. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Abra o *Views\StoreManager\Create.cshtml* de arquivo e substitua a marcação de gênero a seguir o código a seguir:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

O `_ChooseGenre` contém toda a lógica para ligar o JavaScript e jQuery usado para implementar a funcionalidade de gênero adicionar nova exibição parcial. Depois que concluir o código será simple fazer o mesmo com o interface do usuário artista.

No Gerenciador de soluções, clique com botão direito do *Views\StoreManager* pasta e selecione **adicionar**, em seguida, **exibição**. No **nome de exibição** de entrada, digite `_ChooseGenre` , em seguida, selecione **adicionar**. Substitua a marcação no *Views\StoreManager\\_ChooseGenre.cshtml* arquivo com o seguinte:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

A primeira linha declara que estamos estão passando um `Album` como nosso modelo, exatamente da mesma instrução encontrado na exibição de criação de modelo. As próximas linhas são o **rótulo** marcação auxiliar. A próxima linha é o **DropDownList** auxiliar chamar, exatamente como o modo de exibição de criação original. A próxima linha adiciona um link com o nome `Add New Genre`, e os estilos-lo como um botão. A linha que contém `ValidationMessageFor` é copiado diretamente da exibição da criação. As seguintes linhas:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

cria um div ocultado, com a ID do `genreDialog`. Usaremos jQuery para ligar nosso **Adicionar Gênero** caixa de diálogo com a ID `genreDialog` neste div. As últimas duas marcas de script contêm links para os arquivos JavaScript que usaremos para implementar a funcionalidade de gênero adicionar novo. O */Scripts/chooseGenre.js* arquivo é fornecido por você no projeto, vamos examiná-lo posteriormente no tutorial.

Execute o aplicativo e clique no **adicionar novo gênero** botão. No **Adicionar Gênero** caixa de diálogo, digite **Opera** no **nome** caixa de entrada.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Clique no botão **Salvar**. Uma chamada AJAX cria a categoria Opera e, em seguida, popula a lista suspensa com Opera e define Opera como o gênero selecionado.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Insira um artista, o título e o preço, em seguida, selecione o **criar** botão. Se você inserir um preço menor que US $8.99, o novo álbum aparecerá na parte superior do modo de exibição do índice. Verifique se que a nova entrada de álbum foi salvo no banco de dados.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Tente criar uma nova forma com apenas uma letra. O código a seguir no *Models\Genre.cs* arquivo define o comprimento mínimo e máximo do nome do gênero.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Validação do lado do cliente informa que você deve digitar uma cadeia de caracteres entre 2 e 20 caracteres.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Examinar como um gênero de novo é adicionado ao banco de dados e a lista Select.

Abra o *Scripts\chooseGenre.js* de arquivo e examine o código.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

A segunda linha usa o ID `genreDialog` para criar uma caixa de diálogo na marca div no *Views\StoreManager\\_ChooseGenre.cshtml* arquivo. A maioria dos parâmetros nomeados são self explicativos. O `autoOpen` parâmetro for definido como falso, selecionando o **criar gênero** botão abre a caixa de diálogo explicitamente (Isso é descrito último em). A caixa de diálogo tem dois botões, **salvar** e **Cancelar**. O **Cancelar** botão fecha a caixa de diálogo. O código a seguir mostra o **salvar** botão de função.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

O `var createGenreForm` é selecionado do `createGenreForm` ID. O `createGenreForm` ID foi definido no código a seguir encontrado no *Views\Genre\\_CreateGenre.cshtml* arquivo.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

O [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) sobrecarga auxiliar usada a *Views\Genre\\_CreateGenre.cshtml* arquivo gera HTML com um atributo de ação que contém a URL para enviar o formulário. Você pode ver isso exibindo a página Criar álbum em um navegador e selecionando Mostrar fonte no navegador. A marcação a seguir mostra o código HTML gerado que contém a marca de formulário.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

O jQuery `$.post` linha faz uma chamada AJAX para o atributo de ação (`/StoreManager/Create`) e passa os dados a partir de **gênero criar** caixa de diálogo. Os dados consistem em nome para o novo gênero e uma descrição opcional. Se a chamada AJAX for bem-sucedida, o novo nome de gênero e valor são adicionados à marcação de Select, e o novo gênero é definido como o valor selecionado. Como isso é gerada dinamicamente marcação, é possível ver a nova opção de selecionar exibindo o código-fonte no navegador. Você pode ver o novo HTML com as ferramentas de desenvolvedor F12 do Internet Explorer 9. Para exibir a nova opção de seleção no Internet Explorer 9, pressione a tecla F12 para iniciar as ferramentas de desenvolvedor F12. Navegue até a página criar e adicionar uma nova forma para que o novo gênero é selecionado na lista de seleção gênero. Nas ferramentas de desenvolvedor F12:

1. Selecione a guia HTML.
2. Clique no ícone de atualização.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Na caixa de pesquisa, digite GenreID.
4. Usando o ícone próximo   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Navegue até a marca de seleção a seguir:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Expanda o último valor de opção.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

O código a seguir no *Scripts\chooseGenre.js* arquivo mostra como o **adicionar novo gênero** botão obtém conectado ao evento de clique e como o **adicionar novo gênero** caixa de diálogo criado.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

A primeira linha cria uma função de clique anexada para o **adicionar novo gênero** botão. A seguinte marcação do Views\StoreManager\\_ChooseGenre.cshtml arquivo mostra como o **adicionar novo gênero** botão é criado:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

O método load cria e abre a caixa de diálogo Adicionar gênero e chama o jQuery `parse` método para validação do cliente ocorra em dados inseridos na caixa de diálogo.

Nesta seção, você aprendeu como criar uma caixa de diálogo que pode ser usada para adicionar novos dados de categoria a uma lista de seleção. Você pode seguir o mesmo procedimento para criar a interface do usuário para adicionar um novo artista à lista de seleção artista. Este tutorial forneceu uma visão geral de como trabalhar com o auxiliar HTML do ASP.NET MVC **DropDownList**. Para obter informações adicionais sobre como trabalhar com o **DropDownList**, consulte a seção de referências de adição abaixo. Informe-nos se este tutorial foi útil.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Referências adicionais

- [ASP.NET MVC – em cascata suspensa lista Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) por [visão de Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Escolhido](http://harvesthq.github.com/chosen/) JavaScript de um plug-in que oferecem suporte a seleção múltipla e filtragem.

### <a name="contributors"></a>Colaboradores

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Revisores

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Anterior](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
