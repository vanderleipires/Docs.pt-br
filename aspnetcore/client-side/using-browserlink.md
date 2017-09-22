---
title: "Link do navegador no núcleo do ASP.NET"
author: ncarandini
description: Um recurso do Visual Studio que vincula o ambiente de desenvolvimento com um ou mais navegadores da web
keywords: "ASP.NET Core, o link do navegador, a sincronização de CSS"
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 211dd5d03e6b8414e0b2ed3234d8970c92f72452
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a>Introdução ao Link de navegador no núcleo do ASP.NET 

Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)

Link do navegador é um recurso no Visual Studio que cria um canal de comunicação entre o ambiente de desenvolvimento e um ou mais navegadores da web. Você pode usar o Link do navegador para atualizar seu aplicativo da web em vários navegadores de uma vez, que é útil para testes entre navegadores.

## <a name="browser-link-setup"></a>Instalação de Link do navegador

O ASP.NET Core **aplicativo Web** projeto modelos no Visual Studio 2015 e posterior incluem todo o necessário para o Link do navegador.

Para adicionar o Link do navegador para um projeto que você criou usando o ASP.NET Core **vazio** ou **API da Web** modelo, siga estas etapas:

1. Adicionar o *Microsoft.VisualStudio.Web.BrowserLink.Loader* pacote 
2. Adicione código de configuração de *Startup.cs* arquivo.

### <a name="add-the-package"></a>Adicionar o pacote

Como esse é um recurso do Visual Studio, a maneira mais fácil para adicionar o pacote é abrir o **Package Manager Console** (**exibição > outras janelas > Package Manager Console**) e execute o seguinte comando:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

Como alternativa, você pode usar **NuGet Package Manager**.  Clique no nome do projeto em **Solution Explorer**e escolha **gerenciar pacotes NuGet**. 

![Abra NuGet Package Manager](using-browserlink/_static/open-nuget-package-manager.png)

Em seguida, localizar e instalar o pacote.

![Adicionar o pacote com o NuGet Package Manager](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a>Adicione código de configuração

Abra o *Startup.cs* arquivo e no `Configure` método adicione o seguinte código:

```csharp
app.UseBrowserLink();
```

Normalmente esse código está dentro de um `if` bloco que permite que o Link do navegador somente no ambiente de desenvolvimento, como mostrado aqui:

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

Para obter mais informações, consulte [Trabalhando com vários ambientes](../fundamentals/environments.md).

## <a name="how-to-use-browser-link"></a>Como usar o Link do navegador

Quando você tiver um projeto do ASP.NET Core aberta, o Visual Studio mostra o controle de barra de ferramentas do Link do navegador ao lado de **destino de depuração** controle de barra de ferramentas:

![Menu de lista suspensa de Link do navegador](using-browserlink/_static/browserLink-dropdown-menu.png)

O controle de barra de ferramentas Link do navegador, você pode:

- Atualizar o aplicativo web em vários navegadores de uma vez
- Abra o **painel de Link do navegador**
- Habilitar ou desabilitar **Link do navegador**
- Habilitar ou desabilitar a sincronização automática de CSS

> [!NOTE]
> Alguns plug-ins do Visual Studio, mais notoriamente *Web 2015 de pacote de extensão* e *Web 2017 de pacote de extensão*, oferecem funcionalidade estendida para o Link do navegador, mas alguns dos recursos adicionais não funcionam com o ASP. Projetos de rede principal.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Atualizar o aplicativo web em vários navegadores de uma vez

Para escolher um navegador web única para iniciar ao iniciar o projeto, use o menu suspenso no **destino de depuração** controle de barra de ferramentas:

![Menu suspenso de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Para abrir vários navegadores de uma vez, escolha **procurar com... ** da mesma lista suspensa.  Mantenha pressionada a tecla CTRL para selecionar os navegadores que você deseja e, em seguida, clique em **procurar**:

![Abrir vários navegadores de uma vez](using-browserlink/_static/open-many-browsers-at-once.png)

Aqui está uma captura de tela de exemplo mostrando o Visual Studio com o modo de exibição do índice aberto e dois navegadores abertas:

![Sincronizar com o exemplo de dois navegadores](using-browserlink/_static/sync-with-two-browsers-example.png)

Passe o mouse sobre o controle de barra de ferramentas de Link do navegador para ver os navegadores que estão conectados ao projeto:

![Passe o mouse dica](using-browserlink/_static/hoover-tip.png)

Alterar a exibição do índice, e todos os navegadores conectados são atualizados quando você clicar no botão de atualização de Link do navegador:

![sincronização de navegadores de alterações](using-browserlink/_static/browsers-sync-to-changes.png)

Link do navegador também funciona com navegadores que você inicia a partir de fora do Visual Studio e navegue até a URL do aplicativo.

### <a name="the-browser-link-dashboard"></a>O painel de Link do navegador

Abra o painel de Link do navegador de menu para gerenciar a conexão com o navegador abertas suspenso Link do navegador:

![Painel de browserslink abrir](using-browserlink/_static/open-browserlink-dashboard.png)

Se nenhum navegador estiver conectado, você pode iniciar a sessão não depuração clicando o _exibir no navegador_ link:

![painel browserlink-conexões não](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Caso contrário, os navegadores conectados são mostrados, com o caminho para a página que está mostrando cada navegador:

![browserlink-painel duas conexões](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Se desejar, você pode clicar em um nome de navegador listados para atualizar o navegador único.

### <a name="enable-or-disable-browser-link"></a>Habilitar ou desabilitar o Link do navegador

Quando você habilitar novamente o Link do navegador depois de desabilitá-lo, você precisa atualizar os navegadores para reconectar-se-los.

### <a name="enable-or-disable-css-auto-sync"></a>Habilitar ou desabilitar a sincronização automática de CSS

Quando a sincronização automática de CSS está habilitada, navegadores conectados são atualizadas automaticamente quando você fizer qualquer alteração nos arquivos CSS.

## <a name="how-does-it-work"></a>Como funciona?

Link do navegador usa SignalR para criar um canal de comunicação entre o navegador e o Visual Studio. Quando o Link do navegador é habilitado, o Visual Studio atua como um servidor de SignalR que vários clientes (navegadores) podem se conectar ao. Link do navegador também registra um componente de middleware no pipeline de solicitação do ASP.NET. Este componente injeta especial `<script>` referências em cada solicitação de página do servidor. Você pode ver as referências de script selecionando **Exibir código-fonte** no navegador e rolar até o final do `<body>` conteúdo de marca:

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Os arquivos de origem não são modificados. O componente de middleware injeta as referências de script dinamicamente. 

Como o código do lado do navegador é todo JavaScript, ele funciona em todos os navegadores com suporte SignalR, sem a necessidade de qualquer plug-in de navegador.
