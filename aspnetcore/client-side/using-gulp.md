---
title: Use o Gulp no núcleo do ASP.NET
author: rick-anderson
description: Saiba como usar o Gulp em ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: 7a3d7807484b76e40e9728e0c750fa1383cc271f
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="5847d-103">Use o Gulp no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5847d-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="5847d-104">Por [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), e [Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="5847d-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="5847d-105">Em um aplicativo web moderna típico, o processo de compilação pode:</span><span class="sxs-lookup"><span data-stu-id="5847d-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="5847d-106">Agrupar e minificada arquivos JavaScript e CSS.</span><span class="sxs-lookup"><span data-stu-id="5847d-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="5847d-107">Execute ferramentas para chamar as tarefas de empacotamento e minimização antes de cada compilação.</span><span class="sxs-lookup"><span data-stu-id="5847d-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="5847d-108">Compilar menos ou SASS arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="5847d-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="5847d-109">Compile arquivos CoffeeScript ou TypeScript para JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5847d-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="5847d-110">Um *executor de tarefas* é uma ferramenta que automatiza a essas tarefas de rotina de desenvolvimento e muito mais.</span><span class="sxs-lookup"><span data-stu-id="5847d-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="5847d-111">O Visual Studio fornece suporte interno para dois executores de tarefas comuns de baseados em JavaScript: [Gulp](https://gulpjs.com/) e [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="5847d-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="5847d-112">gulp</span><span class="sxs-lookup"><span data-stu-id="5847d-112">Gulp</span></span>

<span data-ttu-id="5847d-113">Gulp é um baseados em JavaScript streaming compilação Kit de ferramentas do código do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="5847d-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="5847d-114">Ele costuma ser usado para transmitir arquivos do lado do cliente por meio de uma série de processos quando um evento específico é acionado em um ambiente de compilação.</span><span class="sxs-lookup"><span data-stu-id="5847d-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="5847d-115">Por exemplo, o Gulp pode ser usado para automatizar [empacotamento e minimização](bundling-and-minification.md) ou a limpeza de um ambiente de desenvolvimento antes de uma nova compilação.</span><span class="sxs-lookup"><span data-stu-id="5847d-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="5847d-116">Um conjunto de tarefas Gulp é definido em *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="5847d-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="5847d-117">O JavaScript a seguir inclui módulos de Gulp e especifica os caminhos de arquivo a ser referenciado de tarefas disponível em breve:</span><span class="sxs-lookup"><span data-stu-id="5847d-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

<span data-ttu-id="5847d-118">O código acima Especifica quais módulos de nó são necessários.</span><span class="sxs-lookup"><span data-stu-id="5847d-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="5847d-119">O `require` função importa cada módulo, para que as tarefas dependentes podem utilizar seus recursos.</span><span class="sxs-lookup"><span data-stu-id="5847d-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="5847d-120">Cada um dos módulos importados é atribuída a uma variável.</span><span class="sxs-lookup"><span data-stu-id="5847d-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="5847d-121">Os módulos podem estar localizados por nome ou caminho.</span><span class="sxs-lookup"><span data-stu-id="5847d-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="5847d-122">Neste exemplo, os módulos denominado `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, e `gulp-uglify` são recuperados por nome.</span><span class="sxs-lookup"><span data-stu-id="5847d-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="5847d-123">Além disso, uma série de caminhos são criadas para que os locais de arquivos CSS e JavaScript podem ser reutilizados e referência de tarefas.</span><span class="sxs-lookup"><span data-stu-id="5847d-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="5847d-124">A tabela a seguir fornece descrições dos módulos incluídos no *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="5847d-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="5847d-125">Nome do Módulo</span><span class="sxs-lookup"><span data-stu-id="5847d-125">Module Name</span></span> | <span data-ttu-id="5847d-126">Descrição</span><span class="sxs-lookup"><span data-stu-id="5847d-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="5847d-127">gulp</span><span class="sxs-lookup"><span data-stu-id="5847d-127">gulp</span></span>        | <span data-ttu-id="5847d-128">O sistema de compilação streaming Gulp.</span><span class="sxs-lookup"><span data-stu-id="5847d-128">The Gulp streaming build system.</span></span> <span data-ttu-id="5847d-129">Para obter mais informações, consulte [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="5847d-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="5847d-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="5847d-130">rimraf</span></span>      | <span data-ttu-id="5847d-131">Um módulo de exclusão do nó.</span><span class="sxs-lookup"><span data-stu-id="5847d-131">A Node deletion module.</span></span> <span data-ttu-id="5847d-132">Para obter mais informações, consulte [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="5847d-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="5847d-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="5847d-133">gulp-concat</span></span> | <span data-ttu-id="5847d-134">Um módulo que concatena arquivos com base em caractere de nova linha do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="5847d-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="5847d-135">Para obter mais informações, consulte [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="5847d-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="5847d-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="5847d-136">gulp-cssmin</span></span> | <span data-ttu-id="5847d-137">Um módulo que minimiza arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="5847d-137">A module that minifies CSS files.</span></span> <span data-ttu-id="5847d-138">Para obter mais informações, consulte [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="5847d-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="5847d-139">uglify gulp</span><span class="sxs-lookup"><span data-stu-id="5847d-139">gulp-uglify</span></span> | <span data-ttu-id="5847d-140">Um módulo que minimiza *. js* arquivos.</span><span class="sxs-lookup"><span data-stu-id="5847d-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="5847d-141">Para obter mais informações, consulte [uglify gulp](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="5847d-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="5847d-142">Depois que os módulos necessários são importados, as tarefas podem ser especificadas.</span><span class="sxs-lookup"><span data-stu-id="5847d-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="5847d-143">Aqui, há seis tarefas registrado, representado pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="5847d-143">Here there are six tasks registered, represented by the following code:</span></span>

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

<span data-ttu-id="5847d-144">A tabela a seguir fornece uma explicação das tarefas especificado no código acima:</span><span class="sxs-lookup"><span data-stu-id="5847d-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="5847d-145">Nome da Tarefa</span><span class="sxs-lookup"><span data-stu-id="5847d-145">Task Name</span></span>|<span data-ttu-id="5847d-146">Descrição</span><span class="sxs-lookup"><span data-stu-id="5847d-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="5847d-147">Limpar: js</span><span class="sxs-lookup"><span data-stu-id="5847d-147">clean:js</span></span>|<span data-ttu-id="5847d-148">Uma tarefa que usa o módulo de exclusão do nó rimraf para remover a versão minimizada do arquivo site.js.</span><span class="sxs-lookup"><span data-stu-id="5847d-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="5847d-149">Limpar: css</span><span class="sxs-lookup"><span data-stu-id="5847d-149">clean:css</span></span>|<span data-ttu-id="5847d-150">Uma tarefa que usa o módulo de exclusão do nó rimraf para remover a versão minimizada do arquivo site.css.</span><span class="sxs-lookup"><span data-stu-id="5847d-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="5847d-151">Limpar</span><span class="sxs-lookup"><span data-stu-id="5847d-151">clean</span></span>|<span data-ttu-id="5847d-152">Uma tarefa que chama o `clean:js` tarefa, seguida de `clean:css` tarefa.</span><span class="sxs-lookup"><span data-stu-id="5847d-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="5847d-153">min:js</span><span class="sxs-lookup"><span data-stu-id="5847d-153">min:js</span></span>|<span data-ttu-id="5847d-154">Uma tarefa que minimiza e concatena todos os arquivos. js na pasta js.</span><span class="sxs-lookup"><span data-stu-id="5847d-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="5847d-155">A. min.js arquivos serão excluídos.</span><span class="sxs-lookup"><span data-stu-id="5847d-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="5847d-156">min:CSS</span><span class="sxs-lookup"><span data-stu-id="5847d-156">min:css</span></span>|<span data-ttu-id="5847d-157">Uma tarefa que minimiza e concatena todos os arquivos. CSS dentro da pasta de css.</span><span class="sxs-lookup"><span data-stu-id="5847d-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="5847d-158">A. min.css arquivos serão excluídos.</span><span class="sxs-lookup"><span data-stu-id="5847d-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="5847d-159">min</span><span class="sxs-lookup"><span data-stu-id="5847d-159">min</span></span>|<span data-ttu-id="5847d-160">Uma tarefa que chama o `min:js` tarefa, seguida de `min:css` tarefa.</span><span class="sxs-lookup"><span data-stu-id="5847d-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="5847d-161">Execução de tarefas padrão</span><span class="sxs-lookup"><span data-stu-id="5847d-161">Running default tasks</span></span>

<span data-ttu-id="5847d-162">Se você ainda não criou um novo aplicativo Web, crie um novo projeto de aplicativo Web ASP.NET no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5847d-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="5847d-163">Adicione um novo arquivo JavaScript ao seu projeto e denomine- *gulpfile.js*, em seguida, copie o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5847d-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  <span data-ttu-id="5847d-164">Abra o *Package. JSON* arquivo (Adicionar se não há) e adicione o seguinte.</span><span class="sxs-lookup"><span data-stu-id="5847d-164">Open the *package.json* file (add if not there) and add the following.</span></span>

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  <span data-ttu-id="5847d-165">Em **Solution Explorer**, clique com botão direito *gulpfile.js*e selecione **Explorador do Executador de tarefas**.</span><span class="sxs-lookup"><span data-stu-id="5847d-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Abra o Explorador do Executador de tarefas no Gerenciador de soluções](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="5847d-167">**Explorador do Executador de tarefas** mostra a lista de tarefas de Gulp.</span><span class="sxs-lookup"><span data-stu-id="5847d-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="5847d-168">(Talvez você precise clicar o **atualização** botão que aparece à esquerda do nome do projeto.)</span><span class="sxs-lookup"><span data-stu-id="5847d-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Explorador do Executador de tarefas](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="5847d-170">O **Explorador do Executador de tarefas** item de menu de contexto será exibida apenas se *gulpfile.js* está no diretório raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="5847d-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4.  <span data-ttu-id="5847d-171">Sob **tarefas** na **Explorador do Executador de tarefas**, clique com botão direito **limpa**e selecione **executar** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="5847d-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Tarefa de limpeza de Explorador do Executador de tarefas](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="5847d-173">**Explorador do Executador de tarefas** criará uma nova guia chamada **limpa** e execute a tarefa de limpeza, conforme definido em *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="5847d-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="5847d-174">Com o botão direito do **limpa** de tarefas e selecione **associações** > **antes de criar**.</span><span class="sxs-lookup"><span data-stu-id="5847d-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Associação BeforeBuild Explorador do Executador de tarefas](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="5847d-176">O **antes de criar** associação configura a tarefa de limpeza para executar automaticamente antes de cada build do projeto.</span><span class="sxs-lookup"><span data-stu-id="5847d-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="5847d-177">As associações que você configurar o com **Explorador do Executador de tarefas** são armazenadas na forma de um comentário na parte superior da sua *gulpfile.js* e entrarão em vigor somente no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5847d-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="5847d-178">É uma alternativa que não requer o Visual Studio configurar a execução automática de tarefas de vez em seu *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5847d-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="5847d-179">Por exemplo, colocar isso em seu *. csproj* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5847d-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="5847d-180">Agora que a tarefa de limpeza é executada quando você executar o projeto no Visual Studio ou de um prompt de comando usando o [dotnet executar](/dotnet/core/tools/dotnet-run) comando (execute `npm install` primeiro).</span><span class="sxs-lookup"><span data-stu-id="5847d-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="5847d-181">Definir e executar uma nova tarefa</span><span class="sxs-lookup"><span data-stu-id="5847d-181">Defining and running a new task</span></span>

<span data-ttu-id="5847d-182">Para definir uma nova tarefa Gulp, modificar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="5847d-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="5847d-183">Adicione o seguinte JavaScript ao final da *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="5847d-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="5847d-184">Essa tarefa é denominada `first`, e simplesmente exibe uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="5847d-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="5847d-185">Salvar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="5847d-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="5847d-186">Em **Solution Explorer**, clique com botão direito *gulpfile.js*e selecione *Explorador do Executador de tarefas*.</span><span class="sxs-lookup"><span data-stu-id="5847d-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="5847d-187">Em **Explorador do Executador de tarefas**, clique com botão direito **primeiro**e selecione **executar**.</span><span class="sxs-lookup"><span data-stu-id="5847d-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Execute a primeira tarefa Explorador do Executador de tarefas](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="5847d-189">O texto de saída é exibido.</span><span class="sxs-lookup"><span data-stu-id="5847d-189">The output text is displayed.</span></span> <span data-ttu-id="5847d-190">Para obter exemplos com base em cenários comuns, consulte [Gulp receitas](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="5847d-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="5847d-191">Definindo e tarefas em execução em uma série</span><span class="sxs-lookup"><span data-stu-id="5847d-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="5847d-192">Quando você executa várias tarefas, as tarefas executadas simultaneamente por padrão.</span><span class="sxs-lookup"><span data-stu-id="5847d-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="5847d-193">No entanto, se você precisar executar tarefas em uma ordem específica, você deve especificar quando cada tarefa é concluída, bem como quais tarefas dependentes na conclusão de outra tarefa.</span><span class="sxs-lookup"><span data-stu-id="5847d-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="5847d-194">Para definir uma série de tarefas a serem executadas em ordem, substitua o `first` tarefas que você adicionou acima na *gulpfile.js* com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5847d-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="5847d-195">Agora você tem três tarefas: `series:first`, `series:second`, e `series`.</span><span class="sxs-lookup"><span data-stu-id="5847d-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="5847d-196">O `series:second` tarefa inclui um segundo parâmetro que especifica uma matriz de tarefas a ser executado e concluído antes do `series:second` tarefa será executada.</span><span class="sxs-lookup"><span data-stu-id="5847d-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="5847d-197">Conforme especificado no código acima, somente o `series:first` tarefa deve ser concluída antes do `series:second` tarefa será executada.</span><span class="sxs-lookup"><span data-stu-id="5847d-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="5847d-198">Salvar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="5847d-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="5847d-199">Em **Solution Explorer**, clique com botão direito *gulpfile.js* e selecione **Explorador do Executador de tarefas** se ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="5847d-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="5847d-200">Em **Explorador do Executador de tarefas**, clique com botão direito **série** e selecione **executar**.</span><span class="sxs-lookup"><span data-stu-id="5847d-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Explorador do Executador de tarefas executar tarefa de série](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="5847d-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="5847d-202">IntelliSense</span></span>

<span data-ttu-id="5847d-203">O IntelliSense oferece conclusão de código, descrições de parâmetro e outros recursos para aumentar a produtividade e diminuir os erros.</span><span class="sxs-lookup"><span data-stu-id="5847d-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="5847d-204">Gulp tarefas são escritas em JavaScript; Portanto, o IntelliSense pode fornecer assistência durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="5847d-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="5847d-205">Conforme você trabalha com JavaScript, IntelliSense listará os objetos, funções, propriedades e parâmetros que estão disponíveis com base em seu contexto atual.</span><span class="sxs-lookup"><span data-stu-id="5847d-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="5847d-206">Selecione uma opção de codificação na lista pop-up fornecida pelo IntelliSense para concluir o código.</span><span class="sxs-lookup"><span data-stu-id="5847d-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="5847d-208">Para obter mais informações sobre o IntelliSense, consulte [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="5847d-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="5847d-209">Ambientes de desenvolvimento, teste e produção</span><span class="sxs-lookup"><span data-stu-id="5847d-209">Development, staging, and production environments</span></span>

<span data-ttu-id="5847d-210">Quando o Gulp é usado para otimizar os arquivos do lado do cliente para produção e preparo, os arquivos processados são salvos em um local de preparo e produção.</span><span class="sxs-lookup"><span data-stu-id="5847d-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="5847d-211">O *cshtml* arquivo usa o **ambiente** marca auxiliar para fornecer duas versões diferentes de arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="5847d-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="5847d-212">É uma versão dos arquivos CSS para o desenvolvimento e a outra versão é otimizada para preparação e produção.</span><span class="sxs-lookup"><span data-stu-id="5847d-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="5847d-213">No Visual Studio de 2017, quando você altera o **ASPNETCORE_ENVIRONMENT** variável de ambiente `Production`, Visual Studio criará o aplicativo Web e um link para os arquivos CSS minimizados.</span><span class="sxs-lookup"><span data-stu-id="5847d-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="5847d-214">A marcação a seguir mostra o **ambiente** marca auxiliares que contém as marcações de link para o `Development` CSS arquivos e o minimizada `Staging, Production` arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="5847d-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a><span data-ttu-id="5847d-215">Alternar entre ambientes</span><span class="sxs-lookup"><span data-stu-id="5847d-215">Switching between environments</span></span>

<span data-ttu-id="5847d-216">Para alternar entre a compilação para ambientes diferentes, modifique o **ASPNETCORE_ENVIRONMENT** valor da variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="5847d-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="5847d-217">Em **Explorador do Executador de tarefas**, verifique o **min** tarefa foi definida para executar **antes de criar**.</span><span class="sxs-lookup"><span data-stu-id="5847d-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="5847d-218">Em **Solution Explorer**, clique no nome do projeto e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="5847d-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="5847d-219">A folha de propriedades para o aplicativo Web é exibida.</span><span class="sxs-lookup"><span data-stu-id="5847d-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="5847d-220">Clique na guia **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="5847d-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="5847d-221">Definir o valor de **: ambiente de hospedagem** variável de ambiente `Production`.</span><span class="sxs-lookup"><span data-stu-id="5847d-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="5847d-222">Pressione **F5** para executar o aplicativo em um navegador.</span><span class="sxs-lookup"><span data-stu-id="5847d-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="5847d-223">Na janela do navegador, a página e selecione **Exibir código-fonte** para exibir o HTML da página.</span><span class="sxs-lookup"><span data-stu-id="5847d-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="5847d-224">Observe que os links de folha de estilos apontam para os arquivos CSS minimizados.</span><span class="sxs-lookup"><span data-stu-id="5847d-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="5847d-225">Feche o navegador para interromper o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="5847d-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="5847d-226">No Visual Studio, retorne a folha de propriedades para o aplicativo Web e altere o **: ambiente de hospedagem** de volta para a variável de ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="5847d-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="5847d-227">Pressione **F5** para executar o aplicativo em um navegador novamente.</span><span class="sxs-lookup"><span data-stu-id="5847d-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="5847d-228">Na janela do navegador, a página e selecione **Exibir código-fonte** para ver o HTML da página.</span><span class="sxs-lookup"><span data-stu-id="5847d-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="5847d-229">Observe que os links de folha de estilos apontam para as versões unminified dos arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="5847d-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="5847d-230">Para obter mais informações relacionadas a ambientes em ASP.NET Core, consulte [trabalhar com vários ambientes](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="5847d-230">For more information related to environments in ASP.NET Core, see [Work with multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="5847d-231">Detalhes da tarefa e o módulo</span><span class="sxs-lookup"><span data-stu-id="5847d-231">Task and module details</span></span>

<span data-ttu-id="5847d-232">Uma tarefa Gulp está registrada com um nome de função.</span><span class="sxs-lookup"><span data-stu-id="5847d-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="5847d-233">É possível especificar dependências se outras tarefas devem ser executadas antes da tarefa atual.</span><span class="sxs-lookup"><span data-stu-id="5847d-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="5847d-234">Funções adicionais permitem que você execute e observar as tarefas de Gulp, bem como definir a origem (*src*) e de destino (*dest*) dos arquivos que está sendo modificados.</span><span class="sxs-lookup"><span data-stu-id="5847d-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="5847d-235">Estas são as funções de API Gulp primárias:</span><span class="sxs-lookup"><span data-stu-id="5847d-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="5847d-236">Função gulp</span><span class="sxs-lookup"><span data-stu-id="5847d-236">Gulp Function</span></span>|<span data-ttu-id="5847d-237">Sintaxe</span><span class="sxs-lookup"><span data-stu-id="5847d-237">Syntax</span></span>|<span data-ttu-id="5847d-238">Descrição</span><span class="sxs-lookup"><span data-stu-id="5847d-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="5847d-239">tarefa</span><span class="sxs-lookup"><span data-stu-id="5847d-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="5847d-240">O `task` função cria uma tarefa.</span><span class="sxs-lookup"><span data-stu-id="5847d-240">The `task` function creates a task.</span></span> <span data-ttu-id="5847d-241">O `name` parâmetro define o nome da tarefa.</span><span class="sxs-lookup"><span data-stu-id="5847d-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="5847d-242">O `deps` parâmetro contém uma matriz de tarefas a serem concluídas antes de executa essa tarefa.</span><span class="sxs-lookup"><span data-stu-id="5847d-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="5847d-243">O `fn` parâmetro representa uma função de retorno de chamada que executa as operações da tarefa.</span><span class="sxs-lookup"><span data-stu-id="5847d-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="5847d-244">Inspecionar</span><span class="sxs-lookup"><span data-stu-id="5847d-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="5847d-245">O `watch` função monitora arquivos e executa tarefas quando ocorre uma alteração de arquivo.</span><span class="sxs-lookup"><span data-stu-id="5847d-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="5847d-246">O `glob` parâmetro é um `string` ou `array` que determina quais arquivos assistir.</span><span class="sxs-lookup"><span data-stu-id="5847d-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="5847d-247">O `opts` parâmetro fornece observando as opções de arquivo adicionais.</span><span class="sxs-lookup"><span data-stu-id="5847d-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="5847d-248">src</span><span class="sxs-lookup"><span data-stu-id="5847d-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="5847d-249">O `src` função fornece os arquivos que correspondem os valores glob.</span><span class="sxs-lookup"><span data-stu-id="5847d-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="5847d-250">O `glob` parâmetro é um `string` ou `array` que determina quais arquivos para leitura.</span><span class="sxs-lookup"><span data-stu-id="5847d-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="5847d-251">O `options` parâmetro fornece outras opções de arquivo.</span><span class="sxs-lookup"><span data-stu-id="5847d-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="5847d-252">dest</span><span class="sxs-lookup"><span data-stu-id="5847d-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="5847d-253">O `dest` função define um local para o qual os arquivos podem ser gravados.</span><span class="sxs-lookup"><span data-stu-id="5847d-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="5847d-254">O `path` parâmetro é uma cadeia de caracteres ou uma função que determina a pasta de destino.</span><span class="sxs-lookup"><span data-stu-id="5847d-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="5847d-255">O `options` parâmetro é um objeto que especifica as opções de pasta de saída.</span><span class="sxs-lookup"><span data-stu-id="5847d-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="5847d-256">Para obter informações de referência de API Gulp, consulte [Gulp documentos API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="5847d-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="5847d-257">Gulp receitas</span><span class="sxs-lookup"><span data-stu-id="5847d-257">Gulp recipes</span></span>

<span data-ttu-id="5847d-258">A comunidade de Gulp fornece Gulp [receitas](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="5847d-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="5847d-259">Essas receitas consistem em tarefas de Gulp para abordar cenários comuns.</span><span class="sxs-lookup"><span data-stu-id="5847d-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5847d-260">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="5847d-260">Additional resources</span></span>

* [<span data-ttu-id="5847d-261">Documentação de gulp</span><span class="sxs-lookup"><span data-stu-id="5847d-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="5847d-262">Empacotamento e minimização no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5847d-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="5847d-263">Use o assistente no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5847d-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
