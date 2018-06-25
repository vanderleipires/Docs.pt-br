---
title: Páginas Razor com o EF Core no ASP.NET Core – Migrações – 4 de 8
author: rick-anderson
description: Neste tutorial, você começa a usar o recurso de migrações do EF Core para gerenciar alterações do modelo de dados em um aplicativo ASP.NET Core MVC.
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: d39e1aa40ff97d5b335f2bde6170242e89f6189a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272342"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Páginas Razor com o EF Core no ASP.NET Core – Migrações – 4 de 8

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Neste tutorial, o recurso de migrações do EF Core para o gerenciamento de alterações do modelo de dados é usado.

Caso tenha problemas que não consiga resolver, baixe o [aplicativo concluído para este estágio](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Quando um novo aplicativo é desenvolvido, o modelo de dados é alterado com frequência. Sempre que o modelo é alterado, ele fica fora de sincronia com o banco de dados. Este tutorial começa configurando o Entity Framework para criar o banco de dados, caso ele não exista. Sempre que o modelo de dados é alterado:

* O BD é removido.
* O EF cria um novo que corresponde ao modelo.
* O aplicativo propaga o BD com os dados de teste.

Essa abordagem para manter o BD em sincronia com o modelo de dados funciona bem até que você implante o aplicativo em produção. Quando o aplicativo é executado em produção, normalmente, ele armazena dados que precisam ser mantidos. O aplicativo não pode começar com um BD de teste sempre que uma alteração é feita (como a adição de uma nova coluna). O recurso Migrações do EF Core resolve esse problema, permitindo que o EF Core atualize o esquema de BD em vez de criar um novo BD.

Em vez de remover e recriar o BD quando o modelo de dados é alterado, as migrações atualizam o esquema e retêm os dados existentes.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacotes NuGet do Entity Framework Core para migrações

Para trabalhar com migrações, use o **PMC** (Console do Gerenciador de Pacotes) ou a CLI (interface de linha de comando). Esses tutoriais mostram como usar comandos da CLI. Encontre informações sobre o PMC no [final deste tutorial](#pmc).

As ferramentas do EF Core para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*, conforme mostrado. **Observação:** esse pacote precisa ser instalado pela edição do arquivo *.csproj*. O comando `install-package` ou a GUI do gerenciador de pacotes não pode ser usado para instalar esse pacote. Edite o arquivo *.csproj* clicando com o botão direito do mouse no nome do projeto no **Gerenciador de Soluções** e selecionando **Editar ContosoUniversity.csproj**.

A seguinte marcação mostra o arquivo *.csproj* atualizado com as ferramentas da CLI do EF Core realçadas:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
No exemplo anterior, os números de versão eram atuais no momento em que o tutorial foi escrito. Use a mesma versão das ferramentas da CLI do EF Core encontrada nos outros pacotes.

## <a name="change-the-connection-string"></a>Alterar a cadeia de conexão

No arquivo *appsettings.json*, altere o nome do BD na cadeia de conexão para ContosoUniversity2.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

A alteração do nome do BD na cadeia de conexão faz com que a primeira migração crie um novo BD. Um novo BD é criado porque um banco de dados com esse nome não existe. A alteração da cadeia de conexão não é necessária para começar a usar as migrações.

Uma alternativa à alteração do nome do BD é excluir o BD. Use o **SSOX** (Pesquisador de Objetos do SQL Server) ou o comando `database drop` da CLI:

 ```console
 dotnet ef database drop
 ```

A seção a seguir explica como executar comandos da CLI.

## <a name="create-an-initial-migration"></a>Criar uma migração inicial

Compile o projeto.

Abra uma janela Comando e navegue para a pasta do projeto. A pasta do projeto contém o arquivo *Startup.cs*.

Insira o seguinte na janela Comando:

```console
dotnet ef migrations add InitialCreate
```

A janela Comando exibe informações semelhantes às seguintes:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Se a migração falhar com a mensagem "*não é possível acessar o arquivo... ContosoUniversity.dll porque ele está sendo usado por outro processo.*" será exibido:

* Pare o IIS Express.

   * Saia e reinicie o Visual Studio ou
   * Encontre o ícone do IIS Express na Bandeja do Sistema do Windows.
   * Clique com o botão direito do mouse no ícone do IIS Express e, em seguida, clique em **ContosoUniversity > Parar Site**.

Se a mensagem de erro "Falha no build." for exibida, execute o comando novamente. Se você receber esse erro, deixe uma observação na parte inferior deste tutorial.

### <a name="examine-the-up-and-down-methods"></a>Examinar os métodos Up e Down

O comando `migrations add` do EF Core gerou um código do qual criar o BD. Esse código de migrações está localizado no arquivo *Migrations\<timestamp>_InitialCreate.cs*. O método `Up` da classe `InitialCreate` cria as tabelas de BD que correspondem aos conjuntos de entidades do modelo de dados. O método `Down` exclui-os, conforme mostrado no seguinte exemplo:

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

As migrações chamam o método `Up` para implementar as alterações do modelo de dados para uma migração. Quando você insere um comando para reverter a atualização, as migrações chamam o método `Down`.

O código anterior refere-se à migração inicial. Esse código foi criado quando o comando `migrations add InitialCreate` foi executado. O parâmetro de nome da migração ("InitialCreate" no exemplo) é usado para o nome do arquivo. O nome da migração pode ser qualquer nome de arquivo válido. É melhor escolher uma palavra ou frase que resume o que está sendo feito na migração. Por exemplo, uma migração que adicionou uma tabela de departamento pode ser chamada "AddDepartmentTable".

Se a migração inicial foi criada e o BD existe:

* O código de criação do BD é gerado.
* O código de criação do BD não precisa ser executado porque o BD já corresponde ao modelo de dados. Se o código de criação do BD for executado, ele não fará nenhuma alteração porque o BD já corresponde ao modelo de dados.

Quando o aplicativo é implantado em um novo ambiente, o código de criação do BD precisa ser executado para criar o BD.

Anteriormente, a cadeia de conexão foi alterada para usar um novo nome para o BD. O BD especificado não existe e, portanto, as migrações criam o BD.

### <a name="the-data-model-snapshot"></a>O instantâneo do modelo de dados

As migrações criam um *instantâneo* do esquema de banco de dados atual em *Migrations/SchoolContextModelSnapshot.cs*. Quando você adiciona uma migração, o EF determina o que foi alterado, comparando o modelo de dados com o arquivo de instantâneo.

Ao excluir uma migração, use o comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove). `dotnet ef migrations remove` exclui a migração e garante que o instantâneo seja redefinido corretamente.

Confira [Migrações do EF Core em ambientes de equipe](/ef/core/managing-schemas/migrations/teams) para obter mais informações de como o arquivo de instantâneo é usado.

## <a name="remove-ensurecreated"></a>Remover EnsureCreated

Para o desenvolvimento inicial, o comando `EnsureCreated` foi usado. Neste tutorial, as migrações são usadas. `EnsureCreated` tem as seguintes limitações:

* Ignora as migrações e cria o BD e o esquema.
* Não cria uma tabela de migrações.
* *Não* pode ser usado com migrações.
* Foi projetado para teste ou criação rápida de protótipos em que o BD é removido e recriado com frequência.

Remova a seguinte linha de `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Aplicar a migração ao BD em desenvolvimento

Na janela Comando, insira o seguinte para criar o BD e as tabelas.

```console
dotnet ef database update
```

Observação: se o comando `update` retornar o erro "Falha no build":

* Execute o comando novamente.
* Se ele falhar novamente, saia do Visual Studio e, em seguida, execute o comando `update`.
* Deixe uma mensagem na parte inferior da página.

A saída do comando é semelhante à saída de comando `migrations add`. No comando anterior, os logs para os comandos SQL que configuraram o BD são exibidos. A maioria dos logs é omitida na seguinte saída de exemplo:

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

Para reduzir o nível de detalhes em mensagens de log, altere os níveis de log no arquivo *appsettings.Development.json*. Para obter mais informações, consulte [Introdução ao log](xref:fundamentals/logging/index).

Use o **Pesquisador de Objetos do SQL Server** para inspecionar o BD. Observe a adição de uma tabela `__EFMigrationsHistory`. A tabela `__EFMigrationsHistory` controla quais migrações foram aplicadas ao BD. Exiba os dados na `__EFMigrationsHistory` tabela; ela mostra uma linha para a primeira migração. O último log no exemplo de saída da CLI anterior mostra a instrução INSERT que cria essa linha.

Execute o aplicativo e verifique se tudo funciona.

## <a name="applying-migrations-in-production"></a>Aplicando migrações na produção

Recomendamos que os aplicativos de produção **não** chamem [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) na inicialização do aplicativo. `Migrate` não deve ser chamado em um aplicativo no farm de servidores. Por exemplo, se o aplicativo foi implantado na nuvem com escalabilidade horizontal (várias instâncias do aplicativo estão sendo executadas).

A migração de banco de dados deve ser feita como parte da implantação e de maneira controlada. Abordagens de migração de banco de dados de produção incluem:

* Uso de migrações para criar scripts SQL e uso dos scripts SQL na implantação.
* Execução de `dotnet ef database update` em um ambiente controlado.

O EF Core usa a tabela `__MigrationsHistory` para ver se uma migração precisa ser executada. Se o BD estiver atualizado, nenhuma migração será executada.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>CLI (interface de linha de comando) vs. PMC (Console do Gerenciador de Pacotes)

As ferramentas do EF Core para o gerenciamento de migrações estão disponíveis em:

* Comandos da CLI do .NET Core.
* Os cmdlets do PowerShell na janela do **PMC** (Console do Gerenciador de Pacotes) no Visual Studio.

Este tutorial mostra como usar a CLI; alguns desenvolvedores preferem usar o PMC.

Os comandos do EF Core para o PMC estão no pacote [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Este pacote está incluído no metapacote [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) e, portanto, não é necessário instalá-lo.

**Importante:** esse não é o mesmo pacote que é instalado para a CLI com a edição do arquivo *.csproj*. O nome deste termina com `Tools`, ao contrário do nome do pacote da CLI que termina com `Tools.DotNet`.

Para obter mais informações sobre os comandos da CLI, consulte [CLI do .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Para obter mais informações sobre os comandos do PMC, consulte [Console do Gerenciador de Pacotes (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Solução de problemas

Baixe o [aplicativo concluído para este estágio](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

O aplicativo gera a seguinte exceção:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solução: execute `dotnet ef database update`

Se o comando `update` retornar o erro "Falha no build":

* Execute o comando novamente.
* Deixe uma mensagem na parte inferior da página.

> [!div class="step-by-step"]
> [Anterior](xref:data/ef-rp/sort-filter-page)
> [Próximo](xref:data/ef-rp/complex-data-model)
