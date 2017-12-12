---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: "Páginas mestras e ASP.NET AJAX (c#) | Microsoft Docs"
author: rick-anderson
description: "Discute as opções para usar o ASP.NET AJAX e páginas mestras. Examina usando a classe ScriptManagerProxy; aborda como vários arquivos JS são carregados dependi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 86ec6454313f5a6e78c0f64433ef4e5a4f8461ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="master-pages-and-aspnet-ajax-c"></a>Páginas mestras e ASP.NET AJAX (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Discute as opções para usar o ASP.NET AJAX e páginas mestras. Examina usando a classe ScriptManagerProxy; aborda como vários arquivos JS são carregados dependendo se o ScriptManager é usado no mestre de página ou página de conteúdo.


## <a name="introduction"></a>Introdução

Criado nos últimos anos, os desenvolvedores mais [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-aplicativos web habilitados. Um site da Web habilitado para AJAX usa um número de tecnologias relacionadas para oferecer uma experiência de usuário mais ágil na resposta. A criação de aplicativos habilitados para AJAX ASP.NET é surpreendentemente simples graças da Microsoft [do ASP.NET AJAX framework](../../../../ajax/index.md). ASP.NET AJAX é incorporado no ASP.NET 3.5 e Visual Studio 2008; também está disponível como um download separado para aplicativos ASP.NET 2.0.

Ao criar páginas de web habilitado para AJAX com a estrutura do ASP.NET AJAX, você deve adicionar precisamente uma [controle ScriptManager](https://msdn.microsoft.com/en-us/library/bb398863.aspx) para cada página que usa a estrutura. Como o nome sugere, o ScriptManager gerencia o script do lado do cliente usado em páginas da web habilitado para AJAX. No mínimo, o ScriptManager emite HTML que instrui o navegador para baixar os arquivos JavaScript que a biblioteca de cliente AJAX ASP.NET de composição. Ele também pode ser usado para registrar os arquivos JavaScript personalizada, serviços web habilitados para script e funcionalidade do serviço de aplicativo personalizado.

Se o mestre de usos do site de páginas (como deveria), você não precisa adicionar um controle ScriptManager para cada página de conteúdo único; em vez disso, você pode adicionar um controle ScriptManager para a página mestra. Este tutorial mostra como adicionar um controle ScriptManager para a página mestra. Ele também analisa como usar o controle ScriptManagerProxy para registrar scripts personalizados e serviços de script em uma página de conteúdo específica.

> [!NOTE]
> Este tutorial não explorar criando ou criação de aplicativos da web habilitado para AJAX com a estrutura do ASP.NET AJAX. Para obter mais informações sobre como usar o AJAX, consulte o [vídeos do ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) e [tutoriais](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md), bem como os recursos listados na seção de leitura adicional no final deste tutorial.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Examinando a marcação emitida pelo controle ScriptManager

O controle ScriptManager emite a marcação que instrui o navegador para baixar os arquivos JavaScript que a biblioteca de cliente AJAX ASP.NET de composição. Ele também adiciona um pouco de embutido JavaScript para a página que inicializa essa biblioteca. A marcação a seguir mostra o conteúdo que é adicionado para a saída renderizada de uma página que inclui um controle ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

O `<script src="url"></script>` marcas instruem o navegador para baixar e executar o arquivo de JavaScript em *url*. O ScriptManager emite três marcas; um faz referência ao arquivo `WebResource.axd`, enquanto as outras duas referenciam o arquivo `ScriptResource.axd`. Esses arquivos não existirem, na verdade, como arquivos no seu site. Em vez disso, quando chega uma solicitação para qualquer um desses arquivos no servidor web, o mecanismo do ASP.NET examina querystring e retorna o conteúdo apropriado do JavaScript. O script fornecido por esses três arquivos JavaScript externos constituem a biblioteca de cliente da estrutura do ASP.NET AJAX. O outro `<script>` marcas emitidas pelo ScriptManager incluem no script embutido que inicializa essa biblioteca.

As referências de script externo e emitidos pelo ScriptManager no script embutido são essenciais para uma página que usa a estrutura do ASP.NET AJAX, mas não é necessária para páginas que não usam a estrutura. Portanto, você pode motivo que ele é ideal para adicionar somente um ScriptManager para as páginas que usam a estrutura do ASP.NET AJAX. E isso é suficiente, mas se você tiver muitas páginas que usam a estrutura acabará adicionando o ScriptManager para todas as páginas - uma tarefas repetitivas, no mínimo. Como alternativa, você pode adicionar um ScriptManager à página mestre, que insere, em seguida, esse script necessário em todas as páginas de conteúdo. Com essa abordagem, você não precisa Lembre-se de adicionar um ScriptManager para uma nova página que usa a estrutura do ASP.NET AJAX, porque ela já está incluída, a página mestra. Etapa 1 aborda a adição de um ScriptManager para a página mestra.

> [!NOTE]
> Se você planeja incluindo a funcionalidade de AJAX na interface do usuário da página mestra, você não tem escolha em questão,-você deve incluir o ScriptManager na página mestra.


Uma desvantagem de adicionar o ScriptManager para a página mestra é que o script acima é emitido nos *cada* página, independentemente se necessário. Claramente, isso leva a largura de banda desperdício para páginas que têm o ScriptManager incluído (por meio da página mestra) ainda não usar quaisquer recursos do ASP.NET AJAX framework. Mas, quanto a largura de banda é desperdiçada?

- O conteúdo real emitido pelo ScriptManager (mostrado acima) dos totais de um pouco mais de 1KB.
- Os três arquivos de script externo referenciados pelo `<script>` elemento, no entanto, compõem aproximadamente 450 KB de dados descompactados; em um site que usa compactação gzip, essa largura de banda total pode ser reduzida perto de 100 KB. No entanto, esses arquivos de script são armazenados em cache pelo navegador para um ano, o que significa que eles só precisam ser baixado uma vez e, em seguida, podem ser reutilizados em outras páginas no site.

Na melhor das hipóteses, em seguida, quando os arquivos de script são armazenados em cache, o custo total é de 1KB, que é insignificante. Na pior das hipóteses, porém - que é quando os arquivos de script ainda não foram baixou e o servidor web não está usando qualquer forma de compactação - a ocorrência de largura de banda é aproximadamente 450KB, que podem ser adicionadas em qualquer lugar de um segundo ou dois sobre uma conexão de banda larga para até um minuto para  usuários sobre modems dial-up. A boa notícia é que porque os arquivos de script externo são armazenados em cache pelo navegador, este pior cenário ocorre com pouca frequência.

> [!NOTE]
> Se você ainda sinta à vontade colocando o controle ScriptManager na página mestra, considere o formulário da Web (o `<form runat="server">` marcação na página mestra). Todas as páginas ASP.NET que usa o modelo de postback devem incluir exatamente um formulário da Web. A adição de um formulário da Web adiciona conteúdo adicional: um número de campos de formulário oculto, o `<form>` marca em si, e, se necessário, uma função JavaScript para iniciar uma nova postagem de script. Essa marcação é desnecessária para páginas que não executa postback. Essa marcação estranhas pode ser eliminada, removendo o formulário da Web da página mestra e adicionando-lo manualmente para cada página de conteúdo que precisa de um. No entanto, os benefícios de ter o formulário da Web na página mestra superam as desvantagens de tê-la adicionado desnecessariamente a certas páginas de conteúdo.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Etapa 1: Adicionar um controle ScriptManager à página mestra

Todas as páginas da web que usa a estrutura do ASP.NET AJAX deve conter exatamente um controle do ScriptManager. Devido a esse requisito, geralmente faz sentido para colocar um único controle ScriptManager na página mestra para que todas as páginas de conteúdo tem o ScriptManager incluído automaticamente. Além disso, o ScriptManager deve vir antes de qualquer um dos controles de servidor ASP.NET AJAX, como os controles do UpdatePanel e UpdateProgress. Portanto, é aconselhável colocar o ScriptManager antes de qualquer controle ContentPlaceHolder dentro do formulário da Web.

Abra o `Site.master` página mestra e adicionar um controle ScriptManager à página dentro do formulário da Web, mas antes de `<div id="topContent">` elemento (consulte a Figura 1). Se você estiver usando o Visual Web Developer 2008 ou o Visual Studio 2008, o controle ScriptManager está localizado na caixa de ferramentas na guia Extensões AJAX. Se você estiver usando o Visual Studio 2005, você precisará primeiro instalar o ASP.NET AJAX framework e adicionar os controles à caixa de ferramentas. Visite o [ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) para obter a estrutura para ASP.NET 2.0.

Depois de adicionar o ScriptManager para a página, alterar seu `ID` de `ScriptManager1` para `MyManager`.


[![Adicionar o ScriptManager para a página mestra](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figura 01**: adicionar o ScriptManager para a página mestra ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Etapa 2: Usando a estrutura do ASP.NET AJAX em uma página de conteúdo

Com o controle ScriptManager adicionado à página mestre, podemos agora adicionar funcionalidade do ASP.NET AJAX framework a qualquer página de conteúdo. Vamos criar uma nova página ASP.NET que exibe um produto selecionado aleatoriamente do banco de dados Northwind. Vamos usar controle de Timer da estrutura do ASP.NET AJAX para atualizar essa exibição a cada 15 segundos, mostrando um novo produto.

Comece criando uma nova página no diretório raiz chamado `ShowRandomProduct.aspx`. Não se esqueça de ligar esta nova página para o `Site.master` página mestra.


[![Adicionar uma nova página ASP.NET para o site](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figura 02**: adicionar uma nova página ASP.NET para o site ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Lembre-se de que o [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial criamos uma classe de página de base personalizada chamada `BasePage` que gerou o título da página, se ele foi não foi explicitamente definido. Vá para o `ShowRandomProduct.aspx` por trás do código da página de classe e a derivam `BasePage` (em vez de `System.Web.UI.Page`).

Por fim, atualize o `Web.sitemap` arquivo para incluir uma entrada para esta lição. Adicione a seguinte marcação sob o `<siteMapNode>` para o mestre lição de interação de página de conteúdo:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

A adição deste `<siteMapNode>` elemento é refletido nas lições lista (veja a Figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Exibindo um produto selecionado aleatoriamente

Retorne ao `ShowRandomProduct.aspx`. No Designer, arraste um controle UpdatePanel da caixa de ferramentas para a `MainContent` controle de conteúdo e defina seu `ID` propriedade `ProductPanel`. O UpdatePanel representa uma região na tela que pode ser atualizada de forma assíncrona por meio de um postback da página parcial.

A primeira tarefa é exibir informações sobre um produto selecionado aleatoriamente no UpdatePanel. Iniciar, arraste um controle DetailsView no UpdatePanel. Definir o controle de DetailsView `ID` propriedade `ProductInfo` e limpar seu `Height` e `Width` propriedades. Expanda a marca inteligente de DetailsView e, na lista suspensa Escolher fonte de dados, optar por associar DetailsView para um novo controle SqlDataSource chamado `RandomProductDataSource`.


[![Associar o DetailsView para um novo controle SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figura 03**: associar o DetailsView para um novo controle SqlDataSource ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Configurar o controle SqlDataSource para se conectar ao banco de dados Northwind, por meio de `NorthwindConnectionString` (que criamos no [ *interagir com a página de conteúdo da página mestre* ](interacting-with-the-content-page-from-the-master-page-cs.md) tutorial). Ao configurar a instrução select optar por especificar uma instrução SQL personalizada e, em seguida, digite a seguinte consulta:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

O `TOP 1` palavra-chave no `SELECT` cláusula retorna apenas o primeiro registro retornado pela consulta. O [ `NEWID()` função](https://msdn.microsoft.com/en-us/library/ms190348.aspx) gera um novo [o valor de identificador global exclusivo (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) e pode ser usado em um `ORDER BY` cláusula para retornar registros da tabela em uma ordem aleatória.


[![Configurar o SqlDataSource para retornar um único registro selecionado aleatoriamente](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figura 04**: configurar o SqlDataSource para retornar um único registro de selecionado aleatoriamente ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Depois de concluir o assistente, o Visual Studio cria um BoundField para as duas colunas retornadas pela consulta acima. Neste ponto declarativo da página deve ser semelhante ao seguinte:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

A Figura 5 mostra o `ShowRandomProduct.aspx` página quando visualizada através de um navegador. Botão de atualização do seu navegador para recarregar a página. Você deve ver o `ProductName` e `UnitPrice` valores para um novo registro selecionado aleatoriamente.


[![Nome de um produto aleatório e o preço é exibido](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figura 05**: nome e o preço do produto A aleatória é exibido ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Exibindo automaticamente um novo produto a cada 15 segundos

A estrutura do ASP.NET AJAX inclui um controle de Timer que executa uma postagem em um horário especificado; o temporizador de postback em `Tick` é gerado. Se o controle de Timer é colocado dentro de um UpdatePanel ele dispara um postback da página parcial, durante o qual é possível reassociar os dados a DetailsView para exibir um novo produto selecionado aleatoriamente.

Para fazer isso, arraste um Timer da caixa de ferramentas e soltá-lo em UpdatePanel. Alterar o Timer `ID` de `Timer1` para `ProductTimer` e sua `Interval` propriedade de 60000 e 15000. O `Interval` propriedade indica o número de milissegundos entre postbacks; configurá-lo como 15000 faz com que o Timer disparar um postback da página parcial a cada 15 segundos. Neste ponto declarativo do temporizador deve ser semelhante ao seguinte:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Criar um manipulador de eventos para o Timer `Tick` eventos. Este manipulador de eventos, precisamos associar novamente os dados de DetailsView chamando o DetailsView `DataBind` método. Isso instrui o DetailsView para recuperar novamente os dados do seu controle de fonte de dados, que seleciona e exibir um novo aleatoriamente selecionado registro (da mesma forma quando recarregar a página clicando no botão Atualizar do navegador).


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Isso é tudo que é necessário para que ele! Revise a página por meio de um navegador. Inicialmente, as informações do produto aleatória são exibidas. Se você observar pacientemente a tela, você observará que, depois de 15 segundos, informações sobre um novo produto magicamente substituem a exibição existente.

Para ver melhor o que está acontecendo aqui, vamos adicionar um controle de rótulo para o UpdatePanel que exibe a hora que da última atualização de exibição. Adicionar um controle de rótulo Web dentro do UpdatePanel, defina seu `ID` para `LastUpdateTime`e desmarque sua `Text` propriedade. Em seguida, crie um manipulador de eventos para o UpdatePanel `Load` eventos e exibir a hora atual no rótulo. (O UpdatePanel `Load` evento é acionado em cada postback da página completo ou parcial.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Com essa alteração é concluída, a página inclui a hora em que o produto atualmente exibido foi carregado. Figura 6 mostra a página quando visitado primeiro. Figura 7 mostra a página mais tarde 15 segundos depois que o controle de Timer tem "marcado" e o UpdatePanel foi atualizado para exibir informações sobre um novo produto.


[![Um produto aleatoriamente selecionado é exibido no carregamento de página](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figura 06**: um produto selecionado aleatoriamente é exibido no carregamento de página ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Um novo aleatoriamente selecionado produto será exibido a cada 15 segundos](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figura 07**: cada 15 segundos é exibido um novo aleatoriamente selecionado produto ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Etapa 3: Usando o controle ScriptManagerProxy

Juntamente com incluindo o script necessário para a biblioteca de cliente de estrutura do ASP.NET AJAX, o ScriptManager também pode registrar arquivos JavaScript personalizados, as referências a serviços Web habilitados para script e autenticação, autorização e serviços de perfil. Geralmente, essas personalizações são específicas para uma determinada página. No entanto, se a autenticação, referências de serviço da Web ou arquivos de script personalizado, autorização ou serviços de perfil são referenciadas no ScriptManager na página mestre, em seguida, são incluídas no *todos os* páginas no site.

Para adicionar personalizações relacionadas ScriptManager na página por página usam o controle ScriptManagerProxy. Você pode adicionar um ScriptManagerProxy para uma página de conteúdo e, em seguida, registre o arquivo JavaScript personalizado, referência de serviço Web, ou autenticação, autorização ou o serviço de perfil de ScriptManagerProxy; Isso tem o efeito de registrar esses serviços para a página de conteúdo específico.

> [!NOTE]
> Uma página ASP.NET só pode ter não mais de um controle ScriptManager presente. Portanto, você não pode adicionar um controle ScriptManager para uma página de conteúdo, se o controle ScriptManager já está definido na página mestra. A única finalidade de ScriptManagerProxy é fornecer uma maneira para os desenvolvedores a definir o ScriptManager na página mestre, mas ainda tem a capacidade de adicionar personalizações ScriptManager na página por página.


Para ver o controle ScriptManagerProxy em ação, vamos aumentar o UpdatePanel em `ShowRandomProduct.aspx` incluir um botão que usa o script do lado do cliente para pausar ou retomar o controle de Timer. O controle de Timer tem três métodos do lado do cliente que podemos usar para atingir essa funcionalidade desejada:

- `_startTimer()`-Inicia o controle de Timer
- `_raiseTick()`-faz com que o controle de Timer para "escala", assim, fazer postback e gerando seu `Tick` eventos no servidor
- `_stopTimer()`-Interrompe o controle de Timer

Vamos criar um arquivo JavaScript com uma variável chamada `timerEnabled` e uma função chamada `ToggleTimer`. O `timerEnabled` variável indica se o controle de Timer está habilitado ou desabilitado no momento, o padrão é true. O `ToggleTimer` função aceita dois parâmetros de entrada: uma referência ao lado do cliente e de botão Pausar ou retomar `id` valor do controle Timer. Essa função alterna o valor da `timerEnabled`, obtém uma referência ao controle do Timer, inicia ou interrompe o Timer (dependendo do valor de `timerEnabled`) e atualiza o texto do botão "Pausar" ou "Resume". Essa função será chamada sempre que clicar no botão Pausar ou retomar.

Comece criando uma nova pasta no site de chamada `Scripts`. Em seguida, adicione um novo arquivo na pasta Scripts chamada `TimerScript.js` do tipo arquivo JScript.


[![Adicionar um novo arquivo de JavaScript para a pasta de Scripts](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figura 08**: adicionar um novo arquivo de JavaScript para o `Scripts` pasta ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Foi adicionado um novo arquivo de JavaScript para o site](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figura 09**: foi adicionado um novo arquivo de JavaScript para o site ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image27.png))


Em seguida, adicione o seguinte script no arquivo TimerScript.js:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Agora, é necessário registrar esse arquivo JavaScript personalizado `ShowRandomProduct.aspx`. Retorne ao `ShowRandomProduct.aspx` e adicionar um controle ScriptManagerProxy à página; definir seu `ID` para `MyManagerProxy`. Para registrar um JavaScript personalizado arquivo seleciona o controle ScriptManagerProxy no Designer e, em seguida, vá para a janela de propriedades. Uma das propriedades é intitulada Scripts. Selecionar essa propriedade exibe o Editor de coleção ScriptReference mostrado na Figura 10. Clique no botão Adicionar para incluir uma nova referência de script e, em seguida, digite o caminho para o arquivo de script na propriedade do caminho: `~/Scripts/TimerScript.js`.


[![Adicione uma referência de Script para o controle ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figura 10**: adicionar uma referência de Script para o controle ScriptManagerProxy ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Depois de adicionar a referência de script do controle ScriptManagerProxy's declarativa marcação é atualizada para incluir um `<Scripts>` coleção com uma única `ScriptReference` entrada, como o seguinte trecho de marcação mostra:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

O `ScriptReference` entrada instrui o ScriptManagerProxy para incluir uma referência ao arquivo JavaScript na sua marcação renderizada. Ou seja, registrando personalizado script o ScriptManagerProxy o `ShowRandomProduct.aspx` saída renderizada da página agora inclui outra `<script src="url"></script>` marca: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Agora podemos chamar o `ToggleTimer` função definida no `TimerScript.js` o script de cliente no `ShowRandomProduct.aspx` página. Adicione o seguinte HTML no UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Isso exibirá um botão com o texto "Pausar". Sempre que é clicado, a função JavaScript `ToggleTimer` é chamado, passando uma referência para o botão e o valor da id do controle Timer (`ProductTimer`). Observe a sintaxe para a obtenção de `id` valor do controle Timer. `<%=ProductTimer.ClientID%>`emite o valor de `ProductTimer` controle de Timer `ClientID` propriedade. No [ *nomenclatura de ID de controle em páginas de conteúdo* ](control-id-naming-in-content-pages-cs.md) tutorial abordamos as diferenças entre o lado do servidor `ID` valor e o lado do cliente resultante `id` valor e como `ClientID` retorna o cliente `id`.

A Figura 11 mostra essa página quando visitado primeiro através de um navegador. O Timer está em execução e atualiza as informações de produto exibido a cada 15 segundos. Figura 12 mostra a tela após ser clicado no botão de pausa. Clicar no botão Pausar interrompe o temporizador e atualiza o texto do botão para "Continuar". As informações de produto de atualização (e continuar para atualizar a cada 15 segundos) depois que o usuário clica em continuar.


[![Clique no botão Pausar para parar o controle de Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figura 11**: clique no botão Pausar para parar o controle de Timer ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Clique no botão Reiniciar para reiniciar o Timer](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figura 12**: clique no botão Reiniciar para reiniciar o Timer ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Resumo

Ao criar aplicativos da web habilitado para AJAX usando a estrutura do ASP.NET AJAX é essencial que todas as páginas da web habilitado para AJAX incluem um controle ScriptManager. Para facilitar esse processo, podemos adicionar um ScriptManager para a página mestra em vez de ter de lembrar para adicionar um ScriptManager para cada página de conteúdo. Etapa 1 mostrada como adicionar o ScriptManager para a página mestra durante a etapa 2, analisamos Implementando a funcionalidade de AJAX em uma página de conteúdo.

Se você precisar adicionar scripts personalizados, as referências a serviços Web habilitados para script, ou personalizado de autenticação, autorização ou serviços de perfil para uma determinada página conteúdo, adicione um controle ScriptManagerProxy à página de conteúdo e, em seguida, configure o personalizações de lá. Etapa 3 examinamos como usar o ScriptManagerProxy para registrar um arquivo JavaScript personalizado em uma página de conteúdo específica.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Estrutura do ASP.NET AJAX](../../../../ajax/index.md)
- [Tutoriais do ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Vídeos do AJAX ASP.NET](../../../videos/aspnet-ajax/index.md)
- [Criando a Interface de usuário interativa com o Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Usando NEWID aleatoriamente os registros de classificação](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Usando o controle de Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](interacting-with-the-content-page-from-the-master-page-cs.md)
[Próximo](specifying-the-master-page-programmatically-cs.md)
