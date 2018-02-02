# <a name="custom-webhost-service-sample"></a>Exemplo de serviço personalizado WebHost

Este exemplo mostra a maneira recomendada para hospedar um aplicativo ASP.NET Core no Windows sem usar o IIS como um serviço do Windows. Este exemplo demonstra os recursos descritos [hospedar um aplicativo ASP.NET Core em um serviço Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Instruções

O aplicativo de exemplo é um simple aplicativo de web MVC modificado de acordo com as instruções em [hospedar um aplicativo ASP.NET Core em um serviço Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Para executar o aplicativo em um serviço, execute as seguintes etapas:

1. Crie uma pasta no *c:\svc*.

1. Publicar o aplicativo para a pasta com `dotnet publish --configuration Release --output c:\\svc`. O comando moverá ativos do aplicativo para a pasta, incluindo o necessária `appsettings.json` arquivo e o `wwwroot` pasta com seu conteúdo.

1. Abra um **administrador** shell de comando.

1. Execute os seguintes comandos:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. Em um navegador, vá para `http://localhost:5000` para verificar se o serviço está em execução.

1. Para interromper o serviço, use o comando:

   ```console
   sc stop MyService
   ```

Se o aplicativo não for iniciado como o esperado quando em execução em um serviço, uma maneira rápida para disponibilizar as mensagens de erro é adicionar um provedor de log, como o [provedor de log de eventos do Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Outra opção é verificar o Log de eventos do aplicativo usando o Visualizador de eventos no sistema. Por exemplo, aqui está uma exceção sem tratamento de um erro de arquivo não encontrado no Log de eventos do aplicativo:

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
