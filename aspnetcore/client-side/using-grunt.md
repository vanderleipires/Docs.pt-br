---
title: "Usando o assistente no núcleo do ASP.NET"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: 959a3e61af9834b9364e9fe4bf65a04962e28969
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="using-grunt-in-aspnet-core"></a>Usando o assistente no núcleo do ASP.NET 

Por [Noel arroz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Pesado é um executor de tarefas do JavaScript que automatiza minimização de script, a compilação TypeScript, ferramentas de "pano" de qualidade do código, pré-processadores de CSS e praticamente qualquer tarefa repetitiva que precisa fazer para dar suporte ao desenvolvimento de cliente. Pesado tem suporte total no Visual Studio, embora os modelos de projeto do ASP.NET usam Gulp por padrão (consulte [usando Gulp](using-gulp.md)).

Este exemplo usa um projeto vazio do ASP.NET Core como ponto de partida, para mostrar como automatizar o processo de compilação do cliente desde o início.

O exemplo concluído limpa o diretório de implantação de destino, combina arquivos JavaScript, verifica a qualidade do código, condensa o conteúdo do arquivo JavaScript e implanta para a raiz do seu aplicativo web. Usaremos os seguintes pacotes:

* **Assistente de**: pacote de executor de tarefas a pesado.

* **Limpeza de Contribuidor pesado**: um plug-in que remove os arquivos ou diretórios.

* **Assistente de Contribuidor de jshint**: um plug-in que analisa a qualidade do código JavaScript.

* **Assistente de Contribuidor de concat**: um plug-in que une os arquivos em um único arquivo.

* **uglify pesado-Contribuidor**: um plug-in que minimiza o JavaScript para reduzir o tamanho.

* **Observação de Contribuidor pesado**: um plug-in que observa a atividade de arquivos.

## <a name="preparing-the-application"></a>Preparando o aplicativo

Para começar, configure um novo aplicativo web vazio e adicionar arquivos de exemplo de TypeScript. Arquivos typeScript são compilados automaticamente para JavaScript usando as configurações do Visual Studio e serão nosso matérias-primas para processar usando o assistente.

1.  No Visual Studio, crie um novo `ASP.NET Web Application`.

2.  No **novo projeto ASP.NET** caixa de diálogo, selecione o ASP.NET Core **vazio** modelo e clique no botão Okey.

3.  No Gerenciador de soluções, revise a estrutura do projeto. O `\src` pasta inclui vazio `wwwroot` e `Dependencies` nós.

    ![solução de web vazio](using-grunt/_static/grunt-solution-explorer.png)

4.  Adicionar uma nova pasta chamada `TypeScript` para o diretório do projeto.

5.  Antes de adicionar todos os arquivos, vamos garantir que o Visual Studio tem a opção ' Compilar ao salvar ' para arquivos TypeScript check. *Ferramentas > Opções > Editor de texto > Typescript > projeto*

    ![Opções de configuração compliation automática dos arquivos TypeScript](using-grunt/_static/typescript-options.png)

6.  Clique com botão direito do `TypeScript` diretório e selecione **Adicionar > Novo Item** no menu de contexto. Selecione o **arquivo JavaScript** item e nomeie o arquivo *Tastes.ts* (Observe o \*extensão. TS). Copie a linha de código do TypeScript abaixo no arquivo (quando você salva, um novo *Tastes.js* arquivo aparecerá com a origem de JavaScript).
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  Adicionar um segundo arquivo para o **TypeScript** diretório e nomeie-o `Food.ts`. Copie o código abaixo para o arquivo.

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

## <a name="configuring-npm"></a>Configurando NPM

Em seguida, configure NPM para baixar pesado e tarefas do assistente.

1. No Gerenciador de soluções, clique com o botão direito e selecione **Adicionar > Novo Item** no menu de contexto. Selecione o **arquivo de configuração NPM** item, deixe o nome padrão, *Package. JSON*e clique no **adicionar** botão.

2. No *Package. JSON* arquivo, dentro de `devDependencies` chaves de objeto, digite "pesado". Selecione `grunt` o Intellisense de lista e pressione a tecla Enter. Visual Studio será colocada entre aspas no nome do pacote pesado e adicionar dois-pontos. À direita dos dois pontos, selecione a versão estável mais recente do pacote da parte superior da lista do Intellisense (pressione `Ctrl-Space` se o Intellisense não aparecer).

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > Usa NPM [controle de versão semântico](http://semver.org/) para organizar as dependências. Controle de versão semântico, também conhecido como SemVer, identifica os pacotes com o esquema de numeração <major>.<minor>. <patch>. IntelliSense simplifica o controle de versão semântico, mostrando apenas algumas opções comuns. O item superior na lista do Intellisense (0.4.5 no exemplo acima) é considerado a versão estável mais recente do pacote. O símbolo de acento circunflexo (^) corresponde a mais recente versão principal e o til (~) corresponde a versão secundária mais recente. Consulte o [referência de analisador NPM semver versão](https://www.npmjs.com/package/semver) como um guia para a expressividade completa que fornece SemVer.

3. Adicionar mais dependências carregar pesadom-Contribuidor -\* pacotes para *limpa*, *jshint*, *concat*, *uglify*e *inspecionar* conforme mostrado no exemplo a seguir. As versões não precisa coincidir com o exemplo.

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

4. Salve o *Package. JSON* arquivo.

Baixarão os pacotes para cada item devDependencies, juntamente com todos os arquivos que requer que cada pacote. Você pode encontrar os arquivos de pacote no `node_modules` diretório, permitindo que o **Mostrar todos os arquivos** botão no Gerenciador de soluções.

![node_modules pesado](using-grunt/_static/node-modules.png)

> [!NOTE]
> Se você precisar, você pode restaurar manualmente as dependências no Gerenciador de soluções clicando em `Dependencies\NPM` e selecionando o **restaurar pacotes** opção de menu.

![restaurar os pacotes](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Assistente de configuração

Pesado estiver configurado para usar um manifesto chamado *Gruntfile.js* que define, carrega e registra as tarefas que podem ser executadas manualmente ou configuradas para ser executado automaticamente com base em eventos no Visual Studio.

1.  Clique com o botão direito e selecione **Adicionar > Novo Item**. Selecione o **arquivo de configuração Grunt** opção, deixe o nome padrão, *Gruntfile.js*e clique no **adicionar** botão.

    O código inicial inclui uma definição de módulo e o `grunt.initConfig()` método. O `initConfig()` é usado para definir opções para cada pacote, e o restante do módulo serão carregadas e registrar tarefas.
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. Dentro de `initConfig()` método, adicionar opções para o `clean` conforme mostrado no exemplo de tarefa *Gruntfile.js* abaixo. A tarefa de limpeza aceita uma matriz de cadeias de caracteres de diretório. Essa tarefa remove arquivos de wwwroot/lib e o diretório temp/inteiro.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. Abaixo do método initConfig(), adicione uma chamada para `grunt.loadNpmTasks()`. Isso fará a tarefa executável do Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. Salvar *Gruntfile.js*. O arquivo deve ser semelhante a captura de tela abaixo.

    ![gruntfile inicial](using-grunt/_static/gruntfile-js-initial.png)

5. Clique com botão direito *Gruntfile.js* e selecione **Explorador do Executador de tarefas** no menu de contexto. A janela Explorador do Executador de tarefas será aberto.

    ![menu do Gerenciador de executor de tarefas](using-grunt/_static/task-runner-explorer-menu.png)

6. Verifique `clean` mostra em **tarefas** no Explorador do Executador de tarefas.

    ![lista de tarefas tarefa runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

7. A tarefa de limpeza e selecione **executar** no menu de contexto. Uma janela de comando exibe o progresso da tarefa.

    ![tarefa de limpeza runner explorer executar tarefa](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > Não existem arquivos ou diretórios para limpar ainda. Se desejar, você pode criá-las manualmente no Gerenciador de soluções e, em seguida, executar a tarefa de limpeza como um teste.
    
8. No método initConfig(), adicione uma entrada para `concat` usando o código abaixo.

    O `src` matriz de propriedade lista arquivos combinar, na ordem em que eles devem ser combinados. O `dest` propriedade atribui o caminho para o arquivo combinado que é produzido.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > O `all` propriedade no código acima é o nome de um destino. Destinos são usados em algumas tarefas pesado para permitir que vários ambientes de desenvolvimento. Você pode exibir os destinos internos usando o Intellisense ou atribuir seu próprio.
    
9. Adicionar o `jshint` tarefas usando o código abaixo.

    O utilitário de qualidade do código jshint é executado em todos os arquivos JavaScript encontrado no diretório temp.
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > A opção "-W069" é um erro gerado pelo jshint quando colchete de JavaScript usa a sintaxe para atribuir uma propriedade em vez da notação de ponto, ou seja, `Tastes["Sweet"]` em vez de `Tastes.Sweet`. A opção desativa o aviso para permitir que o restante do processo para continuar.

10.  Adicionar o `uglify` tarefas usando o código abaixo.

    A tarefa minimiza o *combined.js* arquivo encontrado no diretório temp e cria o arquivo de resultado no wwwroot/lib seguindo a convenção de nomenclatura padrão  *\<nome de arquivo\>. min.js*.
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. Sob o grunt.loadNpmTasks() de chamada que carrega a limpeza de Contribuidor pesado, incluem a mesma chamada para jshint, concat e uglify usando o código abaixo.
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. Salvar *Gruntfile.js*. O arquivo deve ser algo como o exemplo a seguir.

    ![exemplo de arquivo pesado concluída](using-grunt/_static/gruntfile-js-complete.png)

13. Observe que a lista de tarefas do Gerenciador de executor inclui `clean`, `concat`, `jshint` e `uglify` tarefas. Execute cada tarefa na ordem e observar os resultados no Gerenciador de soluções. Cada tarefa deve ser executada sem erros.
    
    ![Execute cada tarefa Explorador do Executador de tarefas](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    A tarefa concat cria um novo *combined.js* de arquivo e o coloca na pasta temp. A tarefa de jshint simplesmente executa e não produz saída. A tarefa uglify cria um novo *combined.min.js* de arquivo e o coloca na wwwroot/lib. Após a conclusão, a solução deve ser semelhante a captura de tela abaixo:
    
    ![Depois que todas as tarefas de Gerenciador de soluções](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > Para obter mais informações sobre as opções para cada pacote, visite [https://www.npmjs.com/](https://www.npmjs.com/) e o nome do pacote na caixa de pesquisa na página principal de pesquisa. Por exemplo, você pode pesquisar o pacote limpeza de Contribuidor pesado para obter um link de documentação que explica a todos os seus parâmetros.

### <a name="all-together-now"></a>Todos juntos agora

Use o Assistente de `registerTask()` método para executar uma série de tarefas em uma determinada sequência. Por exemplo, para executar o exemplo etapas acima na ordem limpa -> concat -> jshint -> uglify, adicione o código abaixo para o módulo. O código deve ser adicionado no mesmo nível como as chamadas loadNpmTasks(), fora initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

A nova tarefa aparece no Explorador do Executador de tarefas em tarefas de Alias. Pode-se com o botão direito e executá-lo como faria com outras tarefas. O `all` tarefa executará `clean`, `concat`, `jshint` e `uglify`, em ordem.

![tarefas do Assistente de alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Monitorar alterações

Um `watch` tarefa fica de olho em arquivos e diretórios. O relógio dispara tarefas automaticamente se detectar alterações. Adicione o código abaixo para initConfig para observar as alterações \*no diretório TypeScript. js. Se um arquivo JavaScript for alterado, `watch` executará o `all` tarefa.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Adicionar uma chamada para `loadNpmTasks()` para mostrar o `watch` tarefa no Explorador do Executador de tarefas.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Clique na tarefa de inspeção no Explorador do Executador de tarefas e selecione Executar no menu de contexto. A janela de comando que mostra a execução de tarefa watch exibirá um "Aguardando..." . Abra um dos arquivos TypeScript, adicione um espaço e, em seguida, salve o arquivo. Isso disparar a tarefa de inspeção e disparar outras tarefas executadas em ordem. Captura de tela abaixo mostra uma exemplo de execução.

![executando a saída de tarefas](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Associação a eventos do Visual Studio

A menos que você deseja iniciar manualmente as tarefas toda vez que você trabalha no Visual Studio, você pode associar tarefas a serem **antes de criar**, **depois de criar**, **limpar**, e  **Projeto aberto** eventos.

Vamos associar `watch` para que ele seja executado sempre que o Visual Studio abrirá. No Explorador do Executador de tarefas, a tarefa de inspeção e selecione **associações > Abrir projeto** no menu de contexto.

![associar uma tarefa para a abertura do projeto](using-grunt/_static/bindings-project-open.png)

Descarregar e recarregar o projeto. Quando o projeto é carregado novamente, a tarefa de inspeção iniciará a execução automaticamente.

## <a name="summary"></a>Resumo

Pesado é um executor de tarefa avançada que pode ser usado para automatizar a maioria das tarefas de compilação do cliente. Pesado aproveita NPM para fornecer seus pacotes e recursos de ferramentas de integração com o Visual Studio. Explorador do Executador de tarefas do Visual Studio detecta alterações nos arquivos de configuração e fornece uma interface conveniente para executar tarefas, exibir tarefas em execução e associar tarefas a eventos do Visual Studio.

## <a name="additional-resources"></a>Recursos adicionais

   * [Usando o Gulp](using-gulp.md)
