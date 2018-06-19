---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Noções básicas sobre atualizações de página parcial com o ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Talvez o recurso mais visível das extensões AJAX do ASP.NET é a capacidade de fazer atualizações de uma página parcial ou incremental sem fazer um postback completo para t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 91a98bf1c9a71ae84c569f7ae40930422cb652e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891315"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Página parcial Noções básicas sobre atualizações com o ASP.NET AJAX
====================
por [Scott Ndicar](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Talvez o recurso mais visível das extensões AJAX do ASP.NET é a capacidade de fazer atualizações de uma página parcial ou incremental sem fazer um postback completo para o servidor, sem alterações de código e alterações mínimas de marcação. As vantagens são abrangentes – o estado do seu multimídia (como o Adobe Flash ou o Windows Media) é alterado, os custos de largura de banda são reduzidos e o cliente não apresenta a cintilação geralmente associada com um postback.


## <a name="introduction"></a>Introdução

Tecnologia ASP.NET da Microsoft oferece um modelo de programação orientada a objeto e controlada por evento e une-lo com os benefícios do código compilado. No entanto, seu modelo de processamento no lado do servidor tem várias desvantagens inerentes a tecnologia:

- Atualizações de página requerem uma viagem para o servidor, o que requer uma atualização de página.
- Ciclos não persistem efeitos gerados por Javascript ou outras tecnologias de cliente (como o Adobe Flash)
- Durante um postback, navegadores que não seja o Microsoft Internet Explorer não dão suporte a restauração automaticamente a posição de rolagem. E até mesmo no Internet Explorer, ainda há a cintilação enquanto a página é atualizada.
- Postagens podem envolver uma grande quantidade de largura de banda como o \_ \_campo de formulário VIEWSTATE pode crescer, especialmente ao lidar com controles como o controle GridView ou repetidores.
- Não há nenhum modelo unificado para acessar os serviços da Web por meio de JavaScript ou outras tecnologias do lado do cliente.

Insira as extensões AJAX ASP.NET da Microsoft. AJAX, o que significa **um** síncrona **J** avaScript **um** nd **X** ML, é uma estrutura integrada para fornecer página incremental atualizações por meio de JavaScript da plataforma cruzada, composta de código do lado do servidor que inclui o Microsoft AJAX Framework e um componente de script chamado a biblioteca de Script do Microsoft AJAX. Os ASP.NET AJAX extensions também fornecem suporte de plataforma cruzada para Acessando serviços Web do ASP.NET por meio de JavaScript.

Este white paper examina a funcionalidade de atualizações parciais de página do ASP.NET AJAX Extensions, que inclui o componente ScriptManager, o controle UpdatePanel e o controle UpdateProgress e considera os cenários em que eles devem ou não devem ser utilizada.

Este white paper baseia-se na versão Beta 2 do Visual Studio 2008 e o .NET Framework 3.5, que integra o ASP.NET AJAX Extensions para a biblioteca de classe Base (onde era anteriormente um componente complementar disponível para o ASP.NET 2.0). Este white paper também pressupõe que você está usando o Visual Studio 2008 e não o Visual Web Developer Express Edition; Alguns modelos de projeto que são referenciados não podem estar disponíveis para usuários do Visual Web Developer Express.

## <a name="partial-page-updates"></a>Atualizações parciais de página

Talvez o recurso mais visível das extensões AJAX do ASP.NET é a capacidade de fazer atualizações de uma página parcial ou incremental sem fazer um postback completo para o servidor, sem alterações de código e alterações mínimas de marcação. As vantagens são abrangentes - o estado do seu multimídia (como o Adobe Flash ou o Windows Media) é alterado, os custos de largura de banda são reduzidos e o cliente não apresenta a cintilação geralmente associada com um postback.

A capacidade de integrar o processamento parcial da página é integrada ao ASP.NET com alterações mínimas em seu projeto.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Passo a passo: Integrando o processamento parcial em um projeto existente


1. No Microsoft Visual Studio 2008, crie um novo projeto de Site da Web ASP.NET, vá para <em>arquivo</em>  <em>- &gt; novo</em>  <em>- &gt; Web Site</em> e selecionando o Site da Web ASP.NET da caixa de diálogo. Você pode atribuir o nome que você quiser, e você pode instalá-lo para o sistema de arquivos ou em serviços de informações da Internet (IIS).
2. Você verá a página padrão em branco com marcação básica do ASP.NET (um formulário do lado do servidor e um `@Page` diretiva). Descartar um rótulo chamado `Label1` e um botão chamado `Button1` para a página de dentro do elemento de formulário. Você pode definir suas propriedades de texto para que você quiser.
3. No modo Design, clique duas vezes em `Button1` para gerar um manipulador de eventos por trás do código. Dentro do manipulador de eventos, definir `Label1.Text` para você clicou no botão! .

**Listando 1: Marcação de default.aspx antes do processamento parcial está habilitado**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Listando 2: Code-behind (cortado) em default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Pressione F5 para iniciar o site da web. O Visual Studio solicitará que você adicionar um arquivo Web. config para habilitar a depuração; fazer isso. Quando você clica no botão, observe que a página é atualizada para alterar o texto no rótulo, e há uma breve cintilação como a página é redesenhada.
2. Depois de fechar a janela do navegador, retorne ao Visual Studio e para a página de marcação. Role para baixo na caixa de ferramentas do Visual Studio e localize a guia chamada AJAX Extensions. (Se você não tiver esta guia porque você está usando uma versão mais antiga de extensões AJAX ou Atlas, consulte o passo a passo para registrar os itens da caixa de ferramentas de extensões AJAX mais tarde neste white paper, ou instale a versão atual com o Windows Installer para download do site).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Problema conhecido:</em>se você instalar o Visual Studio 2008 em um computador que já tenha o Visual Studio 2005 instalado com as extensões AJAX do ASP.NET 2.0, o Visual Studio 2008 importará os itens da caixa de ferramentas de extensões AJAX. Você pode determinar se esse for o caso, examinando-se a dica de ferramenta de componentes. eles devem dizer versão 3.5.0.0. Se dizem versão 2.0.0.0, em seguida, você importou seus itens antigos da caixa de ferramentas e será necessário importá-las manualmente usando a caixa de diálogo Escolher itens da caixa de ferramentas no Visual Studio. Não será possível adicionar controles de versão 2 por meio do designer.

2. Antes do `<asp:Label>` marca começa, criar uma linha em branco e clique duas vezes no controle UpdatePanel na caixa de ferramentas. Observe que um novo `@Register` diretiva está incluída na parte superior da página, indicando que os controles no namespace System.Web.UI devem ser importados usando o `asp:` prefixo.
3. Arraste o fechamento `</asp:UpdatePanel>` marca após o fim do elemento Button, para que o elemento está bem formado com os controles de rótulo e o botão encapsulados.
4. Após a abertura `<asp:UpdatePanel>` marca, começar a abrir uma nova marca. Observe que o IntelliSense solicitações duas opções. Nesse caso, crie um `<ContentTemplate>` marca. Certifique-se de quebrar essa marca ao redor de seu rótulo e um botão para que a marcação seja bem formada.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. Em qualquer lugar dentro do `<form>` elemento, incluem um controle ScriptManager clicando duas vezes no `ScriptManager` item na caixa de ferramentas.
2. Editar o `<asp:ScriptManager>` marca para que ela inclua o atributo `EnablePartialRendering= true`.

**Listando 3: Marcação de default.aspx com o processamento parcial habilitado**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Abra o arquivo Web. config. Observe que o Visual Studio adicionou uma referência de compilação para System.Web.Extensions.dll automaticamente.

1. O que há de novo no Visual Studio 2008: O Web. config que vem com os modelos de projeto de Site da Web ASP.NET automaticamente inclui todas as referências necessárias para o ASP.NET AJAX Extensions e comentadas seções de informações de configuração que podem ser não é comentado para habilitar a funcionalidade adicional. O Visual Studio 2005 tinha modelos semelhantes quando extensões AJAX ASP.NET 2.0 foram instaladas. No entanto, no Visual Studio 2008, as extensões AJAX são recusar por padrão (ou seja, eles são referenciados por padrão, mas pode ser removidos como referências).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Pressione F5 para iniciar o seu site. Observe como nenhuma alteração de código de origem foi necessária para dar suporte ao processamento parcial - marcação só foi alterado.

Quando você inicia o seu site, você verá que o processamento parcial é habilitado, pois quando você clica no botão existe será sem cintilação, nem haverá qualquer alteração na posição de rolagem de página (neste exemplo não Demonstre que,). Se você observar a fonte da página renderizada após clicar no botão, ele confirmará que na verdade um POST-backup não ocorreu - o texto do rótulo original ainda é parte da marcação de origem e o rótulo foi alterado por meio de JavaScript.

O Visual Studio 2008 não parecem vir com um modelo predefinido para um site da web habilitado para AJAX do ASP.NET. No entanto, modelo estava disponível no Visual Studio 2005, se o Visual Studio 2005 e extensões AJAX ASP.NET 2.0 foram instaladas. Consequentemente, configurar um site da web e iniciar com o modelo de Site de Web habilitado para AJAX provavelmente será ainda mais fácil, pois o modelo deve incluir um arquivo Web. config totalmente configurado (suporte a todas as extensões AJAX do ASP.NET, incluindo o acesso de serviços Web e serialização JSON - JavaScript Object Notation) e inclui um UpdatePanel e ContentTemplate dentro da página de Web Forms principal por padrão. Habilitar o processamento parcial com essa página padrão é tão simple quanto revisitar etapa 10 deste passo a passo e soltando os controles para a página.

## <a name="the-scriptmanager-control"></a>O controle ScriptManager

## <a name="scriptmanager-control-reference"></a>Referência de controle ScriptManager

Propriedades da marcação habilitada:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | Especifica se deve usar a seção de erro personalizada do arquivo Web. config para lidar com erros. |
| AsyncPostBackError-Message | Cadeia de caracteres | Obtém ou define a mensagem de erro enviada ao cliente se ocorrer um erro. |
| AsyncPostBack-Timeout | Int32 | Obtém ou define o valor padrão de um tempo em que um cliente deve esperar para concluir a solicitação assíncrona. |
| EnableScript-Globalization | Bool | Obtém ou define se globalização de script está habilitada. |
| EnableScript-Localization | Bool | Obtém ou define se a localização do script está habilitada. |
| ScriptLoadTimeout | Int32 | Determina o número de segundos permitido para carregamento de scripts no cliente |
| ScriptMode | Enum (Auto, depurar, versão, herdam) | Obtém ou define se deve processar versões de lançamento de scripts |
| ScriptPath | Cadeia de caracteres | Obtém ou define o caminho raiz para o local dos arquivos de script a ser enviado ao cliente. |

Propriedades de código:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Obtém os detalhes sobre o proxy do serviço de autenticação do ASP.NET que será enviada ao cliente. |
| IsDebuggingEnabled | Bool | Obtém se script e depuração de código está habilitada. |
| IsInAsyncPostback | Bool | Obtém se a página está atualmente em uma solicitação de postagem assíncrona. |
| ProfileService | ProfileService-Manager | Obtém os detalhes sobre o proxy do serviço de criação de perfil do ASP.NET que será enviada ao cliente. |
| Scripts | Coleção&lt;referência de Script&gt; | Obtém uma coleção de referências de script que será enviada ao cliente. |
| Serviços | Coleção&lt;referência de serviço&gt; | Obtém uma coleção de referências de proxy de serviço Web que será enviada ao cliente. |
| SupportsPartialRendering | Bool | Obtém se o cliente atual oferece suporte ao processamento parcial. Se essa propriedade retorna **false**, todas as solicitações de página serão postagens padrão. |

Métodos públicos de código:

| **Nome do método** | **Tipo** | **Descrição** |
| --- | --- | --- |
| SetFocus(string) | Void | Define o foco do cliente para um determinado controle quando a solicitação foi concluída. |

Marcação descendentes:

| **Tag** | **Descrição** |
| --- | --- |
| &lt;AuthenticationService&gt; | Fornece detalhes sobre o proxy para o serviço de autenticação do ASP.NET. |
| &lt;ProfileService&gt; | Fornece detalhes sobre o proxy para a criação de perfil de serviço do ASP.NET. |
| &lt;Scripts&gt; | Fornece referências de script adicionais. |
| &lt;asp:ScriptReference&gt; | Indica uma referência de script específico. |
| &lt;Serviço&gt; | Fornece referências adicionais do serviço Web que terão as classes de proxy geradas. |
| &lt;asp:ServiceReference&gt; | Indica uma referência de serviço Web específica. |

O controle ScriptManager é o núcleo essencial para o ASP.NET AJAX Extensions. Ele fornece acesso à biblioteca de script (incluindo o sistema de tipos abrangentes script do lado do cliente), oferece suporte à renderização parcial e fornece amplo suporte para serviços adicionais do ASP.NET (como autenticação e criação de perfil, mas também outros serviços da Web). O controle ScriptManager também fornece suportam de globalização e localização para os scripts de cliente.

## <a name="providing-alterative-and-supplemental-scripts"></a>Fornecendo Scripts alternativo e complementares

Enquanto o Microsoft ASP.NET 2.0 AJAX Extensions incluir o código de script inteiro em ambos os depuração e liberação edições como recursos incorporados no assembly referenciado, os desenvolvedores estão livres para redirecionar o ScriptManager para os arquivos de script personalizado, bem como registrar scripts necessárias adicionais.

Para substituir a associação padrão para os scripts incluídos normalmente (como aqueles que dão suporte a namespace WebForms e o sistema de digitação personalizado), você pode se registrar para o `ResolveScriptReference` evento da classe ScriptManager. Quando este método é chamado, o manipulador de eventos tem a oportunidade de alterar o caminho para o arquivo de script em questão; o Gerenciador de script, em seguida, enviará uma cópia personalizada ou diferente dos scripts para o cliente.

Além disso, referências de script (representado pela `ScriptReference` classe) pode ser incluído programaticamente ou por meio de marcação. Para fazer isso, modifique programaticamente o `ScriptManager.Scripts` coleção, ou incluir `<asp:ScriptReference>` marcas sob o `<Scripts>` marca, que é um filho de primeiro nível do controle ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Tratamento UpdatePanels de erro personalizado

Embora as atualizações são controladas por gatilhos especificados por controles do UpdatePanel, o suporte para tratamento de erros e mensagens de erro personalizadas é tratado pela instância do controle ScriptManager da página. Isso é feito pela exposição de um evento, `AsyncPostBackError`, para a página que pode, em seguida, fornecer lógica de tratamento de exceção personalizada.

Consome o evento AsyncPostBackError, você pode especificar o `AsyncPostBackErrorMessage` propriedade, que, em seguida, faz com que uma caixa de alerta a ser gerado após a conclusão do retorno de chamada.

Personalização no lado do cliente também é possível em vez de usar a caixa de alerta padrão; Por exemplo, você talvez queira exibir um personalizado `<div>` elemento, em vez da caixa de diálogo padrão navegador modal. Nesse caso, você pode manipular o erro no script de cliente:

**Listando 5: O script do lado do cliente para exibir os erros personalizados**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Simplesmente, o script acima registra um retorno de chamada com o tempo de execução do cliente AJAX para quando a solicitação assíncrona for concluída. Em seguida, verifica se um erro foi relatado e nesse caso, processa os detalhes de, por fim que indica o tempo de execução que o erro foi tratado em um script personalizado.

## <a name="globalization-and-localization-support"></a>Suporte de localização e globalização

O controle ScriptManager fornece amplo suporte para localização de cadeias de caracteres de script e componentes de interface do usuário; No entanto, esse tópico está fora do escopo deste white paper. Para obter mais informações, consulte o white paper, suporte de globalização no ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>O controle UpdatePanel

## <a name="updatepanel-control-reference"></a>Referência de controle UpdatePanel

Propriedades da marcação habilitada:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Especifica se os controles filho automaticamente chamam atualização em um postback. |
| RenderMode | Enum (bloco, embutido) | Especifica a maneira como o conteúdo será apresentado visualmente. |
| UpdateMode | Enum (sempre, condicional) | Especifica se o UpdatePanel sempre é atualizado durante uma renderização parcial ou se ele é atualizado somente quando um disparador for atingido. |

Propriedades de código:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| IsInPartialRendering | bool | Obtém se o UpdatePanel estiver dando suporte a processamento parcial para a solicitação atual. |
| ContentTemplate | ITemplate | Obtém o modelo de marcação para a solicitação de atualização. |
| ContentTemplateContainer | Controle | Obtém o modelo de programação para a solicitação de atualização. |
| Gatilhos | UpdatePanel - TriggerCollection | Obtém a lista de disparadores associados com o UpdatePanel atual. |

Métodos públicos de código:

| **Nome do método** | **Tipo** | **Descrição** |
| --- | --- | --- |
| Update) | Void | Atualiza o UpdatePanel especificado programaticamente. Permite que uma solicitação de servidor disparar uma renderização parcial de um UpdatePanel caso contrário não disparado. |

Marcação descendentes:

| **Tag** | **Descrição** |
| --- | --- |
| &lt;ContentTemplate&gt; | Especifica a marcação a ser usado para renderizar o resultado da renderização parcial. Filho de &lt;asp: UpdatePanel&gt;. |
| &lt;Gatilhos&gt; | Especifica uma coleção de *n* controles associados ao atualizar este UpdatePanel. Filho de &lt;asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Especifica um gatilho que invoca uma renderização de página parcial para o UpdatePanel determinado. Isso pode ou não ser um controle como um descendente do UpdatePanel em questão. Granulares para o nome do evento. Filho de &lt;gatilhos&gt;. |
| &lt;asp:PostBackTrigger&gt; | Especifica um controle que faz com que toda a página Atualizar. Isso pode ou não ser um controle como um descendente do UpdatePanel em questão. Granulares para o objeto. Filho de &lt;gatilhos&gt;. |

O `UpdatePanel` controle é o controle que delimita o conteúdo do servidor que levará a parte à funcionalidade de processamento parcial das extensões AJAX. Não há nenhum limite para o número de controles do UpdatePanel que pode estar em uma página, e eles podem ser aninhados. Cada UpdatePanel é isolado, de forma que cada um pode trabalhar de forma independente (você pode ter dois UpdatePanels em execução ao mesmo tempo, de renderização diferentes partes da página, independente de postback da página).

O UpdatePanel controlar lida principalmente com gatilhos de controle - por padrão, qualquer controle dentro de um UpdatePanel `ContentTemplate` que cria uma nova postagem é registrado como um disparador para o UpdatePanel. Isso significa que o UpdatePanel pode trabalhar com os controles de associação de dados padrão (como o GridView), com controles de usuário, e eles podem ser programados no script.

Por padrão, quando uma renderização de página parcial é disparada, todos os controles do UpdatePanel na página serão atualizados, se o UpdatePanel controles definidos gatilhos para tal ação. Por exemplo, se o UpdatePanel define um controle de botão, e esse controle de botão é clicado, todos os controles do UpdatePanel nessa página serão atualizados por padrão. Isso ocorre porque, por padrão, o `UpdateMode` do UpdatePanel é definida como `Always`. Como alternativa, você pode definir a propriedade UpdateMode `Conditional`, o que significa que o UpdatePanel será atualizado somente se um gatilho específico for atingido.

## <a name="custom-control-notes"></a>Notas de controle personalizado

Um UpdatePanel pode ser adicionado a qualquer controle de usuário ou um controle personalizado; No entanto, a página em que esses controles estão incluídos também deve incluir um controle ScriptManager com a propriedade EnablePartialRendering é definida como **true**.

Uma maneira na qual você pode levar isso em conta ao usar controles personalizados da Web é substituir protegido `CreateChildControls()` método o `CompositeControl` classe. Fazendo isso, você pode injetar um UpdatePanel entre filhos do controle e o mundo exterior se você determinar que a página dá suporte a processamento parcial; Caso contrário, você pode simplesmente camada controles filho em um contêiner `Control` instância.

## <a name="updatepanel-considerations"></a>Considerações de UpdatePanel

UpdatePanel opera como uma caixa preta, encapsulamento postbacks ASP.NET dentro do contexto de uma JavaScript XMLHttpRequest. No entanto, há considerações de desempenho significativa deve ter em mente, em termos de comportamento e velocidade. Para entender como funciona o UpdatePanel, para que você possa decidir melhor quando é apropriado a seu uso, você deve examinar a troca de AJAX. O exemplo a seguir usa um site existente e, Mozilla Firefox com a extensão de Firebug (Firebug captura dados de XMLHttpRequest).

Considere a possibilidade de um formulário que, entre outras coisas, tem uma caixa de texto de código postal que deve para preencher um campo de cidade e estado em um formulário ou controle. Este formulário, por fim, coleta informações de associação, incluindo nome, endereço e informações de contato de um usuário. Existem muitas considerações de design para levar em conta, com base nos requisitos de um projeto específico.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


Na iteração original deste aplicativo, um controle foi criado que incorporado a totalidade dos dados de registro do usuário, incluindo o código postal, cidade e estado. Todo o controle foi encapsulado dentro de um UpdatePanel e solta em um formulário da Web. Quando o código postal é inserido pelo usuário, o UpdatePanel detecta o evento (o evento TextChanged correspondente no back-end, especificando os gatilhos ou usando a propriedade de ChildrenAsTriggers definida como true). AJAX publica todos os campos do UpdatePanel, capturados pelo FireBug (consulte o diagrama à direita).

Como a captura de tela indica, os valores de cada controle dentro do UpdatePanel são entregues (nesse caso, eles estão vazios), bem como o campo de ViewState. No total, em 9kb de dados é enviado, quando na verdade, somente cinco bytes de dados eram necessários para fazer essa solicitação específica. A resposta é ainda mais inchada: no total, kb 57 é enviada para o cliente simplesmente para atualizar um campo de texto e um campo de lista suspensa.

Ele também pode ser de interesse para ver como o ASP.NET AJAX atualiza a apresentação. A parte da resposta da solicitação de atualização do UpdatePanel é mostrada na exibição do console Firebug à esquerda; é uma cadeia de delimitada desenvolvidas especialmente dividida pelo script de cliente e, em seguida, reorganizada na página. Especificamente, o ASP.NET AJAX define o *innerHTML* propriedade do elemento HTML no cliente que representa o UpdatePanel. Como o navegador gera novamente o DOM, há um ligeiro atraso, dependendo da quantidade de informações que precisam ser processada.

A regeneração de DOM dispara um número de problemas adicionais:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Clique para exibir a imagem em tamanho normal](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Se o elemento HTML focalizado está inserido no UpdatePanel, ele perderá o foco. Portanto, para usuários que pressionada a tecla Tab para sair da caixa de texto de código postal, seu próximo destino seria a caixa de texto de cidade. No entanto, depois que o UpdatePanel atualizada a exibição, o formulário não teria foco e pressionar Tab teria iniciado realçar os elementos de foco (como links).
- Se qualquer tipo de script do lado do cliente personalizado está em uso que acessa os elementos de DOM, referências persistentes por funções pode ser expirado após um postback parcial.

UpdatePanels não pretendem ser soluções pega-tudo. Em vez disso, eles fornecem uma solução rápida para determinadas situações, incluindo a criação de protótipos, atualizações de controle pequeno e fornecem uma interface familiar aos desenvolvedores do ASP.NET que podem estar familiarizado com o modelo de objeto do .NET, mas para menos com o DOM. Há um número de alternativas que podem resultar em melhor desempenho, dependendo do cenário de aplicativo:

- Considere o uso de PageMethods e JSON (JavaScript Object Notation) permite que o desenvolvedor chamar métodos estáticos em uma página, como se invocar uma chamada de serviço web. Como os métodos são estáticos, nenhum estado é necessário; o chamador de script fornece os parâmetros e o resultado é retornado de forma assíncrona.
- Considere o uso de um serviço Web e JSON se precisar de um único controle a ser usado em vários locais em um aplicativo. Novamente, isso requer muito pouco trabalho especial e funciona de forma assíncrona.

Incorporar a funcionalidade por meio de serviços Web ou métodos de página tem desvantagens também. Primeiramente, os desenvolvedores do ASP.NET é normalmente tendem a compilar pequenos componentes da funcionalidade em controles de usuário (arquivos. ascx). Métodos de página não podem ser hospedados nesses arquivos; eles devem ser hospedados dentro da classe de página. aspx real. Da mesma forma, os serviços da Web, devem ser hospedados dentro da classe. asmx. Dependendo do aplicativo, essa arquitetura pode violar o princípio da responsabilidade única, em que a funcionalidade de um único componente agora é disseminada entre dois ou mais componentes físicos que podem ter ties coesos pouca ou nenhuma.

Finalmente, se um aplicativo requer que UpdatePanels são usadas, as diretrizes a seguir devem ajudar com manutenção e solução de problemas.

- **Aninhar UpdatePanels mínimo possível, não apenas em unidades, mas também em unidades de código.** Por exemplo, um UpdatePanel em uma página que envolve um controle, enquanto que o controle também contém um UpdatePanel, que contém outro controle que contém um UpdatePanel, é ter entre unidade aninhamento. Isso ajuda a limpar os elementos devem ser atualizados e impede que inesperado atualizações para filho UpdatePanels.
- **Manter o *ChildrenAsTriggers* propriedade definida como false e definir explicitamente o disparo de eventos.** Utilizando o `<Triggers>` coleção é uma maneira muito mais clara para manipular eventos e pode impedir que um comportamento inesperado, ajudando com tarefas de manutenção e forçar um desenvolvedor para participar de um evento.
- **Use a menor unidade possíveis para obter funcionalidade.** Conforme observado na discussão sobre o serviço de código postal, encapsulamento que somente o mínimo reduz o tempo para o servidor de processamento total e o espaço da troca de cliente-servidor, melhorando o desempenho.

## <a name="the-updateprogress-control"></a>O controle UpdateProgress

## <a name="updateprogress-control-reference"></a>Referência de controle UpdateProgress

Propriedades da marcação habilitada:

| **Nome da propriedade** | **Tipo** | **Descrição** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Cadeia de caracteres | Especifica a ID do UpdatePanel que este UpdateProgress deve relatar. |
| DisplayAfter | Int | Especifica o tempo limite em milissegundos antes que esse controle é exibido depois que a solicitação assíncrona começa. |
| DynamicLayout | bool | Especifica se o progresso é processado dinamicamente. |

Marcação descendentes:

| **Tag** | **Descrição** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contém o modelo de controle definido para o conteúdo que será exibido com este controle. |

O controle UpdateProgress fornece uma medida de comentários para manter o interesse dos usuários ao fazer o trabalho necessário para o transporte para o servidor. Isso pode ajudar os usuários a saber o que você está fazendo algo mesmo que ele pode não ser aparente, especialmente porque a maioria dos usuários são usados para a página de atualização e ver a barra de status de realce.

Como uma observação, controles UpdateProgress podem aparecer em qualquer lugar em uma hierarquia de página. No entanto, em casos em que um postback parcial é iniciado a partir de um filho do UpdatePanel (onde um UpdatePanel é aninhado dentro de outro UpdatePanel), postbacks que disparam o filho UpdatePanel causará UpdateProgress modelos a serem exibidos para o filho UpdatePanel, bem como o pai UpdatePanel. Mas, se o gatilho é um filho direto do pai UpdatePanel, apenas os modelos de UpdateProgress associados com o pai serão exibidos.

## <a name="summary"></a>Resumo

Os Microsoft ASP.NET AJAX extensions são produtos sofisticados, projetados para ajudar na criação de conteúdo da web mais acessível e fornecer uma experiência de usuário mais rica aos aplicativos web. Como parte do ASP.NET AJAX Extensions, os controles de renderização parcial da página, incluindo os controles ScriptManager, UpdatePanel e UpdateProgress é alguns dos componentes mais visíveis do Kit de ferramentas.

O componente ScriptManager integra a provisão de cliente JavaScript para as extensões, bem como permite que os vários componentes de servidor e cliente trabalhar junto com o investimento de desenvolvimento mínimo.

O controle UpdatePanel é caixa aparente magic - marcação no UpdatePanel pode ter code-behind do lado do servidor e não disparar uma atualização de página. Controles do UpdatePanel podem ser aninhados e podem ser dependentes de controles em outros UpdatePanels. Por padrão, UpdatePanels lidar com quaisquer postagens invocadas por seus controles descendentes, embora essa funcionalidade pode ser bem ajustada, declarativamente ou programaticamente.

Ao usar o controle UpdatePanel, os desenvolvedores devem conhecer o impacto de desempenho que pode surgir potencialmente. Possíveis alternativas incluem serviços web e os métodos de página, embora o design do aplicativo deve ser considerado.

O UpdateProgress controle permite que o usuário sabe que ele não está sendo ignorado, e que a solicitação dos bastidores está acontecendo enquanto a página não fazer nada para responder a entrada do usuário. Ele também inclui a capacidade de anular os resultados de processamento parcial.

Juntas, essas ferramentas ajudam criando uma experiência de usuário avançada e consistente, fazendo o trabalho de servidor menos aparente para o usuário e interromper o fluxo de trabalho menor.

## <a name="bio"></a>Bio

Scott Cate trabalha com tecnologias Microsoft Web desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especializada em escrever ASP.NET com base em aplicativos voltados para soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado via email em [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Avançar](understanding-asp-net-ajax-updatepanel-triggers.md)
