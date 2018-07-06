---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Exibindo dados em um gráfico com páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: microsoft
description: Este capítulo explica como exibir dados em um gráfico. Nos capítulos anteriores, você aprendeu como exibir os dados manualmente e em uma grade. Este capítulo explica...
ms.author: aspnetcontent
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 161dfa1b2c0676c79baebb00e303e8cb9df1d4e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812575"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Exibindo dados em um gráfico com páginas da Web ASP.NET (Razor)
====================
por [Microsoft](https://github.com/microsoft)

> Este artigo explica como usar um gráfico para exibir dados em um site de páginas da Web do ASP.NET (Razor) usando o `Chart` auxiliar.
> 
> **O que você vai aprender**:
> 
> - Como exibir dados em um gráfico.
> - Como os gráficos de estilo usando temas internos.
> - Como salvar gráficos e como armazenar em cache-los para melhorar o desempenho.
> 
> Estes são os recursos introduzidos no artigo de programação do ASP.NET:
> 
> - O `Chart` auxiliar.
> 
> > [!NOTE]
> > As informações neste artigo se aplica a 1.0 de páginas da Web do ASP.NET e Web Pages 2.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>O auxiliar de gráfico

Quando você deseja exibir seus dados em formato gráfico, você pode usar `Chart` auxiliar. O `Chart` auxiliar pode renderizar uma imagem que exibe dados em uma variedade de tipos de gráfico. Ele dá suporte a várias opções para formatar e rotular. O `Chart` auxiliar pode processar mais de 30 tipos de gráficos, incluindo todos os tipos de gráficos que você pode estar familiarizado do Microsoft Excel ou outras ferramentas de &#8212; gráficos de área, gráficos de barras, gráficos de colunas, gráficos de linhas e gráficos de pizza, juntamente com muito mais gráficos especializados, como gráficos de ações.

| **Gráfico de área** ![Descrição: a imagem do tipo de gráfico de área](7-displaying-data-in-a-chart/_static/image1.jpg) | **Gráfico de barras** ![Descrição: a imagem do tipo de gráfico de barras](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Gráfico de colunas** ![Descrição: a imagem do tipo de gráfico de coluna](7-displaying-data-in-a-chart/_static/image3.jpg) | **Gráfico de linhas** ![Descrição: a imagem do tipo de gráfico de linha](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Gráfico de pizza** ![Descrição: a imagem do tipo de gráfico de pizza](7-displaying-data-in-a-chart/_static/image5.jpg) | **Gráfico de estoque** ![Descrição: a imagem do tipo de gráfico de estoque](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elementos do gráfico

Gráficos mostram dados e elementos adicionais, como legendas, eixos, série e assim por diante. A imagem a seguir mostra muitos dos elementos do gráfico que você pode personalizar quando você usa o `Chart` auxiliar. Este artigo mostra como definir algumas (não todas) desses elementos.

![Descrição: Imagem mostrando os elementos do gráfico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Criar um gráfico de dados

Os dados exibidos em um gráfico podem ser de uma matriz, dos resultados retornados de um banco de dados ou de dados que estão em um arquivo XML.

### <a name="using-an-array"></a>Usando uma matriz

Conforme explicado em [Introdução ao ASP.NET páginas da Web de programação usando a sintaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890), uma matriz permite armazenar uma coleção de itens semelhantes em uma única variável. Você pode usar matrizes para conter os dados que você deseja incluir em seu gráfico.

Este procedimento mostra como você pode criar um gráfico dos dados em matrizes, usando o tipo de gráfico padrão. Ele também mostra como exibir o gráfico dentro da página.

1. Criar um novo arquivo chamado *ChartArrayBasic.cshtml*.
2. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    O código primeiro cria um novo gráfico e define sua largura e altura. Especifique o título do gráfico usando o `AddTitle` método. Para adicionar dados, você deve usar o `AddSeries` método. Neste exemplo, você deve usar o `name`, `xValue`, e `yValues` parâmetros do `AddSeries` método. O `name` parâmetro é exibido na legenda do gráfico. O `xValue` parâmetro contém uma matriz de dados que são exibidos ao longo do eixo horizontal do gráfico. O `yValues` parâmetro contém uma matriz de dados que são usados para plotar os pontos verticais do gráfico.

    O `Write` método renderiza, na verdade, o gráfico. Nesse caso, porque você não especificou um tipo de gráfico, o `Chart` auxiliar renderiza seu gráfico de padrão, que é um gráfico de colunas.
3. Execute a página no navegador. O navegador exibe o gráfico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Usando uma consulta de banco de dados para dados do gráfico

Se as informações que você deseja criar o gráfico estiverem em um banco de dados, você pode executar uma consulta de banco de dados e, em seguida, usar dados de resultados para criar o gráfico. Este procedimento mostra como ler e exibir os dados do banco de dados criado no artigo [Introdução ao trabalho com um banco de dados em Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Adicionar um *App\_dados* pasta na raiz do site da Web se a pasta ainda não existir.
2. No *App\_dados* pasta, adicione o arquivo de banco de dados chamado *SmallBakery.sdf* que é descrita em [Introdução ao trabalho com um banco de dados em Sites de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Criar um novo arquivo chamado *ChartDataQuery.cshtml*.
4. Substitua o conteúdo existente pelo seguinte:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    O código primeiro abre o banco de dados SmallBakery e atribui a uma variável chamada `db`. Essa variável representa um `Database` objeto que pode ser usado para ler e gravar no banco de dados. Em seguida, o código executa uma consulta SQL para obter o nome e o preço de cada produto. O código cria um novo gráfico e passa a consulta de banco de dados a ele, chamando o gráfico `DataBindTable` método. Esse método usa dois parâmetros: o `dataSource` parâmetro é para os dados da consulta e o `xField` parâmetro permite que você defina qual coluna de dados é usada para o eixo x do gráfico.

    Como uma alternativa ao uso de `DataBindTable` método, você pode usar o `AddSeries` método da `Chart` auxiliar. O `AddSeries` método permite que você defina a `xValue` e `yValues` parâmetros. Por exemplo, em vez de usar o `DataBindTable` método como este:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Você pode usar o `AddSeries` método como este:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Ambas renderizam os mesmos resultados. O `AddSeries` método é mais flexível porque você pode especificar o tipo de gráfico e os dados de forma mais explícita, mas o `DataBindTable` método é mais fácil de usar se você não precisa mais flexibilidade.
5. Execute a página em um navegador. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Usando dados XML

A terceira opção para criar gráficos é usar um arquivo XML como os dados para o gráfico. Isso requer que o arquivo XML também tenha um arquivo de esquema (*. xsd* arquivo) que descreve a estrutura XML. Este procedimento mostra como ler dados de um arquivo XML.

1. No *App\_dados* pasta, crie um novo arquivo XML denominado *Data. XML*.
2. Substitua o XML existente com o seguinte, que alguns dados XML sobre funcionários em uma empresa fictícia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. No *App\_dados* pasta, crie um novo arquivo XML denominado *data.xsd*. (Observe que a extensão neste momento *. xsd*.)
4. Substitua o XML existente com o seguinte: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Na raiz do site, crie um novo arquivo chamado *ChartDataXML.cshtml*.
6. Substitua o conteúdo existente pelo seguinte: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    O código cria primeiro um `DataSet` objeto. Esse objeto é usado para gerenciar os dados são lidos do arquivo XML e organizam-o de acordo com as informações no arquivo de esquema. (Observe que a parte superior do código inclui a instrução `using SystemData`. Isso é necessário para poder trabalhar com o `DataSet` objeto. Para obter mais informações, consulte [ &quot;Using&quot; instruções e os nomes totalmente qualificados](#SB_UsingStatements) mais adiante neste artigo.)

    Em seguida, o código cria um `DataView` objeto com base no conjunto de dados. A exibição de dados fornece um objeto que o gráfico pode associar a &#8212; ou seja, ler e criar gráficos. O gráfico é associado a dados usando o `AddSeries` método, como você viu anteriormente ao criar um gráfico de dados da matriz, exceto que desta vez o `xValue` e `yValues` parâmetros estão definidos para o `DataView` objeto.

    Este exemplo mostra como especificar um determinado tipo de gráfico. Quando os dados são adicionados na `AddSeries` método, o `chartType` parâmetro também é configurado para exibir um gráfico de pizza.
7. Execute a página em um navegador. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Instruções "Using" e os nomes totalmente qualificados
> 
> O ASP.NET Web Pages com sintaxe do Razor com base no .NET Framework consiste em vários milhares de componentes (classes). Para tornar gerenciáveis para trabalhar com todas essas classes, eles estão organizados em *namespaces*, que são um pouco como bibliotecas. Por exemplo, o `System.Web` namespace contém classes que oferecem suporte à comunicação de navegador e o servidor, o `System.Xml` namespace contém classes que são usadas para criar e ler arquivos XML, e o `System.Data` namespace contém classes que permitem que você trabalhe com os dados.
> 
> Para acessar qualquer determinada classe no .NET Framework, o código precisa conhecer não apenas o nome de classe, mas também o namespace que contém a classe. Por exemplo, para usar o `Chart` auxiliar, o código precisa localizar o `System.Web.Helpers.Chart` classe, que combina o namespace (`System.Web.Helpers`) com o nome de classe (`Chart`). Isso é conhecido como a classe *totalmente qualificado* nome &#8212; sua localidade completa e inequívoca dentro a vastness do .NET Framework. No código, isso seria a seguinte aparência:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> No entanto, é complicado (e sujeito a erros) que usar esses nomes longos e totalmente qualificado sempre que você deseja fazer referência a uma classe ou auxiliar. Portanto, para tornar mais fácil de usar nomes de classe, você pode *importação* os namespaces que você está interessado, que é geralmente é apenas um punhado dentre os muitos namespaces no .NET Framework. Se você importar um namespace, você pode usar apenas um nome de classe (`Chart`) em vez do nome totalmente qualificado (`System.Web.Helpers.Chart`). Quando seu código é executado e encontra um nome de classe, ele pode examinar apenas os namespaces que você importou para encontrar essa classe.
> 
> Quando você usa o ASP.NET Web Pages com sintaxe do Razor para criar páginas da web, você normalmente usa o mesmo conjunto de classes de cada vez, incluindo o `WebPage` classe, os vários auxiliares e assim por diante. Para salvar o trabalho de importar os namespaces relevantes, sempre que você cria um site, o ASP.NET está configurado para que ele importa automaticamente um conjunto de namespaces do principal para todos os sites. É por isso que você ainda não teve que lidar com namespaces ou importando até agora; todas as classes que você já trabalhou com estão em namespaces já importados para você.
> 
> No entanto, às vezes, você precisará trabalhar com uma classe que não está em um namespace que é importado automaticamente para você. Nesse caso, você pode usar nome totalmente qualificado dessa classe, ou você pode importar manualmente o namespace que contém a classe. Para importar um namespace, você deve usar o `using` instrução (`import` no Visual Basic), como visto anteriormente em um exemplo de artigo.
> 
> Por exemplo, o `DataSet` classe está no `System.Data` namespace. O `System.Data` namespace não está automaticamente disponível para páginas Razor do ASP.NET. Portanto, para trabalhar com o `DataSet` usando seu nome totalmente qualificado da classe, você pode usar código como este:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Se você tiver que usar o `DataSet` classe repetidamente você pode importar um namespace assim e, em seguida, usar apenas o nome de classe no código:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Você pode adicionar `using` instruções para quaisquer outros namespaces do .NET Framework que você deseja referenciar. No entanto, conforme observado, você não precisará fazer isso com frequência, porque a maioria das classes que você usará para trabalhar com namespaces, que são importados automaticamente pelo ASP.NET para uso em *. cshtml* e *. vbhtml* páginas.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Exibir gráficos dentro de uma página da Web

Nos exemplos você viu até agora, você cria um gráfico e, em seguida, o gráfico é renderizado diretamente para o navegador como um elemento gráfico. No entanto, em muitos casos, você deseja exibir um gráfico como parte de uma página, não apenas por si só no navegador. Para fazer isso requer um processo em duas etapas. A primeira etapa é criar uma página que gera o gráfico, como você já viu.

A segunda etapa é exibir a imagem resultante em outra página. Para exibir a imagem, você usar um HTML `<img>` elemento, da mesma maneira que faria para exibir qualquer imagem. No entanto, em vez de referenciar um *. jpg* ou *. PNG* arquivo, o `<img>` referências de elemento a *. cshtml* arquivo que contém o `Chart` auxiliar que cria o gráfico. Quando a página de exibição é executada, o `<img>` elemento obtém a saída a `Chart` auxiliar e renderiza o gráfico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Crie um arquivo chamado *ShowChart.cshtml*.
2. Substitua o conteúdo existente pelo seguinte: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    O código usa o `<img>` elemento para exibir o gráfico que você criou anteriormente na *ChartArrayBasic.cshtml* arquivo.
3. Execute a página da web em um navegador. O *ShowChart.cshtml* arquivo exibe a imagem do gráfico com base em que o código contido na *ChartArrayBasic.cshtml* arquivo.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Definir o estilo de um gráfico

O `Chart` auxiliar dá suporte a um grande número de opções que permitem que você personalize a aparência do gráfico. Você pode definir cores, fontes, bordas e assim por diante. Uma maneira fácil de personalizar a aparência de um gráfico é usar um *tema*. Os temas são conjuntos de informações que especificam como renderizar um gráfico usando fontes, cores, rótulos, paletas, bordas e efeitos. (Observe que o estilo de um gráfico não indica o tipo de gráfico).

A tabela a seguir lista os temas internos.

| {1&gt;Tema&lt;1} | Descrição |
| --- | --- |
| `Vanilla` | Exibe colunas vermelhas em um plano de fundo branco. |
| `Blue` | Exibe a colunas em um plano de fundo gradiente azul de azul. |
| `Green` | Exibe a colunas em uma tela de fundo gradiente verde de azul. |
| `Yellow` | Exibe colunas laranjas em um plano de fundo gradiente amarelo. |
| `Vanilla3D` | Exibe colunas vermelhas 3D em um plano de fundo branco. |

Você pode especificar o tema a ser usado ao criar um novo gráfico.

1. Criar um novo arquivo chamado *ChartStyleGreen.cshtml*.
2. Substitua o conteúdo existente no página com o seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Esse código é o mesmo que o exemplo anterior que usa o banco de dados para dados, mas adiciona o `theme` parâmetro quando ele cria o `Chart` objeto. O exemplo a seguir mostra o código alterado:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Execute a página em um navegador. Você verá os mesmos dados que antes, mas o gráfico se parece mais refinado: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Salvar um gráfico

Quando você usa o `Chart` auxiliar que você já viu até agora neste artigo, o auxiliar recria o gráfico do zero toda vez que ele é invocado. Se necessário, o código para o gráfico também consulta novamente o banco de dados ou lê novamente o arquivo XML para obter os dados. Em alguns casos, isso pode ser uma operação complexa, como se você estiver consultando o banco de dados for grande, ou se o arquivo XML contém muitos dados. Mesmo se o gráfico não envolve muitos dados, o processo de criar dinamicamente uma imagem leva os recursos do servidor e, se muitas pessoas solicitou a página ou páginas que exibem o gráfico, pode haver um impacto no desempenho do seu site.

Para ajudar a reduzir o possível impacto no desempenho da criação de um gráfico, você pode criar um gráfico o primeiro tempo você precisar dele e, em seguida, salvá-lo. Quando o gráfico for necessário novamente, em vez de gerar novamente, você pode apenas buscar a versão salva e renderizar isso.

Você pode salvar um gráfico das seguintes maneiras:

- Armazenar em cache o gráfico na memória do computador (no servidor).
- Salve o gráfico como um arquivo de imagem.
- Salve o gráfico como um arquivo XML. Essa opção permite modificar o gráfico antes de salvá-lo.

### <a name="caching-a-chart"></a>Um gráfico de cache

Depois de criar um gráfico, você pode armazenar em cache-lo. Um gráfico de cache significa que ele não precisa ser criada novamente se ele precisar ser exibida novamente. Quando você salva um gráfico no cache, você atribuir uma chave que deve ser exclusiva para que o gráfico.

Gráficos de salvos no cache podem ser removidos se o servidor é executa com pouco memória. Além disso, o cache é limpo, se seu aplicativo for reiniciado por qualquer motivo. Portanto, o modo padrão para trabalhar com um gráfico em cache é sempre verificar primeiro se ele está disponível no cache e se não, em seguida criar ou recriá-la.

1. Na raiz do seu site, crie um arquivo chamado *ShowCachedChart.cshtml*.
2. Substitua o conteúdo existente pelo seguinte: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    O `<img>` marca inclui um `src` atributo que aponta para o *ChartSaveToCache.cshtml* de arquivo e passa uma chave para a página como uma cadeia de caracteres de consulta. A chave contém o valor &quot;myChartKey&quot;. O *ChartSaveToCache.cshtml* arquivo contém o `Chart` auxiliar que cria o gráfico. Você criará nesta página em alguns instantes.

    No final da página, há um link para uma página chamada *ClearCache.cshtml*. Que é uma página que você também criará em breve. Você precisa de *ClearCache.cshtml* somente para testar o cache para este exemplo — não é um link ou uma página que você normalmente inclua ao trabalhar com gráficos armazenados em cache.
3. Na raiz do seu site, crie um novo arquivo chamado *ChartSaveToCache.cshtml*.
4. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    O código primeiro verifica se nada foi passado como o valor da chave na cadeia de caracteres de consulta. Se assim, o código tenta ler um gráfico de fora do cache chamando o `GetFromCache` método e passá-lo a chave. Se for descoberto que não há nada no cache sob essa chave (o que aconteceria na primeira vez que o gráfico é solicitado), o código cria o gráfico como de costume. Quando o gráfico for concluído, o código salvará o cache chamando `SaveToCache`. Esse método requer uma chave (portanto, o gráfico pode ser solicitado mais tarde) e a quantidade de tempo que o gráfico deve ser salvo no cache. (O tempo exato que você poderia armazenar em cache um gráfico dependeria a frequência com que você achou que os dados que ele representa pode ser alterado.) O `SaveToCache` método também requer um `slidingExpiration` parâmetro &#8212; se isso estiver definido como true, o tempo limite do contador é redefinido cada vez que o gráfico é acessado. Nesse caso, ele na verdade significa que a entrada de cache do gráfico expira 2 minutos após a última vez que alguém acessado o gráfico. (A alternativa para a expiração deslizante é uma expiração absoluta, que significa que a entrada de cache deve expirar exatamente 2 minutos depois que ele foi colocado no cache, não importa quantas vezes ela havia sido acessada.)

    Por fim, o código usa o `WriteFromCache` método para buscar e renderizar o gráfico do cache. Observe que esse método está fora do `if` bloco que verifica o cache, porque ele obterá o gráfico do cache se o gráfico estava lá para começar ou tinha que ser gerada e salva no cache.

    Observe que no exemplo, o `AddTitle` método inclui um carimbo de hora. (Ele adiciona a data e hora atuais &#8212; `DateTime.Now` &#8212; para o título.)
5. Criar uma nova página chamada *ClearCache.cshtml* e substitua seu conteúdo pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Essa página usa o `WebCache` auxiliar para remover o gráfico que é armazenado em cache no *ChartSaveToCache.cshtml*. Conforme observado anteriormente, você normalmente não precisa ter uma página como essa. Você está criando-o aqui apenas para tornar mais fácil testar o cache.
6. Execute o *ShowCachedChart.cshtml* página da web em um navegador. A página exibe a imagem do gráfico com base em que o código contido na *ChartSaveToCache.cshtml* arquivo. Observe que o carimbo de hora diga o título do gráfico. 

    ![Descrição: Imagem de gráfico básico com carimbo de hora em que o título do gráfico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Feche o navegador.
8. Execute o *ShowCachedChart.cshtml* novamente. Observe que o carimbo de hora é o mesmo de antes, que indica que o gráfico não foi regenerado, mas em vez disso, foi lido no cache.
9. Na *ShowCachedChart.cshtml*, clique no **Limpar cache** link. Isso leva você para *ClearCache.cshtml*, que informa que o cache foi limpo.
10. Clique o **retornar ao ShowCachedChart.cshtml** vincular ou executar novamente *ShowCachedChart.cshtml* do WebMatrix. Observe que dessa vez o carimbo de hora foi alterado, porque o cache foi limpo. Portanto, o código era necessário regenerar o gráfico e colocá-lo de volta para o cache.

### <a name="saving-a-chart-as-an-image-file"></a>Salvar um gráfico como um arquivo de imagem

Você também pode salvar um gráfico como um arquivo de imagem (por exemplo, como um *. jpg* arquivo) no servidor. Em seguida, você pode usar o arquivo de imagem da maneira como faria qualquer imagem. A vantagem é que o arquivo é armazenado em vez de salvos em um cache temporário. Você pode salvar uma nova imagem de gráfico em momentos diferentes (por exemplo, a cada hora) e, em seguida, manter um registro permanente das alterações que ocorrem ao longo do tempo. Observe que, certifique-se de que seu aplicativo web tem permissão para salvar um arquivo para a pasta no servidor onde você deseja colocar o arquivo de imagem.

1. Na raiz do seu site, crie uma pasta chamada  *\_ChartFiles* se ele ainda não existir.
2. Na raiz do seu site, crie um novo arquivo chamado *ChartSave.cshtml*.
3. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    O código primeiro verifica para ver se o *. jpg* arquivo existir, chamando o `File.Exists` método. Se o arquivo não existir, o código cria um novo `Chart` de uma matriz. Neste momento, o código chama o `Save` e passa o `path` parâmetro para especificar o caminho do arquivo e o nome de arquivo de onde deseja salvar o gráfico. No corpo da página, uma `<img>` elemento usa o caminho para apontar para o *. jpg* arquivo a ser exibido.
4. Execute o *ChartSave.cshtml* arquivo.
5. Retorne para o WebMatrix. Observe que um arquivo de imagem denominado *chart01.jpg* terá sido salva nas  *\_ChartFiles* pasta.

### <a name="saving-a-chart-as-an-xml-file"></a>Salvar um gráfico como um arquivo XML

Por fim, você pode salvar um gráfico como um arquivo XML no servidor. Uma vantagem de usar esse método sobre o gráfico de cache ou salvar o gráfico em um arquivo é que você poderia modificar o XML antes de exibir o gráfico se você quisesse. Seu aplicativo deve ter permissões de leitura/gravação para a pasta no servidor onde você deseja colocar o arquivo de imagem.

1. Na raiz do seu site, crie um novo arquivo chamado *ChartSaveXml.cshtml*.
2. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Esse código é semelhante ao código que você viu anteriormente para armazenar um gráfico no cache, exceto que ele usa um arquivo XML. O código primeiro verifica para ver se o arquivo XML existe chamando o `File.Exists` método. Se o arquivo existir, o código cria uma nova `Chart` do objeto e passa o nome de arquivo como o `themePath` parâmetro. Isso cria o gráfico com base em tudo o que está no arquivo XML. Se o arquivo XML ainda não existir, o código cria um gráfico como normal e, em seguida, chama `SaveXml` salvá-lo. O gráfico é renderizado utilizando o `Write` método, como você viu antes.

    Assim como acontece com a página que mostrou o armazenamento em cache, esse código inclui um carimbo de hora em que o título do gráfico.
3. Criar uma nova página chamada *ChartDisplayXMLChart.cshtml* e adicione a seguinte marcação a ele: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Execute o *ChartDisplayXMLChart.cshtml* página. O gráfico é exibido. Anote o carimbo de hora no título do gráfico.
5. Feche o navegador.
6. No WebMatrix, clique com botão direito do  *\_ChartFiles* pasta, clique em **atualizar**e, em seguida, abra a pasta. O *XMLChart.xml* arquivo nessa pasta foi criado pelo `Chart` auxiliar. 

    ![Descrição: A _ChartFiles pasta mostrando o arquivo XMLChart.xml criado pelo auxiliar de gráfico.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Execute o *ChartDisplayXMLChart.cshtml* página novamente. O gráfico mostra o mesmo carimbo de hora, como na primeira vez que você executou a página. Isso ocorre porque o gráfico está sendo gerado do XML que você salvou anteriormente.
8. No WebMatrix, abra o  *\_ChartFiles* pasta e excluir os *XMLChart.xml* arquivo.
9. Execute o *ChartDisplayXMLChart.cshtml* página mais uma vez. Neste momento, o carimbo de hora é atualizado, pois o `Chart` auxiliar era necessário recriar o arquivo XML. Verifique se você quiser, o  *\_ChartFiles* pasta e observe que o arquivo XML está de volta.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Sites de páginas de Introdução ao trabalho com um banco de dados da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Usar o cache no ASP.NET Web Sites para melhorar o desempenho de páginas](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Classe gráfico](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (referência de API de páginas da Web do ASP.NET no MSDN)
