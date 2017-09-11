---
title: Adicionando um modelo
author: rick-anderson
description: Adicione um modelo para um aplicativo simples do ASP.NET Core.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 054ef316461447ab651cca8c4f324e7b4e98f856
ms.sourcegitcommit: d9e2c99c837078fcac0e315025f8fbfbd45ea6e8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Adicionar uma classe denominada *Movie.cs* à pasta *Modelos*.
* Adicione o código a seguir ao arquivo *Models/Movie.cs*:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

O campo `ID` é necessário para o banco de dados para a chave primária. 

Compile o aplicativo para verificar se você não tem nenhum erro e, por fim, você adicionou um **M**odelo ao seu aplicativo **M**VC.

## <a name="prepare-the-project-for-scaffolding"></a>Preparar o projeto para scaffolding

- Adicione os pacotes NuGet realçados a seguir ao arquivo *MvcMovie.csproj*:
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Salve o arquivo e selecione **Restaurar** para a mensagem **Informativa** "Há dependências não resolvidas".
- Crie um arquivo *Models/MvcMovieContext.cs* e adicione a seguinte classe `MvcMovieContext`:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Abra o arquivo *Startup.cs* e adicione dois usings:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Adicione o contexto do banco de dados para o arquivo *Startup.cs*:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Isso informa ao Entity Framework quais classes de modelo estão incluídas no modelo de dados. Você está definindo um *conjunto de entidades* de objetos Movie, que serão representados no banco de dados como uma tabela Movie.

- Crie o projeto para verificar se não existem erros.

## <a name="scaffold-the-moviecontroller"></a>Faça o scaffolding do MovieController

Abra uma janela de terminal na pasta do projeto e execute os seguintes comandos:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> Se você receber um erro quando executar o comando de scaffolding, consulte [problema 444 no repositório de scaffolding](https://github.com/aspnet/scaffolding/issues/444) para uma solução alternativa.

O mecanismo de scaffolding cria o seguinte:

* Um controlador de filmes (*Controllers/MoviesController.cs*)
* Arquivos de exibição do Razor para as páginas Criar, Excluir, Detalhes, Editar e Índice (*Views/Movies/\*.cshtml*)

A criação automática das exibições e métodos de ação [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) é conhecida como *scaffolding*. Logo você terá um aplicativo Web totalmente funcional que permitirá que você gerencie um banco de dados de filmes.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados. No próximo tutorial, trabalharemos com o banco de dados.

### <a name="additional-resources"></a>Recursos adicionais

* [Auxiliares de Marcas](xref:mvc/views/tag-helpers/intro)
* [Globalização e localização](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Anterior – adicionar uma exibição](adding-view.md)
[Próximo – trabalhando com SQLite](working-with-sql.md)
