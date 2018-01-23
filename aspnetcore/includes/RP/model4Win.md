<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="d19ce-101">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="d19ce-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="d19ce-102">Execute o seguinte na linha de comando (o diretório do projeto que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*):</span><span class="sxs-lookup"><span data-stu-id="d19ce-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="d19ce-103">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="d19ce-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="d19ce-104">Abra um Shell de Comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="d19ce-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="d19ce-105">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="d19ce-105">If you get the error:</span></span>
  ```
  The process cannot access the file
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll'
  because it is being used by another process.
  ```

<span data-ttu-id="d19ce-106">Saia do Visual Studio e execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="d19ce-106">Exit Visual Studio and run the command again.</span></span>
