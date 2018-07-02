---
title: Adicionar um modelo a um aplicativo ASP.NET Core MVC com o Visual Studio para Mac
author: rick-anderson
description: Adicione um modelo a um aplicativo ASP.NET Core simples.
ms.author: riande
ms.date: 09/22/2017
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 6db079558ccf4515a37a90f7a9e2608333acd7cf
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961380"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

* Clique com o botão direito do mouse na pasta *Modelos* e, em seguida, selecione **Adicionar** > **Novo Arquivo**. 
* Na caixa de diálogo **Novo Arquivo**:

  * Selecione **Geral** no painel esquerdo.
  * Selecione **Classe Vazia** no painel central.
  * Nomeie a classe **Movie** e selecione **Novo**.

Adicione as seguintes propriedades à classe `Movie`:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

O campo `ID` é necessário para o banco de dados para a chave primária.

Compile o projeto para verificar se não há erros. Agora você tem um **M**odelo no seu aplicativo **M**VC.

## <a name="prepare-the-project-for-scaffolding"></a>Preparar o projeto para scaffolding

- Clique com o botão direito do mouse no arquivo de projeto e, em seguida, selecione **Ferramentas > Editar Arquivo**.

  ![exibição da etapa acima](adding-model/_static/1.png)

- Adicione os pacotes NuGet realçados a seguir ao arquivo *MvcMovie.csproj*:
             
  [!code-csharp[](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Salve o arquivo.

- Crie um arquivo *Models/MvcMovieContext.cs* e adicione a seguinte classe `MvcMovieContext`:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Abra o arquivo *Startup.cs* e adicione dois usings:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Adicione o contexto do banco de dados para o arquivo *Startup.cs*:

   [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Isso informa ao Entity Framework quais classes de modelo estão incluídas no modelo de dados. Você está definindo um *conjunto de entidades* de objetos Movie, que serão representados no banco de dados como uma tabela Movie.

- Crie o projeto para verificar se não existem erros.

## <a name="scaffold-the-moviecontroller"></a>Faça o scaffolding do MovieController

Abra uma janela de terminal na pasta do projeto e execute os seguintes comandos:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Se você obtiver o erro `No executable found matching command "dotnet-aspnet-codegenerator", verify`:

 * Você está no diretório do projeto. O diretório do projeto tem os arquivos *Program.cs*, *Startup.cs* e *.csproj*.
 * Sua versão do dotnet é 1.1 ou posterior. Execute `dotnet` para obter a versão.
 * Você adicionou o elemento `<DotNetCliToolReference>` ao [arquivo MvcMovie.csproj](#prepare-the-project-for-scaffolding).
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

O mecanismo de scaffolding cria o seguinte:

* Um controlador de filmes (*Controllers/MoviesController.cs*)
* Arquivos de exibição do Razor para as páginas Criar, Excluir, Detalhes, Editar e Índice (*Views/Movies/\*.cshtml*)

A criação automática das exibições e métodos de ação [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) é conhecida como *scaffolding*. Logo você terá um aplicativo Web totalmente funcional que permitirá que você gerencie um banco de dados de filmes.

### <a name="add-the-files-to-visual-studio"></a>Adicionar os arquivos ao Visual Studio

* Adicione o arquivo *MovieController.cs* ao projeto do Visual Studio:

  * Clique com o botão direito do mouse na pasta *Controladores* e selecione **Adicionar > Adicionar Arquivos**.
  * Selecione o arquivo *MovieController.cs*.

* Adicione a pasta *Filmes* e as exibições:

  * Clique com o botão direito do mouse na pasta *Exibições* e selecione **Adicionar > Adicionar Pasta Existente**.
  * Navegue até a pasta *Exibições*, selecione *Exibições\Filmes* e, em seguida, selecione **Abrir**.
  * Na caixa de diálogo **Selecionar arquivos para adicionar de Filmes**, selecione **Incluir Todos** e, em seguida, **OK**.

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados. No próximo tutorial, trabalharemos com o banco de dados.

## <a name="additional-resources"></a>Recursos adicionais

* [Auxiliares de marcação](xref:mvc/views/tag-helpers/intro)
* [Globalização e localização](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Anterior – Adicionando uma exibição](adding-view.md)
> [Próximo – Trabalhando com o SQL](working-with-sql.md)  
