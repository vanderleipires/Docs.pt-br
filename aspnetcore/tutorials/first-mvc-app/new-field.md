---
title: Adicionar um novo campo a um aplicativo ASP.NET Core MVC
author: rick-anderson
description: Saiba como usar as Migrações do Entity Framework Code First para adicionar um novo campo a um modelo e migrar essa alteração para um banco de dados.
ms.author: riande
ms.date: 10/06/2017
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: b63bad99c4a966703634c711e5406d86e5bd140c
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010878"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Adicionar um novo campo a um aplicativo ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Nesta seção, você usará as Migrações do [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First para adicionar um novo campo ao modelo e migrar essa alteração ao banco de dados.

Ao usar o EF Code First para criar um banco de dados automaticamente, o Code First adiciona uma tabela ao banco de dados para ajudar a acompanhar se o esquema do banco de dados está sincronizado com as classes de modelo com base nas quais ele foi gerado. Se ele não estiver sincronizado, o EF gerará uma exceção. Isso facilita a descoberta de problemas de código/banco de dados inconsistente.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Abra o arquivo *Models/Movie.cs* e adicione uma propriedade `Rating`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

Compile o aplicativo (Ctrl+Shift+B).

Como você adicionou um novo campo à classe `Movie`, você também precisa atualizar a lista de permissões de associação para que essa nova propriedade seja incluída. Em *MoviesController.cs*, atualize o atributo `[Bind]` dos métodos de ação `Create` e `Edit` para incluir a propriedade `Rating`:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Você também precisa atualizar os modelos de exibição para exibir, criar e editar a nova propriedade `Rating` na exibição do navegador.

Edite o arquivo */Views/Movies/Index.cshtml* e adicione um campo `Rating`:

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Atualize */Views/Movies/Create.cshtml* com um campo `Rating`. Copie/cole o “grupo de formulário” anterior e permita que o IntelliSense ajude você a atualizar os campos. O IntelliSense funciona com os [Auxiliares de Marcação](xref:mvc/views/tag-helpers/intro). Observação: na versão RTM do Visual Studio 2017, você precisa instalar o [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) para o IntelliSense do Razor. Isso será corrigido na próxima versão.

![O desenvolvedor digitou a letra R para o valor do atributo asp-for no segundo elemento de rótulo da exibição. Um menu contextual do IntelliSense foi exibido, mostrando os campos disponíveis, incluindo Classificação, que é realçada na lista automaticamente. Quando o desenvolvedor clicar no campo ou pressionar Enter no teclado, o valor será definido como Classificação.](new-field/_static/cr.png)

O aplicativo não funcionará até que atualizemos o BD para incluir o novo campo. Se você executá-lo agora, obterá o seguinte `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Você está vendo este erro porque a classe de modelo Movie atualizada é diferente do esquema da tabela Movie do banco de dados existente. (Não há nenhuma coluna Classificação na tabela de banco de dados.)

Existem algumas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente no início do ciclo de desenvolvimento, quando você está fazendo o desenvolvimento ativo em um banco de dados de teste; ela permite que você desenvolva rapidamente o modelo e o esquema de banco de dados juntos. No entanto, a desvantagem é que você perde os dados existentes no banco de dados – portanto, você não deseja usar essa abordagem em um banco de dados de produção! Muitas vezes, o uso de um inicializador para propagar um banco de dados com os dados de teste automaticamente é uma maneira produtiva de desenvolver um aplicativo.

2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.

3. Use as Migrações do Code First para atualizar o esquema de banco de dados.

Para este tutorial, usaremos as Migrações do Code First.

Atualize a classe `SeedData` para que ela forneça um valor para a nova coluna. Uma alteração de amostra é mostrada abaixo, mas é recomendável fazer essa alteração em cada `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Compile a solução.

No menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.

  ![Menu do PMC](adding-model/_static/pmc.png)

No PMC, insira os seguintes comandos:

```powershell
Add-Migration Rating
Update-Database
```

O comando `Add-Migration` informa a estrutura de migração para examinar o atual modelo `Movie` com o atual esquema de BD `Movie` e criar o código necessário para migrar o BD para o novo modelo. O nome “Classificação” é arbitrário e é usado para nomear o arquivo de migração. É útil usar um nome significativo para o arquivo de migração.

Se você excluir todos os registros do BD, o inicializador propagará o BD e incluirá o campo `Rating`. Faça isso com os links Excluir no navegador ou no SSOX.

Execute o aplicativo e verifique se você pode criar/editar/exibir filmes com um campo `Rating`. Você também deve adicionar o campo `Rating` aos modelos de exibição `Edit`, `Details` e `Delete`.

> [!div class="step-by-step"]
> [Anterior](search.md)
> [Próximo](validation.md)  
