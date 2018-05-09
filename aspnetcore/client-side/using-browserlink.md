---
title: Link do navegador no ASP.NET Core
author: ncarandini
description: Explica como o Link do navegador é um recurso do Visual Studio que vincula o ambiente de desenvolvimento com um ou mais navegadores da web.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: 0496f9df35956b8fe7ca9fcc7c03df33437d5a87
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="browser-link-in-aspnet-core"></a>Link do navegador no ASP.NET Core

Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)

Link do navegador é um recurso no Visual Studio que cria um canal de comunicação entre o ambiente de desenvolvimento e um ou mais navegadores da web. Você pode usar o Link do navegador para atualizar seu aplicativo da web em vários navegadores de uma vez, que é útil para testes entre navegadores.

## <a name="browser-link-setup"></a>Instalação de Link do navegador

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O ASP.NET Core 2. x **aplicativo Web**, **vazio**, e **API da Web** modelo projetos usam o [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) pacote meta, que contém uma referência de pacote para [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Portanto, usando o `Microsoft.AspNetCore.All` pacote meta não requer nenhuma ação adicional para disponibilizar o Link do navegador para uso.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O ASP.NET Core 1. x **aplicativo Web** modelo de projeto possui uma referência de pacote para o [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacote. O **vazio** ou **API da Web** projetos de modelo exigem que você adicione uma referência de pacote para `Microsoft.VisualStudio.Web.BrowserLink`.

Como este é um recurso do Visual Studio, a maneira mais fácil para adicionar o pacote a um **vazio** ou **API da Web** projeto modelo é abrir o **Package Manager Console** (**Exibição** > **outras janelas** > **Package Manager Console**) e execute o seguinte comando:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Como alternativa, você pode usar **NuGet Package Manager**. Clique no nome do projeto em **Solution Explorer** e escolha **gerenciar pacotes NuGet**:

![Abra NuGet Package Manager](using-browserlink/_static/open-nuget-package-manager.png)

Localizar e instalar o pacote:

![Adicionar o pacote com o NuGet Package Manager](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>Configuração

No `Configure` método o *Startup.cs* arquivo:

```csharp
app.UseBrowserLink();
```

Geralmente o código está dentro de um `if` bloco que permite apenas o Link de navegador no ambiente de desenvolvimento, como mostrado aqui:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Para obter mais informações, consulte [usar vários ambientes](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Como usar o Link do navegador

Quando um projeto do ASP.NET Core está aberto, o Visual Studio mostra o controle de barra de ferramentas do Link do navegador ao lado do controle de barra de ferramentas do **Destino de Depuração**:

![Menu de lista suspensa de Link do navegador](using-browserlink/_static/browserLink-dropdown-menu.png)

O controle de barra de ferramentas Link do navegador, você pode:

* Atualizar o aplicativo Web em vários navegadores de uma vez.
* Abrir o **Painel de link do navegador**.
* Habilitar ou desabilitar **Link do navegador**. Observação: O Link do navegador está desabilitado por padrão no Visual Studio 2017 (15,3).
* Habilitar ou desabilitar [sincronização automática de CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Alguns plug-ins do Visual Studio, mais notoriamente *Web 2015 de pacote de extensão* e *Web 2017 de pacote de extensão*, oferecem funcionalidade estendida para o Link do navegador, mas alguns dos recursos adicionais não funcionam com o ASP. Projetos de rede principal.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Atualizar o aplicativo web em vários navegadores de uma vez

Para escolher um único navegador Web para abrir ao iniciar o projeto, use o menu suspenso do controle de barra de ferramentas do **Destino de depuração**:

![Menu suspenso de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Para abrir vários navegadores de uma vez, escolha **procurar com...**  da mesma lista suspensa. Mantenha pressionada a tecla CTRL para selecionar os navegadores que você deseja e, em seguida, clique em **procurar**:

![Abrir vários navegadores de uma vez](using-browserlink/_static/open-many-browsers-at-once.png)

Aqui está uma captura de tela mostrando o Visual Studio com o modo de exibição do índice aberto e dois navegadores abertas:

![Sincronizar com o exemplo de dois navegadores](using-browserlink/_static/sync-with-two-browsers-example.png)

Passe o mouse sobre o controle de barra de ferramentas de Link do navegador para ver os navegadores que estão conectados ao projeto:

![Passe o mouse dica](using-browserlink/_static/hoover-tip.png)

Alterar a exibição do índice, e todos os navegadores conectados são atualizados quando você clicar no botão de atualização de Link do navegador:

![sincronização de navegadores de alterações](using-browserlink/_static/browsers-sync-to-changes.png)

Link do navegador também funciona com navegadores que você inicia a partir de fora do Visual Studio e navegue até a URL do aplicativo.

### <a name="the-browser-link-dashboard"></a>O painel de Link do navegador

Abra o painel de Link do navegador de menu para gerenciar a conexão com o navegador abertas suspenso Link do navegador:

![Painel de browserslink abrir](using-browserlink/_static/open-browserlink-dashboard.png)

Se nenhum navegador estiver conectado, você pode iniciar uma sessão de depuração não, selecionando Se nenhum navegador estiver conectado, você poderá iniciar uma sessão de não depuração selecionando o link *exibir no navegador*:

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Caso contrário, os navegadores conectados são mostrados com o caminho para a página que está mostrando cada navegador:

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Se desejar, você pode clicar em um nome de navegador listados para atualizar o navegador único.

### <a name="enable-or-disable-browser-link"></a>Habilitar ou desabilitar o Link do navegador

Quando você habilitar novamente o Link do navegador depois de desabilitá-lo, você deve atualizar os navegadores para reconectar-se-los.

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

Como o código do lado do navegador é todo JavaScript, ele funciona em todos os navegadores que oferece suporte a SignalR sem a necessidade de um plug-in de navegador.
