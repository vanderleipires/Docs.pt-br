---
title: "Adicionando um modelo a um aplicativo de Páginas do Razor no ASP.NET Core"
author: rick-anderson
description: "Adicionando um modelo a um aplicativo de Páginas do Razor no ASP.NET Core"
keywords: "ASP.NET Core, Páginas do Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/modelz
ms.openlocfilehash: 1a08ecf1ee12fa0860cb6a18c1a63eaff2ddfbed
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a>Adicionando um modelo a um aplicativo de Páginas do Razor

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Adicionar um modelo de dados

No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **RazorPagesMovie** > **Adicionar** > **Nova Pasta**. Nomeie a pasta como *Modelos*.

Clique com o botão direito do mouse na pasta *Modelos* > **Adicionar** > **Classe**. Nomeie a classe como **Filmes** e adicione as seguintes propriedades:

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Adicionar uma cadeia de conexão de banco de dados

Adicione uma cadeia de conexão ao arquivo *appsettings.json*.

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Registrar o contexto do banco de dados

Registre o contexto do banco de dados com o contêiner de [injeção de dependência](xref:fundamentals/dependency-injection) no arquivo *Startup.cs*.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

Crie o projeto para verificar se você não tem nenhum erro.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Adicionar ferramentas de scaffold e executar a migração inicial

Nesta seção, você deve usar o PMC (Console de Gerenciador de Pacotes) para:

* Adicione o pacote de geração de código da Web do Visual Studio. Esse pacote é necessário para executar o mecanismo de scaffolding.
* Adicionar uma migração inicial.
* Atualizar o banco de dados com a migração inicial.

Do menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

No PMC, digite os seguintes comandos:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

O comando `Install-Package` instala as ferramentas necessárias para executar o mecanismo de scaffolding.

O comando `Add-Migration` gera código para criar o esquema de banco de dados inicial. O esquema é baseado no modelo especificado no `DbContext` (no arquivo *Models/MovieContext.cs*). O argumento `Initial` é usado para nomear as migrações. Você pode usar qualquer nome, mas, por convenção, você escolhe um nome que descreve a migração. Veja [Introdução a migrações](xref:data/ef-mvc/migrations#introduction-to-migrations) para obter mais informações.

O comando `Update-Database` executa o método `Up` no arquivo *Migrations/\<time-stamp>_InitialCreate.cs*, que cria o banco de dados.

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

O tutorial a seguir explica os arquivos criados por scaffolding.

>[!div class="step-by-step"]
[Anterior: Introdução](xref:tutorials/razor-pages/razor-pages-start)
[Próximo: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)    