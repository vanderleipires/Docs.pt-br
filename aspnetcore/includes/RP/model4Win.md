<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Fazer scaffolding do modelo de filme

* Execute o seguinte na linha de comando (o diretório do projeto que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*):

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Se você obtiver o erro:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

O erro anterior ocorre quando você está no diretório errado. Abra um shell de comando para o diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*) e, em seguida, execute o comando anterior.

Se você obtiver o erro:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Saia do Visual Studio e execute o comando novamente.
