---
title: "Estrutura Knockout. js MVVM no núcleo do ASP.NET"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: 87b4fdc86f6bb870ae0a8cc85688a549fd0740ac
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>Estrutura Knockout. js MVVM no núcleo do ASP.NET

Por [Steve Smith](http://ardalis.com)

Separação é uma biblioteca JavaScript popular que simplifica a criação de interfaces de usuário complexas com base em dados. Ele pode ser usado sozinho ou com outras bibliotecas, como jQuery. Sua finalidade principal é associar os elementos de interface do usuário para um modelo de dados definido como um objeto de JavaScript, de modo que quando as alterações são feitas para a interface do usuário, o modelo é atualizado e vice-versa. Knockout facilita o uso de um padrão Model-View-ViewModel (MVVM) no comportamento do cliente de um aplicativo web. Os dois principais conceitos um deve saber ao trabalhar com a implementação do MVVM do Knockout são observáveis e associações.

## <a name="getting-started"></a>Introdução

Separação é implantada como um único arquivo de JavaScript, para instalar e usá-lo é muito simples usando [bower](bower.md). Supondo que você já tiver [bower](bower.md) e [gulp](using-gulp.md) configurado, abra *bower. JSON* no seu ASP.NET Core do projeto e adicionar a dependência de separação, conforme mostrado aqui:

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

Com isso em vigor, você pode manualmente executar bower abrindo o Explorador do Executador de tarefas (na exibição ‣ outras janelas ‣ Explorador do Executador de tarefas) e, em seguida, em tarefas, clique em bower e selecione Executar. O resultado deve ser semelhante a este:

![bower knockout em execução no Explorador do Executador de tarefas](knockout/_static/bower-knockout.png)

Agora, se você olhar no seu projeto `wwwroot` pasta, você deve ver knockout instalado na pasta lib.

![Knockout instalado na pasta lib](knockout/_static/wwwroot-knockout.png)

Recomenda-se em seu ambiente de produção referência knockout por meio de uma rede de fornecimento de conteúdo ou CDN, pois isso aumenta a probabilidade de que os usuários já terá uma cópia armazenada em cache do arquivo e, portanto, não serão necessário baixá-lo em todos os. Knockout está disponível em vários CDNs, incluindo a Microsoft Ajax CDN, aqui:

[http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

Para incluir Knockout em uma página que irá usá-la, basta adicionar uma `<script>` elemento para referenciar o arquivo de onde você hospedará ele (com o seu aplicativo, ou por meio de um CDN):

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>Observáveis ViewModels e associação simples

Você já pode estar familiarizado com o uso de JavaScript para manipular elementos em uma página da web, via acesso direto ao DOM ou usando uma biblioteca como jQuery. Normalmente, esse tipo de comportamento é obtido escrevendo código para definir valores de elemento diretamente em resposta a determinadas ações do usuário. Com Knockout, uma abordagem declarativa é obtida em vez disso, por meio do qual os elementos na página estão associados a propriedades em um objeto. Em vez de escrever código para manipular elementos DOM, ações do usuário simplesmente interagem com o objeto ViewModel e Knockout cuida de garantir que os elementos da página são sincronizados.

Como um exemplo simples, considere a lista de página abaixo. Ele inclui um `<span>` elemento com um `data-bind` indicando que o conteúdo de texto deve ser associado aos authorName de atributo. Em seguida, em um bloco JavaScript viewModel um variável é definido com uma única propriedade, `authorName`, um conjunto de valores. Por fim, uma chamada para `ko.applyBindings` é feita, passando essa variável viewModel.

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

Quando exibido no navegador, o conteúdo de <span> elemento é substituído pelo valor na variável viewModel:

![associação simples de separação](knockout/_static/simple-binding-screenshot.png)

Agora temos trabalho simples associação unidirecional. Observe que isso no código que escrevemos JavaScript para atribuir um valor para o conteúdo do alcance. Se quisermos manipular o ViewModel, podemos ir um pouco mais e adicionar uma caixa de texto de entrada HTML e associar ao seu valor, como para:

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

Recarregar a página, podemos ver que esse valor, na verdade, está associado à caixa de entrada:

![associação de entrada de separação](knockout/_static/input-binding-screenshot.png)

No entanto, se alterarmos o valor na caixa de texto, o valor correspondente no `<span>` elemento não é alterado. Por quê?

O problema é que nada notificadas de `<span>` que ele precisa ser atualizado. Simplesmente atualizar ViewModel não sozinho suficientes, a menos que as propriedades do ViewModel são encapsuladas em um tipo especial. Precisamos usar **observables** no ViewModel para qualquer propriedade que precisam ter alterações atualizadas automaticamente à medida que eles ocorrem. Alterando o ViewModel usar `ko.observable("value")` em vez de apenas "valor", o ViewModel atualizará todos os elementos HTML que estão associados a seu valor sempre que ocorre uma alteração. Observe que as caixas de entrada não atualizar seu valor até que eles perdem foco, você não verá alterações associado elementos conforme você digita.

> [!NOTE]
> Adicionando suporte para a atualização dinâmica após cada keypress é simplesmente uma questão de adicionar `valueUpdate: "afterkeydown"` para o `data-bind` conteúdo do atributo. Você também pode obter esse comportamento usando `data-bind="textInput: authorName"` para obter atualizações de instantâneas de valores. 

Nosso viewModel, após a atualização para usar ko.observable:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Separação dá suporte a vários tipos diferentes de associações. Até agora, vimos como vincular a `text` e `value`. Você também pode associar a qualquer determinado atributo. Por exemplo, para criar um hiperlink com uma marca de âncora de `src` atributo pode ser associado ao viewModel. Knockout também dá suporte à associação a funções. Para demonstrar isso, vamos atualizar viewModel para incluir o identificador do twitter do autor e exibir o identificador do twitter como um link para a página do twitter do autor. Faremos isso em três etapas.

Primeiro, adicione o HTML para exibir o hiperlink, o que vamos mostrar entre parênteses após o nome do autor:

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

Em seguida, atualize o viewModel para incluir as propriedades twitterUrl e twitterAlias:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

Observe que agora é ainda não tiver atualizado twitterUrl para ir para a URL correta para este alias twitter – ele é simplesmente apontando twitter.com. Além disso, observe que estamos usando uma nova função de separação, `computed`, para twitterUrl. Essa é uma função observável que notificará quaisquer elementos de interface do usuário se ele for alterado. No entanto, para que ele tem acesso a outras propriedades no viewModel, precisamos alterar como estamos criando viewModel, de forma que cada propriedade é sua própria instrução.

A declaração de viewModel revisado é mostrada abaixo. Agora é declarada como uma função. Observe que cada propriedade é sua própria instrução agora, terminando com um ponto e vírgula. Além disso, observe que, para acessar o valor da propriedade twitterAlias, é necessário executá-lo, para que sua referência inclui ().

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

O resultado funciona conforme esperado no navegador:

![hiperlink de separação](knockout/_static/hyperlink-screenshot.png)

Knockout também dá suporte à associação a determinados eventos do elemento da interface do usuário, como o evento de clique. Isso permite que você facilmente e declarativamente vincular elementos de interface do usuário a funções dentro de viewModel do aplicativo. Como um exemplo simples, podemos adicionar um botão que, quando clicado, modifica twitterAlias do autor para ser todas em maiusculas.

Primeiro, podemos adicionar o botão, associação para o botão em eventos e referenciando o nome da função, vamos adicionar ao viewModel:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

Em seguida, adicione a função ao viewModel e conectá-lo para modificar o estado do viewModel. Observe que para definir um novo valor para a propriedade twitterAlias, podemos chamá-lo como um método e passar o novo valor.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

A execução do código e clicando no botão modifica o link exibido conforme o esperado:

![colocar em maiuscula de hiperlink](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>Fluxo de controle

Knockout inclui associações que podem executar operações condicionais e loop. Operações loop são especialmente úteis para listas de dados de associação a listas, menus e grades ou tabelas de interface do usuário. A associação de foreach irá iterar em uma matriz. Quando usado com uma matriz observável, ele atualizará automaticamente os elementos de interface do usuário quando itens são adicionados ou removidos da matriz, sem recriar todos os elementos na árvore de interface do usuário. O exemplo a seguir usa um novo viewModel que inclui uma matriz observável dos resultados de jogos. Ele é associado a uma tabela simples com duas colunas usando um `foreach` associação no `<tbody>` elemento. Cada `<tr>` elemento dentro do `<tbody>` será associada a um elemento da coleção gameResults.

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

Observe que neste momento estamos usando ViewModel com uma letra maiuscula "V" porque esperamos para construí-lo usando "nova" (na chamada applyBindings). Quando executada, a página resulta na seguinte saída:

![modelo de exibição de registro de separação](knockout/_static/record-screenshot.png)

Para demonstrar que a coleção observável está funcionando, vamos adicionar mais funcionalidade. Podemos incluem a capacidade de gravar os resultados de outro jogo ao ViewModel e, em seguida, adicione um botão e algumas interfaces do usuário para trabalhar com essa nova função.  Primeiro, vamos criar o método addResult:

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

Associar a este método em um botão usando o `click` associação:

```html
<button data-bind="click: addResult">Add New Result</button>
```

Abra a página no navegador e clique no botão algumas vezes, resultando em uma nova linha na tabela com cada clique:

![Adicionar os resultados](knockout/_static/record-addresult-screenshot.png)

Há algumas maneiras de suporte à adição de novos registros na interface de usuário, normalmente em linha ou de forma separada. Podemos facilmente modificar a tabela para usar as caixas de texto e dropdownlists para que tudo é editável. Alterar o `<tr>` elemento conforme mostrado:

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

Observe que `$root` refere-se à raiz ViewModel, que é onde as opções possíveis são expostas. `$data`refere-se de que o modelo atual está dentro de um determinado contexto - nesse caso, que ele se refere a um elemento individual da matriz resultChoices, cada um deles é uma cadeia de caracteres simple.

Com essa alteração, toda a grade fica editável:

![Grade editável](knockout/_static/editable-grid-screenshot.png)

Se não estivéssemos usando Knockout, podemos poderia obter tudo isso usando jQuery, mas provavelmente não seria ser quase tão eficiente. Knockout controla quais dados associados a itens no ViewModel correspondem quais elementos de interface do usuário e atualiza somente os elementos que precisam ser adicionados, removidos ou atualizados. Seria necessário esforço significativo para fazer isso nos usando jQuery ou manipulação direta do DOM, e mesmo se quiséssemos, em seguida exibir os resultados de agregação (como um registro de ganho perda) com base nos dados da tabela, é preciso percorrer a ele mais uma vez e analisar o Elementos HTML.  Com Knockout, exibir o registro de ganho perda é simples. Podemos executar cálculos em ViewModel em si e, em seguida, exibi-lo com uma associação de texto simples e um `<span>`.

Para criar a cadeia de caracteres do registro de ganho perda, podemos usar um observável computada. Observe que faz referência às propriedades observáveis nos ViewModel deve ser chamadas de função, caso contrário, eles não recuperará o valor do observável (ou seja, `gameResults()` não `gameResults` no código mostrado):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

Associar a essa função para um intervalo dentro do `<h1>` elemento na parte superior da página:

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

O resultado:

![Ganhos e perdas](knockout/_static/record-winloss-screenshot.png)

Adicionando linhas ou modificando o elemento selecionado na coluna de resultado de qualquer linha atualizará o registro mostrado na parte superior da janela.

Além de associação para valores, você também pode usar praticamente qualquer expressão JavaScript válido dentro de uma associação. Por exemplo, se um elemento de interface do usuário deve aparecer apenas em determinadas condições, como quando um valor excede um certo limite, você pode especificar isso logicamente dentro da expressão de associação:

```html
<div data-bind="visible: customerValue > 100"></div>
```

Isso `<div>` só ficará visível quando o customerValue estiver acima de 100.

## <a name="templates"></a>Modelos

Knockout tem suporte para modelos, para que você pode separar facilmente sua interface do usuário de seu comportamento ou carregar incrementalmente os elementos de interface do usuário em um grande aplicativo sob demanda. Podemos atualizar nosso exemplo anterior para tornar seu próprio modelo de cada linha simplesmente efetuando pull HTML out em um modelo e especificando o modelo por nome na chamada de associação de dados em `<tbody>`.

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Knockout também oferece suporte a outros mecanismos de aplicação de modelos, como a biblioteca de jQuery.tmpl e mecanismo de modelagem do Underscore.js.

## <a name="components"></a>Componentes

Componentes permitem organizar e reutilizar o código de interface do usuário, normalmente junto com os dados de ViewModel dos quais depende o código de interface do usuário. Para criar um componente, basta especificar o seu modelo e seu viewModel e dê a ele um nome. Isso é feito chamando `ko.components.register()`. Além de definir os modelos e viewmodel embutido, eles podem ser carregados de arquivos externos usando uma biblioteca como *require.js*, resultando em código muito limpo e eficiente.

## <a name="communicating-with-apis"></a>Comunicação com APIs

Knockout pode trabalhar com os dados no formato JSON. É uma maneira comum para recuperar e salvar dados usando o Knockout com jQuery, que oferece suporte a `$.getJSON()` função para recuperar dados e o `$.post()` método para enviar dados do navegador para um ponto de extremidade de API. É claro que, se você preferir um modo diferente para enviar e receber dados JSON, Knockout também funciona com ele.

## <a name="summary"></a>Resumo

Knockout fornece uma maneira simples e discreta para associar os elementos de interface do usuário para o estado atual do aplicativo cliente, definido em um ViewModel. Sintaxe de associação do Knockout usa o atributo de associação de dados, aplicado a elementos HTML que serão processados. Separação é capaz de renderizar e atualizar grandes conjuntos de dados, rastreando a elementos de interface do usuário com eficiência e processando apenas alterações afetados elementos. Aplicativos grandes podem dividir a lógica de interface do usuário usando modelos e componentes, que podem ser carregados sob demanda de arquivos externos. Atualmente versão 3, Knockout é uma biblioteca JavaScript estável que pode melhorar a aplicativos da web que exigem a interatividade de cliente avançado.
