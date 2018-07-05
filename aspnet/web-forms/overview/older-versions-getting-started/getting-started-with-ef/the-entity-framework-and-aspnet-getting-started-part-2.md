---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Introdução ao banco de dados do Entity Framework 4.0 First e 4 do ASP.NET Web Forms - parte 2 | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 476f3e45608bf79a6d2665424eba09cbfccd78fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371097"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Introdução ao banco de dados do Entity Framework 4.0 First e 4 do Web Forms do ASP.NET – parte 2
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial na série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>O controle EntityDataSource

No tutorial anterior, você criou um site da web, um banco de dados e um modelo de dados. Neste tutorial, você trabalha com o `EntityDataSource` controle ASP.NET fornece para torná-lo mais fácil trabalhar com um modelo de dados do Entity Framework. Você criará um `GridView` controle para exibir e editar dados de alunos, uma `DetailsView` controle para a adição de novos alunos e um `DropDownList` controle para selecionar um departamento (o que você usará posteriormente para exibir o curso associado).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Observe que esse aplicativo você não ser adicionando validação de entrada para páginas que atualizam o banco de dados, e alguns o tratamento de erros não serão tão robustas quanto seria necessário em um aplicativo de produção. Isso mantém o tutorial voltado para o Entity Framework e impede que ela ficando muito longa. Para obter detalhes sobre como adicionar esses recursos ao seu aplicativo, consulte [Validating User Input in ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) e [tratamento de erros em páginas ASP.NET e aplicativos](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Adicionando e configurando o controle EntityDataSource

Você começará Configurando uma `EntityDataSource` controle para ler `Person` entidades a partir de `People` conjunto de entidades.

Verifique se você tiver o Visual Studio abra e que você está trabalhando com o projeto que você criou na parte 1. Se você ainda não criou o projeto desde que você criou o modelo de dados ou desde a última alteração feita nele, compile o projeto agora. As alterações no modelo de dados não ficam disponíveis para o designer até que o projeto é compilado.

Criar uma nova página da web usando o **formulário da Web usando página mestra** modelo e nomeie-o *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Especificar *Master* como a página mestra. Todas as páginas que você criar para esses tutoriais usará essa página mestra.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

Na **fonte** exibir, adicionar um `h2` título para o `Content` controle denominado `Content2`, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Dos **dados** guia da **caixa de ferramentas**, arraste uma `EntityDataSource` o controle para a página, solte-o abaixo do título e altere a ID para `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Alterne para **Design** exibir, clique em marca inteligente do controle de fonte de dados e, em seguida, clique em **configurar fonte de dados** para iniciar o **configurar fonte de dados** assistente.

[![Para Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

No **configurar ObjectContext** etapa do assistente, selecione **SchoolEntities** como o valor para **Conexão denominada**e selecione **SchoolEntities**como o **DefaultContainerName** valor. Clique em **Avançar**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Observação: Se você receber a seguinte caixa de diálogo neste ponto, você precisa compilar o projeto antes de continuar.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

No **configurar seleção de dados** etapa, selecione **pessoas** como o valor para **EntitySetName**. Sob **selecionar**, certifique-se a **selecionar um** ll caixa de seleção está selecionada. Em seguida, selecione as opções para habilitar a atualização e exclusão. Quando terminar, clique em **concluir**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configurando regras de banco de dados para permitir a exclusão

Você criará uma página que permite aos usuários a excluir alunos do `Person` tabela, que tem três relações com outras tabelas (`Course`, `StudentGrade`, e `OfficeAssignment`). Por padrão, o banco de dados impedirá você de exclusão de uma linha em `Person` se não houver linhas relacionadas em uma das outras tabelas. Você pode excluir manualmente as linhas relacionadas pela primeira vez, ou você pode configurar o banco de dados para excluí-los automaticamente quando você exclui um `Person` linha. Para registros de alunos neste tutorial, você vai configurar o banco de dados para excluir os dados relacionados automaticamente. Porque os alunos podem ter linhas relacionadas somente no `StudentGrade` tabela, você precisa configurar apenas um dos três relações.

Se você estiver usando o *School.mdf* arquivo que você baixou do projeto que acompanha este tutorial, você poderá ignorar esta seção porque essas alterações de configuração já tem sido feitas. Se você criou o banco de dados executando um script, configure o banco de dados, realizando os procedimentos a seguir.

Na **Gerenciador de servidores**, abra o diagrama de banco de dados que você criou na parte 1. A relação entre o botão direito do mouse `Person` e `StudentGrade` (a linha entre as tabelas) e, em seguida, selecione **propriedades**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

No **propriedades** janela, expanda **especificação INSERT e UPDATE** e defina o **DeleteRule** propriedade **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Salve e feche o diagrama. Se você for solicitado se deseja atualizar o banco de dados, clique em **Sim**.

Para certificar-se de que o modelo mantém entidades que estão na memória em sincronia com o que está fazendo o banco de dados, você deve definir as regras correspondentes no modelo de dados. Abra *Schoolmodel*, clique com botão direito na linha de associação entre `Person` e `StudentGrade`e, em seguida, selecione **propriedades**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

No **propriedades** janela, defina **End1 OnDelete** para **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Salve e feche o *Schoolmodel* de arquivo e, em seguida, recompile o projeto.

Em geral, quando o banco de dados for alterado, você tem várias opções para como sincronizar o modelo:

- Para determinados tipos de alterações (por exemplo, adicionar ou atualizar tabelas, exibições ou procedimentos armazenados), clique com botão direito no designer e selecione **modelo de atualização do banco de dados** tenha o designer, verifique as alterações automaticamente.
- Gerar novamente o modelo de dados.
- Fazer atualizações manuais como este.

Nesse caso, você poderia ter gerado novamente o modelo de ou atualizado as tabelas afetadas pela alteração de relação, mas, em seguida, você terá que fazer a alteração de nome de campo novamente (de `FirstName` para `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Usando um controle GridView para ler e atualizar entidades

Nesta seção, você usará um `GridView` controle para exibir, atualizar ou excluir os alunos.

Abra ou alterne para *Students.aspx* e alterne para o **Design** modo de exibição. Do **dados** guia da **caixa de ferramentas**, arraste uma `GridView` controle à direita do `EntityDataSource` controlar, nomeie- `StudentsGridView`, clique na marca inteligente e, em seguida, selecione  **StudentsEntityDataSource** como fonte de dados.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Clique em **Refresh Schema** (clique em **Sim** se você for solicitado a confirmar), em seguida, clique em **habilitar paginação**, **Habilitar classificação**, **Habilitar edição**, e **habilitar a exclusão de**.

Clique em **Editar colunas**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

No **campos selecionados** box, exclua **PersonID**, **LastName**, e **HireDate**. Você normalmente não exibe uma chave de registro para os usuários, data de contratação não é relevante para os alunos, e você colocará as duas partes do nome em um campo, portanto, você precisará apenas de um dos campos de nome.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Selecione o **FirstMidName** campo e, em seguida, clique em **converter este campo em um TemplateField**.

Faça o mesmo para **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Clique em **Okey** e, em seguida, alterne para o **origem** modo de exibição. As alterações restantes serão mais fácil de fazer diretamente na marcação. O `GridView` marcação agora parece semelhante ao seguinte exemplo de controle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

A primeira coluna depois que o campo de comando é um campo de modelo que atualmente exibe o nome do primeiro. Altere a marcação para esse campo de modelo para se parecer com o exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

No modo de exibição, dois `Label` controles exibem o nome e sobrenome. No modo de edição, duas caixas de texto são fornecidas para que você possa alterar o nome e sobrenome. Assim como acontece com o `Label` controles em modo de exibição, você usar `Bind` e `Eval` expressões exatamente como você faria com controles de fonte de dados do ASP.NET que se conectam diretamente aos bancos de dados. A única diferença é que você está especificando as propriedades da entidade em vez de colunas de banco de dados.

A última coluna é um campo de modelo que exibe a data de registro. Altere a marcação para esse campo para se parecer com o exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

Em ambos os exibem e modo de edição, a cadeia de caracteres de formato "{0, d}" faz com que a data a ser exibida no formato "data abreviada". (O computador pode ser configurado para exibir esse formato de forma diferente entre as imagens de tela mostradas neste tutorial.)

Observe que, em cada um desses campos do modelo, o designer usado uma `Bind` expressão por padrão, mas você tiver alterado para um `Eval` expressão no `ItemTemplate` elementos. O `Bind` expressão torna os dados disponíveis no `GridView` propriedades de controle, caso seja necessário acessar os dados no código. Nesta página, você não precisa acessar esses dados no código, portanto, você pode usar `Eval`, que é mais eficiente. Para obter mais informações, consulte [obtendo seus dados a controles de dados](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>A revisão de marcação de Controle EntityDataSource para melhorar o desempenho

Na marcação para o `EntityDataSource` controlar, remova o `ConnectionString` e `DefaultContainerName` atributos e substituí-los com um `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. Essa é uma alteração que você deve fazer sempre que você cria um `EntityDataSource` controlar, a menos que você precisa usar uma conexão diferente daquele que é embutido no código na classe de contexto de objeto. Usando o `ContextTypeName` atributo fornece os seguintes benefícios:

- Melhor desempenho. Quando o `EntityDataSource` controle inicializa o modelo de dados usando o `ConnectionString` e `DefaultContainerName` atributos, ele executa trabalho adicional para carregar os metadados em cada solicitação. Isso não é necessário se você especificar o `ContextTypeName` atributo.
- Carregamento lento é ativado por padrão nas classes de contexto de objeto gerado (como `SchoolEntities` neste tutorial) no Entity Framework 4.0. Isso significa que as propriedades de navegação são carregadas com os dados relacionados automaticamente à direita quando necessário. Carregamento lento é explicado em mais detalhes posteriormente neste tutorial.
- Todas as personalizações que você tiver aplicado à classe de contexto de objeto (nesse caso, o `SchoolEntities` classe) estarão disponíveis para controles que usam o `EntityDataSource` controle. Personalizar a classe de contexto de objeto é um tópico avançado que não é abordado nesta série de tutoriais. Para obter mais informações, consulte [estendendo o Entity Framework geradas tipos](https://msdn.microsoft.com/library/dd456844.aspx).

A marcação agora será semelhante ao exemplo a seguir (a ordem das propriedades pode ser diferente):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

O `EnableFlattening` atributo se refere a um recurso que foi necessário em versões anteriores do Entity Framework, porque as colunas de chave estrangeira não estavam expostas como propriedades de entidade. A versão atual, é possível usar *associações de chave estrangeiras*, que significa que as propriedades de chave estrangeiras são expostas para associações de tudo, mas muitos-para-muitos. Se as entidades têm propriedades de chave estrangeira e nenhum [tipos complexos](https://msdn.microsoft.com/library/bb738472.aspx), você pode deixar esse atributo definido como `False`. Não remova o atributo de marcação, como o valor padrão é `True`. Para obter mais informações, consulte [mesclar objetos (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Execute a página e você verá uma lista dos alunos e funcionários (filtrará os alunos apenas no próximo tutorial). O nome e sobrenome são exibidos juntos.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Para classificar a exibição, clique em um nome de coluna.

Clique em **editar** em qualquer linha. Caixas de texto são exibidas em que você pode alterar o nome e sobrenome.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

O **excluir** botão também funciona. Clique em Excluir para uma linha que tem uma data de registro e a linha desaparece. (Linhas sem uma data de registro representam instrutores e você pode receber um erro de integridade referencial. No próximo tutorial você filtrar essa lista para incluir os alunos apenas.)

## <a name="displaying-data-from-a-navigation-property"></a>Exibir dados de uma propriedade de navegação

Agora suponha que você queira saber quantos cursos cada aluno está registrado no. O Entity Framework fornece essas informações na `StudentGrades` propriedade de navegação do `Person` entidade. Como o design de banco de dados não permite um aluno que serão registrados em um curso sem a necessidade de uma nota atribuída, para este tutorial você pode presumir que ter uma linha no `StudentGrade` linha da tabela que está associada um curso é o mesmo que estão sendo registrados no curso. (O `Courses` propriedade de navegação é apenas para os instrutores.)

Quando você usa o `ContextTypeName` atributo do `EntityDataSource` controle, o Entity Framework automaticamente recupera as informações de uma propriedade de navegação quando você acessar essa propriedade. Isso é chamado *carregamento lento*. No entanto, isso pode ser ineficiente, porque ele resulta em uma chamada separada para o banco de dados que cada informações adicionais de tempo é necessário. Se precisar de dados da propriedade de navegação para cada entidade retornada pelo `EntityDataSource` controle, é mais eficiente para recuperar os dados relacionados, juntamente com a própria entidade em uma única chamada ao banco de dados. Isso é chamado *o carregamento adiantado*, e você especificar o carregamento adiantado para uma propriedade de navegação, definindo o `Include` propriedade do `EntityDataSource` controle.

Na *Students.aspx*, para mostrar a quantidade de cursos de cada aluno, o carregamento adiantado assim é a melhor opção. Se você estava exibindo todos os alunos, mas mostrando o número de cursos somente para alguns deles (que exigem a escrita de algum código além da marcação), o carregamento lento pode ser uma opção melhor.

Abra ou alterne para *Students.aspx*, alterne para o **Design** exibição, selecione `StudentsEntityDataSource`e, no **propriedades** janela conjunto o **Include**propriedade para **StudentGrades**. (Se você quisesse obter várias propriedades de navegação, você pode especificar seus nomes separados por vírgulas — por exemplo, **StudentGrades, cursos**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Alterne para **origem** modo de exibição. No `StudentsGridView` controle, após o último `asp:TemplateField` elemento, adicione o novo campo de modelo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

No `Eval` expressão, você pode fazer referência à propriedade de navegação `StudentGrades`. Como essa propriedade contém uma coleção, ele tem um `Count` propriedade que você pode usar para exibir o número de cursos em que o aluno está registrado. Um tutorial posterior, você verá como exibir dados de propriedades de navegação que contêm entidades únicas, em vez de coleções. (Observe que você não pode usar `BoundField` elementos para exibir dados de propriedades de navegação.)

Executar a página e você agora verá quantos cursos de cada aluno está registrado no.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Usando um controle DetailsView para inserir entidades

A próxima etapa é criar uma página que tem um `DetailsView` controle que permitirá que você adicione novos estudantes. Feche o navegador e, em seguida, crie uma nova página da web usando o *Master* página mestra. Nomeie a página *StudentsAdd.aspx*e, em seguida, alterne para o **origem** modo de exibição.

Adicione a seguinte marcação para substituir a marcação existente para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Essa marcação cria um `EntityDataSource` controle que é semelhante ao que você criou na *Students.aspx*, exceto que ele permite a inserção. Assim como acontece com o `GridView` controlar, os campos associados do `DetailsView` controle são codificados exatamente como seriam para um controle de dados que se conecta diretamente a um banco de dados, exceto que eles fazem referência a propriedades da entidade. Nesse caso, o `DetailsView` controle só é usado para inserir linhas, para que você definiu o modo padrão como `Insert`.

Execute a página e adicione um novo aluno.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Nada acontecerá depois de inserir um novo aluno, mas se você executar agora *Students.aspx*, você verá as novas informações de aluno.

## <a name="displaying-data-in-a-drop-down-list"></a>Exibindo dados em uma lista suspensa

Nas etapas a seguir, você vai databind uma `DropDownList` controle a uma entidade definida usando um `EntityDataSource` controle. Nesta parte do tutorial, você não fará muita coisa com esta lista. Em partes subsequentes, no entanto, você usará a lista para permitir que usuários selecionem um departamento para exibir os cursos associados com o departamento.

Criar uma nova página da web denominado *Courses.aspx*. Na **fonte** exibir, adicionar um título para o `Content` controle denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

Na **Design** exibir, adicionar um `EntityDataSource` controle para a página como fez anteriormente, exceto que desta vez Nomeie- `DepartmentsEntityDataSource`. Selecione **departamentos** como o **EntitySetName** valor e, em seguida, selecione apenas o **DepartmentID** e **nome** propriedades.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Dos **padrão** guia da **caixa de ferramentas**, arraste uma `DropDownList` o controle para a página, nomeie- `DepartmentsDropDownList`, clique na marca inteligente e selecione **Escolher fonte de dados** para Iniciar o **Assistente de configuração de fonte de dados**.

[![image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

No **escolher uma fonte de dados** etapa, selecione **DepartmentsEntityDataSource** como fonte de dados, clique em **Refresh Schema**e, em seguida, selecione **nome** como o campo de dados para exibir e **DepartmentID** como o campo de dados de valor. Clique em **OK**.

[![image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

O método usado para databind do controle usando o Entity Framework é o mesmo com outros dados do ASP.NET controles de fonte, exceto que você está especificando entidades e propriedades da entidade.

Alterne para **fonte** exibir e adicionar "Selecione um departamento:" imediatamente antes do `DropDownList` controle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Como lembrete, altere a marcação para o `EntityDataSource` controle neste ponto, substituindo o `ConnectionString` e `DefaultContainerName` atributos com um `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. Geralmente é melhor esperar até depois de criar o controle associado a dados que esteja vinculado ao controle de fonte de dados antes de alterar o `EntityDataSource` controlar marcação, porque depois de fazer a alteração, o designer não fornecerá a você com um **atualizar Esquema** opção no controle associado a dados.

Execute a página e você pode selecionar um departamento na lista suspensa.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Isso conclui a introdução ao uso de `EntityDataSource` controle. Trabalhar com esse controle geralmente não é diferente de trabalhar com outros dados do ASP.NET controles de fonte, exceto pelo fato de você fazer referência a entidades e propriedades em vez de tabelas e colunas. A única exceção é quando você deseja acessar propriedades de navegação. No próximo tutorial você verá que a sintaxe que você use com `EntityDataSource` controle também pode ser diferentes dos outros controles de fonte de dados quando você filtra, agrupar e dados de pedidos.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-3.md)
