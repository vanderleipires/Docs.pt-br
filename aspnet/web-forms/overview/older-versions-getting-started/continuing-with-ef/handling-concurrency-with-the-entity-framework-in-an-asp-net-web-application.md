---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Tratamento de simultaneidade com o Entity Framework 4.0 em um aplicativo ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo web Contoso University que é criado pelo Getting Started with a série de tutoriais do Entity Framework 4.0. EU...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: c22f06e68b15d3fbf13e9f2af0e19631abe076ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392731"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Tratamento de simultaneidade com o Entity Framework 4.0 em um aplicativo ASP.NET 4
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais baseia-se no aplicativo web Contoso University que é criado pela [Introdução ao Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você teria criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, você pode postá-los para o [Fórum do Entity Framework do ASP.NET](https://forums.asp.net/1227.aspx).


No tutorial anterior, você aprendeu como classificar e filtrar dados usando o `ObjectDataSource` controle e o Entity Framework. Este tutorial mostra as opções para tratamento de simultaneidade em um aplicativo web ASP.NET que usa o Entity Framework. Você criará uma nova página da web que é dedicado a atualizar as atribuições de escritório do instrutor. Você lidará com problemas de simultaneidade nessa página e na página de departamentos que você criou anteriormente.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Para Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflitos de simultaneidade

Um conflito de simultaneidade ocorre quando um usuário edita um registro e outro usuário edita o mesmo registro antes que a primeira alteração do usuário seja gravada no banco de dados. Se você não configurar o Entity Framework para detectar esses conflitos, última pessoa que atualiza o banco de dados substitui as alterações de outros usuários. Em muitos aplicativos, esse risco é aceitável e você não precisa configurar o aplicativo para lidar com conflitos de simultaneidade possíveis. (Se houver poucos usuários ou poucas atualizações ou se não for realmente crítico se algumas alterações forem substituídas, o custo de programação para simultaneidade poderá superar o benefício.) Se você não precisa se preocupar sobre conflitos de simultaneidade, você poderá ignorar este tutorial; os dois tutoriais restantes da série não dependem de qualquer coisa que você cria neste.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidade pessimista (bloqueio)

Se o aplicativo precisar evitar a perda acidental de dados em cenários de simultaneidade, uma maneira de fazer isso será usar bloqueios de banco de dados. Isso é chamado *simultaneidade pessimista*. Por exemplo, antes de ler uma linha de um banco de dados, você solicita um bloqueio para o acesso somente leitura ou de atualização. Se você bloquear uma linha para o acesso de atualização, nenhum outro usuário terá permissão para bloquear a linha para o acesso somente leitura ou de atualização, porque ele obterá uma cópia dos dados que estão sendo alterados. Se você bloquear uma linha para o acesso somente leitura, outros também poderão bloqueá-la para o acesso somente leitura, mas não para atualização.

Gerenciamento de bloqueios traz algumas desvantagens. Ele pode ser complexo de ser programado. Requer recursos de gerenciamento de banco de dados significativos e pode causar problemas de desempenho como o número de usuários de um aplicativo aumenta (ou seja, ele não ser bem dimensionada). Por esses motivos, nem todos os sistemas de gerenciamento de banco de dados dão suporte à simultaneidade pessimista. O Entity Framework fornece nenhum suporte interno para ele, e este tutorial não mostra como implementá-lo.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

É a alternativa à simultaneidade pessimista *simultaneidade otimista*. Simultaneidade otimista significa permitir que conflitos de simultaneidade ocorram e responder adequadamente se eles ocorrerem. Por exemplo, John executa o *Department.aspx* página, cliques a **editar** link para o departamento de histórico e reduz a **orçamento** quantidade de US $1,000,000.00 $ $ 125,000.00. (John administra um departamento concorrente e deseja liberar o dinheiro para seu próprio departamento.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Antes de José clica em **atualização**, Jane executa a mesma página, cliques a **editar** link para o departamento de histórico e, em seguida, as alterações a **data de início** campo 10/1/2011 para 1/1 / 1999. (Jane administra o departamento de histórico e deseja dar a ele mais tempo de serviço.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John clica **atualização** pela primeira vez, em seguida, Alice clica em **atualização**. Listas de agora do navegador de Jane a **orçamento** quantidade como $1,000,000.00, mas isso está incorreta porque a quantidade foi alterada por John para US $125,000.00.

Algumas das ações que você pode realizar nesse cenário incluem o seguinte:

- Controle qual propriedade um usuário modificou e atualize apenas as colunas correspondentes no banco de dados. No cenário de exemplo, nenhum dado é perdido, porque propriedades diferentes foram atualizadas pelos dois usuários. Na próxima vez que alguém navegar pelo departamento de histórico, verá 1/1/1999 e US $125,000.00. 

    Esse é o comportamento padrão no Entity Framework, e ele pode reduzir significativamente o número de conflitos que podem resultar em perda de dados. No entanto, esse comportamento não evita a perda de dados se forem feitas alterações concorrentes à mesma propriedade de uma entidade. Além disso, esse comportamento não é sempre possível; Quando você mapeia procedimentos armazenados para um tipo de entidade, todas as propriedades da entidade são atualizadas quando são feitas alterações à entidade no banco de dados.
- Você pode deixar a alteração de Jane substituir alterações de julio. Após clica de Jane **atualização**, o **orçamento** quantidade remonta para US $1,000,000.00. Isso é chamado de um cenário *O cliente vence* ou *O último vence*. (Os valores do cliente têm precedência sobre o que está no repositório de dados.)
- Você pode impedir que as alterações de Jane sejam atualizados no banco de dados. Normalmente, você seria exibir uma mensagem de erro, mostrar o estado atual dos dados e permitir que ela insira novamente suas alterações se ela ainda desejar fazê-las. Ainda mais, você pode automatizar o processo, salvando sua entrada e dar a ela uma oportunidade de reaplicá-lo sem a necessidade de redigitá-lo. Isso é chamado de um cenário *O armazenamento vence*. (Os valores do armazenamento de dados têm precedência sobre os valores enviados pelo cliente.)

### <a name="detecting-concurrency-conflicts"></a>Detectando conflitos de simultaneidade

No Entity Framework, você pode resolver conflitos manipulando `OptimisticConcurrencyException` as exceções que o Entity Framework gera. Para saber quando gerar essas exceções, o Entity Framework precisa poder detectar conflitos. Portanto, é necessário configurar o banco de dados e o modelo de dados de forma adequada. Algumas opções para habilitar a detecção de conflitos incluem as seguintes:

- No banco de dados, inclua uma coluna de tabela que pode ser usada para determinar quando uma linha foi alterada. Você pode configurar o Entity Framework para incluir essa coluna na `Where` cláusula SQL `Update` ou `Delete` comandos.

    Essa é a finalidade do `Timestamp` coluna o `OfficeAssignment` tabela.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    O tipo de dados do `Timestamp` também é chamada de coluna `Timestamp`. No entanto, a coluna, na verdade, não contém um valor de data ou hora. Em vez disso, o valor é um número sequencial que é incrementado sempre que a linha é atualizada. Em um `Update` ou `Delete` comando, o `Where` cláusula inclui original `Timestamp` valor. Se a linha que está sendo atualizada tiver sido alterada por outro usuário, o valor em `Timestamp` é diferente do valor original, portanto, o `Where` cláusula não retorna nenhuma linha a ser atualizada. Quando o Entity Framework não localiza nenhuma linha foi atualizada pela atual `Update` ou `Delete` de comando (ou seja, quando o número de linhas afetadas seja zero), ele interpreta isso como um conflito de simultaneidade.
- Configure o Entity Framework para incluir os valores originais de cada coluna na tabela de `Where` cláusula de `Update` e `Delete` comandos.

    Como a primeira opção, se nada na linha foi alterada desde que a linha foi lido pela primeira vez, o `Where` cláusula não retornará uma linha a ser atualizada, que o Entity Framework interpreta como um conflito de simultaneidade. Esse método é tão eficiente quanto usar um `Timestamp` de campo, mas pode ser ineficiente. Para tabelas de banco de dados que têm muitas colunas, isso pode resultar em grandes `Where` cláusulas, e, em um aplicativo web pode exigir que você mantenha grandes quantidades de estado. Manter grandes quantidades de estado pode afetar o desempenho do aplicativo porque ele requer recursos de servidor (por exemplo, o estado de sessão) ou deve ser incluído na página da web em si (por exemplo, o estado de exibição).

Neste tutorial, você irá adicionar tratamento de erros para conflitos de simultaneidade otimista para uma entidade que não tem uma propriedade de acompanhamento (a `Department` entidade) e para uma entidade que tem uma propriedade de controle (o `OfficeAssignment` entidade).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Tratamento de simultaneidade otimista sem uma propriedade de acompanhamento

Para implementar a simultaneidade otimista para o `Department` entidade, que não tem um rastreamento (`Timestamp`) propriedade, você concluirá as seguintes tarefas:

- Alterar o modelo de dados para habilitar a simultaneidade de acompanhamento para `Department` entidades.
- No `SchoolRepository` classe, manipular exceções de simultaneidade no `SaveChanges` método.
- No *Departments.aspx* página, manipular exceções de simultaneidade, exibindo uma mensagem para o usuário que a tentativa de alteração foram bem-sucedidos de aviso. O usuário pode ver os valores atuais e tente novamente as alterações se eles ainda são necessários.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Habilitando o modelo de dados de controle de simultaneidade

No Visual Studio, abra o aplicativo web Contoso University que você estava trabalhando no tutorial anterior desta série.

Abra *Schoolmodel*e no designer de modelo de dados, clique com botão direito a `Name` propriedade no `Department` entidade e clique **propriedades**. No **propriedades** janela, altere o `ConcurrencyMode` propriedade `Fixed`.

[![image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Faça o mesmo para as outras propriedades escalares de chave não primária (`Budget`, `StartDate`, e `Administrator`.) (Você não pode fazer isso para propriedades de navegação.) Isso especifica que sempre que o Entity Framework gera uma `Update` ou `Delete` comando SQL para atualizar o `Department` entidade no banco de dados, essas colunas (com valores originais) devem ser incluídas no `Where` cláusula. Se nenhuma linha for encontrada quando o `Update` ou `Delete` comando é executado, o Entity Framework gerará uma exceção de simultaneidade otimista.

Salve e feche o modelo de dados.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Tratamento de exceções de simultaneidade em DAL

Abra *SchoolRepository.cs* e adicione o seguinte `using` instrução para o `System.Data` namespace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Adicione a seguinte nova `SaveChanges` método, que lida com exceções de simultaneidade otimista:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Se um erro de simultaneidade ocorre quando este método é chamado, os valores de propriedade da entidade na memória são substituídos pelos valores no momento no banco de dados. A exceção de simultaneidade será gerada novamente para que a página da web possa tratá-lo.

No `DeleteDepartment` e `UpdateDepartment` métodos, substitua a chamada existente para `context.SaveChanges()` com uma chamada para `SaveChanges()` para invocar o novo método.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Tratamento de exceções de simultaneidade na camada de apresentação

Abra *Departments.aspx* e adicione um `OnDeleted="DepartmentsObjectDataSource_Deleted"` de atributo para o `DepartmentsObjectDataSource` controle. A marca de abertura para o controle agora será semelhante ao exemplo a seguir.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

No `DepartmentsGridView` controlar, especifique todas as colunas de tabela no `DataKeyNames` de atributo, conforme mostrado no exemplo a seguir. Observe que isso irá criar exibição muito grande campos de estado, que é um dos motivos por que usar um campo de controle geralmente é a maneira preferencial para controlar conflitos de simultaneidade.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Abra *Departments.aspx.cs* e adicione o seguinte `using` instrução para o `System.Data` namespace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Adicione o seguinte método novo, do controle de fonte de dados, você chamará `Updated` e `Deleted` manipuladores de eventos para tratamento de exceções de simultaneidade:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Esse código verifica o tipo de exceção e, se for uma exceção de simultaneidade, o código cria dinamicamente uma `CustomValidator` que por sua vez, exibe uma mensagem no controle de `ValidationSummary` controle.

Chame o novo método da `Updated` manipulador de eventos que você adicionou anteriormente. Além disso, crie um novo `Deleted` manipulador de eventos que chama o mesmo método (mas não fazer mais nada):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Teste de simultaneidade otimista na página de departamentos

Execute o *Departments.aspx* página.

[![image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Clique em **editar** em uma linha e altere o valor de **orçamento** coluna. (Lembre-se de que você só pode editar os registros que você criou para este tutorial, porque existente `School` registros do banco de dados contêm alguns dados inválidos. O registro para o departamento de economia é seguro para experimentar.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Abra uma nova janela do navegador e execute novamente a página (copiar a URL da caixa de endereço da primeira janela de navegador para a segunda janela do navegador).

[![image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Clique em **edite** na mesma linha que você editou anteriormente e altere o **orçamento** valor para algo diferente.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Na segunda janela do navegador, clique em **atualização**. O **orçamento** quantidade for alterada com êxito para esse novo valor.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Na primeira janela de navegador, clique em **atualização**. A atualização falhará. O **orçamento** quantidade é exibida novamente usando o valor definido na segunda janela do navegador e você verá uma mensagem de erro.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Tratamento de simultaneidade otimista usando uma propriedade de acompanhamento

Para lidar com a simultaneidade otimista para uma entidade que tem uma propriedade de controle, você concluirá as seguintes tarefas:

- Adicionar procedimentos armazenados ao modelo de dados para gerenciar `OfficeAssignment` entidades. (Propriedades de controle e os procedimentos armazenados não precisam ser usados juntos; eles são apenas agrupados juntos aqui para fins ilustrativos.)
- Adicionar métodos CRUD a DAL e a BLL para `OfficeAssignment` entidades, incluindo o código para tratar exceções de simultaneidade otimista em DAL.
- Crie uma página da web de atribuições de escritório.
- Testar a simultaneidade otimista na nova página da web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Adicionar OfficeAssignment procedimentos armazenados para o modelo de dados

Abra o *Schoolmodel* arquivo no designer de modelo, clique com botão direito na superfície de design e clique em **atualizar o modelo de banco de dados**. No **Add** guia da **Choose Your Database Objects** caixa de diálogo caixa, expanda **procedimentos armazenados** e selecione os três `OfficeAssignment` procedimentos armazenados (consulte a tela a seguir) e, em seguida, clique em **concluir**. (Esses procedimentos armazenados já estavam no banco de dados quando você baixou ou criou usando um script.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Clique com botão direito do `OfficeAssignment` entidade e selecione **mapeamento de procedimento armazenado**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Defina as **inserir**, **atualização**, e **excluir** procedimentos armazenados de funções para usar seus correspondentes. Para o `OrigTimestamp` parâmetro do `Update` funcionar, defina as **propriedade** para `Timestamp` e selecione o **usar o valor Original** opção.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Quando o Entity Framework chama o `UpdateOfficeAssignment` procedimento armazenado, ele passará o valor original do `Timestamp` coluna o `OrigTimestamp` parâmetro. O procedimento armazenado usa esse parâmetro em seu `Where` cláusula:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

O procedimento armazenado também seleciona o novo valor da `Timestamp` coluna após a atualização para que o Entity Framework possa manter o `OfficeAssignment` entidade que está na memória em sincronia com a linha correspondente do banco de dados.

(Observe que o procedimento armazenado para excluir uma atribuição de escritório não tem um `OrigTimestamp` parâmetro. Por isso, o Entity Framework não é possível verificar que uma entidade não é alterada antes de excluí-lo.)

Salve e feche o modelo de dados.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Adição de métodos de OfficeAssignment a DAL

Abra *ISchoolRepository.cs* e adicione os seguintes métodos CRUD para o `OfficeAssignment` conjunto de entidades:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Adicionar novos métodos a seguir para *SchoolRepository.cs*. No `UpdateOfficeAssignment` , é chamada de método local `SaveChanges` método em vez de `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

No projeto de teste, abra *MockSchoolRepository.cs* e adicione o seguinte `OfficeAssignment` coleta e métodos CRUD a ele. (O repositório fictício deve implementar a interface do repositório, ou a solução não será compilado.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Adição de métodos de OfficeAssignment a BLL

No projeto principal, abra *SchoolBL.cs* e adicione os seguintes métodos CRUD para o `OfficeAssignment` do conjunto de entidades a ele:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Criando uma página da Web OfficeAssignments

Criar uma nova página da web que usa o *Master* página mestra e nomeie-o *OfficeAssignments.aspx*. Adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Observe que, nos `DataKeyNames` de atributo, a marcação especifica o `Timestamp` propriedade, bem como a chave de registro (`InstructorID`). Especificando propriedades no `DataKeyNames` atributo faz com que o controle para salvá-los no estado do controle (que é semelhante ao estado de exibição) para que os valores originais estejam disponíveis durante o processamento de postback.

Se você não salvar a `Timestamp` valor, o Entity Framework não teria-lo para o `Where` cláusula do SQL `Update` comando. Consequentemente, nada seria encontrado ao atualizar. Como resultado, o Entity Framework gerará uma exceção de simultaneidade otimista sempre que um `OfficeAssignment` entidade é atualizada.

Abra *OfficeAssignments.aspx.cs* e adicione o seguinte `using` instrução para a camada de acesso a dados:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Adicione o seguinte `Page_Init` método, que habilita a funcionalidade de dados dinâmicos. Também adicione o seguinte manipulador para o `ObjectDataSource` do controle `Updated` eventos para verificar se há erros de simultaneidade:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Teste de simultaneidade otimista na página OfficeAssignments

Execute o *OfficeAssignments.aspx* página.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Clique em **edite** em uma linha e altere o valor na **local** coluna.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Abra uma nova janela do navegador e execute novamente a página (copiar a URL da primeira janela de navegador para a segunda janela do navegador).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Clique em **edite** na mesma linha que você editou anteriormente e altere o **local** valor para algo diferente.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Na segunda janela do navegador, clique em **atualização**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Alterne para a primeira janela do navegador e clique em **atualização**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Você verá uma mensagem de erro e o **local** valor foi atualizado para mostrar o valor que você tenha feito na segunda janela do navegador.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Tratamento de simultaneidade com o controle EntityDataSource

O `EntityDataSource` controle inclui lógica interna que reconhece as configurações de simultaneidade no modelo de dados e identificadores de atualizar e excluir operações adequadamente. No entanto, assim como acontece com todas as exceções, você deve tratar `OptimisticConcurrencyException` exceções por conta própria a fim de fornecer uma mensagem de erro amigável.

Em seguida, você configurará o *Courses.aspx* página (que usa um `EntityDataSource` controle) para permitir que a atualização e exclusão de operações e para exibir uma mensagem de erro se ocorrer um conflito de simultaneidade. O `Course` entidade não tem uma simultaneidade de acompanhamento de coluna, portanto, você usará o mesmo método que você fez com o `Department` entidade: controlar os valores de todas as propriedades não chave.

Abra o *Schoolmodel* arquivo. Para as propriedades não-chave da `Course` entidade (`Title`, `Credits`, e `DepartmentID`), defina a **modo de simultaneidade** propriedade `Fixed`. Em seguida, salve e feche o modelo de dados.

Abra o *Courses.aspx* página e faça as seguintes alterações:

- No `CoursesEntityDataSource` de controle, adicione `EnableUpdate="true"` e `EnableDelete="true"` atributos. Agora a marca de abertura para o controle é semelhante ao exemplo a seguir:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- No `CoursesGridView` controlar, altere o `DataKeyNames` valor ao atributo `"CourseID,Title,Credits,DepartmentID"`. Em seguida, adicione uma `CommandField` elemento para o `Columns` elemento mostra **editar** e **excluir** botões (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). O `GridView` controle agora é semelhante ao exemplo a seguir:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Execute a página e criar uma situação de conflito, como fez anteriormente na página de departamentos. Execute a página em duas janelas de navegador, clique em **editar** na mesma linha em cada janela e fazer uma alteração diferente em cada um. Clique em **atualização** em uma janela e clique **atualização** na outra janela. Quando você clica em **atualização** na segunda vez, você ver a página de erro que resulta de uma exceção sem tratamento de simultaneidade.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Você manipular esse erro de uma maneira muito semelhante ao modo como você manipuladas-lo para o `ObjectDataSource` controle. Abra o *Courses.aspx* página e, na `CoursesEntityDataSource` controle, especificam os manipuladores para o `Deleted` e `Updated` eventos. Agora a marca de abertura do controle é semelhante ao exemplo a seguir:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Antes do `CoursesGridView` de controle, adicione o seguinte `ValidationSummary` controle:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

Na *Courses.aspx.cs*, adicione uma `using` instrução para o `System.Data` namespace, adicione um método que verifica para exceções de simultaneidade e adicione manipuladores para o `EntityDataSource` do controle `Updated` e `Deleted`manipuladores. O código será a seguinte aparência:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

A única diferença entre esse código e o que você fez o `ObjectDataSource` controle é que, nesse caso, a exceção de simultaneidade está no `Exception` propriedade do objeto de argumentos de evento em vez de nessa exceção `InnerException` propriedade.

Execute a página e criar um conflito de simultaneidade novamente. Neste momento, você verá uma mensagem de erro:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Isso conclui a introdução à manipulação de conflitos de simultaneidade. O próximo tutorial fornecerá diretrizes sobre como melhorar o desempenho em um aplicativo web que usa o Entity Framework.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Próximo](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
