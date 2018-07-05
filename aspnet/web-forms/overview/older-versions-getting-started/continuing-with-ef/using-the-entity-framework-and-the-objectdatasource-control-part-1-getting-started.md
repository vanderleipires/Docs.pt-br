---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 1: Introdução | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo web Contoso University que é criado pelo Getting Started with a série de tutoriais do Entity Framework. Se yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: ba3b65dededeca3534b9273bfd3ae48429711fce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369741"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 1: Introdução
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais baseia-se no aplicativo web Contoso University que é criado pela [Introdução ao Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você teria criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa.
> 
> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. O aplicativo de exemplo é um site de uma Contoso University fictícia. Ele inclui funcionalidades como admissão de alunos, criação de cursos e atribuições de instrutor.
> 
> O tutorial mostra exemplos em c#. O [amostra para download](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contém código em c# e Visual Basic.
> 
> ## <a name="database-first"></a>Primeiro banco de dados
> 
> Há três maneiras que você pode trabalhar com dados no Entity Framework: *Database First*, *Model First*, e *Code First*. Este tutorial é para o banco de dados primeiro. Para obter informações sobre as diferenças entre esses fluxos de trabalho e diretrizes sobre como escolher o melhor para seu cenário, consulte [fluxos de trabalho de desenvolvimento do Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Como a série de Introdução, esta série de tutoriais usa o modelo de Web Forms do ASP.NET e pressupõe que você sabe como trabalhar com Web Forms do ASP.NET no Visual Studio. Se você não fizer isso, consulte [Introdução ao Web Forms do ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se você preferir trabalhar com o ASP.NET MVC framework, consulte [guia de Introdução com o Entity Framework usando o ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versões de software
> 
> | **Mostrado no tutorial** | **Também funciona com** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | O Visual Studio 2010 Express para Web. O tutorial não foi testado com versões posteriores do Visual Studio. Há muitas diferenças nas seleções de menu, caixas de diálogo e modelos. |
> | .NET 4 | .NET 4.5 é compatível com versões anteriores com o .NET 4, mas o tutorial não foi testado com o .NET 4.5. |
> | Entity Framework 4 | O tutorial não foi testado com versões posteriores do Entity Framework. Começando com o Entity Framework 5, o EF usa por padrão o `DbContext API` que foi introduzido com o EF 4.1. O controle EntityDataSource foi projetado para usar o `ObjectContext` API. Para obter informações sobre como usar o EntityDataSource controlar com o `DbContext` API, consulte [nesta postagem de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Perguntas
> 
> Se você tiver perguntas que não estão diretamente relacionadas para o tutorial, você pode postá-los para o [Fórum do Entity Framework do ASP.NET](https://forums.asp.net/1227.aspx), o [Entity Framework e LINQ para o fórum de entidades](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), ou [ StackOverflow.com](http://stackoverflow.com/).


O `EntityDataSource` controle permite que você criar um aplicativo muito rapidamente, mas normalmente requer que você mantenha uma quantidade significativa de lógica de negócios e lógica de acesso a dados em seu *. aspx* páginas. Se você espera que seu aplicativo aumente de complexidade e requer manutenção contínua, você pode investir mais tempo de desenvolvimento com antecedência para criar uma *fileiras* ou *em camadas* estrutura de aplicativo Isso é mais sustentável. Para implementar essa arquitetura, você pode separar a camada de apresentação da camada de lógica de negócios (BLL) e a camada de acesso a dados (DAL). Uma maneira de implementar essa estrutura é usar o `ObjectDataSource` controlar em vez do `EntityDataSource` controle. Quando você usa o `ObjectDataSource` controle, implementar seu próprio código de acesso a dados e, em seguida, invoque-o na *. aspx* páginas usando um controle que tem muitos dos mesmos recursos que os outros controles de fonte de dados. Isso permite que você combine as vantagens de uma abordagem de n camadas com os benefícios de usar um controle de formulários da Web para acesso a dados.

O `ObjectDataSource` controle oferece mais flexibilidade de outras formas também. Porque você escrever seu próprio código de acesso a dados, é mais fácil mais do que apenas ler, inserir, atualizar ou excluir um tipo de entidade específica, que são as tarefas que o `EntityDataSource` controle foi projetado para executar. Por exemplo, você pode realizar o registro em log sempre que uma entidade é atualizada, arquivar dados sempre que uma entidade é excluída, ou automaticamente verificação e atualização de dados relacionados conforme necessário ao inserir uma linha com um valor de chave estrangeira.

## <a name="business-logic-and-repository-classes"></a>Lógica de negócios e Classes de repositório

Um `ObjectDataSource` funciona com a invocação de uma classe que você cria o controle. A classe inclui métodos que recuperam e atualizam dados, além de fornecer os nomes desses métodos para o `ObjectDataSource` controle na marcação. Durante o processamento ou processamento de postagem, o `ObjectDataSource` chama os métodos que você especificou.

Além das operações CRUD básicas, a classe que você cria para usar com o `ObjectDataSource` controle talvez seja necessário executar a lógica de negócios quando a `ObjectDataSource` lê ou atualiza dados. Por exemplo, quando você atualiza um departamento, você precisa validar que não há outros departamentos tenham o mesmo administrador porque uma pessoa não pode ser um administrador de mais de um departamento.

Em alguns `ObjectDataSource` documentação, como o [visão geral da classe ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), o controle chama uma classe conhecida como um *objeto comercial* que inclui a lógica de negócios e lógica de acesso a dados . Neste tutorial, você criará as classes separadas para lógica de negócios e lógica de acesso a dados. A classe que encapsula a lógica de acesso a dados é chamada uma *repositório*. A classe de lógica de negócios inclui métodos de lógica de negócios e os métodos de acesso a dados, mas os métodos de acesso a dados chamam o repositório para executar tarefas de acesso a dados.

Você também criará uma camada de abstração entre a sua BLL e DAL que facilita a unidade automatizados Testando a BLL. Essa camada de abstração é implementada, criando uma interface e usando a interface ao instanciar o repositório na classe de lógica de negócios. Isso torna possível para fornecer a classe de lógica de negócios com uma referência a qualquer objeto que implementa a interface do repositório. Para uma operação normal, você deve fornecer um objeto de repositório que funciona com o Entity Framework. Para testar, você deve fornecer um objeto de repositório que funciona com dados armazenados em uma forma que você pode manipular com facilidade, como variáveis de classe definidas como coleções.

A ilustração a seguir mostra a diferença entre uma classe de lógica de negócios que inclui a lógica de acesso a dados sem um repositório e outro que usa um repositório.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Você começará criando páginas da web no qual o `ObjectDataSource` controle está vinculado diretamente a um repositório porque ele só executa tarefas básicas de acesso a dados. No próximo tutorial você criará uma classe de lógica de negócios com a lógica de validação e associar o `ObjectDataSource` controle para essa classe em vez de à classe repository. Você também criará testes de unidade para a lógica de validação. O terceiro tutorial desta série, você adicionará a classificação e filtragem de funcionalidade para o aplicativo.

As páginas que você cria neste tutorial funcionam com o `Departments` conjunto de entidades do modelo de dados que você criou na [série de tutoriais de Introdução](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Para Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Atualizando o banco de dados e o modelo de dados

Você começará neste tutorial, tornando duas alterações no banco de dados, ambas exigem que as alterações correspondentes ao modelo de dados que você criou na [Introdução ao Entity Framework e Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) tutoriais. Em um desses tutoriais, você fez alterações no designer de manualmente para sincronizar o modelo de dados com o banco de dados após uma alteração de banco de dados. Neste tutorial, você usará o designer **atualização do banco de dados modelo** ferramenta para atualizar o modelo de dados automaticamente.

### <a name="adding-a-relationship-to-the-database"></a>Adicionando uma relação para o banco de dados

No Visual Studio, abra o aplicativo de web Contoso University criado na [Introdução ao Entity Framework e Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série de tutoriais e, em seguida, abra o `SchoolDiagram` diagrama de banco de dados.

Se você examinar a `Department` tabela no diagrama de banco de dados, você verá que ele tem um `Administrator` coluna. Essa coluna é uma chave estrangeira para a `Person` tabela, mas nenhuma relação de chave estrangeira é definida no banco de dados. Você precisa criar a relação e atualize o modelo de dados, de forma que o Entity Framework pode tratar automaticamente essa relação.

No diagrama de banco de dados, clique com botão direito do `Department` da tabela e selecione **relações**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

No **relações de chave estrangeira** caixa de clicar **Add**, em seguida, clique no botão de reticências para **especificação de tabelas e colunas**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

No **tabelas e colunas** diálogo caixa, defina a tabela de chave primária e um campo para `Person` e `PersonID`e definir a tabela de chave estrangeira e um campo para `Department` e `Administrator`. (Quando você fizer isso, o nome da relação será alterado de `FK_Department_Department` para `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Clique em **Okey** na **tabelas e colunas** , clique em **fechar** no **relações de chave estrangeira** caixa e salve as alterações. Se você for solicitado se você deseja salvar o `Person` e `Department` tabelas, clique em **Sim**.

> [!NOTE]
> Se você tiver excluído `Person` linhas que correspondem aos dados que já está no `Administrator` coluna, você não poderá salvar essa alteração. Nesse caso, use o editor de tabela no **Gerenciador de servidores** para certificar-se de que o `Administrator` valor em cada `Department` linha contém a ID de um registro que realmente existe no `Person` tabela.
> 
> Depois de salvar a alteração, você não poderá excluir uma linha do `Person` tabela se essa pessoa é um administrador de departamento. Em um aplicativo de produção, você forneceria uma mensagem de erro específica quando uma restrição de banco de dados impede a exclusão ou, você especificaria uma exclusão em cascata. Para obter um exemplo de como especificar uma exclusão em cascata, consulte [de Entity Framework e do ASP.NET – Introdução à parte 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Adicionando uma exibição no banco de dados

Na nova *Departments.aspx* página que você criará, que você deseja fornecer uma lista suspensa de instrutores, com nomes no formato "Sobrenome, nome" para que os usuários podem selecionar os administradores de departamento. Para tornar mais fácil de fazer isso, você criará um modo de exibição no banco de dados. O modo de exibição consistirá apenas os dados necessários para a lista suspensa: o nome completo (formatado corretamente) e a chave de registro.

Na **Gerenciador de servidores**, expanda *School.mdf*, clique com botão direito a **modos de exibição** pasta e selecione **adicionar nova exibição**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Clique em **feche** quando o **adicionar tabela** caixa de diálogo é exibida e cole a seguinte instrução SQL no painel SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Salvar o modo de exibição como `vInstructorName`.

### <a name="updating-the-data-model"></a>Atualizando o modelo de dados

No *DAL* pasta, abra o *Schoolmodel* do arquivo, clique com botão direito na superfície de design e, em seguida, selecione **modelo de atualização do banco de dados**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

No **Choose Your Database Objects** caixa de diálogo, selecione o **Add** guia e selecione o modo de exibição que você acabou de criar.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Clique em **Finalizar**.

No designer, você verá que a ferramenta está sendo criado um `vInstructorName` entidade e uma nova associação entre o `Department` e `Person` entidades.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> No **saída** e **lista de erros** tecla do windows, você poderá ver uma mensagem de aviso informando que a ferramenta criar automaticamente um primário para o novo `vInstructorName` modo de exibição. Esse comportamento é esperado.


Quando você se referir ao novo `vInstructorName` entidade no código, você não quiser usar a convenção de banco de dados de prefixar um "v" em letras minúsculas para ele. Portanto, você renomeará a entidade e a entidade definida no modelo.

Abra o **navegador de modelos**. Você verá `vInstructorName` listado como um tipo de entidade e um modo de exibição.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Sob **SchoolModel** (não **SchoolModel.Store**), clique com botão direito **vInstructorName** e selecione **propriedades**. No **propriedades** janela, altere o **nome** propriedade como "InstructorName" e altere a **definir nome de entidade** propriedade como "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Salve e feche o modelo de dados e, em seguida, recompile o projeto.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Usando uma classe de repositório e um controle ObjectDataSource

Criar um novo arquivo de classe na *DAL* pasta, nomeie- *SchoolRepository.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Esse código fornece uma única `GetDepartments` método que retorna todas as entidades no `Departments` conjunto de entidades. Porque você sabe o que você terá acesso a `Person` propriedade de navegação para todas as linhas retornadas, você especificar o carregamento adiantada para essa propriedade usando o `Include` método. A classe também implementa o `IDisposable` interface para garantir que a conexão de banco de dados é liberado quando o objeto é descartado.

> [!NOTE]
> Uma prática comum é criar uma classe de repositório para cada tipo de entidade. Neste tutorial, uma classe de repositório para vários tipos de entidade é usada. Para obter mais informações sobre o padrão de repositório, consulte as postagens no [blog da equipe do Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) e [blog de Julie](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


O `GetDepartments` método retorna um `IEnumerable` objeto em vez de um `IQueryable` objeto para garantir que a coleção retornada é utilizável até mesmo depois que o próprio objeto de repositório é descartado. Um `IQueryable` objeto pode fazer com que o acesso de banco de dados sempre que ele é acessado, mas o objeto de repositório pode ser descartado no momento em um controle de vinculação de dados tenta renderizar os dados. Você pode retornar outro tipo de coleção, como um `IList` do objeto, em vez de um `IEnumerable` objeto. No entanto, retornando um `IEnumerable` objeto garante que você pode realizar tarefas de processamento de lista típica de somente leitura, como `foreach` loops e consultas LINQ, mas você não pode adicionar a ou remover itens na coleção, que poderá indicar que essas alterações seria persistentes no banco de dados.

Criar uma *Departments.aspx* página que usa o *Master* página mestra e adicione a seguinte marcação no `Content` controle chamado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Essa marcação cria um `ObjectDataSource` controle que usa a classe de repositório que você acabou de criar, e um `GridView` controle para exibir os dados. O `GridView` controle especifica **edite** e **excluir** comandos, mas você ainda não adicionou o código para dar suporte a eles ainda.

Usarem várias colunas `DynamicField` controles de modo que você pode tirar proveito da funcionalidade de formatação e validação de dados automática. Para que isso funcione, você precisa chamar o `EnableDynamicData` método no `Page_Init` manipulador de eventos. (`DynamicControl` controles não são usados no `Administrator` campo porque eles não funcionam com as propriedades de navegação.)

O `Vertical-Align="Top"` atributos será importantes posteriormente quando você adiciona uma coluna que tenha um aninhada `GridView` controle à grade.

Abra o *Departments.aspx.cs* do arquivo e adicione o seguinte `using` instrução:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Em seguida, adicione o seguinte manipulador para a página `Init` eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

No *DAL* pasta, crie um novo arquivo de classe chamado *Department.cs* e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Esse código adiciona metadados ao modelo de dados. Especifica que o `Budget` propriedade do `Department` entidade representa moeda, na verdade, embora seu tipo de dados seja `Decimal`, e especifica que o valor deve ser entre 0 e US $1,000,000.00. Ela também especifica que o `StartDate` propriedade deve ser formatada como uma data no formato mm/dd/aaaa.

Execute o *Departments.aspx* página.

[![Para Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Observe que, embora você não especificar uma cadeia de caracteres de formato na *Departments.aspx* marcação da página para o **orçamento** ou **data de início** padrão de colunas, moeda e data a formatação foi aplicada a eles pela `DynamicField` controla, usando os metadados que você forneceu na *Department.cs* arquivo.

## <a name="adding-insert-and-delete-functionality"></a>Adição de inserção e a funcionalidade de exclusão

Abra *SchoolRepository.cs*, adicione o seguinte código para criar um `Insert` método e um `Delete` método. O código também inclui um método chamado `GenerateDepartmentID` que calcula o próximo valor de chave do Registro disponíveis para uso pelo `Insert` método. Isso é necessário porque o banco de dados não está configurado para calcular isso automaticamente para o `Department` tabela.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>O método Attach

O `DeleteDepartment` chamadas de método a `Attach` método para restabelecer o link que é mantido no Gerenciador de estado do objeto do contexto de objeto entre as entidades na memória e o banco de dados de linha ele representa. Isso deve ocorrer antes do método chama o `SaveChanges` método.

O termo *contexto de objeto* refere-se à classe Entity Framework que deriva de `ObjectContext` classe que você usa para acessar seus conjuntos de entidades e entidades. O código para este projeto, a classe é chamada `SchoolEntities`, e uma instância dele é sempre denominada `context`. O contexto de objeto *Gerenciador de estado do objeto* é uma classe que deriva de `ObjectStateManager` classe. O contato de objeto usa o Gerenciador de estado do objeto para armazenar objetos de entidade e para controlar se cada um deles está em sincronizado com a sua linha correspondente da tabela ou as linhas no banco de dados.

Quando você lê uma entidade, o contexto de objeto armazena-o no Gerenciador de estado do objeto e controla se essa representação do objeto está em sincronia com o banco de dados. Por exemplo, se você alterar um valor de propriedade, um sinalizador é definido para indicar que a propriedade que você alterou não está mais em sincronia com o banco de dados. Em seguida, quando você chama o `SaveChanges` método, o contexto de objeto sabe o que fazer no banco de dados porque o Gerenciador de estado do objeto sabe exatamente qual é a diferença entre o estado atual da entidade e o estado do banco de dados.

No entanto, esse processo geralmente não funciona em um aplicativo web, porque a instância do contexto de objeto que lê uma entidade, juntamente com todo o conteúdo do seu Gerenciador de estado do objeto é descartada depois que uma página é renderizada. A instância do contexto de objeto que deve aplicar as alterações é um novo é instanciado para processamento de postagem. No caso do `DeleteDepartment` método, o `ObjectDataSource` controle cria novamente a versão original da entidade para você de valores em estado de exibição, mas isso recriado `Department` entidade não existe no Gerenciador de estado do objeto. Se você chamou o `DeleteObject` método nessa entidade criada novamente, a chamada falhará porque o contexto de objeto não sabe se a entidade está em sincronia com o banco de dados. No entanto, chamar o `Attach` método restabelece a mesma de controle entre a entidade criada novamente e os valores no banco de dados que originalmente era feito automaticamente quando a entidade foi lido em uma instância anterior do contexto de objeto.

Há vezes quando você não quer o contexto de objeto para acompanhar as entidades no Gerenciador de estado de objeto, e você pode definir sinalizadores para impedir a fazer isso. Exemplos disso são mostrados nos tutoriais mais adiante nesta série.

### <a name="the-savechanges-method"></a>O método SaveChanges

Essa classe de repositório simples ilustra os princípios básicos de como realizar operações CRUD. Neste exemplo, o `SaveChanges` método é chamado imediatamente após cada atualização. Em um aplicativo de produção, você talvez queira chamar o `SaveChanges` método de um método separado para lhe dar mais controle sobre quando o banco de dados é atualizado. (No final do próximo tutorial você encontrará um link para um white paper que aborda o padrão de unidade de trabalho que é uma abordagem para coordenar as atualizações relacionadas.) Observe também que no exemplo, o `DeleteDepartment` método não inclui código para lidar com conflitos de simultaneidade, o código para fazer isso será adicionado em um tutorial posterior nesta série.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Recuperando nomes de instrutor para selecionar ao inserir

Os usuários devem poder selecionar um administrador em uma lista de instrutores em uma lista suspensa durante a criação de novos departamentos. Portanto, adicione o seguinte código ao *SchoolRepository.cs* para criar um método para recuperar a lista de instrutores usando o modo de exibição que você criou anteriormente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Criando uma página para a inserção de departamentos

Criar uma *DepartmentsAdd.aspx* página que usa o *Master* da página e adicione a seguinte marcação no `Content` controle chamado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Essa marcação cria dois `ObjectDataSource` controles, uma para inserir novos `Department` entidades e outra para recuperar os nomes de instrutor para o `DropDownList` controle que é usado para selecionar os administradores de departamento. A marcação cria um `DetailsView` para inserir novos departamentos e ele especifica um manipulador para o controle de controle `ItemInserting` evento para que você possa definir o `Administrator` valor de chave estrangeira. No final é um `ValidationSummary` controle para exibir mensagens de erro.

Abra *DepartmentsAdd.aspx.cs* e adicione o seguinte `using` instrução:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Adicione os métodos e a variável de classe a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

O `Page_Init` método habilita a funcionalidade de dados dinâmicos. O manipulador para o `DropDownList` do controle `Init` evento salva uma referência para o controle e o manipulador para o `DetailsView` do controle `Inserting` evento usa essa referência para obter o `PersonID` valor da atualização e do instrutor selecionado o `Administrator` propriedade de chave estrangeira do `Department` entidade.

Execute a página, adicione informações do departamento e, em seguida, clique no **inserir** link.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Insira valores para outro novo departamento. Insira um número maior que 1,000,000.00 na **orçamento** campo e o guia para o próximo campo. Um asterisco é exibido no campo e, se você mantiver o ponteiro do mouse sobre ele, você pode ver a mensagem de erro que você inseriu nos metadados para esse campo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Clique em **inserir**, e você verá a mensagem de erro exibida pelo `ValidationSummary` controle na parte inferior da página.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Em seguida, feche o navegador e abra o *Departments.aspx* página. Adicionar funcionalidade de exclusão para o *Departments.aspx* página adicionando um `DeleteMethod` de atributo para o `ObjectDataSource` controle e uma `DataKeyNames` atributo para o `GridView` controle. As marcas de abertura para esses controles agora serão semelhante ao exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Execute a página.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Exclui o departamento que você adicionou quando você executou o *DepartmentsAdd.aspx* página.

## <a name="adding-update-functionality"></a>Adicionando funcionalidade de atualização

Abra *SchoolRepository.cs* e adicione o seguinte `Update` método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Quando você clica em **atualização** na *Departments.aspx* página, o `ObjectDataSource` controle cria dois `Department` entidades a serem passados para o `UpdateDepartment` método. Uma contém os valores originais que foram armazenados no estado de exibição e o outro contém os novos valores que foram inseridos no `GridView` controle. O código na `UpdateDepartment` método passa a `Department` entidade que tem os valores originais para o `Attach` método para estabelecer o rastreamento entre a entidade e o que está no banco de dados. Em seguida, o código passa o `Department` entidade que tem os novos valores para o `ApplyCurrentValues` método. O contexto de objeto compara os valores novos e antigos. Se um novo valor for diferente de um valor antigo, o contexto de objeto altera o valor da propriedade. O `SaveChanges` método, em seguida, atualiza apenas as colunas alteradas no banco de dados. (No entanto, se a função de atualização para esta entidade foram mapeada para um procedimento armazenado, a linha inteira será atualizada, independentemente de qual colunas foram alteradas.)

Abra o *Departments.aspx* arquivo e adicione os seguintes atributos para o `DepartmentsObjectDataSource` controle:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Esse valores antigos faz com que sejam armazenados no estado de exibição para que eles podem ser comparados com os novos valores no `Update` método.
- `OldValuesParameterFormatString="orig{0}"`   
 Isso informa o controle que é o nome do parâmetro de valores originais `origDepartment` .

A marcação para a marca de abertura do `ObjectDataSource` controle agora é semelhante ao exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Adicionar um `OnRowUpdating="DepartmentsGridView_RowUpdating"` de atributo para o `GridView` controle. Você usará isso para definir o `Administrator` valor da propriedade com base na linha em que o usuário seleciona em uma lista suspensa. O `GridView` marca de abertura agora se parece com o exemplo a seguir:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Adicionar um `EditItemTemplate` de controle para o `Administrator` coluna para o `GridView` controlar, imediatamente após o `ItemTemplate` controle para essa coluna:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Isso `EditItemTemplate` controle é semelhante de `InsertItemTemplate` no controlar a *DepartmentsAdd.aspx* página. A diferença é que o valor inicial do controle é definido usando o `SelectedValue` atributo.

Antes do `GridView` de controle, adicione uma `ValidationSummary` controlam como você fez na *DepartmentsAdd.aspx* página.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Abra *Departments.aspx.cs* e imediatamente após a declaração de classe parcial, adicione o código a seguir para criar um campo particular para fazer referência a `DropDownList` controle:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Em seguida, adicione manipuladores para o `DropDownList` do controle `Init` evento e o `GridView` do controle `RowUpdating` eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

O manipulador para o `Init` evento salva uma referência para o `DropDownList` controle no campo de classe. O manipulador para o `RowUpdating` evento usa a referência para obter o valor inserido pelo usuário e aplicá-lo para o `Administrator` propriedade do `Department` entidade.

Use o *DepartmentsAdd.aspx* página para adicionar um novo departamento, em seguida, execute o *Departments.aspx* da página e clique em **editar** na linha que você adicionou.

> [!NOTE]
> Você não poderá editar as linhas que você não adicionou (ou seja, que já estavam no banco de dados), devido aos dados inválidos no banco de dados; os administradores para as linhas que foram criados com o banco de dados são os alunos. Se você tentar editar um deles, você obterá uma página de erro que informa um erro como `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Se você inserir um inválido **orçamento** amount e, em seguida, clique em **atualização**, você verá a mesmo asterisco e mensagem de erro que você viu no *Departments.aspx* página.

Alterar um valor de campo ou selecione um administrador diferente e clique em **atualização**. A alteração é exibida.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Isso conclui a introdução ao uso o `ObjectDataSource` controle para básica CRUD (criar, ler, atualizar e excluir) operações com o Entity Framework. Você criou um aplicativo de n camadas simple, mas a camada de lógica de negócios ainda estará rigidamente acoplada à camada de acesso a dados, o que complica o teste de unidade automatizado. O tutorial a seguir, você verá como implementar o padrão de repositório para facilitar o teste de unidade.

> [!div class="step-by-step"]
> [Avançar](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
