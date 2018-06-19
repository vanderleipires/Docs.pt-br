---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Controles de servidor | Microsoft Docs
author: microsoft
description: O ASP.NET 2.0 aprimora a controles de servidor de várias maneiras. Neste módulo, abordaremos algumas das alterações de arquitetura para a forma como o ASP.NET 2.0 e 200 do Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 72e9cac7cf9a01791c30783fa56ad7ea205a5a11
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885187"
---
<a name="server-controls"></a>Controles de servidor
====================
por [Microsoft](https://github.com/microsoft)

> O ASP.NET 2.0 aprimora a controles de servidor de várias maneiras. Neste módulo, vamos abordar algumas das alterações na arquitetura, a maneira como o ASP.NET 2.0 e o Visual Studio 2005 lida com controles de servidor.


O ASP.NET 2.0 aprimora a controles de servidor de várias maneiras. Neste módulo, vamos abordar algumas das alterações na arquitetura, a maneira como o ASP.NET 2.0 e o Visual Studio 2005 lida com controles de servidor.

## <a name="view-state"></a>Estado de exibição

A principal alteração no estado de exibição no ASP.NET 2.0 é uma drástica redução no tamanho. Considere uma página com apenas um controle de calendário nele. Aqui está o estado de exibição no ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Agora aqui é o estado de exibição em uma página idêntica no ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Uma alteração bastante significativa, e considerar que o estado de exibição é executado e para trás eletronicamente, essa alteração pode dar aos desenvolvedores um aumento significativo de desempenho. A redução no tamanho do estado de exibição é em grande parte, devido à maneira que podemos manipular internamente. Lembre-se de que a cadeia de caracteres codificada exibição de estado é uma Base64. Para entender melhor a alteração no estado de exibição no ASP.NET 2.0, ter vamos examinar os valores decodificados nos exemplos acima.

Aqui está o estado de 1.1 exibição decodificado:

[!code-css[Main](server-controls/samples/sample3.css)]

Isso pode parecer um pouco sem sentido, mas não há um padrão aqui. No ASP.NET 1. x, é usado caracteres únicos para identificar tipos de dados e delimitado por valores usando o &lt; &gt; caracteres. O "t" no exemplo de estado de exibição acima representa um trio. O Trio contém um par de ArrayLists ("l" representa um ArrayList.) Um desses ArrayLists contém um Int32 ("i") com um valor de 1 e o outro contém Trio de outro. O Trio contém um par de ArrayLists, etc. Importante lembrar é que podemos usar conjuntos que contêm pares, é identificar os tipos de dados por meio de uma letra e usamos o &lt; e &gt; caracteres como delimitadores.

No ASP.NET 2.0, o estado de exibição decodificada parece um pouco diferente.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Você deve observar uma enorme alteração na exibição do estado de exibição decodificada. Essa alteração tem várias bases arquitetura. Estado de exibição no ASP.NET 1. x usado o LosFormatter para serializar dados. 2.0, usamos a nova classe de ObjectStateFormatter. Essa classe foi especificamente projetada para ajudar a serialização e desserialização de estado de exibição e o estado do controle. (Estado de controle será abordado na próxima seção.) Há muitos benefícios obtidos ao alterar o método pelo qual a serialização e desserialização ocorrem. Uma das mais significativas é o fato de que, ao contrário de LosFormatter que usa um TextWriter, o ObjectStateFormatter usa um BinaryWriter. Isso permite que o ASP.NET 2.0 armazenar o estado de exibição de uma série de bytes em vez de cadeias de caracteres. Considere, por exemplo, um número inteiro. No ASP.NET 1.1, um número inteiro necessário 4 bytes de estado de exibição. No ASP.NET 2.0, esse mesmo inteiro exige apenas 1 byte. Outros aprimoramentos foram feitos para diminuir a quantidade de estado de exibição é armazenada. Valores de data e hora, por exemplo, agora são armazenados usando um TickCount em vez de uma cadeia de caracteres.

Como se todos os que não foram suficientes, atenção especial foi dada ao fato de que um dos maiores consumidores de estado de exibição em 1. x foi DataGrid e controles semelhantes. Uma grande desvantagem de controles, como a grade de dados onde está preocupada com o estado de exibição é que ele geralmente contém grandes quantidades de informações repetidas. No ASP.NET 1. x, que é repetido informações simplesmente foi armazenadas em e em novamente, resultando em um estado de exibição inflados. No ASP.NET 2.0, usamos a nova classe IndexedString para armazenar esses dados. Se uma cadeia de caracteres se repetir, apenas armazenamos o token para o IndexedString e o índice dentro de uma tabela em execução de objetos IndexedString.

## <a name="control-state"></a>Estado de controle

Os principais gripes que os desenvolvedores precisavam com estado de exibição foram o tamanho que ele adicionou à carga HTTP. Como mencionado anteriormente, um dos maiores consumidores de estado de exibição é o controle DataGrid. Para evitar a enorme quantidade de estado de exibição gerado por uma grade de dados, muitos desenvolvedores simplesmente desabilitado o estado de exibição para o controle. Infelizmente, essa solução não era sempre uma boa. Estado de exibição no ASP.NET 1. x contém não apenas os dados necessários para a funcionalidade correta do controle. Ele também contém informações sobre o estado da interface de usuário do controle. Isso significa que se você deseja permitir para paginação em uma grade de dados, você deve habilitar o estado de exibição, mesmo se não precisar de todas as informações de interface do usuário que exibir estado contém. É um cenário de tudo ou nada.

No ASP.NET 2.0, o estado de controle resolve esse problema bem via a introdução do estado de controle. Estado de controle contém os dados que é absolutamente necessários para a funcionalidade adequada de um controle. Ao contrário do estado de exibição, o estado de controle não pode ser desabilitado. Portanto, é importante que os dados sejam armazenados no estado do controle é cuidadosamente controlados.

> [!NOTE]
> Estado do controle é mantido junto com o estado de exibição no \_ \_campo de formulário oculto VIEWSTATE.


Este vídeo é um passo a passo do estado de exibição e do estado de controle.


![](server-controls/_static/image1.png)


[Abra vídeo de tela inteira](server-controls/_static/state1.wmv)


Em ordem para um controle de servidor ler e gravar para controlar o estado, você deve tomar as três etapas.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Etapa 1: Chamar o método RegisterRequiresControlState

O método RegisterRequiresControlState informa ao ASP.NET que um controle precisa persistir o estado de controle. Ele usa um argumento de tipo de controle que é o controle que está sendo registrado.

É importante observar que o registro não persiste de solicitação para solicitar. Portanto, esse método deve ser chamado em cada solicitação, se for um controle persistir o estado de controle. É recomendável que o método ser chamado no OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Etapa 2: Substituir SaveControlState

O método SaveControlState salva controle de alterações de estado para um controle desde o último postback. Retorna um objeto que representa o estado do controle.

## <a name="step-3-override-loadcontrolstate"></a>Etapa 3: Substituir LoadControlState

O método LoadControlState carrega o estado salvo em um controle. O método aceita um argumento de tipo de objeto que mantém o estado salvo para o controle.

## <a name="full-xhtml-compliance"></a>Conformidade XHTML completo

Qualquer desenvolvedor da Web sabe a importância de padrões em aplicativos da Web. Para manter um ambiente de desenvolvimento baseado em padrões, o ASP.NET 2.0 é totalmente compatível com XHTML. Portanto, todas as marcas são renderizadas de acordo com os padrões XHTML em navegadores que oferecem suporte a HTML 4.0 ou posterior.

A definição de tipo de documento no ASP.NET 1.1 foi da seguinte maneira:

[!code-html[Main](server-controls/samples/sample6.html)]

No ASP.NET 2.0, a definição de tipo de documento padrão é o seguinte:

[!code-html[Main](server-controls/samples/sample7.html)]

Se você escolher, você pode alterar a conformidade de XHML padrão por meio do nó xhtmlConformance no arquivo de configuração. Por exemplo, o nó no arquivo Web. config será alterado conformidade XHTML para XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Se você escolher, você também pode configurar o ASP.NET para usar a configuração herdada no ASP.NET 1. x da seguinte maneira:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptável usando adaptadores de renderização

No ASP.NET 1. x, o arquivo de configuração que continha uma &lt;browserCaps&gt; seção preenchido um objeto HttpBrowserCapabilities. Esse objeto permitido um desenvolvedor determinar qual dispositivo está fazendo uma solicitação específica e renderizar o código adequadamente. No ASP.NET 2.0, o modelo foi aprimorada e agora usa a nova classe ControlAdapter. A classe ControlAdapter substitui os eventos no ciclo de vida do controle e controla a renderização de controles com base em recursos do agente do usuário. Os recursos de um agente de usuário específico são definidos por um arquivo de definição de navegador (um arquivo com uma extensão de arquivo. browser) armazenado na c:\windows\microsoft.net\framework\v2.0. \* \* \* \*\CONFIG\Browsers pasta.

> [!NOTE]
> A classe ControlAdapter é uma classe abstrata.


Assim como o &lt;browserCaps&gt; seção 1. x, o arquivo de definição de navegador usa uma expressão Regular para analisar a cadeia de caracteres de agente do usuário para identificar o navegador solicitante. --Los define recursos específicos do agente de usuário. O ControlAdapter renderiza o controle por meio do método de renderização. Portanto, se você substituir o método de renderização, você não deve chamar renderização na classe base. Isso pode causar a renderização ocorra duas vezes, uma vez para o adaptador e outra para o controle em si.

## <a name="developing-a-custom-adapter"></a>Desenvolvendo um adaptador personalizado

Você pode desenvolver seu próprio adaptador personalizado herdando de ControlAdapter. Além disso, é possível herdar a classe abstrata PageAdapter em casos em que um adaptador é necessário para uma página. Mapeamento de controles para o adaptador personalizado é realizado por meio de &lt;controlAdapters&gt; elemento no arquivo de definição do navegador. Por exemplo, o seguinte XML de um arquivo de definição de navegador mapeia o controle de Menu para a classe MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Usando esse modelo, ele se torna muito fácil para um desenvolvedor de controle para um determinado dispositivo ou navegador de destino. Também é muito simple para um desenvolvedor tenha controle completo sobre como páginas renderizadas em cada dispositivo.

## <a name="per-device-rendering"></a>Renderização por dispositivo

Propriedades de controle de servidor no ASP.NET 2.0 podem ser especificado por dispositivo usando um prefixo de navegador específico. Por exemplo, o código a seguir alterará o texto de um rótulo, dependendo de qual dispositivo está sendo usado para procurar a página.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Quando a página que contém este rótulo é procurada do Internet Explorer, o rótulo será exibido o texto dizendo "Você está navegando do Internet Explorer." Quando a página é acessada de Firefox, o rótulo será exibido o texto "Você está navegando do Firefox." Quando a página é acessada de qualquer outro dispositivo, ele exibirá "Você está navegando em um dispositivo desconhecido". Qualquer propriedade pode ser especificada usando a seguinte sintaxe especial.

## <a name="setting-focus"></a>Definir o foco

Os desenvolvedores do ASP.NET 1. x frequentes sobre como definir o foco inicial em um controle específico. Por exemplo, em uma página de logon, é útil ter a caixa de texto do ID de usuário a obter o foco quando a página for carregada pela primeira vez. No ASP.NET 1. x, isso necessário escrever alguns scripts do lado do cliente. Embora esse script é uma tarefa simples, não é necessário no ASP.NET 2.0 graças o método SetFocus. O método SetFocus usa um argumento que indica o controle que deve receber o foco. Esse argumento pode ser a ID do cliente do controle como uma cadeia de caracteres ou o nome do controle do servidor como um objeto de controle. Por exemplo, para definir o foco inicial para um controle de caixa de texto chamado txtUserID quando a página for carregada pela primeira vez, adicione o seguinte código à página\_carga:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

– ou

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 usa o manipulador WebResource (discutido anteriormente) para renderizar uma função de cliente que define o foco. O nome da função do lado do cliente é WebForm\_AutoFocus conforme mostrado aqui:

[!code-html[Main](server-controls/samples/sample14.html)]

Como alternativa, você pode usar o método de foco para um controle para definir o foco inicial para esse controle. O método de foco deriva da classe de controle e está disponível para todos os controles do ASP.NET 2.0. Também é possível definir o foco para um determinado controle quando ocorre um erro de validação. Isso será abordado em um módulo posterior.

## <a name="new-server-controls-in-aspnet-20"></a>Novos controles de servidor no ASP.NET 2.0

Estes são os novos controles de servidor no ASP.NET 2.0. Podemos entrará em mais detalhes em alguns deles em módulos posteriores.

## <a name="imagemap-control"></a>Controle ImageMap

O controle ImageMap permite que você adicione pontos de acesso a uma imagem que pode iniciar um postback ou navegar até uma URL. Há três tipos de pontos de acesso disponíveis. CircleHotSpot, RectangleHotSpot e PolygonHotSpot. Pontos de acesso são adicionados por meio de um editor de coleção no Visual Studio ou programaticamente no código. Não há nenhuma interface de usuário disponível para desenhar pontos de acesso em uma imagem. As coordenadas e o tamanho ou o raio do ponto de acesso deve ser especificado declarativamente. Também não há nenhuma representação visual de um ponto de acesso no designer. Se um ponto de acesso está configurado para navegar até uma URL, a URL é especificada por meio da propriedade NavigateUrl do ponto de acesso. No caso de uma postagem de fazer o ponto de acesso, PostBackValue propriedade permite que você passar uma cadeia de caracteres na postagem volta que pode ser recuperada no código do lado do servidor.


![Editor de coleção de ponto de acesso no Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: Editor de coleção de ponto de acesso no Visual Studio


## <a name="bulletedlist-control"></a>Controle BulletedList

O controle BulletedList é uma lista com marcadores que pode ser facilmente os dados associados. A lista pode ser ordenada (numerada) ou não ordenado por meio da propriedade BulletStyle. Cada item na lista é representado por um objeto de item de lista.


![Controle de BulletedList no Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: controle de BulletedList no Visual Studio


## <a name="hiddenfield-control"></a>Controle HiddenField

O controle HiddenField adiciona um campo de formulário oculto para sua página, o valor que está disponível no código do lado do servidor. O valor de um campo de formulário oculto geralmente deve permanecer inalterada entre post faz. No entanto, é possível que um usuário mal-intencionado alterar o antes de valor para lançar novamente. Se isso acontecer, o controle HiddenField irá gerar o evento ValueChanged. Se você tiver informações confidenciais no controle HiddenField e você deseja garantir que ele permaneça inalterado, você deve tratar o evento ValueChanged em seu código.

## <a name="fileupload-control"></a>Controle de carregamento de arquivos

O controle de carregamento de arquivos no ASP.NET 2.0 possibilita carregar arquivos em um servidor Web por meio de uma página ASP.NET. Esse controle é bastante semelhante à classe ASP.NET 1. x HtmlInputFile com algumas exceções. No ASP.NET 1. x, era recomendado que a propriedade PostedFile ser verificada para null para determinar se você tivesse um arquivo válido. O controle de carregamento de arquivos no ASP.NET 2.0 adiciona uma nova propriedade HasFile que você pode usar para o mesmo propósito e é um pouco mais eficiente.

A propriedade PostedFile ainda está disponível para acesso a um objeto HttpPostedFile, mas algumas das funcionalidades de HttpPostedFile está agora disponível intrinsecamente com o controle de carregamento de arquivos. Por exemplo, para salvar um arquivo carregado no ASP.NET 1. x, você chamar o método SaveAs no objeto HttpPostedFile. Usando o controle de carregamento de arquivos no ASP.NET 2.0, você poderia chamar o método SaveAs do controle de carregamento de arquivos em si.

Outra alteração significativa no comportamento 2.0 (e, provavelmente, a alteração mais significativa) é que não é necessário carregar um arquivo carregado inteiro na memória antes de salvar. 1. x, qualquer arquivo que foi carregado é salvo inteiramente na memória antes de serem gravados no disco. Essa arquitetura evita o carregamento de arquivos grandes.

No ASP.NET 2.0, o atributo requestLengthDiskThreshold de um elemento httpRuntime permite que você configure quantos quilobytes são mantidos em um buffer na memória antes de serem gravados no disco.

**IMPORTANTE**: documentação MSDN (e documentação em outro lugar) Especifica que esse valor é em bytes (não quilobytes) e o padrão é 256. O valor é especificado, na verdade, em quilobytes, e o valor padrão é 80. Por ter um valor padrão de 80 mil, garantimos que o buffer não termine em heap de objeto grande.

## <a name="wizard-control"></a>Assistente de controle

É bastante comum encontrar dificuldades com tentando coletar informações em uma série de "páginas" usando painéis ou pela transferência de uma página de desenvolvedores do ASP.NET. Mais frequentemente que não, o desafio é um frustrante e demorado. O novo controle Wizard resolve os problemas, permitindo não linear e etapas em uma interface de assistente que usuários estão familiarizados com. O controle Wizard apresenta formulários de entrada em uma série de etapas. Cada etapa é de um determinado tipo especificado pela propriedade StepType do controle. Os tipos de etapa disponíveis são os seguintes:

| **Tipo de etapa** | **Explanation** |
| --- | --- |
| Auto | O assistente determina automaticamente o tipo de etapa com base em sua posição dentro da hierarquia de etapa. |
| Início | A primeira etapa, geralmente usada para apresentar uma instrução de Introdução. |
| Etapa | Uma etapa normal. |
| Concluir | A etapa final, geralmente usada para apresentar um botão para concluir o assistente. |
| Concluir | Apresenta uma mensagem de êxito ou falha de comunicação. |

> [!NOTE]
> O controle Wizard mantém o controle de seu estado usando o estado de controle do ASP.NET. Portanto, a propriedade pode ser definida como false, sem qualquer dano.


Este vídeo é um passo a passo do controle Wizard.


![](server-controls/_static/image2.png)


[Abra vídeo de tela inteira](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Localizar o controle

O controle Localize é semelhante a um controle Literal. No entanto, o controle Localize tem um **modo** propriedade que controla como a marcação que é adicionada a ele é renderizada. A propriedade Mode suporta os seguintes valores:

| **Modo** | **Explanation** |
| --- | --- |
| Transformar | Marcação é transformada de acordo com o protocolo do navegador que está fazendo a solicitação. |
| PassThrough | Marcação é renderizada como-é. |
| Codificar | Marcação que é adicionada ao controle é codificada usando HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView e controles de exibição

O controle MultiView atua como um contêiner para controles de exibição e o controle de exibição atua como um contêiner (semelhante a um painel de controle) para outros controles. Cada modo de exibição em um controle MultiView é representado por um controle de exibição único. O primeiro controle de exibição de MultiView é exibição 0, o segundo é o modo de exibição 1, etc. Você pode alternar os modos de exibição especificando ActiveViewIndex do controle MultiView.

## <a name="substitution-control"></a>Controle de substituição

O controle Substitution é usado em conjunto com o cache do ASP.NET. Em casos onde você deseja tirar proveito do armazenamento em cache, mas tem partes de uma página que deve ser atualizado em cada solicitação (em outras palavras, partes de uma página que estão isentos do cache), o componente de substituição fornece uma ótima solução. O controle, na verdade, não gera nenhuma saída por conta própria. Em vez disso, ele é associado a um método no código do lado do servidor. Quando a página é solicitada, o método é chamado e a marcação retornada é renderizada em vez do controle substitution.

O método ao qual o controle Substitution está associado é especificado por meio de **MethodName** propriedade. Esse método deve atender aos seguintes critérios:

- Ele deve ser um método estático (compartilhado no VB).
- Ele aceita um parâmetro de tipo HttpContext.
- Retorna uma cadeia de caracteres que representa a marcação que deve substituir o controle na página.

O controle de substituição não tem a capacidade de modificar qualquer outro controle na página, mas ele tem acesso para o HttpContext atual por meio de seu parâmetro.

## <a name="gridview-control"></a>Controle GridView

O controle GridView é o substituto do controle DataGrid. Esse controle será abordado mais detalhadamente em um módulo posterior.

## <a name="detailsview-control"></a>Controle DetailsView

O controle DetailsView permite exibir um único registro de uma fonte de dados e editar ou excluí-lo. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="formview-control"></a>Controle FormView

O controle FormView é usado para exibir um registro individual de uma fonte de dados em uma interface configurável. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="accessdatasource-control"></a>Controle AccessDataSource

O controle AccessDataSource é usado para associação de dados de um banco de dados. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="objectdatasource-control"></a>Controle ObjectDataSource

O controle ObjectDataSource é usado para dar suporte a uma arquitetura de três camadas para que os controles podem ser associado a dados a um objeto comercial de camada intermediária em vez de um modelo de duas camadas em que os controles são vinculados diretamente à fonte de dados. Ele será abordado mais detalhadamente em um módulo posterior.

## <a name="xmldatasource-control"></a>Controle XmlDataSource

O controle XmlDataSource é usado para associar dados a uma fonte de dados XML. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="sitemapdatasource-control"></a>Controle SiteMapDataSource

O controle SiteMapDataSource fornece vinculação de dados para controles de navegação do site com base em um mapa de site. Ele será abordado mais detalhadamente em um módulo posterior.

## <a name="sitemappath-control"></a>Controle SiteMapPath

O controle SiteMapPath exibe uma série de links de navegação comumente chamado de trilha. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="menu-control"></a>Controle de Menu

O controle de Menu exibe menus dinâmicos usando DHTML. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="treeview-control"></a>Controle TreeView

O controle TreeView é usado para exibir um modo de exibição de árvore hierárquica de dados. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="login-control"></a>Controle de logon

Fornece o controle de logon para um mecanismo fazer logon em um site da Web. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginview-control"></a>Controle LoginView

O controle LoginView permite a exibição de modelos diferentes com base no status de logon do usuário. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="passwordrecovery-control"></a>Controle PasswordRecovery

O controle PasswordRecovery é usado para recuperar senhas esquecidas por usuários de um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginstatus"></a>LoginStatus

O controle de status de logon exibe o status de logon do usuário. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginname"></a>LoginName

O controle LoginName exibe o nome do usuário após é registrada em um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard é um assistente configurável que fornece aos usuários a capacidade de criar uma conta de associação ASP.NET para uso em um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="changepassword"></a>ChangePassword

O controle ChangePassword permite que os usuários alterem suas senhas para um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="various-webparts"></a>Vários WebParts

O ASP.NET 2.0 é fornecido com diversas partes da Web. Eles serão abordados detalhadamente em um módulo posterior.
