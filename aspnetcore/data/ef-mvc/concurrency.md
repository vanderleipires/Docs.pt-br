---
title: "Núcleo do ASP.NET MVC com núcleo EF - simultaneidade - 8 de 10"
author: tdykstra
description: "Este tutorial mostra como lidar com conflitos quando a mesma entidade de atualização de vários usuários ao mesmo tempo."
keywords: Simultaneidade do ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 15e79e15-bda5-441d-80c7-8032a2628605
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 9a9ce7eed7ab43a11e1285dec4aa6c0715889dd2
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>Manipulando conflitos de simultaneidade - Tutorial EF Core de EF com ASP.NET Core MVC (8 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

Nos tutoriais anteriores, você aprendeu como atualizar dados. Este tutorial mostra como lidar com conflitos quando a mesma entidade de atualização de vários usuários ao mesmo tempo.

Você vai criar páginas da web que funcionam com a entidade de departamento e tratar erros de simultaneidade. As ilustrações a seguir mostram as páginas, editar e excluir, incluindo algumas mensagens que são exibidas se ocorrer um conflito de simultaneidade.

![Página de edição de departamento](concurrency/_static/edit-error.png)

![Página de exclusão de departamento](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>Conflitos de simultaneidade

Um conflito de simultaneidade ocorre quando um usuário exibe dados de uma entidade para editá-lo e, em seguida, outro usuário atualiza os dados da mesma entidade antes que a primeira alteração do usuário seja gravada no banco de dados. Se você não habilitar a detecção desses conflitos, última pessoa que atualiza o banco de dados substitui as outras alterações do usuário. Em muitos aplicativos, esse risco é aceitável: se houver poucos usuários ou algumas atualizações, ou se não for mais importante se algumas alterações são substituídas, o custo de programação para simultaneidade pode sobrecarregar o benefício. Nesse caso, você não precisa configurar o aplicativo para lidar com conflitos de simultaneidade.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidade pessimista (bloqueio)

Se seu aplicativo precisar evitar a perda acidental de dados em cenários de simultaneidade, uma maneira de fazer isso é usar bloqueios de banco de dados. Isso é chamado de simultaneidade pessimista. Por exemplo, antes de ler uma linha de um banco de dados, você solicita um bloqueio de somente leitura ou para acesso de atualização. Se você bloquear uma linha para acesso de atualização, outros usuários não têm permissão para bloquear a linha para somente leitura ou de acesso de atualização, porque eles obterá uma cópia dos dados que está sendo alterado. Se você bloquear uma linha para acesso somente leitura, outros também bloqueá-la para acesso somente leitura, mas não para atualização.

Gerenciamento de bloqueios tem desvantagens. Ele pode ser complexo para o programa. Requer recursos de gerenciamento de banco de dados, e pode causar problemas de desempenho como o número de usuários de um aplicativo aumenta. Por esses motivos, nem todos os sistemas de gerenciamento de banco de dados oferecem suporte a simultaneidade pessimista. Entity Framework Core fornece sem suporte interno para ele, e este tutorial não mostra como implementá-la.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

A alternativa de simultaneidade pessimista é simultaneidade otimista. Simultaneidade otimista significa permitindo que os conflitos de simultaneidade ocorrer e reagir adequadamente se eles fazem. Por exemplo, Jane visita a página Editar departamento e altera o valor do orçamento para o departamento de inglês de US $350,000.00 para $0,00.

![Alteração do orçamento para 0](concurrency/_static/change-budget.png)

Antes de clica Jane **salvar**, João visita a mesma página e altera o campo de data de início de 1/9/2007 para 1/9/2013.

![Alterando a data de início para 2013](concurrency/_static/change-date.png)

Jane clica **salvar** primeiro e vê-la a alterar quando o navegador retorna à página de índice.

![Alocação de alterado para zero](concurrency/_static/budget-zero.png)

Em seguida, José clica em **salvar** em uma página de edição que ainda mostra uma alocação de US $350,000.00. O que acontece em seguida é determinado por como lidar com conflitos de simultaneidade.

Algumas das opções incluem o seguinte:

* Você pode controlar qual propriedade de um usuário tiver modificado e atualizar apenas as colunas correspondentes no banco de dados.

     No cenário de exemplo, nenhum dado for perdido, porque as propriedades diferentes foram atualizadas por dois usuários. Na próxima vez em que alguém procura do departamento em inglês, ele verá alterações de Jane e de John – a data de início de 1/9/2013 e um orçamento de zero dólares. Esse método de atualização pode reduzir o número de conflitos que podem resultar em perda de dados, mas ele não é possível evitar a perda de dados se forem feitas alterações concorrentes a mesma propriedade de uma entidade. Se o Entity Framework funciona dessa maneira depende de como você pode implementar seu código de atualização. Geralmente não é prático em um aplicativo web, porque isso pode exigir que você mantenha grandes quantidades de estado para manter o controle de todos os valores de propriedade original para uma entidade, bem como novos valores. Manutenção de grandes quantidades de estado pode afetar o desempenho de aplicativo porque ele requer recursos de servidor ou deve ser incluído na própria página da web (por exemplo, nos campos ocultos) ou em um cookie.

* Você pode deixar a alteração de John substituir a alteração de Jane.

     Na próxima vez que alguém procura o departamento de inglês, ele verá 1/9/2013 e o valor de US $350,000.00 restaurado. Isso é chamado de um *cliente ganha* ou *última no Wins* cenário. (Todos os valores do cliente têm precedência sobre o que está no repositório de dados.) Conforme observado na introdução a esta seção se você não fizer nenhuma codificação para manipulação de simultaneidade, isso acontecerá automaticamente.

* Você pode impedir que alterações de John sendo atualizado no banco de dados.

     Normalmente, você seria exibir uma mensagem de erro, mostrar o estado atual dos dados ele e permitem que ele reaplique suas alterações se ele ainda deseja torná-los. Isso é chamado de um *repositório Wins* cenário. (Os valores de repositório de dados têm precedência sobre os valores enviados pelo cliente.) Você implementará o cenário de armazenamento Wins neste tutorial. Esse método garante que nenhuma alteração é substituídas sem um usuário que está sendo alertado sobre o que está acontecendo.

### <a name="detecting-concurrency-conflicts"></a>Detectando conflitos de simultaneidade

Você pode resolver conflitos manipulando `DbConcurrencyException` exceções que gera o Entity Framework. Para saber quando gerar essas exceções, o Entity Framework deve ser capaz de detectar conflitos. Portanto, você deve configurar o banco de dados e o modelo de dados adequadamente. Algumas opções para ativar a detecção de conflitos incluem o seguinte:

* A tabela de banco de dados inclua uma coluna de rastreamento que pode ser usada para determinar quando uma linha foi alterada. Você pode configurar o Entity Framework para incluir essa coluna no local em que a cláusula de comandos de exclusão ou atualização do SQL.

     O tipo de dados da coluna de rastreamento é normalmente `rowversion`. O `rowversion` valor é um número sequencial que tem incrementado toda vez que a linha é atualizada. Em um comando Update ou Delete, onde cláusula inclui o valor original da coluna de rastreamento (a versão de linha original). Se a linha que está sendo atualizada tiver sido alterada por outro usuário, o valor de `rowversion` coluna é diferente do valor original, para que a instrução Update ou Delete não é possível localizar a linha a ser atualizada devido a onde cláusula. Quando encontra o Entity Framework que nenhuma linha foi atualizada por Update ou Delete de comando (ou seja, quando o número de linhas afetadas será zero), que ele interpreta como um conflito de simultaneidade.

* Configurar o Entity Framework para incluir os valores originais de cada coluna na tabela onde a cláusula de comandos Update e Delete.

     Como a primeira opção, se nada na linha foi alterado desde que a linha foi lido pela primeira vez, onde cláusula não retornará uma linha a ser atualizada, que o Entity Framework interpreta como um conflito de simultaneidade. Para tabelas de banco de dados que têm um número de colunas, essa abordagem pode resultar em grande onde cláusulas e pode exigir que você mantenha grandes quantidades de estado. Conforme observado anteriormente, manter grandes quantidades de estado pode afetar o desempenho do aplicativo. Portanto, essa abordagem geralmente não é recomendada e não é o método usado neste tutorial.

     Se você quiser implementar essa abordagem, você precisa marcar todas as propriedades de chave não primária da entidade que você deseja controlar a simultaneidade para adicionando o `ConcurrencyCheck` atributo a eles. Essa alteração permite que o Entity Framework para incluir todas as colunas na cláusula Where SQL instruções Update e Delete.

O restante deste tutorial, você adicionará um `rowversion` propriedade de controle para a entidade de departamento, criar um controlador e modos de exibição e de teste para verificar se tudo está funcionando corretamente.

## <a name="add-a-tracking-property-to-the-department-entity"></a>Adicionar uma propriedade de controle para a entidade de departamento

Em *Models/Department.cs*, adicionar uma propriedade de controle chamada RowVersion:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

O `Timestamp` atributo especifica que esta coluna será incluída no local em que a cláusula de comandos Update e Delete enviados para o banco de dados. O atributo é chamado `Timestamp` porque versões anteriores do SQL Server usaram um SQL `timestamp` tipo de dados antes do SQL `rowversion` substituído. O tipo de .NET para `rowversion` é uma matriz de bytes.

Se você preferir usar a API fluente, você pode usar o `IsConcurrencyToken` método (em *Data/SchoolContext.cs*) para especificar a propriedade de controle, conforme mostrado no exemplo a seguir:

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

Adicionando uma propriedade você alterou o modelo de banco de dados, portanto você precisa fazer a migração de outra.

Salve suas alterações e compilar o projeto e, em seguida, digite os comandos a seguir na janela de comando:

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>Criar um controlador de departamentos e modos de exibição

Scaffold um controlador de departamentos e modos de exibição, como você fez anteriormente para instrutores, cursos e alunos.

![Scaffold departamento](concurrency/_static/add-departments-controller.png)

No *DepartmentsController.cs* de arquivo, altere todas as quatro ocorrências de "FirstMidName" para "FullName" para que o departamento administrador nas listas suspensas conterá o nome completo do instrutor em vez de apenas o último nome.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>Atualizar a exibição do índice de departamentos

O mecanismo de scaffolding criou uma coluna de RowVersion na exibição do índice, mas esse campo não deve ser exibido.

Substitua o código em *Views/Departments/Index.cshtml* com o código a seguir.

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

Isso altera o título "Departamentos", exclui a coluna de RowVersion e mostra o nome completo em vez de nome para o administrador.

## <a name="update-the-edit-methods-in-the-departments-controller"></a>Atualizar os métodos de edição no controlador de departamentos

Em ambos o HttpGet `Edit` método e o `Details` método, adicione `AsNoTracking`. No HttpGet `Edit` método, adicione o carregamento rápido para o administrador.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

Substitua o código existente para o HttpPost `Edit` método com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

O código começa com a tentativa de ler o departamento a ser atualizado. Se o `SingleOrDefaultAsync` método retornará nulo, o departamento foi excluído por outro usuário. Nesse caso, o código usa os valores de postagem de formulário para criar uma entidade de departamento para que a página de edição pode ser exibida novamente com uma mensagem de erro. Como alternativa, você não precisa recriar a entidade de departamento, se você exibir uma mensagem de erro sem exibir novamente os campos de departamento.

O modo de exibição armazena original `RowVersion` valor em um campo oculto e esse método recebe esse valor no `rowVersion` parâmetro. Antes de chamar `SaveChanges`, você precisa colocar que original `RowVersion` no valor da propriedade de `OriginalValues` coleção para a entidade.

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

Quando o Entity Framework cria um comando de atualização do SQL, o comando incluirá uma cláusula WHERE que procura uma linha que tenha o original `RowVersion` valor. Se nenhuma linha é afetada pelo comando de atualização (linhas não ter original `RowVersion` valor), o Entity Framework lança uma `DbUpdateConcurrencyException` exceção.

O código em um bloco catch para essa exceção obtém a entidade Department afetada que tem os valores atualizados do `Entries` propriedade no objeto de exceção.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

O `Entries` coleção terá apenas um `EntityEntry` objeto.  Você pode usar esse objeto para obter os novos valores inseridos pelo usuário e os valores de banco de dados atual.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

O código adiciona uma mensagem de erro personalizada para cada coluna com valores do banco de dados diferentes do que o usuário inseriu no menu Editar página (apenas um campo é mostrado aqui para fins de brevidade).

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

Por fim, o código define o `RowVersion` valor o `departmentToUpdate` para o novo valor recuperado do banco de dados. Essa nova `RowVersion` valor será armazenado no campo oculto quando a edição da página é exibida novamente e a próxima vez que o usuário clica em **salvar**, somente os erros de simultaneidade ocorrem pois a reexibição da página de edição será capturada.

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

O `ModelState.Remove` instrução é necessária porque `ModelState` tem o antigo `RowVersion` valor. No modo de exibição, o `ModelState` valor de um campo tem precedência sobre os valores de propriedade do modelo, quando ambos estiverem presentes.

## <a name="update-the-department-edit-view"></a>Atualizar a exibição de departamento editar

Em *Views/Departments/Edit.cshtml*, faça as seguintes alterações:

* Adicionar um campo oculto para salvar o `RowVersion` valor da propriedade, imediatamente após o campo oculto para o `DepartmentID` propriedade.

* Adicione uma opção "Selecionar administrador" à lista suspensa.

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>Conflitos de simultaneidade de teste na página de edição

Executar o aplicativo e vá para a página de índice de departamentos. Com o botão direito a **editar** hiperlink do departamento em inglês e selecione **abrir em nova guia**, em seguida, clique no **editar** hiperlink para o departamento em inglês. As guias de dois navegador agora exibem as mesmas informações.

Alterar um campo na primeira guia do navegador e clique em **salvar**.

![Página 1 após a alteração de edição de departamento](concurrency/_static/edit-after-change-1.png)

O navegador mostra a página de índice com o valor alterado.

Altere um campo na segunda guia do navegador.

![Página 2 após a alteração de edição de departamento](concurrency/_static/edit-after-change-2.png)

Clique em **Salvar**. Você vê uma mensagem de erro:

![Mensagem de erro de página de edição de departamento](concurrency/_static/edit-error.png)

Clique em **salvar** novamente. O valor digitado na segunda guia do navegador é salvo. Você vê os valores salvos quando a página de índice é exibida.

## <a name="update-the-delete-page"></a>Atualizar a página de exclusão

Para a página de exclusão, o Entity Framework detecta conflitos de simultaneidade causados por outra edição ou o departamento de maneira semelhante. Quando o HttpGet `Delete` método exibe o modo de confirmação, o modo de exibição inclui original `RowVersion` valor em um campo oculto. Que valor estará disponível para o HttpPost `Delete` método é chamado quando o usuário confirmar a exclusão. Quando o Entity Framework cria o comando SQL DELETE, ele inclui uma cláusula WHERE com original `RowVersion` valor. Se os resultados do comando em zero linhas afetado (ou seja, a linha foi alterada depois que a página de confirmação de exclusão foi exibida), é gerada uma exceção de simultaneidade e o HttpGet `Delete` método é chamado com um erro sinalizador definido como true para exibir novamente o página de confirmação com uma mensagem de erro. Também é possível que o zero linhas foram afetadas porque a linha foi excluída por outro usuário, portanto, nesse caso, nenhuma mensagem de erro é exibida.

### <a name="update-the-delete-methods-in-the-departments-controller"></a>Atualizar os métodos de exclusão no controlador de departamentos

Em *DepartmentsController.cs*, substitua o HttpGet `Delete` método com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

O método aceita um parâmetro opcional que indica se a página está sendo exibida novamente depois de um erro de simultaneidade. Se esse sinalizador for verdadeiro e o departamento especificado não existe, ele foi excluído por outro usuário. Nesse caso, o código redireciona para a página de índice.  Se esse sinalizador é verdadeiro e o departamento de existir, ele foi alterado por outro usuário. Nesse caso, o código envia uma mensagem de erro para o modo de exibição usando `ViewData`.  

Substitua o código no HttpPost `Delete` método (chamado `DeleteConfirmed`) com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

No código de scaffolding que você acabou de ser substituído, esse método aceita apenas uma ID de registro:


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

Você alterou esse parâmetro para uma instância de entidade do departamento criada pelo associador de modelo. Isso permite o acesso EF para o valor da propriedade RowVersion além da chave de registro.

```csharp
public async Task<IActionResult> Delete(Department department)
```

Você também tiver alterado o nome do método de ação de `DeleteConfirmed` para `Delete`. O código de scaffolding usado o nome `DeleteConfirmed` para fornecer o método HttpPost uma assinatura exclusiva. (Requer métodos sobrecarregados tenha parâmetros de método diferente de CLR.) Agora que as assinaturas são exclusivas, você pode continuar com a convenção MVC e usar o mesmo nome para os métodos de exclusão HttpPost e HttpGet.

Se o departamento já foi excluído, o `AnyAsync` método retornará false e o aplicativo apenas voltará para o método de índice.

Se um erro de simultaneidade é detectado, o código exibe novamente a página de confirmação de exclusão e fornece um sinalizador que indica que ele deve exibir uma mensagem de erro de simultaneidade.

### <a name="update-the-delete-view"></a>Atualizar a exibição de exclusão

Em *Views/Departments/Delete.cshtml*, substitua o código scaffolding com o seguinte código que adiciona um campo de mensagem de erro e os campos ocultos para as propriedades DepartmentID e RowVersion. As alterações são realçadas.

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

Isso faz as seguintes alterações:

* Adiciona uma mensagem de erro entre o `h2` e `h3` títulos.

* Substitui FirstMidName com FullName no **administrador** campo.

* Remove o campo RowVersion.

* Adiciona um campo oculto para o `RowVersion` propriedade.

Executar o aplicativo e vá para a página de índice de departamentos. Com o botão direito do **excluir** hiperlink do departamento em inglês e selecione **abrir em nova guia**, clique na primeira guia o **editar** hiperlink para o departamento em inglês.

Na primeira janela, alterar um dos valores e clique em **salvar**:

![Página de edição de departamento após a alteração antes de excluir](concurrency/_static/edit-after-change-for-delete.png)

Na segunda guia, clique em **excluir**. Consulte a mensagem de erro de simultaneidade e os valores de departamento são atualizados com o que está atualmente no banco de dados.

![Página de confirmação de exclusão de departamento com erro de simultaneidade](concurrency/_static/delete-error.png)

Se você clicar em **excluir** novamente, você será redirecionado para a página de índice, que mostra que o departamento foi excluído.

## <a name="update-details-and-create-views"></a>Detalhes da atualização e criar modos de exibição

Opcionalmente, você pode limpar scaffolding código nos detalhes e criar modos de exibição.

Substitua o código em *Views/Departments/Details.cshtml* para excluir a coluna de RowVersion e mostrar o nome completo do administrador.

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

Substitua o código em *Views/Departments/Create.cshtml* para adicionar uma opção de selecionar na lista suspensa.

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>Resumo

Isso conclui a introdução à manipulação de conflitos de simultaneidade. Para obter mais informações sobre como lidar com simultaneidade no núcleo do EF, consulte [conflitos de simultaneidade](https://docs.microsoft.com/ef/core/saving/concurrency). O seguinte tutorial mostra como implementar a tabela por hierarquia herança para as entidades do aluno e do instrutor.

>[!div class="step-by-step"]
[Anterior](update-related-data.md)
[Próximo](inheritance.md)  
