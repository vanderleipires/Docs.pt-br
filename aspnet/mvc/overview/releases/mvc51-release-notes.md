---
uid: mvc/overview/releases/mvc51-release-notes
title: O que há de novo no ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: d2e67f64e725e73c3bf9021295da3fe870079a45
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825642"
---
<a name="whats-new-in-aspnet-mvc-51"></a>O que há de novo no ASP.NET MVC 5.1
====================
por [Microsoft](https://github.com/microsoft)

Este tópico descreve o que há de novo para ASP.NET Web MVC 5.1.

- [Requisitos de software](#SoftwareRequirements)
- [Baixar](#download)
- [Documentação](#documentation)
- [Novos recursos no ASP.NET MVC 5.1](#new-features)

    - [Aprimoramentos de roteamento de atributo](#AttributeRouting)
    - [Suporte de inicialização para modelos de editor](#Bootstrap)
    - [Suporte a enum nos modos de exibição](#Enum)
    - [Validação não invasiva para MinLength/MaxLength atributos](#Unobtrusive)
    - [Suporte a contexto 'this' em Ajax discreto](#thisContext)
- [Problemas conhecidos e alterações significativas](#KnownBreakingChanges)- [correções de bugs](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Requisitos de software

- Visual Studio 2012: Baixe o [ASP.NET e Web Tools 2013.1 para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Baixe o [Visual Studio 2013 atualização 1](https://go.microsoft.com/fwlink/?LinkId=390064). Esta atualização é necessária para a edição de exibições do Razor do ASP.NET MVC 5.1.

<a id="download"></a>
## <a name="download"></a>Baixar

Os recursos de tempo de execução são lançados como pacotes NuGet na Galeria do NuGet. Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação. O pacote mais recente da versão RTM do ASP.NET MVC 5.1 tem a seguinte versão: "5.1.2". Você pode instalar ou atualizar esses pacotes por meio [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). A versão também inclui pacotes localizados correspondentes no NuGet.

Você pode instalar ou atualizar para os pacotes do NuGet lançados por meio do Console do Gerenciador de pacotes NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre a versão RTM do ASP.NET MVC 5.1 estão disponíveis no site da web do ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Novos recursos no ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Aprimoramentos de roteamento de atributo

 Roteamento agora dá suporte a restrições, habilitando o controle de versão e o cabeçalho de atributo com base em seleção de rotas. Muitos aspectos de rotas de atributo agora podem ser personalizados por meio de `IDirectRouteFactory` interface e `RouteFactoryAttribute` classe. O prefixo da rota agora é extensível por meio de `IRoutePrefix` interface e `RoutePrefixAttribute` classe. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Suporte a enum nos modos de exibição

1. Novo `@Html.EnumDropDownListFor()` métodos auxiliares. Eles devem ser usados como a maioria dos auxiliares HTML com a ressalva de que a expressão deve ser avaliada como um [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo ou um [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) onde `T` é um [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo. Use `EnumHelper.IsValidForEnumHelper()` estes requisitos.
2. Novos `EnumHelper.GetSelectList()` métodos que retornam um `IList<SelectListItem>`. Isso é útil quando você precisar manipular uma lista de seleção antes de chamar, por exemplo, `@Html.DropDownListFor()`, ou quando você deseja exibir os nomes que `@Html.EnumDropDownListFor()` mostra.

O código a seguir mostra essas APIs.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Você pode ver um exemplo completo [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Suporte de inicialização para modelos de editor

Agora podemos permitir a passagem de atributos HTML na [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) como um [objeto anônimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Por exemplo:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Validação não invasiva para MinLengthAttribute e MaxLengthAttribute

Validação do lado do cliente para tipos de cadeia de caracteres e matriz agora terá suporte para as propriedades decoradas com o [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) e [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributos.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Suporte a contexto 'this' em Ajax discreto

As funções de retorno de chamada (`OnBegin, OnComplete, OnFailure, OnSuccess`) agora será capaz de localizar o elemento invocando via o `this` contexto. Por exemplo:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

### <a name="attribute-routing"></a>Roteamento de atributo

Ambiguidades em correspondências de roteamento de atributo agora relatará um erro em vez de escolher a primeira correspondência.

Rotas de atributo estão proibidas de usar o `{controller}` parâmetro e que usa o `{action}` colocado de parâmetro nas rotas em ações. Usa um desses parâmetros muito provavelmente poderia levar a ambiguidades. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>O scaffolding de API da Web do MVC em um projeto com resultados de pacotes 5.1 em 5.0 pacotes para aqueles que já não existem no projeto

Atualizando pacotes do NuGet para o RTM do ASP.NET MVC 5.1 não atualiza as ferramentas do Visual Studio, como o scaffolding do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET. Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (Version=5.0.0.0). Como resultado, o scaffolding do ASP.NET instalará a versão anterior (Version=5.0.0.0) dos pacotes necessários, se eles já não estão disponíveis em seus projetos. No entanto, o scaffolding do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos. Se você usar o scaffolding do ASP.NET depois de atualizar os pacotes de seus projetos Web API 2.1 ou ASP.NET MVC 5.1, verifique se que as versões de API da Web e ASP.NET MVC são consistentes. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Realce de sintaxe para as exibições do Razor no Visual Studio 2013

Se você atualizar para o RTM do ASP.NET MVC 5.1 sem atualizar o Visual Studio 2013, você não terá suporte ao editor do Visual Studio para o realce de sintaxe ao editar os modos de exibição do Razor. Você precisará atualizar o Visual Studio 2013 para obter esse suporte. 

### <a name="type-renames"></a>Renomeações de tipo

Alguns dos tipos usados para extensibilidade de roteamento de atributo são renomeados no RTM 5.1.

| **Nome do tipo antigo (5.1 RC)** | **Novo tipo de nome (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correções de Bug

Esta versão também inclui várias correções de bugs. Você pode encontrar a lista completa aqui:

- [5.1.0 pacote](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pacote de](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

O 5.1.2 pacote contém as atualizações do IntelliSense, mas não há correções de bugs.
