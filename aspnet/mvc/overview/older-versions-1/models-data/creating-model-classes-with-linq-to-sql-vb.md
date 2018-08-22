---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Criação de Classes de modelo com o LINQ to SQL (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá a compilar modelo c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 168fbb914b54f88a78db63c16b03c55cc59c4a11
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829998"
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>Criação de Classes de modelo com o LINQ to SQL (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como criar classes de modelo e executar o acesso de banco de dados, tirando proveito do Microsoft LINQ to SQL.


O objetivo deste tutorial é explicar um método de criação de classes de modelo para um aplicativo ASP.NET MVC. Neste tutorial, você aprenderá como criar classes de modelo e executar o acesso de banco de dados, tirando proveito do Microsoft LINQ to SQL.

Neste tutorial, vamos criar um aplicativo básico do banco de dados do filme. Vamos começar criando o aplicativo de banco de dados do filme da maneira mais rápida e fácil possível. Podemos executar todos de nosso acesso a dados diretamente do nosso ações do controlador.

Em seguida, você aprenderá como usar o padrão de repositório. Usando o padrão de repositório requer um pouco mais trabalho. No entanto, a vantagem de adotar esse padrão é que ele permite que você crie aplicativos que são adaptáveis para alterar e pode ser facilmente testados.

## <a name="what-is-a-model-class"></a>O que é uma classe de modelo?

Um modelo MVC contém toda a lógica de aplicativo que não está contida em uma exibição do MVC ou controlador MVC. Em particular, um modelo MVC contém todos os seus negócios de aplicativos e a lógica de acesso a dados.

Você pode usar uma variedade de tecnologias diferentes para implementar a lógica de acesso de dados. Por exemplo, você pode criar suas classes de acesso de dados usando as classes do Microsoft Entity Framework, NHibernate, o Subsonic ou ADO.NET.

Neste tutorial, uso o LINQ to SQL para consultar e atualizar o banco de dados. LINQ to SQL oferece um método muito fácil de interagir com um banco de dados do Microsoft SQL Server. No entanto, é importante entender que o ASP.NET MVC framework não está vinculado ao LINQ to SQL de qualquer forma. ASP.NET MVC é compatível com qualquer tecnologia de acesso a dados.

## <a name="create-a-movie-database"></a>Criar um banco de dados do filme

Neste tutorial, para ilustrar como você pode criar classes de modelo – vamos criar um aplicativo de banco de dados de filme simples. A primeira etapa é criar um novo banco de dados. O aplicativo com o botão direito\_pasta de dados na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, Item novo**. Selecione o modelo de banco de dados do SQL Server, atribua o nome MoviesDB.mdf e clique no **adicionar** botão (consulte a Figura 1).


[![Adicionando um novo banco de dados do SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Figura 01**: adicionar um novo banco de dados do SQL Server ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


Depois de criar o novo banco de dados, é possível abrir o banco de dados, duas vezes no arquivo MoviesDB.mdf no aplicativo\_pasta de dados. Duas vezes no arquivo MoviesDB.mdf abre a janela do Gerenciador de servidores (consulte a Figura 2).


|   | A janela do Gerenciador de servidores é chamada a janela Gerenciador de banco de dados ao usar o Visual Web Developer. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![Usando a janela do Gerenciador de servidores](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Figura 02**: usando a janela do Gerenciador de servidores ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


Precisamos adicionar uma tabela ao nosso banco de dados que representa nosso filmes. Clique com botão direito na pasta tabelas e selecione a opção de menu **adicionar nova tabela**. Selecionar essa opção de menu abre o Designer de tabela (veja a Figura 3).


[![Usando a janela do Gerenciador de servidores](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Figura 03**: O Designer de tabela ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


É preciso adicionar as colunas a seguir para nossa tabela de banco de dados:

| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Título | Nvarchar(200) | False |
| Diretor | nvarchar (50) | False |

Você precisa fazer duas coisas especiais para a coluna de Id. Primeiro, você precisa marcar a coluna de Id como uma coluna de chave primária, selecionando a coluna no Designer de tabela e clicando no ícone de uma chave. LINQ to SQL requer que você especifique suas colunas de chave primária quando executar inserções ou atualizações no banco de dados.

Em seguida, você precisará marcar a coluna Id como uma coluna de identidade, atribuindo o valor Sim para o **é identidade** propriedade (veja a Figura 3). Uma coluna de identidade é uma coluna que é atribuída um novo número automaticamente sempre que você adicionar uma nova linha de dados a uma tabela.

Depois de fazer essas alterações, salve a tabela com o nome tblMovie. Você pode salvar a tabela clicando no botão Salvar.

## <a name="create-linq-to-sql-classes"></a>Criar Classes LINQ to SQL

Nosso modelo MVC conterá o LINQ para SQL classes que representam a tabela de banco de dados tblMovie. A maneira mais fácil de criar essas classes LINQ to SQL é com o botão direito na pasta modelos, selecione **Add, o novo Item**, selecione o LINQ para o modelo de Classes do SQL, nomeie as classes de Movie.dbml e clique no **Add**botão (consulte a Figura 4).


[![Criando o LINQ para classes SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Figura 04**: Criando classes LINQ to SQL ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


Imediatamente depois de criar o filme Classes LINQ to SQL, o Object Relational Designer é exibido. Você pode arrastar tabelas de banco de dados da janela Gerenciador de servidores para o Object Relational Designer para criar Classes LINQ to SQL que representam as tabelas de banco de dados específico. Precisamos adicionar a tabela de banco de dados de tblMovie em Object Relational Designer (consulte a Figura 4).


[![Usando o Object Relational Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Figura 05**: usando o Object Relational Designer ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


Por padrão, o Object Relational Designer cria uma classe com o mesmo nome como a tabela de banco de dados que você arrasta para o Designer. No entanto, não queremos chamar tblMovie nossa classe. Portanto, clique no nome da classe no Designer e altere o nome da classe para filme.

Por fim, lembre-se de clicar o **salvar** botão (a imagem de disquete) para salvar o LINQ to SQL Classes. Caso contrário, a Classes LINQ to SQL não gerado pelo Designer relacional de objeto.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Usando LINQ to SQL em uma ação do controlador

Agora que temos nosso classes LINQ to SQL, podemos usar essas classes para recuperar dados do banco de dados. Nesta seção, você aprenderá a usar LINQ to SQL classes diretamente dentro de uma ação do controlador. Podemos vai exibir a lista de filmes da tabela de banco de dados tblMovies em uma exibição do MVC.

Primeiro, precisamos modificar a classe HomeController. Essa classe pode ser encontrada na pasta controladores do seu aplicativo. Modifique a classe para que ele se parece com a classe na listagem 1.

**Listagem 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

A ação Index () na listagem 1 usa uma classe LINQ to SQL DataContext (o MovieDataContext) para representar o banco de dados MoviesDB. A classe MoveDataContext foi gerada pelo Visual Studio o Object Relational Designer.

Uma consulta LINQ é executada em relação a DataContext para recuperar todos os filmes da tabela de banco de dados tblMovies. A lista de filmes é atribuída a uma variável local denominada filmes. Por fim, a lista de filmes é passada para o modo de exibição por meio de exibir dados.

Para mostrar os filmes, em seguida, precisamos modificar a exibição de índice. Você pode encontrar a exibição de índice na pasta Views\Home\. Atualize a exibição de índice para que ele se parece com o modo de exibição na listagem 2.

**Listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Observe que o modo de exibição do índice modificado inclui um &lt;% @ import namespace %&gt; diretiva na parte superior do modo de exibição. Essa diretiva importa o namespace MvcApplication1. Precisamos esse namespace para trabalhar com as classes de modelo – em particular, a classe Movie – no modo de exibição.

A exibição na listagem 2 contém uma para cada loop que itera em todos os itens, representados pela propriedade ViewData.Model. O valor da propriedade de título é exibido para cada filme.

Observe que o valor da propriedade ViewData.Model é convertido em um IEnumerable. Isso é necessário para executar um loop pelo conteúdo da ViewData.Model. Outra opção aqui é criar uma exibição fortemente tipada. Quando você cria uma exibição fortemente tipado, você converter a propriedade de ViewData.Model para um tipo específico na classe code-behind de uma exibição.

Se você executar o aplicativo depois de modificar a classe HomeController e a exibição de índice, você obterá uma página em branco. Você obterá uma página em branco porque não há nenhum registro de filme na tabela de banco de dados tblMovies.

Para adicionar registros à tabela de banco de dados tblMovies, a tabela de banco de dados tblMovies na janela Gerenciador de servidores (janela do Gerenciador de banco de dados no Visual Web Developer) com o botão direito e selecione a opção de menu **Mostrar dados da tabela**. Você pode inserir registros de filme usando a grade que aparece (veja a Figura 5).


[![Inserção de filmes](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Figura 06**: a inserção de filmes ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


Depois de adicionar alguns registros de banco de dados à tabela tblMovies e executar o aplicativo, você verá a página na Figura 7. Todos os registros de banco de dados do filme são exibidos em uma lista com marcadores.


[![Exibição de filmes com o modo de exibição de índice](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Figura 07**: exibir filmes com o modo de exibição de índice ([clique para exibir a imagem em tamanho normal](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Usando o padrão de repositório

Na seção anterior, usamos o LINQ para SQL classes diretamente dentro de uma ação do controlador. Nós usamos a classe MovieDataContext diretamente da ação de controlador de Index (). Não há nada de errado com isso no caso de um aplicativo simples. No entanto, trabalhar diretamente com o LINQ to SQL em uma classe de controlador cria problemas quando você precisa para criar um aplicativo mais complexo.

Usando LINQ to SQL dentro de uma classe de controlador torna difícil alternar as tecnologias de acesso no futuro. Por exemplo, você pode decidir alternar entre usar o Microsoft LINQ to SQL usando o Entity Framework da Microsoft como sua tecnologia de acesso a dados. Nesse caso, você precisaria reescrever cada controlador que acessa o banco de dados dentro de seu aplicativo.

Usando LINQ to SQL dentro de uma classe de controlador também torna difíceis de criar testes de unidade para seu aplicativo. Normalmente, você não deseja interagir com um banco de dados ao executar testes de unidade. Você deseja usar os testes de unidade para testar a lógica do aplicativo e não o servidor de banco de dados.

A fim de criar um aplicativo MVC, que é mais adaptável para futuras alterações e que podem ser testados com mais facilidade, você deve considerar usar o padrão de repositório. Quando você usa o padrão de repositório, você pode criar uma classe de repositório separado que contém todos os de sua lógica de acesso do banco de dados.

Quando você cria a classe de repositório, você pode criar uma interface que representa todos os métodos usados pela classe de repositório. Dentro dos controladores, você escreve seu código com base na interface em vez de no repositório. Dessa forma, você pode implementar o repositório usando tecnologias de acesso a dados diferentes no futuro.

A interface na listagem 3 é denominada IMovieRepository e representa um único método chamado ListAll().

**Listagem 3 – `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

A classe de repositório na listagem 4 implementa a interface IMovieRepository. Observe que ele contém um método chamado ListAll() que corresponde ao método exigido pela interface IMovieRepository.

**Listagem 4 – `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Por fim, a classe MoviesController na listagem 5 usa o padrão de repositório. Ele não usa mais LINQ para classes SQL diretamente.

**Listagem 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Observe que a classe MoviesController na listagem 5 tem dois construtores. O primeiro construtor, o construtor sem parâmetros, é chamado quando o aplicativo é executado. Este construtor cria uma instância da classe MovieRepository e passa-o para o segundo construtor.

O segundo construtor tem um único parâmetro: um parâmetro IMovieRepository. Esse construtor simplesmente atribui o valor do parâmetro a um campo de nível de classe chamado \_repositório.

A classe MoviesController está aproveitando um padrão de design de software chamado o padrão de injeção de dependência. Em particular, ele está usando algo chamado injeção de dependência de construtor. Você pode ler mais sobre esse padrão, lendo o artigo a seguir por Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Observe que todo o código na classe MoviesController (exceto o primeiro construtor) interage com a interface IMovieRepository em vez da classe MovieRepository real. O código interage com uma interface abstrata, em vez de uma implementação concreta da interface.

Se você quiser modificar a tecnologia de acesso de dados usada pelo aplicativo, em seguida, você pode simplesmente implementar a interface de IMovieRepository com uma classe que usa a tecnologia de acesso alternativo do banco de dados. Por exemplo, você poderia criar uma classe de EntityFrameworkMovieRepository ou uma classe SubSonicMovieRepository. Como a classe de controlador é programada com base na interface, você pode passar uma nova implementação de IMovieRepository à classe do controlador e a classe continuará a funcionar.

Além disso, se você quiser testar a classe MoviesController, você pode passar uma classe de repositório de filme falso para o MoviesController. Você pode implementar a classe IMovieRepository com uma classe que não acessa o banco de dados, na verdade, mas contém todos os métodos da interface IMovieRepository necessários. Dessa forma, você pode testar a unidade a classe MoviesController sem, na verdade, acessar um banco de dados real.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era demonstrar como você pode criar classes de modelo do MVC, tirando proveito do Microsoft LINQ to SQL. Examinamos duas estratégias para exibir dados do banco de dados em um aplicativo ASP.NET MVC. Primeiro, criamos LINQ para classes SQL e usamos as classes diretamente dentro de uma ação do controlador. Usando o LINQ para classes SQL dentro de um controlador permite que você rapidamente e facilmente exibir dados de banco de dados em um aplicativo MVC.

Em seguida, exploramos um caminho um pouco mais difícil, mas definitivamente mais virtuoso, para exibir os dados do banco de dados. Podemos aproveitou o padrão de repositório e colocado todos nossa lógica de acesso do banco de dados em uma classe de repositório separado. Em nosso controller, escrevemos todo o nosso código em relação a uma interface em vez de uma classe concreta. A vantagem do padrão de repositório é que ele nos permite alterar facilmente as tecnologias de acesso de banco de dados no futuro, e ele nos permite testar facilmente a nossas classes de controlador.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-the-entity-framework-vb.md)
> [Próximo](displaying-a-table-of-database-data-vb.md)
