---
title: Auxiliar de Marca de Ambiente no ASP.NET Core
author: pkellner
description: Definição de Auxiliar de Marca de Ambiente do ASP.NET Core, incluindo todas as propriedades
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
ms.locfileid: "28901178"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Auxiliar de Marca de Ambiente no ASP.NET Core

Por [Peter Kellner](http://peterkellner.net) e [Hisham Bin Ateya](https://twitter.com/hishambinateya)

O Auxiliar de Marca de Ambiente renderiza condicionalmente seu conteúdo contido com base no ambiente de hospedagem atual. Seu único atributo `names` é uma lista separada por vírgula de nomes de ambiente. Se houver uma correspondência de um nome ao ambiente atual, ele disparará o conteúdo contido a ser renderizado.

## <a name="environment-tag-helper-attributes"></a>Atributos do Auxiliar de Marca de Ambiente

### <a name="names"></a>nomes

Aceita um único nome de ambiente de hospedagem ou uma lista separada por vírgula de nomes de ambiente de hospedagem que disparam a renderização do conteúdo contido.

Esses valores são comparados com o valor atual retornado da propriedade estática `HostingEnvironment.EnvironmentName` do ASP.NET Core.  Esse valor é um dos seguintes: **Preparo**, **Desenvolvimento** ou **Produção**. A comparação ignora o uso de maiúsculas.

Um exemplo de um auxiliar de marca `environment` válido é:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>incluir e excluir atributos

O ASP.NET Core 2.x adiciona os atributos `include` & `exclude`. Esses atributos controlam a renderização do conteúdo contido com base nos nomes de ambiente de hospedagem incluídos ou excluídos.

### <a name="include-aspnet-core-20-and-later"></a>incluir o Core ASP.NET Core 2.0 e posterior

A propriedade `include` tem um comportamento semelhante do atributo `names` no ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>excluir o ASP.NET Core 2.0 e posterior

Por outro lado, a propriedade `exclude` permite que `EnvironmentTagHelper` renderize o conteúdo contido para todos os nomes de ambiente de hospedagem, exceto aqueles especificados.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Recursos adicionais

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
