<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Fazer scaffolding do modelo de filme

* Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).
* Execute o seguinte comando:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
Se você obtiver o erro:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).