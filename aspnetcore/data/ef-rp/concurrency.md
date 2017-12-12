---
title: "Páginas Razor com núcleo EF - simultaneidade - 8 de 8"
author: rick-anderson
description: "Este tutorial mostra como lidar com conflitos quando a mesma entidade de atualização de vários usuários ao mesmo tempo."
keywords: Simultaneidade do ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 0c49376fd1b602fe03ef2a152d19b58513ae2710
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2017
---
en-us /

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>Manipulando conflitos de simultaneidade - Core de EF com páginas Razor (8 8)

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), e [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Este tutorial mostra como tratar conflitos quando vários usuários atualizam uma entidade simultaneamente (ao mesmo tempo). Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).

## <a name="concurrency-conflicts"></a>Conflitos de simultaneidade

Ocorre um conflito de simultaneidade quando:

* Um usuário navega para a página de edição para uma entidade.
* Outro usuário atualiza a mesma entidade antes que a primeira alteração do usuário seja gravada para o banco de dados.

Se a detecção de simultaneidade não estiver habilitada, quando ocorrem atualizações simultâneas:

* Última atualização wins. Ou seja, os últimos valores de atualização são salvas para o banco de dados.
* A primeira das atualizações atuais serão perdidas.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

Simultaneidade otimista permite que os conflitos de simultaneidade ocorrer e, em seguida, reage adequadamente quando fazem. Por exemplo, Jane visita a página de edição do departamento e altera a alocação do departamento em inglês de US $350,000.00 para $0,00.

![Alteração do orçamento para 0](concurrency/_static/change-budget.png)

Antes de clica Jane **salvar**, João visita a mesma página e altera o campo de data de início de 1/9/2007 para 1/9/2013.

![Alterando a data de início para 2013](concurrency/_static/change-date.png)

Jane clica **salvar** primeiro e vê-la a alterar quando o navegador exibirá a página de índice.

![Alocação de alterado para zero](concurrency/_static/budget-zero.png)

José clica em **salvar** em uma página de edição que ainda mostra uma alocação de US $350,000.00. O que acontece em seguida é determinado por como lidar com conflitos de simultaneidade.

Simultaneidade otimista inclui as seguintes opções:

* Você pode controlar qual propriedade de um usuário tiver modificado e atualizar apenas as colunas correspondentes no banco de dados.

 No cenário, não há dados seriam perdidos. Propriedades diferentes foram atualizadas por dois usuários. Na próxima vez em que alguém procura do departamento em inglês, ele verá as alterações de Jane e de John. Esse método de atualização pode reduzir o número de conflitos que podem resultar em perda de dados. Essa abordagem: * não é possível evitar a perda de dados se forem feitas alterações concorrentes a mesma propriedade.
        * É geralmente não é prático em um aplicativo web. Ele exige manutenção estado significante para manter o controle de busca de todos os valores e novos. Manter grandes quantidades de estado pode afetar o desempenho do aplicativo.
        * Pode aumentar a complexidade do aplicativo em comparação comparada detecção de simultaneidade em uma entidade.

* Você pode deixar a alteração de John substituir a alteração de Jane.

 Na próxima vez que alguém procura o departamento de inglês, ele verá 1/9/2013 e o valor de US $350,000.00 buscadas. Essa abordagem é chamada uma *cliente ganha* ou *última no Wins* cenário. (Todos os valores do cliente têm precedência sobre o que está no repositório de dados.) Se você não fizer nenhuma codificação para manipulação de simultaneidade, cliente ganha ocorre automaticamente.

* Você pode impedir que alterações de John sendo atualizado no banco de dados. Normalmente, o aplicativo seria: * exibir uma mensagem de erro.
        * Mostre o estado atual dos dados.
        * Permitir ao usuário reaplicar as alterações.

 Isso é chamado de um *repositório Wins* cenário. (Os valores de repositório de dados têm precedência sobre os valores enviados pelo cliente.) Você implementar o cenário de armazenamento Wins neste tutorial. Esse método garante que nenhuma alteração é substituídas sem um usuário que está sendo alertado.

## <a name="handling-concurrency"></a>Tratamento de simultaneidade 

Quando uma propriedade é configurada como um [token de simultaneidade](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):

* Núcleo EF verifica que a propriedade não tiver sido modificada depois que ele foi buscado. A verificação ocorre quando [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) ou [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) é chamado.
* Se a propriedade tiver sido alterada depois que ele foi buscado, um [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) é gerada. 

O modelo de dados e banco de dados deve ser configurado para dar suporte a gerar `DbUpdateConcurrencyException`.

### <a name="detecting-concurrency-conflicts-on-a-property"></a>Detectando conflitos de simultaneidade em uma propriedade

Conflitos de simultaneidade podem ser detectados no nível de propriedade com o [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atributo. O atributo pode ser aplicado a várias propriedades no modelo. Para obter mais informações, consulte [anotações de dados-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).

O `[ConcurrencyCheck]` atributo não é usado neste tutorial.

### <a name="detecting-concurrency-conflicts-on-a-row"></a>Detectando conflitos de simultaneidade em uma linha

Para detectar conflitos de simultaneidade, um [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) controle de coluna é adicionada ao modelo.  `rowversion` :

* SQL Server é específico. Outros bancos de dados não podem fornecer um recurso semelhante.
* É usado para determinar que uma entidade não foi alterada desde que ele foi obtido do BD. 

O banco de dados gera um sequencial `rowversion` número que tenha incrementado toda vez que a linha é atualizado. Em um `Update` ou `Delete` comando, o `Where` cláusula inclui o valor de busca de `rowversion`. Se a linha que está sendo atualizada foi alterada:

 * `rowversion`não corresponde ao valor de busca.
 * O `Update` ou `Delete` comandos não encontram uma linha porque o `Where` cláusula inclui a busca `rowversion`.
 * Um `DbUpdateConcurrencyException` é gerada.

No núcleo do EF, quando nenhuma linha foi atualizada por um `Update` ou `Delete` de comando, é gerada uma exceção de simultaneidade.

### <a name="add-a-tracking-property-to-the-department-entity"></a>Adicionar uma propriedade de controle para a entidade de departamento

Em *Models/Department.cs*, adicionar uma propriedade de controle chamada RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

O [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atributo especifica que essa coluna é incluída no `Where` cláusula de `Update` e `Delete` comandos. O atributo é chamado `Timestamp` porque versões anteriores do SQL Server usaram um SQL `timestamp` tipo de dados antes do SQL `rowversion` tipo substituído.

A API fluente também pode especificar a propriedade de controle:

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

O código a seguir mostra uma parte do T-SQL gerada pelo EF principal quando o nome de departamento é atualizado:

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

Anterior realçado código mostra o `WHERE` cláusula que contém `RowVersion`. Se o banco de dados `RowVersion` não é igual a `RowVersion` parâmetro (`@p2`), nenhuma linha é atualizada.

O seguinte código mostra o T-SQL que verifica a exatamente uma linha foi atualizada:

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) retorna o número de linhas afetadas pela última instrução. Não linhas são atualizadas, Core EF lança um `DbUpdateConcurrencyException`.

Você pode ver que o núcleo do T-SQL EF gera na janela de saída do Visual Studio.

### <a name="update-the-db"></a>Atualizar o banco de dados

Adicionando o `RowVersion` propriedade altera o modelo de banco de dados, o que requer uma migração.

Compile o projeto. Digite o seguinte em uma janela de comando:

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

Os comandos anteriores:

* Adiciona o *migrações / {time stamp}_RowVersion.cs* arquivo de migração.
* Atualizações de *Migrations/SchoolContextModelSnapshot.cs* arquivo. A atualização adiciona o seguinte código realçado para o `BuildModel` método:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* Executa migrações para atualizar o banco de dados.

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Scaffold o modelo de departamentos

* Sair do Visual Studio.
* Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).
* Execute o seguinte comando:

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

Os scaffolds comando anterior a `Department` modelo. Abra o projeto no Visual Studio.

Compile o projeto. A compilação gera erros, como o seguinte:

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Globalmente altere `_context.Department` para `_context.Departments` (ou seja, adicionar um "s" para `Department`). 7 ocorrências forem encontradas e atualizadas.

### <a name="update-the-departments-index-page"></a>Atualizar a página de índice de departamentos

O mecanismo de scaffolding criado um `RowVersion` coluna para a página de índice, mas esse campo não deve ser exibida. Neste tutorial, o último byte do `RowVersion` é exibida para ajudar a entender a simultaneidade. O último byte não é garantido como sendo exclusivo. Um aplicativo real não exibe `RowVersion` ou o último byte de `RowVersion`.

Atualize a página de índice:

* Substitua o índice com departamentos.
* Substitua a marcação que contém `RowVersion` com o último byte de `RowVersion`.
* Substitua FirstMidName FullName.

A marcação a seguir mostra a página atualizada:

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Atualizar o modelo de página de edição

Atualização *pages\departments\edit.cshtml.cs* com o código a seguir:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

Para detectar um problema de simultaneidade, o [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) é atualizado com o `rowVersion` o valor da entidade era foi encontrado. Núcleo EF gera um comando de atualização do SQL com uma cláusula WHERE que contém o original `RowVersion` valor. Se nenhuma linha é afetada pelo comando de atualização (linhas não ter original `RowVersion` valor), um `DbUpdateConcurrencyException` exceção será lançada.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

No código anterior, `Department.RowVersion` é o valor quando a entidade foi encontrada. `OriginalValue`é o valor no banco de dados quando `FirstOrDefaultAsync` foi chamado nesse método.

O código a seguir obtém os valores de cliente (os valores postados para este método) e os valores do banco de dados:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

O seguinte código adiciona uma mensagem de erro personalizada para valores de cada coluna que tem o banco de dados diferente da que foi lançado para `OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

O seguinte realçado conjuntos de códigos de `RowVersion` valor para o novo valor recuperado do banco de dados. Na próxima vez que o usuário clica em **salvar**, somente os erros de simultaneidade que acontecem desde a última exibição da página de edição será capturada.

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

O `ModelState.Remove` instrução é necessária porque `ModelState` tem o antigo `RowVersion` valor. Na página do Razor, o `ModelState` valor de um campo tem precedência sobre os valores de propriedade do modelo, quando ambos estiverem presentes.

## <a name="update-the-edit-page"></a>Atualizar a página de edição

Atualização *Pages/Departments/Edit.cshtml* com a seguinte marcação:

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

A marcação anterior:

* Atualizações de `page` diretiva de `@page` para `@page "{id:int}"`.
* Adiciona uma versão de linha oculta. `RowVersion`deve ser adicionado para que postagem associa o valor.
* Exibe o último byte de `RowVersion` para fins de depuração.
* Substitui `ViewData` com o tipo mais acentuado `InstructorNameSL`.

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Conflitos de simultaneidade de teste com a página de edição

Abra duas instâncias de navegadores de edição no departamento de inglês:

* Execute o aplicativo e selecione departamentos.
* Clique com botão direito do **editar** hiperlink do departamento em inglês e selecione **abrir em nova guia**.
* Na primeira guia, clique o **editar** hiperlink para o departamento em inglês.

As guias do dois navegador exibem as mesmas informações.

Altere o nome na primeira guia do navegador e clique em **salvar**.

![Página 1 após a alteração de edição de departamento](concurrency/_static/edit-after-change-1.png)

O navegador mostra a página de índice com o valor alterado e o indicador de rowVersion atualizado. Observe o indicador de rowVersion atualizado, ele será exibido no segundo postback na guia do.

Altere um campo diferente na segunda guia do navegador.

![Página 2 após a alteração de edição de departamento](concurrency/_static/edit-after-change-2.png)

Clique em **Salvar**. Ver mensagens de erro para todos os campos que não coincidem com os valores do banco de dados:

![Mensagem de erro de página de edição de departamento](concurrency/_static/edit-error.png)

Esta janela do navegador não pretendia alterar o campo de nome. Copie e cole o valor atual (idiomas) no campo nome. Guia saída. Validação do lado do cliente remove a mensagem de erro.

![Mensagem de erro de página de edição de departamento](concurrency/_static/cv.png)

Clique em **salvar** novamente. O valor digitado na segunda guia do navegador é salvo. Você ver os valores salvos na página de índice.

## <a name="update-the-delete-page"></a>Atualizar a página de exclusão

Atualize o modelo de página de exclusão com o código a seguir:

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

A página de exclusão detecta conflitos de simultaneidade quando a entidade foi alterado depois que ele foi buscado. `Department.RowVersion`é a versão de linha quando a entidade foi encontrada. Quando o EF Core cria o comando SQL DELETE, ele inclui uma cláusula WHERE com `RowVersion`. Se os resultados do comando SQL DELETE em zero linhas afetadas:

* O `RowVersion` no SQL DELETE comando não corresponde `RowVersion` no banco de dados.
* Uma exceção DbUpdateConcurrencyException é lançada.
* `OnGetAsync`é chamado com o `concurrencyError`.

### <a name="update-the-delete-page"></a>Atualizar a página de exclusão

Atualização *Pages/Departments/Delete.cshtml* com o código a seguir:

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


A marcação anterior faz as seguintes alterações:

* Atualizações de `page` diretiva de `@page` para `@page "{id:int}"`.
* Adiciona uma mensagem de erro.
* Substitui FirstMidName com FullName no **administrador** campo.
* Alterações `RowVersion` para exibir o último byte.
* Adiciona uma versão de linha oculta. `RowVersion`deve ser adicionado para que postagem associa o valor.

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Conflitos de simultaneidade de teste com a página de exclusão

Crie um departamento de teste.

Abra duas instâncias de navegadores de exclusão no departamento de teste:

* Execute o aplicativo e selecione departamentos.
* Clique com botão direito do **excluir** hiperlink para o departamento de teste e selecione **abrir em nova guia**.
* Clique o **editar** hiperlink para o departamento de teste.

As guias do dois navegador exibem as mesmas informações.

Alterar a alocação na primeira guia do navegador e clique em **salvar**.

O navegador mostra a página de índice com o valor alterado e o indicador de rowVersion atualizado. Observe o indicador de rowVersion atualizado, ele será exibido no segundo postback na guia do.

Exclua o departamento de teste a partir da segunda guia. Um erro de simultaneidade é a exibição com os valores atuais do banco de dados. Clicando em **excluir** exclui a entidade, a menos que `RowVersion` foi updated.department foi excluído.

Consulte [herança](xref:data/ef-mvc/inheritance) para obter instruções sobre como herança no modelo de dados.

### <a name="additional-resources"></a>Recursos adicionais

* [Tokens de simultaneidade no núcleo do EF](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [Tratamento de simultaneidade no núcleo do EF](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/update-related-data)
