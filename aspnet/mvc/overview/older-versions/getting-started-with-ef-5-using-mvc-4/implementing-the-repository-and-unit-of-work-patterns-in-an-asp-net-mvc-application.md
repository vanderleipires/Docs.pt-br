---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementando o repositório e a unidade de trabalho padrões em um aplicativo ASP.NET MVC (9 de 10) | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f870b61658686769304a7809bde62e66da3bd0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementando o repositório e a unidade de trabalho padrões em um aplicativo ASP.NET MVC (9 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Entity Framework 5 Code First e o Visual Studio 2012. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Você pode iniciar a série de tutoriais do início ou [baixar um projeto inicial para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) e comece aqui.
> 
> > [!NOTE] 
> > 
> > Se você tiver um problema que não é possível resolver, [baixar o capítulo concluído](building-the-ef5-mvc4-chapter-downloads.md) e tentar reproduzir o problema. Geralmente, você pode encontrar a solução para o problema, comparando o seu código para o código completo. Para alguns erros comuns e como resolvê-los, consulte [erros e soluções alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


No tutorial anterior, você usou herança para reduzir o código redundante no `Student` e `Instructor` classes de entidade. Neste tutorial, você verá algumas maneiras de usar o repositório e a unidade de trabalho padrões para as operações CRUD. Como o tutorial anterior, este você alterará a maneira como o código funciona com páginas você já criados em vez de criar novas páginas.

## <a name="the-repository-and-unit-of-work-patterns"></a>O repositório e a unidade de trabalho padrões

O repositório e a unidade de trabalho padrões destinam-se para criar uma camada de abstração entre a camada de acesso a dados e a camada de lógica comercial de um aplicativo. A implementação desses padrões pode ajudar a isolar o aplicativo de alterações no armazenamento de dados e pode facilitar o teste de unidade automatizado ou TDD (desenvolvimento orientado por testes).

Neste tutorial, você poderá implementar uma classe de repositório para cada tipo de entidade. Para o `Student` tipo de entidade, você criará uma interface do repositório e uma classe de repositório. Quando você cria uma instância do repositório em seu controlador, você usará a interface para que o controlador aceitará uma referência a qualquer objeto que implementa a interface do repositório. Quando o controlador é executado em um servidor web, ele recebe um repositório que funciona com o Entity Framework. Quando o controlador é executado em uma classe de teste de unidade, ele recebe um repositório que funciona com dados armazenados em uma forma que você pode manipular facilmente para teste, como uma coleção de memória.

Posteriormente no tutorial você usará vários repositórios e uma classe de unidade de trabalho para o `Course` e `Department` tipos de entidade no `Course` controlador. A classe de unidade de trabalho coordena o trabalho de vários repositórios, criando uma classe de contexto de único banco de dados compartilhada por todos eles. Se você quiser ser capaz de executar testes de unidade automatizados, crie e use interfaces para essas classes da mesma maneira que você fez para o `Student` repositório. No entanto, para simplificar o tutorial, você criará e use essas classes sem interfaces.

A ilustração a seguir mostra uma maneira de Conceituar as relações entre o controlador e classes de contexto comparados a não usar o repositório ou unidade de trabalho padrão em todos os.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Você não criar testes de unidade em série neste tutorial. Para obter uma introdução para TDD com um aplicativo MVC que usa o padrão de repositório, consulte [passo a passo: usando TDD com o ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Para obter mais informações sobre o padrão de repositório, consulte os seguintes recursos:

- [O padrão de repositório](https://msdn.microsoft.com/library/ff649690.aspx) no MSDN.
- [Usar padrões de repositório e unidade de trabalho com o Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) no blog da equipe do Entity Framework.
- [Agile Entity Framework 4 repositório](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) série de postagens no blog de Julie Lerman.
- [Criando a conta em um aplicativo de HTML5/jQuery rapidamente](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) no blog de Wahlin.

> [!NOTE]
> Há muitas maneiras de implementar o repositório e a unidade de trabalho padrões. Você pode usar classes de repositório com ou sem uma classe de unidade de trabalho. Você pode implementar um único repositório para todos os tipos de entidade ou um para cada tipo. Se você implementar um para cada tipo, você pode usar classes separadas, uma classe genérica de base e classes derivadas, ou uma classe base abstrata e classes derivadas. Você pode incluir a lógica de negócios em seu repositório ou restringi-la a lógica de acesso a dados. Você também pode criar uma camada de abstração em sua classe de contexto de banco de dados usando [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) interfaces existe em vez de [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) tipos para os conjuntos de entidade. A abordagem para implementar uma camada de abstração mostrada neste tutorial é uma opção para você, não uma recomendação para todos os cenários e ambientes.


## <a name="creating-the-student-repository-class"></a>Criando a classe de repositório do aluno

No *DAL* pasta, crie um arquivo de classe chamado *IStudentRepository.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Esse código declara um conjunto típico de métodos CRUD, incluindo dois métodos de leitura — uma que retorna todos os `Student` entidades e que localiza um único `Student` entidade por ID.

No *DAL* pasta, crie um arquivo de classe chamado *StudentRepository.cs* arquivo. Substitua o código existente pelo código a seguir, que implementa o `IStudentRepository` interface:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

O contexto do banco de dados é definido em uma variável de classe e o construtor espera o objeto de chamada para passar uma instância do contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Você pode instanciar um novo contexto no repositório, mas se você usou vários repositórios em um controlador, cada um deve terminar com um contexto separado. Mais tarde você usará vários repositórios de `Course` controlador e você verá como uma classe de unidade de trabalho pode assegurar que todos os repositórios usam o mesmo contexto.

Implementa o repositório [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) e descarta o contexto do banco de dados, como você viu anteriormente no controlador e seus métodos CRUD fazem chamadas para o contexto do banco de dados da mesma forma que você viu anteriormente.

## <a name="change-the-student-controller-to-use-the-repository"></a>Altere o controlador de estudante para usar o repositório

Em *StudentController.cs*, substitua o código no momento na classe com o código a seguir. As alterações são realçadas.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

O controlador agora declara uma variável de classe para um objeto que implementa o `IStudentRepository` interface em vez da classe de contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

O construtor (sem parâmetros) padrão cria uma nova instância de contexto e um construtor opcional permite que o chamador passar uma instância de contexto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Se você estivesse usando *injeção de dependência*, ou injeção de dependência, não será necessário o construtor padrão porque o software de DI garante que o objeto de repositório correto sempre seria fornecido.)

Em métodos CRUD, o repositório agora é chamado em vez do contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

E o `Dispose` método agora descarta o repositório em vez de contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Executar o site e clique no **alunos** guia.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

A página de pesquisa e funciona da mesma forma que tinha antes de você alterou o código para usar o repositório e as outras páginas do aluno também funcionam da mesma. No entanto, há uma diferença importante no modo como o `Index` método do controlador faz a filtragem e classificação. A versão original deste método continha o código a seguir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

A atualização `Index` método contém o código a seguir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Somente o código foi alterado.

Na versão original do código, `students` é digitada como um `IQueryable` objeto. A consulta não será enviada para o banco de dados até que ele é convertido em uma coleção usando um método como `ToList`, que não ocorre até que a exibição índice acessa o modelo de student. O `Where` método no código acima original torna-se um `WHERE` cláusula na consulta SQL que é enviada para o banco de dados. Por sua vez, isso significa que apenas as entidades selecionadas são retornadas pelo banco de dados. No entanto, como resultado da alteração `context.Students` para `studentRepository.GetStudents()`, o `students` variável depois que essa instrução é um `IEnumerable` coleção que inclui todos os alunos no banco de dados. O resultado final da aplicação de `Where` método é o mesmo, mas agora o trabalho é feito na memória no servidor web e não pelo banco de dados. Para consultas que retornam grandes volumes de dados, isso pode ser ineficiente.

> [!TIP]
> 
> **IQueryable vs. IEnumerable**
> 
> Depois de implementar o repositório como mostrado aqui, mesmo se você digitar algo no **pesquisa** caixa a consulta enviada para o SQL Server retorna todas as linhas de estudante porque ela não inclui os critérios de pesquisa:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Esta consulta retorna todos os dados de estudante porque o repositório de execução da consulta sem saber sobre os critérios de pesquisa. O processo de classificação, aplicar os critérios de pesquisa e selecionar um subconjunto dos dados para paginação (mostrando apenas 3 linhas neste caso) é feita na memória posteriormente, quando o `ToPagedList` método é chamado no `IEnumerable` coleção.
> 
> Na versão anterior do código (antes da implementação do repositório), a consulta não é enviada ao banco de dados até depois de aplicar os critérios de pesquisa quando `ToPagedList` é chamado de `IQueryable` objeto.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Quando ToPagedList é chamado em um `IQueryable` do objeto, a consulta enviada para o SQL Server Especifica a cadeia de caracteres de pesquisa, como resultado somente as linhas que atendem aos critérios de pesquisa são retornadas e nenhuma filtragem precisa ser feito na memória.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (O tutorial a seguir explica como examinar as consultas enviadas ao SQL Server.)


A seção a seguir mostra como implementar os métodos de repositório que permitem que você especifique que o trabalho deve ser feito pelo banco de dados.

Agora você criou uma camada de abstração entre o controlador e o contexto do banco de dados do Entity Framework. Se você for executar automatizada testes de unidade com este aplicativo, você pode criar uma classe de repositório alternativo em um projeto de teste de unidade que implementa `IStudentRepository` *.* Em vez de chamar o contexto para ler e gravar dados, essa classe de repositório de simulação pode manipular coleções na memória para funções de controlador de teste.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementar um repositório genérico e uma classe de unidade de trabalho

Criando uma classe de repositório para cada tipo de entidade pode resultar em uma grande quantidade de códigos redundantes, e isso poderia resultar em atualizações parciais. Por exemplo, suponha que você precisa atualizar os dois tipos de entidade diferente como parte da mesma transação. Se cada uma usa uma instância de contexto do banco de dados separado, um pode ter êxito e o outro poderá falhar. Uma forma de minimizar a redundância código é usar um repositório genérico e uma maneira de garantir que todos os repositórios usam o mesmo contexto de banco de dados (e, portanto, todas as atualizações de coordenadas) são usar uma classe de unidade de trabalho.

Nesta seção do tutorial, você criará uma `GenericRepository` classe e um `UnitOfWork` classe e usá-los no `Course` controlador para acessar o `Department` e `Course` conjuntos de entidades. Conforme explicado anteriormente, para simplificar a essa parte do tutorial, você não está criando interfaces para essas classes. Mas se você for usá-las para facilitar TDD, normalmente implementaria com interfaces da mesma maneira que você fez o `Student` repositório.

### <a name="create-a-generic-repository"></a>Criar um repositório genérico

No *DAL* pasta, criar *GenericRepository.cs* e substitua o código existente pelo seguinte código:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Variáveis de classe são declaradas para o contexto do banco de dados e para o conjunto de entidades que o repositório é instanciado para:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

O construtor aceita uma instância de contexto do banco de dados e inicializa a variável de conjunto de entidade:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

O `Get` método usa expressões lambda para permitir que o código de chamada especificar uma condição de filtro e uma coluna para classificar os resultados por e um parâmetro de cadeia de caracteres permite que o chamador forneça uma lista delimitada por vírgulas de propriedades de navegação para carregamento rápido:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

O código `Expression<Func<TEntity, bool>> filter` significa que o chamador fornecerá uma expressão lambda com base no `TEntity` tipo e essa expressão retorna um valor booliano. Por exemplo, se o repositório é instanciado para o `Student` tipo de entidade, o código no método de chamada pode especificar `student => student.LastName == "Smith` &quot; para o `filter` parâmetro.

O código `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` também significa que o chamador fornecerá uma expressão lambda. Mas nesse caso, a entrada para a expressão for um `IQueryable` de objeto para o `TEntity` tipo. A expressão retornará uma versão ordenada do que `IQueryable` objeto. Por exemplo, se o repositório é instanciado para o `Student` tipo de entidade, o código no método de chamada pode especificar `q => q.OrderBy(s => s.LastName)` para o `orderBy` parâmetro.

O código de `Get` método cria um `IQueryable` de objeto e, em seguida, aplica-se a expressão de filtro se houver um:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Em seguida ele se aplica as expressões de carregamento rápido depois de analisar a lista delimitada por vírgula:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Finalmente, ele se aplica a `orderBy` expressão se houver e retorna os resultados; caso contrário, ele retorna os resultados da consulta não ordenada:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quando você chama o `Get` método, você pode fazer a filtragem e classificação de `IEnumerable` coleção retornada pelo método em vez de fornecer parâmetros para essas funções. Mas a classificação e filtragem de trabalho, em seguida, deve ser feitos na memória no servidor web. Usando esses parâmetros, você garantir que o trabalho é feito pelo banco de dados, em vez do servidor web. Uma alternativa é criar classes derivadas para tipos de entidade específico e adicionar especializado `Get` métodos, como `GetStudentsInNameOrder` ou `GetStudentsByName`. No entanto, em um aplicativo complexo, isso pode resultar em um grande número dessas classes derivadas e métodos especializados, que podem ser mais trabalho para manter.

O código de `GetByID`, `Insert`, e `Update` métodos é semelhante ao que você viu no repositório não genérico. (Você não fornece um parâmetro de carregamento rápido no `GetByID` assinatura, porque você não pode fazer o carregamento rápido com o `Find` método.)

Duas sobrecargas são fornecidas para o `Delete` método:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Um desses permite que você transmitir apenas a ID da entidade a ser excluído e um recebe uma instância de entidade. Como você viu no [tratamento simultaneidade](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial para simultaneidade tratamento você precisa um `Delete` método que usa uma instância de entidade que contém o valor original de uma propriedade de controle.

Esse repositório genérico tratará requisitos CRUD típicos. Quando um tipo de entidade específico tem requisitos especiais, como filtragem mais complexas ou pedidos, você pode criar uma classe derivada que tem métodos adicionais para esse tipo.

## <a name="creating-the-unit-of-work-class"></a>Criando a classe de unidade de trabalho

A classe de unidade de trabalho tem uma finalidade: para garantir que quando você usa vários repositórios, eles compartilham um contexto de banco de dados único. Dessa forma, quando uma unidade de trabalho for concluída, você pode chamar o `SaveChanges` método nessa instância do contexto de e ter certeza de que todos os relacionados a alterações serão coordenadas. Tudo o que as necessidades de classe é um `Save` método e uma propriedade para cada repositório. Cada propriedade do repositório retorna uma instância de repositório que foi instanciada usando a mesma instância de contexto do banco de dados como as outras instâncias do repositório.

No *DAL* pasta, crie um arquivo de classe chamado *UnitOfWork.cs* e substitua o código de modelo com o código a seguir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

O código cria variáveis de classe para o contexto do banco de dados e cada repositório. Para o `context` variável, um novo contexto é instanciado:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Cada propriedade do repositório verifica se o repositório já existe. Caso contrário, ele cria uma instância do repositório, passando a instância do contexto. Como resultado, todos os repositórios compartilham a mesma instância de contexto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

O `Save` chamadas de método `SaveChanges` no contexto do banco de dados.

Qualquer classe que instancia um contexto de banco de dados em uma variável de classe, como o `UnitOfWork` classe implementa `IDisposable` e descarta o contexto.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Alterar o controlador de curso para usar a classe UnitOfWork e repositórios

Substitua o código que você tem atualmente em *CourseController.cs* com o código a seguir:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Esse código adiciona uma variável de classe para o `UnitOfWork` classe. (Se você estivesse usando interfaces aqui, você não inicializar a variável aqui; em vez disso, você poderia implementar um padrão de dois construtores exatamente como você fez o `Student` repositório.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

No restante da classe, todas as referências para o contexto do banco de dados são substituídas por referências para o repositório apropriado, usando `UnitOfWork` propriedades para acessar o repositório. O `Dispose` método descarta o `UnitOfWork` instância.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Executar o site e clique no **cursos** guia.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

A página de pesquisa e funciona da mesma forma que antes suas alterações e as outras páginas de curso também funcionam da mesma.

## <a name="summary"></a>Resumo

Você implementou o repositório e a unidade de padrões de trabalho. Você usou as expressões lambda como parâmetros de método no repositório genérico. Para obter mais informações sobre como usar essas expressões com um `IQueryable` de objeto, consulte [IQueryable(T) Interface (LINQ)](https://msdn.microsoft.com/library/bb351562.aspx) na biblioteca MSDN. Na próxima tutorial, você aprenderá como lidar com alguns cenários avançados.

Links para outros recursos do Entity Framework podem ser encontradas no [ASP.NET mapa de conteúdo de acesso de dados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Próximo](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
