---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Páginas mestras e AJAX ASP.NET (c#) | Microsoft Docs
author: rick-anderson
description: Discute opções para usar o ASP.NET AJAX e páginas mestras. Examina usando a classe ScriptManagerProxy; Discute como os diversos arquivos JS são carregados dependi...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 47201a0cfeb5d1e548721094d11488e9e804dc9c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830634"
---
<a name="master-pages-and-aspnet-ajax-c"></a>Páginas mestras e AJAX ASP.NET (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Discute opções para usar o ASP.NET AJAX e páginas mestras. Examina usando a classe ScriptManagerProxy; Discute como os diversos arquivos JS são carregados, dependendo se o ScriptManager é usado no mestre, página ou página de conteúdo.


## <a name="introduction"></a>Introdução

Nos últimos anos, foram criadas mais e mais desenvolvedores [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-aplicativos web habilitados. Um site da Web habilitado para AJAX usa um número de tecnologias da web relacionados a oferecer uma experiência de usuário mais responsiva. A criação de aplicativos habilitados para AJAX ASP.NET é surpreendentemente fácil graças da Microsoft [estrutura do ASP.NET AJAX](../../../../ajax/index.md). ASP.NET AJAX é incorporado no ASP.NET 3.5 e Visual Studio 2008; Ele também está disponível como um download separado para aplicativos ASP.NET 2.0.

Ao criar páginas da web habilitadas para AJAX com a estrutura ASP.NET AJAX, você deve adicionar precisamente uma [controle ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) para cada página que usa a estrutura. Como o nome sugere, o ScriptManager gerencia o script do lado do cliente usado em páginas da web habilitado para AJAX. No mínimo, o ScriptManager emite HTML que instrui o navegador para baixar os arquivos JavaScript que a biblioteca de cliente ASP.NET AJAX de composição. Ele também pode ser usado para registrar os arquivos de JavaScript personalizados, serviços web habilitados para script e funcionalidade do serviço de aplicativo personalizado.

Se o mestre de usos do site de páginas (como deveria), você não precisa necessariamente adicione um controle ScriptManager em cada página de conteúdo única; em vez disso, você pode adicionar um controle ScriptManager à página mestra. Este tutorial mostra como adicionar o controle ScriptManager à página mestra. Ele também mostra como usar o controle ScriptManagerProxy para registrar scripts personalizados e serviços de script em uma página com conteúdo específico.

> [!NOTE]
> Este tutorial não explora Projetando ou criação de aplicativos web habilitados para AJAX com a estrutura ASP.NET AJAX. Para obter mais informações sobre como usar o AJAX, consulte o [vídeos do ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) e [tutoriais](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md), bem como os recursos listados na seção leitura adicional no final deste tutorial.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Exame da marcação emitida pelo controle ScriptManager

O controle ScriptManager emite marcação que instrui o navegador para baixar os arquivos JavaScript que a biblioteca de cliente ASP.NET AJAX de composição. Ele também adiciona um pouco de JavaScript embutido para a página que inicializa essa biblioteca. A marcação a seguir mostra o conteúdo que é adicionado para a saída renderizada de uma página que inclui um controle ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

O `<script src="url"></script>` marcas instruem o navegador para baixar e executar o arquivo JavaScript no *url*. O ScriptManager emite três marcas; um faz referência ao arquivo `WebResource.axd`, enquanto as outras duas referenciar o arquivo `ScriptResource.axd`. Esses arquivos não existirem, na verdade, como arquivos em seu site. Em vez disso, quando chega uma solicitação para qualquer um desses arquivos no servidor web, o mecanismo do ASP.NET examina a cadeia de consulta e retorna o conteúdo apropriado do JavaScript. O script fornecido por esses três arquivos JavaScript externos constituem a biblioteca de cliente da estrutura ASP.NET AJAX. O outro `<script>` marcas emitidas pelo ScriptManager incluem script embutido que inicializa essa biblioteca.

As referências de script externo e o script embutido emitidos pelo ScriptManager são essenciais para uma página que usa a estrutura ASP.NET AJAX, mas não é necessária para páginas que não usam a estrutura. Portanto, você pode avaliar a que ele é ideal para adicionar somente um ScriptManager para essas páginas que usam a estrutura ASP.NET AJAX. E isso é suficiente, mas se você tiver muitas páginas que usam o framework acabará adicionando o controle ScriptManager para todas as páginas - uma tarefa repetitiva, no mínimo. Como alternativa, você pode adicionar um ScriptManager à página mestre, que injeta, em seguida, esse script necessário em todas as páginas de conteúdo. Com essa abordagem, você não precisa se lembrar adicionar um ScriptManager para uma nova página que usa a estrutura ASP.NET AJAX, porque ele já está incluído por página mestra. Etapa 1 explica adicionando um ScriptManager à página mestra.

> [!NOTE]
> Se você planeja incluir funcionalidades do AJAX dentro da interface do usuário da página mestra, em seguida, você não tem escolha em questão – você deve incluir o ScriptManager na página mestra.


Uma desvantagem de adicionar o ScriptManager à página mestra é que o script acima é emitido em *cada* página, independentemente se necessário. Claramente, isso leva a largura de banda desperdício para páginas que não usam os recursos da estrutura ASP.NET AJAX ainda tiver o ScriptManager incluído (por meio da página mestra). Mas, quanto a largura de banda é desperdiçada?

- O conteúdo real emitido pelo ScriptManager (mostrado acima) dos totais de um pouco mais de 1KB.
- Os três arquivos de script externo referenciados pelo `<script>` elemento, no entanto, compõem aproximadamente 450 KB de dados descompactados; em um site que usa compactação gzip, essa largura de banda total pode ser reduzida perto de 100 KB. No entanto, esses arquivos de script são armazenados em cache pelo navegador para um ano, o que significa que eles só precisam ser baixados de uma vez e, em seguida, podem ser reutilizados em outras páginas no site.

Na melhor das hipóteses, em seguida, quando os arquivos de script são armazenados em cache, o custo total é de 1KB, que é insignificante. Na pior das hipóteses, no entanto - que é quando os arquivos de script ainda não foram baixou e o servidor web não está usando qualquer forma de compactação - o impacto de largura de banda é cerca de 450KB, que pode adicionar em qualquer lugar de um segundo ou dois ao longo de uma conexão de banda larga para até um minuto para  usuários sobre modems dial-up. A boa notícia é que como os arquivos de script externo são armazenadas em cache pelo navegador, esse cenário de pior caso ocorre com pouca frequência.

> [!NOTE]
> Se você ainda não se sinta confortável colocando o controle do ScriptManager na página mestra, considere o formulário da Web (o `<form runat="server">` marcação na página mestra). Todas as páginas ASP.NET que usa o modelo de postback devem incluir exatamente um Web Form. A adição de um formulário da Web adiciona o conteúdo adicional: um número de campos de formulário oculto, o `<form>` de marca em si, e, se necessário, uma função JavaScript para iniciar um postback do script. Essa marcação é desnecessária para páginas que não de postback. Essa marcação incorreta pode ser eliminada, removendo o formulário da Web da página mestra e adicionando-o manualmente para cada página de conteúdo que precisa de um. No entanto, os benefícios de ter o formulário da Web na página mestra superam as desvantagens da necessidade de adicioná-lo desnecessariamente a determinadas páginas de conteúdo.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Etapa 1: Adicionar um controle ScriptManager à página mestra

Todas as páginas da web que usa a estrutura ASP.NET AJAX deve conter exatamente um controle do ScriptManager. Devido a esse requisito, geralmente faz sentido colocar um único controle ScriptManager na página mestra para que todas as páginas de conteúdo tem o controle do ScriptManager incluído automaticamente. Além disso, o ScriptManager deve vir antes de qualquer um dos controles de servidor ASP.NET AJAX, como os controles UpdatePanel e UpdateProgress. Portanto, é melhor colocar o ScriptManager antes de qualquer controle ContentPlaceHolder dentro do formulário da Web.

Abra o `Site.master` página mestra e adicione um controle ScriptManager à página dentro do formulário da Web, mas antes de `<div id="topContent">` elemento (consulte a Figura 1). Se você estiver usando o Visual Web Developer 2008 ou o Visual Studio 2008, o controle do ScriptManager está localizado na caixa de ferramentas na guia Extensões AJAX. Se você estiver usando o Visual Studio 2005, você precisará primeiro instalar a estrutura ASP.NET AJAX e adicione os controles à caixa de ferramentas. Visite o [Wiki do AJAX ASP.NET](https://github.com/DevExpress/AjaxControlToolkit/wiki) para obter a estrutura do ASP.NET 2.0.

Depois de adicionar o ScriptManager à página, alterar sua `ID` partir `ScriptManager1` para `MyManager`.


[![Adicionar o ScriptManager à página mestra](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figura 01**: adicionar o ScriptManager à página mestra ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Etapa 2: Usando o ASP.NET AJAX Framework de uma página de conteúdo

Com o controle ScriptManager à página mestra agora adicionamos a funcionalidade do ASP.NET AJAX framework para qualquer página de conteúdo. Vamos criar uma nova página ASP.NET que exibe um produto selecionado aleatoriamente do banco de dados Northwind. Vamos usar o controle de Timer da estrutura ASP.NET AJAX para atualizar essa exibição a cada 15 segundos, mostrando um novo produto.

Comece criando uma nova página no diretório raiz chamado `ShowRandomProduct.aspx`. Não se esqueça de vincular essa nova página para o `Site.master` página mestra.


[![Adicionar uma nova página ASP.NET para o site](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figura 02**: adicionar uma nova página ASP.NET para o site ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Lembre-se de que no [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial, criamos uma classe de página de base personalizada chamada `BasePage` que gerou o título da página, se ele tiver sido não foi explicitamente definido. Vá para o `ShowRandomProduct.aspx` de lógica da página de classe e fazer com que ele derivam `BasePage` (em vez do `System.Web.UI.Page`).

Por fim, atualize o `Web.sitemap` arquivo para incluir uma entrada para esta lição. Adicione a seguinte marcação abaixo o `<siteMapNode>` para o mestre de lição de interação de página de conteúdo:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

A adição deste `<siteMapNode>` elemento é refletido nas lições lista (veja a Figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Exibindo um produto selecionado aleatoriamente

Retorne ao `ShowRandomProduct.aspx`. No Designer, arraste um controle UpdatePanel da caixa de ferramentas para o `MainContent` controle de conteúdo e defina sua `ID` propriedade para `ProductPanel`. O UpdatePanel representa uma região da tela que pode ser atualizada de forma assíncrona por meio de um postback de página parcial.

Nossa primeira tarefa é exibir informações sobre um produto selecionado aleatoriamente no UpdatePanel. Comece a arrastar um controle DetailsView para o UpdatePanel. Defina o controle de DetailsView `ID` propriedade para `ProductInfo` e limpe seu `Height` e `Width` propriedades. Expandir a marca inteligente de DetailsView e, na lista suspensa Escolher fonte de dados, optar por associar DetailsView para um novo controle SqlDataSource chamado `RandomProductDataSource`.


[![Associar DetailsView para um novo controle SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figura 03**: associar DetailsView para um novo controle SqlDataSource ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Configurar o controle SqlDataSource para se conectar ao banco de dados Northwind, por meio de `NorthwindConnectionString` (que criamos na [ *interagindo com a página mestra através da página de conteúdo* ](interacting-with-the-content-page-from-the-master-page-cs.md) tutorial). Quando configurar a instrução select escolhe especificar uma instrução SQL personalizada e, em seguida, insira a seguinte consulta:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

O `TOP 1` palavra-chave no `SELECT` cláusula retorna apenas o primeiro registro retornado pela consulta. O [ `NEWID()` função](https://msdn.microsoft.com/library/ms190348.aspx) gera uma nova [valor de identificador global exclusivo (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) e podem ser usados em um `ORDER BY` cláusula para retornar os registros da tabela em ordem aleatória.


[![Configurar o SqlDataSource para retornar um registro único, selecionado aleatoriamente](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figura 04**: configurar o SqlDataSource para retornar um único registro de selecionado aleatoriamente ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Depois de concluir o assistente, o Visual Studio cria um BoundField para as duas colunas retornadas pela consulta acima. Neste ponto marcação declarativa de sua página deve ser semelhante ao seguinte:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

A Figura 5 mostra o `ShowRandomProduct.aspx` página quando visualizado por meio de um navegador. Clique o botão de atualização do seu navegador para recarregar a página; Você deve ver a `ProductName` e `UnitPrice` valores para um novo registro selecionado aleatoriamente.


[![Nome e o preço de um produto aleatório é exibido](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figura 05**: nome e o preço do produto A aleatório é exibida ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Exibindo automaticamente um novo produto a cada 15 segundos

A estrutura ASP.NET AJAX inclui um controle de Timer que executa um postback em um horário especificado; o temporizador de postback em `Tick` é gerado. Se o controle de Timer é colocado dentro de um UpdatePanel, ele dispara um postback de página parcial, durante o qual podemos pode associar novamente os dados a serem DetailsView para exibir um novo produto selecionado aleatoriamente.

Para fazer isso, arraste um Timer na caixa de ferramentas e soltá-lo em UpdatePanel. Alterar o Timer `ID` de `Timer1` ao `ProductTimer` e sua `Interval` propriedade de 60000 para 15000. O `Interval` propriedade indica o número de milissegundos entre postbacks, configurá-lo como 15000 faz com que o Timer disparar um postback de página parcial a cada 15 segundos. Neste ponto, marcação declarativa do temporizador deve ser semelhante ao seguinte:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Criar um manipulador de eventos para o temporizador `Tick` eventos. Nesse manipulador de eventos, precisamos associar novamente os dados a serem DetailsView chamando o DetailsView `DataBind` método. Isso instrui o DetailsView para recuperar novamente os dados do seu controle de fonte de dados, que selecionar e exibir um novo aleatoriamente selecionado registro (assim como ao recarregar a página clicando no botão de atualização do navegador).


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Isso é tudo para ele! Examine a página por meio de um navegador. Inicialmente, as informações do produto aleatório são exibidas. Se você observar pacientemente a tela, você observará que, depois de 15 segundos, informações sobre um novo produto magicamente substituem a exibição existente.

Para ver melhor o que está acontecendo aqui, vamos adicionar um controle de rótulo ao UpdatePanel que exibe a hora em que a exibição foi atualizada pela última vez. Adicione um controle de rótulo Web dentro do UpdatePanel, defina suas `ID` ao `LastUpdateTime`e limpar seu `Text` propriedade. Em seguida, crie um manipulador de eventos para o UpdatePanel `Load` evento e a hora atual no rótulo da exibição. (O UpdatePanel `Load` evento é acionado em cada postback completo ou parcial de página.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Com essa alteração completa, a página inclui a hora em que o produto exibido no momento foi carregado. Figura 6 mostra a página quando visitado pela primeira vez. Figura 7 mostra a página depois de 15 segundos depois que o controle Timer tem "marcada" e o UpdatePanel foi atualizado para exibir informações sobre um novo produto.


[![Um produto aleatoriamente selecionado é exibido no carregamento da página](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figura 06**: um produto selecionado aleatoriamente é exibido no carregamento da página ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Um novo aleatoriamente selecionado produto é exibido a cada 15 segundos](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figura 07**: segundos cada 15 um novo aleatoriamente selecionado produto é exibido ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Etapa 3: Usando o controle ScriptManagerProxy

Juntamente com incluindo o script necessário para a biblioteca de cliente de estrutura do ASP.NET AJAX, o ScriptManager também pode registrar arquivos personalizados de JavaScript, as referências a serviços Web habilitados para script e personalizados de autenticação, autorização e serviços de perfil. Normalmente, essas personalizações são específicas para uma determinada página. No entanto, se personalizado de script de arquivos, referências de serviço da Web ou a autenticação, autorização ou serviços de perfil são referenciados no ScriptManager na página mestra e em seguida, eles são incluídos na *todos os* páginas no site.

Para adicionar personalizações relacionadas ao ScriptManager em uma base de página por página usam o controle ScriptManagerProxy. Você pode adicionar um ScriptManagerProxy a uma página de conteúdo e, em seguida, registrar o arquivo de JavaScript personalizado, referência de serviço Web, ou autenticação, autorização ou o serviço de perfil do ScriptManagerProxy; Isso tem o efeito de registro desses serviços para a página de conteúdo específico.

> [!NOTE]
> Uma página ASP.NET só pode ter não mais de um controle do ScriptManager presente. Portanto, você não pode adicionar um controle ScriptManager para uma página de conteúdo, se o controle ScriptManager já está definido na página mestra. O único propósito do ScriptManagerProxy é fornecer uma maneira para os desenvolvedores definem o ScriptManager na página mestra, mas ainda têm a capacidade de adicionar o ScriptManager personalizações em uma base de página por página.


Para ver o controle ScriptManagerProxy em ação, vamos ampliar o UpdatePanel no `ShowRandomProduct.aspx` incluir um botão que usa o script do lado do cliente para pausar ou retomar o controle Timer. O controle Timer tem três métodos do lado do cliente que podemos usar para atingir essa funcionalidade desejada:

- `_startTimer()` -Inicia o controle Timer
- `_raiseTick()` – faz com que o controle de Timer para "escala", assim, postback e acionar seu `Tick` eventos no servidor
- `_stopTimer()` -Interrompe o controle Timer

Vamos criar um arquivo JavaScript com uma variável denominada `timerEnabled` e uma função chamada `ToggleTimer`. O `timerEnabled` variável indica se o controle Timer está habilitado ou desabilitado no momento; ele assume true como padrão. O `ToggleTimer` função aceita dois parâmetros de entrada: uma referência para o botão Pausar/retomar e do lado do cliente `id` valor do controle Timer. Essa função alterna o valor da `timerEnabled`, obtém uma referência ao controle de temporizador, inicia ou interrompe o temporizador (dependendo do valor de `timerEnabled`) e atualiza o texto de exibição do botão "Pausar" ou "Resume". Essa função será chamada sempre que o botão Pausar/retomar é clicado.

Comece criando uma nova pasta no site de chamada `Scripts`. Em seguida, adicione um novo arquivo na pasta de Scripts chamada `TimerScript.js` do tipo arquivo JScript.


[![Adicionar um novo arquivo JavaScript para a pasta de Scripts](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figura 08**: Adicione um novo arquivo JavaScript para o `Scripts` pasta ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Um novo arquivo JavaScript foi adicionado ao site](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figura 09**: um novo arquivo de JavaScript foi adicionado ao site ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image27.png))


Em seguida, adicione o script a seguir ao arquivo TimerScript.js:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Agora, é necessário registrar esse arquivo JavaScript personalizado `ShowRandomProduct.aspx`. Retorne ao `ShowRandomProduct.aspx` e adicione um controle ScriptManagerProxy à página; defina seus `ID` para `MyManagerProxy`. Para registrar um JavaScript personalizado arquivo seleciona o controle ScriptManagerProxy no Designer e, em seguida, vá para a janela de propriedades. Uma das propriedades é intitulada Scripts. Essa propriedade exibe o Editor de coleção ScriptReference mostrado na Figura 10. Clique no botão Adicionar para incluir uma nova referência de script e, em seguida, insira o caminho para o arquivo de script na propriedade do caminho: `~/Scripts/TimerScript.js`.


[![Adicione uma referência de Script para o controle ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figura 10**: adicionar uma referência de Script para o controle ScriptManagerProxy ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Depois de adicionar a referência de script do controle ScriptManagerProxy's declarativa marcação é atualizada para incluir um `<Scripts>` coleção com um único `ScriptReference` entrada, como o trecho de marcação a seguir mostra:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

O `ScriptReference` entrada instrui o ScriptManagerProxy para incluir uma referência ao arquivo JavaScript na sua marcação renderizada. Ou seja, registrando personalizado do script no ScriptManagerProxy a `ShowRandomProduct.aspx` saída renderizada da página agora inclui um outro `<script src="url"></script>` marca: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Agora podemos chamar o `ToggleTimer` função definida no `TimerScript.js` o script de cliente no `ShowRandomProduct.aspx` página. Adicione o seguinte HTML inserido no UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Isso exibe um botão com o texto "Pausar". Sempre que ele é clicado, a função JavaScript `ToggleTimer` é chamado, passando uma referência para o botão e o valor da id de controle do temporizador (`ProductTimer`). Observe a sintaxe para obter o `id` valor do controle Timer. `<%=ProductTimer.ClientID%>` emite o valor de `ProductTimer` controle de temporizador `ClientID` propriedade. No [ *nomenclatura de ID de controle em páginas de conteúdo* ](control-id-naming-in-content-pages-cs.md) tutorial discutimos as diferenças entre o lado do servidor `ID` valor e o lado do cliente resultante `id` valor e como `ClientID` retorna o lado do cliente `id`.

Figura 11 mostra essa página quando visitado pela primeira vez por meio de um navegador. O temporizador está em execução e atualiza as informações de produto exibido a cada 15 segundos. Figura 12 mostra a tela depois que o botão Pausar foi clicado. Clicar no botão de pausa interrompe o temporizador e atualiza o texto do botão para "Continuar". As informações do produto de atualização (e continuar para atualizar a cada 15 segundos) depois que o usuário clicar em continuar.


[![Clique no botão de pausa para parar o controle Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figura 11**: clique no botão de pausa para parar o controle Timer ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Clique no botão Reiniciar para reiniciar o temporizador](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figura 12**: clique no botão Reiniciar para reiniciar o temporizador ([clique para exibir a imagem em tamanho normal](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Resumo

Ao criar aplicativos web habilitados para AJAX usando a estrutura ASP.NET AJAX é imperativo que todas as páginas da web habilitado para AJAX incluem um controle ScriptManager. Para facilitar esse processo, podemos adicionar um ScriptManager à página mestra em vez de ter que lembrar-se de adicionar um ScriptManager à página de conteúdo de cada. Etapa 1 mostrada como adicionar o ScriptManager à página mestra durante a etapa 2, examinamos Implementando a funcionalidade do AJAX em uma página de conteúdo.

Se você precisar adicionar scripts personalizados, referências a serviços Web habilitados para script, ou personalizado de autenticação, autorização ou serviços de perfil em uma página de conteúdo particular, adicionar um controle ScriptManagerProxy à página de conteúdo e, em seguida, configurar o personalizações de lá. Etapa 3 examinamos como usar o ScriptManagerProxy para registrar um arquivo JavaScript personalizado em uma página com conteúdo específico.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Estrutura AJAX ASP.NET](../../../../ajax/index.md)
- [Tutoriais do AJAX ASP.NET](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Vídeos do AJAX ASP.NET](../../../videos/aspnet-ajax/index.md)
- [Criando Interface do usuário interativa com o Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Usando NEWID aleatoriamente os registros de classificação](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Usando o controle Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Próximo](specifying-the-master-page-programmatically-cs.md)
