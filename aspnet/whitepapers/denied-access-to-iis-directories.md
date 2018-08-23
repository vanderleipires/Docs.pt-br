---
uid: whitepapers/denied-access-to-iis-directories
title: Acesso negado do ASP.NET para IIS diretórios | Microsoft Docs
author: rick-anderson
description: Este white paper descreve o que você deve fazer uma solicitação para seu aplicativo ASP.NET retorna o erro "acesso negado ao diretório DirectoryName. Falha ao s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: c3a14f51df7aaf5c5935cf60ee4e687c10048e91
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831659"
---
<a name="aspnet-denied-access-to-iis-directories"></a>Acesso negado do ASP.NET a diretórios do IIS
====================
> Este white paper descreve o que você deve fazer uma solicitação para seu aplicativo ASP.NET retorna o erro "acesso negado ao *Nomedodiretório* directory. Falha ao iniciar o monitoramento de alterações de diretório".
> 
> Aplica-se para o ASP.NET 1.0 e ASP.NET 1.1.


ASP.NET V1 RTM agora é executada usando um menor com privilégios de conta do windows - registrada como a conta "ASPNET" em um computador local.

Em alguns bloqueado sistemas, essa conta pode não por padrão tem acesso de leitura segurança para diretórios de conteúdo de um site, o diretório raiz do aplicativo ou o diretório raiz do site da web. Nesse caso, você receberá o seguinte erro ao solicitar as páginas de um determinado aplicativo web:

![](denied-access-to-iis-directories/_static/image1.jpg)

Para corrigir esse problema, você precisará alterar as permissões de segurança nos diretórios apropriados.

Especificamente, o ASP.NET exige leitura, executar e lista de acesso para a conta ASPNET para a raiz do site da web (por exemplo: c:\inetpub\wwwroot ou em qualquer diretório de site alternativo que você pode ter configurado no IIS), o diretório de conteúdo e o diretório raiz do aplicativo para monitorar as alterações de arquivo de configuração. A raiz do aplicativo corresponde ao caminho da pasta associado com o diretório virtual do aplicativo na ferramenta de administração do IIS (inetmgr).

Por exemplo, considere a seguinte hierarquia de aplicativo sob a pasta wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Neste exemplo, a conta ASPNET precisa de permissões de leitura definidas acima para o conteúdo de myapp e o diretório wwwroot. Uma ACL herdada única na pasta raiz também pode ser usada opcionalmente para ambos os diretórios se eles estão aninhados.

Para adicionar permissões para um diretório, execute as seguintes etapas:

- Usando o Windows explorer, navegue até o diretório
- Clique com o botão direito na pasta de diretório e escolha "Propriedades"
- Navegue até a guia "Segurança" na caixa de diálogo de propriedade
- Clique no botão "Adicionar" e insira o nome do computador seguido pelo nome da conta ASPNET. Por exemplo, em um computador chamado "webdev", você seria insira webdev\ASPNET e pressione "Okey".
- Certifique-se de que a conta ASPNET tem a "leitura &amp; Execute", "Listar conteúdo da pasta" e "Leitura" caixas de seleção marcadas.
- Pressione Okey para fechar a caixa de diálogo e salvar as alterações.

![](denied-access-to-iis-directories/_static/image2.jpg)

Se desejado, essas alterações podem ser automatizadas usando scripts ou a ferramenta de "cacls.exe" que é fornecido com o Windows. Para obter mais informações sobre a conta ASPNET, consulte o [documentos de perguntas frequentes sobre](https://go.microsoft.com/fwlink/?LinkId=5828).

Se um determinado aplicativo web se baseia em ter a gravação ou modificar as permissões para um arquivo ou pasta em particular, isso pode ser concedido seguindo o mesmo procedimento e marcando as caixas de seleção de "Gravação" e/ou "Modificar".

Em computadores que permitem que todas as pessoas ou o acesso de leitura do grupo de usuários a esses diretórios (que é a configuração padrão), nenhum problema será encontrado e as etapas acima não será necessárias.
