Execute o scaffolder de identidade:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* De **Solution Explorer**, com o botão direito no projeto > **adicionar** > **Novo Item de Scaffold**.
* No painel esquerdo do **adicionar Scaffold** caixa de diálogo, selecione **identidade** > **adicionar**.
* No **adicionar identidade** caixa de diálogo, selecione as opções desejadas.
  * Selecione a página de layout existente ou o arquivo de layout será sobrescrito com marcação incorretova. Quando um arquivo layout. cshtml existente é selecionado, é **não** substituído.

 Por exemplo `~/Pages/Shared/_Layout.cshtml` para as páginas Razor `~/Views/Shared/_Layout.cshtml` para projetos MVC
* Para usar o contexto de dados existente, selecione pelo menos um arquivo para substituir. Você deve selecionar pelo menos um arquivo para adicionar seu contexto de dados.
  * Selecione sua classe de contexto de dados.
  * Selecione **adicionar**.
* Para criar um novo contexto de usuário e possivelmente criar uma classe de usuário personalizadas de identidade:
  * Selecione o **+** botão para criar um novo **classe de contexto de dados**.
  * Selecione **adicionar**.

Observação: Se você estiver criando um novo contexto de usuário, você não precisa selecionar um arquivo de substituição.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Se você ainda não tiver instalado o scaffolder ASP.NET, instalá-lo agora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Adicione uma referência de pacote para [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) ao projeto (\*. csproj) arquivos. Execute o seguinte comando no diretório do projeto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Execute o seguinte comando para listar as opções de scaffolder de identidade:

```cli
dotnet aspnet-codegenerator identity -h
```

Na pasta do projeto, execute o scaffolder de identidade com as opções desejadas. Por exemplo, para configurar a identidade com a interface do usuário padrão e o número mínimo de arquivos, execute o seguinte comando. Use o nome totalmente qualificado correto para o contexto de banco de dados:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
