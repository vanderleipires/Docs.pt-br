---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Avançada de cenários de Entity Framework para um aplicativo Web do MVC (10 de 10) | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5fa6ff439790f70cd60d0985a791df26d69c7107
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825428"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Cenários de estrutura avançada de entidade para um aplicativo Web do MVC (10 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais de início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece por aqui.
> 
> > [!NOTE] 
> > 
> > Se você enfrentar um problema que você não conseguir resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução ao problema comparando seu código com o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


No tutorial anterior, você implementou o repositório e unidade de padrões de trabalho. Este tutorial aborda os tópicos a seguir:

- Executando consultas SQL brutas.
- Executando consultas sem controle.
- Examinar as consultas enviadas ao banco de dados.
- Trabalhando com classes de proxy.
- Desabilitando a detecção automática de alterações.
- Desabilitar a validação ao salvar alterações.
- [Erros e soluções](#errors)

Para a maioria desses tópicos, você trabalhará com as páginas que você já criou. Para usar o SQL bruto para fazer atualizações em massa, você criará uma nova página que atualiza o número de créditos de todos os cursos no banco de dados:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

E, para usar uma consulta sem controle, você adicionará o nova lógica de validação para a página Editar Departamento:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Desempenho de consultas SQL brutas

A API do Entity Framework Code First inclui métodos que permitem passar comandos SQL diretamente para o banco de dados. Você tem as seguintes opções:

- Use o método `DbSet.SqlQuery` para consultas que retornam tipos de entidade. Os objetos retornados devem ser do tipo esperado pelo `DbSet` objeto e elas são rastreadas automaticamente pelo contexto de banco de dados, a menos que você desative o controle. (Consulte a seção a seguir o `AsNoTracking` método.)
- Use o `Database.SqlQuery` método para consultas que retornam tipos que não são entidades. Os dados retornados não são controlados pelo contexto de banco de dados, mesmo se esse método é usado para recuperar tipos de entidade.
- Use o [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) para comandos sem consulta.

Uma das vantagens de usar o Entity Framework é que ele evita vincular o código de forma muito próxima a um método específico de armazenamento de dados. Ele faz isso pela geração de consultas SQL e comandos para você, que também libera você da necessidade de escrevê-los. Mas há casos excepcionais, quando você precisa executar consultas SQL específicas que você criou manualmente, e esses métodos tornam possível para que você possa lidar com essas exceções.

Como é sempre verdadeiro quando você executa comandos SQL em um aplicativo Web, é necessário tomar precauções para proteger o site contra ataques de injeção de SQL. Uma maneira de fazer isso é usar consultas parametrizadas para garantir que as cadeias de caracteres enviadas por uma página da Web não possam ser interpretadas como comandos SQL. Neste tutorial, você usará consultas parametrizadas ao integrar a entrada do usuário a uma consulta.

### <a name="calling-a-query-that-returns-entities"></a>Chamar uma consulta que retorna entidades

Suponha que você deseja o `GenericRepository` classe para fornecer filtragem adicional e uma classificação flexibilidade sem exigir que você crie uma classe derivada com métodos adicionais. Uma maneira de fazer isso seria adicionar um método que aceita uma consulta SQL. Você pode especificar qualquer tipo de filtragem ou classificação que você deseja no controlador, como um `Where` cláusula que depende de um junções ou uma subconsulta. Nesta seção, você verá como implementar um método desse tipo.

Criar o `GetWithRawSql` método adicionando o seguinte código ao *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

Na *CourseController.cs*, chame o novo método da `Details` método, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Nesse caso, você poderia ter usado o `GetByID` método, mas você estiver usando o `GetWithRawSql` método para verificar se o `GetWithRawSQL` método funciona.

Execute a página de detalhes para verificar que a consulta select funciona (selecione o **curso** guia e, em seguida **detalhes** para um curso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chamar uma consulta que retorna outros tipos de objetos

Anteriormente, você criou uma grade de estatísticas de alunos para a página Sobre que mostrava o número de alunos para cada data de registro. O código que faz isso no *HomeController.cs* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Suponha que você deseja escrever o código que recupera esses dados diretamente no SQL em vez de usar o LINQ. Para fazer isso, você precisa executar uma consulta que retorna algo diferente de objetos de entidade, que significa que você precisa usar o `Database.SqlQuery` método.

Na *HomeController.cs*, substitua a instrução LINQ no `About` método com o código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Execute a página sobre. Ela exibe os mesmos dados que antes.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Chamar uma consulta Update

Suponha que os administradores da Contoso University desejam ser capaz de realizar alterações em massa no banco de dados, como alterar o número de créditos para cada curso. Se a universidade tiver uma grande quantidade de cursos, poderá ser ineficiente recuperá-los como entidades e alterá-los individualmente. Nesta seção, você implementará uma página da web que permite ao usuário especificar um fator pelo qual alterar o número de créditos para todos os cursos e fará a alteração executando um SQL `UPDATE` instrução. A página da Web será semelhante à seguinte ilustração:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

No tutorial anterior, você usou o repositório genérico para ler e atualizar `Course` entidades no `Course` controlador. Para essa operação de atualização em massa, você precisa criar um novo método de repositório não está no repositório genérico. Para fazer isso, você criará um dedicado `CourseRepository` classe que deriva de `GenericRepository` classe.

No *DAL* pasta, crie *CourseRepository.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

Na *UnitOfWork.cs*, altere o `Course` tipo de repositório de `GenericRepository<Course>` para `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

Na *CourseContoller.cs*, adicione um `UpdateCourseCredits` método:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Esse método será usado para ambos `HttpGet` e `HttpPost`. Quando o `HttpGet` `UpdateCourseCredits` execuções de método, o `multiplier` variável será nula e o modo de exibição exibirá uma caixa de texto vazia e um botão de envio, conforme mostrado na ilustração anterior.

Quando o **atualização** botão é clicado e o `HttpPost` execuções de método, `multiplier` terá o valor inserido na caixa de texto. O código, em seguida, chama o repositório `UpdateCourseCredits` afetados de método, que retorna o número de linhas e esse valor é armazenado no `ViewBag` objeto. Quando o modo de exibição recebe o número de linhas afetadas no `ViewBag` do objeto, ele exibirá esse número em vez da caixa de texto e enviar o botão, conforme mostrado na ilustração a seguir:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Criar uma exibição na *Views\Course* pasta para a página Atualizar créditos de curso:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

Na *Views\Course\UpdateCourseCredits.cshtml*, substitua o código de modelo pelo código a seguir:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Execute o método `UpdateCourseCredits` selecionando a guia **Cursos**, adicionando, em seguida, "/UpdateCourseCredits" ao final da URL na barra de endereços do navegador (por exemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Insira um número na caixa de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Clique em **Atualizar**. O número de linhas afetadas é exibido:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Clique em **Voltar para a Lista** para ver a lista de cursos com o número revisado de créditos.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obter mais informações sobre consultas SQL brutas, consulte [consultas SQL brutas](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) no blog da equipe do Entity Framework.

## <a name="no-tracking-queries"></a>Consultas sem acompanhamento

Quando um contexto de banco de dados recupera linhas do banco de dados e cria objetos de entidade que representá-los, por padrão ele mantém o controle de se as entidades em memória estão em sincronia com o que está no banco de dados. Os dados em memória atuam como um cache e são usados quando uma entidade é atualizada. Esse cache costuma ser desnecessário em um aplicativo Web porque as instâncias de contexto são normalmente de curta duração (uma nova é criada e descartada para cada solicitação) e o contexto que lê uma entidade normalmente é descartado antes que essa entidade seja usada novamente.

Você pode especificar se o contexto controla os objetos de entidade para uma consulta usando o `AsNoTracking` método. Os cenários típicos em que talvez você deseje fazer isso incluem os seguintes:

- A consulta recupera um grande volume de dados que podem aprimorar o desempenho visivelmente desativar o rastreamento.
- Você deseja anexar uma entidade para atualizá-lo, mas você recuperou anteriormente a mesma entidade para uma finalidade diferente. Como a entidade já está sendo controlada pelo contexto de banco de dados, não é possível anexar a entidade que você deseja alterar. Uma maneira de evitar esse problema é usar o `AsNoTracking` opção com a consulta anterior.

Nesta seção você implementará a lógica de negócios que ilustra o segundo desses cenários. Especificamente, você vai impor uma regra de negócio que diz que um instrutor não pode ser o administrador de mais de um departamento.

Na *DepartmentController.cs*, adicione um novo método que você pode chamar a partir de `Edit` e `Create` métodos para certificar-se de que nenhum dois departamentos tenham o mesmo administrador:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Adicione código a `try` block do `HttpPost` `Edit` método para chamar esse novo método, se não houver nenhum erro de validação. O `try` bloco agora se parece com o exemplo a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Execute a página Editar departamento e tente alterar o administrador de um departamento para um instrutor que já é o administrador de um outro departamento. Você receber a mensagem de erro esperado:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Agora execute a página Editar departamento novamente e essa alteração de hora a **orçamento** quantidade. Quando você clica em **salvar**, você verá uma página de erro:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

A mensagem de erro de exceção é "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Isso ocorreu devido a sequência de eventos a seguir:

- O `Edit` chamadas de método a `ValidateOneAdministratorAssignmentPerInstructor` método, que recupera todos os departamentos que têm Kim Abercrombie como administrador. Que faz com que o departamento de inglês a serem lidos. Porque esse é o departamento que está sendo editado, nenhum erro será relatado. Como resultado da operação de leitura, no entanto, a entidade de departamento de inglês que foi lido do banco de dados é agora está sendo controlada pelo contexto de banco de dados.
- O `Edit` método tenta definir o `Modified` sinalizador em inglês entidade departamento criada pelo associador de modelo MVC, mas que falhará porque o contexto já estiver rastreando uma entidade para o departamento de inglês.

Uma solução para esse problema é para impedir que o contexto de acompanhamento recuperadas pela consulta de validação de entidades de departamento na memória. Não há nenhuma desvantagem para fazer isso, porque você não atualizar esta entidade ou lê-lo novamente de forma que se beneficiaria dele sejam armazenados em cache na memória.

Na *DepartmentController.cs*, no `ValidateOneAdministratorAssignmentPerInstructor` método, não especificar nenhum acompanhamento, conforme mostrado a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Repita a tentativa de editar o **orçamento** quantidade de um departamento. Neste momento, a operação for bem-sucedida, e o site retorna conforme o esperado para a página de índice de departamentos, mostrando o valor do orçamento revisado.

## <a name="examining-queries-sent-to-the-database"></a>Examinando as consultas enviadas ao banco de dados

Às vezes, é útil poder ver as consultas SQL reais que são enviadas ao banco de dados. Para fazer isso, você pode examinar uma variável de consulta no depurador ou chamar a consulta `ToString` método. Para testá-la, você veja uma consulta simple e, em seguida, examinar o que acontece a ela conforme você adiciona opções tal eager carregando, filtragem e classificação.

Na *controladores/CourseController*, substitua o `Index` método com o código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Agora definir um ponto de interrupção no *GenericRepository.cs* sobre o `return query.ToList();` e o `return orderBy(query).ToList();` instruções da `Get` método. Execute o projeto no modo de depuração e selecione a página de índice do curso. Quando o código atinge o ponto de interrupção, examinar o `query` variável. Você vê a consulta que é enviada para o SQL Server. É um simples `Select` instrução:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Consultas podem ser muito longas para ser exibido nas janelas de depuração do Visual Studio. Para ver a consulta inteira, você pode copiar o valor da variável e cole-o em um editor de texto:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Agora você adicionará uma lista suspensa para a página de índice do curso para que os usuários podem filtrar para um determinado departamento. Você classificará os cursos por título, e você especificará o carregamento adiantado para a `Department` propriedade de navegação. Na *CourseController.cs*, substitua o `Index` método com o código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

O método recebe o valor selecionado da lista suspensa no `SelectedDepartment` parâmetro. Se nada estiver selecionado, esse parâmetro será nulo.

Um `SelectList` coleção que contém todos os departamentos é passada para o modo de exibição para a lista suspensa. Os parâmetros passados para o `SelectList` construtor especifique o nome do campo de valor, o nome do campo de texto e o item selecionado.

Para o `Get` método da `Course` repositório, o código especifica uma expressão de filtro, uma ordem de classificação e o carregamento adiantado para a `Department` propriedade de navegação. A expressão de filtro sempre retorna `true` se nenhuma opção estiver selecionada na lista suspensa (ou seja, `SelectedDepartment` é nulo).

Na *Views\Course\Index.cshtml*, imediatamente antes da abertura `table` marca, adicione o seguinte código para criar a lista suspensa e um botão de envio:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Com pontos de interrupção ainda no `GenericRepository` classe, execute a página de índice do curso. Prossiga com as duas primeiras horas que o código atinge um ponto de interrupção, para que a página é exibida no navegador. Selecione um departamento na lista suspensa e clique em **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Desta vez o primeiro ponto de interrupção será para a consulta de departamentos para obter a lista suspensa. Pular essa etapa e exibir o `query` variável na próxima vez que o código atinge o ponto de interrupção para ver o que o `Course` consulta agora se parece com. Você verá algo semelhante ao seguinte:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Você pode ver que a consulta agora é um `JOIN` consulta que carrega `Department` dados junto com o `Course` dados e que ele inclui um `WHERE` cláusula.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Trabalhando com Classes de Proxy

Quando o Entity Framework cria instâncias de entidade (por exemplo, quando você executar uma consulta), ele cria geralmente-los como instâncias de um tipo derivado gerado dinamicamente que atua como um proxy para a entidade. Esse proxy substitui alguns virtuais propriedades da entidade para inserir os ganchos para executar as ações automaticamente quando a propriedade é acessada. Por exemplo, esse mecanismo é usado para dar suporte a carregamento lento de relações.

Na maioria das vezes você não precisa estar atento esse uso de proxies, mas há exceções:

- Em alguns cenários você talvez queira impedir que o Entity Framework desde a criação de instâncias de proxy. Por exemplo, serializar instâncias de proxy não pode ser mais eficiente do que a serialização de instâncias de proxy.
- Quando você instancia uma classe de entidade usando o `new` operador, você não obtém uma instância do proxy. Isso significa que você não obtém a funcionalidade, como o controle de alterações automático e o carregamento lento. Isso é normalmente okey; Você geralmente não precisa o carregamento lento, pois você está criando uma nova entidade que não está no banco de dados e geralmente não precisa se você estiver marcando explicitamente a entidade como o controle de alterações `Added`. No entanto, se você precisar carregamento lento e você precisa de controle de alterações, você pode criar novas instâncias de entidade com proxies usando o `Create` método da `DbSet` classe.
- Talvez você queira obter um tipo de entidade real de um tipo de proxy. Você pode usar o `GetObjectType` método da `ObjectContext` classe para obter o tipo de entidade real de uma instância do tipo de proxy.

Para obter mais informações, consulte [trabalhar com Proxies](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) no blog da equipe do Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Desabilitar a detecção automática de alterações

O Entity Framework determina como uma entidade foi alterada (e, portanto, quais atualizações precisam ser enviadas ao banco de dados), comparando os valores atuais de uma entidade com os valores originais. Os valores originais são armazenados quando a entidade foi consultada ou anexada. Alguns dos métodos que causam a detecção automática de alterações são os seguintes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se você estiver controlando um grande número de entidades e chamar um desses métodos muitas vezes em um loop, você pode obter melhorias significativas de desempenho desativando temporariamente a detecção de alterações automático usando o [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) propriedade. Para obter mais informações, consulte [detectando alterações automaticamente](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Desabilitar a validação ao salvar alterações

Quando você chama o `SaveChanges` método, por padrão, o Entity Framework valida os dados em todas as propriedades de todas as entidades alteradas antes de atualizar o banco de dados. Se você tiver atualizado um grande número de entidades e você já tiver validado os dados, esse trabalho é desnecessário e você pode tornar o processo de salvar as alterações levam menos tempo, desativando temporariamente a validação. Você pode fazer isso usando o [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) propriedade. Para obter mais informações, consulte [validação](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Resumo

Isso conclui esta série de tutoriais sobre como usar o Entity Framework em um aplicativo ASP.NET MVC. Links para outros recursos do Entity Framework podem ser encontradas na [mapa de conteúdo de acesso do ASP.NET dados](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obter mais informações sobre como implantar seu aplicativo web depois que ela foi criada, consulte [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx) na biblioteca MSDN.

Para obter informações sobre outros tópicos relacionados ao MVC, como autenticação e autorização, consulte a [recursos do MVC recomendados](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Agradecimentos

- Tom Dykstra escreveu a versão original deste tutorial e é um escritor de programação sênior na Microsoft Web Platform e ferramentas de equipe de conteúdo.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) foi co-autor deste tutorial e realizou a maior parte do trabalho de atualizá-lo para o EF 5 e MVC 4. Rick é um escritor de programação sênior da Microsoft, concentrando-se no Azure e no MVC.
- [Rowan Miller](http://www.romiller.com) e outros membros da equipe do Entity Framework assistido com revisões de código e ajudaram a depurar vários problemas com as migrações que surgiram durante estamos foram atualizando o tutorial do EF 5.

## <a name="vb"></a>VB

Quando o tutorial foi originalmente produzido, fornecemos as versões c# e VB do projeto download concluído. Com essa atualização, estamos oferecendo um projeto de que pode ser baixado em C# para cada capítulo tornar mais fácil começar a usar em qualquer lugar na série, mas devido a limitações de tempo e outras prioridades que não foi feito que para VB. Se você compilar um projeto do VB usando esses tutoriais e seria dispostos a compartilhá-la com outras pessoas, informe-nos.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Erros e soluções alternativas

### <a name="cannot-createshadow-copy"></a>Não é possível criar/sombra cópia

Mensagem de erro:

*Não é possível criar/sombra cópia 'DotNetOpenAuth.OpenId' quando ele já existe.*

Solução:

Aguarde alguns segundos e atualize a página.

### <a name="update-database-not-recognized"></a>Update-Database não reconhecido

Mensagem de erro:

*O termo 'Update-Database' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome, ou se um caminho foi incluído, verifique se o caminho está correto e tente novamente.* (Da *`Update-Database`* comando no PMC.)

Solução:

Saia do Visual Studio. Reabra o projeto e tente novamente.

### <a name="validation-failed"></a>Falha na validação

Mensagem de erro:

*Falha na validação de uma ou mais entidades. Consulte a propriedade 'EntityValidationErrors' para obter mais detalhes.* (Da *`Update-Database`* comando no PMC.)

Solução:

Uma causa desse problema é erros de validação quando o `Seed` execuções de método. Ver [propagando e depurando Entity Framework (EF) BDs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obter dicas sobre como depurar o `Seed` método.

### <a name="http-50019-error"></a>HTTP 500.19 erro

Mensagem de erro:

*Erro HTTP 500.19 - erro interno do servidor a página solicitada não pode ser acessado porque os dados de configuração relacionados para a página são inválidos.*

Solução:

Uma maneira que você pode obter esse erro é de várias cópias da solução, cada um deles usando o mesmo número de porta. Normalmente, você pode resolver esse problema saindo todas as instâncias do Visual Studio, em seguida, reiniciar o projeto está trabalhando no. Se isso não funcionar, tente alterar o número da porta. Clique com botão direito no arquivo de projeto e, em seguida, clique em Propriedades. Selecione o **Web** guia e, em seguida, alterar o número da porta a **Url do projeto** caixa de texto.

### <a name="error-locating-sql-server-instance"></a>Erro ao localizar a instância do SQL Server

Mensagem de erro:

*Ocorreu um erro relacionado à rede ou específico da instância ao estabelecer uma conexão com o SQL Server. O servidor não foi encontrado ou não estava acessível. Verifique se o nome da instância está correto e que o SQL Server está configurado para permitir conexões remotas. (provedor: Interfaces de rede do SQL, erro: 26 - erro ao localizar servidor/instância especificada)*

Solução:

Verifique a cadeia de caracteres de conexão. Se você excluiu manualmente o banco de dados, altere o nome do banco de dados na cadeia de caracteres de construção.

> [!div class="step-by-step"]
> [Anterior](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [Próximo](building-the-ef5-mvc4-chapter-downloads.md)
