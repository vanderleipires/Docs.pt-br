---
title: Adicione um modelo a um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Adicione um modelo a um aplicativo ASP.NET Core simples.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5a820789ee3a761025d09aa78f3c42e59fc5fa38
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011359"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Clique com o botão direito do mouse na pasta *Models* > **Adicionar** > **Classe**. Nomeie a classe **Movie** e adicione as seguintes propriedades:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

O campo `ID` é necessário para o banco de dados para a chave primária. 

Compile o projeto para verificar se não há erros. Agora você tem um **M**odelo no aplicativo **M**VC.

## <a name="scaffolding-a-controller"></a>Gerando um controlador por scaffolding

::: moniker range=">= aspnetcore-2.1"

No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Controladores* **> Adicionar > Novo Item com Scaffold**.

![exibição da etapa acima](adding-model/_static/add_controller21.png)

Na caixa de diálogo **Adicionar Scaffold**, toque em **Controlador MVC com exibições, usando o Entity Framework > Adicionar**.

![Caixa de diálogo Adicionar Scaffold](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta *Controllers* **> Adicionar > Controlador**.

![exibição da etapa acima](adding-model/_static/add_controller.png)

Se a caixa de diálogo **Adicionar Dependências do MVC** for exibida:

* [Atualize o Visual Studio para a última versão](https://www.visualstudio.com/downloads/). Versões do Visual Studio anteriores a 15.5 mostram essa caixa de diálogo.
* Se não puder atualizar, selecione **ADICIONAR** e, em seguida, siga as etapas de adição do controlador novamente.

Na caixa de diálogo **Adicionar Scaffold**, toque em **Controlador MVC com exibições, usando o Entity Framework > Adicionar**.

![Caixa de diálogo Adicionar Scaffold](adding-model/_static/add_scaffold2.png)

::: moniker-end

Preencha a caixa de diálogo **Adicionar Controlador**:

* **Classe de modelo:** *Movie (MvcMovie.Models)*
* **Classe de contexto de dados:** selecione o ícone **+** e adicione o **MvcMovie.Models.MvcMovieContext** padrão

![Adicionar contexto de dados](adding-model/_static/dc.png)

* **Exibições:** mantenha o padrão de cada opção marcado
* **Nome do controlador:** mantenha o *MoviesController* padrão
* Toque em **Adicionar**

![Caixa de diálogo Adicionar Controlador](adding-model/_static/add_controller2.png)

O Visual Studio cria:

* Uma [classe de contexto de banco de dados](xref:data/ef-mvc/intro#create-the-database-context) do Entity Framework Core (*Data/MvcMovieContext.cs*)
* Um controlador de filmes (*Controllers/MoviesController.cs*)
* Arquivos de exibição do Razor para as páginas Criar, Excluir, Detalhes, Editar e Índice (<em>Views/Movies/&ast;.cshtml</em>)

A criação automática do contexto de banco de dados e das exibições e métodos de ação [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) é conhecida como *scaffolding*. Logo você terá um aplicativo Web totalmente funcional que permitirá que você gerencie um banco de dados de filmes.

Se você executar o aplicativo e clicar no link **Filme do MVC**, receberá um erro semelhante ao seguinte:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Você precisa criar o banco de dados e usará o recurso [Migrações](xref:data/ef-mvc/migrations) do EF Core para fazer isso. As Migrações permitem criar um banco de dados que corresponde ao seu modelo de dados e atualizar o esquema de banco de dados quando o modelo de dados é alterado.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Adicionar ferramentas do EF e executar a migração inicial

Nesta seção, você usará o PMC (Console de Gerenciador de Pacotes) para:

* Adicione o pacote de Ferramentas do Entity Framework Core. Esse pacote é necessário adicionar migrações e atualizar o banco de dados.
* Adicione uma migração inicial.
* Atualize o banco de dados com a migração inicial.

No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.

<!-- following image shared with uid: tutorials/razor-pages/model --> ![Menu do PMC](adding-model/_static/pmc.png)

No PMC, insira os seguintes comandos:

::: moniker range=">= aspnetcore-2.1"

``` PMC
Add-Migration Initial
Update-Database
```

Ignore a mensagem de erro a seguir, estamos corrigindo-a no próximo tutorial:

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *Nenhum tipo foi especificado para a coluna decimal "Preço" no tipo de entidade "Filme". Isso fará com que valores sejam truncados silenciosamente se não couberem na precisão e na escala padrão. Especifique explicitamente o tipo de coluna do SQL Server que pode acomodar todos os valores usando 'ForHasColumnType()'.*

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Observação:** se você receber um erro com o comando `Install-Package`, abra o Gerenciador de Pacotes NuGet e pesquise pelo pacote `Microsoft.EntityFrameworkCore.Tools`. Isso permite que você instale o pacote ou verifique se ele já está instalado. Observação: consulte a [abordagem da CLI](#cli) caso você tenha problemas com o PMC.

::: moniker-end

O comando `Add-Migration` cria um código para criar o esquema de banco de dados inicial. O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Data/MvcMovieContext.cs*). O argumento `Initial` é usado para nomear as migrações. Você pode usar qualquer nome, mas, por convenção, escolha um nome que descreve a migração. Consulte [Introdução às migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.

O comando `Update-Database` executa o método `Up` no arquivo *Migrations/\<time-stamp>_Initial.cs*, que cria o banco de dados.

<a name="cli"></a> Execute as etapas anteriores usando a CLI (interface de linha de comando) em vez do PMC:

* Adicione as [ferramentas do EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) ao arquivo *.csproj*.
* Execute os seguintes comandos no console (no diretório do projeto):

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Se você executar o aplicativo e obtiver o erro:

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

Provavelmente você não terá executado `dotnet ef database update`.

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Menu contextual do IntelliSense em um item de Modelo listando as propriedades disponíveis para ID, Preço, Data de Lançamento e Título](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Recursos adicionais

* [Auxiliares de marcação](xref:mvc/views/tag-helpers/intro)
* [Globalização e localização](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Anterior – Adicionando uma exibição](adding-view.md)
> [Próximo – Trabalhando com o SQL](working-with-sql.md)  
