---
title: Hospedar o ASP.NET Core no Windows com o IIS
author: guardrex
description: "Saiba como hospedar aplicativos ASP.NET Core no Windows Server IIS (Serviços de Informações da Internet)."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1df438af2394f41b686413cd1ce5ad73a9416ec5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Hospedar o ASP.NET Core no Windows com o IIS

Por [Luke Latham](https://github.com/guardrex) e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Sistemas operacionais com suporte

Há suporte para os seguintes sistemas operacionais:

* Windows 7 e mais recente
* Windows Server 2008 R2 e mais recente&#8224;

&#8224;Conceitualmente, a configuração do IIS descrita neste documento também se aplica à hospedagem de aplicativos ASP.NET Core no IIS do Nano Server, mas consulte [ASP.NET Core com o IIS no Nano Server](xref:tutorials/nano-server) para obter instruções específicas.

O [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente chamado de [WebListener](xref:fundamentals/servers/weblistener)) não funcionará em uma configuração de proxy reverso com IIS. Use o [servidor Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Configuração do IIS

Habilite a função **Servidor Web (IIS)** e estabeleça serviços de função.

### <a name="windows-desktop-operating-systems"></a>Sistemas operacionais de área de trabalho do Windows

Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela). Abra o grupo de **Serviços de Informações da Internet** e **Ferramentas de Gerenciamento da Web**. Marque a caixa de **Console de Gerenciamento do IIS**. Marque a caixa de **Serviços na World Wide Web**. Aceite os recursos padrão dos **Serviços na World Wide Web** ou personalize os recursos do IIS.

![O Console de Gerenciamento do IIS e os Serviços na World Wide Web estão selecionados em Recursos do Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Sistemas operacionais do Windows Server

Para sistemas operacionais de servidor, use o assistente para **Adicionar Funções e Recursos** por meio do menu **Gerenciar** ou do link no **Gerenciador do Servidor**. Na etapa **Funções de Servidor**, marque a caixa de **Servidor Web (IIS)**.

![A função de Servidor Web IIS é selecionada na etapa Selecionar funções de servidor.](index/_static/server-roles-ws2016.png)

Na etapa **Serviços de função**, selecione os serviços de função do IIS desejados ou aceite os serviços de função padrão fornecidos.

![Os serviços de função padrão são selecionados na etapa Selecionar serviços de função.](index/_static/role-services-ws2016.png)

Continue para a etapa **Confirmação** para instalar os serviços e a função de servidor Web. A reinicialização de um servidor/IIS não é necessária após a instalação da função do Servidor Web (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Instalar o pacote de hospedagem do Windows Server do .NET Core

1. Instale o [pacote de hospedagem do Windows Server do .NET Core](https://aka.ms/dotnetcore-2-windowshosting) no sistema de hospedagem. O pacote instala o Tempo de Execução .NET Core, a Biblioteca do .NET Core e o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). O módulo cria o proxy reverso entre o IIS e o servidor Kestrel. Se o sistema não tiver uma conexão com a Internet, obtenha e instale os [Pacotes redistribuíveis do Microsoft Visual C++ 2015](https://www.microsoft.com/download/details.aspx?id=53840) antes de instalar o pacote de hospedagem do Windows Server do .NET Core.

2. Reinicie o sistema ou execute **net stop was /y** seguido por **net start w3svc** em um prompt de comando para acompanhar uma alteração no PATH do sistema.

> [!NOTE]
> Para obter informações sobre a Configuração Compartilhada do IIS, consulte [Módulo do ASP.NET Core com a Configuração Compartilhada do IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Instalar a Implantação da Web durante a publicação com o Visual Studio

Ao implantar aplicativos para servidores com [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy), instale a versão mais recente da Implantação da Web no servidor. Para instalar a Implantação da Web, use o [WebPI (Web Platform Installer)](https://www.microsoft.com/web/downloads/platform.aspx) ou obtenha um instalador diretamente no [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=43717). O método preferencial é usar o WebPI. O WebPI oferece uma instalação autônoma e uma configuração para provedores de hospedagem.

## <a name="application-configuration"></a>Configuração do aplicativo

### <a name="enabling-the-iisintegration-components"></a>Habilitando os componentes de IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Um típico *Program.cs* chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host. `CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) como o servidor Web e habilita a integração IIS configurando o caminho base e a porta para o [módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Inclua uma dependência no pacote [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nas dependências do aplicativo. Incorpore o middleware Integração do IIS no aplicativo adicionando o método de extensão *UseIISIntegration* ao *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Ambos `UseKestrel` e `UseIISIntegration` são necessários. A chamada do código a `UseIISIntegration` não afeta a portabilidade do código. Se o aplicativo não é executado por trás do IIS (por exemplo, o aplicativo é executado diretamente em Kestrel), `UseIISIntegration` fica sem operações.

---

Para obter mais informações sobre hospedagem, consulte [Hospedagem em ASP.NET Core](xref:fundamentals/hosting).

### <a name="iis-options"></a>Opções do IIS

Para configurar opções do IIS, inclua uma configuração de serviço para `IISOptions` em `ConfigureServices`:

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| Opção                         | Padrão | Configuração |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | Se `true`, o middleware de autenticação configurará o `HttpContext.User` e responderá a desafios genéricos. Se `false`, o middleware de autenticação fornecerá apenas uma identidade (`HttpContext.User`) e responderá a desafios quando explicitamente solicitado pelo `AuthenticationScheme`. A autenticação do Windows deve estar habilitada no IIS para que o `AutomaticAuthentication` funcione. |
| `AuthenticationDisplayName`    | `null`  | Configura o nome de exibição mostrado aos usuários em páginas de logon. |
| `ForwardClientCertificate`     | `true`  | Se `true` e o cabeçalho da solicitação `MS-ASPNETCORE-CLIENTCERT` estiverem presentes, o `HttpContext.Connection.ClientCertificate` será populado. |

### <a name="webconfig"></a>web.config

O objetivo principal do arquivo *web.config* é configurar o [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Como opção, ele pode fornecer definições de configuração IIS adicionais. A criação, transformação e publicação do *web.config* é gerenciada pelo SDK Web do .NET Core (`Microsoft.NET.Sdk.Web`). O SDK é definido na parte superior do arquivo de projeto, `<Project Sdk="Microsoft.NET.Sdk.Web">`. Para impedir que o SDK se transforme no arquivo *web.config*, adicione a propriedade **\<IsTransformWebConfigDisabled>** ao arquivo de projeto com a configuração `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Se um arquivo *web.config* estiver no projeto, ele será transformado com o *processPath* e os *arguments* corretos para configurar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e será movido para o [resultado publicado](xref:host-and-deploy/directory-structure). A transformação não altera as definições de configuração do IIS no arquivo.

### <a name="webconfig-location"></a>local de web.config

Os aplicativos .NET Core são hospedados por meio de um proxy reverso entre o IIS e o servidor Kestrel. Para criar um proxy reverso, o arquivo *web.config* deve estar presente no caminho raiz do conteúdo (geralmente, o aplicativo base do caminho) do aplicativo implantado, que é o caminho físico do site fornecido ao IIS. O arquivo *web.config* é necessário na raiz do aplicativo para habilitar a publicação de vários aplicativos usando a Implantação da Web.

Existem arquivos confidenciais no caminho físico do aplicativo, incluindo subpastas, como *\<nome_do_assembly>.runtimeconfig.json*, *\<nome_do_assembly>.xml* (comentários da Documentação XML) e *\<nome_do_assembly>.deps.json*. Quando o arquivo *web.config* está presente e configura o site, o IIS impede que esses arquivos confidenciais sejam servidos. **Portanto, é importante que o arquivo *web.config* não seja renomeado ou removido acidentalmente da implantação.**

## <a name="create-the-iis-website"></a>Criar o site do IIS

1. No sistema do IIS de destino, crie uma pasta para conter as pastas e os arquivos publicados do aplicativo, que são descritos em [Estrutura de diretório](xref:host-and-deploy/directory-structure).

2. Na pasta, crie uma pasta *logs* para armazenar os logs do stdout quando o log do stdout estiver habilitado. Se o aplicativo for implantado com uma pasta *logs* no conteúdo, ignore esta etapa. Para obter instruções sobre como fazer com que o MSBuild crie a pasta *logs*, consulte o tópico [Estrutura de diretórios](xref:host-and-deploy/directory-structure).

3. Em **Gerenciador do IIS**, crie um novo site. Forneça um **Nome do site** e defina o **Caminho físico** como a pasta de implantação do aplicativo. Forneça a configuração **Associação** e crie o site.

4. Defina o pool de aplicativos como **Sem Código Gerenciado**. O ASP.NET Core é executado em um processo separado e gerencia o tempo de execução.

5. Abra a janela **Adicionar Site**.

   ![Selecione 'Adicionar Site' no menu contextual de Sites.](index/_static/add-website-context-menu-ws2016.png)

6. Configure o site.

   ![Forneça o Nome do site, o caminho físico e o Nome do host na etapa Adicionar Site.](index/_static/add-website-ws2016.png)

7. No painel **Pools de Aplicativos**, abra a janela **Editar pool de aplicativos** clicando com o botão direito do mouse no pool de aplicativos do site da Web e selecionando **Configurações Básicas...** no menu pop-up.

   ![Selecione Configurações Básicas no menu contextual do Pool de Aplicativos.](index/_static/apppools-basic-settings-ws2016.png)

8. Defina a **versão do CLR do .NET** como **Sem Código Gerenciado**.

   ![Defina Sem Código Gerenciado para a versão do CLR do .NET.](index/_static/edit-apppool-ws2016.png)
     
    Observação: a configuração da **versão do CLR do .NET** como **Sem Código Gerenciado** é opcional. O ASP.NET Core não depende do carregamento do CLR de área de trabalho.

9. Confirme se a identidade do modelo de processo tem as permissões apropriadas.

   Se você alterar a identidade padrão do pool de aplicativos (**Modelo de Processo** > **Identidade**) em **ApplicationPoolIdentity** para outra, verifique se a nova identidade tem as permissões necessárias para acessar a pasta do aplicativo, o banco de dados e outros recursos necessários.
   
## <a name="deploy-the-app"></a>Implantar o aplicativo

Implante o aplicativo na pasta criada no sistema do IIS de destino. A [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) é o mecanismo recomendado para implantação.

Confirme se o aplicativo publicado para implantação não está em execução. Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução. Arquivos bloqueados não podem ser substituídos. Para liberar os arquivos bloqueados em uma implantação, interrompa o pool de aplicativos:

* Manualmente no Gerenciador do IIS no servidor.
* Usar a Implantação da Web e referenciar `Microsoft.NET.Sdk.Web` no arquivo de projeto. Um arquivo *app_offline.htm* é colocado na raiz do diretório de aplicativo da Web. Quando o arquivo estiver presente, o módulo do ASP.NET Core apenas desligará o aplicativo e servirá o arquivo *app_offline.htm* durante a implantação. Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).
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

### <a name="web-deploy-with-visual-studio"></a>Implantação da Web com o Visual Studio

Consulte o tópico [Perfis de publicação do Visual Studio para implantação de aplicativos ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) para saber como criar um perfil de publicação para uso com a Implantação da Web. Se o provedor de hospedagem fornecer um Perfil de Publicação ou o suporte para a criação de um, baixe o perfil e importe-o usando a caixa de diálogo **Publicar** do Visual Studio.

![Página de caixa de diálogo Publicar](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Implantação da Web fora do Visual Studio

Também é possível usar a [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) fora do Visual Studio na linha de comando. Para obter mais informações, consulte [Ferramenta de Implantação da Web](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativas à Implantação da Web

Use qualquer um dos vários métodos para mover o aplicativo para o sistema host, como Xcopy, Robocopy ou PowerShell. Os usuários do Visual Studio podem usar as [Amostras de Publicação](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Navegar no site

![O navegador Microsoft Edge carregou a página de inicialização do IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a>Proteção de dados

A Proteção de Dados é usada por vários middlewares ASP.NET, incluindo aqueles usados na autenticação. Mesmo se APIs de proteção de dados não são chamadas do código do usuário, a proteção de dados deve ser configurada com um script de implantação ou no código do usuário para criar um repositório de chaves persistente. Se a proteção de dados não estiver configurada, as chaves serão mantidas na memória e descartadas quando o aplicativo for reiniciado.

Se o token de autenticação for armazenado na memória, quando o aplicativo for reiniciado:

* Todos os tokens de autenticação de formulários serão inválidos. 
* Os usuários precisam entrar novamente na próxima solicitação deles. 
* Todos os dados protegidos com o token de autenticação não podem mais ser descriptografados.

Para configurar a Proteção de dados no IIS, use **uma** das seguintes abordagens:

### <a name="create-a-data-protection-registry-hive"></a>Criar um Hive do Registro de Proteção de Dados

As chaves de Proteção de Dados usadas pelos aplicativos ASP.NET são armazenadas em hives do Registro externos aos aplicativos. Para persistir as chaves de determinado aplicativo, crie um hive do registro para o pool do aplicativo.

Para instalações autônomas do IIS não Web Farm, você pode usar o [script Provision-AutoGenKeys.ps1 de Proteção de Dados do PowerShell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para cada pool de aplicativos usado com um aplicativo ASP.NET Core. Esse script cria uma chave do registro especial no registro HKLM que tem a ACL acessível apenas para a conta do processo de trabalho. As chaves são criptografadas em repouso usando a DPAPI com uma chave para todo o computador.

Em cenários de web farm, um aplicativo pode ser configurado para usar um caminho UNC para armazenar seu token de autenticação de proteção de dados. Por padrão, as chaves de proteção de dados não são criptografadas. Garanta que as permissões de arquivo de um compartilhamento como esse são limitadas à conta do Windows na qual o aplicativo é executado. Além disso, um certificado X509 pode ser usado para proteger chaves em repouso. Considere um mecanismo para permitir aos usuários carregar certificados: coloque os certificados no repositório de certificados confiáveis do usuário e certifique-se de que eles estejam disponíveis em todos os computadores nos quais o aplicativo do usuário é executado. Consulte [Configurando a Proteção de Dados](xref:security/data-protection/configuration/overview) para obter detalhes.

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>Configurar o Pool de Aplicativos do IIS para carregar o perfil do usuário

Essa configuração está na seção **Modelo de processo** nas **Configurações avançadas** do pool de aplicativos. Defina Carregar perfil do usuário como `True`. Isso armazena as chaves no diretório do perfil do usuário e as protege usando a DPAPI com uma chave específica à conta de usuário usada para o pool de aplicativos.

### <a name="use-the-file-system-as-a-key-ring-store"></a>Usar o sistema de arquivos como um repositório de tokens de autenticação

Ajuste o código do aplicativo para [usar o sistema de arquivos como um repositório de tokens de autenticação](xref:security/data-protection/configuration/overview). Use um certificado X509 para proteger o token de autenticação e verifique se ele é um certificado confiável. Se ele for um certificado autoassinado, você deverá colocá-lo no repositório Raiz confiável.

Ao usar o IIS em uma web farm:

* Use um compartilhamento de arquivos que pode ser acessado por todos os computadores.
* Implante um certificado X509 em cada computador. Configure a [proteção de dados no código](xref:security/data-protection/configuration/overview).

### <a name="set-a-machine-wide-policy-for-data-protection"></a>Definir uma política para todo o computador para proteção de dados

O sistema de proteção de dados tem suporte limitado para a configuração da [política de todo o computador](xref:security/data-protection/configuration/machine-wide-policy) padrão para todos os aplicativos que consomem as APIs de proteção de dados. Consulte a documentação de [proteção de dados](xref:security/data-protection/index) para obter mais detalhes.

## <a name="configuration-of-sub-applications"></a>Configuração de subaplicativos

Os subaplicativos adicionados no aplicativo raiz não devem incluir o Módulo do ASP.NET Core como um manipulador. Se o módulo for adicionado como um manipulador em um arquivo *web.config* de um subaplicativo, quando tentar procurar o subaplicativo, você receberá um 500.19 (Erro interno do servidor) que referenciará o arquivo de configuração com falha. O seguinte exemplo mostra o conteúdo de um arquivo *web.config* publicado de um subaplicativo ASP.NET Core:

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
      <remove name="aspNetCore"/>
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

A configuração do IIS é influenciada pela seção **\<system.webServer>** de *web.config* para os recursos do IIS que se aplicam a uma configuração de proxy reverso. Se o IIS é configurado no nível do sistema para usar a compactação dinâmica, essa configuração pode ser desabilitada para um aplicativo com o elemento **\<urlCompression>** no arquivo *web.config* do aplicativo. Para obter mais informações, consulte a [referência de configuração do \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), a [referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e [Usando os Módulos do IIS com o ASP.NET Core](xref:host-and-deploy/iis/modules). Se precisar definir variáveis de ambiente para aplicativos individuais executados em pools de aplicativos isolados (compatível no IIS 10.0+), consulte a seção *comando AppCmd.exe* do tópico [Variáveis de ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) na documentação de referência do IIS.

## <a name="configuration-sections-of-webconfig"></a>Seções de configuração de web.config

Seções de configuração de aplicativos ASP.NET Framework em *web.config* não são usados por aplicativos ASP.NET Core para configuração:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<location>**

Aplicativos ASP.NET Core são configurados para usar outros provedores de configuração. Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pools de aplicativos

Ao hospedar vários sites em um único sistema, isole os aplicativos entre si, executando cada aplicativo em seu próprio pool de aplicativos. A caixa de diálogo **Adicionar Site** do IIS usa como padrão essa configuração. Quando você fornece um **Nome do site**, o texto é transferido automaticamente para a caixa de texto **Pool de aplicativos**. Um novo pool de aplicativos é criado usando o nome do site quando você adicionar o site.

## <a name="application-pool-identity"></a>Identidade do pool de aplicativos

Uma conta de identidade do pool de aplicativos permite executar um aplicativo em uma conta exclusiva sem a necessidade de criar e gerenciar domínios ou contas locais. No IIS 8.0+, o WAS (Processo de trabalho do administrador) do IIS cria uma conta virtual com o nome do novo pool de aplicativos e executa os processos de trabalho do pool de aplicativos nesta conta por padrão. No Console de Gerenciamento do IIS, em **Configurações avançadas** do pool de aplicativos, verifique se a **Identidade** é definida para usar **ApplicationPoolIdentity**:

![Caixa de diálogo Configurações avançadas do pool de aplicativos](index/_static/apppool-identity.png)

O processo de gerenciamento do IIS cria um identificador seguro com o nome do pool de aplicativos no Sistema de segurança do Windows. Os recursos podem ser protegidos usando essa identidade; no entanto, essa identidade não é uma conta de usuário real e não será mostrada no Console de Gerenciamento de usuário do Windows.

Se o processo de trabalho do IIS requerer acesso elevado ao aplicativo, modifique a ACL (lista de controle de acesso) do diretório que contém o aplicativo:

1. Abra o Windows Explorer e navegue para o diretório.

2. Clique com o botão direito do mouse no diretório e selecione **Propriedades**.

3. Na guia **Segurança**, selecione o botão **Editar** e, em seguida, no botão **Adicionar**.

4. Clique no botão **Locais** e verifique se o sistema está selecionado.

5. Insira **IIS AppPool\DefaultAppPool** na caixa de texto **Inserir os nomes de objeto a serem selecionados**.

   ![Caixa de diálogo Selecionar usuários ou grupos da pasta do aplicativo](index/_static/select-users-or-groups-1.png)

6. Selecione o botão **Verificar Nomes**. Selecione **OK**.

   ![Caixa de diálogo Selecionar usuários ou grupos da pasta do aplicativo](index/_static/select-users-or-groups-2.png)

Isso também pode ser feito por meio de um prompt de comando usando a ferramenta **ICACLS**:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>Recursos adicionais

* [Solucionar problemas do ASP.NET Core no IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Usando Módulos do IIS com o ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Introdução ao ASP.NET Core](../index.md)
* [O site oficial da IIS da Microsoft](https://www.iis.net/)
* [Biblioteca Microsoft TechNet: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
