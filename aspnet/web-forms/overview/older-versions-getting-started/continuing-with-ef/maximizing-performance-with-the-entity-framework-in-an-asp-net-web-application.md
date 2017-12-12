---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximizar o desempenho com o Entity Framework 4.0 em um aplicativo ASP.NET 4 | Microsoft Docs
author: tdykstra
description: "Esta série de tutorial se baseia no aplicativo web Contoso Universidade que é criado pelo guia de Introdução com a série de tutoriais do Entity Framework 4.0. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 9e257f5061f6bdf14ad0776ff6385fb526d6dcb1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximizar o desempenho com o Entity Framework 4.0 em um aplicativo ASP.NET 4
====================
Por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais se baseia no aplicativo da web Contoso Universidade que é criado pelo [guia de Introdução com o Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você pode ter sido criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série tutorial completo. Se você tiver dúvidas sobre os tutoriais, você poderá postá-los para o [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


No tutorial anterior, você viu como lidar com conflitos de simultaneidade. Este tutorial mostra opções para melhorar o desempenho de um aplicativo web ASP.NET que usa o Entity Framework. Você aprenderá a vários métodos para maximizar o desempenho ou diagnosticar problemas de desempenho.

As informações apresentadas nas seções a seguir são provavelmente serão úteis em uma ampla variedade de cenários:

- Carregar com eficiência os dados relacionados.
- Gerencie o estado de exibição.

As informações apresentadas nas seções a seguir podem ser útil se você tiver consultas individuais que problemas de desempenho presentes:

- Use o `NoTracking` opção de mesclagem.
- Pré-compile consultas LINQ.
- Examine os comandos de consulta enviados para o banco de dados.

As informações apresentadas na seção a seguir são potencialmente útil para aplicativos que têm modelos de dados muito grande:

- Gere previamente modos de exibição.

> [!NOTE]
> Desempenho do aplicativo Web é afetado por muitos fatores, incluindo o tamanho dos dados de solicitação e resposta, a velocidade de consultas de banco de dados, quantas solicitações que o servidor pode enfileirar e rapidez pode servi-los e até mesmo a eficiência de qualquer bibliotecas de scripts de cliente que talvez você esteja usando. Se o desempenho for crítico em seu aplicativo, ou se o teste ou a experiência mostra que o desempenho do aplicativo não é satisfatório, você deve seguir o protocolo normal para ajuste de desempenho. Medidas para determinar onde os afunilamentos de desempenho estão ocorrendo e resolva as áreas que terão o maior impacto no desempenho geral do aplicativo.
> 
> Este tópico se concentra principalmente em maneiras em que é possível melhorar o desempenho especificamente do Entity Framework no ASP.NET. As sugestões aqui são úteis se você determinar que o acesso a dados é um dos afunilamentos de desempenho em seu aplicativo. Exceto conforme observado, os métodos explicados aqui não devem ser considerados &quot;práticas recomendadas&quot; em geral, muitas delas são apropriadas somente em situações excepcionais ou a tipos de endereço muito específica de afunilamentos de desempenho.


Para iniciar o tutorial, inicie o Visual Studio e abra o aplicativo web Contoso Universidade que você estava trabalhando no tutorial anterior.

## <a name="efficiently-loading-related-data"></a>Carregar com eficiência os dados relacionados

Há várias maneiras que o Entity Framework pode carregar dados relacionados para as propriedades de navegação de uma entidade:

- *Carregamento preguiçoso*. Quando a entidade é lido pela primeira vez, os dados relacionados não são recuperados. No entanto, na primeira vez que tentar acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Isso resulta em várias consultas enviadas ao banco de dados — um para a própria entidade e um cada vez que os dados para a entidade relacionados deve ser recuperado. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Carregamento adiantado*. Quando a entidade é lido, os dados relacionados são recuperados com ela. Normalmente, isso resulta em uma consulta de junção único que recupera todos os dados que é necessário. Especifique o carregamento rápido usando o `Include` método, como já vimos nos tutoriais.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Carregamento explícito*. Isso é semelhante ao carregamento lento, exceto que você explicitamente recuperar os dados relacionados no código; Isso não acontece automaticamente quando você acessa uma propriedade de navegação. Carregar dados relacionados manualmente usando o `Load` método da propriedade de navegação para coleções, ou usar o `Load` método da propriedade de referência para propriedades que contêm um único objeto. (Por exemplo, você chama o `PersonReference.Load` método ao carregar o `Person` propriedade de navegação de um `Department` entidade.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Porque eles imediatamente não recuperarem os valores de propriedade, carregamento lento e carregamento explícito também são ambos conhecidos como *carregamento diferido*.

Carregamento lento é o comportamento padrão de um contexto de objeto que foi gerado pelo designer. Se você abrir o *SchoolModel.Designer.cs* arquivo que define a classe de contexto de objeto, você encontrará três métodos de construtor, e cada um deles inclui a instrução a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Em geral, se você souber terá dados relacionados para cada entidade recuperado, eager carregamento oferece o melhor desempenho, porque uma única consulta enviada para o banco de dados é geralmente mais eficiente do que consultas separadas para cada entidade recuperada. Por outro lado, se você precisar acessar as propriedades de navegação de uma entidade raramente ou apenas para um pequeno conjunto de entidades, o carregamento lento ou o carregamento explícito pode ser mais eficiente, porque mais dados que você precisa recuperar o carregamento rápido.

Em um aplicativo web, carregamento lento pode ser relativamente pouco valor mesmo assim, porque ações de usuário que afetam a necessidade de dados relacionados ocorrem no navegador, que não tem nenhuma conexão para o contexto de objeto que a página renderizada. Por outro lado, quando você databind um controle, você normalmente sabe quais dados você precisa e geralmente é melhor escolher carregamento adiantado ou carregamento adiado com base no que é apropriado em cada cenário.

Além disso, um controle de ligação de dados pode usar um objeto de entidade depois que o contexto do objeto é descartado. Nesse caso, uma tentativa de uma propriedade de navegação lento carga falhará. Você recebe a mensagem de erro é clara:&quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

O `EntityDataSource` desabilita o controle de carregamento lento por padrão. Para o `ObjectDataSource` controle que você está usando para o tutorial atual (ou se você acessar o contexto de objeto de código da página), há várias maneiras que você pode tornar lenta carregamento desabilitado por padrão. Você pode desabilitá-lo quando você criar uma instância de um contexto de objeto. Por exemplo, você pode adicionar a linha a seguir para o método de construtor do `SchoolRepository` classe:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Para o aplicativo da Contoso University, você fará o contexto de objeto desabilitar o carregamento preguiçoso para que essa propriedade não precisa ser definido sempre que um contexto de instância é criado automaticamente.

Abra o *SchoolModel.edmx* dados de modelo, clique na superfície de design e, em seguida, no painel Propriedades, defina o **carregamento lento habilitada** propriedade `False`. Salve e feche o modelo de dados.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Gerenciamento de estado de exibição

Para fornecer funcionalidade de atualização, uma página da web deve armazenar os valores de propriedade original de uma entidade quando uma página é processada. Durante um postback para processar o controle pode recriar o estado original da entidade e chamar a entidade `Attach` método antes de aplicar as alterações e chamar o `SaveChanges` método. Por padrão, os controles de formulários da Web ASP.NET de dados usam o estado de exibição para armazenar os valores originais. No entanto, o estado de exibição pode afetar o desempenho, porque ela está armazenada nos campos ocultos que podem aumentar consideravelmente o tamanho da página que é enviado para e do navegador.

Técnicas para gerenciar o estado de exibição ou alternativas, como estado de sessão, não são exclusivas para o Entity Framework para que este tutorial não entram nesse tópico em detalhes. Para obter mais informações, consulte os links no final do tutorial.

No entanto, a versão 4 do ASP.NET fornece uma nova maneira de trabalhar com o estado de exibição que todos os desenvolvedores do ASP.NET de aplicativos Web Forms devem estar atento: o `ViewStateMode` propriedade. Essa nova propriedade pode ser definida no nível de página ou controle e permite que você desabilite o estado de exibição por padrão para uma página e habilitá-lo somente para controles que precisam dela.

Para aplicativos em que o desempenho for crítico, uma boa prática é sempre desabilitar o estado de exibição no nível da página e habilitá-lo somente para controles que a exigem. O tamanho do estado de exibição nas páginas de Contoso Universidade não diminuir substancialmente por esse método, mas para ver como isso funciona, você fará isso o *Instructors.aspx* página. Essa página contém vários controles, incluindo um `Label` controle com estado de exibição desabilitado. Nenhum dos controles nesta página, na verdade, precisa ter o estado ativado de modo de exibição. (O `DataKeyNames` propriedade o `GridView` controle especifica o estado que deve ser mantido entre postbacks, mas esses valores são mantidos no estado de controle, que não é afetado pelo `ViewStateMode` propriedade.)

O `Page` diretiva e `Label` marcação controle atualmente se parece com o exemplo a seguir:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Faça as seguintes alterações:

- Adicionar `ViewStateMode="Disabled"` para o `Page` diretiva.
- Remover `ViewStateMode="Disabled"` do `Label` controle.

A marcação agora se parece com o exemplo a seguir:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Estado de exibição agora é desabilitado para todos os controles. Se mais tarde você adicionar um controle que precisam usar o estado de exibição, tudo o que você precisa fazer é incluir o `ViewStateMode="Enabled"` atributo para o controle.

## <a name="using-the-notracking-merge-option"></a>Usando a opção de mesclagem NoTracking

Quando um contexto de objeto recupera linhas do banco de dados e cria objetos de entidade que representam-los, por padrão ele também controla os objetos de entidade usando o Gerenciador de estado do objeto. Dados de controle atua como um cache e são usados quando você atualizar uma entidade. Como um aplicativo da web geralmente tem instâncias de contexto do objeto de curta duração, consultas normalmente retornam dados que não precisam ser controlados, porque o contexto de objeto que lê-los será descartado antes de qualquer uma das entidades lê usadas novamente ou atualizada.

O Entity Framework, você pode especificar se o contexto do objeto controla os objetos de entidade, definindo um *opção de mesclagem*. Você pode definir a opção de mesclagem para consultas individuais ou para conjuntos de entidades. Se você defini-lo para um conjunto de entidades, que significa que você está definindo a opção de mesclagem padrão para todas as consultas que são criados para esse conjunto de entidades.

Para o aplicativo da Contoso University, o controle não é necessária para qualquer um dos conjuntos de entidades que você acessa do repositório, você pode definir a opção de mesclagem `NoTracking` para esses conjuntos de entidade quando você criar uma instância de contexto do objeto da classe do repositório. (Observe que neste tutorial, definindo a opção de mesclagem não terá um efeito significativo no desempenho do aplicativo. O `NoTracking` opção é provável que faça uma melhoria de desempenho observável apenas em determinados cenários de alto volume de dados.)

Na pasta DAL, abra o *SchoolRepository.cs* de arquivos e adicionar um método de construtor que define a opção de mesclagem para a entidade define que acessa o repositório:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Pré-compilação consultas do LINQ

Na primeira vez que o Entity Framework executa uma consulta de Entity SQL dentro de vida de um determinado `ObjectContext` instância, levará algum tempo para compilar a consulta. O resultado da compilação é armazenado em cache, que significa que as execuções subsequentes da consulta serão muito mais rápidas. Consultas LINQ seguem um padrão semelhante, exceto que parte do trabalho necessário para compilar a consulta é feita sempre que a consulta é executada. Em outras palavras, para consultas LINQ, por padrão não todos os resultados da compilação são armazenados em cache.

Se você tiver uma consulta LINQ que você pretende executar repetidamente na vida de um contexto de objeto, você pode escrever código que faz com que todos os resultados da compilação a ser armazenado em cache a primeira vez que a consulta LINQ é executada.

Como ilustração, isso será feito de duas `Get` métodos o `SchoolRepository` classe, um dos quais não usa nenhum parâmetro (o `GetInstructorNames` método) e outra que requer um parâmetro (o `GetDepartmentsByAdministrator` método). Esses métodos como estão agora, na verdade, não precisam ser compilado porque eles não são consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

No entanto, para que você pode experimentar consultas compiladas, você vai continuar como se esses tinham sido escritos como as consultas LINQ a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Você pode alterar o código desses métodos que tem mostrado acima e execute o aplicativo para verificar se ele funciona antes de continuar. Mas as instruções a seguir ir diretamente para a criação de versões pré-compilado deles.

Criar um arquivo de classe no *DAL* pasta, nomeie-o *SchoolEntities.cs*e substitua o código existente pelo seguinte código:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Esse código cria uma classe parcial que estende a classe de contexto de objeto gerado automaticamente. A classe parcial inclui duas consultas do LINQ compiladas usando o `Compile` método o `CompiledQuery` classe. Ele também cria métodos que você pode usar para chamar as consultas. Salve e feche o arquivo.

Em seguida, na *SchoolRepository.cs*, alterar existente `GetInstructorNames` e `GetDepartmentsByAdministrator` métodos no repositório de classe para que eles chamam de consultas compiladas:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Execute o *Departments.aspx* página para verificar se ele funciona como antes. O `GetInstructorNames` método é chamado para preencher a lista suspensa de administrador e o `GetDepartmentsByAdministrator` método é chamado quando você clicar em **atualização** para verificar se nenhum instrutor é um administrador de mais de um departamento.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Você tem consultas pré-compiladas no aplicativo Contoso University somente para ver como fazer isso, não porque ele seria, melhorar o desempenho. Pré-compilando consultas LINQ adicionar um nível de complexidade ao seu código, portanto certifique-se de que fazer isso apenas para consultas que realmente representam afunilamentos de desempenho em seu aplicativo.

## <a name="examining-queries-sent-to-the-database"></a>Examinando as consultas enviadas ao banco de dados

Quando você estiver investigando problemas de desempenho, às vezes é útil saber os comandos SQL exatos que está enviando o Entity Framework para o banco de dados. Se você estiver trabalhando com um `IQueryable` do objeto, uma maneira de fazer isso é usar o `ToTraceString` método.

Em *SchoolRepository.cs*, altere o código no `GetDepartmentsByName` método para corresponder ao exemplo a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

O `departments` variável deve ser convertida em um `ObjectQuery` tipo porque somente o `Where` método no final da linha anterior cria um `IQueryable` objeto; sem o `Where` método, a conversão não é necessária.

Defina um ponto de interrupção a `return` de linha e, em seguida, execute o *Departments.aspx* página no depurador. Quando você atinge o ponto de interrupção, examine o `commandText` variável o **locais** janela e use o Visualizador de texto (na Lupa no **valor** coluna) para exibir seu valor no **Visualizador de texto** janela. Você pode ver o comando SQL inteiro resultante este código:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Como alternativa, o recurso IntelliTrace no Visual Studio Ultimate fornece uma maneira de exibir comandos SQL gerados pelo Entity Framework que não exigem que você altere seu código ou até mesmo definir um ponto de interrupção.

> [!NOTE]
> Você pode executar os procedimentos a seguir somente se você tiver o Visual Studio Ultimate.


Restaurar o código original no `GetDepartmentsByName` método e, em seguida, execute o *Departments.aspx* página no depurador.

No Visual Studio, selecione o **depurar** menu, em seguida, **IntelliTrace**e, em seguida, **eventos do IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

No **IntelliTrace** janela, clique em **interromper tudo**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

O **IntelliTrace** janela exibe uma lista de eventos recentes:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Clique o **ADO.NET** linha. Expande para mostrar o texto do comando:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Você pode copiar a cadeia de caracteres de texto de comando inteiro para a área de transferência do **locais** janela.

Suponha que você estava trabalhando com um banco de dados com mais tabelas, relações e colunas que simples `School` banco de dados. Você pode achar que uma consulta que reúne todas as informações que você precisa em um único `Select` instrução contendo várias `Join` cláusulas torna-se muito complexo para trabalhar com eficiência. Nesse caso, você pode alternar de carregamento para carregamento explícito para simplificar a consulta rápido.

Por exemplo, tente alterar o código de `GetDepartmentsByName` método *SchoolRepository.cs*. No momento em que método você tem uma consulta de objeto que tem `Include` métodos para o `Person` e `Courses` propriedades de navegação. Substitua o `return` instrução com o código que executa o carregamento explícito, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Execute o *Departments.aspx* página no depurador e verifique o **IntelliTrace** janela novamente como você fez antes. Agora, em que houve uma única consulta antes, você verá uma sequência longa de-los.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Clique no primeiro **ADO.NET** linha para ver o que aconteceu com a consulta complexa é visualizado anteriormente.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

A consulta de departamentos se tornou um simples `Select` consulta sem nenhum `Join` cláusula, mas ele é seguido por consultas separadas que recuperam cursos relacionados e um administrador, usando um conjunto de duas consultas para cada departamento retornado pelo original consulta.

> [!NOTE]
> Se você deixar lento carregamento habilitado, o padrão que você vê aqui, com a mesma consulta repetida várias vezes, pode resultar de carregamento lento. Um padrão que você geralmente deseja evitar é dados relacionados de carregamento lento para cada linha da tabela primária. A menos que você verificou que uma consulta de junção único é muito complexa para ser eficiente, normalmente seria capaz de melhorar o desempenho em tais casos, alterando a consulta principal para usar o carregamento rápido.


## <a name="pre-generating-views"></a>Pré-geração de modos de exibição

Quando um `ObjectContext` objeto é criado pela primeira vez em um novo domínio de aplicativo, o Entity Framework gera um conjunto de classes que ele usa para acessar o banco de dados. Essas classes são chamadas *exibições*, e se você tiver um modelo de dados muito grandes, gerando esses modos de exibição pode atrasar a resposta do site da web para a primeira solicitação para uma página depois que um novo domínio de aplicativo é inicializado. Você pode reduzir esse atraso da primeira solicitação criando os modos de exibição em tempo de compilação em vez de em tempo de execução.

> [!NOTE]
> Se seu aplicativo não tiver um modelo de dados muito grande, ou se ele tem um modelo de dados grandes, mas você não estiver preocupado com um problema de desempenho que afeta apenas a primeira solicitação de página depois que o IIS é reciclado, você poderá ignorar esta seção. Exibição de criação não acontece sempre que você criar uma instância de um `ObjectContext` o objeto, pois os modos de exibição são armazenados em cache no domínio do aplicativo. Portanto, a menos que você está frequentemente reciclagem de seu aplicativo no IIS, poucas solicitações de páginas pode se beneficiar do exibições pré-geradas.


Pré-você pode gerar exibições usando o *EdmGen.exe* ferramenta de linha de comando ou usando um *Text Template Transformation Toolkit* modelo (T4). Neste tutorial, você usará um modelo T4.

No *DAL* pasta, adicionar um arquivo usando o **modelo de texto** modelo (está sob o **geral** nó o **modelos instalados** lista), e nomeie-o *SchoolModel.Views.tt*. Substitua o código existente no arquivo com o código a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Esse código gera os modos de exibição para um *. edmx* arquivo que está localizado na mesma pasta que o modelo e que tem o mesmo nome do arquivo de modelo. Por exemplo, se o arquivo de modelo é denominado *SchoolModel.Views.tt*, ela procurará um arquivo de modelo de dados chamado *SchoolModel.edmx*.

Salve o arquivo, em seguida, clique no arquivo em **Solution Explorer** e selecione **executar personalizado ferramenta**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

O Visual Studio gera um arquivo de código que cria os modos de exibição, que é chamado de *SchoolModel.Views.cs* com base no modelo. (Você deve ter notado que o arquivo de código é gerado, mesmo antes de selecionar **executar personalizado ferramenta**, assim que você salvar o arquivo de modelo.)

[![Para Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Agora você pode executar o aplicativo e verificar se ela funciona como antes.

Para obter mais informações sobre modos de exibição pré-gerados, consulte os seguintes recursos:

- [Como: gerar previamente exibições para melhorar o desempenho da consulta](https://msdn.microsoft.com/en-us/library/bb896240.aspx) no site do MSDN. Explica como usar o `EdmGen.exe` ferramenta de linha de comando para gerar previamente modos de exibição.
- [Isolamento de desempenho com pré-compilado/pré-generated exibições no Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) no blog da equipe de consultoria do Windows Server AppFabric ao cliente.

Isso conclui a introdução para melhorar o desempenho de um aplicativo web ASP.NET que usa o Entity Framework. Para obter mais informações, consulte os seguintes recursos:

- [Considerações de desempenho (Entity Framework)](https://msdn.microsoft.com/en-us/library/cc853327.aspx) no site do MSDN.
- [Relacionados ao desempenho postagens no blog da equipe do Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF mesclagem opções e consultas compiladas](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Postagem do blog que explica os comportamentos inesperados de consultas compiladas e mesclagem opções como `NoTracking`. Se você planeja usar consultas compiladas ou manipular as configurações de opção de mesclagem em seu aplicativo, leia primeiro.
- [Postagens no blog de dados e modelagem a equipe de consultoria do cliente no Entity Framework relacionadas](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Inclui postagens em consultas compiladas e usando o Visual Studio 2010 Profiler para descobrir problemas de desempenho.
- [Thread do Fórum do Entity Framework com avisos sobre como melhorar o desempenho de consultas complexas altamente](https://social.msdn.microsoft.com/Forums/en-US/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Recomendações de gerenciamento de estado do ASP.NET](https://msdn.microsoft.com/en-us/library/z1hkazw7.aspx).
- [Usando o Entity Framework e o ObjectDataSource: paginação personalizada](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Postagem do blog que se baseia no aplicativo ContosoUniversity criado nos tutoriais para explicar como implementar a paginação no *Departments.aspx* página.

O seguinte tutorial examina alguns dos aprimoramentos importantes para o Entity Framework que são novos na versão 4.

>[!div class="step-by-step"]
[Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
[Próximo](what-s-new-in-the-entity-framework-4.md)
