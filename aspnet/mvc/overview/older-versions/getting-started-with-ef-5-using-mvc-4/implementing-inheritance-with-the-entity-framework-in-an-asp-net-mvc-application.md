---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementando a herança com o Entity Framework em um aplicativo ASP.NET MVC (8 a 10) | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873997"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementando a herança com o Entity Framework em um aplicativo ASP.NET MVC (8 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais do início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece aqui.
> 
> > [!NOTE] 
> > 
> > Se você tiver um problema que não é possível resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tentar reproduzir o problema. Geralmente, você pode encontrar a solução para o problema, comparando o seu código para o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


No tutorial anterior é tratada exceções de simultaneidade. Este tutorial mostrará como implementar a herança no modelo de dados.

Em programação orientada a objeto, você pode usar a herança para eliminar código redundante. Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos. Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabela por hierarquia e herança de tabela por tipo

Em programação orientada a objeto, você pode usar a herança para tornar mais fácil trabalhar com classes relacionadas. Por exemplo, o `Instructor` e `Student` classes de `School` modelo de dados compartilham várias propriedades, o que resulta em código redundante:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`. Você pode criar um `Person` classe que contém somente as propriedades compartilhadas base, em seguida, verifique o `Instructor` e `Student` entidades herdam da classe base, conforme mostrado na ilustração a seguir:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados. Você pode ter um `Person` tabela que inclui informações sobre os alunos e instrutores em uma única tabela. Algumas das colunas podem aplicar somente a instrutores (`HireDate`), alguns somente para os alunos (`EnrollmentDate`), alguns para ambos os (`LastName`, `FirstName`). Normalmente, você teria um *discriminador* coluna para indicar que tipo de cada linha representa. Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.

![Table-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados único é chamado *tabela por hierarquia* herança (TPH).

Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança. Por exemplo, você pode ter apenas os campos de nome no `Person` de tabela e ter separado `Instructor` e `Student` tabelas com os campos de data.

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamado *tabela por tipo* herança (TPT).

Padrões de herança TPH geralmente fornecem melhor desempenho no Entity Framework de padrões de herança TPT, como padrões TPT podem resultar em consultas de junção complexas. Este tutorial demonstra como implementar a herança TPH. Você fará isso executando as seguintes etapas:

- Criar um `Person` classe e altere o `Instructor` e `Student` classes derivar `Person`.
- Adicione o código de mapeamento de modelo de banco de dados para a classe de contexto de banco de dados.
- Alterar `InstructorID` e `StudentID` referências em todo o projeto para `PersonID`.

## <a name="creating-the-person-class"></a>Criando a classe pessoa

 Observação: Você não poderá compilar o projeto depois de criar as classes abaixo até que você atualize os controladores que usa essas classes. 

No *modelos* pasta, criar *Person.cs* e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Em *Instructor.cs*, derivar o `Instructor` classe o `Person` classe e remover os campos de chave e nome. O código será semelhante ao seguinte exemplo:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Fazer alterações semelhantes para *Student.cs*. O `Student` classe se parecerá com o exemplo a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Adicionando o tipo de entidade de pessoa ao modelo

Em *SchoolContext.cs*, adicione um `DbSet` propriedade para o `Person` tipo de entidade:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia. Como você verá, quando o banco de dados é recriado, ele terá um `Person` tabela em vez do `Student` e `Instructor` tabelas.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Alteração InstructorID e StudentID PersonID

Em *SchoolContext.cs*, a instrução de mapeamento do instrutor curso, alterar `MapRightKey("InstructorID")` para `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Essa alteração não é necessária. Ele apenas altera o nome da coluna InstructorID na tabela de junção de muitos-para-muitos. Se você deixar o nome como InstructorID, o aplicativo ainda funciona corretamente. Aqui está concluído *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Em seguida, você precisa alterar `InstructorID` para `PersonID` e `StudentID` para `PersonID` em todo o projeto ***exceto*** nos arquivos de migrações com carimbo de hora no *migrações* pasta. Para fazer isso você localizar e abrir apenas os arquivos que precisam ser alteradas e executar uma alteração global em arquivos abertos. O único arquivo no *migrações* é de pasta, você deve alterar *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Comece fechando todos os arquivos abertos no Visual Studio.
2. Clique em **localizar e substituir – localizar todos os arquivos** no **editar** menu e, em seguida, pesquise a todos os arquivos no projeto que contém `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Abrir cada arquivo a **localizar resultados** janela ***exceto*** o &lt;carimbo de data / hora&gt;*\_. CS* arquivos de migração no *Migrações* pasta, clicando duas vezes em uma linha para cada arquivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Abra o **substituir nos arquivos** caixa de diálogo e alterar **examinar** para **todos os documentos abertos**.
5. Use o **substituir nos arquivos** caixa de diálogo para alterar todas as `InstructorID` para `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Localizar todos os arquivos no projeto que contém `StudentID`.
7. Abra cada arquivo a **localizar resultados** janela ***exceto*** o &lt;carimbo de data / hora&gt;*\_\*. CS* os arquivos de migração no *migrações* pasta, clicando duas vezes em uma linha para cada arquivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Abra o **substituir nos arquivos** caixa de diálogo e alterar **examinar** para **todos os documentos abertos**.
9. Use o **substituir nos arquivos** caixa de diálogo para alterar todas as `StudentID` para `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Compile o projeto.

(Observe que isso demonstra um *desvantagem* do `classnameID` padrão de nomeação de chaves primárias. Se você tiver chaves primárias ID sem o prefixo de nome de classe, *sem* renomeando seria necessário agora.)

## <a name="create-and-update-a-migrations-file"></a>Criar e atualizar um arquivo de migrações

No pacote Manager Console (PMC), digite o seguinte comando:

`Add-Migration Inheritance`

Execute o `Update-Database` do PMC. O comando falhará neste momento porque temos os dados existentes que migrações não sabe como tratar. Você obterá o seguinte erro:

*A instrução ALTER TABLE em conflito com a restrição de chave estrangeira "FK\_dbo. Departamento\_dbo. Pessoa\_PersonID ". O conflito ocorreu no banco de dados "ContosoUniversity", tabela "dbo. Pessoa", coluna 'PersonID'.*

Abra *migrações\&lt; carimbo de hora&gt;\_Inheritance.cs* e substitua o `Up` método com o código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Execute o `update-database` comando novamente.

> [!NOTE]
> É possível obter outros erros durante a migração de dados e fazer alterações de esquema. Se você obtiver erros de migração não é possível resolver, você pode continuar com o tutorial, alterando a cadeia de conexão do *Web. config* arquivo ou excluir o banco de dados. A abordagem mais simples é renomear o banco de dados no *Web. config* arquivo. Por exemplo, altere o nome de banco de dados para atualização Cumulativa\_testar como mostrado no exemplo a seguir:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Com um novo banco de dados, não há nenhum dado para migrar e o `update-database` comando é muito mais probabilidade de ser concluído sem erros. Para obter instruções sobre como excluir o banco de dados, consulte [como descartar um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se você usar essa abordagem para continuar com o tutorial, ignore a etapa de implantação no final deste tutorial, pois o site implantado obterá o mesmo erro ao executar migrações automaticamente. Se você quiser solucionar o erro migrações, o melhor recurso é um dos fóruns do Entity Framework ou StackOverflow.com.


## <a name="testing"></a>Testes

Executar o site e tente várias páginas. Tudo funciona da mesma maneira que antes.

Em **Gerenciador de servidores,** expanda **SchoolContext** e **tabelas**, e você verá que o **aluno** e **instrutor**  tabelas foram substituídas por um **pessoa** tabela. Expanda o **pessoa** tabela e ver que tem todas as colunas que existiam no **aluno** e **instrutor** tabelas.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

O diagrama a seguir ilustra a estrutura do novo banco de dados de escola:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Resumo

Herança de tabela por hierarquia agora foi implementada para o `Person`, `Student`, e `Instructor` classes. Para obter mais informações sobre essa e outras estruturas de herança, consulte [estratégias de mapeamento de herança](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) no blog de Morteza Manavi. O seguinte tutorial, você verá algumas maneiras de implementar o repositório e a unidade de trabalho padrões.

Links para outros recursos do Entity Framework podem ser encontradas no [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Próximo](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
