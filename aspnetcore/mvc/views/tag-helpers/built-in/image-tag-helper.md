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
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072256"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="f90f1-103">Auxiliar de Marca de Imagem no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f90f1-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="f90f1-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="f90f1-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="f90f1-105">O Auxiliar de Marca de Imagem aprimora a marca `img` (`<img>`).</span><span class="sxs-lookup"><span data-stu-id="f90f1-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="f90f1-106">Ele exige uma marca `src`, bem como o atributo de `boolean` `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="f90f1-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="f90f1-107">Se a origem da imagem (`src`) é um arquivo estático no servidor Web host, uma cadeia de caracteres de extrapolação de cache exclusivo é acrescentada como um parâmetro de consulta à origem da imagem.</span><span class="sxs-lookup"><span data-stu-id="f90f1-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="f90f1-108">Isso garante que, se o arquivo no servidor Web host é alterado, uma URL de solicitação exclusiva que inclui o parâmetro de solicitação atualizado seja gerada.</span><span class="sxs-lookup"><span data-stu-id="f90f1-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="f90f1-109">A cadeia de caracteres de extrapolação de cache é um valor exclusivo que representa o hash do arquivo de imagem estática.</span><span class="sxs-lookup"><span data-stu-id="f90f1-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="f90f1-110">Se a origem da imagem (`src`) não é um arquivo estático (por exemplo, uma URL remota ou o arquivo não existe no servidor), o atributo `src` da marca `<img>` é gerado sem um parâmetro da cadeia de caracteres de consulta de extrapolação de cache.</span><span class="sxs-lookup"><span data-stu-id="f90f1-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="f90f1-111">Atributos de Auxiliar de Marca de Imagem</span><span class="sxs-lookup"><span data-stu-id="f90f1-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="f90f1-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="f90f1-112">asp-append-version</span></span>

<span data-ttu-id="f90f1-113">Quando especificado junto com um atributo `src`, o Auxiliar de Marca de Imagem é invocado.</span><span class="sxs-lookup"><span data-stu-id="f90f1-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="f90f1-114">Um exemplo de um auxiliar de marca `img` válido é:</span><span class="sxs-lookup"><span data-stu-id="f90f1-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="f90f1-115">Se o arquivo estático existe no diretório *.wwwroot/images/asplogo.png*, o HTML gerado é semelhante ao seguinte (o hash será diferente):</span><span class="sxs-lookup"><span data-stu-id="f90f1-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="f90f1-116">O valor atribuído ao parâmetro `v` é o valor de hash do arquivo em disco.</span><span class="sxs-lookup"><span data-stu-id="f90f1-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="f90f1-117">Se o servidor Web não pode obter o acesso de leitura ao arquivo estático referenciado, nenhum parâmetro `v` é adicionado ao atributo `src`.</span><span class="sxs-lookup"><span data-stu-id="f90f1-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="f90f1-118">src</span><span class="sxs-lookup"><span data-stu-id="f90f1-118">src</span></span>

<span data-ttu-id="f90f1-119">Para ativar o Auxiliar de Marca de Imagem, o atributo src é obrigatório no elemento `<img>`.</span><span class="sxs-lookup"><span data-stu-id="f90f1-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="f90f1-120">O Auxiliar de Marca de Imagem usa o provedor `Cache` no servidor Web local para armazenar o `Sha512` calculado de determinado arquivo.</span><span class="sxs-lookup"><span data-stu-id="f90f1-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="f90f1-121">Se o arquivo é solicitado novamente, o `Sha512` não precisa ser recalculado.</span><span class="sxs-lookup"><span data-stu-id="f90f1-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="f90f1-122">O Cache é invalidado por um inspetor de arquivo que é anexado ao arquivo quando o `Sha512` do arquivo é calculado.</span><span class="sxs-lookup"><span data-stu-id="f90f1-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f90f1-123">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="f90f1-123">Additional resources</span></span>

* <xref:performance/caching/memory>
