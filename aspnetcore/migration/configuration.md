---
title: Migrar a configuração para o ASP.NET Core
author: ardalis
description: Saiba como migrar a configuração de um projeto ASP.NET MVC para um projeto MVC do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: ead4f96aa0041cd919caa972d3bb05bd94a857b3
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851716"
---
# <a name="migrate-configuration-to-aspnet-core"></a>Migrar a configuração para o ASP.NET Core

Por [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

O artigo anterior, começamos [migrar um projeto ASP.NET MVC para MVC do ASP.NET Core](xref:migration/mvc). Neste artigo, vamos migrar a configuração.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Configuração da instalação

ASP.NET Core não usa mais o *global. asax* e *Web. config* arquivos utilizadas de versões anteriores do ASP.NET. Em versões anteriores do ASP.NET, a lógica de inicialização do aplicativo foi colocada em uma `Application_StartUp` método *global. asax*. Posteriormente, no ASP.NET MVC, um *Startup.cs* arquivo foi incluído na raiz do projeto; e ele foi chamado quando o aplicativo foi iniciado. ASP.NET Core adotou essa abordagem completamente colocando toda lógica de inicialização no *Startup.cs* arquivo.

O *Web. config* arquivo também foi substituído no núcleo do ASP.NET. Configuração propriamente dita agora pode ser configurada como parte do procedimento de inicialização de aplicativo descrito em *Startup.cs*. Configuração ainda pode utilizar para arquivos XML, mas normalmente projetos do ASP.NET Core colocará os valores de configuração em um arquivo no formato JSON, como *appSettings. JSON*. Sistema de configuração do ASP.NET Core facilmente pode acessar variáveis de ambiente, que podem fornecer um [local mais seguro e robusto](xref:security/app-secrets) para valores específicos do ambiente. Isso é especialmente verdadeiro para segredos, como cadeias de caracteres de conexão e chaves de API que não devem ser verificadas no controle de origem. Consulte [configuração](xref:fundamentals/configuration/index) para saber mais sobre a configuração no núcleo do ASP.NET.

Neste artigo, estamos começando com o projeto do ASP.NET Core parcialmente migrado de [artigo anterior](xref:migration/mvc). Para configurar a configuração, adicione o seguinte construtor e propriedade para o *Startup.cs* arquivo localizado na raiz do projeto:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Observe que neste ponto, o *Startup.cs* arquivo não será compilado, pois precisamos adicionar o seguinte `using` instrução:

```csharp
using Microsoft.Extensions.Configuration;
```

Adicionar uma *appSettings. JSON* arquivo para a raiz do projeto usando o modelo de item apropriado:

![Adicionar AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migrar configurações de Web. config

Nosso projeto ASP.NET MVC incluído a cadeia de caracteres de conexão de banco de dados necessários no *Web. config*, além de `<connectionStrings>` elemento. Em nosso projeto do ASP.NET Core, vamos armazenar essas informações no *appSettings. JSON* arquivo. Abra *appSettings. JSON*e observe que ele já inclui o seguinte:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

Na linha realçada descrita acima, altere o nome do banco de dados **_CHANGE_ME** para o nome do banco de dados.

## <a name="summary"></a>Resumo

ASP.NET Core coloca toda a lógica de inicialização para o aplicativo em um único arquivo, no qual os serviços necessários e dependências podem ser definidas e configuradas. Ele substitui o *Web. config* arquivo com um recurso de configuração flexíveis que pode aproveitar uma variedade de formatos de arquivo, como JSON, bem como variáveis de ambiente.
