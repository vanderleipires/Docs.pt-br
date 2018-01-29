---
title: "Núcleo do ASP.NET MVC com núcleo EF - avançado - 10 de 10"
author: tdykstra
description: "Este tutorial apresenta vários tópicos que são úteis a serem consideradas quando você vá além do básico do desenvolvimento de aplicativos web ASP.NET que usam o Entity Framework Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: b1d1cb8710595acd72bd5a0786bf1b1fed8b1d79
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/26/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a>Tópicos avançados - Core de EF com o tutorial do MVC do ASP.NET Core (10 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

No tutorial anterior, você implementou herança de tabela por hierarquia. Este tutorial apresenta vários tópicos que são úteis a serem consideradas quando você vá além do básico do desenvolvimento de aplicativos web ASP.NET Core que usam o Entity Framework Core.

## <a name="raw-sql-queries"></a>Consultas SQL bruto

Uma das vantagens de usar o Entity Framework é que ela evita vincular seu código muito semelhante a um método específico de armazenamento de dados. Ele faz isso através da geração de consultas SQL e comandos para você, que também libera você da necessidade de gravá-los. Mas há casos excepcionais, quando você precisa executar consultas específicas de SQL que você criou manualmente. Para esses cenários, a API do Entity Framework código primeiro inclui métodos que permitem que você passe comandos SQL diretamente para o banco de dados. Você tem as seguintes opções no EF Core 1.0:

* Use o `DbSet.FromSql` método para consultas que retornam tipos de entidade. Os objetos retornados devem ser do tipo esperado pelo `DbSet` objeto e eles automaticamente estiverem controladas pelo contexto de banco de dados, a menos que você [desativar rastreamento](crud.md#no-tracking-queries).

* Use o `Database.ExecuteSqlCommand` para comandos sem consulta.

Se você precisar executar uma consulta que retorna tipos que não são entidades, você pode usar o ADO.NET com a conexão de banco de dados fornecido pelo EF. Os dados retornados não são controlados pelo contexto de banco de dados, mesmo se você usar esse método para recuperar tipos de entidade.

Como sempre é verdadeiro quando você executa comandos SQL em um aplicativo web, você deve tomar precauções para proteger o site contra ataques de injeção de SQL. Uma maneira de fazer isso é usar consultas parametrizadas para certificar-se de que as cadeias de caracteres enviadas por uma página da web não podem ser interpretadas como comandos SQL. Neste tutorial você usará consultas parametrizadas ao integrar a entrada do usuário em uma consulta.

## <a name="call-a-query-that-returns-entities"></a>Chamar uma consulta que retorna entidades

O `DbSet<TEntity>` classe fornece um método que você pode usar para executar uma consulta que retorna uma entidade do tipo `TEntity`. Para ver como isso funciona, alterará o código de `Details` método do controlador de departamento.

Em *DepartmentsController.cs*, no `Details` método, substitua o código que recupera um departamento com um `FromSql` chamada de método, como mostra o seguinte código:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

Para verificar se o novo código funciona corretamente, selecione o **departamentos** guia e, em seguida, **detalhes** para um dos departamentos.

![Detalhes do departamento](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>Chamar uma consulta que retorna os outros tipos

Anteriormente, você criou uma grade de estatísticas de estudante para a página sobre que mostrou o número de alunos para cada data de registro. Você obteve os dados do conjunto de entidades de alunos (`_context.Students`) e usada LINQ para projetar os resultados em uma lista de `EnrollmentDateGroup` exibir objetos de modelo. Suponha que você deseja gravar o SQL em vez de usar o LINQ. Para fazer o que você precisa executar uma consulta SQL que retorna algo diferente de objetos de entidade. No EF Core 1.0, uma maneira de fazer isso é escrever código ADO.NET e obter a conexão de banco de dados do EF.

Em *HomeController*, substitua o `About` método com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Adicionar um uso instrução:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Execute o aplicativo e vá para a página sobre. Ele exibe os mesmos dados que antes.

![Sobre a página](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Chamar uma consulta update

Suponha que os administradores de Contoso University desejam executar alterações globais no banco de dados, como alterar o número de créditos para cada curso. Se a university tiver um grande número de cursos, poderá ser ineficiente para recuperá-las como entidades e alterá-los individualmente. Nesta seção, você implementará uma página da web que permite que o usuário especifique um fator alterar o número de créditos para todos os cursos e fará a alteração, executando uma instrução SQL UPDATE. A página da web será semelhante a ilustração a seguir:

![Página de atualização de curso créditos](advanced/_static/update-credits.png)

Em *CoursesContoller.cs*, adicionar métodos UpdateCourseCredits para HttpGet e HttpPost:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Quando o controlador processa uma solicitação HttpGet, nada será retornado no `ViewData["RowsAffected"]`, e o modo de exibição exibe uma caixa de texto e um botão de envio, conforme mostrado na ilustração anterior.

Quando o **atualização** botão é clicado, o método HttpPost é chamado e multiplicador tem o valor inserido na caixa de texto. O código, em seguida, executa o SQL que atualiza os cursos e retorna o número de linhas afetadas para o modo de exibição `ViewData`. Quando o modo de exibição obtém uma `RowsAffected` valor, ele exibe o número de linhas atualizadas.

Em **Gerenciador de soluções**, com o botão direito do *exibições/cursos* pasta e clique **Adicionar > Novo Item**.

No **Adicionar Novo Item** caixa de diálogo, clique em **ASP.NET** em **instalado** no painel esquerdo, clique em **exibir página MVC**e nomeie o novo modo de exibição  *UpdateCourseCredits.cshtml*.

Em *Views/Courses/UpdateCourseCredits.cshtml*, substitua o código de modelo com o código a seguir:

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Execute o `UpdateCourseCredits` método selecionando o **cursos** guia, em seguida, adicionando "/ UpdateCourseCredits" ao final da URL na barra de endereços do navegador (por exemplo: `http://localhost:5813/Courses/UpdateCourseCredits`). Insira um número na caixa de texto:

![Página de atualização de curso créditos](advanced/_static/update-credits.png)

Clique em **Atualizar**. Você ver o número de linhas afetadas:

![Página de atualização curso créditos linhas afetadas](advanced/_static/update-credits-rows-affected.png)

Clique em **voltar para a lista** para ver a lista de cursos com o número de créditos de revisado.

Observe que o código de produção deve assegurar que sempre atualiza resultar em dados válidos. O código simplificado mostrado aqui pode multiplique o número de créditos suficiente resultar em um número maior que 5. (O `Credits` propriedade tem um `[Range(0, 5)]` atributo.) Consulta update funcionaria, mas os dados inválidos podem causar resultados inesperados em outras partes do sistema que assumem que o número de créditos é 5 ou menos.

Para obter mais informações sobre consultas SQL brutas, consulte [bruto consultas de SQL](https://docs.microsoft.com/ef/core/querying/raw-sql).

## <a name="examine-sql-sent-to-the-database"></a>Examine SQL enviada ao banco de dados

Às vezes é útil ser capaz de ver as consultas SQL reais que são enviadas para o banco de dados. Funcionalidade de registro em log internos para ASP.NET Core será usada automaticamente pelo EF principais para gravar logs que contêm o SQL para consultas e atualizações. Nesta seção, você verá alguns exemplos de log SQL.

Abra *StudentsController.cs* e no `Details` método defina um ponto de interrupção a `if (student == null)` instrução.

Executar o aplicativo no modo de depuração e vá para a página de detalhes para um aluno.

Vá para o **saída** mostrando a depuração de janela de saída, e você vê a consulta:

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

Você observará alguma coisa que pode ser surpreendente: o SQL seleciona linhas até 2 (`TOP(2)`) da tabela Person. O `SingleOrDefaultAsync` método não resolve a 1 linha no servidor. Eis o porquê:

* Se a consulta retornaria várias linhas, o método retornará nulo.
* Para determinar se a consulta retornaria várias linhas, EF tem que verificar se ele retorna pelo menos 2.

Observe que você não precisa usar o modo de depuração e parar no ponto de interrupção para obter saída de log no **saída** janela. É um modo conveniente para parar o log no ponto em que você deseja examinar a saída. Se você não fizer isso, o registro em log continuará e você precise rolar para baixo para localizar as partes do que seu interesse.

## <a name="repository-and-unit-of-work-patterns"></a>Repositório e unidade de padrões de trabalho

Muitos desenvolvedores escrevem código para implementar o repositório e a unidade de padrões de trabalho como um wrapper em torno de código que funciona com o Entity Framework. Esses padrões destinam-se para criar uma camada de abstração entre a camada de acesso a dados e a camada de lógica comercial de um aplicativo. Implementando esses padrões podem ajudar a isolar seu aplicativo de alterações no repositório de dados e pode facilitar o teste de unidade automatizado ou desenvolvimento controlado por testes (TDD). No entanto, gravar código adicional para implementar esses padrões nem sempre é a melhor escolha para aplicativos que usam o EF, por vários motivos:

* A própria classe de contexto EF protege seu código de código específico do repositório de dados.

* A classe de contexto EF pode agir como uma classe de unidade de trabalho para o banco de dados de atualizações que você faça usando EF.

* EF inclui recursos para implementar TDD sem escrever código de repositório.

Para obter informações sobre como implementar o repositório e a unidade de padrões de trabalho, consulte [a versão do Entity Framework 5 desta série de tutoriais](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Entity Framework Core implementa um provedor de banco de dados na memória que pode ser usado para teste. Para obter mais informações, consulte [testes com InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Detecção de alterações automáticas

O Entity Framework determina como uma entidade foi alterado (e, portanto, as atualizações que precisam ser enviados para o banco de dados), comparando os valores atuais de uma entidade com os valores originais. Os valores originais são armazenados quando a entidade é consultada ou anexada. Alguns dos métodos que causam a detecção de alterações automáticas são os seguintes:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

Se você estiver rastreando um grande número de entidades e chamar um desses métodos muitas vezes em um loop, você pode obter melhorias significativas de desempenho desativar temporariamente detecção de alterações automáticas usando o `ChangeTracker.AutoDetectChangesEnabled` propriedade. Por exemplo:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Planos de desenvolvimento e o código de origem de Framework Core do entidade

A fonte do Entity Framework Core está em [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore). O repositório de núcleo EF contém compilações noturnas, acompanhamento, especificações de recurso, design de anotações de reuniões, e [o roteiro para desenvolvimento futuro](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap). Arquivo ou localizar bugs e contribuir.

Embora o código-fonte estiver aberto, Entity Framework Core tem suporte completo como um produto da Microsoft. A equipe do Microsoft Entity Framework mantém controle sobre quais contribuições são aceitos e testa todas as alterações de código para assegurar a qualidade de cada versão.

## <a name="reverse-engineer-from-existing-database"></a>Engenharia reversa de banco de dados existente

Para fazer engenharia reversa um modelo de dados, incluindo classes de entidade de banco de dados existente, use o [scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) comando. Consulte o [tutorial de Introdução](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>Usar o LINQ dinâmico para simplificar o código de classificação de seleção

O [terceiro tutorial nesta série](sort-filter-page.md) mostra como escrever um código LINQ, codificar nomes de colunas em um `switch` instrução. Com duas colunas à sua escolha, isso funciona bem, mas se você tiver muitas colunas o código poderia ficar detalhado. Para resolver esse problema, você pode usar o `EF.Property` método para especificar o nome da propriedade como uma cadeia de caracteres. Para usar essa abordagem, substitua o `Index` método o `StudentsController` com o código a seguir.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>Próximas etapas

Isso conclui esta série de tutoriais sobre como usar o Entity Framework Core em um aplicativo ASP.NET MVC.

Para obter mais informações sobre o EF Core, consulte o [documentação do Entity Framework Core](https://docs.microsoft.com/ef/core). Um livro também está disponível: [Entity Framework Core em ação](https://www.manning.com/books/entity-framework-core-in-action).

Para obter informações sobre como implantar um aplicativo web, consulte [Host e implantar](xref:host-and-deploy/index).

Para obter informações sobre outros tópicos relacionados ao ASP.NET MVC de núcleo, como autenticação e autorização, consulte o [documentação do ASP.NET Core](https://docs.microsoft.com/aspnet/core/).

## <a name="acknowledgments"></a>Confirmações

Tom Dykstra e Rick Anderson (twitter @RickAndMSFT) gravou neste tutorial. Rowan Miller, Diego Vega e outros membros da equipe do Entity Framework assistido com revisões de código e ajudaram a depurar problemas ocorreu enquanto estamos foram escrevendo código para os tutoriais.

## <a name="common-errors"></a>Erros comuns  

### <a name="contosouniversitydll-used-by-another-process"></a>ContosoUniversity.dll usado por outro processo

Mensagem de erro:

> Não é possível abrir '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' para gravação – ' o processo não pode acessar o arquivo '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' porque ele está sendo usado por outro processo.

Solução:

Pare o site no IIS Express. Vá para bandeja de sistema do Windows, localizar o IIS Express e com o botão direito no ícone, selecione o site da Universidade de Contoso e, em seguida, clique em **parar Site**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Scaffold sem nenhum código em cima e métodos de migração

Possível causa:

Os comandos de CLI EF não fechar automaticamente e salvar arquivos de código. Se você tem alterações não salvas quando você executa o `migrations add` comando EF não encontrará suas alterações.

Solução:

Execute o `migrations remove` de comando, salvar as alterações de código e execute novamente o `migrations add` comando.

### <a name="errors-while-running-database-update"></a>Erros durante a atualização do banco de dados em execução

É possível obter outros erros ao fazer alterações de esquema em um banco de dados que contém dados existentes. Se você obtiver erros de migração que não é possível resolver, você pode alterar o nome do banco de dados na cadeia de conexão ou excluir o banco de dados. Com um novo banco de dados, não há nenhum dado para migrar e o comando de atualização de banco de dados é muito mais probabilidade de ser concluído sem erros.

A abordagem mais simples é renomear o banco de dados em *appSettings. JSON*. Na próxima vez que você executar `database update`, será criado um novo banco de dados.

Para excluir um banco de dados SSOX, o banco de dados, clique **excluir**e, em seguida, o **excluir banco de dados** select da caixa de diálogo **fechar conexões existentes** e clique em  **Okey**.

Para excluir um banco de dados usando a CLI, execute o `database drop` comando CLI:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Instância do SQL Server localização de erro

Mensagem de erro:

> Ocorreu um erro relacionado à rede ou específico da instância ao estabelecer uma conexão com o SQL Server. O servidor não foi encontrado ou não estava acessível. Verifique se o nome da instância está correto e se o SQL Server está configurado para permitir conexões remotas. (provedor: Interfaces de rede do SQL, erro: 26 - erro ao localizar servidor/instância especificada)

Solução:

Verifique a cadeia de caracteres de conexão. Se você excluiu manualmente o arquivo de banco de dados, altere o nome do banco de dados na cadeia de caracteres de construção para começar com um novo banco de dados.

>[!div class="step-by-step"]
[Anterior](inheritance.md)
