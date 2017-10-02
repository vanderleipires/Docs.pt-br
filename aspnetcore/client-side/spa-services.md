---
title: "Usando JavaScriptServices para criar aplicativos de única página"
author: scottaddie
description: "Saiba mais sobre os benefícios de usar JavaScriptServices para criar um aplicativo de página única (SPA) com o apoio de ASP.NET Core."
keywords: "Núcleo do ASP.NET, Angular, SPA, JavaScriptServices, SpaServices"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/spa-services
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a93dae3edec73f1b5254aa60662834ca83de62fd
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2017
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a>Usando JavaScriptServices para criar aplicativos de única página com o ASP.NET Core

Por [Scott Addie](https://github.com/scottaddie) e [Fiyaz Hasan](http://fiyazhasan.me/)

Um aplicativo de página única (SPA) é um tipo popular de aplicativo da web devido a sua experiência de usuário avançada inerente. Integração de estruturas SPA ou bibliotecas de cliente, como [Angular](https://angular.io/) ou [reagir](https://facebook.github.io/react/), com as estruturas do lado do servidor como o ASP.NET Core pode ser difícil. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) foi desenvolvido para reduzir a fricção no processo de integração. Ele permite que uma operação contínua entre o cliente diferentes e pilhas de tecnologia do servidor.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>O que é JavaScriptServices?

JavaScriptServices é uma coleção de tecnologias de cliente para o ASP.NET Core. Sua meta é posicionar o ASP.NET Core como plataforma de lado do servidor preferencial dos desenvolvedores para a criação de SPAs.

JavaScriptServices consiste em três pacotes do NuGet distintos:
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Esses pacotes são úteis se você:
* Executar o JavaScript no servidor
* Use uma estrutura SPA ou biblioteca
* Criar ativos do lado do cliente com Webpack

O foco deste artigo é colocado sobre como usar o pacote de SpaServices.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>O que é SpaServices?

SpaServices foi criado para posicionar o ASP.NET Core como plataforma de lado do servidor preferencial dos desenvolvedores para a criação de SPAs. SpaServices não é necessário para desenvolver SPAs com ASP.NET Core, e ele não bloqueie você em uma estrutura de cliente específico.

SpaServices fornece a infraestrutura úteis, como:
* [Pré-processamento do lado do servidor](#server-prerendering)
* [Middleware de desenvolvimento webpack](#webpack-dev-middleware)
* [Substituição do módulo ativa](#hot-module-replacement)
* [Roteamentos auxiliares](#routing-helpers)

Coletivamente, esses componentes de infraestrutura melhoram o fluxo de trabalho de desenvolvimento e a experiência de tempo de execução. Os componentes podem ser adotados individualmente.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Pré-requisitos para usar SpaServices

Para trabalhar com SpaServices, instale o seguinte:
* [Node. js](https://nodejs.org/) (versão 6 ou posterior) com npm
    * Para verificar se esses componentes estão instalados e podem ser encontrados, execute o seguinte na linha de comando:

    ```console
    node -v && npm -v
    ```

Observação: Se você estiver implantando em um site do Azure, você não precisa fazer nada aqui &mdash; Node. js está instalado e disponível em ambientes de servidor.

* [SDK do .NET core](https://www.microsoft.com/net/download/core) 1.0 (ou posterior)
    * Se você estiver no Windows, isso pode ser instalado, selecionando o Visual Studio 2017 **desenvolvimento de plataforma cruzada do .NET Core** carga de trabalho.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) pacote NuGet

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Pré-processamento do lado do servidor

Um universal (também conhecido como isomórficos) é um aplicativo JavaScript capaz de executar tanto no servidor e o cliente. Angular, reagir e outras estruturas populares fornecem uma plataforma universal para esse estilo de desenvolvimento de aplicativo. A ideia é primeiro renderizar os componentes do framework no servidor por meio do Node. js e, em seguida, delegado ainda mais a execução para o cliente.

ASP.NET Core [auxiliares de marcação](xref:mvc/views/tag-helpers/intro) fornecida pelo SpaServices simplificar a implementação de pré-processamento do lado do servidor ao chamar as funções JavaScript no servidor.

### <a name="prerequisites"></a>Pré-requisitos

Instale o seguinte:
* [pré-processamento ASPNET](https://www.npmjs.com/package/aspnet-prerendering) pacote npm:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Configuração

Os auxiliares de marca são feitos detectáveis por meio de registro de namespace do projeto *viewimports. cshtml* arquivo:

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Esses auxiliares de marcação abstrair a complexidade de se comunicar diretamente com as APIs de baixo nível utilizando uma sintaxe semelhante do HTML no modo de exibição Razor:

[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>O `asp-prerender-module` auxiliar de marca

O `asp-prerender-module` auxiliar de marca, usados no exemplo de código anterior, executa *ClientApp/dist/main-server.js* no servidor por meio do Node. js. Para clareza, *principal server.js* arquivo é um artefato da tarefa transpilation TypeScript e JavaScript no [Webpack](http://webpack.github.io/) do processo de compilação. Webpack define um alias de ponto de entrada de `main-server`; e, passagem do gráfico de dependência para este alias começa a *inicialização/ClientApp-server.ts* arquivo:

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

No exemplo a seguir Angular, o *inicialização/ClientApp-server.ts* arquivo utiliza o `createServerRenderer` função e `RenderResult` tipo do `aspnet-prerendering` pacote npm para configurar o processamento de servidor por meio do Node. js. A marcação HTML destinada a renderização do lado do servidor é passada para uma chamada de função de resolução, que é encapsulada em um JavaScript fortemente tipada `Promise` objeto. O `Promise` significância do objeto é que ele fornece assincronamente a marcação HTML para a página para inclusão no elemento de espaço reservado do DOM.

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>O `asp-prerender-data` auxiliar de marca

Quando combinado com o `asp-prerender-module` auxiliar de marca, o `asp-prerender-data` auxiliar de marca pode ser usado para passar informações contextuais da exibição do Razor para o JavaScript do lado do servidor. Por exemplo, a seguinte marcação transmite dados de usuário para o `main-server` módulo:

[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Recebido `UserName` argumento é serializado usando o serializador JSON interno e é armazenado no `params.data` objeto. No exemplo a seguir Angular, os dados são usados para construir uma saudação personalizada em um `h1` elemento:

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Observação: Os nomes de propriedade passados auxiliares de marca são representados com **PascalCase** notação. Compare isso com a do JavaScript, onde os mesmos nomes de propriedade são representados com **camelCase**. A configuração de serialização JSON padrão é responsável por essa diferença.

Para expandir o exemplo de código anterior, dados podem ser transmitidos do servidor para o modo de exibição por hydrating o `globals` propriedade fornecida para o `resolve` função:

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

O `postList` matriz definida dentro do `globals` objeto está anexado no navegador global `window` objeto. Essa variável suspensão em escopo global elimina a duplicação de esforço, particularmente pois ela pertence ao carregar os mesmos dados uma vez no servidor e novamente no cliente.

![variável global de postList anexado ao objeto de janela](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Middleware de desenvolvimento webpack

[Middleware de desenvolvimento webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) introduz um fluxo de trabalho de desenvolvimento simplificado por meio do qual Webpack cria recursos sob demanda. O middleware automaticamente compila e serve de recursos do cliente quando uma página é recarregada no navegador. A abordagem alternativa é invocar Webpack manualmente por meio do script de compilação do projeto npm quando uma dependência de terceiros ou o código personalizado é alterado. Script de compilação de um npm *Package. JSON* arquivo é mostrado no exemplo a seguir:

[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>Pré-requisitos

Instale o seguinte:
* [ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pacote npm:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Configuração

Middleware de desenvolvimento webpack está registrado no pipeline de solicitação HTTP por meio de código a seguir no *Startup.cs* do arquivo `Configure` método:

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

O `UseWebpackDevMiddleware` método de extensão deve ser chamado antes de [Registrando a hospedagem de arquivo estático](xref:fundamentals/static-files) por meio de `UseStaticFiles` método de extensão. Por motivos de segurança, registre o middleware somente quando o aplicativo é executado no modo de desenvolvimento.

O *webpack.config.js* do arquivo `output.publicPath` propriedade informa o middleware para observar o `dist` pasta alterações:

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Substituição do módulo ativa

Pense do Webpack [módulo substituições](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) recurso (HMR) como uma evolução do [Webpack desenvolvimento Middleware](#webpack-dev-middleware). HMR apresenta os mesmos benefícios, mas ele simplifica ainda mais o fluxo de trabalho de desenvolvimento por atualizar automaticamente o conteúdo da página depois de compilar as alterações. Não confunda isso com uma atualização do navegador, o que poderia interferir com o estado de memória atual e a sessão de depuração do SPA. Há um vínculo dinâmico entre o serviço de Middleware de desenvolvimento Webpack e o navegador, o que significa que as alterações são ~ simplesmente outra palavra proibida ~ enviada por push para o navegador.

### <a name="prerequisites"></a>Pré-requisitos

Instale o seguinte:
* [middleware hot webpack](https://www.npmjs.com/package/webpack-hot-middleware) pacote npm:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Configuração

O componente HMR deve ser registrado no pipeline de solicitação HTTP do MVC no `Configure` método:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Como ocorria no [Webpack desenvolvimento Middleware](#webpack-dev-middleware), o `UseWebpackDevMiddleware` método de extensão deve ser chamado antes do `UseStaticFiles` método de extensão. Por motivos de segurança, registre o middleware somente quando o aplicativo é executado no modo de desenvolvimento.

O *webpack.config.js* arquivo deve definir um `plugins` de matriz, mesmo se ele for deixado em branco:

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Depois de carregar o aplicativo no navegador, a guia Console de ferramentas de desenvolvedor fornece confirmação de ativação de HMR:

![Hot mensagem conectada de substituição do módulo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Roteamentos auxiliares

Na maioria dos SPAs baseado em núcleo do ASP.NET, você vai querer roteamento do lado do cliente além do roteamento do lado do servidor. Os sistemas de roteamento SPA e MVC podem trabalhar de forma independente, sem interferência. No entanto, há um caso de borda apresentando desafios: identificar as respostas HTTP 404.

Considere o cenário no qual uma rota sem extensão de `/some/page` é usado. Suponha que a solicitação não-correspondência de padrão uma rota do lado do servidor, mas o padrão corresponde a uma rota de cliente. Agora, considere uma solicitação de entrada para `/images/user-512.png`, que geralmente espera encontrar um arquivo de imagem no servidor. Se esse caminho de recurso solicitado não corresponde a qualquer rota do lado do servidor ou um arquivo estático, é improvável que o aplicativo cliente deve tratá-la, você geralmente deseja retornar um código de status HTTP 404.

### <a name="prerequisites"></a>Pré-requisitos

Instale o seguinte:
* O pacote npm de roteamento do lado do cliente. Usando Angular como um exemplo:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Configuração

Um método de extensão denominado `MapSpaFallbackRoute` é usado no `Configure` método:

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Dica: As rotas são avaliadas na ordem em que eles foram configurados. Consequentemente, o `default` rota no exemplo de código anterior é usada pela primeira vez para correspondência de padrões.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Criar um novo projeto

JavaScriptServices fornece modelos de aplicativo pré-configurado. SpaServices é usada nesses modelos, junto com diferentes estruturas e bibliotecas como Angular, Aurelia, Knockout, reagir e Vue.

Esses modelos podem ser instalados por meio da CLI do .NET Core executando o seguinte comando:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Uma lista de modelos disponíveis do SPA é exibida:

| Modelos                                 | Nome curto | Linguagem | Marcas        |
|:------------------------------------------|:-----------|:---------|:------------|
| Núcleo do ASP.NET MVC com Angular             | angular    | [C#]     | MVC/Web/SPA |
| Núcleo do ASP.NET MVC com Aurelia             | aurelia    | [C#]     | MVC/Web/SPA |
| Núcleo do ASP.NET MVC com Knockout. js         | separação   | [C#]     | MVC/Web/SPA |
| Núcleo do ASP.NET MVC com React.js            | react      | [C#]     | MVC/Web/SPA |
| Núcleo do ASP.NET MVC com React.js e retorno  | reactredux | [C#]     | MVC/Web/SPA |
| Núcleo do ASP.NET MVC com Vue.js              | VUE        | [C#]     | MVC/Web/SPA | 

Para criar um novo projeto usando um dos modelos de SPA, inclua o **nome curto** do modelo no `dotnet new` comando. O comando a seguir cria um aplicativo Angular com ASP.NET MVC de núcleo configurado para o lado do servidor:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Definir o modo de configuração de tempo de execução

Existem dois modos de configuração de tempo de execução principal:
* **Desenvolvimento**:
    * Inclui mapas de origem para facilitar a depuração.
    * Não Otimize o código do lado do cliente para o desempenho.
* **Produção**:
    * Exclui os mapas de origem.
    * Otimiza o código do lado do cliente por meio de empacotamento e minimização.

ASP.NET Core usa uma variável de ambiente denominada `ASPNETCORE_ENVIRONMENT` para armazenar o modo de configuração. Consulte  **[Configurando o ambiente](xref:fundamentals/environments#setting-the-environment)**  para obter mais informações.

### <a name="running-with-net-core-cli"></a>Executando com .NET Core CLI

Restaure o NuGet necessário e os pacotes de npm executando o seguinte comando na raiz do projeto:

```console
dotnet restore && npm i
```

Compilar e executar o aplicativo:

```console
dotnet run
```

O aplicativo for iniciado no localhost de acordo com o [modo de configuração de tempo de execução](#runtime-config-mode). Navegar para `http://localhost:5000` no navegador exibe a página inicial.

### <a name="running-with-visual-studio-2017"></a>Executando o Visual Studio de 2017

Abra o *. csproj* arquivo gerado pelo `dotnet new` comando. Os pacotes NuGet e npm necessários são restaurados automaticamente após a abertura do projeto. Esse processo de restauração pode demorar alguns minutos e o aplicativo está pronto para ser executado quando ele for concluído. Clique no botão verde de execução ou pressione `Ctrl + F5`, e o navegador abre a página de aterrissagem do aplicativo. O aplicativo é executado no localhost de acordo com o [modo de configuração de tempo de execução](#runtime-config-mode). 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>O aplicativo de teste

Modelos de SpaServices são pré-configurados para executar testes de cliente usando [Karma](https://karma-runner.github.io/1.0/index.html) e [Jasmine](https://jasmine.github.io/). Jasmine é uma unidade popular de estrutura de teste para JavaScript, enquanto Karma é um executor de teste para os testes. Karma está configurado para funcionar com o [Webpack desenvolvimento Middleware](#webpack-dev-middleware) , de modo que você não precisa parar e o teste seja executado sempre que forem feitas alterações. Se o código em execução no caso de teste ou o caso de teste, o teste será executado automaticamente.

Usando o aplicativo Angular como exemplo, dois casos de teste Jasmine já são fornecidos para o `CounterComponent` no *counter.component.spec.ts* arquivo:

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Abra o prompt de comando na raiz do projeto e execute o seguinte comando:

```console
npm test
```

O script inicia o executor de testes Karma, que lê as configurações definidas de *karma.conf.js* arquivo. Entre outras configurações, o *karma.conf.js* identifica os arquivos de teste para ser executado por meio de seu `files` matriz:

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Publicando o aplicativo

Combinando os ativos do lado do cliente gerados e os artefatos do ASP.NET Core publicados em um pacote pronto para implantar pode ser trabalhoso. Felizmente, SpaServices orquestra esse processo de publicação inteira com um destino do MSBuild personalizado chamado `RunWebpack`:

[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

O destino do MSBuild tem as seguintes responsabilidades:
1. Restaure os pacotes de npm
1. Crie uma compilação de nível de produção dos ativos de terceiros, do lado do cliente
1. Crie uma compilação de nível de produção dos ativos do lado do cliente personalizados
1. Copie os ativos Webpack gerado para a pasta de publicação

O destino do MSBuild é chamado durante a execução:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Recursos adicionais

* [Documentos angulares](https://angular.io/docs)
