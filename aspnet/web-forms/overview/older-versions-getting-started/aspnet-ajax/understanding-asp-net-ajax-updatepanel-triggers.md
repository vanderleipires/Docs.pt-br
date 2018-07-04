---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Noções básicas sobre os gatilhos UpdatePanel do ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Ao trabalhar no editor de marcação no Visual Studio, você pode perceber (do IntelliSense) que há dois elementos filho de um controle UpdatePanel. Uma das perguntas...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 180491b6aee188a9dca750d6d325217f1ad5212c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363715"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Noções básicas sobre os gatilhos UpdatePanel do ASP.NET AJAX
====================
por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Ao trabalhar no editor de marcação no Visual Studio, você pode perceber (do IntelliSense) que há dois elementos filho de um controle UpdatePanel. Um dos quais é o elemento de gatilhos, que especifica os controles na página (ou controle de usuário, se você estiver usando um) que acionará uma renderização parcial do controle UpdatePanel no qual reside o elemento.


## <a name="introduction"></a>Introdução

Tecnologia ASP.NET da Microsoft oferece um modelo de programação orientada a objeto e controlada por evento e une-lo com os benefícios do código compilado. No entanto, seu modelo de processamento do lado do servidor tem várias desvantagens inerentes da tecnologia, muitos dos quais podem ser tratados, os novos recursos incluídos nas extensões do AJAX do Microsoft ASP.NET 3.5. Essas extensões permitem vários novos recursos de cliente avançados, inclusive a renderização parcial de páginas sem exigir uma atualização de página inteira, a capacidade de acessar serviços Web por meio de script de cliente (incluindo a API de criação de perfil do ASP.NET) e uma extensa API do lado do cliente projetado para espelhar a muitos dos esquemas de controle vistos no conjunto de controle de servidor ASP.NET.

Este white paper examina a funcionalidade de gatilhos de XML do ASP.NET AJAX `UpdatePanel` componente. Gatilhos de XML dar controle granular sobre os componentes que podem causar a renderização parcial para os controles UpdatePanel específicos.

Este white paper se baseia na versão Beta 2 do .NET Framework 3.5 e Visual Studio 2008. As extensões do AJAX ASP.NET, anteriormente um assembly de complemento destinado a ASP.NET 2.0, agora estão integradas a biblioteca de classe Base do .NET Framework. Este white paper também pressupõe que você estará trabalhando com o Visual Studio 2008, não Visual Web Developer Express e fornecerá instruções passo a passo de acordo com a interface do usuário do Visual Studio (embora listagens de código serão totalmente compatíveis independentemente de ambiente de desenvolvimento).

## <a name="triggers"></a>*Gatilhos*

Gatilhos para um determinado UpdatePanel, por padrão, incluem automaticamente todos os controles filho que invocam um postback, (por exemplo) incluindo controles de caixa de texto que têm suas `AutoPostBack` propriedade definida como **verdadeiro**. No entanto, os gatilhos também podem ser incluídos declarativamente usando marcação; Isso é feito dentro do `<triggers>` seção da declaração do controle UpdatePanel. Embora os gatilhos podem ser acessados por meio do `Triggers` propriedade de coleção, é recomendável que você registre quaisquer gatilhos de renderização parcial no tempo de execução (por exemplo, se um controle não está disponível em tempo de design) usando o `RegisterAsyncPostBackControl(Control)` método da Objeto ScriptManager para sua página, dentro de `Page_Load` eventos. Lembre-se de que as páginas são sem monitoração de estado e, portanto, você deverá registrar novamente esses controles sempre que eles são criados.

Inclusão de gatilho automático filho também pode ser desabilitado (de modo que os controles filho que criar postagens não disparam automaticamente renderiza parcial), definindo a `ChildrenAsTriggers` propriedade para **falso**. Isso permite maior flexibilidade na atribuição de quais controles específicos pode invocar uma renderização de página e é recomendável, para que um desenvolvedor será opt-in para responder a um evento, em vez de tratamento de todos os eventos que podem surgir.

Observe que quando os controles UpdatePanel são aninhados, quando o UpdateMode é definido como **condicional**, se o filho UpdatePanel for disparado, mas o pai, em seguida, não, é apenas o filho UpdatePanel será atualizado. No entanto, se o pai UpdatePanel é atualizado, em seguida, o UpdatePanel filho também será atualizado.

## <a name="the-lttriggersgt-element"></a>*O &lt;gatilhos&gt; elemento*

Ao trabalhar no editor de marcação no Visual Studio, você pode perceber (do IntelliSense) que há dois elementos filho de um `UpdatePanel` controle. O elemento visto mais frequentemente é o `<ContentTemplate>` elemento, que essencialmente encapsula o conteúdo que será mantido pelo painel de atualização (o conteúdo para o qual estamos possibilitando a renderização parcial). O outro elemento é o `<Triggers>` elemento, que especifica os controles na página (ou controle de usuário, se você estiver usando um) que acionará uma renderização parcial do controle UpdatePanel no qual o &lt;gatilhos&gt; elemento reside.

O `<Triggers>` elemento pode conter qualquer número cada de dois nós filho: `<asp:AsyncPostBackTrigger>` e `<asp:PostBackTrigger>`. Ambas aceitam dois atributos, `ControlID` e `EventName`e pode especificar qualquer controle dentro da unidade atual de encapsulamento (por exemplo, se o controle UpdatePanel reside dentro de um controle de usuário da Web, você não deve tentar fazer referência a um controle em a página na qual o controle de usuário residem).

O `<asp:AsyncPostBackTrigger>` elemento é particularmente útil em que ele pode direcionar qualquer evento de um controle que existe como um filho de *qualquer* controle UpdatePanel na unidade de encapsulamento, não apenas o UpdatePanel sob a qual esse gatilho é um filho . Portanto, qualquer controle pode ser feita para disparar uma atualização de página parcial.

Da mesma forma, o `<asp:PostBackTrigger>` elemento pode ser usado para renderizar uma página parcial do gatilho, mas que exige uma ida e volta completa para o servidor. Este elemento de gatilho também pode ser usado para forçar uma renderização de página inteira quando um controle caso contrário, normalmente acionaria uma renderização de página parcial (por exemplo, quando um `Button` controle existe o `<ContentTemplate>` elemento de um controle UpdatePanel). Novamente, o elemento PostBackTrigger pode especificar qualquer controle que é um filho de qualquer controle UpdatePanel na unidade atual de encapsulamento.

## <a name="lttriggersgt-element-reference"></a>*&lt;Gatilhos&gt; referência de elemento*

*Descendentes de marcação:*

| **Marca** | **Descrição** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Especifica um controle e o evento que fará com que uma atualização parcial de página para o UpdatePanel que contém essa referência de gatilho. |
| &lt;asp:PostBackTrigger&gt; | Especifica um controle e o evento que fará com que uma atualização de página inteira (uma atualização completa de página). Essa marca pode ser usada para forçar uma atualização completa quando um controle dispararia caso contrário, o processamento parcial. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Passo a passo: Cruzado UpdatePanel gatilhos*

1. Crie uma nova página ASP.NET com um objeto ScriptManager definido para permitir o processamento parcial. Adicionar dois UpdatePanels a esta página - na primeira, inclua um controle de rótulo (Label1) e dois controles de botão (Button1 e Button2). Button1 deve dizer clique para atualizar os dois e Button2 deve dizer clique para atualizar isso ou algo semelhante. No segundo UpdatePanel, incluir apenas um controle de rótulo (Label2), mas defina sua propriedade de cor de primeiro plano para algo diferente do padrão para diferenciá-lo.
2. Defina a propriedade UpdateMode de ambas as marcas de UpdatePanel para **condicional**.

**Listagem 1: A marcação para default. aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. No manipulador de eventos Click para Button1, defina Label1.Text e Label2.Text como algo dependente de tempo (como DateTime.Now.ToLongTimeString()). Para o manipulador de eventos para Button2, defina Label1.Text apenas com o valor de dependente de tempo.

**Listagem 2: Code-behind (cortado) em default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Pressione F5 para compilar e executar o projeto. Observe que, quando você clica em painéis de atualização de ambos, ambos os rótulos de alterar o texto; No entanto, quando você clica em um painel esta atualização, apenas Label1 atualizações.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Nos bastidores*

Utilizando o exemplo que acabou de criar, podemos pode dar uma olhada no ASP.NET AJAX está fazendo e como funcionam os gatilhos de painel entre nossos UpdatePanel. Para fazer isso, trabalharemos com a origem da página gerado HTML, bem como a extensão do Mozilla Firefox chamado FireBug - com ele, podemos examinar com facilidade os postbacks AJAX. Também usaremos a ferramenta .NET Reflector por Lutz Roeder ' s. Ambas as ferramentas estão disponíveis gratuitamente online e podem ser encontradas com uma pesquisa na internet.

Um exame do código-fonte página mostra quase nada fora do comum; os controles UpdatePanel são processados como `<div>` contêineres e podemos ver o recurso de script inclui fornecida pelo `<asp:ScriptManager>`. Há também algumas novas chamadas de específico do AJAX para PageRequestManager internas para a biblioteca de script de cliente AJAX. Por fim, podemos ver dois contêineres de UpdatePanel - um com o renderizado `<input>` botões com os dois `<asp:Label>` controles são renderizados como `<span>` contêineres. (Se você inspecionar a árvore DOM no FireBug, você observará que os rótulos estão esmaecidos para indicar que eles não estão produzindo conteúdo visível).

Clique no botão do painel desta atualização e observe que o UpdatePanel superior será atualizado com a hora atual do servidor. No FireBug, escolha a guia Console para que você possa examinar a solicitação. Examine primeiro os parâmetros de solicitação POST:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Observe que o UpdatePanel foi indicado para o código de AJAX do lado do servidor precisamente qual árvore de controle foi acionado por meio do parâmetro ScriptManager1: `Button1` do `UpdatePanel1` controle. Agora, clique no botão de atualização de painéis tanto. Em seguida, examinar a resposta, podemos ver uma série delimitado por pipe de variáveis definidas em uma cadeia de caracteres; Especificamente, podemos ver que o UpdatePanel superior, `UpdatePanel1`, tem todo o seu HTML enviado ao navegador. A biblioteca de script de cliente AJAX substitui original conteúdo em HTML do UpdatePanel com o novo conteúdo por meio de `.innerHTML` propriedade, e, portanto, o servidor envia o conteúdo alterado do servidor como HTML.

Agora, clique no botão de atualização de painéis tanto e examinar os resultados do servidor. Os resultados são muito semelhantes - ambos os UpdatePanels receber novo HTML do servidor. Assim como acontece com o retorno de chamada anterior, o estado da página adicional é enviado.

Como podemos ver, porque nenhum código especial é utilizado para realizar um postback AJAX, a biblioteca de script de cliente do AJAX é capaz de interceptar as postagens de formulário sem nenhum código adicional. Controles de servidor utilizam, automaticamente, JavaScript, de modo que eles não enviar automaticamente o formulário – automaticamente, o ASP.NET injeta código para validação do formulário e estado já, principalmente obtido com a inclusão de recursos de script automático, a classe PostBackOptions e a classe do ClientScriptManager.

Por exemplo, considere um controle de caixa de seleção; Examine a desmontagem de classe no .NET Reflector. Para fazer isso, certifique-se de que o assembly System. Web é aberto e navegue até a `System.Web.UI.WebControls.CheckBox` classe, abrindo o `RenderInputTag` método. Procure uma condicional que verifica o `AutoPostBack` propriedade:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Quando o postback automático está habilitado em uma `CheckBox` controlar (via a propriedade AutoPostBack sendo verdadeiro), o resultante `<input>` marca, portanto, é renderizada com um script de manipulação de eventos do ASP.NET seu `onclick` atributo. A interceptação de envio do formulário, em seguida, permite que o ASP.NET AJAX seja injetado na página nonintrusively, ajudando a evitar qualquer possibilidade de quebrar as alterações que podem ocorrer ao utilizar uma substituição de cadeia de caracteres possivelmente imprecisa. Além disso, isso permite *qualquer* controle ASP.NET personalizado para utilizar o poder do ASP.NET AJAX sem qualquer código adicional para dar suporte ao seu uso dentro de um contêiner de UpdatePanel.

O `<triggers>` funcionalidade corresponde aos valores inicializados na chamada a PageRequestManager \_updateControls (Observe que a biblioteca de script de cliente ASP.NET AJAX utiliza a convenção de que métodos, eventos e nomes de campo que começam com um sublinhado são marcados como internos e não se destinam para uso fora da própria biblioteca). Com ele, podemos pode observar que controla se destinam a causar postbacks AJAX.

Por exemplo, vamos adicionar dois controles adicionais para a página, deixando um controle fora os UpdatePanels inteiramente e deixando uma dentro de um UpdatePanel. Adicionaremos um controle de caixa de seleção no canto superior UpdatePanel e solte uma DropDownList com um número de cores definidas dentro da lista. Aqui está a nova marcação:

**Listagem 3: Nova marcação**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

E aqui está o code-behind novo:

**Listagem 4: code-behind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

A ideia por trás desta página é que a lista suspensa, selecione uma das três cores para mostrar o segundo rótulo, que a caixa de seleção determina se ele está em negrito, e se os rótulos exibem a data, bem como o tempo. A caixa de seleção não deve causar uma atualização do AJAX, mas a lista suspensa deve, mesmo que ele não está hospedado em um UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Como está aparente na captura de tela acima, o botão mais recente para ser clicado era o botão direito do painel esta atualização, que atualizado o tempo superior, independentemente do tempo inferior. A data também foi desligada entre cliques, como a data é visível no rótulo inferior. Por fim de interesse é a cor do rótulo inferior: ele foi atualizado mais recentemente do que o texto do rótulo, que demonstra que o estado do controle é importante, e os usuários esperam que ele seja preservado por meio de postbacks AJAX. *No entanto*, o tempo não foi atualizado. O tempo foi repopulado automaticamente por meio da persistência do \_ \_campo VIEWSTATE da página seja interpretado pelo runtime do ASP.NET quando o controle estava sendo re-renderizado no servidor. O código de servidor ASP.NET AJAX não reconhece na qual os controles de métodos estão alterando o estado; ele simplesmente preenche novamente do estado de exibição e, em seguida, executa os eventos que são apropriados.

Ele deve ser apontado, no entanto, que eu tinha inicializada a hora dentro da página\_evento Load, o tempo seria ter sido incrementado corretamente. Consequentemente, os desenvolvedores devem ter cautela que o código apropriado está sendo executado durante os manipuladores de eventos apropriado e evite o uso da página\_carregar quando um manipulador de eventos do controle seria apropriado.

## <a name="summary"></a>Resumo

O controle UpdatePanel do ASP.NET AJAX Extensions é versátil e pode utilizar um número de métodos para identificar os eventos de controle que devem fazer com que a atualização. Ele dá suporte a seus controles filho que está sendo atualizada automaticamente, mas também pode responder a eventos de controle em outro lugar na página.

Para reduzir o potencial de carga de processamento do servidor, é recomendável que o `ChildrenAsTriggers` propriedade de um UpdatePanel ser definida como `false`, e que eventos ser aceito-em vez de incluídos por padrão. Isso também impede que todos os eventos desnecessários causando efeitos potencialmente indesejado, inclusive validação e alterações em campos de entrada. Esses tipos de erros podem ser difícil isolar, porque a página de atualizações de forma transparente para o usuário e a causa pode, portanto, não ser imediatamente óbvia.

Examinando o funcionamento interno do formulário do ASP.NET AJAX após o modelo de interceptação, fomos capazes de determinar que ela utiliza a estrutura já fornecida pelo ASP.NET. Dessa forma, ele preserva a compatibilidade máxima com controles criados usando a mesma estrutura e minimamente atua em qualquer JavaScript adicional criado para a página.

## <a name="bio"></a>Biografia

Rob Paveza é desenvolvedor sênior de aplicativos .NET em Terralever ([www.terralever.com](http://www.terralever.com)), uma empresa de marketing interativa à esquerda no Tempe, AZ. Ele pode ser contatado pelo [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), e seu blog está localizado em [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Cate tem trabalhado com tecnologias Web Microsoft desde 1997 e é o presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especialista na escrita de ASP.NET com base em aplicativos com foco em soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado através do email [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Próximo](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
