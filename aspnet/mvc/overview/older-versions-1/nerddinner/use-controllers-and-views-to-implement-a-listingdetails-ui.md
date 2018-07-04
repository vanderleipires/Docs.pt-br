---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usar controladores e exibições para implementar uma interface do usuário de listagem/detalhes | Microsoft Docs
author: microsoft
description: Etapa 4 mostra como adicionar um controlador para o aplicativo que tira proveito do nosso modelo para fornecer aos usuários uma experiência de navegação de listagem/detalhes de dados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 4fe065e29950a076de07d73205a97399f82f07d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377870"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usar controladores e exibições para implementar uma interface do usuário de listagem/detalhes
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 4 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 4 mostra como adicionar um controlador para o aplicativo que tira proveito do nosso modelo para fornecer aos usuários uma experiência de navegação da listagem/detalhes de dados para jantares em nosso site do NerdDinner.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-4-controllers-and-views"></a>Etapa 4 do NerdDinner: Controladores e exibições

Com estruturas da web tradicional (ASP clássico, Web Forms do ASP.NET, PHP etc), as URLs de entrada são mapeadas normalmente a arquivos no disco. Por exemplo: uma solicitação para uma URL como "/ products. aspx" ou "/ products" pode ser processada por um arquivo de "Products. aspx" ou "Products".

Estruturas MVC baseado na Web mapeiam URLs para o código do servidor de forma ligeiramente diferente. Em vez de mapeamento de URLs de entrada aos arquivos, em vez disso, eles mapeiam URLs para métodos nas classes. Essas classes são chamadas de "Controladores" e eles são responsáveis pelo processamento de solicitações HTTP de entrada, manipulando a entrada do usuário, recuperar e salvar dados e determinar a resposta para enviar de volta para o cliente (exibição de HTML, baixar um arquivo, redirecionar para outra URL, etc.).

Agora que criamos um modelo básico de nosso aplicativo NerdDinner, nossa próxima etapa será adicionar um controlador para o aplicativo que tira proveito para fornecer aos usuários uma experiência de navegação da listagem/detalhes de dados para jantares em nosso site.

### <a name="adding-a-dinnerscontroller-controller"></a>Adicionando um controlador DinnersController

Vamos começar pelo botão direito do mouse na pasta "Controladores de" dentro do nosso projeto web e, em seguida, selecione a **Add -&gt;controlador** comando de menu (você também pode executar esse comando, digitando Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar controlador":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Vamos nomear o novo controlador "DinnersController" e clique no botão "Adicionar". Visual Studio, em seguida, adicionará um arquivo DinnersController.cs em nosso diretório \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Ele também será aberta a nova classe DinnersController dentro do editor de código.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Adicionando Index () e métodos de ação de Details() à classe DinnersController

Queremos permitir que os visitantes usando nosso aplicativo para procurar uma lista de jantares futuros e permitir que eles se clicar em qualquer jantar na lista para ver detalhes específicos sobre ele. Faremos isso ao publicar as seguintes URLs do nosso aplicativo:

| **URL** | **Finalidade** |
| --- | --- |
| */Dinners/* | Exibir uma lista HTML de jantares futuras |
| */Dinners/detalhes / [id]* | Exibir detalhes sobre um jantar específico indicado por um parâmetro de "id" embutido na URL – que corresponderá a DinnerID de jantar no banco de dados. Por exemplo: /Dinners/Details/2 exibiria uma página HTML com detalhes sobre o jantar cujo valor DinnerID é 2. |

Publicaremos inicias implementações dessas URLs, adicionando duas público "métodos de ação" à nossa classe DinnersController como abaixo:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Em seguida, vamos executar o aplicativo NerdDinner e usar nosso navegador para invocá-los. Digitando o *"Jantares /"* fará com que a URL do nosso *Index ()* método para execução e ele enviará de volta a seguinte resposta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Digitando o *"/ detalhes/jantares/2"* fará com que a URL do nosso *Details()* método para executar e enviar de volta a seguinte resposta:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Você deve estar se perguntando - como ASP.NET MVC sabia para criar nossa classe DinnersController e chamar esses métodos? Para entender que vamos dar uma olhada rápida em como funciona o roteamento.

### <a name="understanding-aspnet-mvc-routing"></a>Noções básicas sobre encaminhamento do ASP.NET MVC

O ASP.NET MVC inclui um poderoso mecanismo de roteamento de URL que fornece uma grande flexibilidade para controlar como as URLs são mapeadas para classes do controlador. Ele nos permite personalizar completamente como o ASP.NET MVC escolhe qual classe de controlador para criar, qual método de invocá-lo, bem como configurar diferentes maneiras que variáveis podem ser automaticamente analisadas a partir do URL/Querystring e passadas para o método como parâmetro argumentos. Ele oferece a flexibilidade para totalmente otimizar um site para SEO (otimização do mecanismo de pesquisa), bem como qualquer estrutura de URL que queremos por meio de um aplicativo de publicação.

Por padrão, os novos projetos do ASP.NET MVC vêm com um conjunto predefinido de regras de roteamento de URL já registrado. Isso nos permite facilmente começar a usar em um aplicativo sem a necessidade de configurar qualquer coisa explicitamente. Os registros de regra de roteamento padrão podem ser encontrados dentro da classe de "Aplicativo" de nossos projetos – o que pode ser aberto clicando duas vezes o arquivo "Global. asax" na raiz do nosso projeto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

As regras de roteamento do ASP.NET MVC padrão são registradas dentro do método "RegisterRoutes" desta classe:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

As rotas". MapRoute() "acima da chamada de método registra uma regra de roteamento padrão que mapeia as URLs de entrada para classes de controlador usando o formato de URL:" / {controller} / {action} / {id} "– onde"controller"é o nome da classe do controlador para criar uma instância,"ação"é o nome de um método público a ser invocado e "id" é um parâmetro opcional embutido na URL que pode ser passado como um argumento para o método. O terceiro parâmetro passado para a chamada do método "MapRoute()" é um conjunto de valores padrão a ser usado para os valores de id/de ação de controlador que não estiverem presentes na URL (controlador = "Home" Action = "Index", Id = "").

Abaixo está uma tabela que demonstra como uma variedade de URLs são mapeados usando o padrão "<em>/ {controladores} / {action} / {id}"</em>regra de rota:

| **URL** | **Classe de controlador** | **Método de ação** | **Parâmetros passados** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(ID) | id=2 |
| *Jantares/5/edição* | DinnersController | Edit(ID) | id=5 |
| */Dinners/Create* | DinnersController | Create) | N/D |
| */ Jantares* | DinnersController | Index) | N/D |
| */ Home* | HomeController | Index) | N/D |
| */* | HomeController | Index) | N/D |

As três últimas linhas mostram os valores padrão (controlador Home, = Action = índice, Id = "") que está sendo usado. Porque o método "Index" é registrado como o nome da ação padrão se nenhuma for especificada, o "/ jantares" e "/home" causa de URLs do método de ação Index () a ser invocada em suas classes de controlador. Porque o controlador "Home" é registrado como o controlador padrão se nenhuma for especificada, a URL "/" faz com que o HomeController a ser criado e o método de ação Index () em que ele seja invocado.

Se você não gostar essas regras de roteamento de URL padrão, a boa notícia é que eles sejam fáceis de alterar - simplesmente edite-os no método RegisterRoutes acima. Para nosso aplicativo NerdDinner, no entanto, não vamos alterar qualquer uma das regras de roteamento de URL padrão – em vez disso, vamos apenas usá-los como-está.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Usando o DinnerRepository do nosso DinnersController

Vamos agora substituir nossa implementação atual do DinnersController métodos de ação Index () e Details() com implementações que usam nosso modelo.

Vamos usar a classe de DinnerRepository que criamos anteriormente para implementar o comportamento. Vamos começar pela adição de uma instrução "using" que faz referência ao namespace "NerdDinner.Models" e, em seguida, declarar uma instância do nosso DinnerRepository como um campo em nossa classe DinnerController.

Mais adiante neste capítulo vamos introduzir o conceito de "Injeção de dependência" e mostrar outra maneira para que os controladores obter uma referência a um DinnerRepository que permite melhor unidade teste – mas, para a direita, agora vamos simplesmente criar uma instância do nosso DinnerRepository embutido, como abaixo.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Agora estamos prontos para gerar uma resposta HTML usando nossos objetos de modelo de dados recuperados.

### <a name="using-views-with-our-controller"></a>Usando exibições com nosso controlador

Embora seja possível escrever código em nossos métodos de ação para montar o HTML e, em seguida, usar o *Response* método auxiliar para enviará de volta para o cliente, que abordagem torna-se bastante complicada rapidamente. Uma abordagem muito melhor é para que possamos executar somente aplicativos e lógica de dados dentro de nossos métodos de ação DinnersController e, em seguida, passar os dados necessários para renderizar uma resposta HTML a um modelo de "exibição" separada que é responsável por gerar a representação de HTML dele. Como veremos daqui a pouco, um modelo de "Exibir" é um arquivo de texto que geralmente contém uma combinação de marcação HTML e código de renderização incorporado.

Separar nossa lógica do controlador de renderização da exibição traz várias grandes benefícios. Em particular, ele ajuda impor uma clara "separação de preocupações" entre o código do aplicativo e o código de formatação/renderização de interface do usuário. Isso facilita muito a lógica do aplicativo de teste de unidade em isolamento de lógica de processamento de interface do usuário. Ele facilita a posterior modificar os modelos de renderização de interface do usuário sem precisar fazer alterações de código do aplicativo. E ele pode tornar mais fácil para desenvolvedores e designers colaborem em projetos.

Podemos atualizar nossa classe DinnersController para indicar que desejamos usar um modelo de exibição para enviar de volta uma resposta de IU em HTML, alterando as assinaturas de método nossos dois de métodos de ação da necessidade de um tipo de retorno de "void" em vez disso, ter um tipo de retorno de "ActionResult". Em seguida, podemos chamar o *View()* método auxiliar na classe base do controlador para retornar um objeto "ViewResult" como abaixo:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

A assinatura do *View()* método auxiliar que estamos usando acima a aparência abaixo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

O primeiro parâmetro para o *View()* método auxiliar é o nome do arquivo de modelo de exibição que queremos usar para processar a resposta HTML. O segundo parâmetro é um objeto de modelo que contém os dados que o modelo de exibição precisa para processar a resposta HTML.

Em nosso método de ação Index (), chamamos o *View()* método auxiliar e que indica que desejamos renderizar uma lista de HTML de jantares usando um modelo de exibição "Index". Estamos passando o modelo de exibição de uma sequência de objetos de jantar para gerar a lista de:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Dentro do nosso método de ação Details() podemos tentar recuperar um objeto de Dinner usando a id fornecida na URL. Se um jantar válido for encontrado, chamamos o *View()* método auxiliar, que indica que queremos usar um modelo de exibição "Detalhes" para renderizar o objeto recuperado do jantar. Se for solicitado um jantar inválido, podemos processar uma mensagem de erro útil que indica que o jantar não existe usando um modelo de exibição de "NotFound" (e uma versão sobrecarregada do *View()* método auxiliar que usa apenas o nome do modelo ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Agora, vamos implementar os modelos de exibição "Index", "Detalhes" e "NotFound".

### <a name="implementing-the-notfound-view-template"></a>Implementando o modelo de exibição de "NotFound"

Vamos começar com a implementação do modelo de exibição de "NotFound" – que exibe uma mensagem de erro amigável indicando que o jantar solicitado não foi encontrado.

Podemos vai criar um novo modelo de exibição posicionando nosso cursor de texto dentro de um método de ação do controlador e, em seguida, clique com botão direito e escolha o comando de menu "Adicionar exibição" (que também pode executar esse comando, digitando Ctrl-M, Ctrl + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Isso abrirá uma caixa de diálogo "Adicionar exibição", como abaixo. Por padrão, que a caixa de diálogo irá preencher previamente o nome de exibição a ser criada para corresponder ao nome do método de ação o cursor estava quando a caixa de diálogo foi iniciada (no caso "Detalhes"). Como queremos primeiro implementar o modelo de "NotFound", vamos substituir esse nome de exibição e defini-la em "NotFound" em vez disso:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Quando clicamos no botão "Adicionar", o Visual Studio criará um novo modelo de exibição de "NotFound.aspx" para que possamos dentro do diretório "\Views\Dinners" (que também será criado se o diretório ainda não existir):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Ele também será aberta nosso novo modelo de exibição de "NotFound.aspx" dentro do editor de código:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Modelos de exibição por padrão têm dois "áreas de conteúdo" em que podemos adicionar conteúdo e o código. A primeira permite personalizar o "title" da página HTML enviada de volta. O segundo nos permite personalizar o "conteúdo principal" da página HTML enviado de volta.

Para implementar nosso modelo de exibição de "NotFound" Vamos adicionar algum conteúdo básico:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Podemos pode, em seguida, experimente dentro do navegador. Para fazer isso vamos solicitação a *"/ detalhes/jantares/9999"* URL. Isso fará referência a um jantar que atualmente não existe no banco de dados e fará com que nosso método de ação DinnersController.Details() renderizar o nosso modelo de exibição de "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Uma coisa que você observará na captura de tela acima é que o nosso modelo de exibição básica herdou um monte de HTML que circunda o conteúdo principal na tela. Isso ocorre porque o nosso modelo de exibição está usando um modelo de "página mestra" que permite aplicar um layout consistente em todas as exibições no site. Discutiremos como páginas mestras funcionam mais em uma parte posterior deste tutorial.

### <a name="implementing-the-details-view-template"></a>Implementando o modelo de exibição "Detalhes"

Agora, vamos implementar o modelo de exibição "Detalhes" – que irá gerar HTML para um único modelo de jantar.

Podemos vai fazer isso posicionando o cursor nosso texto dentro do método de ação de detalhes e, em seguida, clique com botão direito e escolha o comando de menu "Adicionar exibição" (ou pressione Ctrl-M, Ctrl + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Isso abrirá a caixa de diálogo "Adicionar exibição". Vamos manter o nome de exibição padrão ("Detalhes"). Também vamos selecionar a caixa de seleção "Criar uma exibição fortemente tipada" na caixa de diálogo e selecionar (usando o controle suspenso combobox) o nome do tipo de modelo que estamos passando do controlador para o modo de exibição. Para este modo de exibição que estão passando um objeto de jantar (o nome totalmente qualificado para este tipo é: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Ao contrário do modelo anterior, onde podemos optar por criar um "modo de exibição vazio", neste momento, que podemos optar por automaticamente "scaffold" modo de exibição usando um modelo de "Detalhes". É possível indicar isso alterando a "Conteúdo do modo de exibição" na lista suspensa caixa de diálogo acima.

"O scaffolding" irá gerar uma implementação inicial de nosso modelo de exibição de detalhes com base no objeto de Dinner que estamos passando a ele. Isso fornece uma maneira fácil para que possamos começar a usar rapidamente em nossa implementação do modelo de exibição.

Quando clicamos no botão "Adicionar", o Visual Studio criará um novo arquivo de modelo de exibição "Details. aspx" para que possamos dentro do nosso diretório "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Ele também será aberta nosso novo modelo de exibição de "Details. aspx" dentro do editor de código. Ele conterá uma implementação de scaffold inicial de uma exibição de detalhes com base em um modelo de jantar. O mecanismo de scaffolding usa a reflexão do .NET para examinar as propriedades públicas expostas na classe passada a ele e adicionará o conteúdo apropriado com base em cada tipo de encontrar:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Podemos pode solicitar a *"/ detalhes/jantares/1"* URL para ver como essa implementação de scaffold "Detalhes" aparece no navegador. Usar essa URL exibirá um dos jantares que manualmente adicionamos ao nosso banco de dados quando é criado ele:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Isso nos leva rapidamente em funcionamento e nos fornece uma implementação inicial de nossa exibição details. aspx. Em seguida, podemos ir e ajustá-lo para personalizar a interface do usuário para nossa satisfação.

Quando olhamos o modelo details. aspx mais de perto, podemos encontrará que ele contém o HTML estático, bem como código de renderização inserido. &lt;% %&gt; nuggets de código para executar código quando renderiza o modelo de exibição, e &lt;% = %&gt; nuggets de código executará o código contido dentro deles e, em seguida, renderizar o resultado ao fluxo de saída do modelo.

Podemos escrever código dentro de nosso modo de exibição que acessa o objeto de modelo "Jantar" que foi passado do nosso controlador usando uma propriedade de "Modelo" fortemente tipados. Visual Studio fornece nós com o intellisense de código completo ao acessar essa propriedade "Model" dentro do editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Vamos fazer alguns ajustes para que a fonte para o nosso modelo de exibição de detalhes final a aparência abaixo:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Quando podemos acessar o *"/ detalhes/jantares/1"* URL novamente ele será agora renderização como abaixo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementando o modelo de exibição "Index"

Agora, vamos implementar o modelo de exibição "Index" – que irá gerar uma listagem de jantares futuros. Tarefas pendentes isso podemos posicionar nosso cursor de texto dentro do método de ação de índice e, em seguida, com o botão direito clique e escolha o comando de menu "Adicionar exibição" (ou pressione Ctrl-M, Ctrl + V).

Na caixa de diálogo "Adicionar exibição" Vamos manter o modelo de exibição denominado "Index" e marque a caixa de seleção "Criar uma exibição fortemente tipada". Desta vez, escolheremos para automaticamente gerar um modelo de exibição de "Lista" e selecione "NerdDinner.Models.Dinner" como o tipo de modelo passado para o modo de exibição (que porque estamos indicou que estamos criando uma "lista" scaffold fará com que a caixa de diálogo Adicionar modo de exibição para supor que estamos passando uma sequência de objetos de jantar de nosso controlador para o modo de exibição):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Quando clicamos no botão "Adicionar", o Visual Studio criará um novo arquivo de modelo de exibição "Aspx" para que possamos dentro do nosso diretório "\Views\Dinners". Ele será "criar o scaffolding de" uma implementação inicial dentro dele que fornece uma listagem de tabela HTML dos jantares passamos para o modo de exibição.

Quando executamos o aplicativo e o acesso a *"Jantares /"* renderizará nossa lista de jantares de URL da seguinte forma:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

A solução de tabela acima nos dá um layout de grade de nossos dados jantar – que não não exatamente o que queremos para nosso consumidor voltado para a listagem de jantar. Podemos atualizar o modelo de exibição de aspx e modificá-lo para menos colunas de dados de lista e use uma &lt;ul&gt; elemento para renderizá-los em vez de uma tabela usando o código a seguir:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Estamos usando a palavra-chave de "var" dentro da instrução foreach acima conforme executamos um loop sobre cada jantar em nosso modelo. Aqueles familiarizados com c# 3.0 podem pensar que, usar "var" significa que o objeto de jantar é associação tardia. Em vez disso, significa que o compilador está usando inferência de tipo em relação a propriedade "Model" com rigidez de tipos (que é do tipo "IEnumerable&lt;Dinner&gt;") e compilar a variável local "Jantar" como um tipo de jantar – o que significa que podemos obter completos IntelliSense e procurando dentro de blocos de código de tempo de compilação:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Quando clicar em atualizar o */Dinners* URL em nosso navegador nossa exibição atualizada agora parece abaixo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Isso é uma aparência bem melhor – mas ainda não está totalmente lá. Nossa última etapa é habilitar os usuários finais a clicar jantares individuais na lista e ver detalhes sobre eles. Implementaremos isso por meio da renderização de elementos de hiperlink HTML que vincule o método de ação de detalhes no nosso DinnersController.

Podemos gerar esses hiperlinks dentro do nosso modo de exibição de índice em uma das duas maneiras. A primeira é criar manualmente o HTML &lt;uma&gt; elementos como abaixo, onde podemos incorporar &lt;% %&gt; bloqueia dentro de &lt;um&gt; elemento HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Uma abordagem alternativa que podemos usar é aproveitar o método de auxiliar interno "Html.ActionLink()" dentro do ASP.NET MVC que oferece suporte à criação por meio de programação um HTML &lt;um&gt; elemento que vincula-se para outro método de ação em um Controlador:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

O primeiro parâmetro para o método auxiliar Html.ActionLink() é o texto do link para exibir (no caso do título do jantar), o segundo parâmetro é o nome da ação de controlador que queremos gerar o link para (neste caso, o método de detalhes) e o terceiro parâmetro é um conjunto de parâmetros a serem enviados para a ação (implementada como um tipo anônimo com valores/nome de propriedade). Nesse caso, estamos especificando o parâmetro "id" do jantar queremos vincular a e como o roteamento de URL padrão de regra no ASP.NET MVC é "{Controller} / {Action} / {id}" o método auxiliar Html.ActionLink() gerará a seguinte saída:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Para nossa exibição aspx usaremos a abordagem de método auxiliar Html.ActionLink() e ter cada jantar no link para a URL apropriada de detalhes lista:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

E agora quando nos deparamos com o */Dinners* URL nossa lista de jantar a aparência abaixo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Quando clicamos em qualquer um dos jantares na lista vamos navegar para ver detalhes sobre ele:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Baseado em convenção de nomenclatura e a estrutura do diretório \Views

Por padrão, aplicativos ASP.NET MVC usam um estrutura de nomenclatura durante a resolução de modelos de exibição de diretório baseado em convenção. Isso permite que os desenvolvedores evitar ter que qualificar totalmente-um caminho de local ao fazer referência a exibições de dentro de uma classe de controlador. Por padrão ASP.NET MVC procurará o arquivo de modelo de exibição dentro de * \Views\[Nomedocontrolador]\* diretório sob o aplicativo.

Por exemplo, temos trabalhado na classe DinnersController – que faz referência explícita a três modelos de exibição: "Index", "Detalhes" e "NotFound". ASP.NET MVC por padrão procurará por essas exibições dentro de *\Views\Dinners* diretório sob nosso diretório raiz do aplicativo:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Observe acima como não há no momento, há três classes de controlador dentro do projeto (DinnersController, HomeController e AccountController – as duas últimas foram adicionadas por padrão, quando criamos o projeto), e há três (um para cada um de seus subdiretórios controlador) dentro do diretório \Views.

Exibições referenciadas nos controladores Home e contas de resolverá automaticamente seus modelos de exibição do respectivo *\Views\Home* e *\Views\Account* diretórios. O *\Views\Shared* subdiretório fornece uma maneira de armazenar modelos de exibição que são reutilizados em vários controladores dentro do aplicativo. Quando o ASP.NET MVC tenta resolver um modelo de exibição, ele primeiro verifica dentro de *\Views\[controlador]* diretório específico, e se ele não é possível localizar o modelo de exibição lá ele irá procurar no *\Views\ Compartilhado* directory.

Quando se trata de modelos de exibição individual de nomeação, a orientação recomendada é ter o modelo de exibição compartilham o mesmo nome que o método de ação que causou a renderizar. Por exemplo, acima nosso "Index" método de ação está usando o modo de exibição "Index" para renderizar o resultado de exibição e o método de ação "Detalhes" está usando o modo de exibição "Detalhes" para processar seus resultados. Isso facilita ver rapidamente o modelo que está associado com cada ação.

Os desenvolvedores não precisam especificar explicitamente o nome do modelo de exibição quando o modelo de exibição tem o mesmo nome que o método de ação que está sendo invocado no controlador. Podemos em vez disso, basta passar o objeto de modelo para o método auxiliar "View()" (sem especificar o nome de exibição) e ASP.NET MVC automaticamente irá inferir que queremos usar o *\Views\[Nomedocontrolador]\[ActionName]* modelo de exibição no disco para renderizá-lo.

Isso nos permite limpar um pouco o nosso código de controlador e evitar a duplicação do nome duas vezes em nosso código:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

O código acima é tudo o que é necessário para implementar um jantar listagem/detalhes boa experiência para o site.

#### <a name="next-step"></a>Próxima etapa

Agora temos um bom jantar criada de experiência de navegação.

Agora, vamos Habilitar suporte de edição de formulários de dados CRUD (Create, Read, Update, Delete).

> [!div class="step-by-step"]
> [Anterior](build-a-model-with-business-rule-validations.md)
> [Próximo](provide-crud-create-read-update-delete-data-form-entry-support.md)
