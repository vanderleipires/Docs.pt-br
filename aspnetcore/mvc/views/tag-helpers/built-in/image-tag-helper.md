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
ms.openlocfilehash: 438c5afb96dce6d8978d26159a3b460614111988
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="imagetaghelper"></a>ImageTagHelper

Por [Peter Kellner](http://peterkellner.net) 

O auxiliar de marca de imagem aprimora o `img` (`<img>`) marca. Ele requer um `src` marca, bem como a `boolean` atributo `asp-append-version`.

Se a origem da imagem (`src`) é um arquivo estático no servidor host da web, um cache exclusivo de eliminação de cadeia de caracteres é anexado como um parâmetro de consulta para a origem da imagem. Isso garante que, se o arquivo no servidor host da web for alterado, uma URL de solicitação exclusivo é gerada que inclui o parâmetro de solicitação atualizada. O cache de eliminação de cadeia de caracteres é um valor exclusivo que representa o hash do arquivo de imagem estática.

Se a origem da imagem (`src`) não é um arquivo estático (por exemplo uma URL remota ou o arquivo não existe no servidor), o `<img>` da marca `src` atributo é gerado sem incluir o parâmetro de cadeia de caracteres de consulta de cache.

## <a name="image-tag-helper-attributes"></a>Atributos de auxiliar de marca de imagem


### <a name="asp-append-version"></a>asp-append-version

Quando especificado junto com um `src` atributo, o auxiliar de marca de imagem é invocado.

Um exemplo de uma opção válida `img` auxiliar de marca é:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Se o arquivo estático existe no diretório *... wwwroot/images/asplogo.PNG* html gerado é semelhante à seguinte (o hash será diferente):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

O valor atribuído ao parâmetro `v` é o valor de hash do arquivo no disco. Se o servidor web não é possível obter o acesso de leitura ao arquivo estático referenciado não `v` parâmetros é adicionada para o `src` atributo.

- - -

### <a name="src"></a>src

Para ativar o auxiliar de marca de imagem, o atributo src é necessária no `<img>` elemento. 

> [!NOTE]
> O auxiliar de marca de imagem usa a `Cache` provedor no servidor web local para armazenar o calculado `Sha512` de um determinado arquivo. Se o arquivo é solicitado novamente o `Sha512` não precisa ser recalculada. O Cache é invalidado por um Inspetor de arquivo que é anexado ao arquivo quando o arquivo `Sha512` é calculado.

## <a name="additional-resources"></a>Recursos adicionais

* <xref:performance/caching/memory>
