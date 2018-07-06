---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: Processamento de exceções sem tratamento (c#) | Microsoft Docs
author: rick-anderson
description: Quando ocorre um erro de tempo de execução em um aplicativo web em produção é importante para notificar um desenvolvedor e para registrar o erro, de modo que podem ser diagnosticado na la...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 07272a10ac9b1ddf3afd6b089b05a3f071834efe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832623"
---
<a name="processing-unhandled-exceptions-c"></a>Processamento de exceções sem tratamento (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs/samples) ([como baixar](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Quando ocorre um erro de tempo de execução em um aplicativo web em produção é importante para notificar um desenvolvedor e para registrar o erro, de modo que podem ser diagnosticado em um momento posterior no tempo. Este tutorial fornece uma visão geral de como o ASP.NET processa os erros de tempo de execução e examina uma maneira de ter o código personalizado executado sempre que um bolhas de exceção sem tratamento até o tempo de execução do ASP.NET.


## <a name="introduction"></a>Introdução

Quando ocorre uma exceção sem tratamento em um aplicativo ASP.NET, ele propaga-se até o tempo de execução do ASP.NET, que gera o `Error` eventos e exibe a página de erro apropriada. Há três tipos diferentes de páginas de erro: o tempo de execução erro amarelo tela de morte (YSOD); os detalhes da exceção YSOD; e páginas de erro personalizadas. No [tutorial anterior](displaying-a-custom-error-page-cs.md) configuramos o aplicativo para usar uma página de erro personalizada para usuários remotos e o YSOD de detalhes de exceção para usuários que visitam localmente.

Usando uma página de erro personalizado de amigável a humanos que corresponda a aparência do site é preferencial para o padrão YSOD de erro de tempo de execução, mas exibindo uma página de erro personalizada é apenas uma parte de uma solução de tratamento de erros abrangente. Quando ocorre um erro em um aplicativo em produção, é importante que os desenvolvedores são notificados sobre o erro para que eles podem revelar a causa da exceção e tratá-lo. Além disso, os detalhes do erro devem ser registrados para que o erro pode ser examinado e diagnosticado em um momento posterior no tempo.

Este tutorial mostra como acessar os detalhes de uma exceção sem tratamento para que eles podem ser registradas em log e um desenvolvedor notificado. Os dois tutoriais seguindo este exploram bibliotecas de log de erro que, após um pouco de configuração, serão automaticamente notificam os desenvolvedores de erros de tempo de execução e seus detalhes de log.

> [!NOTE]
> As informações examinadas neste tutorial são mais útil se você precisar processar exceções não manipuladas de alguma maneira exclusiva ou personalizada. Em casos em que você só precisa registrar a exceção e notificar um desenvolvedor, usando uma biblioteca de registro em log de erro é a melhor opção. Os próximos dois tutoriais fornecem uma visão geral dos dois essas bibliotecas.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Executar código quando o`Error`é gerado

Eventos fornecem um mecanismo para sinalizar que algo interessante ocorreu e para outro objeto para executar código em resposta de um objeto. Como desenvolvedor ASP.NET, você está acostumado a pensar em termos de eventos. Se você quiser executar um código quando o visitante clica em um botão específico, crie um manipulador de eventos para esse botão `Click` eventos e colocar seu código lá. Considerando que o tempo de execução do ASP.NET gera sua [ `Error` evento](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) sempre que ocorrer uma exceção sem tratamento, ele segue o código para registrar em log os detalhes do erro iria em um manipulador de eventos. Mas como você cria um manipulador de eventos para o `Error` evento?

O `Error` evento é um dos muitos eventos em de [ `HttpApplication` classe](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) que são gerados em determinados estágios no pipeline HTTP durante o tempo de vida de uma solicitação. Por exemplo, o `HttpApplication` da classe [ `BeginRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) é gerado no início de cada solicitação; seu [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) é gerado quando um módulo de segurança identificou o solicitante. Eles `HttpApplication` eventos oferecem ao desenvolvedor da página um meio para executar a lógica personalizada em vários pontos no tempo de vida de uma solicitação.

Manipuladores de eventos para o `HttpApplication` eventos podem ser colocados em um arquivo especial chamado `Global.asax`. Para criar esse arquivo no seu site, adicione um novo item para a raiz do seu site usando o modelo de classe de aplicativo Global com o nome `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Figura 1**: adicionar `Global.asax` ao seu aplicativo Web  
([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-cs/_static/image3.png))

O conteúdo e a estrutura do `Global.asax` arquivo criado pelo Visual Studio pode diferir um pouco dependendo se você estiver usando um projeto de aplicativo da Web (WAP) ou um projeto de Site da Web (WSP). Com um WAP, o `Global.asax` é implementada como dois arquivos separados - `Global.asax` e `Global.asax.cs`. O `Global.asax` arquivo não contém nada, mas uma `@Application` diretiva que faz referência a `.cs` arquivo; o evento de manipuladores de interesse são definidos no `Global.asax.cs` arquivo. Para WSPs, apenas um único arquivo é criado, `Global.asax`, e os manipuladores de eventos são definidos em um `<script runat="server">` bloco.

O `Global.asax` arquivo criado em um WAP pelo modelo de classe de aplicativo Global do Visual Studio inclui manipuladores de eventos nomeados `Application_BeginRequest`, `Application_AuthenticateRequest`, e `Application_Error`, que são os manipuladores de eventos para o `HttpApplication` eventos `BeginRequest`, `AuthenticateRequest`, e `Error`, respectivamente. Também há manipuladores de eventos nomeados `Application_Start`, `Session_Start`, `Application_End`, e `Session_End`, que são os manipuladores de eventos que são acionados quando o aplicativo web é iniciado, quando começa uma nova sessão, quando o aplicativo termina, e quando uma sessão é encerrada, respectivamente. O `Global.asax` arquivo criado em um WSP pelo Visual Studio contém apenas o `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, e `Session_End` manipuladores de eventos.

> [!NOTE]
> Ao implantar o aplicativo ASP.NET, você precisa copiar o `Global.asax` arquivo para o ambiente de produção. O `Global.asax.cs` arquivo, que é criado no WAP, não precisam ser copiados para a produção porque esse código é compilado no assembly do projeto.


Os manipuladores de eventos criados pelo modelo de classe de aplicativo Global do Visual Studio não são exaustivos. Você pode adicionar um manipulador de eventos para qualquer `HttpApplication` evento ao nomear o manipulador de eventos `Application_EventName`. Por exemplo, você poderia adicionar o código a seguir para o `Global.asax` arquivo para criar um manipulador de eventos para o [ `AuthorizeRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-cs[Main](processing-unhandled-exceptions-cs/samples/sample1.cs)]

Da mesma forma, você pode remover quaisquer manipuladores de eventos criados pelo modelo de classe de aplicativo Global que não são necessários. Para este tutorial apenas exigimos um manipulador de eventos para o `Error` evento; sinta-se livre para remover os outros manipuladores de eventos do `Global.asax` arquivo.

> [!NOTE]
> *Módulos HTTP* oferecem outra maneira de definir manipuladores de eventos para `HttpApplication` eventos. Módulos HTTP são criados como um arquivo de classe que pode ser colocado diretamente dentro do projeto de aplicativo web ou separado em uma biblioteca de classe separada. Porque eles podem ser separados em uma biblioteca de classes, módulos HTTP oferecem um modelo mais flexível e reutilizável para a criação de `HttpApplication` manipuladores de eventos. Enquanto o `Global.asax` arquivo é específico ao aplicativo web onde eles residem, módulos HTTP podem ser compilados em assemblies, no ponto em que adicionar o módulo HTTP para um site da Web é tão simple quanto soltando o assembly `Bin` pasta e registrando o Módulo no `Web.config`. Este tutorial não é afetada de criação e uso de módulos HTTP, mas as bibliotecas de log de erros de dois usadas nos dois tutoriais a seguir são implementadas como módulos HTTP. Para obter mais informações sobre os benefícios dos módulos HTTP, consulte [usando módulos e manipuladores HTTP para criar componentes do ASP.NET conectáveis](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Recuperando informações sobre a exceção sem tratamento

Neste ponto, temos um arquivo global. asax com um `Application_Error` manipulador de eventos. Quando esse manipulador de eventos é executado, é necessário notificar um desenvolvedor do erro e seus detalhes de log. Para realizar essas tarefas, primeiro precisamos determinar os detalhes da exceção que foi gerado. Usar o objeto de servidor [ `GetLastError` método](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) para recuperar os detalhes da exceção sem tratamento que causou o `Error` evento seja acionado.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

O `GetLastError` método retorna um objeto do tipo `Exception`, que é o tipo base para todas as exceções no .NET Framework. No entanto, no código acima eu estou convertendo o objeto de exceção retornado pelo `GetLastError` em um `HttpException` objeto. Se o `Error` evento está sendo disparado porque ocorreu uma exceção durante o processamento de um recurso do ASP.NET e em seguida, a exceção foi gerada é encapsulada dentro de um `HttpException`. Para obter a exceção que originou o uso de eventos de erro realmente o `InnerException` propriedade. Se o `Error` evento foi gerado devido a uma exceção com base em HTTP, como uma solicitação para uma página inexistente, um `HttpException` for lançada, mas não tem uma exceção interna.

O código a seguir usa o `GetLastErrormessage` para recuperar informações sobre a exceção que disparou a `Error` evento, armazenando os `HttpException` em uma variável chamada `lastErrorWrapper`. Em seguida, armazena o tipo, a mensagem e o rastreamento de pilha da exceção original em três variáveis de cadeia de caracteres, verificando se o `lastErrorWrapper` é a exceção real que disparou o `Error` evento (no caso de exceções com base em HTTP) ou se ele é meramente um wrapper para uma exceção foi acionada ao processar a solicitação.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

Neste ponto, você tem todas as informações que você precisa escrever código que registrará em log os detalhes da exceção para uma tabela de banco de dados. Você pode criar uma tabela de banco de dados com colunas para cada um dos detalhes do erro de interesse - o tipo, a mensagem, o rastreamento de pilha e assim por diante - juntamente com outras informações úteis, como a URL da página solicitada e o nome do usuário conectado no momento. No `Application_Error` manipulador de eventos, em seguida, você se conectar ao banco de dados e inserir um registro na tabela. Da mesma forma, você poderia adicionar código para um desenvolvedor do erro por meio de email de alerta.

As bibliotecas de registro em log de erros examinadas nos próximos dois tutoriais fornecem essa funcionalidade de imediato, portanto, não há nenhuma necessidade de criar este registro em log o erro e notificação por conta própria. No entanto, para ilustrar que o `Error` evento está sendo gerado e que o `Application_Error` manipulador de eventos pode ser usado para registrar em log os detalhes de erro e notificar um desenvolvedor, vamos adicionar código que notifica um desenvolvedor quando ocorre um erro.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notificando um desenvolvedor quando ocorre uma exceção sem tratamento

Quando uma exceção sem tratamento ocorre no ambiente de produção é importante alertar a equipe de desenvolvimento para que eles possam avaliar o erro e determinar quais ações precisam ser tomadas. Por exemplo, se houver um erro ao conectar-se ao banco de dados, você precisará duplas Verifique sua cadeia de conexão e, talvez, abra um tíquete de suporte com a empresa de hospedagem. Se a exceção ocorreu devido a um erro de programação, código ou lógica de validação talvez precise ser adicionado para evitar esses erros no futuro.

As classes .NET Framework na [ `System.Net.Mail` namespace](https://msdn.microsoft.com/library/system.net.mail.aspx) tornam mais fácil de enviar um email. O [ `MailMessage` classe](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) representa uma mensagem de email e tem propriedades como `To`, `From`, `Subject`, `Body`, e `Attachments`. O `SmtpClass` é usado para enviar uma `MailMessage` usando um servidor SMTP especificado do objeto; as configurações do servidor SMTP podem ser especificadas de forma declarativa ou programaticamente na [ `<system.net>` elemento](https://msdn.microsoft.com/library/6484zdc1.aspx) no `Web.config file`. Para obter mais informações sobre o envio de email mensagens em um aplicativo ASP.NET Confira meu artigo [envio de Email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)e o [perguntas frequentes sobre o mail](http://systemnetmail.com/).

> [!NOTE]
> O `<system.net>` elemento contém as configurações do servidor SMTP usadas pelo `SmtpClient` ao enviar um email de classe. Hospedagem da empresa provavelmente tem um servidor SMTP que você pode usar para enviar o email do seu aplicativo. Consulte a seção de suporte do host da web para obter informações sobre configurações do servidor SMTP, que você deve usar em seu aplicativo web.


Adicione o seguinte código para o `Application_Error` manipulador de eventos para enviar um email de um desenvolvedor quando ocorre um erro:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Enquanto o código acima é muito longo, a maior parte dele cria o HTML que aparece no email enviado para o desenvolvedor. O código começa fazendo referência a `HttpException` retornado pela `GetLastError` método (`lastErrorWrapper`). A exceção que foi gerada pela solicitação realmente é recuperada via `lastErrorWrapper.InnerException` e é atribuído à variável `lastError`. O tipo, a mensagem e a pilha de informações de rastreamento são recuperadas do `lastError` e armazenados em três variáveis de cadeia de caracteres.

Em seguida, uma `MailMessage` objeto chamado `mm` é criado. O corpo do email é formatado em HTML e exibe a URL da página solicitada, o nome do usuário conectado no momento e informações sobre a exceção (o tipo de mensagem e rastreamento de pilha). Uma das coisas legais sobre o `HttpException` classe é que você pode gerar o HTML usado para criar a exceção detalhes amarelo tela de morte (YSOD) chamando o [GetHtmlErrorMessage método](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Esse método é usado aqui para recuperar a marcação YSOD de detalhes da exceção e adicioná-lo para o email como um anexo. Um aviso de cuidado: se a exceção que disparou a `Error` evento foi uma exceção com base em HTTP (por exemplo, uma solicitação para uma página inexistente) e em seguida, o `GetHtmlErrorMessage` método retornará `null`.

A etapa final é enviar o `MailMessage`. Isso é feito criando um novo `SmtpClient` método e chamar seu `Send` método.

> [!NOTE]
> Antes de usar esse código em seu aplicativo web você desejará alterar os valores na `ToAddress` e `FromAddress` constantes da support@example.com a qualquer email endereço de email de notificação de erro devem ser enviados para e originam. Você também precisará especificar as configurações do servidor SMTP na `<system.net>` seção `Web.config`. Consulte seu provedor de host da web para determinar as configurações do servidor SMTP para usar.


Com este código em lugar sempre que houver um erro, o desenvolvedor é enviado uma mensagem de email que resume o erro e inclui o YSOD. No tutorial anterior, demonstramos um erro de tempo de execução visitando Genre.aspx e passando um inválido `ID` de valor por meio da cadeia de consulta, como `Genre.aspx?ID=foo`. Visitar a página com o `Global.asax` arquivo in-loco produz a mesma experiência de usuário, como no tutorial anterior - no ambiente de desenvolvimento você continuará vendo a exceção detalhes amarelo tela de morte, enquanto no ambiente de produção, você vai Consulte a página de erro personalizada. Além desse comportamento existente, o desenvolvedor é enviado um email.

**Figura 2** mostra as mensagens de email recebidas quando visitar `Genre.aspx?ID=foo`. O corpo do email resume as informações de exceção, enquanto o `YSOD.htm` anexo exibe o conteúdo que é mostrado na YSOD de detalhes de exceção (consulte **Figura3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Figura 2**: O desenvolvedor é enviado uma notificação por Email sempre que houver uma exceção sem tratamento  
([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Figura 3**: A notificação por Email inclui os detalhes da exceção YSOD como um anexo  
([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>E quanto a usar a página de erro personalizada?

Este tutorial mostrou como usar `Global.asax` e o `Application_Error` manipulador de eventos para executar código quando ocorre uma exceção sem tratamento. Especificamente, usamos este manipulador de eventos para notificar um desenvolvedor de um erro; Poderíamos ampliar para também registrar os detalhes do erro em um banco de dados. A presença do `Application_Error` manipulador de eventos não afeta a experiência do usuário final. Eles ainda veem a página de erro configurado, seja o YSOD de detalhes do erro, o YSOD de erro de tempo de execução ou a página de erro personalizada.

É natural se preocupar se o `Global.asax` arquivo e `Application_Error` evento é necessário ao usar uma página de erro personalizada. Quando ocorre um erro de usuário é mostrado a página de erro personalizada então por que não é possível colocamos o código para notificar o desenvolvedor e os detalhes do erro de log para a classe code-behind da página de erro personalizada? Embora, certamente, você pode adicionar código à classe de code-behind da página de erro personalizadas não possuem acesso aos detalhes da exceção que disparou o `Error` evento ao usar a técnica que exploramos no tutorial anterior. Chamar o `GetLastError` método de página de erro personalizada retorna `Nothing`.

O motivo para esse comportamento é porque a página de erro personalizada é alcançada por meio de um redirecionamento. Quando uma exceção sem tratamento atinge o tempo de execução do ASP.NET pelo mecanismo ASP.NET gera sua `Error` evento (que executa o `Application_Error` manipulador de eventos) e, em seguida *redireciona* o usuário para a página de erro personalizada, emitindo um `Response.Redirect(customErrorPageUrl)`. O `Response.Redirect` método envia uma resposta ao cliente com um código de status HTTP 302, instruindo o navegador para solicitar uma nova URL, ou seja, a página de erro personalizada. O navegador solicita automaticamente essa nova página. Você pode dizer que a página de erro personalizada foi solicitada separadamente da página em que o erro foi originado porque a barra de endereços do navegador muda para a URL da página de erro personalizada (consulte **Figura 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Figura 4**: quando ocorre um erro de navegador é redirecionado para a URL da página de erro personalizada  
([Clique para exibir a imagem em tamanho normal](processing-unhandled-exceptions-cs/_static/image12.png))

O efeito líquido é que a solicitação onde ocorreu a exceção sem tratamento termina quando o servidor responde com o redirecionamento HTTP 302. A solicitação subsequente para a página de erro personalizada é uma nova solicitação; Neste ponto, o ASP.NET mecanismo descartou as informações de erro e, além disso, não tem nenhuma maneira de associar a exceção sem tratamento na solicitação anterior com a nova solicitação para a página de erro personalizada. É por isso `GetLastError` retorna `null` quando chamado a partir da página de erro personalizada.

No entanto, é possível fazer com que a página de erro personalizado executada durante a mesma solicitação que causou o erro. O [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) método transfere a execução para a URL especificada e processa-os dentro da mesma solicitação. Você pode mover o código na `Application_Error` manipulador de eventos à classe de code-behind da página de erro personalizada, substituindo-o no `Global.asax` com o código a seguir:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

Agora quando ocorre uma exceção sem tratamento a `Application_Error` manipulador de eventos transfere o controle para a página de erro personalizado apropriado com base no código de status HTTP. Como o controle foi transferido, a página de erro personalizado tem acesso às informações de exceção sem tratamento por meio de `Server.GetLastError` e podem notificar um desenvolvedor do erro e seus detalhes de log. O `Server.Transfer` chamada interrompe o mecanismo do ASP.NET de redirecionar o usuário para a página de erro personalizada. Em vez disso, o conteúdo da página de erro personalizada é retornado como a resposta para a página que gerou o erro.

## <a name="summary"></a>Resumo

Quando ocorrer uma exceção sem tratamento em um aplicativo web ASP.NET o tempo de execução do ASP.NET gera o `Error` eventos e exibe a página de erro configurado. Podemos pode notificar o desenvolvedor do erro, seus detalhes de log ou processá-lo de alguma outra forma, criando um manipulador de eventos para o evento de erro. Há duas maneiras de criar um manipulador de eventos `HttpApplication` eventos como `Error`: no `Global.asax` arquivo ou de um módulo HTTP. Este tutorial mostrou como criar uma `Error` manipulador de eventos no `Global.asax` arquivo que notifica os desenvolvedores de um erro por meio de uma mensagem de email.

Criando um `Error` manipulador de eventos é útil se você precisar processar exceções não tratadas de alguma maneira exclusiva ou personalizada. No entanto, criar seu próprio `Error` manipulador de eventos para registrar a exceção ou para notificar um desenvolvedor não é o uso mais eficiente do seu tempo, pois já existem bibliotecas de registro em log de erro gratuito e fácil de usar que podem ser configurados em questão de minutos. Os próximos dois tutoriais examinam dois essas bibliotecas.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Módulos HTTP do ASP.NET e visão geral de manipuladores HTTP](https://support.microsoft.com/kb/307985)
- [Normalmente, responder a exceções sem tratamento - processamento de exceções sem tratamento](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Classe e o objeto de aplicativo do ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Manipuladores HTTP e módulos HTTP no ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Envio de Email no ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Noções básicas sobre o `Global.asax` arquivo](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Usando manipuladores e módulos HTTP para criar componentes ASP.NET conectáveis](https://msdn.microsoft.com/library/aa479332.aspx)
- [Trabalhando com o ASP.NET `Global.asax` arquivo](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Trabalhando com `HttpApplication` instâncias](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Anterior](displaying-a-custom-error-page-cs.md)
> [Próximo](logging-error-details-with-asp-net-health-monitoring-cs.md)
