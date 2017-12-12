---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: "Noções básicas sobre o ASP.NET AJAX UpdatePanel gatilhos | Microsoft Docs"
author: scottcate
description: "Ao trabalhar no editor de marcação no Visual Studio, você pode perceber (de IntelliSense) que há dois elementos filho de um controle UpdatePanel. Uma das perguntas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 1338ef0763d9bfab451bc30cafa39f715200153d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Noções básicas sobre o ASP.NET AJAX UpdatePanel gatilhos
====================
por [Scott Ndicar](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Ao trabalhar no editor de marcação no Visual Studio, você pode perceber (de IntelliSense) que há dois elementos filho de um controle UpdatePanel. Um dos quais é o elemento de gatilhos, que especifica os controles na página (ou controle de usuário, se você estiver usando um) que acionará uma renderização parcial do controle UpdatePanel no qual reside o elemento.


## <a name="introduction"></a>Introdução

Tecnologia ASP.NET da Microsoft oferece um modelo de programação orientada a objeto e controlada por evento e une-lo com os benefícios do código compilado. No entanto, seu modelo de processamento no lado do servidor tem várias desvantagens inerentes a tecnologia, muitos dos quais podem ser endereçados por novos recursos incluídos nas extensões de AJAX do Microsoft ASP.NET 3.5. Essas extensões permitem que vários novos recursos de cliente avançados, incluindo o processamento parcial de páginas sem exigir uma atualização de página inteira, a capacidade de acessar os serviços da Web por meio de script de cliente (incluindo a API de criação de perfil do ASP.NET) e uma extensa API de cliente projetado para espelhar muitos dos esquemas de controle vistos no conjunto de controle de servidor ASP.NET.

Este white paper examina a funcionalidade de gatilhos de XML do ASP.NET AJAX `UpdatePanel` componente. Gatilhos de XML oferecem um controle granular sobre os componentes que podem fazer com que o processamento parcial para controles do UpdatePanel específicos.

Este white paper se baseia na versão Beta 2 do .NET Framework 3.5 e Visual Studio 2008. O ASP.NET AJAX Extensions, anteriormente um assembly de complemento voltado para ASP.NET 2.0, agora estão integrados a biblioteca de classes de Base do .NET Framework. Este white paper também presume que você estará trabalhando com o Visual Studio 2008, não Visual Web Developer Express e fornece instruções passo a passo de acordo com a interface do usuário do Visual Studio (embora as listagens de código será totalmente compatíveis, independentemente de ambiente de desenvolvimento).

## <a name="triggers"></a>*Gatilhos*

Para um determinado UpdatePanel, por padrão, os gatilhos incluem automaticamente os controles filho que invocam um postback, (por exemplo) incluindo controles de caixa de texto que têm suas `AutoPostBack` propriedade definida como **true**. No entanto, os gatilhos também podem ser incluídos declarativamente usando marcação; Isso é feito dentro de `<triggers>` seção da declaração de controle UpdatePanel. Embora os gatilhos podem ser acessados por meio de `Triggers` propriedade de coleção, é recomendável que você registre os disparadores de renderização parcial em tempo de execução (por exemplo, se um controle não está disponível em tempo de design) usando o `RegisterAsyncPostBackControl(Control)` método do ScriptManager objeto dentro de sua página de `Page_Load` eventos. Lembre-se de que as páginas são sem monitoração de estado e assim você deve registrar novamente esses controles sempre que eles são criados.

Inclusão de gatilho automático filho também pode ser desabilitado (de forma que os controles filho que criar postbacks não disparam automaticamente renderiza parcial), definindo o `ChildrenAsTriggers` propriedade **false**. Isso permite maior flexibilidade na atribuição de quais controles específicos pode invocar uma renderização de página e é recomendável, para que um desenvolvedor será participar para responder a um evento, em vez de tratar todos os eventos que podem surgir.

Observe que quando os controles do UpdatePanel são aninhados, quando o UpdateMode está definido como **condicional**, se o filho do UpdatePanel for disparado, mas o pai não for, somente o filho UpdatePanel será atualizado. No entanto, se o UpdatePanel pai é atualizado, em seguida, o filho do UpdatePanel também serão atualizado.

## <a name="the-lttriggersgt-element"></a>*O &lt;gatilhos&gt; elemento*

Ao trabalhar no editor de marcação no Visual Studio, você pode perceber (de IntelliSense) que há dois elementos filho de um `UpdatePanel` controle. O elemento mais frequentemente visto é o `<ContentTemplate>` elemento, que é essencialmente encapsula o conteúdo que será mantido pelo painel de atualização (o conteúdo para o qual estamos estiver habilitando o processamento parcial). O outro elemento é o `<Triggers>` elemento, que especifica os controles na página (ou controle de usuário, se você estiver usando um) que acionará uma renderização parcial do controle UpdatePanel em que o &lt;gatilhos&gt; elemento reside.

O `<Triggers>` elemento pode conter qualquer número a cada dois nós filho: `<asp:AsyncPostBackTrigger>` e `<asp:PostBackTrigger>`. Aceitar dois atributos, `ControlID` e `EventName`e pode especificar qualquer controle dentro da unidade atual do encapsulamento (por exemplo, se o controle UpdatePanel reside em um controle de usuário da Web, você não deve tentar fazer referência a um controle em a página em que o controle de usuário residem).

O `<asp:AsyncPostBackTrigger>` elemento é particularmente útil em que ele pode direcionar qualquer evento de um controle que existe como um filho de *qualquer* controle UpdatePanel na unidade de encapsulamento, não apenas o UpdatePanel sob a qual esse gatilho é um filho . Portanto, qualquer controle pode ser feito para disparar uma atualização parcial de página.

Da mesma forma, o `<asp:PostBackTrigger>` elemento pode ser usado para renderizar uma página parcial de disparador, mas requer uma viagem completa para o servidor. Este elemento de gatilho também pode ser usado para forçar uma renderização de página inteira quando um controle caso contrário, normalmente acionaria uma renderização de página parcial (por exemplo, quando um `Button` controle existe o `<ContentTemplate>` elemento de um controle UpdatePanel). Novamente, o elemento PostBackTrigger pode especificar qualquer controle que é um filho de qualquer controle UpdatePanel na unidade atual de encapsulamento.

## <a name="lttriggersgt-element-reference"></a>*&lt;Gatilhos&gt; referência de elemento*

*Marcação descendentes:*

| **Marca** | **Descrição** |
| --- | --- |
| &lt;ASP: AsyncPostBackTrigger&gt; | Especifica um controle e o evento que fará com que uma atualização parcial de página para o UpdatePanel que contém esta referência de gatilho. |
| &lt;ASP: PostBackTrigger&gt; | Especifica um controle e o evento que fará com que uma atualização de página inteira (uma atualização de página inteira). Essa marca pode ser usada para forçar uma atualização completa quando um controle dispararia caso contrário, o processamento parcial. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Passo a passo: UpdatePanel entre gatilhos*

1. Crie uma nova página ASP.NET com um objeto ScriptManager definido para permitir o processamento parcial. Adicionar dois UpdatePanels a esta página - na primeira, incluem um controle de rótulo (Label1) e dois controles de botão (Button1 e Button2). Button1 deve dizer clique para atualizar e Button2 deve dizer clique para atualizar esse ou algo semelhante. No segundo UpdatePanel, incluir apenas um controle de rótulo (Label2), mas defina sua propriedade de cor de primeiro plano para algo diferente do padrão para diferenciá-la.
2. Defina a propriedade UpdateMode de ambas as marcas de UpdatePanel para **condicional**.

**Listando 1: A marcação de default.aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. No manipulador de eventos de clique para Button1, definir Label1.Text e Label2.Text para algo dependentes de tempo (como DateTime.Now.ToLongTimeString()). Para o manipulador de eventos para Button2, defina Label1.Text apenas com o valor de dependentes de tempo.

**Listando 2: Code-behind (cortado) em default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Pressione F5 para compilar e executar o projeto. Observe que, quando você clica em painéis de atualização de ambos, ambos os rótulos alterar texto; No entanto, quando você clica em um painel esta atualização, apenas Label1 atualizações.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Nos bastidores*

Utilizando o exemplo que acabou de criar, podemos pode dar uma olhada o ASP.NET AJAX e como funcionam os gatilhos de painel entre UpdatePanel. Para fazer isso, trabalharemos com a origem da página gerado HTML, bem como a extensão Mozilla Firefox chamado FireBug - com, podemos examinar facilmente os postbacks AJAX. Também, usaremos a ferramenta .NET Reflector por Lutz Roeder. Ambas as ferramentas estão disponíveis gratuitamente online e podem ser encontradas com uma pesquisa na internet.

Um exame do código-fonte página mostra quase nada fora do comum; os controles do UpdatePanel são renderizados como `<div>` contêineres e pode ver o recurso de script inclui fornecido pelo `<asp:ScriptManager>`. Também há algumas novas chamadas de específico do AJAX para PageRequestManager internas para a biblioteca de script de cliente AJAX. Por fim, podemos ver dois contêineres UpdatePanel - um com o renderizado `<input>` botões com os dois `<asp:Label>` controles renderizado como `<span>` contêineres. (Se você inspecionar a árvore DOM FireBug, você observará que os rótulos estão esmaecidos para indicar que eles não estão produzindo conteúdo visível).

Clique no botão Painel esta atualização e observe que o superior UpdatePanel será atualizado com a hora atual do servidor. No FireBug, escolha a guia Console de forma que você pode examinar a solicitação. Examine primeiro os parâmetros de solicitação POST:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Observe que o UpdatePanel foi indicado para o código de AJAX do lado do servidor com precisão qual árvore de controle foi acionado por meio do parâmetro ScriptManager1: `Button1` do `UpdatePanel1` controle. Agora, clique no botão de painéis tanto de atualização. Em seguida, examinar a resposta, podemos ver uma série de delimitada de variáveis definidas em uma cadeia de caracteres; Especificamente, podemos ver que o UpdatePanel superior, `UpdatePanel1`, tem todo o HTML enviado ao navegador. A biblioteca de script de cliente AJAX substitui original conteúdo em HTML do UpdatePanel com o novo conteúdo por meio de `.innerHTML` propriedade, e, portanto, o servidor envia o conteúdo alterado do servidor como HTML.

Agora, clique no botão painéis tanto de atualização e examine os resultados do servidor. Os resultados são muito semelhantes - ambos os UpdatePanels receber o novo HTML do servidor. Assim como acontece com o retorno de chamada anterior, o estado de página adicionais é enviado.

Como podemos ver, porque nenhum código especial é utilizado para executar um postback AJAX, a biblioteca de script de cliente AJAX é intercepte postbacks de formulário sem qualquer código adicional. Controles de servidor utilizam JavaScript automaticamente para que eles não enviar automaticamente o formulário - ASP.NET automaticamente insere um código de validação do formulário e estado, principalmente obtido com a inclusão de recursos de script automático, a classe PostBackOptions e a classe ClientScriptManager.

Por exemplo, considere um controle de caixa de seleção; Examine a desmontagem de classe no .NET Reflector. Para fazer isso, certifique-se de que o assembly System. Web é aberto e navegue até o `System.Web.UI.WebControls.CheckBox` classe, abrindo o `RenderInputTag` método. Procure uma condicional que verifica a `AutoPostBack` propriedade:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Quando o postback automático está habilitado em um `CheckBox` controlar (via a propriedade AutoPostBack sendo verdadeiro), o resultante `<input>` marca, portanto, é renderizada com um script de manipulação de eventos do ASP.NET seu `onclick` atributo. A interceptação de envio do formulário, em seguida, permite que o ASP.NET AJAX ao serem injetados na página nonintrusively, ajuda a evitar possíveis alterações de quebra que podem ocorrer com o uso de uma substituição de cadeia de caracteres possivelmente imprecisa. Além disso, isso permite *qualquer* controle personalizado do ASP.NET para utilizar o poder do ASP.NET AJAX sem qualquer código adicional para dar suporte ao seu uso dentro de um contêiner de UpdatePanel.

O `<triggers>` funcionalidade corresponde aos valores inicializados na chamada PageRequestManager \_updateControls (Observe que a biblioteca de script de cliente ASP.NET AJAX utiliza a convenção de que nomes de campo que começam, métodos e eventos com um sublinhado são marcados como internos e não se destinam para uso fora da própria biblioteca). Com ele, podemos pode observar quais controles devem causar postbacks AJAX.

Por exemplo, vamos adicionar dois controles adicionais para a página, deixando um controle fora os UpdatePanels inteiramente e deixando uma dentro de um UpdatePanel. É adicionar um controle de caixa de seleção no UpdatePanel superior e descartar DropDownList com um número de cores definidas dentro da lista. Aqui está a marcação novo:

**A listagem 3: Nova marcação**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

E aqui está o code-behind novo:

**A listagem 4: code-behind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

A ideia por trás desta página é que a lista suspensa seleciona uma das três cores para mostrar o rótulo de segundo, que a caixa de seleção determina se ele está em negrito, e se os rótulos exibem a data, bem como o tempo. A caixa de seleção não deve causar uma atualização AJAX, mas a lista suspensa deve, mesmo que ele não está hospedado em um UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Como é aparente na captura de tela acima, o botão mais recente para ser clicado era o botão direito do painel esta atualização, que atualizar o tempo superior, independentemente da hora inferior. A data também foi desligada entre cliques, como a data é visível no rótulo da parte inferior. Finalmente interesse é cor do rótulo inferior: ele foi atualizado mais recentemente do que o texto do rótulo, que demonstra que o estado de controle é importante, e os usuários esperam para ser preservada durante postbacks AJAX. *No entanto*, o tempo não foi atualizado. O tempo foi automaticamente preenchida novamente por meio de persistência do \_ \_campo VIEWSTATE da página seja interpretado pelo tempo de execução do ASP.NET quando o controle estava sendo processado novamente no servidor. O código de servidor AJAX ASP.NET não reconhece em que os controles de métodos estão alterando o estado; ele simplesmente preenche do estado de exibição e, em seguida, executa os eventos que são apropriados.

Ele deve ser indicado, no entanto, que eu tivesse inicializado tempo dentro da página\_eventos de carga, o tempo poderia ter for incrementado corretamente. Consequentemente, os desenvolvedores devem cuidado-se de que o código apropriado está sendo executado durante os manipuladores de eventos apropriado e evitar o uso da página\_carregar quando um manipulador de eventos do controle seria apropriado.

## <a name="summary"></a>Resumo

O controle UpdatePanel do ASP.NET AJAX Extensions versátil e pode usar vários métodos para identificar os eventos de controle que devem fazer com que ele seja atualizado. Ele dá suporte a serem atualizados automaticamente por seus controles filhos, mas também pode responder a eventos de controle em outro lugar na página.

Para reduzir o potencial de carga de processamento do servidor, é recomendável que o `ChildrenAsTriggers` propriedade de um UpdatePanel ser definida como `false`, e que os eventos ser incluído-em vez de incluído por padrão. Isso também impede que todos os eventos desnecessários causando efeitos potencialmente indesejado, inclusive validação e alterações para campos de entrada. Esses tipos de erros podem ser difícil isolar, porque a página de atualizações de forma transparente para o usuário e, portanto, não é possível imediatamente óbvia.

Examinando o funcionamento interno do formulário ASP.NET AJAX após o modelo de interceptação, pudemos determinar que utiliza a estrutura já fornecida pelo ASP.NET. Dessa forma, ele preserva a máxima compatibilidade com controles criados usando a mesma estrutura e minimamente atua em qualquer adicional JavaScript gravado para a página.

## <a name="bio"></a>Biografia do

Rob Paveza é um desenvolvedor de aplicativos .NET sênior em Terralever ([www.terralever.com](http://www.terralever.com)), uma empresa de marketing interativa líder em Tempe, AZ. Ele pode ser contatado em [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), e seu blog está localizado em [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott Cate trabalha com tecnologias Microsoft Web desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especializada em escrever ASP.NET com base em aplicativos voltados para soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado via email em [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Anterior](understanding-partial-page-updates-with-asp-net-ajax.md)
[Próximo](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
