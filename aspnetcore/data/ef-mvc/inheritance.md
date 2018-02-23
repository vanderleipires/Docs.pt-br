---
title: "ASP.NET Core MVC com EF Core – herança – 9 de 10"
author: tdykstra
description: "Este tutorial mostrará como implementar a herança no modelo de dados, usando o Entity Framework Core em um aplicativo ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 985cc38b10ef830b8274e40ad5f7050157fd4d86
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/31/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>Herança – tutorial do EF Core com o ASP.NET Core MVC (9 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos web do ASP.NET Core MVC usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](intro.md).

No tutorial anterior, você tratou exceções de simultaneidade. Este tutorial mostrará como implementar a herança no modelo de dados.

Na programação orientada a objeto, você pode usar a herança para facilitar a reutilização de código. Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos. Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opções para o mapeamento de herança para as tabelas de banco de dados

As classes `Instructor` e `Student` no modelo de dados Escola têm várias propriedades idênticas:

![Classes Student e Instructor](inheritance/_static/no-inheritance.png)

Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`. Ou você deseja escrever um serviço que pode formatar nomes sem se preocupar se o nome foi obtido de um instrutor ou um aluno. Você pode criar uma classe base `Person` que contém apenas essas propriedades compartilhadas e, em seguida, fazer com que as classes `Instructor` e `Student` herdem dessa classe base, conforme mostrado na seguinte ilustração:

![Classes Student e Instructor derivando da classe Person](inheritance/_static/inheritance.png)

Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados. Você pode ter uma tabela Person que inclui informações sobre alunos e instrutores em uma única tabela. Algumas das colunas podem se aplicar somente a instrutores (HireDate), algumas somente a alunos (EnrollmentDate) e outras a ambos (LastName, FirstName). Normalmente, você terá uma coluna discriminatória para indicar qual tipo cada linha representa. Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.

![Exemplo de tabela por hierarquia](inheritance/_static/tph.png)

Esse padrão de geração de uma estrutura de herança de entidade com base em uma tabela de banco de dados individual é chamado de herança TPH (tabela por hierarquia).

Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança. Por exemplo, você pode ter apenas os campos de nome na tabela Pessoa e as tabelas separadas Instrutor e Aluno com os campos de data.

![Herança de tabela por tipo](inheritance/_static/tpt.png)

Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamado de herança TPT (tabela por tipo).

Outra opção é mapear todos os tipos não abstratos para tabelas individuais. Todas as propriedades de uma classe, incluindo propriedades herdadas, são mapeadas para colunas da tabela correspondente. Esse padrão é chamado de herança TPC (Tabela por Classe Concreta). Se você tiver implementado a herança TPC para as classes Person, Student e Instructor, conforme mostrado anteriormente, as tabelas Aluno e Instrutor não parecerão diferentes após a implementação da herança do que antes.

Em geral, os padrões de herança TPC e TPH oferecem melhor desempenho do que os padrões de herança TPT, porque os padrões TPT podem resultar em consultas de junção complexas.

Este tutorial demonstra como implementar a herança TPH. TPH é o único padrão de herança compatível com o Entity Framework Core.  O que você fará é criar uma classe `Person`, alterar as classes `Instructor` e `Student` para que elas derivem de `Person`, adicionar a nova classe ao `DbContext` e criar uma migração.

> [!TIP] 
> Considere a possibilidade de salvar uma cópia do projeto antes de fazer as alterações a seguir.  Em seguida, se você tiver problemas e precisar recomeçar, será mais fácil começar do projeto salvo, em vez de reverter as etapas executadas para este tutorial ou voltar ao início da série inteira.

## <a name="create-the-person-class"></a>Criar a classe Person

Na pasta Models, crie Person.cs e substitua o código de modelo pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Fazer com que as classes Student e Instructor herdem de Person

Em *Instructor.cs*, derive a classe Instructor da classe Person e remova os campos de nome e chave. O código será semelhante ao seguinte exemplo:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Faça as mesmas alterações em *Student.cs*.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Adicionar o tipo de entidade Person ao modelo de dados

Adicione o tipo de entidade Person a *SchoolContext.cs*. As novas linhas são realçadas.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia. Como você verá, quando o banco de dados for atualizado, ele terá uma tabela Pessoa no lugar das tabelas Aluno e Instrutor.

## <a name="create-and-customize-migration-code"></a>Criar e personalizar o código de migração

Salve as alterações e compile o projeto. Em seguida, abra a janela Comando na pasta do projeto e insira o seguinte comando:

```console
dotnet ef migrations add Inheritance
```

Não execute o comando `database update` ainda. Esse comando resultará em perda de dados porque ele removerá a tabela Instrutor e renomeará a tabela Aluno como Pessoa. Você precisa fornecer o código personalizado para preservar os dados existentes.

Abra *Migrations/\<timestamp>_Inheritance.cs* e substitua o método `Up` pelo seguinte código:

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Este código é responsável pelas seguintes tarefas de atualização de banco de dados:

* Remove as restrições de chave estrangeira e índices que apontam para a tabela Aluno.

* Renomeia a tabela Instrutor como Pessoa e faz as alterações necessárias para que ela armazene dados de Aluno:

* Adiciona EnrollmentDate que permite valo nulo para os alunos.

* Adiciona a coluna Discriminatória para indicar se uma linha refere-se a um aluno ou um instrutor.

* Faz com que HireDate permita valor nulo, pois linhas de alunos não terão datas de contratação.

* Adiciona um campo temporário que será usado para atualizar chaves estrangeiras que apontam para alunos. Quando você copiar os alunos para a tabela Person, eles receberão novos valores de chave primária.

* Copia os dados da tabela Aluno para a tabela Pessoa. Isso faz com que os alunos recebam novos valores de chave primária.

* Corrige valores de chave estrangeira que apontam para alunos.

* Recria restrições de chave estrangeira e índices, agora apontando-os para a tabela Person.

(Se você tiver usado o GUID, em vez de inteiro como o tipo de chave primária, os valores de chave primária dos alunos não precisarão ser alterados e várias dessas etapas poderão ser omitidas.)

Execute o comando `database update`:

```console
dotnet ef database update
```

(Em um sistema de produção, você fará as alterações correspondentes no método `Down`, caso já tenha usado isso para voltar à versão anterior do banco de dados. Para este tutorial, você não usará o método `Down`.)

> [!NOTE] 
> É possível receber outros erros ao fazer alterações de esquema em um banco de dados que contém dados existentes. Se você receber erros de migração que não consegue resolver, altere o nome do banco de dados na cadeia de conexão ou exclua o banco de dados. Com um novo banco de dados, não há nenhum dado a ser migrado e o comando de atualização de banco de dados terá uma probabilidade maior de ser concluído sem erros. Para excluir o banco de dados, use o SSOX ou execute o comando `database drop` da CLI.

## <a name="test-with-inheritance-implemented"></a>Testar com a herança implementada

Execute o aplicativo e teste várias páginas. Tudo funciona da mesma maneira que antes.

No **Pesquisador de Objetos do SQL Server**, expanda **Data Connections/SchoolContext** e, em seguida, **Tabelas** e você verá que as tabelas Aluno e Instrutor foram substituídas por uma tabela Pessoa. Abra o designer de tabela Pessoa e você verá que ela contém todas as colunas que costumavam estar nas tabelas Aluno e Instrutor.

![Tabela Person no SSOX](inheritance/_static/ssox-person-table.png)

Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.

![Tabela Person no SSOX – dados de tabela](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Resumo

Você implementou a herança de tabela por hierarquia para as classes `Person`, `Student` e `Instructor`. Para obter mais informações sobre a herança no Entity Framework Core, consulte [Herança](https://docs.microsoft.com/ef/core/modeling/inheritance). No próximo tutorial, você verá como lidar com uma variedade de cenários relativamente avançados do Entity Framework.

>[!div class="step-by-step"]
[Anterior](concurrency.md)
[Próximo](advanced.md)  
