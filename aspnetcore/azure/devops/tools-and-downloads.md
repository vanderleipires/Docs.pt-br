---
title: DevOps com o ASP.NET Core e o Azure | Ferramentas e downloads
author: CamSoper
description: Um guia que fornece orientação de ponta a ponta sobre a criação de um pipeline de DevOps para um aplicativo ASP.NET Core hospedado no Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: a63e97d9ab9eb0ed2fbd30e8c2e033f0c048d33e
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312295"
---
# <a name="tools-and-downloads"></a>Ferramentas e downloads

O Azure tem várias interfaces para provisionamento e gerenciamento de recursos, como o [portal do Azure](https://portal.azure.com), [CLI do Azure](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [nuvem do Azure Shell](https://shell.azure.com/bash)e o Visual Studio. Este guia usa uma abordagem minimalista e usa o Azure Cloud Shell sempre que possível para reduzir as etapas necessárias. No entanto, o portal do Azure deve ser usado para algumas partes.

## <a name="prerequisites"></a>Pré-requisitos

As assinaturas a seguir são necessárias:

* Azure &mdash; se você não tiver uma conta [Obtenha uma avaliação gratuita](https://azure.microsoft.com/free/).
* Visual Studio Team Services (VSTS) &mdash; essa conta é criada no capítulo 4.
* GitHub &mdash; se você não tiver uma conta [Inscreva-se gratuitamente](https://github.com/join).

As ferramentas a seguir são necessárias:

* [Git](https://git-scm.com/downloads) &mdash; um entendimento fundamental do Git é recomendado para este guia. Examine os [documentação do Git](https://git-scm.com/doc), especificamente [git remoto](https://git-scm.com/docs/git-remote) e [git push](https://git-scm.com/docs/git-push).
* [SDK do .NET core](https://www.microsoft.com/net/download/) &mdash; versão 2.1.300 ou posterior é necessário para compilar e executar o aplicativo de exemplo. Se o Visual Studio é instalado com o **desenvolvimento de plataforma cruzada do .NET Core** carga de trabalho, o SDK do .NET Core já está instalada.

    Verifique se a instalação do SDK do .NET Core. Abra um shell de comando e execute o seguinte comando:

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Ferramentas recomendadas (somente Windows)

* [Visual Studio](https://www.visualstudio.com/)do robustas ferramentas do Azure fornecem uma interface gráfica para a maioria das funcionalidades descritas neste guia. Qualquer edição do Visual Studio funcionará, incluindo o Visual Studio Community Edition gratuito. Os tutoriais são gravados para demonstrar o desenvolvimento, implantação e operações de desenvolvimento com e sem o Visual Studio.

  Confirme se o Visual Studio tem o seguinte [cargas de trabalho](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) instalado:

  * Desenvolvimento do ASP.NET e para a Web
  * Desenvolvimento do Azure
  * Desenvolvimento de plataforma cruzada do .NET Core
