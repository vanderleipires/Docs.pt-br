---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementando a funcionalidade CRUD básica com o Entity Framework no aplicativo ASP.NET MVC (2 de 10) | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f580ba6d0db07430e991fba2d061bb2632c95353
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817077"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementando a funcionalidade CRUD básica com o Entity Framework no aplicativo ASP.NET MVC (2 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais de início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece por aqui.
> 
> > [!NOTE] 
> > 
> > Se você enfrentar um problema que você não conseguir resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução ao problema comparando seu código com o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


No tutorial anterior, você criou um aplicativo MVC que armazena e exibe os dados usando o Entity Framework e o LocalDB do SQL Server. Neste tutorial você examinará e personalizará o CRUD (criar, ler, atualizar e excluir) que o scaffolding do MVC cria automaticamente para você em controladores e modos de exibição de código.

> [!NOTE]
> É uma prática comum implementar o padrão de repositório para criar uma camada de abstração entre o controlador e a camada de acesso a dados. Para manter esses tutoriais simples, você não implementar um repositório até um tutorial posterior nesta série.


Neste tutorial, você criará as seguintes páginas da web:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Criando uma página de detalhes

O código gerado automaticamente para os alunos `Index` página deixada de fora a `Enrollments` propriedade, porque essa propriedade contém uma coleção. No `Details` página você exibirá o conteúdo da coleção em uma tabela HTML.

 Na *Controllers\StudentController.cs*, o método de ação para o `Details` exibir usa o `Find` método para recuperar uma única `Student` entidade. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 O valor da chave é passado para o método como o `id` parâmetro e vem dos dados de rota na **detalhes** hiperlink na página de índice. 

1. Abra *Views\Student\Details.cshtml*. Cada campo é exibido usando um `DisplayFor` auxiliares, conforme mostrado no exemplo a seguir: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Após o `EnrollmentDate` campo e imediatamente antes do fechamento `fieldset` marca, adicione o código para exibir uma lista de registros, conforme mostrado no exemplo a seguir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Esse código percorre as entidades na propriedade de navegação `Enrollments`. Para cada `Enrollment` entidade na propriedade, ele exibe o título do curso e a classificação. O título do curso é recuperado do `Course` entidade que é armazenada em de `Course` propriedade de navegação do `Enrollments` entidade. Todos esses dados são recuperados do banco de dados automaticamente quando for necessário. (Em outras palavras, você está usando o carregamento lento aqui. Você não especificou *o carregamento adiantado* para o `Courses` propriedade de navegação, portanto, na primeira vez que você tentar acessar essa propriedade, uma consulta é enviada para o banco de dados para recuperar os dados. Você pode ler mais sobre o carregamento lento e o carregamento adiantado na [lendo dados relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial posteriormente nessa série.)
3. Execute a página, selecionando o **alunos** guia e clicando em um **detalhes** link para Alexander Carson. Você verá a lista de cursos e notas do aluno selecionado:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Atualizando a página criar

1. Na *Controllers\StudentController.cs*, substitua o `HttpPost``Create` método de ação com o código a seguir para adicionar um `try-catch` bloco e o [atributo Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) para o método gerado por scaffolding: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Esse código adiciona o `Student` entidade criada pelo associador de modelo ASP.NET MVC para o `Students` entidade definida e, em seguida, salva as alterações no banco de dados. (*Associador de modelo* refere-se a funcionalidade do ASP.NET MVC que torna mais fácil para você trabalhar com os dados enviados por um formulário; um associador de modelo converte postado formulário valores para tipos CLR e passa para o método de ação em parâmetros. In this case, o associador de modelo cria uma instância de um `Student` os valores de entidade usando a propriedade do `Form` coleção.)

    O `ValidateAntiForgeryToken` atributo ajuda a impedir [falsificação de solicitação intersite](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataques.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Execute a página, selecionando o **alunos** guia e clicando em **criar novo**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Alguma validação de dados funciona por padrão. Insira nomes e uma data inválida e clique em **criar** para ver a mensagem de erro.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    O seguinte código realçado mostra a verificação de validação de modelo.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Altere a data para um valor válido, como 9/1/2005 e clique em **Create** para ver o novo aluno aparecem na **índice** página.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Atualizando a página Editar POSTAGEM

Na *Controllers\StudentController.cs*, o `HttpGet` `Edit` método (aquele sem o `HttpPost` atributo) usa o `Find` método para recuperar o selecionado `Student` entidade, como você viu no `Details` método. Não é necessário alterar esse método.

No entanto, substituir os `HttpPost` `Edit` método de ação com o código a seguir para adicionar um `try-catch` bloco e o [atributo Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Esse código é semelhante ao que você viu na `HttpPost` `Create` método. No entanto, em vez de adicionar a entidade criada pelo associador de modelos ao conjunto de entidades, esse código define um sinalizador na entidade indicando que ela foi alterada. Quando o [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método é chamado, o [modificado](https://msdn.microsoft.com/library/system.data.entitystate.aspx) sinalizador faz com que o Entity Framework criar instruções SQL para atualizar a linha de banco de dados. Todas as colunas da linha de banco de dados serão atualizadas, incluindo aqueles que o usuário não for alterado, e conflitos de simultaneidade são ignorados. (Você aprenderá a lidar com a simultaneidade em um tutorial posterior nesta série.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Estados da entidade e os métodos de SaveChanges e anexar

O contexto de banco de dados controla se as entidades em memória estão em sincronia com suas linhas correspondentes no banco de dados, e essas informações determinam o que acontece quando você chama o método `SaveChanges`. Por exemplo, quando você passa uma nova entidade para o [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) método, o que o estado da entidade é definido como `Added`. Em seguida, quando você chama o [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método, o contexto de banco de dados emite um SQL `INSERT` comando.

Uma entidade pode estar em um dos[estados a seguir](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. A entidade ainda não existe no banco de dados. O `SaveChanges` método deve emitir uma `INSERT` instrução.
- `Unchanged`. Nada precisa ser feito com essa entidade pelo método `SaveChanges`. Ao ler uma entidade do banco de dados, a entidade começa com esse status.
- `Modified`. Alguns ou todos os valores de propriedade da entidade foram modificados. O `SaveChanges` método deve emitir uma `UPDATE` instrução.
- `Deleted`. A entidade foi marcada para exclusão. O `SaveChanges` método deve emitir uma `DELETE` instrução.
- `Detached`. A entidade não está sendo controlada pelo contexto de banco de dados.

Em um aplicativo da área de trabalho, em geral, as alterações de estado são definidas automaticamente. Em um tipo de área de trabalho do aplicativo, você lê uma entidade e fazer alterações em alguns de seus valores de propriedade. Isso faz com que seu estado da entidade seja alterado automaticamente para `Modified`. Em seguida, quando você chama `SaveChanges`, o Entity Framework gera um SQL `UPDATE` instrução que atualiza apenas as propriedades reais que você alterou.

A natureza desconectada do aplicativos web não permite que essa sequência contínua. O [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) que lê uma entidade é descartada depois que uma página é renderizada. Quando o `HttpPost` `Edit` método de ação é chamado, uma nova solicitação é feita e você tem uma nova instância das [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), para que você precise definir manualmente o estado da entidade para `Modified.` e em seguida, quando você chama `SaveChanges`, o Entity Framework atualiza todas as colunas da linha de banco de dados, porque o contexto não tem como saber quais propriedades foram alteradas.

Se você quiser que o SQL `Update` instrução para atualizar somente os campos que o usuário realmente foram alterado, você pode salvar os valores originais de alguma forma (como campos ocultos) para que eles estejam disponíveis quando o `HttpPost` `Edit` método é chamado. Em seguida, você pode criar uma `Student` entidade usando os valores originais, chamada a `Attach` método com a versão original da entidade, atualize os valores da entidade para os novos valores e, em seguida, chame `SaveChanges.` para obter mais informações, consulte [ Estados da entidade e SaveChanges](https://msdn.microsoft.com/data/jj592676) e [dados locais](https://msdn.microsoft.com/data/jj592872) no MSDN Data Developer Center.

O código na *Views\Student\Edit.cshtml* é semelhante ao que você viu na *Create. cshtml*, e nenhuma alteração é necessária.

Execute a página, selecionando o **alunos** guia e, em seguida, clicando em um **editar** hiperlink.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Altere alguns dos dados e clique em **Salvar**. Você verá os dados alterados na página de índice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Atualizando a página Excluir

No *Controllers\StudentController.cs*, o código de modelo para o `HttpGet` `Delete` método usa o `Find` método para recuperar o selecionados `Student` entidade, como você viu no `Details` e `Edit` métodos. No entanto, para implementar uma mensagem de erro personalizada quando a chamada a `SaveChanges` falhar, você adicionará uma funcionalidade a esse método e à sua exibição correspondente.

Como você viu para operações de atualização e criação, as operações de exclusão exigem dois métodos de ação. O método que é chamado em resposta a uma solicitação GET mostra uma exibição que dá ao usuário a oportunidade de aprovar ou cancelar a operação de exclusão. Se o usuário aprová-la, uma solicitação POST será criada. Quando isso acontece, o `HttpPost` `Delete` método é chamado e, em seguida, esse método, na verdade, executa a operação de exclusão.

Você adicionará uma `try-catch` bloco para o `HttpPost` `Delete` método para tratar os erros que podem ocorrer quando o banco de dados é atualizado. Se ocorrer um erro, o `HttpPost` `Delete` chamadas de método de `HttpGet` `Delete` método, passando um parâmetro que indica que ocorreu um erro. O `HttpGet Delete` método, em seguida, exibe novamente a página de confirmação, junto com a mensagem de erro, dando ao usuário uma oportunidade de cancelar ou tentar novamente.

1. Substitua os `HttpGet` `Delete` método de ação com o código a seguir, que gerencia o relatório de erros: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Este código aceita uma [opcional](https://msdn.microsoft.com/library/dd264739.aspx) parâmetro booliano que indica se ele foi chamado após uma falha ao salvar as alterações. Esse parâmetro é `false` quando o `HttpGet` `Delete` método for chamado sem uma falha anterior. Quando ele é chamado o `HttpPost` `Delete` método em resposta a um erro de atualização de banco de dados, o parâmetro é `true` e uma mensagem de erro é passada para o modo de exibição.
2. Substitua os `HttpPost` `Delete` método de ação (chamado `DeleteConfirmed`) com o código a seguir, que executa a operação de exclusão real e captura os erros de atualização do banco de dados.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Esse código recupera a entidade selecionada, em seguida, chama o [remova](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) método para definir o status da entidade `Deleted`. Quando `SaveChanges` é chamado, um SQL `DELETE` comando é gerado. Você também alterou o nome do método de ação de `DeleteConfirmed` para `Delete`. O código gerado por scaffolding chamado a `HttpPost` `Delete` método `DeleteConfirmed` para dar o `HttpPost` método uma assinatura exclusiva. (O CLR exige métodos sobrecarregados tenham diferentes parâmetros de método). Agora que as assinaturas são exclusivas, você pode continuar com a convenção do MVC e usar o mesmo nome para o `HttpPost` e `HttpGet` excluir métodos.

     Se a melhoria do desempenho de um aplicativo de alto volume for uma prioridade, você poderia evitar uma consulta SQL desnecessária para recuperar a linha, substituindo as linhas de código que chamem o `Find` e `Remove` métodos com o código a seguir, conforme mostrado em amarelo Destaque:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Esse código instancia uma `Student` entidade usando apenas o valor de chave primária e, em seguida, define o estado da entidade para `Deleted`. Isso é tudo o que o Entity Framework precisa para excluir a entidade.

     Conforme observado, o `HttpGet` `Delete` método não exclui os dados. Executando uma operação de exclusão em resposta a um GET de solicitação (ou para fato, a execução de qualquer operação de edição, criação ou qualquer outra operação que altera os dados) cria um risco à segurança. Para obter mais informações, consulte [ASP.NET MVC dica n º 46 — não use Excluir Links porque eles criaram buracos na segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) no blog de Stephen Walther.
3. Na *Views\Student\Delete.cshtml*, adicione uma mensagem de erro entre o `h2` título e o `h3` título, conforme mostrado no exemplo a seguir:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Execute a página, selecionando o **alunos** guia e clicando em um **excluir** hiperlink:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Clique em **Excluir**. A página Índice será exibida sem o aluno excluído. (Você verá um exemplo de código na ação de manipulação de erros do [tratamento de simultaneidade](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial posteriormente nessa série.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Garantindo que as conexões de banco de dados não são deixadas abertas

Para certificar-se de que as conexões de banco de dados são fechadas corretamente e os recursos mantêm liberados para cima, você deverá ver a ele que a instância de contexto for descartada. Isto é porque o código gerado por scaffolding fornece um [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) método no final do `StudentController` classe *StudentController.cs*, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

A base `Controller` já classe implementa o `IDisposable` interface, portanto, esse código simplesmente adiciona uma substituição para o `Dispose(bool)` método para descartar explicitamente a instância do contexto.

## <a name="summary"></a>Resumo

Agora você tem um conjunto completo de páginas que executam operações CRUD simples para `Student` entidades. Você usou os auxiliares de MVC para gerar os elementos de interface do usuário para campos de dados. Para obter mais informações sobre os auxiliares de MVC, consulte [auxiliares de HTML usando um formato de renderização](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (a página é para o MVC 3, mas ainda é relevante para MVC 4).

No próximo tutorial, você expandirá a funcionalidade da página de índice, adicionando a classificação e paginação.

Links para outros recursos do Entity Framework podem ser encontradas na [mapa de conteúdo de acesso do ASP.NET dados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Próximo](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
