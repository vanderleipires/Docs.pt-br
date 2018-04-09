---
uid: whitepapers/ms03-32-issue
title: Correção de erro 'Aplicativo de servidor não disponível' depois de aplicar a atualização de segurança do IE | Microsoft Docs
author: rick-anderson
description: Este documento descreve o patch que corrige um problema com a atualização de segurança MS03-32 para o Internet Explorer que afeta os aplicativos ASP.NET 1.0 em execução no Wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: dd1a564cd347364abc3ca5ac0a9ffda448bcede8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Corrigir para 'Aplicativo de servidor indisponível' erro depois de aplicar a atualização de segurança do IE
====================
> Este documento descreve o patch que corrige um problema com a atualização de segurança MS03-32 para o Internet Explorer que afeta os aplicativos ASP.NET 1.0 em execução no Windows XP Professional.
> 
> Aplica-se ao Windows XP Professional e ASP.NET 1.0.


A Microsoft identificou um problema com a atualização de segurança MS03-32 para o patch de segurança do Internet Explorer e o ASP.NET 1.0 em execução no Windows XP. Esse patch pode ser instalado manualmente ou por meio de atualizações críticas recentes do site do Windows Update.

O sintoma desse problema é que, depois de instalar o patch em um computador Windows XP, todas as solicitações para aplicativos ASP.NET em execução no servidor web local do IIS 5.1 resultam em uma mensagem de erro dizendo "Servidor de aplicativo não está disponível". Solicitações para servidores web remoto são afetadas.

Esse problema só afeta as instalações estiverem executando o ASP.NET 1.0 no Windows XP. Ele não afeta as máquinas que executam o Windows 2000 ou Windows Server 2003. Ele também não afetar máquinas que executam o Windows XP com o ASP.NET 1.1 instalado.

Observe que esse problema **não** um bug de segurança com o ASP.NET. Ele **não** abrir ou permitir ataques maliciosos em relação a um aplicativo ASP.NET ou servidor. Em vez disso, é meramente um bug funcional causado pelo patch em si.

Estamos trabalhando duro em uma solução permanente para este problema. Enquanto isso, você pode executar o seguinte arquivo de lote como uma solução alternativa para o problema. O arquivo em lote faz o seguinte:

1. Para os serviços de estado do IIS e ASP.NET
2. Exclui e recria a conta ASPNET com uma senha temporária conhecida
3. Usa o Windows `runas` comando para iniciar um executável que cria um perfil de usuário ASPNET
4. Registra novamente o ASP.NET. Isso cria uma nova senha aleatória para a conta e aplica as configurações de controle de acesso padrão ASP.NET para ele
5. Reinicia o serviço IIS

O arquivo de lote contém uma senha temporária codificado de "<strong>1pass@word</strong>" que você será solicitado a inserir para o comando executar quando o arquivo em lotes é executado. Depois de concluir o comando runas, a senha da conta ASPNET é recriada com um valor aleatório. Observe que o arquivo em lote pode falhar se a senha codificada não atende aos requisitos de complexidade de senha em seu ambiente. Se esse for o caso, você pode alterá-lo para outro valor que seja apropriado para seu ambiente.

*> [!IMPORTANT]* Se você tiver adicionado as configurações de controle de acesso personalizadas ou as permissões de conta do banco de dados para a conta ASPNET, precisam ser recriados após esse arquivo em lotes. Isso ocorre porque quando a conta for recriada, ele receberá um novo identificador de segurança (SID).

*> [!IMPORTANT]* Se você estiver executando o processo de trabalho do ASP.NET com uma conta personalizada diferente da conta ASPNET, em seguida, você não deve executar esse arquivo em lotes. Em vez disso, você deve fazer logon interativamente no ou usar o comando Executar como com uma conta que criará um perfil de usuário para essa conta.

O arquivo em lotes está incluído no arquivo de extração automática abaixo. Para usá-lo:

1. Você deve estar executando como uma conta com privilégios de administrador
2. [Baixe e abra o arquivo executável auto-extraível](ms03-32-issue/_static/fixup1.exe)
3. Extraia o conteúdo para c:\
4. Selecione Executar... no menu Iniciar e digite `cmd.exe`
5. Na janela comando Abrir, digite `c:\fixup.cmd`.
6. Quando solicitado, insira <strong>1pass@word</strong> como a senha.
7. Se você tiver permissões de conta do banco de dados para a conta ASPNET ou de configurações de controle de acesso personalizado anteriormente, você precisará aplicar novamente essas configurações agora.

Muitas desculpas pelo inconveniente que isso causou. Publicaremos informações adicionais quando estiver disponível.

A matriz abaixo fornece detalhes sobre plataformas e versões afetadas por esse problema.

| .NET Framework | Plataforma | Afetado |
| --- | --- | --- |
| Versão 1.0 | Windows 2000 Professional | Não |
| Versão 1.0 | Windows 2000 Server | Não |
| Versão 1.0 | Windows XP Professional | Sim |
| Versão 1.0 | Windows Server 2003 | Não |
| Versão 1.0 | Página inicial do Windows XP com Cassini | Não |
| Versão 1.1 | Windows 2000 Professional | Não |
| Versão 1.1 | Windows 2000 Server | Não |
| Versão 1.1 | Windows XP Professional | Não |
| Versão 1.1 | Windows Server 2003 | Não |
| Versão 1.1 | Página inicial do Windows XP com Cassini | Não |

Obrigado,   
 A equipe do ASP.NET
