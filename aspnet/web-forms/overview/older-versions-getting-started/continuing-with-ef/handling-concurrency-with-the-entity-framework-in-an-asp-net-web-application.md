---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Tratamento de simultaneidade com o Entity Framework 4.0 em um aplicativo ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Esta série de tutorial se baseia no aplicativo web Contoso Universidade que é criado pelo guia de Introdução com a série de tutoriais do Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: f40695270006e4f8b0c9ad8e94049e5239f06e63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Tratamento de simultaneidade com o Entity Framework 4.0 em um aplicativo ASP.NET 4
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo da web Contoso Universidade que é criado pelo [guia de Introdução com o Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você pode ter sido criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série tutorial completo. Se você tiver dúvidas sobre os tutoriais, você poderá postá-los para o [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


No tutorial anterior, você aprendeu como classificar e filtrar dados usando o `ObjectDataSource` controle e o Entity Framework. Este tutorial mostra opções para tratar de simultaneidade em um aplicativo web ASP.NET que usa o Entity Framework. Você criará uma nova página da web que é dedicado a atualização das atribuições do instrutor office. Você lida com problemas de simultaneidade nessa página e na página de departamentos que você criou anteriormente.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflitos de simultaneidade

Um conflito de simultaneidade ocorre quando um usuário edita um registro e outro usuário edita o mesmo registro antes que a primeira alteração do usuário seja gravada no banco de dados. Se você não configurar o Entity Framework para detectar esses conflitos, última pessoa que atualiza o banco de dados substitui as outras alterações do usuário. Em muitos aplicativos, esse risco é aceitável, e você não precisa configurar o aplicativo para lidar com conflitos de simultaneidade possíveis. (Se houver poucos usuários ou algumas atualizações, ou se não for mais importante se algumas alterações são substituídas, o custo de programação para simultaneidade pode sobrecarregar o benefício.) Se você não precisa se preocupar sobre conflitos de simultaneidade, você poderá ignorar este tutorial; os dois tutoriais restantes na série não dependem de qualquer coisa que você cria neste.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidade pessimista (bloqueio)

Se o aplicativo precisar evitar a perda acidental de dados em cenários de simultaneidade, uma maneira de fazer isso será usar bloqueios de banco de dados. Isso é chamado de *simultaneidade pessimista*. Por exemplo, antes de ler uma linha de um banco de dados, você solicita um bloqueio para o acesso somente leitura ou de atualização. Se você bloquear uma linha para o acesso de atualização, nenhum outro usuário terá permissão para bloquear a linha para o acesso somente leitura ou de atualização, porque ele obterá uma cópia dos dados que estão sendo alterados. Se você bloquear uma linha para o acesso somente leitura, outros também poderão bloqueá-la para o acesso somente leitura, mas não para atualização.

Gerenciar bloqueios apresenta algumas desvantagens. Ele pode ser complexo de ser programado. Requer recursos de gerenciamento de banco de dados, e pode causar problemas de desempenho como o número de usuários de um aplicativo aumenta (ou seja, ele não é bem dimensionável). Por esses motivos, nem todos os sistemas de gerenciamento de banco de dados dão suporte à simultaneidade pessimista. O Entity Framework fornece sem suporte interno para ele, e este tutorial não mostra como implementá-la.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

A alternativa de simultaneidade pessimista é *simultaneidade otimista*. Simultaneidade otimista significa permitir que conflitos de simultaneidade ocorram e responder adequadamente se eles ocorrerem. Por exemplo, João executa o *Department.aspx* página, cliques o **editar** link para o departamento de histórico e reduz a **orçamento** valor de US $1,000,000.00 a $ 125,000.00. (João administra um departamento concorrente e deseja liberar dinheiro para seu próprio departamento.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Antes de José clica em **atualização**, Jane executa a mesma página, cliques de **editar** link para o departamento de histórico e, em seguida, as alterações a **data de início** campo de 1/10/2011 para 1/1 / 1999. (Jane administra o departamento de histórico e quer dar mais tempo de serviço.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

José clica em **atualização** em primeiro lugar, em seguida, clica Jane **atualização**. Listas de agora do navegador de Jane o **orçamento** valor como $1,000,000.00, mas isso está incorreto porque a quantidade foi alterada por João para $125,000.00.

Algumas das ações que você pode executar este cenário incluem o seguinte:

- Controle qual propriedade um usuário modificou e atualize apenas as colunas correspondentes no banco de dados. No cenário de exemplo, nenhum dado é perdido, porque propriedades diferentes foram atualizadas pelos dois usuários. Na próxima vez que alguém procura o departamento de histórico, eles verão 1/1/1999 e US $125,000.00. 

    Esse é o comportamento padrão no Entity Framework, e ele pode reduzir significativamente o número de conflitos que podem resultar em perda de dados. No entanto, esse comportamento não evita perda de dados se forem feitas alterações concorrentes a mesma propriedade de uma entidade. Além disso, esse comportamento não é sempre possível; Quando você mapeia procedimentos armazenados para um tipo de entidade, todas as propriedades da entidade são atualizadas quando todas as alterações da entidade são feitas no banco de dados.
- Você pode deixar a alteração de Jane substituir a alteração de João. Após clica Jane **atualização**, o **orçamento** quantidade volta para $1,000,000.00. Isso é chamado de um cenário *O cliente vence* ou *O último vence*. (Os valores do cliente têm precedência sobre o que está no repositório de dados.)
- Você pode impedir a alteração de Jane seja atualizado no banco de dados. Normalmente, poderia exibir uma mensagem de erro, mostrar o estado atual dos dados e permitir que ela insira novamente suas alterações se ainda desejar torná-los. Ainda mais, você pode automatizar o processo salvando sua entrada e dando a ela uma oportunidade para reaplicá-lo sem a necessidade de redigitá-lo. Isso é chamado de um cenário *O armazenamento vence*. (Os valores do armazenamento de dados têm precedência sobre os valores enviados pelo cliente.)

### <a name="detecting-concurrency-conflicts"></a>Detectando conflitos de simultaneidade

No Entity Framework, você pode resolver conflitos manipulando `OptimisticConcurrencyException` exceções que gera o Entity Framework. Para saber quando gerar essas exceções, o Entity Framework precisa poder detectar conflitos. Portanto, é necessário configurar o banco de dados e o modelo de dados de forma adequada. Algumas opções para habilitar a detecção de conflitos incluem as seguintes:

- No banco de dados e inclua uma coluna de tabela que pode ser usada para determinar quando uma linha foi alterada. Você pode configurar o Entity Framework para incluir essa coluna no `Where` cláusula SQL `Update` ou `Delete` comandos.

    Essa é a finalidade do `Timestamp` coluna o `OfficeAssignment` tabela.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    O tipo de dados de `Timestamp` coluna também é chamada de `Timestamp`. No entanto, a coluna, na verdade, não contém um valor de data ou hora. Em vez disso, o valor é um número sequencial que tem incrementado toda vez que a linha é atualizada. Em um `Update` ou `Delete` comando, o `Where` cláusula inclui original `Timestamp` valor. Se a linha que está sendo atualizada tiver sido alterada por outro usuário, o valor em `Timestamp` é diferente do valor original, portanto, o `Where` cláusula não retorna nenhuma linha para atualizar. Quando o Entity Framework localiza nenhuma linha foi atualizada por atual `Update` ou `Delete` de comando (ou seja, quando o número de linhas afetadas será zero), ele interpreta que como um conflito de simultaneidade.
- Configurar o Entity Framework para incluir os valores originais de cada coluna na tabela de `Where` cláusula de `Update` e `Delete` comandos.

    Como a primeira opção, se nada na linha foi alterado desde que a linha foi lido pela primeira vez, o `Where` cláusula não retornará uma linha a ser atualizada, que o Entity Framework interpreta como um conflito de simultaneidade. Esse método é tão eficiente quanto usando um `Timestamp` campo, mas pode ser ineficiente. Para tabelas de banco de dados que têm várias colunas, isso pode resultar em grandes `Where` cláusulas, e em um aplicativo web pode exigir que você mantenha grandes quantidades de estado. Manter grandes quantidades de estado pode afetar o desempenho do aplicativo porque ele requer recursos de servidor (por exemplo, o estado de sessão) ou deve ser incluído na página da web em si (por exemplo, o estado de exibição).

Neste tutorial, você irá adicionar o tratamento de erros para conflitos de simultaneidade otimista para uma entidade que não tem uma propriedade de controle (o `Department` entidade) e para uma entidade que tem uma propriedade de controle (o `OfficeAssignment` entidade).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Tratamento de simultaneidade otimista sem uma propriedade de controle

Para implementar a simultaneidade otimista para o `Department` entidade, que não tem um controle (`Timestamp`) a propriedade, você concluirá as seguintes tarefas:

- Alterar o modelo de dados para habilitar a simultaneidade de controle para `Department` entidades.
- No `SchoolRepository` classe manipular exceções de simultaneidade no `SaveChanges` método.
- No *Departments.aspx* página manipular exceções de simultaneidade exibindo uma mensagem para o usuário avisando que a tentativa de alteração foi bem-sucedida. O usuário pode ver os valores atuais e repita as alterações se eles ainda são necessários.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Habilitar o modelo de dados de controle de simultaneidade

No Visual Studio, abra o aplicativo web Contoso Universidade que você estava trabalhando no tutorial anterior nesta série.

Abra *SchoolModel.edmx*e no designer de modelo de dados, clique o `Name` propriedade no `Department` entidade e clique **propriedades**. No **propriedades** janela, altere o `ConcurrencyMode` propriedade `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Faça o mesmo para as outras propriedades escalares de chave não primária (`Budget`, `StartDate`, e `Administrator`.) (Você não pode fazer isso para propriedades de navegação.) Especifica que sempre que o Entity Framework gera um `Update` ou `Delete` comando SQL para atualizar o `Department` entidade no banco de dados, essas colunas (com valores originais) devem ser incluídas no `Where` cláusula. Se nenhuma linha for encontrada quando o `Update` ou `Delete` comando é executado, o Entity Framework lançará uma exceção de simultaneidade otimista.

Salve e feche o modelo de dados.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Tratamento de exceções de simultaneidade em DAL

Abra *SchoolRepository.cs* e adicione o seguinte `using` instrução para o `System.Data` namespace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Adicione o seguinte novo `SaveChanges` método que manipula exceções de simultaneidade otimista:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Se ocorrer um erro de simultaneidade quando este método é chamado, os valores de propriedade da entidade na memória são substituídos pelos valores no momento no banco de dados. A exceção de simultaneidade é lançada novamente para que a página da web possa tratá-la.

No `DeleteDepartment` e `UpdateDepartment` métodos, substitua a chamada existente para `context.SaveChanges()` com uma chamada para `SaveChanges()` para invocar o método de novo.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Tratamento de exceções de simultaneidade na camada de apresentação

Abra *Departments.aspx* e adicione um `OnDeleted="DepartmentsObjectDataSource_Deleted"` de atributo para o `DepartmentsObjectDataSource` controle. A marca de abertura para o controle agora será parecida com o exemplo a seguir.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

No `DepartmentsGridView` controlar, especificar todas as colunas da tabela no `DataKeyNames` de atributo, conforme mostrado no exemplo a seguir. Observe que isso irá criar exibição muito grande campos de estado, que é um motivo por que usar um campo de controle geralmente é a melhor maneira de controlar conflitos de simultaneidade.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Abra *Departments.aspx.cs* e adicione o seguinte `using` instrução para o `System.Data` namespace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Adicione o seguinte método novo, você chamará a partir do controle de fonte de dados `Updated` e `Deleted` manipuladores de eventos para tratamento de exceções de simultaneidade:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Esse código verifica o tipo de exceção, e se é uma exceção de simultaneidade, o código cria dinamicamente um `CustomValidator` que por sua vez exibe uma mensagem no controle de `ValidationSummary` controle.

Chamar o novo método do `Updated` manipulador de eventos que você adicionou anteriormente. Além disso, crie um novo `Deleted` manipulador de eventos que chama o mesmo método (mas não fazer nada):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Teste de simultaneidade otimista na página de departamentos

Execute o *Departments.aspx* página.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Clique em **editar** em uma linha e altere o valor de **orçamento** coluna. (Lembre-se de que você só pode editar os registros que você criou para este tutorial, porque existente `School` registros do banco de dados contêm alguns dados inválidos. O registro para o departamento de economia é seguro para experimentar.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Abra uma nova janela do navegador e execute novamente a página (copiar a URL na caixa de endereço da primeira janela de navegador para a segunda janela do navegador).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Clique em **editar** na mesma linha é editado anteriormente e altere o **orçamento** valor para algo diferente.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Na segunda janela do navegador, clique em **atualização**. O **orçamento** quantidade é alterada com êxito para esse novo valor.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Na primeira janela de navegador, clique em **atualização**. A atualização falhará. O **orçamento** quantidade é exibida novamente usando o valor definido na segunda janela do navegador e você verá uma mensagem de erro.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Tratamento usando uma propriedade de controle de simultaneidade otimista

Para tratar de simultaneidade otimista para uma entidade que tem uma propriedade de controle, você executará as seguintes tarefas:

- Adicionar procedimentos armazenados para o modelo de dados para gerenciar `OfficeAssignment` entidades. (Propriedades de controle e procedimentos armazenados não precisam ser usados juntos; eles são apenas agrupados juntos aqui para fins ilustrativos.)
- Adicionar métodos CRUD a DAL e BLL para `OfficeAssignment` entidades, incluindo o código para tratar exceções de simultaneidade otimista em DAL.
- Crie uma página da web de atribuições do office.
- Teste a simultaneidade otimista na nova página da web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Adicionar OfficeAssignment procedimentos armazenados para o modelo de dados

Abra o *SchoolModel.edmx* arquivo no designer de modelo, clique na superfície de design e clique em **modelo de atualização de banco de dados**. No **adicionar** guia do **escolher seus objetos de banco de dados** caixa de diálogo caixa, expanda **procedimentos armazenados** e selecione os três `OfficeAssignment` procedimentos armazenados (consulte a a seguir captura de tela) e, em seguida, clique em **concluir**. (Esses procedimentos armazenados já estavam no banco de dados quando você baixou ou criou usando um script.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Clique com botão direito do `OfficeAssignment` entidade e selecione **mapeamento de procedimento armazenado**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Definir o **inserir**, **atualização**, e **excluir** procedimentos armazenados de funções para usar seus correspondente. Para o `OrigTimestamp` parâmetro o `Update` funcionar, defina o **propriedade** para `Timestamp` e selecione o **usar o valor Original** opção.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Quando o Entity Framework chama o `UpdateOfficeAssignment` procedimento armazenado, ele passará o valor original do `Timestamp` coluna o `OrigTimestamp` parâmetro. O procedimento armazenado usa esse parâmetro em seu `Where` cláusula:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

O procedimento armazenado também seleciona o novo valor da `Timestamp` coluna depois da atualização para que o Entity Framework possa manter o `OfficeAssignment` entidade que está na memória em sincronia com a linha correspondente do banco de dados.

(Observe que o procedimento armazenado para excluir uma atribuição de office não tem um `OrigTimestamp` parâmetro. Por isso, o Entity Framework não é possível verificar que uma entidade é alterada antes de excluí-la.)

Salve e feche o modelo de dados.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Adicionar métodos OfficeAssignment a DAL

Abra *ISchoolRepository.cs* e adicione os seguintes métodos CRUD para o `OfficeAssignment` conjunto de entidades:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Adicionar os novos métodos seguintes para *SchoolRepository.cs*. No `UpdateOfficeAssignment` , é chamada de método local `SaveChanges` método em vez de `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

No projeto teste, abra *MockSchoolRepository.cs* e adicione o seguinte `OfficeAssignment` coleta e métodos CRUD a ele. (O repositório de simulação deve implementar a interface do repositório, ou a solução não será compilado.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>A adição de métodos OfficeAssignment BLL

No projeto principal, abra *SchoolBL.cs* e adicione os seguintes métodos CRUD para o `OfficeAssignment` do conjunto de entidades a ele:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Criando uma página da Web OfficeAssignments

Criar uma nova página da web que usa o *Site.Master* página mestra e nomeie-o *OfficeAssignments.aspx*. Adicione a seguinte marcação para o `Content` controle chamado `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Observe que no `DataKeyNames` de atributo, a marcação especifica o `Timestamp` propriedade, bem como a chave de registro (`InstructorID`). Especificar as propriedades no `DataKeyNames` atributo faz com que o controle para salvá-los em estado de controle (que é semelhante ao estado de exibição) para que os valores originais estão disponíveis durante o processamento de postback.

Se você não salvar o `Timestamp` valor, o Entity Framework não teria para o `Where` cláusula do SQL `Update` comando. Consequentemente, nada será encontrado para atualização. Como resultado, o Entity Framework lançaria uma exceção de simultaneidade otimista sempre um `OfficeAssignment` entidade é atualizada.

Abra *OfficeAssignments.aspx.cs* e adicione o seguinte `using` instrução para a camada de acesso a dados:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Adicione o seguinte `Page_Init` método, que habilita a funcionalidade de dados dinâmicos. Também adicione o seguinte manipulador para o `ObjectDataSource` do controle `Updated` eventos para verificar se há erros de simultaneidade:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Teste de simultaneidade otimista na página OfficeAssignments

Execute o *OfficeAssignments.aspx* página.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Clique em **editar** em uma linha e altere o valor de **local** coluna.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Abra uma nova janela do navegador e execute novamente a página (copie a URL da janela do navegador primeiro para a segunda janela do navegador).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Clique em **editar** na mesma linha é editado anteriormente e altere o **local** valor para algo diferente.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Na segunda janela do navegador, clique em **atualização**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Alterne para a primeira janela de navegador e clique em **atualização**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Você verá uma mensagem de erro e o **local** valor foi atualizado para mostrar o valor alterados para a segunda janela do navegador.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Tratamento de simultaneidade com o controle de EntityDataSource

O `EntityDataSource` controle inclui lógica interna que reconhece as configurações de simultaneidade no modelo de dados e identificadores de atualizam e excluir operações adequadamente. No entanto, assim como acontece com todas as exceções, você deve tratar `OptimisticConcurrencyException` exceções por conta própria para fornecer uma mensagem de erro amigável.

Em seguida, você configurará o *Courses.aspx* página (que usa um `EntityDataSource` controle) para permitir que a atualização e operações de exclusão e exibir uma mensagem de erro se ocorrer um conflito de simultaneidade. O `Course` entidade não tem uma simultaneidade de controle de coluna, para que você usará o mesmo método que você fez com o `Department` entidade: rastrear os valores de todas as propriedades não-chave.

Abra o *SchoolModel.edmx* arquivo. Para as propriedades não-chave do `Course` entidade (`Title`, `Credits`, e `DepartmentID`), defina o **modo de simultaneidade** propriedade `Fixed`. Em seguida, salve e feche o modelo de dados.

Abra o *Courses.aspx* página e faça as seguintes alterações:

- No `CoursesEntityDataSource` de controle, adicione `EnableUpdate="true"` e `EnableDelete="true"` atributos. A marca de abertura para o controle agora se parece com o exemplo a seguir:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- No `CoursesGridView` controlar, alterar o `DataKeyNames` valor ao atributo `"CourseID,Title,Credits,DepartmentID"`. Em seguida, adicione um `CommandField` elemento para o `Columns` elemento mostra **editar** e **excluir** botões (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). O `GridView` controle agora se parece com o exemplo a seguir:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Execute a página e crie uma situação de conflito novamente na página de departamentos. Execute a página duas janelas de navegador, clique em **editar** na mesma linha em cada janela e fazer uma alteração diferente em cada uma. Clique em **atualização** na janela e clique **atualização** na outra janela. Quando você clica em **atualização** pela segunda vez, consulte a página de erro que resulta de uma exceção sem tratamento de simultaneidade.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Manipular esse erro de maneira muito semelhante a como você fez isso para o `ObjectDataSource` controle. Abra o *Courses.aspx* página e no `CoursesEntityDataSource` controle, especificam os manipuladores para o `Deleted` e `Updated` eventos. A marca de abertura do controle agora se parece com o exemplo a seguir:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Antes do `CoursesGridView` de controle, adicione o seguinte `ValidationSummary` controle:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

Em *Courses.aspx.cs*, adicione um `using` instrução para o `System.Data` namespace, adicione um método que verifica para exceções de simultaneidade e adicionar manipuladores para o `EntityDataSource` do controle `Updated` e `Deleted`manipuladores. O código será semelhante a seguinte forma:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

A única diferença entre esse código e o que você fez o `ObjectDataSource` controle está que nesse caso a exceção de simultaneidade o `Exception` propriedade do objeto de argumentos de evento em vez de nessa exceção `InnerException` propriedade.

Execute a página e criar um conflito de simultaneidade novamente. Neste momento você vê uma mensagem de erro:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Isso conclui a introdução à manipulação de conflitos de simultaneidade. O seguinte tutorial fornece orientação sobre como melhorar o desempenho em um aplicativo web que usa o Entity Framework.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Próximo](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
