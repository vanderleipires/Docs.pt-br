---
title: Use o modelo de projeto Angular
author: SteveSandersonMS
description: "Saiba como começar a usar o modelo de projeto do ASP.NET Core única página aplicativo (SPA) release candidate para Angular e a CLI Angular."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: b54798a43f6a448c2e2aad0613ee60805a61f303
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
# <a name="use-the-angular-project-template-release-candidate"></a>Use o modelo de projeto Angular (versão release candidate)

> [!NOTE]
> Esta documentação não é sobre o modelo de projeto Angular lançada. **Esta documentação é sobre a versão release candidate do modelo Angular.** Esperamos que acompanham a versão de lançamento antecipado 2018.

O modelo de projeto Angular atualizado fornece um ponto inicial conveniente para ASP.NET Core aplicativos Angular 5 e a CLI Angular para implementar uma interface de usuário do lado do cliente avançado (IU).

O modelo é equivalente à criação de um projeto do ASP.NET Core para atuar como um back-end de API e um projeto de CLI Angular para atuar como uma interface do usuário. O modelo oferece a conveniência de hospedagem de ambos os tipos de projeto em um projeto de aplicativo único. Consequentemente, o projeto de aplicativo pode ser criado e publicado como uma única unidade.

## <a name="create-a-new-app"></a>Criar um novo aplicativo

Para começar, certifique-se de que você [o modelo de projeto Angular atualizado instalado](xref:spa/index#installation). Estas instruções não se aplicam ao modelo de projeto Angular anterior incluído no núcleo do .NET 2.0. x SDK.

Criar um novo projeto a partir de um prompt de comando usando o comando `dotnet new angular` em um diretório vazio. Por exemplo, os seguintes comandos criam o aplicativo em um *aplicativo my-novo* diretório e alterne para o diretório:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Execute o aplicativo do Visual Studio ou o .NET Core CLI:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Abra o gerado *. csproj* de arquivo e executar o aplicativo como normal de lá.

O processo de compilação restaura npm dependências na primeira execução, o que pode levar vários minutos. Compilações subsequentes são muito mais rápidas.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Verifique se você tem uma variável de ambiente chamada `ASPNETCORE_Environment` com um valor de `Development`. No Windows (no prompt do PowerShell não), execute `SET ASPNETCORE_Environment=Development`. No Linux ou macOS, execute `export ASPNETCORE_Environment=Development`.

Execute `dotnet build` verificar se o aplicativo cria corretamente. Na primeira execução, o processo de compilação restaura dependências npm, o que podem levar vários minutos. Compilações subsequentes são muito mais rápidas.

Execute `dotnet run` para iniciar o aplicativo. Uma mensagem semelhante à seguinte será registrada:

```console
Now listening on: http://localhost:<port>
```

Navegue até essa URL em um navegador.

O aplicativo inicia uma instância do servidor Angular CLI em segundo plano. Uma mensagem semelhante à seguinte será registrada: *NG Live Development Server está escutando no localhost:&lt;otherport&gt;, abra seu navegador em http://localhost:&lt;otherport&gt; /* . Ignorar essa mensagem&mdash;tem **não** a URL para o aplicativo ASP.NET Core e a CLI Angular combinado.

---

O modelo de projeto cria um aplicativo do ASP.NET Core e um aplicativo Angular. O aplicativo do ASP.NET Core destina-se a ser usado para acesso a dados, autorização e outras questões do lado do servidor. O aplicativo Angular, que residem no *ClientApp* subdiretório, destina-se a ser usado para todas as questões de interface do usuário.

## <a name="add-pages-images-styles-modules-etc"></a>Adicione páginas, imagens, estilos, módulos, etc.

O *ClientApp* diretório contém um aplicativo de CLI Angular padrão. Consulte o oficial [documentação Angular](https://github.com/angular/angular-cli/wiki) para obter mais informações.

Há pequenas diferenças entre o aplicativo Angular criado por este modelo e criado pela CLI Angular em si (via `ng new`); no entanto, os recursos do aplicativo foram modificados. O aplicativo criado pelo modelo contém um [Bootstrap](https://getbootstrap.com/)-com base em layout e um exemplo básico de roteamento.

## <a name="run-ng-commands"></a>Execute comandos ng

No prompt de comando, alterne para o *ClientApp* subdiretório:

```console
cd ClientApp
```

Se você tiver o `ng` ferramenta instalada globalmente, você pode executar qualquer um dos seus comandos. Por exemplo, você pode executar `ng lint`, `ng test`, ou quaisquer outras [comandos CLI Angular](https://github.com/angular/angular-cli/wiki#additional-commands). Não é necessário para executar `ng serve` , como seu aplicativo ASP.NET Core lida com que atende ao lado do servidor e cliente partes do seu aplicativo. Internamente, ele usa `ng serve` em desenvolvimento.

Se você não tiver o `ng` ferramenta instalada, execute `npm run ng` em vez disso. Por exemplo, você pode executar `npm run ng lint` ou `npm run ng test`.

## <a name="install-npm-packages"></a>Instalar pacotes de npm

Para instalar pacotes de terceiros npm, use um prompt de comando no *ClientApp* subdiretório. Por exemplo:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publicar e implantar

No desenvolvimento, o aplicativo é executado em um modo otimizado para conveniência do desenvolvedor. Por exemplo, pacotes de JavaScript incluem mapas de origem (de modo que durante a depuração, você pode ver o código de TypeScript original). O aplicativo observa TypeScript, HTML e CSS alterações de arquivo no disco e recompila automaticamente e recarrega quando ele vê esses arquivos alterar.

Em produção, atende a uma versão de seu aplicativo que é otimizado para desempenho. Isso é configurado para ocorrer automaticamente. Quando você publica, a configuração de build emite um minimizada, antecipada de tempo (AoT) compilados compilação do código do lado do cliente. Diferentemente de compilação de desenvolvimento, a compilação de produção não requer Node. js ser instalado no servidor (a menos que você habilitou [pré-processamento do lado do servidor](#server-side-rendering)).

Você pode usar o padrão [métodos de implantação e hospedagem ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>Executar "ng sirvam" independentemente

O projeto está configurado para iniciar sua própria instância do servidor Angular CLI em segundo plano quando o aplicativo ASP.NET Core é iniciado no modo de desenvolvimento. Isso é conveniente, porque você não precisa executar um servidor separado manualmente.

Há uma desvantagem para essa configuração padrão. Cada vez que você modificar seu código c# e o ASP.NET Core aplicativo precisa ser reiniciado, o servidor de CLI Angular for reiniciado. Cerca de 10 segundos é necessária para iniciar um backup. Se você estiver fazendo frequentes edições de código c# e não quiser esperar Angular CLI reiniciar, execute a CLI Angular servidor externamente, independentemente do processo do ASP.NET Core. Para fazer isso:

1. No prompt de comando, alterne para o *ClientApp* subdiretório e iniciar o servidor de desenvolvimento Angular CLI:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Use `npm start` para iniciar o servidor de desenvolvimento de CLI Angular não `ng serve`, de modo que a configuração no *Package. JSON* é respeitado. Para passar parâmetros adicionais para o servidor de CLI Angular, adicioná-los ao relevante `scripts` de linha em sua *Package. JSON* arquivo.

2. Modifique seu aplicativo ASP.NET Core para usar a instância de CLI Angular externa em vez de iniciar um dos seus próprios. No seu *inicialização* classe, substitua o `spa.UseAngularCliServer` chamada com o seguinte:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Quando você iniciar seu aplicativo ASP.NET Core, ele não é possível abrir um servidor Angular CLI. Em vez disso, é usada a instância que é iniciado manualmente. Isso permite iniciar e reiniciar mais rapidamente. Ele não está aguardando Angular CLI recriar seu aplicativo cliente a cada vez.

## <a name="server-side-rendering"></a>Renderização do lado do servidor

Como um recurso de desempenho, você pode escolher a pré-processar seu aplicativo Angular no servidor, bem como executá-lo no cliente. Isso significa que os navegadores recebam uma marcação HTML que representa a interface de usuário inicial do seu aplicativo, para exibirem mesmo antes de baixar e executar seus pacotes de JavaScript. A maioria da implementação disso vêm de um recurso Angular chamado [Angular Universal](https://universal.angular.io/).

> [!TIP]
> Habilitar renderização do lado do servidor (SSR) apresenta uma série de complicações extras tanto durante o desenvolvimento e implantação. Leitura [desvantagens SSR](#drawbacks-of-ssr) para determinar se SSR é uma boa opção para suas necessidades.

Para habilitar SSR, você precisa fazer um número de adições ao seu projeto.

No *inicialização* classe *depois* a linha que configura `spa.Options.SourcePath`, e *antes de* a chamada para `UseAngularCliServer` ou `UseProxyToSpaDevelopmentServer`, adicione o seguinte:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

No modo de desenvolvimento, esse código tentará criar o pacote SSR executando o script `build:ssr`, que é definido em *ClientApp\package.json*. Isso cria um aplicativo Angular chamado `ssr`, que ainda não está definido. 

No final do `apps` de matriz em *ClientApp/.angular-cli.json*, definir um aplicativo adicional com o nome `ssr`. Use as seguintes opções:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Essa nova configuração de aplicativo habilitado para SSR requer dois arquivos adicionais: *tsconfig.server.json* e *main.server.ts*. O *tsconfig.server.json* arquivo Especifica opções de compilação de TypeScript. O *main.server.ts* arquivo serve como o ponto de entrada de código durante SSR.

Adicionar um novo arquivo chamado *tsconfig.server.json* dentro de *ClientApp/src* (junto com o existente *tsconfig.app.json*), que contém o seguinte:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Esse arquivo configura o compilador de AoT do Angular para procurar por um módulo chamado `app.server.module`. Adicionar isso ao criar um novo arquivo em *ClientApp/src/app/app.server.module.ts* (junto com o existente *app.module.ts*) que contém o seguinte: 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Esse módulo herda de seu cliente `app.module` e define quais módulos extra Angular estão disponíveis durante SSR.

Lembre-se de que o novo `ssr` entrada *.angular cli.json* referenciado de um arquivo de ponto de entrada chamado *main.server.ts*. Você ainda não adicionou esse arquivo e agora é hora de fazê-lo. Criar um novo arquivo em *ClientApp/src/main.server.ts* (junto com o existente *main.ts*), que contém o seguinte:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Deste arquivo é código ASP.NET Core executado para cada solicitação quando ele é executado o `UseSpaPrerendering` middleware que você adicionou para a *inicialização* classe. Trata de recebimento `params` o código .NET (por exemplo, a URL que está sendo solicitada) e fazer chamadas a APIs de SSR Angular para obter o HTML resultante. 

Rigor, isso é suficiente para habilitar SSR no modo de desenvolvimento. É essencial para fazer uma alteração final para que seu aplicativo funcione corretamente quando publicados. No principal do seu aplicativo *. csproj* de arquivo, defina o `BuildServerSideRenderer` valor da propriedade `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Isso configura o processo de compilação para executar `build:ssr` durante a publicação e implantar os arquivos SSR no servidor. Se você não habilitar isso, SSR falhará em produção.

Quando seu aplicativo é executado no modo de desenvolvimento ou de produção, o código Angular é previamente renderizada como HTML no servidor. O código do lado do cliente é executada normalmente.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Passar dados de código .NET no código do TypeScript

Durante SSR, convém passar dados de cada solicitação de seu aplicativo ASP.NET Core em seu aplicativo Angular. Por exemplo, você pode transmitir informações de cookie ou algo ler de um banco de dados. Para fazer isso, edite o *inicialização* classe. No retorno de chamada para `UseSpaPrerendering`, defina um valor para `options.SupplyData` como o seguinte:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that is passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

O `SupplyData` permite que o retorno de chamada é passar arbitrários, por solicitação, dados serializáveis em JSON (por exemplo, cadeias de caracteres, boolianos ou números). O *main.server.ts* código recebe como `params.data`. Por exemplo, o exemplo de código anterior passa um valor booliano como `params.data.isHttpsRequest` para o `createServerRenderer` retorno de chamada. Você pode passar essa a outras partes do seu aplicativo de alguma forma Angular com suporte. Por exemplo, consulte como *main.server.ts* passa o `BASE_URL` valor para qualquer componente cujo construtor está declarado para recebê-lo.

### <a name="drawbacks-of-ssr"></a>Desvantagens de SSR

Nem todos os aplicativos se beneficiam SSR. O principal benefício é percebido desempenho. Os visitantes atingir seu aplicativo em uma conexão de rede lenta ou em dispositivos móveis lenta consulte a interface do usuário inicial rapidamente, mesmo que leva algum tempo para buscar ou para analisar os pacotes de JavaScript. No entanto, muitos SPAs são usados principalmente em redes da empresa internos, rápida em computadores rápidos onde o aplicativo aparece quase instantaneamente.

Ao mesmo tempo, há desvantagens significativas para habilitar SSR. Ele adiciona complexidade ao seu processo de desenvolvimento. O código deve ser executado em dois ambientes diferentes: do lado do cliente e do servidor (em um ambiente de Node. js invocado do ASP.NET Core). Aqui estão algumas coisas a considerar:

* SSR requer uma instalação de Node. js nos servidores de produção. Isso ocorre automaticamente para alguns cenários de implantação, como serviços de aplicativo do Azure, mas não para outras pessoas, como o Azure Service Fabric.
* Habilitando o `BuildServerSideRenderer` faz com que o sinalizador de compilação seu *node_modules* directory para publicar. Esta pasta contém arquivos de 20.000, que aumenta o tempo de implantação.
* Para executar o código em um ambiente de Node. js, ele não pode contar com a existência de navegador JavaScript APIs específicas, como `window` ou `localStorage`. Se seu código (ou alguns biblioteca de terceiros que você faz referência) tenta usar essas APIs, você obterá um erro durante SSR. Por exemplo, não use jQuery porque faz referência a APIs específicas do navegador em vários locais. Para evitar erros, você deve evitar SSR ou evitar APIs ou bibliotecas de navegador específico. Você pode encapsular todas as chamadas para essas APIs em verificações para garantir que eles não são chamados durante SSR. Por exemplo, use uma seleção como o seguinte no código JavaScript ou TypeScript:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
