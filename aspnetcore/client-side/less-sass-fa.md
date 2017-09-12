---
title: "Menor, Sass e fonte Awesome no núcleo do ASP.NET"
author: ardalis
description: "Saiba como usar menos, Sass e fonte incrível em aplicativos do ASP.NET Core."
keywords: "ASP.NET Core, menos Sass, fonte Awesome, pré-processadores"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 94c988f9-95fd-425d-b37e-7f846598c6d4
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 128612eb2f7c6c8fdd0cc01f10b8e522df46dcf6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a>Introdução aos aplicativos de estilo com menor, Sass e fonte Awesome no núcleo do ASP.NET

Por [Steve Smith](https://ardalis.com/)

Os usuários de aplicativos da web têm expectativas cada vez mais altas quando se trata de estilo e a experiência geral. Aplicativos web modernos frequentemente aproveitam avançadas ferramentas e estruturas para definir e gerenciar sua aparência de uma maneira consistente. Estruturas como [inicialização](http://getbootstrap.com/) pode ir um longo caminho para definir um conjunto comum de opções de layout para sites da web e estilos. No entanto, a maioria dos sites não trivial também se beneficiar de ser capaz de definir e manter estilos e arquivos de folhas de estilo em cascata com eficiência, bem como acesso fácil a imagem não ícones que ajudam a tornar a interface do site mais intuitiva. É onde linguagens e ferramentas que dão suporte a [menos](http://lesscss.org/) e [Sass](http://sass-lang.com/), e bibliotecas, como [fonte Awesome](http://fontawesome.io/), entrar.

## <a name="css-preprocessor-languages"></a>Idiomas de pré-processador de CSS

Idiomas que são compilados em outros idiomas, para melhorar a experiência de trabalhar com o idioma base são chamados de pré-processador. Há dois pré-processadores populares de CSS: menor e Sass.  Esses pré-processadores adicionar recursos a CSS, como suporte para variáveis e regras aninhadas que melhorar a facilidade de manutenção de folhas de estilo grandes e complexas. CSS como uma linguagem é muito básica, sem suporte a até mesmo algo simples, como variáveis e isso tende a fazer arquivos CSS repetitivas e inchada. Adicionando recursos de idioma real de programação via pré-processadores pode ajudar a reduzir a duplicação e fornecer melhor organização de regras de estilo. Visual Studio fornece suporte interno para ambos os menor e Sass, bem como as extensões que podem melhorar ainda mais a experiência de desenvolvimento ao trabalhar com esses idiomas.

Como um exemplo rápido de como pré-processadores podem melhorar a leitura e a manutenção de informações de estilo, considere este CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Usando o menor, isso pode ser reescrito para eliminar a duplicação, usando um *mesclado* (chamada assim porque ele permite que você "mix" propriedades de uma classe ou conjunto de regras em outro):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>Menos

O CSS menos pré-processador é executado usando o Node. js. Para instalar menor, use o Gerenciador de pacotes de nó (npm) em um prompt de comando (-g significa "global"):

```console
npm install -g less
```

Se você estiver usando o Visual Studio, você pode começar com menos adicionando um ou mais arquivos menos ao seu projeto e, em seguida, configurando Gulp (ou Grunt) para processá-los em tempo de compilação. Adicionar um *estilos* pasta ao seu projeto e, em seguida, adicione um novo menos um arquivo chamado *main.less* nesta pasta.

![Adicionar arquivo less](less-sass-fa/_static/add-less-file.png)

Depois de adicionado, a estrutura de pastas deve ser algo assim:

![Estrutura de pastas](less-sass-fa/_static/folder-structure.png)

Agora você pode adicionar alguns estilos básicos para o arquivo, que será compilado em CSS e implantado na pasta wwwroot por vez.

Modificar *main.less* para incluir o conteúdo a seguir, que cria uma paleta de cores simples de uma única cor base.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base`e o outro @-prefixed itens são variáveis. Cada um deles representa uma cor. Exceto para `@base`, eles são definidos usando funções de cor: mais claro, mais escuro e rotação. Mais claro e escureça fazer muito bem o que você esperaria; rotação ajusta o matiz da cor por um número de graus (ao redor do círculo de cores). O processador de menos é inteligente ignore variáveis que não são usados, por isso, para demonstrar como funcionam essas variáveis, precisamos usá-los em algum lugar. As classes `.baseColor`, etc. demonstrará os valores calculados de cada uma das variáveis no arquivo CSS que é produzida.

### <a name="getting-started"></a>Introdução

Criar um **arquivo de configuração npm** (*Package. JSON*) na pasta do projeto e editá-lo para fazer referência a `gulp` e `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Instalar as dependências em um prompt de comando na pasta do projeto ou no Visual Studio **Solution Explorer** (**dependências > npm > restaurar os pacotes**).

```console
npm install
```

![Restaurar os pacotes do VS](less-sass-fa/_static/restore-packages.png)

Na pasta do projeto, criar um **Gulp arquivo de configuração** (*gulpfile.js*) para definir o processo automatizado.  Adicione uma variável na parte superior do arquivo para representar menor e uma tarefa para execução menor:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Abra o **Explorador do Executador de tarefas** (**exibição > outras janelas > Explorador do Executador de tarefas**). Entre as tarefas, você verá uma nova tarefa denominada `less`. Você talvez precise atualizar a janela.

Execute o `less` tarefa e você verá uma saída semelhante ao que é mostrado aqui:

![menos executor de tarefas](less-sass-fa/_static/less-task-runner.png)

O *wwwroot/css* pasta agora contém um novo arquivo, *Main*:

![css principal criado](less-sass-fa/_static/main-css-created.png)

Abra *Main* e você verá algo parecido com o seguinte:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Adicionar uma página HTML simples para o *wwwroot* pasta e referência *Main* para ver a paleta de cores em ação.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

Você pode ver que o grau de 180 girar na `@base` usada para produzir `@background` resultou no disco de cores opostas cor de `@base`:

![menos de teste de exemplo](less-sass-fa/_static/less-test-screenshot.png)

Menor também oferece suporte para regras aninhadas, bem como as consultas de mídia aninhada. Por exemplo, definindo hierarquias aninhadas como menus podem resultar em regras de CSS detalhadas como estes:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

O ideal é todas as regras de estilo relacionados serão colocadas juntos dentro do arquivo CSS, mas na prática, não há nada impor essa regra exceto convenção e talvez os comentários do bloco.

Definir essas mesmas regras usando menos tem esta aparência:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Observe que, nesse caso, todos os elementos subordinados do `nav` estão contidos dentro de seu escopo. Não há nenhuma repetição dos elementos pai (`nav`, `li`, `a`), e a contagem de linha de total caiu também (embora alguns dos que é um resultado de colocar valores nas linhas da mesmas no segundo exemplo). Ele pode ser muito útil, organizacional, para ver todas as regras para um determinado elemento de interface do usuário em um escopo explicitamente associado, nesse caso definido do restante do arquivo de chaves.

O `&` sintaxe é um recurso menos seletor, com & que representa o pai de seletor atual. Portanto, dentro do {...} bloco, `&` representa um `a` marca e, portanto, `&:link` é equivalente a `a:link`.

Consultas de mídia, extremamente úteis na criação de designs de resposta também podem contribuir muito para repetição e a complexidade em CSS. Menor permite que as consultas de mídia a ser aninhada dentro de classes, para que a definição de classe inteira não precisa ser repetido em diferentes nível superior `@media` elementos. Por exemplo, aqui está o CSS para um menu de resposta:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

Isso pode ser melhor definido em menos como:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Outro recurso de menor que já vimos é seu suporte para operações matemáticas, permitindo que os atributos de estilo a ser construído de variáveis predefinidas. Isso facilita a atualização estilos relacionados muito mais fácil, já que a variável de base pode ser modificada e todos os valores dependentes alterar automaticamente.

Arquivos CSS, especialmente para grandes sites (e especialmente se as consultas de mídia estão sendo usadas), tendem a ter muito grandes ao longo do tempo, tornando a trabalhar com elas complicada. Menos arquivos podem ser definidos separadamente, obtidas usando `@import` diretivas. Menor também pode ser usado para importar CSS arquivos individuais, também, se desejado.

*Mixins* pode aceitar parâmetros e menor oferece suporte à lógica condicional na forma de protege mesclado, que fornecem uma maneira declarativa para definir quando determinados mixins entra em vigor. Um uso comum para protege mesclado ajustar as cores com base em como a luz ou escuro a cor da fonte. Dado um mesclado que aceita um parâmetro para a cor, um protetor mesclado pode ser usado para modificar o mesclado com base na cor:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Considerando nossa atual `@base` valor `#663333`, esse script menor produzirá o seguinte CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Menor fornece uma série de recursos adicionais, mas isso deve dar uma ideia da energia desse idioma de pré-processamento.

## <a name="sass"></a>Sass

Sass é semelhante ao menos, fornecendo suporte para muitos dos mesmos recursos, mas com sintaxe ligeiramente diferente. Ele é criado usando o Ruby, em vez de JavaScript, e portanto tem requisitos de instalação diferentes. O idioma Sass original não usou chaves ou ponto e vírgula, mas em vez disso, definida escopo usando o recuo e espaços em branco. Na versão 3 dos Sass, uma nova sintaxe foi introduzida, **SCSS** ("Sassy CSS"). SCSS é semelhante ao CSS que ignora os níveis de recuo e espaços em branco e, em vez disso, usa o ponto e vírgula e chaves.

Para instalar Sass, normalmente você deve primeiro instalar Ruby (pré-instalado no Mac) e, em seguida, execute:

```console
gem install sass
```

No entanto, se você estiver executando o Visual Studio, você pode começar com Sass em grande parte da mesma maneira como você faria com menos. Abra *Package. JSON* e adicionar o pacote "gulp sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

Em seguida, modifique *gulpfile.js* para adicionar uma variável de sass e uma tarefa para compilar os arquivos Sass e colocar os resultados na pasta wwwroot:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Agora você pode adicionar o arquivo Sass *main2.scss* para o *estilos* pasta na raiz do projeto:

![Adicionar arquivo scss](less-sass-fa/_static/add-scss-file.png)

Abra *main2.scss* e adicione o seguinte:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Salve todos os arquivos. Agora quando você atualiza **Explorador do Executador de tarefas**, você verá um `sass` tarefa. Executá-lo e examinar o */wwwroot/css* pasta. Agora há uma *main2.css* arquivo com esse conteúdo:

```css
body {
    background-color: #CC0000;
}
```

Sass oferece suporte a aninhamento praticamente o mesmo foi que menor não, fornecer benefícios semelhantes. Os arquivos podem ser divididos por função e incluídos usando a `@import` diretiva:

```sass
@import 'anotherfile';
```

Sass oferece suporte a mixins, usando o `@mixin` palavra-chave para defini-los e `@include` para incluí-los, como neste exemplo de [sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Além mixins, Sass também oferece suporte ao conceito de herança, permitindo que uma classe estender o outro. Ele é conceitualmente semelhante a um mesclado, mas resulta em menos código CSS. Ela é realizada usando o `@extend` palavra-chave. Para testar mixins, adicione o seguinte ao seu *main2.scss* arquivo:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Examine a saída em *main2.css* depois de executar o `sass` tarefa no **Explorador do Executador de tarefas**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Observe que todas as propriedades comuns do alerta mesclado são repetidas em cada classe. O mesclado foi um bom trabalho de ajudar a eliminar a duplicação no tempo de desenvolvimento, mas ela ainda está criando um CSS com muita duplicação, resultando em maior do que arquivos CSS necessários - um possível problema de desempenho.

Agora, substitua o alerta mesclado com um `.alert` classe e altere `@include` para `@extend` (Lembre-se estender `.alert`, não `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Execute Sass mais uma vez e examine o CSS resultante:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Agora as propriedades são definidas apenas como quantas vezes forem necessárias, e melhor CSS é gerado.

Sass também inclui funções e operações de lógica condicional, semelhantes ao menor. Na verdade, os recursos de dois idiomas são muito semelhantes.

## <a name="less-or-sass"></a>Menor ou Sass?

Ainda há consenso se geralmente é melhor usar menor ou Sass (ou até mesmo se preferir o Sass original ou a sintaxe SCSS mais recente em Sass). Provavelmente, a decisão mais importante é **usar uma dessas ferramentas**, em vez de apenas mão-coding seus arquivos CSS. Depois que você fez que de decisão, ambos sem e Sass são boas opções.

## <a name="font-awesome"></a>Fonte incrível

Além de pré-processadores CSS, outro excelente recurso para aplicativos web modernos de estilo é incrível de fonte. Lista de fonte é um kit de ferramentas fornece mais de 500 ícones de SVG que podem ser usados livremente em seus aplicativos web. Ela foi originalmente projetada para trabalhar com inicialização, mas ele não tem nenhuma dependência no framework ou em qualquer biblioteca de JavaScript.

A maneira mais fácil começar com o incríveis fonte é adicionar uma referência a ele, usando seu local de rede (CDN) do fornecimento de conteúdo público:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

Você também pode adicioná-lo ao seu projeto do Visual Studio adicionando-as "dependências" em *bower. JSON*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Depois que você tem uma referência para o incríveis fonte em uma página, você pode adicionar ícones para o seu aplicativo aplicando fonte Awesome classes, normalmente é prefixados com "fa-", para os elementos embutidos HTML (como `<span>` ou `<i>`).  Por exemplo, você pode adicionar ícones de listas simples e menus usando código como este:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

Isso produz o seguinte no navegador - Observe o ícone ao lado de cada item:

![ícones de lista](less-sass-fa/_static/list-icons-screenshot.png)

Você pode exibir uma lista completa de ícones disponíveis:

http://fontawesome.IO/icons/

## <a name="summary"></a>Resumo

Aplicativos web modernos exigem cada vez mais responsivos, fluidos designs que são normal, intuitiva e fácil de usar de uma variedade de dispositivos. Gerenciar a complexidade das folhas de estilo CSS necessárias para atingir essas metas, melhor é feito usando um tipo de pré-processador menos ou Sass. Além disso, kits de ferramentas como o incríveis fonte rapidamente fornecem ícones conhecidos a menus de navegação textual e experiência de botões, melhorando o usuário do seu aplicativo.
