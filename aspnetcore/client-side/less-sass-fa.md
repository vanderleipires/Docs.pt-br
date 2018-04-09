---
title: Less, Sass e fonte Awesome no ASP.NET Core
author: ardalis
description: Saiba como usar Less, Sass e fonte Awesome em aplicativos ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/less-sass-fa
ms.openlocfilehash: 3bb1c9006f8633485a420b52b5fa9b91b1875cc5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="14eae-103">Less, Sass e fonte Awesome no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14eae-103">Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="14eae-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="14eae-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="14eae-105">Os usuários de aplicativos da web têm expectativas cada vez mais altas quando se trata de estilo e a experiência geral.</span><span class="sxs-lookup"><span data-stu-id="14eae-105">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="14eae-106">Aplicativos web modernos frequentemente aproveitam avançadas ferramentas e estruturas para definir e gerenciar sua aparência de uma maneira consistente.</span><span class="sxs-lookup"><span data-stu-id="14eae-106">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="14eae-107">Estruturas como [inicialização](http://getbootstrap.com/) pode ir um longo caminho para definir um conjunto comum de opções de layout para sites da web e estilos.</span><span class="sxs-lookup"><span data-stu-id="14eae-107">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="14eae-108">No entanto, a maioria dos sites não trivial também se beneficiar de ser capaz de definir e manter estilos e arquivos de folhas de estilo em cascata com eficiência, bem como acesso fácil a imagem não ícones que ajudam a tornar a interface do site mais intuitiva.</span><span class="sxs-lookup"><span data-stu-id="14eae-108">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="14eae-109">É onde linguagens e ferramentas que dão suporte a [menos](http://lesscss.org/) e [Sass](http://sass-lang.com/), e bibliotecas, como [fonte Awesome](http://fontawesome.io/), entrar.</span><span class="sxs-lookup"><span data-stu-id="14eae-109">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="14eae-110">Idiomas de pré-processador de CSS</span><span class="sxs-lookup"><span data-stu-id="14eae-110">CSS preprocessor languages</span></span>

<span data-ttu-id="14eae-111">Idiomas que são compilados em outros idiomas, para melhorar a experiência de trabalhar com o idioma base são chamados de pré-processador.</span><span class="sxs-lookup"><span data-stu-id="14eae-111">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="14eae-112">Há dois pré-processadores populares de CSS: menor e Sass.</span><span class="sxs-lookup"><span data-stu-id="14eae-112">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="14eae-113">Esses pré-processadores adicionar recursos a CSS, como suporte para variáveis e regras aninhadas que melhorar a facilidade de manutenção de folhas de estilo grandes e complexas.</span><span class="sxs-lookup"><span data-stu-id="14eae-113">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="14eae-114">CSS como uma linguagem é muito básica, sem suporte a até mesmo algo simples, como variáveis e isso tende a fazer arquivos CSS repetitivas e inchada.</span><span class="sxs-lookup"><span data-stu-id="14eae-114">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="14eae-115">Adicionando recursos de idioma real de programação via pré-processadores pode ajudar a reduzir a duplicação e fornecer melhor organização de regras de estilo.</span><span class="sxs-lookup"><span data-stu-id="14eae-115">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="14eae-116">Visual Studio fornece suporte interno para ambos os menor e Sass, bem como as extensões que podem melhorar ainda mais a experiência de desenvolvimento ao trabalhar com esses idiomas.</span><span class="sxs-lookup"><span data-stu-id="14eae-116">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="14eae-117">Como um exemplo rápido de como pré-processadores podem melhorar a leitura e a manutenção de informações de estilo, considere este CSS:</span><span class="sxs-lookup"><span data-stu-id="14eae-117">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

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

<span data-ttu-id="14eae-118">Usando Less, isso pode ser reescrito para eliminar a duplicação, usando um *mixin* (chamada assim porque ele permite que você "misture" propriedades de uma classe ou conjunto de regras em outra):</span><span class="sxs-lookup"><span data-stu-id="14eae-118">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

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

## <a name="less"></a><span data-ttu-id="14eae-119">Less</span><span class="sxs-lookup"><span data-stu-id="14eae-119">Less</span></span>

<span data-ttu-id="14eae-120">O pré-processador CSS less é executado usando o Node. js.</span><span class="sxs-lookup"><span data-stu-id="14eae-120">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="14eae-121">Para instalar less, use o Gerenciador de pacotes de nó (npm) em um prompt de comando (-g significa "global"):</span><span class="sxs-lookup"><span data-stu-id="14eae-121">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="14eae-122">Se você estiver usando o Visual Studio, você pode começar com less adicionando um ou mais arquivos less ao seu projeto e, em seguida, configurando Gulp (ou Grunt) para processá-los em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="14eae-122">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="14eae-123">Adicione uma pasta *estilos* ao seu projeto e, em seguida, adicione um novo arquivo less chamado *main.less* nesta pasta.</span><span class="sxs-lookup"><span data-stu-id="14eae-123">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Adicionar arquivo less](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="14eae-125">Depois de adicionado, a estrutura de pastas deve ser algo assim:</span><span class="sxs-lookup"><span data-stu-id="14eae-125">Once added, your folder structure should look something like this:</span></span>

![Estrutura de pastas](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="14eae-127">Agora você pode adicionar alguns estilos básicos para o arquivo, que será compilado em CSS e implantado na pasta wwwroot por vez.</span><span class="sxs-lookup"><span data-stu-id="14eae-127">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="14eae-128">Modificar *main.less* para incluir o conteúdo a seguir, que cria uma paleta de cores simples de uma única cor base.</span><span class="sxs-lookup"><span data-stu-id="14eae-128">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

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

<span data-ttu-id="14eae-129">`@base` e o outro @-prefixed itens são variáveis.</span><span class="sxs-lookup"><span data-stu-id="14eae-129">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="14eae-130">Cada um deles representa uma cor.</span><span class="sxs-lookup"><span data-stu-id="14eae-130">Each of them represents a color.</span></span> <span data-ttu-id="14eae-131">Exceto para `@base`, eles são definidos usando funções de cor: mais claro, mais escuro e rotação.</span><span class="sxs-lookup"><span data-stu-id="14eae-131">Except for `@base`, they're set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="14eae-132">Mais claro e escureça fazer muito bem o que você esperaria; rotação ajusta o matiz da cor por um número de graus (ao redor do círculo de cores).</span><span class="sxs-lookup"><span data-stu-id="14eae-132">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="14eae-133">O processador de menos é inteligente ignore variáveis que não são usados, por isso, para demonstrar como funcionam essas variáveis, precisamos usá-los em algum lugar.</span><span class="sxs-lookup"><span data-stu-id="14eae-133">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="14eae-134">As classes `.baseColor`, etc. demonstrará os valores calculados de cada uma das variáveis no arquivo CSS que é produzida.</span><span class="sxs-lookup"><span data-stu-id="14eae-134">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that's produced.</span></span>

### <a name="get-started"></a><span data-ttu-id="14eae-135">Introdução</span><span class="sxs-lookup"><span data-stu-id="14eae-135">Get started</span></span>

<span data-ttu-id="14eae-136">Criar um **arquivo de configuração npm** (*Package. JSON*) na pasta do projeto e editá-lo para fazer referência a `gulp` e `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="14eae-136">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

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

<span data-ttu-id="14eae-137">Instalar as dependências em um prompt de comando na pasta do projeto ou no Visual Studio **Solution Explorer** (**dependências > npm > restaurar os pacotes**).</span><span class="sxs-lookup"><span data-stu-id="14eae-137">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![Restaurar os pacotes do VS](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="14eae-139">Na pasta do projeto, criar um **Gulp arquivo de configuração** (*gulpfile.js*) para definir o processo automatizado.</span><span class="sxs-lookup"><span data-stu-id="14eae-139">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="14eae-140">Adicione uma variável na parte superior do arquivo para representar less e uma tarefa para execução less:</span><span class="sxs-lookup"><span data-stu-id="14eae-140">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

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

<span data-ttu-id="14eae-141">Abra o **Explorador do Executador de tarefas** (**exibição > outras janelas > Explorador do Executador de tarefas**).</span><span class="sxs-lookup"><span data-stu-id="14eae-141">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="14eae-142">Entre as tarefas, você verá uma nova tarefa denominada `less`.</span><span class="sxs-lookup"><span data-stu-id="14eae-142">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="14eae-143">Você talvez precise atualizar a janela.</span><span class="sxs-lookup"><span data-stu-id="14eae-143">You might have to refresh the window.</span></span>

<span data-ttu-id="14eae-144">Execute o `less` tarefa e você verá uma saída semelhante ao que é mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="14eae-144">Run the `less` task, and you see output similar to what is shown here:</span></span>

![less executor de tarefas](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="14eae-146">O *wwwroot/css* pasta agora contém um novo arquivo, *Main*:</span><span class="sxs-lookup"><span data-stu-id="14eae-146">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![css principal criado](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="14eae-148">Abra *Main* e você verá algo parecido com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="14eae-148">Open *main.css* and you see something like the following:</span></span>

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

<span data-ttu-id="14eae-149">Adicionar uma página HTML simples para o *wwwroot* pasta e referência *Main* para ver a paleta de cores em ação.</span><span class="sxs-lookup"><span data-stu-id="14eae-149">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

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

<span data-ttu-id="14eae-150">Você pode ver que o grau de 180 girar na `@base` usada para produzir `@background` resultou no disco de cores opostas cor de `@base`:</span><span class="sxs-lookup"><span data-stu-id="14eae-150">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![exemplo de teste de less](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="14eae-152">Less também oferece suporte para regras aninhadas, bem como as consultas de mídia aninhada.</span><span class="sxs-lookup"><span data-stu-id="14eae-152">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="14eae-153">Por exemplo, definindo hierarquias aninhadas como menus podem resultar em regras de CSS detalhadas como estes:</span><span class="sxs-lookup"><span data-stu-id="14eae-153">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

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

<span data-ttu-id="14eae-154">O ideal é todas as regras de estilo relacionados serão colocadas juntos dentro do arquivo CSS, mas na prática, não há nada impor essa regra exceto convenção e talvez os comentários do bloco.</span><span class="sxs-lookup"><span data-stu-id="14eae-154">Ideally all of the related style rules will be placed together within the CSS file, but in practice there's nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="14eae-155">Definir essas mesmas regras usando less tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="14eae-155">Defining these same rules using Less looks like this:</span></span>

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

<span data-ttu-id="14eae-156">Observe que, nesse caso, todos os elementos subordinados do `nav` estão contidos dentro de seu escopo.</span><span class="sxs-lookup"><span data-stu-id="14eae-156">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="14eae-157">Não há nenhuma repetição dos elementos pai (`nav`, `li`, `a`), e a contagem de linha de total caiu também (embora algumas das é um resultado de colocar valores nas linhas da mesmas no segundo exemplo).</span><span class="sxs-lookup"><span data-stu-id="14eae-157">There's no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that's a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="14eae-158">Ele pode ser muito útil, organizacional, para ver todas as regras para um determinado elemento de interface do usuário em um escopo explicitamente associado, nesse caso definido do restante do arquivo de chaves.</span><span class="sxs-lookup"><span data-stu-id="14eae-158">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="14eae-159">O `&` sintaxe é um recurso seletor less, com & que representa o pai de seletor atual.</span><span class="sxs-lookup"><span data-stu-id="14eae-159">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="14eae-160">Portanto, dentro do {...}</span><span class="sxs-lookup"><span data-stu-id="14eae-160">So, within the a {...}</span></span> <span data-ttu-id="14eae-161">bloco, `&` representa um `a` marca e, portanto, `&:link` é equivalente a `a:link`.</span><span class="sxs-lookup"><span data-stu-id="14eae-161">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="14eae-162">Consultas de mídia, extremamente úteis na criação de designs de resposta também podem contribuir muito para repetição e a complexidade em CSS.</span><span class="sxs-lookup"><span data-stu-id="14eae-162">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="14eae-163">Less permite que as consultas de mídia a ser aninhada dentro de classes, para que a definição de classe inteira não precisa ser repetido em diferentes nível superior `@media` elementos.</span><span class="sxs-lookup"><span data-stu-id="14eae-163">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="14eae-164">Por exemplo, aqui está o CSS para um menu de resposta:</span><span class="sxs-lookup"><span data-stu-id="14eae-164">For example, here is CSS for a responsive menu:</span></span>

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

<span data-ttu-id="14eae-165">Isso pode ser melhor definido em less como:</span><span class="sxs-lookup"><span data-stu-id="14eae-165">This can be better defined in Less as:</span></span>

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

<span data-ttu-id="14eae-166">Outro recurso de less que já vimos é seu suporte para operações matemáticas, permitindo que os atributos de estilo a ser construído de variáveis predefinidas.</span><span class="sxs-lookup"><span data-stu-id="14eae-166">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="14eae-167">Isso facilita a atualização estilos relacionados muito mais fácil, já que a variável de base pode ser modificada e todos os valores dependentes alterar automaticamente.</span><span class="sxs-lookup"><span data-stu-id="14eae-167">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="14eae-168">Arquivos CSS, especialmente para grandes sites (e especialmente se as consultas de mídia estão sendo usadas), tendem a ter muito grandes ao longo do tempo, tornando a trabalhar com elas complicada.</span><span class="sxs-lookup"><span data-stu-id="14eae-168">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="14eae-169">Arquivos less podem ser definidos separadamente, obtidas usando `@import` diretivas.</span><span class="sxs-lookup"><span data-stu-id="14eae-169">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="14eae-170">Less também pode ser usado para importar arquivos CSS individuais, se desejado.</span><span class="sxs-lookup"><span data-stu-id="14eae-170">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="14eae-171">*Mixins* pode aceitar parâmetros e less oferece suporte à lógica condicional na forma de protege mesclado, que fornecem uma maneira declarativa para definir quando determinados mixins entra em vigor.</span><span class="sxs-lookup"><span data-stu-id="14eae-171">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="14eae-172">Um uso comum para protege mesclado ajustar as cores com base em como a luz ou escuro a cor da fonte.</span><span class="sxs-lookup"><span data-stu-id="14eae-172">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="14eae-173">Dado um mesclado que aceita um parâmetro para a cor, um protetor mesclado pode ser usado para modificar o mesclado com base na cor:</span><span class="sxs-lookup"><span data-stu-id="14eae-173">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

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

<span data-ttu-id="14eae-174">Considerando nossa atual `@base` valor `#663333`, esse script less produzirá o seguinte CSS:</span><span class="sxs-lookup"><span data-stu-id="14eae-174">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="14eae-175">Less fornece uma série de recursos adicionais, mas isso deve dar uma ideia da energia desse idioma de pré-processamento.</span><span class="sxs-lookup"><span data-stu-id="14eae-175">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="14eae-176">Sass</span><span class="sxs-lookup"><span data-stu-id="14eae-176">Sass</span></span>

<span data-ttu-id="14eae-177">Sass é semelhante ao menos, fornecendo suporte para muitos dos mesmos recursos, mas com sintaxe ligeiramente diferente.</span><span class="sxs-lookup"><span data-stu-id="14eae-177">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="14eae-178">Ele é criado usando o Ruby, em vez de JavaScript, e portanto tem requisitos de instalação diferentes.</span><span class="sxs-lookup"><span data-stu-id="14eae-178">It's built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="14eae-179">O idioma Sass original não usa chaves ou ponto e vírgula, mas em vez disso, definida escopo usando o recuo e espaços em branco.</span><span class="sxs-lookup"><span data-stu-id="14eae-179">The original Sass language didn't use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="14eae-180">Na versão 3 dos Sass, uma nova sintaxe foi introduzida, **SCSS** ("Sassy CSS").</span><span class="sxs-lookup"><span data-stu-id="14eae-180">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="14eae-181">SCSS é semelhante ao CSS que ignora os níveis de recuo e espaços em branco e, em vez disso, usa o ponto e vírgula e chaves.</span><span class="sxs-lookup"><span data-stu-id="14eae-181">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="14eae-182">Para instalar Sass, normalmente você deve primeiro instalar Ruby (pré-instalado macOS) e, em seguida, execute:</span><span class="sxs-lookup"><span data-stu-id="14eae-182">To install Sass, typically you would first install Ruby (pre-installed on macOS), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="14eae-183">No entanto, se você estiver executando o Visual Studio, você pode começar com Sass em grande parte da mesma maneira como você faria com less.</span><span class="sxs-lookup"><span data-stu-id="14eae-183">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="14eae-184">Abra *Package. JSON* e adicionar o pacote "gulp sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="14eae-184">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="14eae-185">Em seguida, modifique *gulpfile.js* para adicionar uma variável de sass e uma tarefa para compilar os arquivos Sass e colocar os resultados na pasta wwwroot:</span><span class="sxs-lookup"><span data-stu-id="14eae-185">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

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

<span data-ttu-id="14eae-186">Agora você pode adicionar o arquivo Sass *main2.scss* para o *estilos* pasta na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="14eae-186">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Adicionar arquivo scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="14eae-188">Abra *main2.scss* e adicione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="14eae-188">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="14eae-189">Salve todos os arquivos.</span><span class="sxs-lookup"><span data-stu-id="14eae-189">Save all of your files.</span></span> <span data-ttu-id="14eae-190">Agora quando você atualiza **Explorador do Executador de tarefas**, você verá um `sass` tarefa.</span><span class="sxs-lookup"><span data-stu-id="14eae-190">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="14eae-191">Executá-lo e examinar o */wwwroot/css* pasta.</span><span class="sxs-lookup"><span data-stu-id="14eae-191">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="14eae-192">Agora há uma *main2.css* arquivo com esse conteúdo:</span><span class="sxs-lookup"><span data-stu-id="14eae-192">There's now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="14eae-193">Sass oferece suporte a aninhamento praticamente o mesmo foi que less não, fornecer benefícios semelhantes.</span><span class="sxs-lookup"><span data-stu-id="14eae-193">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="14eae-194">Os arquivos podem ser divididos por função e incluídos usando a `@import` diretiva:</span><span class="sxs-lookup"><span data-stu-id="14eae-194">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="14eae-195">Sass oferece suporte a mixins, usando o `@mixin` palavra-chave para defini-los e `@include` para incluí-los, como neste exemplo de [sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="14eae-195">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="14eae-196">Além mixins, Sass também oferece suporte ao conceito de herança, permitindo que uma classe estender o outro.</span><span class="sxs-lookup"><span data-stu-id="14eae-196">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="14eae-197">Ele é conceitualmente semelhante a um mesclado, mas resulta em menos código CSS.</span><span class="sxs-lookup"><span data-stu-id="14eae-197">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="14eae-198">Ela é realizada usando o `@extend` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="14eae-198">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="14eae-199">Para testar mixins, adicione o seguinte ao seu *main2.scss* arquivo:</span><span class="sxs-lookup"><span data-stu-id="14eae-199">To try out mixins, add the following to your *main2.scss* file:</span></span>

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

<span data-ttu-id="14eae-200">Examine a saída em *main2.css* depois de executar o `sass` tarefa no **Explorador do Executador de tarefas**:</span><span class="sxs-lookup"><span data-stu-id="14eae-200">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

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

<span data-ttu-id="14eae-201">Observe que todas as propriedades comuns do alerta mesclado são repetidas em cada classe.</span><span class="sxs-lookup"><span data-stu-id="14eae-201">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="14eae-202">O mesclado foi um bom trabalho de ajudar a eliminar a duplicação no tempo de desenvolvimento, mas ela ainda está criando um CSS com muita duplicação, resultando em maior do que arquivos CSS necessários - um possível problema de desempenho.</span><span class="sxs-lookup"><span data-stu-id="14eae-202">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="14eae-203">Agora, substitua o alerta mesclado com um `.alert` classe e altere `@include` para `@extend` (Lembre-se estender `.alert`, não `alert`):</span><span class="sxs-lookup"><span data-stu-id="14eae-203">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

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

<span data-ttu-id="14eae-204">Execute Sass mais uma vez e examine o CSS resultante:</span><span class="sxs-lookup"><span data-stu-id="14eae-204">Run Sass once more, and examine the resulting CSS:</span></span>

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

<span data-ttu-id="14eae-205">Agora as propriedades são definidas apenas como quantas vezes forem necessárias, e melhor CSS é gerado.</span><span class="sxs-lookup"><span data-stu-id="14eae-205">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="14eae-206">Sass também inclui funções e operações de lógica condicional, semelhantes ao less.</span><span class="sxs-lookup"><span data-stu-id="14eae-206">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="14eae-207">Na verdade, os recursos de dois idiomas são muito semelhantes.</span><span class="sxs-lookup"><span data-stu-id="14eae-207">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="14eae-208">Less ou Sass?</span><span class="sxs-lookup"><span data-stu-id="14eae-208">Less or Sass?</span></span>

<span data-ttu-id="14eae-209">Ainda não há consenso se geralmente é melhor usar less ou Sass (ou até mesmo se preferir o Sass original ou a sintaxe SCSS mais recente em Sass).</span><span class="sxs-lookup"><span data-stu-id="14eae-209">There's still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="14eae-210">Provavelmente, a decisão mais importante é **usar uma dessas ferramentas**, em vez de apenas codificação manual em seus arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="14eae-210">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="14eae-211">Depois que você fez essa decisão, ambos Less e Sass são boas opções.</span><span class="sxs-lookup"><span data-stu-id="14eae-211">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="14eae-212">Font Awesome</span><span class="sxs-lookup"><span data-stu-id="14eae-212">Font Awesome</span></span>

<span data-ttu-id="14eae-213">Além de pré-processadores CSS, outro excelente recurso para aplicativos web modernos de estilo é incrível de fonte.</span><span class="sxs-lookup"><span data-stu-id="14eae-213">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="14eae-214">Lista de fonte é um kit de ferramentas fornece mais de 500 ícones de SVG que podem ser usados livremente em seus aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="14eae-214">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="14eae-215">Ela foi originalmente projetada para trabalhar com inicialização, mas ele não tem nenhuma dependência no framework ou em qualquer biblioteca de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="14eae-215">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="14eae-216">A maneira mais fácil começar com o incríveis fonte é adicionar uma referência a ele, usando seu local de rede (CDN) do fornecimento de conteúdo público:</span><span class="sxs-lookup"><span data-stu-id="14eae-216">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="14eae-217">Você também pode adicioná-lo ao seu projeto do Visual Studio adicionando-as "dependências" em *bower. JSON*:</span><span class="sxs-lookup"><span data-stu-id="14eae-217">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

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

<span data-ttu-id="14eae-218">Depois que você tem uma referência para o incríveis fonte em uma página, você pode adicionar ícones para o seu aplicativo aplicando fonte Awesome classes, normalmente é prefixados com "fa-", para os elementos embutidos HTML (como `<span>` ou `<i>`).</span><span class="sxs-lookup"><span data-stu-id="14eae-218">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="14eae-219">Por exemplo, você pode adicionar ícones de listas simples e menus usando código como este:</span><span class="sxs-lookup"><span data-stu-id="14eae-219">For example, you can add icons to simple lists and menus using code like this:</span></span>

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

<span data-ttu-id="14eae-220">Isso produz o seguinte no navegador - Observe o ícone ao lado de cada item:</span><span class="sxs-lookup"><span data-stu-id="14eae-220">This produces the following in the browser - note the icon beside each item:</span></span>

![ícones de lista](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="14eae-222">Você pode exibir uma lista completa de ícones disponíveis:</span><span class="sxs-lookup"><span data-stu-id="14eae-222">You can view a complete list of the available icons here:</span></span>

http://fontawesome.io/icons/

## <a name="summary"></a><span data-ttu-id="14eae-223">Resumo</span><span class="sxs-lookup"><span data-stu-id="14eae-223">Summary</span></span>

<span data-ttu-id="14eae-224">Aplicativos web modernos exigem designs cada vez mais responsivos e fluidos que são normais, intuitivos e fáceis de usar em uma variedade de dispositivos.</span><span class="sxs-lookup"><span data-stu-id="14eae-224">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="14eae-225">Gerenciar a complexidade das folhas de estilo CSS necessárias para atingir essas metas é melhor usando um tipo de pré-processador less ou Sass.</span><span class="sxs-lookup"><span data-stu-id="14eae-225">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="14eae-226">Além disso, kits de ferramentas como a Awesome fonte rapidamente fornecem ícones conhecidos a menus de navegação textual e experiência de botões, melhorando o usuário do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="14eae-226">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
