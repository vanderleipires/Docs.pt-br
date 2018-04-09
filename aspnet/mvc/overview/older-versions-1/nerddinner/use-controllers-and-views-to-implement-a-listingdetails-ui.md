---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usar controladores e exibições para implementar uma interface de usuário/detalhes da listagem | Microsoft Docs
author: microsoft
description: Etapa 4 mostra como adicionar um controlador para o aplicativo que aproveita nosso modelo para fornecer aos usuários uma experiência de navegação de lista-detalhes de dados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: ac3568941eeef24bd9857c5787471aadea15fc7f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usar controladores e exibições para implementar uma interface de usuário de lista-detalhes
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 4 da livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 4 mostra como adicionar um controlador para o aplicativo que aproveita nosso modelo para fornecer aos usuários uma experiência de navegação/detalhes da listagem de dados para jantares em nosso site NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner etapa 4: Controladores e exibições

Estruturas da web tradicional (ASP clássico, Web Forms do ASP.NET, PHP etc), URLs de entrada geralmente são mapeados para arquivos no disco. Por exemplo: uma solicitação para uma URL, como "/ products. aspx" ou "/ Products.php" pode ser processado por um arquivo de "Products. aspx" ou "Products.php".

Estruturas MVC baseado na Web mapeiam URLs para o código de servidor de forma ligeiramente diferente. Em vez de mapeamento de URLs de entrada aos arquivos, em vez disso, eles são mapeados URLs para métodos nas classes. Essas classes são chamadas de "Controladores" e eles são responsáveis pelo processamento de solicitações HTTP de entrada, manipulação de entrada do usuário, recuperar e salvar dados e determinar a resposta para enviar de volta ao cliente (exibição de HTML, baixar um arquivo, Redirecione para outro URL, etc.).

Agora que criamos um modelo básico de nosso aplicativo NerdDinner, nossa próxima etapa será adicionar um controlador para o aplicativo que tira proveito dele para fornecer aos usuários uma experiência de navegação/detalhes da listagem de dados para jantares em nosso site.

### <a name="adding-a-dinnerscontroller-controller"></a>Adicionar um controlador de DinnersController

Vamos começar clicando na pasta "Controladores" em nosso projeto web e, em seguida, selecione o **Add -&gt;controlador** comando de menu (você também pode executar esse comando digitando Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar controlador":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Vamos nomear o novo controlador de "DinnersController" e clique no botão "Adicionar". O Visual Studio irá adicionar um arquivo de DinnersController.cs em nosso diretório \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Ele também abrirá a nova classe DinnersController no editor de código.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Adicionando Index () e os métodos de ação Details() à classe DinnersController

Queremos permitem que os visitantes usando nosso aplicativo para procurar uma lista de jantares futuros e permitir que eles se clicar em qualquer uma refeição na lista para ver os detalhes específicos sobre ele. Faremos isso publicando as seguintes URLs de nosso aplicativo:

| **URL** | **Finalidade** |
| --- | --- |
| */Dinners/* | Exibir uma lista HTML de jantares futuros |
| */Dinners/Details/[id]* | Exibir detalhes sobre uma refeição específico indicado por um parâmetro de "id" incorporado a URL – que corresponderá a DinnerID de refeição no banco de dados. Por exemplo: /Dinners/Details/2 exibirá uma página HTML com detalhes sobre a refeição cujo valor DinnerID é 2. |

Publicaremos implementações inicias dessas URLs, adicionando dois público "métodos de ação" a nossa classe DinnersController como abaixo:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Em seguida, vamos executar o aplicativo NerdDinner e usar nosso navegador invocá-los. Digitando o *"Jantares /"* fará com que a URL de nosso *Index ()* método a ser executado e retornará a seguinte resposta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Digitando o *"/ detalhes/jantares/2"* URL fará com que nosso *Details()* método para executar e enviar de volta a resposta a seguir:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Você deve estar se perguntando - como ASP.NET MVC sabia para criar nossa classe DinnersController e chamar os métodos? Para entender que vamos dar uma olhada rápida como roteamento funciona.

### <a name="understanding-aspnet-mvc-routing"></a>Noções básicas sobre encaminhamento do ASP.NET MVC

O ASP.NET MVC inclui um poderoso mecanismo de roteamento de URL que fornece muita flexibilidade para controlar como as URLs são mapeados para classes do controlador. Isso permite que nós completamente personalizar como o ASP.NET MVC escolhe qual classe de controlador para criar, qual método de invocá-lo, bem como configurar formas diferentes que variáveis podem ser automaticamente analisadas a partir de URL/Querystring e passadas para o método como parâmetro argumentos. Ele oferece a flexibilidade para totalmente otimizar um site para SEO (otimização do mecanismo de pesquisa), bem como qualquer estrutura de URL que desejamos por meio de um aplicativo de publicação.

Por padrão, os novos projetos do ASP.NET MVC vem com um conjunto predefinido de regras de roteamento de URL já está registrado. Isso nos permite facilmente começar em um aplicativo sem a necessidade de configurar qualquer coisa explicitamente. Os registros de regra de roteamento padrão podem ser encontrados na classe "Aplicativo" dos nossos projetos – o que pode ser aberto clicando duas vezes no arquivo "Global. asax" na raiz do projeto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

As regras de roteamento do ASP.NET MVC padrão são registradas no método "RegisterRoutes" desta classe:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

As rotas". MapRoute() "acima da chamada de método registra uma regra de roteamento padrão que mapeia a URLs de entrada para classes do controlador usando o formato de URL:" / {controller} / {action} / {id} "– onde"controller"é o nome da classe do controlador para criar uma instância,"ação"é o nome de um método público a ser invocado, e "id" é um parâmetro opcional incorporado a URL que pode ser passada como um argumento para o método. O terceiro parâmetro passado para a chamada do método "MapRoute()" é um conjunto de valores padrão a ser usado para os valores de id/de ação de controlador que eles não estão presentes na URL (controlador = "Home", ação = "Index", Id = "").

Abaixo está uma tabela que demonstra como uma variedade de URLs são mapeados usando o padrão "<em>/ {controladores} / {action} / {id}"</em>regra de rota:

| **URL** | **Classe do controlador** | **Método de ação** | **Parâmetros passados** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(ID) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edit(ID) | id=5 |
| */Dinners/Create* | DinnersController | Create() | N/D |
| */Dinners* | DinnersController | Index() | N/D |
| */Home* | HomeController | Index() | N/D |
| */* | HomeController | Index() | N/D |

As três últimas linhas mostram os valores padrão (controlador Home, = ação = índice, Id = "") que está sendo usado. Porque o método "Index" está registrado como o nome de ação padrão se nenhuma for especificada, o "/ jantares" e "/home" causa de URLs o método de ação Index () a ser invocado em suas classes de controlador. Porque o controlador "Início" é registrado como o controlador padrão se nenhuma for especificada, a URL "/" faz com que o HomeController a ser criado e o método de ação Index () em que ele seja invocado.

Se você não deseja que essas regras de roteamento de URL padrão, a boa notícia é que eles são fáceis de alterar - apenas editá-las no método RegisterRoutes acima. Em nosso aplicativo NerdDinner, porém, não vamos alterar qualquer uma das regras de roteamento de URL padrão – em vez disso, usaremos-los como-é.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Usando o DinnerRepository do nosso DinnersController

Vamos agora substituir nossa implementação atual do DinnersController métodos de ação Index () e Details() com implementações de usam nosso modelo.

Vamos usar a classe DinnerRepository que criamos anteriormente para implementar o comportamento. Vamos começar adicionando uma instrução "using" que faz referência ao namespace "NerdDinner.Models" e, em seguida, declarar uma instância de nossa DinnerRepository como um campo em nossa classe DinnerController.

Neste capítulo vamos apresentar o conceito de "Injeção de dependência" e mostrar outra maneira para nosso controladores obter uma referência a um DinnerRepository que permite que a unidade melhor teste – mas para direita agora vamos apenas criar uma instância do nosso DinnerRepository embutido como abaixo.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Agora você está pronto para gerar uma resposta HTML usando os objetos de modelo de dados recuperados.

### <a name="using-views-with-our-controller"></a>Usando exibições com nosso controlador

Embora seja possível gravar o código em nossos métodos de ação para reunir HTML e, em seguida, use o *Response* método auxiliar para enviar ao cliente, esse método torna-se bastante complicado rapidamente. Uma abordagem muito melhor é para que possamos para executar apenas o aplicativo e a lógica de dados dentro de nossas DinnersController os métodos de ação e, em seguida, passar os dados necessários para renderizar uma resposta HTML para um modelo de "exibição" separada que é responsável por gerar a representação de HTML dele. Como você verá em breve, um modelo de "Exibir" é um arquivo de texto que geralmente contém uma combinação de marcação HTML e código de renderização incorporado.

Separar nossa lógica do controlador de nosso processamento em modo traz várias grandes benefícios. Em particular, ele ajuda impor uma "separação de preocupações" clara entre o código do aplicativo e o código de formatação/processamento de interface do usuário. Isso facilita muito a lógica do aplicativo de teste de unidade em isolamento de lógica de processamento de interface do usuário. Ele torna mais fácil modificar os modelos de renderização de interface do usuário posteriormente sem precisar fazer alterações de código do aplicativo. E ele pode tornar mais fácil para os desenvolvedores e designers colaborarem juntos em projetos.

Podemos atualizar nossa classe DinnersController para indicar que queremos usar um modelo de exibição para enviar de volta uma resposta de IU em HTML, alterando as assinaturas de método nossas duas de métodos de ação de ter um tipo de retorno de "void" em vez disso, ter um tipo de retorno de "ActionResult". Em seguida, podemos chamar o *View()* método auxiliar na classe base do controlador para retornar novamente um objeto "ViewResult" como abaixo:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

A assinatura do *View()* método auxiliar que estamos usando acima aparência abaixo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

O primeiro parâmetro para o *View()* método auxiliar é o nome do arquivo do modelo de exibição que deseja usar para processar a resposta HTML. O segundo parâmetro é um objeto de modelo que contém os dados que o modelo de exibição precisa para processar a resposta HTML.

Em nosso método de ação Index () está chamando o *View()* método auxiliar e que indica que desejamos renderizar uma listagem HTML de jantares usando um modelo de exibição "Index". Passa o modelo de exibição de uma sequência de objetos de uma refeição para gerar a lista de:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Em nosso método de ação Details() podemos tentar recuperar um objeto de uma refeição usando a id fornecida na URL de. Se uma refeição válida for encontrada, podemos chamar o *View()* método auxiliar, que indica que desejamos usar um modelo de exibição "Detalhes" para renderizar o objeto refeição recuperado. Se for solicitada uma refeição inválida, podemos processar uma mensagem de erro úteis que indica que a refeição não existe usando um modelo de exibição de "NotFound" (e uma versão sobrecarregada do *View()* método auxiliar que usa apenas o nome do modelo ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Agora, vamos implementar os modelos de exibição de "NotFound", "Detalhes" e "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementando o modelo de exibição de "NotFound"

Vamos começar implementando o modelo de exibição de "NotFound" – que exibe uma mensagem de erro amigável indicando que uma refeição solicitada não foi encontrada.

Vamos criar um novo modelo de exibição posicionando o cursor nosso texto dentro de um método de ação do controlador e, em seguida, clique com botão direito e escolha o comando de menu "Adicionar modo de exibição" (que também pode executar este comando digitando Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Isso abrirá uma caixa de diálogo "Adicionar modo de exibição" como abaixo. Por padrão, que a caixa de diálogo serão preenchidas automaticamente o nome de exibição a ser criada para corresponder ao nome do método de ação o cursor estava quando a caixa de diálogo foi iniciada (no caso "Detalhes"). Como queremos primeiro implementar o modelo de "NotFound", vamos substituir esse nome de exibição e defini-lo para ser "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Quando, clique no botão "Adicionar", o Visual Studio criará um novo modelo de exibição "NotFound.aspx" para que possamos dentro do diretório "\Views\Dinners" (que ele criará também se o diretório ainda não existir):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Ele também abrirá nosso novo modelo de exibição de "NotFound.aspx" no editor de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Modelos de exibição por padrão têm dois "áreas de conteúdo" em que podemos adicionar conteúdo e código. A primeira permite personalizar o "title" da página HTML enviada de volta. A segunda permite personalizar o "conteúdo principal" da página HTML enviado de volta.

Para implementar o nosso modelo de exibição de "NotFound" Vamos adicionar alguns conteúdo básico:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

É possível, em seguida, experimente dentro do navegador. Para fazer isso vamos solicitação o *"/ detalhes/jantares/9999"* URL. Isso fará referência a uma refeição que atualmente não existe no banco de dados e fará com que o método de ação nosso DinnersController.Details() renderizar o nosso modelo de exibição de "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Uma coisa que você observará na captura de tela acima é que nosso modelo de exibição básica tem herdada de uma série de HTML ao redor do conteúdo principal na tela. Isso ocorre porque o nosso modelo de exibição está usando um modelo de "página mestra" que permite aplicar um layout consistente em todos os modos de exibição no site. Vamos discutir como páginas mestras funcionam mais na parte posterior deste tutorial.

### <a name="implementing-the-details-view-template"></a>Implementando o modelo de exibição "Detalhes"

Agora, vamos implementar o modelo de exibição "Detalhes" – que irá gerar o HTML para um único modelo refeição.

Podemos vai fazer isso posicionando o cursor nosso texto dentro do método de ação de detalhes e, em seguida, clique com botão direito e escolha o comando de menu "Adicionar modo de exibição" (ou pressione Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Isso abrirá a caixa de diálogo "Adicionar modo de exibição". Vamos manter o nome de exibição padrão ("Detalhes"). Também vamos selecionar a caixa de seleção "Criar uma exibição fortemente tipada" na caixa de diálogo e selecione (usando a lista suspensa combobox) o nome do tipo de modelo, passa do controlador para o modo de exibição. Para este modo de exibição, passa um objeto de uma refeição (o nome totalmente qualificado para este tipo é: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Ao contrário do modelo anterior, em que escolhemos para criar um "modo de exibição vazio", neste momento, que podemos escolher para automaticamente "scaffold" modo de exibição usando um modelo de "Detalhes". Podemos pode indicar isso alterando "Exibir conteúdo" lista suspensa na caixa de diálogo acima.

"Estrutura" irá gerar uma implementação inicial de nosso modelo de exibição de detalhes com base no objeto refeição, passa a ele. Isso fornece uma maneira fácil para que possamos começar rapidamente em nossa implementação do modelo de exibição.

Quando, clique no botão "Adicionar", o Visual Studio criará um novo arquivo de modelo de exibição "Details.aspx" para nós em nosso diretório "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Ele também abrirá nosso novo modelo de exibição de "Details.aspx" no editor de código. Ele conterá uma implementação de scaffold inicial de uma exibição de detalhes com base em um modelo de uma refeição. O mecanismo de scaffolding usa reflexão do .NET para examinar as propriedades públicas expostas na classe passada a ele e adicionará o conteúdo apropriado com base em cada tipo encontrar:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

É possível solicitar a *"/ detalhes/jantares/1"* URL para ver como essa implementação de scaffold "Detalhes" aparece no navegador. Usar esta URL será exibida uma as jantares que manualmente adicionamos ao nosso banco de dados quando é criado pela primeira vez que ele:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Isso nos leva em funcionamento rapidamente e fornece uma implementação inicial de nosso exibição Details.aspx. Em seguida, podemos ir e ajustá-lo para personalizar a interface do usuário para nosso satisfação.

Ao examinarmos o modelo Details.aspx mais de perto, descobriremos que ele contém HTML estático, bem como código de renderização inserido. &lt;% %&gt; novidades sobre o código para executar código quando processa o modelo de exibição, e &lt;% = %&gt; novidades sobre o código executar o código contido neles e, em seguida, processar o resultado ao fluxo de saída do modelo.

Podemos escrever código dentro de nossa visão que acessa o objeto de modelo "Refeição" que foi passado do nosso controlador usando uma propriedade de "Modelo" fortemente tipada. O Visual Studio forneça intellisense de código completo ao acessar essa propriedade "Modelo" dentro do editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Vamos fazer alguns ajustes para que a fonte para nosso modelo de exibição de detalhes final aparência abaixo:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Quando o acesso é o *"/ detalhes/jantares/1"* URL novamente ele agora será renderização como abaixo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementando o modelo de exibição "Index"

Agora, vamos implementar o modelo de exibição "Index" – que irá gerar uma lista de jantares futuros. Fazer isso vamos posicionar o cursor nosso texto dentro do método de ação de índice e a direita, em seguida, clique em e escolha o comando de menu "Adicionar modo de exibição" (ou pressione Ctrl-M, Ctrl-V).

A caixa de diálogo "Adicionar modo de exibição" Vamos manter o modelo de exibição denominado "Index" e selecione a caixa de seleção "Criar uma exibição fortemente tipada". Neste momento, podemos escolher para automaticamente gerar um modelo de exibição "List" e selecione "NerdDinner.Models.Dinner" como o tipo de modelo passado para o modo de exibição (que porque estamos indicaram que estamos criando uma "lista" scaffold fará com que a caixa de diálogo Adicionar modo de exibição supor que estamos passando uma sequência de objetos de uma refeição de nosso controlador para o modo de exibição):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Quando, clique no botão "Adicionar", o Visual Studio criará um novo arquivo de modelo de exibição "Index.aspx" para que possamos em nosso diretório "\Views\Dinners". Ele será "scaffold" uma implementação inicial dentro dele que fornece uma listagem de tabela HTML das jantares que podemos passar para o modo de exibição.

Quando executamos o aplicativo e o acesso a *"Jantares /"* URL ele processará nossa lista de jantares da seguinte forma:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

A solução de tabela acima nos dá um layout de grade de nossos dados refeição – que não não exatamente o que queremos para nosso cliente voltados para a listagem de refeição. Podemos atualizar o modelo de exibição Index.aspx e modificá-lo para a lista menos colunas de dados e use um &lt;ul&gt; elemento para renderizá-los em vez de uma tabela usando o código a seguir:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Estamos usando a palavra-chave de "var" dentro da instrução foreach acima, faremos um loop sobre cada uma refeição em nosso modelo. Aqueles familiarizado com c# 3.0 poderá achar que o uso de "var" significa que o objeto de uma refeição é associação tardia. Em vez disso, significa que o compilador está usando inferência de tipo em relação à propriedade "Modelo" fortemente tipada (que é do tipo "IEnumerable&lt;refeição&gt;") e compilar a variável local "refeição" como um tipo de uma refeição – o que significa que obtemos completos IntelliSense e procurando dentro de blocos de código de tempo de compilação:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Quando ocorreu a atualização */Dinners* URL em nosso navegador nosso exibição atualizada agora parece abaixo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Isso está procurando melhor – mas ainda não está totalmente lá. Nossa última etapa é habilitar os usuários finais clique jantares individuais na lista e ver os detalhes sobre eles. Implementaremos isso por elementos de hiperlink HTML com links para o método de ação de detalhes em nosso DinnersController de renderização.

Podemos gerar esses hiperlinks em nossa visualização de índice em uma das duas maneiras. A primeira é criar manualmente o HTML &lt;um&gt; elementos como abaixo, onde podemos inserir &lt;% %&gt; blocos dentro de &lt;um&gt; elemento HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

É uma abordagem alternativa que podemos usar tirar proveito do método de auxiliares de "Html.ActionLink()" interno dentro do ASP.NET MVC que oferece suporte à criação programaticamente um HTML &lt;um&gt; elemento que é vinculado a outro método de ação em um Controlador:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

O primeiro parâmetro para o método auxiliar Html.ActionLink() é o texto do link para exibir (no caso do título do refeição), o segundo parâmetro é o nome de ação do controlador que desejamos para gerar o link para (nesse caso, o método de detalhes), e o terceiro parâmetro é um conjunto de parâmetros a serem enviados para a ação (implementada como um tipo anônimo com valores/nome de propriedade). Nesse caso estamos especificando o parâmetro "id" de refeição que deseja vincular e como o roteamento de URL padrão de regra no ASP.NET MVC é "{Controller} / {Action} / {id}" o método auxiliar Html.ActionLink() gerará a seguinte saída:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Para exibir nossa Index.aspx vamos usar a abordagem de método auxiliar Html.ActionLink() e ter cada uma refeição no link para a URL apropriada de detalhes lista:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

E agora quando ocorreu o */Dinners* URL nossa lista refeição aparência abaixo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Quando clicar em qualquer um dos jantares na lista de nós navegará para ver detalhes sobre ele:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Baseado em convenção de nomenclatura e a estrutura do diretório \Views

Por padrão, aplicativos ASP.NET MVC usam um diretório baseado em convenção de nomenclatura estrutura durante a resolução de exibição de modelos. Isso permite que os desenvolvedores a evitar ter que qualificar totalmente-um caminho de local ao fazer referência a exibições de dentro de uma classe de controlador. Por padrão ASP.NET MVC procurará o arquivo de modelo de exibição dentro de * \Views\[ControllerName]\* diretório sob o aplicativo.

Por exemplo, podemos trabalhado na classe DinnersController – que referencia explicitamente os três modelos de exibição: "Index", "Detalhes" e "NotFound". ASP.NET MVC por padrão procurará por essas exibições dentro do *\Views\Dinners* diretório sob nosso diretório raiz do aplicativo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Observe acima como não há atualmente há três classes de controlador dentro do projeto (DinnersController, HomeController e AccountController – as duas últimas foram adicionadas por padrão, quando criamos o projeto), e há três subdiretórios (uma para cada controlador) dentro do diretório \Views.

Exibições referenciadas dos controladores de casa e contas serão resolvida automaticamente seus modelos de exibição dos respectivos *\Views\Home* e *\Views\Account* diretórios. O *\Views\Shared* subdiretório fornece uma maneira de armazenar modelos de exibição que são reutilizados em vários controladores de dentro do aplicativo. Quando o ASP.NET MVC tenta resolver um modelo de exibição, ele verificará primeiro dentro o *\Views\[controlador]* diretório específico, e se ele não é possível localizar o modelo de exibição existe terá aparência dentro do *\Views\ Compartilhado* directory.

Quando se trata de modelos de exibição individual de nomeação, as diretrizes recomendadas é para que o modelo de exibição compartilham o mesmo nome que o método de ação que causou a processar. Por exemplo, acima nosso "Index" método de ação estiver usando o modo de "Index" para processar o resultado da exibição, e o método de ação "Detalhes" está usando o modo de exibição "Detalhes" para processar os resultados. Isso facilita ver rapidamente qual modelo é associado a cada ação.

Os desenvolvedores não precisam especificar explicitamente o nome do modelo de exibição quando o modelo de exibição tem o mesmo nome que o método de ação que está sendo invocado no controlador. Podemos em vez disso, basta passar o objeto de modelo para o método auxiliar "View()" (sem especificar o nome de exibição) e o ASP.NET MVC deduzirá automaticamente que queremos usar o *\Views\[ControllerName]\[ActionName]* exibir modelo em disco para renderizá-lo.

Isso permite limpar um pouco o nosso código do controlador e evitar a duplicação do nome duas vezes em nosso código:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

O código acima é tudo o que é necessário para implementar uma refeição listagem/detalhes adequado para o site.

#### <a name="next-step"></a>Próxima etapa

Agora temos uma refeição adequada criada de experiência de navegação.

Agora, vamos Habilitar suporte de edição de formulários de dados CRUD (criar, ler, atualizar, excluir).

> [!div class="step-by-step"]
> [Anterior](build-a-model-with-business-rule-validations.md)
> [Próximo](provide-crud-create-read-update-delete-data-form-entry-support.md)
