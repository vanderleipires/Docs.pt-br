---
title: "Solucionar problemas de núcleo do ASP.NET no IIS"
author: guardrex
description: "Saiba como diagnosticar problemas com as implantações do IIS de aplicativos do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2e42d5904e2be8c031c5c6d4bbc104a251b699f2
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Solucionar problemas de núcleo do ASP.NET no IIS

Por [Luke Latham](https://github.com/guardrex)

Para diagnosticar problemas com as implantações do IIS:

* Estude a saída do navegador.
* Examine o log de **Aplicativo** do sistema por meio do **Visualizador de eventos**.
* Habilite o registro em log do `stdout`. O log do **Módulo do ASP.NET Core** é encontrado no caminho fornecido no atributo *stdoutLogFile* do elemento `<aspNetCore>` em *web.config*. As pastas no caminho fornecido no valor do atributo devem existir na implantação. Definir *stdoutLogEnabled* para `true`. Aplicativos que usam o o `Microsoft.NET.Sdk.Web` SDK para criar o *Web. config* padrão de arquivos o *stdoutLogEnabled* definindo como `false`, manualmente fornecem o *deWeb.config* de arquivos ou modificar o arquivo para habilitar `stdout` log.

Use as informações dessas três fontes com o [tópico de referência de erros comuns](xref:host-and-deploy/azure-iis-errors-reference) para determinar o problema. Siga os avisos de solução de problemas fornecidos para resolver o problema.

Vários dos erros comuns não são exibidos no navegador, no Log do aplicativo e no Log do módulo do ASP.NET Core até que o módulo *startupTimeLimit* (padrão: 120 segundos) e *startupRetryCount* (padrão: 2) tenham decorrido. Portanto, aguarde seis minutos completos antes de deduzir que o módulo falhou ao iniciar um processo para o aplicativo.

Uma maneira rápida de determinar se o aplicativo está funcionando corretamente é executar o aplicativo diretamente no Kestrel. Se o aplicativo foi publicado como um [implantação dependentes de framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), execute `dotnet <assembly_name>.dll` na pasta de implantação, que é o caminho físico do IIS para o aplicativo. Se o aplicativo foi publicado como um [implantação autossuficiente](/dotnet/core/deploying/#self-contained-deployments-scd), execute o aplicativo do executável diretamente a partir de um prompt de comando, `<assembly_name>.exe`, na pasta de implantação. Se Kestrel está escutando na porta padrão 5000, o aplicativo deve estar disponível em `http://localhost:5000/`. Se o aplicativo normalmente responde no endereço de ponto de extremidade Kestrel, o problema é mais provável relacionados à configuração de proxy reverso e menos provável dentro do aplicativo.

É uma maneira de determinar se o proxy reverso está funcionando corretamente executar uma solicitação de arquivo estático simples para uma folha de estilos, um script ou uma imagem de arquivos estáticos do aplicativo *wwwroot* usando [Middleware de arquivo estático](xref:fundamentals/static-files). Se o aplicativo pode servir arquivos estáticos, mas exibições do MVC e outros pontos de extremidade estão falhando, o problema é menos provável relacionados à configuração de proxy reverso e provavelmente dentro do aplicativo (por exemplo, o roteamento MVC ou 500 Erro interno do servidor).

Quando Kestrel é iniciado normalmente por trás do IIS, mas o aplicativo não será executado no sistema depois de executado com êxito, uma variável de ambiente pode ser adicionada temporariamente para *Web. config* para definir o `ASPNETCORE_ENVIRONMENT` para `Development`. Como o ambiente não é substituído na inicialização do aplicativo, definindo a variável de ambiente permite que o [página de exceção de desenvolvedor](xref:fundamentals/error-handling) aparecem quando o aplicativo é executado. Definir a variável de ambiente para `ASPNETCORE_ENVIRONMENT` dessa maneira só é recomendado para servidores de preparo/teste que não são expostos à Internet. Certifique-se de remover a variável de ambiente a *Web. config* quando terminar de arquivo. Para obter informações sobre como definir variáveis de ambiente por meio de *Web. config*, consulte [environmentVariables elemento filho aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

Na maioria dos casos, a habilitação do log do aplicativo ajudará na solução de problemas com o aplicativo ou o proxy reverso. Consulte [Log](xref:fundamentals/logging/index) para obter mais informações.

A última dica de solução de problemas se refere aos aplicativos que não são executados depois de atualizar o .NET Core SDK nas versões de pacote ou de máquina de desenvolvimento dentro do aplicativo. Em alguns casos, pacotes incoerentes podem interromper um aplicativo ao executar atualizações principais. A maioria desses problemas pode ser corrigida por:

* Excluindo o `bin` e `obj` pastas no projeto.
* Pacote de compensação armazena em cache em `%UserProfile%\.nuget\packages\` e `%LocalAppData%\Nuget\v3-cache`.
* A restauração e recompilar o projeto.
* Confirmando que a implantação anterior no servidor foi excluída completamente antes de reimplantar o aplicativo.

> [!TIP]
> Uma maneira conveniente para Limpar caches do pacote é executar `dotnet nuget locals all --clear` em um prompt de comando.
> 
> Limpar os caches de pacote também pode ser feito usando o [nuget.exe](https://www.nuget.org/downloads) ferramenta e executar o comando `nuget locals all -clear`. *NuGet.exe* não é uma instalação de pacote com o Windows 10 e devem ser obtidos separadamente do site do NuGet.
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>Recursos adicionais

* [Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
