---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementando a herança com o Entity Framework em um aplicativo ASP.NET MVC (8 de 10) | Microsoft Docs
author: tdykstra
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d3c3d19a62f601d007b0fac031ade1eef9f35771
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378930"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementando a herança com o Entity Framework em um aplicativo ASP.NET MVC (8 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais de início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece por aqui.
> 
> > [!NOTE] 
> > 
> > Se você enfrentar um problema que você não conseguir resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tente reproduzir o problema. Em geral, você pode encontrar a solução ao problema comparando seu código com o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


No tutorial anterior, você tratou exceções de simultaneidade. Este tutorial mostrará como implementar a herança no modelo de dados.

Na programação orientada a objeto, você pode usar a herança para eliminar código redundante. Neste tutorial, você alterará as classes `Instructor` e `Student`, de modo que elas derivem de uma classe base `Person` que contém propriedades, como `LastName`, comuns a instrutores e alunos. Você não adicionará nem alterará as páginas da Web, mas alterará uma parte do código, e essas alterações serão refletidas automaticamente no banco de dados.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabela por hierarquia versus a herança de tabela por tipo

Na programação orientada a objeto, você pode usar a herança para torná-lo mais fácil trabalhar com classes relacionadas. Por exemplo, o `Instructor` e `Student` as classes no `School` modelo de dados compartilham várias propriedades, que resulta em código redundante:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Suponha que você deseje eliminar o código redundante para as propriedades compartilhadas pelas entidades `Instructor` e `Student`. Você pode criar uma `Person` classe que contém apenas essas propriedades compartilhadas base, em seguida, fazer a `Instructor` e `Student` entidades herdam dessa classe base, conforme mostrado na ilustração a seguir:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Há várias maneiras pelas quais essa estrutura de herança pode ser representada no banco de dados. Você poderia ter um `Person` tabela que inclui informações sobre os alunos e instrutores em uma única tabela. Algumas das colunas pode aplicar somente a instrutores (`HireDate`), algumas somente a alunos (`EnrollmentDate`), alguns para ambos (`LastName`, `FirstName`). Normalmente, você teria uma *discriminador* coluna para indicar qual tipo cada linha representa. Por exemplo, a coluna discriminatória pode ter "Instrutor" para instrutores e "Aluno" para alunos.

![Tabela por hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Esse padrão de geração de uma estrutura de herança de entidade de uma tabela de banco de dados individual é chamado *tabela por hierarquia* herança (TPH).

Uma alternativa é fazer com que o banco de dados se pareça mais com a estrutura de herança. Por exemplo, você poderia ter apenas os campos de nome `Person` de tabela e ter separado `Instructor` e `Student` tabelas com os campos de data.

![Tabela por type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Esse padrão de criação de uma tabela de banco de dados para cada classe de entidade é chamada *tabela por tipo* herança (TPT).

Padrões de herança TPH oferecem melhor desempenho geralmente no Entity Framework que os padrões de herança TPT, porque os padrões TPT podem resultar em consultas de junção complexas. Este tutorial demonstra como implementar a herança TPH. Você terá de fazer isso executando as seguintes etapas:

- Criar uma `Person` de classe e altere o `Instructor` e `Student` classes sejam derivadas de `Person`.
- Adicione o código de mapeamento do modelo de banco de dados para a classe de contexto do banco de dados.
- Alteração `InstructorID` e `StudentID` referências em todo o projeto para `PersonID`.

## <a name="creating-the-person-class"></a>Criando a classe Person

 Observação: Você não será capaz de compilar o projeto depois de criar as classes abaixo até que você atualize os controladores que usa essas classes. 

No *modelos* pasta, crie *Person.cs* e substitua o código de modelo pelo código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Na *Instructor.cs*, derivar o `Instructor` classe o `Person` de classe e remova os campos nomes e chaves. O código será semelhante ao seguinte exemplo:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Fazer alterações semelhantes ao *Student.cs*. O `Student` classe se parecerá com o exemplo a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Adicionando o tipo de entidade Person ao modelo

Na *SchoolContext.cs*, adicione uma `DbSet` propriedade para o `Person` tipo de entidade:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Isso é tudo o que o Entity Framework precisa para configurar a herança de tabela por hierarquia. Como você verá, quando o banco de dados é recriado, ele terá um `Person` de tabela em vez do `Student` e `Instructor` tabelas.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Alterando InstructorID e StudentID para PersonID

Na *SchoolContext.cs*, na instrução de mapeamento de curso do instrutor, altere `MapRightKey("InstructorID")` para `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Essa alteração não é necessária. ela apenas altera o nome da coluna InstructorID na tabela de junção de muitos-para-muitos. Se você deixou o nome como InstructorID, o aplicativo ainda funciona corretamente. Aqui está concluído *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Em seguida, você precisará alterar `InstructorID` para `PersonID` e `StudentID` ao `PersonID` em todo o projeto ***exceto*** nos arquivos de migrações de carimbo de data / hora no *migrações* pasta. Para fazer isso você localizar e abrir apenas os arquivos que precisam ser alteradas e executar uma alteração global em arquivos abertos. O único arquivo na *migrações* pasta deve ser alterada é *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Comece fechando todos os arquivos abertos no Visual Studio.
2. Clique em **localizar e substituir – localizar todos os arquivos** na **editar** menu e, em seguida, pesquise todos os arquivos no projeto que contêm `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Abra cada arquivo na **Find Results** janela ***exceto*** o &lt;carimbo de data / hora&gt;*\_. CS* arquivos de migração na *Migrações* pasta, clicando duas vezes em uma linha para cada arquivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Abra o **substituir nos arquivos** caixa de diálogo e altere **examinar** para **todos os documentos abertos**.
5. Use o **substituir nos arquivos** caixa de diálogo para alterar todas as `InstructorID` para `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Localizar todos os arquivos no projeto que contêm `StudentID`.
7. Abra cada arquivo na **Find Results** janela ***exceto*** o &lt;carimbo de data / hora&gt;*\_\*. CS* arquivos de migração no *migrações* pasta, clicando duas vezes em uma linha para cada arquivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Abra o **substituir nos arquivos** caixa de diálogo e altere **examinar** para **todos os documentos abertos**.
9. Use o **substituir nos arquivos** caixa de diálogo para alterar todas `StudentID` para `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Compile o projeto.

(Observe que isso demonstra um *desvantagem* do `classnameID` padrão para chaves primárias de nomenclatura. Se você tivesse chamado ID, chaves primárias sem prefixar o nome de classe *nenhum* renomeando seria necessário agora.)

## <a name="create-and-update-a-migrations-file"></a>Criar e atualizar um arquivo de migrações

No pacote Manager Console (PMC), digite o seguinte comando:

`Add-Migration Inheritance`

Execute o `Update-Database` comando no PMC. O comando falhará neste momento porque temos os dados existentes que as migrações não sabe como tratar. Você obterá o seguinte erro:

*A instrução ALTER TABLE entrou em conflito com a restrição FOREIGN KEY "FK\_dbo. Departamento\_dbo. Pessoa\_PersonID ". O conflito ocorreu no banco de dados "ContosoUniversity" tabela "dbo. "Person", coluna 'PersonID'.*

Abra *migrações\&lt; carimbo de hora&gt;\_Inheritance.cs* e substitua o `Up` método com o código a seguir:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Execute o `update-database` comando novamente.

> [!NOTE]
> É possível receber outros erros durante a migração de dados e fazer alterações de esquema. Se você obtiver erros de migração não é possível resolver, você pode continuar com o tutorial, alterando a cadeia de conexão na *Web. config* arquivo ou excluir o banco de dados. A abordagem mais simples é renomear o banco de dados do *Web. config* arquivo. Por exemplo, altere o nome de banco de dados para o CU\_testar, conforme mostrado no exemplo a seguir:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Com um novo banco de dados, não há nenhum dado para migrar e o `update-database` comando é muito mais provável de ser concluído sem erros. Para obter instruções sobre como excluir o banco de dados, consulte [como descartar um banco de dados do Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se você usar essa abordagem para continuar com o tutorial, ignore a etapa de implantação no final deste tutorial, uma vez que o site implantado obteria o mesmo erro quando ele é automaticamente executado as migrações. Se você quiser solucionar o erro migrações, o melhor recurso é um dos fóruns do Entity Framework ou StackOverflow.com.


## <a name="testing"></a>Testes

Executar o site e tente várias páginas. Tudo funciona da mesma maneira que antes.

Na **Gerenciador de servidores** expandir **SchoolContext** e, em seguida, **tabelas**, e você verá que o **aluno** e **instrutor**  tabelas foram substituídas por um **pessoa** tabela. Expanda o **pessoa** tabela e você ver que ele tem todas as colunas que costumavam estar na **aluno** e **instrutor** tabelas.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Clique com o botão direito do mouse na tabela Person e, em seguida, clique em **Mostrar Dados da Tabela** para ver a coluna discriminatória.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

O diagrama a seguir ilustra a estrutura do novo banco de dados de escola:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Resumo

Herança de tabela por hierarquia agora foi implementada para o `Person`, `Student`, e `Instructor` classes. Para obter mais informações sobre esta e outras estruturas de herança, consulte [estratégias de mapeamento de herança](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) no blog de Morteza Manavi. O próximo tutorial, você verá algumas maneiras de implementar o repositório e unidade de padrões de trabalho.

Links para outros recursos do Entity Framework podem ser encontradas na [mapa de conteúdo de acesso do ASP.NET dados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Próximo](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
