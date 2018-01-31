---
title: Trabalhando com o SQL Server LocalDB e o ASP.NET Core
author: rick-anderson
description: Explica como trabalhar com o SQL Server LocalDB e o ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 07f024e2e178828c4488adfd866fc6eec3b251dd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a>Trabalhando com o SQL Server LocalDB e o ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette) 

O objeto `MovieContext` cuida da tarefa de se conectar ao banco de dados e mapear objetos `Movie` para registros do banco de dados. O contexto de banco de dados é registrado com o contêiner [Injeção de Dependência](xref:fundamentals/dependency-injection) no método `ConfigureServices` no arquivo *Startup.cs*:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

O sistema de [Configuração](xref:fundamentals/configuration/index) do ASP.NET Core lê a `ConnectionString`. Para o desenvolvimento local, ele obtém a cadeia de conexão do arquivo *appsettings.json*:

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Quando você implanta o aplicativo em um servidor de teste ou de produção, você pode usar uma variável de ambiente ou outra abordagem para definir a cadeia de conexão como um SQL Server real. Consulte [Configuração](xref:fundamentals/configuration/index) para obter mais informações.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

O LocalDB é uma versão leve do Mecanismo de Banco de Dados do SQL Server Express, que é direcionado para o desenvolvimento de programas. O LocalDB é iniciado sob demanda e executado no modo de usuário e, portanto, não há nenhuma configuração complexa. Por padrão, o banco de dados LocalDB cria arquivos “\*.mdf” no diretório *C:/Users/\<user\>*.

<a name="ssox"></a>
* No menu **Exibir**, abra **SSOX** (Pesquisador de Objetos do SQL Server).

  ![Menu de exibição](sql/_static/ssox.png)

* Clique com o botão direito do mouse na tabela `Movie` e selecione **Designer de exibição**:

  ![Menu contextual aberto na tabela Movie](sql/_static/design.png)

  ![Tabela Movie aberta no Designer](sql/_static/dv.png)

Observe o ícone de chave ao lado de `ID`. Por padrão, o EF cria uma propriedade chamada `ID` para a chave primária.

* Clique com o botão direito do mouse na tabela `Movie` e selecione **Exibir dados**:

  ![Tabela Movie aberta mostrando os dados da tabela](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Propagar o banco de dados

Crie uma nova classe chamada `SeedData` na pasta *Models*. Substitua o código gerado pelo seguinte:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

Se houver um filme no BD, o inicializador de semeadura será retornado e nenhum filme será adicionado.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Adicionar o inicializador de semeadura

Adicione o inicializador de semeadura ao final do método `Main` no arquivo *Program.cs*:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

Testar o aplicativo

* Exclua todos os registros no BD. Faça isso com os links Excluir no navegador ou no [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Force o aplicativo a ser inicializado (chame os métodos na classe `Startup`) para que o método de semeadura seja executado. Para forçar a inicialização, o IIS Express deve ser interrompido e reiniciado. Faça isso com uma das seguintes abordagens:

  * Clique com botão direito do mouse no ícone de bandeja do sistema do IIS Express na área de notificação e toque em **Sair** ou **Parar site**:

    ![Ícone de bandeja do sistema do IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextual](sql/_static/stopIIS.png)

   * Se você estiver executando o VS no modo sem depuração, pressione F5 para executar no modo de depuração.
   * Se você estiver executando o VS no modo de depuração, pare o depurador e pressione F5.
   
O aplicativo mostra os dados propagados:

![Aplicativo de filme aberto no Chrome mostrando os dados do filme](sql/_static/m55.png)

O próximo tutorial limpará a apresentação dos dados.

>[!div class="step-by-step"]
[Anterior: Páginas do Razor geradas por scaffolding](xref:tutorials/razor-pages/page)
[Próximo: Atualização das páginas](xref:tutorials/razor-pages/da1)
