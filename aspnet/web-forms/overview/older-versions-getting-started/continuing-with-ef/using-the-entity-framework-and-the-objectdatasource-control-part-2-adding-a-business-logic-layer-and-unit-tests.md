---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 2: adicionando uma camada de lógica de negócios e testes de unidade | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo web Contoso University que é criado pelo Getting Started with a série de tutoriais do Entity Framework 4.0. EU...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 02f0b86203eb879ca618655b8956f22dc67858cd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394236"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Usando o Entity Framework 4.0 e o controle ObjectDataSource, parte 2: adicionando uma camada de lógica de negócios e testes de unidade
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais baseia-se no aplicativo web Contoso University que é criado pela [Introdução ao Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você teria criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, você pode postá-los para o [Fórum do Entity Framework do ASP.NET](https://forums.asp.net/1227.aspx).


No tutorial anterior, você criou um aplicativo web de n camadas usando o Entity Framework e o `ObjectDataSource` controle. Este tutorial mostra como adicionar lógica de negócios, mantendo a camada de lógica de negócios (BLL) e a camada de acesso a dados (DAL) separada, e mostra como criar testes de unidade automatizados para a BLL.

Neste tutorial, você concluirá as seguintes tarefas:

- Crie uma interface de repositório que declara os métodos de acesso a dados que você precisa.
- Implemente a interface do repositório na classe de repositório.
- Crie uma classe de lógica de negócios que chama a classe de repositório para executar funções de acesso a dados.
- Conectar-se a `ObjectDataSource` controle para a classe de lógica de negócios em vez de para a classe de repositório.
- Crie um projeto de teste de unidade e uma classe de repositório que usa coleções na memória para seu armazenamento de dados.
- Crie um teste de unidade lógica de negócios que você deseja adicionar à classe de lógica de negócios, em seguida, execute o teste e vê-lo falhar.
- Implementar a lógica de negócios na classe de lógica de negócios e, em seguida, execute novamente a unidade de teste e vê-lo passar.

Você trabalhará com o *Departments.aspx* e *DepartmentsAdd.aspx* páginas que você criou no tutorial anterior.

## <a name="creating-a-repository-interface"></a>Criando uma Interface de repositório

Você começará criando a interface do repositório.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

No *DAL* pasta, crie um novo arquivo de classe, nomeie- *ISchoolRepository.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

A interface define um método para cada um de CRUD (criar, ler, atualizar e excluir) os métodos que você criou na classe de repositório.

No `SchoolRepository` classe *SchoolRepository.cs*, indicar que essa classe implementa o `ISchoolRepository` interface:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Criando uma classe de lógica de negócios

Em seguida, você criará a classe de lógica de negócios. Fazer isso para que você possa adicionar lógica de negócios que será executada pelo `ObjectDataSource` controlar, embora você não fará isso ainda. Por enquanto, a nova classe de lógica de negócios apenas executará as mesmas operações CRUD que faz o repositório.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Crie uma nova pasta e denomine *BLL*. (Em um aplicativo do mundo real, a camada de lógica de negócios normalmente será implementada como uma biblioteca de classes — um projeto separado, mas para simplificar este tutorial, as classes BLL serão mantidas em uma pasta de projeto.)

No *BLL* pasta, crie um novo arquivo de classe, nomeie- *SchoolBL.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Esse código cria os mesmos métodos CRUD, que você viu anteriormente na classe de repositório, mas em vez de acessar diretamente os métodos do Entity Framework, ele chama o repositório de métodos de classe.

A variável de classe que contém uma referência para a classe de repositório é definida como um tipo de interface e o código que instancia a classe de repositório está contido em dois construtores. O construtor sem parâmetro será usado pelo `ObjectDataSource` controle. Ele cria uma instância da `SchoolRepository` classe que você criou anteriormente. O outro construtor permite que qualquer código que instancia a classe de lógica de negócios para passar em qualquer objeto que implementa a interface do repositório.

Os métodos CRUD que chamam a classe de repositório e os dois construtores tornam possível usar a classe de lógica de negócios com qualquer repositório de dados de back-end que você escolher. A classe de lógica de negócios não precisa estar ciente de como a classe que está chamando persiste os dados. (Isso é frequentemente chamado *ignorância de persistência*.) Isso facilita os testes de unidade, como a classe de lógica de negócios podem se conectar a uma implementação de repositório que use algo tão simples como na memória `List` coleções para armazenar dados.

> [!NOTE]
> Tecnicamente, os objetos de entidade são ainda não com ignorância de persistência, porque eles são instanciados de classes que herdam do Entity Framework `EntityObject` classe. Ignorância de persistência concluída, você pode usar *objetos CLR antigos simples*, ou *POCOs*, em vez de objetos que herdam a `EntityObject` classe. Usar POCOs está além do escopo deste tutorial. Para obter mais informações, consulte [possibilidade de teste e o Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) no site do MSDN.)


Agora, você pode conectar o `ObjectDataSource` controles para a lógica de negócios de classe em vez de no repositório e verificar se tudo está funcionando como antes.

Na *Departments.aspx* e *DepartmentsAdd.aspx*, altere cada ocorrência de `TypeName="ContosoUniversity.DAL.SchoolRepository"` para `TypeName="ContosoUniversity.BLL.SchoolBL`". (Há quatro instâncias em todas.)

Execute o *Departments.aspx* e *DepartmentsAdd.aspx* páginas para verificar se eles ainda funcionarão como antes.

[![Para Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Criando um projeto de teste de unidade e a implementação de repositório

Adicionar um novo projeto à solução usando o **projeto de teste** modelo e nomeie-o `ContosoUniversity.Tests`.

No projeto de teste, adicione uma referência a `System.Data.Entity` e adicione uma referência de projeto para o `ContosoUniversity` projeto.

Agora você pode criar a classe de repositório que você usará com testes de unidade. O armazenamento de dados para esse repositório será dentro da classe.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

No projeto de teste, crie um novo arquivo de classe, nomeie- *MockSchoolRepository.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Essa classe de repositório tem os mesmos métodos CRUD que acessa o Entity Framework diretamente, mas eles funcionam com `List` coleções na memória, em vez de com um banco de dados. Isso torna mais fácil para uma classe de teste configurar e validar testes de unidade para a classe de lógica de negócios.

## <a name="creating-unit-tests"></a>Criando testes de unidade

O **testar** modelo de projeto criado uma classe de teste de unidade de stub para você e sua próxima tarefa é modificar essa classe com a adição de métodos de teste de unidade de lógica comercial que você deseja adicionar à classe de lógica de negócios.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Na Contoso University, qualquer instrutor individual só pode ser o administrador de um único departamento e você precisa adicionar lógica de negócios para impor essa regra. Você começará adicionando testes e executar os testes para vê-los a falhar. Em seguida, você adicionará o código e executar novamente os testes, esperando para vê-los a passar.

Abra o *UnitTest1.cs* arquivo e adicione `using` instruções para as camadas de lógica e acesso a dados de negócios que você criou no projeto ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Substitua o `TestMethod1` método com os seguintes métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

O `CreateSchoolBL` método cria uma instância da classe de repositório que você criou para o teste de unidade projeto, que, em seguida, ele passa para uma nova instância da classe de lógica de negócios. O método, em seguida, usa a classe de lógica de negócios para inserir três departamentos que pode ser usado em métodos de teste.

Os métodos de teste, verifique se que a classe de lógica de negócios gera uma exceção se alguém tentar inserir um novo departamento com o mesmo administrador como um departamento existente, ou se alguém tentar atualizar o administrador de um departamento definindo-a para a ID de uma pessoa quem já é o administrador de outro departamento.

Você não criou a classe de exceção ainda, portanto, esse código não compilará. Para obter compilá-lo, clique com botão direito `DuplicateAdministratorException` e selecione **gerar**e então **classe**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Isso cria uma classe no projeto de teste que você pode excluir depois de criar a classe de exceção no projeto principal. e implementados a lógica de negócios.

Execute o projeto de teste. Conforme o esperado, os testes falham.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Adicionando lógica de negócios para fazer uma passagem de teste

Em seguida, você implementará a lógica de negócios que torna impossível definir como o administrador de um departamento que alguém que já seja um administrador de outro departamento. Você lançar uma exceção da camada de lógica de negócios e capturá-la na camada de apresentação se um usuário edita um departamento e clica **atualização** depois de selecionar a alguém que já seja um administrador. (Você também pode remover os instrutores da lista suspensa que já são administradores antes de renderizar a página, mas o objetivo aqui é trabalhar com a camada de lógica de negócios).

Comece criando a classe de exceção que você vai lançar quando um usuário tenta fazer um instrutor o administrador de mais de um departamento. No projeto principal, crie um novo arquivo de classe na *BLL* pasta, nomeie- *DuplicateAdministratorException.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Excluir agora temporários *DuplicateAdministratorException.cs* arquivo que você criou no projeto de teste para poder compilar.

No projeto principal, abra o *SchoolBL.cs* arquivo e adicione o seguinte método que contém a lógica de validação. (O código se refere a um método que você criará posteriormente).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Você vai chamar este método quando você estiver inserindo ou atualizando `Department` entidades para verificar se outro departamento já tem o mesmo administrador.

O código chama um método para pesquisar o banco de dados para um `Department` entidade que tem o mesmo `Administrator` valor da propriedade como a entidade que está sendo inserida ou atualizada. Se um for encontrado, o código gera uma exceção. É necessária nenhuma verificação de validação se a entidade que está sendo inserido ou atualizado não tem nenhum `Administrator` valor e nenhuma exceção será lançada se o método é chamado durante uma atualização e o `Department` entidade encontrada corresponde a `Department` entidade que está sendo atualizada.

Chame o novo método a partir de `Insert` e `Update` métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

Na *ISchoolRepository.cs*, adicione a seguinte declaração para o novo método de acesso a dados:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

Na *SchoolRepository.cs*, adicione o seguinte `using` instrução:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

Na *SchoolRepository.cs*, adicione o novo método de acesso a dados a seguir:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Esse código recupera `Department` entidades que têm um administrador especificada. Deve estar presente apenas um departamento (se houver). No entanto, porque nenhuma restrição é criada no banco de dados, o tipo de retorno é uma coleção, no caso de vários departamentos são encontrados.

Por padrão, quando o contexto de objeto recupera entidades de banco de dados, ele mantém o controle sobre eles em seu Gerenciador de estado do objeto. O `MergeOption.NoTracking` parâmetro especifica que esse controle não será feito para esta consulta. Isso é necessário porque a consulta pode retornar a entidade exata que você está tentando atualizar e, em seguida, você não poderá anexar essa entidade. Por exemplo, se você editar o departamento de histórico a *Departments.aspx* página e deixa o administrador inalterado, essa consulta retornará o departamento de histórico. Se `NoTracking` não for definido, o contexto de objeto já teria a entidade de departamento histórico em seu Gerenciador de estado do objeto. Em seguida, quando você anexa a entidade de departamento de histórico é recriada do estado de exibição, o contexto de objeto lançaria uma exceção informando `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Como uma alternativa para especificar `MergeOption.NoTracking`, você pode criar um novo contexto de objeto apenas para essa consulta. Porque o novo contexto de objeto teria seu próprio Gerenciador de estado do objeto, não haveria nenhum conflito quando você chama o `Attach` método. O novo contexto de objeto compartilhariam conexão de banco de dados e metadados com o contexto de objeto original, portanto, a penalidade de desempenho dessa abordagem alternativa seria mínimo. A abordagem mostrada aqui, no entanto, apresenta o `NoTracking` opção, o que você encontrará útil em outros contextos. O `NoTracking` opção é discutida mais detalhadamente em um tutorial posterior nesta série.)

No projeto de teste, adicione o novo método de acesso a dados para *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Esse código usa LINQ para executar a mesma seleção de dados que o `ContosoUniversity` LINQ to Entities para usa o repositório do projeto.

Execute novamente o projeto de teste. Dessa vez os testes são aprovados.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Tratamento de exceções do ObjectDataSource

No `ContosoUniversity` projeto, execute o *Departments.aspx* página e tente alterar o administrador de um departamento para alguém que já seja um administrador para outro departamento. (Lembre-se de que você só pode editar os departamentos que você adicionou durante este tutorial, porque o banco de dados vem pré-carregado com dados inválidos.) Você obtém a página de erro de servidor a seguir:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Você não quer que os usuários vejam esse tipo de página de erro, portanto, você precisa adicionar o código de tratamento de erros. Abra *Departments.aspx* e especifique um manipulador para o `OnUpdated` evento do `DepartmentsObjectDataSource`. O `ObjectDataSource` marca de abertura agora se parece com o exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

Na *Departments.aspx.cs*, adicione o seguinte `using` instrução:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Adicione o seguinte manipulador para o `Updated` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Se o `ObjectDataSource` controle captura uma exceção ao tentar executar a atualização, ele passa a exceção no argumento do evento (`e`) para este manipulador. O código no manipulador verifica para ver se a exceção é a exceção de administrador duplicado. Se for, o código cria um controle de validador que contém uma mensagem de erro para o `ValidationSummary` controle para exibir.

Execute a página e tente tornar alguém o administrador de dois departamentos novamente. Desta vez o `ValidationSummary` controle exibe uma mensagem de erro.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Fazer alterações semelhantes para o *DepartmentsAdd.aspx* página. Na *DepartmentsAdd.aspx*, especifique um manipulador para o `OnInserted` evento do `DepartmentsObjectDataSource`. A marcação resultante será semelhante ao exemplo a seguir.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

Na *DepartmentsAdd.aspx.cs*, adicione o mesmo `using` instrução:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Adicione o seguinte manipulador de eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Agora você pode testar o *DepartmentsAdd.aspx.cs* página para verificar se que ele manipulará corretamente também tenta tornar o administrador de mais de um departamento de uma pessoa.

Isso conclui a introdução para implementar o padrão de repositório para usar o `ObjectDataSource` controle com o Entity Framework. Para obter mais informações sobre o padrão de repositório e a capacidade de teste, consulte o white paper MSDN [possibilidade de teste e o Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

O tutorial a seguir, você verá como adicionar classificação e filtragem de funcionalidade para o aplicativo.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Próximo](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
