---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "Implementando a funcionalidade básica CRUD com o Entity Framework no aplicativo ASP.NET MVC | Microsoft Docs"
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c63b8f591023b68720c523d1c9184a527a34e9cc
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2017
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implementando a funcionalidade básica CRUD com o Entity Framework no aplicativo ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


No tutorial anterior, você criou um aplicativo MVC que armazena e exibe os dados usando o Entity Framework e o LocalDB do SQL Server. Neste tutorial você examinar e personaliza o CRUD (criar, ler, atualizar e excluir) código que a estrutura MVC cria automaticamente para você em controladores e exibições.

> [!NOTE]
> É uma prática comum para implementar o padrão de repositório para criar uma camada de abstração entre o controlador e a camada de acesso a dados. Para manter esses tutoriais simples e voltada para ensinar como usar o Entity Framework em si, eles não usam repositórios. Para obter informações sobre como implementar repositórios, consulte o [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).


Neste tutorial, você criará as seguintes páginas da web:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Criar uma página de detalhes

O código de scaffolding para os alunos `Index` página de lado a `Enrollments` propriedade, porque essa propriedade contém uma coleção. No `Details` página você exibirá o conteúdo da coleção em uma tabela HTML.

 Em *Controllers\StudentController.cs*, o método de ação para o `Details` exibir usa o [localizar](https://msdn.microsoft.com/en-us/library/gg696418(v=VS.103).aspx) método para recuperar um único `Student` entidade. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

O valor da chave é passado para o método como o `id` parâmetro e vêm de *rotear dados* no **detalhes** hiperlink na página de índice.

### <a name="tip-route-data"></a>Dica: **dados de rota**

Dados de rota são dados que o associador de modelo encontrado em um segmento de URL especificado na tabela de roteamento. Por exemplo, a rota padrão especifica `controller`, `action`, e `id` segmentos:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Na URL a seguir mapeia a rota padrão `Instructor` como o `controller`, `Index` como o `action` e 1 como o `id`; esses são valores de dados de rota.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID = 2021" é um valor de cadeia de caracteres de consulta. O associador de modelo também funcionarão se você passar o `id` como um valor de cadeia de caracteres de consulta:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

As URLs são criadas pelo `ActionLink` instruções no modo de exibição Razor. No código a seguir, o `id` parâmetro corresponde a rota padrão, portanto `id` é adicionada aos dados de rota.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

No código a seguir, `courseID` não corresponde a um parâmetro de rota padrão, portanto, é adicionado como uma cadeia de caracteres de consulta.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Abra *Views\Student\Details.cshtml*. Cada campo é exibido usando um `DisplayFor` auxiliar, conforme mostrado no exemplo a seguir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Após o `EnrollmentDate` campo e imediatamente antes do fechamento `</dl>` marca, adicione o código realçado para exibir uma lista de registros, conforme mostrado no exemplo a seguir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Se o recuo do código está errado depois de colar o código, pressione CTRL-K-D para corrigi-lo.

    Esse código percorre as entidades de `Enrollments` propriedade de navegação. Para cada `Enrollment` entidade na propriedade, ele exibe o nome do curso e a classificação. O título do curso é recuperado do `Course` entidade que é armazenada no `Course` propriedade de navegação a `Enrollments` entidade. Todos esses dados são recuperados do banco de dados automaticamente quando necessário. (Em outras palavras, você está usando carregamento preguiçoso aqui. Você não especificou *carregamento adiantado* para o `Courses` propriedade de navegação, para que os registros não foram recuperados na mesma consulta que obteve os alunos. Em vez disso, na primeira vez que você tentar acessar o `Enrollments` propriedade de navegação, uma nova consulta é enviada para o banco de dados para recuperar os dados. Você pode ler mais sobre carregamento lento e carregamento adiantado no [dados relacionados de leitura](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial posteriormente na série.)
3. Execute a página, selecionando o **alunos** guia e clicando em um **detalhes** link para Alexander Carson. (Se você pressionar CTRL + F5 enquanto o arquivo Details.cshtml está aberto, você obterá um erro HTTP 400 porque o Visual Studio tenta executar a página de detalhes, mas ele não foi atingido um link que especifica o aluno para exibir. In that case, apenas remover "Aluno/detalhes" da URL e tente novamente, ou feche o navegador, clique com o botão direito e clique em **exibição**e, em seguida, clique em **exibir no navegador**.)

    Você pode ver a lista de cursos e notas do aluno selecionado:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Atualizar a página de criação

1. Em *Controllers\StudentController.cs*, substitua o `HttpPost``Create` método de ação com o código a seguir para adicionar um `try-catch` bloquear e remover `ID` do [atributo de associação](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) para o método scaffolding:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Esse código adiciona a `Student` entidade criado pelo associador de modelo do ASP.NET MVC para o `Students` entidade definida e, em seguida, salva as alterações no banco de dados. (*Associador de modelo* refere-se a funcionalidade do ASP.NET MVC que torna mais fácil para você trabalhar com dados enviados por um formulário; um associador de modelo formulário converte postadas valores para tipos CLR e passa para o método de ação em parâmetros. In this case, o associador de modelo cria um `Student` entidade usando a propriedade de valores do `Form` coleção.)

    Você removido `ID` de associação de atributo porque `ID` é o valor de chave primária do SQL Server será definida automaticamente quando a linha é inserida. Entrada do usuário não define o `ID` valor.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Aviso de segurança - o `ValidateAntiForgeryToken` atributo ajuda a impedir [falsificação de solicitação intersite](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataques. Ele requer um correspondente `Html.AntiForgeryToken()` instrução no modo de exibição, você verá mais tarde.

    O `Bind` atributo é uma maneira de proteger contra *excesso de lançamento* em criar cenários. Por exemplo, suponha que o `Student` entidade inclui um `Secret` propriedade que você não deseja que esta página da web para definir.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Mesmo se você não tiver um `Secret` campo na página da web, um hacker pode usar uma ferramenta como [fiddler](http://fiddler2.com/home), ou escrever alguns JavaScript, postar um `Secret` formar o valor. Sem o [associar](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) atributo limitando os campos que o associador de modelo usa quando ele cria uma `Student` instância*,* escolheria o associador de modelo que `Secret` formar o valor e usá-lo para criar o `Student` instância de entidade. Em seguida, o valor que o hacker especificado para o `Secret` campo de formulário será atualizado no banco de dados. A imagem a seguir mostra o fiddler ferramenta adicionando o `Secret` campo (com o valor "OverPost") para os valores de formulário postado.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    O valor "OverPost", em seguida, seria adicionado com êxito para o `Secret` propriedade inserido linha, embora você nunca se destina a página da web poderá definir essa propriedade.

    É uma prática recomendada de segurança para usar o `Include` parâmetro com o `Bind` atributo *lista branca* campos. Também é possível usar o `Exclude` parâmetro *lista negra* campos que você deseja excluir. O motivo `Include` é mais segura é que quando você adiciona uma nova propriedade para a entidade, o novo campo não é protegido automaticamente por um `Exclude` lista.

    Você pode impedir que overposting em cenários de edição está lendo a entidade do banco de dados primeiro e, em seguida, chamar `TryUpdateModel`, passando uma lista de propriedades permitidos explícita. Que é o método usado nos tutoriais.

    Uma maneira alternativa para evitar overposting é preferida por muitos desenvolvedores é usar modelos de exibição em vez de classes de entidade com associação de modelo. Inclua apenas as propriedades que você deseja atualizar o modelo de exibição. Quando tiver terminado de associador de modelo MVC, copiar as propriedades do modelo de exibição para a instância da entidade, opcionalmente usando uma ferramenta como [AutoMapper](http://automapper.org/). Use o banco de dados. Entrada na instância de entidade para definir seu estado para inalterado e, em seguida, Property("PropertyName"). IsModified como verdadeira em cada propriedade de entidade que está incluída no modelo de exibição. Esse método funciona em ambos os editar e criar cenários.

    Diferente de `Bind` atributo, o `try-catch` bloco é a única alteração que você fez para o código de scaffolding. Se uma exceção que é derivada de [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) é detectada enquanto as alterações estão sendo salvas, será exibida uma mensagem de erro genérica. [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) exceções às vezes são causadas por algo externo para o aplicativo em vez de um erro de programação para o usuário é avisado para tentar novamente. Embora não implementado neste exemplo, um aplicativo de qualidade de produção deve registrar a exceção. Para obter mais informações, consulte o **Log para mais informações** seção [monitoramento e telemetria (Criando aplicativos de nuvem de mundo Real com o Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    O código em *Views\Student\Create.cshtml* é semelhante ao que você viu no *Details.cshtml*, exceto que `EditorFor` e `ValidationMessageFor` auxiliares são usados para cada campo em vez de `DisplayFor`. Aqui está o código relevante:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* também inclui `@Html.AntiForgeryToken()`, que funciona com o `ValidateAntiForgeryToken` atributo no controlador para ajudar a impedir [falsificação de solicitação intersite](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataques.

    Nenhuma alteração é necessária no *Create.cshtml*.
2. Execute a página, selecionando o **alunos** guia e clicando em **criar novo**.
3. Digite nomes e uma data inválida e clique em **criar** para ver a mensagem de erro.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Isso é a validação do lado do servidor que você obtém por padrão. um tutorial posterior, você verá como adicionar atributos que irá gerar o código para validação do lado do cliente também. O seguinte código mostra a verificação de validação no modelo de **criar** método.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Altere a data para um valor válido e clique em **criar** para ver o novo aluno no **índice** página.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Atualizar o método HttpPost de edição

Em *Controllers\StudentController.cs*, o `HttpGet` `Edit` método (aquele sem o `HttpPost` atributo) usa o `Find` método para recuperar selecionado `Student` entidade, como você viu no `Details` método. Você não precisa alterar esse método.

No entanto, substitua o `HttpPost` `Edit` método de ação com o código a seguir:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Essas alterações implementam uma prática recomendada de segurança para evitar [mais](#overpost), o scaffolder gerado um `Bind` de atributos e adicionar a entidade criada pelo associador de modelo para a entidade definida com um sinalizador de modificação. Se o código não é recomendado porque o `Bind` atributo limpa os dados previamente existentes nos campos não listados no `Include` parâmetro. No futuro, o scaffolder do controlador MVC será atualizado para que ele não gera `Bind` atributos para os métodos de edição.

O novo código lê a entidade existente e chama [TryUpdateModel](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) atualizar campos de entrada do usuário nos dados de postagem de formulário. Conjuntos de controle de alterações automático do Entity Framework de [modificadas](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx) sinalizador na entidade. Quando o [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método é chamado, o `Modified` sinalizador faz com que o Entity Framework para criar instruções SQL para atualizar a linha de banco de dados. [Conflitos de simultaneidade](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) são ignorados, e todas as colunas da linha de banco de dados são atualizadas, incluindo aqueles que o usuário não foi alterado. (Um tutorial posterior mostra como lidar com conflitos de simultaneidade, e se você desejar somente campos individuais a serem atualizadas no banco de dados, você pode definir a entidade para inalterado e defina individuais campos a serem modificados.)

Como uma prática recomendada para evitar overposting, os campos que você deseja ser atualizável, a página Editar estão na lista de permissões no `TryUpdateModel` parâmetros. Atualmente não há nenhum campo adicional que você está protegendo, mas listar os campos que você deseja o associador de modelo para associar garante que se você adicionar campos ao modelo de dados no futuro, eles são automaticamente protegidos até que você os adicione explicitamente aqui.

Como resultado dessas alterações, a assinatura do método do método HttpPost editar é o mesmo que o método de edição HttpGet; Portanto, você já renomeou o método EditPost.

> [!TIP] 
> 
> **Estados da entidade e a anexar e métodos SaveChanges**
> 
> O contexto do banco de dados mantém o controle de entidades na memória estão em sincronia com suas linhas correspondentes no banco de dados, e essas informações determinam o que acontece quando você chamar o `SaveChanges` método. Por exemplo, quando você passar uma nova entidade para o [adicionar](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.add(v=vs.103).aspx) método, o estado da entidade definida como `Added`. Em seguida, quando você chama o [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método, o contexto do banco de dados emite um SQL `INSERT` comando.
> 
> Uma entidade pode estar em um do[estados a seguir](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx):
> 
> - `Added`. A entidade ainda não existir no banco de dados. O `SaveChanges` método deve emitir uma `INSERT` instrução.
> - `Unchanged`. Nada precisa ser feito com essa entidade pelo `SaveChanges` método. Ao ler uma entidade do banco de dados, a entidade começa com esse status.
> - `Modified`. Alguns ou todos os valores de propriedade da entidade foram modificados. O `SaveChanges` método deve emitir uma `UPDATE` instrução.
> - `Deleted`. A entidade foi marcada para exclusão. O `SaveChanges` método deve emitir uma `DELETE` instrução.
> - `Detached`. A entidade não está sendo controlada pelo contexto de banco de dados.
> 
> Em um aplicativo de área de trabalho, as alterações de estado geralmente são definidas automaticamente. Em um tipo de área de trabalho do aplicativo, uma entidade de leitura e fazer alterações em alguns de seus valores de propriedade. Isso faz com que seu estado de entidade a ser alterada automaticamente para `Modified`. Em seguida, quando você chama `SaveChanges`, o Entity Framework gera um SQL `UPDATE` instrução que atualiza apenas as propriedades reais que você tenha alterado.
> 
> A natureza desconectada de aplicativos da web não permite esta sequência contínua. O [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) que lê uma entidade for descartada depois que uma página é processada. Quando o `HttpPost` `Edit` é chamado de método de ação, uma nova solicitação é feita e você tem uma nova instância do [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx), portanto, é necessário configurar manualmente o estado da entidade `Modified.` , em seguida, quando você chama `SaveChanges`, o Entity Framework atualiza todas as colunas da linha de banco de dados, porque o contexto não tem como saber quais propriedades é alterada.
> 
> Se você quiser que o SQL `Update` instrução atualizar somente os campos que o usuário realmente alterado, você pode salvar os valores originais de alguma forma (como campos ocultos) para que eles fiquem disponíveis quando o `HttpPost` `Edit` método é chamado. Em seguida, você pode criar um `Student` entidade usando os valores originais, chamada de `Attach` método com que a versão original da entidade, atualizar os valores da entidade para os novos valores e, em seguida, chame `SaveChanges.` para obter mais informações, consulte [ Estados de entidade e SaveChanges](https://msdn.microsoft.com/en-us/data/jj592676) e [dados locais](https://msdn.microsoft.com/en-us/data/jj592872) no MSDN Data Developer Center.


O código HTML e Razor em *Views\Student\Edit.cshtml* é semelhante ao que você viu no *Create.cshtml*, e não são necessárias alterações.

Execute a página, selecionando o **alunos** guia e, em seguida, clicando em um **editar** hiperlink.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Alterar alguns dos dados e clique em **salvar**. Você vê os dados alterados na página de índice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Atualizando a página de exclusão

Em *Controllers\StudentController.cs*, o código de modelo para o `HttpGet` `Delete` método usa o `Find` método para recuperar selecionado `Student` entidade, como você viu no `Details` e `Edit` métodos. No entanto, para implementar um erro personalizado mensagem quando a chamada para `SaveChanges` falhar, você adicionará algumas funcionalidades para este método e sua exibição correspondente.

Como você viu para atualização e operações de criar, excluir operações exigem dois métodos de ação. O método é chamado em resposta a uma solicitação GET exibe uma exibição que dá ao usuário uma chance para aprovar ou cancelar a operação de exclusão. Se o usuário aprova, uma solicitação POST é criada. Quando isso acontece, o `HttpPost` `Delete` método é chamado e, em seguida, esse método realmente executa a operação de exclusão.

Você adicionará um `try-catch` bloquear o `HttpPost` `Delete` método para tratar os erros que podem ocorrer quando o banco de dados é atualizado. Se ocorrer um erro, o `HttpPost` `Delete` chamadas de método de `HttpGet` `Delete` método, passando um parâmetro que indica que ocorreu um erro. O `HttpGet Delete` método exibe novamente, em seguida, a página de confirmação, junto com a mensagem de erro, dar ao usuário a oportunidade de cancelar ou tente novamente.

1. Substitua o `HttpGet` `Delete` método de ação com o código a seguir, que gerencia o relatório de erros: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Este código aceita um [parâmetro opcional](https://msdn.microsoft.com/en-us/library/dd264739.aspx) que indica se o método foi chamado após uma falha ao salvar as alterações. Esse parâmetro é `false` quando o `HttpGet` `Delete` método for chamado sem uma falha anterior. Quando ele é chamado pelo `HttpPost` `Delete` em resposta a um erro de atualização de banco de dados, o parâmetro é `true` e uma mensagem de erro é passada para o modo de exibição.
- Substitua o `HttpPost` `Delete` método de ação (chamado `DeleteConfirmed`) com o código a seguir, que executa a operação de exclusão real e captura quaisquer erros de atualização do banco de dados.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Este código recupera a entidade selecionada, em seguida, chama o [remover](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.remove(v=vs.103).aspx) método para definir o status da entidade `Deleted`. Quando `SaveChanges` é chamado, um SQL `DELETE` comando é gerado. Você também tiver alterado o nome do método de ação de `DeleteConfirmed` para `Delete`. O código de scaffolding chamado o `HttpPost` `Delete` método `DeleteConfirmed` para dar o `HttpPost` método uma assinatura exclusiva. (Requer métodos sobrecarregados tenha parâmetros de método diferente de CLR.) Agora que as assinaturas são exclusivas, você pode continuar com a convenção MVC e usar o mesmo nome para o `HttpPost` e `HttpGet` excluir métodos.

    Se a melhorar o desempenho de um aplicativo de alto volume for uma prioridade, você pode evitar uma consulta SQL desnecessária para recuperar a linha, substituindo as linhas de código que chamam o `Find` e `Remove` métodos com o código a seguir:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Esse código cria um `Student` entidade usando apenas o valor de chave primária e, em seguida, define o estado da entidade para `Deleted`. Isso é tudo o que precisa do Entity Framework para excluir a entidade.

    Conforme observado, o `HttpGet` `Delete` método não exclui os dados. Executando uma operação de exclusão em resposta a um GET de solicitação (ou para esse fim, executar qualquer operação de edição, criar operação ou qualquer outra operação que altera dados) cria um risco de segurança. Para obter mais informações, consulte [ASP.NET MVC dica 46 — não use Excluir Links porque eles criar vulnerabilidades de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) no blog de Stephen Walther.
- Em *Views\Student\Delete.cshtml*, adicionar uma mensagem de erro entre o `h2` título e o `h3` título, conforme mostrado no exemplo a seguir:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

    Execute a página, selecionando o **alunos** guia e clicando em um **excluir** hiperlink:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
- Clique em **excluir**. A página de índice será exibida sem o aluno excluído. (Você verá um exemplo de código na ação de manipulação de erros de [tutorial de simultaneidade](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Fechando conexões de banco de dados

Para fechar conexões de banco de dados e liberar os recursos que mantenham assim que possível, descarte a instância do contexto quando tiver terminado com ele. Isto é porque o código de scaffolding fornece um [Dispose](https://msdn.microsoft.com/en-us/library/system.idisposable.dispose(v=vs.110).aspx) método no final o `StudentController` classe no *StudentController.cs*, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

A base de `Controller` já classe implementa o `IDisposable` interface, para que esse código simplesmente adiciona uma substituição para o `Dispose(bool)` método dispose explicitamente a instância do contexto.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Processamento de transações

Por padrão o Entity Framework implicitamente implementa transações. Em cenários em que você pode fazer alterações em várias linhas ou tabelas e, em seguida, chamar `SaveChanges`, o Entity Framework automaticamente garante que todas as alterações bem-sucedidas ou que todos falham. Se algumas alterações são feitas pela primeira vez e, em seguida, um erro ocorre, essas alterações são automaticamente revertidas. Para cenários em que você precisa de mais controle – por exemplo, se você quiser incluir operações feitas fora do Entity Framework em uma transação – consulte [trabalhando com transações](https://msdn.microsoft.com/en-US/data/dn456843) no MSDN.

## <a name="summary"></a>Resumo

Agora você tem um conjunto completo de páginas que executam operações CRUD simples para `Student` entidades. Você usou auxiliares do MVC para gerar elementos de interface do usuário para campos de dados. Para obter mais informações sobre os auxiliares MVC, consulte [auxiliares de HTML usando um formato de renderização](https://msdn.microsoft.com/en-us/library/dd410596(v=VS.98).aspx) (a página é para MVC 3 mas ainda é relevante para MVC 5).

No próximo tutorial você expandir a funcionalidade da página de índice, adicionando a classificação e paginação.

Deixe comentários em como você gostou neste tutorial e nós poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[Próximo](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
