---
title: "Estrutura de diretórios do ASP.NET Core"
author: guardrex
description: "A estrutura de diretório de aplicativos publicados do ASP.NET Core."
keywords: "ASP.NET Core, a estrutura de diretórios"
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: f406d866bb1c8ac2d4371c8ddf669fc08af0fada
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Estrutura de diretório de aplicativos do ASP.NET Core publicados

Por [Luke Latham](https://github.com/guardrex)

No núcleo do ASP.NET, o diretório de aplicativo, *publicar*, é composta de arquivos de aplicativo, os arquivos de configuração, ativos estáticos, pacotes e o tempo de execução (para aplicativos independentes). Essa é a mesma estrutura de diretório que versões anteriores do ASP.NET, onde o aplicativo inteiro reside no diretório raiz da web.

| Tipo de aplicativo | Estrutura de diretórios |
| --- | --- |
| Dependente de estrutura de implantação | <ul><li>Publicar\*<ul><li>logs de\* (se for incluído em publishOptions)</li><li>refs\*</li><li>tempos de execução\*</li><li>Modos de exibição\* (se for incluído em publishOptions)</li><li>wwwroot\* (se for incluído em publishOptions)</li><li>Arquivos .dll</li><li>MyApp.DEPs.JSON</li><li>como Myapp.dll</li><li>MyApp.PDB</li><li>MyApp. PrecompiledViews.dll (se pré-compilando exibições Razor)</li><li>MyApp. PrecompiledViews.pdb (se pré-compilando exibições Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>Web. config (se for incluído em publishOptions)</li></ul></li></ul> |
| Independente de implantação | <ul><li>Publicar\*<ul><li>logs de\* (se for incluído em publishOptions)</li><li>refs\*</li><li>Modos de exibição\* (se for incluído em publishOptions)</li><li>wwwroot\* (se for incluído em publishOptions)</li><li>Arquivos .dll</li><li>MyApp.DEPs.JSON</li><li>MyApp.exe</li><li>MyApp.PDB</li><li>MyApp. PrecompiledViews.dll (se pré-compilando exibições Razor)</li><li>MyApp. PrecompiledViews.pdb (se pré-compilando exibições Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>Web. config (se for incluído em publishOptions)</li></ul></li></ul> |
\*Indica um diretório

O conteúdo do *publicar* diretório representa o *caminho raiz de conteúdo*, também chamado de *caminho base do aplicativo*, da implantação. O nome que é fornecido para o *publicar* serve de seu local de diretório na implantação, como a caminho físico do servidor para o aplicativo hospedado. O *wwwroot* diretório, se presente, contém ativos estáticos somente. O *logs* diretório pode ser incluído na implantação ao criá-lo no projeto e adicionar o `<Target>` elemento mostrado abaixo ao seu *. csproj* arquivo ou criando o diretório fisicamente no servidor.

```xml
<Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
  <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
  <MakeDir Directories="$(PublishUrl)Logs" Condition="!Exists('$(PublishUrl)Logs')" />
</Target>
```

A primeira `<MakeDir>` elemento, que usa o `PublishDir` propriedade, é usado pelo .NET Core CLI para determinar o local de destino para a operação de publicação. A segunda `<MakeDir>` elemento, que usa o `PublishUrl` propriedade, é usado pelo Visual Studio para determinar o local de destino. O Visual Studio usa o `PublishUrl` propriedade para compatibilidade com os projetos não .NET Core.

O diretório de implantação requer permissões de leitura/execução, enquanto o *logs* directory requer permissões de leitura/gravação. Diretórios adicionais onde ativos serão gravados exigem permissões de leitura/gravação.
