---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "Criar o perfil e depurar seu aplicativo ASP.NET MVC com prévia | Microsoft Docs"
author: Rick-Anderson
description: "Visão rápida é um prosperando e aumentando a família de pacotes do NuGet código-fonte aberto que fornece desempenho detalhada, depuração e informações de diagnóstico para o ASP.NET um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 98b21a54ba00a8c82c3be7ba4e39d44041ed42c6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Criar o perfil e depurar seu aplicativo ASP.NET MVC com prévia
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Visão rápida é um prosperando e aumentando a família de pacotes do NuGet código-fonte aberto que fornece desempenho detalhada, depuração e informações de diagnóstico para aplicativos ASP.NET. Ele é simples de instalar, leve, ultrarrápido e exibe as principais métricas de desempenho na parte inferior de cada página. Ele permite que você fazer drill down em seu aplicativo quando você precisa saber o que está acontecendo no servidor. Visão rápida fornece muito valiosas informações, que recomendamos que você pode usá-lo durante o ciclo de desenvolvimento, incluindo o seu ambiente de teste do Azure. Enquanto [Fiddler](http://www.telerik.com/fiddler) e [ferramentas de desenvolvimento F-12](https://msdn.microsoft.com/en-us/library/ie/gg589512(v=vs.85).aspx) fornecem um lado do cliente exibição, a visão rápida fornece uma exibição detalhada do servidor. Este tutorial se concentrará em usando o ASP.NET MVC de amostra e pacotes EF, mas muitos outros pacotes estão disponíveis. Sempre que possível, será vinculado ao apropriado [vislumbrar documentos](http://getglimpse.com/Docs/) que ajudam a manter. Visão rápida é um projeto, você também pode contribuir com o código-fonte e os documentos.


- [Instalação prévia](#ig)
- [Habilitar prévia para localhost](#eg)
- [Na guia Cronograma](#Time)
- [Associação de modelos](#mb)
- [Rotas](#route)
- [Usando a amostra no Azure](#da)
- [Recursos adicionais](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalação prévia

Você pode instalar prévia de console do Gerenciador de pacote do NuGet ou do **gerenciar pacotes NuGet** console. Para esta demonstração, eu instalará os pacotes Mvc5 e EF6:

![instalar prévia do NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Procurar *Glimpse.EF*

![Glimpse.EF do NuGet instalação dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Selecionando **pacotes instalados**, você pode ver os módulos dependentes prévia instalados:

![Instalado pacotes de amostra do DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Os comandos a seguir instalam os módulos MVC5 prévia e EF6 do console do Gerenciador de pacote:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Habilitar prévia para localhost

Navegue até http://localhost:&lt;porta #&gt;/glimpse.axd e clique no **ativar prévia** botão.

![Página de axd prévia](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Se você tiver a barra de favoritos exibida, você pode arrastar e soltar os botões de amostra e adicioná-los como bookmarklets:

![IE com boookmarklets de amostra](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Agora você pode navegar seu aplicativo e o **cabeçotes a exibição** (HUD) é mostrado na parte inferior da página.

![Página do Gerenciador de contato com HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

O [página prévia HUD](http://getglimpse.com/Docs/Heads-up-Display) fornece detalhes sobre as informações de tempo mostradas acima. As exibições de dados a HUD desempenho discreto podem notificá-lo de um problema imediatamente - antes de obter o ciclo de teste. Clicar no &quot;g&quot; no canto inferior direito exibe o painel de amostra:

![Painel de visão rápida](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Na imagem acima, o [guia execução](http://getglimpse.com/Docs/Execution-Tab) for selecionada, que mostra detalhes de tempo das ações e os filtros no pipeline. Você pode ver meu [timer de filtro de inspeção parar](http://www.nuget.org/packages/StopWatch/) Iniciar etapa 6 do pipeline. Enquanto o timer de peso leve pode fornecer útil perfil/dados de tempo, erros todo o tempo gasto na autorização e o modo de exibição de renderização. Você pode ler sobre meu timer em [de perfil e a hora de seu aplicativo ASP.NET MVC para Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). O [guias](http://getglimpse.com/Docs/Tabs) página fornece links para informações detalhadas sobre cada guia.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Na guia Cronograma

Modificar de Tom Dykstra pendentes [tutorial de 6/MVC 5 EF](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) com o código a seguir, altere para o controlador de professores:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

O código acima permite passar na cadeia de caracteres de consulta (`eager`) para controlar eager ou explícita de carregamento de dados. Na imagem abaixo, o carregamento explícito é usado e a página de controle de tempo mostra cada registro carregado no `Index` método de ação:

![carregamento explícito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

No código a seguir, eager for especificado, e cada registro é buscado após o `Index` é chamado de modo de exibição:

![eager for especificado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Você pode focalizar um segmento de tempo para obter informações detalhadas de tempo:

![Focalize tempo detalhados](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Associação de modelo

O [guia modelo de associação](http://getglimpse.com/Docs/Model-Binding-Tab) fornece uma grande quantidade de informações para ajudá-lo a entender como as variáveis de formulário são limitadas e por que alguns não são associados conforme o esperado. A imagem abaixo mostra o **?** ícone, você pode clicar em para abrir a página de ajuda de amostra para esse recurso.

![mostra a exibição do modelo de associação](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Rotas

 Na guia rotas prévia pode ajudará você depurar e entender o roteamento. Na imagem abaixo, a rota de produto é selecionada (e aparece em verde, uma convenção de amostra). ![nome do produto selecionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) tokens de dados, restrições e áreas de rota também são exibidos. Consulte [prévia rotas](http://getglimpse.com/Docs/Routes-Tab) e [roteamento de atributo no ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obter mais informações. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Usando a amostra no Azure

A política de segurança padrão prévia apenas permite que os dados de amostra a ser exibida no host local. Você pode alterar essa política de segurança para que você pode exibir esses dados em um servidor remoto (como um aplicativo web no Azure). Para ambientes de teste no Azure, adicione a marca realçada até a parte inferior da *confg* arquivo para habilitar a amostra:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Com essa alteração sozinha, qualquer usuário pode ver os dados de amostra em um local remoto. Considere adicionar a marcação acima para um perfil de publicação para que ele tem apenas implantado um aplicada quando você usar esse perfil de publicação (por exemplo, proifle o teste do Azure.) Para restringir os dados de amostra, adicionaremos o `canViewGlimpseData` função e só permitir que os usuários nesta função para exibir dados de amostra.

Remova os comentários do *GlimpseSecurityPolicy.cs* de arquivo e altere o [IsInRole](https://msdn.microsoft.com/en-us/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) chamar de `Administrator` para o `canViewGlimpseData` função:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Segurança - os dados ricos fornecidos pelo prévia podem expor a segurança do seu aplicativo. Microsoft não realizou uma auditoria de segurança de amostra para uso em aplicativos de produção.


Para obter informações sobre a adição de funções, consulte Meus [implantar um aplicativo da web seguro ASP.NET MVC 5 com associação, OAuth e o banco de dados do SQL Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Implantar um aplicativo de seguro ASP.NET MVC 5 com associação, OAuth e o banco de dados SQL do Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Mostra a configuração](http://getglimpse.com/Docs/Configuration) -página de documento sobre como configurar guias, diretiva de tempo de execução, registro em log e muito mais.
