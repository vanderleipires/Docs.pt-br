---
title: Interface do usuário do Razor reutilizável em bibliotecas de classes com o ASP.NET Core
author: Rick-Anderson
description: Explica como criar a interface do usuário do Razor reutilizável em uma biblioteca de classes.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Crie interface do usuário reutilizável usando o projeto de biblioteca de classes Razor no ASP.NET Core.

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

A exibições, as páginas, os controladores, os modelos de página e os modelos de dados Razor podem ser construídos em uma RCL (Biblioteca de Classes Razor). A RCL pode ser e empacotada e reutilizada. Os aplicativos podem incluir a RCL e substituir as exibições e as páginas que ela contém. Quando uma exibição, uma exibição parcial ou uma página Razor for encontrada no aplicativo Web e na RCL, a marcação Razor (arquivo *.cshtml*) no aplicativo Web terá precedência.

Este recurso requer o [!INCLUDE[](~/includes/2.1-SDK.md)]

[!INCLUDE[](~/includes/2.1.md)]

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Criar uma biblioteca de classes contendo a interface do usuário do Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.
* Selecione **Aplicativo Web ASP.NET Core**.
* Verifique se o **ASP.NET Core 2.1** ou posterior está selecionado.
* Selecione **Biblioteca de Classes Razor** > **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Na linha de comando, execute `dotnet new razorclasslib`. Por exemplo:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Para obter mais informações, confira [dotnet new](/dotnet/core/tools/dotnet-new).

------
Adicione arquivos Razor na RCL.

É recomendável inserir o conteúdo da RCL na pasta *Áreas*. 


## <a name="referencing-razor-class-library-content"></a>Referenciando o conteúdo da Biblioteca de Classes Razor

A RCL pode ser referenciada por:

* Pacote do NuGet. Confira [Criando pacotes do NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Criar e publicar um pacote do NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}.csproj*. Confira [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

### <a name="partial-files-access-in-the-rcl"></a>Acesso a arquivos parciais na RCL

Para o conteúdo externo da RCL, o tempo de execução do ASP.NET Core não pesquisa arquivos parciais na RCL.

Por exemplo, no download do exemplo, a exibição parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* **não** pode ser referenciada em *WebApp1\Pages\About.cshtml*. No entanto, as páginas na RCL (*RazorUIClassLib/* **podem** acessar *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Passo a passo: Criar um projeto de biblioteca de classes Razor e usar um projeto de Páginas Razor

Você pode baixar o [projeto completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) e testá-lo em vez de criá-lo. O download de exemplo contém um código adicional e links que facilitam o teste do projeto. Você pode deixar comentários [nesse problema do GitHub](https://github.com/aspnet/Docs/issues/6098) com suas opiniões sobre os exemplos de download em relação às instruções passo a passo.

### <a name="test-the-download-app"></a>Testar o aplicativo de download

Se você não tiver baixado o aplicativo concluído e preferir criar o projeto do passo a passo, vá para a [próxima seção](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Abra o arquivo *.sln* no Visual Studio. Execute o aplicativo.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Em um prompt de comando no diretório *cli*, crie a RCL e o aplicativo Web.

``` CLI
dotnet build
```

Acesse o diretório *WebApp1* e execute o aplicativo:

``` CLI
dotnet run
```
------

Siga as instruções em [Testar WebApp1](#test)

## <a name="create-a-razor-class-library"></a>Criar uma Biblioteca de Classes Razor

Nesta seção, uma RCL (Biblioteca de Classes Razor) é criada. Arquivos Razor são adicionados à RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Crie o projeto da RCL:

* No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.
* Selecione **Aplicativo Web ASP.NET Core**.
* Nomeie o aplicativo como **RazorUIClassLib**.
* Verifique se o **ASP.NET Core 2.1** ou posterior está selecionado.
* Selecione **Biblioteca de Classes Razor** > **OK**.

Crie aplicativo Web Páginas Razor:

* No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução > **Adicionar** >  **Novo Projeto**.
* Selecione **Aplicativo Web ASP.NET Core**.
* Nomeie o aplicativo como **WebApp1**.
* Verifique se o **ASP.NET Core 2.1** ou posterior está selecionado.
* Selecione **Aplicativo Web** > **OK**.

### <a name="add-razor-files-and-folders-to-the-project"></a>Adicione pastas e arquivos Razor ao projeto.

* Adicione um arquivo Razor de exibição parcial chamado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.
* Substitua a marcação em *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* pelo código a seguir:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Copie o arquivo *_ViewStart.cshtml* do projeto WebApp1 para *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.

  O arquivo [viewstart](xref:mvc/views/layout#running-code-before-each-view) é necessário para usar o layout do projeto Páginas Razor.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Na linha de comando, execute o seguinte:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Os comandos anteriores:

* Criam a RCL (Biblioteca de Classes Razor) `RazorUIClassLib`.
* Criam uma página Razor _Message e a adicionam à RCL. O parâmetro `-np` cria a página sem um `PageModel`.
* Criam um arquivo [viewstart](xref:mvc/views/layout#running-code-before-each-view) e o adicionam à RCL.

O arquivo viewstart é necessário para usar o layout do projeto Páginas Razor (que é adicionado na próxima seção).

Atualize as Páginas Razor:

* Substitua a marcação em *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* pelo código a seguir:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Substitua a marcação em *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* pelo código a seguir:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` é necessário para usar a exibição parcial (`<partial name="_Message" />`). Em vez de incluir a diretiva `@addTagHelper`, você pode adicionar um arquivo *_ViewImports.cshtml*. Por exemplo:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Para obter mais informações sobre viewimports, confira [Importando diretivas compartilhadas](xref:mvc/views/layout#importing-shared-directives)

* Crie a biblioteca de classes para verificar se não há nenhum erro de compilador:

``` CLI
dotnet build RazorUIClassLib
```

A saída do build contém *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* contém o conteúdo Razor compilado.

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Use a biblioteca da interface do usuário do Razor de um projeto Páginas Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* No **Gerenciador de Soluções**, clique com o botão direito do mouse em **WebApp1** e selecione **Definir como projeto de inicialização**.
* No **Gerenciador de Soluções**, clique com o botão direito do mouse em **WebApp1** e selecione **Dependências de Build** > **Dependências do Projeto**.
* Marque **RazorUIClassLib** como uma dependência de **WebApp1**.
* No **Gerenciador de Soluções**, clique com o botão direito do mouse em **WebApp1** e selecione **Adicionar** > **Referência**.
* Na caixa de diálogo **Gerenciador de Referências**, marque **RazorUIClassLib** > **OK**.

Execute o aplicativo.

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli)

Crie um aplicativo Web Páginas Razor e um arquivo de solução contendo o aplicativo Páginas Razor e a Biblioteca de Classes Razor:

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Crie e execute o aplicativo Web:

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a>Testar o WebApp1

Verifique se a biblioteca de classes da interface do usuário do Razor está sendo usada.

* Navegue para `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Substituir exibições, exibições parciais e páginas

Quando uma exibição, uma exibição parcial ou uma Página Razor for encontrada no aplicativo Web e na Biblioteca de Classes Razor, a marcação Razor (arquivo *.cshtml*) no aplicativo Web terá precedência. Por exemplo, adicione *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* ao WebApp1, e a Page1 ao WebApp1 terá precedência sobre a Page1 na Biblioteca de Classes do Razor.

No download de exemplo, renomeie *WebApp1/Areas/MyFeature2* como *WebApp1/Areas/MyFeature* para testar a precedência.

Copie a exibição parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* para *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Atualize a marcação para indicar o novo local. Crie e execute o aplicativo para verificar se a versão da parcial do aplicativo está sendo usada.
