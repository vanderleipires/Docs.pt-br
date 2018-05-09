---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: O que há de novo no ASP.NET MVC 5.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a>O que há de novo no ASP.NET MVC 5.2
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) e [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [Requisitos de software](#softRequire)
- [Baixar](#download)
- [Documentação](#documentation)
- [Novos recursos no ASP.NET MVC 5.2](#new-features)

    - [Aprimoramentos de roteamentos de atributo](#attributerouting)
- [Problemas conhecidos e as alterações recentes](#knownbreakingchanges)
- [Correções de bugs](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Requisitos de software

- O Visual Studio 2012: Baixe [2013.1 para Visual Studio 2012 de ferramentas do ASP.NET e Web](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Baixar o [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) ou superior. Essa atualização é necessária para editar modos de exibição da Razor 5.2 ASP.NET MVC.

<a id="download"></a>
## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes do NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote mais recente do ASP.NET MVC 5.2 tem a seguinte versão: "5.2.0". Você pode instalar ou atualizar esses pacotes por meio de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados usando o NuGet Package Manager Console:

Pacote de instalação Microsoft.AspNet.Mvc-versão 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o ASP.NET MVC 5.2 estão disponíveis no site da web ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Novos recursos no ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Aprimoramentos de roteamentos de atributo

Roteamento de atributo agora fornece um ponto de extensibilidade chamado IDirectRouteProvider, que permite o controle completo sobre como as rotas de atributo são descobertas e configuradas. Um IDirectRouteProvider é responsável por fornecer uma lista de ações e controladores juntamente com informações de rota associado para especificar exatamente qual configuração de roteamento for desejada para essas ações. Uma implementação de IDirectRouteProvider pode ser especificada ao chamar MapAttributes/MapHttpAttributeRoutes.

Personalizar IDirectRouteProvider será mais fácil estendendo nossa implementação do padrão, DefaultDirectRouteProvider. Essa classe fornece métodos substituíveis virtuais separados para alterar a lógica de detecção de atributos, criando entradas de rota e descoberta de prefixo da rota e o prefixo da área.

Com a nova extensibilidade de roteamento atributo do IDirectRouteProvider, um usuário poderia fazer o seguinte:

1. Suporte a herança de rotas de atributo. Por exemplo, no cenário a seguir controladores Blog e armazenamento estão usando uma convenção de rota do atributo é definida pelo BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Gere automaticamente nomes de rotas para rotas de atributo. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Modifique os prefixos de rota em um local central antes das rotas são adicionadas à tabela de rotas.
4. Filtre os controladores no qual você deseja que o roteamento de atributo para procurar. Esperamos blog em 3 e 4 em breve.

### <a name="facebook-fixes-for-changed-api-surface"></a>Correções de Facebook para a superfície de API alterada

O pacote do Facebook MVC [foi interrompida](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) devido a algumas alterações de API feitas pelo Facebook. Também estamos lançando um novo pacote do Facebook (Microsoft.AspNet.Facebook 1.0.0) para corrigir esses problemas.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Estrutura MVC/Web API em um projeto com 5.2.0 resultados de pacotes em 5.1.2 pacotes para aqueles que já não existe no projeto

Atualizar pacotes do NuGet do ASP.NET MVC 5.2.0 não atualizar as ferramentas do Visual Studio, como a estrutura do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET. Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (por exemplo, 5.1.2 na atualização 2). Como resultado, o ASP.NET scaffolding instalará a versão anterior (por exemplo, 5.1.2 na atualização 2) dos pacotes necessários, se eles já não estão disponíveis em seus projetos. No entanto, a estrutura do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos. Se você usar ASP.NET scaffolding depois de atualizar os pacotes de seus projetos Web API 2.2 ou 5.2 do ASP.NET MVC, assegure-se de que as versões de API da Web e ASP.NET MVC são consistentes.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Instalação do pacote Microsoft.jQuery.Unobtrusive.Validation NuGet falhará porque não é possível localizar uma versão de Microsoft.jQuery.Unobtrusive.Validation compatível com jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation requer jQuery &gt;= 1.8 e jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1.8) precisa jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Por isso, quando o NuGet instala a versão 1.8 do JQuery e jQuery.Validation 1.8 ao mesmo tempo, ele falha. Quando você encontrar esse problema, você pode simplesmente atualizar a versão do jQuery.Validation para &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) que tem o limite de jQuery fixado primeiro, você deve ser capaz de instalar Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>O jquery. Versão do pacote nuget validação 1.13.0 não reconhece alguns endereços de email internacionais

versão do pacote nuget jQuery.Validation 1.11.1 é a última versão conhecida que reconhece os seguintes endereços de email válidos. Todas as versões posteriores não consiga reconhecê-los. Por exemplo:

Padrão de internacionalização do endereço (EAI) email (por exemplo, [&#29992; &#25143;@domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + identificadores de recurso internacionalizado (íris) (ex., [&#29992; &#25143; @&#1076; &#1086; &#1084; &#1077; &#1085;. &#1088; &#1092; ](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

O problema é relatado no [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Realce de sintaxe para modos de exibição Razor no Visual Studio 2013

Se você atualizar para o ASP.NET MVC 5.2 sem atualizar o Visual Studio 2013, você não terá suporte de editor do Visual Studio para realce de sintaxe ao editar os modos de exibição do Razor. Você precisará atualizar o Visual Studio 2013 para obter esse suporte.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correções de bugs e atualizações de recursos secundários

Esta versão também inclui várias correções de bugs e recursos secundários atualizações. Você pode encontrar a lista completa aqui:

- [pacote 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Esta versão não tem novos recursos ou correções de bugs no MVC. Fizemos uma [alterar em páginas da Web](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) para uma melhoria significativa de desempenho e subsequentemente atualizou todos os outros pacotes dependentes que temos para depender essa nova versão de páginas da Web.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

Você pode ler sobre a versão [aqui](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Esta versão contém somente as correções de bugs. Você pode usar [esta consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver a lista de problemas corrigidos nesta versão.
