---
title: "Páginas Razor com núcleo EF - simultaneidade - 8 de 8"
author: rick-anderson
description: "Este tutorial mostra como lidar com conflitos quando a mesma entidade de atualização de vários usuários ao mesmo tempo."
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: 1c6cdefa1410839606711d7460a8f4d0f1d6c72b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
<span data-ttu-id="3a2f2-103">en-us /</span><span class="sxs-lookup"><span data-stu-id="3a2f2-103">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="3a2f2-104">Manipulando conflitos de simultaneidade - Core de EF com páginas Razor (8 8)</span><span class="sxs-lookup"><span data-stu-id="3a2f2-104">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="3a2f2-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), e [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="3a2f2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="3a2f2-106">Este tutorial mostra como tratar conflitos quando vários usuários atualizam uma entidade simultaneamente (ao mesmo tempo).</span><span class="sxs-lookup"><span data-stu-id="3a2f2-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="3a2f2-107">Se você tiver problemas, você não conseguir resolver, baixe o [aplicativo concluído para este estágio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="3a2f2-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="3a2f2-108">Conflitos de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="3a2f2-108">Concurrency conflicts</span></span>

<span data-ttu-id="3a2f2-109">Ocorre um conflito de simultaneidade quando:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="3a2f2-110">Um usuário navega para a página de edição para uma entidade.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="3a2f2-111">Outro usuário atualiza a mesma entidade antes que a primeira alteração do usuário seja gravada para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="3a2f2-112">Se a detecção de simultaneidade não estiver habilitada, quando ocorrem atualizações simultâneas:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="3a2f2-113">Última atualização wins.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-113">The last update wins.</span></span> <span data-ttu-id="3a2f2-114">Ou seja, os últimos valores de atualização são salvas para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="3a2f2-115">A primeira das atualizações atuais serão perdidas.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="3a2f2-116">Simultaneidade otimista</span><span class="sxs-lookup"><span data-stu-id="3a2f2-116">Optimistic concurrency</span></span>

<span data-ttu-id="3a2f2-117">Simultaneidade otimista permite que os conflitos de simultaneidade ocorrer e, em seguida, reage adequadamente quando fazem.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="3a2f2-118">Por exemplo, Jane visita a página de edição do departamento e altera a alocação do departamento em inglês de US $350,000.00 para $0,00.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Alteração do orçamento para 0](concurrency/_static/change-budget.png)

<span data-ttu-id="3a2f2-120">Antes de clica Jane **salvar**, João visita a mesma página e altera o campo de data de início de 1/9/2007 para 1/9/2013.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Alterando a data de início para 2013](concurrency/_static/change-date.png)

<span data-ttu-id="3a2f2-122">Jane clica **salvar** primeiro e vê-la a alterar quando o navegador exibirá a página de índice.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Alocação de alterado para zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="3a2f2-124">José clica em **salvar** em uma página de edição que ainda mostra uma alocação de US $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="3a2f2-125">O que acontece em seguida é determinado por como lidar com conflitos de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="3a2f2-126">Simultaneidade otimista inclui as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="3a2f2-127">Você pode controlar qual propriedade de um usuário tiver modificado e atualizar apenas as colunas correspondentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="3a2f2-128">No cenário, não há dados seriam perdidos.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="3a2f2-129">Propriedades diferentes foram atualizadas por dois usuários.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="3a2f2-130">Na próxima vez em que alguém procura do departamento em inglês, eles verão as alterações de Jane e de John.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="3a2f2-131">Esse método de atualização pode reduzir o número de conflitos que podem resultar em perda de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="3a2f2-132">Essa abordagem: \* não é possível evitar a perda de dados se forem feitas alterações concorrentes a mesma propriedade.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="3a2f2-133">\* É geralmente não é prático em um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="3a2f2-134">Ele exige manutenção estado significante para manter o controle de busca de todos os valores e novos.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="3a2f2-135">Manter grandes quantidades de estado pode afetar o desempenho do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="3a2f2-136">\* Pode aumentar a complexidade do aplicativo em comparação comparada detecção de simultaneidade em uma entidade.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="3a2f2-137">Você pode deixar a alteração de John substituir a alteração de Jane.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-137">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="3a2f2-138">Na próxima vez que alguém procura o departamento de inglês, eles verão 1/9/2013 e o valor de US $350,000.00 buscadas.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="3a2f2-139">Essa abordagem é chamada uma *cliente ganha* ou *última no Wins* cenário.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="3a2f2-140">(Todos os valores do cliente têm precedência sobre o que está no repositório de dados.) Se você não fizer nenhuma codificação para manipulação de simultaneidade, cliente ganha ocorre automaticamente.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="3a2f2-141">Você pode impedir que alterações de John sendo atualizado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="3a2f2-142">Normalmente, o aplicativo seria: \* exibir uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="3a2f2-143">\* Mostre o estado atual dos dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="3a2f2-144">\* Permitir ao usuário reaplicar as alterações.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-144">\* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="3a2f2-145">Isso é chamado de um *repositório Wins* cenário.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="3a2f2-146">(Os valores de repositório de dados têm precedência sobre os valores enviados pelo cliente.) Você implementar o cenário de armazenamento Wins neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="3a2f2-147">Esse método garante que nenhuma alteração é substituídas sem um usuário que está sendo alertado.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="3a2f2-148">Tratamento de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="3a2f2-148">Handling concurrency</span></span> 

<span data-ttu-id="3a2f2-149">Quando uma propriedade é configurada como um [token de simultaneidade](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="3a2f2-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="3a2f2-150">Núcleo EF verifica que a propriedade não tiver sido modificada depois que ele foi buscado.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="3a2f2-151">A verificação ocorre quando [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) ou [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) é chamado.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-151">The check occurs when [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="3a2f2-152">Se a propriedade tiver sido alterada depois que ele foi buscado, um [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) é gerada.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="3a2f2-153">O modelo de dados e banco de dados deve ser configurado para dar suporte a gerar `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="3a2f2-154">Detectando conflitos de simultaneidade em uma propriedade</span><span class="sxs-lookup"><span data-stu-id="3a2f2-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="3a2f2-155">Conflitos de simultaneidade podem ser detectados no nível de propriedade com o [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atributo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="3a2f2-156">O atributo pode ser aplicado a várias propriedades no modelo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="3a2f2-157">Para obter mais informações, consulte [anotações de dados-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="3a2f2-157">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="3a2f2-158">O `[ConcurrencyCheck]` atributo não é usado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-158">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="3a2f2-159">Detectando conflitos de simultaneidade em uma linha</span><span class="sxs-lookup"><span data-stu-id="3a2f2-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="3a2f2-160">Para detectar conflitos de simultaneidade, um [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) controle de coluna é adicionada ao modelo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-160">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="3a2f2-161">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="3a2f2-161">`rowversion` :</span></span>

* <span data-ttu-id="3a2f2-162">SQL Server é específico.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-162">Is SQL Server specific.</span></span> <span data-ttu-id="3a2f2-163">Outros bancos de dados não podem fornecer um recurso semelhante.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="3a2f2-164">É usado para determinar que uma entidade não foi alterada desde que ele foi obtido do BD.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="3a2f2-165">O banco de dados gera um sequencial `rowversion` número que tenha incrementado toda vez que a linha é atualizado.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="3a2f2-166">Em um `Update` ou `Delete` comando, o `Where` cláusula inclui o valor de busca de `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="3a2f2-167">Se a linha que está sendo atualizada foi alterada:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="3a2f2-168">`rowversion`não corresponde ao valor de busca.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="3a2f2-169">O `Update` ou `Delete` comandos não encontram uma linha porque o `Where` cláusula inclui a busca `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="3a2f2-170">Um `DbUpdateConcurrencyException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="3a2f2-171">No núcleo do EF, quando nenhuma linha foi atualizada por um `Update` ou `Delete` de comando, é gerada uma exceção de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="3a2f2-172">Adicionar uma propriedade de controle para a entidade de departamento</span><span class="sxs-lookup"><span data-stu-id="3a2f2-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="3a2f2-173">Em *Models/Department.cs*, adicionar uma propriedade de controle chamada RowVersion:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="3a2f2-174">O [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atributo especifica que essa coluna é incluída no `Where` cláusula de `Update` e `Delete` comandos.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-174">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="3a2f2-175">O atributo é chamado `Timestamp` porque versões anteriores do SQL Server usaram um SQL `timestamp` tipo de dados antes do SQL `rowversion` tipo substituído.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="3a2f2-176">A API fluente também pode especificar a propriedade de controle:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="3a2f2-177">O código a seguir mostra uma parte do T-SQL gerada pelo EF principal quando o nome de departamento é atualizado:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="3a2f2-178">Anterior realçado código mostra o `WHERE` cláusula que contém `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="3a2f2-179">Se o banco de dados `RowVersion` não é igual a `RowVersion` parâmetro (`@p2`), nenhuma linha é atualizada.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="3a2f2-180">O seguinte código mostra o T-SQL que verifica a exatamente uma linha foi atualizada:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="3a2f2-181">[@@ROWCOUNT ](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) retorna o número de linhas afetadas pela última instrução.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-181">[@@ROWCOUNT](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="3a2f2-182">Não linhas são atualizadas, Core EF lança um `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="3a2f2-183">Você pode ver que o núcleo do T-SQL EF gera na janela de saída do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="3a2f2-184">Atualizar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="3a2f2-184">Update the DB</span></span>

<span data-ttu-id="3a2f2-185">Adicionando o `RowVersion` propriedade altera o modelo de banco de dados, o que requer uma migração.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="3a2f2-186">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-186">Build the project.</span></span> <span data-ttu-id="3a2f2-187">Digite o seguinte em uma janela de comando:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="3a2f2-188">Os comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-188">The preceding commands:</span></span>

* <span data-ttu-id="3a2f2-189">Adiciona o *migrações / {time stamp}_RowVersion.cs* arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="3a2f2-190">Atualizações de *Migrations/SchoolContextModelSnapshot.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="3a2f2-191">A atualização adiciona o seguinte código realçado para o `BuildModel` método:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="3a2f2-192">Executa migrações para atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="3a2f2-193">Scaffold o modelo de departamentos</span><span class="sxs-lookup"><span data-stu-id="3a2f2-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="3a2f2-194">Sair do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="3a2f2-195">Abra uma janela de comando no diretório do projeto (o diretório que contém os arquivos *Program.cs*, *Startup.cs* e *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="3a2f2-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="3a2f2-196">Execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-196">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="3a2f2-197">Os scaffolds comando anterior a `Department` modelo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="3a2f2-198">Abra o projeto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="3a2f2-199">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-199">Build the project.</span></span> <span data-ttu-id="3a2f2-200">A compilação gera erros, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="3a2f2-201">Globalmente altere `_context.Department` para `_context.Departments` (ou seja, adicionar um "s" para `Department`).</span><span class="sxs-lookup"><span data-stu-id="3a2f2-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="3a2f2-202">7 ocorrências forem encontradas e atualizadas.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="3a2f2-203">Atualizar a página de índice de departamentos</span><span class="sxs-lookup"><span data-stu-id="3a2f2-203">Update the Departments Index page</span></span>

<span data-ttu-id="3a2f2-204">O mecanismo de scaffolding criado um `RowVersion` coluna para a página de índice, mas esse campo não deve ser exibida.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="3a2f2-205">Neste tutorial, o último byte do `RowVersion` é exibida para ajudar a entender a simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="3a2f2-206">O último byte não é garantido como exclusivo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="3a2f2-207">Um aplicativo real não exibe `RowVersion` ou o último byte de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="3a2f2-208">Atualize a página de índice:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-208">Update the Index page:</span></span>

* <span data-ttu-id="3a2f2-209">Substitua o índice com departamentos.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="3a2f2-210">Substitua a marcação que contém `RowVersion` com o último byte de `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="3a2f2-211">Substitua FirstMidName FullName.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="3a2f2-212">A marcação a seguir mostra a página atualizada:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="3a2f2-213">Atualizar o modelo de página de edição</span><span class="sxs-lookup"><span data-stu-id="3a2f2-213">Update the Edit page model</span></span>

<span data-ttu-id="3a2f2-214">Atualização *pages\departments\edit.cshtml.cs* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="3a2f2-215">Para detectar um problema de simultaneidade, o [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) é atualizado com o `rowVersion` valor da entidade foi encontrado.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-215">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="3a2f2-216">Núcleo EF gera um comando de atualização do SQL com uma cláusula WHERE que contém o original `RowVersion` valor.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="3a2f2-217">Se nenhuma linha é afetada pelo comando de atualização (linhas não ter original `RowVersion` valor), um `DbUpdateConcurrencyException` exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="3a2f2-218">No código anterior, `Department.RowVersion` é o valor quando a entidade foi encontrada.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="3a2f2-219">`OriginalValue`é o valor no banco de dados quando `FirstOrDefaultAsync` foi chamado nesse método.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="3a2f2-220">O código a seguir obtém os valores de cliente (os valores postados para este método) e os valores do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="3a2f2-221">O seguinte código adiciona uma mensagem de erro personalizada para valores de cada coluna que tem o banco de dados diferente da que foi lançado para `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="3a2f2-222">O seguinte realçado conjuntos de códigos de `RowVersion` valor para o novo valor recuperado do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="3a2f2-223">Na próxima vez que o usuário clica em **salvar**, somente os erros de simultaneidade que acontecem desde a última exibição da página de edição será capturada.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="3a2f2-224">O `ModelState.Remove` instrução é necessária porque `ModelState` tem o antigo `RowVersion` valor.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="3a2f2-225">Na página do Razor, o `ModelState` valor de um campo tem precedência sobre os valores de propriedade do modelo, quando ambos estiverem presentes.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="3a2f2-226">Atualizar a página de edição</span><span class="sxs-lookup"><span data-stu-id="3a2f2-226">Update the Edit page</span></span>

<span data-ttu-id="3a2f2-227">Atualização *Pages/Departments/Edit.cshtml* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="3a2f2-228">A marcação anterior:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-228">The preceding markup:</span></span>

* <span data-ttu-id="3a2f2-229">Atualizações de `page` diretiva de `@page` para `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="3a2f2-230">Adiciona uma versão de linha oculta.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-230">Adds a hidden row version.</span></span> <span data-ttu-id="3a2f2-231">`RowVersion`deve ser adicionado para que postagem associa o valor.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="3a2f2-232">Exibe o último byte de `RowVersion` para fins de depuração.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="3a2f2-233">Substitui `ViewData` com o tipo mais acentuado `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="3a2f2-234">Conflitos de simultaneidade de teste com a página de edição</span><span class="sxs-lookup"><span data-stu-id="3a2f2-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="3a2f2-235">Abra duas instâncias de navegadores de edição no departamento de inglês:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="3a2f2-236">Execute o aplicativo e selecione departamentos.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="3a2f2-237">Clique com botão direito do **editar** hiperlink do departamento em inglês e selecione **abrir em nova guia**.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="3a2f2-238">Na primeira guia, clique o **editar** hiperlink para o departamento em inglês.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="3a2f2-239">As guias do dois navegador exibem as mesmas informações.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="3a2f2-240">Altere o nome na primeira guia do navegador e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-240">Change the name in the first browser tab and click **Save**.</span></span>

![Página 1 após a alteração de edição de departamento](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="3a2f2-242">O navegador mostra a página de índice com o valor alterado e o indicador de rowVersion atualizado.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="3a2f2-243">Observe o indicador de rowVersion atualizado, ele será exibido no segundo postback na guia do.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="3a2f2-244">Altere um campo diferente na segunda guia do navegador.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-244">Change a different field in the second browser tab.</span></span>

![Página 2 após a alteração de edição de departamento](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="3a2f2-246">Clique em **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-246">Click **Save**.</span></span> <span data-ttu-id="3a2f2-247">Ver mensagens de erro para todos os campos que não coincidem com os valores do banco de dados:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-247">You see error messages for all fields that don't match the DB values:</span></span>

![Mensagem de erro de página de edição de departamento](concurrency/_static/edit-error.png)

<span data-ttu-id="3a2f2-249">Esta janela do navegador não pretendia alterar o campo de nome.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="3a2f2-250">Copie e cole o valor atual (idiomas) no campo nome.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="3a2f2-251">Guia saída. Validação do lado do cliente remove a mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-251">Tab out. Client-side validation removes the error message.</span></span>

![Mensagem de erro de página de edição de departamento](concurrency/_static/cv.png)

<span data-ttu-id="3a2f2-253">Clique em **salvar** novamente.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-253">Click **Save** again.</span></span> <span data-ttu-id="3a2f2-254">O valor digitado na segunda guia do navegador é salvo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="3a2f2-255">Você ver os valores salvos na página de índice.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="3a2f2-256">Atualizar a página de exclusão</span><span class="sxs-lookup"><span data-stu-id="3a2f2-256">Update the Delete page</span></span>

<span data-ttu-id="3a2f2-257">Atualize o modelo de página de exclusão com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="3a2f2-258">A página de exclusão detecta conflitos de simultaneidade quando a entidade foi alterado depois que ele foi buscado.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="3a2f2-259">`Department.RowVersion`é a versão de linha quando a entidade foi encontrada.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="3a2f2-260">Quando o EF Core cria o comando SQL DELETE, ele inclui uma cláusula WHERE com `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="3a2f2-261">Se os resultados do comando SQL DELETE em zero linhas afetadas:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="3a2f2-262">O `RowVersion` no SQL DELETE comando não corresponde `RowVersion` no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="3a2f2-263">Uma exceção DbUpdateConcurrencyException é lançada.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="3a2f2-264">`OnGetAsync`é chamado com o `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="3a2f2-265">Atualizar a página de exclusão</span><span class="sxs-lookup"><span data-stu-id="3a2f2-265">Update the Delete page</span></span>

<span data-ttu-id="3a2f2-266">Atualização *Pages/Departments/Delete.cshtml* com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="3a2f2-267">A marcação anterior faz as seguintes alterações:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="3a2f2-268">Atualizações de `page` diretiva de `@page` para `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="3a2f2-269">Adiciona uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-269">Adds an error message.</span></span>
* <span data-ttu-id="3a2f2-270">Substitui FirstMidName com FullName no **administrador** campo.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="3a2f2-271">Alterações `RowVersion` para exibir o último byte.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="3a2f2-272">Adiciona uma versão de linha oculta.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-272">Adds a hidden row version.</span></span> <span data-ttu-id="3a2f2-273">`RowVersion`deve ser adicionado para que postagem associa o valor.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="3a2f2-274">Conflitos de simultaneidade de teste com a página de exclusão</span><span class="sxs-lookup"><span data-stu-id="3a2f2-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="3a2f2-275">Crie um departamento de teste.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-275">Create a test department.</span></span>

<span data-ttu-id="3a2f2-276">Abra duas instâncias de navegadores de exclusão no departamento de teste:</span><span class="sxs-lookup"><span data-stu-id="3a2f2-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="3a2f2-277">Execute o aplicativo e selecione departamentos.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="3a2f2-278">Clique com botão direito do **excluir** hiperlink para o departamento de teste e selecione **abrir em nova guia**.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="3a2f2-279">Clique o **editar** hiperlink para o departamento de teste.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="3a2f2-280">As guias do dois navegador exibem as mesmas informações.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="3a2f2-281">Alterar a alocação na primeira guia do navegador e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="3a2f2-282">O navegador mostra a página de índice com o valor alterado e o indicador de rowVersion atualizado.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="3a2f2-283">Observe o indicador de rowVersion atualizado, ele será exibido no segundo postback na guia do.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="3a2f2-284">Exclua o departamento de teste a partir da segunda guia. Um erro de simultaneidade é a exibição com os valores atuais do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="3a2f2-285">Clicando em **excluir** exclui a entidade, a menos que `RowVersion` foi updated.department foi excluído.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="3a2f2-286">Consulte [herança](xref:data/ef-mvc/inheritance) sobre como herdar de um modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="3a2f2-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="3a2f2-287">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="3a2f2-287">Additional resources</span></span>

* [<span data-ttu-id="3a2f2-288">Tokens de simultaneidade no núcleo do EF</span><span class="sxs-lookup"><span data-stu-id="3a2f2-288">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [<span data-ttu-id="3a2f2-289">Tratamento de simultaneidade no núcleo do EF</span><span class="sxs-lookup"><span data-stu-id="3a2f2-289">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="3a2f2-290">Anterior</span><span class="sxs-lookup"><span data-stu-id="3a2f2-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
