---
title: Auxiliar de Marca de Ambiente no ASP.NET Core
author: pkellner
description: Definição de Auxiliar de Marca de Ambiente do ASP.NET Core, incluindo todas as propriedades
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 05c07b06a4fedac0b0ff39d168807f5e2e6996cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276910"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="c214a-103">Auxiliar de Marca de Ambiente no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c214a-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="c214a-104">Por [Peter Kellner](http://peterkellner.net) e [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="c214a-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="c214a-105">O Auxiliar de Marca de Ambiente renderiza condicionalmente seu conteúdo contido com base no ambiente de hospedagem atual.</span><span class="sxs-lookup"><span data-stu-id="c214a-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="c214a-106">Seu único atributo `names` é uma lista separada por vírgula de nomes de ambiente. Se houver uma correspondência de um nome ao ambiente atual, ele disparará o conteúdo contido a ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="c214a-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="c214a-107">Atributos do Auxiliar de Marca de Ambiente</span><span class="sxs-lookup"><span data-stu-id="c214a-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="c214a-108">nomes</span><span class="sxs-lookup"><span data-stu-id="c214a-108">names</span></span>

<span data-ttu-id="c214a-109">Aceita um único nome de ambiente de hospedagem ou uma lista separada por vírgula de nomes de ambiente de hospedagem que disparam a renderização do conteúdo contido.</span><span class="sxs-lookup"><span data-stu-id="c214a-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="c214a-110">Esses valores são comparados com o valor atual retornado da propriedade estática `HostingEnvironment.EnvironmentName` do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c214a-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="c214a-111">Esse valor é um dos seguintes: **Preparo**, **Desenvolvimento** ou **Produção**.</span><span class="sxs-lookup"><span data-stu-id="c214a-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="c214a-112">A comparação ignora o uso de maiúsculas.</span><span class="sxs-lookup"><span data-stu-id="c214a-112">The comparison ignores case.</span></span>

<span data-ttu-id="c214a-113">Um exemplo de um auxiliar de marca `environment` válido é:</span><span class="sxs-lookup"><span data-stu-id="c214a-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="c214a-114">incluir e excluir atributos</span><span class="sxs-lookup"><span data-stu-id="c214a-114">include and exclude attributes</span></span>

<span data-ttu-id="c214a-115">O ASP.NET Core 2.x adiciona os atributos `include` & `exclude`.</span><span class="sxs-lookup"><span data-stu-id="c214a-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="c214a-116">Esses atributos controlam a renderização do conteúdo contido com base nos nomes de ambiente de hospedagem incluídos ou excluídos.</span><span class="sxs-lookup"><span data-stu-id="c214a-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="c214a-117">incluir o Core ASP.NET Core 2.0 e posterior</span><span class="sxs-lookup"><span data-stu-id="c214a-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="c214a-118">A propriedade `include` tem um comportamento semelhante do atributo `names` no ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="c214a-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="c214a-119">excluir o ASP.NET Core 2.0 e posterior</span><span class="sxs-lookup"><span data-stu-id="c214a-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="c214a-120">Por outro lado, a propriedade `exclude` permite que `EnvironmentTagHelper` renderize o conteúdo contido para todos os nomes de ambiente de hospedagem, exceto aqueles especificados.</span><span class="sxs-lookup"><span data-stu-id="c214a-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="c214a-121">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c214a-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
