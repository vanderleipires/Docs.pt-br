---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maximizar o desempenho com o Entity Framework 4.0 em um aplicativo ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Esta série de tutoriais se baseia no aplicativo web Contoso University que é criado pelo Getting Started with a série de tutoriais do Entity Framework 4.0. EU...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7d7c66289f09179a98e09532172477d5b06c70bd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834929"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maximizar o desempenho com o Entity Framework 4.0 em um aplicativo ASP.NET 4
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta série de tutoriais baseia-se no aplicativo web Contoso University que é criado pela [Introdução ao Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de tutoriais. Se você não concluir os tutoriais anteriores, como um ponto de partida para este tutorial, você pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que você teria criado. Você também pode [baixar o aplicativo](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que é criado pela série de tutoriais completa. Se você tiver dúvidas sobre os tutoriais, você pode postá-los para o [Fórum do Entity Framework do ASP.NET](https://forums.asp.net/1227.aspx).


No tutorial anterior, você viu como lidar com conflitos de simultaneidade. Este tutorial mostra as opções para melhorar o desempenho de um aplicativo web ASP.NET que usa o Entity Framework. Você aprenderá sobre os vários métodos para maximizar o desempenho ou para diagnosticar problemas de desempenho.

Informações apresentadas nas seções a seguir são provavelmente serão úteis em uma ampla variedade de cenários:

- Carregar com eficiência os dados relacionados.
- Gerencie o estado de exibição.

Informações apresentadas nas seções a seguir podem ser útil se você tiver consultas individuais que problemas de desempenho presentes:

- Use o `NoTracking` opção de mesclagem.
- Pré-compile as consultas LINQ.
- Examine os comandos de consulta enviados para o banco de dados.

Informações apresentadas na seção a seguir são potencialmente útil para aplicativos que têm modelos de dados extremamente grandes:

- Gere previamente exibições.

> [!NOTE]
> Desempenho do aplicativo Web é afetado por muitos fatores, inclusive coisas como o tamanho dos dados de solicitação e resposta, a velocidade das consultas de banco de dados, quantas solicitações que o servidor pode enfileirar e quão rapidamente pode atendê-los e até mesmo a eficiência de qualquer bibliotecas de script de cliente que talvez você esteja usando. Se o desempenho for crítico em seu aplicativo, ou se o teste ou a experiência mostra que o desempenho do aplicativo não é satisfatório, você deve seguir o protocolo normal para ajuste de desempenho. Medir para determinar onde estão ocorrendo gargalos de desempenho e, em seguida, resolva as áreas que terão o maior impacto no desempenho geral do aplicativo.
> 
> Este tópico se concentra principalmente em maneiras em que você pode melhorar o desempenho especificamente do Entity Framework no ASP.NET. As sugestões aqui são úteis se você determinar que o acesso a dados é um dos afunilamentos de desempenho em seu aplicativo. Exceto conforme observado, os métodos explicados aqui não devem ser considerados &quot;práticas recomendadas&quot; em geral — muitas delas são apropriadas apenas em situações excepcionais ou a tipos muito específicos de endereço de gargalos de desempenho.


Para iniciar o tutorial, inicie o Visual Studio e abra o aplicativo web Contoso University que você estava trabalhando no tutorial anterior.

## <a name="efficiently-loading-related-data"></a>Carregamento de dados relacionados com eficiência

Há várias maneiras que o Entity Framework pode carregar dados relacionados nas propriedades de navegação de uma entidade:

- *Carregamento lento*. Quando a entidade é lida pela primeira vez, os dados relacionados não são recuperados. No entanto, na primeira vez que você tenta acessar uma propriedade de navegação, os dados necessários para essa propriedade de navegação são recuperados automaticamente. Isso resulta em várias consultas enviadas ao banco de dados — um para a própria entidade e um cada vez que os dados para a entidade relacionados deve ser recuperado. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Carregamento adiantado*. Quando a entidade é lida, os dados relacionados são recuperados com ela. Normalmente, isso resulta em uma única consulta de junção que recupera todos os dados necessários. Especificar o carregamento adiantado, usando o `Include` método, como você já viu nestes tutoriais.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Carregamento explícito*. Isso é semelhante ao carregamento lento, exceto que você explicitamente recupera os dados relacionados no código; Isso não acontece automaticamente quando você acessa uma propriedade de navegação. Carregar dados relacionados manualmente usando o `Load` método da propriedade de navegação para coleções, ou usar o `Load` método da propriedade de referência para propriedades que contêm um único objeto. (Por exemplo, você chama o `PersonReference.Load` método para carregar o `Person` propriedade de navegação de um `Department` entity.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Porque eles imediatamente não recuperarem os valores de propriedade, carregamento lento e o carregamento explícito também são ambos conhecidos como *carregamento adiado*.

Carregamento lento é o comportamento padrão para um contexto de objeto que foi gerado pelo designer. Se você abrir o *SchoolModel.Designer.cs* arquivo que define a classe de contexto de objeto, você encontrará três métodos de construtor, e cada um deles inclui a instrução a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Em geral, se você souber você precisa dados relacionados para cada entidade que o carregamento adiantado, recuperado oferece o melhor desempenho, porque uma única consulta enviada ao banco de dados é geralmente mais eficiente do que consultas separadas para cada entidade recuperada. Por outro lado, se você precisar acessar propriedades de navegação de uma entidade com pouca frequência, ou apenas para um pequeno conjunto de entidades, o carregamento lento ou o carregamento explícito pode ser mais eficiente, pois o carregamento adiantado seria recuperar mais dados do que você precisa.

Em um aplicativo web, carregamento lento pode ser de relativamente pouco valor de qualquer forma, porque ações do usuário que afetam a necessidade de dados relacionados ocorrem no navegador, que não tem nenhuma conexão ao contexto de objeto que a página renderizada. Por outro lado, quando você databind um controle, você normalmente sabe quais dados você precisa e, portanto, geralmente é melhor para escolher o carregamento adiantado ou o carregamento adiado, com base no que é apropriado em cada cenário.

Além disso, um controle de vinculação de dados pode usar um objeto de entidade depois que o contexto de objeto é descartado. Nesse caso, uma tentativa de uma propriedade de navegação lenta carga falharia. A mensagem de erro recebida é clara: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

O `EntityDataSource` controle desabilita o carregamento lento por padrão. Para o `ObjectDataSource` controle que você está usando para o tutorial atual (ou, se você acessar o contexto de objeto do código de página), há várias maneiras que você pode tornar lenta carregamento desabilitado por padrão. Você pode desabilitá-lo quando você criar uma instância de um contexto de objeto. Por exemplo, você pode adicionar a seguinte linha ao método do construtor de `SchoolRepository` classe:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Para o aplicativo Contoso University, você fará o contexto de objeto automaticamente desabilitar o carregamento lento para que essa propriedade não precisa ser definido sempre que um contexto é instanciado.

Abra o *Schoolmodel* dados de modelo, clique na superfície de design e, em seguida, no painel Propriedades, defina o **carregamento lento ativado** propriedade `False`. Salve e feche o modelo de dados.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Gerenciando o estado de exibição

Para fornecer a funcionalidade de atualização, uma página da web do ASP.NET deve armazenar os valores de propriedade originais de uma entidade quando uma página é renderizada. Durante o processamento do controle de postback pode recriar o estado original da entidade e chamar a entidade `Attach` método antes de aplicar as alterações e chamar o `SaveChanges` método. Por padrão, os controles de dados de Web Forms do ASP.NET usam o estado de exibição para armazenar os valores originais. No entanto, o estado de exibição pode afetar o desempenho, porque ele é armazenado em campos ocultos que podem aumentar substancialmente o tamanho da página que é enviado para e do navegador.

Técnicas para gerenciar o estado de exibição ou alternativas, como estado de sessão, não são exclusivas para o Entity Framework, para que este tutorial não vá para esse tópico em detalhes. Para obter mais informações, consulte os links no final do tutorial.

No entanto, a versão 4 do ASP.NET fornece uma nova maneira de trabalhar com o estado de exibição que todos os desenvolvedores de aplicativos Web Forms ASP.NET devem estar atento: o `ViewStateMode` propriedade. Essa nova propriedade pode ser definida no nível de página ou controle, e permite que você desabilite o estado de exibição por padrão para uma página e habilitá-lo somente para controles que precisam dele.

Para aplicativos em que o desempenho é crítico, uma prática recomendada é sempre desabilite o estado de exibição no nível da página e habilitá-lo somente para controles que precisam dele. O tamanho do estado de exibição nas páginas do Contoso University não seria substancialmente reduzido por esse método, mas para ver como ele funciona, você fará isso o *Instructors.aspx* página. Essa página contém muitos controles, incluindo um `Label` controle que tem o estado de exibição desabilitado. Nenhum dos controles nesta página é realmente necessário ter o modo de exibição estado habilitado. (O `DataKeyNames` propriedade do `GridView` controle especifica o estado que deve ser mantido entre postbacks, mas esses valores são mantidos no estado de controle, que não é afetado pelo `ViewStateMode` propriedade.)

O `Page` diretiva e `Label` marcação de controle atualmente é semelhante ao exemplo a seguir:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Faça as seguintes alterações:

- Adicione `ViewStateMode="Disabled"` para o `Page` diretiva.
- Remova `ViewStateMode="Disabled"` do `Label` controle.

A marcação agora se parece com o exemplo a seguir:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Estado de exibição agora é desabilitado para todos os controles. Se depois você adicionar um controle que precisam usar o estado de exibição, tudo o que você precisa fazer é incluir o `ViewStateMode="Enabled"` atributo para o controle.

## <a name="using-the-notracking-merge-option"></a>Usando a opção de mesclagem NoTracking

Quando um contexto de objeto recupera linhas do banco de dados e cria objetos de entidade que representá-los, por padrão ele também controla os objetos de entidade usando seu Gerenciador de estado do objeto. Esses dados de rastreamento atuam como um cache e são usados quando você atualiza uma entidade. Como um aplicativo da web geralmente tem instâncias de contexto de objeto e de curta duração, consultas normalmente retornam dados que não precisam ser controlados, porque o contexto de objeto que lê-los será descartado antes de qualquer uma das entidades que ele lê usadas novamente ou atualizadas.

No Entity Framework, você pode especificar se o contexto de objeto acompanha os objetos de entidade, definindo uma *opção de mesclagem*. Você pode definir a opção de mesclagem para consultas individuais ou para conjuntos de entidades. Se você defini-lo para um conjunto de entidades, que significa que você está definindo a opção de mesclagem padrão para todas as consultas que são criados para esse conjunto de entidades.

Para o aplicativo Contoso University, acompanhamento não é necessário para qualquer um dos conjuntos de entidade que você acessa do repositório, portanto, você pode definir a opção de mesclagem `NoTracking` para esses conjuntos de entidades ao instanciar o contexto de objeto na classe de repositório. (Observe que neste tutorial, definindo a opção de mesclagem não terá um efeito notável no desempenho do aplicativo. O `NoTracking` opção é provável que faça uma melhoria de desempenho observável somente em determinados cenários de alto volume de dados.)

Na pasta DAL, abra o *SchoolRepository.cs* arquivo e adicione um método de construtor que define a opção de mesclagem para a entidade define que o repositório acessa:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Pré-compilando consultas do LINQ

Na primeira vez que o Entity Framework executa uma consulta Entity SQL na vida de um determinado `ObjectContext` instância, leva algum tempo para compilar a consulta. O resultado da compilação é armazenado em cache, que significa que as execuções subsequentes da consulta são muito mais rápidas. Consultas LINQ seguem um padrão semelhante, exceto que a parte do trabalho necessário para compilar a consulta é feita sempre que a consulta é executada. Em outras palavras, para consultas LINQ, por padrão nem todos os resultados da compilação são armazenados em cache.

Se você tiver uma consulta LINQ que você pretende executar repetidamente na vida de um contexto de objeto, você pode escrever código que faz com que todos os resultados da compilação a ser armazenado em cache a primeira vez em que a consulta LINQ é executada.

Como ilustração, você fará isso para dois `Get` métodos na `SchoolRepository` classe, um dos quais não usa parâmetros (o `GetInstructorNames` método) e outra que exigem um parâmetro (o `GetDepartmentsByAdministrator` método). Esses métodos como estão agora, na verdade, não precisam ser compilada porque eles não são consultas LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

No entanto, para que você pode experimentar consultas compiladas, você vai continuar como se essas tinham sido escritas como as consultas LINQ a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Você pode alterar o código nesses métodos ao que foi mostrado acima e execute o aplicativo para verificar se ele funciona antes de continuar. Mas as instruções a seguir ir diretamente para criar versões pré-compiladas deles.

Criar um arquivo de classe na *DAL* pasta, nomeie- *SchoolEntities.cs*e substitua o código existente pelo código a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Esse código cria uma classe parcial que estende a classe de contexto de objeto gerado automaticamente. A classe parcial inclui duas consultas LINQ compiladas usando o `Compile` método da `CompiledQuery` classe. Ele também cria métodos que você pode usar para chamar as consultas. Salve e feche esse arquivo.

Em seguida, na *SchoolRepository.cs*, altere o existente `GetInstructorNames` e `GetDepartmentsByAdministrator` métodos no repositório de classe para que eles chamam as consultas compiladas:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Execute o *Departments.aspx* página para verificar se ele funciona como antes. O `GetInstructorNames` método é chamado para preencher a lista suspensa de administrador e o `GetDepartmentsByAdministrator` método é chamado quando você clica em **atualização** para verificar se nenhum instrutor é um administrador de mais de um departamento.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Você tem consultas pré-compilação compiladas no aplicativo Contoso University apenas para ver como fazer isso, não porque ele seria melhorar o desempenho consideravelmente. Pré-compilando consultas LINQ adicionam um nível de complexidade ao seu código, portanto certifique-se de que fazer isso apenas para consultas que, na verdade, representam os gargalos de desempenho em seu aplicativo.

## <a name="examining-queries-sent-to-the-database"></a>Examinando as consultas enviadas ao banco de dados

Quando você estiver investigando problemas de desempenho, às vezes é útil saber os comandos SQL exatos que o Entity Framework está enviando para o banco de dados. Se você estiver trabalhando com um `IQueryable` do objeto, uma maneira de fazer isso é usar o `ToTraceString` método.

Na *SchoolRepository.cs*, altere o código no `GetDepartmentsByName` método para corresponder ao exemplo a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

O `departments` variável deve ser convertida em um `ObjectQuery` digitar apenas porque a `Where` método no final da linha anterior cria um `IQueryable` do objeto; sem o `Where` método, a conversão não seria necessária.

Defina um ponto de interrupção a `return` de linha e, em seguida, execute o *Departments.aspx* página no depurador. Quando você atinge o ponto de interrupção, examinar os `commandText` variável no **Locals** janela e use o Visualizador de texto (na Lupa no **valor** coluna) para exibir seu valor em que o **Visualizador de texto** janela. Você pode ver o comando inteiro do SQL que é o resultado desse código:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Como alternativa, o recurso IntelliTrace no Visual Studio Ultimate fornece uma maneira para exibir comandos SQL gerados pelo Entity Framework que não exige que você altere seu código ou até mesmo definir um ponto de interrupção.

> [!NOTE]
> Você pode executar os procedimentos a seguir somente se você tiver o Visual Studio Ultimate.


Restaurar o código original na `GetDepartmentsByName` método e, em seguida, execute o *Departments.aspx* página no depurador.

No Visual Studio, selecione o **Debug** menu, em seguida, **IntelliTrace**e então **eventos do IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

No **IntelliTrace** janela, clique em **interromper tudo**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

O **IntelliTrace** janela exibe uma lista dos eventos recentes:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Clique o **ADO.NET** linha. Ela se expande para mostrar o texto do comando:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Você pode copiar a cadeia de caracteres de texto do comando inteiro para a área de transferência do **Locals** janela.

Suponha que você estava trabalhando com um banco de dados com mais tabelas, relações e colunas do que o simples `School` banco de dados. Você pode achar que uma consulta que reúne todas as informações que você precisa em uma única `Select` instrução que contém vários `Join` cláusulas se torna muito complexo para trabalhar com eficiência. Nesse caso, você pode alternar de carregamento rápido para o carregamento explícito para simplificar a consulta.

Por exemplo, tente alterar o código na `GetDepartmentsByName` método no *SchoolRepository.cs*. No momento em que método você tem uma consulta de objeto que tem `Include` métodos para o `Person` e `Courses` propriedades de navegação. Substitua o `return` instrução com o código que executa o carregamento explícito, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Execute o *Departments.aspx* página no depurador e verifique o **IntelliTrace** janela novamente, como você fazia antes. Agora, onde havia uma única consulta antes, você verá uma sequência longa de-los.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Clique no primeiro **ADO.NET** linha para ver o que aconteceu com a consulta complexa é exibido anteriormente.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

A consulta de departamentos se tornou um simples `Select` consulta sem nenhum `Join` cláusula, mas ele é seguido por consultas separadas que recuperam cursos relacionados e um administrador, usando um conjunto de duas consultas para cada departamento retornado pelo original consulta.

> [!NOTE]
> Se você deixar lento carregamento habilitado, o padrão visto aqui, com a mesma consulta repetida várias vezes, pode ser resultado de carregamento lento. Um padrão que você geralmente deseja evitar é o carregamento lento de dados relacionados para cada linha da tabela primária. A menos que você verificou que uma única consulta de junção é muito complexa para ser eficiente, normalmente seria capaz de melhorar o desempenho em tais casos, alterando a consulta principal para usar o carregamento adiantado.


## <a name="pre-generating-views"></a>Gerar previamente exibições

Quando um `ObjectContext` objeto é criado em um novo domínio de aplicativo, o Entity Framework gera um conjunto de classes que ele usa para acessar o banco de dados. Essas classes são chamadas *modos de exibição*, e se você tiver um modelo de dados muito grandes, gerar esses modos de exibição pode atrasar a resposta do site da web para a primeira solicitação para uma página depois que um novo domínio de aplicativo é inicializado. Você pode reduzir esse atraso da primeira solicitação, criando os modos de exibição em tempo de compilação em vez de em tempo de execução.

> [!NOTE]
> Se seu aplicativo não tiver um modelo de dados extremamente grandes, ou se ele tem um modelo de dados grandes, mas você não estiver preocupado com um problema de desempenho que afeta somente a primeira solicitação de página depois que o IIS é reciclado, você poderá ignorar esta seção. Exibição de criação não acontece sempre que você criar uma instância de um `ObjectContext` do objeto, porque os modos de exibição são armazenados em cache no domínio do aplicativo. Portanto, a menos que você está frequentemente reciclagem de seu aplicativo no IIS, muito poucos solicitações de página se beneficiaria da exibições pré-geradas.


Você pode gerar previamente exibições usando o *EdmGen.exe* ferramenta de linha de comando ou usando uma *Text Template Transformation Toolkit* modelo (T4). Neste tutorial, você usará um modelo T4.

No *DAL* pasta, adicione um arquivo usando o **modelo de texto** modelo (ele está sob o **geral** nó no **modelos instalados** lista), e nomeie-o *SchoolModel.Views.tt*. Substitua o código existente no arquivo pelo código a seguir:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Esse código gera os modos de exibição para um *. edmx* arquivo que está localizado na mesma pasta que o modelo e que tem o mesmo nome que o arquivo de modelo. Por exemplo, se o arquivo de modelo *SchoolModel.Views.tt*, ele procurará um arquivo de modelo de dados chamado *Schoolmodel*.

Salve o arquivo, em seguida, clique no arquivo no **Gerenciador de soluções** e selecione **executar ferramenta personalizada**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio gera um arquivo de código que cria os modos de exibição, que é chamado *SchoolModel.Views.cs* com base no modelo. (Você deve ter notado que o arquivo de código é gerado, mesmo antes de selecionar **executar ferramenta personalizada**, assim que você salvar o arquivo de modelo.)

[![Para Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Agora você pode executar o aplicativo e verificar se ele funciona como antes.

Para obter mais informações sobre modos de exibição gerados previamente, consulte os seguintes recursos:

- [Como: gerar previamente exibições para melhorar o desempenho da consulta](https://msdn.microsoft.com/library/bb896240.aspx) no site do MSDN. Explica como usar o `EdmGen.exe` ferramenta de linha de comando para gerar previamente exibições.
- [Isolamento de desempenho com pré-compilado/pré-gerados exibições no Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) no blog da equipe consultiva para clientes Windows Server AppFabric.

Isso conclui a introdução à melhoria do desempenho de um aplicativo web ASP.NET que usa o Entity Framework. Para obter mais informações, consulte os seguintes recursos:

- [Considerações de desempenho (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) no site do MSDN.
- [Relacionados ao desempenho postagens no blog da equipe do Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Opções e consultas compiladas de mesclagem de EF](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Opções de postagem de blog que explica os comportamentos inesperados de consultas compiladas e mesclagem como `NoTracking`. Se você planeja usar consultas compiladas ou manipular as configurações de opção de mesclagem de seu aplicativo, leia isso primeiro.
- [Entity Framework relacionadas postagens no blog de dados e modelagem equipe consultiva para clientes](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Inclui postagens sobre consultas compiladas e usar o Profiler de 2010 Visual Studio para descobrir problemas de desempenho.
- [Thread de Fórum do Entity Framework com conselhos sobre como melhorar o desempenho das consultas altamente complexas](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Recomendações de gerenciamento de estado do ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Usando o Entity Framework e o ObjectDataSource: paginação personalizada](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Postagem de blog que se baseia no aplicativo de ContosoUniversity criado nesses tutoriais para explicar como implementar a paginação na *Departments.aspx* página.

O próximo tutorial analisa alguns dos aprimoramentos importantes para o Entity Framework que são novos na versão 4.

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Próximo](what-s-new-in-the-entity-framework-4.md)
