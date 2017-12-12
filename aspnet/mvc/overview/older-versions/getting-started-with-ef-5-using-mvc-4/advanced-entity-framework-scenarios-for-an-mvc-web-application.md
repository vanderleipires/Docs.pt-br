---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Entity Framework cenários avançados de um aplicativo MVC (10 de 10) | Microsoft Docs"
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d58a745896b29317c1d1049e3bf1a5ec2e628820
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Cenários de estrutura avançada de entidade para um aplicativo MVC (10 de 10)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais do início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece aqui.
> 
> > [!NOTE] 
> > 
> > Se você tiver um problema que não é possível resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tentar reproduzir o problema. Geralmente, você pode encontrar a solução para o problema, comparando o seu código para o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


No tutorial anterior, você implementou o repositório e a unidade de trabalho padrões. Este tutorial aborda os seguintes tópicos:

- Executar consultas SQL brutas.
- Executando consultas de acompanhamento não.
- Examinar as consultas enviadas ao banco de dados.
- Trabalhando com classes de proxy.
- Desabilitar a detecção automática de alterações.
- Desabilitar a validação ao salvar alterações.
- [Erros e soluções](#errors)

Para a maioria desses tópicos, você trabalhará com páginas que você já tenha criado. Para usar SQL bruto para fazer atualizações em massa, você criará uma nova página que atualiza o número de créditos de todos os cursos no banco de dados:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Além de usar uma consulta de acompanhamento não que adicionará a nova lógica de validação para a página Editar Departamento:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Desempenho de consultas SQL bruto

API de primeiro código do Entity Framework inclui métodos que permitem que você passar comandos SQL diretamente para o banco de dados. Você tem as seguintes opções:

- Use o `DbSet.SqlQuery` método para consultas que retornam tipos de entidade. Os objetos retornados devem ser do tipo esperado pelo `DbSet` objeto e eles são controladas automaticamente pelo contexto de banco de dados, a menos que você desativar o rastreamento. (Consulte a seção a seguir o `AsNoTracking` método.)
- Use o `Database.SqlQuery` método para consultas que retornam tipos que não são entidades. Os dados retornados não são controlados pelo contexto de banco de dados, mesmo se você usar esse método para recuperar tipos de entidade.
- Use o [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456(v=vs.103).aspx) para comandos sem consulta.

Uma das vantagens de usar o Entity Framework é que ela evita vincular seu código muito semelhante a um método específico de armazenamento de dados. Ele faz isso através da geração de consultas SQL e comandos para você, que também libera você da necessidade de gravá-los. Mas há casos excepcionais, quando você precisa executar consultas específicas de SQL que você criou manualmente, e esses métodos tornam possível para tratar essas exceções.

Como sempre é verdadeiro quando você executa comandos SQL em um aplicativo web, você deve tomar precauções para proteger o site contra ataques de injeção de SQL. Uma maneira de fazer isso é usar consultas parametrizadas para certificar-se de que as cadeias de caracteres enviadas por uma página da web não podem ser interpretadas como comandos SQL. Neste tutorial você usará consultas parametrizadas ao integrar a entrada do usuário em uma consulta.

### <a name="calling-a-query-that-returns-entities"></a>Chamando uma consulta que retorna entidades

Suponha que você deseja o `GenericRepository` classe forneça adicionais de filtragem e classificação flexibilidade sem exigir que você crie uma classe derivada com métodos adicionais. Uma maneira de fazer isso seria adicionar um método que aceita uma consulta SQL. Você pode especificar qualquer tipo de filtragem ou classificação que você deseja no controlador, como um `Where` cláusula que depende de um junções ou uma subconsulta. Nesta seção, você verá como implementar esse método.

Criar o `GetWithRawSql` método adicionando o seguinte código ao *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

Em *CourseController.cs*, chame o novo método do `Details` método, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Neste caso, você poderia ter usado o `GetByID` método, mas você estiver usando o `GetWithRawSql` método para verificar se o `GetWithRawSQL` método funciona.

Execute a página de detalhes para verificar se a instrução select consulta funciona (selecione o **curso** guia e, em seguida, **detalhes** para um curso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chamando uma consulta que retorna a outros tipos de objetos

Anteriormente, você criou uma grade de estatísticas de estudante para a página sobre que mostrou o número de alunos para cada data de registro. O código que faz isso em *HomeController* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Suponha que você deseja gravar o código que recupera dados diretamente no SQL em vez de usar o LINQ. Para fazer o que você precisa executar uma consulta que retorna algo diferente de objetos de entidade, que significa que você precisa usar o `Database.SqlQuery` método.

Em *HomeController*, substitua a instrução LINQ no `About` método com o código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Execute a página sobre. Ele exibe os mesmos dados que antes.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Chamando uma consulta Update

Suponha que os administradores University Contoso desejam ser capaz de executar alterações em massa no banco de dados, como alterar o número de créditos para cada curso. Se a university tiver um grande número de cursos, poderá ser ineficiente para recuperá-las como entidades e alterá-los individualmente. Nesta seção, você implementará uma página da web que permite que o usuário especifique um fator alterar o número de créditos para todos os cursos e fará a alteração executando um SQL `UPDATE` instrução. A página da web será semelhante a ilustração a seguir:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

No tutorial anterior, você usou o repositório genérico para ler e atualizar `Course` entidades a `Course` controlador. Para essa operação de atualização em massa, você precisa criar um novo método de repositório não está no repositório genérico. Para fazer isso, você criará um dedicado `CourseRepository` classe que deriva de `GenericRepository` classe.

No *DAL* pasta, criar *CourseRepository.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

Em *UnitOfWork.cs*, altere o `Course` tipo de repositório de `GenericRepository<Course>` para`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

Em *CourseContoller.cs*, adicione um `UpdateCourseCredits` método:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Esse método será usado para ambos `HttpGet` e `HttpPost`. Quando o `HttpGet` `UpdateCourseCredits` execuções do método, o `multiplier` variável será nula e o modo de exibição exibirá uma caixa de texto e um botão de envio, conforme mostrado na ilustração anterior.

Quando o **atualização** botão é clicado e o `HttpPost` execuções do método, `multiplier` terá o valor digitado na caixa de texto. O código, em seguida, chama o repositório `UpdateCourseCredits` afetados de método, que retorna o número de linhas e esse valor é armazenado no `ViewBag` objeto. Quando o modo de exibição recebe o número de linhas afetadas no `ViewBag` do objeto, ele exibe o número em vez da caixa de texto e envie o botão, conforme mostrado na ilustração a seguir:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Criar uma exibição no *Views\Course* pasta para a página de créditos de curso de atualização:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

Em *Views\Course\UpdateCourseCredits.cshtml*, substitua o código de modelo com o código a seguir:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Execute o `UpdateCourseCredits` método selecionando o **cursos** guia, em seguida, adicionando "/ UpdateCourseCredits" ao final da URL na barra de endereços do navegador (por exemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Insira um número na caixa de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Clique em **Atualizar**. Você ver o número de linhas afetadas:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Clique em **voltar para a lista** para ver a lista de cursos com o número de créditos de revisado.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obter mais informações sobre consultas SQL brutas, consulte [bruto consultas de SQL](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) no blog da equipe do Entity Framework.

## <a name="no-tracking-queries"></a>Consultas de acompanhamento não

Quando um contexto de banco de dados recupera linhas do banco de dados e cria objetos de entidade que representam-los, por padrão ele mantém o controle de se as entidades na memória estão em sincronia com o que está no banco de dados. Os dados na memória atua como um cache e são usados quando você atualizar uma entidade. Esse cache geralmente é desnecessário em um aplicativo web porque as instâncias de contexto são normalmente curta duração (uma nova é criada e descartado para cada solicitação) e o contexto que lê uma entidade normalmente é descartada antes que essa entidade é usada novamente.

Você pode especificar se o contexto controla os objetos de entidade para uma consulta usando o `AsNoTracking` método. Os cenários típicos em que você talvez queira fazer isso incluem o seguinte:

- A consulta recupera um grande volume de dados que podem melhorar o desempenho visivelmente desativar o rastreamento.
- Para anexar uma entidade para atualizá-lo, mas você recuperou anteriormente a mesma entidade para uma finalidade diferente. Porque a entidade já está sendo controlada pelo contexto de banco de dados, não é possível anexar a entidade que você deseja alterar. Uma maneira de evitar esse problema é usar o `AsNoTracking` opção com a consulta anterior.

Nesta seção você implementará a lógica de negócios que ilustra o segundo desses cenários. Especificamente, você vai impor uma regra de negócio que diz que instrutor não pode ser o administrador de mais de um departamento.

Em *DepartmentController.cs*, adicione um novo método que você pode chamar a partir de `Edit` e `Create` métodos para garantir que nenhum dois departamentos tenham a mesma do administrador:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Adicione código de `try` block do `HttpPost` `Edit` método para chamar esse método novo se não houver nenhum erro de validação. O `try` bloco agora é semelhante ao exemplo a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Execute a página Editar departamento e tente alterar o administrador do departamento para o instrutor que já é o administrador de um outro departamento. Você receberá a mensagem de erro esperado:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Agora execute a página Editar departamento novamente e essa alteração de tempo de **orçamento** quantidade. Quando você clica em **salvar**, verá uma página de erro:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

A mensagem de erro de exceção é "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Isso ocorreu devido a seguinte sequência de eventos:

- O `Edit` chamadas de método de `ValidateOneAdministratorAssignmentPerInstructor` método, que recupera todos os departamentos que têm Kim Abercrombie como administrador. Isso faz com que o departamento de inglês a ser lido. Como o que é o departamento que está sendo editado, nenhum erro for relatado. Como resultado da operação de leitura, no entanto, a entidade de departamento em inglês que foi lido do banco de dados está agora sendo controlada pelo contexto de banco de dados.
- O `Edit` método tenta definir a `Modified` sinalizador em inglês entidade department criado pelo associador de modelo MVC, mas que falhará porque o contexto já estiver rastreando uma entidade para o departamento em inglês.

Uma solução para esse problema é para evitar que o contexto de entidades na memória departamento recuperadas pela consulta de validação de controle. Não há nenhuma desvantagem para fazer isso, porque você não atualizar esta entidade ou lê-lo novamente de forma que se beneficiaria dele sejam armazenados em cache na memória.

Em *DepartmentController.cs*, no `ValidateOneAdministratorAssignmentPerInstructor` método, especifique sem rastreamento, conforme mostrado a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Repita a tentativa de editar o **orçamento** quantidade de um departamento. Neste momento, a operação for bem-sucedida, e o site retorna conforme o esperado para a página de índice de departamentos, mostrando o valor de orçamento revisado.

## <a name="examining-queries-sent-to-the-database"></a>Examinando as consultas enviadas ao banco de dados

Às vezes é útil ser capaz de ver as consultas SQL reais que são enviadas para o banco de dados. Para fazer isso, você pode examinar uma variável de consulta no depurador ou chamar a consulta `ToString` método. Para testar isso, você observar uma consulta simples e, em seguida, examinar o que acontece a ela conforme você adiciona opções tal eager carregar, filtragem e classificação.

Em *controladores/CourseController*, substitua o `Index` método com o código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Agora, defina um ponto de interrupção *GenericRepository.cs* no `return query.ToList();` e `return orderBy(query).ToList();` instruções do `Get` método. Execute o projeto no modo de depuração e selecione a página de índice do curso. Quando o código atinge o ponto de interrupção, examine o `query` variável. Você verá a consulta que é enviada para o SQL Server. É um simples `Select` instrução:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Consultas podem ser muito longas para serem exibidos em janelas de depuração no Visual Studio. Para ver a consulta inteira, você pode copiar o valor da variável e cole-o em um editor de texto:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Agora você adicionará uma lista suspensa para a página de índice de curso para que os usuários podem filtrar para um determinado departamento. Você classificará os cursos por título, e você especificará o carregamento rápido para o `Department` propriedade de navegação. Em *CourseController.cs*, substitua o `Index` método com o código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

O método recebe o valor selecionado da lista suspensa no `SelectedDepartment` parâmetro. Se nada estiver selecionado, esse parâmetro será nulo.

Um `SelectList` coleção que contém todos os departamentos é passada para o modo de exibição da lista suspensa. Os parâmetros passados para o `SelectList` construtor especificar o nome do campo de valor, o nome do campo de texto e o item selecionado.

Para o `Get` método o `Course` repositório, o código especifica uma expressão de filtro, uma ordem de classificação e carregamento para rápido o `Department` propriedade de navegação. Retorna a expressão de filtro sempre `true` se nada estiver selecionado na lista suspensa (ou seja, `SelectedDepartment` é nulo).

Em *Views\Course\Index.cshtml*, imediatamente antes de abertura `table` marca, adicione o seguinte código para criar a lista suspensa e um botão de envio:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Com pontos de interrupção ainda no `GenericRepository` classe, execute a página de índice do curso. Prossiga com os primeiro duas vezes em que o código atinge um ponto de interrupção, para que a página é exibida no navegador. Selecione um departamento na lista suspensa e clique em **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Neste momento o primeiro ponto de interrupção será usado para a consulta de departamentos da lista suspensa. Ignorar que e exibir o `query` variável na próxima vez que o código atinge o ponto de interrupção para ver o que o `Course` agora a aparência de consulta. Você verá algo semelhante ao seguinte:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Você pode ver que a consulta é agora um `JOIN` consulta carrega `Department` dados junto com o `Course` dados e que ele inclui um `WHERE` cláusula.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Trabalhando com Classes de Proxy

Quando o Entity Framework cria instâncias de entidade (por exemplo, quando você executa uma consulta), ele cria geralmente-los como instâncias de um tipo derivado gerado dinamicamente que atua como um proxy para a entidade. Este proxy substitui algumas propriedades virtuais da entidade para inserir ganchos para executar ações automaticamente quando a propriedade for acessada. Por exemplo, esse mecanismo é usado para dar suporte a carregamento lento de relações.

Na maioria das vezes você não precisa estar ciente deste uso de proxies, mas há exceções:

- Em alguns cenários, convém impedir que o Entity Framework criar instâncias de proxy. Por exemplo, serializar instâncias do proxy não pode ser mais eficiente do que serializar instâncias do proxy.
- Quando você instancia uma classe de entidade usando o `new` operador, você não obtém uma instância do proxy. Isso significa que você não obtiver funcionalidade, como controle de alterações automático e carregamento lento. Isso normalmente é okey; geralmente não é necessário carregar lento, porque você está criando uma nova entidade que não está no banco de dados, e você geralmente não é necessário se você estiver marcando explicitamente a entidade que o controle de alterações `Added`. No entanto, se você precisa de carregamento lento e controle de alterações, você pode criar novas instâncias de entidade com proxies usando o `Create` método o `DbSet` classe.
- Deseja obter um tipo de entidade real de um tipo de proxy. Você pode usar o `GetObjectType` método o `ObjectContext` classe ao obter o tipo de entidade real de uma instância do tipo de proxy.

Para obter mais informações, consulte [trabalhando com Proxies](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) no blog da equipe do Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Desabilitar a detecção automática de alterações

O Entity Framework determina como uma entidade foi alterado (e, portanto, as atualizações que precisam ser enviados para o banco de dados), comparando os valores atuais de uma entidade com os valores originais. Os valores originais são armazenados quando a entidade foi consultada ou anexada. Alguns dos métodos que causam a detecção de alterações automáticas são os seguintes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se você estiver rastreando um grande número de entidades e chamar um desses métodos muitas vezes em um loop, você pode obter melhorias significativas de desempenho desativar temporariamente detecção de alterações automáticas usando o [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) propriedade. Para obter mais informações, consulte [automaticamente detectar alterações](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Desabilitar a validação ao salvar alterações

Quando você chama o `SaveChanges` método, por padrão o Entity Framework valida os dados em todas as propriedades de todas as entidades alteradas antes de atualizar o banco de dados. Se você atualizou um grande número de entidades e você já tiver validado os dados, esse trabalho é desnecessário, você pode tornar o processo de salvar as alterações terão menos tempo desativando temporariamente a validação. Você pode fazer essa usando o [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) propriedade. Para obter mais informações, consulte [validação](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Resumo

Isso conclui esta série de tutoriais sobre como usar o Entity Framework em um aplicativo ASP.NET MVC. Links para outros recursos do Entity Framework podem ser encontradas no [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obter mais informações sobre como implantar o aplicativo web depois que você construiu, consulte [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/en-us/library/bb386521.aspx) na biblioteca MSDN.

Para obter informações sobre outros tópicos relacionados ao MVC, como autenticação e autorização, consulte o [MVC recomendado recursos](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Confirmações

- Tom Dykstra gravou a versão original deste tutorial e é escritor técnico de programação no Microsoft Web Platform e ferramentas de equipe de conteúdo.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) conjunto criado neste tutorial e fez a maioria do trabalho atualizá-lo para o EF 5 e MVC 4. Rick é escritor técnico de programação para a Microsoft se concentra no Azure e MVC.
- [Rowan Miller](http://www.romiller.com) e outros membros da equipe do Entity Framework assistido com revisões de código e ajudaram a depurar vários problemas com migrações que ocorreu enquanto estamos foram atualizando o tutorial para EF 5.

## <a name="vb"></a>VB

Quando o tutorial foi produzido, fornecemos versões c# e VB do projeto concluído de download. Com esta atualização, estamos oferecendo um projeto de baixável c# para cada capítulo tornar mais fácil começar em qualquer lugar na série, mas devido a limitações de tempo e outras prioridades que não foi feito que para VB. Se você compila um projeto VB usando esses tutoriais e para compartilhá-la com outras pessoas, informe-nos.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Erros e soluções alternativas

### <a name="cannot-createshadow-copy"></a>Não é possível criar/shadow copy

Mensagem de erro:

*Não é possível criar/shadow copy 'DotNetOpenAuth.OpenId' quando ele já existe.*

Solução:

Aguarde alguns segundos e atualize a página.

### <a name="update-database-not-recognized"></a>Update-Database não reconhecido

Mensagem de erro:

*O termo 'Update-Database' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome, ou se um caminho tiver sido incluído, verifique se o caminho está correto e tente novamente.* (Da  *`Update-Database`*  do PMC.)

Solução:

Sair do Visual Studio. Reabra o projeto e tente novamente.

### <a name="validation-failed"></a>Falha na validação

Mensagem de erro:

*Falha na validação de uma ou mais entidades. Consulte a propriedade 'EntityValidationErrors' para obter mais detalhes.* (Da  *`Update-Database`*  do PMC.)

Solução:

Uma causa desse problema é erros de validação quando o `Seed` execuções do método. Consulte [Seeding e bancos de dados de depuração Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obter dicas sobre depuração de `Seed` método.

### <a name="http-50019-error"></a>HTTP 500.19 erro

Mensagem de erro:

*Erro HTTP 500.19 - erro interno do servidor a página solicitada não pode ser acessado porque os dados de configuração relacionados para a página são inválidos.*

Solução:

Uma forma que você pode obter esse erro é de várias cópias da solução, cada um deles usando o mesmo número de porta. Normalmente, você pode resolver esse problema saindo todas as instâncias do Visual Studio, em seguida, reiniciar o projeto de seu trabalho no. Se isso não funcionar, tente alterar o número da porta. Clique com o botão direito no arquivo de projeto e, em seguida, clique em Propriedades. Selecione o **Web** guia e, em seguida, alterar o número da porta de **Url do projeto** caixa de texto.

### <a name="error-locating-sql-server-instance"></a>Instância do SQL Server localização de erro

Mensagem de erro:

*Ocorreu um erro relacionado à rede ou específico da instância ao estabelecer uma conexão com o SQL Server. O servidor não foi encontrado ou não estava acessível. Verifique se o nome da instância está correto e que o SQL Server está configurado para permitir conexões remotas. (provedor: Interfaces de rede do SQL, erro: 26 - erro ao localizar servidor/instância especificada)*

Solução:

Verifique a cadeia de caracteres de conexão. Se você excluiu manualmente o banco de dados, altere o nome do banco de dados na cadeia de caracteres de construção.

>[!div class="step-by-step"]
[Anterior](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[Próximo](building-the-ef5-mvc4-chapter-downloads.md)
