---
title: "Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet"
author: rick-anderson
description: "Mostra como usar a inspeção dotnet."
keywords: "ASP.NET Core, usando a inspeção dotnet"
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 30e0d07bdfbd16a475e03c1a21cdd10220bd1630
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Desenvolvendo aplicativos ASP.NET Core usando a inspeção dotnet


Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` é uma ferramenta que executa um comando `dotnet` quando os arquivos de origem são alterados. Por exemplo, uma alteração de arquivo pode disparar uma compilação, testes ou uma implantação.

Neste tutorial, usamos um aplicativo de API Web existente com dois pontos de extremidade: um que retorna uma soma e outro que retorna um produto. O método de produto contém um bug que corrigiremos como parte deste tutorial.

Baixe o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Ele contém dois projetos, `WebApp` (um aplicativo Web) e `WebAppTests` (testes de unidade para o aplicativo Web).

Em um console, navegue para a pasta WebApp e execute os seguintes comandos:

- `dotnet restore`
- `dotnet run`

O resultado do console mostrará mensagens semelhantes à seguinte (indicando que o aplicativo está em execução e aguarda solicitações):

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Em um navegador da Web, navegue para `http://localhost:5000/api/math/sum?a=4&b=5`. Você deverá ver o resultado `9`.

Navegue para a API do produto (`http://localhost:5000/api/math/product?a=4&b=5`); ela retorna `9`, não `20`, conforme esperado. Corrigiremos isso mais adiante no tutorial.

## <a name="add-dotnet-watch-to-a-project"></a>Adicionar `dotnet watch` a um projeto

- Adicione `Microsoft.DotNet.Watcher.Tools` ao arquivo *.csproj*:
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="1.0.0" />
 </ItemGroup> 
 ```

- Execute `dotnet restore`.

## <a name="running-dotnet-commands-using-dotnet-watch"></a>Executando comandos `dotnet` usando `dotnet watch`

Qualquer comando `dotnet` pode ser executado com `dotnet watch`, por exemplo:

| Comando | Comando com inspeção |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f net451 | dotnet watch run -f net451 |
| dotnet run -f net451 -- --arg1 | dotnet watch run -f net451 -- --arg1 |
| dotnet test | dotnet watch test |

Execute `dotnet watch run` na pasta `WebApp`. O resultado do console indicará que `watch` foi iniciado.

## <a name="making-changes-with-dotnet-watch"></a>Fazer alterações com `dotnet watch`

Verifique se `dotnet watch` está em execução.

Corrija o bug no método `Product` do `MathController` para que ele retorne o produto e não a soma.

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Salve o arquivo. O resultado do console mostrará mensagens indicando que `dotnet watch` detectou uma alteração de arquivo e reiniciou o aplicativo.

Verifique se `http://localhost:5000/api/math/product?a=4&b=5` retorna o resultado correto.

## <a name="running-tests-using-dotnet-watch"></a>Executando testes usando `dotnet watch`

- Altere o método `Product` do `MathController` novamente para retornar a soma e salve o arquivo.
- Em uma janela de comando, navegue para a pasta `WebAppTests`.
- Execute `dotnet restore`
- Execute `dotnet watch test`. Veja o resultado indicando que um teste falhou e que o inspetor está aguardando as alterações de arquivo:

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- Corrija o código do método `Product` para que ele retorne o produto. Salve o arquivo.

`dotnet watch` detecta a alteração de arquivo e executa os testes novamente. O resultado do console mostrará a aprovação nos testes.

## <a name="dotnet-watch-in-github"></a>dotnet-watch no GitHub

dotnet-watch faz parte do [repositório DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) do GitHub.

A [seção MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) do [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explica como o dotnet-watch pode ser configurado por meio do arquivo de projeto MSBuild inspecionado. O [Leiame do dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contém informações sobre o dotnet-watch não abordadas neste tutorial.
