---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Recursos móveis do ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Agora há uma versão do MVC 5 deste tutorial com exemplos de código em implantar um aplicativo da Web do ASP.NET MVC 5 móveis nos Sites do Azure.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 5b029aa7e87f064622d72feacaf7e97ea4da5cca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384810"
---
<a name="aspnet-mvc-4-mobile-features"></a>Recursos móveis do ASP.NET MVC 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Agora há uma versão do MVC 5 deste tutorial com exemplos de código em [implantar um aplicativo da Web do ASP.NET MVC 5 móveis nos Sites do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Este tutorial ensinará os fundamentos de como trabalhar com recursos móveis em um aplicativo Web do ASP.NET MVC 4. Para este tutorial, você pode usar [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou o Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer ou VWD&quot;). Se você já tiver isso, você pode usar a versão professional do Visual Studio.

Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.

- [O Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recomendado) ou o Visual Studio Web Developer Express SP1. Visual Studio 2012 contém ASP.NET MVC 4. Se você estiver usando o Visual Web Developer 2010, instale [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Você também precisará de um emulador do navegador móvel. Qualquer uma das opções a seguir funcionará:

- [Emulador de telefone do Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Esse é o emulador que é usado na maioria das capturas de tela neste tutorial.)
- Altere a cadeia de caracteres de agente do usuário para emular um iPhone. Ver [isso](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) entrada de blog.
- [Emulador do opera Mobile](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) com o agente do usuário definido para iPhone. Para obter instruções sobre como configurar o agente do usuário no Safari, como "iPhone", consulte [como permitir que o Safari finja que é o IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) no blog de David Alison.

Projetos do Visual Studio com o código-fonte c# estão disponíveis para acompanhar este tópico:

- [Download do projeto inicial](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Download do projeto concluído](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>O que você vai criar

Para este tutorial, você adicionará recursos móveis ao aplicativo de listagem de conferência simples que é fornecido na [projeto starter](https://go.microsoft.com/fwlink/?LinkId=228307). Captura de tela a seguir mostra a página de marcas do aplicativo concluído, como visto na [emulador do Windows Phone 7 Windows](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Ver [mapeamento do teclado para o Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) para simplificar a entrada do teclado.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Você pode usar o Internet Explorer versão 9 ou 10, FireFox ou Chrome para desenvolver seu aplicativo móvel, definindo o [cadeia de caracteres de agente de usuário](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). A imagem a seguir mostra o tutorial concluído usando o Internet Explorer emulando um iPhone. Você pode usar as ferramentas de desenvolvedor do Internet Explorer F-12 e o [ferramenta Fiddler](http://www.fiddler2.com/fiddler2/) para ajudar a depurar seu aplicativo.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Habilidades que você aprenderá

Aqui está o que você aprenderá:

- Como os modelos do ASP.NET MVC 4 usam HTML5 `viewport` atributo e renderização adaptável para melhorar a exibição em dispositivos móveis.
- Como criar exibições específicas para dispositivos móveis.
- Como criar um alternador de alternância de usuários que permite entre uma exibição móvel e uma exibição da área de trabalho do aplicativo.

### <a name="getting-started"></a>Guia de Introdução

Baixe o aplicativo de listagem de conferência para o projeto inicial usando o seguinte link: [baixar](https://go.microsoft.com/fwlink/?LinkId=228307). No Windows Explorer, clique com o *MvcMobile.zip* do arquivo e escolha **propriedades**. No **propriedades MvcMobile.zip** diálogo caixa, escolha o **desbloquear** botão. (Desbloquear impede um aviso de segurança que ocorre quando você tentar usar um *. zip* arquivo que você baixou da web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Clique com botão direito do *MvcMobile.zip* de arquivo e selecione **extrair tudo** para descompactar o arquivo. No Visual Studio, abra o *MvcMobile.sln* arquivo.

Pressione CTRL + F5 para executar o aplicativo, que será exibido no navegador da área de trabalho. Iniciar o emulador do navegador móvel, copie a URL para o aplicativo de conferência do emulador e, em seguida, clique no **procurar por marca** link. Se você estiver usando o emulador do Windows Phone, clique na barra de URL e pressione a tecla Pause para obter acesso do teclado. A imagem abaixo mostra os *AllTags* modo de exibição (de escolher **procurar por marca**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

A exibição é muito legível em um dispositivo móvel. Escolha o link do ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

O modo de exibição de marca do ASP.NET é muito desorganizado. Por exemplo, o **data** coluna é muito difícil de ler. Posteriormente no tutorial, você criará uma versão dos *AllTags* modo de exibição que é especificamente para navegadores móveis e isso fará com que a exibição legível.

Observação: Atualmente, existe um bug no mecanismo de cache móvel. Para aplicativos de produção, você deve instalar o [DisplayModes fixa](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacote NuGet. Ver [ASP.NET MVC 4 Mobile cache Bug fixa](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção.

## <a name="css-media-queries"></a>Consultas de mídia do CSS

[Consultas de mídia do CSS](http://www.w3.org/TR/css3-mediaqueries/) são uma extensão para o CSS para tipos de mídia. Eles permitem que você crie regras que substituem as regras CSS padrão para navegadores específicos (agentes de usuário). Uma regra comum para CSS que se destina a navegadores para dispositivos móveis é definir o tamanho máximo da tela. O *Content\Site.css* arquivo que é criado quando você cria um novo projeto ASP.NET MVC 4 Internet contém a seguinte consulta de mídia:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Se a janela do navegador for 850 pixels ou menos, ele usará as regras de CSS dentro desse bloco de mídia. Você pode usar consultas de mídia do CSS como este para fornecer uma melhor exibição do conteúdo HTML em navegadores pequeno (como navegadores para dispositivos móveis) que as regras CSS padrão que são projetadas para as exibições mais amplas de navegadores de desktop.

## <a name="the-viewport-meta-tag"></a>A marca Viewport Meta

Navegadores de dispositivos móveis mais definem uma largura de janela do navegador virtual (o *visor*) que é muito maior do que a largura real do dispositivo móvel. Isso permite que navegadores de dispositivos móveis para se ajustar toda a página da web dentro de exibição virtual. Os usuários podem, em seguida, ampliar conteúdo interessante. No entanto, se você definir a largura do visor para a largura do dispositivo real, sem aumentar o zoom é necessário, porque o conteúdo se ajusta no navegador móvel.

O visor `<meta>` marca no arquivo de layout ASP.NET MVC 4 define o visor para a largura do dispositivo. A linha a seguir mostra o visor `<meta>` marca no arquivo de layout ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Examinando o efeito de consultas de mídia do CSS e a marca Viewport Meta

Abra o *Views\Shared\\layout. cshtml* arquivo no editor e comente o visor `<meta>` marca. A marcação a seguir mostra a linha comentada.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Abra o *MvcMobile\Content\Site.css* no editor de arquivo e altere a largura máxima da consulta de mídia para zero pixels. Isso impedirá que as regras de CSS que está sendo usado em navegadores de dispositivos móveis. A linha a seguir mostra a consulta de mídia modificado:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Salve suas alterações e navegue até o aplicativo de conferência em um emulador de navegador móvel. O texto pequeno na imagem a seguir é o resultado da remoção do visor `<meta>` marca. Com nenhuma visor `<meta>` marca, o navegador é diminuir o zoom para a largura do visor padrão (850 pixels ou maior para a maioria dos navegadores.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Desfaça as alterações — descomente o visor `<meta>` marca no arquivo de layout e restaurar a consulta de mídia a 850 pixels a *CSS* arquivo. Salve suas alterações e atualize o navegador móvel para verificar se a exibição de dispositivos móveis foi restaurada.

O visor `<meta>` marca e a consulta de mídia do CSS não são específicas para o ASP.NET MVC 4, e você pode tirar proveito desses recursos em qualquer aplicativo web. Mas agora, eles são criados para os arquivos que são gerados quando você cria um novo projeto ASP.NET MVC 4.

Para obter mais informações sobre o visor `<meta>` marcação, consulte [uma história de duas visores — parte dois](http://www.quirksmode.org/mobile/viewports2.html).

Na próxima seção, você verá como fornecer modos de exibição específicos para navegadores móveis.

## <a name="overriding-views-layouts-and-partial-views"></a>Substituir exibições, Layouts e exibições parciais

Um novo recurso significativo no ASP.NET MVC 4 é um mecanismo simples que permite substituir qualquer modo de exibição (inclusive layouts e exibições parciais) para navegadores de dispositivos móveis em geral, para um navegador móvel individual ou para qualquer navegador específico. Para fornecer um modo de exibição específicas para dispositivos móveis, você pode copiar um arquivo de exibição e adicionar *. Mobile* ao nome do arquivo. Por exemplo, para criar o dispositivo móvel *índice* exibir, copiar *Views\Home\Index.cshtml* para *Views\Home\Index.Mobile.cshtml*.

Nesta seção, você criará um arquivo de layout específicas para dispositivos móveis.

Para começar, copie *Views\Shared\\layout. cshtml* à *Views\Shared\\cshtml*. Abra  *\_cshtml* e altere o título de **MVC4 conferência** para **Conference (móvel)**.

Em cada `Html.ActionLink` chamar, remova "Procurar por" em cada link *ActionLink*. O código a seguir mostra a seção de corpo completo do arquivo de layout para dispositivos móveis.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Cópia de *Views\Home\AllTags.cshtml* arquivo *Views\Home\AllTags.Mobile.cshtml*. Abra o novo arquivo e altere o `<h2>` elemento das "Marcas" para "marcas (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Navegue até a página de marcas usando um navegador da área de trabalho e usando o emulador do navegador móvel. O emulador do navegador móvel mostra as duas alterações feitas por você.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Por outro lado, a exibição da área de trabalho não foi alterado.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Modos de exibição de navegador específico

Além das exibições específicas para dispositivos móveis e área de trabalho específica, você pode criar modos de exibição para um único navegador. Por exemplo, você pode criar modos de exibição que são especificamente para o navegador do iPhone. Nesta seção, você criará um layout para o navegador do iPhone e uma versão do iPhone a *AllTags* exibição.

Abra o *global. asax* arquivo e adicione o seguinte código para o `Application_Start` método.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Esse código define um novo modo de exibição denominado "iPhone" que será comparado com cada solicitação de entrada. Se a solicitação de entrada corresponder à condição que você definiu (isto é, se o agente do usuário contém a cadeia de caracteres "iPhone"), o ASP.NET MVC procurará exibições cujo nome contém o sufixo "iPhone".

No código, clique com botão direito `DefaultDisplayMode`, escolha **resolver**e, em seguida, escolha `using System.Web.WebPages;`. Isso adiciona uma referência para o `System.Web.WebPages` namespace, que é onde o `DisplayModes` e `DefaultDisplayMode` tipos são definidos.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Como alternativa, você pode adicionar a seguinte linha à apenas manualmente o `using` seção do arquivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Todo o conteúdo da *global. asax* arquivo é mostrado abaixo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Salve as alterações. Cópia de *MvcMobile\Views\Shared\\cshtml* arquivo *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Abra o novo arquivo e, em seguida, altere a `h1` título do `Conference (Mobile)` para `Conference (iPhone)`.

Cópia de *MvcMobile\Views\Home\AllTags.Mobile.cshtml* arquivo *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. No novo arquivo, altere o `<h2>` elemento de "Tags (M)" para "Tags (iPhone)".

Execute o aplicativo. Executar um emulador de navegador móvel, verifique se o agente do usuário é definido como "iPhone" e navegue até a *AllTags* exibição. A captura de tela a seguir mostra a *AllTags* exibição renderizada na [Safari](http://www.apple.com/safari/download/) navegador. Você pode baixar o Safari para Windows [aqui](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

Nesta seção, vimos como criar layouts e exibições móveis e como criar layouts e exibições para dispositivos específicos, como o iPhone. Na próxima seção, você verá como aproveitar o jQuery Mobile para mais interessantes modos de exibição móveis.

## <a name="using-jquery-mobile"></a>Usando o jQuery Mobile

O [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) biblioteca fornece uma estrutura de interface do usuário que funciona em todos os principais navegadores para dispositivos móveis. aplica-se do jQuery Mobile *aprimoramento progressivo* para navegadores de dispositivos móveis que dão suporte a CSS e JavaScript. Aprimoramento progressivo permite que todos os navegadores exibir o conteúdo básico de uma página da web, permitindo que mais potentes navegadores e dispositivos para ter uma exibição mais avançada. Os arquivos JavaScript e CSS que são incluídos com o jQuery Mobile muitos elementos de acordo com os navegadores móveis sem fazer nenhuma alteração de marcação de estilo.

Nesta seção, você instalará o *jQuery.Mobile.MVC* pacote NuGet, que instala um widget do alternador de exibição e o jQuery Mobile.

Para iniciar, excluir o *Shared\\cshtml* e *compartilhado\\_Layout.iPhone.cshtml* arquivos que você criou anteriormente.

Renomeie *Views\Home\AllTags.Mobile.cshtml* e *Views\Home\AllTags.iPhone.cshtml* arquivos *Views\Home\AllTags.iPhone.cshtml.hide* e  *Views\Home\AllTags.Mobile.cshtml.Hide*. Porque os arquivos não têm uma *. cshtml* extensão, não serão usados pelo tempo de execução do ASP.NET MVC para renderizar o *AllTags* exibição.

Instalar o *jQuery.Mobile.MVC* pacote NuGet ao fazer isso:

1. Dos **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca**e, em seguida, selecione **Package Manager Console**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. No **Package Manager Console**, insira `Install-Package jQuery.Mobile.MVC -version 1.0.0`

A imagem a seguir mostra os arquivos adicionados e alterados para o projeto MvcMobile pelo pacote NuGet jQuery.Mobile.MVC. Arquivos que são adicionados a ter [Adicionar] acrescentado após o nome do arquivo. A imagem não mostra o GIF e PNG arquivos adicionados à *Content\images* pasta.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

O pacote jQuery.Mobile.MVC NuGet instala o seguinte:

- O *App\_Start\BundleMobileConfig.cs* arquivo, que é necessário para fazer referência aos arquivos de JavaScript e CSS do jQuery adicionados. Você deve seguir as instruções abaixo e referenciar o pacote móvel definido neste arquivo.
- arquivos do jQuery Mobile CSS.
- Um `ViewSwitcher` widget de controlador (*Controllers\ViewSwitcherController.cs*).
- arquivos do jQuery Mobile JavaScript.
- Um arquivo de layout com estilo Mobile jQuery (*Views\Shared\\cshtml*).
- Um modo de exibição do alternador de exibição parcial *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) que fornece um link na parte superior de cada página para alternar da exibição da área de trabalho para o modo de exibição móvel e vice-versa.
- Vários<em>. PNG</em> e <em>. gif</em> arquivo de imagem na <em>Content\images</em> pasta.

Abra o *global. asax* do arquivo e adicione o código a seguir como a última linha do `Application_Start` método.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

O código a seguir mostra todo *global. asax* arquivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Se você estiver usando o Internet Explorer 9 e você não vir as `BundleMobileConfig` linha acima no realce amarelo, clique em de [botão de exibição de compatibilidade](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![imagem do botão de exibição de compatibilidade (desativado)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Imagem do botão de exibição de compatibilidade (desativado)") no IE para alterar o ícone de uma estrutura de tópicos ![imagem do botão de exibição de compatibilidade (desativado)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "imagem do botão de exibição de compatibilidade (desativado) ") para uma cor sólida ![imagem do botão de exibição de compatibilidade (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "imagem do botão de exibição de compatibilidade (on)"). Como alternativa, você pode exibir este tutorial no FireFox ou Chrome.


Abra o *MvcMobile\Views\Shared\\cshtml* arquivo e adicione a marcação a seguir diretamente após o `Html.Partial` chamar:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

A conclusão *MvcMobile\Views\Shared\\cshtml* arquivo é mostrado abaixo:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compilar o aplicativo e no seu emulador de navegador móvel, navegue até a *AllTags* exibição. Você verá o seguinte:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Você pode depurar o código específico do móvel por [definindo a cadeia de caracteres de agente do usuário](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) para o Internet Explorer ou Chrome para iPhone e, em seguida, usando as ferramentas de desenvolvedor F-12. Se o navegador móvel não exibir o **página inicial**, **alto-falante**, **marca**, e **data** links como botões, as referências ao jQuery Mobile scripts e arquivos CSS provavelmente não estão corretos.


Além das alterações de estilo, você verá **exibição do celular** e um link que permite que você alternar do modo de exibição móvel para a exibição da área de trabalho. Escolha o **modo de exibição da área de trabalho** link e o modo de exibição da área de trabalho é exibida.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

O modo de exibição da área de trabalho não fornece uma maneira de navegar diretamente para o modo de exibição móvel. Você corrigirá isso agora. Abra o *Views\Shared\\layout. cshtml* arquivo. Apenas na página de `body` elemento, adicione o código a seguir, que renderiza o widget do alternador de exibição:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Atualizar o *AllTags* exibir no navegador móvel. Agora você pode navegar entre exibições móveis e desktops.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Depurar Observação: você pode adicionar o seguinte código ao final do Views\Shared\\_ViewSwitcher.cshtml para ajudar a depurar modos de exibição quando usando um navegador, a cadeia de caracteres de agente do usuário definido como um dispositivo móvel.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  e adicionar o título para o *Views\Shared\\layout. cshtml* arquivo.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Navegue até a *AllTags* página em um navegador da área de trabalho. O widget do alternador de exibição não é exibido em um navegador da área de trabalho porque ele é adicionado somente para a página de layout para dispositivos móveis. Posteriormente no tutorial, você verá como é possível adicionar o widget do alternador de exibição para a exibição da área de trabalho.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Melhorando a lista de palestrantes

No navegador móvel, selecione a **alto-falantes** link. Porque não há nenhum modo de exibição móvel (*Allspeakers*), exibem os alto-falantes padrão (*allspeakers. cshtml*) é renderizado utilizando o modo de exibição de layout para dispositivos móveis ( *\_ Cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Você pode desativar globalmente a um modo de exibição padrão (não móvel) da renderização dentro de um layout para dispositivos móveis, definindo `RequireConsistentDisplayMode` ao `true` na *exibições\\viewstart* arquivo, como este:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Quando `RequireConsistentDisplayMode` é definido como `true`, o layout para dispositivos móveis (<em>\_cshtml</em>) é usado apenas para modos de exibição móveis. (Ou seja, o arquivo de exibição tem o formato <em>* * ViewName</em><em>. Cshtml</em>.) Você talvez queira definir `RequireConsistentDisplayMode` para `true` se seu layout para dispositivos móveis não funcionar bem com seus modos de exibição não móveis. A captura de tela abaixo mostra como o <em>alto-falantes</em> página é renderizada quando `RequireConsistentDisplayMode` é definido como `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Você pode desativar o modo de exibição consistente em uma exibição, definindo `RequireConsistentDisplayMode` para `false` no arquivo de exibição. A marcação a seguir na *Views\Home\AllSpeakers.cshtml* arquivo define `RequireConsistentDisplayMode` para `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Criando uma exibição de alto-falantes móvel

Como você acabou de ver, o *alto-falantes* exibição é legível, mas os links são pequenos e difíceis de tocar em um dispositivo móvel. Nesta seção, você criará uma específicas para dispositivos móveis *alto-falantes* exibição que se parece com um aplicativo móvel moderno — ela exibe grandes, fáceis de tocar links e contém uma caixa de pesquisa para localizar rapidamente os alto-falantes.

Cópia *Allspeakers* à *Allspeakers*. Abra o *Allspeakers* do arquivo e remover o `<h2>` elemento de título.

No `<ul>` marca, adicione o `data-role` do atributo e definir seu valor como `listview`. Como qualquer outro [ `data-*` atributos](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` faz os itens da lista grande mais fácil ao toque. Este é o que a marcação concluída se parece com:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Atualize o navegador móvel. O modo de exibição atualizado tem esta aparência:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Embora o modo de exibição móvel melhorou, é difícil navegar pela longa lista de alto-falantes. Para corrigir isso, no `<ul>` marca, adicione o `data-filter` do atributo e defini-lo como `true`. O código a seguir mostra o `ul` marcação.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

A imagem a seguir mostra a caixa de filtro de pesquisa na parte superior da página que resulta do `data-filter` atributo.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Conforme você digita cada letra na caixa de pesquisa, o jQuery Mobile filtra a lista exibida, conforme mostrado na imagem abaixo.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Melhorando a lista de marcas

Como o padrão *alto-falantes* modo de exibição, o *marcas* exibição é legível, mas os links são pequenos e difíceis de tocar em um dispositivo móvel. Nesta seção, você poderá corrigir os *marcas* exibir da mesma maneira que você corrigir a *alto-falantes* modo de exibição.

Remover o &quot;ocultar&quot; sufixo para o *Views\Home\AllTags.Mobile.cshtml.hide* portanto, é o nome do arquivo *Views\Home\AllTags.Mobile.cshtml*. Abra o arquivo renomeado e remova o `<h2>` elemento.

Adicione a `data-role` e `data-filter` atributos para o `<ul>` de marca, conforme mostrado aqui:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

A imagem abaixo mostra a página de marcas de filtragem na letra `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Melhorando a lista de datas

Você pode melhorar o *datas* exibir como você melhorou a *alto-falantes* e *marcas* modos de exibição, para que ele seja mais fácil de usar em um dispositivo móvel.

Cópia de *Views\Home\AllDates.cshtml* arquivo *Views\Home\AllDates.Mobile.cshtml*. Abra o novo arquivo e remova o `<h2>` elemento.

Adicione `data-role="listview"` para o `<ul>` marca, como este:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

A imagem abaixo mostra o que o **data** página parece com o `data-role` atributo em vigor.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) substitua o conteúdo do *Views\Home\AllDates.Mobile.cshtml* arquivo pelo código a seguir:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Esse código agrupa todas as sessões por dias. Ele cria um divisor de lista para cada novo dia, e ele lista todas as sessões para cada dia em um divisor. Aqui está o que se parece quando esse código é executado:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Melhorando o modo de exibição

Nesta seção, você criará um modo de exibição de sessões específicas para dispositivos móveis. As alterações que fazemos serão mais abrangentes que outros modos de exibição que criamos.

No navegador móvel, toque o **alto-falante** botão e, em seguida, insira `Sc` na caixa de pesquisa.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Toque o **Scott Hanselman** link.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Como você pode ver, a exibição é difícil de ler em um navegador móvel. A coluna de data é difícil de ler e a coluna de marcas está fora do modo de exibição. Para corrigir esse problema, copie *Views\Home\SessionsTable.cshtml* à *Views\Home\SessionsTable.Mobile.cshtml*e, em seguida, substitua o conteúdo do arquivo pelo código a seguir:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

O código remove a sala e marcas de colunas e formata o título, palestrante e data verticalmente, de modo que todas essas informações sejam legíveis em um navegador móvel. A imagem abaixo reflete as alterações de código.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Melhorando o modo de exibição SessionByCode

Por fim, você criará uma exibição do mobile específica a *SessionByCode* exibição. No navegador móvel, toque o **alto-falante** botão e, em seguida, insira `Sc` na caixa de pesquisa.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Toque o **Scott Hanselman** link. Sessões de Hanselman são exibidas.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Escolha o **uma visão geral de MS Web pilha de amor** link.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

O modo de exibição padrão da área de trabalho é bom, mas você pode melhorá-lo.

Cópia de *Views\Home\SessionByCode.cshtml* à *Views\Home\SessionByCode.Mobile.cshtml* e substitua o conteúdo do *Views\Home\SessionByCode.Mobile.cshtml*arquivo com a seguinte marcação:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

A nova marcação usa o `data-role` atributo para aprimorar o layout do modo de exibição.

Atualize o navegador móvel. A imagem a seguir reflete as alterações de código que você acabou de:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Revisão e Wrapup

Este tutorial tenha introduzido os novos recursos móveis do ASP.NET MVC 4 Developer Preview. Os recursos móveis incluem:

- A capacidade de substituir o layout, exibições e exibições parciais, globalmente e para obter uma exibição individual.
- Controle sobre o layout e a imposição de substituição parcial usando o `RequireConsistentDisplayMode` propriedade.
- Um widget do alternador de exibição para dispositivos móveis exibições que também podem ser exibidas nos modos de exibição da área de trabalho.
- Suporte para dar suporte a navegadores específicos, como o navegador do iPhone.

## <a name="see-also"></a>Consulte também

- [jQuery Mobile](http://jquerymobile.com) site.
- [jQuery Mobile Overview](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Práticas recomendadas para aplicativos W3C recomendação Web móvel](http://www.w3.org/TR/mwabp/)
- [Recomendação candidata do W3C para consultas de mídia](http://www.w3.org/TR/css3-mediaqueries/)
