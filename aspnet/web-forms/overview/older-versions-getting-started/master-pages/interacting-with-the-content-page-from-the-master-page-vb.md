---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interagindo com a página de conteúdo através da página mestra (VB) | Microsoft Docs
author: rick-anderson
description: Examina como chamar métodos, definir propriedades, etc. da página de conteúdo do código na página mestra.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 2222cb67c1d327e43f6bb54b34a3241fbbcdaaa4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381303"
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interagindo com a página de conteúdo através da página mestra (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Examina como chamar métodos, definir propriedades, etc. da página de conteúdo do código na página mestra.


## <a name="introduction"></a>Introdução

O tutorial anterior examinado como fazer com que a página de conteúdo interagir programaticamente com sua página mestra. Lembre-se de que atualizamos a página mestra para incluir um controle GridView que são listados os cinco mais recentemente adicionado produtos. Em seguida, criamos uma página de conteúdo na qual o usuário pode adicionar um novo produto. Após a adição de um novo produto, a página de conteúdo necessário para instruir a página mestra para atualizar seu GridView para que ele incluiria o produto just-adicionado. Essa funcionalidade foi feita, adicionando um método público para a página mestra que atualizado os dados associados a GridView e, em seguida, invocando o método da página de conteúdo.

A forma mais comum de conteúdo e interação de página mestra é proveniente da página de conteúdo. No entanto, é possível para a página mestra para rouse a página de conteúdo atual em ação, e essa funcionalidade pode ser necessária se a página mestra contém elementos de interface do usuário que permitem que os usuários modifiquem dados também são exibidos na página de conteúdo. Considere uma página de conteúdo que exibe as informações de produtos em um GridView controle e controlar uma página mestra que inclui um botão que, quando clicado, dobrará os preços de todos os produtos. Muito semelhante ao exemplo do tutorial anterior, o GridView precisa ser atualizada após o preço duplo do que botão é clicado, de modo que ele mostre os novos preços, mas esse cenário é a página mestra que precise rouse a página de conteúdo em ação.

Este tutorial explora como fazer com que a página mestra invocar a funcionalidade definida na página de conteúdo.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Provocam interação programática por meio de um evento e manipuladores de eventos

Invocar a funcionalidade da página de conteúdo de uma página mestra é mais difícil do que o oposto. Como uma página de conteúdo tem uma única página mestra, quando provocam a interação programática da página de conteúdo, nós sabemos quais métodos e propriedades públicas são à nossa disposição. Uma página mestra, no entanto, pode ter muitos diferentes páginas de conteúdo, cada um com seu próprio conjunto de propriedades e métodos. Como, em seguida, podemos escrever código na página mestra para realizar alguma ação em sua página de conteúdo quando não sabemos qual página de conteúdo será invocada até que o tempo de execução?

Considere um controle de Web do ASP.NET, como o controle de botão. Um controle de botão pode aparecer em qualquer número de páginas ASP.NET e precisa de um mecanismo pelo qual ele pode alertar a página que foi clicado. Isso é feito usando *eventos*. Em particular, o controle de botão aciona seu `Click` eventos quando ele for clicado; a página do ASP.NET que contém o botão opcionalmente pode responder a essa notificação por meio de um *manipulador de eventos*.

Esse mesmo padrão pode ser usado para ter uma funcionalidade de gatilho de página mestra em suas páginas de conteúdo:

1. Adicione um evento para a página mestra.
2. Gere o evento sempre que a página mestra precisa se comunicar com sua página de conteúdo. Por exemplo, se a página mestra precisa de sua página de conteúdo de alerta que o usuário dobrou os preços, seu evento teria gerado imediatamente depois que os preços duplicou.
3. Crie um manipulador de eventos nessas páginas de conteúdo que precisa executar alguma ação.

O restante deste tutorial implementa o exemplo descrito na introdução; ou seja, uma página de conteúdo que lista os produtos no banco de dados e uma página mestra que inclui um botão de controle para duas vezes os preços.

## <a name="step-1-displaying-products-in-a-content-page"></a>Etapa 1: Exibindo produtos em uma página de conteúdo

Nossa tarefa prioritária é criar uma página de conteúdo que lista os produtos do banco de dados Northwind. (Adicionamos o banco de dados Northwind ao projeto no tutorial anterior, [ *interagindo com a página mestra através da página de conteúdo*](interacting-with-the-master-page-from-the-content-page-vb.md).) Comece adicionando uma nova página ASP.NET para o `~/Admin` pasta chamada `Products.aspx`, deixando de associá-lo para o `Site.master` página mestra. Figura 1 mostra o Gerenciador de soluções depois que esta página foi adicionada ao site.


[![Adicionar uma nova página ASP.NET para a pasta Admin](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Figura 01**: Adicione uma nova página ASP.NET para o `Admin` pasta ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


Lembre-se de que no [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial, criamos uma classe de página de base personalizada chamada `BasePage` que gera o título da página, se não for defina explicitamente. Vá para o `Products.aspx` de lógica da página de classe e fazer com que ele derivam `BasePage` (em vez do `System.Web.UI.Page`).

Por fim, atualize o `Web.sitemap` arquivo para incluir uma entrada para esta lição. Adicione a seguinte marcação abaixo o `<siteMapNode>` para o conteúdo a lição de interação de página mestra:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

A adição deste `<siteMapNode>` elemento é refletido nas lições lista (veja a Figura 5).

Retorne ao `Products.aspx`. No controle de conteúdo de `MainContent`, adicione um controle GridView e nomeie- `ProductsGrid`. Associar o GridView para um novo controle SqlDataSource chamado `ProductsDataSource`.


[![Associar o GridView para um novo controle SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Figura 02**: associar o GridView para um novo controle SqlDataSource ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


Configure o Assistente para que ele usa o banco de dados Northwind. Se você trabalhou com o tutorial anterior, você já deve ter uma cadeia de caracteres de conexão denominada `NorthwindConnectionString` em `Web.config`. Escolha essa cadeia de caracteres de conexão na lista suspensa, conforme mostrado na Figura 3.


[![Configurar o SqlDataSource para usar o banco de dados Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Figura 03**: configurar o SqlDataSource para usar o banco de dados Northwind ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


Em seguida, especifique o controle de fonte de dados `SELECT` instrução escolhendo a tabela de produtos na lista suspensa e retornando o `ProductName` e `UnitPrice` colunas (veja a Figura 4). Clique em Avançar e em seguida Concluir para concluir o Assistente Configurar fonte de dados.


[![Retornar os campos de UnitPrice e ProductName da tabela produtos](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Figura 04**: retornar o `ProductName` e `UnitPrice` campos dos `Products` tabela ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


Isso é tudo para ele! Depois de concluir o Assistente para o Visual Studio adiciona dois BoundFields ao GridView para espelhar os dois campos retornados pelo controle SqlDataSource. Marcação dos controles GridView e SqlDataSource segue. Figura 5 mostra os resultados quando exibidos por meio de um navegador.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![Cada produto e seu preço está listado em GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Figura 05**: cada produto e seu preço está listado no GridView ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> Fique à vontade limpar a aparência de GridView. Algumas sugestões incluem o valor de UnitPrice exibido como uma moeda de formatação e o uso de fontes e cores de plano de fundo para melhorar a aparência da grade. Para obter mais informações sobre como exibir e formatar dados no ASP.NET, consulte a minha [trabalhando com a série de tutoriais de dados](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Etapa 2: Adicionando um botão de preços de Double para a página mestra

Nossa próxima tarefa é adicionar um controle da Web de botão para o mestre de página que, quando clicado, será duas vezes o preço de todos os produtos no banco de dados. Abra o `Site.master` página mestra e arraste um botão da caixa de ferramentas para o Designer, colocando-a sob o `RecentProductsDataSource` controle SqlDataSource que adicionamos no tutorial anterior. Definir o botão `ID` propriedade para `DoublePrice` e seu `Text` propriedade como "Dupla os preços dos produtos".

Em seguida, adicione um controle SqlDataSource para a página mestra, nomeando- `DoublePricesDataSource`. Este SqlDataSource será usado para executar o `UPDATE` instrução duplicar todos os preços. Especificamente, precisamos definir seus `ConnectionString` e `UpdateCommand` propriedades para a cadeia de caracteres de conexão apropriado e `UPDATE` instrução. Em seguida, precisamos chamar deste controle SqlDataSource `Update` método quando o `DoublePrice` botão é clicado. Para definir a `ConnectionString` e `UpdateCommand` propriedades, selecione o controle SqlDataSource e, em seguida, vá para a janela de propriedades. O `ConnectionString` essas cadeias de caracteres de conexão já armazenadas em listas de propriedades `Web.config` em uma lista suspensa; escolha o `NorthwindConnectionString` opção conforme mostrado na Figura 6.


[![Configurar o SqlDataSource para usar o NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Figura 06**: configurar o SqlDataSource para usar o `NorthwindConnectionString` ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


Para definir o `UpdateCommand` propriedade, localize a opção UpdateQuery na janela Propriedades. Essa propriedade, quando selecionado, exibe um botão com reticências; Clique neste botão para exibir a caixa de diálogo Editor de comando e parâmetro, mostrada na Figura 7. Digite o seguinte `UPDATE` instrução na caixa de texto da caixa de diálogo:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Essa instrução, quando executada, dobre o `UnitPrice` valor para cada registro no `Products` tabela.


[![Definir propriedade de UpdateCommand do SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Figura 07**: defina SqlDataSource `UpdateCommand` propriedade ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


Depois de definir essas propriedades, a marcação declarativa de seu botão e SqlDataSource dos controles deve ser semelhante ao seguinte:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Tudo o que resta é chamar seu `Update` método quando o `DoublePrice` botão é clicado. Criar uma `Click` manipulador de eventos para o `DoublePrice` botão e adicione o seguinte código:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Para testar essa funcionalidade, visite o `~/Admin/Products.aspx` página podemos criado na etapa 1 e clique no botão "Double os preços dos produtos". Clicando no botão faz com que um postback e executa o `DoublePrice` do botão `Click` manipulador de eventos, dobrando os preços de todos os produtos. A página é renderizada novamente e a marcação é retornada e novamente exibida no navegador. O GridView na página de conteúdo, no entanto, lista os mesmos preços como antes do "Double os preços dos produtos" botão foi clicado. Isso ocorre porque os dados carregados inicialmente no GridView tinham o estado armazenado no estado de exibição, portanto, não será recarregada em postbacks, a menos que indicado o contrário. Se você visitar uma página diferente e, em seguida, retornar para o `~/Admin/Products.aspx` página você verá os preços atualizados.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Etapa 3: Gerar um evento quando o preços serão duplicadas

Porque o GridView no `~/Admin/Products.aspx` página não reflete imediatamente da duplicação de preço, um usuário pode pensar é compreensível que eles não clicar no botão de "Double os preços dos produtos", ou que não funcionou. Eles talvez tente clicar no botão mais algumas vezes, dobrando os preços e novamente. Para corrigir isso, precisamos ter a grade no conteúdo de exibição de página os novos preços imediatamente depois que eles estão duplicados.

Conforme discutido anteriormente neste tutorial, é necessário acionar um evento na página mestra sempre que o usuário clica o `DoublePrice` botão. Um evento é uma maneira para uma classe (um editor de eventos) notifique outro sobre um conjunto de outras classes (os assinantes do evento) que algo interessante ocorreu. Neste exemplo, a página mestra é o Editor de eventos; aqueles que se preocupam quando páginas de conteúdo a `DoublePrice` botão é clicado são os assinantes.

Uma classe assina um evento com a criação de um *manipulador de eventos*, que é um método que é executado em resposta ao evento sendo gerado. O fornecedor define os eventos que ele gera com a definição de um *delegado do evento*. O delegado do evento especifica que o manipulador de eventos deve aceitar parâmetros de entrada. No .NET Framework, representantes de eventos não retornar qualquer valor em aceitar dois parâmetros de entrada:

- Um `Object`, que identifica a origem do evento, e
- Uma classe derivada de `System.EventArgs`

O segundo parâmetro passado para um manipulador de eventos pode incluir informações adicionais sobre o evento. Embora a base `EventArgs` classe não transmitir qualquer informação, o .NET Framework inclui um número de classes que estendem `EventArgs` e abrangem propriedades adicionais. Por exemplo, uma `CommandEventArgs` instância é passada para manipuladores de eventos que respondem ao `Command` evento e inclui duas propriedades informativas: `CommandArgument` e `CommandName`.

> [!NOTE]
> Para obter mais informações sobre como criar, gerando e manipulando eventos, consulte [eventos e delegados](https://msdn.microsoft.com/library/17sde2xt.aspx) e [representantes de eventos em inglês simples](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Para definir um evento use a seguinte sintaxe:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Como precisamos apenas para a página de conteúdo de alerta quando o usuário clicou com o `DoublePrice` botão e não precisar passar ao longo de qualquer outra informação, podemos usar o delegado do evento `EventHandler`, que define um manipulador de eventos que aceita como seu segundo um objeto do tipo de parâmetro `System.EventArgs`. Para criar o evento na página mestra, adicione a seguinte linha de código à classe de code-behind da página mestra:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

O código acima adiciona um evento público para a página mestra chamada `PricesDoubled`. Agora, precisamos gere este evento após os preços duplicou. Para elevar um evento use a seguinte sintaxe:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Em que *remetente* e *eventArgs* são os valores que você deseja passar para o manipulador de eventos do assinante.

Atualizar o `DoublePrice` `Click` manipulador de eventos com o código a seguir:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Como antes, o `Click` manipulador de eventos inicia com a chamada a `DoublePricesDataSource` do controle SqlDataSource `Update` método duplicar os preços de todos os produtos. Há duas adições ao manipulador de eventos a seguir. Primeiro, o `RecentProducts` dados do GridView são atualizados. Este GridView foi adicionado à página mestra no tutorial anterior e exibe os cinco produtos recentemente adicionados. É necessário atualizar essa grade para que ele mostra os preços dobrado apenas para esses cinco produtos. Depois disso, o `PricesDoubled` é gerado. Uma referência para a página mestra em si (`Me`) é enviado para o manipulador de eventos como a origem do evento e um vazio `EventArgs` objeto é enviado como os argumentos do evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Etapa 4: Manipulando o evento na página de conteúdo

Neste momento a página mestra aciona seu `PricesDoubled` evento sempre que o `DoublePrice` controle de botão é clicado. No entanto, isso é apenas metade da batalha - precisamos manipular o evento no assinante. Isso envolve duas etapas: Criando o manipulador de eventos e adicionando o código de fiação de evento para que quando o evento é gerado, o manipulador de eventos é executado.

Comece criando um manipulador de eventos chamado `Master_PricesDoubled`. Devido a como definimos os `PricesDoubled` evento na página mestra dois parâmetros de entrada do manipulador de eventos devem ser de tipos `Object` e `EventArgs`, respectivamente. Na chamada do manipulador de eventos do `ProductsGrid` do GridView `DataBind` método associar novamente os dados à grade.


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

O código para o manipulador de eventos está concluído, mas ainda não encontramos a página mestra na transmissão `PricesDoubled` eventos para este manipulador de eventos. O assinante conecta um evento para um manipulador de eventos por meio da sintaxe a seguir:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*Publisher* é uma referência ao objeto que oferece o evento *eventName*, e *methodName* é o nome do manipulador de eventos definido no assinante.

Esse código de fiação de evento deve ser executado na primeira visita de página e postbacks subsequentes e deve ocorrer em um ponto do ciclo de vida de página anterior quando o evento pode ser gerado. Um bom momento para adicionar o código de fiação de evento está no estágio de PreInit, que ocorre muito cedo no ciclo de vida de página.

Abra `~/Admin/Products.aspx` e crie um `Page_PreInit` manipulador de eventos:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Para concluir esse código de fiação precisamos de uma referência de programação para a página mestra da página de conteúdo. Conforme observado no tutorial anterior, há duas maneiras de fazer isso:

- Convertendo a tipagem `Page.Master` propriedade para o tipo apropriado na página mestra, ou
- Adicionando um `@MasterType` diretiva a `.aspx` página e, em seguida, usando o tipo mais acentuado `Master` propriedade.

Vamos usar a segunda abordagem. Adicione o seguinte `@MasterType` diretiva na parte superior da marcação declarativa da página:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Em seguida, adicione o seguinte código de fiação de evento no `Page_PreInit` manipulador de eventos:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Com esse código em vigor, o GridView na página de conteúdo é atualizado sempre que o `DoublePrice` botão é clicado.

As figuras 8 e 9 ilustram esse comportamento. Figura 8 mostra a página quando visitado pela primeira vez. Observe que os valores de preço em ambos os `RecentProducts` GridView (na coluna esquerda da página mestra) e o `ProductsGrid` GridView (na página de conteúdo). Mostra a Figura 9 a mesma tela imediatamente após o `DoublePrice` botão foi clicado. Como você pode ver, os novos preços instantaneamente são refletidos em ambos os GridViews.


[![Os valores de preço inicial](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Figura 08**: os valores iniciais de preço ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Os preços Just-Doubled são exibidos no GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Figura 09**: os preços de The Just-Doubled são exibidos no GridViews ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>Resumo

O ideal é que uma página mestra e suas páginas de conteúdo são completamente separadas umas das outras e nenhum nível de interação. No entanto, se você tiver uma página mestra ou a página de conteúdo que exibe os dados que podem ser modificados na página mestra ou página de conteúdo, em seguida, você pode precisa ter a página mestra de alerta a página de conteúdo (ou vice-versa uma) quando os dados são modificados para que a exibição pode ser atualizada. No tutorial anterior, vimos como fazer com que uma página de conteúdo interagir programaticamente com sua página mestra; Neste tutorial vimos como fazer uma inicialização de página mestra a interação.

Enquanto a interação programática entre uma página mestra e de conteúdo pode ser obtidos o conteúdo ou a página mestra, o padrão de interação usado depende da origem. As diferenças são devido ao fato de que uma página de conteúdo tem uma única página mestra, mas uma página mestre pode ter muitas páginas de conteúdo diferentes. Em vez de ter uma página mestra que interagem diretamente com uma página de conteúdo, uma abordagem melhor é ter a página mestra acionar um evento para sinalizar que alguma ação ocorreu. Essas páginas de conteúdo que se preocupam com a ação podem criar manipuladores de eventos.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Acessar e atualizar dados no ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Delegados e eventos](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Passando informações entre o conteúdo e páginas mestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabalhando com dados nos tutoriais do ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Suchi Banerjee. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-master-page-from-the-content-page-vb.md)
> [Próximo](master-pages-and-asp-net-ajax-vb.md)
