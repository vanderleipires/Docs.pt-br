---
uid: whitepapers/ms03-32-issue
title: Correção para o erro de ''servidor aplicativo não está disponível após a aplicação de atualização de segurança para o IE | Microsoft Docs
author: rick-anderson
description: Este documento descreve o patch que corrige um problema com a atualização de segurança MS03-32 para o Internet Explorer que afeta os aplicativos do ASP.NET 1.0 em execução no Wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 186ed7ea7ad9b317506fb3951a974682b44b27a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402464"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Correção de erro 'Aplicativo de servidor não disponível' após a aplicação de atualização de segurança para o IE
====================
> Este documento descreve o patch que corrige um problema com a atualização de segurança MS03-32 para o Internet Explorer que afeta os aplicativos do ASP.NET 1.0 em execução no Windows XP Professional.
> 
> Aplica-se para o ASP.NET 1.0 e o Windows XP Professional.


A Microsoft identificou um problema com a atualização de segurança MS03-32 para o patch de segurança do Internet Explorer e o ASP.NET 1.0 em execução no Windows XP. Esse patch pode ser instalado manualmente ou por meio da obtenção de atualizações críticas recentes do site do Windows Update.

O sintoma desse problema é que, depois de instalar o patch em um computador com Windows XP, todas as solicitações para aplicativos ASP.NET em execução no servidor web local IIS 5.1 resultam em uma mensagem de erro dizendo "Servidor de aplicativo indisponível". As solicitações para servidores web remotos não são afetadas.

Esse problema só afeta as instalações estiverem executando o ASP.NET 1.0 no Windows XP. Isso não afetará as máquinas que executam o Windows 2000 ou Windows Server 2003. Ele também não afeta máquinas que executam o Windows XP com o ASP.NET 1.1 instalado.

Observe que esse problema **não é** um bug de segurança com o ASP.NET. Ele **não** abrir ou permitir ataques maliciosos contra um aplicativo ASP.NET ou servidor. Em vez disso, ele é puramente um bug funcional causado pelo patch em si.

Estamos trabalhando em uma solução permanente para esse problema. Enquanto isso, você pode executar o seguinte arquivo em lotes como uma solução alternativa para o problema. O arquivo em lote faz o seguinte:

1. Para os serviços de estado do IIS e ASP.NET
2. Exclui e recria a conta ASPNET com uma senha temporária conhecida
3. Usa o Windows `runas` comando para iniciar um executável que cria um perfil de usuário ASPNET
4. Registra novamente o ASP.NET. Isso cria uma nova senha aleatória para a conta e aplica as configurações de controle de acesso padrão ASP.NET para ele
5. Reinicia o serviço IIS

O arquivo de lote contém uma senha temporária de embutidos em código de "<strong>1pass@word</strong>" que você será solicitado a inserir para o comando executar quando o arquivo em lotes é executado. Depois de concluir o comando runas, a senha da conta ASPNET é recriada com um valor aleatório. Observe que o arquivo em lotes poderá falhar se a senha codificada não atende aos requisitos de complexidade de senha em seu ambiente. Se esse for o caso, você pode alterá-lo para outro valor que seja apropriado para seu ambiente.

*> [!IMPORTANT]* Se você tiver adicionado as configurações de controle de acesso personalizadas ou permissões de conta de banco de dados para a conta ASPNET, precisará ser recriado depois que esse arquivo em lotes for concluído. Isso ocorre porque quando a conta for recriada, ele obterá um novo identificador de segurança (SID).

*> [!IMPORTANT]* Se você estiver executando o processo de trabalho do ASP.NET com uma conta personalizada que não seja a conta ASPNET, em seguida, você não deve executar esse arquivo em lotes. Em vez disso, você deve fazer logon interativamente ou use o comando runas com uma conta que criará um perfil de usuário para essa conta.

O arquivo em lotes está incluído no arquivo de extração automática abaixo. Para usá-lo:

1. Você deve estar executando como uma conta com privilégios de administrador
2. [Baixe e abra o arquivo executável auto-extraível](ms03-32-issue/_static/fixup1.exe)
3. Extraia o conteúdo para o c:\
4. Selecione Executar... no menu Iniciar e digite `cmd.exe`
5. Nas janelas de comando aberta, digite `c:\fixup.cmd`.
6. Quando solicitado, insira <strong>1pass@word</strong> como a senha.
7. Se você tiver permissões de conta de banco de dados para a conta ASPNET ou configurações de controle de acesso personalizado anteriormente, você precisará aplicar novamente essas configurações agora.

Muitas desculpas pelo inconveniente que isso causou. Publicaremos informações adicionais conforme é disponibilizada.

A matriz a seguir detalha as plataformas e versões afetadas por esse problema.

| .NET Framework | Plataforma | Afetados |
| --- | --- | --- |
| Versão 1.0 | Windows 2000 Professional | Não |
| Versão 1.0 | Windows 2000 Server | Não |
| Versão 1.0 | Windows XP Professional | Sim |
| Versão 1.0 | Windows Server 2003 | Não |
| Versão 1.0 | Página inicial do Windows XP com o Cassini | Não |
| Versão 1.1 | Windows 2000 Professional | Não |
| Versão 1.1 | Windows 2000 Server | Não |
| Versão 1.1 | Windows XP Professional | Não |
| Versão 1.1 | Windows Server 2003 | Não |
| Versão 1.1 | Página inicial do Windows XP com o Cassini | Não |

Obrigado,   
 A equipe do ASP.NET
