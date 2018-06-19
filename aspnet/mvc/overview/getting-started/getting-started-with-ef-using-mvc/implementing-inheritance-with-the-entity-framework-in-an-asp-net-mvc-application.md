---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementando a herança com o Entity Framework 6 em um aplicativo ASP.NET MVC 5 (11 de 12) | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1826659626106993d4796641492c62fcbd22a1b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873398"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Implementando a herança com o Entity Framework 6 em um aplicativo ASP.NET MVC 5 (11 de 12)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


No tutorial anterior é tratada exceções de simultaneidade. Este tutorial mostrará como implementar a herança no modelo de dados.

Em programação orientada a objeto, você pode usar [herança](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) para facilitar a [reutilização de código](http://en.wikipedia.org/wiki/Code_reuse). Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos. Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opções para o mapeamento de herança para as tabelas de banco de dados

O `Instructor` e `Student` classes de `School` modelo de dados tem várias propriedades que são idênticas:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`. Ou você deseja escrever um serviço que pode formatar nomes sem se preocupar se o nome foi obtido de um instrutor ou um aluno. Você pode criar um `Person` classe que contém somente as propriedades compartilhadas base, em seguida, verifique o `Instructor` e `Student` entidades herdam da classe base, conforme mostrado na ilustração a seguir:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados. Você pode ter um `Person` tabela que inclui informações sobre os alunos e instrutores em uma única tabela. Algumas das colunas podem aplicar somente a instrutores (`HireDate`), alguns somente para os alunos (`EnrollmentDate`), alguns para ambos os (`LastName`, `FirstName`). Normalmente, você teria um *discriminador* coluna para indicar que tipo de cada linha representa. Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados único é chamado *tabela por hierarquia* herança (TPH).

Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança. Por exemplo, você pode ter apenas os campos de nome no `Person` de tabela e ter separado `Instructor` e `Student` tabelas com os campos de data.

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamado *tabela por tipo* herança (TPT).

Outra opção é mapear todos os tipos não abstratos para tabelas individuais. Todas as propriedades de uma classe, incluindo propriedades herdadas, são mapeadas para colunas da tabela correspondente. Esse padrão é chamado de herança TPC (Tabela por Classe Concreta). Se você tiver implementado herança TPC para o `Person`, `Student`, e `Instructor` classes como mostrado anteriormente, o `Student` e `Instructor` tabelas seria não diferentes depois de implementar a herança do que antes.

TPC e padrões de herança TPH geralmente oferecem um desempenho melhor no Entity Framework de padrões de herança TPT, como padrões TPT podem resultar em consultas de junção complexas.

Este tutorial demonstra como implementar a herança TPH. TPH é o padrão de herança no Entity Framework, portanto, tudo o que você precisa fazer é criar um `Person` classe, altere o `Instructor` e `Student` classes derivar `Person`, adicione a nova classe para o `DbContext`e criar um migração. (Para obter informações sobre como implementar os padrões de herança, consulte [mapeando a herança de tabela por tipo (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) e [mapeando a herança de classe de tabela por concreto (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) no MSDN Documentação de estrutura de entidade.)

## <a name="create-the-person-class"></a>Criar a classe Person

No *modelos* pasta, criar *Person.cs* e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Fazer com que as classes Student e Instructor herdem de Person

Em *Instructor.cs*, derivar o `Instructor` classe o `Person` classe e remover os campos de chave e nome. O código será semelhante ao seguinte exemplo:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Fazer alterações semelhantes para *Student.cs*. O `Student` classe se parecerá com o exemplo a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Adicione o tipo de entidade de pessoa para o modelo

Em *SchoolContext.cs*, adicione um `DbSet` propriedade para o `Person` tipo de entidade:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia. Como você verá, quando o banco de dados é atualizado, ele terá um `Person` tabela em vez do `Student` e `Instructor` tabelas.

## <a name="create-and-update-a-migrations-file"></a>Criar e atualizar um arquivo de migrações

No pacote Manager Console (PMC), digite o seguinte comando:

`Add-Migration Inheritance`

Execute o `Update-Database` do PMC. O comando falhará neste momento porque temos os dados existentes que migrações não sabe como tratar. Você receberá uma mensagem de erro semelhante à seguinte:

> *Não foi possível descartar o objeto ' dbo. Instrutor ' porque ele é referenciado por uma restrição FOREIGN KEY.*


Abra *migrações\&lt; carimbo de hora&gt;\_Inheritance.cs* e substitua o `Up` método com o código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Este código é responsável pelas seguintes tarefas de atualização de banco de dados:

- Remove as restrições de chave estrangeira e índices que apontam para a tabela Aluno.
- Renomeia a tabela Instrutor como Pessoa e faz as alterações necessárias para que ela armazene dados de Aluno:

    - Adiciona EnrollmentDate que permite valo nulo para os alunos.
    - Adiciona a coluna Discriminatória para indicar se uma linha refere-se a um aluno ou um instrutor.
    - Faz com que HireDate permita valor nulo, pois linhas de alunos não terão datas de contratação.
    - Adiciona um campo temporário que será usado para atualizar chaves estrangeiras que apontam para alunos. Quando você copia os alunos para a tabela pessoa que receberão novos valores de chave primária.
- Copia os dados da tabela Aluno para a tabela Pessoa. Isso faz com que os alunos recebam novos valores de chave primária.
- Corrige valores de chave estrangeira que apontam para alunos.
- Recria restrições de chave estrangeira e índices, agora apontando-os para a tabela Person.

(Se você tiver usado o GUID, em vez de inteiro como o tipo de chave primária, os valores de chave primária dos alunos não precisarão ser alterados e várias dessas etapas poderão ser omitidas.)

Execute o `update-database` comando novamente.

(Em um sistema de produção faça as alterações correspondentes para o método de busca no caso de você teve de usá-la para voltar para a versão anterior do banco de dados. Neste tutorial você não irá usar o método de busca.)

> [!NOTE]
> É possível obter outros erros durante a migração de dados e fazer alterações de esquema. Se você obtiver erros de migração não é possível resolver, você pode continuar com o tutorial, alterando a cadeia de conexão do *Web. config* de arquivos ou por excluir o banco de dados. A abordagem mais simples é renomear o banco de dados no *Web. config* arquivo. Por exemplo, altere o nome do banco de dados para ContosoUniversity2 conforme mostrado no exemplo a seguir:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Com um novo banco de dados, não há nenhum dado para migrar e o `update-database` comando é muito mais probabilidade de ser concluído sem erros. Para obter instruções sobre como excluir o banco de dados, consulte [como descartar um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se você usar essa abordagem para continuar com o tutorial, ignore a etapa da implantação no final deste tutorial ou implantar um novo site e o banco de dados. Se você implantar uma atualização no mesmo site que você tenha sido Implantando já, EF receberá o mesmo erro existe quando ele é automaticamente executado migrações. Se você quiser solucionar o erro migrações, o melhor recurso é um dos fóruns do Entity Framework ou StackOverflow.com.


## <a name="testing"></a>Testes

Executar o site e tente várias páginas. Tudo funciona da mesma maneira que antes.

Em **Gerenciador de servidores,** expanda **dados Connections\SchoolContext** e, em seguida, **tabelas**, e você verá que o **aluno** e **Instrutor** tabelas foram substituídas por um **pessoa** tabela. Expanda o **pessoa** tabela e ver que tem todas as colunas que existiam no **aluno** e **instrutor** tabelas.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

O diagrama a seguir ilustra a estrutura do novo banco de dados de escola:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Implantar no Azure

Esta seção requer que você concluiu o opcional **implantar o aplicativo no Azure** seção [parte 3, classificação, filtragem e paginação](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) da série neste tutorial. Se você tiver erros de migrações resolvido excluindo o banco de dados em seu projeto local, ignore esta etapa; ou crie um novo site e o banco de dados e implantar para o novo ambiente.

1. No Visual Studio, clique com botão direito no projeto no **Solution Explorer** e selecione **publicar** no menu de contexto.  
  
    ![Publicar no menu de contexto do projeto](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Clique em **Publicar**.  
  
    ![publicar](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   O aplicativo Web será aberto no navegador padrão.
3. Testar o aplicativo para verificar se ela está funcionando.

    Na primeira vez que você executa uma página que acessa o banco de dados, o Entity Framework executa todas as migrações `Up` métodos necessários para colocar o banco de dados atualizado com o modelo de dados atual.

## <a name="summary"></a>Resumo

Você implementou a herança de tabela por hierarquia para as classes `Person`, `Student` e `Instructor`. Para obter mais informações sobre essa e outras estruturas de herança, consulte [padrão de herança TPT](https://msdn.microsoft.com/data/jj618293) e [padrão de herança TPH](https://msdn.microsoft.com/data/jj618292) no MSDN. No próximo tutorial, você verá como lidar com uma variedade de cenários relativamente avançados do Entity Framework.

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Próximo](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
