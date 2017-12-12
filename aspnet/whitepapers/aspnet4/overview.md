---
uid: whitepapers/aspnet4/overview
title: "O ASP.NET 4 e visão geral do desenvolvimento do Visual Studio 2010 Web | Microsoft Docs"
author: rick-anderson
description: "Este documento fornece uma visão geral dos muitos dos novos recursos do ASP.NET que estão incluídos no.NET Framework 4 e no Visual Studio 2010."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 226ef83f289b8fbe9a68f0d0741c7eca0d96ba94
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>O ASP.NET 4 e visão geral do desenvolvimento do Visual Studio 2010 Web
====================
> Este documento fornece uma visão geral dos muitos dos novos recursos do ASP.NET que estão incluídos no.NET Framework 4 e no Visual Studio 2010.
> 
> [Baixe o white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Conteúdo**

**[Serviços principais](#0.2__Toc253429238 "_Toc253429238")**  
[Arquivo Web. config refatoração](#0.2__Toc253429239 "_Toc253429239")  
[Cache de saída extensível](#0.2__Toc253429240 "_Toc253429240")  
[Aplicativos da Web de auto-Start](#0.2__Toc253429241 "_Toc253429241")  
[Redirecionando permanentemente uma página](#0.2__Toc253429242 "_Toc253429242")  
[A redução de estado de sessão](#0.2__Toc253429243 "_Toc253429243")  
[Expandindo o intervalo de URLs permitidas](#0.2__Toc253429244 "_Toc253429244")  
[Validação de solicitação extensível](#0.2__Toc253429245 "_Toc253429245")  
[Objeto de cache e cache de extensibilidade do objeto](#0.2__Toc253429246 "_Toc253429246")  
[HTML extensível, a URL e a codificação do cabeçalho HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitoramento de desempenho para aplicativos individuais em um único processo de trabalho](#0.2__Toc253429248 "_Toc253429248")  
[Multiplataforma](#0.2__Toc253429249 "_Toc253429249")

**[AJAX](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery incluídos com o Web Forms e MVC](#0.2__Toc253429251 "_Toc253429251")  
[Suporte de rede de fornecimento de conteúdo](#0.2__Toc253429252 "_Toc253429252")  
[Scripts explícito do ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Formulários da Web](#0.2__Toc253429256 "_Toc253429256")**  
[Definir marcas Meta com as propriedades de Page.MetaDescription e Page.MetaKeywords](#0.2__Toc253429257 "_Toc253429257")  
[Habilitar o estado de exibição para os controles individuais](#0.2__Toc253429258 "_Toc253429258")  
[Alterações em recursos do navegador](#0.2__Toc253429259 "_Toc253429259")  
[Roteamento no ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Definindo as IDs de cliente](#0.2__Toc253429261 "_Toc253429261")  
[Manter seleção de linha em controles de dados](#0.2__Toc253429262 "_Toc253429262")  
[Controle de gráfico de ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtrando dados com o controle de QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Expressões de código codificada em HTML](#0.2__Toc253429265 "_Toc253429265")  
[Alterações no modelo de projeto](#0.2__Toc253429266 "_Toc253429266")  
[Aprimoramentos de CSS](#0.2__Toc253429267 "_Toc253429267")  
[Ocultando elementos em torno de campos ocultos da div](#0.2__Toc253429268 "_Toc253429268")  
[Renderização de uma tabela externa para controles modelados](#0.2__Toc253429269 "_Toc253429269")  
[Aprimoramentos do controle ListView](#0.2__Toc253429270 "_Toc253429270")  
[Aprimoramentos de controle RadioButtonList e CheckBoxList](#0.2__Toc253429271 "_Toc253429271")  
[Melhorias do menu de controle](#0.2__Toc253429272 "_Toc253429272")  
[Assistente e controles CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[O ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Suporte de áreas](#0.2__Toc253429275 "_Toc253429275")  
[Suporte à validação de atributo de anotação de dados](#0.2__Toc253429276 "_Toc253429276")  
[Auxiliares modelo](#0.2__Toc253429277 "_Toc253429277")

**[Dados dinâmicos](#0.2__Toc253429278 "_Toc253429278")**  
[Habilitar dados dinâmicos para projetos existentes](#0.2__Toc253429279 "_Toc253429279")  
[Sintaxe de controle DynamicDataManager declarativo](#0.2__Toc253429280 "_Toc253429280")  
[Modelos de entidade](#0.2__Toc253429281 "_Toc253429281")  
[Novos modelos de campo para URLs e endereços de email](#0.2__Toc253429282 "_Toc253429282")  
[Criação de Links com o controle DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Suporte para herança no modelo de dados](#0.2__Toc253429284 "_Toc253429284")  
[Suporte a relações muitos-para-muitos (somente para o Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Novos atributos de exibição de controle e enumerações de suporte](#0.2__Toc253429286 "_Toc253429286")  
[Suporte aprimorado para filtros](#0.2__Toc253429287 "_Toc253429287")

**[Melhorias de desenvolvimento da Web do Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[Melhor compatibilidade CSS](#0.2__Toc253429289 "_Toc253429289")  
[HTML e JavaScript trechos](#0.2__Toc253429290 "_Toc253429290")  
[Aprimoramentos de JavaScript IntelliSense](#0.2__Toc253429291 "_Toc253429291")

**[Implantação de aplicativo com o Visual Studio 2010 Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web empacotamento](#0.2__Toc253429293 "_Toc253429293")  
[Transformação do Web. config](#0.2__Toc253429294 "_Toc253429294")  
[Implantação de banco de dados](#0.2__Toc253429295 "_Toc253429295")  
[Um clique para publicar aplicativos Web](#0.2__Toc253429296 "_Toc253429296")  
[Recursos](#0.2__Toc253429297 "_Toc253429297")

**[Isenção de responsabilidade](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Serviços principais

O ASP.NET 4 apresenta uma série de recursos que melhoram os serviços do ASP.NET core como cache de saída e o armazenamento de estado de sessão.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Arquivo Web. config de refatoração

O `Web.config` arquivo que contém a configuração para um aplicativo Web aumentou consideravelmente nas versões anteriores do .NET Framework como novos recursos foram adicionados, como Ajax, roteamento e integração com o IIS 7. Isso tornou mais difícil de configurar ou iniciar um novo aplicativo Web sem uma ferramenta como o Visual Studio. Em aumentam NET Framework 4, os elementos de configuração importantes foram movidos para o `machine.config` arquivos e aplicativos agora herdam essas configurações. Isso permite que o `Web.config` arquivo em aplicativos ASP.NET 4 para estar vazio ou conter apenas as linhas a seguir, que especificam para Visual Studio qual versão do framework que o aplicativo está direcionando:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Cache de saída extensível

Desde o momento em que o ASP.NET 1.0 foi lançado, o cache de saída habilitou os desenvolvedores armazenar a saída gerada de páginas, controles e respostas HTTP na memória. Em solicitações subsequentes de Web, ASP.NET pode servir conteúdo mais rapidamente, recuperando a saída gerada da memória, em vez de gerar a saída do zero. No entanto, essa abordagem tem uma limitação — conteúdo gerado sempre precisa ser armazenada na memória e nos servidores que estão enfrentando o tráfego intenso, a memória consumida pelo cache de saída pode entrem em conflito com as exigências de memória de outras partes de um aplicativo Web.

O ASP.NET 4 adiciona um ponto de extensibilidade para o cache de saída que permite que você configure um ou mais provedores de cache de saída personalizados. Provedores de cache de saída podem usar qualquer mecanismo de armazenamento para manter o conteúdo HTML. Isso torna possível criar provedores personalizados de cache de saída para mecanismos de persistência diferentes, que podem incluir discos locais ou remotos, armazenamento em nuvem e distribuídas mecanismos de cache.

Criar um provedor de cache de saída personalizado como uma classe que deriva da nova *System.Web.Caching.OutputCacheProvider* tipo. Você pode configurar o provedor no `Web.config` arquivo usando o novo *provedores* subseção do *outputCache* elemento, conforme mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample2.xml)]

Por padrão no ASP.NET 4, todas as respostas HTTP, renderizado páginas e controles usam o cache de saída na memória, conforme mostrado no exemplo anterior, onde o *defaultProvider* atributo é definido como AspNetInternalProvider. Você pode alterar o provedor de cache de saída padrão usado para um aplicativo Web, especificando um nome de provedor diferente para *defaultProvider*.

Além disso, você pode selecionar diferentes provedores de cache de saída por controle e por solicitação. É a maneira mais fácil para escolher um provedor de cache de saída diferente para diferentes controles de usuário da Web fazer assim declarativamente usando o novo *providerName* atributo em uma diretiva de controle, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Especificando um provedor de cache de saída diferente para uma solicitação HTTP requer um pouco mais trabalho. Em vez de especificar declarativamente o provedor, você substitui o novo *GetOuputCacheProviderName* método o `Global.asax` arquivo para especificar qual provedor será usado para uma solicitação específica de forma programática. O exemplo a seguir mostra como fazer isso.

[!code-csharp[Main](overview/samples/sample4.cs)]

Com a adição de extensibilidade de provedor de cache de saída para o ASP.NET 4, agora você pode buscar mais curtas e mais inteligentes estratégias de cache de saída para sites da Web. Por exemplo, agora é possível armazenar em cache as páginas de "10 principais" de um site na memória, enquanto armazena em cache páginas que terão menos tráfego no disco. Como alternativa, você pode armazenar em cache todas as combinações variam por uma página renderizada, mas usar um cache distribuído para que o consumo de memória é descarregado a partir de servidores Web front-end.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Aplicativos da Web de início automático

Alguns aplicativos da Web precisam carregar grandes quantidades de dados ou executar inicialização dispendiosa processamento antes de atender a primeira solicitação. Em versões anteriores do ASP.NET, para essas situações era necessário planejar abordagens personalizadas para "Ativar" um aplicativo ASP.NET e, em seguida, execute o código de inicialização durante a *aplicativo\_carga* método o `Global.asax` arquivo.

Um novo recurso de escalabilidade *início automático* que diretamente endereços neste cenário está disponível quando 4 ASP.NET é executado no IIS 7.5 no Windows Server 2008 R2. O recurso auto-start fornece uma abordagem controlada para iniciar um pool de aplicativos, inicializando um aplicativo ASP.NET e, em seguida, aceitando solicitações HTTP.

> [!NOTE] 
> 
> Módulo de aquecimento de aplicativos do IIS para o IIS 7.5
> 
> A equipe do IIS lançou a primeira versão de teste beta do módulo de aquecimento de aplicativo para IIS 7.5. Isso torna aquecendo seus aplicativos mais fácil do descrito anteriormente. Em vez de escrever código personalizado, você deve especificar as URLs de recursos deve ser executada antes que o aplicativo da Web aceita solicitações da rede. Este aquecimento ocorre durante a inicialização do serviço IIS (se você configurou o pool de aplicativos do IIS como *AlwaysRunning*) e quando se reciclar um processo de trabalho do IIS. Durante a reciclagem, o processo de trabalho do IIS antigo continua a executar solicitações até que o processo de trabalho recém-gerada é totalmente aquecido, para que aplicativos experimentem sem interrupções ou outros problemas devido a unprimed caches. Observe que esse módulo funciona com qualquer versão do ASP.NET, começando com a versão 2.0.
> 
> Para obter mais informações, consulte [Application warm-up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) no site da IIS.net Web. Para uma explicação passo a passo que ilustra como usar o recurso de aquecimento, consulte [guia de Introdução com o módulo de aquecimento de aplicativos do IIS 7.5](https://www.iis.net/learn/manage) no site da IIS.net Web.


Para usar o recurso auto-start, um administrador do IIS define um pool de aplicativos no IIS 7.5 para ser iniciado automaticamente usando a configuração a seguir no `applicationHost.config` arquivo:

[!code-xml[Main](overview/samples/sample5.xml)]

Como um único pool de aplicativos pode conter vários aplicativos, você especificar aplicativos individuais para ser iniciado automaticamente usando a configuração a seguir no `applicationHost.config` arquivo:

[!code-xml[Main](overview/samples/sample6.xml)]

Quando um servidor IIS 7.5 é iniciada a frio ou um pool de aplicativos é reciclado, IIS 7.5 usa as informações de `applicationHost.config` arquivo para determinar quais aplicativos precisam de Web para ser iniciado automaticamente. Para cada aplicativo que está marcado para inicialização automática, IIS 7.5 envia uma solicitação para ASP.NET 4 para iniciar o aplicativo em um estado durante o qual o aplicativo temporariamente não aceita solicitações HTTP. Quando ele estiver nesse estado, o ASP.NET cria uma instância do tipo definido pelo *serviceAutoStartProvider* atributo (conforme mostrado no exemplo anterior) e chama o seu ponto de entrada pública.

Criar um tipo de início automático gerenciado com o ponto de entrada necessário Implementando o *IProcessHostPreloadClient* de interface, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample7.cs)]

Após a inicialização do seu código é executado no *Preload* método e o método retorna, o aplicativo ASP.NET está pronto para processar solicitações.

Com a adição de auto-start.5 do IIS e ASP.NET 4, agora você tem uma abordagem bem definida para executar a inicialização do aplicativo caro antes de processar a solicitação HTTP primeiro. Por exemplo, você pode usar o novo recurso de início automático para inicializar um aplicativo e, em seguida, sinalizar um balanceador de carga que o aplicativo foi inicializado e pronto para aceitar o tráfego HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Redirecionando permanentemente uma página

É uma prática comum em aplicativos da Web para mover páginas e outros tipos de conteúdo em torno ao longo do tempo, o que pode levar a um acúmulo de links obsoletos nos mecanismos de pesquisa. No ASP.NET, os desenvolvedores tradicionalmente tratou solicitações para URLs antigas usando o *Response. Redirect* método para encaminhar uma solicitação para a nova URL. No entanto, o *redirecionar* método envia uma resposta HTTP 302 encontrado (redirecionamento temporário), o que resulta em um HTTP adicional de ida e volta quando os usuários tentam acessar as URLs antigas.

O ASP.NET 4 adiciona um novo *RedirectPermanent* método auxiliar que torna mais fácil a questão HTTP 301 movido permanentemente respostas, como no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample8.cs)]

Mecanismos de pesquisa e outros agentes de usuário que reconhece redirecionamentos permanentes armazenará a nova URL que está associada com o conteúdo, o que elimina a desnecessária viagem de ida e feita pelo navegador para redirecionamentos temporários.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>A redução de estado de sessão

O ASP.NET fornece duas opções padrão para armazenar o estado de sessão em um farm da Web: um provedor de estado de sessão que invoca um servidor de estado de sessão fora do processo e um provedor de estado de sessão que armazena dados em um banco de dados do Microsoft SQL Server. Como ambas as opções envolvem armazenar informações de estado fora do processo de trabalho de um aplicativo Web, o estado da sessão precisa ser serializada antes de ser enviada para o armazenamento remoto. Dependendo de quantas informações que um desenvolvedor salva no estado de sessão, o tamanho dos dados serializados pode ficar muito grande.

O ASP.NET 4 introduz uma nova opção de compactação para os dois tipos de provedores de estado de sessão fora do processo. Quando o *compressionEnabled* mostrada no exemplo a seguir de opção de configuração é definida como *true*, ASP.NET compactará (e descompactar) estado da sessão serializado usando o .NET Framework  *System.IO.Compression.GZipStream* classe.

[!code-xml[Main](overview/samples/sample9.xml)]

Com a simple adição do novo atributo para o `Web.config` arquivos, aplicativos com reposição ciclos de CPU em servidores Web podem perceber substanciais reduções no tamanho dos dados de estado de sessão serializados.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Expandindo o intervalo de URLs permitidas

O ASP.NET 4 apresenta novas opções para expandir o tamanho de URLs de aplicativos. Versões anteriores do ASP.NET restrita comprimentos de caminho de URL para 260 caracteres, com base no limite de caminho de arquivo NTFS. No ASP.NET 4, você tem a opção para aumentar (ou diminuir) esse limite conforme apropriado para seus aplicativos, usando dois novos *httpRuntime* atributos de configuração. O exemplo a seguir mostra esses novos atributos.

[!code-xml[Main](overview/samples/sample10.xml)]

Para permitir caminhos maiores ou menores (a parte da URL que não inclua o protocolo, o nome do servidor e a cadeia de caracteres de consulta), modifique o  *[maxUrlLength](https://msdn.microsoft.com/en-us/library/system.web.configuration.httpruntimesection.maxurllength.aspx)*  atributo. Para permitir que cadeias de caracteres de consulta mais ou menos, modifique o valor da  *[maxQueryStringLength](https://msdn.microsoft.com/en-us/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)*  atributo.

O ASP.NET 4 também permite que você configure os caracteres que são usados pela verificação de caracteres de URL. Quando o ASP.NET encontra um caractere inválido na parte do caminho de URL, ele rejeita a solicitação e emite um erro HTTP 400. Nas versões anteriores do ASP.NET, as verificações de caracteres de URL eram limitadas a um conjunto fixo de caracteres. No ASP.NET 4, você pode personalizar o conjunto de caracteres válidos usando o novo *requestPathInvalidChars* atributo o *httpRuntime* elemento de configuração, conforme mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample11.xml)]

Por padrão, o *requestPathInvalidChars* atributo define oito caracteres como inválido. (Na cadeia de caracteres que é atribuída a *requestPathInvalidChars* por padrão*,*o menor que (&lt;), maior que (&gt;) e "e" comercial (&amp;) são caracteres codificado, porque o `Web.config` arquivo é um arquivo XML.) Você pode personalizar o conjunto de caracteres inválidos, conforme necessário.

> [!NOTE]
> Observação ASP.NET 4 sempre rejeita os caminhos de URL que contêm caracteres no intervalo ASCII de 0x00 a 0x1F, porque eles são caracteres de URL inválidos, conforme definido na RFC 2396 da IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Em versões do Windows Server que executam o IIS 6 ou superior, o driver de dispositivo do protocolo HTTP. sys rejeita automaticamente URLs com esses caracteres.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Validação de solicitação extensível

Validação de solicitação do ASP.NET procura dados recebidos de solicitação HTTP para cadeias de caracteres que são normalmente usadas em ataques de script entre sites (XSS). Se cadeias de caracteres possíveis XSS forem encontradas, a validação de solicitação sinaliza a cadeia de caracteres suspeita e retornará um erro. A validação de solicitação interna retorna um erro apenas quando ele encontra as cadeias de caracteres mais comuns usadas em ataques XSS. Tentativas anteriores de fazer a validação de XSS mais agressiva resultaram em muitos falsos positivos. No entanto, os clientes talvez queira verificações de validação de solicitação que é mais agressiva ou por outro lado talvez queira intencionalmente relaxar XSS para páginas específicas ou para tipos específicos de solicitações.

No ASP.NET 4, o recurso de validação de solicitação foi feito extensível para que você pode usar a lógica de validação de solicitação personalizado. Para estender a validação de solicitação, você cria uma classe que deriva da nova *System.Web.Util.RequestValidator* tipo e você configurar o aplicativo (no *httpRuntime* seção o `Web.config`arquivo) para usar o tipo personalizado. O exemplo a seguir mostra como definir uma classe personalizada de validação de solicitação:

[!code-xml[Main](overview/samples/sample12.xml)]

O novo *requestValidationType* atributo requer uma cadeia de identificador de tipo do .NET Framework padrão que especifica a classe que fornece validação de solicitação personalizado. Para cada solicitação, o ASP.NET invoca o tipo personalizado para processar cada parte dos dados recebidos de solicitação HTTP. O corpo da entidade, todos os cabeçalhos HTTP (cookies e cabeçalhos personalizados) e a URL de entrada estão disponíveis para inspeção por uma classe de validação de solicitação personalizado como o mostrado no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample13.cs)]

Para casos em que você não deseja inspecionar uma parte dos dados de entrada HTTP, a classe de validação de solicitação pode voltar para permitir que a validação de solicitação do ASP.NET padrão executar simplesmente chamando *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Objeto de cache e cache de extensibilidade do objeto

Desde sua primeira versão, o ASP.NET incluiu um cache de objeto avançado de na memória (*Caching*). A implementação de cache foi tão popular que ele tenha sido usado em aplicativos Web não. No entanto, é estranho para um aplicativo Windows Forms ou WPF incluir uma referência a `System.Web.dll` apenas para poder usar o cache de objetos do ASP.NET.

Para disponibilizar o cache para todos os aplicativos, o .NET Framework 4 apresenta um novo assembly, um novo namespace, alguns tipos de base e um implementação de cache de concreto. O novo `System.Runtime.Caching.dll` assembly contém uma nova API de cache no *System.Runtime.Caching* namespace. O namespace contém dois conjuntos de núcleo de classes:

- Tipos abstratos que fornecem a base para a criação de qualquer tipo de implementação de cache personalizada.
- Uma implementação do objeto concreto de na memória cache (o *System.Runtime.Caching.MemoryCache* classe).

O novo *MemoryCache* classe é modelada perto do cache do ASP.NET e compartilha grande parte da lógica do mecanismo de cache interno com o ASP.NET. Embora as APIs de cache públicas em *System.Runtime.Caching* foram atualizados para dar suporte ao desenvolvimento de caches personalizados, se você tiver usado o ASP.NET *Cache* objeto, você encontrará conceitos familiares de novas APIs.

Uma discussão detalhada sobre o novo *MemoryCache* classe e o suporte a APIs base requerem um documento inteiro. No entanto, o exemplo a seguir fornece uma ideia de como funciona a nova API de cache. O exemplo foi escrito para um aplicativo de Windows Forms, sem qualquer dependência no `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>HTML extensível, a URL e a codificação do cabeçalho HTTP

No ASP.NET 4, você pode criar rotinas de codificação personalizadas para as seguintes tarefas de codificação de texto comuns:

- Codificação HTML.
- Codificação de URL.
- Codificação do atributo HTML.
- Codificação de cabeçalhos HTTP de saída.

Você pode criar um codificador personalizado derivando de novo *System.Web.Util.HttpEncoder* tipo e, em seguida, configurar o ASP.NET para usar o tipo personalizado no *httpRuntime* seção o `Web.config` arquivo, como como mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample15.xml)]

Depois de um codificador personalizado tiver sido configurado, o ASP.NET automaticamente chama a implementação de codificação personalizada sempre que pública codificação métodos do *System.Web.HttpUtility* ou *System.Web.HttpServerUtility* classes são chamadas. Isso permite que uma parte de uma equipe de desenvolvimento da Web crie um codificador personalizado que implementa a codificação de caracteres agressiva, enquanto o restante da equipe de desenvolvimento da Web continua a usar o ASP.NET pública codificação APIs. Configurando centralmente um codificador personalizado de *httpRuntime* elemento, você terá garantia de que todas as chamadas de codificação de texto do ASP.NET pública codificação APIs são roteadas através do codificador personalizado.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitoramento de desempenho para aplicativos individuais em um único processo de trabalho

Para aumentar o número de sites da Web que pode ser hospedado em um único servidor, muitos hosters executar vários aplicativos ASP.NET em um único processo de trabalho. No entanto, se vários aplicativos usam um processo de trabalho compartilhada, é difícil para os administradores do servidor identificar um aplicativo que está apresentando problemas.

O ASP.NET 4 aproveita a nova funcionalidade de monitoramento de recursos introduzida pelo CLR. Para habilitar essa funcionalidade, você pode adicionar o seguinte trecho de configuração XML para o `aspnet.config` arquivo de configuração.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Observe o `aspnet.config` arquivo está no diretório em que o .NET Framework está instalado. Não é o `Web.config` arquivo.


Quando o *appDomainResourceMonitoring* recurso foi habilitado, dois novos contadores de desempenho estão disponíveis na categoria de desempenho "Aplicativos ASP.NET": *% tempo do processador gerenciado* e  *Gerenciado de memória usada*. Ambos esses contadores de desempenho usam o novo recurso de gerenciamento de recursos de domínio de aplicativo do CLR para controlar o tempo estimado de CPU e a utilização de memória gerenciada de aplicativos ASP.NET individuais. Como resultado, com ASP.NET 4, os administradores agora têm uma exibição mais detalhada para o consumo de recursos de aplicativos individuais executados em um único processo de trabalho.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Multiplataforma

Você pode criar um aplicativo que tem como alvo uma versão específica do .NET Framework. No ASP.NET 4, um novo atributo de *compilação* elemento o `Web.config` arquivo permite que você direcione o .NET Framework 4 e posterior. Se você direcionar explicitamente o .NET Framework 4, e se você incluir elementos opcionais no `Web.config` arquivo como entradas para *System. CodeDom*, esses elementos devem estar corretos para o .NET Framework 4. (Se você não explicitamente direcionar o .NET Framework 4, a estrutura de destino é inferida da falta de uma entrada de `Web.config` arquivo.)

O exemplo a seguir mostra o uso do *targetFramework* atributo no *compilação* elemento o `Web.config` arquivo.

[!code-xml[Main](overview/samples/sample17.xml)]

Observe o seguinte sobre como alvo uma versão específica do .NET Framework:

- Em um pool de aplicativos do .NET Framework 4, o sistema de compilação do ASP.NET assume o .NET Framework 4 como um destino se o `Web.config` arquivo não inclui o *targetFramework* atributo ou se o `Web.config` arquivo está ausente. (Talvez seja necessário fazer alterações de código ao seu aplicativo para que ele seja executado sob o .NET Framework 4.)
- Se você incluir o *targetFramework* atributo e se o *System. CodeDom* elemento é definido o `Web.config` arquivo, esse arquivo deve conter as entradas corretas para o .NET Framework 4.
- Se você estiver usando o *aspnet\_compilador* comando para pré-compilar seu aplicativo (como em um ambiente de compilação), você deve usar a versão correta do *aspnet\_compilador* comando para a estrutura de destino. Use o compilador fornecido com o .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) para compilar o .NET Framework 3.5 e versões anteriores. Use o compilador é fornecido com o .NET Framework 4 para compilar aplicativos criados usando essa estrutura ou versões posteriores.
- Em tempo de execução, o compilador usa os assemblies do framework mais recentes que estão instalados no computador (e, portanto, no GAC). Se uma atualização for feita posteriormente para o framework (por exemplo, uma versão hipotética 4.1 está instalada), você poderá usar os recursos na versão mais recente do framework, embora o *targetFramework* atributo tem como alvo uma versão inferior (como 4.0). (No entanto, em tempo de design no Visual Studio 2010 ou quando você usa o *aspnet\_compilador* de comando, usar os recursos mais recentes do framework fará com que os erros de compilador).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery incluídos com o Web Forms e MVC

Os modelos do Visual Studio para Web Forms e MVC incluem a biblioteca de código-fonte aberto jQuery. Quando você cria um novo site ou projeto, é criada uma pasta de Scripts que contém os seguintes arquivos de 3:

- jQuery-1.4.1.js – o legível, unminified versão da biblioteca do jQuery.
- jQuery-14.1.min.js – a versão minimizada da biblioteca jQuery.
- jQuery-1.4.1-vsdoc.js – o arquivo de documentação do Intellisense para a biblioteca jQuery.

Inclua a versão unminified do jQuery ao desenvolver um aplicativo. Inclua a versão minimizada do jQuery para aplicativos de produção.

Por exemplo, a seguinte página de Web Forms ilustra como você pode usar o jQuery para alterar a cor de plano de fundo de controles de caixa de texto do ASP.NET para amarelo quando ele tem foco.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Suporte de rede de fornecimento de conteúdo

A Microsoft Ajax Content Delivery Network (CDN) permite que você adicione facilmente o ASP.NET Ajax e jQuery scripts para seus aplicativos Web. Por exemplo, você pode iniciar usando a biblioteca do jQuery simplesmente adicionando um `<script>` marca para a página que aponta para Ajax.microsoft.com como este:

[!code-html[Main](overview/samples/sample19.html)]

Aproveitando a CDN do Microsoft Ajax, você pode melhorar significativamente o desempenho de seus aplicativos Ajax. O conteúdo do Microsoft Ajax CDN é armazenados em cache nos servidores localizados em todo o mundo. Além disso, a Microsoft Ajax CDN permite que navegadores para reutilizar arquivos armazenados em cache de JavaScript para sites da Web que estão localizados em domínios diferentes.

A rede de entrega de conteúdo do Microsoft Ajax dá suporte a SSL (HTTPS), caso seja necessário atender a uma página da web usando o protocolo SSL.

Para saber mais sobre a CDN do Microsoft Ajax, visite o seguinte site:

[https://www.ASP.NET/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

O ScriptManager ASP.NET oferece suporte a CDN do Microsoft Ajax. Simplesmente por uma propriedade de configuração, a propriedade EnableCdn, você pode recuperar todos os arquivos de JavaScript do ASP.NET framework da CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Depois de definir a propriedade EnableCdn para o valor true, a estrutura do ASP.NET irá recuperar todos os arquivos de JavaScript do ASP.NET framework da CDN, incluindo todos os arquivos de JavaScript usados para validação e o UpdatePanel. A definição dessa um propriedade pode ter um impacto significativo sobre o desempenho do seu aplicativo web.

Você pode definir o caminho da CDN para seus próprios arquivos JavaScript usando o atributo WebResource. A nova propriedade CdnPath Especifica o caminho para a CDN usado quando você definir a propriedade EnableCdn como o valor true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Scripts explícito do ScriptManager

No passado, se você usou o ScriptManger ASP.NET, em seguida, era necessário para carregar a biblioteca de Ajax ASP.NET monolítico inteira. Ao aproveitar a nova propriedade ScriptManager.AjaxFrameworkMode, você pode controlar exatamente quais componentes da biblioteca do Ajax do ASP.NET são carregados e carregar somente os componentes da biblioteca de Ajax do ASP.NET que você precisa.

A propriedade ScriptManager.AjaxFrameworkMode pode ser definida com os seguintes valores:

- Habilitado – Especifica que o controle ScriptManager inclui automaticamente o arquivo de script MicrosoftAjax. js, que é um arquivo de script combinado de todos os scripts de framework core (comportamento herdado).
- Desativado – Especifica que todos os recursos de script do Microsoft Ajax estão desabilitados e que o ScriptManager não faz referência a todos os scripts automaticamente.
- Explícita – Especifica que você explicitamente inclua referências de script ao arquivo de script do framework individuais core que requer que a página e que você inclua referências para as dependências que requer que cada arquivo de script.

Por exemplo, se você definir a propriedade AjaxFrameworkMode como o valor explícito, em seguida, você pode especificar os scripts de componente específicos do Ajax do ASP.NET que você precisa:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Formulários da Web foi dos principais recursos no ASP.NET desde o lançamento do ASP.NET 1.0. Muitos aprimoramentos foram nessa área para ASP.NET 4, incluindo o seguinte:

- A capacidade de definir *meta* marcas.
- Mais controle sobre o estado de exibição.
- Maneiras mais fáceis de trabalhar com recursos do navegador.
- Suporte para usar o roteamento ASP.NET Web Forms.
- Mais controle sobre IDs geradas.
- A capacidade de persistir as linhas selecionadas em controles de dados.
- Mais controle sobre o HTML renderizado no *FormView* e *ListView* controles.
- Filtragem de suporte para controles de fonte de dados.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Marcas de metadados de configuração com as propriedades de Page.MetaDescription e Page.MetaKeywords

O ASP.NET 4 adiciona duas propriedades para o *página* classe *MetaKeywords* e *MetaDescription*. Essas duas propriedades representam correspondente *meta* marcas em sua página, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Essas duas propriedades funcionam da mesma forma que a página *título* propriedade faz. Eles seguem estas regras:

1. Se não houver nenhum *meta* marcas no *head* elemento que correspondem aos nomes de propriedade (ou seja, nome = "keywords" para *Page.MetaKeywords* e o nome = "Descrição" para  *Page.MetaDescription*, o que significa que essas propriedades não foram definidas), o *meta* marcas serão adicionadas à página quando ela é processada.
2. Se já houver *meta* marcas com esses nomes, essas propriedades atuam como obter e definir métodos para o conteúdo das marcas existentes.

Você pode definir essas propriedades em tempo de execução, que lhe permite obter o conteúdo de um banco de dados ou outra fonte, e que permite que você defina as marcas dinamicamente para descrever o que é uma página específica para.

Você também pode definir o *palavras-chave* e *descrição* propriedades de *@ Page* diretiva na parte superior da marcação da página de Web Forms, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Isso substituirá o *meta* marca conteúdo (se houver) já declarado na página.

O conteúdo da descrição *meta* marca são usados para melhorar a pesquisa listando visualizações no Google. (Para obter detalhes, consulte [melhorar trechos de código com uma transformação de descrição de metadados](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) no blog da Central do Google Webmaster.) Google e o Windows Live Search não usam o conteúdo, as palavras-chave para qualquer coisa, mas talvez outros mecanismos de pesquisa. Para obter mais informações, consulte [Meta palavras-chave conselho](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) no site da Web de guia do mecanismo de pesquisa.

Essas novas propriedades são um recurso simple, mas salvar você de requisito para adicioná-las manualmente ou escrevendo seu próprio código para criar o *meta* marcas.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Habilitar o estado de exibição para os controles individuais

Por padrão, o estado de exibição está habilitado para a página, com o resultado que cada controle na página potencialmente armazena o estado de exibição mesmo se ele não é necessário para o aplicativo. Dados de estado de exibição estão incluídos na marcação que uma página gera e aumenta a quantidade de tempo que leva para enviar uma página para o cliente e enviá-lo novamente. Armazenar estado de exibição mais do que o necessário pode causar degradação significativa no desempenho. Em versões anteriores do ASP.NET, os desenvolvedores podem desabilitar o estado de exibição para os controles individuais para reduzir o tamanho da página, mas tiveram que fazer explicitamente para controles individuais. No ASP.NET 4, controles de servidor Web incluem um *ViewStateMode* que permite que você desabilitar o estado de exibição por padrão e, em seguida, habilitá-lo somente para os controles que exigem a ele na página de propriedade.

O *ViewStateMode* propriedade tem uma enumeração que tem três valores: *habilitado*, *desabilitado*, e *herdar*. *Habilitado* permite exibir o estado do controle e para os controles filho que são definidos como *herdar* ou que tenha nada definido. *Desabilitado* desativa o estado de exibição e *herdar* Especifica que o controle utiliza o *ViewStateMode* configuração do controle pai.

A exemplo a seguir mostra como o *ViewStateMode* propriedade funciona. A marcação e o código para os controles na página a seguir inclui valores para o *ViewStateMode* propriedade:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Como você pode ver, o código desativa o estado de exibição para o controle PlaceHolder1. O controle de label1 filho herda esse valor de propriedade (*herdar* é o valor padrão para *ViewStateMode* para controles.) e não salva, portanto, nenhum estado de exibição. No controle PlaceHolder2, *ViewStateMode* é definido como *habilitado*, portanto, label2 herda essa propriedade e salva o estado de exibição. Quando a página for carregada pela primeira vez, o *texto* propriedade do *rótulo* controles é definido como a cadeia de caracteres "[DynamicValue]".

O efeito dessas configurações é que, quando a página for carregada na primeira vez, a seguinte saída é exibida no navegador:

Desabilitado`: [DynamicValue]`

Habilitado:`[DynamicValue]`

Após um postback, no entanto, a seguinte saída é exibida:

Desabilitado`: [DeclaredValue]`

Habilitado:`[DynamicValue]`

Controle label1 (cujo *ViewStateMode* valor é definido como *desabilitado*) não foi preservada o valor que foi definida no código. No entanto, o label2 controlar (cujo *ViewStateMode* valor é definido como *habilitado*) tenha preservado seu estado.

Você também pode definir *ViewStateMode* no *@ Page* diretiva, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample26.aspx)]

O *página* classe é apenas outro controle; ele atua como o controle pai para todos os outros controles na página. O valor padrão de *ViewStateMode* é *habilitado* para instâncias de *página*. Pois controles padrão *herdar*, controles herdarão o *habilitado* a menos que você defina o valor de propriedade *ViewStateMode* no nível de página ou controle.

O valor da *ViewStateMode* propriedade determina se o estado de exibição é mantido somente se o *EnableViewState* está definida como *true*. Se o *EnableViewState* está definida como *false*, estado de exibição não será mantido mesmo se *ViewStateMode* é definido como *habilitado*.

É um bom uso para esse recurso com *ContentPlaceHolder* controles em páginas mestras, onde é possível definir *ViewStateMode* para *desabilitado* para o mestre de página e, em seguida, habilitá-lo individualmente para *ContentPlaceHolder* controles que por sua vez contêm controles que exigem o estado de exibição.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Alterações em recursos do navegador

ASP.NET determina os recursos do navegador que um usuário está usando para procurar o site usando um recurso chamado *recursos do navegador*. Recursos do navegador são representados pelo *HttpBrowserCapabilities* objeto (exposto pelo *Request. browser* propriedade). Por exemplo, você pode usar o *HttpBrowserCapabilities* objeto para determinar se o tipo e a versão do navegador atual oferece suporte a uma versão específica do JavaScript. Ou, você pode usar o *HttpBrowserCapabilities* objeto para determinar se a solicitação teve origem em um dispositivo móvel.

O *HttpBrowserCapabilities* objeto é controlado por um conjunto de arquivos de definição do navegador. Esses arquivos contêm informações sobre os recursos dos navegadores específicos. No ASP.NET 4, esses arquivos de definição do navegador foram atualizados para conter informações sobre navegadores introduzidas recentemente e dispositivos, como Google Chrome, pesquisa em smartphones BlackBerry movimento e Apple iPhone.

A lista a seguir mostra o novo navegador arquivos de definição:

- *BlackBerry.browser*
- *Chrome.browser*
- *Default.browser*
- *Firefox.browser*
- *gateway.browser*
- *Generic.browser*
- *IE.browser*
- *iemobile.browser*
- *iPhone.browser*
- *opera.browser*
- *Safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Usando provedores de recursos do navegador

No ASP.NET versão 3.5 Service Pack 1, você pode definir os recursos de um navegador das seguintes maneiras:

- No nível do computador, você pode criar ou atualizar um `.browser` arquivo XML na seguinte pasta:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Depois de definir a capacidade do navegador, execute o seguinte comando do Visual Studio Prompt de comando para recompilar o assembly de recursos do navegador e adicioná-lo no GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Para um aplicativo individual, você cria um `.browser` arquivo do aplicativo `App_Browsers` pasta.

Esses métodos exigem que você altere os arquivos XML e para alterações no nível de computador, você deve reiniciar o aplicativo depois de executar o aspnet\_regbrowsers.exe processo.

O ASP.NET 4 inclui um recurso chamado *provedores de recursos do navegador*. Como o nome sugere, isso permite que você criar um provedor que por sua vez, permite que você use seu próprio código para determinar os recursos do navegador.

Na prática, os desenvolvedores geralmente não definir recursos de navegador personalizado. Arquivos de navegador são difíceis de atualizar, o processo para atualizá-las é bastante complicada e a sintaxe XML para `.browser` arquivos podem ser complexos para usar e definir. O que seria facilita esse processo é se houvesse uma sintaxe de definição de navegador comum ou um banco de dados que continha definições atualizadas de navegador ou até mesmo um serviço da Web para um banco de dados. O novo recurso de provedores de recursos do navegador torna esses cenários possíveis e prático para desenvolvedores de terceiros.

Há duas abordagens principais para usar o novo recurso de provedor de recursos de navegador ASP.NET 4: estendendo os recursos do navegador ASP.NET a funcionalidade de definição ou totalmente substituí-lo. As seções a seguir primeiro descrevem como substituir a funcionalidade e como estendê-lo.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Substituir a funcionalidade de recursos do navegador do ASP.NET

Para substituir a funcionalidade de definição de recursos de navegador ASP.NET completamente, siga estas etapas:

1. Criar uma classe de provedor que deriva de *HttpCapabilitiesProvider* e que substitui o *GetBrowserCapabilities* método, como no exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    O código neste exemplo cria um novo *HttpBrowserCapabilities* de objeto, especificando somente o recurso denominado de navegador e configurando esse recurso para MyCustomBrowser.
2. Registre o provedor com o aplicativo. 

    Para usar um provedor com um aplicativo, você deve adicionar o *provedor* de atributo para o *browserCaps* seção o `Web.config` ou `Machine.config` arquivos. (Você também pode definir os atributos de provedor em um *local* elemento para diretórios específicos no aplicativo, como em uma pasta para um dispositivo móvel específico.) O exemplo a seguir mostra como definir o *provedor* atributo em um arquivo de configuração:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Outra maneira de registrar a nova definição de recurso do navegador é usar o código, conforme mostrado no exemplo a seguir:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Esse código deve ser executado *aplicativo\_iniciar* evento o `Global.asax` arquivo. Qualquer alteração de *BrowserCapabilitiesProvider* classe deve ocorrer antes de executa qualquer código no aplicativo, para certificar-se de que o cache permanece em um estado válido para o resolvido *HttpCapabilitiesBase* objeto.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Armazenamento em cache o objeto HttpBrowserCapabilities

O exemplo anterior tem um problema, o que é que o código será executado sempre que o provedor personalizado é chamado para obter o *HttpBrowserCapabilities* objeto. Isso pode ocorrer várias vezes durante cada solicitação. No exemplo, o código para o provedor não faz muito. No entanto, se o código no seu provedor personalizado executa um trabalho significativo em ordem para obter o *HttpBrowserCapabilities* do objeto, isso pode afetar o desempenho. Para evitar que isso aconteça, você pode armazenar em cache o *HttpBrowserCapabilities* objeto. Siga estas etapas:

1. Criar uma classe que deriva de *HttpCapabilitiesProvider*, conforme mostrado no exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    No exemplo, o código gera uma chave de cache chamando um método BuildCacheKey personalizado e obtém o período de tempo para cache chamando um método GetCacheTime personalizado. O código, em seguida, adiciona o resolvido *HttpBrowserCapabilities* objeto ao cache. O objeto pode ser recuperado do cache e reutilizado em solicitações subsequentes que usam o provedor personalizado.
2. Registre o provedor com o aplicativo conforme descrito no procedimento anterior.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Estendendo a funcionalidade de recursos do navegador do ASP.NET

A seção anterior descreveu como criar um novo *HttpBrowserCapabilities* objeto no ASP.NET 4. Você também pode estender a funcionalidade de recursos de navegador do ASP.NET adicionando novas definições de recursos do navegador para aqueles que já estão no ASP.NET. Você pode fazer isso sem usar as definições de navegador do XML. O procedimento a seguir mostra como.

1. Criar uma classe que deriva de *HttpCapabilitiesEvaluator* e que substitui o *GetBrowserCapabilities* método, conforme mostrado no exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Este código primeiro usa a funcionalidade de recursos do ASP.NET navegador para tentar identificar o navegador. No entanto, se nenhum navegador é identificado com base nas informações definidas na solicitação de (ou seja, se o *navegador* propriedade o *HttpBrowserCapabilities* objeto é a cadeia de caracteres "Desconhecido"), as chamadas de código o provedor personalizado (MyBrowserCapabilitiesEvaluator) para identificar o navegador.
2. Registre o provedor com o aplicativo conforme descrito no exemplo anterior.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Estender a funcionalidade de recursos do navegador, adicionando novos recursos para definições de recursos existente

Além de criar um provedor de definição de navegador personalizado e criando dinamicamente novas definições de navegador, você pode estender as definições de navegador existentes com recursos adicionais. Isso permite que você use uma definição que é quase o que você deseja, mas não tiver apenas alguns recursos. Para fazer isso, use as etapas a seguir.

1. Criar uma classe que deriva de *HttpCapabilitiesEvaluator* e que substitui o *GetBrowserCapabilities* método, conforme mostrado no exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    O exemplo de código estende o ASP.NET existente *HttpCapabilitiesEvaluator* classe e obtém o *HttpBrowserCapabilities* objeto que corresponde à definição de solicitação atual usando o código a seguir :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    O código, em seguida, adicionar ou modificar um recurso para este navegador. Há duas maneiras de especificar um novo recurso do navegador:

    - Adicionar um par chave/valor para o *IDictionary* objeto que é exposto pelo *recursos* propriedade o *HttpCapabilitiesBase* objeto. No exemplo anterior, o código adiciona um recurso chamado MultiTouch com um valor de *true*.
    - Definir as propriedades existentes do *HttpCapabilitiesBase* objeto. No exemplo anterior, o código define o *quadros* propriedade *true*. Esta propriedade é simplesmente um acessador para a *IDictionary* objeto que é exposto pelo *recursos* propriedade. 

        > [!NOTE]
        > Observe que esse modelo se aplica a qualquer propriedade de *HttpBrowserCapabilities*, incluindo os adaptadores de controle.
2. Registre o provedor com o aplicativo conforme descrito no procedimento anterior.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Roteamento no ASP.NET 4

O ASP.NET 4 adiciona suporte interno para usar o roteamento com formulários da Web. Roteamento permite que configurar um aplicativo para aceitar solicitação de URLs que não mapeiam para arquivos físicos. Em vez disso, você pode usar o roteamento para definir URLs que são significativos para usuários e que podem ajudar com o mecanismo de pesquisa SEO (otimização) para seu aplicativo. Por exemplo, a URL para uma página que exibe as categorias de produtos em um aplicativo existente pode parecer com o exemplo a seguir:

[!code-console[Main](overview/samples/sample36.cmd)]

Usando o roteamento, você pode configurar o aplicativo para aceitar a URL a seguir para processar as mesmas informações:

[!code-console[Main](overview/samples/sample37.cmd)]

Roteamento foi disponível a partir do ASP.NET 3.5 SP1. (Para obter um exemplo de como usar o roteamento no ASP.NET 3.5 SP1, consulte a entrada [usando roteamento com WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "título desta entrada.") blog de Phil Haack.) No entanto, o ASP.NET 4 inclui alguns recursos que tornam mais fácil de usar o roteamento, incluindo o seguinte:

- O *PageRouteHandler* classe, que é um manipulador HTTP simple que você usa quando você definir rotas. A classe transmite dados para a página que a solicitação é encaminhada para.
- As novas propriedades *HttpRequest.RequestContext* e *Page.RouteData* (que é um proxy para o *HttpRequest.RequestContext.RouteData* objeto). Essas propriedades facilitam para acessar as informações que são passadas de rota.
- Os seguintes construtores de expressão novo, que são definidos no *System.Web.Compilation.RouteUrlExpressionBuilder* e *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, que fornece uma maneira simple de criar uma URL que corresponde a uma URL de rota em um controle de servidor ASP.NET.
- *RouteValue*, que fornece uma maneira simples para extrair as informações do *RouteContext* objeto.
- O *RouteParameter* classe, o que torna mais fácil passar os dados contidos em um *RouteContext* objeto para uma consulta para um controle de fonte de dados (semelhante a [ *FormParameter* ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Roteamento para páginas Web Forms

O exemplo a seguir mostra como definir uma rota de Web Forms usando o novo *MapPageRoute* método o *rota* classe:

[!code-csharp[Main](overview/samples/sample38.cs)]

O ASP.NET 4 apresenta o *MapPageRoute* método. O exemplo a seguir é equivalente à definição de SearchRoute mostrada no exemplo anterior, mas usa o *PageRouteHandler* classe.

[!code-csharp[Main](overview/samples/sample39.cs)]

O código de exemplo mapeia a rota para uma página física (na rota primeiro, para `~/search.aspx`). A primeira definição de rota também especifica que o parâmetro denominado searchterm deve ser extraído da URL e passado para a página.

O *MapPageRoute* método oferece suporte as sobrecargas de método a seguir:

- *MapPageRoute (routeName de cadeia de caracteres, cadeia de caracteres routeUrl, physicalFile de cadeia de caracteres, checkPhysicalUrlAccess bool)*
- *MapPageRoute (routeName de cadeia de caracteres, routeUrl de cadeia de caracteres, physicalFile de cadeia de caracteres, bool checkPhysicalUrlAccess, RouteValueDictionary padrões)*
- *MapPageRoute (routeName de cadeia de caracteres, routeUrl de cadeia de caracteres, physicalFile de cadeia de caracteres, bool checkPhysicalUrlAccess, RouteValueDictionary padrões, restrições de RouteValueDictionary)*

O *checkPhysicalUrlAccess* parâmetro especifica se a rota deve verificar as permissões de segurança para a página física que está sendo roteado para (nesse caso, aspx) e as permissões na URL de entrada (nesse caso, pesquisar / {searchterm}). Se o valor de *checkPhysicalUrlAccess* é *false*, somente as permissões da URL de entrada serão verificadas. Essas permissões são definidas no `Web.config` arquivo usando as configurações como o seguinte:

[!code-xml[Main](overview/samples/sample40.xml)]

Na configuração de exemplo, o acesso foi negado para a página física `search.aspx` para todos os usuários, exceto aqueles que estão na função de administrador. Quando o *checkPhysicalUrlAccess* parâmetro está definido como *true* (que é o valor padrão), somente os usuários administrativos têm permissão para acessar o /search/ URL {searchterm}, porque é o aspx página física restrito a usuários nessa função. Se *checkPhysicalUrlAccess* é definido como *false* e o site está configurado como mostrado no exemplo anterior, todos os usuários autenticados têm permissão para acessar a URL /search/ {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Ler informações de roteamento em uma página Web Forms

No código da página física formulários da Web, você pode acessar as informações de roteamento foi extraído da URL (ou outras informações que o outro objeto foi adicionado para o *RouteData* objeto) usando duas novas propriedades:  *HttpRequest.RequestContext* e *Page.RouteData*. (*Page.RouteData* encapsula *HttpRequest.RequestContext.RouteData*.) O exemplo a seguir mostra como usar *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

O código extrai o valor passado para o parâmetro searchterm, conforme definido na rota de exemplo anterior. Considere a seguinte URL de solicitação:

[!code-console[Main](overview/samples/sample42.cmd)]

Quando essa solicitação é feita, a palavra "scott" seria processados no `search.aspx` página.

#### <a name="accessing-routing-information-in-markup"></a>Acessando informações de roteamento na marcação

O método descrito na seção anterior mostra como obter dados de rota no código em uma página Web Forms. Você também pode usar expressões na marcação que lhe dará acesso às mesmas informações. Construtores de expressões são uma maneira eficiente e elegante para trabalhar com o código declarativo. (Para obter mais informações, consulte a entrada [Express por conta própria com personalizado construtores de expressões](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) no blog de Phil Haack.)

O ASP.NET 4 inclui dois novos construtores de expressões para roteamento de formulários da Web. O exemplo a seguir mostra como usá-los.

[!code-aspx[Main](overview/samples/sample43.aspx)]

No exemplo, o *RouteUrl* expressão é usada para definir uma URL que é baseada em um parâmetro de rota. Isso evita que você tenha para codificar a URL completa na marcação e lhe permite alterar a estrutura de URL mais tarde sem exigir qualquer alteração para este link.

Essa marcação com base na rota definida anteriormente, gera a seguinte URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET estabelece automaticamente a rota correta (ou seja, ele gera a URL correta) com base em parâmetros de entrada. Você também pode incluir um nome de rota na expressão, que permite que você especifique uma rota para usar.

O exemplo a seguir mostra como usar o *RouteValue* expressão.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Quando a página que contém este controle é executado, o valor "scott" é exibido no rótulo.

O *RouteValue* expressão torna simples para usar dados de rota na marcação e evitar ter que trabalhar com os mais complexos Page.RouteData["x"] sintaxe na marcação.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Usando dados de rota para parâmetros de controle de fonte de dados

O *RouteParameter* classe permite que você especifique os dados de rota como um valor de parâmetro para consultas em um controle de fonte de dados. Ele [funciona bem como o](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.formparameter.aspx) de classe, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample46.aspx)]

Nesse caso, o valor de searchterm de parâmetro de rota será usado para o @companyname parâmetro o *selecione* instrução.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>IDs de configuração de cliente

O novo *ClientIDMode* propriedade soluciona um problema longa no ASP.NET, ou seja, como criar controles de *id* atributo para elementos que eles processem. Saber o *id* atributo para elementos renderizados é importante se o aplicativo incluir um script de cliente que faz referência a esses elementos.

O *id* atributo em HTML que é renderizado para controles de servidor Web é gerado com base no *ClientID* propriedade do controle. Até que o ASP.NET 4, o algoritmo para gerar o *id* de atributo do *ClientID* propriedade tiver sido concatenar o contêiner de nomenclatura (se houver) com a ID e, no caso de controles repetidos (como em controles de dados), para adicionar um prefixo e um número sequencial. Enquanto isso sempre tem garantia de que as IDs dos controles na página são exclusivas, o algoritmo resultou em identificações de controle que não eram previsível e, portanto, eram difíceis de referência no script de cliente.

O novo *ClientIDMode* propriedade permite especificar mais precisamente como a ID do cliente é gerada para controles. Você pode definir o *ClientIDMode* propriedade para qualquer controle, inclusive para a página. Configurações possíveis são os seguintes:

- *AutoID* – isso é equivalente ao algoritmo para gerar *ClientID* valores de propriedade que foi usado em versões anteriores do ASP.NET.
- *Estático* – Especifica que o *ClientID* valor será igual à ID sem concatenando as IDs do pai contêiners de nomeação. Isso pode ser útil em controles de usuário da Web. Como um controle de usuário da Web pode ser localizado em diferentes páginas e controles de contêiner diferentes, pode ser difícil de escrever script de cliente para controles que usam o *AutoID* algoritmo porque você não pode prever quais serão os valores de ID .
- *Previsível* – essa opção é principalmente para uso em controles de dados que usam modelos de repetição. Ele concatena as propriedades de ID de contêineres de nomenclatura do controle, mas gerado *ClientID* valores não contêm cadeias de caracteres como "ctlxxx". Essa configuração funciona em conjunto com o *ClientIDRowSuffix* propriedade do controle. Definir o *ClientIDRowSuffix* propriedade para o nome de um campo de dados e o valor desse campo é usada como o sufixo para gerado *ClientID* valor. Normalmente, você poderia usar a chave primária de um registro de dados como o *ClientIDRowSuffix* valor.
- *Herdar* – essa configuração é o comportamento padrão de controles; Especifica que a geração de ID do controle é o mesmo que seu pai.

Você pode definir o *ClientIDMode* propriedade no nível da página. Isso define o padrão *ClientIDMode* valor para todos os controles na página atual.

O padrão *ClientIDMode* valor no nível da página é *AutoID*e o padrão *ClientIDMode* valor no nível de controle é *herdar*. Como resultado, se você não definir essa propriedade em qualquer lugar no seu código, todos os controles padrão serão o *AutoID* algoritmo.

Definir o valor de nível de página no *@ Page* diretiva, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Você também pode definir o *ClientIDMode* valor no arquivo de configuração no nível do computador (máquina) ou no nível do aplicativo. Isso define o padrão *ClientIDMode* configuração para todos os controles em todas as páginas do aplicativo. Se você definir o valor no nível do computador, ele define o padrão *ClientIDMode* configuração para todos os sites no computador. A exemplo a seguir mostra o *ClientIDMode* configuração no arquivo de configuração:

[!code-xml[Main](overview/samples/sample48.xml)]

Conforme observado anteriormente, o valor de *ClientID* propriedade é derivada do contêiner de nomenclatura para o pai do controle. Em alguns cenários, como quando você estiver usando páginas mestras, controles podem acabar com IDs como aqueles no exemplo a seguir renderizado HTML:

[!code-html[Main](overview/samples/sample49.html)]

Embora o *entrada* mostrado na marcação de elemento (de um *TextBox* controle) é apenas dois contêineres de nomenclatura profundo na página (aninhada *ContentPlaceholder* controles), devido à maneira como páginas mestras são processadas, o resultado final é uma ID de controle com o seguinte:

[!code-console[Main](overview/samples/sample50.cmd)]

Essa ID é garantida como exclusivo na página, mas é desnecessariamente longo para a maioria das finalidades. Imagine que você deseja reduzir o comprimento da ID renderizado e para ter mais controle sobre como a ID é gerada. (Por exemplo, você deseja eliminar os prefixos "ctlxxx".) É a maneira mais fácil de fazer isso, definindo o *ClientIDMode* propriedade conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample51.aspx)]

Neste exemplo, o *ClientIDMode* está definida como *estático* para externo *NamingPanel* elemento e definido como *previsível* para interna *NamingControl* elemento. Essas configurações resultam na seguinte marcação (o restante da página e a página mestra deve para ser o mesmo como no exemplo anterior):

[!code-html[Main](overview/samples/sample52.html)]

O *estático* configuração tem o efeito de redefinir a hierarquia de nomeação para os controles dentro externo *NamingPanel* elemento e de eliminar o *ContentPlaceHolder* e *MasterPage* IDs da identificação do gerado. (O *nome* atributo de elementos renderizados é afetado, para a funcionalidade normal do ASP.NET é retida por eventos, o estado de exibição e assim por diante.) Um efeito colateral de redefinir a hierarquia de nomeação é que, mesmo se você mover a marcação para o *NamingPanel* elementos para outro *ContentPlaceholder* controle, as IDs de cliente processada permanecem os mesmos.

> [!NOTE]
> Observe que cabe a você para certificar-se de que as IDs do controle processado são exclusivas. Se não forem, elas poderão interromper qualquer funcionalidade que exija IDs exclusivas para elementos HTML individuais, como o cliente *getElementById* função.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Criando as IDs de cliente previsível em controles associados a dados

O *ClientID* valores que são gerados para controles em um controle de lista associado a dados pelo algoritmo herdado podem ser de comprimento e não são realmente previsíveis. O *ClientIDMode* funcionalidade pode ajudá-lo a ter mais controle sobre como essas IDs são gerados.

A marcação no exemplo a seguir inclui um *ListView* controle:

[!code-aspx[Main](overview/samples/sample53.aspx)]

No exemplo anterior, o *ClientIDMode* e *RowClientIDRowSuffix* propriedades são definidas na marcação. O *ClientIDRowSuffix* propriedade pode ser usada somente em controles associados a dados e seu comportamento difere dependendo de qual controle você está usando. As diferenças são estes:

- *GridView* controle — você pode especificar o nome de uma ou mais colunas na fonte de dados, que são combinados em tempo de execução para criar o cliente IDs. Por exemplo, se você definir *RowClientIDRowSuffix* para "ProductName, ProductId," controlar IDs para elementos renderizados terá um formato semelhante ao seguinte:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* controle — você pode especificar uma única coluna na fonte de dados que é anexada à ID de cliente. Por exemplo, se você definir *ClientIDRowSuffix* para "ProductName", o controle renderizado IDs terá um formato semelhante ao seguinte:

- [!code-console[Main](overview/samples/sample55.cmd)]

- Nesse caso o 1 à direita é derivado da ID do produto do item de dados atual.

- *Repetidor* controle — esse controle não dá suporte a *ClientIDRowSuffix* propriedade. Em um *repetidor* controle, o índice da linha atual é usado. Quando você usa ClientIDMode = "Previsível" com um *repetidor* controlar, IDs são gerados de cliente que têm o seguinte formato:

- [!code-console[Main](overview/samples/sample56.cmd)]

- A 0 à direita é o índice da linha atual.

O *FormView* e *DetailsView* controles não exibem várias linhas, para que eles não dão suporte a *ClientIDRowSuffix* propriedade.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Manter seleção de linha em controles de dados

O *GridView* e *ListView* controles podem permitir que os usuários selecionam uma linha. Nas versões anteriores do ASP.NET, seleção foi baseada no índice da linha na página. Por exemplo, se você selecionar o terceiro item na página 1 e, em seguida, mova para a página 2, o terceiro item nessa página é selecionado.

Seleção persistente foi inicialmente tem suporte apenas nos projetos de dados dinâmicos no .NET Framework 3.5 SP1. Quando esse recurso está habilitado, o item selecionado atual se baseia a chave de dados para o item. Isso significa que, se você selecionar a terceira linha na página 1 e move para a página 2, nada será selecionado na página 2. Quando você voltar à página 1, a terceira linha ainda está selecionada. Agora há suporte para seleção persistente para o *GridView* e *ListView* controles em todos os projetos usando o *EnablePersistedSelection* propriedade, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Controle de gráfico do ASP.NET

O ASP.NET *gráfico* controle expande as ofertas de visualização de dados do .NET Framework. Usando o *gráfico* controle, você pode criar páginas ASP.NET que tenham gráficos intuitivos e visualmente atraentes para análise estatística ou financeiro complexa. O ASP.NET *gráfico* controle foi introduzido como um complemento para a versão do .NET Framework versão 3.5 SP1 e é parte da versão do .NET Framework 4.

O controle inclui os seguintes recursos:

- tipos de 35 distintas de gráfico.
- Um número ilimitado de áreas de gráfico, títulos, legendas e anotações.
- Uma ampla variedade de configurações de aparência para todos os elementos do gráfico.
- Suporte para a maioria dos tipos de gráfico 3D.
- Rótulos de dados inteligentes que podem se ajustar automaticamente em torno de pontos de dados.
- Faixas, quebras de escala e a escala logarítmica.
- Mais de 50 fórmulas financeiras e estatísticas para análise de dados e transformação.
- Associação simples e manipulação de dados do gráfico.
- Suporte para formatos de dados comuns, como datas, horas e moedas.
- Suporte para interatividade e personalização controlada por evento, incluindo o cliente, clique em eventos usando Ajax.
- Gerenciamento de estado.
- Fluxo binário.

As figuras a seguir mostram exemplos de gráficos financeiros que são produzidos pelo controle do gráfico do ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figura 2: Exemplos de controle de gráfico do ASP.NET

Para obter mais exemplos de como usar o controle de gráfico do ASP.NET, baixe o código de exemplo no [ambiente de exemplos para Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) página no site do MSDN. Você pode encontrar mais amostras da comunidade de conteúdo no [Fórum de controle de gráfico](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Adicionando o controle de gráfico para uma página ASP.NET

O exemplo a seguir mostra como adicionar um *gráfico* controle a uma página ASP.NET usando a marcação. No exemplo, o *gráfico* controle produz um gráfico de coluna para os pontos de dados estáticos.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Usando gráficos 3D

O *gráfico* controle contém um *ChartAreas* coleção, que pode conter *ChartArea* objetos que definem as características de áreas de gráfico. Por exemplo, para usar 3D para uma área do gráfico, use o *Area3DStyle* propriedade, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample59.aspx)]

A figura a seguir mostra um gráfico 3D com quatro séries do *barra* tipo de gráfico.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figura 3: gráfico de barras 3D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Usando quebras de escala e escalas logarítmicas

Quebras de escala e escalas logarítmicas são duas outras maneiras de adicionar sofisticação ao gráfico. Esses recursos são específicos para cada eixo em uma área do gráfico. Por exemplo, para usar esses recursos no eixo Y primário de uma área do gráfico, use o *AxisY.IsLogarithmic* e *ScaleBreakStyle* propriedades em um *ChartArea* objeto. O trecho a seguir mostra como usar quebras de escala no eixo Y primário.

[!code-aspx[Main](overview/samples/sample60.aspx)]

A figura a seguir mostra o eixo Y com quebras de escala habilitadas.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figura 4: Quebras de escala

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrando dados com o controle QueryExtender

É uma tarefa muito comum para os desenvolvedores que criam sites controlados por dados filtrar dados. Isso tradicionalmente foi executado pela criação de *onde* cláusulas nos dados de origem controles. Essa abordagem pode ser complicada e, em alguns casos o *onde* sintaxe não permite que você aproveite a funcionalidade completa do banco de dados subjacente.

Para tornar mais fácil, filtragem de um novo *QueryExtender* controle foi adicionado no ASP.NET 4. Este controle pode ser adicionado ao *EntityDataSource* ou *LinqDataSource* controles para filtrar os dados retornados por esses controles. Porque o *QueryExtender* controle depende de LINQ, o filtro é aplicado no servidor de banco de dados antes dos dados são enviados para a página, o que resulta em operações muito eficientes.

O *QueryExtender* controle oferece suporte a uma variedade de opções de filtro. As seções a seguir descrevem essas opções e fornecem exemplos de como usá-los.

#### <a name="search"></a>Pesquisar

Para a opção de pesquisa, o *QueryExtender* controle executa uma pesquisa em campos especificados. No exemplo a seguir, o controle usa o texto que é inserido no controle TextBoxSearch e procura por seu conteúdo no `ProductName` e `Supplier.CompanyName` colunas nos dados que são retornados o *LinqDataSource* controle.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Intervalo

A opção de intervalo é semelhante à opção de pesquisa, mas Especifica um par de valores para definir o intervalo. No exemplo a seguir, o *QueryExtender* pesquisas de controlar o `UnitPrice` coluna nos dados retornados do *LinqDataSource* controle. O intervalo é de leitura dos controles TextBoxFrom e TextBoxTo na página.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

A opção de expressão de propriedade permite definir uma comparação para um valor de propriedade. Se a expressão for avaliada como *true*, os dados que estão sendo examinados são retornados. No exemplo a seguir, o *QueryExtender* controle filtra dados comparando os dados a `Discontinued` coluna para o valor do controle CheckBoxDiscontinued na página.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Por fim, você pode especificar uma expressão personalizada para usar com o *QueryExtender* controle. Essa opção permite que você chame uma função na página que define a lógica de filtro personalizado. O exemplo a seguir mostra como especificar declarativamente uma expressão personalizada no *QueryExtender* controle.

[!code-aspx[Main](overview/samples/sample64.aspx)]

O exemplo a seguir mostra a função personalizada que é invocada com o *QueryExtender* controle. Nesse caso, em vez de usar uma consulta de banco de dados que inclui um *onde* cláusula, o código usa uma consulta LINQ para filtrar os dados.

[!code-csharp[Main](overview/samples/sample65.cs)]

Estes exemplos mostram apenas uma expressão que está sendo usada no *QueryExtender* controle por vez. No entanto, você pode incluir várias expressões dentro do *QueryExtender* controle.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Expressões de código codificada em HTML

Alguns sites ASP.NET (especialmente com o ASP.NET MVC) dependem fortemente usando `<%` =  `expression %>` sintaxe (geralmente chamado de "código novidades sobre") para gravar algum texto para a resposta. Quando você usar expressões de código, é fácil esquecer de entrada de texto, se o texto vem do usuário, ele pode deixar páginas aberto a ataques XSS (Cross Site Scripting) com codificação HTML.

O ASP.NET 4 apresenta a nova sintaxe para expressões de código a seguir:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Essa sintaxe usa a codificação HTML por padrão ao gravar a resposta. Esta nova expressão efetivamente converte para o seguinte:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Por exemplo, &lt;%: solicitação ["UserInput"] %&gt; executa a codificação HTML no valor de *solicitação ["UserInput"]*.

O objetivo desse recurso é para que seja possível substituir todas as instâncias da sintaxe antiga com a nova sintaxe para que você não é obrigado a decidir qual delas usar cada etapa. No entanto, há casos em que o texto que está sendo a saída deve ser HTML ou já está codificado, caso em que isso pode levar a codificação dupla.

Para esses casos, o ASP.NET 4 apresenta uma nova interface, *IHtmlString*, juntamente com uma implementação concreta, *HtmlString*. Instâncias desses tipos permitem que você indicar que o valor de retorno é codificado já corretamente (ou caso contrário examinado) para exibir como HTML, e que, portanto, o valor não deve ser codificada em HTML novamente. Por exemplo, o seguinte não deve ser (e não é) codificada em HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Métodos de auxiliares do ASP.NET MVC 2 tem sido atualizado para trabalhar com essa nova sintaxe para que eles não sejam duplos codificado, mas somente quando você estiver executando o ASP.NET 4. Essa nova sintaxe não funciona quando você executar um aplicativo usando o ASP.NET 3.5 SP1.

Tenha em mente que isso não garante a proteção contra ataques XSS. Por exemplo, HTML, que usa valores de atributo que não estão entre aspas pode conter entradas do usuário que ainda são suscetíveis. Observe que a saída de controles do ASP.NET e auxiliares do ASP.NET MVC sempre inclui valores de atributo entre aspas, que é a abordagem recomendada.

Da mesma forma, essa sintaxe não executar a codificação de JavaScript, como quando você cria uma cadeia de caracteres de JavaScript com base na entrada do usuário.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Alterações do modelo de projeto

Em versões anteriores do ASP.NET, quando você usa o Visual Studio para criar um novo projeto de Site da Web ou um projeto de aplicativo Web, os projetos resultantes contém apenas uma página Default.aspx, um padrão `Web.config` arquivo e o `App_Data` pasta, conforme mostrado no exemplo a seguir ilustração:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

O Visual Studio também oferece suporte a um tipo de projeto de Site da Web vazio, que não contém arquivos em todos os, conforme mostrado na figura a seguir:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

O resultado é que para os iniciantes, há muito pouca orientação sobre como criar um aplicativo Web de produção. Portanto, o ASP.NET 4 apresenta três novos modelos, um para um projeto de aplicativo Web vazio e um para um projeto de aplicativo Web e o Site da Web.

#### <a name="empty-web-application-template"></a>Modelo de aplicativo Web vazio

Como o nome sugere, o modelo de aplicativo Web vazio é um projeto de aplicativo Web simplificado. Você pode selecionar este modelo de projeto na caixa de diálogo Novo projeto do Visual Studio, conforme mostrado na figura a seguir:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image8.png))

Quando você cria um aplicativo de Web ASP.NET vazio, o Visual Studio cria o layout de pasta a seguir:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Isso é semelhante ao layout do Site da Web vazio de versões anteriores do ASP.NET, com uma exceção. No Visual Studio 2010, projetos de aplicativo Web vazio e o Site da Web vazio contêm o seguinte mínimo `Web.config` arquivo que contém informações usadas pelo Visual Studio para identificar a estrutura que o projeto está definido:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Sem isso *targetFramework* propriedade, padrões do Visual Studio para direcionamento do .NET Framework 2.0 para preservar a compatibilidade ao abrir os aplicativos mais antigos.

#### <a name="web-application-and-web-site-project-templates"></a>Aplicativo Web e modelos de projeto de Site da Web

Os outros dois novos modelos de projeto que são fornecidos com o Visual Studio 2010 contêm alterações principais. A figura a seguir mostra o layout de projeto que é criado quando você cria um novo projeto de aplicativo Web. (O layout de um projeto de Site da Web é praticamente idêntico).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

O projeto inclui um número de arquivos que não foram criados em versões anteriores. Além disso, o novo projeto de aplicativo Web é configurado com a funcionalidade de associação básica, o que permite que você a começar rapidamente na proteção de acesso para o novo aplicativo. Devido a essa inclusão a `Web.config` arquivo para o novo projeto inclui entradas que são usadas para configurar a associação, funções e perfis. A exemplo a seguir mostra o `Web.config` arquivo para um novo projeto de aplicativo Web. (Nesse caso, *roleManager* está desabilitado.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image14.png))

O projeto contém também uma segunda `Web.config` arquivo o `Account` directory. O segundo arquivo de configuração fornece uma maneira de proteger o acesso à página ChangePassword para não-os usuários conectados. O exemplo a seguir mostra o conteúdo do segundo `Web.config` arquivo.

![](overview/_static/image15.png)

As páginas criadas por padrão em novos modelos de projeto também contêm conteúdo mais do que nas versões anteriores. O projeto contém uma página mestra padrão e o arquivo CSS, e a página padrão (Default.aspx) é configurada para usar a página mestra por padrão. O resultado é que, quando você executa o aplicativo Web ou site pela primeira vez, a página (base) padrão já é funcional. Na verdade, é semelhante para a página padrão que você vê quando você iniciar um novo aplicativo MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image18.png))

A intenção dessas alterações nos modelos de projeto é fornecer orientação sobre como começar a criar um novo aplicativo Web. Com semanticamente correta estrito XHTML 1.0 compatível com marcação e o layout especificado por meio de CSS, as páginas nos modelos representam as práticas recomendadas para a criação de aplicativos Web do ASP.NET 4. As páginas padrão também têm um layout de duas colunas que você pode personalizar facilmente.

Por exemplo, imagine que, para um novo aplicativo Web você deseja alterar algumas das cores e inserir o logotipo da empresa no lugar do logotipo do meu aplicativo ASP.NET. Para fazer isso, você cria um novo diretório em `Content` para armazenar sua imagem de logotipo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Para adicionar a imagem para a página, em seguida, abrir o `Site.Master` de arquivos, localizar onde o texto de meu aplicativo ASP.NET é definido e substituí-lo por um *imagem* elemento cujo *src* atributo é definido como o novo logotipo imagem, como no exemplo a seguir:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image22.png))

Você pode ir para o arquivo Site.css e modificar definições de classe CSS para alterar a cor de plano de fundo da página, bem como que o cabeçalho.

O resultado dessas alterações é que você pode exibir uma página inicial personalizada com muito pouco esforço:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Aprimoramentos de CSS

Uma das principais áreas de trabalho no ASP.NET 4 foi ajudam a renderizar HTML que está em conformidade com os padrões mais recentes de HTML. Isso inclui alterações em como os controles de servidor Web do ASP.NET usam estilos CSS.

#### <a name="compatibility-setting-for-rendering"></a>Configuração de compatibilidade de renderização

Por padrão, quando um aplicativo Web ou site da Web tem como alvo o .NET Framework 4, o *controlRenderingCompatibilityVersion* atributo o *páginas* é definido como "4.0". Esse elemento é definido no nível do computador `Web.config` de arquivo e por padrão se aplica a todos os aplicativos ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

O valor de *controlRenderingCompatibility* é uma cadeia de caracteres, o que permite que novas definições de versão potencial em versões futuras. Na versão atual, os valores a seguir têm suporte para essa propriedade:

- "3.5". Essa configuração indica a marcação e renderização de herança. Marcação processada por controles é 100% compatível com versões anteriores e a configuração do *xhtmlConformance* propriedade é respeitada.
- "4.0". Se a propriedade tem essa configuração, os controles de servidor Web do ASP.NET faça o seguinte:
- O *xhtmlConformance* propriedade sempre é tratada como "Strict". Como resultado, os controles renderizar marcação XHTML 1.0 Strict.
- Desabilitando os controles de entrada não não processa estilos inválidos.
- *div* elementos ao redor de campos ocultos são denominados agora para que eles não interfere com regras CSS criadas pelo usuário.
- Controles de menu renderizam a marcação que é semanticamente correta e em conformidade com as diretrizes de acessibilidade.
- Controles de validação não processam estilos embutidos.
- Controla que anteriormente renderizado border = "0" (controles que derivam do ASP.NET *tabela* controle e o ASP.NET *imagem* controle) não processará mais esse atributo.

#### <a name="disabling-controls"></a>A desabilitação dos controles

No ASP.NET 3.5 SP1 e versões anteriores, a estrutura renderiza o *desabilitado* atributo na marcação HTML para qualquer controle cuja *habilitado* propriedade definida como *false*. No entanto, de acordo com a especificação do HTML 4.01, apenas *entrada* elementos devem ter este atributo.

No ASP.NET 4, você pode definir o *controlRenderingCompatabilityVersion* propriedade como "3.5", como no exemplo a seguir:

[!code-xml[Main](overview/samples/sample70.xml)]

Você pode criar marcação para um *rótulo* controle semelhante à seguinte, o que desabilita o controle:

[!code-aspx[Main](overview/samples/sample71.aspx)]

O *rótulo* controle deve processar o HTML a seguir:

[!code-html[Main](overview/samples/sample72.html)]

No ASP.NET 4, você pode definir o *controlRenderingCompatabilityVersion* como "4.0". Nesse caso, apenas controla que renderizam *entrada* elementos processará uma *desabilitado* atributo quando o controle *habilitado* está definida como *false* . Controles que não processam HTML *entrada* elementos renderizados em vez disso, um *classe* atributo que faz referência a uma classe CSS que você pode usar para definir uma aparência desabilitada para o controle. Por exemplo, o *rótulo* controle mostrado no exemplo anterior produziria a seguinte marcação:

[!code-html[Main](overview/samples/sample73.html)]

O valor padrão para a classe especificada para este controle é "aspNetDisabled". No entanto, você pode alterar esse valor padrão definindo estático *DisabledCssClass* propriedade estática do *WebControl* classe. Para desenvolvedores de controle, o comportamento a ser usado para um controle específico também pode ser definido usando o *SupportsDisabledAttribute* propriedade.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ocultando div elementos em torno de campos ocultos

O ASP.NET 2.0 e versões posteriores renderizam campos ocultos específicos do sistema (como o *oculta* elemento usado para armazenar informações de estado de exibição) dentro de *div* elemento para manter a conformidade com o padrão XHTML. No entanto, isso pode causar um problema quando uma regra de CSS afeta *div* elementos em uma página. Por exemplo, ele pode resultar em uma linha de um pixel que aparecem na página de aproximadamente oculto *div* elementos. No ASP.NET 4, *div* elementos que inclua os campos ocultos gerados pelo ASP.NET adicionam uma referência de classe CSS como no exemplo a seguir:

[!code-html[Main](overview/samples/sample74.html)]

Você pode definir uma classe CSS que se aplica somente ao *oculta* elementos que são gerados pelo ASP.NET, como no exemplo a seguir:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Renderização de uma tabela externa para controles modelos

Por padrão, os seguintes controles de servidor Web do ASP.NET que oferecem suporte a modelos automaticamente dispostos em uma tabela externa que é usada para aplicar estilos embutidos:

- *FormView*
- *Logon*
- *PasswordRecovery*
- *ChangePassword*
- *Assistente*
- *CreateUserWizard*

Uma nova propriedade chamada *RenderOuterTable* foi adicionado para esses controles que permite que a tabela externa a ser removido da marcação. Por exemplo, considere o seguinte exemplo de uma *FormView* controle:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Essa marcação processa a saída para a página, que inclui uma tabela HTML a seguir:

[!code-html[Main](overview/samples/sample77.html)]

Para impedir que a tabela está sendo processado, você pode definir o *FormView* do controle *RenderOuterTable* propriedade, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample78.aspx)]

O exemplo anterior renderiza a seguinte saída, sem o *tabela*, *tr*, e *td* elementos:

> Conteúdo


Esse aprimoramento pode facilitar a estilo o conteúdo do controle com o CSS, porque nenhuma marcação inesperadas é processadas pelo controle.

> [!NOTE]
> Observe que esta alteração desabilita o suporte para a função Formatar automaticamente no Visual Studio 2010 designer, porque não há um *tabela* elemento que pode hospedar os atributos de estilo que são gerados pela opção de formatação automática.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Aprimoramentos do controle ListView

O *ListView* controle foi feito mais fácil de usar no ASP.NET 4. A versão anterior do controle necessário especificar um modelo de layout que continha um controle de servidor com uma ID conhecida. A marcação a seguir mostra um exemplo típico de como usar o *ListView* controle no ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

No ASP.NET 4, o *ListView* controle não requer um modelo de layout. A marcação mostrada no exemplo anterior pode ser substituída com a seguinte marcação:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Aprimoramentos de controle RadioButtonList e CheckBoxList

No ASP.NET 3.5, você pode especificar o layout para o *CheckBoxList* e *RadioButtonList* usando duas configurações a seguir:

- *Fluxo de*. O controle processa *abrangem* elementos para conter o seu conteúdo.
- *Tabela*. O controle apresenta um *tabela* elemento para conter o seu conteúdo.

O exemplo a seguir mostra a marcação de cada um desses controles.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Por padrão, os controles de renderização HTML semelhante à seguinte:

[!code-html[Main](overview/samples/sample82.html)]

Como esses controles contêm listas de itens para renderizar HTML semanticamente correta, eles devem ser renderizados seu conteúdo usando a lista em HTML (*li*) elementos. Isso torna mais fácil para os usuários que ler páginas da Web usando a tecnologia assistencial e facilita os controles de estilo de CSS.

No ASP.NET 4, o *CheckBoxList* e *RadioButtonList* controles suportam os seguintes novos valores para o *RepeatLayout* propriedade:

- *OrderedList* – o conteúdo é renderizado como *li* elementos dentro de um *ol* elemento.
- *UnorderedList* – o conteúdo é renderizado como *li* elementos dentro de um *ul* elemento.

O exemplo a seguir mostra como usar esses novos valores.

[!code-aspx[Main](overview/samples/sample83.aspx)]

A marcação anterior gera o HTML a seguir:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Observação: se você definir *RepeatLayout* para *OrderedList* ou *UnorderedList*, o *RepeatDirection* propriedade não pode mais ser usada e irá lançar uma exceção em tempo de execução se a propriedade foi definida na sua marcação ou código. A propriedade não teria nenhum valor porque o layout visual desses controles definido usando a CSS em vez disso.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Melhorias do menu de controle

Antes de ASP.NET 4, o *Menu* controle processado uma série de tabelas HTML. Isso tornou mais difícil aplicar estilos CSS fora definindo propriedades em linha e também não estava em conformidade com padrões de acessibilidade.

No ASP.NET 4, o controle agora renderiza HTML usando marcação semântica que consiste em uma lista não ordenada e elementos de lista. O exemplo a seguir mostra a marcação em uma página ASP.NET para o *Menu* controle.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Quando a página é processada, o controle produz o seguinte HTML (o *onclick* código foi omitido para maior clareza):

[!code-html[Main](overview/samples/sample86.html)]

Além das melhorias de renderização, navegação de teclado do menu foi aprimorada com gerenciamento de foco. Quando o *Menu* controle obtém foco, você pode usar as teclas de direção para navegar elementos. O *Menu* controle agora também anexa acessível funções de aplicativos (ARIA) de internet avançado e atributos a seguir[asa o](http://www.w3.org/TR/wai-aria-practices/#menu "diretrizes de Menu ARIA")para aprimorado acessibilidade.

Estilos para o controle de menu são renderizados em um bloco de estilo na parte superior da página, em vez de acordo com os elementos HTML renderizados. Se quiser assumir o controle completo sobre o estilo para o controle, você pode definir o novo *IncludeStyleBlock* propriedade *false*, caso em que o bloco de estilo não é emitido. Uma maneira de usar essa propriedade é usar o recurso de AutoFormatação no designer do Visual Studio para definir a aparência do menu. Em seguida, você pode executar a página, abra a origem da página e, em seguida, copie o bloco de estilo renderizado para um arquivo CSS externo. No Visual Studio, desfazer o estilo e o conjunto *IncludeStyleBlock* para *false*. O resultado é que a aparência de menu é definida usando estilos em uma folha de estilos externa.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Assistente e CreateUserWizard controles

O ASP.NET *assistente* e *CreateUserWizard* controles dão suporte a modelos que permitem que você defina o HTML que eles sejam renderizados. (*CreateUserWizard* deriva *assistente*.) O exemplo a seguir mostra a marcação para um modelo totalmente *CreateUserWizard* controle:

[!code-aspx[Main](overview/samples/sample87.aspx)]

O controle renderiza HTML semelhante à seguinte:

[!code-html[Main](overview/samples/sample88.html)]

No ASP.NET 3.5 SP1, embora você possa alterar o conteúdo do modelo, você ainda tiver limitados controle sobre a saída do *assistente* controle. No ASP.NET 4, você pode criar um *LayoutTemplate* modelo e insert *espaço reservado* controles (usando nomes reservados) para especificar como você deseja que o *controle Wizard* para processar. O exemplo a seguir mostra a isso:

[!code-aspx[Main](overview/samples/sample89.aspx)]

O exemplo contém o seguinte espaços reservados em nomeados o *LayoutTemplate* elemento:

- *headerPlaceholder* – em tempo de execução, este será substituído pelo conteúdo de *HeaderTemplate* elemento.
- *sideBarPlaceholder* – em tempo de execução, este será substituído pelo conteúdo de *SideBarTemplate* elemento.
- *wizardStepPlaceHolder* – em tempo de execução, este será substituído pelo conteúdo de *WizardStepTemplate* elemento.
- *navigationPlaceholder* – em tempo de execução, isso é substituído por nenhum modelo de navegação que você definiu.

A marcação no exemplo que usa espaços reservados renderiza HTML a seguir (sem o conteúdo, na verdade, definido em modelos):

[!code-html[Main](overview/samples/sample90.html)]

Somente o HTML é agora não definidas pelo usuário é um *abrangem* elemento. (Antecipamos ou em futuras versões, até mesmo o *abrangem* elemento não será processado.) Agora dá a você controle total sobre praticamente todo o conteúdo que é gerado pelo *assistente* controle.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC foi introduzido como uma estrutura de complemento para o ASP.NET 3.5 SP1 em março de 2009. Visual Studio 2010 inclui ASP.NET MVC 2, que inclui novos recursos e funcionalidades.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Suporte de áreas

Áreas permitem controladores de grupo e modos de exibição em seções de um aplicativo grande em isolamento relativo de outras seções. Cada área pode ser implementada como um projeto ASP.NET MVC separado que pode ser referenciado por principal do aplicativo. Isso ajuda a gerenciar a complexidade quando você compila um aplicativo grande e torna mais fácil para várias equipes trabalhem juntos em um único aplicativo.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Suporte à validação de atributo de anotação de dados

*DataAnnotations* atributos permitem que você anexar a lógica de validação a um modelo usando os atributos de metadados. *DataAnnotations* atributos foram introduzidos nos dados dinâmicos do ASP.NET no ASP.NET 3.5 SP1. Esses atributos foram integrados ao associador de modelo padrão e fornecem um meio orientados a metadados para validar a entrada do usuário.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Auxiliares modelo

Auxiliares modelo lhe permitem associar automaticamente editar e exibem modelos com tipos de dados. Por exemplo, você pode usar um auxiliar de modelo para especificar que um elemento de interface do usuário do seletor de data será renderizado automaticamente para um *System. DateTime* valor. Isso é semelhante aos modelos de campo nos dados dinâmicos do ASP.NET.

O *Html.EditorFor* e *Html.DisplayFor* métodos auxiliares têm suporte interno para renderização de dados padrão tipos de objetos complexos, bem como com várias propriedades. Eles também personalizar a renderização, permitindo que você aplicar atributos de anotação de dados como *DisplayName* e *ScaffoldColumn* para o *ViewModel* objeto.

Geralmente você deseja personalizar a saída de auxiliares de interface do usuário ainda mais e tem total controle sobre o que é gerado. O *Html.EditorFor* e *Html.DisplayFor* métodos auxiliares dar suporte a isso usando um mecanismo de modelagem que permite que você defina a modelos externos podem substituir e controle a saída renderizada. Os modelos podem ser processados individualmente para uma classe.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dados dinâmicos

Dados dinâmicos foi introduzidos na versão .NET Framework 3.5 SP1 em meados de 2008. Esse recurso oferece muitos aprimoramentos para a criação de aplicativos controlados por dados, incluindo o seguinte:

- Uma experiência RAD para criar rapidamente um site orientado a dados.
- Validação automática com base nas restrições definidas no modelo de dados.
- A capacidade de alterar facilmente a marcação que é gerada para campos de *GridView* e *DetailsView* controles usando modelos de campo que fazem parte de seu projeto de dados dinâmicos.

> [!NOTE]
> Observação: para obter mais informações, consulte o [documentação de dados dinâmicos](https://msdn.microsoft.com/en-us/library/cc488545.aspx) na biblioteca MSDN.


Para o ASP.NET 4, dados dinâmicos foi aprimorado para dar aos desenvolvedores ainda mais energia para criar rapidamente sites controlados por dados.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Habilitar dados dinâmicos para projetos existentes

Recursos dinâmicos de dados fornecido com o .NET Framework 3.5 SP1 colocado novos recursos, como o seguinte:

- Modelos de campo – elas fornecem modelos com base em tipo de dados para controles associados a dados. Modelos de campo fornecem uma maneira simples de personalizar a aparência dos controles de dados que o uso de campos do modelo para cada campo.
- A validação – dados dinâmicos permite utilizar atributos nas classes de dados para especificar a validação para cenários comuns, como campos obrigatórios, verificação de intervalo, verificação de tipo, usando expressões regulares de correspondência de padrões e validação personalizada. A validação é imposta pelos controles de dados.

No entanto, esses recursos tinham os seguintes requisitos:

- A camada de acesso a dados precisava ser baseado em LINQ to SQL ou do Entity Framework.
- A únicos fonte de dados controles com suporte para esses recursos foram a *EntityDataSource* ou *LinqDataSource* controles.
- Os recursos necessários a um projeto da Web que foi criado usando os modelos de entidades de dados dinâmicos ou dados dinâmicos para ter todos os arquivos que seriam necessários para suporte ao recurso.

Uma meta principal do suporte de dados dinâmicos do ASP.NET 4 é habilitar a nova funcionalidade de dados dinâmicos para qualquer aplicativo ASP.NET. O exemplo a seguir mostra a marcação para controles que podem se beneficiar da funcionalidade de dados dinâmicos em uma página existente.

[!code-aspx[Main](overview/samples/sample91.aspx)]

O código para a página, o código a seguir precisa ser adicionado para habilitar o suporte de dados dinâmicos para esses controles:

[!code-csharp[Main](overview/samples/sample92.cs)]

Quando o *GridView* controle está no modo de edição, dados dinâmicos automaticamente valida que os dados inseridos no formato correto. Se não for, uma mensagem de erro será exibida.

Essa funcionalidade também fornece outros benefícios, como poder especificar padrão valores para o modo de inserção. Sem dados dinâmicos, para implementar um valor padrão para um campo, você deve anexar a um evento, localize o controle (usando *FindControl*) e defina seu valor. No ASP.NET 4, o *EnableDynamicData* chamada oferece suporte a um segundo parâmetro que lhe permite passar valores padrão para qualquer campo no objeto, conforme mostrado neste exemplo:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Sintaxe de controle DynamicDataManager declarativo

O *DynamicDataManager* controle foi aprimorado para que você possa configurar declarativamente, assim como acontece com a maioria dos controles do ASP.NET, em vez de apenas no código. A marcação para o *DynamicDataManager* controle é semelhante ao exemplo a seguir:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Essa marcação habilita um comportamento dinâmico de dados para o controle de GridView1 que é referenciado no *DataControls* seção o *DynamicDataManager* controle.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Modelos de entidade

Modelos de entidade oferecem uma nova maneira de personalizar o layout dos dados sem a necessidade de criar uma página personalizada. Página use modelos de *FormView* controle (em vez do *DetailsView* controle, conforme usado em modelos de página em versões anteriores dos dados dinâmicos) e o *DynamicEntity* controle para processar modelos de entidade. Isso lhe dá mais controle sobre a marcação que é processada pelos dados dinâmicos.

A lista a seguir mostra o novo layout de diretório do projeto que contém os modelos de entidade:

[!code-console[Main](overview/samples/sample95.cmd)]

O `EntityTemplate` diretório contém modelos para como exibir objetos de modelo de dados. Por padrão, os objetos são processados usando o `Default.ascx` modelo, que fornece a marcação que se parece com a marcação criada pelo *DetailsView* controle usado por dados dinâmicos no ASP.NET 3.5 SP1. O exemplo a seguir mostra a marcação para o `Default.ascx` controle:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Os modelos padrão podem ser editados para alterar a aparência de todo o site. Há modelos para exibir, editar e operações de inserção. Novos modelos podem ser adicionados com base no nome do objeto de dados para alterar a aparência de um tipo de objeto. Por exemplo, você pode adicionar o modelo a seguir:

[!code-console[Main](overview/samples/sample97.cmd)]

O modelo pode conter a seguinte marcação:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Os novos modelos de entidade são exibidos em uma página usando a nova *DynamicEntity* controle. Em tempo de execução, esse controle é substituído pelo conteúdo do modelo de entidade. A marcação a seguir mostra o *FormView* controlar o `Detail.aspx` modelo de página que usa o modelo de entidade. Observe o *DynamicEntity* elemento na marcação.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-e-mail-addresses"></a>Novos modelos de campo para URLs e endereços de email

O ASP.NET 4 apresenta dois novos modelos de campo interno, `EmailAddress.ascx` e `Url.ascx`. Esses modelos são usados para os campos que são marcados como *EmailAddress* ou *Url* com o *DataType* atributo. Para *EmailAddress* objetos, o campo é exibido como um hiperlink que é criado usando o *mailto:* protocolo. Quando os usuários clicarem no link, ele abre o cliente de email do usuário e cria uma mensagem de esqueleto. Objetos digitados como *Url* são exibidos como hiperlinks comuns.

O exemplo a seguir mostra como os campos pode ser marcados.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Criação de Links com o controle DynamicHyperLink

Dados dinâmicos usam o novo recurso de roteamento foi adicionado no .NET Framework 3.5 SP1 para controlar as URLs que os usuários finais verão quando eles acessarem o site da Web. O novo *DynamicHyperLink* controle torna fácil criar links para páginas em um site de dados dinâmicos. O exemplo a seguir mostra como usar o *DynamicHyperLink* controle:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Essa marcação cria um link que aponta para a página de lista para o `Products` tabela com base nas rotas que estão definidas no `Global.asax` arquivo. O controle usa automaticamente o nome da tabela padrão que a página de dados dinâmico se baseia.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Suporte para herança no modelo de dados

O Entity Framework e o LINQ to SQL suportam herança nos modelos de dados. Um exemplo disso pode ser um banco de dados que tem um `InsurancePolicy` tabela. Ele também pode conter `CarPolicy` e `HousePolicy` tabelas que têm os mesmos campos de `InsurancePolicy` e, em seguida, adicionar mais campos. Dados dinâmicos foi modificados para entender a objetos herdados no modelo de dados e para dar suporte a scaffolding para tabelas herdadas.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Suporte a relações muitos-para-muitos (somente para o Entity Framework)

O Entity Framework tem compatibilidade avançada para relações muitos-para-muitos entre tabelas, que é implementada, expondo a relação como uma coleção em uma *entidade* objeto. Novo `ManyToMany.ascx` e `ManyToMany_Edit.ascx` modelos de campo foram adicionados para oferecer suporte para exibir e editar dados envolvidos em relações muitos-para-muitos.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Novos atributos de exibição de controle e enumerações de suporte

O *DisplayAttribute* foi adicionado para dar a você mais controle sobre como os campos são exibidos. O *DisplayName* atributo em versões anteriores do permitido alterar o nome que é usado como uma legenda para um campo de dados dinâmicos. O novo *DisplayAttribute* classe permite que você especifique mais opções para exibir um campo, como a ordem em que um campo é exibido e se um campo será usado como um filtro. O atributo também fornece controle independente do nome usado para os rótulos em um *GridView* controlar o nome usado em um *DetailsView* controlar, o texto de ajuda para o campo, e a marca d'água usada para o campo (se o campo aceita a entrada de texto).

O *EnumDataTypeAttribute* classe foi adicionada para permitir que você mapear campos para enumerações. Quando você aplica esse atributo para um campo, você pode especificar um tipo de enumeração. Dados dinâmicos usam o novo `Enumeration.ascx` modelo de campo para criar a interface do usuário para exibir e editar valores de enumeração. O modelo mapeia os valores do banco de dados para os nomes na enumeração.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Suporte aprimorado para filtros

1.0 dinâmico de dados fornecido com filtros internos para colunas Boolianas e colunas de chave estrangeira. Os filtros não permitiu que você especificar se elas foram exibidas ou em que ordem eram exibidos. O novo *DisplayAttribute* atributo soluciona ambos esses problemas, oferecendo a você controle sobre se uma coluna é exibida como um filtro e na ordem na qual ele será exibido.

Um aprimoramento adicional é que o suporte à filtragem[escritas para usar o novo](#0.2__QueryExtender "_QueryExtender") recurso de Web Forms. Isso lhe permite criar filtros, sem a necessidade de conhecimento do controle de fonte de dados que serão usados com os filtros. Com essas extensões, filtros também foram ativados em controles de modelo, que permite que você adicionar novos. Por fim, o *DisplayAttribute* classe mencionado permite que o filtro padrão a ser substituído, da mesma forma que *UIHint* permite que o modelo de campo padrão para uma coluna a ser substituído.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Melhorias de desenvolvimento da Web do Visual Studio 2010

Desenvolvimento da Web no Visual Studio 2010 foi aprimorado para maior compatibilidade CSS, aumento da produtividade por meio de trechos de marcação HTML e ASP.NET e novo IntelliSense JavaScript dinâmico.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilidade CSS aprimorado

O designer Visual Web Developer no Visual Studio 2010 foi atualizado para melhorar a conformidade com padrões CSS 2.1. O designer melhor preserva a integridade do código-fonte HTML e é mais robusto do que nas versões anteriores do Visual Studio. Nos bastidores, melhorias na arquitetura também foram feitas que melhor habilitar futuros aprimoramentos na capacidade de manutenção, layout e renderização.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML e JavaScript trechos de código

No editor de HTML, o IntelliSense é preenchida automaticamente nomes de marca. O recurso de trechos de código IntelliSense é preenchida automaticamente marcas inteiras e muito mais. No Visual Studio 2010, trechos de código IntelliSense têm suporte para JavaScript, juntamente com c# e Visual Basic, que tinham suporte em versões anteriores do Visual Studio.

Visual Studio 2010 inclui mais de 200 trechos de código que o ajudarão AutoCompletar ASP.NET e HTML marcas comuns, incluindo atributos necessários (como runat = "server") e específico para uma marca de atributos comuns (como *ID*,  *DataSourceID*, *ControlToValidate*, e *texto*).

Você pode baixar trechos de código adicionais, ou você pode escrever seus próprios trechos que encapsulam os blocos de marcação que você ou sua equipe usados para tarefas comuns.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Aprimoramentos de JavaScript IntelliSense

No Visual 2010, o JavaScript IntelliSense foi reprojetado para fornecer uma experiência de edição ainda melhores. Agora, o IntelliSense reconhece objetos dinamicamente gerados por métodos como *egisterNamespace* e por semelhantes técnicas usadas por outras estruturas de JavaScript. Desempenho aprimorado para analisar grandes bibliotecas de script e exibir o IntelliSense com pouco ou nenhum atraso de processamento. Compatibilidade drasticamente aumentou para dar suporte a quase todas as bibliotecas de terceiros e para dar suporte a diferentes estilos de codificação. Comentários de documentação agora são analisados, conforme você digita e imediatamente serão utilizados pelo IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Implantação de aplicativos Web com o Visual Studio 2010

Quando os desenvolvedores do ASP.NET implantar um aplicativo Web, ele encontrar geralmente se encontram problemas como os seguintes:

- Implantando em um site de hospedagem compartilhado requer tecnologias como FTP, que pode ser lenta. Além disso, você deve executar manualmente as tarefas, como executar scripts SQL para configurar um banco de dados e você deve alterar as configurações do IIS, como configurar uma pasta de diretório virtual como um aplicativo.
- Em um ambiente empresarial, além de implantar os arquivos de aplicativo da Web, os administradores frequentemente devem modificar arquivos de configuração do ASP.NET e as configurações do IIS. Os administradores de banco de dados devem executar uma série de scripts SQL para obter o banco de dados do aplicativo em execução. Essas instalações são muito trabalho, geralmente levar horas para ser concluída e deve ser documentada com cuidado.

Visual Studio 2010 inclui tecnologias que esses problemas e que permitem que você implantar aplicativos da Web. Uma dessas tecnologias é a ferramenta de implantação da Web do IIS (MsDeploy.exe).

Recursos de implantação da Web no Visual Studio 2010 incluem as seguintes áreas principais:

- Empacotamento de Web
- Transformação do Web. config
- Implantação de banco de dados
- Um clique para publicar aplicativos Web

As seções a seguir fornecem detalhes sobre esses recursos.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Empacotamento de Web

Visual Studio 2010 usa a ferramenta de MSDeploy para criar um arquivo compactado (. zip) para seu aplicativo, que é conhecido como um *pacote da Web*. O arquivo de pacote contém metadados sobre seu aplicativo mais o conteúdo a seguir:

- Configurações do IIS, que inclui as configurações do pool de aplicativos, configurações de página de erro e assim por diante.
- O conteúdo da Web real, que inclui páginas da Web, controles de usuário, conteúdo estático (imagens e arquivos HTML) e assim por diante.
- Dados e esquemas de banco de dados do SQL Server.
- Certificados de segurança, componentes a serem instalados no GAC, as configurações do registro, etc.

Um pacote da Web pode ser copiado para qualquer servidor e, em seguida, instalado manualmente usando o Gerenciador do IIS. Como alternativa, para a implantação automática, o pacote pode ser instalado usando os comandos de linha de comando ou usando APIs de implantação.

Visual Studio 2010 fornece tarefas do MSBuild e destinos para criar pacotes da Web internos. Para obter mais informações, consulte [visão geral de implantação de projeto de aplicativo Web ASP.NET](https://msdn.microsoft.com/en-us/library/dd394698%28VS.100%29.aspx) no site do MSDN e [10 + 20 razões por que você deve criar um pacote da Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) no blog de Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformação do Web. config

Para implantação de aplicativo Web, o Visual Studio 2010 introduz [de transformação de documento XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), que é um recurso que permite transformar um `Web.config` o arquivo de configurações de desenvolvimento para configurações de produção. Configurações de transformação são especificadas em arquivos de transformação chamados `web.debug.config`, `web.release.config`e assim por diante. (Os nomes desses arquivos corresponder as configurações do MSBuild.) Um arquivo de transformação inclui apenas as alterações que você precisa fazer implantado `Web.config` arquivo. Você pode especificar as alterações usando a sintaxe simples.

O exemplo a seguir mostra uma parte de um `web.release.config` arquivo que pode ser produzido para implantação da sua configuração de versão. A palavra-chave de substituição no exemplo especifica que durante a implantação a *connectionString* nó o `Web.config` arquivo substituirá os valores que estão listados no exemplo.

[!code-xml[Main](overview/samples/sample102.xml)]

Para obter mais informações, consulte [sintaxe de transformação do Web. config para implantação de projeto de aplicativo Web](https://msdn.microsoft.com/en-us/library/dd465326%28VS.100%29.aspx) no MSDN <a id="0.2_a"> </a> site da Web e[implantação da Web: transformação do Web. config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)no blog de Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Implantação de banco de dados

Um pacote de implantação do Visual Studio 2010 pode incluir dependências nos bancos de dados do SQL Server. Como parte da definição do pacote, você pode fornecer a cadeia de caracteres de conexão para o banco de dados de origem. Quando você cria o pacote da Web, o Visual Studio 2010 cria scripts SQL para o esquema de banco de dados e, opcionalmente, para os dados e, em seguida, adiciona esses ao pacote. Você pode também fornecer scripts SQL personalizados e especifique a sequência na qual ele deve ser executado no servidor. No momento da implantação, você fornecer uma cadeia de caracteres de conexão que é apropriada para o servidor de destino; o processo de implantação, em seguida, usa essa cadeia de caracteres de conexão para executar os scripts que cria o esquema de banco de dados e adicionar os dados.

Além disso, usando um clique, publicar, você pode configurar a implantação para publicar seu banco de dados diretamente quando o aplicativo é publicado em um site remoto de hospedagem compartilhado. Para obter mais informações, consulte [como: implantar um banco de dados com um projeto de aplicativo Web](https://msdn.microsoft.com/en-us/library/dd465343%28VS.100%29.aspx) no site do MSDN e [implantação de banco de dados com o VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) no blog de Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Um clique para publicar aplicativos Web

Visual Studio 2010 também permite usar o serviço de gerenciamento remoto do IIS para publicar um aplicativo Web em um servidor remoto. Você pode criar um perfil de publicação para sua conta de hospedagem ou servidores de teste ou servidores de teste. Cada perfil pode salvar as credenciais apropriadas com segurança. Você pode implantar a qualquer destino servidores com um clique, usando um clique Web publiquem barra de ferramentas. Com o Visual Studio 2010, você também pode publicar por meio da linha de comando do MSBuild. Isso lhe permite configurar seu ambiente de compilação de equipe para incluir a publicação em um modelo de integração contínua.

Para obter mais informações, consulte [como: implantar um aplicativo usando um clique publicação do projeto Web e implantação da Web](https://msdn.microsoft.com/en-us/library/dd465337%28VS.100%29.aspx) no site do MSDN e [Web publicar 1 Clique com o VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) no blog de Vishal Joshi. Para exibir apresentações de vídeo sobre implantação de aplicativos Web no Visual Studio 2010, consulte [VS 2010 para visualizações de desenvolvedor da Web](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) no blog de Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Recursos

Os sites a seguir fornecem informações adicionais sobre o ASP.NET 4 e Visual Studio 2010.

- [O ASP.NET 4](https://msdn.microsoft.com/en-us/library/ee532866%28VS.100%29.aspx) — a documentação oficial do ASP.NET 4 no site do MSDN.
- [https://www.ASP.NET/](https://www.asp.net/) — ASP.NET o site da Web da equipe.
- [https://www.ASP.NET/dynamicdata/](https://msdn.microsoft.com/en-us/library/cc488545.aspx) e [mapa de conteúdo de dados dinâmicos ASP.NET](https://msdn.microsoft.com/en-us/library/cc488545%28VS.100%29.aspx) — recursos Online no site de equipe do ASP.NET e na documentação oficial do Dynamic Data do ASP.NET.
- [https://www.ASP.NET/AJAX/](../../ajax/index.md) — o recurso da Web principal para o desenvolvimento do ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — blog a equipe Visual de desenvolvedor da Web, que inclui informações sobre recursos no Visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — o recurso da Web principal para as versões de preview do ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este é um documento preliminar e pode ser alterado substancialmente antes da versão comercial final do software descrito aqui.

As informações contidas neste documento representam a visão atual da Microsoft Corporation sobre os problemas discutidos a partir da data da publicação. Como a Microsoft deve responder às condições do mercado em constante mudança, este documento não deve ser interpretado como um compromisso por parte da Microsoft, e a Microsoft não pode garantir a exatidão de nenhuma informação apresentada após a data da publicação.

Este whitepaper é apenas para fins informativos. A MICROSOFT NÃO OFERECE GARANTIA, EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA, DAS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO.

Estar em conformidade com todas as leis de direitos autorais aplicáveis é responsabilidade do usuário. Sem limitar os direitos autorais, nenhuma parte deste documento pode ser reproduzida, armazenada ou inserida em um sistema de recuperação, ou transmitida de qualquer forma, por qualquer meio (eletrônico, mecânico, fotocópia, gravação ou outro) ou para qualquer fim, sem a permissão expressa, por escrito, da Microsoft Corporation.

A Microsoft pode ter patentes, solicitações de patente, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual que abrangem o conteúdo deste documento. A não ser que especificado expressamente em um contrato de licença da Microsoft, o fornecimento deste documento não confere a você nenhum direito sobre as supracitadas patentes, marcas comerciais, direitos autorais ou outra propriedade intelectual.

Salvo indicação em contrário, os exemplos de empresas, organizações, produtos, nomes de domínio, endereços de email, logotipos, pessoas, locais e eventos descritos no presente documento são fictícios, e qualquer ligação com qualquer empresa, organização, produto, nome de domínio, endereço de email, logotipo, pessoa, local ou eventos reais é intencional ou deve ser inferida.

© 2009 Microsoft Corporation. Todos os direitos reservados.

Microsoft e Windows são marcas registradas ou comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.

Os nomes de empresas reais e produtos mencionados aqui podem ser marcas comerciais de seus respectivos proprietários.
