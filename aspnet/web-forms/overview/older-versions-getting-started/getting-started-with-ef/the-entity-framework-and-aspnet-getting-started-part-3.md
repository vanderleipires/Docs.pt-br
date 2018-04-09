---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Introdução ao banco de dados do Entity Framework 4.0 primeiro e o ASP.NET 4 Web Forms - parte 3 | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework. O aplicativo de exemplo é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Introdução ao banco de dados do Entity Framework 4.0 primeiro e 4 Web Forms do ASP.NET - parte 3
====================
por [Tom Dykstra](https://github.com/tdykstra)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos Web Forms do ASP.NET usando o Entity Framework 4.0 e o Visual Studio 2010. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial da série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtragem, classificação e agrupamento de dados

No tutorial anterior, você usou o `EntityDataSource` controle para exibir e editar dados. Neste tutorial você filtrar, ordenar e agrupar dados. Quando você fizer isso, definindo propriedades do `EntityDataSource` controle, a sintaxe é diferente de outros controles de fonte de dados. Como você pode ver, no entanto, você pode usar o `QueryExtender` controle para minimizar essas diferenças.

Você alterará a *Students.aspx* página Filtrar para estudantes, classifique por nome e a pesquisa de nome. Você também alterará o *Courses.aspx* página para exibir o cursos para o departamento e pesquisa de cursos por nome. Por fim, você adicionará as estatísticas de aluno para o *About* página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Usando a propriedade "Where" de EntityDataSource para filtrar dados

Abra o *Students.aspx* página que você criou no tutorial anterior. Como configurado, o `GridView` controle na página exibe todos os nomes da `People` conjunto de entidades. No entanto, você deseja mostrar apenas os alunos, que você pode encontrar selecionando `Person` entidades que têm datas de registro não nulo.

Alternar para **Design** exibir e selecionar o `EntityDataSource` controle. No **propriedades** janela, defina o `Where` propriedade `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

A sintaxe que você use o `Where` propriedade do `EntityDataSource` controle é Entity SQL. Entidade SQL é semelhante ao Transact-SQL, mas ele é personalizado para uso com entidades em vez de objetos de banco de dados. Na expressão `it.EnrollmentDate is not null`, a palavra `it` representa uma referência à entidade retornada pela consulta. Portanto, `it.EnrollmentDate` refere-se ao `EnrollmentDate` propriedade o `Person` entidade que o `EntityDataSource` controlar retorna.

Execute a página. A lista de alunos agora contém apenas os alunos. (Não existem linhas exibidas em que há uma data de registro.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Usando a propriedade de "OrderBy" EntityDataSource para dados de pedidos

Você deseja que essa lista para estar em ordem de nome quando ele for exibido primeiro. Com o *Students.aspx* página ainda está aberta no **Design** exibição e com o `EntityDataSource` controle ainda selecionado, no **propriedades** janela conjunto o  **OrderBy** propriedade `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Execute a página. A lista de alunos agora está na ordem por sobrenome.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Usando um parâmetro de controle para definir a propriedade "Where"

Como com outros controles de fonte de dados, você pode passar valores de parâmetro para o `Where` propriedade. Sobre o *Courses.aspx* página que você criou na parte 2 do tutorial, você pode usar esse método para exibir os cursos que estão associados com o departamento de que um usuário seleciona na lista suspensa.

Abra *Courses.aspx* e alterne para o **Design** exibição. Adicionar um segundo `EntityDataSource` controlar para a página e o nome de `CoursesEntityDataSource`. Conectá-lo para o `SchoolEntities` de modelo e selecione `Courses` como o **EntitySetName** valor.

No **propriedades** janela, clique no botão de reticências no **onde** caixa da propriedade. (Verifique se o `CoursesEntityDataSource` controle ainda está selecionado antes de usar o **propriedades** janela.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

O **Editor de expressão** caixa de diálogo é exibida. Na caixa de diálogo, selecione **gerar automaticamente onde expressão com base nos parâmetros fornecidos**e, em seguida, clique em **Adicionar parâmetro**. Nome do parâmetro `DepartmentID`, selecione **controle** como o **origem do parâmetro** valor e, em seguida, selecione **DepartmentsDropDownList** como o **ControlID**  valor.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Clique em **Mostrar propriedades avançadas**e no **propriedades** janela do **Editor de expressão** caixa de diálogo, altere o `Type` propriedade `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Quando terminar, clique em **Okey**.

Abaixo da lista suspensa, adicione um `GridView` controlar para a página e nomeie-o `CoursesGridView`. Conectá-lo ao `CoursesEntityDataSource` fonte de dados de controle, clique em **atualizar esquema**, clique em **Editar colunas**e remover o `DepartmentID` coluna. O `GridView` o exemplo a seguir é semelhante a marcação de controle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Quando o usuário altera o departamento selecionado na lista suspensa, você deseja que a lista de cursos associados para alterar automaticamente. Para fazer isso acontecer, selecione a lista suspensa e no **propriedades** janela conjunto o `AutoPostBack` propriedade `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Agora que você terminar de usar o designer, alterne para **fonte** exibir e substitua o `ConnectionString` e `DefaultContainer` propriedades de nomes de `CoursesEntityDataSource` controlar com o `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. Quando terminar, a marcação para o controle parecerá com o exemplo a seguir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Execute a página e use a lista suspensa para selecionar diferentes departamentos. Somente os cursos que são oferecidos por departamento selecionado são exibidos no `GridView` controle.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Usando a propriedade de "GroupBy" EntityDataSource para agrupar dados

Suponha que a Contoso University deseja colocar algumas estatísticas aluno corpo em sua página sobre. Especificamente, ele deseja mostrar uma divisão de um número de alunos pela data em que eles registrados.

Abra *About*e, em **fonte** exibir, substitua o conteúdo existente do `BodyContent` controlar com "Estatísticas de corpo de estudante" entre `h2` marcas:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Após o título, adicione um `EntityDataSource` controlar e nomeie-o `StudentStatisticsEntityDataSource`. Conectá-lo ao `SchoolEntities`, selecione o `People` entidade definida e deixe o **selecione** caixa no assistente inalterado. Defina as seguintes propriedades **propriedades** janela:

- Para filtrar apenas para alunos, defina o `Where` propriedade `it.EnrollmentDate is not null`.
- Para agrupar os resultados por data de inscrição, defina o `GroupBy` propriedade `it.EnrollmentDate`.
- Para selecionar a data de inscrição e o número de alunos, defina o `Select` propriedade `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Para ordenar os resultados na data de registro, defina o `OrderBy` propriedade `it.EnrollmentDate`.

Em **fonte** exibir, substitua o `ConnectionString` e `DefaultContainer` propriedades com nomes um `ContextTypeName` propriedade. O `EntityDataSource` marcação de controle agora se parece com o exemplo a seguir.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

A sintaxe do `Select`, `GroupBy`, e `Where` propriedades se parece com o Transact-SQL, exceto para o `it` palavra-chave que especifica a entidade atual.

Adicione a seguinte marcação para criar um `GridView` controle para exibir os dados.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Execute a página para ver uma lista mostrando o número de alunos por data de inscrição.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Usando o controle de QueryExtender para filtragem e classificação

O `QueryExtender` controle fornece uma maneira de especificar filtrando e classificando na marcação. A sintaxe é independente do sistema de gerenciamento de banco de dados (DBMS) você está usando. Também é geralmente independente do Entity Framework, com exceção de que a sintaxe usada para propriedades de navegação é exclusivo para o Entity Framework.

Esta parte do tutorial, você usará um `QueryExtender` controle de filtro e a classificação de dados e um dos campos de order by sejam uma propriedade de navegação.

(Se você preferir usar código em vez de marcações para estender as consultas que são geradas automaticamente pelo `EntityDataSource` controle, você pode fazer isso tratar o `QueryCreated` evento. Isso é como o `QueryExtender` controle estende `EntityDataSource` controlar consultas também.)

Abra o *Courses.aspx* página e abaixo a marcação que você adicionou anteriormente, insira a seguinte marcação para criar um título, uma caixa de texto para digitar cadeias de caracteres de pesquisa, um botão de pesquisa e um `EntityDataSource` controle associado para o `Courses` conjunto de entidades.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Observe que o `EntityDataSource` do controle `Include` está definida como `Department`. No banco de dados, o `Course` a tabela não contém o nome do departamento; ele contém um `DepartmentID` coluna de chave estrangeira. Se foram consultando o banco de dados diretamente, para obter o nome do departamento junto com dados de curso, você teria que unir o `Course` e `Department` tabelas. Definindo o `Include` propriedade `Department`, você especificar que o Entity Framework devem fazer o trabalho de obtenção relacionado `Department` entidade quando ele recebe um `Course` entidade. O `Department` entidade é armazenada no `Department` propriedade de navegação a `Course` entidade. (Por padrão, o `SchoolEntities` classe que foi gerado pelo designer de modelo de dados recupera dados relacionados quando for necessário e você tiver associado o controle de fonte de dados para essa classe, portanto, a definição de `Include` propriedade não é necessária. No entanto, configurá-la melhora o desempenho da página, pois caso contrário, o Entity Framework tornaria separar chamadas para o banco de dados para recuperar dados para o `Course` entidades e para relacionado `Department` entidades.)

Após o `EntityDataSource` controle apenas que você criou, inserir a seguinte marcação para criar um `QueryExtender` controle associado ao `EntityDataSource` controle.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

O `SearchExpression` elemento Especifica que você deseja selecionar cursos cujos títulos correspondam ao valor inserido na caixa de texto. Somente um número de caracteres são inseridos na caixa de texto será comparada, porque o `SearchType` propriedade especifica `StartsWith`.

O `OrderByExpression` elemento Especifica que o conjunto de resultados será ordenado por título de curso em nome de departamento. Observe como o nome do departamento é especificado: `Department.Name`. Porque a associação entre o `Course` entidade e o `Department` entidade é um para um, o `Department` propriedade de navegação contém um `Department` entidade. (Se esse fosse uma relação um-para-muitos, a propriedade conterá uma coleção.) Para obter o nome do departamento, você deve especificar o `Name` propriedade o `Department` entidade.

Finalmente, adicione um `GridView` controle para exibir a lista de cursos:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

A primeira coluna é um campo de modelo que exibe o nome do departamento. Especifica a expressão de associação de dados `Department.Name`, assim como você viu no `QueryExtender` controle.

Execute a página. A exibição inicial mostra uma lista de todos os cursos em ordem por departamento e, em seguida, por título do curso.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Insira um "m" e clique em **pesquisa** para ver todos os cursos cujos títulos começam com "m" (a pesquisa não é maiusculas e minúsculas).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Usando o operador "Como" para filtrar dados

Você pode obter um efeito semelhante a de `QueryExtender` do controle `StartsWith`, `Contains`, e `EndsWith` pesquisar tipos usando um `Like` operador no `EntityDataSource` do controle `Where` propriedade. Nesta parte do tutorial, você verá como usar o `Like` operador para pesquisar um aluno por nome.

Abra *Students.aspx* na **fonte** exibição. Após o `GridView` controle, adicione a seguinte marcação:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Essa marcação é semelhante ao que foi visto anteriormente exceto para o `Where` o valor da propriedade. A segunda parte do `Where` expressão define uma pesquisa de subcadeia de caracteres (`LIKE %FirstMidName% or LIKE %LastName%`) que procura nomes e sobrenomes para que for inserido na caixa de texto.

Execute a página. Inicialmente você verá todos os alunos porque o valor padrão para o `StudentName` parâmetro é "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Digite a letra "g" na caixa de texto e clique em **pesquisa**. Você verá uma lista de alunos que têm "g" em ambos o nome ou sobrenome.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Você agora exibido, atualizado, filtrados, ordenados e dados de tabelas individuais agrupados. O seguinte tutorial, você começará a trabalhar com dados relacionados (cenários de detalhes mestre).

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Próximo](the-entity-framework-and-aspnet-getting-started-part-4.md)
