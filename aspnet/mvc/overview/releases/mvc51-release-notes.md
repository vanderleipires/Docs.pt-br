---
uid: mvc/overview/releases/mvc51-release-notes
title: O que há de novo no ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
ms.locfileid: "26504045"
---
<a name="whats-new-in-aspnet-mvc-51"></a>O que há de novo no ASP.NET MVC 5.1
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para ASP.NET Web MVC 5.1.

- [Requisitos de software](#SoftwareRequirements)
- [Baixar](#download)
- [Documentação](#documentation)
- [Novos recursos no ASP.NET MVC 5.1](#new-features)

    - [Aprimoramentos de roteamentos de atributo](#AttributeRouting)
    - [Suporte de inicialização para modelos do editor](#Bootstrap)
    - [Suporte a enum nos modos de exibição](#Enum)
    - [Validação discreta para atributos de MinLength/MaxLength](#Unobtrusive)
    - [Dando suporte o contexto 'this' em Ajax discreto](#thisContext)
- [Problemas conhecidos e as alterações recentes](#KnownBreakingChanges)- [correções de bugs](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Requisitos de software

- O Visual Studio 2012: Baixe [2013.1 para Visual Studio 2012 de ferramentas do ASP.NET e Web](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Baixar o [Visual Studio 2013 atualização 1](https://go.microsoft.com/fwlink/?LinkId=390064). Essa atualização é necessária para edição exibições Razor do ASP.NET MVC 5.1.

<a id="download"></a>
## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes do NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote mais recente do RTM do ASP.NET MVC 5.1 tem a seguinte versão: "5.1.2". Você pode instalar ou atualizar esses pacotes por meio de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados usando o NuGet Package Manager Console:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre a versão RTM do ASP.NET MVC 5.1 estão disponíveis no site da web ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Novos recursos no ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Aprimoramentos de roteamentos de atributo

 Roteamento agora dá suporte a restrições, habilitar o controle de versão e o cabeçalho de atributo com base em seleção de rota. Muitos aspectos de rotas de atributo agora são personalizáveis por meio de `IDirectRouteFactory` interface e `RouteFactoryAttribute` classe. O prefixo da rota agora é extensível via o `IRoutePrefix` interface e `RoutePrefixAttribute` classe. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Suporte a enum nos modos de exibição

1. Novo `@Html.EnumDropDownListFor()` métodos auxiliares. Eles devem ser usados como a maioria dos auxiliares HTML com a ressalva de que a expressão deve ser avaliada como um [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo ou um [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) onde `T` é um [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo. Use `EnumHelper.IsValidForEnumHelper()` para verificar se esses requisitos.
2. Novo `EnumHelper.GetSelectList()` métodos que retornam um `IList<SelectListItem>`. Isso é útil quando você precisa manipular uma lista de seleção antes de chamar, por exemplo, `@Html.DropDownListFor()`, ou quando você deseja exibir os nomes que `@Html.EnumDropDownListFor()` mostra.

O código a seguir mostra essas APIs.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Você pode ver um exemplo completo [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Suporte de inicialização para modelos do editor

Agora permita a passagem nos atributos HTML na [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) como um [objeto anônimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Por exemplo:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Validação discreta para MinLengthAttribute e MaxLengthAttribute

Validação do lado do cliente para tipos de cadeia de caracteres e matriz agora terá suporte para propriedades decorados com o [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) e [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributos.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Dando suporte o contexto 'this' em Ajax discreto

As funções de retorno de chamada (`OnBegin, OnComplete, OnFailure, OnSuccess`) agora será capaz de localizar o elemento chamar por meio de `this` contexto. Por exemplo:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

### <a name="attribute-routing"></a>Roteamento de atributo

Ambiguidades em correspondências de roteamento de atributo agora relatará um erro em vez de escolher a primeira correspondência.

Rotas de atributo são proibidas de usar o `{controller}` parâmetro e usa o `{action}` parâmetro nas rotas colocadas em ações. Usa esses parâmetros seria muito provavelmente resultará em ambiguidades. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Estrutura MVC/Web API em um projeto com 5.1 resultados de pacotes em 5.0 pacotes para aqueles que já não existe no projeto

Atualizando pacotes do NuGet para a versão RTM do ASP.NET MVC 5.1 não atualizar as ferramentas do Visual Studio, como a estrutura do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET. Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (5.0.0.0). Como resultado, o ASP.NET scaffolding instalará a versão anterior (5.0.0.0) dos pacotes necessários, se eles já não estão disponíveis em seus projetos. No entanto, a estrutura do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos. Se você usar o ASP.NET scaffolding depois de atualizar os pacotes de seus projetos Web API 2.1 ou ASP.NET MVC 5.1, verifique se que as versões de API da Web e ASP.NET MVC são consistentes. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Realce de sintaxe para modos de exibição Razor no Visual Studio 2013

Se você atualizar para a versão RTM do ASP.NET MVC 5.1 sem atualizar o Visual Studio 2013, você não terá suporte de editor do Visual Studio para realce de sintaxe ao editar os modos de exibição do Razor. Você precisará atualizar o Visual Studio 2013 para obter esse suporte. 

### <a name="type-renames"></a>Renomeações de tipo

Alguns dos tipos usados para extensibilidade de roteamento de atributo são renomeados no RTM 5.1.

| **Nome do tipo antigo (5.1 RC)** | **Novo tipo de nome (RTM 5.1)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correções de Bug

Esta versão também inclui várias correções de bug. Você pode encontrar a lista completa aqui:

- [5.1.0 pacote de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pacote de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

O 5.1.2 pacote contém atualizações do IntelliSense, mas nenhuma correção de bug.
