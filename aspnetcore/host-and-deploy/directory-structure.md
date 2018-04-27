---
title: Estrutura de diretórios do ASP.NET Core
author: guardrex
description: Saiba mais sobre a estrutura do diretório de aplicativos publicados do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ac9b777bcc7f4a8634161fc1347a4d0fdc3b4784
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/19/2018
---
# <a name="aspnet-core-directory-structure"></a>Estrutura de diretórios do ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

No núcleo do ASP.NET, o diretório de aplicativo publicado, *publicar*, é composta de arquivos de aplicativo, os arquivos de configuração, ativos estáticos, pacotes e o tempo de execução (para [implantações autossuficientes](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Tipo de aplicativo | Estrutura de diretórios |
| -------- | ------------------- |
| [Dependente de estrutura de implantação](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Publicar&dagger;<ul><li>logs&dagger; (opcional, a menos que necessário para receber os logs de stdout)</li><li>Modos de exibição&dagger; (aplicativos MVC; se os modos de exibição não pré-compilado)</li><li>Páginas&dagger; (MVC ou páginas Razor aplicativos; se páginas não pré-compilado)</li><li>wwwroot&dagger;</li><li>*\.arquivos DLL</li><li>\<nome do assembly >. deps.json</li><li>\<nome do assembly >. dll</li><li>\<nome do assembly >. PDB</li><li>\<nome do assembly >. PrecompiledViews.dll</li><li>\<nome do assembly >. PrecompiledViews.pdb</li><li>\<nome do assembly >. runtimeconfig.json</li><li>Web. config (as implantações do IIS)</li></ul></li></ul> |
| [Independente de implantação](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Publicar&dagger;<ul><li>logs&dagger; (opcional, a menos que necessário para receber os logs de stdout)</li><li>refs&dagger;</li><li>Modos de exibição&dagger; (aplicativos MVC; se os modos de exibição não pré-compilado)</li><li>Páginas&dagger; (MVC ou páginas Razor aplicativos; se páginas não pré-compilado)</li><li>wwwroot&dagger;</li><li>\*arquivos. dll</li><li>\<nome do assembly >. deps.json</li><li>\<nome do assembly > .exe</li><li>\<nome do assembly >. PDB</li><li>\<nome do assembly >. PrecompiledViews.dll</li><li>\<nome do assembly >. PrecompiledViews.pdb</li><li>\<nome do assembly >. runtimeconfig.json</li><li>Web. config (as implantações do IIS)</li></ul></li></ul> |

&dagger;Indica um diretório

O *publicar* diretório representa o *caminho raiz de conteúdo*, também chamado de *caminho base do aplicativo*, da implantação. O nome que é fornecido para o *publicar* serve de seu local de diretório do aplicativo implantado no servidor, como a caminho físico do servidor para o aplicativo hospedado.

O *wwwroot* diretório, se presente, contém ativos estáticos somente.

Stdout *logs* diretório pode ser criado a implantação usando uma das duas abordagens a seguir:

* Adicione o seguinte `<Target>` elemento ao arquivo de projeto:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   O `<MakeDir>` elemento cria vazio *Logs* pasta na saída publicada. O elemento usa o `PublishDir` propriedade para determinar o local de destino para criar a pasta. Vários métodos de implantação, como a implantação da Web, ignore as pastas vazias durante a implantação. O `<WriteLinesToFile>` elemento gera um arquivo no *Logs* pasta, o que garante a implantação da pasta para o servidor. Observe que a criação de pasta ainda pode falhar se o processo do operador não tem acesso de gravação para a pasta de destino.

* Criar fisicamente o *Logs* diretório no servidor na implantação.

O diretório de implantação requer permissões de leitura/execução. O *Logs* directory requer permissões de leitura/gravação. Diretórios adicionais onde os arquivos são gravados exigem permissões de leitura/gravação.
