---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Entity Framework 6 cenários avançados de para um aplicativo Web 5 do MVC (12 de 12) | Microsoft Docs"
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 3d6cc52f7fa3089f30f1a6bbd76593f1eca95009
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Avançadas Entity Framework 6 cenários para um aplicativo Web 5 do MVC (12 de 12)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


No tutorial anterior é implementado de herança de tabela por hierarquia. Este tutorial inclui apresenta vários tópicos que são úteis a serem consideradas quando você vá além do básico do desenvolvimento de aplicativos web ASP.NET que usam o Entity Framework Code First. Instruções passo a passo percorrer o código e usando o Visual Studio para os seguintes tópicos:

- [Executar consultas SQL brutas](#rawsql)
- [Executar consultas de controle não](#notracking)
- [Examinar SQL enviadas ao banco de dados](#sql)

O tutorial apresenta vários tópicos com breves apresentações seguidas de links para recursos para obter mais informações:

- [Repositório e unidade de padrões de trabalho](#repo)
- [Classes de proxy](#proxies)
- [Detecção de alterações automáticas](#changedetection)
- [Validação automática](#validation)
- [Ferramentas EF para Visual Studio](#tools)
- [Código do Entity Framework](#source)

O tutorial também inclui as seções a seguir:

- [Resumo](#summary)
- [Confirmações](#acknowledgments)
- [Uma observação sobre VB](#vb)
- [Os erros comuns e soluções ou soluções alternativas para eles](#errors)

Para a maioria desses tópicos, você trabalhará com páginas que você já tenha criado. Para usar SQL bruto para fazer atualizações em massa, você criará uma nova página que atualiza o número de créditos de todos os cursos no banco de dados:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Desempenho de consultas SQL bruto

API de primeiro código do Entity Framework inclui métodos que permitem que você passar comandos SQL diretamente para o banco de dados. Você tem as seguintes opções:

- Use o [DbSet.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.sqlquery.aspx) método para consultas que retornam tipos de entidade. Os objetos retornados devem ser do tipo esperado pelo `DbSet` objeto e eles são controladas automaticamente pelo contexto de banco de dados, a menos que você desativar o rastreamento. (Consulte a seção a seguir o [AsNoTracking](https://msdn.microsoft.com/en-us/library/system.data.entity.dbextensions.asnotracking.aspx) método.)
- Use o [Database.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.database.sqlquery.aspx) método para consultas que retornam tipos que não são entidades. Os dados retornados não são controlados pelo contexto de banco de dados, mesmo se você usar esse método para recuperar tipos de entidade.
- Use o [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456.aspx) para comandos sem consulta.

Uma das vantagens de usar o Entity Framework é que ela evita vincular seu código muito semelhante a um método específico de armazenamento de dados. Ele faz isso através da geração de consultas SQL e comandos para você, que também libera você da necessidade de gravá-los. Mas há casos excepcionais, quando você precisa executar consultas específicas de SQL que você criou manualmente, e esses métodos tornam possível para tratar essas exceções.

Como sempre é verdadeiro quando você executa comandos SQL em um aplicativo web, você deve tomar precauções para proteger o site contra ataques de injeção de SQL. Uma maneira de fazer isso é usar consultas parametrizadas para certificar-se de que as cadeias de caracteres enviadas por uma página da web não podem ser interpretadas como comandos SQL. Neste tutorial você usará consultas parametrizadas ao integrar a entrada do usuário em uma consulta.

### <a name="calling-a-query-that-returns-entities"></a>Chamando uma consulta que retorna entidades

O [DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/en-us/library/gg696460.aspx) classe fornece um método que você pode usar para executar uma consulta que retorna uma entidade do tipo `TEntity`. Para ver como isso funciona, alterará o código a `Details` método o `Department` controlador.

Em *DepartmentController.cs*, no `Details` método, substitua o `db.Departments.FindAsync` chamada de método com um `db.Departments.SqlQuery` chamada de método, como mostra o seguinte código:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Para verificar se o novo código funciona corretamente, selecione o **departamentos** guia e, em seguida, **detalhes** para um dos departamentos.

![Detalhes do departamento](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chamando uma consulta que retorna a outros tipos de objetos

Anteriormente, você criou uma grade de estatísticas de estudante para a página sobre que mostrou o número de alunos para cada data de registro. O código que faz isso em *HomeController* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Suponha que você deseja gravar o código que recupera dados diretamente no SQL em vez de usar o LINQ. Para fazer o que você precisa executar uma consulta que retorna algo diferente de objetos de entidade, que significa que você precisa usar o [Database.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.database.sqlquery(v=VS.103).aspx) método.

Em *HomeController*, substitua a instrução LINQ no `About` método com uma instrução SQL, como mostra o seguinte código:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Execute a página sobre. Ele exibe os mesmos dados que antes.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Chamando uma consulta Update

Suponha que os administradores University Contoso desejam ser capaz de executar alterações em massa no banco de dados, como alterar o número de créditos para cada curso. Se a university tiver um grande número de cursos, poderá ser ineficiente para recuperá-las como entidades e alterá-los individualmente. Nesta seção, você implementará uma página da web que permite que o usuário especifique um fator alterar o número de créditos para todos os cursos e fará a alteração executando um SQL `UPDATE` instrução. A página da web será semelhante a ilustração a seguir:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

Em *CourseContoller.cs*, adicionar `UpdateCourseCredits` métodos para `HttpGet` e `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Quando o controlador processa um `HttpGet` solicitação, nada será retornado no `ViewBag.RowsAffected` variável e o modo de exibição exibe uma caixa de texto e um botão de envio, conforme mostrado na ilustração anterior.

Quando o **atualização** botão é clicado, o `HttpPost` método é chamado, e `multiplier` tem o valor inserido na caixa de texto. O código, em seguida, executa o SQL que atualiza os cursos e retorna o número de linhas afetadas para o modo de exibição de `ViewBag.RowsAffected` variável. Quando o modo de exibição obtém um valor nessa variável, ele exibe o número de linhas atualizadas em vez da caixa de texto e enviar botão, conforme mostrado na ilustração a seguir:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Em *CourseController.cs*, clique em um do `UpdateCourseCredits` métodos e depois clique em **adicionar exibição**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Em *Views\Course\UpdateCourseCredits.cshtml*, substitua o código de modelo com o código a seguir:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Execute o `UpdateCourseCredits` método selecionando o **cursos** guia, em seguida, adicionando "/ UpdateCourseCredits" ao final da URL na barra de endereços do navegador (por exemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Insira um número na caixa de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Clique em **Atualizar**. Você ver o número de linhas afetadas:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Clique em **voltar para a lista** para ver a lista de cursos com o número de créditos de revisado.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obter mais informações sobre consultas SQL brutas, consulte [bruto consultas de SQL](https://msdn.microsoft.com/en-us/data/jj592907) no MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Consultas de acompanhamento não

Quando um contexto de banco de dados recupera linhas da tabela e cria objetos de entidade que representam-los, por padrão ele mantém o controle de se as entidades na memória estão em sincronia com o que está no banco de dados. Os dados na memória atua como um cache e são usados quando você atualizar uma entidade. Esse cache geralmente é desnecessário em um aplicativo web porque as instâncias de contexto são normalmente curta duração (uma nova é criada e descartado para cada solicitação) e o contexto que lê uma entidade normalmente é descartada antes que essa entidade é usada novamente.

Você pode desabilitar o controle de objetos de entidade na memória usando o [AsNoTracking](https://msdn.microsoft.com/en-us/library/gg679352(v=vs.103).aspx) método. Os cenários típicos em que você talvez queira fazer isso incluem o seguinte:

- Uma consulta recupera um grande volume de dados que podem melhorar o desempenho visivelmente desativar o rastreamento.
- Para anexar uma entidade para atualizá-lo, mas você recuperou anteriormente a mesma entidade para uma finalidade diferente. Porque a entidade já está sendo controlada pelo contexto de banco de dados, não é possível anexar a entidade que você deseja alterar. Uma maneira de lidar com essa situação é usar o `AsNoTracking` opção com a consulta anterior.

Para obter um exemplo que demonstra como usar o [AsNoTracking](https://msdn.microsoft.com/en-us/library/gg679352(v=vs.103).aspx) método, consulte [a versão anterior deste tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Esta versão do tutorial não defina o sinalizador de modificados em uma entidade de associador de modelo criado no método de edição, portanto ele não precisa `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Examinar SQL enviadas ao banco de dados

Às vezes é útil ser capaz de ver as consultas SQL reais que são enviadas para o banco de dados. Um tutorial anterior, você viu como fazer isso no código interceptador; Agora, você verá algumas maneiras de fazer isso sem escrever código interceptador. Para testar isso, você observar uma consulta simples e, em seguida, examinar o que acontece a ela conforme você adiciona opções tal eager carregar, filtragem e classificação.

Em *controladores/CourseController*, substitua o `Index` método com o código a seguir, para interromper temporariamente o carregamento rápido:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Agora defina um ponto de interrupção a `return` instrução (F9 com o cursor na linha). Pressione F5 para executar o projeto no modo de depuração e selecione a página de índice do curso. Quando o código atinge o ponto de interrupção, examine o `sql` variável. Você verá a consulta que é enviada para o SQL Server. É um simples `Select` instrução.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Clique na classe de ampliação para ver a consulta a **Visualizador de texto**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Agora você adicionará uma lista suspensa para a página de índice de cursos para que os usuários podem filtrar para um determinado departamento. Você classificará os cursos por título, e você especificará o carregamento rápido para o `Department` propriedade de navegação.

Em *CourseController.cs*, substitua o `Index` método com o código a seguir:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Restaurar o ponto de interrupção no `return` instrução.

O método recebe o valor selecionado da lista suspensa no `SelectedDepartment` parâmetro. Se nada estiver selecionado, esse parâmetro será nulo.

Um `SelectList` coleção que contém todos os departamentos é passada para o modo de exibição da lista suspensa. Os parâmetros passados para o `SelectList` construtor especificar o nome do campo de valor, o nome do campo de texto e o item selecionado.

Para o `Get` método o `Course` repositório, o código especifica uma expressão de filtro, uma ordem de classificação e carregamento para rápido o `Department` propriedade de navegação. Retorna a expressão de filtro sempre `true` se nada estiver selecionado na lista suspensa (ou seja, `SelectedDepartment` é nulo).

Em *Views\Course\Index.cshtml*, imediatamente antes de abertura `table` marca, adicione o seguinte código para criar a lista suspensa e um botão de envio:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Com o ponto de interrupção definido ainda, execute a página de índice do curso. Prossiga com os primeiro vezes que o código atinge um ponto de interrupção, para que a página é exibida no navegador. Selecione um departamento na lista suspensa e clique em **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Neste momento o primeiro ponto de interrupção será usado para a consulta de departamentos da lista suspensa. Ignorar que e exibir o `query` variável na próxima vez que o código atinge o ponto de interrupção para ver o que o `Course` agora a aparência de consulta. Você verá algo semelhante ao seguinte:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Você pode ver que a consulta é agora um `JOIN` consulta carrega `Department` dados junto com o `Course` dados e que ele inclui um `WHERE` cláusula.

Remover o `var sql = courses.ToString()` linha.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Repositório e unidade de padrões de trabalho

Muitos desenvolvedores escrevem código para implementar o repositório e a unidade de padrões de trabalho como um wrapper em torno de código que funciona com o Entity Framework. Esses padrões destinam-se para criar uma camada de abstração entre a camada de acesso a dados e a camada de lógica comercial de um aplicativo. Implementando esses padrões podem ajudar a isolar seu aplicativo de alterações no repositório de dados e pode facilitar o teste de unidade automatizado ou desenvolvimento controlado por testes (TDD). No entanto, gravar código adicional para implementar esses padrões não é sempre a melhor escolha para aplicativos que usam o EF, por vários motivos:

- A própria classe de contexto EF protege seu código de código específico do repositório de dados.
- A classe de contexto EF pode agir como uma classe de unidade de trabalho para o banco de dados de atualizações que você faça usando EF.
- Recursos introduzidos no Entity Framework 6 tornam mais fácil implementar TDD sem escrever código de repositório.

Para obter mais informações sobre como implementar o repositório e a unidade de padrões de trabalho, consulte [a versão do Entity Framework 5 desta série de tutoriais](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Para obter informações sobre maneiras de implementar TDD no Entity Framework 6, consulte os seguintes recursos:

- [Como EF6 permite Mocking DbSets mais facilmente](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Teste com uma estrutura fictícia](https://msdn.microsoft.com/en-us/data/dn314429)
- [Testando com seus próprio duplicatas de teste](https://msdn.microsoft.com/en-us/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Classes de proxy

Quando o Entity Framework cria instâncias de entidade (por exemplo, quando você executa uma consulta), ele cria geralmente-los como instâncias de um tipo derivado gerado dinamicamente que atua como um proxy para a entidade. Por exemplo, consulte as seguintes duas imagens de depurador. Na primeira imagem, você vê que o `student` variável é o esperado `Student` digite imediatamente depois que você criar uma instância da entidade. A imagem de segundo após EF foi usado para ler uma entidade do aluno do banco de dados, consulte a classe proxy.

![Antes de classe de proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Após a classe de proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Essa classe de proxy substitui algumas propriedades virtuais da entidade para inserir ganchos para executar ações automaticamente quando a propriedade for acessada. Uma função para que esse mecanismo é usado é carregamento lento.

Na maioria das vezes você não precisa estar ciente deste uso de proxies, mas há exceções:

- Em alguns cenários, convém impedir que o Entity Framework criar instâncias de proxy. Por exemplo, quando a serialização de entidades você geralmente deseja POCO classes, não as classes de proxy. É uma maneira de evitar problemas de serialização serializar objetos de transferência de dados (DTOs) em vez de objetos de entidade, conforme mostrado no [usando API da Web com o Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) tutorial. Outra maneira é [desativar a criação de proxy](https://msdn.microsoft.com/en-US/data/jj592886.aspx).
- Quando você instancia uma classe de entidade usando o `new` operador, você não obtém uma instância do proxy. Isso significa que você não obtiver funcionalidade, como controle de alterações automático e carregamento lento. Isso normalmente é okey; geralmente não é necessário carregar lento, porque você está criando uma nova entidade que não está no banco de dados, e você geralmente não é necessário se você estiver marcando explicitamente a entidade que o controle de alterações `Added`. No entanto, se você precisa de carregamento lento e controle de alterações, você pode criar novas instâncias de entidade com proxies usando o [criar](https://msdn.microsoft.com/en-us/library/gg679504.aspx) método o `DbSet` classe.
- Deseja obter um tipo de entidade real de um tipo de proxy. Você pode usar o [GetObjectType](https://msdn.microsoft.com/en-us/library/system.data.objects.objectcontext.getobjecttype.aspx) método o `ObjectContext` classe ao obter o tipo de entidade real de uma instância do tipo de proxy.

Para obter mais informações, consulte [trabalhando com Proxies](https://msdn.microsoft.com/en-us/data/JJ592886.aspx) no MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Detecção de alterações automáticas

O Entity Framework determina como uma entidade foi alterado (e, portanto, as atualizações que precisam ser enviados para o banco de dados), comparando os valores atuais de uma entidade com os valores originais. Os valores originais são armazenados quando a entidade é consultada ou anexada. Alguns dos métodos que causam a detecção de alterações automáticas são os seguintes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se você estiver rastreando um grande número de entidades e chamar um desses métodos muitas vezes em um loop, você pode obter melhorias significativas de desempenho desativar temporariamente detecção de alterações automáticas usando o [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) propriedade. Para obter mais informações, consulte [automaticamente detectar alterações](https://msdn.microsoft.com/en-us/data/jj556205) no MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Validação automática

Quando você chama o `SaveChanges` método, por padrão o Entity Framework valida os dados em todas as propriedades de todas as entidades alteradas antes de atualizar o banco de dados. Se você atualizou um grande número de entidades e você já tiver validado os dados, esse trabalho é desnecessário, você pode tornar o processo de salvar as alterações terão menos tempo desativando temporariamente a validação. Você pode fazer essa usando o [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) propriedade. Para obter mais informações, consulte [validação](https://msdn.microsoft.com/en-us/data/gg193959) no MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Power Tools do Entity Framework

[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) é um suplemento do Visual Studio que foi usado para criar diagramas de modelo de dados mostrado nos tutoriais. As ferramentas também podem fazer outra função como gerar classes de entidade com base nas tabelas no banco de dados existente para que você pode usar o banco de dados com o Code First. Depois de instalar as ferramentas, algumas opções adicionais aparecem nos menus de contexto. Por exemplo, quando você clica classe contexto **Solution Explorer**, você terá uma opção para gerar um diagrama. Quando você estiver usando Code First não é possível alterar o modelo de dados no diagrama, mas você pode se mover itens para tornar mais fácil de entender.

![EF no menu de contexto](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![Diagrama EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Código do Entity Framework

O código-fonte para Entity Framework 6 está disponível em [GitHub](https://github.com/aspnet/EntityFramework6). Você poderá arquivar bugs e você pode contribuir seus próprios aprimoramentos para o código-fonte EF.

Embora o código-fonte estiver aberto, Entity Framework tem suporte completo como um produto da Microsoft. A equipe do Microsoft Entity Framework mantém controle sobre quais contribuições são aceitos e testa todas as alterações de código para assegurar a qualidade de cada versão.

<a id="summary"></a>
## <a name="summary"></a>Resumo

Isso conclui esta série de tutoriais sobre como usar o Entity Framework em um aplicativo ASP.NET MVC. Para obter mais informações sobre como trabalhar com dados usando o Entity Framework, consulte o [página de documentação do EF no MSDN](https://msdn.microsoft.com/en-us/data/ee712907) e [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obter mais informações sobre como implantar o aplicativo web depois que você construiu, consulte [implantação da Web do ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-web-deployment-content-map.md) na biblioteca MSDN.

Para obter informações sobre outros tópicos relacionados ao MVC, como autenticação e autorização, consulte o [ASP.NET MVC - recomendado recursos](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Confirmações

- Tom Dykstra gravou a versão original deste tutorial, criados junto a atualização do EF 5 e gravou a atualização 6 EF. Tom é escritor técnico de programação no Microsoft Web Platform e ferramentas de equipe de conteúdo.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) foi a maioria do trabalho de atualização do tutorial de EF 5 e MVC 4 e criados junto a atualização 6 EF. Rick é escritor técnico de programação para a Microsoft se concentra no Azure e MVC.
- [Rowan Miller](http://www.romiller.com) e outros membros da equipe do Entity Framework assistido com revisões de código e ajudaram a depurar vários problemas com migrações que ocorreu enquanto estamos foram atualizando o tutorial para EF 5 e 6 de EF.

<a id="vb"></a>
## <a name="vb"></a>VB

Quando o tutorial foi produzido para EF 4.1, fornecemos versões c# e VB do projeto concluído de download. Devido a limitações de tempo e prioridades de outros nós não tiver feito isso para esta versão. Se você compila um projeto VB usando esses tutoriais e para compartilhá-la com outras pessoas, informe-nos.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Os erros comuns e soluções ou soluções alternativas para eles

### <a name="cannot-createshadow-copy"></a>Não é possível criar/shadow copy

Mensagem de erro:

> Não é possível criar/shadow copy '&lt;filename&gt;' quando o arquivo já existe.


Solução

Aguarde alguns segundos e atualize a página.

### <a name="update-database-not-recognized"></a>Update-Database não reconhecido

Mensagem de erro (da `Update-Database` do PMC):

> O termo 'Update-Database' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome, ou se um caminho tiver sido incluído, verifique se o caminho está correto e tente novamente.


Solução

Sair do Visual Studio. Reabra o projeto e tente novamente.

### <a name="validation-failed"></a>Falha na validação

Mensagem de erro (da `Update-Database` do PMC):

> Falha na validação de uma ou mais entidades. Consulte a propriedade 'EntityValidationErrors' para obter mais detalhes.


Solução

Uma causa desse problema é erros de validação quando o `Seed` execuções do método. Consulte [Seeding e bancos de dados de depuração Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obter dicas sobre depuração de `Seed` método.

### <a name="http-50019-error"></a>HTTP 500.19 erro

Mensagem de erro:

> Erro HTTP 500.19 - erro interno do servidor  
> A página solicitada não pode ser acessada porque os dados de configuração relacionados para a página são inválidos.


Solução

Uma forma que você pode obter esse erro é de várias cópias da solução, cada um deles usando o mesmo número de porta. Normalmente, você pode resolver esse problema saindo todas as instâncias do Visual Studio, em seguida, reiniciar o projeto que você está trabalhando. Se isso não funcionar, tente alterar o número da porta. Clique com o botão direito no arquivo de projeto e, em seguida, clique em Propriedades. Selecione o **Web** guia e, em seguida, alterar o número da porta de **Url do projeto** caixa de texto.

### <a name="error-locating-sql-server-instance"></a>Instância do SQL Server localização de erro

Mensagem de erro:

> Ocorreu um erro relacionado à rede ou específico da instância ao estabelecer uma conexão com o SQL Server. O servidor não foi encontrado ou não estava acessível. Verifique se o nome da instância está correto e se o SQL Server está configurado para permitir conexões remotas. (provedor: Interfaces de rede do SQL, erro: 26 - erro ao localizar servidor/instância especificada)


Solução

Verifique a cadeia de caracteres de conexão. Se você excluiu manualmente o banco de dados, altere o nome do banco de dados na cadeia de caracteres de construção.

>[!div class="step-by-step"]
[Anterior](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
