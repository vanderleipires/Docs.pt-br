# <a name="custom-webhost-service-sample"></a>Exemplo de serviço WebHost personalizado

Este exemplo mostra como hospedar um aplicativo ASP.NET Core como um Serviço Windows sem usar IIS. Este exemplo demonstra o cenário descrito em [Hospedar um aplicativo ASP.NET Core em um Serviço Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Instruções

O aplicativo de exemplo é um aplicativo Web Razor Pages simples modificado de acordo com as instruções em [Hospedar um aplicativo ASP.NET Core em um Serviço Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Para executar o aplicativo em um serviço, realize as seguintes etapas:

1. Crie uma pasta em *c:\svc*.

1. Publicar o aplicativo na pasta com `dotnet publish --configuration Release --output c:\\svc`. O comando move os ativos do aplicativo para a pasta *svc*, incluindo o arquivo `appsettings.json` necessário e a pasta `wwwroot`.

1. Abra um prompt de comando de **administrador**.

1. Execute os seguintes comandos:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *O espaço entre o sinal de igual e o início da cadeia de caracteres do caminho é necessário.*

1. Em um navegador, vá para `http://localhost:5000` e verifique se o serviço está em execução. O aplicativo redireciona para o ponto de extremidade seguro `https://localhost:5001`.

1. Para interromper o serviço, use o comando:

   ```console
   sc stop MyService
   ```

Se o aplicativo não for iniciado conforme o esperado, uma maneira rápida de tornar as mensagens de erro acessíveis será adicionar um provedor de log, como o [provedor de EventLog do Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Outra opção é verificar o Log de Eventos do Aplicativo usando o Visualizador de Eventos no sistema. Por exemplo, aqui está uma exceção sem tratamento de um erro FileNotFound no Log de Eventos do Aplicativo:

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
