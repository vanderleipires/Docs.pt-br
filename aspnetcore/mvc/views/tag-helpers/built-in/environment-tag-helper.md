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
