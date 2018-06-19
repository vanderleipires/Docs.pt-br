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
# <a name="image-tag-helper-in-aspnet-core"></a>Auxiliar de Marca de Imagem no ASP.NET Core

Por [Peter Kellner](http://peterkellner.net) 

O Auxiliar de Marca de Imagem aprimora a marca `img` (`<img>`). Ele exige uma marca `src`, bem como o atributo de `boolean` `asp-append-version`.

Se a origem da imagem (`src`) é um arquivo estático no servidor Web host, uma cadeia de caracteres de extrapolação de cache exclusivo é acrescentada como um parâmetro de consulta à origem da imagem. Isso garante que, se o arquivo no servidor Web host é alterado, uma URL de solicitação exclusiva que inclui o parâmetro de solicitação atualizado seja gerada. A cadeia de caracteres de extrapolação de cache é um valor exclusivo que representa o hash do arquivo de imagem estática.

Se a origem da imagem (`src`) não é um arquivo estático (por exemplo, uma URL remota ou o arquivo não existe no servidor), o atributo `src` da marca `<img>` é gerado sem um parâmetro da cadeia de caracteres de consulta de extrapolação de cache.

## <a name="image-tag-helper-attributes"></a>Atributos de Auxiliar de Marca de Imagem


### <a name="asp-append-version"></a>asp-append-version

Quando especificado junto com um atributo `src`, o Auxiliar de Marca de Imagem é invocado.

Um exemplo de um auxiliar de marca `img` válido é:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Se o arquivo estático existe no diretório *.wwwroot/images/asplogo.png*, o HTML gerado é semelhante ao seguinte (o hash será diferente):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

O valor atribuído ao parâmetro `v` é o valor de hash do arquivo em disco. Se o servidor Web não pode obter o acesso de leitura ao arquivo estático referenciado, nenhum parâmetro `v` é adicionado ao atributo `src`.

- - -

### <a name="src"></a>src

Para ativar o Auxiliar de Marca de Imagem, o atributo src é obrigatório no elemento `<img>`. 

> [!NOTE]
> O Auxiliar de Marca de Imagem usa o provedor `Cache` no servidor Web local para armazenar o `Sha512` calculado de determinado arquivo. Se o arquivo é solicitado novamente, o `Sha512` não precisa ser recalculado. O Cache é invalidado por um inspetor de arquivo que é anexado ao arquivo quando o `Sha512` do arquivo é calculado.

## <a name="additional-resources"></a>Recursos adicionais

* <xref:performance/caching/memory>
