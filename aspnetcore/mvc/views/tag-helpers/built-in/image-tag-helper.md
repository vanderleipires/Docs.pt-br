---
title: Auxiliar de Marca de Imagem no ASP.NET Core
author: pkellner
description: Mostra como trabalhar com o Auxiliar de Marca de Imagem
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 75bddd01a95f3ae0b1ea19de0eb64ad3b9066319
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="imagetaghelper"></a><span data-ttu-id="338f2-103">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="338f2-103">ImageTagHelper</span></span>

<span data-ttu-id="338f2-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="338f2-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="338f2-105">O Auxiliar de Marca de Imagem aprimora a marca `img` (`<img>`).</span><span class="sxs-lookup"><span data-stu-id="338f2-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="338f2-106">Ele exige uma marca `src`, bem como o atributo de `boolean` `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="338f2-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="338f2-107">Se a origem da imagem (`src`) é um arquivo estático no servidor Web host, uma cadeia de caracteres de extrapolação de cache exclusivo é acrescentada como um parâmetro de consulta à origem da imagem.</span><span class="sxs-lookup"><span data-stu-id="338f2-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="338f2-108">Isso garante que, se o arquivo no servidor Web host é alterado, uma URL de solicitação exclusiva que inclui o parâmetro de solicitação atualizado seja gerada.</span><span class="sxs-lookup"><span data-stu-id="338f2-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="338f2-109">A cadeia de caracteres de extrapolação de cache é um valor exclusivo que representa o hash do arquivo de imagem estática.</span><span class="sxs-lookup"><span data-stu-id="338f2-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="338f2-110">Se a origem da imagem (`src`) não é um arquivo estático (por exemplo, uma URL remota ou o arquivo não existe no servidor), o atributo `src` da marca `<img>` é gerado sem um parâmetro da cadeia de caracteres de consulta de extrapolação de cache.</span><span class="sxs-lookup"><span data-stu-id="338f2-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="338f2-111">Atributos de Auxiliar de Marca de Imagem</span><span class="sxs-lookup"><span data-stu-id="338f2-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="338f2-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="338f2-112">asp-append-version</span></span>

<span data-ttu-id="338f2-113">Quando especificado junto com um atributo `src`, o Auxiliar de Marca de Imagem é invocado.</span><span class="sxs-lookup"><span data-stu-id="338f2-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="338f2-114">Um exemplo de um auxiliar de marca `img` válido é:</span><span class="sxs-lookup"><span data-stu-id="338f2-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="338f2-115">Se o arquivo estático existe no diretório *.wwwroot/images/asplogo.png*, o HTML gerado é semelhante ao seguinte (o hash será diferente):</span><span class="sxs-lookup"><span data-stu-id="338f2-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="338f2-116">O valor atribuído ao parâmetro `v` é o valor de hash do arquivo em disco.</span><span class="sxs-lookup"><span data-stu-id="338f2-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="338f2-117">Se o servidor Web não pode obter o acesso de leitura ao arquivo estático referenciado, nenhum parâmetro `v` é adicionado ao atributo `src`.</span><span class="sxs-lookup"><span data-stu-id="338f2-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="338f2-118">src</span><span class="sxs-lookup"><span data-stu-id="338f2-118">src</span></span>

<span data-ttu-id="338f2-119">Para ativar o Auxiliar de Marca de Imagem, o atributo src é obrigatório no elemento `<img>`.</span><span class="sxs-lookup"><span data-stu-id="338f2-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="338f2-120">O Auxiliar de Marca de Imagem usa o provedor `Cache` no servidor Web local para armazenar o `Sha512` calculado de determinado arquivo.</span><span class="sxs-lookup"><span data-stu-id="338f2-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="338f2-121">Se o arquivo é solicitado novamente, o `Sha512` não precisa ser recalculado.</span><span class="sxs-lookup"><span data-stu-id="338f2-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="338f2-122">O Cache é invalidado por um inspetor de arquivo que é anexado ao arquivo quando o `Sha512` do arquivo é calculado.</span><span class="sxs-lookup"><span data-stu-id="338f2-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="338f2-123">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="338f2-123">Additional resources</span></span>

* <xref:performance/caching/memory>
