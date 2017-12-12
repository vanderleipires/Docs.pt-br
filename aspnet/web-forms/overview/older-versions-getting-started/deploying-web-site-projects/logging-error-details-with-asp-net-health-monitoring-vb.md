---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: Registro em log os detalhes do erro com ASP.NET (VB) de monitoramento de integridade | Microsoft Docs
author: rick-anderson
description: "Sistema de monitoramento de integridade da Microsoft fornece uma maneira fácil e personalizável para vários eventos da web, incluindo exceções sem tratamento de log. Este tutorial orienta passag..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 6a1533b80828532b756940d0b08fe4c6dab2d5dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Detalhes de erro de log com o ASP.NET (VB) de monitoramento de integridade
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Sistema de monitoramento de integridade da Microsoft fornece uma maneira fácil e personalizável para vários eventos da web, incluindo exceções sem tratamento de log. Este tutorial orienta na configuração de sistema de monitoramento de integridade para registrar exceções não manipuladas para um banco de dados e para notificar os desenvolvedores por meio de uma mensagem de email.


## <a name="introduction"></a>Introdução

Registro em log é uma ferramenta útil para monitorar a integridade de um aplicativo implantado e para diagnosticar os problemas que podem surgir. É especialmente importante registrar erros que ocorrem em um aplicativo implantado, para que eles podem ser corrigidos. O `Error` é gerado sempre que uma exceção não tratada ocorre em um aplicativo ASP.NET; o [tutorial anterior](processing-unhandled-exceptions-vb.md) mostrou como notificar um desenvolvedor de um erro e seus detalhes de log com a criação de um manipulador de eventos para o `Error` eventos. No entanto, criando um `Error` manipulador de eventos para registrar em log os detalhes do erro e notificar um desenvolvedor é desnecessário, pois esta tarefa pode ser executada pelo ASP. Do NET *sistema de monitoramento de integridade*.

Sistema de monitoramento de integridade foi introduzida no ASP.NET 2.0 e foi projetada para monitorar a integridade de um aplicativo ASP.NET implantado pelo log de eventos que ocorrem durante o aplicativo ou o tempo de vida da solicitação. Os eventos registrados pelo sistema de monitoramento de integridade são denominados *eventos de monitoramento de integridade* ou *Web eventos*e incluem:

- Eventos de tempo de vida do aplicativo, como quando um aplicativo é iniciado ou interrompido
- Eventos de segurança, inclusive tentativas de logon e falha nas solicitações de autorização de URL
- Erros de aplicativo, incluindo sem tratamento de exceções, exceções, as exceções de validação de solicitação e erros de compilação, entre outros tipos de erros de análise de estado de exibição.

Quando um evento de monitoramento é gerado de integridade pode ser registrado para qualquer número de especificado *fontes de log*. Sistema de monitoramento de integridade é fornecido com fontes de log que o log de eventos da Web para um banco de dados do Microsoft SQL Server, o Log de eventos do Windows ou por meio de uma mensagem de email, entre outros. Você também pode criar suas próprias fontes de log.

Os eventos de logs de sistema de monitoramento de integridade, juntamente com as fontes de log usadas, são definidos em `Web.config`. Com algumas linhas de marcação de configuração, você pode usar para fazer todas as exceções sem tratamento para um banco de dados e para notificá-lo da exceção por email de monitoramento de integridade.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Explorando a configuração do sistema de monitoramento de integridade

Comportamento do sistema de monitoramento de integridade é definida por suas informações de configuração, que estão localizadas no [ `<healthMonitoring>` elemento](https://msdn.microsoft.com/en-us/library/2fwh2ss9.aspx) em `Web.config`. Esta seção de configuração define, entre outras coisas, as três partes importantes de informações a seguir:

1. Eventos de monitoramento de integridade que, quando disparado, deve ser registrado
2. As fontes de log, e
3. Como cada evento definido no (1) de monitoramento de integridade é mapeada para as fontes de log definido no (2).

Essa informação é especificada por meio de elementos de configuração de três filhos: [ `<eventMappings>` ](https://msdn.microsoft.com/en-us/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/en-us/library/zaa41kz1.aspx), e [ `<rules>` ](https://msdn.microsoft.com/en-us/library/fe5wyxa0.aspx), respectivamente.

Informações de configuração do sistema de monitoramento de integridade de padrão pode ser encontrada no `Web.config` arquivo `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` pasta. Essas informações de configuração padrão, com algumas marcações removida por questão de brevidade, são mostradas abaixo:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

A integridade do monitoramento de eventos de interesse são definidos no `<eventMappings>` elemento, que fornece um nome amigável a humanos em uma classe de eventos de monitoramento de integridade. Na marcação acima, o `<eventMappings>` elemento atribui o nome amigável a humanos "Todos os erros" para eventos de tipo de monitoramento de integridade `WebBaseErrorEvent` e o nome "auditorias de falha" para eventos de tipo de monitoramento de integridade `WebFailureAuditEvent`.

O `<providers>` elemento define as fontes de log, dando-lhes um nome amigável a humanos e especificando as informações de configuração de específico de fonte do log. A primeira `<add>` elemento define o provedor "EventLogProvider", que registra eventos por meio de monitoramento de integridade especificada o `EventLogWebEventProvider` classe. O `EventLogWebEventProvider` classe registra o evento no Log de eventos do Windows. A segunda `<add>` elemento define o provedor de "SqlWebEventProvider", que registra eventos de um banco de dados do Microsoft SQL Server por meio de `SqlWebEventProvider` classe. A configuração de "SqlWebEventProvider" Especifica a cadeia de caracteres de conexão do banco de dados (`connectionStringName`) entre as outras opções de configuração.

O `<rules>` os eventos especificados no elemento é mapeado a `<eventMappings>` elemento para entrar fontes a `<providers>` elemento. Por padrão, os aplicativos web ASP.NET registrar todas as exceções não manipuladas e falhas no Log de eventos do Windows.

## <a name="logging-events-to-a-database"></a>Eventos de log para um banco de dados

A configuração padrão do sistema de monitoramento de integridade pode ser personalizada em uma base de aplicativo web pelo aplicativo web, adicionando um `<healthMonitoring>` seção para o aplicativo `Web.config` arquivo. Você pode incluir elementos adicionais no `<eventMappings>`, `<providers>`, e `<rules>` seções usando o `<add>` elemento. Para remover uma configuração do uso de configuração padrão de `<remove>` elemento ou use `<clear />` para remover todos os valores padrão de uma dessas seções. Vamos configurar o aplicativo web de revisões de livros para registrar exceções sem tratamento todas as para um banco de dados do Microsoft SQL Server usando o `SqlWebEventProvider` classe.

O `SqlWebEventProvider` classe faz parte do sistema de monitoramento de integridade e registra uma evento para um banco de dados do SQL Server especificado de monitoramento de integridade. O `SqlWebEventProvider` classe espera que o banco de dados especificado inclui um procedimento armazenado denominado `aspnet_WebEvent_LogEvent`. Esse procedimento armazenado é passado os detalhes do evento e é a tarefa de armazenar os detalhes do evento. A boa notícia é que você não precisa criar esse procedimento nem a tabela para armazenar os detalhes do evento. Você pode adicionar esses objetos para o banco de dados usando o `aspnet_regsql.exe` ferramenta.

> [!NOTE]
> O `aspnet_regsql.exe` ferramenta foi discutida no [ *Configurando um site que usa serviços de aplicativos* tutorial](configuring-a-website-that-uses-application-services-vb.md) quando adicionamos suporte para ASP. Serviços de aplicativo do NET. Consequentemente, o banco de dados do site revisões de livros já contém o `aspnet_WebEvent_LogEvent` procedimento armazenado, que armazena as informações de evento em uma tabela nomeada `aspnet_WebEvent_Events`.


Uma vez que o procedimento armazenado necessário e a tabela adicionada ao banco de dados, tudo o que falta é instruir para registrar em log todas as exceções sem tratamento no banco de dados de monitoramento de integridade. Fazer isso, adicione a seguinte marcação para o seu site `Web.config` arquivo:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Marcação de configuração acima usa de monitoramento de integridade `<clear />` elementos para apagar as informações de configuração de monitoramento de integridade predefinida a `<eventMappings>`, `<providers>`, e `<rules>` seções. Em seguida, ele adiciona uma única entrada para cada uma dessas seções.

- O `<eventMappings>` elemento define uma único evento de interesse denominado "Todos os erros", que é gerado sempre que ocorre uma exceção sem tratamento de monitoramento de integridade.
- O `<providers>` elemento define uma fonte de logon único, chamada "SqlWebEventProvider" que usa o `SqlWebEventProvider` classe. O `connectionStringName` atributo foi definido como "ReviewsConnectionString", que é o nome do nosso conexão cadeia de caracteres definida no `<connectionStrings>` seção.
- Por fim, o &lt;regras&gt; elemento indica que quando um evento de "Todos os erros" ocorre que ele deve ser registrado usando o provedor "SqlWebEventProvider".

Essas informações de configuração instrui a sistema para registrar em log todas as exceções sem tratamento no banco de dados de catálogo revisões de monitoramento de integridade.

> [!NOTE]
> O `WebBaseErrorEvent` evento é gerado apenas para erros de servidor; ele não é gerado para erros HTTP, como uma solicitação para um recurso do ASP.NET que não foi encontrado. Isso é diferente do comportamento do `HttpApplication` da classe `Error` evento, que é gerado para erros HTTP e de servidor.


Para ver a sistema em ação de monitoramento de integridade, visite o site e gerar um erro de tempo de execução visitando `Genre.aspx?ID=foo`. Você verá a página de erro apropriado - a exceção detalhes amarelo tela de morte (quando visitar localmente) ou a página de erro personalizada (quando visitar o site de produção). Nos bastidores, o sistema de monitoramento de integridade registradas as informações de erro para o banco de dados. Deve haver um registro de `aspnet_WebEvent_Events` tabela (consulte **Figura 1**); este registro contém informações sobre o erro de tempo de execução que ocorreu.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Figura 1**: os detalhes do erro foram registrados para o `aspnet_WebEvent_Events` tabela  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Exibindo o Log de erros em uma página da Web

Com a configuração atual do site, o sistema de monitoramento de integridade registra todas as exceções sem tratamento no banco de dados. No entanto, o monitoramento de integridade não fornece qualquer mecanismo para exibir o log de erros por meio de uma página da web. No entanto, você pode criar uma página ASP.NET que exibe essas informações do banco de dados. (Conforme veremos momentaneamente, você pode optar por ter os detalhes do erro enviados a você em uma mensagem de email.)

Se você criar esse tipo de página, verifique se que você adotar medidas para permitir que somente usuários autorizados exibir os detalhes do erro. Se seu site já utiliza as contas de usuário, em seguida, você pode usar regras de autorização de URL para restringir o acesso para a página para determinados usuários ou funções. Para obter mais informações sobre como conceder ou restringir o acesso a páginas da web com base no usuário conectado, consulte Meus [tutoriais de segurança de site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> O tutorial subsequente explora um sistema de notificação e registro em log de erro alternativas denominado ELMAH. ELMAH inclui um mecanismo interno para exibir o log de erros de ambos os uma página da web e um RSS feed.


## <a name="logging-events-to-e-mail"></a>Log de eventos para email

A integridade do sistema de monitoramento inclui um provedor de origem de log que "registra" um evento para uma mensagem de email. A fonte de log inclui as mesmas informações que estão conectadas ao banco de dados no corpo da mensagem de email. Você pode usar esta fonte de log para notificar um desenvolvedor quando ocorre um determinado evento de monitoramento de integridade.

Vamos atualizar as revisões de catálogo de configuração do site para que recebemos um email sempre que uma exceção ocorre. Para fazer isso, precisamos executar três tarefas:

1. Configure o aplicativo da web ASP.NET para enviar email. Isso é feito especificando como os emails são enviados por meio de `<system.net>` elemento de configuração. Para obter mais informações sobre o envio de email mensagens em um aplicativo ASP.NET consulte [enviar o Email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) e [mail perguntas frequentes sobre](http://systemnetmail.com/).
2. Registrar o provedor de origem do log de email no `<providers>` elemento, e
3. Adicione uma entrada para o `<rules>` elemento que mapeia o evento de "Todos os erros" para o provedor de origem do log adicionado na etapa (2).

Sistema de monitoramento de integridade inclui duas classes de provedor de origem para log email: `SimpleMailWebEventProvider` e `TemplatedMailWebEventProvider`. O [ `SimpleMailWebEventProvider` classe](https://msdn.microsoft.com/en-us/library/system.web.management.simplemailwebeventprovider.aspx) envia uma mensagem de email de texto simples que inclui o evento de detalhes e fornece pouca personalização do corpo do email. Com o [ `TemplatedMailWebEventProvider` classe](https://msdn.microsoft.com/en-us/library/system.web.management.templatedmailwebeventprovider.aspx) você especificar uma página ASP.NET cuja marcação renderizada é usada como o corpo da mensagem de email. O [ `TemplatedMailWebEventProvider` classe](https://msdn.microsoft.com/en-us/library/system.web.management.templatedmailwebeventprovider.aspx) lhe dá muito maior controle sobre o conteúdo e o formato da mensagem de email, mas exige um pouco mais trabalho inicial já que você precisa criar a página ASP.NET que gera o corpo da mensagem de email. Este tutorial se concentra no uso de `SimpleMailWebEventProvider` classe.

Atualização do sistema de monitoramento de integridade `<providers>` elemento o `Web.config` arquivo para incluir uma fonte de log para o `SimpleMailWebEventProvider` classe:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

A marcação acima usa o `SimpleMailWebEventProvider` classe como o provedor de origem do log e atribui o nome amigável "EmailWebEventProvider". Além disso, o `<add>` atributo inclui opções de configuração adicionais, como para e de endereços de email.

Com a fonte de log de email definida, tudo o que falta é instruir a sistema para usar essa fonte para "Registrar" exceções sem tratamento de monitoramento de integridade. Isso é feito adicionando uma nova regra no `<rules>` seção:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

O `<rules>` seção agora inclui duas regras. O primeiro deles, chamado de "Todos os erros de email", envia todas as exceções sem tratamento para a fonte de log "EmailWebEventProvider". Essa regra tem o efeito de enviar detalhes sobre erros no site especificado para o endereço. A regra "Todos os erros no banco de dados" registra os detalhes do erro para o banco de dados do site. Consequentemente, sempre que ocorre uma exceção sem tratamento no site de seus detalhes são ambos conectadas ao banco de dados e enviadas ao endereço de email especificado.

**Figura 2** mostra o email gerado pelo `SimpleMailWebEventProvider` classe ao visitar `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Figura 2**: os detalhes do erro são enviadas em uma mensagem de email  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Resumo

O sistema de monitoramento de integridade do ASP.NET é projetado para permitir que os administradores a monitorar a integridade de um aplicativo da web implantados. Eventos de monitoramento de integridade são gerados quando Desdobrar determinadas ações, como quando o aplicativo for interrompida, quando um usuário fizer para o site, ou quando ocorre uma exceção sem tratamento. Esses eventos podem ser registrados em qualquer número de fontes de log. Este tutorial mostrou como registrar os detalhes de exceções sem tratamento para um banco de dados e por meio de uma mensagem de email.

Este tutorial voltada para usar para registrar exceções não manipuladas, mas tenha em mente que o monitoramento de integridade é projetado para medir a integridade geral de um aplicativo implantado do ASP.NET e inclui uma grande quantidade de eventos de monitoramento de integridade e fontes de log não de monitoramento de integridade explorados aqui. O que é mais, você pode criar sua próprias fontes de log e eventos de monitoramento de integridade necessidade surgir. Se você estiver interessado em saber mais sobre o monitoramento de integridade, um bom começo é ler [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)do [perguntas Frequentes de monitoramento de integridade](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Depois disso, consulte [como: Use monitoramento de integridade no ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998306.aspx).

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Visão geral do monitoramento de integridade do ASP.NET](https://msdn.microsoft.com/en-us/library/bb398933.aspx)
- [Configurar e personalizar a sistema do ASP.NET de monitoramento de integridade](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Perguntas Frequentes – no ASP.NET 2.0 de monitoramento de integridade](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Como: Enviar email para notificações de monitoramento de integridade](https://msdn.microsoft.com/en-us/library/ms227553.aspx)
- [Como: Usar o monitoramento de integridade no ASP.NET](https://msdn.microsoft.com/en-us/library/ms998306.aspx)
- [Integridade de monitoramento no ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

>[!div class="step-by-step"]
[Anterior](processing-unhandled-exceptions-vb.md)
[Próximo](logging-error-details-with-elmah-vb.md)
