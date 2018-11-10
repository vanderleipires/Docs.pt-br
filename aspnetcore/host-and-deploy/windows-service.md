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
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="960f4-103">Hospedar o ASP.NET Core em um serviço Windows</span><span class="sxs-lookup"><span data-stu-id="960f4-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="960f4-104">Por [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="960f4-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="960f4-105">Um aplicativo ASP.NET Core pode ser hospedado no Windows sem usar o IIS como um [Serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="960f4-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="960f4-106">Quando hospedado como um Serviço Windows, o aplicativo é iniciado automaticamente após a reinicialização.</span><span class="sxs-lookup"><span data-stu-id="960f4-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="960f4-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([como baixar](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="960f4-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="960f4-108">Converter um projeto em um serviço Windows</span><span class="sxs-lookup"><span data-stu-id="960f4-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="960f4-109">As alterações mínimas a seguir são necessárias para configurar um projeto existente do ASP.NET Core a ser executado como um serviço:</span><span class="sxs-lookup"><span data-stu-id="960f4-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="960f4-110">No arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="960f4-110">In the project file:</span></span>

   * <span data-ttu-id="960f4-111">Confirme a presença de um [RID (Identificador de Tempo de Execução)](/dotnet/core/rid-catalog) do Windows ou adicione-a ao `<PropertyGroup>` que contém a estrutura de destino:</span><span class="sxs-lookup"><span data-stu-id="960f4-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="960f4-112">Para publicar para vários RIDs:</span><span class="sxs-lookup"><span data-stu-id="960f4-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="960f4-113">Forneça os RIDs em uma lista delimitada por ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="960f4-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="960f4-114">Use o nome da propriedade `<RuntimeIdentifiers>` (plural).</span><span class="sxs-lookup"><span data-stu-id="960f4-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="960f4-115">Para obter mais informações, consulte [Catálogo de RID do .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="960f4-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="960f4-116">Adicionar uma referência de pacote para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="960f4-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="960f4-117">Faça as seguintes alterações em `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="960f4-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="960f4-118">Chame [host. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice), em vez de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="960f4-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="960f4-119">Chame [UseContentRoot](xref:fundamentals/host/web-host#content-root) e use um caminho para o local publicado do aplicativo, em vez de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="960f4-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="960f4-120">Publique o aplicativo usando [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="960f4-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="960f4-121">Ao usar o Visual Studio, selecione o **FolderProfile** e configure o **Local de destino** antes de selecionar o botão **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="960f4-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="960f4-122">Para publicar o aplicativo de exemplo usando as ferramentas da CLI (interface de linha de comando), execute o comando [dotnet publish](/dotnet/core/tools/dotnet-publish) em um prompt de comando da pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="960f4-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="960f4-123">O RID deve ser especificado na propriedade `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) do arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="960f4-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="960f4-124">No exemplo a seguir, o aplicativo é publicado na Configuração de versão para o tempo de execução `win7-x64` para uma pasta criada em *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="960f4-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="960f4-125">Crie uma conta de usuário para o serviço usando o comando `net user`:</span><span class="sxs-lookup"><span data-stu-id="960f4-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="960f4-126">Para o aplicativo de exemplo, crie uma conta de usuário com o nome `ServiceUser` e uma senha.</span><span class="sxs-lookup"><span data-stu-id="960f4-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="960f4-127">No comando a seguir, substitua `{PASSWORD}` por uma [senha forte](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="960f4-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="960f4-128">Se você precisar adicionar o usuário a um grupo, use o comando `net localgroup`, onde `{GROUP}` é o nome do grupo:</span><span class="sxs-lookup"><span data-stu-id="960f4-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="960f4-129">Para obter mais informações, confira [Contas de Usuário do Serviço](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="960f4-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="960f4-130">Conceda acesso de gravação/leitura/execução para a pasta do aplicativo usando o comando [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="960f4-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="960f4-131">`{PATH}` &ndash; Caminho para a pasta do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="960f4-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="960f4-132">`{USER ACCOUNT}` &ndash; A conta de usuário (SID).</span><span class="sxs-lookup"><span data-stu-id="960f4-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="960f4-133">`(OI)` &ndash; O sinalizador de Herança de Objeto propaga as permissão para os arquivos subordinados.</span><span class="sxs-lookup"><span data-stu-id="960f4-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="960f4-134">`(CI)` &ndash; O sinalizador de Herança de Contêiner propaga as permissão para as pastas subordinadas.</span><span class="sxs-lookup"><span data-stu-id="960f4-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="960f4-135">`{PERMISSION FLAGS}` &ndash; Define as permissões de acesso do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="960f4-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="960f4-136">Gravar (`W`)</span><span class="sxs-lookup"><span data-stu-id="960f4-136">Write (`W`)</span></span>
     * <span data-ttu-id="960f4-137">Ler (`R`)</span><span class="sxs-lookup"><span data-stu-id="960f4-137">Read (`R`)</span></span>
     * <span data-ttu-id="960f4-138">Executar (`X`)</span><span class="sxs-lookup"><span data-stu-id="960f4-138">Execute (`X`)</span></span>
     * <span data-ttu-id="960f4-139">Completo (`F`)</span><span class="sxs-lookup"><span data-stu-id="960f4-139">Full (`F`)</span></span>
     * <span data-ttu-id="960f4-140">Modificar (`M`)</span><span class="sxs-lookup"><span data-stu-id="960f4-140">Modify (`M`)</span></span>
   * <span data-ttu-id="960f4-141">`/t` &ndash; Aplique recursivamente aos arquivos e pastas subordinadas existentes.</span><span class="sxs-lookup"><span data-stu-id="960f4-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="960f4-142">Para o aplicativo de exemplo publicado na pasta *c:\\svc* e a conta `ServiceUser` com permissões de gravação/leitura/execução, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="960f4-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="960f4-143">Para obter mais informações, confira [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="960f4-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="960f4-144">Use a ferramenta de linha de comando [sc.exe](https://technet.microsoft.com/library/bb490995) para criar o serviço.</span><span class="sxs-lookup"><span data-stu-id="960f4-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="960f4-145">O valor `binPath` é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável.</span><span class="sxs-lookup"><span data-stu-id="960f4-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="960f4-146">**É necessário o espaço entre o sinal de igual e o caractere de aspas de cada parâmetro e valor.**</span><span class="sxs-lookup"><span data-stu-id="960f4-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="960f4-147">`{SERVICE NAME}` &ndash; O nome a ser atribuído ao serviço no [Gerenciador de Controle de Serviço](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="960f4-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="960f4-148">`{PATH}` &ndash; O caminho para o executável do serviço.</span><span class="sxs-lookup"><span data-stu-id="960f4-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="960f4-149">`{DOMAIN}` (ou se o computador não estiver ingressado no domínio, o nome do computador local) e `{USER ACCOUNT}` &ndash; O domínio (ou o nome do computador local) e a conta de usuário nos quais o serviço é executado.</span><span class="sxs-lookup"><span data-stu-id="960f4-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="960f4-150">**Não** omita o parâmetro `obj`.</span><span class="sxs-lookup"><span data-stu-id="960f4-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="960f4-151">O valor padrão para `obj` é a [conta LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="960f4-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="960f4-152">Executar um serviço na conta `LocalSystem` apresenta um risco de segurança significativo.</span><span class="sxs-lookup"><span data-stu-id="960f4-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="960f4-153">Sempre execute um serviço em uma conta de usuário com privilégios restritos no servidor.</span><span class="sxs-lookup"><span data-stu-id="960f4-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="960f4-154">`{PASSWORD}` &ndash; A senhas da conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="960f4-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="960f4-155">No exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="960f4-155">In the following example:</span></span>

   * <span data-ttu-id="960f4-156">O nome do serviço é **MyService**.</span><span class="sxs-lookup"><span data-stu-id="960f4-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="960f4-157">O serviço publicado reside na pasta *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="960f4-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="960f4-158">O nome do executável de aplicativo é *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="960f4-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="960f4-159">O valor `binPath` é colocado entre aspas retas (").</span><span class="sxs-lookup"><span data-stu-id="960f4-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="960f4-160">O serviço é executado na conta `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="960f4-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="960f4-161">Substitua `{DOMAIN}` com o domínio ou nome do computador local da conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="960f4-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="960f4-162">Coloque o valor `obj` entre aspas retas (").</span><span class="sxs-lookup"><span data-stu-id="960f4-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="960f4-163">Exemplo: se o sistema de hospedagem é uma máquina local denominada `MairaPC`, defina `obj` para `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="960f4-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="960f4-164">Substitua `{PASSWORD}` com a senha da conta do usuário.</span><span class="sxs-lookup"><span data-stu-id="960f4-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="960f4-165">O valor `password` é colocado entre aspas retas (").</span><span class="sxs-lookup"><span data-stu-id="960f4-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="960f4-166">Certifique-se de que os espaços entre os sinais de igual e os valores dos parâmetros estão presentes.</span><span class="sxs-lookup"><span data-stu-id="960f4-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="960f4-167">Inicie o serviço com o comando `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="960f4-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="960f4-168">Para iniciar o serviço de aplicativo de exemplo, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="960f4-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="960f4-169">O comando leva alguns segundos para iniciar o serviço.</span><span class="sxs-lookup"><span data-stu-id="960f4-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="960f4-170">Para verificar o status do serviço, use o comando `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="960f4-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="960f4-171">O status é relatado como um dos seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="960f4-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="960f4-172">Use o seguinte comando para verificar o status do serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="960f4-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="960f4-173">Quando o serviço estiver no estado `RUNNING` e se o serviço for um aplicativo Web, procure o aplicativo em seu caminho (por padrão, `http://localhost:5000`, que redireciona para `https://localhost:5001` ao usar [Middleware de Redirecionamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="960f4-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="960f4-174">Para o serviço de aplicativo de exemplo, procure o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="960f4-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="960f4-175">Interrompa o serviço com o comando `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="960f4-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="960f4-176">O comando a seguir interrompe o serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="960f4-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="960f4-177">Após um pequeno atraso para interromper um serviço, desinstale o serviço com o comando `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="960f4-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="960f4-178">Verifique o status do serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="960f4-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="960f4-179">Quando o serviço de aplicativo de exemplo estiver no estado `STOPPED`, use o seguinte comando para desinstalar o serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="960f4-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="960f4-180">Execute o aplicativo fora de um serviço</span><span class="sxs-lookup"><span data-stu-id="960f4-180">Run the app outside of a service</span></span>

<span data-ttu-id="960f4-181">É mais fácil testar e depurar ao executar fora de um serviço, então é comum adicionar código que chama `RunAsService` apenas em determinadas condições.</span><span class="sxs-lookup"><span data-stu-id="960f4-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="960f4-182">Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um argumento de linha de comando `--console` ou se o depurador está anexado:</span><span class="sxs-lookup"><span data-stu-id="960f4-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="960f4-183">Porque a configuração do ASP.NET Core requer pares nome-valor para argumentos de linha de comando, a opção `--console` é removida antes que os argumentos sejam passados para [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="960f4-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="960f4-184">`isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="960f4-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="960f4-185">Manipular eventos de início e de parada</span><span class="sxs-lookup"><span data-stu-id="960f4-185">Handle stopping and starting events</span></span>

<span data-ttu-id="960f4-186">Para tratar eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), faça as seguintes alterações adicionais:</span><span class="sxs-lookup"><span data-stu-id="960f4-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="960f4-187">Crie uma classe que derive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="960f4-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="960f4-188">Crie um método de extensão para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que passe o `WebHostService` personalizado para [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="960f4-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="960f4-189">Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="960f4-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="960f4-190">`isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="960f4-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="960f4-191">Se o código `WebHostService` personalizado exigir um serviço de injeção de dependência (como um agente), obtenha-o da propriedade [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="960f4-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="960f4-192">Servidor proxy e cenários de balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="960f4-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="960f4-193">Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estão atrás de um proxy ou de um balanceador de carga podem exigir configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="960f4-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="960f4-194">Para obter mais informações, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="960f4-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="960f4-195">Configurar o HTTPS</span><span class="sxs-lookup"><span data-stu-id="960f4-195">Configure HTTPS</span></span>

<span data-ttu-id="960f4-196">Para configurar o serviço com um ponto de extremidade seguro:</span><span class="sxs-lookup"><span data-stu-id="960f4-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="960f4-197">Crie um certificado X.509 para o sistema de hospedagem usando os mecanismos de implantação e aquisição do certificado da sua plataforma.</span><span class="sxs-lookup"><span data-stu-id="960f4-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="960f4-198">Especifique uma [Configuração do ponto de extremidade HTTPS do servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar o certificado.</span><span class="sxs-lookup"><span data-stu-id="960f4-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="960f4-199">Não há suporte para uso do certificado de desenvolvimento HTTPS do ASP.NET Core para proteger um ponto de extremidade de serviço.</span><span class="sxs-lookup"><span data-stu-id="960f4-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="960f4-200">Diretório atual e a raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="960f4-200">Current directory and content root</span></span>

<span data-ttu-id="960f4-201">O diretório de trabalho atual retornado ao chamar `Directory.GetCurrentDirectory()` de um serviço Windows é a pasta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="960f4-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="960f4-202">A pasta *system32* não é um local adequado para armazenar os arquivos de um serviço (por exemplo, os arquivos de configurações).</span><span class="sxs-lookup"><span data-stu-id="960f4-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="960f4-203">Use uma das seguintes abordagens para manter e acessar os arquivos de ativos e de configurações de um serviço com [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) ao usar um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="960f4-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="960f4-204">Use o caminho raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="960f4-204">Use the content root path.</span></span> <span data-ttu-id="960f4-205">O `IHostingEnvironment.ContentRootPath` é o mesmo caminho fornecido para o argumento `binPath` quando o serviço é criado.</span><span class="sxs-lookup"><span data-stu-id="960f4-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="960f4-206">Em vez de usar `Directory.GetCurrentDirectory()` para criar caminhos para arquivos de configurações, use o caminho raiz do conteúdo e mantenha os arquivos na raiz do conteúdo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="960f4-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="960f4-207">Armazene os arquivos em um local adequado no disco.</span><span class="sxs-lookup"><span data-stu-id="960f4-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="960f4-208">Especifique um caminho absoluto com `SetBasePath` para a pasta que contém os arquivos.</span><span class="sxs-lookup"><span data-stu-id="960f4-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="960f4-209">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="960f4-209">Additional resources</span></span>

* <span data-ttu-id="960f4-210">[Configuração do ponto de extremidade Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclui a configuração HTTPS e suporte à SNI)</span><span class="sxs-lookup"><span data-stu-id="960f4-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
