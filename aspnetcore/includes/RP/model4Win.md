<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="35e9c-101">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="35e9c-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="35e9c-102">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="35e9c-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="35e9c-103">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="35e9c-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="35e9c-104">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="35e9c-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="35e9c-105">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="35e9c-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="35e9c-106">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="35e9c-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="35e9c-107">Saia do Visual Studio e execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="35e9c-107">Exit Visual Studio and run the command again.</span></span>