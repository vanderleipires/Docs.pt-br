---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Usar o AJAX para implementar cenários de mapeamento | Microsoft Docs
author: microsoft
description: Etapa 11 mostra como integrar o suporte de mapeamento do AJAX em nosso aplicativo NerdDinner, permitindo que os usuários que estão criando, editando ou exibindo os jantares para ver o l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f7de23ca46e6dc00fe8075e28068a8b3f95d02cd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830601"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Usar o AJAX para implementar cenários de mapeamento
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 11 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 11 mostra como integrar o suporte de mapeamento do AJAX em nosso aplicativo NerdDinner, permitindo que os usuários que estão criando, editando ou exibindo os jantares para ver o local do jantar graficamente.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner etapa 11: Um mapa do AJAX a integração

Vamos agora fazer nosso aplicativo um pouco mais visualmente interessantes integrando o suporte de mapeamento do AJAX. Isso permitirá que os usuários que estão criando, editando ou exibindo os jantares para ver o local do jantar graficamente.

### <a name="creating-a-map-partial-view"></a>Criando uma exibição parcial do mapa

Vamos usar a funcionalidade de mapeamento em vários locais dentro do nosso aplicativo. Para manter nosso código DRY, vamos encapsular a funcionalidade comum dentro de um único modelo parcial que podemos usar novamente em várias ações do controlador e modos de exibição de mapa. Vamos nomear esta exibição parcial "map.ascx" e criá-lo dentro do diretório \Views\Dinners.

Podemos criar o map.ascx parcial clicando duas vezes no diretório \Views\Dinners e escolhendo Add -&gt;exibir comando de menu. Vamos nomear o modo de exibição "Map.ascx", verificá-lo como uma exibição parcial e indicar que vamos passá-lo em uma classe de modelo fortemente tipados "Jantar":

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Quando clicamos no botão "Adicionar" nosso modelo parcial será criado. Em seguida, atualizaremos o arquivo Map.ascx para ter o seguinte conteúdo:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

A primeira &lt;script&gt; pontos para a biblioteca de mapeamento do Microsoft Virtual Earth 6.2 de referência. A segunda &lt;script&gt; pontos em um arquivo map.js daqui a pouco, criaremos que encapsulará nossa lógica de mapeamento comum Javascript de referência. O &lt;div id = "theMap"&gt; elemento é o contêiner HTML que Virtual Earth usará para hospedar o mapa.

Em seguida, temos um incorporado &lt;script&gt; bloco que contém duas funções de JavaScript específicas para este modo de exibição. A primeira função usa o jQuery para transmissão para cima uma função que é executada quando a página está pronta para executar o script do lado do cliente. Ele chama uma função de auxiliar LoadMap() que vamos definir nossa Map.js arquivo de script para carregar o controle de mapa virtual earth. A segunda função é um manipulador de eventos de retorno de chamada que adiciona um pino ao mapa que identifica um local.

Observe como estamos usando um servidor &lt;% = %&gt; bloco dentro do bloco de script do lado do cliente para inserir a latitude e longitude de Dinner que desejamos mapear em JavaScript. Essa é uma técnica útil para gerar valores dinâmicos que podem ser usados pelo script do lado do cliente (sem exigir uma separada chamada AJAX para o servidor para recuperar os valores – que torna mais rápido). O &lt;% = %&gt; blocos serão executado quando o modo de exibição está sendo renderizada no servidor – e então a saída do HTML apenas terminarão com valores inseridos de JavaScript (por exemplo: latitude var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Criando uma biblioteca de utilitários Map.js

Agora, vamos criar o arquivo de Map.js que podemos usar para encapsular a funcionalidade de JavaScript para nosso mapa (e implementar os métodos LoadMap e LoadPin acima). Podemos fazer isso clicando com o diretório \Scripts dentro do nosso projeto e, em seguida, escolha o "Add -&gt;Novo Item" comando de menu, selecione o item de JScript e nomeie-a como "Map.js".

Abaixo está o código JavaScript, adicionaremos ao arquivo Map.js que irão interagir com o Virtual Earth para exibir nosso mapa e adicione os pinos de locais para ele para nosso jantares:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>A integração do mapa com Create e formulários de edição

Iremos agora integrar o suporte de mapa com nossos cenários existentes de criar e editar. A boa notícia é que isso é muito fácil pendentes e não requer que alterar qualquer um dos nosso código de controlador. Como nossos modos de exibição criar e editar compartilham uma exibição parcial de "DinnerForm" comum para implementar o interface do usuário do formulário de jantar, podemos adicionar o mapa em um só lugar e ter nossos cenários de criar e editar a usá-lo.

Tudo que precisamos de tarefas pendentes é abrir o modo de exibição parcial \Views\Dinners\DinnerForm.ascx e atualizá-lo para incluir nosso novo mapa parcial. Abaixo está a aparência de DinnerForm atualizada depois que o mapa é adicionado (Observação: os elementos de formulário HTML são omitidos do trecho de código abaixo para fins de brevidade):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

O DinnerForm parcial acima usa um objeto do tipo "DinnerFormViewModel" como seu tipo de modelo (porque ele precisa de um objeto de jantar, bem como um SelectList para preencher a dropdownlist de países). Nosso mapa parcial precisa apenas de um objeto do tipo "Jantar" como seu tipo de modelo e, portanto, quando podemos renderizar o mapa parcial estamos passando apenas a jantar subpropriedade de DinnerFormViewModel a ele:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

A função JavaScript, adicionamos a jQuery usa parcial para anexar a um evento de "desfoque" à caixa de texto "Address" HTML. Você provavelmente já ouviu de eventos de "foco" que são acionados quando um usuário clica ou guias em uma caixa de texto. O oposto é um evento de "desfoque" que é acionado quando um usuário sai de uma caixa de texto. O manipulador de eventos acima limpa os valores de caixa de texto de latitude e longitude, quando isso acontece e, em seguida, plota o novo local de endereço em nosso mapa. Um manipulador de eventos de retorno de chamada que definimos dentro do arquivo map.js atualizarão as caixas de texto de longitude e latitude em nosso formulário usando valores retornados pelo virtual earth com base no endereço que demos a ela.

E agora quando executar nosso aplicativo novamente e clique na guia "Host jantar", veremos um padrão mapear exibido juntamente com nossos elementos de formulário padrão do jantar:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Ao digitar um endereço e, em seguida, guia imediatamente, o mapa será atualizado dinamicamente para exibir a localização e nosso manipulador de eventos preencherá as caixas de texto de latitude/longitude com os valores de local:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Se salvamos o jantar novo e, em seguida, abra-o novamente para edição, estamos encontrará que o local do mapa é exibido quando a página for carregada:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Sempre que o campo de endereço for alterado, o mapa e as coordenadas de latitude/longitude serão atualizado.

Agora que o mapa exibe o local de jantar, também podemos alterar os campos de formulário de Latitude e Longitude sejam visíveis caixas de texto em vez disso, ser elementos ocultos (já que o mapa está atualizando-os automaticamente sempre que um endereço seja inserido). Tarefas pendentes isso voltaremos do usando o auxiliar HTML Html.TextBox() usando o método auxiliar Html.Hidden():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

E agora, nossos formulários são um pouco mais fácil de usar e evitar a exibição de latitude/longitude bruto (enquanto ainda armazená-los com cada jantar no banco de dados):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>A integração do mapa com a exibição de detalhes

Agora que temos o mapa integrado com nossos cenários de criar e editar, vamos também integrá-lo com o nosso cenário de detalhes. Tudo que precisamos de tarefas pendentes é chamar &lt;% Html.RenderPartial("map"); %&gt; dentro da exibição de detalhes.

Abaixo está o que o código-fonte para a exibição de detalhes completo (com a integração do mapa) se parece com:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

E agora quando um usuário navega para uma URL de /Dinners/detalhes / [id] ele verá detalhes sobre o jantar, o local do jantar no mapa (completo com um pino que, quando colocado em cima exibe o título do jantar e o endereço dele), e ter um vínculo de AJAX com RSVP fo r-lo:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementando a pesquisa de localização em nosso banco de dados e o repositório

Para finalizar a nossa implementação de AJAX, vamos adicionar um mapa para a home page do aplicativo que permite que os usuários procurem graficamente jantares próximo a eles.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Vamos começar com a implementação de suporte dentro de nossa camada de repositório de banco de dados e para executar com eficiência uma pesquisa de radius baseados na localização de jantares. Poderíamos usar o novo [recursos geoespaciais do SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) implementar isso, ou como alternativa, podemos usar uma abordagem de função do SQL Gary Dryden discutido no artigo aqui: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) e Rob Conery publicou no blog sobre o uso com o LINQ to SQL aqui: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Para implementar essa técnica, podemos será abrir o "Gerenciador de servidores" dentro do Visual Studio, selecione o banco de dados do NerdDinner e, em seguida, clique duas vezes no subnó "funções" nele e optar por criar uma novo "função de valor escalar":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Em seguida, colar a seguinte função DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Em seguida, criaremos uma nova função com valor de tabela no SQL Server, que chamaremos de "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Essa função de tabela "NearestDinners" usa a função de auxiliar DistanceBetween para retornar todos os jantares 100 milhas de latitude e longitude podemos fornecê-lo:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Para chamar essa função, vai primeiro abrir o designer do LINQ to SQL clicando duas vezes no arquivo NerdDinner.dbml dentro do nosso diretório \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Podemos será, em seguida, arraste as funções NearestDinners e DistanceBetween o LINQ ao designer SQL, que fará com que eles sejam adicionados como métodos em nossa LINQ para SQL NerdDinnerDataContext classe:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Em seguida, é possível expor um método de consulta "FindByLocation" em nossa classe DinnerRepository que usa a função NearestDinner para retornar futuros jantares são 100 milhas do local especificado:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementando um método de ação de pesquisa do AJAX com base em JSON

Agora, implementaremos um método de ação do controlador que tira proveito do novo método de repositório FindByLocation() para retornar uma lista de dados de Dinner que podem ser usados para popular um mapa de volta. Teremos que esse método de ação retornar novamente os jantar dados em um formato JSON (JavaScript Object Notation), de modo que ele pode ser facilmente manipulado usando JavaScript no cliente.

Para implementar isso, vamos criar uma nova classe "SearchController" clicando duas vezes no diretório \Controllers e escolhendo Add -&gt;comando de menu do controlador. Em seguida, implementaremos um método de ação de "SearchByLocation" dentro da nova classe de SearchController como abaixo:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Internamente, o método de ação do SearchController SearchByLocation chama o método de FindByLocation no DinnerRespository para obter uma lista dos próximos jantares. Em vez de retornar os objetos de jantar diretamente ao cliente, no entanto, ele retorna objetos JsonDinner. A classe JsonDinner expõe um subconjunto das propriedades de jantar (por exemplo: por razões de segurança, ele não divulga os nomes das pessoas que têm RSVP'd em um jantar). Ele também inclui uma propriedade RSVPCount que não existe no jantar – e que é dinamicamente calculado pela contagem do número de objetos RSVP associados com uma refeição específica.

Em seguida, usamos o método auxiliar Json() na classe base do controlador para retornar a sequência de jantares usando um formato com fio baseado em JSON. JSON é um formato de texto padrão para representar estruturas de dados simples. Abaixo está um exemplo de uma lista formatada em JSON de dois objetos JsonDinner aparência quando retornado do nosso método de ação:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Chamar o método baseado em JSON AJAX usando jQuery

Agora estamos prontos para atualizar a home page do aplicativo NerdDinner para usar o método de ação do SearchController SearchByLocation. Tarefas pendentes isso, vamos abrir o modelo de exibição /Views/Home/Index.aspx e atualizá-lo para ter uma caixa de texto, botão de pesquisa, nosso mapa e uma &lt;div&gt; elemento chamado dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Em seguida, adicionamos duas funções de JavaScript à página:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

A primeira função de JavaScript carrega o mapa quando a página for carregada. O segundo JavaScript função conecta um JavaScript clique o manipulador de eventos no botão de pesquisa. Quando o botão é pressionado, ele chama a função de JavaScript FindDinnersGivenLocation() que adicionaremos nosso arquivo Map.js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Essa função FindDinnersGivenLocation() chama o mapa. Find () no controle do Virtual Earth para centralizá-la no local de inserido. Quando o serviço de mapa virtual earth retorna, o mapa. Método Find () chama o método de retorno de chamada de callbackUpdateMapDinners que passamos a ele como o argumento final.

O método callbackUpdateMapDinners() é onde o trabalho real é feito. Ele usa o método de auxiliares de $.post() do jQuery para executar uma chamada AJAX ao método de ação SearchByLocation() do nosso SearchController – passá-lo a latitude e longitude do mapa centralizado recentemente. Ele define uma função embutida que será chamada quando o método auxiliar $.post() for concluída, e os resultados de jantar formatada em JSON retornados do método de ação será passado usando uma variável chamada "jantares" de SearchByLocation(). Ele faz um foreach ao longo de cada jantar retornada e usa a latitude do jantar e a longitude e outras propriedades para adicionar um novo pin no mapa. Ele também adiciona uma entrada de jantar à lista HTML de jantares à direita do mapa. Ele, em seguida, fios para cima um evento de passagem para anotações e a lista HTML para que os detalhes sobre o jantar são exibidos quando um usuário passa o mouse sobre eles:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

E agora quando executar o aplicativo e visite a página inicial, verá um mapa. Quando inserimos o nome de uma cidade, o mapa exibirá os jantares futuros próximo a ele:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Passar o mouse sobre um jantar exibirá detalhes sobre ele.

Clicando no título de jantar na bolha ou no lado direito da lista de HTML navegará-nos para o jantar – que, em seguida, pode, opcionalmente, CONFIRME para o:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Próxima etapa

Agora, implementamos toda a funcionalidade de aplicativo do nosso aplicativo NerdDinner. Vamos agora observar como podemos pode habilitar unidade automatizado teste dele.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-deliver-dynamic-updates.md)
> [Próximo](enable-automated-unit-testing.md)
