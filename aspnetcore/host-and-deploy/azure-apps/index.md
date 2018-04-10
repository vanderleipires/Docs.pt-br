---
title: Hospedar o ASP.NET Core no Serviço de Aplicativo do Azure
author: guardrex
description: Descubra como hospedar aplicativos ASP.NET Core no Serviço de Aplicativo do Azure com links para recursos úteis.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
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

## <a name="application-configuration"></a>Configuração do aplicativo

Com o ASP.NET Core 2.0 e posterior, três pacotes no metapacote [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) fornecem recursos de registro em log automático para aplicativos implantados no Serviço de Aplicativo do Azure:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) para fornecer integração leve do ASP.NET Core com o Serviço de Aplicativo do Azure. Os recursos de registro em log adicionais são fornecidos pelo pacote `Microsoft.AspNetCore.AzureAppServicesIntegration`.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executa [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) para adicionar provedores de log de diagnósticos do Serviço de Aplicativo do Azure no pacote `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fornece implementações de agente para dar suporte a recursos de streaming de log e logs de diagnóstico do Serviço de Aplicativo do Azure.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de balanceador de carga

O Middleware de integração do IIS, que configura Middleware de cabeçalhos encaminhados, e o módulo do ASP.NET Core são configurados para encaminhar o esquema (HTTP/HTTPS) e o endereço IP remoto de onde a solicitação foi originada. Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy adicionais e balanceadores de carga. Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Monitoramento e registro em log

Para monitoramento, registro em log e informações de solução de problemas, veja os seguintes artigos:

[Como: monitorar aplicativos no Serviço de Aplicativo do Azure](/azure/app-service/web-sites-monitor)  
Saiba como examinar as cotas e métricas para aplicativos e planos do Serviço de Aplicativo.

[Habilitar log de diagnósticos para aplicativos Web no Serviço de Aplicativo do Azure](/azure/app-service/web-sites-enable-diagnostic-log)  
Descubra como habilitar e acessar o log de diagnósticos para os códigos de status HTTP, solicitações com falha e atividade do servidor Web.

[Introdução ao tratamento de erro no ASP.NET Core](xref:fundamentals/error-handling)  
Entenda abordagens comuns para o tratamento de erros em aplicativos ASP.NET Core.

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

* [Instalar a extensão de site de visualização](#site-x)
* [Implantar o aplicativo autocontido](#self)
* [Usar o Docker com aplicativos Web para contêineres](#docker)

Se você tiver problemas ao usar a extensão de site de visualização, abra um problema no [GitHub](https://github.com/aspnet/azureintegration/issues/new).

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a>Instalar a extensão de site de visualização

* No portal do Azure, navegue até a folha Serviço de Aplicativo.
* Digite "ex" na caixa de pesquisa.
* Selecione **Extensões**.
* Selecione "Adicionar".

![Folha do Azure App com etapas anteriores](index/_static/x1.png)

* Selecione **Extensões de tempo de execução do ASP.NET Core**.
* Selecione **OK** > **OK**.

Quando as operações de adição forem concluídas, a visualização mais recente do .NET Core 2.1 será instalada. Você pode verificar a instalação executando `dotnet --info` no console. Na folha de Serviço de Aplicativo:

* Digite "con" na caixa de pesquisa.
* Selecione **Console**.
* Digite `dotnet --info` no console.

![Folha do Azure App com etapas anteriores](index/_static/cons.png)

A imagem anterior era atual no momento em que este texto foi escrito. Você pode ver uma versão diferente.

O `dotnet --info` exibe o caminho para a extensão de site em que a visualização foi instalada. Ele mostra que o aplicativo está em execução na extensão de site em vez do local padrão *ProgramFiles*. Se você vir *ProgramFiles*, reinicie o site e execute `dotnet --info`.

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a>Usar a extensão de site de visualização com um modelo do ARM

Se você estiver usando um modelo do ARM para criar e implantar aplicativos, poderá usar o tipo de recurso `siteextensions` para adicionar a extensão de site a um aplicativo Web. Por exemplo:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a>Implantar o aplicativo autocontido

Você pode implantar um [aplicativo autocontido](/dotnet/core/deploying/#self-contained-deployments-scd) que carrega o tempo de execução de visualização com ele quando está sendo implantado. Ao implantar um aplicativo autocontido:

* Não é necessário preparar o seu site.
* Exige que você publique seu aplicativo de forma diferente que ao implantar um aplicativo depois que o SDK está instalado no servidor.

Aplicativos autocontidos são uma opção para todos os aplicativos do .NET Core.

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a>Usar o Docker com aplicativos Web para contêineres

O [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contém as imagens de visualização do Docker 2.1 mais recentes. Você pode usá-las como sua imagem de base e implantar em Aplicativos Web para Contêineres como faria normalmente.

## <a name="additional-resources"></a>Recursos adicionais

* [Visão geral de aplicativos Web (vídeo de visão geral com 5 minutos)](/azure/app-service/app-service-web-overview)
* [Serviço de Aplicativo do Azure: o melhor lugar para hospedar seus aplicativos .NET (vídeo de visão geral com 55 minutos)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: experiência de diagnóstico e solução de problemas do Serviço de Aplicativo do Azure (vídeo com 12 minutos)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Visão geral de diagnóstico do Serviço de Aplicativo do Azure](/azure/app-service/app-service-diagnostics)

O Serviço de Aplicativo do Azure no Windows Server usa o [IIS (Serviços de Informações da Internet)](https://www.iis.net/). Os tópicos a seguir estão relacionados com a tecnologia subjacente do IIS:

* [Hospedar o ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index)
* [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Módulos do IIS com o ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Biblioteca Microsoft TechNet: Windows Server](/windows-server/windows-server-versions)
