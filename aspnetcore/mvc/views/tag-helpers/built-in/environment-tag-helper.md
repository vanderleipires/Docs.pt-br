---
title: Auxiliar de Marca de Ambiente no ASP.NET Core
author: pkellner
description: "Definição de Auxiliar de Marca de Ambiente do ASP.NET Core, incluindo todas as propriedades"
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="78dee-103">Auxiliar de Marca de Ambiente no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78dee-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="78dee-104">Por [Peter Kellner](http://peterkellner.net) e [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="78dee-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="78dee-105">O Auxiliar de Marca de Ambiente renderiza condicionalmente seu conteúdo contido com base no ambiente de hospedagem atual.</span><span class="sxs-lookup"><span data-stu-id="78dee-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="78dee-106">Seu único atributo `names` é uma lista separada por vírgula de nomes de ambiente. Se houver uma correspondência de um nome ao ambiente atual, ele disparará o conteúdo contido a ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="78dee-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="78dee-107">Atributos do Auxiliar de Marca de Ambiente</span><span class="sxs-lookup"><span data-stu-id="78dee-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="78dee-108">nomes</span><span class="sxs-lookup"><span data-stu-id="78dee-108">names</span></span>

<span data-ttu-id="78dee-109">Aceita um único nome de ambiente de hospedagem ou uma lista separada por vírgula de nomes de ambiente de hospedagem que disparam a renderização do conteúdo contido.</span><span class="sxs-lookup"><span data-stu-id="78dee-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="78dee-110">Esses valores são comparados com o valor atual retornado da propriedade estática `HostingEnvironment.EnvironmentName` do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="78dee-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="78dee-111">Esse valor é um dos seguintes: **Preparo**, **Desenvolvimento** ou **Produção**.</span><span class="sxs-lookup"><span data-stu-id="78dee-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="78dee-112">A comparação ignora o uso de maiúsculas.</span><span class="sxs-lookup"><span data-stu-id="78dee-112">The comparison ignores case.</span></span>

<span data-ttu-id="78dee-113">Um exemplo de um auxiliar de marca `environment` válido é:</span><span class="sxs-lookup"><span data-stu-id="78dee-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="78dee-114">incluir e excluir atributos</span><span class="sxs-lookup"><span data-stu-id="78dee-114">include and exclude attributes</span></span>

<span data-ttu-id="78dee-115">O ASP.NET Core 2.x adiciona os atributos `include` & `exclude`.</span><span class="sxs-lookup"><span data-stu-id="78dee-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="78dee-116">Esses atributos controlam a renderização do conteúdo contido com base nos nomes de ambiente de hospedagem incluídos ou excluídos.</span><span class="sxs-lookup"><span data-stu-id="78dee-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="78dee-117">incluir o Core ASP.NET Core 2.0 e posterior</span><span class="sxs-lookup"><span data-stu-id="78dee-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="78dee-118">A propriedade `include` tem um comportamento semelhante do atributo `names` no ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="78dee-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="78dee-119">excluir o ASP.NET Core 2.0 e posterior</span><span class="sxs-lookup"><span data-stu-id="78dee-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="78dee-120">Por outro lado, a propriedade `exclude` permite que `EnvironmentTagHelper` renderize o conteúdo contido para todos os nomes de ambiente de hospedagem, exceto aqueles especificados.</span><span class="sxs-lookup"><span data-stu-id="78dee-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="78dee-121">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="78dee-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
