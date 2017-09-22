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
ms.openlocfilehash: 159377300d33e98393fd6705d0fec578f8f6b735
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="5323d-104">Introdução aos aplicativos de estilo com menor, Sass e fonte Awesome no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5323d-104">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="5323d-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5323d-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5323d-106">Os usuários de aplicativos da web têm expectativas cada vez mais altas quando se trata de estilo e a experiência geral.</span><span class="sxs-lookup"><span data-stu-id="5323d-106">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="5323d-107">Aplicativos web modernos frequentemente aproveitam avançadas ferramentas e estruturas para definir e gerenciar sua aparência de uma maneira consistente.</span><span class="sxs-lookup"><span data-stu-id="5323d-107">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="5323d-108">Estruturas como [inicialização](http://getbootstrap.com/) pode ir um longo caminho para definir um conjunto comum de opções de layout para sites da web e estilos.</span><span class="sxs-lookup"><span data-stu-id="5323d-108">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="5323d-109">No entanto, a maioria dos sites não trivial também se beneficiar de ser capaz de definir e manter estilos e arquivos de folhas de estilo em cascata com eficiência, bem como acesso fácil a imagem não ícones que ajudam a tornar a interface do site mais intuitiva.</span><span class="sxs-lookup"><span data-stu-id="5323d-109">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="5323d-110">É onde linguagens e ferramentas que dão suporte a [menos](http://lesscss.org/) e [Sass](http://sass-lang.com/), e bibliotecas, como [fonte Awesome](http://fontawesome.io/), entrar.</span><span class="sxs-lookup"><span data-stu-id="5323d-110">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="5323d-111">Idiomas de pré-processador de CSS</span><span class="sxs-lookup"><span data-stu-id="5323d-111">CSS preprocessor languages</span></span>

<span data-ttu-id="5323d-112">Idiomas que são compilados em outros idiomas, para melhorar a experiência de trabalhar com o idioma base são chamados de pré-processador.</span><span class="sxs-lookup"><span data-stu-id="5323d-112">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="5323d-113">Há dois pré-processadores populares de CSS: menor e Sass.</span><span class="sxs-lookup"><span data-stu-id="5323d-113">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="5323d-114">Esses pré-processadores adicionar recursos a CSS, como suporte para variáveis e regras aninhadas que melhorar a facilidade de manutenção de folhas de estilo grandes e complexas.</span><span class="sxs-lookup"><span data-stu-id="5323d-114">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="5323d-115">CSS como uma linguagem é muito básica, sem suporte a até mesmo algo simples, como variáveis e isso tende a fazer arquivos CSS repetitivas e inchada.</span><span class="sxs-lookup"><span data-stu-id="5323d-115">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="5323d-116">Adicionando recursos de idioma real de programação via pré-processadores pode ajudar a reduzir a duplicação e fornecer melhor organização de regras de estilo.</span><span class="sxs-lookup"><span data-stu-id="5323d-116">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="5323d-117">Visual Studio fornece suporte interno para ambos os menor e Sass, bem como as extensões que podem melhorar ainda mais a experiência de desenvolvimento ao trabalhar com esses idiomas.</span><span class="sxs-lookup"><span data-stu-id="5323d-117">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="5323d-118">Como um exemplo rápido de como pré-processadores podem melhorar a leitura e a manutenção de informações de estilo, considere este CSS:</span><span class="sxs-lookup"><span data-stu-id="5323d-118">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

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

<span data-ttu-id="5323d-119">Usando o menor, isso pode ser reescrito para eliminar a duplicação, usando um *mesclado* (chamada assim porque ele permite que você "mix" propriedades de uma classe ou conjunto de regras em outro):</span><span class="sxs-lookup"><span data-stu-id="5323d-119">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

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

## <a name="less"></a><span data-ttu-id="5323d-120">Menos</span><span class="sxs-lookup"><span data-stu-id="5323d-120">Less</span></span>

<span data-ttu-id="5323d-121">O CSS menos pré-processador é executado usando o Node. js.</span><span class="sxs-lookup"><span data-stu-id="5323d-121">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="5323d-122">Para instalar menor, use o Gerenciador de pacotes de nó (npm) em um prompt de comando (-g significa "global"):</span><span class="sxs-lookup"><span data-stu-id="5323d-122">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="5323d-123">Se você estiver usando o Visual Studio, você pode começar com menos adicionando um ou mais arquivos menos ao seu projeto e, em seguida, configurando Gulp (ou Grunt) para processá-los em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="5323d-123">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="5323d-124">Adicionar um *estilos* pasta ao seu projeto e, em seguida, adicione um novo menos um arquivo chamado *main.less* nesta pasta.</span><span class="sxs-lookup"><span data-stu-id="5323d-124">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Adicionar arquivo less](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="5323d-126">Depois de adicionado, a estrutura de pastas deve ser algo assim:</span><span class="sxs-lookup"><span data-stu-id="5323d-126">Once added, your folder structure should look something like this:</span></span>

![Estrutura de pastas](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="5323d-128">Agora você pode adicionar alguns estilos básicos para o arquivo, que será compilado em CSS e implantado na pasta wwwroot por vez.</span><span class="sxs-lookup"><span data-stu-id="5323d-128">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="5323d-129">Modificar *main.less* para incluir o conteúdo a seguir, que cria uma paleta de cores simples de uma única cor base.</span><span class="sxs-lookup"><span data-stu-id="5323d-129">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

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

<span data-ttu-id="5323d-130">`@base`e o outro @-prefixed itens são variáveis.</span><span class="sxs-lookup"><span data-stu-id="5323d-130">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="5323d-131">Cada um deles representa uma cor.</span><span class="sxs-lookup"><span data-stu-id="5323d-131">Each of them represents a color.</span></span> <span data-ttu-id="5323d-132">Exceto para `@base`, eles são definidos usando funções de cor: mais claro, mais escuro e rotação.</span><span class="sxs-lookup"><span data-stu-id="5323d-132">Except for `@base`, they are set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="5323d-133">Mais claro e escureça fazer muito bem o que você esperaria; rotação ajusta o matiz da cor por um número de graus (ao redor do círculo de cores).</span><span class="sxs-lookup"><span data-stu-id="5323d-133">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="5323d-134">O processador de menos é inteligente ignore variáveis que não são usados, por isso, para demonstrar como funcionam essas variáveis, precisamos usá-los em algum lugar.</span><span class="sxs-lookup"><span data-stu-id="5323d-134">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="5323d-135">As classes `.baseColor`, etc. demonstrará os valores calculados de cada uma das variáveis no arquivo CSS que é produzida.</span><span class="sxs-lookup"><span data-stu-id="5323d-135">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that is produced.</span></span>

### <a name="getting-started"></a><span data-ttu-id="5323d-136">Introdução</span><span class="sxs-lookup"><span data-stu-id="5323d-136">Getting started</span></span>

<span data-ttu-id="5323d-137">Criar um **arquivo de configuração npm** (*Package. JSON*) na pasta do projeto e editá-lo para fazer referência a `gulp` e `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="5323d-137">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

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

<span data-ttu-id="5323d-138">Instalar as dependências em um prompt de comando na pasta do projeto ou no Visual Studio **Solution Explorer** (**dependências > npm > restaurar os pacotes**).</span><span class="sxs-lookup"><span data-stu-id="5323d-138">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![Restaurar os pacotes do VS](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="5323d-140">Na pasta do projeto, criar um **Gulp arquivo de configuração** (*gulpfile.js*) para definir o processo automatizado.</span><span class="sxs-lookup"><span data-stu-id="5323d-140">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="5323d-141">Adicione uma variável na parte superior do arquivo para representar menor e uma tarefa para execução menor:</span><span class="sxs-lookup"><span data-stu-id="5323d-141">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

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

<span data-ttu-id="5323d-142">Abra o **Explorador do Executador de tarefas** (**exibição > outras janelas > Explorador do Executador de tarefas**).</span><span class="sxs-lookup"><span data-stu-id="5323d-142">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="5323d-143">Entre as tarefas, você verá uma nova tarefa denominada `less`.</span><span class="sxs-lookup"><span data-stu-id="5323d-143">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="5323d-144">Você talvez precise atualizar a janela.</span><span class="sxs-lookup"><span data-stu-id="5323d-144">You might have to refresh the window.</span></span>

<span data-ttu-id="5323d-145">Execute o `less` tarefa e você verá uma saída semelhante ao que é mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="5323d-145">Run the `less` task, and you see output similar to what is shown here:</span></span>

![menos executor de tarefas](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="5323d-147">O *wwwroot/css* pasta agora contém um novo arquivo, *Main*:</span><span class="sxs-lookup"><span data-stu-id="5323d-147">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![css principal criado](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="5323d-149">Abra *Main* e você verá algo parecido com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5323d-149">Open *main.css* and you see something like the following:</span></span>

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

<span data-ttu-id="5323d-150">Adicionar uma página HTML simples para o *wwwroot* pasta e referência *Main* para ver a paleta de cores em ação.</span><span class="sxs-lookup"><span data-stu-id="5323d-150">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

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

<span data-ttu-id="5323d-151">Você pode ver que o grau de 180 girar na `@base` usada para produzir `@background` resultou no disco de cores opostas cor de `@base`:</span><span class="sxs-lookup"><span data-stu-id="5323d-151">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![menos de teste de exemplo](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="5323d-153">Menor também oferece suporte para regras aninhadas, bem como as consultas de mídia aninhada.</span><span class="sxs-lookup"><span data-stu-id="5323d-153">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="5323d-154">Por exemplo, definindo hierarquias aninhadas como menus podem resultar em regras de CSS detalhadas como estes:</span><span class="sxs-lookup"><span data-stu-id="5323d-154">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

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

<span data-ttu-id="5323d-155">O ideal é todas as regras de estilo relacionados serão colocadas juntos dentro do arquivo CSS, mas na prática, não há nada impor essa regra exceto convenção e talvez os comentários do bloco.</span><span class="sxs-lookup"><span data-stu-id="5323d-155">Ideally all of the related style rules will be placed together within the CSS file, but in practice there is nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="5323d-156">Definir essas mesmas regras usando menos tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="5323d-156">Defining these same rules using Less looks like this:</span></span>

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

<span data-ttu-id="5323d-157">Observe que, nesse caso, todos os elementos subordinados do `nav` estão contidos dentro de seu escopo.</span><span class="sxs-lookup"><span data-stu-id="5323d-157">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="5323d-158">Não há nenhuma repetição dos elementos pai (`nav`, `li`, `a`), e a contagem de linha de total caiu também (embora alguns dos que é um resultado de colocar valores nas linhas da mesmas no segundo exemplo).</span><span class="sxs-lookup"><span data-stu-id="5323d-158">There is no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that is a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="5323d-159">Ele pode ser muito útil, organizacional, para ver todas as regras para um determinado elemento de interface do usuário em um escopo explicitamente associado, nesse caso definido do restante do arquivo de chaves.</span><span class="sxs-lookup"><span data-stu-id="5323d-159">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="5323d-160">O `&` sintaxe é um recurso menos seletor, com & que representa o pai de seletor atual.</span><span class="sxs-lookup"><span data-stu-id="5323d-160">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="5323d-161">Portanto, dentro do {...}</span><span class="sxs-lookup"><span data-stu-id="5323d-161">So, within the a {...}</span></span> <span data-ttu-id="5323d-162">bloco, `&` representa um `a` marca e, portanto, `&:link` é equivalente a `a:link`.</span><span class="sxs-lookup"><span data-stu-id="5323d-162">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="5323d-163">Consultas de mídia, extremamente úteis na criação de designs de resposta também podem contribuir muito para repetição e a complexidade em CSS.</span><span class="sxs-lookup"><span data-stu-id="5323d-163">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="5323d-164">Menor permite que as consultas de mídia a ser aninhada dentro de classes, para que a definição de classe inteira não precisa ser repetido em diferentes nível superior `@media` elementos.</span><span class="sxs-lookup"><span data-stu-id="5323d-164">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="5323d-165">Por exemplo, aqui está o CSS para um menu de resposta:</span><span class="sxs-lookup"><span data-stu-id="5323d-165">For example, here is CSS for a responsive menu:</span></span>

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

<span data-ttu-id="5323d-166">Isso pode ser melhor definido em menos como:</span><span class="sxs-lookup"><span data-stu-id="5323d-166">This can be better defined in Less as:</span></span>

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

<span data-ttu-id="5323d-167">Outro recurso de menor que já vimos é seu suporte para operações matemáticas, permitindo que os atributos de estilo a ser construído de variáveis predefinidas.</span><span class="sxs-lookup"><span data-stu-id="5323d-167">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="5323d-168">Isso facilita a atualização estilos relacionados muito mais fácil, já que a variável de base pode ser modificada e todos os valores dependentes alterar automaticamente.</span><span class="sxs-lookup"><span data-stu-id="5323d-168">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="5323d-169">Arquivos CSS, especialmente para grandes sites (e especialmente se as consultas de mídia estão sendo usadas), tendem a ter muito grandes ao longo do tempo, tornando a trabalhar com elas complicada.</span><span class="sxs-lookup"><span data-stu-id="5323d-169">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="5323d-170">Menos arquivos podem ser definidos separadamente, obtidas usando `@import` diretivas.</span><span class="sxs-lookup"><span data-stu-id="5323d-170">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="5323d-171">Menor também pode ser usado para importar CSS arquivos individuais, também, se desejado.</span><span class="sxs-lookup"><span data-stu-id="5323d-171">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="5323d-172">*Mixins* pode aceitar parâmetros e menor oferece suporte à lógica condicional na forma de protege mesclado, que fornecem uma maneira declarativa para definir quando determinados mixins entra em vigor.</span><span class="sxs-lookup"><span data-stu-id="5323d-172">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="5323d-173">Um uso comum para protege mesclado ajustar as cores com base em como a luz ou escuro a cor da fonte.</span><span class="sxs-lookup"><span data-stu-id="5323d-173">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="5323d-174">Dado um mesclado que aceita um parâmetro para a cor, um protetor mesclado pode ser usado para modificar o mesclado com base na cor:</span><span class="sxs-lookup"><span data-stu-id="5323d-174">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

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

<span data-ttu-id="5323d-175">Considerando nossa atual `@base` valor `#663333`, esse script menor produzirá o seguinte CSS:</span><span class="sxs-lookup"><span data-stu-id="5323d-175">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="5323d-176">Menor fornece uma série de recursos adicionais, mas isso deve dar uma ideia da energia desse idioma de pré-processamento.</span><span class="sxs-lookup"><span data-stu-id="5323d-176">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="5323d-177">Sass</span><span class="sxs-lookup"><span data-stu-id="5323d-177">Sass</span></span>

<span data-ttu-id="5323d-178">Sass é semelhante ao menos, fornecendo suporte para muitos dos mesmos recursos, mas com sintaxe ligeiramente diferente.</span><span class="sxs-lookup"><span data-stu-id="5323d-178">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="5323d-179">Ele é criado usando o Ruby, em vez de JavaScript, e portanto tem requisitos de instalação diferentes.</span><span class="sxs-lookup"><span data-stu-id="5323d-179">It is built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="5323d-180">O idioma Sass original não usou chaves ou ponto e vírgula, mas em vez disso, definida escopo usando o recuo e espaços em branco.</span><span class="sxs-lookup"><span data-stu-id="5323d-180">The original Sass language did not use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="5323d-181">Na versão 3 dos Sass, uma nova sintaxe foi introduzida, **SCSS** ("Sassy CSS").</span><span class="sxs-lookup"><span data-stu-id="5323d-181">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="5323d-182">SCSS é semelhante ao CSS que ignora os níveis de recuo e espaços em branco e, em vez disso, usa o ponto e vírgula e chaves.</span><span class="sxs-lookup"><span data-stu-id="5323d-182">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="5323d-183">Para instalar Sass, normalmente você deve primeiro instalar Ruby (pré-instalado no Mac) e, em seguida, execute:</span><span class="sxs-lookup"><span data-stu-id="5323d-183">To install Sass, typically you would first install Ruby (pre-installed on Mac), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="5323d-184">No entanto, se você estiver executando o Visual Studio, você pode começar com Sass em grande parte da mesma maneira como você faria com menos.</span><span class="sxs-lookup"><span data-stu-id="5323d-184">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="5323d-185">Abra *Package. JSON* e adicionar o pacote "gulp sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="5323d-185">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="5323d-186">Em seguida, modifique *gulpfile.js* para adicionar uma variável de sass e uma tarefa para compilar os arquivos Sass e colocar os resultados na pasta wwwroot:</span><span class="sxs-lookup"><span data-stu-id="5323d-186">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

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

<span data-ttu-id="5323d-187">Agora você pode adicionar o arquivo Sass *main2.scss* para o *estilos* pasta na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="5323d-187">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Adicionar arquivo scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="5323d-189">Abra *main2.scss* e adicione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5323d-189">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="5323d-190">Salve todos os arquivos.</span><span class="sxs-lookup"><span data-stu-id="5323d-190">Save all of your files.</span></span> <span data-ttu-id="5323d-191">Agora quando você atualiza **Explorador do Executador de tarefas**, você verá um `sass` tarefa.</span><span class="sxs-lookup"><span data-stu-id="5323d-191">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="5323d-192">Executá-lo e examinar o */wwwroot/css* pasta.</span><span class="sxs-lookup"><span data-stu-id="5323d-192">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="5323d-193">Agora há uma *main2.css* arquivo com esse conteúdo:</span><span class="sxs-lookup"><span data-stu-id="5323d-193">There is now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="5323d-194">Sass oferece suporte a aninhamento praticamente o mesmo foi que menor não, fornecer benefícios semelhantes.</span><span class="sxs-lookup"><span data-stu-id="5323d-194">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="5323d-195">Os arquivos podem ser divididos por função e incluídos usando a `@import` diretiva:</span><span class="sxs-lookup"><span data-stu-id="5323d-195">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="5323d-196">Sass oferece suporte a mixins, usando o `@mixin` palavra-chave para defini-los e `@include` para incluí-los, como neste exemplo de [sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="5323d-196">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="5323d-197">Além mixins, Sass também oferece suporte ao conceito de herança, permitindo que uma classe estender o outro.</span><span class="sxs-lookup"><span data-stu-id="5323d-197">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="5323d-198">Ele é conceitualmente semelhante a um mesclado, mas resulta em menos código CSS.</span><span class="sxs-lookup"><span data-stu-id="5323d-198">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="5323d-199">Ela é realizada usando o `@extend` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="5323d-199">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="5323d-200">Para testar mixins, adicione o seguinte ao seu *main2.scss* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5323d-200">To try out mixins, add the following to your *main2.scss* file:</span></span>

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

<span data-ttu-id="5323d-201">Examine a saída em *main2.css* depois de executar o `sass` tarefa no **Explorador do Executador de tarefas**:</span><span class="sxs-lookup"><span data-stu-id="5323d-201">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

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

<span data-ttu-id="5323d-202">Observe que todas as propriedades comuns do alerta mesclado são repetidas em cada classe.</span><span class="sxs-lookup"><span data-stu-id="5323d-202">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="5323d-203">O mesclado foi um bom trabalho de ajudar a eliminar a duplicação no tempo de desenvolvimento, mas ela ainda está criando um CSS com muita duplicação, resultando em maior do que arquivos CSS necessários - um possível problema de desempenho.</span><span class="sxs-lookup"><span data-stu-id="5323d-203">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="5323d-204">Agora, substitua o alerta mesclado com um `.alert` classe e altere `@include` para `@extend` (Lembre-se estender `.alert`, não `alert`):</span><span class="sxs-lookup"><span data-stu-id="5323d-204">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

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

<span data-ttu-id="5323d-205">Execute Sass mais uma vez e examine o CSS resultante:</span><span class="sxs-lookup"><span data-stu-id="5323d-205">Run Sass once more, and examine the resulting CSS:</span></span>

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

<span data-ttu-id="5323d-206">Agora as propriedades são definidas apenas como quantas vezes forem necessárias, e melhor CSS é gerado.</span><span class="sxs-lookup"><span data-stu-id="5323d-206">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="5323d-207">Sass também inclui funções e operações de lógica condicional, semelhantes ao menor.</span><span class="sxs-lookup"><span data-stu-id="5323d-207">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="5323d-208">Na verdade, os recursos de dois idiomas são muito semelhantes.</span><span class="sxs-lookup"><span data-stu-id="5323d-208">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="5323d-209">Menor ou Sass?</span><span class="sxs-lookup"><span data-stu-id="5323d-209">Less or Sass?</span></span>

<span data-ttu-id="5323d-210">Ainda há consenso se geralmente é melhor usar menor ou Sass (ou até mesmo se preferir o Sass original ou a sintaxe SCSS mais recente em Sass).</span><span class="sxs-lookup"><span data-stu-id="5323d-210">There is still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="5323d-211">Provavelmente, a decisão mais importante é **usar uma dessas ferramentas**, em vez de apenas mão-coding seus arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="5323d-211">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="5323d-212">Depois que você fez que de decisão, ambos sem e Sass são boas opções.</span><span class="sxs-lookup"><span data-stu-id="5323d-212">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="5323d-213">Fonte incrível</span><span class="sxs-lookup"><span data-stu-id="5323d-213">Font Awesome</span></span>

<span data-ttu-id="5323d-214">Além de pré-processadores CSS, outro excelente recurso para aplicativos web modernos de estilo é incrível de fonte.</span><span class="sxs-lookup"><span data-stu-id="5323d-214">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="5323d-215">Lista de fonte é um kit de ferramentas fornece mais de 500 ícones de SVG que podem ser usados livremente em seus aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="5323d-215">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="5323d-216">Ela foi originalmente projetada para trabalhar com inicialização, mas ele não tem nenhuma dependência no framework ou em qualquer biblioteca de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5323d-216">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="5323d-217">A maneira mais fácil começar com o incríveis fonte é adicionar uma referência a ele, usando seu local de rede (CDN) do fornecimento de conteúdo público:</span><span class="sxs-lookup"><span data-stu-id="5323d-217">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="5323d-218">Você também pode adicioná-lo ao seu projeto do Visual Studio adicionando-as "dependências" em *bower. JSON*:</span><span class="sxs-lookup"><span data-stu-id="5323d-218">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

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

<span data-ttu-id="5323d-219">Depois que você tem uma referência para o incríveis fonte em uma página, você pode adicionar ícones para o seu aplicativo aplicando fonte Awesome classes, normalmente é prefixados com "fa-", para os elementos embutidos HTML (como `<span>` ou `<i>`).</span><span class="sxs-lookup"><span data-stu-id="5323d-219">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="5323d-220">Por exemplo, você pode adicionar ícones de listas simples e menus usando código como este:</span><span class="sxs-lookup"><span data-stu-id="5323d-220">For example, you can add icons to simple lists and menus using code like this:</span></span>

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

<span data-ttu-id="5323d-221">Isso produz o seguinte no navegador - Observe o ícone ao lado de cada item:</span><span class="sxs-lookup"><span data-stu-id="5323d-221">This produces the following in the browser - note the icon beside each item:</span></span>

![ícones de lista](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="5323d-223">Você pode exibir uma lista completa de ícones disponíveis:</span><span class="sxs-lookup"><span data-stu-id="5323d-223">You can view a complete list of the available icons here:</span></span>

<span data-ttu-id="5323d-224">http://fontawesome.IO/icons/</span><span class="sxs-lookup"><span data-stu-id="5323d-224">http://fontawesome.io/icons/</span></span>

## <a name="summary"></a><span data-ttu-id="5323d-225">Resumo</span><span class="sxs-lookup"><span data-stu-id="5323d-225">Summary</span></span>

<span data-ttu-id="5323d-226">Aplicativos web modernos exigem cada vez mais responsivos, fluidos designs que são normal, intuitiva e fácil de usar de uma variedade de dispositivos.</span><span class="sxs-lookup"><span data-stu-id="5323d-226">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="5323d-227">Gerenciar a complexidade das folhas de estilo CSS necessárias para atingir essas metas, melhor é feito usando um tipo de pré-processador menos ou Sass.</span><span class="sxs-lookup"><span data-stu-id="5323d-227">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="5323d-228">Além disso, kits de ferramentas como o incríveis fonte rapidamente fornecem ícones conhecidos a menus de navegação textual e experiência de botões, melhorando o usuário do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5323d-228">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
