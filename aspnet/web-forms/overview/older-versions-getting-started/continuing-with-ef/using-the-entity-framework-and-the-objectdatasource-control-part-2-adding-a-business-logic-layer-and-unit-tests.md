---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 2: adicionar uma camada de lógica de negócios e testes de unidade | Microsoft Docs'
author: tdykstra
description: Esta série de tutorial se baseia no aplicativo web Contoso Universidade que é criado pelo guia de Introdução com a série de tutoriais do Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: ecdfb2bdc546f55778ec4cc1f61aa66e129134ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 2: adicionar uma camada de lógica de negócios e testes de unidade
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo da web Contoso Universidade que é criado pelo [guia de Introdução com o Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você pode ter sido criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série tutorial completo. Se você tiver dúvidas sobre os tutoriais, você poderá postá-los para o [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


No tutorial anterior, você criou um aplicativo web de n camadas usando o Entity Framework e o `ObjectDataSource` controle. Este tutorial mostra como adicionar lógica de negócios, mantendo a camada de lógica de negócios (BLL) e a camada de acesso a dados (DAL) separados, e ele mostra como criar testes de unidade automatizados para BLL.

Neste tutorial, você concluirá as seguintes tarefas:

- Crie uma interface de repositório que declara os métodos de acesso a dados que você precisa.
- Implemente a interface do repositório na classe do repositório.
- Crie uma classe de lógica de negócios que chama a classe de repositório para executar funções de acesso a dados.
- Conecte-se a `ObjectDataSource` controle à classe em vez de lógica de negócios para a classe do repositório.
- Crie um projeto de teste de unidade e uma classe de repositório que usa coleções na memória para seu repositório de dados.
- Crie um teste de unidade lógica de negócios que você deseja adicionar à classe de lógica de negócios, em seguida, execute o teste e vê-lo a falhar.
- Implementar a lógica de negócios na classe de lógica de negócios, em seguida, execute novamente a unidade de teste e vê-lo a passar.

Você trabalhará com o *Departments.aspx* e *DepartmentsAdd.aspx* páginas que você criou no tutorial anterior.

## <a name="creating-a-repository-interface"></a>Criando uma Interface do repositório

Você começará criando a interface do repositório.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

No *DAL* pasta, crie um novo arquivo de classe, nomeie-o *ISchoolRepository.cs*e substitua o código existente pelo seguinte código:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

A interface define um método para cada o CRUD (criar, ler, atualizar e excluir) métodos que você criou na classe do repositório.

No `SchoolRepository` classe em *SchoolRepository.cs*, indicar que esta classe implementa o `ISchoolRepository` interface:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Criando uma classe de lógica de negócios

Em seguida, você criará a classe de lógica de negócios. Fazer isso para que você possa adicionar lógica de negócios que será executada pelo `ObjectDataSource` controlar, embora você não fará isso ainda. Agora, a nova classe de lógica de negócios executará somente as operações CRUD mesmo que o repositório.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Crie uma nova pasta e nomeie- *BLL*. (Em um aplicativo do mundo real, a camada de lógica de negócios normalmente seria implementada como uma biblioteca de classe — um projeto separado, mas para simplificar este tutorial, classes BLL serão mantidos em uma pasta de projeto.)

No *BLL* pasta, crie um novo arquivo de classe, nomeie-o *SchoolBL.cs*e substitua o código existente pelo seguinte código:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Esse código cria os mesmos métodos CRUD visto anteriormente na classe de repositório, mas em vez de acessar diretamente os métodos Entity Framework, ele chama o repositório métodos da classe.

A variável de classe que contém uma referência para a classe de repositório é definida como um tipo de interface e o código que cria uma instância da classe do repositório está contido em dois construtores. O construtor sem parâmetro será usado pelo `ObjectDataSource` controle. Ele cria uma instância do `SchoolRepository` classe que você criou anteriormente. O outro construtor permite que qualquer código que cria uma instância da classe de lógica de negócios para transmitir qualquer objeto que implementa a interface do repositório.

Os métodos CRUD que chamam a classe do repositório e dois construtores tornam possível usar a classe de lógica de negócios com qualquer repositório de dados de back-end que você escolher. A classe de lógica de negócios não precisa estar ciente de como a classe que está chamando persiste os dados. (Isso é geralmente chamado *ignorância de persistência*.) Isso facilita o teste de unidade, como a classe de lógica de negócios podem se conectar a uma implementação de repositório que use algo como simples como na memória `List` coleções para armazenar dados.

> [!NOTE]
> Tecnicamente, os objetos de entidade são ainda não persistência desconhecem, porque eles estiverem instanciados a partir de classes que herdam o Entity Framework `EntityObject` classe. Para ignorância de persistência concluída, você pode usar *simples objetos CLR antigos*, ou *POCOs*, em vez de objetos que herdam o `EntityObject` classe. Usar POCOs está além do escopo deste tutorial. Para obter mais informações, consulte [capacidade de teste e o Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) no site do MSDN.)


Agora você pode se conectar a `ObjectDataSource` controles para a lógica de negócios de classe em vez de para o repositório e verificar se tudo está funcionando como antes.

Em *Departments.aspx* e *DepartmentsAdd.aspx*, alterar cada ocorrência de `TypeName="ContosoUniversity.DAL.SchoolRepository"` para `TypeName="ContosoUniversity.BLL.SchoolBL`". (Há quatro instâncias em todas.)

Execute o *Departments.aspx* e *DepartmentsAdd.aspx* páginas para verificar se eles ainda funcionam como antes.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Criando um projeto de teste de unidade e a implementação do repositório

Adicionar um novo projeto à solução usando o **projeto de teste** modelo e nomeie-o `ContosoUniversity.Tests`.

No projeto de teste, adicione uma referência a `System.Data.Entity` e adicione uma referência de projeto para o `ContosoUniversity` projeto.

Agora você pode criar a classe de repositório que você usará com testes de unidade. O repositório de dados para este repositório será dentro da classe.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

No projeto de teste, crie um novo arquivo de classe, nomeie-o *MockSchoolRepository.cs*e substitua o código existente pelo seguinte código:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Essa classe de repositório tem os mesmos métodos CRUD que acessa o Entity Framework diretamente, mas elas funcionam com `List` coleções na memória, em vez de um banco de dados. Isso torna mais fácil para uma classe de teste configurar e validar testes de unidade para a classe de lógica de negócios.

## <a name="creating-unit-tests"></a>Criar testes de unidade

O **teste** modelo de projeto criado uma classe de teste de unidade de stub para você e a próxima tarefa é modificar essa classe adicionando métodos de teste de unidade a ele para lógica de negócios que você deseja adicionar à classe de lógica de negócios.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Universidade Contoso, qualquer instrutor individual só pode ser o administrador de um único departamento e você precisa adicionar lógica de negócios para impor essa regra. Você começará adicionando testes e executar os testes para vê-los a falhar. Em seguida, você adicionará o código e execute novamente os testes, esperando para vê-los passar.

Abra o *UnitTest1.cs* de arquivos e adicionar `using` instruções para as camadas de acesso a dados e de lógica de negócios que você criou no projeto ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Substitua o `TestMethod1` método com os seguintes métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

O `CreateSchoolBL` método cria uma instância da classe de repositório que você criou para o teste de unidade projeto, o que ele passa para uma nova instância da classe de lógica de negócios. O método usa a classe de lógica de negócios para inserir três departamentos que podem ser usados em métodos de teste.

Os métodos de teste, verifique se que a classe de lógica de negócios lança uma exceção se alguém tenta inserir um novo departamento com o mesmo administrador de um departamento existente, ou se alguém tentar atualizar o administrador do departamento definindo-o para a ID de uma pessoa que já é o administrador de outro departamento.

Você não criou ainda, a classe de exceção para que esse código não será compilado. Para obter compilá-lo, clique com botão direito `DuplicateAdministratorException` e selecione **gerar**e, em seguida, **classe**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Isso cria uma classe no projeto de teste que você pode excluir depois de criar a classe de exceção no projeto principal. e implementado a lógica de negócios.

Execute o projeto de teste. Conforme o esperado, os testes falham.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Adicionando lógica de negócios para fazer uma passagem de teste

Em seguida, você irá implementar a lógica de negócios que torna impossível definir como o administrador de um departamento que alguém que já seja um administrador de outro departamento. Você lançar uma exceção da camada de lógica de negócios e, em seguida, capturar na camada de apresentação se um usuário edita um departamento e clica em **atualização** depois de selecionar alguém que já seja um administrador. (Você também pode remover instrutores na lista suspensa que já são administradores antes de renderizar a página, mas a finalidade aqui é trabalhar com a camada de lógica de negócios).

Comece criando a classe de exceção que você vai lançar quando um usuário tenta fazer um instrutor o administrador de mais de um departamento. No projeto principal, crie um novo arquivo de classe no *BLL* pasta, nomeie-o *DuplicateAdministratorException.cs*e substitua o código existente pelo seguinte código:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Excluir agora temporárias *DuplicateAdministratorException.cs* arquivo que você criou no projeto de teste anteriores para que seja possível compilar.

No projeto principal, abra o *SchoolBL.cs* de arquivo e adicione o seguinte método que contém a lógica de validação. (O código se refere a um método que você criará mais tarde).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Você poderá chamar este método quando você inserir ou atualizar `Department` entidades para verificar se outro departamento já tem a mesma do administrador.

O código chama um método para pesquisar o banco de dados para um `Department` entidade que tem o mesmo `Administrator` valor da propriedade da entidade que está sendo inserido ou atualizado. Se for encontrado, o código gera uma exceção. Não há verificação de validação é necessária se a entidade que está sendo inserido ou atualizado não tem nenhum `Administrator` valor e nenhuma exceção é gerada se o método é chamado durante uma atualização e o `Department` entidade encontrada corresponde a `Department` entidade que está sendo atualizada.

Chamar o novo método do `Insert` e `Update` métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

Em *ISchoolRepository.cs*, adicione a declaração a seguir para o novo método de acesso a dados:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

Em *SchoolRepository.cs*, adicione o seguinte `using` instrução:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

Em *SchoolRepository.cs*, adicione o novo método de acesso a dados a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Este código recupera `Department` entidades que têm um administrador especificado. Deve estar presente apenas um departamento (se houver). No entanto, porque nenhuma restrição é criada no banco de dados, o tipo de retorno é uma coleção, no caso de vários departamentos são encontrados.

Por padrão, quando o contexto de objeto recupera entidades de banco de dados, ele mantém o controle de-los em seu Gerenciador de estado do objeto. O `MergeOption.NoTracking` parâmetro especifica que esse controle não será feito para esta consulta. Isso é necessário porque a consulta pode retornar a entidade exata que você está tentando atualizar e, em seguida, você não poderá anexar essa entidade. Por exemplo, se você editar o departamento de histórico no *Departments.aspx* página e deixe que o administrador inalterado, essa consulta retornará o departamento de histórico. Se `NoTracking` não estiver definido, o contexto de objeto já terá a entidade do departamento de histórico de seu Gerenciador de estado do objeto. Em seguida, quando você anexa a entidade de departamento de histórico é recriada do estado de exibição, o contexto de objeto lançaria uma exceção que diz `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Como uma alternativa para especificar `MergeOption.NoTracking`, você pode criar um novo contexto de objeto para esta consulta. Porque o novo contexto de objeto tem seu próprio Gerenciador de estado do objeto, não haveria nenhum conflito quando você chamar o `Attach` método. O novo contexto de objeto seria compartilhar conexão de banco de dados e metadados com o contexto de objeto original, para a degradação de desempenho dessa abordagem alternativa seria mínima. A abordagem mostrada aqui, no entanto, apresenta o `NoTracking` opção que você encontrará úteis em outros contextos. O `NoTracking` opção é discutida mais um tutorial posterior nesta série.)

No projeto teste, adicione o novo método de acesso a dados para *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Esse código usa LINQ para executar a seleção de dados mesmo que o `ContosoUniversity` repositório do projeto usa LINQ to Entities para.

Execute o projeto de teste novamente. Dessa vez os testes são aprovados.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Tratamento de exceções de ObjectDataSource

No `ContosoUniversity` projeto, execute o *Departments.aspx* página e tente alterar o administrador de um departamento para alguém que já seja um administrador para outro departamento. (Lembre-se de que você só pode editar os departamentos que você adicionou durante este tutorial, porque o banco de dados vem pré-carregado com dados inválidos.) Você obtém a página de erro de servidor a seguir:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Não convém que os usuários vejam esse tipo de página de erro, portanto você precisa adicionar o código de tratamento de erros. Abra *Departments.aspx* e especifique um manipulador para o `OnUpdated` eventos do `DepartmentsObjectDataSource`. O `ObjectDataSource` à marca de abertura agora se parece com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

Em *Departments.aspx.cs*, adicione o seguinte `using` instrução:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Adicione o seguinte manipulador para o `Updated` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Se o `ObjectDataSource` controle captura uma exceção ao tentar executar a atualização, ele passa a exceção de argumento do evento (`e`) a esse manipulador. O código no manipulador verifica se a exceção é a exceção do administrador duplicado. Se for, o código cria um controle de validação que contém uma mensagem de erro para o `ValidationSummary` controle para exibir.

Execute a página e tente fazer a alguém a administrador de dois departamentos novamente. Neste momento o `ValidationSummary` controle exibe uma mensagem de erro.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Fazer alterações semelhantes para o *DepartmentsAdd.aspx* página. Em *DepartmentsAdd.aspx*, especifique um manipulador para o `OnInserted` eventos do `DepartmentsObjectDataSource`. A marcação resultante será parecida com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

Em *DepartmentsAdd.aspx.cs*, adicionar a mesma `using` instrução:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Adicione o manipulador de eventos a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Agora você pode testar o *DepartmentsAdd.aspx.cs* página para verificar que manipula corretamente também tenta fazer uma pessoa que o administrador de mais de um departamento.

Isso conclui a introdução ao implementar o padrão de repositório para usar o `ObjectDataSource` controle com o Entity Framework. Para obter mais informações sobre o repositório padrão e a capacidade de teste, consulte o white paper MSDN [capacidade de teste e o Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

O tutorial a seguir, você verá como adicionar classificação e filtragem de funcionalidade para o aplicativo.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Próximo](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
