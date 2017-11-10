---
title: "Usando AngularJS para aplicativos de página única (SPAs)"
author: rick-anderson
description: Saiba como criar um aplicativo ASP.NET SPA estilo usando AngularJS
keywords: "Núcleo do ASP.NET, AngularJS, SPA"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccdf1625cdaf2400780500ac5ab86f41537964a9
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>Usando AngularJS para aplicativos de página única (SPAs) com o ASP.NET Core


Por [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) e [Scott Addie](https://scottaddie.com)

Neste artigo, você aprenderá como criar um aplicativo ASP.NET SPA estilo usando AngularJS.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-angularjs"></a>O que é AngularJS?

[AngularJS](https://angularjs.org/) é uma estrutura moderna de JavaScript do Google comumente usado para trabalhar com aplicativos de página única (SPAs). AngularJS aberta originado sob a licença do MIT, e o progresso de desenvolvimento de AngularJS pode ser seguido [seu repositório GitHub](https://github.com/angular/angular.js). A biblioteca é chamada Angular como HTML usa colchetes angulares formatada.

AngularJS não é uma biblioteca de manipulação de DOM como jQuery, mas ele usa um subconjunto de jQuery chamado jQLite. AngularJS baseia-se principalmente a declarativos atributos HTML que você pode adicionar a suas marcas HTML. Você pode tentar AngularJS em seu navegador usando o [site código escola](https://www.codeschool.com/courses/shaping-up-with-angularjs) ou [W3Schools site](https://www.w3schools.com/angular/).

Este artigo se concentra em AngularJS com algumas observações sobre onde Angular é título.

## <a name="getting-started"></a>Introdução

Para começar a usar o AngularJS em seu aplicativo ASP.NET, você deve instalá-lo como parte de seu projeto ou referenciá-lo a partir de uma rede de fornecimento de conteúdo (CDN).

### <a name="installation"></a>Instalação

Há várias maneiras de adicionar AngularJS ao seu aplicativo. Se você estiver iniciando um novo aplicativo web ASP.NET Core no Visual Studio, você pode adicionar AngularJS usando o interno [Bower](bower.md) suporte. Abra *bower. JSON*e adicione uma entrada para o `dependencies` propriedade:

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

Ao salvar o *bower. JSON* , Angular será instalado em seu projeto *wwwroot/lib* pasta. Além disso, ele será listado dentro do `Dependencies/Bower` pasta. Consulte a captura de tela abaixo.

![Gerenciador de soluções com o Project AngularJS](angular/_static/angular-solution-explorer.png)

Em seguida, adicione um `<script>` referência à parte inferior do `<body>` seção da página HTML ou *cshtml* de arquivo, como mostrado aqui:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

É recomendável que os aplicativos de produção utilizam CDNs para bibliotecas comuns como AngularJS. Você pode fazer referência a AngularJS de uma das várias CDNs, como este:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

Quando você tem uma referência para o *angular.js* arquivo de script, você está pronto para começar a usar o AngularJS nas páginas da web.

## <a name="key-components"></a>Componentes principais

AngularJS inclui um número de componentes principais, como *diretivas*, *modelos*, *repetidores*, *módulos*,  *controladores*, *componentes*, *roteador componente* e muito mais. Vamos examinar como esses componentes trabalham juntos para adicionar um comportamento a páginas da web.

### <a name="directives"></a>Diretivas

Usa AngularJS [diretivas](https://docs.angularjs.org/guide/directive) para estender o HTML com os elementos e atributos personalizados. Diretivas de AngularJS são definidas por meio de `data-ng-*` ou `ng-*` prefixos (`ng` é a abreviação de angular). Há dois tipos de diretivas do AngularJS:

   1. **Diretivas primitivo**: esses são predefinidos pela equipe de Angular e fazem parte do framework AngularJS.

   2. **Diretivas personalizadas**: esses são diretivas personalizadas que você pode definir.

Uma das diretivas primitivo usadas em todos os aplicativos de AngularJS é o `ng-app` diretiva, que inicializa o aplicativo AngularJS. Essa diretiva pode ser aplicada para o `<body>` marca ou a um elemento filho do corpo. Vejamos um exemplo em ação. Supondo que você está em um projeto do ASP.NET, você pode adicionar um arquivo HTML para o `wwwroot` pasta, ou adicionar uma nova ação de controlador e uma exibição associada. Nesse caso, adicionei um novo `Directives` método de ação `HomeController.cs`. A exibição associada é mostrada aqui:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

Para manter esses exemplos independentes um do outro, não estou usando o arquivo de layout compartilhada. Você pode ver que estamos decorado marca body com o `ng-app` diretiva para indicar esta página é um aplicativo AngularJS. O `{{2+2}}` é uma expressão de associação de dados Angular que você aprenderá mais sobre daqui a pouco. Aqui está o resultado se você executar este aplicativo:

![Diretiva Angular Simple](angular/_static/simple-directive.png)

Outros primitivas diretivas em AngularJS incluem:

`ng-controller`Determina qual controlador de JavaScript está associado ao modo de exibição.

`ng-model`Determina o modelo ao qual os valores das propriedades de um elemento HTML estão associados.

`ng-init`Usado para inicializar os dados de aplicativo na forma de uma expressão para o escopo atual.

`ng-if`Remove ou recria o determinado elemento HTML no DOM com base em truthiness da expressão fornecida.

`ng-repeat`Repete um determinado bloco de HTML em um conjunto de dados.

`ng-show`Mostra ou oculta o elemento HTML determinado de acordo com a expressão fornecida.

Para obter uma lista completa de todas as diretivas primitivo com suporte no AngularJS, consulte o [seção de diretiva de documentação no site de documentação do AngularJS](https://docs.angularjs.org/api/ng/directive).

### <a name="data-binding"></a>Associação de dados

Fornece AngularJS [associação de dados](https://docs.angularjs.org/guide/databinding) suporte de-prontos usando o `ng-bind` diretiva ou uma sintaxe de expressão de associação, como de dados `{{expression}}`. AngularJS dá suporte à associação de dados bidirecional onde os dados de um modelo são mantidos em sincronização com um modelo de exibição em todos os momentos. As alterações para o modo de exibição são refletidas automaticamente no modelo. Da mesma forma, as alterações no modelo são refletidas no modo de exibição.

Criar um arquivo HTML ou uma ação do controlador com um modo de exibição que acompanha denominado `Databinding`. Inclua o seguinte no modo de exibição:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

Observe que você pode exibir valores de modelo usando diretivas ou dados de associação (`ng-bind`). A página resultante deve ter esta aparência:

![Associação de dados Simple](angular/_static/simple-databinding.png)

### <a name="templates"></a>Modelos

[Modelos de](https://docs.angularjs.org/guide/templates) em AngularJS são apenas páginas em HTML decoradas com artefatos e diretivas de AngularJS. Um modelo em AngularJS é uma combinação de diretivas, expressões, filtros e controles que combinam com HTML para a exibição de formulário.

Adicionar outro modo de exibição para demonstrar os modelos e adicione o seguinte para ele:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

O modelo tem diretivas AngularJS como `ng-app`, `ng-init`, `ng-model` e sintaxe de expressão de associação de dados para associar o `personName` propriedade. Em execução no navegador, a exibição é semelhante a captura de tela abaixo:

![Exemplo de modelos simples 1](angular/_static/simple-templates-1.png)

Se você alterar o nome digitando-o no campo de entrada, você verá o texto ao lado do campo de entrada dinamicamente atualização, mostrando a associação de dados bidirecional Angular em ação.

![Exemplo de modelos simples 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>Expressões

[Expressões](https://docs.angularjs.org/guide/expression) AngularJS são trechos de código JavaScript que são gravados dentro de `{{ expression }}` sintaxe. Os dados a partir dessas expressões estão associados a HTML da mesma maneira que `ng-bind` diretivas. A principal diferença entre AngularJS expressões e expressões regulares do JavaScript é que AngularJS as expressões são avaliadas em relação a `$scope` objeto em AngularJS.

As expressões de AngularJS no exemplo abaixo ligação `personName` e um simple JavaScript calculado expressão:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

O exemplo em execução no navegador mostre o `personName` dados e os resultados do cálculo:

![Expressões simples](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>Repetidores

AngularJS de repetição é feita por meio de uma diretiva primitivo chamada `ng-repeat`. O `ng-repeat` diretiva repete um determinado elemento HTML em uma exibição sobre o tamanho de uma matriz de dados repetidos. Repetidores em AngularJS podem repetir em uma matriz de cadeias de caracteres ou objetos. Aqui está um exemplo de uso de repetição em uma matriz de cadeias de caracteres:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

O [diretiva repeat](https://docs.angularjs.org/api/ng/directive/ngRepeat) gera uma série de itens de lista em uma lista não ordenada, como você pode ver nas ferramentas de desenvolvedor mostradas nesta captura de tela:

![Exemplo de Repetidor](angular/_static/repeater.png)

Aqui está um exemplo que se repete em uma matriz de objetos. O `ng-init` diretiva estabelece um `names` matriz, onde cada elemento é um objeto que contém primeiro e último nome. O `ng-repeat` atribuição, `name in names`, gera um item de lista para cada elemento da matriz.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

Nesse caso a saída é igual ao exemplo anterior.

Angular fornece algumas diretivas adicionais que podem ajudar a fornecer um comportamento com base em onde o loop está em execução.

`$index`

Use `$index` no `ng-repeat` loop para determinar qual índice posicionar o loop no momento é no.

`$even` e `$odd`

Use `$even` no `ng-repeat` loop para determinar se o índice atual em seu loop é uma linha até mesmo indexada. Da mesma forma, use `$odd` para determinar se o índice atual é uma linha indexada ímpar.

`$first` e `$last`

Use `$first` no `ng-repeat` loop para determinar se o índice atual em seu loop é a primeira linha. Da mesma forma, use `$last` para determinar se o índice atual é a última linha.

Abaixo está um exemplo que mostra `$index`, `$even`, `$odd`, `$first`, e `$last` em ação:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

Aqui está a saída resultante:

![Exemplo de Repetidor 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`é um objeto JavaScript que atua como união entre o modo de exibição (modelo) e o controlador (explicado abaixo). Um modelo de exibição em AngularJS só conhece os valores anexados para o `$scope` objeto no controlador.

> [!NOTE]
> No mundo MVVM, o `$scope` objeto em AngularJS geralmente é definido como o ViewModel. A equipe do AngularJS refere-se para o `$scope` objeto como o modelo de dados. [Saiba mais sobre os escopos em AngularJS](https://docs.angularjs.org/guide/scope).

Abaixo está um exemplo simples que mostra como definir propriedades em `$scope` dentro de um arquivo separado do JavaScript, *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

Observe o `$scope` parâmetro passado para o controlador na linha 2. Este objeto é o que o modo de exibição conhece. Na linha 3, uma propriedade chamada "name" para "Jane Mary" está sendo configurado.

O que acontece quando uma determinada propriedade não é encontrada pela exibição? O modo de exibição definido a seguir se refere às propriedades de "nome" e "Idade":

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

Observe na linha 9 que estamos pedindo Angular para mostrar a propriedade "name" usando a sintaxe de expressão. Linha de 10, em seguida, faz referência a "Idade", uma propriedade que não existe. O exemplo de execução mostra o nome definido como "Mary Jane" e nada para idade. Propriedades ausentes são ignoradas.

![Exemplo de escopo](angular/_static/scope.png)

### <a name="modules"></a>Módulos

Um [módulo](https://docs.angularjs.org/guide/module) em AngularJS é uma coleção de controladores, serviços, diretivas, etc. O `angular.module()` chamada de função é usada para criar, registrar e recuperar os módulos em AngularJS. Todos os módulos, incluindo aqueles fornecidos pela equipe de AngularJS e bibliotecas de terceiros, devem ser registrados com o `angular.module()` função.

Abaixo está um trecho de código que mostra como criar um novo módulo no AngularJS. O primeiro parâmetro é o nome do módulo. O segundo parâmetro define dependências em outros módulos. Neste artigo, podemos ser exibida como transmitir essas dependências para um `angular.module()` chamada de método.

```javascript
var personApp = angular.module('personApp', []);
```

Use o `ng-app` diretiva para representar um módulo AngularJS na página. Para usar um módulo, atribuir o nome do módulo, `personApp` neste exemplo, para o `ng-app` diretiva em nosso modelo.

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>Controladores

[Controladores](https://docs.angularjs.org/guide/controller) AngularJS são o primeiro ponto de entrada para seu código. O `<module name>.controller()` chamada de função é usada para criar e registrar os controladores em AngularJS. O `ng-controller` diretiva é usada para representar um controlador AngularJS na página HTML. A função do controlador no Angular é definir o estado e o comportamento do modelo de dados (`$scope`). Controladores não devem ser usados para manipular o DOM diretamente.

Abaixo está um trecho de código que registra um novo controlador. O `personApp` variável no trecho faz referência a um módulo Angular, que é definido na linha 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

O modo de exibição usando o `ng-controller` diretiva atribui o nome do controlador:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

A página mostra "Mary" e "Jane" que correspondem do `firstName` e `lastName` propriedades anexadas para o `$scope` objeto:

![Exemplo de controlador](angular/_static/controllers.png)

### <a name="components"></a>Componentes

[Componentes](https://docs.angularjs.org/guide/component) em Angular 1.5. x permitir o encapsulamento e a capacidade de criar elementos HTML individuais. 1.4 Angular você obteria o mesmo recurso usando o método .directive().

Usando o método .component(), desenvolvimento é simplificado obter a funcionalidade da diretiva e o controlador. Outros benefícios incluem; isolamento de escopo, as práticas recomendadas são inerentes e migração para 2 Angular torna-se uma tarefa mais fácil. O `<module name>.component()` chamada de função é usada para criar e registrar componentes na AngularJS.

Abaixo está um trecho de código que registra um novo componente. O `personApp` variável no trecho faz referência a um módulo Angular, que é definido na linha 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

O modo de exibição onde podemos estiver exibindo o elemento HTML personalizado.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

O modelo associado usado pelo componente:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

A página mostra "Aftab" e "Ansari" que correspondem do `firstName` e `lastName` propriedades anexadas para o `vm` objeto:

![Exemplo de componentes](angular/_static/components.png)

### <a name="services"></a>Serviços

[Serviços](https://docs.angularjs.org/guide/services) em AngularJS normalmente são usados para código compartilhado abstraído em um arquivo que pode ser usado em todo o tempo de vida de um aplicativo Angular. Serviços são instanciados lentamente, o que significa que não haverá uma instância de um serviço, a menos que um componente que depende do serviço é usado. Fábricas são um exemplo de um serviço usado em aplicativos de AngularJS. Fábricas são criadas usando o `myApp.factory()` função chamada, onde `myApp` é o módulo.

Abaixo está um exemplo que mostra como usar as fábricas em AngularJS:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

Para chamar esta fábrica do controlador, transmita `personFactory` como um parâmetro para o `controller` função:

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>Usando serviços de falar com um ponto de extremidade REST

Abaixo está um exemplo de ponta a ponta usando serviços em AngularJS para interagir com um ponto de extremidade de API da Web do ASP.NET Core. O exemplo obtém os dados da API da Web e exibe os dados em um modelo de exibição. Vamos começar com o modo de exibição primeiro:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

Nesta exibição, temos um módulo Angular chamado `PersonsApp` e um controlador chamado `personController`. Estamos usando `ng-repeat` para iterar sobre a lista de pessoas. Podemos faz referência três arquivos JavaScript personalizados em linhas 17-19.

O *personApp.js* arquivo é usado para registrar o `PersonsApp` módulo; e a sintaxe é semelhante aos exemplos anteriores. Estamos usando o `angular.module` função para criar uma nova instância do módulo que trabalharemos com.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

Vamos dar uma olhada *personFactory.js*, abaixo. Estamos ligando para o módulo `factory` método para criar uma fábrica. Linha de 12 mostra o Angular interna `$http` recuperar informações sobre as pessoas de um serviço web do serviço.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

Em *personController.js*, estamos ligando para o módulo `controller` método para criar o controlador. O `$scope` do objeto `people` propriedade recebe os dados retornados do personFactory (linha 13).

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

Vamos dar uma olhada rápida a API da Web e o modelo por trás dele. O `Person` modelo é um POCO (objeto Plain Old CLR) com `Id`, `FirstName`, e `LastName` propriedades:

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

O `Person` retorna uma lista formatada em JSON de `Person` objetos:

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

Vamos ver o aplicativo em ação:

![Exibindo resultados REST controlador](angular/_static/rest-bound.png)

Você pode [exibir a estrutura do aplicativo no GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

> [!NOTE]
> Para obter mais informações sobre como estruturar AngularJS aplicativos, consulte [guia de estilo do John Papa Angular](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> Para criar o módulo AngularJS, controlador, fábrica, arquivos de diretiva e exibir facilmente, certifique-se fazer check-out do Hashimi Sayed [SideWaffle o pacote de modelo para o Visual Studio](http://sidewaffle.com/). Hashimi sayed é gerente de programas sênior da equipe do Web Visual Studio na Microsoft e SideWaffle modelos são considerados o padrão. No momento da redação deste artigo, SideWaffle está disponível para o Visual Studio 2012, 2013 e 2015.

### <a name="routing-and-multiple-views"></a>Roteamento e vários modos de exibição

AngularJS tem um provedor de rota interna para tratar SPA (aplicativo de página única) com base em navegação. Para trabalhar com roteamento em AngularJS, você deve adicionar o `angular-route` biblioteca usando o Bower. Você pode ver no [bower. JSON](#angular-bower-json) arquivo referenciado no início deste artigo que nós são já faz referência a ele em nosso projeto.

Depois de instalar o pacote, adicione a referência de script (*route.js angular*) ao modo de exibição.

Agora vamos dar o aplicativo de pessoa é construído e adicionar navegação a ele. Primeiro, faremos uma cópia do aplicativo, criando um novo `PeopleController` ação chamada `Spa` e correspondente `Spa.cshtml` exibição copiando o modo de exibição cshtml o `People` pasta. Adicione uma referência de script para `angular-route` (consulte a linha 11). Também adicionar uma `div` marcado com o `ng-view` diretiva (consulte a linha 6) como um espaço reservado para colocar os modos de exibição no. Vamos usar vários adicionais *. js* arquivos que são referenciados nas linhas de 13 a 16.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

Vamos dar uma olhada *personModule.js* arquivo para ver como podemos são instanciar o módulo com o roteamento. Passa `ngRoute` como uma biblioteca no módulo. Este módulo manipula o roteamento em nosso aplicativo.

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

O *personRoutes.js* arquivo, abaixo, define rotas com base no provedor de rota. Linhas 4 a 7 definem navegação efetivamente dizendo, quando uma URL com `/persons` é solicitada, usar um modelo chamado `partials/personlist` trabalhando `personListController`. Linhas de 8 a 11 indicam uma página de detalhes com um parâmetro de rota do `personId`. Se a URL não coincidir com um dos padrões, Angular assume como padrão o `/persons` exibição.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

O `personlist.html` arquivo é uma exibição parcial que contém apenas o HTML necessário para exibir a lista de pessoas.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

O controlador é definido usando o módulo `controller` funcionar em *personListController.js*.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

Se executar este aplicativo e navegue até o `people/spa#/persons` URL, veremos:

![Exibição de lista de pessoas](angular/_static/spa-persons.png)

Se, navegue até uma página de detalhes, por exemplo `people/spa#/persons/2`, veremos a exibição parcial detalhes:

![Exibição de detalhes da pessoa](angular/_static/spa-persons-2.png)

Você pode exibir o código-fonte completo e todos os arquivos não mostrados neste artigo em [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

### <a name="event-handlers"></a>Manipuladores de Eventos

Há um número de diretivas em AngularJS que adiciona recursos de manipulação de eventos para os elementos de entrada no seu HTML DOM. Abaixo está uma lista dos eventos que são criados em AngularJS.

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> Você pode adicionar seus próprios manipuladores de eventos usando o [diretivas personalizadas de recursos em AngularJS](https://docs.angularjs.org/guide/directive).

Vamos examinar como o `ng-click` evento está conectado. Criar um novo arquivo JavaScript chamado *eventHandlerController.js*e adicione o seguinte para ele:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

Observe o novo `sayName` funcionar em `eventHandlerController` na linha 5 acima. O método All está fazendo para agora está mostrando um alerta de JavaScript para o usuário com uma mensagem de boas-vinda.

O modo de exibição a seguir associa uma função de controlador para um evento AngularJS. A linha 9 tem um botão no qual o `ng-click` Angular diretiva foi aplicada. Ele chama nosso `sayName` função, que é anexada ao `$scope` objeto passado para este modo de exibição.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

O exemplo de execução demonstra que o controlador `sayName` função é chamada automaticamente quando o botão é clicado.

![Evento de clique](angular/_static/events.png)

Para obter mais detalhes sobre as diretivas de manipulador de eventos internos do AngularJS, certifique-se de cabeçalho para o [site de documentação](https://docs.angularjs.org/api/ng/directive/ngClick) do AngularJS.

## <a name="additional-resources"></a>Recursos adicionais

* [Documentos angulares](https://docs.angularjs.org)

* [Info 2 angular](https://angular.io/)
