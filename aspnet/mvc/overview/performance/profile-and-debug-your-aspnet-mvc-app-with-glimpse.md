---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Analisar e depurar seu aplicativo ASP.NET MVC com Glimpse | Microsoft Docs
author: Rick-Anderson
description: Visão rápida é prosperando e aumentando a família de pacotes do NuGet de software livre que fornece desempenho detalhados, depuração e informações de diagnóstico para o ASP.NET um...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 94a72f22cbcd7fa84528dde502cceaa1e26dcaa1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577282"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Analisar e depurar seu aplicativo ASP.NET MVC com Glimpse
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Visão rápida é prosperando e aumentando a família de pacotes do NuGet de software livre que fornece desempenho detalhados, depuração e informações de diagnóstico para aplicativos ASP.NET. Ele é trivial para instalar, leve, com rapidez e exibe as principais métricas de desempenho na parte inferior de cada página. Ele permite fazer uma busca detalhada em seu aplicativo quando você precisa descobrir o que está acontecendo no servidor. Visão rápida fornece informações muito valiosas que é recomendável que usá-lo em todo seu ciclo de desenvolvimento, incluindo o seu ambiente de teste do Azure. Enquanto [Fiddler](http://www.telerik.com/fiddler) e o [ferramentas de desenvolvimento F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) fornecem um cliente modo de exibição, a visão rápida fornece uma exibição detalhada do servidor. Este tutorial se concentrará em usando o ASP.NET MVC de amostra e pacotes do EF, mas muitos outros pacotes estão disponíveis. Sempre que possível, eu vinculará a apropriado [dê uma olhada docs](http://getglimpse.com/Docs/) que ajudam a manter. Visão rápida é um projeto de código-fonte aberto, você também pode contribuir com o código-fonte e os documentos.


- [Instalando a amostra](#ig)
- [Habilitar Glimpse para localhost](#eg)
- [Na guia da linha do tempo](#Time)
- [Model binding](#mb)
- [Rotas](#route)
- [Usando a amostra no Azure](#da)
- [Recursos adicionais](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalando a amostra

Você pode instalar Glimpse partir do console do Gerenciador de pacotes NuGet ou o **gerenciar pacotes NuGet** console. Para esta demonstração, vou instalar os pacotes Mvc5 e EF6:

![instalar a prévia do NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Pesquise *Glimpse.EF*

![Glimpse.EF do NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Selecionando **pacotes instalados**, você pode ver os módulos dependentes do Glimpse instalados:

![Os pacotes de prévia instalados de DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Os seguintes comandos instalam os módulos de visão rápida MVC5 e o EF6 no console do Gerenciador de pacote:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Habilitar Glimpse para localhost

Navegue até http://localhost:&lt; a porta n º&gt;/glimpse.axd e clique no <strong>ativar Glimpse</strong> botão.

![Página de axd visão rápida](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Se você tiver sua barra de favoritos exibida, você pode arrastar e soltar os botões de amostra e adicioná-los como bookmarklets:

![IE com Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Agora, você pode navegar seu aplicativo e o **cabeças backup exibição** (HUD) é mostrada na parte inferior da página.

![Página do Gerenciador de contatos com HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

O [página de visão rápida HUD](http://getglimpse.com/Docs/Heads-up-Display) fornece detalhes sobre as informações de medição de tempo mostradas acima. As exibições de dados o HUD desempenho discreto podem notificá-lo de um problema imediatamente - antes de chegar ao ciclo de teste. Clicar na &quot;g&quot; no canto inferior direito abre o painel de visão rápida:

![Painel de visão rápida](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Na imagem acima, o [guia de execução](http://getglimpse.com/Docs/Execution-Tab) for selecionado, que mostra detalhes de medição de tempo das ações e os filtros no pipeline. Você pode ver minha [temporizador de filtro de inspeção parar](http://www.nuget.org/packages/StopWatch/) começam em 6 de estágio do pipeline. Embora minha temporizador leve possa fornecer útil perfil/dados de tempo, ele perde todo o tempo gasto na autorização e o modo de exibição de renderização. Você pode ler sobre meu timer em [criar o perfil e a hora de seu aplicativo ASP.NET MVC todo o caminho para o Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). O [guias](http://getglimpse.com/Docs/Tabs) página fornece links para informações detalhadas sobre cada guia.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Na guia da linha do tempo

Eu modifiquei de Tom Dykstra pendentes [tutorial do EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) com o código a seguir, altere para o controlador instrutores:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

O código acima permite que eu passe na cadeia de caracteres de consulta (`eager`) para controlar adiantado ou explícito, carregamento de dados. Na imagem abaixo, o carregamento explícito é usado e a página de medição de tempo mostra cada registro carregado no `Index` método de ação:

![carregamento explícito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

No código a seguir, adiantado é especificado, e cada registro é buscado após o `Index` é chamado de modo de exibição:

![adiantado é especificado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Você pode passar o mouse sobre um segmento de tempo para obter informações detalhadas de tempo:

![Passe o mouse para ver o tempo detalhados](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Model binding

O [guia de associação de modelo](http://getglimpse.com/Docs/Model-Binding-Tab) fornece uma grande quantidade de informações para ajudá-lo a entender como as variáveis de formulário são associadas e por que alguns não são associados conforme o esperado. A imagem abaixo mostra o **?** ícone, você poderá clicar para abrir a página de ajuda de amostra para esse recurso.

![Dê uma olhada a exibição do modelo de associação](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Rotas

 Guia de visão rápida de rotas pode ajudará você depurar e entender o roteamento. Na imagem abaixo, a rota de produto é selecionada (e aparece em verde, uma convenção de visão rápida). ![nome do produto selecionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) tokens de dados, áreas e restrições de rota também são exibidos. Ver [rotas Glimpse](http://getglimpse.com/Docs/Routes-Tab) e [roteamento de atributo no ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obter mais informações. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Usando a amostra no Azure

A política de segurança padrão Glimpse permite apenas dados de amostra a serem exibidos no host local. Você pode alterar essa política de segurança para que você possa exibir esses dados em um servidor remoto (por exemplo, um aplicativo web no Azure). Para ambientes de teste no Azure, adicione a marca realçada até a parte inferior da *web.confg* arquivo para habilitar a visão rápida:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Com essa alteração sozinha, qualquer usuário pode ver os dados de amostra em um site remoto. Considere adicionar a marcação acima a um perfil de publicação para que ele tem apenas implantado um aplicada quando você usa esse perfil de publicação (por exemplo, seu proifle de teste do Azure.) Para restringir os dados de amostra, adicionaremos o `canViewGlimpseData` função e só permitir que os usuários nessa função para exibir dados de amostra.

Remova os comentários do *GlimpseSecurityPolicy.cs* do arquivo e altere o [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) chamar de `Administrator` para o `canViewGlimpseData` função:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Security - dados avançados fornecidos pelo ideia poderia expor a segurança do seu aplicativo. Microsoft não realizou uma auditoria de segurança do Glimpse para uso em aplicativos de produções.


Para obter informações sobre como adicionar funções, consulte minha [implantar um aplicativo da web ASP.NET MVC 5 seguro com associação, OAuth e banco de dados SQL no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Implantar um aplicativo ASP.NET MVC 5 seguro com associação, OAuth e banco de dados SQL do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Dê uma olhada configuração](http://getglimpse.com/Docs/Configuration) -página de documentação sobre como configurar as guias, política de tempo de execução, registro em log e muito mais.
