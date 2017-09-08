---
title: Limitar o tempo de vida das cargas protegidos
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 4ff13803b328c1e9dd2934c38c88b43f5798de03
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>Limitar o tempo de vida das cargas protegidos

Há cenários onde quer que o desenvolvedor do aplicativo para criar uma carga protegida que expira após um período de tempo definido. Por exemplo, a carga protegida pode representar um token de redefinição de senha que deve ser válido somente por uma hora. É certamente possível para o desenvolvedor criar seu próprio formato de carga que contém uma data de expiração incorporados e os desenvolvedores avançados talvez queira fazer isso mesmo assim, mas para a maioria dos desenvolvedores gerenciar esses expirações pode crescer entediante.

Para facilitar isso para nossos público de desenvolvedores, o pacote Microsoft.AspNetCore.DataProtection.Extensions contém APIs do utilitário para criar cargas expirarem automaticamente após um período de tempo definido. Essas APIs de suspensão do tipo ITimeLimitedDataProtector.

## <a name="api-usage"></a>Uso de API

A interface ITimeLimitedDataProtector é a interface principal para proteger e ao desproteger o limite de tempo / automaticamente expirando cargas. Para criar uma instância de um ITimeLimitedDataProtector, primeiro será necessário uma instância de uma expressão [IDataProtector](overview.md) construído com uma finalidade específica. Quando a instância de IDataProtector estiver disponível, chame o método de extensão IDataProtector.ToTimeLimitedDataProtector para retornar um protetor com recursos internos de expiração.

ITimeLimitedDataProtector apresenta os seguintes métodos de extensão e de superfície de API:

* CreateProtector (objetivo de cadeia de caracteres): ITimeLimitedDataProtector essa API é semelhante para o IDataProtectionProvider.CreateProtector existente que pode ser usado para criar [finalidade cadeias](purpose-strings.md) de um protetor de tempo limite de raiz.

* Proteger (byte [] texto sem formatação, expiração de DateTimeOffset): byte]

* Proteger (texto sem formatação do byte [], tempo de vida de TimeSpan): byte]

* Proteger (byte [] texto sem formatação): byte]

* Proteger (cadeia de caracteres em texto sem formatação, expiração de DateTimeOffset): cadeia de caracteres

* Proteger (texto sem formatação de cadeia de caracteres, tempo de vida de TimeSpan): cadeia de caracteres

* Proteger (cadeia de caracteres em texto sem formatação): cadeia de caracteres

Além de métodos de proteger core que levar apenas o texto não criptografado, há novas sobrecargas que permitem especificar a data de validade da carga. A data de validade pode ser especificada como uma data absoluta (por meio de um DateTimeOffset) ou como uma hora relativa (da hora atual do sistema, por meio de um período de tempo). Se uma sobrecarga que não tem uma expiração é chamada, a carga será considerada nunca para expirar.

* Desproteger (byte [] protectedData, limite de expiração de DateTimeOffset): byte]

* Desproteger (byte [] protectedData): byte]

* Desproteger (protectedData de cadeia de caracteres, limite de expiração de DateTimeOffset): cadeia de caracteres

* Desproteger (cadeia de caracteres protectedData): cadeia de caracteres

Os métodos de Desproteger retornam os dados desprotegidos originais. Se a carga ainda não tiver expirado, a expiração absoluta é retornada como um parâmetro junto com os dados desprotegidos originais out opcional. Se a carga tiver expirada, todas as sobrecargas do método Desproteger lançará CryptographicException.

>[!WARNING]
> Não é recomendável usar essas APIs para proteger cargas que requerem a persistência de longo prazo ou indefinida. "Pode suportar para as cargas protegidas sejam irrecuperáveis permanentemente após um mês?" pode servir como uma boa regra prática; Se a resposta for nenhum desenvolvedores, em seguida, considere APIs alternativas.

O exemplo abaixo usa o [caminhos de código não DI](../configuration/non-di-scenarios.md) para instanciar o sistema de proteção de dados. Para executar este exemplo, certifique-se de que você adicionou uma referência ao pacote Microsoft.AspNetCore.DataProtection.Extensions primeiro.

[!code-none[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
