# <a name="custom-webhost-service-sample"></a><span data-ttu-id="6773c-101">Exemplo de serviço WebHost personalizado</span><span class="sxs-lookup"><span data-stu-id="6773c-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="6773c-102">Este exemplo mostra como hospedar um aplicativo ASP.NET Core como um Serviço Windows sem usar IIS.</span><span class="sxs-lookup"><span data-stu-id="6773c-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="6773c-103">Este exemplo demonstra o cenário descrito em [Hospedar um aplicativo ASP.NET Core em um Serviço Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="6773c-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="6773c-104">Instruções</span><span class="sxs-lookup"><span data-stu-id="6773c-104">Instructions</span></span>

<span data-ttu-id="6773c-105">O aplicativo de exemplo é um aplicativo Web Razor Pages simples modificado de acordo com as instruções em [Hospedar um aplicativo ASP.NET Core em um Serviço Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="6773c-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="6773c-106">Para executar o aplicativo em um serviço, realize as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="6773c-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="6773c-107">Crie uma pasta em *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="6773c-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="6773c-108">Publicar o aplicativo na pasta com `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="6773c-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="6773c-109">O comando move os ativos do aplicativo para a pasta *svc*, incluindo o arquivo `appsettings.json` necessário e a pasta `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="6773c-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="6773c-110">Abra um prompt de comando de **administrador**.</span><span class="sxs-lookup"><span data-stu-id="6773c-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="6773c-111">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="6773c-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="6773c-112">*O espaço entre o sinal de igual e o início da cadeia de caracteres do caminho é necessário.*</span><span class="sxs-lookup"><span data-stu-id="6773c-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="6773c-113">Em um navegador, vá para `http://localhost:5000` e verifique se o serviço está em execução.</span><span class="sxs-lookup"><span data-stu-id="6773c-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="6773c-114">O aplicativo redireciona para o ponto de extremidade seguro `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6773c-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="6773c-115">Para interromper o serviço, use o comando:</span><span class="sxs-lookup"><span data-stu-id="6773c-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="6773c-116">Se o aplicativo não for iniciado conforme o esperado, uma maneira rápida de tornar as mensagens de erro acessíveis será adicionar um provedor de log, como o [provedor de EventLog do Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="6773c-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="6773c-117">Outra opção é verificar o Log de Eventos do Aplicativo usando o Visualizador de Eventos no sistema.</span><span class="sxs-lookup"><span data-stu-id="6773c-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="6773c-118">Por exemplo, aqui está uma exceção sem tratamento de um erro FileNotFound no Log de Eventos do Aplicativo:</span><span class="sxs-lookup"><span data-stu-id="6773c-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
