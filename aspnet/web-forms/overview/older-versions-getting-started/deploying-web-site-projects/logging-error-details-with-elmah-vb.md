---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Registro em log os detalhes do erro com ELMAH (VB) | Microsoft Docs
author: rick-anderson
description: "Erro de log de módulos e manipuladores (ELMAH) oferece outra abordagem para o log de erros de tempo de execução em um ambiente de produção. ELMAH é um erro de software livre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: d7082808d4aeb21767524c1e687dd688324d4d46
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="logging-error-details-with-elmah-vb"></a>Detalhes de erro de log com ELMAH (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Erro de log de módulos e manipuladores (ELMAH) oferece outra abordagem para o log de erros de tempo de execução em um ambiente de produção. ELMAH é uma biblioteca de registro em log de erros de software livre que inclui recursos como filtragem de erro e a capacidade de exibir o log de erros de uma página da web, como um feed RSS ou baixá-lo como um arquivo delimitado por vírgula. Este tutorial explica como baixar e configurar ELMAH.


## <a name="introduction"></a>Introdução

O [tutorial anterior](logging-error-details-with-asp-net-health-monitoring-vb.md) examinado ASP. Sistema, que oferece um fora da biblioteca de caixa para registrar uma ampla variedade de eventos da Web de monitoramento da integridade da rede. Muitos desenvolvedores usam para fazer logon e os detalhes de exceções sem tratamento de email de monitoramento de integridade. No entanto, há alguns pontos problemáticos com este sistema. Primeiramente, é a falta qualquer tipo de interface do usuário para exibir informações sobre os eventos registrados. Se você quiser ver um resumo das últimos 10 erros ou exibir os detalhes de erro que ocorreu a última semana, você deve ou enviar por meio do banco de dados, examinar a sua caixa de entrada ou criar uma página da web que exibe informações do `aspnet_WebEvent_Events` tabela.

Outro ponto problemático gira em torno de complexidade do monitoramento de integridade. Como monitoramento de integridade pode ser usado para gravar uma grande quantidade de eventos diferentes, e há uma variedade de opções para instruções de como e quando os eventos são registrados, configurar corretamente a sistema de monitoramento de integridade pode ser uma tarefa onerosa. Por fim, existem problemas de compatibilidade. Porque o monitoramento de integridade foi adicionado para o .NET Framework versão 2.0, não está disponível para aplicativos da web mais antigos, criados usando a versão do ASP.NET 1. x. Além disso, a `SqlWebEventProvider` classe, que são usados no tutorial anterior logs detalhes do erro para um banco de dados, só funciona com bancos de dados do Microsoft SQL Server. Você precisará criar uma classe de provedor de log personalizado se você precisar registrar erros em um repositório de dados alternativos, como um arquivo XML ou banco de dados Oracle.

Uma alternativa para a sistema de monitoramento de integridade é erro log módulos e manipuladores (ELMAH), um sistema de log de erros de livre, o código-fonte aberto criado por [Atif Aziz](http://www.raboof.com/). A diferença mais notável entre os dois sistemas é a capacidade do ELAMH para exibir uma lista de erros e os detalhes de um erro específico de uma página da web e um RSS feed. ELMAH é mais fácil de configurar do que o monitoramento de integridade porque ele registra apenas erros. Além disso, o ELMAH inclui suporte para ASP.NET 1. x, aplicativos ASP.NET 2.0 e o ASP.NET 3.5 e é fornecido com uma variedade de provedores de log de origem.

Este tutorial apresenta as etapas envolvidas na adição ELMAH para um aplicativo ASP.NET. Vamos começar!

> [!NOTE]
> ELMAH e sistema de monitoramento de integridade têm seus próprios conjuntos de prós e contras. Recomendo que você tente ambos os sistemas e decidir quais uma melhor se adequa às suas necessidades.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Adicionando ELMAH para um aplicativo Web ASP.NET

Integrar ELMAH em um aplicativo novo ou existente do ASP.NET é um processo simples e fácil que leva menos de cinco minutos. Em resumo, ela envolve quatro etapas simples:

1. Baixar ELMAH e adicionar o `Elmah.dll` assembly para seu aplicativo web
2. Registrar módulos HTTP e o manipulador do ELMAH `Web.config`,
3. Especificar opções de configuração do ELMAH, e
4. Crie a infraestrutura de origem do log de erro, se necessário.

Vamos examinar cada uma dessas quatro etapas, um de cada vez.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Etapa 1: Baixar os arquivos de projeto do ELMAH e adicionando`Elmah.dll`ao seu aplicativo Web

ELMAH 1.0 BETA 3 (compilação 10617), a versão mais recente no momento da gravação, está incluído no download com este tutorial. Como alternativa, você pode visitar o [site ELMAH](https://code.google.com/p/elmah/) para obter a versão mais recente ou para baixar o código-fonte. Extrair o download do ELMAH para uma pasta na área de trabalho e localize o arquivo de assembly do ELMAH (`Elmah.dll`).

> [!NOTE]
> O `Elmah.dll` arquivo está localizado no download do `Bin` pasta com subpastas para diferentes versões do .NET Framework em compilações de lançamento e de depuração. Use a compilação de versão para a versão do framework apropriado. Por exemplo, se você estiver criando um aplicativo web do ASP.NET 3.5, copie o `Elmah.dll` arquivo o `Bin\net-3.5\Release` pasta.


Em seguida, abra o Visual Studio e adicione o assembly ao seu projeto clicando no nome do site no Gerenciador de soluções e escolha Adicionar referência no menu de contexto. Isso abre a caixa de diálogo Adicionar referência. Navegue até a guia do navegador e escolha o `Elmah.dll` arquivo. Essa ação adiciona o `Elmah.dll` arquivo para o aplicativo de web `Bin` pasta.

> [!NOTE]
> O tipo de projeto de aplicativo da Web (WAP) não mostrar o `Bin` pasta no Gerenciador de soluções. Em vez disso, ele lista esses itens na pasta de referências.


O `Elmah.dll` assembly inclui as classes usadas pelo sistema ELMAH. Essas classes se enquadram em três categorias:

- **Módulos HTTP** -um módulo HTTP é uma classe que define os manipuladores de eventos para `HttpApplication` eventos, como o `Error` evento. ELMAH inclui vários módulos de HTTP, três os mais germane sendo: 

    - `ErrorLogModule`-registra exceções sem tratamento em uma origem de log.
    - `ErrorMailModule`-envia os detalhes de uma exceção sem tratamento em uma mensagem de email.
    - `ErrorFilterModule`-aplica filtros especificado pelo desenvolvedor para determinar quais exceções são registradas e o que aqueles são ignorados.
- **Manipuladores HTTP** -um manipulador HTTP é uma classe que é responsável por gerar a marcação para um determinado tipo de solicitação. ELMAH inclui manipuladores HTTP que processam os detalhes do erro como uma página da web, como um feed RSS ou como um arquivo delimitado por vírgulas (CSV).
- **Fontes de Log de erro** - predefinido ELMAH pode registrar erros de memória para um banco de dados do Microsoft SQL Server, um banco de dados do Microsoft Access, um banco de dados Oracle, para um arquivo XML, um banco de dados SQLite ou para um banco de dados do banco de dados do Vista. Como a sistema de monitoramento de integridade, a arquitetura do ELMAH foi criada usando o modelo de provedor, o que significa que você pode criar e integrar perfeitamente seus próprios provedores de log personalizado de fonte, se necessário.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Etapa 2: Registrar o módulo HTTP do ELMAH e o manipulador

Enquanto o `Elmah.dll` arquivo contém os módulos HTTP e manipulador necessário para registrar automaticamente exceções não manipuladas e exibir detalhes do erro de uma página da web, eles devem ser explicitamente registrados na configuração do aplicativo da web. O `ErrorLogModule` módulo de HTTP, uma vez registrados, assina o `HttpApplication`do `Error` evento. Sempre que esse evento é gerado o `ErrorLogModule` registra os detalhes da exceção para uma fonte de log especificado. Vamos ver como definir o provedor de origem do log na próxima seção, "Configurando ELMAH". O `ErrorLogPageFactory` fábrica de manipulador HTTP é responsável por gerar a marcação ao exibir o log de erros de uma página da web.

A sintaxe específica para registrar manipuladores e módulos HTTP depende do servidor web que está sendo o site. Para o servidor de desenvolvimento ASP.NET e do Microsoft IIS versão 6.0 e versões anteriores, módulos HTTP e manipuladores são registrados no `<httpModules>` e `<httpHandlers>` seções, que aparecem dentro de `<system.web>` elemento. Se você estiver usando o IIS 7.0, eles precisam ser registrados no `<system.webServer>` do elemento `<modules>` e `<handlers>` seções. Felizmente, você pode definir os módulos HTTP e manipuladores de *ambos* coloca independentemente do servidor web que está sendo usado. Essa opção é a mais portáteis, ele permite que a mesma configuração a ser usada em ambientes de desenvolvimento e produção, independentemente do servidor web que está sendo usado.

Iniciar registrando o `ErrorLogModule` módulo HTTP e o `ErrorLogPageFactory` manipulador HTTP no `<httpModules>` e `<httpHandlers>` seção `<system.web>`. Se sua configuração já define esses dois elementos, em seguida, basta incluir o `<add>` elemento para o módulo HTTP do ELMAH e o manipulador.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

Em seguida, registre o módulo HTTP do ELMAH e o manipulador de `<system.webServer>` elemento. Como antes, se este elemento já não estiver presente em sua configuração, em seguida, adicioná-lo.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

Por padrão, o IIS 7 reclama que se módulos HTTP e manipuladores são registrados no `<system.web>` seção. O `validateIntegratedModeConfiguration` atributo o `<validation>` elemento instrui o IIS 7 para suprimir essas mensagens de erro.

Observe que a sintaxe para registrar o `ErrorLogPageFactory` manipulador HTTP inclui um `path` atributo, que é definido como `elmah.axd`. Esse atributo informa o aplicativo web que se uma solicitação chega para uma página chamada `elmah.axd` , em seguida, a solicitação deve ser processada pelo `ErrorLogPageFactory` manipulador HTTP. Você verá o `ErrorLogPageFactory` manipulador HTTP na ação mais tarde neste tutorial.

### <a name="step-3-configuring-elmah"></a>Etapa 3: Configurar ELMAH

ELMAH procura as opções de configuração do site `Web.config` arquivo em uma seção de configuração personalizada denominada `<elmah>`. Para usar uma seção personalizada no `Web.config` deve ser definida primeiro no `<configSections>` elemento. Abra o `Web.config` de arquivo e adicione a seguinte marcação para o `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Se você estiver configurando ELMAH para um aplicativo do ASP.NET 1. x, em seguida, remova o `requirePermission="false"` de atributo do `<section>` elementos acima.


A sintaxe acima registra personalizado `<elmah>` seção e suas subseções: `<security>`, `<errorLog>`, `<errorMail>`, e `<errorFilter>`.

Em seguida, adicione o `<elmah>` seção `Web.config`. Esta seção deve aparecer no mesmo nível como o `<system.web>` elemento. Dentro de `<elmah>` seção adicionar o `<security>` e `<errorLog>` seções da seguinte forma:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

O `<security>` da seção `allowRemoteAccess` atributo indica se o acesso remoto é permitido. Se esse valor é definido como 0, a página de web de registro de erro só pode exibido localmente. Se esse atributo é definido como 1, em seguida, a página de web de registro de erro está habilitado para os visitantes locais e remotos. Por enquanto, vamos desabilitar a página de web de registro de erro para os visitantes remotos. Podemos vai permitir o acesso remoto posteriormente depois que temos uma oportunidade para discutir as preocupações de segurança de fazer isso.

O `<errorLog>` seção define a origem de log de erro, que impõe onde os detalhes do erro são registrados; ele é semelhante do `<providers>` seção no sistema de monitoramento de integridade. Especifica a sintaxe acima de `SqlErrorLog` classe como a origem de log de erro, que registra os erros em um banco de dados do Microsoft SQL Server especificado pelo `connectionStringName` valor do atributo.

> [!NOTE]
> ELMAH é fornecido com provedores de log de erro adicionais que podem ser usadas para registrar erros em um arquivo XML, um banco de dados do Microsoft Access, um banco de dados Oracle e outros repositórios de dados. Consulte o exemplo `Web.config` arquivo que está incluído no download do ELMAH para obter informações sobre como usar esses provedores de log de erro alternativo.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Etapa 4: Criando a infraestrutura de origem do Log de erro

Do ELMAH `SqlErrorLog` provedor registra os detalhes do erro para um banco de dados do Microsoft SQL Server especificado. O `SqlErrorLog` provedor espera que esse banco de dados para ter uma tabela chamada `ELMAH_Error` e três procedimentos armazenados: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, e `ELMAH_LogError`. O download do ELMAH inclui um arquivo chamado `SQLServer.sql` no `db` procedimentos armazenados de pasta que contém o T-SQL para criar essa tabela e eles. Você precisará executar essas instruções em seu banco de dados para usar o `SqlErrorLog` provedor.

**Figuras 1** e **2** mostrar o Gerenciador de banco de dados no Visual Studio após os objetos de banco de dados necessários para o `SqlErrorLog` provedor foram adicionados.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Figura 1**: O `SqlErrorLog` provedor de Logs de erros para o `ELMAH_Error` tabela

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Figura 2**: O `SqlErrorLog` provedor usa três procedimentos armazenados

## <a name="elmah-in-action"></a>ELMAH em ação

Neste ponto, adicionamos ELMAH para o aplicativo web, registrado o `ErrorLogModule` módulo HTTP e o `ErrorLogPageFactory` manipulador HTTP, especificadas opções de configuração do ELMAH no `Web.config`e adicionado os objetos de banco de dados necessários para o `SqlErrorLog` provedor de log de erro. Agora estamos prontos para ver ELMAH em ação! Visite o site de revisões de livros e visita uma página que gera um erro de tempo de execução, como `Genre.aspx?ID=foo`, ou uma página inexistente, como `NoSuchPage.aspx`. O que você vê ao visitar essas páginas depende de `<customErrors>` configuração e você está visitando localmente ou remotamente. (Consulte o [ *exibir uma página de erro personalizada* tutorial](displaying-a-custom-error-page-vb.md) para uma atualização sobre este tópico.)

ELMAH não afeta o conteúdo que é mostrado ao usuário quando ocorre uma exceção sem tratamento; Assim, ele registra seus detalhes. Esse log de erros está acessível da página da web `elmah.axd` da raiz do seu site, como `http://localhost/BookReviews/elmah.axd`. (Este arquivo não existem fisicamente em seu projeto, mas quando uma solicitação vier de `elmah.axd` o tempo de execução envia-o para o `ErrorLogPageFactory` manipulador HTTP, que gera a marcação enviada de volta para o navegador.)

> [!NOTE]
> Você também pode usar o `elmah.axd` página para instruir o ELMAH para gerar um erro de teste. Visitando `elmah.axd/test` (como em `http://localhost/BookReviews/elmah.axd/test`) faz com que o ELMAH lançar uma exceção do tipo `Elmah.TestException`, que tem a mensagem de erro: "Esta é uma exceção de teste que pode ser ignorada."


**Figura 3** mostra o log de erros ao visitar `elmah.axd` do ambiente de desenvolvimento.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Figura 3**: `Elmah.axd` exibe o Log de erros de uma página da Web  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image7.png))

O log de erros **Figura 3** contém seis entradas de erro. Cada entrada inclui o código de status HTTP (404 ou 500, esses erros), o tipo, a descrição, o nome do usuário conectado quando o erro ocorreu e a data e hora. Clique no link de detalhes exibe uma página que inclui a mesma mensagem de erro mostrada o erro detalhes amarelo tela de morte (consulte **Figura 4**) junto com os valores das variáveis de servidor no momento do erro (consulte  **Figura 5**). Você também pode exibir o XML bruto no qual os detalhes do erro são salvos, que inclui informações adicionais, como os valores no cabeçalho HTTP POST.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Figura 4**: exibir os detalhes do erro YSOD  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Figura 5**: explorar os valores da coleção de variáveis de servidor no momento do erro  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image13.png))

Implantar ELMAH para o site de produção envolve:

- Copiando o `Elmah.dll` o arquivo para o `Bin` pasta em produção,
- Copiar as definições de configuração específicas do ELMAH para a `Web.config` arquivo usado na produção, e
- Adicionando a infraestrutura de origem do log de erro para o banco de dados de produção.

Podemos depois de explorar técnicas para copiar arquivos de desenvolvimento para produção nos tutoriais anteriores. Talvez a maneira mais fácil de obter a infraestrutura de origem do log de erro do banco de dados de produção é usar o SQL Server Management Studio para se conectar ao banco de dados de produção e, em seguida, execute o `SqlServer.sql` arquivo de script, o que criará a tabela necessária e armazenados procedimentos.

### <a name="viewing-the-error-details-page-on-production"></a>Exibindo a página de detalhes do erro na produção

Depois de implantar o site de produção, visite o site de produção e gerar uma exceção sem tratamento. Como o ambiente de desenvolvimento, ELMAH não tem nenhum efeito na página de erro exibida quando ocorre uma exceção sem tratamento; em vez disso, ele simplesmente registra o erro. Se você tentar visitar a página de erro do log (`elmah.axd`) do ambiente de produção, você será recebido com a página proibido mostrada na **Figura 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Figura 6**: por padrão, os visitantes remotos não é possível exibir a página de Web de registro de erro  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image16.png))

Lembre-se de que a configuração de ELMAH `<security>` seção definimos o `allowRemoteAccess` atributo como 0, o que proíbe que usuários remotos exibindo o log de erros. É importante impedir que os visitantes anônimos exibindo o log de erros, como os detalhes do erro podem revelar vulnerabilidades de segurança ou outras informações confidenciais. Se você decidir definir esse atributo como 1 e habilitar o acesso remoto para o log de erros, é importante para bloquear o `elmah.axd` caminho de modo que somente os visitantes autorizados pode acessá-lo. Isso pode ser feito adicionando uma `<location>` elemento para o `Web.config` arquivo.

A configuração a seguir permite que somente os usuários na função de administrador para acessar a página de web de registro de erro:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> A função de administrador e três usuários no sistema de Scott, Jisun e Alice - foram adicionados a [ *Configurando um site que usa serviços de aplicativos* tutorial](configuring-a-website-that-uses-application-services-vb.md). Os usuários Scott e Jisun são membros da função de administrador. Para obter mais informações sobre autenticação e autorização, consulte Meus [tutoriais de segurança de site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


O log de erros no ambiente de produção agora pode ser exibido por usuários remotos; Voltar ao **figuras 3**, **4**, e **5** para capturas de tela da página da web de registro de erro. No entanto, se um usuário anônimo ou não-administrador tentar exibir a página de erro do log eles são automaticamente redirecionados para a página de logon (`Login.aspx`), como **Figura 7** mostra.

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Figura 7**: os usuários não autorizados são redirecionados automaticamente para a página de logon  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Log de erros por meio de programação

Do ELMAH `ErrorLogModule` módulo HTTP registra automaticamente exceções sem tratamento para a fonte de log especificado. Como alternativa, você pode registrar um erro sem precisar gerar uma exceção sem tratamento usando o `ErrorSignal` classe e seu `Raise` método. O `Raise` método é passado um `Exception` do objeto e o registra como se essa exceção tive sido lançada e atingiu o tempo de execução do ASP.NET sem que estão sendo tratados. No entanto, a diferença é que a solicitação continuará normalmente depois de executar o `Raise` método foi chamado, enquanto que uma exceção sem tratamento, lançada interrompe a execução normal da solicitação e faz com que o tempo de execução do ASP.NET exibir configurado página de erro.

O `ErrorSignal` classe é útil em situações em que há alguma ação que pode falhar, mas não é sua falha catastrófica para a operação geral que está sendo executada. Por exemplo, um site pode conter um formulário que usa a entrada do usuário, armazena em um banco de dados e envia um email informando-o usuário que as informações foram processadas. O que deve acontecer se as informações são salvas no banco de dados com êxito, mas há um erro ao enviar a mensagem de email? Uma opção seria lançar uma exceção e enviar o usuário para a página de erro. No entanto, isso pode confundir o usuário a pensar que não foi salva as informações inseridos por eles. Outra abordagem seria log o erro relacionado a email, mas não alteram a experiência do usuário de qualquer maneira. Isso é onde o `ErrorSignal` classe é útil.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-e-mail"></a>Erro de notificação por email

Junto com o log de erros para um banco de dados, ELMAH também pode ser configurado para enviar email detalhes do erro para um destinatário especificado. Essa funcionalidade é fornecida pelo `ErrorMailModule` módulo HTTP; portanto, você deve registrar este módulo HTTP em `Web.config` para o envio de detalhes de erro via email.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

Em seguida, especifique as informações sobre o email de erro no `<elmah>` do elemento `<errorMail>` seção, indicando que o email de remetente e destinatário, assunto, e se o email é enviado assincronamente.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Com as configurações acima em vigor, sempre que um erro de tempo de execução ocorre ELMAH envia um email para support@example.com com os detalhes do erro. Email de erros do ELMAH inclui as mesmas informações mostradas na página erro detalhes da web, ou seja, a mensagem de erro, o rastreamento de pilha e as variáveis de servidor (se referem aos **figuras 4** e **5**). O email de erro também inclui o conteúdo de exceção detalhes amarelo tela de morte como um anexo (`YSOD.html`).

**Figura 8** mostra email de erros do ELMAH gerado visitando `Genre.aspx?ID=foo`. Enquanto **Figura 8** mostra apenas o erro mensagem e rastreamento de pilha, as variáveis de servidor estão incluídas ainda mais para baixo no corpo do email.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Figura 8**: você pode configurar ELMAH para enviar detalhes de erro por email  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Somente log de erros de interesse

Por padrão, o ELMAH registra os detalhes de cada exceção sem tratamento, incluindo 404 e outros erros HTTP. Você pode instruir o ELMAH para ignorar estes ou outros tipos de erros usando a filtragem de erro. A lógica de filtragem é executada por do ELMAH `ErrorFilterModule` módulo de HTTP, que você precisará registrar no `Web.config` para usar a lógica de filtragem. As regras de filtragem são especificadas no `<errorFilter>` seção.

A seguinte marcação instrui ELMAH para não registrar 404 erros.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> Não se esqueça de, para usar a filtragem de erro, você deve registrar o `ErrorFilterModule` módulo HTTP.


O `<equal>` elemento dentro do `<test>` seção é conhecida como uma declaração. Se a asserção é avaliada como true, em seguida, o erro é filtrado do log do ELMAH. Há outras declarações, incluindo: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`e assim por diante. Você também pode combinar asserções usando o `<and>` e `<or>` operadores boolianos. Além disso, você pode até mesmo incluir uma expressão simples de JavaScript como uma declaração, ou escrever suas próprias declarações em c# ou Visual Basic.

Para obter mais informações sobre erros do ELMAH recursos de filtragem, consulte o [seção de filtragem de erro](https://code.google.com/p/elmah/wiki/ErrorFiltering) no [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Resumo

ELMAH fornece um mecanismo simple, mas poderoso para registrar erros em um aplicativo web ASP.NET. Como o sistema de monitoramento de integridade da Microsoft, ELMAH pode registrar erros em um banco de dados e pode enviar os detalhes do erro para um desenvolvedor via email. Ao contrário do sistema de monitoramento de integridade, ELMAH inclui suporte imediato para uma grande variedade de fontes de dados de log de erro, inclusive: Microsoft SQL Server, Microsoft Access, Oracle, arquivos XML e vários outros. Além disso, o ELMAH oferece um mecanismo interno para exibir o log de erros e os detalhes sobre um erro específico de uma página da web, `elmah.axd`. O `elmah.axd` página também pode processar informações de erro como um feed RSS ou como um arquivo de valores separados por vírgulas (CSV), que você pode ler usando o Microsoft Excel. Você também pode instruir o ELMAH para filtrar erros do log usando asserções declarativas ou através de programação. E ELMAH pode ser usado com aplicativos do ASP.NET versão 1. x.

Todos os aplicativos implantados devem ter algum mecanismo para registros automaticamente exceções sem tratamento e notificação seja enviada para a equipe de desenvolvimento. Se isso é feito usando o monitoramento de integridade ou ELMAH é secundário. Em outras palavras, não importa muito se você usar o monitoramento de integridade ou ELMAH; Avalie os dois sistemas e, em seguida, escolha a opção que melhor atenda às suas necessidades. O que é essencialmente importante é que algum mecanismo sejam colocados em prática para registrar exceções sem tratamento no ambiente de produção.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [ELMAH - manipuladores e módulos de log de erros](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Página de projeto do ELMAH](https://code.google.com/p/elmah/) (origem do código, exemplos, wiki)
- [Conectando ELMAH em um aplicativo Web para capturar exceções não tratadas](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (vídeo)
- [Páginas de Log de erro de segurança](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Usando módulos HTTP e manipuladores para criar componentes ASP.NET conectáveis](https://msdn.microsoft.com/en-us/library/aa479332.aspx)
- [Tutoriais de segurança de site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[Anterior](logging-error-details-with-asp-net-health-monitoring-vb.md)
[Próximo](precompiling-your-website-vb.md)
