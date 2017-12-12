---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: "Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 Web Forms - parte 2 | Microsoft Docs"
author: tdykstra
description: "O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 4e2a3176aaedccd40ef6b619efa3c4052dd8470b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Introdução ao banco de dados do Entity Framework 4.0 primeiro e 4 Web Forms do ASP.NET - parte 2
====================
Por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>O controle de EntityDataSource

No tutorial anterior, você criou um site da web, um banco de dados e um modelo de dados. Neste tutorial, você trabalha com o `EntityDataSource` controle ASP.NET fornece para facilitar a trabalhar com um modelo de dados do Entity Framework. Você criará uma `GridView` controle para exibir e editar dados aluno, um `DetailsView` controle para a adição de novos alunos e um `DropDownList` controle para selecionar um departamento (que você usará mais tarde para exibir os cursos associados).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Observe que neste aplicativo você não adicionar o validação de entrada para páginas que atualizam o banco de dados, e alguns o tratamento de erros não serão tão robustas quanto seria necessário em um aplicativo de produção. Que mantém o tutorial se concentra no Entity Framework e os mantém obtenham muito longo. Para obter detalhes sobre como adicionar esses recursos ao seu aplicativo, consulte [Validando a entrada do usuário em páginas da Web do ASP.NET](https://msdn.microsoft.com/en-us/library/7kh55542.aspx) e [tratamento de erros em páginas ASP.NET e aplicativos](https://msdn.microsoft.com/en-us/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Adicionando e configurando o controle de EntityDataSource

Você começará, configurando um `EntityDataSource` controle ler `Person` entidades a `People` conjunto de entidades.

Verifique se você tiver o Visual Studio aberta e que você está trabalhando com o projeto que você criou na parte 1. Se você ainda não criou o projeto desde que você criou o modelo de dados ou desde a última alteração feita nele, compile o projeto agora. As alterações no modelo de dados não são disponibilizadas para o designer até que o projeto é compilado.

Criar uma nova página da web usando o **formulário da Web usando página mestra** modelo e nomeie-o *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Especifique *Site.Master* como a página mestra. Todas as páginas que você criar para esses tutoriais usará essa página mestra.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

Em **fonte** exibir, adicionar um `h2` título para o `Content` controle chamado `Content2`, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Do **dados** guia do **caixa de ferramentas**, arraste um `EntityDataSource` controlar para a página, abaixo do título e alterar a ID para `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Alternar para **Design** exibir, clique em marca inteligente do controle de fonte de dados e, em seguida, clique em **configurar fonte de dados** para iniciar o **configurar fonte de dados** assistente.

[![Para Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

No **configurar ObjectContext** etapa do assistente, selecione **SchoolEntities** como o valor para **Conexão denominada**e selecione **SchoolEntities**como o **DefaultContainerName** valor. Clique em **Avançar**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Observação: Se você receber a seguinte caixa de diálogo neste ponto, você precisa compilar o projeto antes de continuar.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

No **configurar seleção de dados** etapa, selecione **pessoas** como o valor para **EntitySetName**. Em **selecione**, verifique se o **selecionar um** ll caixa é selecionada. Em seguida, selecione as opções para habilitar a atualização e exclusão. Quando terminar, clique em **concluir**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configurando regras de banco de dados para permitir a exclusão

Você criará uma página que permite aos usuários excluir alunos do `Person` tabela, que tem três relações com outras tabelas (`Course`, `StudentGrade`, e `OfficeAssignment`). Por padrão, o banco de dados impedirão a exclusão de uma linha em `Person` se não houver linhas relacionadas em uma das outras tabelas. Você pode excluir manualmente as linhas relacionadas pela primeira vez, ou você pode configurar o banco de dados para excluí-los automaticamente quando você exclui um `Person` linha. Para registros de aluno neste tutorial, você configurará o banco de dados para excluir os dados relacionados automaticamente. Como os alunos podem ter linhas relacionadas somente no `StudentGrade` tabela, você precisa configurar apenas um dos três relações.

Se você estiver usando o *School.mdf* arquivo que você baixou do projeto que acompanha este tutorial, você poderá ignorar esta seção porque essas alterações de configuração já tenham sido feitas. Se você criou o banco de dados executando um script, configure o banco de dados, executando os procedimentos a seguir.

Em **Server Explorer**, abra o diagrama de banco de dados que você criou na parte 1. Clique com botão direito a relação entre `Person` e `StudentGrade` (a linha entre as tabelas) e, em seguida, selecione **propriedades**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

No **propriedades** janela, expanda **especificação INSERT e UPDATE** e defina o **DeleteRule** propriedade **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Salve e feche o diagrama. Se você for questionado se deseja atualizar o banco de dados, clique em **Sim**.

Para certificar-se de que o modelo mantém entidades que estão na memória em sincronia com o que está fazendo o banco de dados, você deve definir as regras correspondentes no modelo de dados. Abra *SchoolModel.edmx*, clique na linha de associação entre `Person` e `StudentGrade`e, em seguida, selecione **propriedades**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

No **propriedades** janela, defina **End1 OnDelete** para **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Salve e feche o *SchoolModel.edmx* de arquivo e, em seguida, recompile o projeto.

Em geral, quando o banco de dados é alterado, você tem várias opções para como o modelo de sincronização:

- Para determinados tipos de alterações (como adicionar ou atualizar tabelas, exibições ou procedimentos armazenados), clique com botão direito no designer e selecione **modelo de atualização de banco de dados** para que o designer Verifique as alterações automaticamente.
- Gerar novamente o modelo de dados.
- Faça atualizações manuais como esta.

Nesse caso, pode ter geradas novamente o modelo ou atualizado as tabelas afetadas pela alteração de relação, mas, em seguida, você precisa fazer a alteração do nome do campo novamente (de `FirstName` para `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Usando um controle GridView para ler e atualizar entidades

Nesta seção, você usará um `GridView` controle para exibir, atualizar ou excluir os alunos.

Abra ou alterne para *Students.aspx* e alterne para o **Design** exibição. Do **dados** guia do **caixa de ferramentas**, arraste um `GridView` controle à direita do `EntityDataSource` controlar, nomeie-o `StudentsGridView`, clique na marca inteligente e, em seguida, selecione  **StudentsEntityDataSource** como a fonte de dados.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Clique em **atualizar esquema** (clique **Sim** se você for solicitado a confirmar), em seguida, clique em **habilitar paginação**, **Habilitar classificação**, **Habilitar edição**, e **habilitar a exclusão de**.

Clique em **Editar colunas**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

No **campos selecionados** caixa, exclua **PersonID**, **LastName**, e **HireDate**. Você normalmente não exibe uma chave de registro para os usuários, data de contratação não é relevante para estudantes, e você colocará ambas as partes do nome em um campo, para que você só precisa de um dos campos de nome.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Selecione o **FirstMidName** campo e, em seguida, clique em **converter este campo em um TemplateField**.

Faça o mesmo para **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Clique em **Okey** e, em seguida, alterne para o **fonte** exibição. O restante das alterações será mais fácil fazer diretamente na marcação. O `GridView` controlar marcação agora parece com o exemplo a seguir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

A primeira coluna depois que o campo de comando é um campo de modelo que está sendo exibe o nome. Altere a marcação para este campo de modelo para se parecer com o exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

No modo de exibição, dois `Label` controles exibem o nome e sobrenome. No modo de edição, duas caixas de texto são fornecidas para que você pode alterar o nome e sobrenome. Assim como acontece com o `Label` controles em modo de exibição, você use `Bind` e `Eval` expressões exatamente como você faria com controles de fonte de dados do ASP.NET que se conectam diretamente aos bancos de dados. A única diferença é que você está especificando propriedades da entidade em vez de colunas de banco de dados.

A última coluna é um campo de modelo que exibe a data de inscrição. Altere a marcação para esse campo para se parecer com o exemplo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

Em ambos são exibidos e modo de edição, a cadeia de caracteres de formato "{0, d}" faz com que a data a ser exibida no formato "abreviada data". (Seu computador pode ser configurado para exibir esse formato de forma diferente de imagens da tela mostrado neste tutorial.)

Observe que, em cada um desses campos do modelo, o designer usado um `Bind` expressão por padrão, mas você tiver alterado para um `Eval` expressão no `ItemTemplate` elementos. O `Bind` expressão disponibiliza os dados no `GridView` propriedades de controle, no caso de você precisar acessar os dados no código. Nesta página, você não precisa acessar esses dados no código, para que você possa usar `Eval`, que é mais eficiente. Para obter mais informações, consulte [obtendo seus dados fora os controles de dados](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Revisar a marcação de controle de EntityDataSource para melhorar o desempenho

Na marcação para o `EntityDataSource` controlar, remova o `ConnectionString` e `DefaultContainerName` atributos e substituí-los por um `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. Essa é uma alteração que você deve fazer toda vez que você criar um `EntityDataSource` de controle, a menos que você precisa usar uma conexão que é diferente do que é codificado na classe de contexto de objeto. Usando o `ContextTypeName` atributo fornece os seguintes benefícios:

- Melhor desempenho. Quando o `EntityDataSource` controle inicializa o modelo de dados usando o `ConnectionString` e `DefaultContainerName` atributos, ele executa trabalho adicional para carregar os metadados em cada solicitação. Isso não é necessário se você especificar o `ContextTypeName` atributo.
- Carregamento lento é ativado por padrão em classes de contexto de objeto gerado (como `SchoolEntities` neste tutorial) no Entity Framework 4.0. Isso significa que as propriedades de navegação são carregadas com dados relacionados automaticamente à direita quando precisar dele. Carregamento preguiçoso é explicado em mais detalhes posteriormente neste tutorial.
- Todas as personalizações que você aplicou a classe de contexto de objeto (nesse caso, o `SchoolEntities` classe) estarão disponíveis para controles que usem o `EntityDataSource` controle. Personalizando a classe de contexto de objeto é um tópico avançado que não é abordado nesta série tutorial. Para obter mais informações, consulte [estendendo Entity Framework gerado tipos](https://msdn.microsoft.com/en-us/library/dd456844.aspx).

A marcação agora será parecida com o exemplo a seguir (a ordem das propriedades pode ser diferente):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

O `EnableFlattening` atributo refere-se a um recurso que foi necessário em versões anteriores do Entity Framework, porque as colunas de chave estrangeira não foram expostas como propriedades de entidade. A versão atual, é possível usar *associações de chave estrangeiras*, que significa que as propriedades de chave estrangeiras são expostos para associações de tudo, exceto muitos-para-muitos. Se as entidades têm propriedades de chave estrangeira e não [tipos complexos](https://msdn.microsoft.com/en-us/library/bb738472.aspx), você pode deixar esse atributo definido como `False`. Não remova o atributo da marcação, como o valor padrão é `True`. Para obter mais informações, consulte [objetos nivelamento (EntityDataSource)](https://msdn.microsoft.com/en-us/library/ee404746.aspx).

Execute a página e ver uma lista de alunos e os funcionários (filtrará os alunos apenas no tutorial próximo). O nome e sobrenome são exibidos juntos.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Para classificar a exibição, clique em um nome de coluna.

Clique em **editar** em qualquer linha. São exibidas caixas de texto onde você pode alterar o nome e sobrenome.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

O **excluir** botão também funciona. Clique em Excluir de uma linha que tem uma data de registro e a linha desaparece. (Linhas sem uma data de registro representam professores e você pode receber um erro de integridade referencial. O seguinte tutorial você filtrará essa lista para incluir apenas alunos.)

## <a name="displaying-data-from-a-navigation-property"></a>Exibir dados de uma propriedade de navegação

Agora suponha que você queira saber quantos cursos cada aluno está registrado no. O Entity Framework oferece essas informações no `StudentGrades` propriedade de navegação a `Person` entidade. Como o design de banco de dados não permitem um aluno sejam registrados em um curso sem ter uma classificação atribuída, para este tutorial pode assumir que ter uma linha no `StudentGrade` linha da tabela que está associada um curso é o mesmo que está sendo registrado no curso. (O `Courses` propriedade de navegação é somente para instrutores.)

Quando você usa o `ContextTypeName` atributo o `EntityDataSource` controle, o Entity Framework automaticamente recupera as informações de uma propriedade de navegação quando você acessar essa propriedade. Isso é chamado de *carregamento preguiçoso*. No entanto, isso pode ser ineficiente, porque resulta em uma chamada separada para o banco de dados que cada informação de hora adicional é necessária. Se precisar de dados da propriedade de navegação para cada entidade retornada pelo `EntityDataSource` controle, é mais eficiente para recuperar os dados relacionados com a entidade em uma única chamada para o banco de dados. Isso é chamado *carregamento adiantado*, e você especificar o carregamento rápido para uma propriedade de navegação, definindo o `Include` propriedade o `EntityDataSource` controle.

Em *Students.aspx*, para mostrar o número de cursos para cada aluno, carregamento tão rápido é a melhor opção. Se você fosse exibindo todos os alunos mas mostrando o número de cursos apenas para alguns deles (o que exigiria a escrita de algum código além de marcação), carregamento lento pode ser uma opção melhor.

Abra ou alterne para *Students.aspx*, alterne para **Design** exibição, selecione `StudentsEntityDataSource`e no **propriedades** janela conjunto o **incluir**propriedade **StudentGrades**. (Se você quiser obter várias propriedades de navegação, você pode especificar seus nomes separados por vírgulas, por exemplo, **StudentGrades, cursos**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Alternar para **fonte** exibição. No `StudentsGridView` controle, após a última `asp:TemplateField` elemento, adicione o novo campo de modelo a seguir:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

No `Eval` expressão, você pode fazer referência a propriedade de navegação `StudentGrades`. Como essa propriedade contém uma coleção, ela tem um `Count` propriedade que você pode usar para exibir o número de cursos em que o aluno está registrado. Um tutorial posterior, você verá como exibir dados de propriedades de navegação que contêm entidades únicas, em vez de coleções. (Observe que você não pode usar `BoundField` elementos para exibir dados de propriedades de navegação.)

Execute a página e agora você ver quantas cursos cada aluno está registrado no.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Usando um controle DetailsView para inserir entidades

A próxima etapa é criar uma página que tem um `DetailsView` controle que permitirá que você adicione novos alunos. Feche o navegador e, em seguida, crie uma nova página da web usando o *Site.Master* página mestra. Nomeie a página *StudentsAdd.aspx*e, em seguida, alterne para o **fonte** exibição.

Adicione a seguinte marcação para substituir a marcação existente para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Essa marcação cria um `EntityDataSource` controle que é semelhante ao que você criou na *Students.aspx*, exceto que ele permite a inserção. Assim como acontece com o `GridView` controle, os campos associados do `DetailsView` controle são codificados exatamente como eles seriam para um controle de dados que se conecta diretamente a um banco de dados, exceto que eles fazem referência a propriedades da entidade. Nesse caso, o `DetailsView` controle só é usado para inserir linhas, para que você definiu o modo padrão para `Insert`.

Execute a página e adicione um novo aluno.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Nada acontece depois que você inserir um novo aluno, mas se você executar *Students.aspx*, você verá as novas informações de student.

## <a name="displaying-data-in-a-drop-down-list"></a>Exibindo dados em uma lista suspensa

Nas etapas a seguir, você vai databind um `DropDownList` controle a uma entidade definida usando um `EntityDataSource` controle. Nesta parte do tutorial, você não faz muito essa lista. Partes subsequentes, no entanto, você usará a lista para permitir aos usuários selecionar um departamento para exibir os cursos associados com o departamento.

Criar uma nova página da web denominado *Courses.aspx*. Em **fonte** exibir, adicionar um título para o `Content` controle chamado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

Em **Design** exibir, adicionar um `EntityDataSource` controle para a página novamente, exceto que desta vez Nomeie- `DepartmentsEntityDataSource`. Selecione **departamentos** como o **EntitySetName** valor e, em seguida, selecione apenas o **DepartmentID** e **nome** propriedades.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Do **padrão** guia do **caixa de ferramentas**, arraste um `DropDownList` controlar para a página, nomeie-o `DepartmentsDropDownList`, clique na marca inteligente e selecione **Escolher fonte de dados** para Iniciar o **Assistente de configuração de fonte de dados**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

No **escolher uma fonte de dados** etapa, selecione **DepartmentsEntityDataSource** como fonte de dados, clique em **atualizar esquema**e, em seguida, selecione **nome** como o campo de dados para exibir e **DepartmentID** como o campo de dados de valor. Clique em **OK**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

O método usado para associação de dados de controle usando o Entity Framework é o mesmo com outros dados ASP.NET controles de fonte, exceto que você está especificando entidades e propriedades da entidade.

Alternar para **fonte** exibir e adicionar "Selecione um departamento:" imediatamente antes do `DropDownList` controle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Como um lembrete, altere a marcação para o `EntityDataSource` controle neste momento, substituindo o `ConnectionString` e `DefaultContainerName` atributos com um `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. Geralmente é melhor esperar até que você criou o controle associado a dados que está vinculado ao controle de fonte de dados antes de alterar o `EntityDataSource` controlar marcação, porque depois de fazer a alteração, o designer não fornecerá um **atualizar Esquema** opção no controle associado a dados.

Execute a página e você pode selecionar um departamento da lista suspensa.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Isso conclui a introdução ao uso de `EntityDataSource` controle. Trabalhar com esse controle geralmente não é diferente de trabalhar com outros dados ASP.NET controles de fonte, exceto pelo fato de você fazer referência a entidades e propriedades em vez de tabelas e colunas. A única exceção é quando você deseja acessar propriedades de navegação. O seguinte tutorial você verá que a sintaxe que você use com `EntityDataSource` controle também pode ser diferentes dos outros controles de fonte de dados quando você filtra, agrupa e dados de pedidos.

>[!div class="step-by-step"]
[Anterior](the-entity-framework-and-aspnet-getting-started-part-1.md)
[Próximo](the-entity-framework-and-aspnet-getting-started-part-3.md)
