# <a name="aspnet-background-tasks-sample-generic-host"></a>Exemplo de tarefas em segundo plano do ASP.NET (Host Genérico)

Este exemplo ilustra o uso de [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). Este exemplo demonstra os recursos descritos no tópico [Tarefas em segundo plano com serviços hospedados no ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services).

Ao executar o exemplo no [Visual Studio Code](https://code.visualstudio.com/), defina o valor **console** da configuração de console em *.vscode/launch.json* como `externalTerminal` ou `integratedTerminal`. Usar o `internalConsole` é incompatível com a entrada por pressionamento de tecla do console que o aplicativo usa para enfileirar itens de trabalho em segundo plano.
