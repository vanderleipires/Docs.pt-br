---
title: Hierarquia de propósito e multilocação no núcleo do ASP.NET
author: rick-anderson
description: Saiba mais sobre hierarquia de cadeia de caracteres de propósito e multilocação como ele se relaciona com as APIs de proteção de dados do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: a1ca2c32f95a86b877cbbe94d106d23b86800443
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30078044"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Hierarquia de propósito e multilocação no núcleo do ASP.NET

Como um `IDataProtector` também é implicitamente um `IDataProtectionProvider`, fins podem ser encadeada. Nesse sentido, `provider.CreateProtector([ "purpose1", "purpose2" ])` é equivalente a `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Isso permite que algumas relações hierárquicas interessantes através do sistema de proteção de dados. No exemplo anterior de [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), pode chamar o componente SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` inicial de uma vez e armazenar em cache o resultado em uma particular `_myProvide` campo. Protetores futuras, em seguida, podem ser criados por meio de chamadas para `_myProvider.CreateProtector("User: username")`, e os protetores são usados para proteger as mensagens individuais.

Isso também pode ser invertido. Considere que hospeda vários locatários (um CMS parece razoável) e cada locatário podem ser configurado com seu próprio sistema de gerenciamento de autenticação e o estado de um único aplicativo lógico. O aplicativo de proteção tem um único provedor mestre e chama `provider.CreateProtector("Tenant 1")` e `provider.CreateProtector("Tenant 2")` para fornecer seu próprio fatia isolada do sistema de proteção de dados de cada locatário. Os locatários, em seguida, podem derivar seus próprio protetores individuais com base em suas necessidades, mas, independentemente de como eles tentam não é possível criar protetores que entrarem em conflito com qualquer outro locatário no sistema. Graficamente, isso é representado como abaixo.

![Vários fins de aluguel](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Isso pressupõe que a proteção controles de aplicativo que APIs estão disponíveis para locatários individuais e que os locatários não podem executar código arbitrário no servidor. Se um locatário pode executar código arbitrário, eles executaria privada reflexão para interromper as garantias de isolamento ou pode apenas ler o material de chave mestre diretamente e derivar qualquer subchaves que desejarem.

Na verdade, o sistema de proteção de dados usa uma classificação de multilocação na configuração padrão da caixa. Por padrão, o material de chave mestra é armazenado na pasta de perfil de usuário da conta de processo do operador (ou o registro, para identidades de pool de aplicativos do IIS). Mas é na verdade bastante comum para usar uma única conta para executar vários aplicativos e, portanto, todos esses aplicativos acabar compartilhando a material da chave mestra. Para resolver isso, o sistema de proteção de dados automaticamente insere um identificador exclusivo por aplicativo como o primeiro elemento da cadeia de finalidade geral. Essa finalidade implícita serve para [isolar aplicativos individuais](xref:security/data-protection/configuration/overview#per-application-isolation) uns dos outros tratando efetivamente cada aplicativo como um locatário exclusivo dentro do sistema e o processo de criação de protetor é idêntico à imagem acima.
