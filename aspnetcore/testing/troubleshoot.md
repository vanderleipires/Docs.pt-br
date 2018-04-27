---
title: Solucionar problemas para o ASP.NET Core
author: Rick-Anderson
description: Compreender e solucionar problemas de avisos e erros com projetos do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: a75dc666621600e1e2fe36c29acbe7484bae9229
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a>Solucionar problemas em projetos do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Os links a seguir fornecem orientação para solução de problemas:

* [Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure](xref:host-and-deploy/azure-apps/troubleshoot)
* [Solucionar problemas do ASP.NET Core no IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: Diagnosticar problemas em aplicativos do ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>Avisos do SDK do .NET core

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Ambas as versões 32 e 64 bits do SDK do núcleo do .NET estão instaladas
No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver os seguintes avisos aparecem na parte superior: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/both32and64bit.png)

Esse aviso aparece quando (x86) 32 bits e versões de 64 bits (x64) da [.NET Core SDK](https://www.microsoft.com/net/download/all) estão instalados. Ambas as versões podem ser instaladas os motivos comuns incluem:

* Você baixou o instalador do SDK do .NET Core usando um computador de 32 bits, mas, em seguida, foi copiado em e originalmente instalado em um computador de 64 bits. 
* O SDK de núcleo do .NET 32 bits foi instalado por outro aplicativo.
* A versão incorreta foi baixada e instalada.

Desinstale o SDK de núcleo do .NET 32 bits para evitar este aviso. Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**. Se você entender por que o aviso é exibido e suas implicações, você pode ignorar o aviso.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>O SDK do .NET Core é instalado em vários locais
No **novo projeto** caixa de diálogo do ASP.NET Core, você poderá ver os seguintes avisos aparecem na parte superior: 

 O SDK do .NET Core é instalado em vários locais. Somente modelos a partir de SDK(s) instalado no ' C:\Program Files\dotnet\sdk\' será exibido.

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/multiplelocations.png)

Você está vendo esta mensagem porque você tem pelo menos uma instalação do SDK .NET Core em um diretório fora do * C:\Program Files\dotnet\sdk\*. Geralmente, isso acontece quando o SDK do .NET Core foi implantado em um computador usando copiar/colar em vez do instalador MSI.

Desinstale o SDK de núcleo do .NET 32 bits para evitar este aviso. Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**. Se você entender por que o aviso é exibido e suas implicações, você pode ignorar o aviso.
