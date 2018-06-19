---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Usar o AJAX para implementar o mapeamento de cenários | Microsoft Docs
author: microsoft
description: Etapa 11 mostra como integrar o suporte de mapeamento de AJAX em nosso aplicativo NerdDinner, permitindo que os usuários que estão criando, editando ou exibindo jantares para ver o l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872709"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Usar o AJAX para implementar cenários de mapeamento
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 11 da livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 11 mostra como integrar o suporte de mapeamento de AJAX em nosso aplicativo NerdDinner, permitindo que os usuários que estão criando, editando ou exibindo jantares para ver o local de uma refeição o graficamente.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner etapa 11: Integrar um mapa de AJAX

Agora faremos nosso aplicativo um pouco mais visualmente interessantes integrando o suporte de mapeamento de AJAX. Isso permitirá que os usuários que estão criando, editando ou exibindo jantares para ver o local de uma refeição o graficamente.

### <a name="creating-a-map-partial-view"></a>Criando uma exibição parcial do mapa

Vamos usar a funcionalidade de mapeamento em vários locais em nosso aplicativo. Para manter o código de teste, será encapsulam a funcionalidade comum de mapa em um único modelo parcial que podemos usar novamente em várias ações do controlador e modos de exibição. Vamos nomear essa exibição parcial "map.ascx" e criá-lo no diretório \Views\Dinners.

Podemos criar o map.ascx parcial clicando duas vezes no diretório \Views\Dinners e escolhendo Adicionar -&gt;exibir comando de menu. Vamos nomeie a exibição "Map.ascx", verifique-o como uma exibição parcial e indicar que vamos passá-lo para uma classe de modelo fortemente tipado "refeição":

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Quando, clique no botão "Adicionar" nosso modelo parcial será criado. Em seguida, vamos atualizar o arquivo de Map.ascx para que o conteúdo a seguir:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

A primeira &lt;script&gt; referência aponta para a biblioteca de mapeamento do Microsoft Virtual Earth 6.2. A segunda &lt;script&gt; pontos para um arquivo map.js logo criaremos que encapsulará nossa lógica de mapeamento de Javascript comuns de referência. O &lt;div id = "theMap"&gt; elemento é o contêiner HTML que Virtual Earth será usado para hospedar o mapa.

Em seguida, temos inserida &lt;script&gt; bloco que contém duas funções JavaScript específicas para este modo de exibição. A primeira função usa jQuery para conexão a uma função que é executado quando a página está pronta para executar o script do lado do cliente. Ele chama uma função auxiliar de LoadMap() que vamos definir nosso Map.js arquivo de script para carregar o controle de mapa virtual earth. A segunda função é um manipulador de eventos de retorno de chamada que adiciona um pino ao mapa que identifica um local.

Observe como estamos usando um lado do servidor &lt;% = %&gt; bloco dentro do bloco de script do lado do cliente para inserir a latitude e longitude de uma refeição que desejarmos mapear para o JavaScript. Isso é uma técnica útil para gerar valores dinâmicos que podem ser usados pelo script do lado do cliente (sem a necessidade de uma separada chamada AJAX para o servidor para recuperar os valores – que torna mais rápido). O &lt;% = %&gt; blocos serão executado quando o modo de exibição é de renderização no servidor – e portanto a saída do HTML apenas acabará com valores inseridos de JavaScript (por exemplo: latitude var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Criar uma biblioteca de utilitário Map.js

Agora vamos criar o arquivo Map.js que podemos usar para encapsular a funcionalidade de JavaScript para o mapa (e implementar os métodos LoadMap e LoadPin acima). Podemos fazer isso clicando no diretório \Scripts em nosso projeto e, em seguida, escolha o "Add -&gt;Novo Item" comando de menu, selecione o item de JScript e nomeie-a como "Map.js".

Abaixo está o código JavaScript, adicionaremos ao arquivo Map.js que irão interagir com o Virtual Earth para exibir o mapa e adicione pins locais para ele para nosso jantares:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrando o mapa de como criar e editar formulários

Agora podemos será integrar o suporte do mapa com nossos cenários existentes de criar e editar. A boa notícia é que isso é muito fácil tarefas e não requer a alteração de qualquer nosso código do controlador. Como os modos de criar e editar compartilham uma exibição parcial de "DinnerForm" comuns para implementar o interface do usuário do formulário de refeição, podemos adicionar o mapa em um único local e ter nossos cenários criar e editar a usá-lo.

Tudo o que precisamos fazer é abrir a exibição parcial \Views\Dinners\DinnerForm.ascx e atualizá-lo para incluir o novo mapa parcial. Abaixo está a aparência de DinnerForm atualizada depois que o mapa é adicionado (Observação: os elementos de formulário HTML são omitidos do trecho de código abaixo para fins de brevidade):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

O DinnerForm parcial acima usa um objeto do tipo "DinnerFormViewModel" como seu tipo de modelo (porque precisa tanto um objeto de uma refeição, bem como um SelectList para preencher a dropdownlist de países). Nosso mapa parcial precisa apenas de um objeto do tipo "Refeição" como seu tipo de modelo, e portanto quando podemos renderizar o mapa parcial, passa apenas a propriedade de subseção refeição de DinnerFormViewModel a ele:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

A função JavaScript adicionamos para o jQuery parcial usa para anexar a um evento de "desfoque" para a caixa de texto "Address" HTML. Você provavelmente ouviu de guias ou eventos de "foco" que são acionados quando um usuário clica em uma caixa de texto. O oposto é um evento de "desfoque" que é acionado quando um usuário sai de uma caixa de texto. O manipulador de eventos acima limpa os valores de latitude e longitude de texto quando isso acontece e, em seguida, plota o novo local de endereço em nosso mapa. Um manipulador de eventos de retorno de chamada que são definidos no arquivo map.js atualizarão as caixas de texto de longitude e latitude em nosso formulário usando valores retornados pelo virtual earth com base no endereço que é deu a ele.

E agora quando executar nosso aplicativo novamente e clique na guia "Host refeição", que veremos um padrão mapear exibido junto com os elementos de formulário refeição padrão:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Ao digitar um endereço e, em seguida, guia ausente, o mapa será atualizado dinamicamente para exibir o local e nosso manipulador de eventos preencherá as caixas de texto de latitude/longitude com os valores do local:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Se salvar uma refeição novo e, em seguida, abra-o novamente para edição, descobriremos que o local do mapa é exibido quando a página for carregada:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Toda vez que o campo de endereço é alterado, o mapa e as coordenadas de latitude/longitude serão atualizado.

Agora que o mapa exibe o local de uma refeição, também podemos alterar os campos de formulário de Latitude e Longitude sejam visíveis caixas de texto a ser elementos ocultos (desde que o mapa está atualizando-os automaticamente cada vez que um endereço seja inserido). Fazer isso podemos alternará do usando o auxiliar HTML Html.TextBox() usando o método auxiliar Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

E agora os formulários são um pouco mais fácil de usar e evitar a exibição a latitude/longitude bruta (enquanto ainda armazená-los com cada uma refeição no banco de dados):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrando o mapa com o modo de exibição de detalhes

Agora que temos o mapa integrado com nossos cenários de criar e editar, vamos também integrá-lo ao nosso cenário de detalhes. Tudo o que precisamos fazer é chamar &lt;% Html.RenderPartial("map"); %&gt; dentro da exibição de detalhes.

Abaixo está a aparência do código-fonte para o modo de exibição de detalhes concluído (com a integração do mapa):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

E agora quando um usuário navega para uma URL de /Dinners/detalhes / [id] ele verá detalhes sobre a refeição, o local de uma refeição no mapa (concluída com um pino que, quando colocado em cima exibe o título do refeição e o endereço dele), e ter um link de AJAX para RSVP fo r-lo:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementando a pesquisa do local em nosso banco de dados e um repositório

Para finalizar a implementação de AJAX, vamos adicionar um mapa para a home page do aplicativo que permite que os usuários pesquisem graficamente jantares próximo a eles.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Vamos começar com a implementação de suporte em nossa camada de repositório e banco de dados para executar com eficiência jantares procure radius com base no local. Podemos usar o novo [recursos geoespaciais do SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) para implementar isso, ou como alternativa, pode usar uma abordagem de função do SQL Gary Dryden discutida no artigo aqui: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) e Rob Conery publicou no blog sobre como usar com LINQ to SQL aqui: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Para implementar essa técnica, podemos será abrir o "Gerenciador de servidores" dentro do Visual Studio, selecione NerdDinner banco de dados e, em seguida, clique no nó "funções" sub nele e optar por criar uma novo "função de valor escalar":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Colaremos, em seguida, a função DistanceBetween a seguir:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Em seguida, vamos criar uma nova função com valor de tabela no SQL Server, que chamaremos de "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Essa função de tabela "NearestDinners" usa a função auxiliar DistanceBetween para retornar todos os jantares dentro de 100 milhas de latitude e longitude, fornecê-lo:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Para chamar esta função, deverá primeiro abrimos o LINQ to SQL designer clicando duas vezes no arquivo NerdDinner.dbml em nosso diretório \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Podemos será, em seguida, arraste as funções NearestDinners e DistanceBetween LINQ ao designer SQL, o que fará com que a ser adicionado como métodos em nosso LINQ para SQL NerdDinnerDataContext classe:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Em seguida, é possível expor um método de consulta "FindByLocation" em nossa classe DinnerRepository que usa a função NearestDinner para retornar futuros jantares que estão dentro de 100 milhas do local especificado:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementando um método de ação de pesquisa de AJAX com base em JSON

Agora, implementaremos um método de ação do controlador que aproveita o novo método de repositório FindByLocation() para retornar uma lista de dados de uma refeição que podem ser usados para preencher um mapa de volta. Teremos que este método de ação retornar novamente os dados de uma refeição em um formato JSON (JavaScript Object Notation) para que ele possa ser facilmente manipulado usando JavaScript no cliente.

Para implementar isso, vamos criar uma nova classe de "SearchController" clicando duas vezes no diretório \Controllers e escolhendo Adicionar -&gt;comando de menu do controlador. Em seguida, implementaremos um método de ação de "SearchByLocation" dentro da nova classe de SearchController como abaixo:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Internamente, o método de ação do SearchController SearchByLocation chama o método de FindByLocation no DinnerRespository para obter uma lista de jantares próximos. Em vez de retornar os objetos de uma refeição diretamente para o cliente, no entanto, em vez disso, retorna objetos JsonDinner. A classe JsonDinner expõe um subconjunto das propriedades de uma refeição (por exemplo: por razões de segurança, ele não revelar os nomes das pessoas que tenham confirmado a presença de uma refeição). Ele também inclui uma propriedade RSVPCount que não existe uma refeição – e que é dinamicamente calculado pela contagem do número de objetos RSVP associados com uma refeição específica.

Estamos, em seguida, usando o método auxiliar Json() na classe base do controlador para retornar a sequência de jantares usando um formato de transmissão com base em JSON. JSON é um formato de texto padrão para a representação de estruturas de dados simples. Abaixo está um exemplo de uma lista formatada em JSON de dois objetos JsonDinner aparência quando retornado do método da ação:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Chamar o método de AJAX com base em JSON usando jQuery

Agora você está pronto para atualizar a página inicial do aplicativo para usar o método de ação do SearchController SearchByLocation NerdDinner. Fazer isso, vamos abrir o modelo de exibição /Views/Home/Index.aspx e atualizá-lo para ter uma caixa de texto, o botão de pesquisa, o mapa e um &lt;div&gt; elemento chamado dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Podemos adicionar duas funções JavaScript para a página:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

A primeira função JavaScript carrega o mapa quando a página for carregada pela primeira vez. Os cabos de função JavaScript segundo backup de um JavaScript clique manipulador de eventos do botão de pesquisa. Quando o botão é pressionado, ele chama a função de JavaScript FindDinnersGivenLocation() que vamos adicionar nossa Map.js arquivo:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Essa função FindDinnersGivenLocation() chama o mapa. Find() no controle Virtual Earth para centralizar-no local digitado. Quando o serviço de mapa virtual earth retorna, o mapa. Método Find() invoca o método de retorno de chamada callbackUpdateMapDinners que passamos como o argumento final.

O método callbackUpdateMapDinners() é onde o trabalho real. Ele usa o método de auxiliares de $.post() do jQuery para executar uma chamada AJAX para o método de ação SearchByLocation() do nosso SearchController – passando a latitude e longitude do mapa recentemente centralizado. Define uma função embutida que será chamada quando o método auxiliar $.post() for concluída, e os resultados de uma refeição formatada em JSON retornados a SearchByLocation() método de ação será passado usando uma variável chamada "jantares". Ele, em seguida, faz um foreach sobre cada uma refeição retornada e usa longitude e latitude da refeição e outras propriedades para adicionar um novo pin no mapa. Ele também adiciona uma entrada de uma refeição à lista HTML de jantares à direita do mapa. Ele, em seguida, fios a um evento de foco de anotações e a lista HTML para que os detalhes sobre a refeição sejam exibidos quando um usuário passa sobre eles:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

E agora quando executar o aplicativo e visite a página inicial, verá um mapa. Quando podemos inserir o nome de uma cidade mapa exibirá os jantares futuros próximo a ela:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Focalizar uma refeição exibirá detalhes sobre ele.

Clicando no título de uma refeição em bolha ou no lado direito da lista de HTML navegará conosco para refeição – que é, em seguida, pode opcionalmente RSVP para:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Próxima etapa

Agora, implementamos toda a funcionalidade do aplicativo do nosso aplicativo NerdDinner. Vamos agora examinar como é possível habilitar automatizada unidade teste dele.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-deliver-dynamic-updates.md)
> [Próximo](enable-automated-unit-testing.md)
