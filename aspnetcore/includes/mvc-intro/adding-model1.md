# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Adicione um modelo a um aplicativo ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Tom Dykstra](https://github.com/tdykstra)

Nesta seção, você adicionará algumas classes para o gerenciamento de filmes em um banco de dados. Essas classes serão a parte “**M**odel” parte do aplicativo **M**VC.

Você usa essas classes com o [EF Core](/ef/core) (Entity Framework Core) para trabalhar com um banco de dados. O EF Core é uma estrutura ORM (mapeamento relacional de objetos) que simplifica o código de acesso a dados que você precisa escrever. [O EF Core dá suporte a vários mecanismos de banco de dados](/ef/core/providers/).

As classes de modelo que serão criadas são conhecidas como classes POCO (de “objetos CLR básicos”) porque elas não têm nenhuma dependência do EF Core. Elas apenas definem as propriedades dos dados que serão armazenados no banco de dados.

Neste tutorial, você escreverá as classes de modelo primeiro e o EF Core criará o banco de dados. Uma abordagem alternativa não abordada aqui é gerar classes de modelo com base em um banco de dados já existente. Para obter informações sobre essa abordagem, consulte [ASP.NET Core – Banco de dados existente](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Adicionar uma classe de modelo de dados
