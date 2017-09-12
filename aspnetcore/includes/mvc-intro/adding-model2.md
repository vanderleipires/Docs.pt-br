## <a name="add-initial-migration-and-update-the-database"></a>Adicionar a migração inicial e atualizar o banco de dados

* Abra um prompt de comando e navegue até o diretório do projeto. (O diretório que contém o *Startup.cs* arquivo).

* Execute os seguintes comandos no prompt de comando:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](https://docs.microsoft.com/dotnet/core/tools/index) é uma implementação de plataforma cruzada do .NET. Aqui está o que fazem estes comandos:

  * `dotnet restore`: Baixa os pacotes do NuGet especificados no *. csproj* arquivo.
  * `dotnet ef migrations add Initial`Executa o comando de migrações de CLI do Entity Framework .NET Core e cria a migração inicial. O parâmetro depois de "Adicionar" é um nome que você atribui para a migração. Aqui você está nomeando a migração "Inicial" porque ele é a migração de banco de dados inicial. Essa operação cria o */migrações de dados/\<data e hora > _Initial.cs* arquivo que contém os comandos de migração para adicionar o *filme* tabela no banco de dados.
  * `dotnet ef database update`Atualiza o banco de dados com a migração que acabamos de criar.

Você saberá mais sobre a cadeia de caracteres de conexão e de banco de dados do tutorial Avançar. Você saberá mais sobre as alterações do modelo de dados no [adicionar um campo](xref:tutorials/first-mvc-app/new-field) tutorial.
