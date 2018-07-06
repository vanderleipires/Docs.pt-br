---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Registro em log os detalhes do erro com o ELMAH (VB) | Microsoft Docs
author: rick-anderson
description: Erro de registro em log módulos e manipuladores (ELMAH) oferece outra abordagem para o log de erros de tempo de execução em um ambiente de produção. O ELMAH é um erro de software livre...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: dafb1facb0e2b1828eb990c423fbf5b1af0731d7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821671"
---
<a name="logging-error-details-with-elmah-vb"></a>Detalhes do erro de registro em log com o ELMAH (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Erro de registro em log módulos e manipuladores (ELMAH) oferece outra abordagem para o log de erros de tempo de execução em um ambiente de produção. O ELMAH é uma biblioteca de registro em log de erros de software livre que inclui recursos como a filtragem de erros e a capacidade de exibir o log de erros de uma página da web, como um RSS feed, ou baixá-lo como um arquivo delimitado por vírgula. Este tutorial orienta a baixar e configurar o ELMAH.


## <a name="introduction"></a>Introdução

O [tutorial anterior](logging-error-details-with-asp-net-health-monitoring-vb.md) examinado ASP. Sistema, que oferece um fora da biblioteca de caixa para registrar uma ampla variedade de eventos da Web de monitoramento da integridade da rede. Muitos desenvolvedores usam para fazer logon e email com os detalhes de exceções sem tratamento de monitoramento de integridade. No entanto, há alguns pontos problemáticos com este sistema. Primeiramente, é a falta qualquer tipo de interface do usuário para exibir informações sobre os eventos registrados. Se você quiser ver um resumo dos erros últimos 10 ou exibir os detalhes de um erro que ocorreu a última semana, você deve a fazer por meio do banco de dados, verificar por meio de sua caixa de entrada de email ou criar uma página da web que exibe informações do `aspnet_WebEvent_Events` tabela.

Outro ponto problemático gira em torno de complexidade do monitoramento de integridade. Como o monitoramento de integridade pode ser usado para gravar uma grande quantidade de eventos diferentes, e há uma variedade de opções para instruindo como e quando os eventos são registrados, configurar corretamente o sistema de monitoramento de integridade pode ser uma tarefa onerosa. Por fim, há problemas de compatibilidade. Porque o monitoramento de integridade foi adicionado para o .NET Framework versão 2.0, ele não está disponível para aplicativos web mais antigos, criados usando a versão do ASP.NET 1. x. Além disso, o `SqlWebEventProvider` classe, que são usados no tutorial anterior para detalhes do erro de logs para um banco de dados, só funciona com bancos de dados do Microsoft SQL Server. Você precisará criar uma classe de provedor de log personalizado caso você precise registrar erros em um repositório de dados alternativos, como um arquivo XML ou banco de dados Oracle.

Uma alternativa para o sistema de monitoramento de integridade é erro registro em log módulos e manipuladores (ELMAH), um sistema de log de erros gratuito, código-fonte aberto criado pelo [Atif Aziz](http://www.raboof.com/). A diferença mais notável entre os dois sistemas é a capacidade do ELAMH para exibir uma lista de erros e os detalhes de um erro específico de uma página da web e como um RSS feed. O ELMAH é mais fácil de configurar do que o monitoramento de integridade porque ele registra apenas erros. Além disso, o ELMAH inclui suporte para ASP.NET 1.x, aplicativos ASP.NET 2.0 e 3.5 do ASP.NET e é fornecido com uma variedade de provedores de log de origem.

Este tutorial orienta as etapas envolvidas na adição de ELMAH para um aplicativo ASP.NET. Vamos começar!

> [!NOTE]
> O ELMAH e sistema de monitoramento de integridade têm seus próprios conjuntos de prós e contras. Eu recomendo que você experimente os dois sistemas e decidir quais um atende melhor às suas necessidades.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Adicionando o ELMAH para um aplicativo Web ASP.NET

A integração do ELMAH em um aplicativo novo ou existente do ASP.NET é um processo fácil e simples que leva menos de cinco minutos. Em resumo, ela envolve quatro etapas simples:

1. Baixe o ELMAH e adicione o `Elmah.dll` assembly para seu aplicativo web,
2. Registrar os módulos HTTP e o manipulador do ELMAH `Web.config`,
3. Especificar opções de configuração do ELMAH, e
4. Crie a infraestrutura de origem do log de erro, se necessário.

Vamos examinar cada uma dessas quatro etapas, um de cada vez.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Etapa 1: Baixar os arquivos de projeto do ELMAH e adicionando`Elmah.dll`ao seu aplicativo Web

O ELMAH 1.0 BETA 3 (Build 10617), a versão mais recente no momento da escrita, está incluído no download com este tutorial. Como alternativa, você pode visitar o [site do ELMAH](https://code.google.com/p/elmah/) para obter a versão mais recente ou para baixar o código-fonte. Extrair o download do ELMAH para uma pasta na área de trabalho e localize o arquivo de assembly do ELMAH (`Elmah.dll`).

> [!NOTE]
> O `Elmah.dll` arquivo está localizado no download do `Bin` pasta que contém subpastas para diferentes versões do .NET Framework em compilações de depuração e versão. Use o build de versão para a versão do framework apropriado. Por exemplo, se você estiver criando um aplicativo web do ASP.NET 3.5, copie o `Elmah.dll` arquivo o `Bin\net-3.5\Release` pasta.


Em seguida, abra o Visual Studio e adicione o assembly ao seu projeto clicando com o nome do site no Gerenciador de soluções e escolhendo Add Reference no menu de contexto. Isso abre a caixa de diálogo Adicionar referência. Navegue até a guia Procurar e escolha o `Elmah.dll` arquivo. Essa ação adiciona o `Elmah.dll` arquivo para o aplicativo de web `Bin` pasta.

> [!NOTE]
> O tipo de projeto de aplicativo da Web (WAP) não mostra o `Bin` pasta no Gerenciador de soluções. Em vez disso, ela lista esses itens na pasta referências.


O `Elmah.dll` assembly inclui as classes usadas pelo sistema ELMAH. Essas classes se enquadram em uma das três categorias:

- **Módulos HTTP** -um módulo HTTP é uma classe que define manipuladores de eventos `HttpApplication` eventos, como o `Error` eventos. O ELMAH inclui vários módulos de HTTP, três os mais similar sendo: 

    - `ErrorLogModule` -registra em log as exceções sem tratamento para uma origem de log.
    - `ErrorMailModule` -envia os detalhes de uma exceção sem tratamento em uma mensagem de email.
    - `ErrorFilterModule` -aplica filtros especificados pelo desenvolvedor para determinar quais exceções são registradas e o que aqueles são ignorados.
- **Manipuladores HTTP** -um manipulador HTTP é uma classe que é responsável por gerar a marcação para um determinado tipo de solicitação. O ELMAH inclui manipuladores HTTP que processam os detalhes do erro como uma página da web, como um RSS feed ou como um arquivo delimitado por vírgulas (CSV).
- **Origens de Log de erro** - fora da caixa ELMAH pode registrar erros para a memória, um banco de dados do Microsoft SQL Server, um banco de dados do Microsoft Access, um banco de dados Oracle, para um arquivo XML, para um banco de dados SQLite, ou para um banco de dados do banco de dados do Vista. Como o sistema de monitoramento de integridade, a arquitetura do ELMAH foi criada usando o modelo de provedor, que significa que você pode criar e integrar seus próprios provedores de fonte de log personalizado, se necessário.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Etapa 2: Registrar o módulo HTTP e o manipulador do ELMAH

Enquanto o `Elmah.dll` arquivo contém os módulos HTTP e manipulador necessário para registrar automaticamente exceções não manipuladas e exibir detalhes do erro de uma página da web, eles devem ser explicitamente registrados na configuração do aplicativo da web. O `ErrorLogModule` módulo HTTP, depois de registrado, assina os `HttpApplication`do `Error` eventos. Sempre que esse evento é gerado o `ErrorLogModule` registra os detalhes da exceção a uma fonte de log especificado. Veremos como definir o provedor de código-fonte do log na próxima seção, "Configurando o ELMAH." O `ErrorLogPageFactory` fábrica de manipulador HTTP é responsável por gerar a marcação ao exibir o log de erros de uma página da web.

A sintaxe específica para o registro de manipuladores e módulos HTTP depende do servidor da web que é alimentar o site. Para o ASP.NET Development Server e do Microsoft IIS versão 6.0 e anteriores, módulos e manipuladores HTTP são registrados na `<httpModules>` e `<httpHandlers>` seções, que aparecem dentro de `<system.web>` elemento. Se você estiver usando o IIS 7.0, eles precisam ser registrados na `<system.webServer>` prvku `<modules>` e `<handlers>` seções. Felizmente, você pode definir os módulos HTTP e manipuladores *ambos* coloca independentemente do servidor web que está sendo usado. Essa opção é aquele mais portáteis, pois permite que a mesma configuração a ser usado em ambientes de desenvolvimento e produção, independentemente do servidor web que está sendo usado.

Comece registrando o `ErrorLogModule` módulo HTTP e o `ErrorLogPageFactory` manipulador HTTP na `<httpModules>` e `<httpHandlers>` seção `<system.web>`. Se sua configuração já define esses dois elementos, em seguida, basta incluir o `<add>` elemento para do ELMAH módulo HTTP e o manipulador.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

Em seguida, registre o módulo HTTP do ELMAH e o manipulador no `<system.webServer>` elemento. Como antes, se esse elemento já não estiver presente em sua configuração, em seguida, adicioná-lo.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

Por padrão, o IIS 7 reclama se manipuladores e módulos HTTP são registrados no `<system.web>` seção. O `validateIntegratedModeConfiguration` de atributo no `<validation>` elemento instrui o IIS 7 para suprimir essas mensagens de erro.

Observe que a sintaxe para registrar o `ErrorLogPageFactory` manipulador HTTP inclui um `path` atributo, que é definido como `elmah.axd`. Esse atributo informa o aplicativo web que, se uma solicitação chega para uma página chamada `elmah.axd` e em seguida, a solicitação deve ser processada pelo `ErrorLogPageFactory` manipulador HTTP. Vamos ver o `ErrorLogPageFactory` manipulador HTTP em ação posteriormente no tutorial.

### <a name="step-3-configuring-elmah"></a>Etapa 3: Configurando o ELMAH

O ELMAH procura por suas opções de configuração em um site do `Web.config` arquivo em uma seção de configuração personalizada denominada `<elmah>`. Para usar uma seção personalizada na `Web.config` deve ser definida primeiro no `<configSections>` elemento. Abra o `Web.config` arquivo e adicione a seguinte marcação para o `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Se você estiver configurando o ELMAH para um aplicativo do ASP.NET 1. x, em seguida, remova os `requirePermission="false"` de atributos do `<section>` elementos acima.


A sintaxe acima registra personalizado `<elmah>` seção e suas subseções: `<security>`, `<errorLog>`, `<errorMail>`, e `<errorFilter>`.

Em seguida, adicione a `<elmah>` seção para `Web.config`. Esta seção deve aparecer no mesmo nível como o `<system.web>` elemento. Dentro de `<elmah>` seção adicionar a `<security>` e `<errorLog>` seções desta forma:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

O `<security>` da seção `allowRemoteAccess` atributo indica se o acesso remoto é permitido. Se esse valor é definido como 0, em seguida, a página de web de registro de erro só podem ser exibidos localmente. Se esse atributo é definido como 1, em seguida, a página de web do log de erros está habilitado para os visitantes de locais e remotos. Por enquanto, vamos desabilitar a página de web de registro de erro para os visitantes remotos. Permitiremos que acesso remoto posteriormente depois que temos uma oportunidade para discutir as preocupações de segurança de fazer isso.

O `<errorLog>` seção define a origem de log de erro, que impõe onde os detalhes do erro são registrados; ele é semelhante ao `<providers>` seção no sistema de monitoramento de integridade. A sintaxe acima Especifica o `SqlErrorLog` classe como a origem de log de erro, que registra os erros em um banco de dados do Microsoft SQL Server especificado pelo `connectionStringName` valor do atributo.

> [!NOTE]
> O ELMAH é fornecido com provedores de log de erro adicionais que podem ser usados para registrar erros para um arquivo XML, um banco de dados do Microsoft Access, um banco de dados Oracle e outros armazenamentos de dados. Consulte o exemplo `Web.config` arquivo que está incluído no download do ELMAH para obter informações sobre como usar esses provedores de log de erro alternativa.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Etapa 4: Criando a infraestrutura de origem do Log de erro

Do ELMAH `SqlErrorLog` provedor registra os detalhes do erro para um banco de dados do Microsoft SQL Server especificado. O `SqlErrorLog` provedor espera que esse banco de dados para uma tabela chamada `ELMAH_Error` e três procedimentos armazenados: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, e `ELMAH_LogError`. O download do ELMAH inclui um arquivo chamado `SQLServer.sql` no `db` procedimentos armazenados de pasta que contém o T-SQL para criar essa tabela e eles. Você precisará executar essas instruções no banco de dados para usar o `SqlErrorLog` provedor.

**As figuras 1** e **2** mostrar o Gerenciador de banco de dados no Visual Studio depois dos objetos de banco de dados necessários para o `SqlErrorLog` provedor foram adicionados.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Figura 1**: O `SqlErrorLog` provedor de Logs de erros para o `ELMAH_Error` tabela

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Figura 2**: O `SqlErrorLog` provedor usa três procedimentos armazenados

## <a name="elmah-in-action"></a>ELMAH em ação

Neste ponto, adicionamos o ELMAH para o aplicativo web registrado a `ErrorLogModule` módulo HTTP e o `ErrorLogPageFactory` manipulador HTTP, especificadas opções de configuração do ELMAH no `Web.config`e adicionado os objetos de banco de dados necessários para o `SqlErrorLog` provedor de log de erro. Agora estamos prontos para ver o ELMAH em ação! Visite o site de resenhas de livros e visitar uma página que gera um erro de tempo de execução, tais como `Genre.aspx?ID=foo`, ou uma página inexistente, tais como `NoSuchPage.aspx`. O que você vê ao visitar essas páginas depende o `<customErrors>` configuração e você está visitando localmente ou remotamente. (Consultar novamente o [ *exibindo uma página de erro personalizada* tutorial](displaying-a-custom-error-page-vb.md) para relembrar neste tópico.)

O ELMAH não afeta o conteúdo que é mostrado ao usuário quando ocorre uma exceção sem tratamento; Assim, ele registra seus detalhes. Esse log de erros está acessível da página da web `elmah.axd` na raiz do seu site, como `http://localhost/BookReviews/elmah.axd`. (Esse arquivo não existe fisicamente em seu projeto, mas quando chega uma solicitação para `elmah.axd` o tempo de execução a envia para o `ErrorLogPageFactory` manipulador HTTP, que gera a marcação enviada de volta para o navegador.)

> [!NOTE]
> Você também pode usar o `elmah.axd` página para instruir o ELMAH para gerar um erro de teste. Visitar `elmah.axd/test` (como na `http://localhost/BookReviews/elmah.axd/test`) faz com que o ELMAH lançar uma exceção do tipo `Elmah.TestException`, que tem a mensagem de erro: "Esta é uma exceção de teste que pode ser ignorada com segurança."


**Figura 3** mostra o log de erros ao visitar `elmah.axd` do ambiente de desenvolvimento.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Figura 3**: `Elmah.axd` exibe o Log de erros de uma página da Web  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image7.png))

O log de erros **Figura 3** contém seis entradas de erro. Cada entrada inclui o código de status HTTP (404 ou 500, esses erros), o tipo, a descrição, o nome do usuário conectado quando o erro ocorreu e a data e hora. Ao clicar no link de detalhes exibe uma página que inclui a mesma mensagem de erro mostrada o erro detalhes amarelo tela da morte (consulte **Figura 4**), juntamente com os valores das variáveis de servidor no momento do erro (consulte  **Figura 5**). Você também pode exibir o XML bruto no qual os detalhes do erro são salvos, que inclui informações adicionais, como os valores no cabeçalho HTTP POST.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Figura 4**: exibir os detalhes do erro YSOD  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Figura 5**: explorar os valores da coleção de variáveis de servidor no momento do erro  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image13.png))

Implantar o ELMAH no site de produção envolve:

- Copiando o `Elmah.dll` o arquivo para o `Bin` pasta em produção,
- Copiar as definições de configuração para ELMAH específicas a `Web.config` arquivo usado na produção, e
- Adicionando a infraestrutura de origem do log de erro para o banco de dados de produção.

Exploramos técnicas para copiar arquivos de desenvolvimento para produção nos tutoriais anteriores. Talvez a maneira mais fácil de obter a infraestrutura de origem do log de erro no banco de dados de produção é usar o SQL Server Management Studio para se conectar ao banco de dados de produção e, em seguida, executar o `SqlServer.sql` arquivo de script, que criará a tabela necessária e armazenados procedimentos.

### <a name="viewing-the-error-details-page-on-production"></a>Exibindo a página de detalhes do erro em produção

Depois de implantar seu site de produção, visite o site de produção e gerar uma exceção sem tratamento. Como no ambiente de desenvolvimento, o ELMAH tem nenhum efeito sobre a página de erro exibida quando ocorre uma exceção sem tratamento; em vez disso, ele simplesmente registra o erro. Se você tentar visitar a página de log de erro (`elmah.axd`) do ambiente de produção, você será saudado com a página proibido mostrada na **Figura 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Figura 6**: por padrão, os visitantes do remotos não é possível exibir a página de Web de registro de erro  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image16.png))

Lembre-se de que na configuração do ELMAH `<security>` seção, definimos o `allowRemoteAccess` como 0, o que proíbe que os usuários remotos exibindo o log de erros de atributo. É importante proibir visitantes anônimos exibam o log de erros, como os detalhes do erro podem revelar vulnerabilidades de segurança ou outras informações confidenciais. Se você decidir definir esse atributo como 1 e habilitar o acesso remoto para o log de erros, é importante para bloquear o `elmah.axd` caminho de modo que somente os visitantes autorizados possa acessá-lo. Isso pode ser feito pela adição de um `<location>` elemento para o `Web.config` arquivo.

A configuração a seguir permite que somente os usuários na função de administrador para acessar a página de web de registro de erro:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> A função de administrador e os três usuários no sistema - Scott, Jisun e Alice - foram adicionados a [ *Configurando um site que usa serviços de aplicativos* tutorial](configuring-a-website-that-uses-application-services-vb.md). Os usuários Scott e Jisun são membros da função de administrador. Para obter mais informações sobre autenticação e autorização, consulte a minha [tutoriais de segurança de site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


O log de erros no ambiente de produção agora pode ser exibido por usuários remotos; consultar novamente **figuras 3**, **4**, e **5** das capturas de tela da página da web de log de erro. No entanto, se um usuário anônimo ou não-administrador tenta exibir a página de erro do log eles são automaticamente redirecionados para a página de logon (`Login.aspx`), como **Figura 7** mostra.

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Figura 7**: os usuários não autorizados são automaticamente redirecionados para a página de logon  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Log de erros por meio de programação

Do ELMAH `ErrorLogModule` módulo HTTP registra automaticamente as exceções sem tratamento para a origem de log especificado. Como alternativa, você pode registrar um erro sem precisar gerar uma exceção sem tratamento, usando o `ErrorSignal` classe e seu `Raise` método. O `Raise` método recebe um `Exception` do objeto e o registra como se essa exceção tivesse sido lançada e atingiu o tempo de execução do ASP.NET sem que está sendo manipulado. No entanto, a diferença é que a solicitação continuará normalmente depois de executar o `Raise` método foi chamado, ao passo que uma exceção sem tratamento, lançada interrompe a execução normal da solicitação e faz com que o tempo de execução do ASP.NET exibir o configurado página de erro.

O `ErrorSignal` classe é útil em situações em que há alguma ação que pode falhar, mas sua falha não é catastrófica para a operação geral que está sendo executada. Por exemplo, um site pode conter um formulário que usa a entrada do usuário, armazena em um banco de dados e, em seguida, envia ao usuário um email informando que eles as informações foram processadas. O que deve acontecer se as informações são salvas no banco de dados com êxito, mas há um erro ao enviar a mensagem de email? Uma opção seria gerar uma exceção e enviar o usuário para a página de erro. No entanto, isso pode confundir o usuário a pensar que as informações inseridos por eles não foram salvos. Outra abordagem seria log o erro relacionado a email, mas não altera a experiência do usuário de qualquer maneira. É aí que o `ErrorSignal` classe é útil.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Erro de notificação por Email

Juntamente com o log de erros para um banco de dados, o ELMAH também pode ser configurado para enviar por email detalhes do erro para um destinatário especificado. Essa funcionalidade é fornecida pelos `ErrorMailModule` módulo HTTP; portanto, você deve registrar esse módulo HTTP em `Web.config` para enviar por email detalhes do erro.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

Em seguida, especifique informações sobre o erro de email na `<elmah>` do elemento `<errorMail>` seção, que indica do email remetente e destinatário, assunto, e se o email é enviado de forma assíncrona.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Com as configurações acima em vigor, sempre que um erro de tempo de execução ocorre ELMAH envia um email para support@example.com com os detalhes do erro. Email de erros do ELMAH inclui as mesmas informações mostradas na página erro detalhes da web, ou seja, a mensagem de erro, o rastreamento de pilha e as variáveis de servidor (consultar novamente **figuras 4** e **5**). O email de erro também inclui o conteúdo de exceção detalhes amarelo tela da morte como um anexo (`YSOD.html`).

**Figura 8** mostra o email de erros do ELMAH gerado visitando `Genre.aspx?ID=foo`. Embora **Figura 8** mostra apenas a mensagens e pilha de rastreamento de erro, as variáveis de servidor estão incluídas mais adiante no corpo do email.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Figura 8**: você pode configurar o ELMAH para enviar detalhes do erro por Email  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Apenas Registrando erros de interesse

Por padrão, o ELMAH registra os detalhes de cada exceção sem tratamento, incluindo 404 e outros erros HTTP. Você pode instruir o ELMAH para ignorar essas ou outros tipos de erros usando a filtragem de erros. A lógica de filtragem é executada do ELMAH `ErrorFilterModule` módulo de HTTP, o que você precisará registrar no `Web.config` para usar a lógica de filtragem. As regras de filtragem são especificadas no `<errorFilter>` seção.

A marcação a seguir instrui o ELMAH para não registrar 404 erros.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> Não se esqueça de, para usar a filtragem de erros, você deve registrar o `ErrorFilterModule` módulo HTTP.


O `<equal>` elemento dentro do `<test>` seção é conhecida como uma asserção. Se a asserção for avaliada como true, em seguida, o erro é filtrado do log do ELMAH. Outras declarações estão disponíveis, incluindo: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`e assim por diante. Você também pode combinar declarações usando o `<and>` e `<or>` operadores boolianos. Além disso, você pode até mesmo incluir uma expressão JavaScript simples como uma asserção, ou escrever seus próprios asserções em c# ou Visual Basic.

Para obter mais informações sobre recursos de filtragem de erros do ELMAH, consulte o [seção de filtragem de erros](https://code.google.com/p/elmah/wiki/ErrorFiltering) na [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Resumo

O ELMAH fornece um mecanismo simple mas poderoso para registrar erros em um aplicativo web ASP.NET. Como o sistema de monitoramento de integridade da Microsoft, o ELMAH pode fazer logon erros de um banco de dados e pode enviar os detalhes do erro para um desenvolvedor por email. Ao contrário do sistema de monitoramento de integridade, o ELMAH inclui suporte para uma maior variedade de armazenamentos de dados de log de erro, incluindo imediato: Microsoft SQL Server, Microsoft Access, Oracle, arquivos XML e vários outros. Além disso, o ELMAH oferece um mecanismo interno para exibir o log de erros e os detalhes sobre um erro específico de uma página da web, `elmah.axd`. O `elmah.axd` página também pode processar informações de erro como um RSS feed ou como um arquivo de valores separados por vírgulas (CSV), que pode ser lido usando o Microsoft Excel. Você também pode instruir o ELMAH para filtrar erros do log usando asserções declarativas ou através de programação. E o ELMAH pode ser usado com aplicativos do ASP.NET versão 1.x.

Todos os aplicativos implantados devem ter algum mecanismo para registrar automaticamente exceções sem tratamento e enviar a notificação para a equipe de desenvolvimento. Se isso é feito usando o monitoramento de integridade ou o ELMAH é secundário. Em outras palavras, isso realmente não importa muito se você usar o monitoramento de integridade ou ELMAH; avaliar os dois sistemas e, em seguida, escolha aquele que melhor atenda às suas necessidades. O que é fundamentalmente importante é que algum mecanismo seja colocado em vigor para registrar exceções não manipuladas no ambiente de produção.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [ELMAH - manipuladores e módulos de registro em log de erros](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Página de projeto do ELMAH](https://code.google.com/p/elmah/) (origem do código, exemplos, wiki)
- [Conectando o ELMAH em um aplicativo Web para capturar exceções não tratadas](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (vídeo)
- [Páginas de Log de erro de segurança](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Usando manipuladores e módulos HTTP para criar componentes ASP.NET conectáveis](https://msdn.microsoft.com/library/aa479332.aspx)
- [Tutoriais de segurança do site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [Próximo](precompiling-your-website-vb.md)
