<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="0bcbd-101">Fazer scaffolding do modelo de filme</span><span class="sxs-lookup"><span data-stu-id="0bcbd-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="0bcbd-102">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="0bcbd-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0bcbd-103">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="0bcbd-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="0bcbd-104">Se você obtiver o erro:</span><span class="sxs-lookup"><span data-stu-id="0bcbd-104">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="0bcbd-105">Saia do Visual Studio e execute o comando novamente.</span><span class="sxs-lookup"><span data-stu-id="0bcbd-105">Exit Visual Studio and run the command again.</span></span>