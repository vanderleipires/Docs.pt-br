---
title: Módulo do ASP.NET Core
author: tdykstra
description: Saiba como o módulo do ASP.NET Core permite que o servidor Web Kestrel use o IIS ou o IIS Express como um servidor proxy reverso.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4e842544f861c3704ba7798fa693b36435d54731
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="aspnet-core-module"></a>Módulo do ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) e [Chris Ross](https://github.com/Tratcher) 

O Módulo do ASP.NET Core permite que os aplicativos ASP.NET Core sejam executados por trás do IIS em uma configuração de proxy reverso. O IIS fornece recursos avançados de segurança e capacidade de gerenciamento de aplicativo Web.

Versões do Windows compatíveis:

* Windows 7 ou posterior
* Windows Server 2008 R2 ou posterior

O Módulo do ASP.NET Core só funciona com o Kestrel. O módulo é incompatível com o [HTTP.sys](xref:fundamentals/servers/httpsys) (chamado anteriormente de [WebListener](xref:fundamentals/servers/weblistener)).

## <a name="aspnet-core-module-description"></a>Descrição de Módulo do ASP.NET Core

O Módulo do ASP.NET Core é um módulo nativo do IIS que se conecta ao pipeline do IIS para redirecionar solicitações da Web para aplicativos ASP.NET Core de back-end. Muitos módulos nativos, como a Autenticação do Windows, permanecem ativos. Para saber mais sobre os módulos ativos do IIS com o módulo, confira [Módulos do IIS](xref:host-and-deploy/iis/modules).

Como os aplicativos ASP.NET Core são executados em um processo separado do processo de trabalho do IIS, o módulo também realiza o gerenciamento de processos. O módulo inicia o processo para o aplicativo ASP.NET Core quando a primeira solicitação chega e reinicia o aplicativo quando ele falha. Isso é basicamente o mesmo comportamento que o dos aplicativos ASP.NET 4.x que são executados dentro do processo do IIS e são gerenciados pelo [WAS (Serviço de Ativação de Processos do Windows)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

O diagrama a seguir ilustra a relação entre o IIS, o Módulo do ASP.NET Core e os aplicativos ASP.NET Core:

![Módulo do ASP.NET Core](aspnet-core-module/_static/ancm.png)

As solicitações chegam da Web para o driver do HTTP.sys no modo kernel. O driver roteia as solicitações ao IIS na porta configurada do site, normalmente, a 80 (HTTP) ou a 443 (HTTPS). O módulo encaminha as solicitações ao Kestrel em uma porta aleatória para o aplicativo, que não seja a porta 80/443.

O módulo especifica a porta por meio de uma variável de ambiente na inicialização e o middleware de integração do IIS configura o servidor para escutar em `http://localhost:{port}`. Outras verificações são executadas e as solicitações que não se originam do módulo são rejeitadas. O módulo não é compatível com encaminhamento de HTTPS, portanto, as solicitações são encaminhadas por HTTP, mesmo se recebidas pelo IIS por HTTPS.

Depois que o Kestrel coleta uma solicitação do módulo, a solicitação é enviada por push ao pipeline do middleware do ASP.NET Core. O pipeline do middleware manipula a solicitação e a passa como uma instância de `HttpContext` para a lógica do aplicativo. A resposta do aplicativo é retornada ao IIS, que a retorna por push para o cliente HTTP que iniciou a solicitação.

O Módulo do ASP.NET Core tem algumas outras funções. O módulo pode:

* Definir variáveis de ambiente para o processo de trabalho.
* Registrar a saída StdOut no armazenamento de arquivo para a solução de problemas de inicialização.
* Encaminhar tokens de autenticação do Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Como instalar e usar o Módulo do ASP.NET Core

Para obter instruções detalhadas de como instalar e usar o Módulo do ASP.NET Core, confira [Hospedar no Windows com o IIS](xref:host-and-deploy/iis/index). Para obter informações de como configurar o módulo, confira a [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

## <a name="additional-resources"></a>Recursos adicionais

* [Hospedar no Windows com o IIS](xref:host-and-deploy/iis/index)
* [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Repositório do GitHub do Módulo do ASP.NET Core (código-fonte)](https://github.com/aspnet/AspNetCoreModule)
