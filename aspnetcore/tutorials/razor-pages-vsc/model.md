---
title: "Adicionando um modelo para um aplicativo de Páginas do Razor com o Visual Studio para Mac"
author: rick-anderson
description: "Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core usando o Visual Studio para Mac"
keywords: "ASP.NET Core, Páginas do Razor, Razor, MVC, modelo"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 39b069f8c81ca9460abc33b32b0bcc27118939cb
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Adicionando um modelo para um aplicativo de Páginas do Razor no ASP.NET Core com o Visual Studio para Mac

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Adicionar um modelo de dados

* Adicione uma pasta denominada *Modelos*.
* Adicionar uma classe denominada *Movie.cs* à pasta *Modelos*.
* Adicione o código a seguir ao arquivo *Models/Movie.cs*:

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Crie o projeto para verificar se você não tem nenhum erro.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacotes NuGet do Entity Framework Core para migrações

As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*. **Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes.

Edite o arquivo *RazorPagesMovie.csproj*:

* Selecione **Arquivo > Abrir Arquivo** e, em seguida, selecione o arquivo *RazorPagesMovie.csproj*.
* Adicione `<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />` conforme destacado no código a seguir:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)] [!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Fazer scaffolding do modelo de filme

* Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).
* Execute o seguinte comando:

**Observação: execute o comando a seguir no Windows. Para MacOS e Linux, consulte o próximo comando**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* No MacOS e Linux, execute o seguinte comando:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Se você obtiver o erro:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Saia do Visual Studio e execute o comando novamente.

[!INCLUDE[model 4](../../includes/RP/model4.md)] O tutorial a seguir explica os arquivos criados por scaffolding.

>[!div class="step-by-step"]
[Anterior: Introdução](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Próximo: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)
