# <a name="how-to-buildrun-secure-user-data-sample"></a>Como exemplo de dados de usuário segura Compile/execute

* Definir a senha com a ferramenta Gerenciador de segredo:

  `dotnet user-secrets set SeedUserPW <pw>`

* Atualize o banco de dados:

    `dotnet ef database update`
