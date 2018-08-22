---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Criar uma API REST com roteamento de atributo na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: da3ca8f89f823fcb2c4ab74af6ddf4f61d4e663a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825415"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Criar uma API REST com roteamento de atributo na API Web ASP.NET 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

API Web 2 dá suporte a um novo tipo de roteamento, chamado *roteamento de atributo*. Para obter uma visão geral de roteamento de atributo, consulte [roteamento de atributo na API Web 2](attribute-routing-in-web-api-2.md). Neste tutorial, você usará o roteamento de atributo para criar uma API REST para uma coleção de livros. A API é compatível com as seguintes ações:

| Ação | Exemplo de URI |
| --- | --- |
| Obtenha uma lista de todos os livros. | / api/manuais |
| Obtenha um livro por ID. | /API/Books/1 |
| Obtenha os detalhes de um livro. | /API/Books/1/Details |
| Obtenha uma lista de livros por gênero. | /API/Books/fantasy |
| Obter uma lista de livros por data de publicação. | /API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (formulário alternativo) |
| Obtenha uma lista de livros publicados por um autor específico. | /API/Authors/1/Books |

Todos os métodos são somente leitura (solicitações HTTP GET).

Para a camada de dados, vamos usar Entity Framework. Registros de livro terão os seguintes campos:

- ID
- Título
- Gênero
- Data de publicação
- Preço
- Descrição
- AuthorID (chave estrangeira em uma tabela de autores)

Para a maioria das solicitações, no entanto, a API retornará um subconjunto desses dados (título, autor e gênero). Para obter o registro completo, o cliente solicitações `/api/books/{id}/details`.

## <a name="prerequisites"></a>Pré-requisitos

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Comece executando o Visual Studio. Dos **arquivo** menu, selecione **New** e, em seguida, selecione **projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**. Em "Adicionar pastas e core references for", selecione a **API Web** caixa de seleção. Clique em **criar projeto**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Isso cria um projeto de esqueleto está configurado para a funcionalidade da API da Web.

### <a name="domain-models"></a>Modelos de domínio

Em seguida, adicione classes para modelos de domínio. No Gerenciador de Soluções, clique com o botão direito na pasta de modelos (Models). Selecione **Add**, em seguida, selecione **classe**. Nomeie a classe `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Substitua o código no Author.cs com o seguinte:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Agora, adicione outra classe denominada `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Adicionar um controlador de API da Web

Nesta etapa, vamos adicionar um controlador de API da Web que usa o Entity Framework como a camada de dados.

Pressione CTRL+SHIFT+B para criar o projeto. Entity Framework usa a reflexão para descobrir as propriedades dos modelos, portanto, ele exige um assembly compilado criar o esquema de banco de dados.

No Gerenciador de soluções, clique com botão direito na pasta controladores. Selecione **Add**, em seguida, selecione **controlador**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

No **adicionar Scaffold** caixa de diálogo, selecione "Web API 2 Controller com ações de leitura/gravação, usando o Entity Framework."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

No **Adicionar controlador** caixa de diálogo, para **nome do controlador**, insira &quot;BooksController&quot;. Selecione o &quot;usar ações do controlador assíncrono&quot; caixa de seleção. Para **classe de modelo**, selecione &quot;livro&quot;. (Se você não vir o `Book` classe listada na lista suspensa, certifique-se de que você compilou o projeto.) Em seguida, clique no botão "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Clique em **Add** na **novo contexto de dados** caixa de diálogo.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Clique em **Add** na **Adicionar controlador** caixa de diálogo. O scaffolding adiciona uma classe chamada `BooksController` que define o controlador da API. Ele também adiciona uma classe chamada `BooksAPIContext` na pasta modelos, que define o contexto de dados para o Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Propagar o banco de dados

No menu Ferramentas, selecione **Gerenciador de pacotes de biblioteca**e, em seguida, selecione **Package Manager Console**.

Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Este comando cria uma pasta migrações e adiciona um novo arquivo de código chamado Configuration.cs. Abra esse arquivo e adicione o seguinte código para o `Configuration.Seed` método.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

Na janela do Console do Gerenciador de pacotes, digite os comandos a seguir.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Esses comandos criam um banco de dados local e invocar o método de semente para preencher o banco de dados.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Adicionar Classes DTO

Se você executa o aplicativo agora e envia uma solicitação GET para /api/books/1, a resposta é semelhante ao seguinte. (Eu adicionei o recuo para facilitar a leitura).

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Em vez disso, quero que esta solicitação para retornar um subconjunto dos campos. Além disso, quero que ela retorne o nome do autor, em vez da ID do autor. Para fazer isso, vamos modificar os métodos do controlador para retornar um *objeto de transferência de dados* (DTO) em vez do modelo do EF. Um DTO é um objeto que foi criado apenas para transportar dados.

No Gerenciador de soluções, clique com botão direito no projeto e selecione **Add** | **nova pasta**. Nomeie a pasta &quot;DTOs&quot;. Adicione uma classe chamada `BookDto` para a pasta de DTOs, com a seguinte definição:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Adicione outra classe denominada `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Em seguida, atualize o `BooksController` classe para retornar `BookDto` instâncias. Vamos usar o [Queryable](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) método ao projeto `Book` para instâncias `BookDto` instâncias. Aqui está o código atualizado para a classe de controlador.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Exclui o `PutBook`, `PostBook`, e `DeleteBook` métodos, porque eles não são necessários para este tutorial.


Agora, se você executa o aplicativo e solicitar /api/books/1, o corpo da resposta deve ser assim:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Adicionar atributos de rota

Em seguida, converteremos o controlador para usar o roteamento de atributo. Primeiro, adicione uma **RoutePrefix** de atributo para o controlador. Este atributo define os segmentos URI inicias para todos os métodos neste controlador.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Em seguida, adicione **[Route]** atributos para as ações do controlador, da seguinte maneira:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

O modelo de rota para cada método de controlador é o prefixo mais a cadeia de caracteres especificada na **rota** atributo. Para o `GetBook` a cadeia de caracteres com parâmetros inclui o método, o modelo de rota &quot;{id: int}&quot;, que corresponde a se o segmento URI contém um valor inteiro.

| Método | Modelo de rota | Exemplo de URI |
| --- | --- | --- |
| `GetBooks` | "api/livros" | `http://localhost/api/books` |
| `GetBook` | "api/manuais / {id: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Obter os detalhes do livro

Para obter detalhes do livro, o cliente enviará uma solicitação GET para `/api/books/{id}/details`, onde *{id}* é a ID do livro.

Adicione o seguinte método à classe `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Se você solicitar `/api/books/1/details`, a resposta tem esta aparência:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Obter livros por gênero

Para obter uma lista de livros em um gênero específico, o cliente enviará uma solicitação GET para `/api/books/genre`, onde *gênero* é o nome do gênero. (Por exemplo, `/api/books/fantasy`.)

Adicione o seguinte método para `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Aqui definimos uma rota que contém um parâmetro {gênero} no modelo de URI. Observe que a API da Web é capaz de distinguir esses dois URIs e roteá-las para métodos diferentes:

`/api/books/1`

`/api/books/fantasy`

Isso ocorre porque o `GetBook` método inclui uma restrição de que o segmento "id" deve ser um valor inteiro:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Se você solicitar /api/books/fantasy, a resposta terá esta aparência:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Obter livros por autor

Para obter uma lista de livros de um para um autor específico, o cliente enviará uma solicitação GET para `/api/authors/id/books`, onde *id* é a ID do autor.

Adicione o seguinte método para `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

Este exemplo é interessante porque &quot;livros&quot; é tratado de um recurso filho de &quot;autores&quot;. Esse padrão é muito comum nas APIs RESTful.

O til (~) no modelo de rota substitui o prefixo da rota na **RoutePrefix** atributo.

## <a name="get-books-by-publication-date"></a>Obter livros por data de publicação

Para obter uma lista de livros por data de publicação, o cliente enviará uma solicitação GET para `/api/books/date/yyyy-mm-dd`, onde *aaaa-mm-dd* é a data.

Aqui está uma maneira de fazer isso:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

O `{pubdate:datetime}` parâmetro é restrito para corresponder a um **DateTime** valor. Isso funciona, mas é realmente mais permissivo do que gostaríamos. Por exemplo, esses URIs também serão compatíveis com a rota:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Não há nada de errado com permitindo que esses URIs. No entanto, você pode restringir a rota para um determinado formato, adicionando uma restrição de expressão regular para o modelo de rota:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Agora apenas as datas no formato &quot;aaaa-mm-dd&quot; fará a correspondência. Observe que não usamos a expressão regular para validar que temos uma data real. Isso é feito quando a API da Web tenta converter o segmento do URI em uma **DateTime** instância. Uma data inválida, como ' 2012-47-99' falhará a ser convertido e o cliente receberá um erro 404.

Você também pode dar suporte a um separador de barra "/" (`/api/books/date/yyyy/mm/dd`), adicionando outro **[Route]** atributo com um regex diferente.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Há um detalhe sutil mas importante aqui. O segundo modelo de rota tem um caractere curinga (\*) no início do parâmetro {pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Isso informa ao mecanismo de roteamento que {pubdate} deve coincidir com o restante do URI. Por padrão, um parâmetro de modelo corresponde a um único segmento do URI. Nesse caso, queremos {pubdate} abrangem vários segmentos URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Código do controlador

Aqui está o código completo para a classe BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Resumo

Roteamento de atributo fornece mais controle e maior flexibilidade ao projetar os URIs para sua API.
