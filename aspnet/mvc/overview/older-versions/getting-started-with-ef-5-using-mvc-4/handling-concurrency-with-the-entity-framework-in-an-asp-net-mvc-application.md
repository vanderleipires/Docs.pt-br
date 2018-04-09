---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Tratamento de simultaneidade com o Entity Framework em um aplicativo ASP.NET MVC (7 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 609f493845f1d00a47d175a1b623a7f4866d191e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Tratamento de simultaneidade com o Entity Framework em um aplicativo ASP.NET MVC (7 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais do início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece aqui.
> 
> > [!NOTE] 
> > 
> > Se você tiver um problema que não é possível resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tentar reproduzir o problema. Geralmente, você pode encontrar a solução para o problema, comparando o seu código para o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nos dois tutoriais anteriores, você trabalhou com dados relacionados. Este tutorial mostra como tratar a simultaneidade. Você criará páginas da web que funcionam com o `Department` entidade e as páginas que editar e excluir `Department` entidades controlará os erros de simultaneidade. As ilustrações a seguir mostram as páginas de índice e Delete, incluindo algumas mensagens que são exibidas se ocorrer um conflito de simultaneidade.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflitos de simultaneidade

Um conflito de simultaneidade ocorre quando um usuário exibe dados de uma entidade para editá-los e, em seguida, outro usuário atualiza os mesmos dados da entidade antes que a primeira alteração do usuário seja gravada no banco de dados. Se você não habilitar a detecção desses conflitos, a última pessoa que atualizar o banco de dados substituirá as outras alterações do usuário. Em muitos aplicativos, esse risco é aceitável: se houver poucos usuários ou poucas atualizações ou se não for realmente crítico se algumas alterações forem substituídas, o custo de programação para simultaneidade poderá superar o benefício. Nesse caso, você não precisa configurar o aplicativo para lidar com conflitos de simultaneidade.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidade pessimista (bloqueio)

Se o aplicativo precisar evitar a perda acidental de dados em cenários de simultaneidade, uma maneira de fazer isso será usar bloqueios de banco de dados. Isso é chamado de *simultaneidade pessimista*. Por exemplo, antes de ler uma linha de um banco de dados, você solicita um bloqueio para o acesso somente leitura ou de atualização. Se você bloquear uma linha para o acesso de atualização, nenhum outro usuário terá permissão para bloquear a linha para o acesso somente leitura ou de atualização, porque ele obterá uma cópia dos dados que estão sendo alterados. Se você bloquear uma linha para o acesso somente leitura, outros também poderão bloqueá-la para o acesso somente leitura, mas não para atualização.

O gerenciamento de bloqueios traz desvantagens. Ele pode ser complexo de ser programado. Requer recursos de gerenciamento de banco de dados, e pode causar problemas de desempenho como o número de usuários de um aplicativo aumenta (ou seja, ele não é bem dimensionável). Por esses motivos, nem todos os sistemas de gerenciamento de banco de dados dão suporte à simultaneidade pessimista. O Entity Framework fornece sem suporte interno para ele, e este tutorial não mostra como implementá-la.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

A alternativa de simultaneidade pessimista é *simultaneidade otimista*. Simultaneidade otimista significa permitir que conflitos de simultaneidade ocorram e responder adequadamente se eles ocorrerem. Por exemplo, João executa a página Editar departamentos, alterações de **orçamento** valor para o departamento de inglês de US $350,000.00 $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Antes de José clica em **salvar**, Jane executa a mesma página e alterações de **Start Date** campo de 1/9/2007, 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

José clica em **salvar** primeiro e vê suas alterações quando o navegador retorna à página de índice, Jane, em seguida, clica em **salvar**. O que acontece em seguida é determinado pela forma como você lida com conflitos de simultaneidade. Algumas das opções incluem o seguinte:

- Controle qual propriedade um usuário modificou e atualize apenas as colunas correspondentes no banco de dados. No cenário de exemplo, nenhum dado é perdido, porque propriedades diferentes foram atualizadas pelos dois usuários. Na próxima vez que alguém procura o departamento de inglês, ele verá as alterações de John e Jane — uma data de início de 8/8/2013 e uma alocação de Zero dólares.

    Esse método de atualização pode reduzir a quantidade de conflitos que podem resultar em perda de dados, mas ele não poderá evitar a perda de dados se forem feitas alterações concorrentes à mesma propriedade de uma entidade. Se o Entity Framework funciona dessa maneira depende de como o código de atualização é implementado. Geralmente, isso não é prático em um aplicativo Web, porque pode exigir que você mantenha grandes quantidades de estado para manter o controle de todos os valores de propriedade originais de uma entidade, bem como novos valores. Manter grandes quantidades de estado pode afetar o desempenho do aplicativo porque ele requer recursos de servidor ou deve ser incluído na própria página da web (por exemplo, nos campos ocultos).
- Você pode deixar a alteração de Jane substituir a alteração de João. Na próxima vez que alguém procura o departamento de inglês, ele verá 8/8/2013 e o valor de US $350,000.00 restaurado. Isso é chamado de um cenário *O cliente vence* ou *O último vence*. (Os valores do cliente têm precedência sobre o que está no repositório de dados.) Conforme observado na introdução a esta seção, se você não fizer nenhuma codificação para a manipulação de simultaneidade, isso ocorrerá automaticamente.
- Você pode impedir a alteração de Jane seja atualizado no banco de dados. Normalmente, você poderia exibir uma mensagem de erro, mostrar o estado atual dos dados e permitir para reaplicar as alterações se ainda desejar torná-los. Isso é chamado de um cenário *O armazenamento vence*. (Os valores do armazenamento de dados têm precedência sobre os valores enviados pelo cliente.) Você implementará o cenário O armazenamento vence neste tutorial. Esse método garante que nenhuma alteração é substituída sem que um usuário seja alertado sobre o que está acontecendo.

### <a name="detecting-concurrency-conflicts"></a>Detectando conflitos de simultaneidade

Você pode resolver conflitos manipulando [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) exceções que gera o Entity Framework. Para saber quando gerar essas exceções, o Entity Framework precisa poder detectar conflitos. Portanto, é necessário configurar o banco de dados e o modelo de dados de forma adequada. Algumas opções para habilitar a detecção de conflitos incluem as seguintes:

- Na tabela de banco de dados, inclua uma coluna de acompanhamento que pode ser usada para determinar quando uma linha é alterada. Você pode configurar o Entity Framework para incluir essa coluna no `Where` cláusula SQL `Update` ou `Delete` comandos.

    O tipo de dados da coluna de rastreamento é normalmente [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). O [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) valor é um número sequencial que tem incrementado toda vez que a linha é atualizada. Em um `Update` ou `Delete` comando, o `Where` cláusula inclui o valor original da coluna de rastreamento (a versão de linha original). Se a linha que está sendo atualizada tiver sido alterada por outro usuário, o valor no `rowversion` coluna é diferente do valor original, portanto, o `Update` ou `Delete` instrução não é possível localizar a linha a ser atualizada porque o `Where` cláusula. Quando o Entity Framework localiza nenhuma linha foi atualizada pelo `Update` ou `Delete` de comando (ou seja, quando o número de linhas afetadas será zero), ele interpreta que como um conflito de simultaneidade.
- Configurar o Entity Framework para incluir os valores originais de cada coluna na tabela de `Where` cláusula de `Update` e `Delete` comandos.

    Como a primeira opção, se nada na linha foi alterado desde que a linha foi lido pela primeira vez, o `Where` cláusula não retornará uma linha a ser atualizada, que o Entity Framework interpreta como um conflito de simultaneidade. Para tabelas de banco de dados que têm um número de colunas, essa abordagem pode resultar em grandes `Where` cláusulas e pode exigir que você mantenha grandes quantidades de estado. Conforme observado anteriormente, manter grandes quantidades de estado pode afetar o desempenho do aplicativo porque ele requer recursos de servidor ou deve ser incluído na própria página da web. Portanto, essa abordagem geralmente não é recomendado e não é o método usado neste tutorial.

    Se você quiser implementar essa abordagem, você precisa marcar todas as propriedades de chave não primária da entidade que você deseja controlar a simultaneidade para adicionando o [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) atributo a eles. Alteração permite que o Entity Framework incluir todas as colunas no SQL `WHERE` cláusula de `UPDATE` instruções.

O restante deste tutorial, você adicionará um [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) controle de propriedade para o `Department` entidade, criar um controlador e modos de exibição e de teste para verificar se tudo está funcionando corretamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Adicionar uma propriedade de simultaneidade otimista para a entidade de departamento

Em *Models\Department.cs*, adicionar uma propriedade de controle chamada `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

O [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) atributo especifica que esta coluna será incluída no `Where` cláusula de `Update` e `Delete` comandos enviados para o banco de dados. O atributo é chamado [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) porque versões anteriores do SQL Server usaram um SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) tipo de dados antes do SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) substituído. O tipo de .net para `rowversion` é uma matriz de bytes. Se você preferir usar a API fluente, você pode usar o [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) método para especificar a propriedade de controle, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Adicionando uma propriedade, você alterou o modelo de banco de dados e, portanto, precisa fazer outra migração. No PMC (Console do Gerenciador de Pacotes), Insira os seguintes comandos:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Criar um controlador de departamento

Criar um `Department` controlador e modos de exibição da mesma maneira que outros controladores, usando as seguintes configurações:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

Em *Controllers\DepartmentController.cs*, adicione um `using` instrução:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Alterar "LastName" para "FullName" em qualquer lugar no arquivo (quatro ocorrências) para que o departamento administrador listas suspensas conterá o nome completo do instrutor em vez de apenas o último nome.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Substitua o código existente para o `HttpPost` `Edit` método com o código a seguir:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

O modo de exibição armazenará original `RowVersion` valor em um campo oculto. Quando o associador de modelo cria o `department` instância, esse objeto terá original `RowVersion` valor da propriedade e os novos valores para as propriedades, conforme inseridos pelo usuário na página de edição. Em seguida, quando o Entity Framework cria um SQL `UPDATE` de comando, que incluirá o comando um `WHERE` cláusula que procura uma linha que tenha o original `RowVersion` valor.

Se nenhuma linha é afetada pelo `UPDATE` comando (linhas não ter original `RowVersion` valor), Entity Framework lança um `DbUpdateConcurrencyException` exceção e o código no `catch` bloco é afetado `Department` entidade da exceção objeto. Esta entidade tem os valores lidos do banco de dados e os novos valores inseridos pelo usuário:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Em seguida, o código adiciona uma mensagem de erro personalizada para cada coluna com valores do banco de dados diferentes do que o usuário especificado na página Editar:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Uma mensagem de erro explica o que aconteceu e o que fazer com ele:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Por fim, o código define o `RowVersion` valor o `Department` objeto para o novo valor é recuperado do banco de dados. Esse novo valor `RowVersion` será armazenado no campo oculto quando a página Editar for exibida novamente, e na próxima vez que o usuário clicar em **Salvar**, somente os erros de simultaneidade que ocorrem desde a nova exibição da página Editar serão capturados.

Em *Views\Department\Edit.cshtml*, adicionar um campo oculto para salvar o `RowVersion` valor da propriedade, imediatamente após o campo oculto para o `DepartmentID` propriedade:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

Em *Views\Department\Index.cshtml*, substitua o código existente com o código a seguir para mover os links de linha à esquerda e alterar os cabeçalhos de título e colunas de página para exibir `FullName` em vez de `LastName` no **Administrador** coluna:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Tratamento de simultaneidade otimista de teste

Executar o site e clique em **departamentos**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Clique com botão direito do **editar** hiperlink Kim Abercrombie e selecione **abrir em nova guia,** , em seguida, clique no **editar** hiperlink para Kim Abercrombie. As duas janelas exibem as mesmas informações.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Alterar um campo na primeira janela de navegador e clique em **salvar**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

O navegador mostra a página Índice com o valor alterado.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Altere qualquer campo na segunda janela do navegador e clique em **salvar**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Clique em **salvar** na segunda janela do navegador. Você verá uma mensagem de erro:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Clique em **Salvar** novamente. O valor inserido no segundo navegador é salva com o valor original dos dados de que alteração no navegador primeiro. Você verá os valores salvos quando a página Índice for exibida.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Atualizando a página de exclusão

Para a página Excluir, o Entity Framework detecta conflitos de simultaneidade causados pela edição por outra pessoa do departamento de maneira semelhante. Quando o `HttpGet` `Delete` método exibe o modo de confirmação, o modo de exibição inclui original `RowVersion` valor em um campo oculto. Que valor estará disponível para o `HttpPost` `Delete` método é chamado quando o usuário confirmar a exclusão. Quando o Entity Framework cria SQL `DELETE` de comando, ele inclui um `WHERE` cláusula com o original `RowVersion` valor. Se os resultados do comando em zero linhas afetado (ou seja, a linha foi alterada depois que a página de confirmação de exclusão foi exibida), é gerada uma exceção de simultaneidade e o `HttpGet Delete` método for chamado com um sinalizador de erro definido como `true` para exibir novamente o página de confirmação com uma mensagem de erro. Também é possível que o zero linhas foram afetadas porque a linha foi excluída por outro usuário, portanto, nesse caso, uma mensagem de erro é exibida.

Em *DepartmentController.cs*, substitua o `HttpGet` `Delete` método com o código a seguir:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

O método aceita um parâmetro opcional que indica se a página está sendo exibida novamente após um erro de simultaneidade. Se esse sinalizador for `true`, uma mensagem de erro é enviada para o modo de exibição usando um `ViewBag` propriedade.

Substitua o código no `HttpPost` `Delete` método (chamado `DeleteConfirmed`) com o código a seguir:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

No código gerado por scaffolding que acabou de ser substituído, esse método aceitou apenas uma ID de registro:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Você alterou esse parâmetro para um `Department` instância de entidade criada pelo associador de modelo. Isso fornece acesso para o `RowVersion` o valor da propriedade além da chave de registro.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Você também alterou o nome do método de ação de `DeleteConfirmed` para `Delete`. O código de scaffolding chamado o `HttpPost` `Delete` método `DeleteConfirmed` para dar o `HttpPost` método uma assinatura exclusiva. (O CLR exige que métodos sobrecarregados tenham diferentes parâmetros de método.) Agora que as assinaturas são exclusivas, você pode continuar com a convenção MVC e usar o mesmo nome para o `HttpPost` e `HttpGet` excluir métodos.

Se um erro de simultaneidade é capturado, o código exibe novamente a página Confirmação de exclusão e fornece um sinalizador que indica que ela deve exibir uma mensagem de erro de simultaneidade.

Em *Views\Department\Delete.cshtml*, substitua o código com o código a seguir que faz parte da formatação de scaffolding alterações e adiciona um campo de mensagem de erro. As alterações são realçadas.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Esse código adiciona uma mensagem de erro entre o `h2` e `h3` cabeçalhos:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Ele substitui `LastName` com `FullName` no `Administrator` campo:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Por fim, ele adiciona campos ocultos para o `DepartmentID` e `RowVersion` propriedades após o `Html.BeginForm` instrução:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Execute a página de índice de departamentos. Clique com botão direito do **excluir** hiperlink do departamento em inglês e selecione **abrir em nova janela,** clique na primeira janela de **editar** hiperlink para o inglês departamento.

Na primeira janela, alterar um dos valores e clique em **salvar** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

A página de índice confirma a alteração.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Na segunda janela, clique em **excluir**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Você verá a mensagem de erro de simultaneidade e os valores de Departamento serão atualizados com o que está atualmente no banco de dados.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Se você clicar em **Excluir** novamente, será redirecionado para a página Índice, que mostra que o departamento foi excluído.

## <a name="summary"></a>Resumo

Isso conclui a introdução à manipulação de conflitos de simultaneidade. Para obter informações sobre outras maneiras de tratar vários cenários de simultaneidade, consulte [padrões de simultaneidade otimista](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) e [trabalhar com valores de propriedade](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) no blog da equipe do Entity Framework. O seguinte tutorial mostra como implementar a herança de tabela por hierarquia para o `Instructor` e `Student` entidades.

Links para outros recursos do Entity Framework podem ser encontradas no [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Próximo](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
