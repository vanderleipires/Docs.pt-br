---
title: "Cadeias de caracteres de finalidade no núcleo do ASP.NET"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9d18c287-e0e6-4541-b09c-7fed45c902d9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: b25af7c1f4dd3c63734290e6ac82e2e30a030c61
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2017
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Hierarquia de propósito e multilocação no núcleo do ASP.NET

Como um IDataProtector também é implicitamente um IDataProtectionProvider, fins podem ser encadeada. Este provedor sentido. CreateProtector (["purpose1", "purpose2"]) é equivalente ao provedor. CreateProtector("purpose1"). CreateProtector("purpose2").

Isso permite que algumas relações hierárquicas interessantes através do sistema de proteção de dados. No exemplo anterior de [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), o componente SecureMessage pode chamar o provedor. CreateProtector("Contoso.Messaging.SecureMessage") quando inicial e o cache o resultado em uma particular `_myProvide` campo. Protetores futuras, em seguida, podem ser criados por meio de chamadas para `_myProvider.CreateProtector("User: username")`, e os protetores são usados para proteger as mensagens individuais.

Isso também pode ser invertido. Considere que hospeda vários locatários (um CMS parece razoável) e cada locatário podem ser configurado com seu próprio sistema de gerenciamento de autenticação e o estado de um único aplicativo lógico. O aplicativo de proteção tem um provedor de mestre único, e ele chama o provedor. CreateProtector ("locatário 1") e o provedor. CreateProtector ("locatário 2") para fornecer seu próprio fatia isolada do sistema de proteção de dados de cada locatário. Os locatários, em seguida, podem derivar seus próprio protetores individuais com base em suas necessidades, mas, independentemente de como eles tentam não é possível criar protetores que entrarem em conflito com qualquer outro locatário no sistema. Graficamente é representado como abaixo.

![Vários fins de aluguel](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Isso pressupõe que a proteção controles de aplicativo que APIs estão disponíveis para locatários individuais e que os locatários não podem executar código arbitrário no servidor. Se um locatário pode executar código arbitrário, eles executaria privada reflexão para interromper as garantias de isolamento ou pode apenas ler o material de chave mestre diretamente e derivar qualquer subchaves que desejarem.

Na verdade, o sistema de proteção de dados usa uma classificação de multilocação na configuração padrão da caixa. Por padrão, o material de chave mestra é armazenado na pasta de perfil de usuário da conta de processo do operador (ou o registro, para identidades de pool de aplicativos do IIS). Mas é na verdade bastante comum para usar uma única conta para executar vários aplicativos e, portanto, todos esses aplicativos acabar compartilhando a material da chave mestra. Para resolver isso, o sistema de proteção de dados automaticamente insere um identificador exclusivo por aplicativo como o primeiro elemento da cadeia de finalidade geral. Essa finalidade implícita serve para [isolar aplicativos individuais](../configuration/overview.md#data-protection-configuration-per-app-isolation) uns dos outros tratando efetivamente cada aplicativo como um locatário exclusivo dentro do sistema e o processo de criação de protetor é idêntico à imagem acima.
