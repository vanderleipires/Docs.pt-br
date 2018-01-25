---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: "Processamento de exceções sem tratamento (c#) | Microsoft Docs"
author: rick-anderson
description: "Quando ocorre um erro de tempo de execução em um aplicativo web na produção é importante para notificar um desenvolvedor e registre o erro, de forma que pode ser diagnosticado em um la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 95102e5e6b3e8b78e2757a2bdee39976003011e3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="processing-unhandled-exceptions-c"></a>Processamento de exceções sem tratamento (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_12_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial12_ErrorHandling_cs.pdf)

> Quando ocorre um erro de tempo de execução em um aplicativo web na produção é importante para notificar um desenvolvedor e registrar o erro para que ele pode ser diagnosticado posteriormente no tempo. Este tutorial fornece uma visão geral de como o ASP.NET processa os erros de tempo de execução e examina uma maneira de código personalizado executado sempre que um bolhas de exceção sem tratamento até o tempo de execução do ASP.NET.


## <a name="introduction"></a>Introdução

Quando ocorre uma exceção sem tratamento em um aplicativo ASP.NET, ele bolhas até o tempo de execução do ASP.NET, que gera o `Error` eventos e exibe a página de erro apropriado. Há três tipos diferentes de páginas de erro: o tempo de execução erro amarelo tela de morte (YSOD); os detalhes da exceção YSOD; e páginas de erro personalizadas. No [tutorial anterior](displaying-a-custom-error-page-cs.md) configuramos o aplicativo para usar uma página de erro personalizada para usuários remotos e a YSOD de detalhes de exceção para os usuários que visitam localmente.

Usando uma página de erro personalizada humanos amigável que corresponda a aparência do site é preferencial para o padrão YSOD de erro de tempo de execução, mas a exibição de uma página de erro personalizada é apenas uma parte de uma abrangente solução de tratamento de erros. Quando ocorre um erro em um aplicativo em produção, é importante que os desenvolvedores notificados sobre o erro para que eles possam revelação a causa da exceção e abordá-lo. Além disso, os detalhes do erro devem ser registrados para que o erro pode ser examinado e diagnosticado posteriormente no tempo.

Este tutorial mostra como acessar os detalhes de uma exceção sem tratamento para que eles podem ser registradas em log e um desenvolvedor notificado. Os dois tutoriais seguindo este exploram as bibliotecas de log de erros que, após um pouco de configuração, serão automaticamente notificar os desenvolvedores de erros de tempo de execução e seus detalhes de log.

> [!NOTE]
> As informações examinadas neste tutorial são mais útil se você precisar processar exceções não manipuladas de alguma maneira exclusiva ou personalizada. Em casos em que você só precisa fazer a exceção e notificar um desenvolvedor, usando uma biblioteca de registro em log de erros é a melhor opção. Os próximos dois tutoriais fornecem uma visão geral dos dois essas bibliotecas.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Executar código quando o`Error`é gerado

Eventos fornecem um mecanismo para sinalizar que algo interessante ocorreu e para outro objeto executar código em resposta de um objeto. Como desenvolvedor ASP.NET, você está acostumado a pensar em termos de eventos. Se você deseja executar um código quando o visitante clica em um botão específico, você criar um manipulador de eventos para esse botão `Click` eventos e coloque seu código existe. Considerando que o tempo de execução do ASP.NET gera seu [ `Error` evento](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) sempre que uma exceção não tratada ocorre, ele segue o código para registrar os detalhes do erro entrarão em um manipulador de eventos. Mas como você cria um manipulador de eventos para o `Error` evento?

O `Error` evento é um dos muitos eventos no [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) que são gerados em determinados estágios no pipeline HTTP durante o tempo de vida de uma solicitação. Por exemplo, o `HttpApplication` da classe [ `BeginRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) é gerado no início de cada solicitação; seu [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) é gerado quando um módulo de segurança identificou o solicitante. Essas `HttpApplication` eventos oferecem ao desenvolvedor de página um meio para executar lógica personalizada em vários pontos no tempo de vida de uma solicitação.

Manipuladores de eventos para o `HttpApplication` eventos podem ser colocados em um arquivo especial chamado `Global.asax`. Para criar esse arquivo no seu site, adicionar um novo item para a raiz do seu site usando o modelo de classe de aplicativo Global com o nome `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Figura 1**: adicionar `Global.asax` ao seu aplicativo Web  
([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-cs/_static/image3.png))

O conteúdo e a estrutura do `Global.asax` arquivo criado pelo Visual Studio diferem um pouco dependendo de se você estiver usando um projeto de aplicativo da Web (WAP) ou o projeto de Site da Web (WSP). Com um WAP, o `Global.asax` é implementado como dois arquivos separados - `Global.asax` e `Global.asax.cs`. O `Global.asax` arquivo contém apenas um `@Application` diretiva que faz referência a `.cs` arquivo; o evento manipuladores de interesse são definidos no `Global.asax.cs` arquivo. Para WSPs, um único arquivo é criado, `Global.asax`, e os manipuladores de eventos são definidos em um `<script runat="server">` bloco.

O `Global.asax` arquivo criado em um WAP pelo modelo de classe de aplicativo Global do Visual Studio inclui os manipuladores de eventos denominados `Application_BeginRequest`, `Application_AuthenticateRequest`, e `Application_Error`, que são manipuladores de eventos para o `HttpApplication` eventos `BeginRequest`, `AuthenticateRequest`, e `Error`, respectivamente. Também há manipuladores de eventos denominados `Application_Start`, `Session_Start`, `Application_End`, e `Session_End`, que são os manipuladores de eventos acionados quando o aplicativo web é iniciado, quando inicia uma nova sessão, quando o aplicativo termina, e quando uma sessão termina, respectivamente. O `Global.asax` arquivo criado em um WSP pelo Visual Studio contém apenas o `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, e `Session_End` manipuladores de eventos.

> [!NOTE]
> Ao implantar o aplicativo ASP.NET, você precisará copiar o `Global.asax` arquivo para o ambiente de produção. O `Global.asax.cs` arquivo, que é criado no WAP, não precisa ser copiada para produção porque esse código é compilado no assembly do projeto.


Os manipuladores de eventos criados pelo modelo de classe de aplicativo Global do Visual Studio não são exaustivos. Você pode adicionar um manipulador de eventos para qualquer `HttpApplication` evento nomeando o manipulador de eventos `Application_EventName`. Por exemplo, você pode adicionar o código a seguir para o `Global.asax` para criar um manipulador de eventos para o [ `AuthorizeRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-cs/samples/sample1.vb)]

Da mesma forma, você pode remover qualquer manipuladores de eventos criados pelo modelo de classe de aplicativo Global que não são necessários. Para este tutorial é necessário apenas um manipulador de eventos para o `Error` evento; fique à vontade para remover outros manipuladores de evento do `Global.asax` arquivo.

> [!NOTE]
> *Módulos HTTP* oferecem outra maneira de definir manipuladores de eventos para `HttpApplication` eventos. Módulos HTTP são criados como um arquivo de classe que pode ser colocado diretamente dentro do projeto de aplicativo web ou separado em uma biblioteca de classe separada. Porque eles podem ser separados em uma biblioteca de classes, módulos HTTP oferecem um modelo mais flexível e reutilizável para criar `HttpApplication` manipuladores de eventos. Enquanto o `Global.asax` arquivo é específico ao aplicativo da web onde ele reside, módulos HTTP podem ser compilados em assemblies, no ponto em que adicionar o módulo HTTP a um site é tão simple quanto soltando o assembly no `Bin` pasta e registrando o Módulo do `Web.config`. Este tutorial não examinar criando e usando módulos de HTTP, mas as bibliotecas de log de erros de dois usadas em dois tutoriais a seguir são implementadas como módulos HTTP. Para obter mais informações sobre os benefícios de módulos HTTP consulte [usando módulos e manipuladores HTTP para criar componentes de ASP.NET conectáveis](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Recuperando informações sobre a exceção não tratada

Neste ponto, temos um arquivo global. asax com um `Application_Error` manipulador de eventos. Quando executa este manipulador de eventos que precisamos notificar um desenvolvedor do erro e seus detalhes de log. Para realizar essas tarefas, que primeiro é preciso determinar os detalhes da exceção que foi gerado. Usar o objeto de servidor [ `GetLastError` método](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) para recuperar os detalhes da exceção sem tratamento que causou o `Error` evento seja acionado.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

O `GetLastError` método retorna um objeto do tipo `Exception`, que é o tipo base para todas as exceções do .NET Framework. No entanto, no código acima, estou converter o objeto de exceção retornado por `GetLastError` em uma `HttpException` objeto. Se o `Error` evento está sendo disparado porque ocorreu uma exceção durante o processamento de um recurso do ASP.NET e a exceção foi lançada é encapsulada dentro de um `HttpException`. Para obter a exceção real que originou a necessidade do uso de eventos de erro do `InnerException` propriedade. Se o `Error` evento foi gerado devido a uma exceção com base em HTTP, como uma solicitação para uma página inexistente, um `HttpException` for lançada, mas ele não tem uma exceção interna.

O código a seguir usa o `GetLastErrormessage` para recuperar informações sobre a exceção que disparou o `Error` evento, armazenando o `HttpException` em uma variável chamada `lastErrorWrapper`. Em seguida, armazena o tipo, a mensagem e o rastreamento de pilha de exceção de origem em três variáveis de cadeia de caracteres, para verificar se o `lastErrorWrapper` é a exceção real que disparou o `Error` evento (no caso de exceções com base em HTTP) ou se ele é simplesmente um wrapper para uma exceção foi acionada ao processar a solicitação.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

Neste ponto, você tem todas as informações que você precisa para escrever código que registrará em log os detalhes da exceção para uma tabela de banco de dados. Você pode criar uma tabela de banco de dados com colunas para cada um dos detalhes do erro de interesse - o tipo, a mensagem, o rastreamento de pilha e assim por diante - juntamente com outras partes úteis de informação, como a URL da página solicitada e o nome do usuário conectado no momento. No `Application_Error` manipulador de eventos, você deve se conectar ao banco de dados e inserir um registro na tabela. Da mesma forma, você pode adicionar código para um desenvolvedor do erro por meio de email de alerta.

As bibliotecas de log de erro examinadas nos próximos dois tutoriais fornecem funcionalidade pronta, portanto não é necessário para criar este registro de erro e notificação por conta própria. No entanto, para ilustrar que o `Error` está sendo gerado e que o `Application_Error` manipulador de eventos pode ser usado para registrar em log os detalhes de erro e notificar um desenvolvedor, vamos adicionar o código que notifica um desenvolvedor quando ocorre um erro.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notificando um desenvolvedor quando ocorre uma exceção sem tratamento

Quando ocorre uma exceção sem tratamento no ambiente de produção é importante alertar a equipe de desenvolvimento para que eles possam avaliar o erro e determinar quais ações precisam ser tomadas. Por exemplo, se houver um erro ao conectar-se ao banco de dados, você precisará duplo Verifique sua cadeia de caracteres de conexão e, talvez, abra um tíquete de suporte com a empresa de hospedagem. Se a exceção ocorreu devido a um erro de programação, talvez seja código adicional ou lógica de validação a ser adicionado para evitar esses erros no futuro.

Classes do .NET Framework a [ `System.Net.Mail` namespace](https://msdn.microsoft.com/library/system.net.mail.aspx) facilitam enviar um email. O [ `MailMessage` classe](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) representa uma mensagem de email e tem propriedades como `To`, `From`, `Subject`, `Body`, e `Attachments`. O `SmtpClass` é usada para enviar um `MailMessage` objeto usando um servidor SMTP especificado; as configurações do servidor SMTP podem ser especificadas de forma programática ou declarativamente no [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx) no `Web.config file`. Para obter mais informações sobre o envio de email mensagens em um aplicativo ASP.NET Confira o artigo [enviar o Email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)e o [mail perguntas frequentes sobre](http://systemnetmail.com/).

> [!NOTE]
> O `<system.net>` elemento contém as configurações do servidor SMTP usadas pelo `SmtpClient` classe ao enviar um email. Hospedagem da empresa provavelmente tem um servidor SMTP que você pode usar para enviar email do seu aplicativo. Consulte a seção de suporte do host da web para obter informações sobre configurações do servidor SMTP, que você deve usar em seu aplicativo da web.


Adicione o seguinte código para o `Application_Error` manipulador de eventos para enviar um email de um desenvolvedor quando ocorre um erro:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Enquanto o código acima é muito longo, a maior parte do mesmo cria o HTML que aparece no email enviado para o desenvolvedor. O código começa consultando o `HttpException` retornado pelo `GetLastError` método (`lastErrorWrapper`). A exceção que foi gerada pela solicitação realmente é recuperada por meio de `lastErrorWrapper.InnerException` e é atribuído à variável `lastError`. O tipo, a mensagem e a pilha de informações de rastreamento são recuperadas do `lastError` e armazenados em três variáveis de cadeia de caracteres.

Em seguida, um `MailMessage` objeto chamado `mm` é criado. O corpo do email é formatado em HTML e exibe a URL da página solicitada, o nome do usuário conectado no momento e informações sobre a exceção (o tipo de mensagem e rastreamento de pilha). Uma das coisas legais sobre o `HttpException` classe é que você pode gerar o HTML usado para criar a exceção detalhes amarelo tela de morte (YSOD) ao chamar o [GetHtmlErrorMessage método](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Esse método é usado aqui para recuperar a marcação de YSOD de detalhes da exceção e adicione-o como um anexo de email. Cuidado: se a exceção que disparou o `Error` evento foi uma exceção com base em HTTP (como uma solicitação para uma página inexistente) o `GetHtmlErrorMessage` método retornará `null`.

A etapa final é enviar o `MailMessage`. Isso é feito criando um novo `SmtpClient` método e chamar sua `Send` método.

> [!NOTE]
> Antes de usar esse código em seu aplicativo web você vai querer alterar os valores no `ToAddress` e `FromAddress` constantes de support@example.com para qualquer endereço de email o email de notificação de erro devem ser enviadas para e originam. Você também precisará especificar configurações do servidor SMTP no `<system.net>` seção `Web.config`. Consulte seu provedor de host da web para determinar as configurações do servidor SMTP a ser usado.


Com esse código no local a qualquer momento, há um erro ao desenvolvedor é enviado uma mensagem de email que resume o erro e inclui o YSOD. No tutorial anterior demonstramos um erro de tempo de execução visitando Genre.aspx e passando inválido `ID` valor por meio de querystring, como `Genre.aspx?ID=foo`. Visitar a página com o `Global.asax` arquivo no local produz a mesma experiência de usuário, como no tutorial anterior - no ambiente de desenvolvimento você continuará a ver a exceção detalhes amarelo tela de morte, enquanto no ambiente de produção, você vai Consulte a página de erro personalizada. Além desse comportamento existente, o desenvolvedor é enviado um email.

**Figura 2** mostra o email recebido ao visitar `Genre.aspx?ID=foo`. O corpo do email resume as informações de exceção, enquanto o `YSOD.htm` anexo exibe o conteúdo que é mostrado na YSOD de detalhes de exceção (consulte **Figura 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Figura 2**: O desenvolvedor é enviado uma notificação por email sempre que houver uma exceção sem tratamento  
([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Figura 3**: A notificação de email inclui os detalhes da exceção YSOD como um anexo  
([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>E sobre como usar a página de erro personalizado?

Este tutorial mostrou como usar `Global.asax` e `Application_Error` manipulador de eventos para executar código quando ocorre uma exceção sem tratamento. Especificamente, usamos este manipulador de eventos para notificar um desenvolvedor de um erro; Poderíamos estender para também registrar em log os detalhes do erro em um banco de dados. A presença de `Application_Error` manipulador de eventos não afeta a experiência do usuário final. Eles ainda consulte a página de erro configurado, seja ele o YSOD de detalhes do erro, o YSOD de erro de tempo de execução ou a página de erro personalizada.

É natural que se preocupar se o `Global.asax` arquivo e `Application_Error` evento é necessário ao usar uma página de erro personalizada. Quando ocorre um erro de usuário é mostrado a página de erro personalizada então por que não é possível é colocar o código para notificar o desenvolvedor e os detalhes do erro de log para a classe code-behind da página de erro personalizada? Enquanto você certamente pode adicionar código à classe de code-behind da página de erro personalizada você não tem acesso para os detalhes da exceção que disparou o `Error` evento ao usar a técnica é explorados no tutorial anterior. Chamando o `GetLastError` método da página de erro personalizada retorna `Nothing`.

O motivo para esse comportamento é porque a página de erro personalizada é atingida por meio de um redirecionamento. Quando uma exceção sem tratamento atinge o tempo de execução do ASP.NET o mecanismo ASP.NET gera seu `Error` evento (que executa o `Application_Error` manipulador de eventos) e, em seguida, *redireciona* o usuário para a página de erro personalizada emitindo um `Response.Redirect(customErrorPageUrl)`. O `Response.Redirect` método envia uma resposta ao cliente com um código de status HTTP 302 instrui o navegador a solicitar uma nova URL, ou seja, a página de erro personalizada. O navegador solicita automaticamente essa nova página. Você pode informar que a página de erro personalizada foi solicitada separadamente da página em que o erro foi originado porque a barra de endereços do navegador muda para a URL da página de erro personalizado (consulte **Figura 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Figura 4**: quando ocorre um erro o navegador é redirecionado para a URL da página de erro personalizada  
([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-cs/_static/image12.png))

O efeito líquido é que a solicitação onde ocorreu a exceção sem tratamento termina quando o servidor responde com o redirecionamento HTTP 302. A solicitação subsequente para a página de erro personalizada é uma nova solicitação; Neste ponto, o ASP.NET mecanismo rejeitou as informações de erro e, além disso, não tem como associar a exceção não tratada na solicitação anterior com a nova solicitação para a página de erro personalizada. É por isso que `GetLastError` retorna `null` quando chamado a partir da página de erro personalizada.

No entanto, é possível fazer com que a página de erro personalizado executada durante a mesma solicitação que causou o erro. O [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) método transfere a execução para a URL especificada e o processa na mesma solicitação. Você pode mover o código `Application_Error` manipulador de eventos para classe de code-behind da página de erro personalizada, substituí-lo no `Global.asax` com o código a seguir:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

Agora quando ocorre uma exceção sem tratamento do `Application_Error` manipulador de eventos transfere o controle para a página de erro personalizado apropriado com base no código de status HTTP. Como o controle foi transferido, a página de erro personalizada tem acesso às informações de exceção sem tratamento via `Server.GetLastError` e podem notificar um desenvolvedor do erro e seus detalhes de log. O `Server.Transfer` chamada interrompe o mecanismo do ASP.NET de redirecionar o usuário para a página de erro personalizada. Em vez disso, o conteúdo da página de erro personalizada é retornado como a resposta para a página que gerou o erro.

## <a name="summary"></a>Resumo

Quando ocorre uma exceção sem tratamento em um aplicativo web ASP.NET o tempo de execução do ASP.NET gera o `Error` eventos e exibe a página de erro configurado. Podemos pode notificar o desenvolvedor do erro, seus detalhes de log ou processá-lo de alguma outra forma, criando um manipulador de eventos para o evento de erro. Há duas maneiras de criar um manipulador de eventos `HttpApplication` eventos `Error`: no `Global.asax` arquivo ou de um módulo HTTP. Este tutorial mostrou como criar um `Error` manipulador de eventos de `Global.asax` arquivo que notifica os desenvolvedores de erro por meio de uma mensagem de email.

Criando um `Error` manipulador de eventos é útil se você precisar processar exceções não manipuladas de alguma maneira exclusiva ou personalizada. No entanto, criar seu próprio `Error` manipulador de eventos para registrar a exceção ou notificar um desenvolvedor não é o uso mais eficiente do tempo, pois já existem bibliotecas de log de erro gratuito e fácil de usar que podem ser configurado em questão de minutos. Os próximos dois tutoriais examinam duas essas bibliotecas.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Módulos HTTP do ASP.NET e visão geral de manipuladores HTTP](https://support.microsoft.com/kb/307985)
- [Respondendo normalmente para exceções não tratadas - processar exceções não tratadas](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`Classe e o objeto de aplicativo do ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Manipuladores HTTP e módulos HTTP no ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Enviar o Email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Noções básicas sobre o `Global.asax` arquivo](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Usando módulos HTTP e manipuladores para criar componentes ASP.NET conectáveis](https://msdn.microsoft.com/library/aa479332.aspx)
- [Trabalhando com o ASP.NET `Global.asax` arquivo](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Trabalhando com `HttpApplication` instâncias](https://msdn.microsoft.com/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[Anterior](displaying-a-custom-error-page-cs.md)
[Próximo](logging-error-details-with-asp-net-health-monitoring-cs.md)
