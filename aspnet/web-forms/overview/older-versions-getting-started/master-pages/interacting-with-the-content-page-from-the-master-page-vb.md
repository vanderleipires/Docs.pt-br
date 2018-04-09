---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interagir com a página de conteúdo da página mestra (VB) | Microsoft Docs
author: rick-anderson
description: Examina como chamar métodos, definir propriedades, etc. da página de conteúdo no código da página mestra.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 9274924b441cb21e33eb57de06ff374428fa036b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interagir com a página de conteúdo da página mestra (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Examina como chamar métodos, definir propriedades, etc. da página de conteúdo no código da página mestra.


## <a name="introduction"></a>Introdução

O tutorial anterior examinadas como fazer com que a página de conteúdo interagir programaticamente com sua página mestra. Lembre-se de que foi atualizado a página mestra para incluir um controle GridView que são listados os cinco mais recentemente adicionado produtos. Em seguida, criamos uma página de conteúdo na qual o usuário pode adicionar um novo produto. Após adicionar um novo produto, a página de conteúdo é necessária para instruir a página mestra para atualizar seu GridView para que ele inclui o produto adicionados apenas. Essa funcionalidade foi feita adicionando um método público para a página mestra que atualizar os dados associados a GridView e, em seguida, chamar o método da página de conteúdo.

A forma mais comum de conteúdo e interação de página mestra se origina a página de conteúdo. No entanto, é possível que a página mestra para rouse a página de conteúdo atual em ação, e essa funcionalidade pode ser necessária se a página mestra contém elementos de interface do usuário que permitem que os usuários modifiquem dados também são exibidos na página de conteúdo. Considere uma página de conteúdo que exibe as informações de produtos em um GridView controle e controle de uma página mestra que inclui um botão que, quando clicado, dobrará os preços de todos os produtos. Como o exemplo no tutorial anterior, GridView precisa ser atualizada após o preço duplo que botão é clicado, para que ele exiba os preços de novo, mas esse cenário é a página mestra que precisa rouse a página de conteúdo em ação.

Este tutorial explora como fazer com que a página mestra invocar a funcionalidade definida na página de conteúdo.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Interação de modo programático provocam por meio de um evento e manipuladores de eventos

Invocar a funcionalidade de página de conteúdo de uma página mestra é mais difícil que o oposto. Como uma página de conteúdo tem uma única página mestre, quando provocam a interação de modo programático da página de conteúdo sabemos quais propriedades e métodos públicos são à nossa disposição. Uma página mestre, no entanto, pode ter muitos conteúdas páginas diferentes, cada qual com seu próprio conjunto de propriedades e métodos. Como, em seguida, pode é escrever código na página mestra para executar alguma ação em sua página de conteúdo quando não sabe qual página de conteúdo será invocada até que o tempo de execução?

Considere a possibilidade de um controle da Web do ASP.NET, como o controle de botão. Um controle de botão pode aparecer em qualquer número de páginas ASP.NET e precisa de um mecanismo pelo qual ele pode alertar a página que foi clicado. Isso é feito usando *eventos*. Em particular, o controle de botão gera seu `Click` eventos quando ele é clicado; a página ASP.NET que contém o botão opcionalmente pode responder a notificação por meio de um *manipulador de eventos*.

Esse mesmo padrão pode ser usado para ter uma funcionalidade de gatilho de página mestra em suas páginas de conteúdo:

1. Adicione um evento para a página mestra.
2. Gere o evento sempre que a página mestra precisa se comunicar com sua página de conteúdo. Por exemplo, se a página mestra precisa de sua página de conteúdo de alerta que o usuário dobrou os preços, o evento será gerado imediatamente após os preços duplicou.
3. Crie um manipulador de eventos nessas páginas de conteúdo que precisam executar alguma ação.

Este restante deste tutorial implementa o exemplo descrito na introdução; Isto é, uma página de conteúdo que lista os produtos no banco de dados e uma página mestra que inclui um botão de controle para duas vezes os preços.

## <a name="step-1-displaying-products-in-a-content-page"></a>Etapa 1: Exibir produtos em uma página de conteúdo

Nossa empresa é criar uma página de conteúdo que lista os produtos do banco de dados Northwind. (Adicionamos o banco de dados Northwind para o projeto no tutorial anterior, [ *interagir com a página de conteúdo da página mestre*](interacting-with-the-master-page-from-the-content-page-vb.md).) Comece adicionando uma nova página ASP.NET para o `~/Admin` pasta denominada `Products.aspx`, tornando-se para associá-lo para o `Site.master` página mestra. A Figura 1 mostra o Gerenciador de soluções depois que esta página foi adicionada para o site.


[![Adicionar uma nova página ASP.NET para a pasta Admin](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Figura 01**: adicionar uma nova página ASP.NET para o `Admin` pasta ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


Lembre-se de que o [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial criamos uma classe de página de base personalizada chamada `BasePage` que gera o título da página, se não for defina explicitamente. Vá para o `Products.aspx` por trás do código da página de classe e a derivam `BasePage` (em vez de `System.Web.UI.Page`).

Por fim, atualize o `Web.sitemap` arquivo para incluir uma entrada para esta lição. Adicione a seguinte marcação sob o `<siteMapNode>` para o conteúdo a lição de interação de página mestra:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

A adição deste `<siteMapNode>` elemento é refletido nas lições lista (veja a Figura 5).

Retorne ao `Products.aspx`. No controle de conteúdo de `MainContent`, adicione um controle GridView e denomine- `ProductsGrid`. Associar o GridView para um novo controle SqlDataSource chamado `ProductsDataSource`.


[![Associar o GridView para um novo controle SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Figura 02**: associar o GridView para um novo controle SqlDataSource ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


Configure o Assistente para que ele usa o banco de dados Northwind. Se você trabalhou com o tutorial anterior, você já deve ter uma cadeia de caracteres de conexão chamada `NorthwindConnectionString` em `Web.config`. Escolha essa cadeia de caracteres de conexão na lista suspensa, conforme mostrado na Figura 3.


[![Configurar o SqlDataSource para usar o banco de dados Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Figura 03**: configurar o SqlDataSource para usar o banco de dados Northwind ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


Em seguida, especifique o controle de fonte de dados `SELECT` instrução, escolha a tabela de produtos na lista suspensa e retornando o `ProductName` e `UnitPrice` colunas (consulte a Figura 4). Clique em Avançar e em seguida Concluir para concluir o Assistente Configurar fonte de dados.


[![Retornar os campos de UnitPrice e ProductName da tabela produtos](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Figura 04**: retornar o `ProductName` e `UnitPrice` os campos do `Products` tabela ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


Isso é tudo que é necessário para que ele! Após concluir o assistente o Visual Studio adiciona dois BoundFields GridView espelhar os dois campos retornados pelo controle SqlDataSource. Marcação dos controles GridView e SqlDataSource segue. A Figura 5 mostra os resultados quando exibidos por meio de um navegador.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![Cada produto e seu preço é listado em GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Figura 05**: cada produto e seu preço é listado em GridView ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> Fique à vontade para limpar a aparência de GridView. Algumas sugestões incluem o valor UnitPrice exibido como uma moeda de formatação e usando fontes e cores de plano de fundo para melhorar a aparência da grade. Para obter mais informações sobre como exibir e formatar dados no ASP.NET, consulte o meu [trabalhando com série de tutoriais de dados](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Etapa 2: Adicionar um botão de preços duplo para a página mestra

Nossa próxima tarefa é adicionar um controle de botão Web com o mestre de página que, quando clicado, será dobrar o preço de todos os produtos no banco de dados. Abra o `Site.master` página mestra e arraste um botão da caixa de ferramentas para o Designer, colocando-o abaixo de `RecentProductsDataSource` controle SqlDataSource adicionamos no tutorial anterior. Definir o botão `ID` propriedade `DoublePrice` e sua `Text` propriedade como "Dupla preços de produtos".

Em seguida, adicione um controle SqlDataSource para a página mestra, nomeando- `DoublePricesDataSource`. Este SqlDataSource será usada para executar o `UPDATE` instrução dobrar todos os preços. Especificamente, precisamos definir seu `ConnectionString` e `UpdateCommand` propriedades para a cadeia de caracteres de conexão apropriado e `UPDATE` instrução. Em seguida, precisamos chamar deste controle SqlDataSource `Update` método quando o `DoublePrice` botão é clicado. Para definir o `ConnectionString` e `UpdateCommand` propriedades, selecione o controle SqlDataSource e, em seguida, vá para a janela de propriedades. O `ConnectionString` essas cadeias de caracteres de conexão já armazenadas em listas de propriedades `Web.config` em uma lista suspensa; escolha o `NorthwindConnectionString` opção conforme mostrado na Figura 6.


[![Configurar o SqlDataSource para usar o NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Figura 06**: configurar o SqlDataSource para usar o `NorthwindConnectionString` ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


Para definir o `UpdateCommand` propriedade, localize a opção UpdateQuery na janela Propriedades. Essa propriedade, quando selecionado, exibe um botão de reticências; Clique neste botão para exibir a caixa de diálogo Editor de comando e parâmetro mostrada na Figura 7. Digite o seguinte `UPDATE` instrução na caixa de texto da caixa de diálogo:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Essa instrução, quando executada, dobre o `UnitPrice` valor para cada registro o `Products` tabela.


[![Definir a propriedade UpdateCommand do SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Figura 07**: definir SqlDataSource `UpdateCommand` propriedade ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


Depois de definir essas propriedades, marcação declarativa de seus controles botão e SqlDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Tudo o que resta é chamar seu `Update` método quando o `DoublePrice` botão é clicado. Criar um `Click` manipulador de eventos para o `DoublePrice` botão e adicione o seguinte código:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Para testar essa funcionalidade, visite o `~/Admin/Products.aspx` página é criado na etapa 1 e clique no botão "Double preços de produtos". Clique no botão causa um postback e executa o `DoublePrice` do botão `Click` manipulador de eventos, dobrando os preços de todos os produtos. A página é renderizada novamente e a marcação é retornada e novamente exibida no navegador. GridView na página de conteúdo, no entanto, a lista de preços mesmo como antes de "Preços de produtos duplo" botão foi clicado. Isso ocorre porque os dados inicialmente carregados em GridView tinham seu estado armazenado no estado de exibição, portanto, não são recarregado em postagens, a menos que indicado o contrário. Se você visita uma página diferente e, em seguida, retornar para o `~/Admin/Products.aspx` página você verá os preços atualizadas.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Etapa 3: Gerar um evento quando o preços serão duplicadas.

Porque o GridView no `~/Admin/Products.aspx` página não reflete imediatamente o dobro de preço, um usuário é compreensível pode pensar que eles não clicar no botão de "Preços de produtos duplo", ou que ele não funcionou. Eles tente clicar no botão mais algumas vezes, dobrando os preços repetidamente. Para corrigir isso, precisamos ter a grade no conteúdo de página exibir os novos preços imediatamente depois que eles serão duplicados.

Conforme discutido anteriormente neste tutorial, é preciso gerar um evento na página mestra sempre que o usuário clica o `DoublePrice` botão. Um evento é uma forma de uma classe (um publicador do evento) para notificar um conjunto de outras classes (os assinantes do evento) que algo interessante ocorreu a outro. Neste exemplo, a página mestra é o publicador do evento; as páginas que se preocupam quando de conteúdo a `DoublePrice` botão é clicado são os assinantes.

Uma classe assina um evento, criando uma *manipulador de eventos*, que é um método que é executado em resposta ao evento que está sendo gerado. O publicador define os eventos que ele gera definindo um *delegado do evento*. O delegado do evento especifica que o manipulador de eventos deve aceitar parâmetros de entrada. O .NET Framework, delegados de eventos não retornar qualquer valor em aceita dois parâmetros de entrada:

- Um `Object`, que identifica a origem do evento, e
- Uma classe derivada de `System.EventArgs`

O segundo parâmetro passado para um manipulador de eventos pode incluir informações adicionais sobre o evento. Enquanto a base de `EventArgs` classe não transmitir qualquer informação, o .NET Framework inclui um número de classes que estendem `EventArgs` e abrangem propriedades adicionais. Por exemplo, um `CommandEventArgs` instância é passada para manipuladores de eventos que respondem ao `Command` evento e inclui duas propriedades informativas: `CommandArgument` e `CommandName`.

> [!NOTE]
> Para obter mais informações sobre como criar, gerando e manipulação de eventos, consulte [eventos e delegados](https://msdn.microsoft.com/library/17sde2xt.aspx) e [delegados de evento simples inglês](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Para definir um evento use a seguinte sintaxe:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Porque precisamos alerta a página de conteúdo quando o usuário clicou o `DoublePrice` botão e não precisa passar por qualquer outra informação adicional, podemos usar o delegado de evento `EventHandler`, que define um manipulador de eventos que aceita como seu segundo um objeto do tipo de parâmetro `System.EventArgs`. Para criar o evento na página mestra, adicione a seguinte linha de código para a classe de code-behind da página mestra:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

O código acima adiciona um evento público para a página mestra chamada `PricesDoubled`. Agora precisamos gere este evento após os preços duplicou. Para gerar um evento use a seguinte sintaxe:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Onde *remetente* e *eventArgs* são os valores que você deseja passar para o manipulador de eventos do assinante.

Atualização de `DoublePrice` `Click` manipulador de eventos com o código a seguir:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Como antes, o `Click` manipulador de eventos é iniciado chamando o `DoublePricesDataSource` do controle SqlDataSource `Update` método dobrar os preços de todos os produtos. Há duas adições ao manipulador de eventos a seguir. Primeiro, o `RecentProducts` dados do GridView são atualizados. Este GridView foi adicionado à página mestre no tutorial anterior e exibe os cinco produtos recentemente adicionado. É necessário atualizar essa grade para que mostre os preços duplicado apenas para esses cinco produtos. Depois disso, o `PricesDoubled` é gerado. Uma referência para a página mestra em si (`Me`) é enviado ao manipulador de eventos como a origem do evento e vazio `EventArgs` objeto seja enviado como os argumentos do evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Etapa 4: Manipulação do evento na página de conteúdo

Agora a página mestra gera seu `PricesDoubled` evento sempre que o `DoublePrice` controle de botão é clicado. No entanto, isso é apenas metade do trabalho - precisamos manipular o evento no assinante. Isso envolve duas etapas: criar o manipulador de eventos e adicionar o código de fiação de evento para que quando o evento é gerado o manipulador de eventos é executado.

Comece criando um manipulador de eventos chamado `Master_PricesDoubled`. Devido a como definimos o `PricesDoubled` evento na página mestra dois parâmetros de entrada do manipulador de eventos devem ser de tipos `Object` e `EventArgs`, respectivamente. Na chamada do manipulador de eventos de `ProductsGrid` do GridView `DataBind` método associar novamente os dados à grade.


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

O código de manipulador de eventos é concluído, mas ainda temos a transmissão da página mestra `PricesDoubled` eventos para este manipulador de eventos. O assinante conecta um evento para um manipulador de eventos por meio da sintaxe a seguir:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*publicador* é uma referência ao objeto que oferece o evento *eventName*, e *methodName* é o nome do manipulador de eventos definido no assinante.

Esse código de cabeamento de evento deve ser executado na primeira visita de página e postagens subsequentes e deve ocorrer em um ponto do ciclo de vida de página anterior quando o evento pode ser gerado. É um bom momento para adicionar o código de evento fiação no estágio de PreInit ocorre logo no início do ciclo de vida da página.

Abra `~/Admin/Products.aspx` e criar um `Page_PreInit` manipulador de eventos:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Para concluir este código de fiação precisamos de uma referência de programação para a página mestra da página de conteúdo. Conforme observado no tutorial anterior, há duas maneiras de fazer isso:

- Convertendo o tipadas vagamente `Page.Master` propriedade para o tipo apropriado na página mestra, ou
- Adicionando um `@MasterType` diretiva no `.aspx` página e, em seguida, usando o tipo mais acentuado `Master` propriedade.

Vamos usar a segunda abordagem. Adicione o seguinte `@MasterType` diretiva na parte superior da marcação declarativa da página:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Em seguida, adicione o seguinte código de ligação de eventos no `Page_PreInit` manipulador de eventos:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Com esse código, GridView na página de conteúdo é atualizado sempre que o `DoublePrice` botão é clicado.

Figuras 8 e 9 ilustram esse comportamento. Figura 8 mostra a página quando visitado primeiro. Observe que os valores de preço em ambos os `RecentProducts` GridView (na coluna esquerda da página mestra) e o `ProductsGrid` GridView (na página de conteúdo). Figura 9 mostra a mesma tela imediatamente após o `DoublePrice` botão foi clicado. Como você pode ver, os novos preços instantaneamente são refletidos em ambos os GridViews.


[![Os valores de preço inicial](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Figura 08**: os valores iniciais de preço ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Os preços Just-Doubled são exibidos no GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Figura 09**: The Just-Doubled preços são exibidos no GridViews ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>Resumo

Idealmente, uma página mestra e suas páginas de conteúdo são completamente separadas uns dos outros e nenhum nível de interação. No entanto, se você tiver uma página mestra ou a página de conteúdo que exibe os dados que podem ser modificados na página mestra ou página de conteúdo, em seguida, você pode precisa ter a página mestra alerta a página de conteúdo (ou vice-versa uma) quando os dados são modificados para que a exibição pode ser atualizada. No tutorial anterior, vimos como fazer com que uma página de conteúdo interagir programaticamente com sua página mestra; Neste tutorial vimos como fazer com que inicie uma página mestra a interação.

Interação entre uma página mestre e o conteúdo de modo programático pode ser obtidos o conteúdo ou a página mestra, o padrão de interação usado depende de origem. As diferenças são devido ao fato de que uma página de conteúdo tem uma única página mestre, mas uma página mestre pode ter muitas páginas de conteúdo diferentes. Em vez de uma página mestra interagir diretamente com uma página de conteúdo, uma abordagem melhor é ter a página mestra disparar um evento para sinalizar que alguma ação ocorreu. Essas páginas de conteúdo que se preocupam com a ação podem criar manipuladores de eventos.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Acessar e atualizar dados no ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Eventos e delegados](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Passando informações entre o conteúdo e páginas mestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabalhando com dados nos tutoriais do ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Suchi Banerjee. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-master-page-from-the-content-page-vb.md)
> [Próximo](master-pages-and-asp-net-ajax-vb.md)
