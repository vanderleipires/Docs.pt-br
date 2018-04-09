---
title: Use o modelo de projeto de reagir com ASP.NET Core
author: SteveSandersonMS
description: Saiba como começar a usar o modelo de projeto de aplicativo de página única (SPA) do ASP.NET Core para reagir e criar reagir-aplicativo.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 4dcfef2bbb99873a9d716a4942f39123944f495c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-project-template-with-aspnet-core"></a>Use o modelo de projeto de reagir com ASP.NET Core

> [!NOTE]
> Esta documentação não está sobre o modelo de projeto reagir incluída no ASP.NET 2.0 de núcleo. Trata-se o modelo de reagir mais recente para o qual você pode atualizar manualmente. O modelo é incluído no ASP.NET Core 2.1 por padrão.

O modelo de projeto reagir atualizado fornece um ponto inicial conveniente para ASP.NET Core aplicativos usando reagir e [criar reagir-aplicativo](https://github.com/facebookincubator/create-react-app) convenções (CRA) para implementar uma interface de usuário do lado do cliente avançado (IU).

O modelo é equivalente à criação de um projeto do ASP.NET Core para atuar como um back-end de API e um projeto padrão CRA reagir para atuar como uma interface do usuário, mas com a conveniência de hospedagem em um projeto de aplicativo único que pode ser criado e publicado como uma única unidade.

## <a name="create-a-new-app"></a>Criar um novo aplicativo

Se usar o ASP.NET 2.0 de núcleo, certifique-se de que você [instalado o modelo de projeto reagir atualizado](xref:spa/index#installation). Se você tiver o ASP.NET Core 2.1, não é necessário instalá-lo.

Criar um novo projeto a partir de um prompt de comando usando o comando `dotnet new react` em um diretório vazio. Por exemplo, os seguintes comandos criam o aplicativo em um *aplicativo my-novo* diretório e alterne para o diretório:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Execute o aplicativo do Visual Studio ou o .NET Core CLI:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Abra o gerado *. csproj* de arquivo e executar o aplicativo como normal de lá.

O processo de compilação restaura npm dependências na primeira execução, o que pode levar vários minutos. Compilações subsequentes são muito mais rápidas.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Verifique se você tem uma variável de ambiente chamada `ASPNETCORE_Environment` com valor de `Development`. No Windows (no prompt do PowerShell não), execute `SET ASPNETCORE_Environment=Development`. No Linux ou macOS, execute `export ASPNETCORE_Environment=Development`.

Executar [dotnet build](/dotnet/core/tools/dotnet-build) verificar se seu aplicativo cria corretamente. Na primeira execução, o processo de compilação restaura dependências npm, o que podem levar vários minutos. Compilações subsequentes são muito mais rápidas.

Executar [dotnet executar](/dotnet/core/tools/dotnet-run) para iniciar o aplicativo.

---

O modelo de projeto cria um aplicativo do ASP.NET Core e um aplicativo reagir. O aplicativo do ASP.NET Core destina-se a ser usado para acesso a dados, autorização e outras questões do lado do servidor. O aplicativo reagir, que residem no *ClientApp* subdiretório, destina-se a ser usado para todas as questões de interface do usuário.

## <a name="add-pages-images-styles-modules-etc"></a>Adicione páginas, imagens, estilos, módulos, etc.

O *ClientApp* diretório é um aplicativo de reagir CRA padrão. Consulte o oficial [documentação CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) para obter mais informações.

Há pequenas diferenças entre o aplicativo reagir criado por este modelo e um criado por CRA em si; No entanto, os recursos do aplicativo são inalterados. O aplicativo criado pelo modelo contém um [Bootstrap](https://getbootstrap.com/)-com base em layout e um exemplo básico de roteamento.

## <a name="install-npm-packages"></a>Instalar pacotes npm

Para instalar pacotes de terceiros npm, use um prompt de comando no *ClientApp* subdiretório. Por exemplo:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publicar e implantar

No desenvolvimento, o aplicativo é executado em um modo otimizado para conveniência do desenvolvedor. Por exemplo, pacotes de JavaScript incluem mapas de origem (de modo que durante a depuração, você pode ver o código-fonte original). O aplicativo observa JavaScript, HTML e CSS alterações de arquivo no disco e recompila automaticamente e recarrega quando ele vê esses arquivos alterar.

Em produção, atende a uma versão de seu aplicativo que é otimizado para desempenho. Isso é configurado para ocorrer automaticamente. Quando você publica, a configuração de build emite uma compilação transpiled minimizada, do seu código do lado do cliente. Diferentemente de compilação de desenvolvimento, a compilação de produção não requer Node. js ser instalado no servidor.

Você pode usar o padrão [métodos de implantação e hospedagem ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-the-cra-server-independently"></a>Executar o servidor CRA independentemente

O projeto está configurado para iniciar sua própria instância do servidor de desenvolvimento CRA em segundo plano quando o aplicativo ASP.NET Core é iniciado no modo de desenvolvimento. Isso é conveniente, porque isso significa que você não precisa executar um servidor separado manualmente.

Há uma desvantagem para essa configuração padrão. Cada vez que você modificar seu código c# e o ASP.NET Core aplicativo precisa ser reiniciado, o servidor CRA for reiniciado. Alguns segundos são necessários para iniciar um backup. Se você estiver fazendo frequentes edições de código c# e não quiser esperar o servidor CRA reiniciar, execute o servidor CRA externamente, independentemente do processo do ASP.NET Core. Para fazer isso:

1. No prompt de comando, alterne para o *ClientApp* subdiretório e iniciar o servidor de desenvolvimento CRA:

    ```console
    cd ClientApp
    npm start
    ```

2. Modifique seu aplicativo ASP.NET Core para usar a instância do servidor CRA externa em vez de iniciar um dos seus próprios. No seu *inicialização* classe, substitua o `spa.UseReactDevelopmentServer` chamada com o seguinte:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

Quando você iniciar seu aplicativo ASP.NET Core, ele não é possível abrir um servidor CRA. Em vez disso, é usada a instância que é iniciado manualmente. Isso permite iniciar e reiniciar mais rapidamente. Ele não esteja mais aguardando para seu aplicativo reagir recriar a cada vez.
