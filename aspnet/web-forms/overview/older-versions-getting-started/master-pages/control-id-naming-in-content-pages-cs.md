---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Controlar a nomenclatura de ID em páginas de conteúdo (c#) | Microsoft Docs
author: rick-anderson
description: Ilustra como os controles ContentPlaceHolder servem como um contêiner de nomenclatura e, portanto, verifique trabalhando programaticamente com um controle difícil (via FindConrol)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e834c38457c8477e0c81598d32f1e98473949d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="control-id-naming-in-content-pages-c"></a>ID de controle de nomenclatura em páginas de conteúdo (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Ilustra como os controles ContentPlaceHolder servem como um contêiner de nomenclatura e, portanto, verifique trabalhando programaticamente com um controle difícil (via FindConrol). Analisa esse problema e soluções alternativas. Também descreve como acessar programaticamente o valor ClientID resultante.


## <a name="introduction"></a>Introdução

Todos os controles de servidor ASP.NET incluem um `ID` propriedade que identifica o controle exclusivamente e é o meio pelo qual o controle é acessado por meio de programação na classe code-behind. Da mesma forma, os elementos em um documento HTML podem incluir um `id` atributo que identifica exclusivamente o elemento; esses `id` valores geralmente são usados no script do lado do cliente para fazer referência a um elemento HTML particular programaticamente. Dito isto, você pode presumir que quando um controle de servidor ASP.NET é renderizado em HTML, seu `ID` valor é usado como o `id` valor do elemento HTML renderizado. Isso não é necessariamente o caso como em determinadas circunstâncias uma única controlar com um único `ID` valor pode aparecer várias vezes em marcação renderizada. Considere a possibilidade de um controle GridView que inclui um TemplateField com um controle de Web de rótulo com um `ID` valor ProductName. Quando o GridView é associado à fonte de dados em tempo de execução, este rótulo é repetido uma vez para cada linha em GridView. Cada renderizado necessidades de rótulo em uma única `id` valor.

Para lidar com esses cenários, o ASP.NET permite que determinados controles para ser marcado como contêiners de nomeação. Um contêiner de nomenclatura serve como um novo `ID` namespace. Os controles de servidor que aparecem no contêiner de nomeação têm seus renderizado `id` valor prefixado com o `ID` do controle no contêiner de nomenclatura. Por exemplo, o `GridView` e `GridViewRow` classes são ambos os contêineres de nomenclatura. Consequentemente, um controle de rótulo definido em um GridView TemplateField com `ID` ProductName recebe um renderizado `id` valor `GridViewID_GridViewRowID_ProductName`. Porque *GridViewRowID* é exclusivo para cada linha em GridView, resultante `id` valores sejam exclusivos.

> [!NOTE]
> O [ `INamingContainer` interface](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) é usado para indicar que um determinado controle de servidor ASP.NET deve funcionar como um contêiner de nomenclatura. O `INamingContainer` interface ortográfica não-out de todos os métodos que o controle de servidor deve implementar; em vez disso, ele é usado como um marcador. Para gerar a marcação renderizada, se um controle implementa esta interface, em seguida, o mecanismo do ASP.NET automaticamente prefixos seu `ID` valor para seus descendentes processado `id` valores de atributo. Esse processo é abordado em mais detalhes na etapa 2.


Contêineres de nomenclatura não apenas alterar o renderizado `id` valor de atributo, mas também afeta como o controle pode ser referenciado por meio de programação de classe de code-behind da página ASP.NET. O `FindControl("controlID")` método normalmente é usado para referenciar programaticamente um controle da Web. No entanto, `FindControl` não entrem por meio de contêineres de nomenclatura. Consequentemente, você não pode usar diretamente o `Page.FindControl` método para fazer referência a controles em um controle GridView ou outro contêiner de nomenclatura.

Como você pode ter imaginado, páginas mestras e ContentPlaceHolders são implementados como contêineres de nomenclatura. Neste tutorial, podemos examinar como mestre páginas afetam HTML elemento `id` valores e maneiras de referenciar programaticamente os controles da Web dentro de uma página de conteúdo usando `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Etapa 1: Adicionar uma nova página ASP.NET

Para demonstrar os conceitos abordados neste tutorial, vamos adicionar uma nova página ASP.NET para o site. Criar uma nova página de conteúdo chamada `IDIssues.aspx` na pasta raiz, associando-a para o `Site.master` página mestra.


![Adicionar o IDIssues.aspx página conteúdo para a pasta raiz](control-id-naming-in-content-pages-cs/_static/image1.png)

**Figura 01**: Adicionar página de conteúdo `IDIssues.aspx` para a pasta raiz


Visual Studio cria automaticamente um controle de conteúdo para cada quatro ContentPlaceHolders da página mestra. Conforme observado no [ *ContentPlaceHolders vários e conteúdo padrão* ](multiple-contentplaceholders-and-default-content-cs.md) tutorial, se não houver um controle de conteúdo de conteúdo de ContentPlaceHolder da página mestra padrão é emitido em vez disso. Porque o `QuickLoginUI` e `LeftColumnContent` ContentPlaceHolders contêm marcação padrão adequado para essa página, vá em frente e remover seus correspondente controles de conteúdo do `IDIssues.aspx`. Neste ponto, marcação declarativa da página de conteúdo deve ter a seguinte aparência:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

No [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial criamos uma classe base de página personalizado (`BasePage`) que configura automaticamente o título da página se ele for não foi explicitamente definido. Para o `IDIssues.aspx` página implantar essa funcionalidade, a classe de code-behind da página deve derivar do `BasePage` classe (em vez de `System.Web.UI.Page`). Modificar a definição da classe por trás do código para que ele se parece com o seguinte:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Por fim, atualize o `Web.sitemap` arquivo para incluir uma entrada para esta lição novo. Adicionar um `<siteMapNode>` elemento e defina seu `title` e `url` atributos para "Problemas de nomenclatura de ID de controle" e `~/IDIssues.aspx`, respectivamente. Depois de fazer essa adição seu `Web.sitemap` marcação do arquivo deve ser semelhante à seguinte:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Conforme ilustrado na Figura 2, a nova entrada de mapa de site em `Web.sitemap` imediatamente é refletido na seção lições na coluna esquerda.


![A seção de lições agora inclui um Link para &quot;problemas de nomenclatura de identificação de controle&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Figura 02**: A seção de lições agora inclui um Link para "Problemas de nomenclatura de ID de controle"


## <a name="step-2-examining-the-renderedidchanges"></a>Etapa 2: Examinando o renderizado`ID`alterações

Para entender melhor as modificações do ASP.NET mecanismo faz o renderizado `id` valores do servidor controla, vamos adicionar alguns controles da Web para o `IDIssues.aspx` página e, em seguida, exibir a marcação renderizada enviada para o navegador. Especificamente, digite o texto "Insira sua idade:" seguido por um controle TextBox Web. Ainda mais para baixo na página adicione um controle de botão Web e um rótulo Web. Defina a caixa de texto `ID` e `Columns` propriedades `Age` e 3, respectivamente. Definir o botão `Text` e `ID` propriedades para "Submit" e `SubmitButton`. Limpe-out do rótulo `Text` propriedade e defina seu `ID` para `Results`.

Neste ponto declarativo do controle de conteúdo deve ser semelhante ao seguinte:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

A Figura 3 mostra a página quando exibida pelo designer do Visual Studio.


[![A página inclui três controles da Web: uma caixa de texto, botão e rótulo](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Figura 03**: A página inclui três controles da Web: uma caixa de texto, o botão e o rótulo ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-cs/_static/image5.png))


Visite a página por meio de um navegador e, em seguida, exibir o código-fonte HTML. Como a marcação abaixo mostra, o `id` os valores dos elementos HTML para os controles de caixa de texto, botão e da Web de rótulo são uma combinação da `ID` valores de controles da Web e o `ID` valores dos contêineres de nomenclatura na página.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Conforme observado anteriormente neste tutorial, a página mestra e seu ContentPlaceHolders servem como contêineres de nomenclatura. Consequentemente, ambos contribuem renderizado `ID` valores de seus controles aninhados. Levar a caixa de texto `id` atributo, por exemplo: `ctl00_MainContent_Age`. Lembre-se de que o controle de caixa de texto `ID` valor era `Age`. Isso é prefixado com o controle de ContentPlaceHolder `ID` valor, `MainContent`. Além disso, esse valor é prefixado com a página mestra `ID` valor, `ctl00`. O efeito líquido é uma `id` valor do atributo que o `ID` valores de página mestra, o controle ContentPlaceHolder e a caixa de texto em si.

A Figura 4 ilustra esse comportamento. Para determinar o renderizado `id` do `Age` caixa de texto, comece com o `ID` o valor do controle TextBox, `Age`. Em seguida, vai até a hierarquia de controle. Em cada contêiner de nomenclatura (esses nós com uma cor de pêssego), prefixo atual renderizado `id` com o contêiner de nomeação `id`.


![Os atributos de id renderizados são com base no valores de ID dos contêineres de nomenclatura](control-id-naming-in-content-pages-cs/_static/image6.png)

**Figura 04**: renderizado o `id` atributos são com base no `ID` valores dos contêineres de nomenclatura


> [!NOTE]
> Conforme abordado, o `ctl00` parte de renderizado `id` atributo constitui a `ID` valor da página mestra, mas você deve estar se perguntando como este `ID` valor foi criado. Não especificamos-lo em qualquer lugar na nossa página mestre ou conteúda. A maioria dos controles de servidor em uma página ASP.NET são adicionados explicitamente por meio de marcação declarativa da página. O `MainContent` controle ContentPlaceHolder foi especificado explicitamente na marcação de `Site.master`; o `Age` caixa de texto foi definida `IDIssues.aspx`da marcação. Podemos especificar o `ID` valores para esses tipos de controles usando a janela Propriedades ou a sintaxe declarativa. Outros controles, como a página mestra em si, não estão definidos na marcação declarativa. Consequentemente, seus `ID` valores devem ser gerados automaticamente para nós. Os conjuntos de mecanismo ASP.NET o `ID` valores em tempo de execução para esses controles cujos IDs não tiverem sido definidos explicitamente. Ele usa o padrão de nomenclatura `ctlXX`, onde *XX* é um valor inteiro crescentes.


Como o mestre de página em si serve como um contêiner de nomeação, os controles da Web definidos na página mestre também tem alterado renderizado `id` valores de atributo. Por exemplo, o `DisplayDate` rótulo são adicionados para a página mestra a [ *criar um Layout de todo o Site com páginas mestras* ](creating-a-site-wide-layout-using-master-pages-cs.md) tutorial tem o seguinte marcação processado:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Observe que o `id` atributo inclui os dois da página mestra `ID` valor (`ctl00`) e o `ID` valor do controle da Web do rótulo (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Etapa 3: Programaticamente controles da Web por meio de referência`FindControl`

Cada controle de servidor ASP.NET inclui um `FindControl("controlID")` método que procura os descendentes do controle para um controle chamado *controlID*. Se um controle como for encontrado, ele é retornado; Se nenhum controle correspondente for encontrado, `FindControl` retorna `null`.

`FindControl` é útil em cenários em que é necessário um controle de acesso, mas você não tem uma referência direta a ele. Ao trabalhar com dados de controles da Web como o GridView, por exemplo, os controles dentro dos campos do GridView são definidos uma vez na sintaxe declarativa, mas no tempo de execução de uma instância do controle é criada para cada linha em GridView. Consequentemente, os controles gerados em tempo de execução existem, mas não temos uma referência direta disponível na classe code-behind. Como resultado, precisamos usar `FindControl` para trabalhar programaticamente com um controle específico dentro dos campos do GridView. (Para obter mais informações sobre como usar `FindControl` para acessar os controles nos modelos de um controle da Web de dados, consulte [personalizado formatação com base em dados](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Esse mesmo cenário ocorre ao adicionar dinamicamente controles da Web a um formulário da Web, um tópico discutido em [criação dinâmica dados de entrada de Interfaces de usuário](https://msdn.microsoft.com/library/aa479330.aspx).

Para ilustrar o uso de `FindControl` método para procurar controles em uma página de conteúdo, crie um manipulador de eventos para o `SubmitButton`do `Click` evento. No manipulador de eventos, adicione o seguinte código, que faz referência a programaticamente o `Age` caixa de texto e `Results` rótulo usando o `FindControl` método e, em seguida, exibe uma mensagem em `Results` com base na entrada do usuário.

> [!NOTE]
> Obviamente, não precisamos usar `FindControl` para referenciar os controles de rótulo e caixa de texto para este exemplo. Podemos poderá fazer referência a eles diretamente por meio de seus `ID` valores de propriedade. Usar `FindControl` aqui para ilustrar o que acontece ao usar `FindControl` de uma página de conteúdo.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Embora a sintaxe usada para chamar o `FindControl` método difere ligeiramente nas duas primeiras linhas do `SubmitButton_Click`, eles são semanticamente equivalentes. Lembre-se de que todos os controles de servidor ASP.NET incluem um `FindControl` método. Isso inclui o `Page` do ASP.NET que todas as classes code-behind devem derivar de classe. Portanto, ao chamar `FindControl("controlID")` é equivalente a chamar `Page.FindControl("controlID")`, supondo que você não tiver substituído o `FindControl` método na sua classe code-behind ou em uma classe base personalizada.

Depois de inserir esse código, visite o `IDIssues.aspx` página através de um navegador, insira sua idade e clique no botão "Enviar". Ao clicar no botão "Enviar" um `NullReferenceException` é gerado (consulte a Figura 5).


[![É gerada uma NullReferenceException](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Figura 05**: um `NullReferenceException` é gerado ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-cs/_static/image9.png))


Se você definir um ponto de interrupção no `SubmitButton_Click` manipulador de eventos, você verá que as chamadas para `FindControl` retornar um `null` valor. O `NullReferenceException` é gerado ao tentar acessar o `Age` da caixa de texto `Text` propriedade.

O problema é que `Control.FindControl` pesquisa somente *controle*do descendentes são *no mesmo contêiner de nomeação*. Como a página mestra constitui um novo contêiner de nomenclatura, uma chamada para `Page.FindControl("controlID")` nunca se interpõe entre o objeto de página mestra `ctl00`. (Consulte a Figura 4 para exibir a hierarquia de controle, que mostra o `Page` objeto como o pai do objeto da página mestra `ctl00`.) Portanto, o `Results` rótulo e `Age` caixa de texto não são encontrados e `ResultsLabel` e `AgeTextBox` são atribuídos valores de `null`.

Há duas soluções alternativas para esse desafio: nós pode fazer drill down, um contêiner de nomenclatura por vez, para o controle apropriado. ou é possível criar nossa própria `FindControl` método que se interpõe entre contêineres de nomenclatura. Vamos examinar cada uma dessas opções.

### <a name="drilling-into-the-appropriate-naming-container"></a>Busca detalhada em um contêiner de nomeação apropriada

Para usar `FindControl` a referência a `Results` rótulo ou `Age` caixa de texto, precisamos chamar `FindControl` de um controle ancestral no mesmo contêiner de nomenclatura. Como a Figura 4 mostraram, o `MainContent` controle ContentPlaceHolder é o ancestral somente do `Results` ou `Age` que esteja dentro do mesmo contêiner de nomeação. Em outras palavras, chamar o `FindControl` método do `MainContent` controle, conforme mostrado no trecho de código abaixo, retorna uma referência ao corretamente o `Results` ou `Age` controles.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

No entanto, nós não funciona com o `MainContent` ContentPlaceHolder da classe de code-behind da nossa página de conteúdo usando a sintaxe acima como o ContentPlaceHolder é definido na página mestra. Em vez disso, precisamos usar `FindControl` para obter uma referência ao `MainContent`. Substitua o código no `SubmitButton_Click` manipulador de eventos com as seguintes modificações:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Se você visitar a página por meio de um navegador, insira sua idade e clique no botão "Enviar", um `NullReferenceException` é gerado. Se você definir um ponto de interrupção no `SubmitButton_Click` manipulador de eventos, você verá que essa exceção ocorre ao tentar chamar o `MainContent` do objeto `FindControl` método. O `MainContent` objeto `null` porque o `FindControl` método não é possível localizar um objeto denominado "MainContent". O motivo subjacente é o mesmo que o `Results` rótulo e `Age` controles TextBox: `FindControl` inicia a pesquisa da parte superior da hierarquia de controle e não entrem na nomenclatura contêineres, mas o `MainContent` ContentPlaceHolder é dentro da página mestra, que é um contêiner de nomenclatura.

Antes que possamos usar `FindControl` para obter uma referência para `MainContent`, é preciso primeiro uma referência para o controle de página mestra. Depois que temos uma referência para a página mestra podemos obter uma referência para o `MainContent` ContentPlaceHolder via `FindControl` e, a partir daí, as referências ao `Results` rótulo e `Age` caixa de texto (novamente, usando `FindControl`). Mas como podemos obter uma referência para a página mestra? Inspecionando o `id` atributos na marcação renderizada é evidente que a página mestra `ID` valor é `ctl00`. Portanto, podemos usar `Page.FindControl("ctl00")` para obter uma referência para a página mestra, em seguida, usar esse objeto para obter uma referência para `MainContent`, e assim por diante. O trecho a seguir ilustra essa lógica:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Enquanto esse código funcionará certamente, ele pressupõe que gerado automaticamente o da página mestra `ID` sempre será `ctl00`. Nunca é uma boa ideia fazer suposições sobre valores gerados automaticamente.

Felizmente, uma referência para a página mestra está acessível por meio de `Page` da classe `Master` propriedade. Portanto, em vez de usar `FindControl("ctl00")` para obter uma referência da página mestra para acessar o `MainContent` ContentPlaceHolder, podemos em vez disso, usar `Page.Master.FindControl("MainContent")`. Atualização de `SubmitButton_Click` manipulador de eventos com o código a seguir:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Neste momento, visitando a página por meio de um navegador, inserir sua idade e clicar no botão "Enviar" exibe a mensagem a `Results` rótulo, conforme o esperado.


[![A duração do usuário é exibida no rótulo](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Figura 06**: idade do usuário é exibida no rótulo ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Recursivamente pesquisar contêineres de nomenclatura

O motivo pelo qual o exemplo de código anterior referenciado a `MainContent` controle ContentPlaceHolder da página mestra e, em seguida, o `Results` rótulo e `Age` controles de caixa de texto de `MainContent`, é porque o `Control.FindControl` apenas o método de pesquisa dentro de *controle*do contêiner de nomeação. Tendo `FindControl` fique dentro do contêiner de nomenclatura faz sentido na maioria dos cenários, como dois controles em dois contêineres de nomenclatura diferentes podem ter o mesmo `ID` valores. Considere o caso de um controle GridView que define um controle de rótulo Web chamado `ProductName` dentro de um dos seus TemplateFields. Quando os dados serão associados à GridView em tempo de execução, um `ProductName` rótulo é criado para cada linha em GridView. Se `FindControl` pesquisado por meio de nomes de todos os contêineres e é chamado `Page.FindControl("ProductName")`, qual instância de rótulo deve o `FindControl` retornar? O `ProductName` rótulo na primeira linha GridView? Uma última linha?

Portanto `Control.FindControl` pesquisar apenas *controle*de nomenclatura do contêiner faz sentido na maioria dos casos. Mas há outros casos, como o voltada para nós, em que temos uma única `ID` em todos os contêiners de nomeação e para evitar a necessidade de referência meticulosa cada contêiner de nomenclatura na hierarquia de controle para um controle de acesso. Ter um `FindControl` variante pesquisas recursivamente todos os contêineres de nomenclatura faz sentido, muito. Infelizmente, o .NET Framework não inclui esse método um.

A boa notícia é que podemos criar nossos própria `FindControl` método que recursivamente procura todos os contêineres de nomenclatura. Na verdade, usando *métodos de extensão* é possível acrescentar um `FindControlRecursive` método para o `Control` classe para acompanhar a existente `FindControl` método.

> [!NOTE]
> Métodos de extensão são um recurso novo do c# 3.0 e o Visual Basic 9, quais são os idiomas que são fornecidos com o .NET Framework versão 3.5 e Visual Studio 2008. Em resumo, métodos de extensão permitem que um desenvolvedor criar um novo método para um tipo de classe existente por meio de uma sintaxe especial. Para obter mais informações sobre esse recurso útil, consulte o artigo [estender funcionalidade do tipo Base com métodos de extensão](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Para criar o método de extensão, adicione um novo arquivo para o `App_Code` pasta denominada `PageExtensionMethods.cs`. Adicionar um método de extensão denominado `FindControlRecursive` que usa como entrada um `string` parâmetro denominado `controlID`. Métodos de extensão funcione corretamente, é essencial que a própria classe e seus métodos de extensão marcadas `static`. Além disso, todos os métodos de extensão devem aceitar como seu primeiro parâmetro um objeto do tipo ao qual se aplica o método de extensão e este parâmetro de entrada devem ser precedido com a palavra-chave `this`.

Adicione o seguinte código para o `PageExtensionMethods.cs` arquivo de classe para definir essa classe e o `FindControlRecursive` método de extensão:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Com esse código, retornar o `IDIssues.aspx` classe code-behind da página e comente atual `FindControl` chamadas de método. Substituí-las por chamadas para `Page.FindControlRecursive("controlID")`. O que é interessante sobre métodos de extensão é que eles aparecem diretamente dentro da IntelliSense nas listas suspensas. Como mostra a Figura 7, quando você digitar página e, em seguida, pressionar período, o `FindControlRecursive` método foi incluído o IntelliSense suspensa junto com outros `Control` métodos de classe.


[![Métodos de extensão são incluídos nas IntelliSense listas suspensas](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Figura 07**: métodos de extensão são incluídos nas IntelliSense listas suspensas ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-cs/_static/image15.png))


Digite o seguinte código para o `SubmitButton_Click` manipulador de eventos e, em seguida, testá-lo a visitar a página, inserindo sua idade, e clicar no botão "Enviar". Conforme mostrado novamente na Figura 6, a saída resultante será a mensagem, "você anos de idade!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Como os métodos de extensão são novos para 3.0 c# e Visual Basic 9, se você estiver usando o Visual Studio 2005, você não pode usar métodos de extensão. Em vez disso, você precisará implementar o `FindControlRecursive` método em uma classe auxiliar. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) tem um exemplo em sua postagem de blog, [ASP.NET Maser páginas e `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Etapa 4: Usando os`id`valor no Script do lado do cliente do atributo

Conforme observado na introdução neste tutorial, um controle da Web do renderizado `id` atributo muitas vezes é usado no script do lado do cliente para fazer referência a um elemento HTML particular programaticamente. Por exemplo, o JavaScript a seguir faz referência a um elemento HTML por seu `id` e, em seguida, exibe o valor em uma caixa de mensagem modal:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Lembre-se que, no ASP.NET, páginas que não têm um contêiner de nomeação, o elemento HTML renderizado `id` atributo é idêntico do controle da Web `ID` o valor da propriedade. Por isso, é tentador codificar em `id` valores de atributo no código JavaScript. Ou seja, se você souber que você deseja acessar o `Age` controle TextBox Web por meio de script do lado do cliente, fazer isso por meio de uma chamada para `document.getElementById("Age")`.

O problema com essa abordagem é que, ao usar páginas mestras (ou outros controles de contêiner de nomenclatura), o HTML renderizado `id` não é sinônimo do controle da Web `ID` propriedade. Sua primeira opção pode ser para visitar a página por meio de um navegador e exibir o código-fonte para determinar o valor real `id` atributo. Depois que você sabe o renderizado `id` valor, você pode colá-lo na chamada a `getElementById` para acessar o elemento HTML que você precisa para trabalhar com por meio de script do lado do cliente. Essa abordagem é inferior ao ideal porque a hierarquia de controle de determinadas alterações para a página ou alterações a `ID` propriedades dos controles de nomenclatura alterará resultante `id` atributo, assim, seu código JavaScript de quebra.

A boa notícia é que o `id` valor de atributo que é renderizado está acessível no código do lado do servidor por meio do controle da Web [ `ClientID` propriedade](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Você deve usar essa propriedade para determinar a `id` usado no script do lado do cliente de valor de atributo. Por exemplo, para adicionar uma função JavaScript para a página que, quando chamado, exibe o valor da `Age` caixa de texto em uma caixa de mensagem modal, adicione o seguinte código para o `Page_Load` manipulador de eventos:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

O código acima, o valor de injeta o `Age` propriedade de ClientID da caixa de texto para a chamada de JavaScript para `getElementById`. Se você visitar a página através de um navegador e exibir o código-fonte HTML, você encontrará o seguinte código JavaScript:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Observe como o correto `id` valor do atributo, `ctl00_MainContent_Age`, é exibido na chamada para `getElementById`. Como esse valor é calculado em tempo de execução, ele funciona independentemente das alterações mais recente para a hierarquia de controle de página.

> [!NOTE]
> Este exemplo de JavaScript simplesmente mostra como adicionar uma função JavaScript que referencia corretamente o elemento HTML renderizado por um controle de servidor. Para usar essa função, você precisaria criar JavaScript adicional para chamar a função quando o documento for carregado ou quando ocorre uma ação de usuário específico. Para obter mais informações sobre esses tópicos relacionados, ler e [trabalhando com Script do lado do cliente](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Resumo

Determinados controles de servidor ASP.NET atuam como contêineres de nomenclatura, que afeta o renderizado `id` atributo valores de seus descendentes controles, bem como o escopo de controles canvassed pelo `FindControl` método. Em relação às páginas mestras, a página mestra próprio e seus controles ContentPlaceHolder estão nomeando contêineres. Consequentemente, é preciso colocar trabalho estabelecido um pouco mais para referenciar programaticamente os controles na página de conteúdo usando `FindControl`. Neste tutorial, examinamos duas técnicas: detalhamento no controle ContentPlaceHolder e chamar sua `FindControl` método; e implantar o nosso próprio `FindControl` implementação que recursivamente pesquisa todos os contêineres de nomenclatura.

Além dos problemas no servidor apresentam nomear contêineres em relação a referência de controles da Web, também há problemas no cliente. Na ausência de nomenclatura de contêineres, o controle da Web `ID` o valor da propriedade e renderizado `id` valor de atributo são os mesmos. Mas com a adição do contêiner de nomenclatura, renderizado `id` atributo inclui tanto o `ID` valores do controle da Web e os contêineres de nomenclatura no histórico da sua hierarquia de controle. Essas preocupações de nomenclatura são um problema, desde que você usar o controle de Web `ClientID` propriedade para determinar o renderizado `id` valor em seu script do lado do cliente do atributo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Páginas mestras do ASP.NET e `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Criando Interfaces de usuário de entrada de dados dinâmicos](https://msdn.microsoft.com/library/aa479330.aspx)
- [Estendendo a funcionalidade do tipo Base com métodos de extensão](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Como: Fazer referência a conteúdo da página mestra ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [De formação páginas: Dicas, truques e interceptações](http://www.odetocode.com/articles/450.aspx)
- [Trabalhando com Script do lado do cliente](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Zack Jones e Suchi Barnerjee. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](urls-in-master-pages-cs.md)
> [Próximo](interacting-with-the-master-page-from-the-content-page-cs.md)
