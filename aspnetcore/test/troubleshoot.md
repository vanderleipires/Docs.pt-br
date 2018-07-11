---
title: Solucionar problemas de projetos do ASP.NET Core
author: Rick-Anderson
description: Compreenda e solucione problemas de avisos e erros com projetos do ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938388"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Solucionar problemas de projetos do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Os links a seguir fornecem diretrizes de solução de problemas:

* [Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure](xref:host-and-deploy/azure-apps/troubleshoot)
* [Solucionar problemas do ASP.NET Core no IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC Conference (2018 Londres): Diagnosticar problemas em aplicativos do ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog do ASP.NET: Solucionando problemas de desempenho do ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Avisos do SDK do .NET core

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Os 32 bits e versões de 64 bits do SDK do .NET Core estão instaladas

No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:

> As versões de bit 32 e 64 do SDK do .NET Core são instaladas. Somente modelos das versões de 64 bits instalados em ' c:\\Program Files\\dotnet\\sdk\\' será exibida.

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/both32and64bit.png)

Esse aviso é exibido quando (x86) 32 bits e 64 bits (x64) versões dos [SDK do .NET Core](https://www.microsoft.com/net/download/all) estão instalados. Ambas as versões podem ser instaladas os motivos comuns incluem:

* Você originalmente baixou o instalador do SDK do .NET Core usando um computador de 32 bits, mas, em seguida, copiado entre e instalado em um computador de 64 bits.
* O .NET Core SDK de 32 bits foi instalado por outro aplicativo.
* A versão incorreta foi baixada e instalada.

Desinstale o .NET Core SDK 32 bits para evitar esse aviso. Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**. Se você entender por que o aviso ocorre e suas implicações, você pode ignorar o aviso.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>SDK do .NET Core é instalado em vários locais

No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:

> SDK do .NET Core é instalado em vários locais. Somente modelos dos SDKs instalados em ' c:\\Program Files\\dotnet\\sdk\\' será exibida.

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/multiplelocations.png)

Você verá esta mensagem quando você tem pelo menos uma instalação do SDK do .NET Core em um diretório fora do *c:\\arquivos de programas\\dotnet\\sdk\\*. Geralmente isso acontece quando o SDK do .NET Core foi implantado em um computador usando copiar/colar em vez do instalador MSI.

Desinstale o .NET Core SDK 32 bits para evitar esse aviso. Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**. Se você entender por que o aviso ocorre e suas implicações, você pode ignorar o aviso.

### <a name="no-net-core-sdks-were-detected"></a>Não há SDKs do .NET Core foram detectados

No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:

> Nenhum SDK do .NET Core foi detectado, verifique se que eles estão incluídos na variável de ambiente 'PATH'.

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/NoNetCore.png)

Esse aviso é exibido quando a variável de ambiente `PATH` não apontar para qualquer SDKs do .NET Core no computador. Para resolver esse problema:

* Instale ou verifique se o que SDK do .NET Core é instalado.
* Verifique o `PATH` variável de ambiente aponta para o local em que o SDK está instalado. O instalador normalmente define o `PATH`.
