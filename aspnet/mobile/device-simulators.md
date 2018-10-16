---
uid: mobile/device-simulators
title: Simular dispositivos móveis populares para teste | Microsoft Docs
author: rick-anderson
description: Você pode baixar os emuladores de dispositivos móveis populares e navegadores seguindo estes links
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348371"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>Simular dispositivos móveis populares para teste

> Você pode baixar os emuladores de dispositivos móveis populares e navegadores seguindo estes links.

| Dispositivo ou navegador | Emulador / Simulator |
| --- | --- |
| Navegador de virtualização hospedada no BrowserStack ![Navegador de virtualização hospedada no BrowserStack](device-simulators/_static/image1.png) | [Virtualização do BrowserStack hospedada no navegador](http://browserstack.com) Teste seu ambiente local ou de produção em qualquer navegador em qualquer plataforma. Você pode criar um túnel entre seu computador e a rede de BrowserStack em sua própria máquina virtual hospedada. Certifique-se de obter o [extensão do Visual Studio BrowserStack](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack) para uma experiência ainda mais transparente. |
| iPhone / iPod / iPad | [Studio Electric do Mobile](http://www.electricplum.com/studio.aspx) iPhone e iPad simuladores para Windows, bem como uma responsiva ferramenta de design. |
| Android | [Android Studio](https://developer.android.com/studio/) ou [emulador do Visual Studio para Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Emulador do opera Mobile clássico](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Kit de ferramentas de desenvolvedor do Windows Mobile 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Observe que para conceder o acesso à rede de telefone, você também precisa o adaptador de rede do VPC incluídos no [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Para conectar-se o IE no telefone para seu servidor de desenvolvimento do Visual Studio, consulte [postagem do blog de Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |

> [!NOTE]
> Se você quiser exibir seu aplicativo em um dispositivo móvel real (que é a única opção para testar totalmente o iPhone ou iPad, já que não há nenhum verdadeiro emulador para Windows) será necessário hospedar seu aplicativo no IIS ou IIS Express. Você não pode facilmente usar servidor de web interno do Visual Studio para isso, porque ele não responderá às solicitações de outros computadores.