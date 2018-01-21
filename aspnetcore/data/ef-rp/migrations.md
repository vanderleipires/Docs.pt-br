---
title: "Páginas Razor com núcleo EF - Migrations - 4 de 8"
author: rick-anderson
description: "Neste tutorial, você começar a usar o recurso de migrações EF principal para gerenciar as alterações do modelo de dados em um aplicativo MVC do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 26fbda99b0c1dfa2d09cf387e43f3123c58215f8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>Migrações - Core EF com tutorial páginas Razor (4 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Neste tutorial, o recurso de migrações de EF principal para o gerenciamento de alterações do modelo de dados é usado.

Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Quando um novo aplicativo é desenvolvido, os modelo de dados de alterações com frequência. Cada vez que as alterações do modelo, o modelo obtém fora de sincronia com o banco de dados. Este tutorial Introdução ao configurar o Entity Framework para criar o banco de dados se ele não existir. Cada vez que os dados de alterações do modelo:

* O banco de dados é descartado.
* EF cria um novo que corresponde ao modelo.
* O aplicativo propaga o banco de dados com dados de teste.

Essa abordagem para manter o banco de dados em sincronia com o modelo de dados funciona bem até que você implantar o aplicativo para produção. Quando o aplicativo é executado em produção, normalmente ele está armazenando dados que precisam ser mantidas. O aplicativo não pode começar com um teste de banco de dados sempre que uma alteração é feita (como adicionar uma nova coluna). O recurso de migrações de núcleo EF resolve esse problema, permitindo que o EF principal atualizar o esquema de banco de dados em vez de criar um novo banco de dados.

Em vez de descartar e recriar o banco de dados quando os dados de alterações do modelo, migrações atualiza o esquema e mantém os dados existentes.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacotes NuGet do Entity Framework Core para migrações

Para trabalhar com migrações, use o **Package Manager Console** (PMC) ou a interface de linha de comando (CLI). Esses tutoriais mostram como usar comandos CLI. Informações sobre o PMC estão no [o fim deste tutorial](#pmc).

As ferramentas de EF principal para a interface de linha de comando (CLI) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar este pacote, adicione-o para o `DotNetCliToolReference` coleção no *. csproj* de arquivo, como mostrado. **Observação:** este pacote deve ser instalado, editando o *. csproj* arquivo. O`install-package` comando ou a GUI do Gerenciador de pacote não pode ser usado para instalar este pacote. Editar o *. csproj* arquivo clicando com o nome do projeto no **Solution Explorer** e selecionando **ContosoUniversity.csproj editar**.

A marcação a seguir mostra a atualização *. csproj* arquivo com as ferramentas de EF Core CLI realçado:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
No exemplo anterior, os números de versão foram atuais quando o tutorial foi escrito. Use a mesma versão das ferramentas de EF Core CLI encontrado nos outros pacotes.

## <a name="change-the-connection-string"></a>Altere a cadeia de conexão

No *appSettings. JSON* de arquivo, altere o nome do banco de dados na cadeia de conexão para ContosoUniversity2.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Alterar o nome do banco de dados na cadeia de conexão faz com que a migração primeiro criar um novo banco de dados. Um novo banco de dados é criado como uma com esse nome não existe. Alterando a cadeia de conexão não é necessário para começar a trabalhar com migrações.

Uma alternativa para alterar o nome do banco de dados está excluindo o banco de dados. Use **Pesquisador de objetos do SQL Server** (SSOX) ou o `database drop` comando CLI:

 ```console
 dotnet ef database drop
 ```

A seção a seguir explica como executar comandos CLI.

## <a name="create-an-initial-migration"></a>Criar uma migração inicial

Compile o projeto.

Abra uma janela de comando e navegue até a pasta do projeto. A pasta do projeto contém o *Startup.cs* arquivo.

Digite o seguinte na janela de comando:

```console
dotnet ef migrations add InitialCreate
```

A janela de comando exibe informações semelhantes ao seguinte:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Se a migração falhar com a mensagem "*não é possível acessar o arquivo... ContosoUniversity.dll porque ele está sendo usado por outro processo.* " é exibida:

* Parar o IIS Express.

   * Saia e reinicie o Visual Studio, ou
   * Localize o IIS Express ícone na bandeja do sistema do Windows.
   * Clique no ícone do IIS Express e, em seguida, clique em **ContosoUniversity > Parar Site**.

Se a mensagem de erro "Falha na compilação." é exibido, execute o comando novamente. Se você receber esse erro, deixe uma nota na parte inferior deste tutorial.

### <a name="examine-the-up-and-down-methods"></a>Examine cima e para baixo métodos

O comando Core EF `migrations add` gerado código para criar o banco de dados do. Esse código de migrações está no *migrações\<timestamp > _InitialCreate.cs* arquivo. O `Up` método o `InitialCreate` classe cria as tabelas de banco de dados que correspondem aos conjuntos de entidades de modelo de dados. O `Down` método exclui-los, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Chamadas de migrações de `Up` método para implementar as alterações do modelo de dados para uma migração. Quando você insere um comando para reverter a atualização, chamadas de migrações de `Down` método.

O código anterior é para a migração inicial. Esse código foi criado quando o `migrations add InitialCreate` comando foi executado. O parâmetro de nome de migração ("InitialCreate" no exemplo) é usado para o nome do arquivo. O nome de migração pode ser qualquer nome de arquivo válido. É melhor escolher uma palavra ou frase que resume o que está sendo feito a migração. Por exemplo, uma migração que adicionou uma tabela de departamento pode ser chamada "AddDepartmentTable".

Se a migração inicial é criada e o banco de dados será encerrado:

* O código de criação do banco de dados é gerado.
* O código de criação do banco de dados não precisa ser executado porque o banco de dados já coincide com o modelo de dados. Se o código de criação do banco de dados for executado, ele não faz quaisquer alterações porque o banco de dados já coincide com o modelo de dados.

Quando o aplicativo é implantado para um novo ambiente, o código de criação do banco de dados deve ser executado para criar o banco de dados.

Anteriormente, a cadeia de caracteres de conexão foi alterada para usar um novo nome para o banco de dados. O banco de dados especificado não existe, migrações cria o banco de dados.

### <a name="examine-the-data-model-snapshot"></a>Examine o instantâneo do modelo de dados

Migrações cria um *instantâneo* do esquema de banco de dados atual em *Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Porque o esquema de banco de dados atual é representado no código, Core EF não precisa interagir com o banco de dados para criar as migrações. Quando você adiciona uma migração, Core EF determina o que mudou, comparando o modelo de dados para o arquivo de instantâneo. EF principal interage com o banco de dados somente quando é necessário atualizar o banco de dados.

O arquivo de instantâneo deve ser sincronizado com as migrações que o criou. A migração não pode ser removida, excluindo o arquivo chamado  *\<timestamp > _\<migrationname >. CS*. Se esse arquivo for excluído, as migrações restantes estão fora de sincronia com o arquivo de instantâneo do banco de dados. Para excluir a última migração adicionada, use o [remover migrações de ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.

## <a name="remove-ensurecreated"></a>Remover EnsureCreated

Para o desenvolvimento inicial, o `EnsureCreated` comando foi usado. Neste tutorial, migrações é usado. `EnsureCreated`tem os seguintes limatitions:

* Ignora as migrações e cria o banco de dados e o esquema.
* Não crie uma tabela de migrações.
* Pode *não* ser usado com migrações.
* Foi projetado para teste ou rápido de protótipos onde o banco de dados é descartado e recriado com frequência.

Remova a seguinte linha de `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Aplicar a migração para o banco de dados em desenvolvimento

Na janela de comando, digite o seguinte para criar o banco de dados e tabelas.

```console
dotnet ef database update
```

Observação: Se o `update` comando retorna o erro "Falha na compilação.":

* Execute o comando novamente.
* Se ele falhar novamente, sair do Visual Studio e, em seguida, execute o `update` comando.
* Deixe uma mensagem na parte inferior da página.

A saída do comando é semelhante do `migrations add` saída de comando. No comando anterior, os logs para os comandos SQL que configurar o banco de dados são exibidos. A maioria dos logs é omitida na saída de exemplo a seguir:

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

Para reduzir o nível de detalhes em mensagens de log, pode alterar os níveis de log no *appsettings. Development.JSON* arquivo. Para obter mais informações, consulte [Introdução ao registro em log](xref:fundamentals/logging/index).

Use **Pesquisador de objetos do SQL Server** para inspecionar o banco de dados. Observe a adição de um `__EFMigrationsHistory` tabela. O `__EFMigrationsHistory` tabela mantém o controle de quais migrações foram aplicadas ao banco de dados. Exibir os dados de `__EFMigrationsHistory` tabela, ele mostra uma linha para a migração primeiro. O último log no exemplo de saída CLI anterior mostra a instrução INSERT que cria essa linha.

Executar o aplicativo e verificar se tudo funciona.

## <a name="appling-migrations-in-production"></a>Migrações de appling em produção

É recomendável que os aplicativos de produção devem **não** chamar [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) na inicialização do aplicativo. `Migrate`não deve ser chamado de um aplicativo no farm de servidores. Por exemplo, se o aplicativo foi implantado com expansão (várias instâncias do aplicativo estão executando) de nuvem.

Migração do banco de dados deve ser feita como parte da implantação e de maneira controlada. Abordagens de migração do banco de dados de produção incluem:

* Uso de migrações para criar scripts SQL e usando os scripts SQL na implantação.
* Executando `dotnet ef database update` de um ambiente controlado.

Núcleo EF usa o `__MigrationsHistory` tabela para ver se quaisquer migrações precisam executar. Se o banco de dados é atualizado, nenhuma migração é executada.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Interface de linha de comando (CLI) vs. Package Manager Console (PMC)

O núcleo de EF ferramentas para gerenciar migrações está disponível em:

* Comandos de CLI do .NET core.
* Os cmdlets do PowerShell no Visual Studio **Package Manager Console** janela (PMC).

Este tutorial mostra como usar a CLI, alguns desenvolvedores preferem usando o PMC.

Os comandos de EF principal para o PMC estão no [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pacote. Este pacote está incluído no [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, portanto não é necessário instalá-lo.

**Importante:** isso não é o mesmo pacote que você instala para CLI editando o *. csproj* arquivo. O nome deste termina `Tools`, ao contrário do nome do pacote CLI que termina em `Tools.DotNet`.

Para obter mais informações sobre os comandos CLI, consulte [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Para obter mais informações sobre os comandos PMC, consulte [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Solução de problemas

Baixe o [aplicativo concluído para este estágio](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

O aplicativo gera a seguinte exceção:

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solução: execute`dotnet ef database update`

Se o `update` comando retorna o erro "Falha na compilação.":

* Execute o comando novamente.
* Deixe uma mensagem na parte inferior da página.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/sort-filter-page)
[Próximo](xref:data/ef-rp/complex-data-model)
