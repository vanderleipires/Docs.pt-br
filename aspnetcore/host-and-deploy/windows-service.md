---
title: Hospedar o ASP.NET Core em um serviço Windows
author: guardrex
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758187"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedar o ASP.NET Core em um serviço Windows

Por [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)

Um aplicativo ASP.NET Core pode ser hospedado no Windows sem usar o IIS como um [Serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando hospedado como um Serviço Windows, o aplicativo é iniciado automaticamente após a reinicialização.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([como baixar](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Converter um projeto em um serviço Windows

As alterações mínimas a seguir são necessárias para configurar um projeto existente do ASP.NET Core a ser executado como um serviço:

1. No arquivo de projeto:

   * Confirme a presença de um [RID (Identificador de Tempo de Execução)](/dotnet/core/rid-catalog) do Windows ou adicione-a ao `<PropertyGroup>` que contém a estrutura de destino:

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      Para publicar para vários RIDs:

      * Forneça os RIDs em uma lista delimitada por ponto e vírgula.
      * Use o nome da propriedade `<RuntimeIdentifiers>` (plural).

      Para obter mais informações, consulte [Catálogo de RID do .NET Core](/dotnet/core/rid-catalog).

   * Adicionar uma referência de pacote para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Faça as seguintes alterações em `Program.Main`:

   * Chame [host. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice), em vez de `host.Run`.

   * Chame [UseContentRoot](xref:fundamentals/host/web-host#content-root) e use um caminho para o local publicado do aplicativo, em vez de `Directory.GetCurrentDirectory()`.

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. Publique o aplicativo usando [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou o Visual Studio Code. Ao usar o Visual Studio, selecione o **FolderProfile** e configure o **Local de destino** antes de selecionar o botão **Publicar**.

   Para publicar o aplicativo de exemplo usando as ferramentas da CLI (interface de linha de comando), execute o comando [dotnet publish](/dotnet/core/tools/dotnet-publish) em um prompt de comando da pasta do projeto. O RID deve ser especificado na propriedade `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) do arquivo de projeto. No exemplo a seguir, o aplicativo é publicado na Configuração de versão para o tempo de execução `win7-x64` para uma pasta criada em *c:\\svc*:

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. Crie uma conta de usuário para o serviço usando o comando `net user`:

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   Para o aplicativo de exemplo, crie uma conta de usuário com o nome `ServiceUser` e uma senha. No comando a seguir, substitua `{PASSWORD}` por uma [senha forte](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   Se você precisar adicionar o usuário a um grupo, use o comando `net localgroup`, onde `{GROUP}` é o nome do grupo:

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   Para obter mais informações, confira [Contas de Usuário do Serviço](/windows/desktop/services/service-user-accounts).

1. Conceda acesso de gravação/leitura/execução para a pasta do aplicativo usando o comando [icacls](/windows-server/administration/windows-commands/icacls):

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}` &ndash; Caminho para a pasta do aplicativo.
   * `{USER ACCOUNT}` &ndash; A conta de usuário (SID).
   * `(OI)` &ndash; O sinalizador de Herança de Objeto propaga as permissão para os arquivos subordinados.
   * `(CI)` &ndash; O sinalizador de Herança de Contêiner propaga as permissão para as pastas subordinadas.
   * `{PERMISSION FLAGS}` &ndash; Define as permissões de acesso do aplicativo.
     * Gravar (`W`)
     * Ler (`R`)
     * Executar (`X`)
     * Completo (`F`)
     * Modificar (`M`)
   * `/t` &ndash; Aplique recursivamente aos arquivos e pastas subordinadas existentes.

   Para o aplicativo de exemplo publicado na pasta *c:\\svc* e a conta `ServiceUser` com permissões de gravação/leitura/execução, use o seguinte comando:

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   Para obter mais informações, confira [icacls](/windows-server/administration/windows-commands/icacls).

1. Use a ferramenta de linha de comando [sc.exe](https://technet.microsoft.com/library/bb490995) para criar o serviço. O valor `binPath` é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável. **É necessário o espaço entre o sinal de igual e o caractere de aspas de cada parâmetro e valor.**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; O nome a ser atribuído ao serviço no [Gerenciador de Controle de Serviço](/windows/desktop/services/service-control-manager).
   * `{PATH}` &ndash; O caminho para o executável do serviço.
   * `{DOMAIN}` (ou se o computador não estiver ingressado no domínio, o nome do computador local) e `{USER ACCOUNT}` &ndash; O domínio (ou o nome do computador local) e a conta de usuário nos quais o serviço é executado. **Não** omita o parâmetro `obj`. O valor padrão para `obj` é a [conta LocalSystem](/windows/desktop/services/localsystem-account). Executar um serviço na conta `LocalSystem` apresenta um risco de segurança significativo. Sempre execute um serviço em uma conta de usuário com privilégios restritos no servidor.
   * `{PASSWORD}` &ndash; A senhas da conta de usuário.

   No exemplo a seguir:

   * O nome do serviço é **MyService**.
   * O serviço publicado reside na pasta *c:\\svc*. O nome do executável de aplicativo é *AspNetCoreService.exe*. O valor `binPath` é colocado entre aspas retas (").
   * O serviço é executado na conta `ServiceUser`. Substitua `{DOMAIN}` com o domínio ou nome do computador local da conta de usuário. Coloque o valor `obj` entre aspas retas ("). Exemplo: se o sistema de hospedagem é uma máquina local denominada `MairaPC`, defina `obj` para `"MairaPC\ServiceUser"`.
   * Substitua `{PASSWORD}` com a senha da conta do usuário. O valor `password` é colocado entre aspas retas (").

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > Certifique-se de que os espaços entre os sinais de igual e os valores dos parâmetros estão presentes.

1. Inicie o serviço com o comando `sc start {SERVICE NAME}`.

   Para iniciar o serviço de aplicativo de exemplo, use o seguinte comando:

   ```console
   sc start MyService
   ```

   O comando leva alguns segundos para iniciar o serviço.

1. Para verificar o status do serviço, use o comando `sc query {SERVICE NAME}`. O status é relatado como um dos seguintes valores:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Use o seguinte comando para verificar o status do serviço de aplicativo de exemplo:

   ```console
   sc query MyService
   ```

1. Quando o serviço estiver no estado `RUNNING` e se o serviço for um aplicativo Web, procure o aplicativo em seu caminho (por padrão, `http://localhost:5000`, que redireciona para `https://localhost:5001` ao usar [Middleware de Redirecionamento HTTPS](xref:security/enforcing-ssl)).

   Para o serviço de aplicativo de exemplo, procure o aplicativo em `http://localhost:5000`.

1. Interrompa o serviço com o comando `sc stop {SERVICE NAME}`.

   O comando a seguir interrompe o serviço de aplicativo de exemplo:

   ```console
   sc stop MyService
   ```

1. Após um pequeno atraso para interromper um serviço, desinstale o serviço com o comando `sc delete {SERVICE NAME}`.

   Verifique o status do serviço de aplicativo de exemplo:

   ```console
   sc query MyService
   ```

   Quando o serviço de aplicativo de exemplo estiver no estado `STOPPED`, use o seguinte comando para desinstalar o serviço de aplicativo de exemplo:

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>Execute o aplicativo fora de um serviço

É mais fácil testar e depurar ao executar fora de um serviço, então é comum adicionar código que chama `RunAsService` apenas em determinadas condições. Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um argumento de linha de comando `--console` ou se o depurador está anexado:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Porque a configuração do ASP.NET Core requer pares nome-valor para argumentos de linha de comando, a opção `--console` é removida antes que os argumentos sejam passados para [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.

## <a name="handle-stopping-and-starting-events"></a>Manipular eventos de início e de parada

Para tratar eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), faça as seguintes alterações adicionais:

1. Crie uma classe que derive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Crie um método de extensão para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que passe o `WebHostService` personalizado para [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.

Se o código `WebHostService` personalizado exigir um serviço de injeção de dependência (como um agente), obtenha-o da propriedade [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de balanceador de carga

Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estão atrás de um proxy ou de um balanceador de carga podem exigir configuração adicional. Para obter mais informações, consulte <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configurar o HTTPS

Para configurar o serviço com um ponto de extremidade seguro:

1. Crie um certificado X.509 para o sistema de hospedagem usando os mecanismos de implantação e aquisição do certificado da sua plataforma.
1. Especifique uma [Configuração do ponto de extremidade HTTPS do servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar o certificado.

Não há suporte para uso do certificado de desenvolvimento HTTPS do ASP.NET Core para proteger um ponto de extremidade de serviço.

## <a name="current-directory-and-content-root"></a>Diretório atual e a raiz do conteúdo

O diretório de trabalho atual retornado ao chamar `Directory.GetCurrentDirectory()` de um serviço Windows é a pasta *C:\\WINDOWS\\system32*. A pasta *system32* não é um local adequado para armazenar os arquivos de um serviço (por exemplo, os arquivos de configurações). Use uma das seguintes abordagens para manter e acessar os arquivos de ativos e de configurações de um serviço com [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) ao usar um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* Use o caminho raiz do conteúdo. O `IHostingEnvironment.ContentRootPath` é o mesmo caminho fornecido para o argumento `binPath` quando o serviço é criado. Em vez de usar `Directory.GetCurrentDirectory()` para criar caminhos para arquivos de configurações, use o caminho raiz do conteúdo e mantenha os arquivos na raiz do conteúdo do aplicativo.
* Armazene os arquivos em um local adequado no disco. Especifique um caminho absoluto com `SetBasePath` para a pasta que contém os arquivos.

## <a name="additional-resources"></a>Recursos adicionais

* [Configuração do ponto de extremidade Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclui a configuração HTTPS e suporte à SNI)
* <xref:fundamentals/host/web-host>
