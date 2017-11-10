---
title: "Núcleo do ASP.NET MVC com núcleo EF - herança - 9 de 10"
author: tdykstra
description: "Este tutorial mostrará a implementação de herança no modelo de dados, usando o Entity Framework Core em um aplicativo do ASP.NET Core."
keywords: "Herança de ASP.NET Core, Entity Framework Core,"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 41dc0db7-6f17-453e-aba6-633430609c74
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 10bde121dac3bdbbf0e55f2d146d91dea0f0210f
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---

# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>Herança - Tutorial do EF Core com ASP.NET Core MVC (9 de 10)


Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos web do ASP.NET Core MVC usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

No tutorial anterior, você tratou exceções de simultaneidade. Este tutorial mostra como implementar a herança no modelo de dados.

Em programação orientada a objeto, você pode usar a herança para facilitar a reutilização de código. Neste tutorial, você alterará a `Instructor` e `Student` classes para que eles derivam de um `Person` classe que contém propriedades, como base `LastName` que são comuns a professores e alunos. Você não adicionar ou alterar todas as páginas da web, mas você alterará a parte do código e essas alterações serão refletidas automaticamente no banco de dados.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opções para o mapeamento de herança para as tabelas de banco de dados

O `Instructor` e `Student` classes no modelo de dados School têm várias propriedades que são idênticas:

![Classes de Student e instrutor](inheritance/_static/no-inheritance.png)

Suponha que você deseja eliminar o código de redundância para as propriedades que são compartilhadas pelo `Instructor` e `Student` entidades. Ou você quiser escrever um serviço que pode formatar nomes sem cuidar se o nome da origem do instrutor ou um aluno. Você pode criar um `Person` classe base que contém apenas as propriedades compartilhadas e faça o `Instructor` e `Student` classes herdam a classe base, conforme mostrado na ilustração a seguir:

![Classes de Student e instrutor derivar da classe pessoa](inheritance/_static/inheritance.png)

Há várias maneiras que essa estrutura de herança poderia ser representada no banco de dados. Você pode ter uma tabela da pessoa que inclui informações sobre os alunos e instrutores em uma única tabela. Algumas das colunas podem aplicar somente a instrutores (HireDate), alguns somente para os alunos (EnrollmentDate), alguns para os dois (Sobrenome, nome). Normalmente, você teria uma coluna discriminadora para indicar qual tipo de cada linha representa. Por exemplo, coluna discriminadora pode ter "Instrutor" para "Aluno" e instrutores para estudantes.

![Exemplo de tabela por hierarquia](inheritance/_static/tph.png)

Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados é chamado de herança (TPH) de tabela por hierarquia.

Uma alternativa é fazer com que o banco de dados pareça mais com a estrutura de herança. Por exemplo, você pode ter apenas os campos de nome na tabela Person e ter tabelas separadas do aluno e do instrutor com os campos de data.

![Herança de tabela por tipo](inheritance/_static/tpt.png)

Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamado de tabela por herança de tipo (TPT).

Outra opção é mapear todos os tipos de não-abstrato para tabelas individuais. Todas as propriedades de uma classe, incluindo propriedades herdadas, mapeiam para colunas da tabela correspondente. Esse padrão é chamado de herança de classe de tabela por concreto TPC (). Se você tiver implementado herança TPC para as classes da pessoa, do aluno e do instrutor como mostrado anteriormente, as tabelas Student e instrutor seria não diferentes depois de implementar a herança do que antes.

Padrões de herança TPC e TPH geralmente oferecem melhor desempenho de padrões de herança TPT, porque padrões TPT podem resultar em consultas de junção complexas.

Este tutorial demonstra como implementar a herança TPH. TPH é o padrão de herança única que o Entity Framework Core oferece suporte.  Você vai fazer é criar um `Person` classe, altere o `Instructor` e `Student` classes derivar de `Person`, adicione a nova classe para o `DbContext`e crie uma migração.

> [!TIP] 
> Considere a possibilidade de salvar uma cópia do projeto antes de fazer as seguintes alterações.  Em seguida, se você tiver problemas e precisa recomeçar, ela será mais fácil de iniciar do projeto em vez de reverter as etapas executadas para este tutorial ou que será salvo novamente para o início da série inteira.

## <a name="create-the-person-class"></a>Criar a classe pessoa

Na pasta de modelos, criar Person.cs e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Verifique o aluno e do instrutor classes herdam a pessoa

Em *Instructor.cs*, a classe do instrutor é derivado da classe pessoa e remover os campos nome e chave. O código será semelhante o exemplo a seguir:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Fazer as mesmas alterações em *Student.cs*.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Adicione o tipo de entidade de pessoa para o modelo de dados

Adicione o tipo de entidade de pessoa para *SchoolContext.cs*. As novas linhas são realçadas.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia. Como você verá, quando o banco de dados é atualizado, ele terá uma tabela pessoa no lugar as tabelas Student e instrutor.

## <a name="create-and-customize-migration-code"></a>Criar e personalizar o código de migração

Salve suas alterações e compilar o projeto. Em seguida, abra a janela de comando na pasta do projeto e digite o seguinte comando:

```console
dotnet ef migrations add Inheritance
```

Não execute o `database update` comando ainda. Esse comando resultará em perda de dados porque ele descarte a tabela instrutor e renomeie a tabela de alunos a pessoa. Você precisa fornecer o código personalizado para preservar os dados existentes.

Abra *migrações /\<timestamp > _Inheritance.cs* e substitua o `Up` método com o código a seguir:

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Esse código cuida das seguintes tarefas de atualização de banco de dados:

* Remove as restrições de chave estrangeira e índices que apontam para a tabela de alunos.

* Renomeia a tabela instrutor como pessoa e faz as alterações necessárias para ele armazenar dados de aluno:

* Adiciona EnrollmentDate anulável para estudantes.

* Adiciona a coluna discriminadora para indicar se uma linha é para um aluno ou instrutor.

* Torna HireDate anulável como linhas de aluno não têm datas de contratação.

* Adiciona um campo temporário que será usado para atualizar chaves estrangeiras que apontam para os alunos. Quando você copia os alunos para a tabela pessoa que receberão novos valores de chave primária.

* Copia os dados da tabela de estudante na tabela Person. Isso faz com que os alunos novos valores de chave primária foi atribuído.

* Correções de valores de chave estrangeira que apontam para os alunos.

* Recria índices, agora apontá-los para a tabela Person e restrições de chave estrangeira.

(Se você tiver usado o GUID, em vez de inteiro como o tipo de chave primário, os valores de chave primária do aluno não precisam alterar e várias dessas etapas foi foi omitidas).

Execute o `database update` comando:

```console
dotnet ef database update
```

(Em um sistema de produção faça as alterações correspondentes a `Down` método caso você teve de usá-la para voltar para a versão anterior do banco de dados. Para este tutorial, você não usará o `Down` método.)

> [!NOTE] 
> É possível obter outros erros ao fazer alterações de esquema em um banco de dados que contém dados existentes. Se você obtiver erros de migração que você não conseguir resolver, você pode alterar o nome do banco de dados na cadeia de conexão ou excluir o banco de dados. Com um novo banco de dados, não há nenhum dado para migrar e o comando de atualização de banco de dados é mais provável de ser concluído sem erros. Para excluir o banco de dados, use SSOX ou execute o `database drop` comando CLI.

## <a name="test-with-inheritance-implemented"></a>Testar com herança implementada

Execute o aplicativo e tente várias páginas. Tudo funciona da mesma forma que antes.

Em **Pesquisador de objetos do SQL Server**, expanda **conexões de dados/SchoolContext** e **tabelas**, e você verá que as tabelas Student e instrutor foram substituídas por um Tabela Person. Abra o designer de tabela pessoa e você verá que ele tem todas as colunas que costumava ser nas tabelas Student e instrutor.

![Tabela Person em SSOX](inheritance/_static/ssox-person-table.png)

Clique com botão direito a tabela Person e, em seguida, clique em **Mostrar dados da tabela** para ver a coluna discriminadora.

![Tabela Person em SSOX - dados de tabela](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Resumo

Você implementou a herança de tabela por hierarquia para o `Person`, `Student`, e `Instructor` classes. Para obter mais informações sobre a herança em Entity Framework Core, consulte [herança](https://docs.microsoft.com/ef/core/modeling/inheritance). O seguinte tutorial, você verá como manipular uma variedade de cenários relativamente avançados do Entity Framework.

>[!div class="step-by-step"]
[Anterior](concurrency.md)
[Próximo](advanced.md)  
