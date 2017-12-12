---
uid: whitepapers/denied-access-to-iis-directories
title: "ASP.NET negado acesso a diretórios IIS | Microsoft Docs"
author: rick-anderson
description: "Este white paper descreve o que você deve fazer uma solicitação para seu aplicativo ASP.NET retorna o erro \"acesso negado ao diretório DirectoryName. Falha ao s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a>Acesso negado para diretórios do IIS do ASP.NET
====================
> Este white paper descreve o que você deve fazer uma solicitação para seu aplicativo ASP.NET retorna o erro "acesso negado ao *DirectoryName* directory. Falha ao iniciar o monitoramento de diretório chaanges."
> 
> Aplica-se ao ASP.NET 1.0 e 1.1 do ASP.NET.


RTM do ASP.NET V1 agora é executada usando um menor com privilégios de conta do windows - registrada como a conta "ASPNET" em um computador local.

Em alguns bloqueados sistemas, essa conta pode não por padrão têm acesso de leitura segurança para diretórios de conteúdo do site, o diretório raiz do aplicativo ou o diretório raiz do site. Nesse caso, você receberá o seguinte erro ao solicitar páginas de um determinado aplicativo web:

![](denied-access-to-iis-directories/_static/image1.jpg)

Para corrigir isso, você precisará alterar as permissões de segurança nos diretórios apropriados.

Especificamente, o ASP.NET exige leitura, executar e lista de acesso para a conta ASPNET para a raiz do site da web (por exemplo: c:\inetpub\wwwroot ou qualquer diretório de site alternativo, talvez você tenha configurado no IIS), o diretório de conteúdo e o diretório raiz do aplicativo para monitorar as alterações de arquivo de configuração. A raiz do aplicativo corresponde ao caminho da pasta associado com o diretório virtual do aplicativo na ferramenta de administração do IIS (inetmgr).

Por exemplo, considere a seguinte hierarquia de aplicativo na pasta wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Neste exemplo, a conta ASPNET precisa de permissões de leitura definidas acima para o conteúdo de myapp e o diretório wwwroot. Uma ACL herdada único na pasta raiz também pode ser usada opcionalmente para ambos os diretórios se ele estão aninhados.

Para adicionar permissões para um diretório, execute as seguintes etapas:

- Usando o Windows explorer, navegue até o diretório
- Clique com o botão direito na pasta do diretório e escolha "Propriedades"
- Navegue até a guia "Segurança" na caixa de diálogo de propriedade
- Clique no botão "Adicionar" e insira o nome do computador seguido pelo nome da conta ASPNET. Por exemplo, em um computador chamado "webdev", você poderia insira webdev\ASPNET e ocorrências de "Okey".
- Verifique se a conta ASPNET tem de "leitura &amp; Execute", "Listar conteúdo da pasta" e "Leitura" caixas de seleção marcadas.
- Acertos Okey para fechar a caixa de diálogo e salvar as alterações.

![](denied-access-to-iis-directories/_static/image2.jpg)

Se desejar, essas alterações podem ser automatizadas usando scripts ou a ferramenta "cacls.exe" que é fornecido com o Windows. Para obter mais informações sobre a conta ASPNET, consulte o [documento perguntas frequentes sobre](https://go.microsoft.com/fwlink/?LinkId=5828).

Se um determinado aplicativo web depende de ter gravar ou modificar permissões para um arquivo ou pasta específica, isso pode receber o mesmo procedimento a seguir e marcando as caixas de seleção "Gravação" e/ou "Modificar".

Em computadores que permitem que todos, ou o acesso de leitura do grupo de usuários para esses diretórios (que é a configuração padrão), nenhum problema será encontrado e as etapas acima não será necessárias.
