---
title: Hospedar o ASP.NET Core no Serviço de Aplicativo do Azure
author: guardrex
description: Descubra como hospedar aplicativos ASP.NET Core no Serviço de Aplicativo do Azure com links para recursos úteis.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: bc2a686c5ddc44fded135c9eed5caf676218773a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312064"
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Hospedar o ASP.NET Core no Serviço de Aplicativo do Azure

[Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/) é um [serviço de plataforma de computação em nuvem da Microsoft](https://azure.microsoft.com/) para hospedar aplicativos Web, incluindo o ASP.NET Core.

## <a name="useful-resources"></a>Recursos úteis

A [Documentação de Aplicativos Web](/azure/app-service/) do Azure é a página inicial para documentação de Azure Apps, tutoriais, exemplos, guias de instruções e outros recursos. Dois tutoriais importantes que pertencem à hospedagem de aplicativos ASP.NET Core são:

[Início rápido: Criar um aplicativo Web ASP.NET Core no Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Use o Visual Studio para criar e implantar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure no Windows.

[Início rápido: Criar um aplicativo Web .NET Core no Serviço de Aplicativo no Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Use a linha de comando do Visual Studio para criar e implantar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure no Linux.

Os artigos a seguir estão disponíveis na documentação do ASP.NET Core:

[Publicar no Azure com o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Aprenda como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio.

[Publicar no Azure com as ferramentas CLI](xref:tutorials/publish-to-azure-webapp-using-cli)  
Saiba como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o cliente de linha de comando do Git.

[Implantação contínua no Azure com o Visual Studio e o Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Saiba como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo no Serviço de Aplicativo do Azure, usando o Git para implantação contínua.

[Implantação contínua no Azure com o VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Configurar um build de CI para um aplicativo ASP.NET Core e, em seguida, criar uma versão de implantação contínua para o Serviço de Aplicativo do Azure.

[Área restrita de aplicativo Web do Azure](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Descubra as limitações de tempo de execução do Serviço de Aplicativo do Azure impostas pela plataforma de Aplicativos do Azure.

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>Configuração do aplicativo

No ASP.NET Core 2.0 ou posterior, os seguintes pacotes NuGet fornecem recursos de registro em log automático para aplicativos implantados para o Serviço de Aplicativo do Azure:

* O [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) para fornecer integração leve do ASP.NET Core com o Serviço de Aplicativo do Azure. Os recursos de registro em log adicionais são fornecidos pelo pacote `Microsoft.AspNetCore.AzureAppServicesIntegration`.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executa [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) para adicionar provedores de log de diagnósticos do Serviço de Aplicativo do Azure no pacote `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fornece implementações de agente para dar suporte a recursos de streaming de log e logs de diagnóstico do Serviço de Aplicativo do Azure.

Se você estiver direcionando para o .NET Core e estiver referenciando o [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage), os pacotes já estarão incluídos. Ambos os pacotes estão ausentes no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) mais novo. Se você estiver direcionando ao .NET Framework ou referenciando ao metapacote `Microsoft.AspNetCore.App`, referencie os pacotes individuais de registro em log.

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a>Substituir a configuração do aplicativo no Portal do Azure

A área **Configurações de aplicativo** da folha **Configurações de aplicativo** permite definir variáveis de ambiente para o aplicativo. As variáveis de ambiente podem ser consumidas pelo [Provedor de configuração de variáveis de ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Quando o aplicativo usa o [host da Web](xref:fundamentals/host/web-host) e cria o host usando [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), as variáveis de ambiente que configuram o host usam o prefixo `ASPNETCORE_`. Para saber mais, confira <xref:fundamentals/host/web-host> e o [Provedor de configuração de variáveis de ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Quando o aplicativo usa o [host genérico](xref:fundamentals/host/generic-host), as variáveis de ambiente não são carregadas na configuração do aplicativo por padrão, e o provedor de configuração deve ser adicionado pelo desenvolvedor. O desenvolvedor determina o prefixo da variável de ambiente quando o provedor de configuração é adicionado. Para saber mais, confira <xref:fundamentals/host/generic-host> e o [Provedor de configuração de variáveis de ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de balanceador de carga

O Middleware de integração do IIS, que configura Middleware de cabeçalhos encaminhados, e o módulo do ASP.NET Core são configurados para encaminhar o esquema (HTTP/HTTPS) e o endereço IP remoto de onde a solicitação foi originada. Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy adicionais e balanceadores de carga. Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Monitoramento e registro em log

Para monitoramento, registro em log e informações de solução de problemas, veja os seguintes artigos:

[Como: monitorar aplicativos no Serviço de Aplicativo do Azure](/azure/app-service/web-sites-monitor)  
Saiba como examinar as cotas e métricas para aplicativos e planos do Serviço de Aplicativo.

[Habilitar log de diagnósticos para aplicativos Web no Serviço de Aplicativo do Azure](/azure/app-service/web-sites-enable-diagnostic-log)  
Descubra como habilitar e acessar o log de diagnósticos para os códigos de status HTTP, solicitações com falha e atividade do servidor Web.

[Introdução ao tratamento de erro no ASP.NET Core](xref:fundamentals/error-handling)  
Entenda as abordagens comuns para o tratamento de erros em aplicativos ASP.NET Core.

[Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure](xref:host-and-deploy/azure-apps/troubleshoot)  
Saiba como diagnosticar problemas com implantações do Serviço de Aplicativo do Azure com aplicativos ASP.NET Core.

[Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
Consulte os erros comuns de configuração de implantação para aplicativos hospedados pelo Serviço de Aplicativo do Azure/IIS com orientação para solução de problemas.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Anel de chave de proteção de dados e slots de implantação

[Chaves de proteção de dados](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) são mantidas na pasta *%HOME%\ASP.NET\DataProtection-Keys*. Essa pasta é apoiada pelo repositório de rede e é sincronizada em todos os computadores que hospedam o aplicativo. As chaves não são protegidas em repouso. Essa pasta fornece o anel de chave para todas as instâncias de um aplicativo em um único slot de implantação. Slots de implantação separados, como de preparo e produção, não compartilham um anel de chave.

Quando ocorre a troca entre os slots de implantação, nenhum sistema que usa a proteção de dados consegue descriptografar dados armazenados usando o anel de chave dentro do slot anterior. O middleware de cookie do ASP.NET usa a proteção de dados para proteger seus cookies. Com isso, os usuários são desconectados de um aplicativo que usa o Middleware de Cookie padrão do ASP.NET. Para uma solução de anel de chave independente de slot, use um provedor de anel de chave externo, como:

* Armazenamento do Blobs do Azure
* Azure Key Vault
* Repositório SQL
* Cache redis

Para obter mais informações, veja [Principais provedores de armazenamento](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Implantar a versão de visualização do ASP.NET Core para o Serviço de Aplicativo do Azure

Aplicativos de visualização do ASP.NET Core podem ser implantados para o Serviço de Aplicativo do Azure com as seguintes abordagens:

* [Instalar a extensão de site da versão prévia](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) -->
* [Usar o Docker com aplicativos Web para contêineres](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a>Instalar a extensão de site de visualização

Se houver problemas ao usar a extensão de site de visualização, abra um problema no [GitHub](https://github.com/aspnet/azureintegration/issues/new).

1. No portal do Azure, navegue até a folha Serviço de Aplicativo.
1. Selecione o aplicativo Web.
1. Insira "ex" na caixa de pesquisa ou role para baixo na lista de seções de gerenciamento para **FERRAMENTAS DE DESENVOLVIMENTO**.
1. Selecione **FERRAMENTAS DE DESENVOLVIMENTO** > **Extensões**.
1. Selecione **Adicionar**.
1. Selecione a extensão de **Tempo de execução do ASP.NET Core &lt;x.y&gt; (x86)** na lista, em que `<x.y>` é a versão prévia do ASP.NET Core (por exemplo, **Tempo de execução do ASP.NET Core 2.2 (x86)**). O tempo de execução x86 é apropriado para [implantações dependentes de estrutura](/dotnet/core/deploying/#framework-dependent-deployments-fdd) que dependem de hospedagem de fora do processo pelo Módulo do ASP.NET Core.
1. Selecione **OK** para aceitar os termos legais.
1. Selecione **OK** para instalar a extensão.

Quando a operação for concluída, a versão prévia mais recente do .NET Core será instalada. Verifique a instalação:

1. Selecione **Ferramentas Avançadas** sob **FERRAMENTAS DE DESENVOLVIMENTO**.
1. Selecione **Ir** na folha **Ferramentas Avançadas**.
1. Selecione o item de menu **Console de depuração** > **PowerShell**.
1. No prompt do PowerShell, execute o seguinte comando. Substitua a versão de tempo de execução do ASP.NET Core por `<x.y>` no comando:

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   Se o tempo de execução da versão prévia instalada for ASP.NET Core 2.2, o comando será:
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   O comando retornará `True` quando o tempo de execução da versão prévia x64 estiver instalado.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> A arquitetura da plataforma (x86/x64) de um aplicativo de Serviços de Aplicativos é definida na folha **Configurações do Aplicativo** em **Configurações Gerais** para aplicativos que são hospedados em uma computação de série A ou em uma melhor camada de hospedagem. Se o aplicativo for executado no modo em processo e a arquitetura da plataforma estiver configurada para 64 bits (x64), o Módulo do ASP.NET Core usará o tempo de execução da versão prévia de 64 bits, se estiver presente. Instalar a extensão **Tempo de execução do ASP.NET Core &lt;x.y&gt; (x64)** (por exemplo, **Tempo de execução do ASP.NET Core 2.2 (x64)**).
>
> Depois de instalar o tempo de execução da versão prévia x64, execute o seguinte comando na janela de comando do Kudu PowerShell para verificar a instalação. Substitua a versão de tempo de execução do ASP.NET Core por `<x.y>` no comando:
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> Se o tempo de execução da versão prévia instalada for ASP.NET Core 2.2, o comando será:
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> O comando retornará `True` quando o tempo de execução da versão prévia x64 estiver instalado.

::: moniker-end

> [!NOTE]
> As **Extensões do ASP.NET Core** habilitam uma funcionalidade adicional para o ASP.NET Core nos Serviços de Aplicativo do Azure, como a habilitação do registro em log do Azure. A extensão é instalada automaticamente durante a implantação do Visual Studio. Se a extensão não estiver instalada, instale-a para o aplicativo.

**Usar a extensão de site de visualização com um modelo do ARM**

Se um modelo do ARM for usado para criar e implantar aplicativos, o tipo de recurso `siteextensions` poderá ser usado para adicionar a extensão de site a um aplicativo Web. Por exemplo:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a>Usar o Docker com aplicativos Web para contêineres

O [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contém as imagens de versão prévia do Docker mais recentes. As imagens podem ser usadas como uma imagem de base. Use a imagem e implante aplicativos Web para contêineres normalmente.

## <a name="additional-resources"></a>Recursos adicionais

* [Visão geral de aplicativos Web (vídeo de visão geral com 5 minutos)](/azure/app-service/app-service-web-overview)
* [Serviço de Aplicativo do Azure: o melhor lugar para hospedar seus aplicativos .NET (vídeo de visão geral com 55 minutos)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: experiência de diagnóstico e solução de problemas do Serviço de Aplicativo do Azure (vídeo com 12 minutos)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Visão geral de diagnóstico do Serviço de Aplicativo do Azure](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

O Serviço de Aplicativo do Azure no Windows Server usa o [IIS (Serviços de Informações da Internet)](https://www.iis.net/). Os tópicos a seguir estão relacionados com a tecnologia subjacente do IIS:

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Biblioteca Microsoft TechNet: Windows Server](/windows-server/windows-server-versions)
