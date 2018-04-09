---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Criando Classes de modelo com LINQ to SQL (c#) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá a criar modelo c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f30ff6c97e565059c57c55f26d2dff477774aeb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Criando Classes de modelo com LINQ to SQL (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como criar classes de modelo e executam acesso a banco de dados, aproveitando a Microsoft LINQ to SQL.


O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como criar classes de modelo e executam acesso a banco de dados, aproveitando a Microsoft LINQ to SQL

Neste tutorial, vamos criar um aplicativo de banco de dados básico do filme. Vamos começar criando o aplicativo de banco de dados do filme da maneira mais fácil e mais rápido possível. Podemos executar todos os nossos acesso a dados diretamente do nosso ações do controlador.

Em seguida, você aprenderá a usar o padrão de repositório. Usando o padrão de repositório exige um pouco mais trabalho. No entanto, a vantagem de adoção esse padrão é que ele permite que você crie aplicativos que são adaptáveis para alterar e podem ser facilmente testados.

## <a name="what-is-a-model-class"></a>O que é uma classe de modelo?

Um modelo MVC contém toda a lógica de aplicativo que não está contida em um modo de exibição do MVC ou controlador MVC. Em particular, um modelo MVC contém todos os seus negócios de aplicativos e a lógica de acesso a dados.

Você pode usar uma variedade de tecnologias diferentes para implementar a lógica de acesso de dados. Por exemplo, você pode criar classes de acesso a dados usando as classes do Microsoft Entity Framework, NHibernate, Subsonic ou ADO.NET.

Neste tutorial, posso usar o LINQ to SQL para consultar e atualizar o banco de dados. O LINQ to SQL fornece um método fácil de interagir com um banco de dados do Microsoft SQL Server. No entanto, é importante entender que a estrutura ASP.NET MVC não está associada ao LINQ to SQL de forma alguma. ASP.NET MVC é compatível com qualquer tecnologia de acesso a dados.

## <a name="create-a-movie-database"></a>Criar um banco de dados do filme

Neste tutorial – para ilustrar como você pode criar classes de modelo – vamos criar um aplicativo simples de banco de dados do filme. A primeira etapa é criar um novo banco de dados. Clique com botão direito do aplicativo\_pasta de dados na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, o novo Item**. Selecione o **o banco de dados do SQL Server** modelo, dê a ele o nome MoviesDB.mdf e clique no **adicionar** botão (consulte a Figura 1).


[![Adicionando um novo banco de dados do SQL Server](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Figura 01**: adicionando um novo banco de dados do SQL Server ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Depois de criar o novo banco de dados, você pode abrir o banco de dados duas vezes no arquivo MoviesDB.mdf no aplicativo\_pasta de dados. Duas vezes no arquivo MoviesDB.mdf abre a janela do Gerenciador de servidores (consulte a Figura 2).

A janela Gerenciador de servidores é chamada a janela Gerenciador de banco de dados ao usar o Visual Web Developer.


[![Usando a janela Gerenciador de servidores](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Figura 02**: usando a janela Gerenciador de servidores ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


É preciso adicionar uma tabela ao nosso banco de dados que representa o nosso filmes. Clique na pasta de tabelas e selecione a opção de menu **adicionar nova tabela**. Selecionar essa opção de menu abre o Designer de tabela (consulte a Figura 3).


[![Usando a janela Gerenciador de servidores](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Figura 03**: O Designer de tabela ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


É necessário adicionar as colunas a seguir à nossa tabela de banco de dados:

| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Título | Nvarchar(200) | False |
| Diretor | Nvarchar(50) | False |

Você precisa fazer duas coisas especiais para a coluna de Id. Primeiro, você precisa marcar a coluna Id como uma coluna de chave primária, selecione a coluna no Designer de tabela e clicando no ícone de uma chave. O LINQ to SQL requer que você especificar colunas de chave primária ao executar insere ou atualiza o banco de dados.

Em seguida, você precisa marcar a coluna Id como uma coluna de identidade, atribuindo o valor Sim para o **é identidade** propriedade (consulte a Figura 3). Uma coluna de identidade é uma coluna que é atribuída um novo número automaticamente sempre que você adicionar uma nova linha de dados em uma tabela.

## <a name="create-linq-to-sql-classes"></a>Criar Classes LINQ to SQL

Nosso modelo MVC conterá LINQ para SQL classes que representam a tabela de banco de dados tblMovie. É a maneira mais fácil de criar essas classes LINQ to SQL com o botão direito na pasta modelos, selecione **adicionar, o novo Item**, selecione o LINQ para o modelo de Classes do SQL, nomeie as classes de Movie.dbml e clique no **adicionar**botão (consulte a Figura 4).


[![Criando LINQ para classes do SQL](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Figura 04**: Criando classes LINQ to SQL ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Imediatamente depois de criar o filme Classes LINQ to SQL, o Object Relational Designer é exibido. Você pode arrastar tabelas de banco de dados da janela Gerenciador de servidores para o Object Relational Designer para criar Classes LINQ to SQL que representam as tabelas de banco de dados específico. É preciso adicionar a tabela de banco de dados tblMovie para Object Relational Designer (consulte a Figura 5).


[![Usando o Object Relational Designer](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Figura 05**: usando o Object Relational Designer ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Por padrão, o Object Relational Designer cria uma classe com o mesmo nome como a tabela de banco de dados que você arrastar para o Designer. No entanto, nós não desejamos chamar nossa classe `tblMovie`. Portanto, clique no nome da classe no Designer e altere o nome da classe para filme.

Por fim, lembre-se de clicar no **salvar** botão (a imagem de disquete) para salvar o LINQ to SQL Classes. Caso contrário, a Classes LINQ to SQL não ser gerado por Object Relational Designer.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Usando o LINQ to SQL em uma ação de controlador

Agora que temos nosso classes LINQ to SQL, podemos usar essas classes para recuperar dados do banco de dados. Nesta seção, você aprenderá como usar o LINQ para classes SQL diretamente dentro de uma ação do controlador. Podemos exibirá a lista de filmes da tabela de banco de dados tblMovies em uma exibição do MVC.

Primeiro, é preciso modificar a classe HomeController. Essa classe pode ser encontrada na pasta controladores do seu aplicativo. Modifique a classe para que ele se parece com a classe na listagem 1.

**Listando 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

O `Index()` ação na listagem 1 usa uma classe LINQ to SQL DataContext (o `MovieDataContext`) para representar o `MoviesDB` banco de dados. O `MoveDataContext` classe foi gerada pelo Visual Studio Object Relational Designer.

Uma consulta LINQ é executada no DataContext para recuperar todos os filmes do `tblMovies` tabela de banco de dados. A lista de filmes é atribuída a uma variável local chamada `movies`. Por fim, a lista de filmes é passada para o modo de exibição por meio de exibir dados.

Para mostrar filmes, em seguida, é preciso modificar a exibição do índice. Você pode encontrar a exibição do índice no `Views\Home\` pasta. Atualize a exibição do índice para que ele se parece com o modo de exibição na listagem 2.

**A listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Observe que a exibição do índice modificada inclui um `<%@ import namespace %>` diretiva na parte superior do modo de exibição. Essa diretiva importa o `MvcApplication1.Models namespace`. Precisamos esse namespace para trabalhar com o `model` classes – em particular, o `Movie` classe – no modo de exibição.

O modo de exibição na lista 2 contém um `foreach` loop itera em todos os itens, representados pelo `ViewData.Model` propriedade. O valor de `Title` propriedade é exibida para cada `movie`.

Observe que o valor de `ViewData.Model` propriedade é convertida em um `IEnumerable`. Isso é necessário para percorrer o conteúdo de `ViewData.Model`. Outra opção é criar fortemente tipado `view`. Quando você cria um tipo mais acentuado `view`, converter o `ViewData.Model` propriedade a um determinado tipo de classe de code-behind do modo de exibição.

Se você executar o aplicativo depois de modificar o `HomeController` classe e o índice de exibição, em seguida, você obterá uma página em branco. Você obterá uma página em branco porque não há nenhum registro filme o `tblMovies` tabela de banco de dados.

Para adicionar registros a `tblMovies` tabela de banco de dados, clique com botão direito do `tblMovies` tabela na janela do Gerenciador de servidores (janela do Pesquisador de objetos de banco de dados no Visual Web Developer) do banco de dados e selecione a opção de menu Mostrar dados da tabela. Você pode inserir `movie` registros usando a grade que aparece (veja a Figura 6).


[![Inserção de filmes](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Figura 06**: inserindo filmes ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Depois de adicionar alguns registros do banco de dados para o `tblMovies` tabela e você executar o aplicativo, você verá a página na Figura 7. Todos os registros de banco de dados do filme são exibidos em uma lista com marcadores.


[![Exibindo filmes com a exibição do índice](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Figura 07**: exibindo filmes com a exibição do índice ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Usando o padrão de repositório

Na seção anterior, usamos o LINQ para classes SQL diretamente dentro de uma ação do controlador. Usamos o `MovieDataContex` classe diretamente a partir de `Index()` ação do controlador. Não há nada errado com isso no caso de um aplicativo simples. No entanto, trabalhar diretamente com o LINQ to SQL em uma classe de controlador cria problemas quando você precisa criar um aplicativo mais complexo.

Usando o LINQ to SQL dentro de uma classe de controlador dificulta alternar tecnologias de acesso a dados no futuro. Por exemplo, você pode decidir usar o Microsoft LINQ to SQL usando o Entity Framework da Microsoft como sua tecnologia de acesso a dados. Nesse caso, seria necessário reescrever todos os controladores que acessa o banco de dados dentro de seu aplicativo.

Usando o LINQ to SQL dentro de uma classe do controlador também torna difícil criar testes de unidade para seu aplicativo. Normalmente, você não deseja interagir com um banco de dados ao executar testes de unidade. Você deseja usar os testes de unidade para testar a lógica do aplicativo e não o servidor de banco de dados.

Para criar um aplicativo MVC mais adaptáveis a futura alteração e que podem ser testadas mais facilmente, considere usar o padrão de repositório. Quando você usa o padrão de repositório, você cria uma classe de repositório separado que contém toda a lógica de acesso de banco de dados.

Quando você cria a classe de repositório, você pode criar uma interface que representa todos os métodos usados pela classe repositório. Dentro de seus controladores, você escreve seu código em relação a interface em vez de repositório. Dessa forma, você pode implementar o repositório usando tecnologias de acesso a dados diferentes no futuro.

A interface na listagem 3 é denominada `IMovieRepository` e representa um único método chamado `ListAll()`.

**A listagem 3 – `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

A classe do repositório na listagem 4 implementa o `IMovieRepository` interface. Observe que ela contém um método chamado `ListAll()` que corresponde ao método necessário para o `IMovieRepository` interface.

**A listagem 4 – `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Por fim, o `MoviesController` classe na listagem 5 usa o padrão de repositório. Ele não usa LINQ para classes SQL diretamente.

**Listando 5 – `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Observe que o `MoviesController` classe na listagem 5 tem dois construtores. O primeiro construtor, o construtor sem parâmetros, é chamado quando o aplicativo é executado. Este construtor cria uma instância do `MovieRepository` classe e o transmite para o construtor de segundo.

O segundo construtor tem um único parâmetro: uma `IMovieRepository` parâmetro. Esse construtor simplesmente atribui o valor do parâmetro para um campo de nível de classe chamado `_repository`.

O `MoviesController` classe está aproveitando as vantagens de um padrão de design de software chamado o padrão de injeção de dependência. Em particular, ele está usando algo chamado injeção de dependência de construtor. Você pode ler mais sobre esse padrão ao ler o seguinte artigo por Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Observe que todo o código no `MoviesController` classe (com exceção do primeiro construtor) interage com o `IMovieRepository` interface em vez de real `MovieRepository` classe. O código interage com uma interface abstrata em vez de uma implementação concreta da interface.

Se você deseja modificar a tecnologia de acesso de dados usada pelo aplicativo, você pode simplesmente implementar o `IMovieRepository` interface com uma classe que usa a tecnologia de acesso alternativo do banco de dados. Por exemplo, você pode criar um `EntityFrameworkMovieRepository` classe ou um `SubSonicMovieRepository` classe. Porque a classe do controlador é programada em relação a interface, você pode passar uma nova implementação de `IMovieRepository` para o controlador de classe e a classe continuará a funcionar.

Além disso, se você quiser testar o `MoviesController` classe, em seguida, você pode passar uma classe de repositório de filme falsa para o `HomeController`. Você pode implementar o `IMovieRepository` classe com uma classe que não tem acesso, na verdade, o banco de dados, mas contém todos os métodos necessários do `IMovieRepository` interface. Dessa forma, é possível que o teste de unidade de `MoviesController` classe sem realmente acessar um banco de dados real.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era demonstrar como você pode criar classes de modelo MVC aproveitando Microsoft LINQ to SQL. Examinamos duas estratégias para exibir dados de banco de dados em um aplicativo ASP.NET MVC. Primeiro, é criado LINQ para classes do SQL e de classes usadas diretamente dentro de uma ação do controlador. Usando o LINQ para classes SQL dentro de um controlador permite que você rapidamente e facilmente exibir dados de banco de dados em um aplicativo MVC.

Em seguida, vamos explorou um caminho um pouco mais difícil, mas definitivamente mais virtuoso, para exibir dados de banco de dados. Estamos aproveitou do padrão de repositório e colocado todos nossa lógica de acesso de banco de dados em uma classe de repositório separado. Em nosso controlador escrevemos todo nosso código em relação a uma interface em vez de uma classe concreta. A vantagem do padrão de repositório é que ele nos permite alterar facilmente as tecnologias de acesso de banco de dados no futuro e ele nos permite facilmente nossos classes do controlador de teste.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-the-entity-framework-cs.md)
> [Próximo](displaying-a-table-of-database-data-cs.md)
