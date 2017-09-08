---
title: "Auxiliar de marca de ambiente no núcleo do ASP.NET"
author: pkellner
description: Auxiliar de marca de ambiente do ASP.Net Core definidos incluindo todas as propriedades
keywords: ASP.NET Core, o auxiliar de marca
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: 5870d9ebd02becf29f892c91310022d3b9a6b7af
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Auxiliar de marca de ambiente no núcleo do ASP.NET

Por [Peter Kellner](http://peterkellner.net) e [Ateya Hisham Bin](https://twitter.com/hishambinateya)

O auxiliar de marca de ambiente condicionalmente renderiza o conteúdo incluído com base no ambiente de hospedagem atual. O único atributo `names` é uma lista separada por vírgulas de ambiente nomes, que se qualquer correspondência com o ambiente atual, irá disparar o conteúdo entre a ser renderizado.

## <a name="environment-tag-helper-attributes"></a>Atributos de auxiliar de marca de ambiente

### <a name="names"></a>nomes

Aceita um único nome de ambiente de hospedagem ou uma lista separada por vírgulas de nomes de ambiente que disparam o processamento do conteúdo do anexo de hospedagem.

Esses valores são comparados com o valor retornado da propriedade estática ASP.NET Core `HostingEnvironment.EnvironmentName`.  Esse valor é um dos seguintes: **preparo**; **Desenvolvimento** ou **produção**. A comparação diferencia maiusculas de minúsculas.

Um exemplo de uma opção válida `environment` auxiliar de marca é:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>incluir e excluir atributos

ASP.NET Core 2. x adiciona o `include`  &  `exclude` atributos. Esses atributos controlam a processar o conteúdo incluído com base nos nomes de ambiente hospedagem incluído ou excluído.

### <a name="include-aspnet-core-20-and-later"></a>incluir Core ASP.NET 2.0 e posterior

O `include` propriedade tem um comportamento semelhante do `names` atributo no ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>excluir o núcleo do ASP.NET 2.0 e posterior

Em contraste, o `exclude` permite que a propriedade de `EnvironmentTagHelper` renderizar o conteúdo entre todos os nomes de ambiente de hospedagem exceto a (s) que você especificou.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Recursos adicionais

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>