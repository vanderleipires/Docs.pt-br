<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="072f0-101">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="072f0-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="072f0-102">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="072f0-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="072f0-103">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="072f0-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```