---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Controlar a nomenclatura de ID em páginas de conteúdo (c#) | Microsoft Docs
author: rick-anderson
description: Ilustra como os controles ContentPlaceHolder servem como um contêiner de nomenclatura e, portanto, fazer trabalhando programaticamente com um controle difícil (via FindConrol)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0ad9224e4cb580e9ef671f39bbd3da39df692a56
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377292"
---
<a name="control-id-naming-in-content-pages-c"></a>ID do controle de nomenclatura em páginas de conteúdo (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Ilustra como os controles ContentPlaceHolder servem como um contêiner de nomenclatura e, portanto, fazer trabalhando programaticamente com um controle difícil (via FindConrol). Aborda esse problema e soluções alternativas. Também descreve como acessar programaticamente o valor de ClientID resultante.


## <a name="introduction"></a>Introdução

Todos os controles de servidor ASP.NET incluem uma `ID` propriedade que identifica o controle exclusivamente e é o meio pelo qual o controle é acessado por meio de programação na classe code-behind. Da mesma forma, os elementos em um documento HTML podem incluir um `id` atributo que identifica exclusivamente o elemento; eles `id` valores geralmente são usados no script do lado do cliente para fazer referência a um determinado elemento HTML por meio de programação. Devido a isso, você pode assumir que, quando um controle de servidor ASP.NET é processado em HTML, sua `ID` valor é usado como o `id` valor do elemento HTML renderizado. Isso não é necessariamente o caso porque em determinadas circunstâncias um único controle com um único `ID` valor pode aparecer várias vezes na marcação renderizada. Considere um controle GridView que inclui um TemplateField com um controle de Web de rótulo com um `ID` valor ProductName. Quando o GridView está associado a sua fonte de dados em tempo de execução, esse rótulo é repetido uma vez para cada linha de GridView. Cada renderizado necessidades de rótulo em um único `id` valor.

Para lidar com esses cenários, o ASP.NET permite que certos controles a ser indicada como nomear os contêineres. Um contêiner de nomenclatura serve como um novo `ID` namespace. Os controles de servidor que aparecem no contêiner de nomeação têm seus renderizado `id` valor prefixado com o `ID` do controle no contêiner de nomenclatura. Por exemplo, o `GridView` e `GridViewRow` classes são ambos os contêineres de nomenclatura. Consequentemente, um controle de rótulo definido em um GridView TemplateField com `ID` ProductName recebe um renderizado `id` valor `GridViewID_GridViewRowID_ProductName`. Porque *GridViewRowID* é exclusivo para cada linha de GridView, resultante `id` valores sejam exclusivos.

> [!NOTE]
> O [ `INamingContainer` interface](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) é usado para indicar que um determinado controle de servidor ASP.NET deve funcionar como um contêiner de nomenclatura. O `INamingContainer` interface não soletrar todos os métodos que o controle de servidor deve implementar; em vez disso, ele é usado como um marcador. Gerar a marcação renderizada, se um controle implementa essa interface, em seguida, o mecanismo do ASP.NET automaticamente inclui prefixos de seu `ID` valor para seus descendentes processado `id` valores de atributo. Esse processo é abordado em mais detalhes na etapa 2.


Contêineres de nomenclatura não apenas alterar o renderizado `id` valor do atributo, mas também afetam como o controle pode ser referenciado por meio de programação de classe de code-behind da página ASP.NET. O `FindControl("controlID")` método normalmente é usado para fazer referência a um controle da Web por meio de programação. No entanto, `FindControl` não invadir por meio de contêineres de nomenclatura. Consequentemente, você não pode usar diretamente o `Page.FindControl` método a referenciar os controles dentro de um GridView ou outro contêiner de nomenclatura.

Como você pode ter imaginado, páginas mestras e ContentPlaceHolders são implementados como contêineres de nomenclatura. Neste tutorial, examinamos como mestre 2&gt;elementos HTML afetam `id` valores e maneiras de fazer referência por meio de programação de controles da Web dentro de uma página de conteúdo usando `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Etapa 1: Adicionando uma nova página ASP.NET

Para demonstrar os conceitos discutidos neste tutorial, vamos adicionar uma nova página ASP.NET ao nosso site. Criar uma nova página de conteúdo denominada `IDIssues.aspx` na pasta raiz, associando-o para o `Site.master` página mestra.


![Adicionar o IDIssues.aspx página conteúdo para a pasta raiz](control-id-naming-in-content-pages-cs/_static/image1.png)

**Figura 01**: Adicionar página de conteúdo `IDIssues.aspx` para a pasta raiz


Visual Studio cria automaticamente um controle de conteúdo para cada um dos ContentPlaceHolders de quatro da página mestra. Conforme observado na [ *vários ContentPlaceHolders e conteúdo padrão* ](multiple-contentplaceholders-and-default-content-cs.md) tutorial, se não houver um controle de conteúdo o conteúdo de ContentPlaceHolder da página mestra padrão é emitido em vez disso. Porque o `QuickLoginUI` e `LeftColumnContent` ContentPlaceHolders contêm marcação padrão adequada para essa página, vá em frente e remover seus controles de conteúdo do `IDIssues.aspx`. Neste ponto, a marcação declarativa da página de conteúdo deve ser semelhante ao seguinte:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

No [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial, criamos uma classe de página de base personalizada (`BasePage`) que configura automaticamente o título da página se ela for não foi explicitamente definido. Para o `IDIssues.aspx` página para empregar essa funcionalidade, a classe de code-behind da página deve derivar do `BasePage` classe (em vez de `System.Web.UI.Page`). Modifique a definição da classe de code-behind para que ele é semelhante ao seguinte:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Por fim, atualize o `Web.sitemap` arquivo para incluir uma entrada para esta lição novo. Adicionar um `<siteMapNode>` elemento e defina sua `title` e `url` atributos "Problemas de nomenclatura de ID de controle" e `~/IDIssues.aspx`, respectivamente. Depois de fazer essa adição seu `Web.sitemap` marcação do arquivo deve ser semelhante ao seguinte:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Como ilustra a Figura 2, a nova entrada de mapa de site no `Web.sitemap` é refletida imediatamente na seção lições na coluna à esquerda.


![A seção de lições agora inclui um Link para &quot;controlar problemas de nomenclatura de ID&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Figura 02**: A seção de lições agora inclui um Link para "Problemas de nomenclatura de ID de controle"


## <a name="step-2-examining-the-renderedidchanges"></a>Etapa 2: Examinar o renderizado`ID`alterações

Para entender melhor as modificações do ASP.NET mecanismo faz para o renderizado `id` controla valores do servidor, vamos adicionar alguns controles da Web para o `IDIssues.aspx` página e, em seguida, visualize a marcação renderizada enviada ao navegador. Especificamente, digite o texto "Insira sua idade:" seguido por um controle de caixa de texto. Ainda mais para baixo na página adicione um controle da Web de botão e um controle de Web de rótulo. Defina a caixa de texto `ID` e `Columns` propriedades a serem `Age` e 3, respectivamente. Definir o botão `Text` e `ID` propriedades como "Submit" e `SubmitButton`. Desmarque out do rótulo `Text` propriedade e defina sua `ID` para `Results`.

Neste ponto marcação declarativa do controle de conteúdo deve ser semelhante ao seguinte:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Figura 3 mostra a página quando visualizado no designer do Visual Studio.


[![A página inclui três controles da Web: uma caixa de texto, botão e Label](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Figura 03**: A página inclui três controles da Web: uma caixa de texto, botão e Label ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-cs/_static/image5.png))


Visite a página por meio de um navegador e, em seguida, exibir o código-fonte HTML. Como a marcação a seguir mostra, o `id` os valores dos elementos HTML para os controles TextBox, Button e Web de rótulo são uma combinação da `ID` valores dos controles da Web e o `ID` valores dos contêineres de nomenclatura na página.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Conforme mencionado anteriormente neste tutorial, a página mestra e seu ContentPlaceHolders servem como contêineres de nomenclatura. Consequentemente, ambos contribuem com o renderizado `ID` valores de seus controles aninhados. Levar a caixa de texto `id` de atributo, por exemplo: `ctl00_MainContent_Age`. Lembre-se de que o controle de caixa de texto `ID` valor era `Age`. Isso é prefixado com seu controle de ContentPlaceHolder `ID` valor, `MainContent`. Além disso, esse valor é prefixado com a página mestra `ID` valor, `ctl00`. O efeito líquido é uma `id` valor de atributo que consiste o `ID` valores de página mestra, controle ContentPlaceHolder e a caixa de texto em si.

Figura 4 ilustra esse comportamento. Para determinar o renderizado `id` do `Age` caixa de texto, comece com o `ID` o valor do controle TextBox, `Age`. Em seguida, faz o caminho até a hierarquia de controle. Em cada contêiner de nomenclatura (esses nós com uma cor de pêssego), de prefixo atual renderizado `id` com o contêiner de nomenclatura `id`.


![Os atributos de id renderizado são com base na valores de ID dos contêineres de nomenclatura](control-id-naming-in-content-pages-cs/_static/image6.png)

**Figura 04**: O renderizado `id` atributos são baseadas no `ID` valores dos contêineres de nomenclatura


> [!NOTE]
> Conforme discutimos, a `ctl00` parte de renderizado `id` atributo constitui a `ID` valor a página mestra, mas você pode estar se perguntando como este `ID` valor surgiu. Não especificamos-lo em qualquer lugar em nossa página mestre ou conteúda. A maioria dos controles de servidor em uma página ASP.NET são adicionados explicitamente por meio de marcação declarativa da página. O `MainContent` controle ContentPlaceHolder foi especificada explicitamente na marcação de `Site.master`; o `Age` caixa de texto foi definida `IDIssues.aspx`da marcação. Podemos especificar o `ID` valores para esses tipos de controles na janela Propriedades ou da sintaxe declarativa. Outros controles, como a página mestra em si, não estão definidos na marcação declarativa. Consequentemente, seus `ID` valores devem ser gerados automaticamente para nós. Os conjuntos de mecanismo do ASP.NET a `ID` valores em tempo de execução para esses controles cujos IDs não tem sido definidos explicitamente. Ele usa o padrão de nomenclatura `ctlXX`, onde *XX* é um valor inteiro crescentes e consecutivos.


Porque a página mestra em si serve como um contêiner de nomenclatura, os controles da Web definidos na página mestre também tem alterado renderizado `id` valores de atributo. Por exemplo, o `DisplayDate` rótulo que adicionamos a página mestra a [ *criar um Layout de todo o Site com páginas mestras* ](creating-a-site-wide-layout-using-master-pages-cs.md) tutorial tem a seguinte marcação processada:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Observe que o `id` atributo inclui a ambas da página mestra `ID` valor (`ctl00`) e o `ID` valor do controle da Web de rótulo (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Etapa 3: Programaticamente referenciando controles da Web por meio de`FindControl`

Cada controle de servidor ASP.NET inclui um `FindControl("controlID")` método que localiza os descendentes do controle para um controle chamado *controlID*. Se um controle desse tipo for encontrado, ele é retornado; Se nenhum controle correspondente for encontrado, `FindControl` retorna `null`.

`FindControl` é útil em cenários em que você precisa acessar um controle, mas você não tiver uma referência direta a ele. Ao trabalhar com dados a controles da Web, como o controle GridView, por exemplo, os controles dentro dos campos do GridView são definidos uma vez na sintaxe declarativa, mas no tempo de execução, uma instância do controle é criada para cada linha de GridView. Consequentemente, existem os controles gerados em tempo de execução, mas não temos uma referência direta disponível na classe code-behind. Como resultado, precisamos usar `FindControl` para trabalhar programaticamente com um controle específico dentro dos campos do GridView. (Para obter mais informações sobre como usar `FindControl` para acessar os controles dentro de modelos de um controle da Web de dados, consulte [formatação com base em dados personalizados](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Esse mesmo cenário ocorre ao adicionar dinamicamente controles da Web a um formulário da Web, um tópico discutido [criação dinâmica dados de entrada de Interfaces do usuário](https://msdn.microsoft.com/library/aa479330.aspx).

Para ilustrar usando o `FindControl` método para procurar controles dentro de uma página de conteúdo, criar um manipulador de eventos para o `SubmitButton`do `Click` eventos. No manipulador de eventos, adicione o código a seguir, que faz referência a programaticamente os `Age` caixa de texto e `Results` rotular usando o `FindControl` método e, em seguida, exibe uma mensagem no `Results` com base na entrada do usuário.

> [!NOTE]
> É claro, não precisamos usar `FindControl` para referenciar os controles de rótulo e a caixa de texto para este exemplo. Podemos pode referenciá-los diretamente por meio de seus `ID` valores de propriedade. Posso usar `FindControl` aqui para ilustrar o que acontece quando uso `FindControl` de uma página de conteúdo.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Embora a sintaxe usada para chamar o `FindControl` método difere ligeiramente nas duas primeiras linhas do `SubmitButton_Click`, eles são semanticamente equivalentes. Lembre-se que todos os controles de servidor ASP.NET incluem uma `FindControl` método. Isso inclui o `Page` classe, do ASP.NET que todas as classes code-behind devem derivar. Portanto, chamar `FindControl("controlID")` é equivalente a chamar `Page.FindControl("controlID")`, supondo que você ainda não tiver substituído o `FindControl` método em sua classe code-behind ou em uma classe base personalizada.

Depois de inserir esse código, visite o `IDIssues.aspx` página por meio de um navegador, insira sua idade e clique no botão "Enviar". Ao clicar no botão "Enviar" um `NullReferenceException` é gerado (consulte a Figura 5).


[![Uma NullReferenceException é gerada](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Figura 05**: uma `NullReferenceException` é gerado ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-cs/_static/image9.png))


Se você definir um ponto de interrupção na `SubmitButton_Click` manipulador de eventos, você verá que as duas chamadas para `FindControl` retornar um `null` valor. O `NullReferenceException` é gerado ao tentar acessar o `Age` da caixa de texto `Text` propriedade.

O problema é que `Control.FindControl` pesquisa somente *controle*do que são descendentes *no mesmo contêiner de nomenclatura*. Como a página mestra constitui um novo contêiner de nomenclatura, uma chamada para `Page.FindControl("controlID")` nunca se interpõe entre o objeto de página mestra `ctl00`. (Consulte a Figura 4 para exibir a hierarquia de controle, que mostra a `Page` objeto como o pai do objeto da página mestra `ctl00`.) Portanto, o `Results` rótulo e `Age` caixa de texto não são encontrados e `ResultsLabel` e `AgeTextBox` são atribuídas aos valores de `null`.

Há duas soluções alternativas para esse desafio: podemos pode fazer drill down, um contêiner de nomenclatura por vez, para o controle apropriado. ou podemos criar nossos próprios `FindControl` método que permeia recipientes de nomenclatura. Vamos examinar cada uma dessas opções.

### <a name="drilling-into-the-appropriate-naming-container"></a>Detalhando o contêiner de nomenclatura apropriada

Para usar `FindControl` a referência a `Results` rótulo ou `Age` caixa de texto, precisamos chamar `FindControl` de um controle de ancestral no mesmo contêiner de nomenclatura. Conforme a Figura 4 mostrou, o `MainContent` controle ContentPlaceHolder é o ancestral somente `Results` ou `Age` que esteja dentro do mesmo contêiner de nomenclatura. Em outras palavras, chamando o `FindControl` método da `MainContent` controle, conforme mostrado no trecho de código abaixo, retorna uma referência ao corretamente a `Results` ou `Age` controles.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

No entanto, podemos não é possível trabalhar com o `MainContent` ContentPlaceHolder da classe de code-behind da página de nosso conteúdo usando a sintaxe acima como o ContentPlaceHolder é definido na página mestra. Em vez disso, temos de usar `FindControl` para obter uma referência ao `MainContent`. Substitua o código no `SubmitButton_Click` manipulador de eventos com as seguintes modificações:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Se você visitar a página por meio de um navegador, insira sua idade e clique no botão "Enviar", um `NullReferenceException` é gerado. Se você definir um ponto de interrupção na `SubmitButton_Click` manipulador de eventos, você verá que essa exceção ocorre ao tentar chamar o `MainContent` do objeto `FindControl` método. O `MainContent` objeto está `null` porque o `FindControl` método não é possível localizar um objeto denominado "MainContent". O motivo subjacente é o mesmo que o `Results` rótulo e `Age` controles de caixa de texto: `FindControl` inicia a pesquisa na parte superior da hierarquia de controle e não invadir a contêineres de nomenclatura, mas o `MainContent` ContentPlaceHolder é dentro da página mestra, que é um contêiner de nomenclatura.

Antes que podemos usar `FindControl` para obter uma referência ao `MainContent`, precisamos primeiro uma referência ao controle de página mestra. Assim que tivermos uma referência para a página mestra podemos obter uma referência para o `MainContent` ContentPlaceHolder por meio `FindControl` e, a partir daí, as referências ao `Results` rótulo e `Age` caixa de texto (novamente, por meio do usando `FindControl`). Mas como podemos obter uma referência para a página mestra? Inspecionando os `id` atributos na marcação renderizada é evidente que a página mestra `ID` valor é `ctl00`. Portanto, poderíamos usar `Page.FindControl("ctl00")` para obter uma referência para a página mestra, em seguida, usar esse objeto para obter uma referência ao `MainContent`e assim por diante. O trecho a seguir ilustra essa lógica:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Embora esse código funcionará sem dúvida, ele pressupõe que gerado automaticamente da página mestra `ID` sempre será `ctl00`. Nunca é uma boa ideia fazer suposições sobre os valores gerados automaticamente.

Felizmente, uma referência para a página mestra é acessível por meio de `Page` da classe `Master` propriedade. Portanto, em vez de ter que usar `FindControl("ctl00")` para obter uma referência da página mestra para acessar o `MainContent` ContentPlaceHolder, em vez disso, usamos `Page.Master.FindControl("MainContent")`. Atualização de `SubmitButton_Click` manipulador de eventos com o código a seguir:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Neste momento, visitando a página por meio de um navegador, inserindo sua idade e clicando no botão "Enviar" exibe a mensagem no `Results` rotular, conforme o esperado.


[![A idade do usuário é exibida no rótulo](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Figura 06**: idade do usuário é exibida no rótulo ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Pesquisar por meio de contêineres de nomenclatura recursivamente

O motivo pelo qual o exemplo de código anterior referenciado a `MainContent` controle ContentPlaceHolder da página mestra e, em seguida, o `Results` rótulo e `Age` controles TextBox de `MainContent`, é porque o `Control.FindControl` método somente pesquisará dentro de *controle*nomenclatura do contêiner. Tendo `FindControl` fique dentro do contêiner de nomenclatura faz sentido na maioria dos cenários, porque os dois controles em dois contêineres de nomenclatura diferentes podem ter o mesmo `ID` valores. Considere o caso de um controle GridView que define um controle de rótulo Web chamado `ProductName` dentro de um dos seus TemplateFields. Quando os dados serão associados ao GridView em tempo de execução, um `ProductName` etiqueta é criada para cada linha de GridView. Se `FindControl` pesquisado por meio de nomenclatura de todos os contêineres e nós chamados `Page.FindControl("ProductName")`, que instância de rótulo deve o `FindControl` retornar? O `ProductName` rótulo na primeira linha GridView? Aquele na última linha?

Então, ter `Control.FindControl` pesquisar apenas *controle*de nomenclatura do contêiner faz sentido na maioria dos casos. Mas há outros casos, como aquele voltados para nós, onde temos um único `ID` em todos os contêineres de nomenclatura e quiser evitar ter que referenciar meticulosamente cada contêiner de nomenclatura na hierarquia de controle para um controle de acesso. Tendo um `FindControl` variante que pesquisa recursivamente todos os contêineres de nomenclatura faz sentido, muito. Infelizmente, o .NET Framework não inclui um método desse tipo.

A boa notícia é que podemos criar nossos próprios `FindControl` método esse recursivamente procura todos os contêineres de nomenclatura. Na verdade, usando *métodos de extensão* podemos pode acrescentar uma `FindControlRecursive` método para o `Control` classe para acompanhar as existentes `FindControl` método.

> [!NOTE]
> Métodos de extensão são um recurso novo c# 3,0 e Visual Basic 9, quais são os idiomas que acompanham o .NET Framework versão 3.5 e o Visual Studio 2008. Em resumo, os métodos de extensão permitem que um desenvolvedor crie um novo método para um tipo de classe existente por meio de uma sintaxe especial. Para obter mais informações sobre este recurso útil, consulte meu artigo [estender funcionalidade do tipo Base com métodos de extensão](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Para criar o método de extensão, adicione um novo arquivo para o `App_Code` pasta chamada `PageExtensionMethods.cs`. Adicionar um método de extensão denominado `FindControlRecursive` que usa como entrada uma `string` parâmetro chamado `controlID`. Para métodos de extensão funcione corretamente, é vital que a própria classe e seus métodos de extensão sejam marcadas `static`. Além disso, todos os métodos de extensão devem aceitar como seu primeiro parâmetro um objeto do tipo ao qual o método de extensão se aplica e esse parâmetro de entrada devem ser precedido com a palavra-chave `this`.

Adicione o seguinte código para o `PageExtensionMethods.cs` arquivo de classe para definir essa classe e o `FindControlRecursive` método de extensão:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Com esse código funcionando, retornar para o `IDIssues.aspx` classe code-behind da página e comente atual `FindControl` chamadas de método. Substitua-as por chamadas para `Page.FindControlRecursive("controlID")`. O que é interessante sobre os métodos de extensão é que eles aparecem diretamente em listas de lista suspensa do IntelliSense. Como mostra a Figura 7, quando você digitar página e, em seguida, pressionar período, o `FindControlRecursive` método está incluído o IntelliSense lista suspensa, juntamente com os outros `Control` métodos de classe.


[![Métodos de extensão são incluídos no IntelliSense suspensos](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Figura 07**: métodos de extensão são incluídos no IntelliSense suspensas ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-cs/_static/image15.png))


Digite o seguinte código para o `SubmitButton_Click` manipulador de eventos e, em seguida, testá-lo visitando a página, inserindo sua idade e clicar no botão "Enviar". Conforme mostrado na Figura 6, a saída resultante será a mensagem, "Você está anos de idade!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Como os métodos de extensão são novos para c# 3,0 e Visual Basic 9, se você estiver usando o Visual Studio 2005, é possível usar métodos de extensão. Em vez disso, você precisará implementar o `FindControlRecursive` método em uma classe auxiliar. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) tem um exemplo em sua postagem de blog, [páginas mestra do ASP.NET e `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Etapa 4: Usar correto`id`valor no Script do lado do cliente do atributo

Conforme observado na introdução deste tutorial, um controle da Web processado do `id` atributo muitas vezes, é usado no script do lado do cliente para fazer referência a um determinado elemento HTML por meio de programação. Por exemplo, o JavaScript a seguir faz referência a um elemento HTML por seu `id` e, em seguida, exibe seu valor em uma caixa de mensagem modal:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Lembre-se que, no ASP.NET, páginas que não incluem do contêiner de nomeação, o elemento HTML renderizado `id` atributo é idêntico do controle de Web `ID` valor da propriedade. Por isso, é tentador embutir no código em `id` valores de atributo em código JavaScript. Ou seja, se você souber você deseja acessar o `Age` controle de caixa de texto Web por meio de script do lado do cliente, fazer isso por meio de uma chamada para `document.getElementById("Age")`.

O problema com essa abordagem é que, ao usar as páginas mestras (ou outros controles de contêiner de nomenclatura), o HTML renderizado `id` não é o mesmo com o controle de Web `ID` propriedade. Pode ser sua primeira reação visite a página por meio de um navegador e exibir o código-fonte para determinar o valor real `id` atributo. Se você souber o renderizado `id` valor, você pode colá-lo na chamada para `getElementById` para acessar o elemento HTML que você precisa para trabalhar com por meio de script do lado do cliente. Essa abordagem é inferior ao ideal porque determinadas alterações para a página de hierarquia de controle ou alterações a `ID` alterarão as propriedades dos controles de nomenclatura resultante `id` atributo, interromper, assim, seu código JavaScript.

A boa notícia é que o `id` valor de atributo que é renderizado é acessível no código do lado do servidor por meio do controle da Web [ `ClientID` propriedade](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Você deve usar essa propriedade para determinar o `id` usado no script do lado do cliente de valor do atributo. Por exemplo, para adicionar uma função JavaScript à página que, quando chamado, exibe o valor da `Age` caixa de texto em uma caixa de mensagem modal, adicione o seguinte código para o `Page_Load` manipulador de eventos:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

O código acima, o valor de injeta a `Age` propriedade de ClientID da caixa de texto para a chamada JavaScript a `getElementById`. Se você visitar esta página por meio de um navegador e exibir o código-fonte HTML, você encontrará o código JavaScript a seguir:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Observe como o correto `id` valor de atributo, `ctl00_MainContent_Age`, é exibido dentro da chamada para `getElementById`. Como esse valor é calculado em tempo de execução, ele funciona independentemente das alterações posteriores a hierarquia de controle de página.

> [!NOTE]
> Este exemplo de JavaScript simplesmente mostra como adicionar uma função JavaScript que referencia corretamente o elemento HTML renderizado por um controle de servidor. Para usar essa função, você precisaria criar JavaScript adicional para chamar a função quando o documento for carregado ou quando ocorre alguma ação de usuário específico. Para obter mais informações sobre essas e tópicos relacionados, leia [trabalhando com Script do lado do cliente](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Resumo

Determinados controles de servidor ASP.NET atuam como contêineres de nomenclatura, que afeta o renderizado `id` atributo valores de seus controles descendentes, bem como o escopo dos controles canvassed pelo `FindControl` método. Com relação a páginas mestras, a página mestra em si e seus controles ContentPlaceHolder nomeando contêineres. Consequentemente, precisamos colocar estabelecido um pouco mais trabalhoso para referenciar programaticamente os controles dentro da página de conteúdo usando `FindControl`. Neste tutorial, examinamos duas técnicas: detalhamento para o controle ContentPlaceHolder e chamar seu `FindControl` método; e sem interrupção nosso próprio `FindControl` implementação que recursivamente pesquisa por todos os contêineres de nomenclatura.

Além dos problemas do lado do servidor recipientes de nomenclatura introduzem com relação à referência de controles da Web, também há problemas do lado do cliente. Na ausência de contêineres, o controle da Web de nomenclatura `ID` o valor da propriedade e renderizado `id` valor de atributo são os mesmos. Mas com a adição de um contêiner de nomenclatura, renderizado `id` atributo inclui tanto o `ID` valores de controle da Web e os contêineres de nomenclatura no ancestral do sua hierarquia de controle. Essas preocupações de nomenclatura são um não problema, desde que você usar o controle de Web `ClientID` propriedade para determinar o renderizado `id` valor em seu script do lado do cliente do atributo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Páginas mestras do ASP.NET e `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Criar Interfaces do usuário de entrada de dados dinâmicos](https://msdn.microsoft.com/library/aa479330.aspx)
- [Estendendo a funcionalidade do tipo Base com métodos de extensão](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Como: Fazer referência a conteúdo da página mestra ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [De formação páginas: Dicas, truques e armadilhas](http://www.odetocode.com/articles/450.aspx)
- [Trabalhando com o Script do lado do cliente](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Zack Jones e Suchi Barnerjee. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](urls-in-master-pages-cs.md)
> [Próximo](interacting-with-the-master-page-from-the-content-page-cs.md)
