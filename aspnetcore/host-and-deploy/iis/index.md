---
title: Hospedar o ASP.NET Core no Windows com o IIS
author: guardrex
description: Saiba como hospedar aplicativos ASP.NET Core no Windows Server IIS (Serviços de Informações da Internet).
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 3a9479dc1bb09218ebb4a5a76078ea514041d751
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Hospedar o ASP.NET Core no Windows com o IIS

Por [Luke Latham](https://github.com/guardrex) e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Sistemas operacionais com suporte

Há suporte para os seguintes sistemas operacionais:

* Windows 7 ou posterior
* Windows Server 2008 R2 ou posterior

O [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente chamado de [WebListener](xref:fundamentals/servers/weblistener)) não funcionará em uma configuração de proxy reverso com IIS. Use o [servidor Kestrel](xref:fundamentals/servers/kestrel).

## <a name="application-configuration"></a>Configuração do aplicativo

### <a name="enable-the-iisintegration-components"></a>Habilitar os componentes de IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Um típico *Program.cs* chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host. `CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) como o servidor Web e habilita a integração IIS configurando o caminho base e a porta para o [módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

O Módulo do ASP.NET Core gera uma porta dinâmica a ser atribuída ao processo de back-end. O método `UseIISIntegration` seleciona a porta dinâmica e configura o Kestrel para escutar em `http://localhost:{dynamicPort}/`. Isso substitui outras configurações de URL, como chamadas a `UseUrls` ou à [API de Escuta do Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration). Portanto, as chamadas a `UseUrls` ou à API `Listen` do Kestrel não são necessárias ao usar o módulo. Se `UseUrls` ou `Listen` for chamado, o Kestrel escutará na porta especificada durante a execução do aplicativo sem o IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Inclua uma dependência no pacote [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nas dependências do aplicativo. Use o middleware Integração do IIS adicionando o método de extensão [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) ao [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) e [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) são necessários. A chamada do código a `UseIISIntegration` não afeta a portabilidade do código. Se o aplicativo não é executado por trás do IIS (por exemplo, o aplicativo é executado diretamente em Kestrel), `UseIISIntegration` não opera.

O Módulo do ASP.NET Core gera uma porta dinâmica a ser atribuída ao processo de back-end. O método `UseIISIntegration` seleciona a porta dinâmica e configura o Kestrel para escutar em `http://locahost:{dynamicPort}/`. Isso substitui outras configurações de URL, como chamadas a `UseUrls`. Portanto, uma chamada a `UseUrls` não é necessária ao usar o módulo. Se `UseUrls` for chamado, o Kestrel escutará na porta especificada durante a execução do aplicativo sem o IIS.

Se `UseUrls` for chamado em um aplicativo do ASP.NET Core 1.0, chame-o **antes** de chamar `UseIISIntegration` para que a porta configurada pelo módulo não seja substituída. Essa ordem de chamada não é necessária com o ASP.NET Core 1.1, porque a configuração do módulo substitui `UseUrls`.

---

Para saber mais sobre hospedagem, confira [Host no ASP.NET Core](xref:fundamentals/host/index).

### <a name="iis-options"></a>Opções do IIS

Para configurar opções de IIS, inclua uma configuração de serviço para [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) em [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). No exemplo a seguir, o encaminhamento de certificados do cliente para o aplicativo para preencher `HttpContext.Connection.ClientCertificate` está desabilitado:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Opção                         | Padrão | Configuração |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Se `true`, o middleware de integração do IIS define o `HttpContext.User` autenticado pela [Autenticação do Windows](xref:security/authentication/windowsauth). Se `false`, o middleware fornecerá apenas uma identidade para `HttpContext.User` e responderá a desafios quando explicitamente solicitado pelo `AuthenticationScheme`. A autenticação do Windows deve estar habilitada no IIS para que o `AutomaticAuthentication` funcione. Saiba mais no tópico [Autenticação do Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Configura o nome de exibição mostrado aos usuários em páginas de logon. |
| `ForwardClientCertificate`     | `true`  | Se `true` e o cabeçalho da solicitação `MS-ASPNETCORE-CLIENTCERT` estiverem presentes, o `HttpContext.Connection.ClientCertificate` será populado. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de balanceador de carga

O Middleware de integração do IIS, que configura Middleware de cabeçalhos encaminhados, e o módulo do ASP.NET Core são configurados para encaminhar o esquema (HTTP/HTTPS) e o endereço IP remoto de onde a solicitação foi originada. Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy adicionais e balanceadores de carga. Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>Arquivo web.config

O arquivo *web.config* configura o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). A criação, transformação e publicação do *web.config* é gerenciada pelo SDK Web do .NET Core (`Microsoft.NET.Sdk.Web`). O SDK é definido na parte superior do arquivo de projeto:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Se um arquivo *web.config* não estiver presente no projeto, ele será criado com o *processPath* e os *argumentos* corretos para configurar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e será transferido para o [resultado publicado](xref:host-and-deploy/directory-structure).

Se um arquivo *web.config* estiver presente no projeto, ele será transformado com o *processPath* e os *argumentos* corretos para configurar o Módulo do ASP.NET Core e será movido para o resultado publicado. A transformação não altera as definições de configuração do IIS no arquivo.

O arquivo *web.config* pode fornecer configurações adicionais do IIS que controlam módulos ativos do IIS. Para saber mais sobre os módulos do IIS que podem processar solicitações com aplicativos do ASP.NET Core, veja o tópico [Módulos do IIS](xref:host-and-deploy/iis/modules).

Para impedir que o SDK Web transforme o arquivo *web.config*, use a propriedade **\<IsTransformWebConfigDisabled>** no arquivo do projeto:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Ao impedir que o SDK Web transforme o arquivo, o *processPath* e os *argumentos* devem ser definidos manualmente pelo desenvolvedor. Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="webconfig-file-location"></a>Local do arquivo web.config

Os aplicativos ASP.NET Core são hospedados em um proxy reverso entre o IIS e o servidor Kestrel. Para criar um proxy reverso, o arquivo *web.config* deve estar presente no caminho raiz do conteúdo (geralmente, o aplicativo base do caminho) do aplicativo implantado. Esse é o mesmo local que o caminho físico do site fornecido ao IIS. O arquivo *web.config* é necessário na raiz do aplicativo para habilitar a publicação de vários aplicativos usando a Implantação da Web.

Existem arquivos confidenciais no caminho físico do aplicativo, como *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (comentários da Documentação XML) e *\<assembly>.deps.json*. Quando o arquivo *web.config* estiver presente e o site for iniciado normalmente, o IIS não servirá esses arquivos confidenciais se eles forem solicitados. Se o arquivo *web.config* estiver ausente, nomeado incorretamente ou se não for possível configurar o site para inicialização normal, o IIS poderá servir arquivos confidenciais publicamente.

**O arquivo *web.config* deve estar presente na implantação em todos os momentos, nomeado corretamente e ser capaz de configurar o site para inicialização normal. Nunca remova o arquivo *web.config* de uma implantação de produção.**

## <a name="iis-configuration"></a>Configuração do IIS

**Sistemas operacionais do Windows Server**

Habilite a função **Servidor Web (IIS)** e estabeleça serviços de função.

1. Use o assistente **Adicionar Funções e Recursos** por meio do menu **Gerenciar** ou do link no **Gerenciador do Servidor**. Na etapa **Funções de Servidor**, marque a caixa de **Servidor Web (IIS)**.

   ![A função de Servidor Web IIS é selecionada na etapa Selecionar funções de servidor.](index/_static/server-roles-ws2016.png)

1. Após a etapa **Recursos**, a etapa **Serviços de função** é carregada para o servidor Web (IIS). Selecione os serviços de função do IIS desejados ou aceite os serviços de função padrão fornecidos.

   ![Os serviços de função padrão são selecionados na etapa Selecionar serviços de função.](index/_static/role-services-ws2016.png)

   **Autenticação do Windows (opcional)**  
   Para habilitar a Autenticação do Windows, expanda os nós a seguir: **Servidor Web** > **Segurança**. Selecione o recurso **Autenticação do Windows**. Saiba mais em [Autenticação do Windows \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) e [Configurar autenticação do Windows](xref:security/authentication/windowsauth).

   **WebSockets (opcional)**  
   O WebSockets é compatível com o ASP.NET Core 1.1 ou posterior. Para habilitar o WebSockets, expanda os nós a seguir: **Servidor Web** > **Desenvolvimento de Aplicativos**. Selecione o recurso **Protocolo WebSocket**. Para obter mais informações, consulte [WebSockets](xref:fundamentals/websockets).

1. Continue para a etapa **Confirmação** para instalar os serviços e a função de servidor Web. Um comando server/IIS restart não será necessário após a instalação da função **Servidor Web (IIS)**.

**Sistemas operacionais Windows de área de trabalho**

Habilite o **Console de Gerenciamento do IIS** e os **Serviços na World Wide Web**.

1. Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).

1. Abra o nó **Serviços de Informações da Internet**. Abra o nó **Ferramentas de Gerenciamento da Web**.

1. Marque a caixa de **Console de Gerenciamento do IIS**.

1. Marque a caixa de **Serviços na World Wide Web**.

1. Aceite os recursos padrão dos **Serviços na World Wide Web** ou personalize os recursos do IIS.

   **Autenticação do Windows (opcional)**  
   Para habilitar a Autenticação do Windows, expanda os nós a seguir: **Serviços World Wide Web** > **Segurança**. Selecione o recurso **Autenticação do Windows**. Saiba mais em [Autenticação do Windows \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) e [Configurar autenticação do Windows](xref:security/authentication/windowsauth).

   **WebSockets (opcional)**  
   O WebSockets é compatível com o ASP.NET Core 1.1 ou posterior. Para habilitar o WebSockets, expanda os nós a seguir: **Serviços World Wide Web** > **Recursos de Desenvolvimento de Aplicativos**. Selecione o recurso **Protocolo WebSocket**. Para obter mais informações, consulte [WebSockets](xref:fundamentals/websockets).

1. Se a instalação do IIS exigir uma reinicialização, reinicie o sistema.

![O Console de Gerenciamento do IIS e os Serviços na World Wide Web estão selecionados em Recursos do Windows.](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-hosting-bundle"></a>Instalar o pacote de hospedagem do .NET Core

1. Instale o *pacote de hospedagem do .NET Core* no sistema de hospedagem. O pacote instala o Tempo de Execução .NET Core, a Biblioteca do .NET Core e o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). O módulo cria o proxy reverso entre o IIS e o servidor Kestrel. Se o sistema não tiver uma conexão com a Internet, obtenha e instale os [Pacotes redistribuíveis do Microsoft Visual C++ 2015](https://www.microsoft.com/download/details.aspx?id=53840) antes de instalar o pacote de hospedagem do .NET Core.

   1. Navegue até a [página Todos os downloads do .NET](https://www.microsoft.com/net/download/all).
   1. Selecione o tempo de execução do .NET Core sem visualização mais recente na lista (**.NET Core** > **Tempo de Execução** > **Tempo de Execução do .NET Core x.y.z**). A menos que você pretenda trabalhar com softwares de visualização, evite tempos de execução com a palavra "visualização" no texto de link.
   1. Na página de download de tempo de execução do .NET Core no **Windows**, selecione o link **Instalador de pacote de hospedagem** para baixar o *pacote de hospedagem do .NET Core*.

   **Importante!** Se o pacote de hospedagem for instalado antes do IIS, a instalação do pacote deverá ser reparada. Execute o instalador do pacote de hospedagem novamente depois de instalar o IIS.
   
   Para impedir que o instalador instale pacotes x86 em sistemas operacionais x64, execute o instalador em um prompt de comando de administrador com a opção `OPT_NO_X86=1`.

1. Reinicie o sistema ou execute **net stop was /y** seguido por **net start w3svc** em um prompt de comando. A reinicialização do IIS identifica uma alteração no caminho do sistema realizada pelo instalador.

> [!NOTE]
> Para obter informações sobre a Configuração Compartilhada do IIS, consulte [Módulo do ASP.NET Core com a Configuração Compartilhada do IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Instalar a Implantação da Web durante a publicação com o Visual Studio

Ao implantar aplicativos para servidores com [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy), instale a versão mais recente da Implantação da Web no servidor. Para instalar a Implantação da Web, use o [WebPI (Web Platform Installer)](https://www.microsoft.com/web/downloads/platform.aspx) ou obtenha um instalador diretamente no [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=43717). O método preferencial é usar o WebPI. O WebPI oferece uma instalação autônoma e uma configuração para provedores de hospedagem.

## <a name="create-the-iis-site"></a>Criar o site do IIS

1. No sistema de hospedagem, crie uma pasta para conter arquivos e pastas publicados do aplicativo. O layout de implantação do aplicativo é descrito no tópico [Estrutura de Diretórios](xref:host-and-deploy/directory-structure).

1. Na nova pasta, crie uma pasta *logs* para armazenar os logs do stdout do Módulo do ASP.NET Core quando o log do stdout estiver habilitado. Se o aplicativo for implantado com uma pasta *logs* no conteúdo, ignore esta etapa. Para obter instruções sobre como habilitar o MSBuild para criar a pasta *logs* automaticamente quando o projeto for criado localmente, veja o tópico [Estrutura de Diretórios](xref:host-and-deploy/directory-structure).

   > [!IMPORTANT]
   > Somente use o log do stdout para solucionar problemas de falhas de inicialização de aplicativo. Nunca use o log do stdout para registrar aplicativos de rotina. Não há limites para o tamanho do arquivo de log ou para o número de arquivos de log criados. O pool de aplicativos deve ter acesso de gravação ao local em que os logs foram gravados. Todas as pastas no caminho para a localização do log devem existir. Para saber mais sobre o log do stdout, veja [Criação e redirecionamento de logs](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection). Para saber mais sobre o registro em um aplicativo do ASP.NET Core, veja o tópico [Registrar](xref:fundamentals/logging/index).

1. No **Gerenciador do IIS**, abra o nó do servidor no painel **Conexões**. Clique com botão direito do mouse na pasta **Sites**. Selecione **Adicionar Site** no menu contextual.

1. Forneça um **Nome do site** e defina o **Caminho físico** como a pasta de implantação do aplicativo. Forneça a configuração **Associação** e crie o site ao selecionar **OK**:

   ![Forneça o Nome do site, o caminho físico e o Nome do host na etapa Adicionar Site.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Associações de curinga de nível superior (`http://*:80/` e `http://+:80`) **não** devem ser usadas. Associações de curinga de nível superior podem abrir o aplicativo para vulnerabilidades de segurança. Isso se aplica a curingas fortes e fracos. Use nomes de host explícitos em vez de curingas. Associações de curinga de subdomínio (por exemplo, `*.mysub.com`) não têm esse risco de segurança se você controlar o domínio pai completo (em vez de `*.com`, o qual é vulnerável). Veja [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) para obter mais informações.

1. No nó do servidor, selecione **Pools de Aplicativos**.

1. Clique com o botão direito do mouse no pool de aplicativos do site e selecione **Configurações Básicas** no menu contextual.

1. Na janela **Editar Pool de Aplicativos**, defina a **versão do CLR do .NET** como **Sem Código Gerenciado**:

   ![Defina Sem Código Gerenciado para a versão do CLR do .NET.](index/_static/edit-apppool-ws2016.png)

    O ASP.NET Core é executado em um processo separado e gerencia o tempo de execução. O ASP.NET Core não depende do carregamento do CLR de área de trabalho. Definir a **versão do CLR do .NET** como **Sem Código Gerenciado** é opcional.

1. Confirme se a identidade do modelo de processo tem as permissões apropriadas.

   Se você alterar a identidade padrão do pool de aplicativos (**Modelo de Processo** > **Identidade**) em **ApplicationPoolIdentity** para outra, verifique se a nova identidade tem as permissões necessárias para acessar a pasta do aplicativo, o banco de dados e outros recursos necessários. Por exemplo, o pool de aplicativos requer acesso de leitura e gravação às pastas nas quais o aplicativo lê e grava os arquivos.

**Configuração de Autenticação do Windows (opcional)**  
Para saber mais, veja [Configurar a Autenticação do Windows](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Implantar o aplicativo

Implante o aplicativo na pasta criada no sistema de hospedagem. A [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) é o mecanismo recomendado para implantação.

### <a name="web-deploy-with-visual-studio"></a>Implantação da Web com o Visual Studio

Consulte o tópico [Perfis de publicação do Visual Studio para implantação de aplicativos ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) para saber como criar um perfil de publicação para uso com a Implantação da Web. Se o provedor de hospedagem fornecer um Perfil de Publicação ou o suporte para a criação de um, baixe o perfil e importe-o usando a caixa de diálogo **Publicar** do Visual Studio.

![Página de caixa de diálogo Publicar](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Implantação da Web fora do Visual Studio

Também é possível usar a [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) fora do Visual Studio na linha de comando. Para obter mais informações, consulte [Ferramenta de Implantação da Web](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativas à Implantação da Web

Use qualquer um dos vários métodos para mover o aplicativo para o sistema host, como cópia manual, Xcopy, Robocopy ou PowerShell.

Para obter mais informações sobre a implantação do ASP.NET Core no IIS, consulte a seção [Recursos de implantação para administradores do IIS](#deployment-resources-for-iis-administrators).

## <a name="browse-the-website"></a>Navegar no site

![O navegador Microsoft Edge carregou a página de inicialização do IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Arquivos de implantação bloqueados

Os arquivos na pasta de implantação são bloqueados quando o aplicativo está em execução. Os arquivos bloqueados não podem ser substituídos durante a implantação. Para liberar os arquivos bloqueados em uma implantação, interrompa o pool de aplicativos usando **uma** das abordagens a seguir:

* Use a Implantação da Web e referencie `Microsoft.NET.Sdk.Web` no arquivo do projeto. Um arquivo *app_offline.htm* é colocado na raiz do diretório de aplicativo da Web. Quando o arquivo estiver presente, o módulo do ASP.NET Core apenas desligará o aplicativo e servirá o arquivo *app_offline.htm* durante a implantação. Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* Manualmente interrompa o pool de aplicativos no Gerenciador do IIS no servidor.
* Use o PowerShell para interromper e reiniciar o pool de aplicativos (requer o PowerShell 5 ou posterior):

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a>Proteção de dados

A [pilha Proteção de Dados do ASP.NET Core](xref:security/data-protection/index) é usada por vários [middlewares](xref:fundamentals/middleware/index) ASP.NET Core, incluindo aqueles usados na autenticação. Mesmo se as APIs de proteção de dados não forem chamadas pelo código do usuário, a proteção de dados deverá ser configurada com um script de implantação ou no código do usuário para criar um [repositório de chaves](xref:security/data-protection/implementation/key-management) criptográfico persistente. Se a proteção de dados não estiver configurada, as chaves serão mantidas na memória e descartadas quando o aplicativo for reiniciado.

Se o token de autenticação for armazenado na memória quando o aplicativo for reiniciado:

* Todos os tokens de autenticação baseados em cookies serão invalidados. 
* Os usuários precisam entrar novamente na próxima solicitação deles. 
* Todos os dados protegidos com o token de autenticação não poderão mais ser descriptografados. Isso pode incluir os [tokens CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) e [cookies TempData do MVC do ASP.NET Core](xref:fundamentals/app-state#tempdata).

Para configurar a proteção de dados no IIS para persistir o token de autenticação, use **uma** das seguintes abordagens:

* **Criar chaves de registro de proteção de dados**

  As chaves de proteção de dados usadas pelos aplicativos ASP.NET Core são armazenadas no registro externo aos aplicativos. Para persistir as chaves de determinado aplicativo, crie uma chave de registro para o pool de aplicativos.

  Para instalações autônomas do IIS não Web Farm, você pode usar o [script Provision-AutoGenKeys.ps1 de Proteção de Dados do PowerShell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para cada pool de aplicativos usado com um aplicativo ASP.NET Core. Esse script cria uma chave de registro no registro HKLM que é acessível apenas para a conta do processo de trabalho do pool de aplicativos do aplicativo. As chaves são criptografadas em repouso usando a DPAPI com uma chave para todo o computador.

  Em cenários de web farm, um aplicativo pode ser configurado para usar um caminho UNC para armazenar seu token de autenticação de proteção de dados. Por padrão, as chaves de proteção de dados não são criptografadas. Garanta que as permissões de arquivo de o compartilhamento de rede sejam limitadas à conta do Windows na qual o aplicativo é executado. Um certificado X509 pode ser usado para proteger chaves em repouso. Considere um mecanismo para permitir aos usuários carregar certificados: coloque os certificados no repositório de certificados confiáveis do usuário e certifique-se de que eles estejam disponíveis em todos os computadores nos quais o aplicativo do usuário é executado. Veja [Configurar a proteção de dados do ASP.NET Core](xref:security/data-protection/configuration/overview) para obter detalhes.

* **Configurar o pool de aplicativos do IIS para carregar o perfil do usuário**

  Essa configuração está na seção **Modelo de processo** nas **Configurações avançadas** do pool de aplicativos. Defina Carregar perfil do usuário como `True`. Isso armazena as chaves no diretório do perfil do usuário e as protege usando a DPAPI com uma chave específica à conta de usuário usada pelo pool de aplicativos.

* **Use o sistema de arquivos como um repositório de tokens de autenticação**

  Ajuste o código do aplicativo para [usar o sistema de arquivos como um repositório de tokens de autenticação](xref:security/data-protection/configuration/overview). Use um certificado X509 para proteger o token de autenticação e verifique se ele é um certificado confiável. Se o certificado for autoassinado, você deverá colocá-lo no repositório Raiz confiável.

  Ao usar o IIS em uma web farm:

  * Use um compartilhamento de arquivos que pode ser acessado por todos os computadores.
  * Implante um certificado X509 em cada computador. Configure a [proteção de dados no código](xref:security/data-protection/configuration/overview).

* **Definir uma política para todo o computador para proteção de dados**

  O sistema de proteção de dados tem suporte limitado para a configuração da [política de todo o computador](xref:security/data-protection/configuration/machine-wide-policy) padrão para todos os aplicativos que consomem as APIs de proteção de dados. Veja a documentação de [proteção de dados](xref:security/data-protection/index) para obter detalhes.

## <a name="sub-application-configuration"></a>Configuração de subaplicativos

Os subaplicativos adicionados no aplicativo raiz não devem incluir o Módulo do ASP.NET Core como um manipulador. Se o módulo for adicionado como um manipulador em um arquivo *web.config* de um subaplicativo, quando tentar procurar o subaplicativo, você receberá um *500.19 Erro Interno do Servidor* que referenciará o arquivo de configuração com falha.

O seguinte exemplo mostra um arquivo *web.config* publicado de um subaplicativo ASP.NET Core:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Ao hospedar um subaplicativo não ASP.NET Core abaixo de um aplicativo ASP.NET Core, remova explicitamente o manipulador herdado no arquivo *web.config* do subaplicativo:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Para obter mais informações sobre como configurar o Módulo do ASP.NET Core, consulte o tópico [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e a [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Configuração do IIS com web.config

A configuração do IIS é influenciada pela seção **\<system.webServer>** de *web.config* para os recursos do IIS que se aplicam a uma configuração de proxy reverso. Se o IIS é configurado no nível do servidor para usar a compactação dinâmica, o elemento **\<urlCompression>** no arquivo *web.config* do aplicativo pode desabilitá-la.

Para saber mais, veja a [referência de configuração do \<system.webServer>](/iis/configuration/system.webServer/), a [referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e [Módulos do IIS com o ASP.NET Core](xref:host-and-deploy/iis/modules). Para definir variáveis de ambiente para aplicativos individuais executados em pools de aplicativos isolados (compatível com o IIS 10.0+ ou posterior), veja a seção *comando AppCmd.exe* do tópico [Variáveis de ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) na documentação de referência do IIS.

## <a name="configuration-sections-of-webconfig"></a>Seções de configuração de web.config

As seções de configuração de aplicativos ASP.NET 4.x em *web.config* não são usadas por aplicativos ASP.NET Core para configuração:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<location>**

Aplicativos ASP.NET Core são configurados para usar outros provedores de configuração. Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pools de aplicativos

Ao hospedar vários sites em um servidor, isole os aplicativos entre si, executando cada aplicativo em seu próprio pool de aplicativos. A caixa de diálogo **Adicionar Site** do IIS usa como padrão essa configuração. Quando você fornece um **Nome do site**, o texto é transferido automaticamente para a caixa de texto **Pool de aplicativos**. Um novo pool de aplicativos é criado usando o nome do site quando você adicionar o site.

## <a name="application-pool-identity"></a>Identidade do pool de aplicativos

Uma conta de identidade do pool de aplicativos permite executar um aplicativo em uma conta exclusiva sem a necessidade de criar e gerenciar domínios ou contas locais. No IIS 8.0 ou posterior, o WAS (Processo de trabalho do administrador) do IIS cria uma conta virtual com o nome do novo pool de aplicativos e executa os processos de trabalho do pool de aplicativos nesta conta por padrão. No Console de Gerenciamento do IIS, em **Configurações avançadas** do pool de aplicativos, verifique se a **Identidade** é definida para usar **ApplicationPoolIdentity**:

![Caixa de diálogo Configurações avançadas do pool de aplicativos](index/_static/apppool-identity.png)

O processo de gerenciamento do IIS cria um identificador seguro com o nome do pool de aplicativos no Sistema de segurança do Windows. Os recursos podem ser protegidos usando essa identidade. No entanto, essa identidade não é uma conta de usuário real e não será mostrada no Console de Gerenciamento de Usuários do Windows.

Se o processo de trabalho do IIS requerer acesso elevado ao aplicativo, modifique a ACL (lista de controle de acesso) do diretório que contém o aplicativo:

1. Abra o Windows Explorer e navegue para o diretório.

1. Clique com o botão direito do mouse no diretório e selecione **Propriedades**.

1. Na guia **Segurança**, selecione o botão **Editar** e, em seguida, no botão **Adicionar**.

1. Clique no botão **Locais** e verifique se o sistema está selecionado.

1. Insira **IIS AppPool\\<nome_pool_aplicativos>** na área **Inserir os nomes de objeto a serem selecionados**. Selecione o botão **Verificar Nomes**. Para o *DefaultAppPool*, verifique os nomes usando **IIS AppPool\DefaultAppPool**. Quando o botão **Verificar Nomes** é selecionado, um valor de **DefaultAppPool** é indicado na área de nomes de objeto. Não é possível inserir o nome do pool de aplicativos diretamente na área de nomes de objeto. Use o formato **IIS AppPool\\<nome_pool_aplicativos>** ao verificar o nome do objeto.

   ![Selecione a caixa de diálogo de usuários ou grupos para a pasta do aplicativo: o nome do pool de aplicativos "DefaultAppPool" é anexado ao "IIS AppPool\" na área de nomes de objeto antes de selecionar"Verificar Nomes".](index/_static/select-users-or-groups-1.png)

1. Selecione **OK**.

   ![Selecione a caixa de diálogo de usuários ou grupos para a pasta do aplicativo: depois de selecionar "Verificar Nomes", o nome do objeto "DefaultAppPool" é mostrado na área de nomes de objeto.](index/_static/select-users-or-groups-2.png)

1. As permissões de leitura &amp; execução devem ser concedidas por padrão. Forneça permissões adicionais conforme necessário.

O acesso também pode ser concedido por meio de um prompt de comando usando a ferramenta **ICACLS**. Usando o *DefaultAppPool* como exemplo, o comando a seguir é usado:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Para saber mais, veja o tópico [icacls](/windows-server/administration/windows-commands/icacls).

## <a name="deployment-resources-for-iis-administrators"></a>Recursos de implantação para administradores do IIS

Conheça o ISS detalhadamente na documentação do IIS.  
[Documentação do ISS](/iis)

Saiba mais sobre os modelos de implantação de aplicativos do .NET Core.  
[Implantação de aplicativos do .NET Core](/dotnet/core/deploying/)

Saiba como o módulo do ASP.NET Core permite que o servidor Web Kestrel use o IIS ou o IIS Express como um servidor proxy reverso.  
[Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)

Saiba como configurar o módulo do ASP.NET Core para hospedar aplicativos do ASP.NET Core.  
[Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Saiba mais sobre a estrutura do diretório de aplicativos publicados do ASP.NET Core.  
[Estrutura de diretórios](xref:host-and-deploy/directory-structure)

Descubra módulos ativos e inativos do IIS para aplicativos do ASP.NET Core e como gerenciar os módulos do IIS.  
[Módulos do IIS](xref:host-and-deploy/iis/troubleshoot)

Saiba como diagnosticar problemas com as implantações do ISS dos aplicativos do ASP.NET Core.  
[Solução de problemas](xref:host-and-deploy/iis/troubleshoot)

Distinga erros comuns ao hospedar aplicativos do ASP.NET Core no IIS.  
[Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Recursos adicionais

* [Introdução ao ASP.NET Core](xref:index)
* [O site oficial da IIS da Microsoft](https://www.iis.net/)
* [Biblioteca de conteúdo técnico do Windows Server](/windows-server/windows-server)
