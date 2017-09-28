---
title: Hospedar o ASP.NET Core no Windows com o IIS
author: guardrex
description: "Configuração e implantação do IIS (Serviços de Informações da Internet) do Windows Server de aplicativos ASP.NET Core."
keywords: "ASP.NET Core, serviços de informações da Internet, IIS, Windows Server, pacote de hospedagem, módulo do ASP.NET Core, Implantação da Web"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: f506fc880e50aa2fdc772fd29b905dfad02cda2b
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-windows-with-iis-and-deploy-to-it"></a>Configurar um ambiente de hospedagem para o ASP.NET Core no Windows com o IIS e implantar nele

Por [Luke Latham](https://github.com/guardrex) e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Sistemas operacionais com suporte

Há suporte para os seguintes sistemas operacionais:

* Windows 7 e mais recente

* Windows Server 2008 R2 e mais recente&#8224;

&#8224;Conceitualmente, a configuração do IIS descrita neste documento também se aplica à hospedagem de aplicativos ASP.NET Core no IIS do Nano Server, mas consulte [ASP.NET Core com o IIS no Nano Server](xref:tutorials/nano-server) para obter instruções específicas.

O [servidor WebListener](xref:fundamentals/servers/weblistener) não funcionará em uma configuração de proxy reverso com o IIS. Você deve usar o [servidor Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Configuração do IIS

Habilite a função **Servidor Web (IIS)** e estabeleça serviços de função.

### <a name="windows-desktop-operating-systems"></a>Sistemas operacionais de área de trabalho do Windows

Navegue para **Painel de Controle > Programas > Programas e Recursos > Ativar ou desativar recursos do Windows** (lado esquerdo da tela). Abra o grupo de **Serviços de Informações da Internet** e **Ferramentas de Gerenciamento da Web**. Marque a caixa de **Console de Gerenciamento do IIS**. Marque a caixa de **Serviços na World Wide Web**. Aceite os recursos padrão dos **Serviços na World Wide Web** ou personalize os recursos do IIS de acordo com suas necessidades.

![O Console de Gerenciamento do IIS e os Serviços na World Wide Web estão selecionados em Recursos do Windows.](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Sistemas operacionais do Windows Server

Para sistemas operacionais de servidor, use o assistente para **Adicionar Funções e Recursos** por meio do menu **Gerenciar** ou do link no **Gerenciador do Servidor**. Na etapa **Funções de Servidor**, marque a caixa de **Servidor Web (IIS)**.

![A função de Servidor Web IIS é selecionada na etapa Selecionar funções de servidor.](iis/_static/server-roles-ws2016.png)

Na etapa **Serviços de função**, selecione os serviços de função do IIS desejados ou aceite os serviços de função padrão fornecidos.

![Os serviços de função padrão são selecionados na etapa Selecionar serviços de função.](iis/_static/role-services-ws2016.png)

Continue para a etapa **Confirmação** para instalar os serviços e a função de servidor Web. Uma reinicialização do servidor/IIS não é necessária após a instalação da função do Servidor Web (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Instalar o pacote de hospedagem do Windows Server do .NET Core

1. Instale o [pacote de hospedagem do Windows Server do .NET Core](https://aka.ms/dotnetcore.2.0.0-windowshosting) no sistema de hospedagem. O pacote instalará o Tempo de Execução .NET Core, a Biblioteca do .NET Core e o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). O módulo cria o proxy reverso entre o IIS e o servidor Kestrel. Observação: se o sistema não tiver uma conexão com a Internet, obtenha e instale os *[Pacotes Redistribuíveis do Microsoft Visual C++ 2015](https://www.microsoft.com/download/details.aspx?id=53840)* antes de instalar o pacote de hospedagem do Windows Server do .NET Core.

2. Reinicie o sistema ou execute **net stop was /y** seguido por **net start w3svc** em um prompt de comando para acompanhar uma alteração no PATH do sistema.

> [!NOTE]
> Se você usar uma Configuração Compartilhada do IIS, consulte [Módulo do ASP.NET Core com a Configuração Compartilhada do IIS](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Instalar a Implantação da Web durante a publicação com o Visual Studio

Se você pretende implantar seus aplicativos com a Implantação da Web no Visual Studio, instale a última versão da Implantação da Web no sistema de hospedagem. Para instalar a Implantação da Web, use o [WebPI (Web Platform Installer)](https://www.microsoft.com/web/downloads/platform.aspx) ou obtenha um instalador diretamente no [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=43717). O método preferencial é usar o WebPI. O WebPI oferece uma instalação autônoma e uma configuração para provedores de hospedagem.

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

Inclua uma dependência no pacote [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nas dependências do aplicativo. Incorpore o middleware Integração do IIS no aplicativo adicionando o método de extensão *UseIISIntegration* a *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Ambos `UseKestrel` e `UseIISIntegration` são necessários. A chamada do código a *UseIISIntegration* não afeta a portabilidade do código. Se o aplicativo não é executado por trás do IIS (por exemplo, o aplicativo é executado diretamente em Kestrel), `UseIISIntegration` fica sem operações.

---

Para obter mais informações sobre hospedagem, consulte [Hospedagem em ASP.NET Core](xref:fundamentals/hosting).

### <a name="iis-options"></a>Opções do IIS

Para configurar opções do serviço *IISIntegration*, inclua uma configuração de serviço para *IISOptions* em *ConfigureServices*:

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

O arquivo *web.config* configura o Módulo do ASP.NET Core e fornece outras configurações do IIS. A criação, transformação e publicação de *web.config* são manipuladas pelo `Microsoft.NET.Sdk.Web`, que é incluído quando você define o SDK do projeto na parte superior do arquivo *.csproj*, `<Project Sdk="Microsoft.NET.Sdk.Web">`. Para impedir que o destino do MSBuild se transforme no arquivo *web.config*, adicione a propriedade **\<IsTransformWebConfigDisabled>** ao arquivo de projeto com a configuração `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Caso você não tenha um arquivo *web.config* no projeto quando publicar com *dotnet publish* ou com a publicação do Visual Studio, o arquivo será criado para você no resultado publicado. Se você tiver o arquivo no projeto, ele será transformado com o *processPath* e os *arguments* corretos para configurar o Módulo do ASP.NET Core e será movido para o resultado publicado. A transformação não altera as definições de configuração do IIS incluídas no arquivo.

## <a name="create-the-iis-website"></a>Criar o site do IIS

1. No sistema do IIS de destino, crie uma pasta para conter as pastas e os arquivos publicados do aplicativo, que são descritos em [Estrutura de diretório](xref:hosting/directory-structure).

2. Na pasta criada, crie uma pasta *logs* para manter os logs do aplicativo (se você pretende habilitar o log). Se você pretende implantar o aplicativo com uma pasta *logs* na carga, ignore esta etapa.

3. Em **Gerenciador do IIS**, crie um novo site. Forneça um **Nome do site** e defina o **Caminho físico** como a pasta de implantação do aplicativo criada. Forneça a configuração **Associação** e crie o site.

4. Defina o pool de aplicativos como **Sem Código Gerenciado**. O ASP.NET Core é executado em um processo separado e gerencia o tempo de execução.

5. Abra a janela **Adicionar Site**.

   ![Clique em Adicionar Site no menu contextual de Sites.](iis/_static/add-website-context-menu-ws2016.png)

6. Configure o site.

   ![Forneça o Nome do site, o caminho físico e o Nome do host na etapa Adicionar Site.](iis/_static/add-website-ws2016.png)

7. No painel **Pools de Aplicativos**, abra a janela **Editar Pool de Aplicativos** clicando com o botão direito do mouse no pool de aplicativos do site e selecionando **Configurações Básicas...** no menu pop-up.

   ![Selecione Configurações Básicas no menu contextual do Pool de Aplicativos.](iis/_static/apppools-basic-settings-ws2016.png)

8. Defina a **versão do CLR do .NET** como **Sem Código Gerenciado**.

   ![Defina Sem Código Gerenciado para a versão do CLR do .NET.](iis/_static/edit-apppool-ws2016.png)
     
    Observação: a configuração da **versão do CLR do .NET** como **Sem Código Gerenciado** é opcional. O ASP.NET Core não depende do carregamento do CLR de área de trabalho.

9. Confirme se a identidade do modelo de processo tem as permissões apropriadas.

    Se você alterar a identidade padrão do pool de aplicativos (**Modelo de Processo** > **Identidade**) em **ApplicationPoolIdentity** para outra identidade, verifique se a nova identidade tem as permissões necessárias para acessar a pasta do aplicativo, o banco de dados e outros recursos necessários.
   
## <a name="deploy-the-application"></a>Implantar o aplicativo
Implante o aplicativo na pasta criada no sistema do IIS de destino. A Implantação da Web é o mecanismo recomendado para implantação. As alternativas à Implantação da Web são listadas abaixo.

Confirme se o aplicativo publicado para implantação não está em execução. Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução. A implantação não pode ocorrer porque os arquivos bloqueados não podem ser copiados.

### <a name="web-deploy-with-visual-studio"></a>Implantação da Web com o Visual Studio
Consulte o tópico [Criar perfis de publicação para o Visual Studio e o MSBuild para implantar aplicativos ASP.NET Core](xref:publishing/web-publishing-vs#publish-profiles) para saber como criar um perfil de publicação para uso com a Implantação da Web. Se o provedor de hospedagem fornecer um Perfil de Publicação ou o suporte para a criação de um, baixe seu perfil e importe-o usando a caixa de diálogo **Publicar** do Visual Studio.

![Página de caixa de diálogo Publicar](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Implantação da Web fora do Visual Studio
Também use a Implantação da Web fora do Visual Studio na linha de comando. Para obter mais informações, consulte [Ferramenta de Implantação da Web](https://docs.microsoft.com/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativas à Implantação da Web
Se você não desejar usar a Implantação da Web ou se não estiver usando o Visual Studio, poderá usar um dos vários métodos para mover o aplicativo para o sistema de hospedagem, como o Xcopy, Robocopy ou PowerShell. Os usuários do Visual Studio podem usar as [Amostras de Publicação](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Navegar no site
![O navegador Microsoft Edge carregou a página de inicialização do IIS.](iis/_static/browsewebsite.png)
   
>[!WARNING]
> Os aplicativos .NET Core são hospedados por meio de um proxy reverso entre o IIS e o servidor Kestrel. Para criar um proxy reverso, o arquivo *web.config* deve estar presente no caminho raiz do conteúdo (geralmente, o aplicativo base do caminho) do aplicativo implantado, que é o caminho físico do site fornecido ao IIS. Existem arquivos confidenciais no caminho físico do aplicativo, incluindo subpastas, como *my_application.runtimeconfig.json*, *my_application.xml* (comentários da Documentação XML) e *my_application.deps.json*. O arquivo *web.config* é necessário para criar o proxy reverso para o Kestrel, que impede que o IIS atenda a esses e outros arquivos confidenciais. **Portanto, é importante que o arquivo *web.config* nunca é renomeado ou removido acidentalmente da implantação.**

## <a name="data-protection"></a>Proteção de dados

Um aplicativo ASP.NET Core armazenará o token de autenticação na memória com a seguinte condição:

* Um site é hospedado com a proteção do IIS.
* A pilha da Proteção de Dados não foi configurada para armazenar o token de autenticação em um armazenamento persistente.

Se o token de autenticação for armazenado na memória, quando o aplicativo for reiniciado:

* Todos os tokens de autenticação de formulários serão inválidos. 
* Os usuários precisarão fazer logon novamente em sua próxima solicitação. 
* Todos os dados protegidos com o token de autenticação não ficarão mais desprotegidos.

> [!WARNING]
> A Proteção de Dados é usada por vários middlewares ASP.NET, incluindo aqueles usados na autenticação. Mesmo se você não chamar especificamente nenhuma API de Proteção de Dados no próprio código, deverá configurar a Proteção de Dados com um script de implantação ou no próprio código. Se você não configurar a proteção de dados, por padrão, as chaves serão mantidas na memória e descartadas quando o aplicativo for reiniciado. A reinicialização invalidará os cookies gravados pela autenticação de cookie e os usuários precisarão fazer logon novamente.

Para configurar a Proteção de Dados no IIS, use uma das seguintes abordagens:

* Execute um [script do PowerShell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para criar as entradas do Registro adequadas (por exemplo, `.\Provision-AutoGenKeys.ps1 DefaultAppPool`). Isso armazenará as chaves no Registro, protegidas usando a DPAPI com uma chave de todo o computador.
* Configure o Pool de Aplicativos do IIS para carregar o perfil do usuário. Essa configuração está na seção **Modelo de Processo** nas **Configurações Avançadas** do pool de aplicativos. Defina **Carregar Perfil do Usuário** como `True`. Isso armazenará as chaves no diretório do perfil do usuário e elas serão protegidas usando a DPAPI com uma chave específica à conta de usuário usada para o pool de aplicativos.
* Ajuste o código do aplicativo para [usar o sistema de arquivos como um repositório de tokens de autenticação](xref:security/data-protection/configuration/overview). Use um certificado X509 para proteger o token de autenticação e verifique se ele é um certificado confiável. Por exemplo, se ele for um certificado autoassinado, você deverá colocá-lo no armazenamento Raiz Confiável.

Ao usar o IIS em uma web farm:

* Use um compartilhamento de arquivos que pode ser acessado por todos os computadores.
* Implante um certificado X509 em cada computador.  Configure a [proteção de dados no código](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).

### <a name="1-create-a-data-protection-registry-hive"></a>1. Criar um Hive do Registro de Proteção de Dados

As chaves de Proteção de Dados usadas pelos aplicativos ASP.NET são armazenadas em hives do Registro externos aos aplicativos. Para persistir as chaves de determinado aplicativo, você deve criar um hive do Registro para o pool de aplicativos do aplicativo.

Para instalações autônomas do IIS, você pode usar o [script Provision-AutoGenKeys.ps1 de Proteção de Dados do PowerShell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para cada pool de aplicativos usado com um aplicativo ASP.NET Core. Esse script criará uma chave do Registro especial no registro HKLM que tem a ACL acessível apenas para a conta do processo de trabalho. As chaves são criptografadas em repouso usando a DPAPI.

Em cenários de web farm, um aplicativo pode ser configurado para usar um caminho UNC para armazenar seu token de autenticação de proteção de dados. Por padrão, as chaves de proteção de dados não são criptografadas. Garanta que as permissões de arquivo de um compartilhamento como esse são limitadas à conta do Windows na qual o aplicativo é executado. Além disso, você pode optar por proteger as chaves em repouso usando um certificado X509. Talvez você deseje considerar um mecanismo para permitir aos usuários carregar certificados, colocá-los no repositório de certificados confiáveis do usuário e garantir que eles estejam disponíveis em todos os computadores nos quais o aplicativo do usuário será executado. Consulte [Configurando a Proteção de Dados](xref:security/data-protection/configuration/overview#data-protection-configuring) para obter detalhes.

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2. Configurar o Pool de Aplicativos do IIS para carregar o perfil do usuário
Essa configuração está na seção Modelo de Processo nas Configurações Avançadas do pool de aplicativos. Defina Carregar Perfil do Usuário como Verdadeiro. Isso armazenará as chaves no diretório do perfil do usuário e elas serão protegidas usando a DPAPI com uma chave específica à conta de usuário usada para o pool de aplicativos.

### <a name="3-machine-wide-policy-for-data-protection"></a>3. Política de todo o computador para proteção de dados

O sistema de proteção de dados tem suporte limitado para a configuração da [política de todo o computador](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy) padrão para todos os aplicativos que consomem as APIs de proteção de dados. Consulte a documentação de [proteção de dados](xref:security/data-protection/index) para obter mais detalhes.

## <a name="configuration-of-sub-applications"></a>Configuração de subaplicativos

Os subaplicativos adicionados no aplicativo raiz não devem incluir o Módulo do ASP.NET Core como um manipulador. Se você adicionar o módulo como um manipulador em um arquivo *web.config* de um subaplicativo, você receberá um 500.19 (Erro interno do servidor) que referencia o arquivo de configuração com falha ao tentar procurar o subaplicativo. O seguinte exemplo mostra o conteúdo de um arquivo *web.config* publicado de um subaplicativo ASP.NET Core:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Se você desejar hospedar um não subaplicativo ASP.NET Core abaixo de um aplicativo ASP.NET Core, deverá remover explicitamente o manipulador herdado no arquivo *web.config* do subaplicativo:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Para obter mais informações sobre como configurar o Módulo do ASP.NET Core com o arquivo *web.config*, consulte o tópico [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e a [referência de configuração do Módulo do Core ASP.NET](xref:hosting/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Configuração do IIS com web.config

A configuração do IIS ainda é influenciada pela seção `<system.webServer>` de *web.config* para os recursos do IIS que se aplicam a uma configuração de proxy reverso. Por exemplo, você pode ter o IIS configurado no nível do sistema para usar a compactação dinâmica, mas poderá desabilitar essa configuração para um aplicativo com o elemento `<urlCompression>` no arquivo *web.config* do aplicativo. Para obter mais informações, consulte a [referência de configuração do `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/), a [referência de configuração do Módulo do ASP.NET Core](xref:hosting/aspnet-core-module) e [Usando os Módulos do IIS com o ASP.NET Core](xref:hosting/iis-modules). Se precisar definir variáveis de ambiente para aplicativos individuais executados em Pools de Aplicativos isolados (com suporte no IIS 10.0+), consulte a seção *comando AppCmd.exe* do tópico [Variáveis de ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) na documentação de referência do IIS.

## <a name="configuration-sections-of-webconfig"></a>Seções de configuração de web.config

Ao contrário dos aplicativos .NET Framework configurados com os elementos `<system.web>`, `<appSettings>`, `<connectionStrings>` e `<location>` em *web.config*, os aplicativos ASP.NET Core são configurados usando outros provedores de configuração. Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration).

## <a name="application-pools"></a>Pools de aplicativos

Ao hospedar vários sites em um único sistema, você deve isolar os aplicativos entre si, executando cada aplicativo em seu próprio pool de aplicativos. A caixa de diálogo **Adicionar Site** do IIS usa como padrão esse comportamento. Quando você fornece um **Nome do site**, o texto é transferido automaticamente para a caixa de texto **Pool de aplicativos**. Um novo pool de aplicativos será criado usando o nome do site quando você adicionar o site.

## <a name="application-pool-identity"></a>Identidade do pool de aplicativos

Uma conta de identidade do pool de aplicativos permite executar um aplicativo em uma conta exclusiva sem a necessidade de criar e gerenciar domínios ou contas locais. No IIS 8.0+, o WAS (Processo de Trabalho do Administrador) do IIS criará uma conta virtual com o nome do novo pool de aplicativos e executará os processos de trabalho do pool de aplicativos nesta conta por padrão. No Console de Gerenciamento do IIS, em Configurações Avançadas do pool de aplicativos, verifique se a identidade é definida para usar **ApplicationPoolIdentity**, conforme mostrado na imagem abaixo.

![Caixa de diálogo Configurações avançadas do pool de aplicativos](iis/_static/apppool-identity.png)

O processo de gerenciamento do IIS cria um identificador seguro com o nome do pool de aplicativos no Sistema de Segurança do Windows. Os recursos podem ser protegidos usando essa identidade; no entanto, essa identidade não é uma conta de usuário real e não será mostrada no Console de Gerenciamento de Usuário do Windows.

Se você precisar conceder ao processo de trabalho do IIS o acesso elevado ao aplicativo, precisará modificar a ACL (Lista de Controle de Acesso) do diretório que contém o aplicativo.

1. Abra o Windows Explorer e navegue para o diretório.

2. Clique com o botão direito do mouse no diretório e clique em **Propriedades**.

3. Na guia **Segurança**, clique no botão **Editar** e, em seguida, no botão **Adicionar**.

4. Clique no botão **Locais** e selecione seu sistema.

5. Insira **IIS AppPool\DefaultAppPool** na caixa de texto **Inserir os nomes de objeto a serem selecionados**.

  ![Caixa de diálogo Selecionar usuários ou grupos da pasta do aplicativo](iis/_static/select-users-or-groups-1.png)

6. Clique no botão **Verificar Nomes** e, em seguida, clique em **OK**.

  ![Caixa de diálogo Selecionar usuários ou grupos da pasta do aplicativo](iis/_static/select-users-or-groups-2.png)

Também faça isso por meio de um prompt de comando usando a ferramenta **ICACLS**:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>Dicas de solução de problemas

Para diagnosticar problemas com as implantações do IIS, estude o resultado do navegador, examine o log **Aplicativo** do sistema por meio do **Visualizador de Eventos** e habilite o log do `stdout`. O log do **Módulo do ASP.NET Core** será encontrado no caminho fornecido no atributo *stdoutLogFile* do elemento `<aspNetCore>` em *web.config*. As pastas no caminho fornecido no valor do atributo devem existir na implantação. Você também deve definir *stdoutLogEnabled="true"*. Os aplicativos que usam o SDK do `Microsoft.NET.Sdk.Web` para criar o arquivo *web.config* usarão como padrão a configuração *stdoutLogEnabled* como *false*. Portanto, é necessário fornecer manualmente o arquivo *web.config* ou modificar o arquivo para habilitar o log do `stdout`.

Vários dos erros comuns não são exibidos no navegador, no Log do Aplicativo e no Log do Módulo do ASP.NET Core até que o módulo *startupTimeLimit* (padrão: 120 segundos) e *startupRetryCount* (padrão: 2) tenham decorrido. Portanto, aguarde seis minutos completos antes de deduzir que o módulo falhou ao iniciar um processo para o aplicativo.

Uma maneira rápida de determinar se o aplicativo está funcionando corretamente é executar o aplicativo diretamente no Kestrel. Se o aplicativo foi publicado como uma implantação dependente de estrutura, execute **dotnet my_application.dll** na pasta de implantação, que é o caminho físico do IIS para o aplicativo. Se o aplicativo foi publicado como uma implantação independente, execute o executável do aplicativo diretamente em um prompt de comando, **my_application.exe**, na pasta de implantação. Se o Kestrel estiver escutando na porta padrão 5000, você poderá procurar o aplicativo em `http://localhost:5000/`. Se o aplicativo responder normalmente no endereço do ponto de extremidade do Kestrel, mais provavelmente, o problema estará relacionado à configuração do IIS-Módulo do ASP.NET Core-Kestrel e, menos provavelmente, ao próprio aplicativo.

Uma maneira de determinar se o proxy reverso do IIS para o servidor Kestrel está funcionando corretamente é executar uma solicitação de arquivo estático simples para uma folha de estilos, um script ou uma imagem dos arquivos estáticos do aplicativo em *wwwroot* usando o [middleware de Arquivo Estático](xref:fundamentals/static-files). Se o aplicativo puder atender a arquivos estáticos, mas as Exibições do MVC e outros pontos de extremidade estiverem falhando, menos provavelmente, o problema será relacionado à configuração do IIS/Módulo do ASP.NET Core/Kestrel e, mais provavelmente, ao próprio aplicativo (por exemplo, roteamento MVC ou 500 Erro interno de servidor).

Quando o Kestrel é iniciado normalmente com a proteção do IIS, mas o aplicativo não é executado no sistema depois de ser executado com êxito localmente, você pode adicionar temporariamente uma variável de ambiente ao *web.config* para definir o `ASPNETCORE_ENVIRONMENT` como `Development`. Desde que você não substitua o ambiente na inicialização do aplicativo, isso permitirá que a [página de exceção do desenvolvedor](xref:fundamentals/error-handling) seja exibida quando o aplicativo é executado no sistema. A configuração da variável de ambiente como `ASPNETCORE_ENVIRONMENT` dessa maneira só é recomendado para sistemas de preparo/teste que não são expostos à Internet. Lembre-se de remover a variável de ambiente do arquivo *web.config* quando terminar. Para obter informações sobre como definir variáveis de ambiente por meio de *web.config* no proxy reverso, consulte [Elemento filho environmentVariables de aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).

Na maioria dos casos, a habilitação do log do aplicativo ajudará na solução de problemas com o aplicativo ou o proxy reverso. Consulte [Log](xref:fundamentals/logging) para obter mais informações.

Nossa última dica de solução de problemas se refere a aplicativos que não são executados após o upgrade do SDK do .NET Core nas versões de pacote ou de computador de desenvolvimento no aplicativo. Em alguns casos, pacotes incoerentes podem interromper um aplicativo ao executar atualizações principais. Corrija a maioria desses problemas excluindo as pastas `bin` e `obj` do projeto, limpando os caches do pacote em `%UserProfile%\.nuget\packages\` e `%LocalAppData%\Nuget\v3-cache`, restaurando o projeto e confirmando se a implantação anterior no sistema foi completamente excluída antes de reimplantar o aplicativo.

>[!TIP]
> Uma maneira conveniente de limpar caches do pacote é obter a ferramenta `NuGet.exe` em [NuGet.org](https://www.nuget.org/), adicioná-la ao PATH do sistema e executar `nuget locals all -clear` em um prompt de comando.

## <a name="common-errors"></a>Erros comuns

A lista a seguir não é uma lista completa de erros. Caso encontre um erro não listado aqui, deixe uma mensagem de erro detalhada na seção de comentários abaixo.

### <a name="installer-unable-to-obtain-vc-redistributable"></a>O instalador não pode obter os Pacotes Redistribuíveis do VC++

* **Exceção do Instalador:** 0x80072efd ou 0x80072f76 – erro não especificado

* **Exceção do Log do Instalador&#8224;:** erro 0x80072efd ou 0x80072f76: falha ao executar o pacote EXE

  &#8224;O log está localizado em C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Solução de problemas:

* Se o sistema não tiver acesso à Internet durante a instalação do servidor que hospeda o pacote, essa exceção ocorrerá quando o instalador for impedido de obter os *Pacotes Redistribuíveis do Microsoft Visual C++ 2015*. Obtenha um instalador no [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=53840). Se o instalador falhar, talvez você não receba o tempo de execução do .NET Core necessário para hospedar implantações dependentes de estrutura. Se você pretende hospedar implantações dependentes de estrutura, confirme se o tempo de execução está instalado em Programas &amp; Recursos. Obtenha um instalador de tempo de execução em [Downloads do .NET](https://www.microsoft.com/net/download/core). Depois de instalar o tempo de execução, reinicie o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>O upgrade do sistema operacional removeu o Módulo do ASP.NET Core de 32 bits

* **Log do Aplicativo:** a DLL do Módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** falhou ao ser carregada. Os dados são o erro.

Solução de problemas:

* Arquivos que não são do sistema operacional no diretório **C:\Windows\SysWOW64\inetsrv** não são preservados durante um upgrade do sistema operacional. Se você tiver o Módulo do ASP.NET Core instalado antes de um upgrade do sistema operacional e, em seguida, tentar executar qualquer AppPool no modo de 32 bits após um upgrade do sistema operacional, encontrará esse problema. Após um upgrade do sistema operacional, repare o Módulo do ASP.NET Core. Consulte [Instalar o pacote de hospedagem do Windows Server do .NET Core](#install-the-net-core-windows-server-hosting-bundle). Selecione **Reparar** ao executar o instalador.

### <a name="platform-conflicts-with-rid"></a>Conflitos de plataforma com o RID

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo “MACHINE/WEBROOT/APPHOST/MY_APPLICATION” com a raiz física “C:\{PATH}\'” falhou ao iniciar o processo com a linha de comando “C:\\{PATH}\my_application.{exe|dll}” , ErrorCode = 0x80004005 : ff.

* **Log do Módulo do ASP.NET Core:** exceção sem tratamento: System.BadImageFormatException: não foi possível carregar o arquivo ou o assembly “my_application.dll”. Foi feita uma tentativa de carregar um programa com um formato incorreto.

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, consulte [Dicas de solução de problemas](#troubleshooting-tips).

* Confirme se você não definiu um `<PlatformTarget>` no *.csproj* que está em conflito com o RID. Por exemplo, não especifique um `<PlatformTarget>` igual a `x86` e publique com um RID igual a `win10-x64`, usando *dotnet publish -c Release -r win10-x64* ou definindo o `<RuntimeIdentifiers>` no *.csproj* como `win10-x64`. O projeto é publicado sem avisos nem erros, mas falha com as exceções registradas acima no sistema.

* Se essa exceção ocorrer para uma implantação dos Aplicativos do Azure ao fazer upgrade de um aplicativo e implantar assemblies mais recentes, exclua manualmente todos os arquivos da implantação anterior. Assemblies incompatíveis remanescentes podem resultar em uma exceção `System.BadImageFormatException` durante a implantação de um aplicativo atualizado.

### <a name="uri-endpoint-wrong-or-stopped-website"></a>Ponto de extremidade de URI incorreto ou site interrompido

* **Navegador:** ERR_CONNECTION_REFUSED

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Confirme se que você está usando o ponto de extremidade de URI correto para o aplicativo. Verifique as associações.

* Confirme se o site do IIS não está no estado *Parado*.

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>Recursos do servidor CoreWebEngine ou W3SVC desabilitados

* **Exceção do Sistema Operacional:** os recursos CoreWebEngine e W3SVC do IIS 7.0 devem ser instalados para usar o Módulo do ASP.NET Core.

Solução de problemas:

* Confirme se você habilitou a função e os recursos apropriados. Consulte [Configuração do IIS](#iis-configuration).

### <a name="incorrect-website-physical-path-or-application-missing"></a>Caminho físico do site incorreto ou aplicativo ausente

* **Navegador:** 403 Proibido – acesso negado **OU** 403.14 Proibido – o servidor Web está configurado para não listar o conteúdo deste diretório.

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Confira as **Configurações Básicas** no site do IIS e a pasta do aplicativo físico. Confirme se o aplicativo está na pasta no **Caminho físico** do site do IIS.

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Função incorreta, módulo não instalado ou permissões incorretas

* **Navegador:** 500.19 Erro interno do servidor – a página solicitada não pode ser acessada porque os dados de configuração relacionados da página são inválidos.

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas:

* Confirme se você habilitou a função apropriada. Consulte [Configuração do IIS](#iis-configuration).

* Verifique **Programas &amp; Recursos** e confirme se o **Módulo do Microsoft ASP.NET Core** foi instalado. Se o **Módulo do Microsoft ASP.NET Core** não estiver presente na lista de programas instalados, instale o módulo. Consulte [Instalar o pacote de hospedagem do Windows Server do .NET Core](#install-the-net-core-windows-server-hosting-bundle).

* Verifique se o **Pool de Aplicativos > Modelo de Processo > Identidade** está definido como **ApplicationPoolIdentity** ou se a identidade personalizada tem as permissões corretas para acessar a pasta de implantação do aplicativo.

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>processPath incorreto, variável de PATH ausente, pacote de hospedagem não instalado, sistema/IIS não reiniciado, Pacotes Redistribuíveis do VC++ não instalados ou violação de acesso de dotnet.exe

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo “MACHINE/WEBROOT/APPHOST/MY_APPLICATION” com a raiz física “C:\\{PATH}\'” falhou ao iniciar o processo com a linha de comando '".\my_application.exe" ', ErrorCode = '0x80070002 : 0.

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, consulte [Dicas de solução de problemas](#troubleshooting-tips).

* Verifique o atributo *processPath* no elemento `<aspNetCore>` em *web.config* para confirmar se ele é *dotnet* para uma implantação dependente de estrutura ou *.\my_application.exe* para uma implantação independente.

* Para uma implantação dependente de estrutura, *dotnet.exe* pode não estar acessível por meio das configurações de PATH. Confirme se *C:\Program Files\dotnet\* existe nas configurações de PATH do Sistema.

* Para uma implantação dependente de estrutura, *dotnet.exe* pode não estar acessível para a identidade do usuário do Pool de Aplicativos. Confirme se a identidade do usuário do AppPool tem acesso ao diretório *C:\Program Files\dotnet*. Confirme se não há nenhuma regra de negação configurada para a identidade do usuário do AppPool no *C:\Program Files\dotnet* e nos diretórios do aplicativo.

* Talvez você tenha implantado uma implantação dependente de estrutura e instalou o .NET Core sem reiniciar o IIS. Reinicie o servidor ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

* Talvez você tenha implantado uma implantação dependente de estrutura sem instalar o tempo de execução do .NET Core no sistema de hospedagem. Se estiver tentando implantar uma implantação dependente de estrutura e não tiver instalado o tempo de execução do .NET Core, execute o **instalador do pacote de Hospedagem do Windows Server do .NET Core** no sistema. Consulte [Instalar o pacote de hospedagem do Windows Server do .NET Core](#install-the-net-core-windows-server-hosting-bundle). Se estiver tentando instalar o tempo de execução do .NET Core em um sistema sem uma conexão com a Internet, obtenha o tempo de execução em [Downloads do .NET](https://www.microsoft.com/net/download/core) e execute o instalador do pacote de hospedagem para instalar o Módulo do ASP.NET Core. Conclua a instalação reiniciando o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

* Talvez você tenha implantado uma implantação dependente de estrutura e instalou o .NET Core sem reiniciar o sistema/IIS. Reinicie o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.

* Talvez você tenha implantado uma implantação dependente de estrutura e os *Pacotes Redistribuíveis do Microsoft Visual C++ 2015 (x64)* não estão instalados no sistema. Obtenha um instalador no [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).

### <a name="incorrect-arguments-of-aspnetcore-element"></a>Argumentos incorretos do elemento \<aspNetCore\>

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo “MACHINE/WEBROOT/APPHOST/MY_APPLICATION” com a raiz física “C:\\{PATH}\'” falhou ao iniciar o processo com a linha de comando “dotnet” .\my_application.dll', ErrorCode = '0x80004005 : 80008081”.

* **Log do Módulo do ASP.NET Core:** o aplicativo a ser executado não existe: “PATH\my_application.dll”

Solução de problemas:

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, consulte [Dicas de solução de problemas](#troubleshooting-tips).

* Examine o atributo *arguments* no elemento `<aspNetCore>` no *web.config* para confirmar se ele é (a) *.\my_application.dll* de uma implantação dependente de estrutura; ou (b) não está presente, uma cadeia de caracteres vazia (*arguments=""*) ou uma lista de argumentos do aplicativo (*arguments="arg1, arg2, ..."*) para uma implantação independente.

### <a name="missing-net-framework-version"></a>Versão do .NET Framework ausente

* **Navegador:** 502.3 Gateway incorreto – Erro de conexão ao tentar encaminhar a solicitação.

* **Log do Aplicativo:** ErrorCode = o aplicativo “MACHINE/WEBROOT/APPHOST/MY_APPLICATION” com a raiz física “C:\\{PATH}\'” falhou ao iniciar o processo com a linha de comando “dotnet” .\my_application.dll', ErrorCode = '0x80004005 : 80008081”.

* **Log do Módulo do ASP.NET Core:** exceção do método, arquivo ou assembly ausente. O método, o arquivo ou o assembly especificado na exceção é um método, arquivo ou assembly do .NET Framework.

Solução de problemas:

* Instale a versão do .NET Framework ausente no sistema.

* Para uma implantação dependente de estrutura, confirme se você tem o tempo de execução correto instalado no sistema. Por exemplo, se você fizer upgrade de um projeto do 1.0 para 1.1, implantar no sistema de hospedagem e receber essa exceção, instale o Framework 1.1 no sistema de hospedagem.

### <a name="stopped-application-pool"></a>Pool de aplicativos interrompido

* **Navegador:** 503 Serviço não disponível

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log não criado

Solução de problemas

* Confirme se o Pool de Aplicativos não está no estado *Parado*.

### <a name="iis-integration-middleware-not-implemented"></a>Middleware de Integração do IIS não implementado

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo “MACHINE/WEBROOT/APPHOST/MY_APPLICATION” com a raiz física “C:\\{PATH}\'” criou um processo com a linha de comando “C:\\{PATH}\my_application.{exe|dll}”, mas falhou, não respondeu ou não escutou na porta determinada '{PORT}', ErrorCode = '0x800705b4'

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mostrando uma operação normal.

Solução de problemas

* Confirme se o aplicativo é executado localmente no Kestrel. Uma falha do processo pode ser o resultado de um problema no aplicativo. Para obter mais informações, consulte [Dicas de solução de problemas](#troubleshooting-tips).

* Confirme se você referenciou corretamente o middleware Integração do IIS chamando o método *.UseIISIntegration()* no *WebHostBuilder()* do aplicativo.

### <a name="sub-application-includes-a-handlers-section"></a>O subaplicativo inclui uma seção \<manipuladores\>

* **Navegador:** 500.19 Erro HTTP – erro interno do servidor

* **Log do Aplicativo:** nenhuma entrada

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mostrando uma operação normal do aplicativo raiz. O arquivo de log não é criado para o subaplicativo.

Solução de problemas

* Confirme se o arquivo *web.config* do subaplicativo não inclui uma seção `<handlers>`.

### <a name="application-configuration-general-issue"></a>Problema geral de configuração do aplicativo

* **Navegador:** 502.5 Erro HTTP – falha do processo

* **Log do Aplicativo:** o aplicativo “MACHINE/WEBROOT/APPHOST/MY_APPLICATION” com a raiz física “C:\\{PATH}\'” criou um processo com a linha de comando “C:\\{PATH}\my_application.{exe|dll}”, mas falhou, não respondeu ou não escutou na porta determinada '{PORT}', ErrorCode = '0x800705b4'

* **Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio

Solução de problemas

* Essa exceção geral indica que o processo não pôde ser iniciado, provavelmente, devido a um problema de configuração do aplicativo. Consultando [Estrutura de diretório](xref:hosting/directory-structure), confirme se as pastas e os arquivos implantados do aplicativo são apropriados e se os arquivos de configuração do aplicativo estão presentes e contêm as configurações corretas para o aplicativo e o ambiente. Para obter mais informações, consulte [Dicas de solução de problemas](#troubleshooting-tips).

## <a name="resources"></a>Recursos

* [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)

* [Referência de configuração do Módulo do ASP.NET Core](xref:hosting/aspnet-core-module)

* [Usando Módulos do IIS com o ASP.NET Core](xref:hosting/iis-modules)

* [Introdução ao ASP.NET Core](../index.md)

* [O site oficial da IIS da Microsoft](https://www.iis.net/)

* [Biblioteca Microsoft TechNet: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
