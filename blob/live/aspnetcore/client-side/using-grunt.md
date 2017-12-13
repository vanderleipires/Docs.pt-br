---
title: "Usando o assistente no núcleo do ASP.NET"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 471112e9-2c33-454b-96fc-32916102ce73
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: 8ae50514ce24c7f9e3bb1e347d5d860e1de43c5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="using-grunt-in-aspnet-core"></a><span data-ttu-id="71066-103">Usando o assistente no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="71066-103">Using Grunt in ASP.NET Core</span></span> 

<span data-ttu-id="71066-104">Por [Noel arroz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="71066-104">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="71066-105">Pesado é um executor de tarefas do JavaScript que automatiza minimização de script, a compilação TypeScript, ferramentas de "pano" de qualidade do código, pré-processadores de CSS e praticamente qualquer tarefa repetitiva que precisa fazer para dar suporte ao desenvolvimento de cliente.</span><span class="sxs-lookup"><span data-stu-id="71066-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="71066-106">Pesado tem suporte total no Visual Studio, embora os modelos de projeto do ASP.NET usam Gulp por padrão (consulte [usando Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="71066-106">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Using Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="71066-107">Este exemplo usa um projeto vazio do ASP.NET Core como ponto de partida, para mostrar como automatizar o processo de compilação do cliente desde o início.</span><span class="sxs-lookup"><span data-stu-id="71066-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="71066-108">O exemplo concluído limpa o diretório de implantação de destino, combina arquivos JavaScript, verifica a qualidade do código, condensa o conteúdo do arquivo JavaScript e implanta para a raiz do seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="71066-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="71066-109">Usaremos os seguintes pacotes:</span><span class="sxs-lookup"><span data-stu-id="71066-109">We will use the following packages:</span></span>

* <span data-ttu-id="71066-110">**Assistente de**: pacote de executor de tarefas a pesado.</span><span class="sxs-lookup"><span data-stu-id="71066-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="71066-111">**Limpeza de Contribuidor pesado**: um plug-in que remove os arquivos ou diretórios.</span><span class="sxs-lookup"><span data-stu-id="71066-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="71066-112">**Assistente de Contribuidor de jshint**: um plug-in que analisa a qualidade do código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71066-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="71066-113">**Assistente de Contribuidor de concat**: um plug-in que une os arquivos em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="71066-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="71066-114">**uglify pesado-Contribuidor**: um plug-in que minimiza o JavaScript para reduzir o tamanho.</span><span class="sxs-lookup"><span data-stu-id="71066-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="71066-115">**Observação de Contribuidor pesado**: um plug-in que observa a atividade de arquivos.</span><span class="sxs-lookup"><span data-stu-id="71066-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="71066-116">Preparando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="71066-116">Preparing the application</span></span>

<span data-ttu-id="71066-117">Para começar, configure um novo aplicativo web vazio e adicionar arquivos de exemplo de TypeScript.</span><span class="sxs-lookup"><span data-stu-id="71066-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="71066-118">Arquivos typeScript são compilados automaticamente para JavaScript usando as configurações do Visual Studio e serão nosso matérias-primas para processar usando o assistente.</span><span class="sxs-lookup"><span data-stu-id="71066-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="71066-119">No Visual Studio, crie um novo `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="71066-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="71066-120">No **novo projeto ASP.NET** caixa de diálogo, selecione o ASP.NET Core **vazio** modelo e clique no botão Okey.</span><span class="sxs-lookup"><span data-stu-id="71066-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="71066-121">No Gerenciador de soluções, revise a estrutura do projeto.</span><span class="sxs-lookup"><span data-stu-id="71066-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="71066-122">O `\src` pasta inclui vazio `wwwroot` e `Dependencies` nós.</span><span class="sxs-lookup"><span data-stu-id="71066-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![solução de web vazio](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="71066-124">Adicionar uma nova pasta chamada `TypeScript` para o diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="71066-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="71066-125">Antes de adicionar todos os arquivos, vamos garantir que o Visual Studio tem a opção ' Compilar ao salvar ' para arquivos TypeScript check.</span><span class="sxs-lookup"><span data-stu-id="71066-125">Before adding any files, let’s make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="71066-126">*Ferramentas > Opções > Editor de texto > Typescript > projeto*</span><span class="sxs-lookup"><span data-stu-id="71066-126">*Tools > Options > Text Editor > Typescript > Project*</span></span>

    ![Opções de configuração compliation automática dos arquivos TypeScript](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="71066-128">Clique com botão direito do `TypeScript` diretório e selecione **Adicionar > Novo Item** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="71066-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="71066-129">Selecione o **arquivo JavaScript** item e nomeie o arquivo *Tastes.ts* (Observe o \*extensão. TS).</span><span class="sxs-lookup"><span data-stu-id="71066-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="71066-130">Copie a linha de código do TypeScript abaixo no arquivo (quando você salva, um novo *Tastes.js* arquivo aparecerá com a origem de JavaScript).</span><span class="sxs-lookup"><span data-stu-id="71066-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="71066-131">Adicionar um segundo arquivo para o **TypeScript** diretório e nomeie-o `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="71066-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="71066-132">Copie o código abaixo para o arquivo.</span><span class="sxs-lookup"><span data-stu-id="71066-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="71066-133">Configurando NPM</span><span class="sxs-lookup"><span data-stu-id="71066-133">Configuring NPM</span></span>

<span data-ttu-id="71066-134">Em seguida, configure NPM para baixar pesado e tarefas do assistente.</span><span class="sxs-lookup"><span data-stu-id="71066-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="71066-135">No Gerenciador de soluções, clique com o botão direito e selecione **Adicionar > Novo Item** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="71066-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="71066-136">Selecione o **arquivo de configuração NPM** item, deixe o nome padrão, *Package. JSON*e clique no **adicionar** botão.</span><span class="sxs-lookup"><span data-stu-id="71066-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="71066-137">No *Package. JSON* arquivo, dentro de `devDependencies` chaves de objeto, digite "pesado".</span><span class="sxs-lookup"><span data-stu-id="71066-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="71066-138">Selecione `grunt` o Intellisense de lista e pressione a tecla Enter.</span><span class="sxs-lookup"><span data-stu-id="71066-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="71066-139">Visual Studio será colocada entre aspas no nome do pacote pesado e adicionar dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="71066-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="71066-140">À direita dos dois pontos, selecione a versão estável mais recente do pacote da parte superior da lista do Intellisense (pressione `Ctrl-Space` se o Intellisense não aparecer).</span><span class="sxs-lookup"><span data-stu-id="71066-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense does not appear).</span></span>

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="71066-142">Usa NPM [controle de versão semântico](http://semver.org/) para organizar as dependências.</span><span class="sxs-lookup"><span data-stu-id="71066-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="71066-143">Controle de versão semântico, também conhecido como SemVer, identifica os pacotes com o esquema de numeração <major>.<minor>. <patch>. IntelliSense simplifica o controle de versão semântico, mostrando apenas algumas opções comuns.</span><span class="sxs-lookup"><span data-stu-id="71066-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="71066-144">O item superior na lista do Intellisense (0.4.5 no exemplo acima) é considerado a versão estável mais recente do pacote.</span><span class="sxs-lookup"><span data-stu-id="71066-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="71066-145">O símbolo de acento circunflexo (^) corresponde a mais recente versão principal e o til (~) corresponde a versão secundária mais recente.</span><span class="sxs-lookup"><span data-stu-id="71066-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="71066-146">Consulte o [referência de analisador NPM semver versão](https://www.npmjs.com/package/semver) como um guia para a expressividade completa que fornece SemVer.</span><span class="sxs-lookup"><span data-stu-id="71066-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="71066-147">Adicionar mais dependências carregar pesadom-Contribuidor -\* pacotes para *limpa*, *jshint*, *concat*, *uglify*e *inspecionar* conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="71066-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="71066-148">As versões não precisa coincidir com o exemplo.</span><span class="sxs-lookup"><span data-stu-id="71066-148">The versions do not need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="71066-149">Salve o *Package. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="71066-149">Save the *package.json* file.</span></span>

<span data-ttu-id="71066-150">Baixarão os pacotes para cada item devDependencies, juntamente com todos os arquivos que requer que cada pacote.</span><span class="sxs-lookup"><span data-stu-id="71066-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="71066-151">Você pode encontrar os arquivos de pacote no `node_modules` diretório, permitindo que o **Mostrar todos os arquivos** botão no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="71066-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![node_modules pesado](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="71066-153">Se você precisar, você pode restaurar manualmente as dependências no Gerenciador de soluções clicando em `Dependencies\NPM` e selecionando o **restaurar pacotes** opção de menu.</span><span class="sxs-lookup"><span data-stu-id="71066-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![restaurar os pacotes](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="71066-155">Assistente de configuração</span><span class="sxs-lookup"><span data-stu-id="71066-155">Configuring Grunt</span></span>

<span data-ttu-id="71066-156">Pesado estiver configurado para usar um manifesto chamado *Gruntfile.js* que define, carrega e registra as tarefas que podem ser executadas manualmente ou configuradas para ser executado automaticamente com base em eventos no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71066-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1.  <span data-ttu-id="71066-157">Clique com o botão direito e selecione **Adicionar > Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="71066-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="71066-158">Selecione o **arquivo de configuração Grunt** opção, deixe o nome padrão, *Gruntfile.js*e clique no **adicionar** botão.</span><span class="sxs-lookup"><span data-stu-id="71066-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

    <span data-ttu-id="71066-159">O código inicial inclui uma definição de módulo e o `grunt.initConfig()` método.</span><span class="sxs-lookup"><span data-stu-id="71066-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="71066-160">O `initConfig()` é usado para definir opções para cada pacote, e o restante do módulo serão carregadas e registrar tarefas.</span><span class="sxs-lookup"><span data-stu-id="71066-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. <span data-ttu-id="71066-161">Dentro de `initConfig()` método, adicionar opções para o `clean` conforme mostrado no exemplo de tarefa *Gruntfile.js* abaixo.</span><span class="sxs-lookup"><span data-stu-id="71066-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="71066-162">A tarefa de limpeza aceita uma matriz de cadeias de caracteres de diretório.</span><span class="sxs-lookup"><span data-stu-id="71066-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="71066-163">Essa tarefa remove arquivos de wwwroot/lib e o diretório temp/inteiro.</span><span class="sxs-lookup"><span data-stu-id="71066-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="71066-164">Abaixo do método initConfig(), adicione uma chamada para `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="71066-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="71066-165">Isso fará a tarefa executável do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71066-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="71066-166">Salvar *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="71066-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="71066-167">O arquivo deve ser semelhante a captura de tela abaixo.</span><span class="sxs-lookup"><span data-stu-id="71066-167">The file should look something like the screenshot below.</span></span>

    ![gruntfile inicial](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="71066-169">Clique com botão direito *Gruntfile.js* e selecione **Explorador do Executador de tarefas** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="71066-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="71066-170">A janela Explorador do Executador de tarefas será aberto.</span><span class="sxs-lookup"><span data-stu-id="71066-170">The Task Runner Explorer window will open.</span></span>

    ![menu do Gerenciador de executor de tarefas](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="71066-172">Verifique `clean` mostra em **tarefas** no Explorador do Executador de tarefas.</span><span class="sxs-lookup"><span data-stu-id="71066-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![lista de tarefas tarefa runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="71066-174">A tarefa de limpeza e selecione **executar** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="71066-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="71066-175">Uma janela de comando exibe o progresso da tarefa.</span><span class="sxs-lookup"><span data-stu-id="71066-175">A command window displays progress of the task.</span></span>

    ![tarefa de limpeza runner explorer executar tarefa](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="71066-177">Não existem arquivos ou diretórios para limpar ainda.</span><span class="sxs-lookup"><span data-stu-id="71066-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="71066-178">Se desejar, você pode criá-las manualmente no Gerenciador de soluções e, em seguida, executar a tarefa de limpeza como um teste.</span><span class="sxs-lookup"><span data-stu-id="71066-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="71066-179">No método initConfig(), adicione uma entrada para `concat` usando o código abaixo.</span><span class="sxs-lookup"><span data-stu-id="71066-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="71066-180">O `src` matriz de propriedade lista arquivos combinar, na ordem em que eles devem ser combinados.</span><span class="sxs-lookup"><span data-stu-id="71066-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="71066-181">O `dest` propriedade atribui o caminho para o arquivo combinado que é produzido.</span><span class="sxs-lookup"><span data-stu-id="71066-181">The `dest` property assigns the path to the combined file that is produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="71066-182">O `all` propriedade no código acima é o nome de um destino.</span><span class="sxs-lookup"><span data-stu-id="71066-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="71066-183">Destinos são usados em algumas tarefas pesado para permitir que vários ambientes de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="71066-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="71066-184">Você pode exibir os destinos internos usando o Intellisense ou atribuir seu próprio.</span><span class="sxs-lookup"><span data-stu-id="71066-184">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="71066-185">Adicionar o `jshint` tarefas usando o código abaixo.</span><span class="sxs-lookup"><span data-stu-id="71066-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="71066-186">O utilitário de qualidade do código jshint é executado em todos os arquivos JavaScript encontrado no diretório temp.</span><span class="sxs-lookup"><span data-stu-id="71066-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="71066-187">A opção "-W069" é um erro gerado pelo jshint quando colchete de JavaScript usa a sintaxe para atribuir uma propriedade em vez da notação de ponto, ou seja, `Tastes["Sweet"]` em vez de `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="71066-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="71066-188">A opção desativa o aviso para permitir que o restante do processo para continuar.</span><span class="sxs-lookup"><span data-stu-id="71066-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10.  <span data-ttu-id="71066-189">Adicionar o `uglify` tarefas usando o código abaixo.</span><span class="sxs-lookup"><span data-stu-id="71066-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="71066-190">A tarefa minimiza o *combined.js* arquivo encontrado no diretório temp e cria o arquivo de resultado no wwwroot/lib seguindo a convenção de nomenclatura padrão  *\<nome de arquivo\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="71066-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. <span data-ttu-id="71066-191">Sob o grunt.loadNpmTasks() de chamada que carrega a limpeza de Contribuidor pesado, incluem a mesma chamada para jshint, concat e uglify usando o código abaixo.</span><span class="sxs-lookup"><span data-stu-id="71066-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="71066-192">Salvar *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="71066-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="71066-193">O arquivo deve ser algo como o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="71066-193">The file should look something like the example below.</span></span>

    ![exemplo de arquivo pesado concluída](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="71066-195">Observe que a lista de tarefas do Gerenciador de executor inclui `clean`, `concat`, `jshint` e `uglify` tarefas.</span><span class="sxs-lookup"><span data-stu-id="71066-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="71066-196">Execute cada tarefa na ordem e observar os resultados no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="71066-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="71066-197">Cada tarefa deve ser executada sem erros.</span><span class="sxs-lookup"><span data-stu-id="71066-197">Each task should run without errors.</span></span>
    
    ![Execute cada tarefa Explorador do Executador de tarefas](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="71066-199">A tarefa concat cria um novo *combined.js* de arquivo e o coloca na pasta temp.</span><span class="sxs-lookup"><span data-stu-id="71066-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="71066-200">A tarefa de jshint simplesmente executa e não produz saída.</span><span class="sxs-lookup"><span data-stu-id="71066-200">The jshint task simply runs and doesn’t produce output.</span></span> <span data-ttu-id="71066-201">A tarefa uglify cria um novo *combined.min.js* de arquivo e o coloca na wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="71066-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="71066-202">Após a conclusão, a solução deve ser semelhante a captura de tela abaixo:</span><span class="sxs-lookup"><span data-stu-id="71066-202">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![Depois que todas as tarefas de Gerenciador de soluções](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="71066-204">Para obter mais informações sobre as opções para cada pacote, visite [https://www.npmjs.com/](https://www.npmjs.com/) e o nome do pacote na caixa de pesquisa na página principal de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="71066-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="71066-205">Por exemplo, você pode pesquisar o pacote limpeza de Contribuidor pesado para obter um link de documentação que explica a todos os seus parâmetros.</span><span class="sxs-lookup"><span data-stu-id="71066-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="71066-206">Todos juntos agora</span><span class="sxs-lookup"><span data-stu-id="71066-206">All together now</span></span>

<span data-ttu-id="71066-207">Use o Assistente de `registerTask()` método para executar uma série de tarefas em uma determinada sequência.</span><span class="sxs-lookup"><span data-stu-id="71066-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="71066-208">Por exemplo, para executar o exemplo etapas acima na ordem limpa -> concat -> jshint -> uglify, adicione o código abaixo para o módulo.</span><span class="sxs-lookup"><span data-stu-id="71066-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="71066-209">O código deve ser adicionado no mesmo nível como as chamadas loadNpmTasks(), fora initConfig.</span><span class="sxs-lookup"><span data-stu-id="71066-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="71066-210">A nova tarefa aparece no Explorador do Executador de tarefas em tarefas de Alias.</span><span class="sxs-lookup"><span data-stu-id="71066-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="71066-211">Pode-se com o botão direito e executá-lo como faria com outras tarefas.</span><span class="sxs-lookup"><span data-stu-id="71066-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="71066-212">O `all` tarefa executará `clean`, `concat`, `jshint` e `uglify`, em ordem.</span><span class="sxs-lookup"><span data-stu-id="71066-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![tarefas do Assistente de alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="71066-214">Monitorar alterações</span><span class="sxs-lookup"><span data-stu-id="71066-214">Watching for changes</span></span>

<span data-ttu-id="71066-215">Um `watch` tarefa fica de olho em arquivos e diretórios.</span><span class="sxs-lookup"><span data-stu-id="71066-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="71066-216">O relógio dispara tarefas automaticamente se detectar alterações.</span><span class="sxs-lookup"><span data-stu-id="71066-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="71066-217">Adicione o código abaixo para initConfig para observar as alterações \*no diretório TypeScript. js.</span><span class="sxs-lookup"><span data-stu-id="71066-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="71066-218">Se um arquivo JavaScript for alterado, `watch` executará o `all` tarefa.</span><span class="sxs-lookup"><span data-stu-id="71066-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="71066-219">Adicionar uma chamada para `loadNpmTasks()` para mostrar o `watch` tarefa no Explorador do Executador de tarefas.</span><span class="sxs-lookup"><span data-stu-id="71066-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="71066-220">Clique na tarefa de inspeção no Explorador do Executador de tarefas e selecione Executar no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="71066-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="71066-221">A janela de comando que mostra a execução de tarefa watch exibirá um "Aguardando..."</span><span class="sxs-lookup"><span data-stu-id="71066-221">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="71066-222">.</span><span class="sxs-lookup"><span data-stu-id="71066-222">message.</span></span> <span data-ttu-id="71066-223">Abra um dos arquivos TypeScript, adicione um espaço e, em seguida, salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="71066-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="71066-224">Isso disparar a tarefa de inspeção e disparar outras tarefas executadas em ordem.</span><span class="sxs-lookup"><span data-stu-id="71066-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="71066-225">Captura de tela abaixo mostra uma exemplo de execução.</span><span class="sxs-lookup"><span data-stu-id="71066-225">The screenshot below shows a sample run.</span></span>

![executando a saída de tarefas](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="71066-227">Associação a eventos do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71066-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="71066-228">A menos que você deseja iniciar manualmente as tarefas toda vez que você trabalha no Visual Studio, você pode associar tarefas a serem **antes de criar**, **depois de criar**, **limpar**, e  **Projeto aberto** eventos.</span><span class="sxs-lookup"><span data-stu-id="71066-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="71066-229">Vamos associar `watch` para que ele seja executado sempre que o Visual Studio abrirá.</span><span class="sxs-lookup"><span data-stu-id="71066-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="71066-230">No Explorador do Executador de tarefas, a tarefa de inspeção e selecione **associações > Abrir projeto** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="71066-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![associar uma tarefa para a abertura do projeto](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="71066-232">Descarregar e recarregar o projeto.</span><span class="sxs-lookup"><span data-stu-id="71066-232">Unload and reload the project.</span></span> <span data-ttu-id="71066-233">Quando o projeto é carregado novamente, a tarefa de inspeção iniciará a execução automaticamente.</span><span class="sxs-lookup"><span data-stu-id="71066-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="71066-234">Resumo</span><span class="sxs-lookup"><span data-stu-id="71066-234">Summary</span></span>

<span data-ttu-id="71066-235">Pesado é um executor de tarefa avançada que pode ser usado para automatizar a maioria das tarefas de compilação do cliente.</span><span class="sxs-lookup"><span data-stu-id="71066-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="71066-236">Pesado aproveita NPM para fornecer seus pacotes e recursos de ferramentas de integração com o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71066-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="71066-237">Explorador do Executador de tarefas do Visual Studio detecta alterações nos arquivos de configuração e fornece uma interface conveniente para executar tarefas, exibir tarefas em execução e associar tarefas a eventos do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71066-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71066-238">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="71066-238">Additional resources</span></span>

   * [<span data-ttu-id="71066-239">Usando o Gulp</span><span class="sxs-lookup"><span data-stu-id="71066-239">Using Gulp</span></span>](using-gulp.md)
