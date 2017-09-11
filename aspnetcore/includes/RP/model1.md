Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Nesta seção, você adiciona classes para gerenciamento de filmes em um banco de dados. Você usa essas classes com o [Entity Framework Core](https://docs.microsoft.com/ef/core) (EF Core) para trabalhar com um banco de dados. O EF Core é uma estrutura ORM (de mapeamento relacional de objetos) que simplifica o código de acesso a dados que você precisa escrever.

As classes de modelo que você cria são conhecidas como classes de dados POCO (de "objetos CLR básicos") porque elas não têm nenhuma dependência do EF Core. Elas definem as propriedades dos dados que são armazenados no banco de dados.

Neste tutorial, você escreve as classes de modelo primeiro e o EF Core cria o banco de dados. Uma abordagem alternativa não abordada aqui é [gerar classes de modelo de um banco de dados existente](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

[Exiba ou baixe](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) a amostra.