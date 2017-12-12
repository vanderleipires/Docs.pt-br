---
title: "Núcleo do ASP.NET MVC com núcleo EF - CRUD - 2 de 10"
author: tdykstra
description: 
keywords: ASP.NET Core, Entity Framework Core, CRUD, criar, ler, atualizar, excluir
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 6e1cd570-40f1-4b24-8b6e-7d2d27758f18
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/crud
ms.openlocfilehash: 9fc2b4c126c4d109deb2125f0db70a355c04eb15
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>Criar, ler, atualizar e excluir -Tutorial EF Core comASP.NET Core MVC (2 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos do ASP.NET MVC de núcleo da web usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

No tutorial anterior, você criou um aplicativo MVC que armazena e exibe os dados usando o Entity Framework e o LocalDB do SQL Server. Neste tutorial, você examine e personaliza o CRUD (criar, ler, atualizar e excluir) código que a estrutura MVC cria automaticamente para você em controladores e exibições.

> [!NOTE] 
> É uma prática comum para implementar o padrão de repositório para criar uma camada de abstração entre o controlador e a camada de acesso a dados. Para manter esses tutoriais simples e voltada para ensinar como usar o Entity Framework em si, eles não usam repositórios. Para obter informações sobre repositórios com EF, consulte [o último tutorial nesta série](advanced.md).

Neste tutorial, você trabalhará com as seguintes páginas da web:

![Página de detalhes do aluno](crud/_static/student-details.png)

![Página de criação do aluno](crud/_static/student-create.png)

![Página de edição do aluno](crud/_static/student-edit.png)

![Página de exclusão do aluno](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>Personalizar a página de detalhes

O código de scaffolding para a página de índice de alunos de lado a `Enrollments` propriedade, porque essa propriedade contém uma coleção. No **detalhes** página, você poderá exibir o conteúdo da coleção em uma tabela HTML.

Em *Controllers/StudentsController.cs*, o método de ação para os detalhes da exibição usa o `SingleOrDefaultAsync` método para recuperar um único `Student` entidade. Adicione o código que chama `Include`. `ThenInclude`, e `AsNoTracking` métodos, como mostra o seguinte código realçado.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

O `Include` e `ThenInclude` métodos fazer com que o contexto para carregar o `Student.Enrollments` propriedade de navegação e dentro de cada registro de `Enrollment.Course` propriedade de navegação.  Você aprenderá mais sobre esses métodos no [ler dados relacionados](read-related-data.md) tutorial.

O `AsNoTracking` método melhora o desempenho em cenários em que as entidades retornadas não serão atualizadas no tempo de vida do contexto atual. Você aprenderá mais sobre `AsNoTracking` no final deste tutorial.

### <a name="route-data"></a>Dados de rota

O valor da chave que é passado para o `Details` proveniente do método *rotear dados*. Dados de rota são dados que o associador de modelo encontrado em um segmento da URL. Por exemplo, a rota padrão especifica os segmentos de controlador, ação e id:

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

Na URL a seguir, a rota padrão mapeia instrutor como o controlador, o índice como a ação e 1 como o id; Estes são valores de dados de rota.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

A última parte da URL ("? courseID = 2021") é um valor de cadeia de caracteres de consulta. O associador de modelo irá passar o valor de ID para o `Details` método `id` parâmetro se você passá-lo como um valor de cadeia de caracteres de consulta:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Na página de índice, as URLs de hiperlinks são criadas por instruções de auxiliar de marca no modo de exibição Razor. No código a seguir Razor, o `id` parâmetro corresponde a rota padrão, portanto `id` é adicionada aos dados de rota.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Isso gera o seguinte HTML quando `item.ID` é 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

No código a seguir Razor, `studentID` não corresponde a um parâmetro de rota padrão, portanto, é adicionado como uma cadeia de caracteres de consulta.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Isso gera o seguinte HTML quando `item.ID` é 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Para obter mais informações sobre os auxiliares de marcação, consulte [auxiliares de marcação no ASP.NET Core](xref:mvc/views/tag-helpers/intro).

### <a name="add-enrollments-to-the-details-view"></a>Adicionar registros para o modo de exibição de detalhes

Abra *Views/Students/Details.cshtml*. Cada campo é exibido usando `DisplayNameFor` e `DisplayFor` auxiliares, conforme mostrado no exemplo a seguir:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Após o último campo e imediatamente antes do fechamento `</dl>` marca, adicione o seguinte código para exibir uma lista de registros:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Se o recuo do código está errado depois de colar o código, pressione CTRL-K-D para corrigi-lo.

Esse código percorre as entidades de `Enrollments` propriedade de navegação. Para cada registro, ele exibe o nome do curso e a classificação. O título do curso é recuperado da entidade curso que é armazenada no `Course` propriedade de navegação de entidade de registros.

Executar o aplicativo, selecione o **alunos** guia e, em seguida, clique no **detalhes** link para um aluno. Você pode ver a lista de cursos e notas do aluno selecionado:

![Página de detalhes do aluno](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Atualizar a página de criação

Em *StudentsController.cs*, modifique o HttpPost `Create` método adicionando um bloco try-catch e removendo o ID do `Bind` atributo.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Esse código adiciona a entidade de aluno criada pelo associador de modelo do ASP.NET MVC para a entidade de alunos definido e, em seguida, salva as alterações no banco de dados. (Associador de modelo refere-se a funcionalidade do ASP.NET MVC que torna mais fácil para você trabalhar com dados enviados por um formulário; um associador de modelo converte valores de formulário postado para tipos de CLR e os passa para o método de ação em parâmetros. Nesse caso, o associador de modelo cria uma entidade do aluno usando valores de propriedade da coleção de formulário.)

Você removido `ID` do `Bind` atributo porque a ID é o valor de chave primária do SQL Server será definida automaticamente quando a linha é inserida. Entrada do usuário não define o valor de ID.

Diferente de `Bind` atributo, o bloco try-catch é a única alteração que você fez para o código de scaffolding. Se uma exceção que é derivada de `DbUpdateException` é detectada enquanto as alterações estão sendo salvas, será exibida uma mensagem de erro genérica. `DbUpdateException`exceções às vezes são causadas por algo externo para o aplicativo em vez de um erro de programação para que o usuário é avisado para tentar novamente. Embora não implementado neste exemplo, um aplicativo de qualidade de produção deve registrar a exceção. Para obter mais informações, consulte o **Log para mais informações** seção [monitoramento e telemetria (Criando aplicativos de nuvem de mundo Real com o Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

O `ValidateAntiForgeryToken` atributo ajuda a impedir ataques CSRF (falsificação) de solicitação entre sites. O token é injetado automaticamente o modo de exibição de [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) e é incluído quando o formulário é enviado pelo usuário. O token é validado pelo `ValidateAntiForgeryToken` atributo. Para obter mais informações sobre CSRF, consulte [falsificação de anti-solicitação](../../security/anti-request-forgery.md).

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Observação de segurança sobre overposting

O `Bind` atributo que inclui o código de scaffolding sobre o `Create` método é uma maneira de proteger contra overposting em criar cenários. Por exemplo, suponha que a entidade do aluno inclui um `Secret` propriedade que você não deseja que esta página da web para definir.

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

Mesmo se você não tiver um `Secret` campo na página da web, um hacker pode usar uma ferramenta como o Fiddler ou escrever alguns JavaScript, postar um `Secret` formar o valor. Sem o `Bind` escolheria atributo limitando os campos que o associador de modelo usa quando ele cria uma instância do aluno, o associador de modelo que `Secret` formar o valor e usá-lo para criar a instância de entidade do aluno. Em seguida, o valor que o hacker especificado para o `Secret` campo de formulário será atualizado no banco de dados. A imagem a seguir mostra a adição de ferramenta Fiddler o `Secret` campo (com o valor "OverPost") para os valores de formulário postado.

![Campo secreto adição do Fiddler](crud/_static/fiddler.png)

O valor "OverPost", em seguida, seria adicionado com êxito para o `Secret` propriedade inserido linha, embora você nunca se destina a página da web poderá definir essa propriedade.

Você pode impedir que overposting em cenários de edição lendo a entidade do banco de dados primeiro e, em seguida, chamar `TryUpdateModel`, passando uma lista de propriedades permitidos explícita. Que é o método usado nos tutoriais.

Uma maneira alternativa para evitar overposting é preferida por muitos desenvolvedores é usar modelos de exibição em vez de classes de entidade com associação de modelo. Inclua apenas as propriedades que você deseja atualizar o modelo de exibição. Quando tiver terminado de associador de modelo MVC, copie as propriedades do modelo de exibição para a instância da entidade, opcionalmente usando uma ferramenta como AutoMapper. Use `_context.Entry` na instância de entidade para definir seu estado como `Unchanged`e, em seguida, defina `Property("PropertyName").IsModified` como verdadeira em cada propriedade de entidade que está incluída no modelo de exibição. Esse método funciona em ambos os editar e criar cenários.

### <a name="test-the-create-page"></a>Página de criação de teste

O código em *Views/Students/Create.cshtml* usa `label`, `input`, e `span` (para mensagens de validação) auxiliares para cada campo de marca.

Executar o aplicativo, selecione o **alunos** guia e, em seguida, clique em **criar novo**.

Digite nomes e uma data. Tente digitar uma data inválida, se seu navegador permite fazer isso. (Alguns navegadores forçá-lo a usar um seletor de data.) Em seguida, clique em **criar** para ver a mensagem de erro.

![Erro de validação de data](crud/_static/date-error.png)

Isso é a validação do lado do servidor que você obtém por padrão. um tutorial posterior, você verá como adicionar atributos que irá gerar o código para validação do lado do cliente também. O seguinte código mostra a verificação de validação no modelo de `Create` método.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Altere a data para um valor válido e clique em **criar** para ver o novo aluno no **índice** página.

## <a name="update-the-edit-page"></a>Atualizar a página de edição

Em *StudentController.cs*, o HttpGet `Edit` método (aquele sem o `HttpPost` atributo) usa o `SingleOrDefaultAsync` método para recuperar a entidade selecionada do aluno, como você viu no `Details` método. Você não precisa alterar esse método.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Recomendado HttpPost editar código: leitura e atualização

Substitua o método de ação HttpPost Editar com o código a seguir.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Essas alterações implementam uma prática recomendada de segurança para evitar overposting. O scaffolder gerado um `Bind` de atributos e adicionar a entidade criada pelo associador de modelo para a entidade definida com um `Modified` sinalizador. Que o código não é recomendado para muitos cenários porque o `Bind` limpa os dados previamente existentes nos campos não listados no atributo de `Include` parâmetro.

O novo código lê a entidade existente e chama `TryUpdateModel` para atualizar os campos a entidade recuperada [com base na entrada do usuário nos dados de formulário postado](xref:mvc/models/model-binding#how-model-binding-works). Conjuntos de controle de alterações automático do Entity Framework a `Modified` sinalizador nos campos de entrada de formulário são alterados. Quando o `SaveChanges` método é chamado, o Entity Framework cria instruções SQL para atualizar a linha de banco de dados. Conflitos de simultaneidade são ignorados e somente as colunas da tabela que foram atualizadas pelo usuário são atualizadas no banco de dados. (Mostra como lidar com conflitos de simultaneidade de um tutorial posterior.)

Como prática recomendada para evitar mais os campos que você deseja ser atualizáveis pelo **editar** página estão na lista de permissões no `TryUpdateModel` parâmetros. (Cadeia de caracteres vazia anterior da lista de campos na lista de parâmetros é um prefixo a ser usado com os nomes de campos do formulário.) Atualmente não há nenhum campo adicional que você está protegendo, mas listar os campos que você deseja o associador de modelo para associar garante que se você adicionar campos ao modelo de dados no futuro, eles são automaticamente protegidos até que você os adicione explicitamente aqui.

Como resultado dessas alterações, a assinatura de método do HttpPost `Edit` método é o mesmo que o HttpGet `Edit` método; portanto, você já renomeou o método `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Código de edição HttpPost alternativos: criar e anexar

O código de edição HttpPost recomendado garante que apenas as colunas alteradas são atualizados e preserva os dados nas propriedades que você não deseja incluídos para associação de modelo. No entanto, a abordagem de leitura requer um banco de dados extra de leitura e pode resultar em código mais complexo para manipular conflitos de simultaneidade. Uma alternativa é anexar uma entidade criada pelo associador de modelo para o contexto EF e marcá-la como modificado. (Não atualizar seu projeto com esse código, ele tem mostrado somente para ilustrar uma abordagem opcional.) 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Você pode usar essa abordagem quando a página da web da interface do usuário inclui todos os campos da entidade e pode atualizar qualquer um deles.

O código de scaffolding usa a abordagem de criar e anexar, mas apenas captura `DbUpdateConcurrencyException` exceções e retorna códigos de erro 404.  O exemplo mostrado captura qualquer exceção de atualização de banco de dados e exibe uma mensagem de erro.

### <a name="entity-states"></a>Estados da entidade

O contexto do banco de dados mantém o controle de entidades na memória estão em sincronia com suas linhas correspondentes no banco de dados, e essas informações determinam o que acontece quando você chamar o `SaveChanges` método. Por exemplo, quando você passar uma nova entidade para o `Add` método, o estado da entidade definida como `Added`. Em seguida, quando você chama o `SaveChanges` método, o contexto do banco de dados emite um comando INSERT SQL.

Uma entidade pode estar em um dos seguintes estados:

* `Added`. A entidade ainda não existir no banco de dados. O `SaveChanges` método emite uma instrução INSERT.

* `Unchanged`. Nada precisa ser feito com essa entidade pelo `SaveChanges` método. Ao ler uma entidade do banco de dados, a entidade começa com esse status.

* `Modified`. Alguns ou todos os valores de propriedade da entidade foram modificados. O `SaveChanges` método emite uma instrução UPDATE.

* `Deleted`. A entidade foi marcada para exclusão. O `SaveChanges` método emite uma instrução DELETE.

* `Detached`. A entidade não está sendo controlada pelo contexto de banco de dados.

Em um aplicativo de área de trabalho, as alterações de estado geralmente são definidas automaticamente. Uma entidade de leitura e fazer alterações em alguns de seus valores de propriedade. Isso faz com que seu estado de entidade a ser alterada automaticamente para `Modified`. Em seguida, quando você chama `SaveChanges`, o Entity Framework gera uma instrução de atualização do SQL que atualiza apenas as propriedades reais que você tenha alterado.

Em um aplicativo web, o `DbContext` que inicialmente lê uma entidade e exibe seus dados a ser editado for descartados depois que uma página é processada. Quando o HttpPost `Edit` é chamado de método de ação, é feita uma nova solicitação de web e você tem uma nova instância do `DbContext`. Se você ler novamente a entidade no novo contexto, você pode simular o processamento de área de trabalho.

Mas se você não quiser fazer o extra de operação de leitura, você deve usar o objeto de entidade criado pelo associador de modelo.  A maneira mais simples de fazer isso é definir o estado da entidade como modificadas como é feito em alternativo HttpPost editar código mostrado anteriormente. Em seguida, quando você chama `SaveChanges`, o Entity Framework atualiza todas as colunas da linha de banco de dados, porque o contexto não tem como saber quais propriedades é alterada.

Se você quiser evitar a abordagem de leitura, mas você deseja que a instrução SQL UPDATE para atualizar somente os campos que o usuário realmente foram alterado, o código é mais complexo. É necessário salvar os valores originais de alguma forma (como usando campos ocultos) para que eles fiquem disponíveis quando o HttpPost `Edit` método é chamado. Em seguida, você pode criar uma entidade de aluno usando os valores originais, chamada de `Attach` método com que a versão original da entidade, atualizar os valores da entidade para os novos valores e, em seguida, chame `SaveChanges`.

### <a name="test-the-edit-page"></a>A página de edição de teste

Executar o aplicativo, selecione o **alunos** guia e, em seguida, clique em uma **editar** hiperlink.

![Página de edição de alunos](crud/_static/student-edit.png)

Alterar alguns dos dados e clique em **salvar**. O **índice** página é aberta e você vê os dados alterados.

## <a name="update-the-delete-page"></a>Atualizar a página de exclusão

Em *StudentController.cs*, o código de modelo para o HttpGet `Delete` método usa o `SingleOrDefaultAsync` método para recuperar a entidade selecionada do aluno, como você viu nos detalhes e editar métodos. No entanto, para implementar um erro personalizado mensagem quando a chamada para `SaveChanges` falhar, você adicionará algumas funcionalidades para este método e sua exibição correspondente.

Como você viu para atualização e operações de criar, excluir operações exigem dois métodos de ação. O método é chamado em resposta a uma solicitação GET exibe uma exibição que dá ao usuário uma chance para aprovar ou cancelar a operação de exclusão. Se o usuário aprova, uma solicitação POST é criada. Quando isso acontece, o HttpPost `Delete` método é chamado e, em seguida, esse método realmente executa a operação de exclusão.

Você adicionará um bloco try-catch para o HttpPost `Delete` método para tratar os erros que podem ocorrer quando o banco de dados é atualizado. Se ocorrer um erro, o método HttpPost Delete chama o método HttpGet Delete, passando um parâmetro que indica que ocorreu um erro. O método HttpGet Delete, em seguida, exibe novamente a página de confirmação, junto com a mensagem de erro, dar ao usuário a oportunidade de cancelar ou tente novamente.

Substitua o HttpGet `Delete` método de ação com o código a seguir, que gerencia o relatório de erros.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Este código aceita um parâmetro opcional que indica se o método foi chamado após uma falha ao salvar as alterações. Esse parâmetro é false quando o HttpGet `Delete` método for chamado sem uma falha anterior. Quando ele é chamado pelo HttpPost `Delete` método em resposta a um erro de atualização de banco de dados, o parâmetro for true, e uma mensagem de erro é passada para o modo de exibição.

### <a name="the-read-first-approach-to-httppost-delete"></a>A abordagem de leitura para HttpPost Delete

Substitua o HttpPost `Delete` método de ação (chamado `DeleteConfirmed`) com o código a seguir, que executa a operação de exclusão real e captura quaisquer erros de atualização do banco de dados.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Este código recupera a entidade selecionada, em seguida, chama o `Remove` método para definir o status da entidade `Deleted`. Quando `SaveChanges` é chamado, um SQL DELETE comando é gerado.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>A abordagem de criar e anexar para HttpPost Delete

Se a melhorar o desempenho de um aplicativo de alto volume for uma prioridade, você pode evitar uma consulta SQL desnecessária criando uma instância de uma entidade de aluno usando somente o primário da chave de valor e, em seguida, definir o estado da entidade `Deleted`. Isso é tudo o que precisa do Entity Framework para excluir a entidade. (Não coloque esse código no seu projeto; aqui é apenas para ilustrar alternativa).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Se a entidade tiver relacionadas a dados também devem ser excluídos, certifique-se de que a exclusão em cascata é configurada no banco de dados. Com essa abordagem para exclusão de entidade, o EF talvez não percebam há entidades relacionadas a ser excluído.

### <a name="update-the-delete-view"></a>Atualizar a exibição de exclusão

Em *Views/Student/Delete.cshtml*, adicionar uma mensagem de erro entre o título de h2 e título h3, conforme mostrado no exemplo a seguir:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Executar o aplicativo, selecione o **alunos** guia e, em seguida, clique em uma **excluir** hiperlink:

![Excluir a página de confirmação](crud/_static/student-delete.png)

Clique em **excluir**. A página de índice será exibida sem o aluno excluído. (Você verá um exemplo de código na ação no tutorial de simultaneidade de manipulação de erros.)

## <a name="closing-database-connections"></a>Fechar conexões de banco de dados

Para liberar os recursos que contém uma conexão de banco de dados, a instância de contexto deve ser descartada assim que possível quando você tiver terminado com ele. O interno do ASP.NET Core [injeção de dependência](../../fundamentals/dependency-injection.md) cuida dessa tarefa para você.

Em *Startup.cs*, você chamar o [o método de extensão AddDbContext](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) para provisionar o `DbContext` classe no contêiner de injeção de dependência do ASP.NET. Que método define o tempo de vida do serviço `Scoped` por padrão. `Scoped`significa o tempo de vida do objeto de contexto coincide com o tempo de vida de solicitação da web, e o `Dispose` método será chamado automaticamente no final da solicitação da web.

## <a name="handling-transactions"></a>Processamento de transações

Por padrão o Entity Framework implicitamente implementa transações. Em cenários em que você pode fazer alterações em várias linhas ou tabelas e, em seguida, chamar `SaveChanges`, automaticamente, o Entity Framework torna-se de que todas as alterações bem-sucedidas ou todos eles falham. Se algumas alterações são feitas pela primeira vez e, em seguida, um erro ocorre, essas alterações são automaticamente revertidas. Para cenários em que você precisa de mais controle – por exemplo, se você quiser incluir operações feitas fora do Entity Framework em uma transação – consulte [transações](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Consultas de acompanhamento não

Quando um contexto de banco de dados recupera linhas da tabela e cria objetos de entidade que representam-los, por padrão ele mantém o controle de se as entidades na memória estão em sincronia com o que está no banco de dados. Os dados na memória atua como um cache e são usados quando você atualizar uma entidade. Esse cache geralmente é desnecessário em um aplicativo web porque as instâncias de contexto são normalmente curta duração (uma nova é criada e descartado para cada solicitação) e o contexto que lê uma entidade normalmente é descartada antes que essa entidade é usada novamente.

Você pode desabilitar o controle de objetos de entidade na memória chamando o `AsNoTracking` método. Os cenários típicos em que você talvez queira fazer isso incluem o seguinte:

* Durante o tempo de vida de contexto não é necessário atualizar quaisquer entidades, e não é necessário EF para [carregar automaticamente as propriedades de navegação com entidades recuperadas por consultas separadas](read-related-data.md). Com frequência, essas condições forem atendidas em métodos de ação HttpGet do controlador.

* Você estiver executando uma consulta que recupera um grande volume de dados, e apenas uma pequena parte dos dados retornados será atualizada. Pode ser mais eficiente para desativar o controle para a consulta de grande e executar uma consulta mais tarde para algumas entidades que precisam ser atualizados.

* Para anexar uma entidade para atualizá-lo, mas anteriormente recuperados da mesma entidade para uma finalidade diferente. Porque a entidade já está sendo controlada pelo contexto de banco de dados, não é possível anexar a entidade que você deseja alterar. Uma maneira de lidar com essa situação é chamar `AsNoTracking` na consulta anterior.

Para obter mais informações, consulte [controle vs. Nenhum controle](https://docs.microsoft.com/ef/core/querying/tracking).

## <a name="summary"></a>Resumo

Agora você tem um conjunto completo de páginas que executam operações CRUD simples para entidades do aluno. O seguinte tutorial você expandir a funcionalidade do **índice** página adicionando a classificação, filtragem e paginação.

>[!div class="step-by-step"]
[Anterior](intro.md)
[Próximo](sort-filter-page.md)  
