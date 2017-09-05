---
title: "Núcleo do ASP.NET MVC com núcleo EF - Migrations - 4 de 10"
author: tdykstra
description: "Neste tutorial, você começar a usar o recurso de migrações EF principal para o gerenciamento de alterações do modelo de dados em um aplicativo MVC do ASP.NET Core."
keywords: "Migrações do ASP.NET Core, Entity Framework Core,"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 81f6c9c2-a819-4f3a-97a4-4b0503b56c26
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 4d81099d1ab97a8a49d96657153a54aa96dd6bf8
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>Migrações - Core EF com o tutorial do MVC do ASP.NET Core (4 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

Neste tutorial, você começar a usar o recurso de migrações EF principal para o gerenciamento de alterações do modelo de dados. Em tutoriais subsequentes, você adicionará mais migrações que você alterar o modelo de dados.

## <a name="introduction-to-migrations"></a>Introdução às migrações

Quando você desenvolve um novo aplicativo, o modelo de dados é alterado com frequência e cada vez que as alterações do modelo, ele obtém fora de sincronia com o banco de dados. Iniciar esses tutoriais, configurando o Entity Framework para criar o banco de dados se ele não existir. Em seguida, cada vez que você alterar o modelo de dados - adiciona, remove, alterar as classes de entidade ou alterar sua classe DbContext – você pode excluir o banco de dados e EF cria um novo que corresponde ao modelo e propaga-lo com dados de teste.

Esse método de manter o banco de dados em sincronia com o modelo de dados funciona bem até que você implantar o aplicativo para produção. Quando o aplicativo é executado em produção que normalmente está armazenando os dados que você deseja manter, e você não quiser perder tudo o que cada vez que você fizer uma alteração, como adicionar uma nova coluna. O recurso de migrações de núcleo EF resolve esse problema, permitindo que o EF atualizar o esquema de banco de dados em vez de criar um novo banco de dados.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacotes do Entity Framework Core NuGet para migrações

Para trabalhar com migrações, você pode usar o **Package Manager Console** (PMC) ou a interface de linha de comando (CLI).  Esses tutoriais mostram como usar comandos CLI. Informações sobre o PMC estão no [o fim deste tutorial](#pmc).

As ferramentas EF para a interface de linha de comando (CLI) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar este pacote, adicione-o para o `DotNetCliToolReference` coleção no *. csproj* de arquivo, como mostrado. **Observação:** é necessário instalar este pacote editando o *. csproj* arquivo; não é possível usar o `install-package` comando ou a GUI do Gerenciador de pacote. Você pode editar o *. csproj* arquivo clicando com o nome do projeto no **Solution Explorer** e selecionando **ContosoUniversity.csproj editar**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Neste exemplo, os números de versão foram atuais quando o tutorial foi escrito). 

## <a name="change-the-connection-string"></a>Altere a cadeia de conexão

No *appSettings. JSON* arquivo, altere o nome do banco de dados na cadeia de conexão para ContosoUniversity2 ou algum outro nome que você ainda não usado no computador que você está usando.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Essa alteração define o projeto para que a primeira migração irá criar um novo banco de dados. Isso não é necessário para começar a trabalhar com migrações, mas você verá posteriormente por que ele é uma boa ideia.

> [!NOTE]
> Como alternativa para alterar o nome do banco de dados, você pode excluir o banco de dados. Use **Pesquisador de objetos do SQL Server** (SSOX) ou o `database drop` comando CLI:
> ```console
> dotnet ef database drop
> ```
> A seção a seguir explica como executar comandos CLI.

## <a name="create-an-initial-migration"></a>Criar uma migração inicial

Salve suas alterações e compilar o projeto. Em seguida, abra uma janela de comando e navegue até a pasta do projeto. Aqui está uma maneira rápida de fazer isso:

* Em **Solution Explorer**, clique com o botão direito e escolha **abrir no Explorador de arquivos** no menu de contexto.

  ![Abrir no item de menu do Explorador de arquivos](migrations/_static/open-in-file-explorer.png)

* Digite "cmd" na barra de endereços e pressione Enter.

  ![Abrir janela de comando](migrations/_static/open-command-window.png)

Digite o seguinte comando na janela de comando:

```console
dotnet ef migrations add InitialCreate
```

Você verá a saída semelhante à seguinte na janela de comando:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Se você vir uma mensagem de erro *sem executável encontrado correspondente comando "dotnet-ef"*, consulte [esta postagem de blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) para solucionar o problema.

Se você vir uma mensagem de erro "*não é possível acessar o arquivo... ContosoUniversity.dll porque ele está sendo usado por outro processo.* ", localize o ícone do IIS Express na bandeja de sistema do Windows, clique duas vezes e clique em **ContosoUniversity > Parar Site**.

## <a name="examine-the-up-and-down-methods"></a>Examine cima e para baixo métodos

Quando você executou o `migrations add` comando EF o código gerado que criará o banco de dados do zero. Esse código está no *migrações* pasta, no arquivo nomeado  *\<timestamp > _InitialCreate.cs*. O `Up` método o `InitialCreate` classe cria as tabelas de banco de dados que correspondem aos conjuntos de entidade do modelo de dados, e o `Down` método exclui-los, conforme mostrado no exemplo a seguir.

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Chamadas de migrações de `Up` método para implementar as alterações do modelo de dados para uma migração. Quando você insere um comando para reverter a atualização, chamadas de migrações de `Down` método.

Esse código é para a migração inicial que foi criada quando você inseriu o `migrations add InitialCreate` comando. O parâmetro de nome de migração ("InitialCreate" no exemplo) é usado para o nome do arquivo e pode ser que desejar. É melhor escolher uma palavra ou frase que resume o que está sendo feito a migração. Por exemplo, você pode nomear uma migração posterior "AddDepartmentTable".

Se você criou a migração inicial quando o banco de dados já existe, o código de criação do banco de dados é gerado, mas ele não precisa ser executado porque o banco de dados já coincide com o modelo de dados. Quando você implanta o aplicativo para outro ambiente onde o banco de dados não existe ainda, esse código será executado para criar o banco de dados, portanto é uma boa ideia para testá-lo primeiro. É por isso que você alterou o nome do banco de dados na cadeia de conexão anteriormente – para que as migrações podem criar um novo a partir do zero.

## <a name="examine-the-data-model-snapshot"></a>Examine o instantâneo do modelo de dados

Migrações também cria um *instantâneo* do esquema de banco de dados atual em *Migrations/SchoolContextModelSnapshot.cs*. Esse código é semelhante ao seguinte:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Porque o esquema de banco de dados atual é representado no código, Core EF não precisa interagir com o banco de dados para criar as migrações. Quando você adiciona uma migração, EF determina o que mudou, comparando o modelo de dados para o arquivo de instantâneo. EF interage com o banco de dados somente quando é necessário atualizar o banco de dados. 

O arquivo de instantâneo deve ser mantido em sincronia com as migrações que criá-la, para que você não pode remover uma migração bastando excluir o arquivo chamado  *\<timestamp > _\<migrationname >. CS*. Se você excluir esse arquivo, as migrações restantes serão fora de sincronia com o arquivo de instantâneo do banco de dados. Para excluir a última migração que você adicionou, use o [remover migrações de ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.

## <a name="apply-the-migration-to-the-database"></a>Aplicar a migração para o banco de dados

Na janela de comando, digite o seguinte comando para criar o banco de dados e tabelas dentro dele.

```console
dotnet ef database update
```

A saída do comando é semelhante de `migrations add` de comando, exceto que você consulte os logs para o SQL comandos que configurar o banco de dados. A maioria dos logs é omitida na saída de exemplo a seguir. Se você preferir não ver esse nível de detalhe em mensagens de log, você pode alterar os níveis de log no *appsettings. Development.JSON* arquivo. Para obter mais informações, consulte [Introdução ao registro em log](xref:fundamentals/logging).

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Use **Pesquisador de objetos do SQL Server** para inspecionar o banco de dados como você fez no primeiro tutorial.  Observe a adição de uma tabela __EFMigrationsHistory que controla quais migrações foram aplicadas ao banco de dados. Exibir os dados nessa tabela e você verá uma linha para a migração primeiro. (O último log no exemplo de saída CLI anterior mostra a instrução INSERT que cria essa linha.)

Execute o aplicativo para verificar se tudo funciona ainda o mesmo de antes.

![Página de índice de alunos](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Interface de linha de comando (CLI) vs. Package Manager Console (PMC)

O EF ferramentas para gerenciar migrações está disponível a partir de comandos .NET Core CLI ou cmdlets do PowerShell no Visual Studio **Package Manager Console** janela (PMC). Este tutorial mostra como usar a CLI, mas você pode usar o PMC se você preferir.

Os comandos EF para os comandos PMC estão no [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pacote. Este pacote já está incluído no [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, portanto não é necessário instalá-lo.

**Importante:** isso não é o mesmo pacote que você instala para CLI editando o *. csproj* arquivo. O nome deste termina `Tools`, ao contrário do nome do pacote CLI que termina em `Tools.DotNet`.

Para obter mais informações sobre os comandos CLI, consulte [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Para obter mais informações sobre os comandos PMC, consulte [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Resumo

Neste tutorial, você viu como criar e aplicar a migração primeiro. O seguinte tutorial, você começará olhando para tópicos mais avançados, expandindo o modelo de dados. Ao longo do processo, você vai criar e aplicar migrações adicionais.

>[!div class="step-by-step"]
[Anterior](sort-filter-page.md)
[Próximo](complex-data-model.md)  
