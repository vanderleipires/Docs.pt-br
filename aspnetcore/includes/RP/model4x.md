<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="6d777-101">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="6d777-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="6d777-102">Execute o seguinte na linha de comando (o diretório do projeto que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*):</span><span class="sxs-lookup"><span data-stu-id="6d777-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="6d777-103">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="6d777-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="6d777-104">Abra um Shell de Comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="6d777-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
