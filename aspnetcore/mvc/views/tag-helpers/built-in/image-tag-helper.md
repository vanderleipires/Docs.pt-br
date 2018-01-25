---
title: Auxiliar de marca de imagem | Microsoft Docs
author: pkellner
description: Mostra como trabalhar com o auxiliar de marca de imagem
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: d0857e1926c341b2357bc824fa379c4fc30affbc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="imagetaghelper"></a><span data-ttu-id="17736-103">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="17736-103">ImageTagHelper</span></span>

<span data-ttu-id="17736-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="17736-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="17736-105">O auxiliar de marca de imagem aprimora o `img` (`<img>`) marca.</span><span class="sxs-lookup"><span data-stu-id="17736-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="17736-106">Ele requer um `src` marca, bem como a `boolean` atributo `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="17736-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="17736-107">Se a origem da imagem (`src`) é um arquivo estático no servidor host da web, um cache exclusivo de eliminação de cadeia de caracteres é anexado como um parâmetro de consulta para a origem da imagem.</span><span class="sxs-lookup"><span data-stu-id="17736-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="17736-108">Isso garante que, se o arquivo no servidor host da web for alterado, uma URL de solicitação exclusivo é gerada que inclui o parâmetro de solicitação atualizada.</span><span class="sxs-lookup"><span data-stu-id="17736-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="17736-109">O cache de eliminação de cadeia de caracteres é um valor exclusivo que representa o hash do arquivo de imagem estática.</span><span class="sxs-lookup"><span data-stu-id="17736-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="17736-110">Se a origem da imagem (`src`) não é um arquivo estático (por exemplo uma URL remota ou o arquivo não existe no servidor), o `<img>` da marca `src` atributo é gerado sem incluir o parâmetro de cadeia de caracteres de consulta de cache.</span><span class="sxs-lookup"><span data-stu-id="17736-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="17736-111">Atributos de auxiliar de marca de imagem</span><span class="sxs-lookup"><span data-stu-id="17736-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="17736-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="17736-112">asp-append-version</span></span>

<span data-ttu-id="17736-113">Quando especificado junto com um `src` atributo, o auxiliar de marca de imagem é invocado.</span><span class="sxs-lookup"><span data-stu-id="17736-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="17736-114">Um exemplo de uma opção válida `img` auxiliar de marca é:</span><span class="sxs-lookup"><span data-stu-id="17736-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="17736-115">Se o arquivo estático existe no diretório *... wwwroot/images/asplogo.PNG* html gerado é semelhante à seguinte (o hash será diferente):</span><span class="sxs-lookup"><span data-stu-id="17736-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="17736-116">O valor atribuído ao parâmetro `v` é o valor de hash do arquivo no disco.</span><span class="sxs-lookup"><span data-stu-id="17736-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="17736-117">Se o servidor web não é possível obter o acesso de leitura ao arquivo estático referenciado não `v` parâmetros é adicionada para o `src` atributo.</span><span class="sxs-lookup"><span data-stu-id="17736-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="17736-118">src</span><span class="sxs-lookup"><span data-stu-id="17736-118">src</span></span>

<span data-ttu-id="17736-119">Para ativar o auxiliar de marca de imagem, o atributo src é necessária no `<img>` elemento.</span><span class="sxs-lookup"><span data-stu-id="17736-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="17736-120">O auxiliar de marca de imagem usa a `Cache` provedor no servidor web local para armazenar o calculado `Sha512` de um determinado arquivo.</span><span class="sxs-lookup"><span data-stu-id="17736-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="17736-121">Se o arquivo é solicitado novamente o `Sha512` não precisa ser recalculada.</span><span class="sxs-lookup"><span data-stu-id="17736-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="17736-122">O Cache é invalidado por um Inspetor de arquivo que é anexado ao arquivo quando o arquivo `Sha512` é calculado.</span><span class="sxs-lookup"><span data-stu-id="17736-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17736-123">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="17736-123">Additional resources</span></span>

* <xref:performance/caching/memory>
