---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Controles de servidor | Microsoft Docs
author: microsoft
description: O ASP.NET 2.0 aprimora a controles de servidor de várias maneiras. Este módulo, abordaremos algumas das alterações de arquitetura para a maneira como o ASP.NET 2.0 e 200 do Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 989986c45e76a65582f35172c7d837a78484d09a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374218"
---
<a name="server-controls"></a>Controles de servidor
====================
por [Microsoft](https://github.com/microsoft)

> O ASP.NET 2.0 aprimora a controles de servidor de várias maneiras. Neste módulo, vamos abordar algumas das alterações na arquitetura na maneira como o ASP.NET 2.0 e o Visual Studio 2005 lida com controles de servidor.


O ASP.NET 2.0 aprimora a controles de servidor de várias maneiras. Neste módulo, vamos abordar algumas das alterações na arquitetura na maneira como o ASP.NET 2.0 e o Visual Studio 2005 lida com controles de servidor.

## <a name="view-state"></a>Estado de exibição

A principal alteração no estado de exibição no ASP.NET 2.0 é uma redução expressiva de tamanho. Considere uma página com apenas um controle de calendário nele. Aqui está o estado de exibição no ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Aqui está o estado de exibição em uma página idêntica no ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Essa é uma alteração bastante significativa e considerando que o estado de exibição é carregado e volta durante a transmissão, essa alteração pode dar aos desenvolvedores um aumento de desempenho significativa. A redução no tamanho do estado de exibição é amplamente devido à maneira que podemos lidar com ele internamente. Lembre-se de que a cadeia de caracteres codificada exibição de estado é uma Base64. Para entender melhor a alteração no estado de exibição no ASP.NET 2.0, vamos dar uma olhada no que os valores decodificados dos exemplos acima.

Aqui está o estado de 1.1 exibição decodificado:

[!code-css[Main](server-controls/samples/sample3.css)]

Isso pode parecer um pouco sem sentido, mas há um padrão aqui. No ASP.NET 1. x, podemos usados caracteres únicos para identificar tipos de dados e delimitado por valores usando o &lt; &gt; caracteres. No exemplo de estado de exibição acima "t" representa um triplo. O Tripleto contém um par de ArrayLists (o "l" representa uma ArrayList.) Uma dessas ArrayLists contém um Int32 ("i") com um valor de 1 e o outro contém outro triplo. O Tripleto contém um par de ArrayLists, etc. O importante a lembrar é que usamos Triplets que contêm pares, podemos identificar os tipos de dados por meio de uma letra e usamos a &lt; e &gt; caracteres como delimitadores.

No ASP.NET 2.0, o estado de exibição decodificada parece um pouco diferente.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Você deve observar uma enorme alteração na aparência do estado de exibição decodificada. Essa alteração tem vários alicerces de arquitetura. Estado de exibição no ASP.NET 1. x usado o LosFormatter para serializar os dados. No 2.0, podemos usar a nova classe ObjectStateFormatter. Essa classe foi projetada especificamente para ajudar a serialização e desserialização de estado de exibição e estado do controle. (Estado do controle será abordado na próxima seção.) Há muitos benefícios obtidos alterando o método pelo qual serialização e desserialização ocorrem. Um dos mais dramática é o fato de que, ao contrário de LosFormatter que usa um TextWriter, o ObjectStateFormatter usa um BinaryWriter. Isso permite que o ASP.NET 2.0 armazenar o estado de exibição de uma série de bytes em vez de cadeias de caracteres. Veja, por exemplo, um número inteiro. No ASP.NET 1.1, um número inteiro necessário 4 bytes do estado de exibição. No ASP.NET 2.0, esse mesmo inteiro requer apenas 1 byte. Outros aprimoramentos foram feitos para diminuir a quantidade de estado de exibição que é armazenado. Valores de data e hora, por exemplo, agora são armazenadas usando um TickCount em vez de uma cadeia de caracteres.

Como se tudo isso não bastasse, atenção especial foi dada ao fato de que um dos maiores consumidores de estado de exibição em 1. x era a DataGrid e os controles semelhantes. Uma grande desvantagem dos controles, como a grade de dados em que o estado de exibição está preocupado é que ele contém muitas vezes grandes quantidades de informações repetidas. No ASP.NET 1. x, a qual repetir informações simplesmente foi armazenadas em e em resultando novamente em um estado de exibição inflados. No ASP.NET 2.0, podemos usar a nova classe de IndexedString para armazenar tais dados. Se uma cadeia de caracteres se repetir, estamos apenas armazenar o token para o IndexedString e o índice dentro de uma tabela em execução de objetos IndexedString.

## <a name="control-state"></a>Estado de controle

Uma das reclamações a principais que os desenvolvedores tinham com estado de exibição foi o tamanho que ele adicionou à carga HTTP. Como mencionado anteriormente, um dos maiores consumidores de estado de exibição é o controle DataGrid. Para evitar as enormes quantidades de estado de exibição gerados por um DataGrid, muitos desenvolvedores simplesmente desabilitado o estado de exibição para o controle. Infelizmente, essa solução não era sempre uma boa. Estado de exibição no ASP.NET 1. x contém não apenas os dados necessários para a funcionalidade correta do controle. Ele também contém informações sobre o estado da interface de usuário do controle. Isso significa que se você deseja permitir para paginação em um DataGrid, você deve habilitar o estado de exibição, mesmo se não precisar de todas as informações de interface do usuário exibir estado contém. É um cenário de tudo ou nada.

No ASP.NET 2.0, o estado do controle resolve o problema perfeitamente via a introdução do estado do controle. Estado de controle contém os dados que seja absolutamente necessários para a funcionalidade adequada de um controle. Ao contrário do estado de exibição, o estado de controle não pode ser desabilitado. Portanto, é importante que os dados sejam armazenados no estado do controle é cuidadosamente controlados.

> [!NOTE]
> Estado do controle é mantido junto com o estado de exibição na \_ \_campo de formulário oculto VIEWSTATE.


Este vídeo é um passo a passo do estado de exibição e estado do controle.


![](server-controls/_static/image1.png)


[Abra vídeo de tela inteira](server-controls/_static/state1.wmv)


Em ordem para um controle de servidor para leitura e gravação ao estado do controle, você deve levar três etapas.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Etapa 1: Chamar o método RegisterRequiresControlState

O método RegisterRequiresControlState informa ao ASP.NET que um controle precisa para manter o estado do controle. Ele usa um argumento de tipo de controle que é o controle que está sendo registrado.

É importante observar que o registro não persiste entre as solicitações. Portanto, esse método deve ser chamado em cada solicitação se um controle é manter o estado do controle. É recomendável que o método ser chamado OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Etapa 2: Substituir SaveControlState

O método SaveControlState salva as alterações de estado para um controle de controle desde o último postback. Ele retorna um objeto que representa o estado do controle.

## <a name="step-3-override-loadcontrolstate"></a>Etapa 3: Substituir LoadControlState

O método LoadControlState carrega o estado salvo em um controle. O método usa um argumento de tipo de objeto que mantém o estado salvo do controle.

## <a name="full-xhtml-compliance"></a>Conformidade XHTML completo

Qualquer desenvolvedor Web sabe a importância dos padrões em aplicativos da Web. Para manter um ambiente de desenvolvimento baseado em padrões, o ASP.NET 2.0 é totalmente compatível com XHTML. Portanto, todas as marcas são renderizada de acordo com os padrões XHTML em navegadores que oferecem suporte a HTML 4.0 ou superior.

A definição de tipo de documento no ASP.NET 1.1 era da seguinte maneira:

[!code-html[Main](server-controls/samples/sample6.html)]

No ASP.NET 2.0, a definição de tipo de documento padrão é o seguinte:

[!code-html[Main](server-controls/samples/sample7.html)]

Se você escolher, você pode alterar a conformidade de XHML padrão através do nó xhtmlConformance no arquivo de configuração. Por exemplo, o nó seguinte no arquivo Web. config será alterado com XHTML para XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Se você escolher, você também pode configurar o ASP.NET para usar a configuração herdada usada no ASP.NET 1. x da seguinte maneira:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptável usando adaptadores de renderização

No ASP.NET 1. x, o arquivo de configuração que continha uma &lt;browserCaps&gt; seção que preenchido de um objeto HttpBrowserCapabilities. Esse objeto de permissão um desenvolvedor determinar qual dispositivo está fazendo uma solicitação específica e processados corretamente o código. No ASP.NET 2.0, o modelo foi melhorada e agora usa a nova classe ControlAdapter. A classe ControlAdapter substitui os eventos no ciclo de vida do controle e controla a renderização de controles com base em recursos do agente do usuário. Os recursos de um agente de usuário específico são definidos por um arquivo de definição do navegador (um arquivo com uma extensão de arquivo. browser) armazenado em do c:\windows\microsoft.net\framework\v2.0. \* \* \* \*\CONFIG\Browsers pasta.

> [!NOTE]
> O ControlAdapter de classe é uma classe abstrata.


Assim como o &lt;browserCaps&gt; seção no 1.x, o arquivo de definição do navegador usa uma expressão Regular para analisar a cadeia de caracteres de agente do usuário para identificar o navegador solicitante. --Los define recursos específicos para que o agente de usuário. O ControlAdapter renderiza o controle por meio do método Render. Portanto, se você substituir o método Render, você não deve chamar Render na classe base. Isso pode causar a renderização ocorra duas vezes, uma vez para o adaptador e uma vez para o controle em si.

## <a name="developing-a-custom-adapter"></a>Desenvolver um adaptador personalizado

Você pode desenvolver seu próprio adaptador personalizado herdando de ControlAdapter. Além disso, é possível herdar a classe abstrata PageAdapter em casos em que um adaptador é necessário para uma página. Mapeamento dos controles ao seu adaptador personalizado é realizado por meio de &lt;controlAdapters&gt; elemento no arquivo de definição do navegador. Por exemplo, o seguinte XML de um arquivo de definição do navegador mapeia o controle de Menu para a classe MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Usando esse modelo, ele se torna muito fácil para um desenvolvedor de controle para um determinado dispositivo ou navegador de destino. Também é bastante simple para o desenvolvedor tem controle total sobre como as páginas são renderizadas em todos os dispositivos.

## <a name="per-device-rendering"></a>Renderização de acordo com o dispositivo

Propriedades de controle de servidor no ASP.NET 2.0 podem ser especificado por dispositivo usando um prefixo específico de navegador. Por exemplo, o código a seguir irá alterar o texto de um rótulo, dependendo de qual dispositivo está sendo usado para procurar a página.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Quando a página que contenham este rótulo é acessada no Internet Explorer, o rótulo exibirá o texto dizendo "Você está navegando do Internet Explorer." Quando a página é acessada no Firefox, o rótulo exibirá o texto "Você está navegando no Firefox." Quando a página é acessada de qualquer outro dispositivo, ele exibirá "Você está navegando em um dispositivo desconhecido". Qualquer propriedade pode ser especificada usando essa sintaxe especial.

## <a name="setting-focus"></a>Definir o foco

Os desenvolvedores do ASP.NET 1.x perguntas frequentes sobre como definir o foco inicial em um controle específico. Por exemplo, em uma página de logon, é útil ter a caixa de texto do ID de usuário a obter o foco quando a página for carregada. No ASP.NET 1. x, fazendo isso necessário escrever um script do lado do cliente. Mesmo que esse script é uma tarefa trivial, não é necessário no ASP.NET 2.0, graças ao método SetFocus. O método SetFocus usa um argumento que indica o controle que deve receber o foco. Esse argumento pode ser a ID do cliente do controle como uma cadeia de caracteres ou o nome do controle do servidor como um objeto de controle. Por exemplo, para definir o foco inicial a um controle de caixa de texto chamado txtUserID quando a página for carregada pela primeira vez, adicione o seguinte código à página\_carga:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

– ou

[!code-csharp[Main](server-controls/samples/sample13.cs)]

O ASP.NET 2.0 usa o manipulador WebResource (discutido anteriormente) para renderizar uma função do lado do cliente que define o foco. O nome da função do lado do cliente é WebForm\_AutoFocus, conforme mostrado aqui:

[!code-html[Main](server-controls/samples/sample14.html)]

Como alternativa, você pode usar o método de foco para um controle para definir o foco inicial para esse controle. O método Focus deriva da classe de controle e está disponível para todos os controles do ASP.NET 2.0. Também é possível definir o foco para um determinado controle quando ocorre um erro de validação. Isso será abordado em um módulo posterior.

## <a name="new-server-controls-in-aspnet-20"></a>Novos controles de servidor no ASP.NET 2.0

Estes são os novos controles de servidor no ASP.NET 2.0. Entraremos em mais detalhes sobre algumas em módulos posteriores.

## <a name="imagemap-control"></a>Controle ImageMap

O controle ImageMap permite que você adicione pontos de acesso a uma imagem que pode iniciar um postback ou navegar até uma URL. Há três tipos de pontos de acesso disponíveis; CircleHotSpot, RectangleHotSpot e PolygonHotSpot. Pontos de acesso são adicionados por meio de um editor de coleção no Visual Studio ou programaticamente no código. Não há nenhuma interface do usuário disponível para desenho de pontos de acesso em uma imagem. As coordenadas e o tamanho ou o radius do ponto de acesso deve ser especificado declarativamente. Também não há nenhuma representação visual de um ponto de acesso no designer. Se um ponto de acesso está configurado para navegar até uma URL, a URL é especificada por meio da propriedade NavigateUrl do ponto de acesso. No caso de uma postagem de fazer o ponto de acesso, PostBackValue propriedade permite que você passe uma cadeia de caracteres na postagem de volta que pode ser recuperada no código do lado do servidor.


![Editor de coleção de pontos de acesso no Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: Editor de coleção de pontos de acesso no Visual Studio


## <a name="bulletedlist-control"></a>Controle BulletedList

O controle BulletedList é uma lista com marcadores que possa ser facilmente os dados associados. A lista pode ser ordenada (numerados) ou não ordenados por meio da propriedade BulletStyle. Cada item na lista é representado por um objeto de item de lista.


![Controle BulletedList no Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: controle BulletedList no Visual Studio


## <a name="hiddenfield-control"></a>Controle HiddenField

O controle HiddenField adiciona um campo de formulário oculto à sua página, o valor que está disponível no código do lado do servidor. O valor de um campo de formulário oculto geralmente espera-se permanecem inalterados entre retornos de postagem. No entanto, é possível que um usuário mal-intencionado altere a antes do valor para o postback. Se isso acontecer, o controle HiddenField irá gerar o evento ValueChanged. Se você tiver informações confidenciais no controle HiddenField e você deseja garantir que ele permaneça inalterado, você deve tratar o evento ValueChanged em seu código.

## <a name="fileupload-control"></a>Controle fileUpload

O controle FileUpload no ASP.NET 2.0 torna possível carregar arquivos para um servidor Web por meio de uma página ASP.NET. Esse controle é bastante semelhante à classe ASP.NET 1.x HtmlInputFile com algumas exceções. No ASP.NET 1.x, era recomendável que a propriedade PostedFile ser verificada para null para determinar se você tivesse um arquivo de BOM. O controle FileUpload no ASP.NET 2.0 adiciona uma nova propriedade HasFile que você pode usar para a mesma finalidade e é um pouco mais eficiente.

A propriedade PostedFile ainda está disponível para acesso a um objeto HttpPostedFile, mas algumas das funcionalidades do HttpPostedFile agora está disponível intrinsecamente com o controle FileUpload. Por exemplo, para salvar um arquivo carregado no ASP.NET 1. x, você chama o método SaveAs no objeto HttpPostedFile. Usando o controle FileUpload no ASP.NET 2.0, você chamaria o método SaveAs no controle FileUpload em si.

Outra alteração significativa no comportamento de 2.0 (e, provavelmente, a alteração mais significativa) é que ele não é mais necessário carregar todo o arquivo carregado na memória antes de salvá-lo. No 1.x, qualquer arquivo que foi carregado é salvo inteiramente na memória antes que estão sendo gravados no disco. Essa arquitetura evita o carregamento de arquivos grandes.

No ASP.NET 2.0, o atributo requestLengthDiskThreshold de um elemento httpRuntime permite que você configure quantos quilobytes serão mantidas em um buffer na memória antes que estão sendo gravados no disco.

**IMPORTANTE**: documentação de MSDN (e documentação em outro lugar) Especifica que esse valor é em bytes (não quilobytes) e o padrão é 256. O valor for especificado, na verdade, em Kilobytes, e o valor padrão é 80. Por ter um valor padrão de 80 mil, podemos garantir que o buffer não terminem no heap de objeto grande.

## <a name="wizard-control"></a>Controle Wizard

Ele é bastante comum encontrar dificuldades com a tentativa de reunir informações em uma série de "páginas" usando painéis ou pela transferência de uma página para os desenvolvedores do ASP.NET. Mais frequentemente que não, o esforço é frustrante e é demorado. O novo controle de assistente resolve os problemas, permitindo não-linear e etapas em uma interface de assistente que os usuários estão familiarizados com. O controle Wizard apresenta os formulários de entrada em uma série de etapas. Cada etapa é de um tipo específico especificado pela propriedade StepType do controle. Os tipos de etapa disponíveis são da seguinte maneira:

| **Tipo de etapa** | **Explicação** |
| --- | --- |
| Auto | O assistente determina automaticamente o tipo de etapa com base em sua posição dentro da hierarquia de etapa. |
| Início | A primeira etapa, geralmente usada para apresentar uma instrução introdutória. |
| Etapa | Uma etapa normal. |
| Concluir | A etapa final, geralmente usada para apresentar um botão para concluir o assistente. |
| Concluir | Apresenta uma mensagem de êxito ou falha de comunicação. |

> [!NOTE]
> O controle Wizard mantém o registro de seu estado usando o estado de controle do ASP.NET. Portanto, a propriedade pode ser definida como false, sem nenhum dano.


Este vídeo é um passo a passo do controle Wizard.


![](server-controls/_static/image2.png)


[Abra vídeo de tela inteira](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Controle Localize

O controle Localize é semelhante a um controle Literal. No entanto, o controle Localize tem um **modo** propriedade que controla como a marcação que é adicionada a ele é renderizada. A propriedade Mode suporta os seguintes valores:

| **Modo** | **Explicação** |
| --- | --- |
| Transformar | Marcação é transformada de acordo com o protocolo do navegador que está fazendo a solicitação. |
| Passagem | Marcação é renderizada como-está. |
| Codificar | Marcação que é adicionada ao controle é codificada usando HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView e controles de exibição

O controle MultiView atua como um contêiner para controles de exibição e o controle de exibição atua como um contêiner (muito parecido com um painel de controle) para outros controles. Cada exibição em um controle MultiView é representada por um único controle de exibição. O primeiro controle de exibição no MultiView exibição 0 e o segundo é o modo de exibição 1, etc. Você pode alternar os modos de exibição, especificando o ActiveViewIndex do controle MultiView.

## <a name="substitution-control"></a>Controle de substituição

O controle Substitution é usado em conjunto com o cache do ASP.NET. Em casos em que você quiser tirar proveito do armazenamento em cache, mas você tem partes de uma página que devem ser atualizados em cada solicitação (em outras palavras, partes de uma página que estão isentos do armazenamento em cache), o componente de substituição fornece uma ótima solução. O controle, na verdade, não gera nenhuma saída por conta própria. Em vez disso, ele é associado a um método no código do lado do servidor. Quando a página é solicitada, o método é chamado e a marcação retornada é renderizada em lugar do controle de substituição.

O método ao qual o controle Substitution está associado é especificado por meio de **MethodName** propriedade. Esse método deve atender aos seguintes critérios:

- Ele deve ser um método estático (compartilhado no VB).
- Ele aceita um parâmetro de tipo HttpContext.
- Ele retorna uma cadeia de caracteres que representa a marcação que deve substituir o controle na página.

O controle Substitution não tem a capacidade de modificar qualquer outro controle na página, mas ela tem acesso ao HttpContext atual por meio de seu parâmetro.

## <a name="gridview-control"></a>Controle GridView

O controle GridView é o substituto para o controle DataGrid. Esse controle será abordado em mais detalhes em um módulo posterior.

## <a name="detailsview-control"></a>Controle DetailsView

O controle DetailsView permite que você exibir um registro individual de uma fonte de dados e editar ou excluí-lo. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="formview-control"></a>Controle FormView

O controle FormView é usado para exibir um registro individual de uma fonte de dados em uma interface configurável. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="accessdatasource-control"></a>Controle AccessDataSource

O controle AccessDataSource é usado para associar dados de um banco de dados. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="objectdatasource-control"></a>Controle ObjectDataSource

O controle ObjectDataSource é usado para dar suporte a uma arquitetura de três camadas, de modo que os controles podem ser associado a dados a um objeto de negócios de camada intermediária em vez de um modelo em duas camadas em que os controles são associados diretamente à fonte de dados. Ele será discutido mais detalhadamente em um módulo posterior.

## <a name="xmldatasource-control"></a>Controle XmlDataSource

O controle XmlDataSource é usado para associar dados a uma fonte de dados XML. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="sitemapdatasource-control"></a>Controle SiteMapDataSource

O controle SiteMapDataSource fornece vinculação de dados para controles de navegação do site com base em um mapa do site. Ele será discutido mais detalhadamente em um módulo posterior.

## <a name="sitemappath-control"></a>Controle SiteMapPath

O controle SiteMapPath exibe uma série de links de navegação conhecido como trilhas. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="menu-control"></a>Controle de Menu

O controle de Menu exibe menus dinâmicos usando DHTML. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="treeview-control"></a>Controle TreeView

O controle TreeView é usado para exibir uma exibição de árvore hierárquica de dados. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="login-control"></a>Controle de logon

Fornece o controle de logon para um mecanismo fazer logon em um site da Web. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginview-control"></a>Controle LoginView

O controle LoginView permite a exibição de modelos diferentes com base no status de logon do usuário. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="passwordrecovery-control"></a>Controle PasswordRecovery

O controle PasswordRecovery é usado para recuperar senhas esquecidas pelos usuários de um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginstatus"></a>LoginStatus

O controle LoginStatus exibe o status de logon do usuário. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="loginname"></a>LoginName

O controle LoginName exibe um nome de usuário depois que está sendo registrado em um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="createuserwizard"></a>CreateUserWizard

O CreateUserWizard é um assistente configurável que dá aos usuários a capacidade de criar uma conta de associação do ASP.NET para uso em um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="changepassword"></a>ChangePassword

O controle ChangePassword permite que os usuários alterem suas senhas para um aplicativo ASP.NET. Ele é abordado em mais detalhes em um módulo posterior.

## <a name="various-webparts"></a>Vários WebParts

O ASP.NET 2.0 é fornecido com várias partes da Web. Eles serão abordados em detalhes em um módulo posterior.
