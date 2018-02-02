# <a name="custom-webhost-service-sample"></a><span data-ttu-id="3d7f3-101">Exemplo de serviço personalizado WebHost</span><span class="sxs-lookup"><span data-stu-id="3d7f3-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="3d7f3-102">Este exemplo mostra a maneira recomendada para hospedar um aplicativo ASP.NET Core no Windows sem usar o IIS como um serviço do Windows.</span><span class="sxs-lookup"><span data-stu-id="3d7f3-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="3d7f3-103">Este exemplo demonstra os recursos descritos [hospedar um aplicativo ASP.NET Core em um serviço Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="3d7f3-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="3d7f3-104">Instruções</span><span class="sxs-lookup"><span data-stu-id="3d7f3-104">Instructions</span></span>

<span data-ttu-id="3d7f3-105">O aplicativo de exemplo é um simple aplicativo de web MVC modificado de acordo com as instruções em [hospedar um aplicativo ASP.NET Core em um serviço Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="3d7f3-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="3d7f3-106">Para executar o aplicativo em um serviço, execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="3d7f3-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="3d7f3-107">Crie uma pasta no *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="3d7f3-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="3d7f3-108">Publicar o aplicativo para a pasta com `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="3d7f3-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="3d7f3-109">O comando moverá ativos do aplicativo para a pasta, incluindo o necessária `appsettings.json` arquivo e o `wwwroot` pasta com seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="3d7f3-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="3d7f3-110">Abra um **administrador** shell de comando.</span><span class="sxs-lookup"><span data-stu-id="3d7f3-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="3d7f3-111">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="3d7f3-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="3d7f3-112">Em um navegador, vá para `http://localhost:5000` para verificar se o serviço está em execução.</span><span class="sxs-lookup"><span data-stu-id="3d7f3-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="3d7f3-113">Para interromper o serviço, use o comando:</span><span class="sxs-lookup"><span data-stu-id="3d7f3-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="3d7f3-114">Se o aplicativo não for iniciado como o esperado quando em execução em um serviço, uma maneira rápida para disponibilizar as mensagens de erro é adicionar um provedor de log, como o [provedor de log de eventos do Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="3d7f3-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="3d7f3-115">Outra opção é verificar o Log de eventos do aplicativo usando o Visualizador de eventos no sistema.</span><span class="sxs-lookup"><span data-stu-id="3d7f3-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="3d7f3-116">Por exemplo, aqui está uma exceção sem tratamento de um erro de arquivo não encontrado no Log de eventos do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="3d7f3-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
