---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Recursos de celular do ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Agora há uma versão do MVC 5 deste tutorial com exemplos de código em implantar um aplicativo da Web do ASP.NET MVC 5 Mobile nos Sites do Azure.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 5f38fcdd8e71ce12f7899214b6b2133e21f9910c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>Recursos móveis do ASP.NET MVC 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Agora há uma versão do MVC 5 deste tutorial com exemplos de código em [implantar um aplicativo da Web do ASP.NET MVC 5 Mobile nos Sites do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Este tutorial ensina as Noções básicas de como trabalhar com recursos de celular em um aplicativo Web do ASP.NET MVC 4. Para este tutorial, você pode usar [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) ou Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer ou VWD&quot;). Você pode usar a versão professional do Visual Studio se ele já está instalado.

Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.

- [O Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recomendado) ou o Visual Studio Web Developer Express SP1. O Visual Studio 2012 contém o ASP.NET MVC 4. Se você estiver usando o Visual Web Developer 2010, você deve instalar [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Você também precisará emulador um navegador móvel. Qualquer um dos seguintes funcionará:

- [Emulador do Windows Phone 7 do Windows](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Esse é o emulador que é usado na maioria das capturas de tela neste tutorial.)
- Altere a cadeia de caracteres de agente de usuário para emular um iPhone. Consulte [isso](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) entrada de blog.
- [Opera Mobile Emulator](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) com o agente do usuário definido para iPhone. Para obter instruções sobre como configurar o agente do usuário no Safari para "iPhone", consulte [como permitir que o Safari fingem é IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) no blog de David Alison.

Projetos do Visual Studio com o código-fonte c# estão disponíveis para acompanhar este tópico:

- [Download do projeto de Starter](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Concluído o download do projeto](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>O que você vai criar

Para este tutorial, você adicionará recursos móveis para o aplicativo de listagem de conferência simple que é fornecido no [projeto starter](https://go.microsoft.com/fwlink/?LinkId=228307). Captura de tela a seguir mostra a página de marcas do aplicativo concluído como visto no [emulador do Windows Phone 7 Windows](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Consulte [mapeamento do teclado para o Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) para simplificar a entrada do teclado.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Você pode usar o Internet Explorer 9 ou 10, FireFox ou Chrome para desenvolver seu aplicativo móvel, definindo o [cadeia de caracteres de agente de usuário](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). A imagem a seguir mostra o tutorial concluído usando o Internet Explorer que emula um iPhone. Você pode usar as ferramentas de desenvolvedor do Internet Explorer F-12 e [ferramenta Fiddler](http://www.fiddler2.com/fiddler2/) para ajudar a depurar seu aplicativo.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Você vai aprender as habilidades

Aqui está o que você aprenderá:

- Como os modelos do ASP.NET MVC 4 usam o HTML5 `viewport` atributo e renderização adaptável para melhorar a exibição em dispositivos móveis.
- Como criar exibições específicas para dispositivos móveis.
- Como criar uma alternância de exibição alternância de usuários que permite entre uma exibição móvel e uma área de trabalho do aplicativo.

### <a name="getting-started"></a>Guia de Introdução

Baixe o aplicativo de conferência listagem para o projeto inicial usando o seguinte link: [baixar](https://go.microsoft.com/fwlink/?LinkId=228307). No Windows Explorer, clique com o *MvcMobile.zip* de arquivo e escolha **propriedades**. No **MvcMobile.zip propriedades** caixa de diálogo caixa, escolha o **desbloquear** botão. (Desbloqueio impede que um aviso de segurança que ocorre quando você tentar usar um *. zip* arquivo que você baixou da web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Com o botão direito do *MvcMobile.zip* de arquivo e selecione **extrair tudo** para descompactar o arquivo. No Visual Studio, abra o *MvcMobile.sln* arquivo.

Pressione CTRL + F5 para executar o aplicativo, que será exibido no navegador da área de trabalho. Iniciar o emulador do navegador de dispositivo móvel, copie a URL para o aplicativo de conferência para o emulador e, em seguida, clique no **procurar por marca** link. Se você estiver usando o emulador do Windows Phone, clique na barra de URL e pressione a tecla Pause para obter acesso de teclado. A imagem abaixo mostra o *AllTags* exibição (escolha **procurar por marca**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

A exibição é muito legível em um dispositivo móvel. Escolha o link do ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

O modo de exibição do ASP.NET marca é muito confuso. Por exemplo, o **data** coluna é muito difícil de ler. Posteriormente no tutorial, você criará uma versão do *AllTags* exibição especificamente para navegadores de dispositivos móveis e para tornar a exibição legível.

Observação: No momento existe um bug no mecanismo de cache móvel. Para aplicativos de produção, você deve instalar o [DisplayModes fixa](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacote preciosidade. Consulte [ASP.NET MVC 4 Mobile cache Bug fixa](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção.

## <a name="css-media-queries"></a>Consultas de mídia CSS

[Consultas de mídia CSS](http://www.w3.org/TR/css3-mediaqueries/) são uma extensão de CSS para tipos de mídia. Eles permitem que você crie regras que substituem as regras CSS padrão para navegadores específicos (agentes de usuário). Uma regra comum de CSS que tem como alvo os navegadores móveis é definir o tamanho máximo da tela. O *Content\Site.css* arquivo criado quando você cria um novo projeto ASP.NET MVC 4 Internet contém a seguinte consulta de mídia:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Se a janela do navegador é 850 pixels ou menos, ele usará as regras de CSS dentro deste bloco de mídia. Você pode usar consultas de mídia CSS como este para fornecer uma melhor exibição de conteúdo HTML em navegadores pequenos (como navegadores móveis) que as regras CSS padrão que são projetadas para o exibe mais amplo de navegadores de desktop.

## <a name="the-viewport-meta-tag"></a>A marca Meta do visor

A maioria dos navegadores definem uma largura da janela de navegador virtual (o *visor*) que é muito maior do que a largura real do dispositivo móvel. Isso permite que navegadores móveis ajustar toda a página da web dentro de exibição virtual. Os usuários podem, em seguida, aplicar zoom na conteúdo interessante. No entanto, se você definir a largura do visor para a largura do dispositivo real, sem zoom é necessário, porque o conteúdo se encaixar no navegador móvel.

O visor `<meta>` marca no arquivo de layout do ASP.NET MVC 4 define o visor para a largura do dispositivo. A linha a seguir mostra o visor `<meta>` marca no arquivo de layout do ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Examinar o efeito de consultas de mídia CSS e a marca Meta do visor

Abra o *exibições \ compartilhadas\\cshtml* arquivo no editor e comente o visor `<meta>` marca. A marcação a seguir mostra a linha de comentado.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Abra o *MvcMobile\Content\Site.css* no editor de arquivo e altere a largura máxima da consulta de mídia para zero pixels. Isso impedirá que as regras de CSS que está sendo usado em navegadores móveis. A linha a seguir mostra a consulta de mídia modificado:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Salve suas alterações e navegue até o aplicativo de conferência em um emulador de navegador móvel. O texto pequeno na imagem a seguir é o resultado da remoção do visor `<meta>` marca. Com nenhuma visor `<meta>` marca, o navegador é diminuir o zoom para a largura do visor padrão (850 pixels ou maior para a maioria dos navegadores.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Desfaça as alterações, descomente o visor `<meta>` marca no arquivo de layout e a consulta de mídia de restauração para 850 pixels do *Site.css* arquivo. Salve suas alterações e atualizar o navegador móvel para verificar se a exibição de dispositivos móveis foi restaurada.

O visor `<meta>` marca e a consulta de mídia do CSS não são específicas para ASP.NET MVC 4, e você pode tirar proveito desses recursos em um aplicativo web. Mas, agora, eles são criados para os arquivos que são gerados quando você cria um novo projeto ASP.NET MVC 4.

Para obter mais informações sobre o visor `<meta>` marca, consulte [uma história de dois visores — parte dois](http://www.quirksmode.org/mobile/viewports2.html).

Na próxima seção, você verá como fornecer exibições específicas do navegador móvel.

## <a name="overriding-views-layouts-and-partial-views"></a>Substituindo exibições parciais, Layouts e modos de exibição

Um novo recurso significativo no ASP.NET MVC 4 é um mecanismo que permite que você substitua qualquer modo de exibição (inclusive layouts e exibições parciais) para navegadores de dispositivos móveis em geral, para um navegador móvel individual ou para qualquer navegador específico. Para fornecer uma exibição específicas para dispositivos móveis, você pode copiar um arquivo de exibição e adicionar *. Mobile* ao nome do arquivo. Por exemplo, para criar um celular *índice* exibir, copiar *Views\Home\Index.cshtml* para *Views\Home\Index.Mobile.cshtml*.

Nesta seção, você criará um arquivo de layout específicas para dispositivos móveis.

Para começar, copie *exibições \ compartilhadas\\cshtml* para *exibições \ compartilhadas\\cshtml*. Abra  *\_Layout.Mobile.cshtml* e altere o título do **MVC4 conferência** para **conferência (móvel)**.

Em cada `Html.ActionLink` chamar, remova "Procurar por" em cada link *ActionLink*. O código a seguir mostra a seção de corpo concluído do arquivo de layout para dispositivos móveis.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copie o *Views\Home\AllTags.cshtml* o arquivo para *Views\Home\AllTags.Mobile.cshtml*. Abra o novo arquivo e altere o `<h2>` elemento de "Tags" para "marcas (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Navegue até a página de marcas usando um navegador da área de trabalho e usar o emulador do navegador móvel. O emulador do navegador móvel mostra as duas alterações feitas por você.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Por outro lado, a exibição da área de trabalho não foi alterado.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Modos de exibição de navegador específico

Além das exibições específicas para dispositivos móveis e área de trabalho específica, você pode criar modos de exibição para um navegador individual. Por exemplo, você pode criar exibições que são específicas para o navegador do iPhone. Nesta seção, você criará um layout para o navegador do iPhone e uma versão iPhone o *AllTags* exibição.

Abra o *global. asax* de arquivo e adicione o seguinte código para o `Application_Start` método.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Esse código define um novo modo de exibição denominado "iPhone" que será comparado com cada solicitação de entrada. Se a solicitação de entrada corresponder a condição definida (ou seja, se o agente do usuário contém a cadeia de caracteres "iPhone"), o ASP.NET MVC procurará exibições cujo nome contém o sufixo "iPhone".

No código, clique com botão direito `DefaultDisplayMode`, escolha **resolver**e, em seguida, escolha `using System.Web.WebPages;`. Isso adiciona uma referência para o `System.Web.WebPages` namespace, que é o local onde o `DisplayModes` e `DefaultDisplayMode` tipos são definidos.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Como alternativa, você pode adicionar a seguinte linha à apenas manualmente o `using` seção do arquivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Todo o conteúdo do *global. asax* arquivo é mostrado abaixo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Salve as alterações. Copie o *MvcMobile\Views\Shared\\cshtml* o arquivo para *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Abra o novo arquivo e, em seguida, alterar o `h1` título do `Conference (Mobile)` para `Conference (iPhone)`.

Copie o *MvcMobile\Views\Home\AllTags.Mobile.cshtml* o arquivo para *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. No novo arquivo, altere o `<h2>` elemento de "marcas (M)" para "Tags (iPhone)".

Execute o aplicativo. Executar um emulador de navegador de dispositivo móvel, verifique se seu agente de usuário é definido como "iPhone" e navegue até o *AllTags* exibição. A captura de tela a seguir mostra o *AllTags* exibição renderizada no [Safari](http://www.apple.com/safari/download/) navegador. Você pode baixar o Safari para Windows [aqui](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

Esta seção vimos como criar exibições e layouts de móveis e como criar layouts e modos de exibição para dispositivos específicos, como o iPhone. Na próxima seção, você verá como aproveitar o jQuery Mobile para mais atraentes modos de exibição móveis.

## <a name="using-jquery-mobile"></a>Usando o jQuery Mobile

O [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) biblioteca fornece uma estrutura de interface de usuário que funciona em todos os principais navegadores móveis. aplica-se do jQuery Mobile *aprimoramento progressivo* para navegadores de dispositivos móveis que oferecem suporte a CSS e JavaScript. Aprimoramento progressivo permite que todos os navegadores exibir o conteúdo básico de uma página da web, permitindo ainda mais potentes navegadores e dispositivos com uma exibição mais rica. Os arquivos JavaScript e CSS que estão incluídos com o jQuery Mobile muitos elementos de navegadores móveis sem fazer qualquer alteração de marcação de estilo.

Nesta seção, você instalará o *jQuery.Mobile.MVC* pacote NuGet, que instala o jQuery Mobile e um widget de alternância de exibição.

Para iniciar, excluir o *compartilhado\\cshtml* e *compartilhado\\_Layout.iPhone.cshtml* arquivos que você criou anteriormente.

Renomear *Views\Home\AllTags.Mobile.cshtml* e *Views\Home\AllTags.iPhone.cshtml* arquivos *Views\Home\AllTags.iPhone.cshtml.hide* e  *Views\Home\AllTags.Mobile.cshtml.Hide*. Porque os arquivos não têm um *. cshtml* extensão, não ser usadas pelo tempo de execução do ASP.NET MVC para renderizar o *AllTags* exibição.

Instalar o *jQuery.Mobile.MVC* pacote NuGet ao fazer isso:

1. Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**e, em seguida, selecione **Package Manager Console**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. No **Package Manager Console**, digite `Install-Package jQuery.Mobile.MVC -version 1.0.0`

A imagem a seguir mostra os arquivos adicionados e alterados para o projeto MvcMobile pelo pacote jQuery.Mobile.MVC NuGet. Arquivos que são adicionados a ter [Adicionar] acrescentado após o nome do arquivo. A imagem não mostra o GIF e PNG arquivos adicionados ao *Content\images* pasta.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

O pacote jQuery.Mobile.MVC NuGet instala o seguinte:

- O *aplicativo\_Start\BundleMobileConfig.cs* arquivo, que é necessário para fazer referência a jQuery JavaScript e CSS arquivos adicionados. Siga as instruções abaixo e deve referenciar o pacote móvel definido nesse arquivo.
- arquivos do jQuery Mobile CSS.
- Um `ViewSwitcher` widget controlador (*Controllers\ViewSwitcherController.cs*).
- arquivos do jQuery Mobile JavaScript.
- Um arquivo de layout com estilo Mobile jQuery (*exibições \ compartilhadas\\cshtml*).
- Uma exibição parcial de alternância de exibição *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) que fornece um link na parte superior de cada página para alternar do modo de exibição da área de trabalho para o modo de exibição móvel e vice-versa.
- Vários<em>. PNG</em> e <em>. gif</em> arquivos de imagem de <em>Content\images</em> pasta.

Abra o *global. asax* de arquivo e adicione o código a seguir como a última linha do `Application_Start` método.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

O código a seguir mostra o completo *global. asax* arquivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Se você estiver usando o Internet Explorer 9 e você não vir o `BundleMobileConfig` linha acima no realce amarelo, clique o [botão modo de exibição de compatibilidade](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![imagem do botão de exibição de compatibilidade (desativado)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Imagem do botão de exibição de compatibilidade (desativado)") no IE para alterar o ícone de uma estrutura de tópicos ![imagem do botão de exibição de compatibilidade (desativado)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "imagem do botão de exibição de compatibilidade (desativado) ") para uma cor sólida ![imagem do botão modo de exibição de compatibilidade (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "imagem do botão modo de exibição de compatibilidade (on)"). Como alternativa, você pode exibir este tutorial no FireFox ou Chrome.


Abra o *MvcMobile\Views\Shared\\cshtml* de arquivo e adicione a seguinte marcação diretamente após o `Html.Partial` chamar:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Completo *MvcMobile\Views\Shared\\cshtml* arquivo é mostrado abaixo:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compilar o aplicativo e no emulador seu navegador de dispositivo móvel, navegue até o *AllTags* exibição. Você verá o seguinte:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Você pode depurar o código móvel específico por [definindo a cadeia de caracteres de agente do usuário](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) para o Internet Explorer ou Chrome para iPhone e, em seguida, usando as ferramentas de desenvolvedor F-12. Se o navegador móvel não exibir o **início**, **locutor**, **marca**, e **data** links como botões, as referências para jQuery Mobile scripts e arquivos CSS provavelmente não estão corretos.


Além das alterações de estilo, você deve ver **exibindo exibição móvel** e um link que permite que você alternar do modo de exibição móvel para o modo de área de trabalho. Escolha o **exibição da área de trabalho** link e o modo de exibição da área de trabalho é exibida.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

O modo de exibição da área de trabalho não fornece uma maneira de navegar diretamente para o modo de exibição móvel. Isso será corrigido agora. Abra o *exibições \ compartilhadas\\cshtml* arquivo. Apenas em página `body` elemento, adicione o seguinte código, que renderiza o widget de alternância de exibição:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Atualizar o *AllTags* exibição no navegador móvel. Agora você pode navegar entre as exibições de desktop e mobile.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Depurar Observação: você pode adicionar o seguinte código ao final da exibições \ compartilhadas\\_ViewSwitcher.cshtml para ajudar a depurar modos de exibição quando usando um navegador, a cadeia de caracteres de agente do usuário definido em um dispositivo móvel.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  e adicionando o seguinte cabeçalho para o *exibições \ compartilhadas\\cshtml* arquivo.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Navegue até o *AllTags* página em um navegador da área de trabalho. O widget de alternância de exibição não é exibido em um navegador da área de trabalho porque ela é adicionada apenas à página de layout para dispositivos móveis. No tutorial posteriormente, você verá como é possível adicionar o widget de alternância de exibição para o modo de exibição da área de trabalho.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Melhorando a lista de alto-falantes

No navegador móvel, selecione o **alto-falantes** link. Porque não há nenhum modo de exibição móvel (*AllSpeakers.Mobile.cshtml*), exibem os alto-falantes padrão (*AllSpeakers.cshtml*) é processado usando o modo de exibição de layout para dispositivos móveis ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Globalmente você pode desabilitar o modo de exibição padrão (não móveis) da renderização dentro de um layout para dispositivos móveis, definindo `RequireConsistentDisplayMode` para `true` no *exibições\\viewstart* arquivo, como este:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Quando `RequireConsistentDisplayMode` é definido como `true`, o layout para dispositivos móveis (<em>\_Layout.Mobile.cshtml</em>) é usado apenas para exibições móveis. (Ou seja, o arquivo de exibição tem o formato <em>* * ViewName</em><em>. Cshtml</em>.) Talvez você queira definir `RequireConsistentDisplayMode` para `true` se o layout para dispositivos móveis não funcionar bem com seus modos de exibição não móveis. Captura de tela a seguir mostra como o <em>alto-falantes</em> page renderiza quando `RequireConsistentDisplayMode` é definido como `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Você pode desabilitar o modo de exibição consistente em uma exibição definindo `RequireConsistentDisplayMode` para `false` no arquivo de exibição. A seguinte marcação no *Views\Home\AllSpeakers.cshtml* conjuntos de arquivos `RequireConsistentDisplayMode` para `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Criação de uma exibição de alto-falantes móvel

Como você acabou de ver o *alto-falantes* modo de exibição pode ser lido, mas os links são pequenos e difíceis de toque em um dispositivo móvel. Nesta seção, você criará uma específicas para dispositivos móveis *alto-falantes* exibição parece um aplicativo móvel moderno — exibe grande, fácil de toque links e contém uma caixa de pesquisa para localizar rapidamente os alto-falantes.

Cópia *AllSpeakers.cshtml* para *AllSpeakers.Mobile.cshtml*. Abra o *AllSpeakers.Mobile.cshtml* de arquivo e remover o `<h2>` elemento de cabeçalho.

No `<ul>` marca, adicione o `data-role` de atributos e defina seu valor como `listview`. Assim como outros [ `data-*` atributos](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` faz com que os itens da lista grande mais fácil para toque. Esta é a aparência de marcação concluída:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Atualize o navegador móvel. O modo de exibição atualizado tem esta aparência:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Embora o modo de exibição móvel melhorou, é difícil navegar a longa lista de alto-falantes. Para corrigir isso, no `<ul>` marca, adicione o `data-filter` de atributos e defina-a como `true`. O código a seguir mostra o `ul` marcação.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

A imagem a seguir mostra a caixa de filtro de pesquisa na parte superior da página que resulta do `data-filter` atributo.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Conforme você digita cada letra na caixa de pesquisa, o jQuery Mobile filtra a lista exibida conforme mostrado na imagem abaixo.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Melhorando a lista de marcas

Como o padrão *alto-falantes* exibição, o *marcas* modo de exibição pode ser lido, mas os links são pequenos e difíceis de toque em um dispositivo móvel. Nesta seção, você corrigirá a *marcas* exibir da mesma maneira que você fixa o *alto-falantes* exibição.

Remover o &quot;ocultar&quot; sufixo para o *Views\Home\AllTags.Mobile.cshtml.hide* é o nome de arquivo *Views\Home\AllTags.Mobile.cshtml*. Abra o arquivo renomeado e remova o `<h2>` elemento.

Adicionar o `data-role` e `data-filter` atributos para o `<ul>` marca, conforme mostrado aqui:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

A imagem a seguir mostra a página de marcas de filtragem na letra `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Melhorando a lista de datas

Você pode melhorar o *datas* exibir como você aprimorado o *alto-falantes* e *marcas* exibe, para que seja mais fácil de usar em um dispositivo móvel.

Copie o *Views\Home\AllDates.cshtml* o arquivo para *Views\Home\AllDates.Mobile.cshtml*. Abra o novo arquivo e remover o `<h2>` elemento.

Adicionar `data-role="listview"` para o `<ul>` marca, como este:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

A imagem abaixo mostra o que o **data** página parece com o `data-role` atributo em vigor.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) substituir o conteúdo do *Views\Home\AllDates.Mobile.cshtml* arquivo com o código a seguir:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Esse código agrupa todas as sessões em dias. Ele cria um divisor de lista para cada novo dia e lista todas as sessões para cada dia em uma linha divisória. Aqui está o que se parece quando esse código é executado:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Melhorando a exibição de SessionsTable

Nesta seção, você criará uma exibição específicas para dispositivos móveis de sessões. As alterações que fazemos serão mais abrangentes que outros modos de exibição que você criou.

No navegador móvel, toque o **locutor** botão e, em seguida, digite `Sc` na caixa de pesquisa.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Toque na **Scott Hanselman** link.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Como você pode ver, a exibição é difícil de ler em um navegador móvel. A coluna de data é difícil a leitura e a coluna de marcas está fora do modo de exibição. Para corrigir isso, copie *Views\Home\SessionsTable.cshtml* para *Views\Home\SessionsTable.Mobile.cshtml*e, em seguida, substitua o conteúdo do arquivo com o código a seguir:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

O código remove sala marcas colunas e formata o título, alto-falantes e data verticalmente, de modo que todas essas informações pode ser lidas em um navegador móvel. A imagem a seguir reflete as alterações de código.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Melhorando a exibição de SessionByCode

Por fim, você criará uma exibição específicas para dispositivos móveis do *SessionByCode* exibição. No navegador móvel, toque o **locutor** botão e, em seguida, digite `Sc` na caixa de pesquisa.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Toque na **Scott Hanselman** link. Sessões de Scott Hanselman são exibidas.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Escolha o **visão geral do MS da pilha de Love** link.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

A exibição da área de trabalho padrão é bom, mas você pode melhorá-la.

Copie o *Views\Home\SessionByCode.cshtml* para *Views\Home\SessionByCode.Mobile.cshtml* e substitua o conteúdo do *Views\Home\SessionByCode.Mobile.cshtml*arquivo com a seguinte marcação:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Usa a marcação de novo o `data-role` atributo para aprimorar o layout do modo de exibição.

Atualize o navegador móvel. A imagem a seguir reflete as alterações de código que você acabou de criar:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Revisão e Wrapup

Este tutorial introduziu novos recursos de visualização do desenvolvedor do ASP.NET MVC 4 móveis. Os recursos móveis incluem:

- A habilidade de substituir o layout, exibições e exibições parciais, globalmente e para uma exibição individual.
- Controle sobre o layout e a substituição parcial imposição usando o `RequireConsistentDisplayMode` propriedade.
- Um widget de alternância de exibição para dispositivos móveis exibições que também podem ser exibidos em modos de exibição da área de trabalho.
- Suporte para dar suporte a navegadores específicos, como o navegador do iPhone.

## <a name="see-also"></a>Consulte também

- [o jQuery Mobile](http://jquerymobile.com) site.
- [jQuery Mobile Overview](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Práticas recomendadas de aplicativo do W3C recomendação Web móvel](http://www.w3.org/TR/mwabp/)
- [Recomendação do W3C candidato para consultas de mídia](http://www.w3.org/TR/css3-mediaqueries/)
