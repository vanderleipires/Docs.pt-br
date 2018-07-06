---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 e visão geral de desenvolvimento Visual Studio 2010 Web | Microsoft Docs
author: rick-anderson
description: Este documento fornece uma visão geral de muitos dos novos recursos do ASP.NET que estão incluídos no.NET Framework 4 e no Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 7a75b0c39c923bb500368dbb2304b534d8ed994d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380773"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 e visão geral de desenvolvimento Visual Studio 2010 Web
====================
> Este documento fornece uma visão geral de muitos dos novos recursos do ASP.NET que estão incluídos no.NET Framework 4 e no Visual Studio 2010.
> 
> [Baixe este white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Conteúdo**

**[Core Services](#0.2__Toc253429238 "_Toc253429238")**  
[Arquivo Web. config refatoração](#0.2__Toc253429239 "_Toc253429239")  
[O cache de saída extensível](#0.2__Toc253429240 "_Toc253429240")  
[Aplicativos da Web de auto-Start](#0.2__Toc253429241 "_Toc253429241")  
[Redirecionando permanentemente uma página](#0.2__Toc253429242 "_Toc253429242")  
[Redução de estado de sessão](#0.2__Toc253429243 "_Toc253429243")  
[A expansão do intervalo de URLs permitidas](#0.2__Toc253429244 "_Toc253429244")  
[Validação de solicitação extensível](#0.2__Toc253429245 "_Toc253429245")  
[Cache de objeto e extensibilidade do cache de objetos](#0.2__Toc253429246 "_Toc253429246")  
[Extensible HTML, a URL e a codificação do cabeçalho HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Monitoramento de desempenho para aplicativos individuais em um único processo de trabalho](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery inclusa com Web Forms e MVC](#0.2__Toc253429251 "_Toc253429251")  
[Suporte de rede de fornecimento de conteúdo](#0.2__Toc253429252 "_Toc253429252")  
[Scripts explícito do ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Definir marcas Meta com as propriedades de Page.MetaDescription e Page.MetaKeywords](#0.2__Toc253429257 "_Toc253429257")  
[Habilitando o estado de exibição de controles individuais](#0.2__Toc253429258 "_Toc253429258")  
[Alterações em recursos do navegador](#0.2__Toc253429259 "_Toc253429259")  
[Roteamento no ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Definindo as IDs de cliente](#0.2__Toc253429261 "_Toc253429261")  
[Manter seleção de linha nos controles de dados](#0.2__Toc253429262 "_Toc253429262")  
[Controle de gráfico do ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtrando dados com o controle QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Expressões de código codificadas em HTML](#0.2__Toc253429265 "_Toc253429265")  
[As alterações do modelo de projeto](#0.2__Toc253429266 "_Toc253429266")  
[Aprimoramentos de CSS](#0.2__Toc253429267 "_Toc253429267")  
[Ocultando elementos em torno de campos ocultos da div](#0.2__Toc253429268 "_Toc253429268")  
[Uma tabela externa de renderização para controles modelados](#0.2__Toc253429269 "_Toc253429269")  
[Aprimoramentos de controles ListView](#0.2__Toc253429270 "_Toc253429270")  
[Aprimoramentos de controle RadioButtonList e CheckBoxList](#0.2__Toc253429271 "_Toc253429271")  
[Melhorias no controle de menu](#0.2__Toc253429272 "_Toc253429272")  
[Assistente e CreateUserWizard controles 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Suporte de áreas](#0.2__Toc253429275 "_Toc253429275")  
[Suporte de validação do atributo de anotação de dados](#0.2__Toc253429276 "_Toc253429276")  
[Auxiliares com modelos](#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[Habilitar dados dinâmicos para projetos existentes](#0.2__Toc253429279 "_Toc253429279")  
[Sintaxe declarativa controle de DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[Modelos de entidade](#0.2__Toc253429281 "_Toc253429281")  
[Novos modelos de campo para URLs e endereços de email](#0.2__Toc253429282 "_Toc253429282")  
[Criação de Links com o controle DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Suporte para herança no modelo de dados](#0.2__Toc253429284 "_Toc253429284")  
[Suporte para relações muitos-para-muitos (apenas no Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Novos atributos para exibição do controle e suporte a enumerações](#0.2__Toc253429286 "_Toc253429286")  
[Suporte aprimorado para filtros](#0.2__Toc253429287 "_Toc253429287")

**[Melhorias de desenvolvimento da Web do Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[CSS compatibilidade aprimorada](#0.2__Toc253429289 "_Toc253429289")  
[HTML e JavaScript trechos](#0.2__Toc253429290 "_Toc253429290")  
[Aprimoramentos do JavaScript IntelliSense](#0.2__Toc253429291 "_Toc253429291")

**[Implantação de aplicativo com o Visual Studio 2010 Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web Packaging](#0.2__Toc253429293 "_Toc253429293")  
[Web.config Transformation](#0.2__Toc253429294 "_Toc253429294")  
[Implantação de banco de dados](#0.2__Toc253429295 "_Toc253429295")  
[Para aplicativos Web de publicação de um clique](#0.2__Toc253429296 "_Toc253429296")  
[Resources](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Serviços principais

ASP.NET 4 apresenta uma série de recursos que melhoram a serviços do ASP.NET core, como o armazenamento de estado de sessão e cache de saída.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Arquivo Web. config de refatoração

O `Web.config` arquivo que contém a configuração para um aplicativo Web cresceu consideravelmente nas versões anteriores do .NET Framework como novos recursos foram adicionados, como o Ajax, roteamento e a integração com o IIS 7. Isso tornou mais difícil de configurar ou iniciar novos aplicativos Web sem uma ferramenta como o Visual Studio. No. the .NET Framework 4, os elementos de configuração principais foram movidos para o `machine.config` arquivos e aplicativos agora herdarão essas configurações. Isso permite que o `Web.config` arquivo em aplicativos ASP.NET 4 estar vazio ou conter apenas as seguintes linhas, que especificam para o Visual Studio qual versão do framework que o aplicativo está definindo como destino:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>O cache de saída extensível

Desde a hora em que o ASP.NET 1.0 foi lançado, o cache de saída permitiu aos desenvolvedores armazenar a saída gerada de páginas, controles e respostas HTTP na memória. Nas próximas solicitações da Web, ASP.NET pode servir conteúdo mais rapidamente, recuperando a saída gerada de memória em vez de regenerar a saída a partir do zero. No entanto, essa abordagem tem uma limitação — conteúdo gerado deve sempre ser armazenados na memória e nos servidores que estão enfrentando tráfego intenso, a memória consumida pelo cache de saída pode competir com demanda de memória de outras partes de um aplicativo Web.

ASP.NET 4 adiciona um ponto de extensibilidade para o cache de saída que permite que você configure um ou mais provedores de cache de saída personalizados. Provedores de cache de saída podem usar qualquer mecanismo de armazenamento para manter o conteúdo HTML. Isso torna possível criar provedores personalizados de cache de saída para mecanismos de persistência diversificado, que podem incluir discos locais ou remotos, armazenamento em nuvem e distribuídas mecanismos de cache.

Criar um provedor de cache de saída personalizado como uma classe que deriva o novo *System.Web.Caching.OutputCacheProvider* tipo. Em seguida, você pode configurar o provedor na `Web.config` arquivo usando o novo *provedores* subseção do *outputCache* elemento, conforme mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample2.xml)]

Por padrão no ASP.NET 4, todas as respostas HTTP, renderizados páginas e controles de usam o cache de saída na memória, conforme mostrado no exemplo anterior, em que o *defaultProvider* atributo é definido como AspNetInternalProvider. Você pode alterar o provedor de cache de saída padrão usado para um aplicativo Web, especificando um nome de provedor diferente para *defaultProvider*.

Além disso, você pode selecionar diferentes provedores de cache de saída por controle e por solicitação. A maneira mais fácil para escolher um provedor de cache de saída diferente para diferentes controles de usuário da Web é fazer então declarativamente usando o novo *providerName* de atributo em uma diretiva de controle, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Especificando um provedor de cache de saída diferente para uma solicitação HTTP requer um pouco mais trabalho. Em vez de especificar declarativamente o provedor, você substitui o novo *GetOuputCacheProviderName* método o `Global.asax` arquivo para especificar qual provedor será usado para uma solicitação específica de forma programática. O exemplo a seguir mostra como fazer isso.

[!code-csharp[Main](overview/samples/sample4.cs)]

Com a adição de extensibilidade do provedor de cache de saída para o ASP.NET 4, agora você pode adotar estratégias de cache de saída mais agressivas e mais inteligentes para seus sites. Por exemplo, agora é possível armazenar em cache as páginas de "10 principais" de um site na memória, enquanto o cache de páginas que recebem tráfego inferior no disco. Como alternativa, você pode armazenar em cache todas as combinações variam por uma página renderizada, mas usar um cache distribuído, de modo que o consumo de memória é descarregado a partir de servidores de front-end da Web.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Aplicativos da Web de início automático

Alguns aplicativos da Web precisam carregar grandes quantidades de dados ou executar a inicialização cara antes de atender a primeira solicitação de processamento. Em versões anteriores do ASP.NET, para essas situações você tinha planejar abordagens personalizadas para "despertar" um aplicativo ASP.NET e, em seguida, executar código de inicialização durante a *Application\_carga* método no `Global.asax` arquivo.

Um novo recurso de escalabilidade *auto-início* que diretamente endereços neste cenário está disponível quando ASP.NET 4 é executado no IIS 7.5 no Windows Server 2008 R2. O recurso auto-start fornece uma abordagem controlada para iniciar um pool de aplicativos, inicialização de um aplicativo ASP.NET e, em seguida, aceitando solicitações HTTP.

> [!NOTE] 
> 
> Módulo de aquecimento de aplicativos do IIS para o IIS 7.5
> 
> A equipe IIS lançou a primeira versão de teste beta do módulo de aquecimento do aplicativo para IIS 7.5. Isso torna aquecendo seus aplicativos ainda mais fácil do que o descrito anteriormente. Em vez de escrever código personalizado, você deve especificar as URLs de recursos a ser executado antes que o aplicativo Web aceitar solicitações da rede. Esse aquecimento ocorre durante a inicialização do serviço IIS (se você tiver configurado o pool de aplicativos do IIS como *AlwaysRunning*) e quando se Recicla um processo de trabalho do IIS. Durante a reciclagem, o processo de trabalho do IIS antigo continua a executar solicitações até que o processo de trabalho recém-gerada seja totalmente aquecido, para que aplicativos experiência sem interrupções ou outros problemas devido a unprimed caches. Observe que esse módulo funciona com qualquer versão do ASP.NET, começando com a versão 2.0.
> 
> Para obter mais informações, consulte [Application warm-up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) no site da Web do IIS.net. Para um passo a passo que ilustra como usar o recurso de aquecimento, consulte [Introdução ao módulo de aquecimento do aplicativo do IIS 7.5](https://www.iis.net/learn/manage) no site da Web do IIS.net.


Para usar o recurso auto-start, um administrador do IIS define um pool de aplicativos no IIS 7.5 seja iniciado automaticamente usando a seguinte configuração no `applicationHost.config` arquivo:

[!code-xml[Main](overview/samples/sample5.xml)]

Como um único pool de aplicativos pode conter vários aplicativos, você especificar aplicativos individuais para ser iniciado automaticamente, usando a seguinte configuração no `applicationHost.config` arquivo:

[!code-xml[Main](overview/samples/sample6.xml)]

Quando um servidor de IIS 7.5 é iniciados a frio ou quando um pool de aplicativos é reciclado, IIS 7.5 usa as informações a `applicationHost.config` arquivo para determinar quais precisam de aplicativos da Web seja iniciado automaticamente. Para cada aplicativo que está marcado para inicialização automática, IIS7.5 envia uma solicitação para o ASP.NET 4 para iniciar o aplicativo em um estado durante o qual o aplicativo temporariamente não aceita solicitações HTTP. Quando ele estiver nesse estado, o ASP.NET cria uma instância do tipo definido pelos *serviceAutoStartProvider* atributo (conforme mostrado no exemplo anterior) e chama seu ponto de entrada pública.

Criar um tipo de início automático gerenciados com o ponto de entrada necessários ao implementar o *IProcessHostPreloadClient* de interface, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample7.cs)]

Após a inicialização do seu código é executado nos *pré-carregamento* método e o método retorna, o aplicativo do ASP.NET está pronto para processar solicitações.

Com a adição de auto-start.5 do IIS e ASP.NET 4, agora você tem uma abordagem bem definida para executar a inicialização do aplicativo caros antes de processar a primeira solicitação HTTP. Por exemplo, você pode usar o novo recurso de início automático para inicializar um aplicativo e, em seguida, sinalizar um balanceador de carga que o aplicativo foi inicializado e está pronto para aceitar o tráfego HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Redirecionando permanentemente uma página

É uma prática comum em aplicativos da Web para mover as páginas e outros conteúdos em torno ao longo do tempo, o que pode levar a um acúmulo de links obsoletos em mecanismos de pesquisa. No ASP.NET, os desenvolvedores tradicionalmente tratou solicitações para URLs antigas usando usando o *Response. Redirect* método para encaminhar uma solicitação para a nova URL. No entanto, o *redirecionar* método emite uma resposta HTTP 302 encontrado (redirecionamento temporário), o que resulta em um HTTP extra ida e volta quando os usuários tentam acessar as URLs antigas.

ASP.NET 4 adiciona um novo *RedirectPermanent* método auxiliar que simplifica a problema HTTP 301 movido permanentemente respostas, como no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample8.cs)]

Mecanismos de pesquisa e outros agentes de usuário que reconhecem redirecionamentos permanentes armazenará a nova URL que está associada com o conteúdo, o que elimina a desnecessária ida e volta feita pelo navegador para redirecionamentos temporários.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Redução de estado de sessão

O ASP.NET oferece duas opções padrão para armazenar o estado de sessão em um farm da Web: um provedor de estado de sessão que invoca um servidor de estado de sessão fora do processo e um provedor de estado de sessão que armazena dados em um banco de dados do Microsoft SQL Server. Porque as duas opções envolvem armazenar informações de estado fora do processo de trabalho de um aplicativo Web, o estado de sessão precisa ser serializado antes de serem enviado para o armazenamento remoto. Dependendo da quantidade de informações um desenvolvedor salva no estado de sessão, o tamanho dos dados serializados pode ficar muito grande.

ASP.NET 4 apresenta uma nova opção de compactação para os dois tipos de provedores de estado de sessão fora do processo. Quando o *compressionEnabled* opção de configuração mostrada no exemplo a seguir é definida como *verdadeiro*, ASP.NET compactará (e descompactar) estado de sessão serializado usando o .NET Framework  *GZipStream* classe.

[!code-xml[Main](overview/samples/sample9.xml)]

Com a simple adição do novo atributo para o `Web.config` arquivos, aplicativos com sobressalentes ciclos de CPU nos servidores Web podem perceber uma reduções significativas no tamanho dos dados serializados do estado de sessão.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>A expansão do intervalo de URLs permitidas

ASP.NET 4 apresenta as novas opções de expansão do tamanho de URLs do aplicativo. Versões anteriores do ASP.NET restrita comprimentos de caminho de URL para 260 caracteres, com base no limite de caminho de arquivo NTFS. No ASP.NET 4, você tem a opção de aumentar (ou diminuir) esse limite conforme apropriado para seus aplicativos, usando dois novos *httpRuntime* atributos de configuração. O exemplo a seguir mostra esses novos atributos.

[!code-xml[Main](overview/samples/sample10.xml)]

Para permitir caminhos maiores ou menores (a parte da URL que não inclui o protocolo, o nome do servidor e a cadeia de caracteres de consulta), modifique a *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* atributo. Para permitir que cadeias de caracteres de consulta mais ou menos, modifique o valor da *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* atributo.

ASP.NET 4 também permite que você configure os caracteres que são usados pela verificação de caracteres de URL. Quando o ASP.NET encontra um caractere inválido na parte do caminho de URL, ele rejeita a solicitação e emite um erro HTTP 400. Nas versões anteriores do ASP.NET, a verificação de caracteres de URL eram limitadas a um conjunto fixo de caracteres. No ASP.NET 4, você pode personalizar o conjunto de caracteres válidos usando o novo *requestPathInvalidChars* atributo da *httpRuntime* elemento de configuração, conforme mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample11.xml)]

Por padrão, o <em>requestPathInvalidChars</em> atributo define oito caracteres como inválido. (Na cadeia de caracteres que é atribuída a <em>requestPathInvalidChars</em> por padrão<em>,</em>o menor que (&lt;), maior que (&gt;) e "e" comercial (&amp;) são caracteres codificado, pois o `Web.config` arquivo é um arquivo XML.) Você pode personalizar o conjunto de caracteres inválidos, conforme necessário.

> [!NOTE]
> Observação ASP.NET 4 sempre rejeita os caminhos de URL que contêm caracteres no intervalo ASCII de 0x00 a 0x1F, pois esses são os caracteres de URL inválidos conforme definido no RFC 2396 da IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Em versões do Windows Server que executam o IIS 6 ou superior, o driver de dispositivo do protocolo HTTP. sys recusa automaticamente as URLs com esses caracteres.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Validação de solicitação extensível

Validação de solicitação ASP.NET procura dados de solicitação HTTP de entrada para cadeias de caracteres que são comumente usadas em ataques de script de entre sites (XSS). Se forem encontradas possíveis cadeias de caracteres XSS, a validação de solicitação sinaliza a cadeia de caracteres suspeita e retornará um erro. A validação de solicitação interna retorna um erro somente quando ele encontra as cadeias de caracteres mais comuns usadas em ataques XSS. Tentativas anteriores de fazer a validação de XSS mais agressivo resultaram em muitos falsos positivos. No entanto, os clientes talvez queira verificações de validação de solicitação que é mais agressiva ou por outro lado pode querer intencionalmente relaxar XSS para páginas específicas ou para tipos específicos de solicitações.

No ASP.NET 4, o recurso de validação de solicitação foi feito extensível para que você possa usar a lógica de validação de solicitação personalizada. Para estender a validação de solicitação, você cria uma classe que deriva da nova *System.Web.Util.RequestValidator* tipo e você configurar o aplicativo (na *httpRuntime* seção os `Web.config`arquivo) para usar o tipo personalizado. O exemplo a seguir mostra como definir uma classe personalizada de validação de solicitação:

[!code-xml[Main](overview/samples/sample12.xml)]

O novo *requestValidationType* atributo requer uma cadeia de identificador de tipo do .NET Framework padrão que especifica a classe que fornece validação de solicitação personalizado. Para cada solicitação ASP.NET invoca o tipo personalizado para processar cada parte dos dados de solicitação HTTP de entrada. O corpo da entidade, todos os cabeçalhos HTTP (cookies e cabeçalhos personalizados) e a URL de entrada estão disponíveis para inspeção por uma classe de validação de solicitação personalizado como o mostrado no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample13.cs)]

Para casos em que você não deseja inspecionar uma parte dos dados de entrada HTTP, a classe de validação de solicitação pode fazer fallback para permitir que a validação de solicitação do ASP.NET padrão seja executado apenas chamando *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Cache de objeto e extensibilidade do cache de objeto

Desde sua primeira versão, o ASP.NET incluiu um cache de objeto de na memória avançados (*Caching*). A implementação de cache foi tão popular que foi usada em aplicativos não-Web. No entanto, é difícil para um aplicativo Windows Forms ou WPF incluir uma referência ao `System.Web.dll` apenas para poder usar o cache de objetos do ASP.NET.

Para disponibilizar o cache para todos os aplicativos, o .NET Framework 4 apresenta um novo assembly, um novo namespace, alguns tipos de base e um implementação de cache de concreto. O novo `System.Runtime.Caching.dll` assembly contém uma nova API de cache na *System.Runtime.Caching* namespace. O namespace contém dois conjuntos de núcleo de classes:

- Tipos abstratos que fornecem a base para a criação de qualquer tipo de implementação personalizada de cache.
- Uma implementação de cache concretas de objeto na memória (o *System.Runtime.Caching.MemoryCache* classe).

O novo *MemoryCache* classe é modelada estreitamente na cache do ASP.NET, e ele compartilha grande parte da lógica do mecanismo de cache interno com o ASP.NET. Embora as APIs públicas de cache no *System.Runtime.Caching* foram atualizados para dar suporte ao desenvolvimento dos caches personalizados, se você tiver usado o ASP.NET *Cache* objeto, você encontrará os conceitos familiares no novas APIs.

Uma discussão detalhada sobre a nova *MemoryCache* classe e dar suporte a APIs base exigiria um documento inteiro. No entanto, o exemplo a seguir dá uma ideia de como funciona a nova API de cache. O exemplo foi escrito para um aplicativo de formulários do Windows, sem qualquer dependência em `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Extensible HTML, a URL e a codificação do cabeçalho HTTP

No ASP.NET 4, você pode criar rotinas de codificação personalizadas para as seguintes tarefas de codificação de texto comuns:

- A codificação HTML.
- Codificação de URL.
- Codificação do atributo HTML.
- Codificação de cabeçalhos HTTP de saída.

Você pode criar um codificador personalizado, derivando da nova *System.Web.Util.HttpEncoder* tipo e, em seguida, configurando o ASP.NET para usar o tipo personalizado na *httpRuntime* seção o `Web.config` arquivo, como como mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample15.xml)]

Depois de um codificador personalizado tiver sido configurado, o ASP.NET chama automaticamente a implementação de codifica personalizada sempre que o público de codificação de métodos do *System.Web.HttpUtility* ou *System.Web.HttpServerUtility* são chamadas de classes. Isso permite que uma parte de uma equipe de desenvolvimento da Web crie um codificador personalizado que implementa a codificação de caracteres agressiva, enquanto o restante da equipe de desenvolvimento da Web continua a usar a codificação de APIs do ASP.NET público. Ao configurar centralmente um codificador personalizado na *httpRuntime* elemento, terá a garantia de que todas as chamadas de codificação de texto no ASP.NET pública codificação APIs sejam roteadas por meio do codificador personalizado.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitoramento de desempenho para aplicativos individuais em um único processo de trabalho

Para aumentar o número de sites da Web que pode ser hospedado em um único servidor, muitos hosters execute vários aplicativos ASP.NET em um único processo de trabalho. No entanto, se vários aplicativos usam um processo de trabalho compartilhada, é difícil para os administradores de servidor identificar um aplicativo individual que está com problemas.

ASP.NET 4 aproveita a nova funcionalidade de monitoramento de recursos introduzida pelo CLR. Para habilitar essa funcionalidade, você pode adicionar o seguinte trecho de configuração XML para o `aspnet.config` arquivo de configuração.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Observação o `aspnet.config` arquivo está no diretório em que o .NET Framework está instalado. Não é o `Web.config` arquivo.


Quando o *appDomainResourceMonitoring* recurso foi habilitado, dois novos contadores de desempenho estão disponíveis na categoria de desempenho "Aplicativos do ASP.NET": *% tempo do processador gerenciado* e  *Memória usada gerenciada*. Ambos esses contadores de desempenho usam o novo recurso de gerenciamento de recursos de domínio de aplicativo do CLR para controlar o tempo estimado de CPU e a utilização de memória gerenciada de aplicativos individuais do ASP.NET. Como resultado, com o ASP.NET 4, os administradores agora têm uma exibição mais granular sobre o consumo de recursos de aplicativos individuais executados em um único processo de trabalho.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Multiplataforma

Você pode criar um aplicativo que tem como alvo uma versão específica do .NET Framework. No ASP.NET 4, um novo atributo na *compilação* elemento da `Web.config` arquivo permite que você direcione o .NET Framework 4 e posterior. Se você direcionar explicitamente o .NET Framework 4, e se você incluir elementos opcionais na `Web.config` arquivo, como as entradas para *System. CodeDom*, esses elementos devem estar corretos para o .NET Framework 4. (Se você não direcione explicitamente o .NET Framework 4, a estrutura de destino é inferida da falta de uma entrada no `Web.config` arquivo.)

O exemplo a seguir mostra o uso do *targetFramework* atributo na *compilação* elemento do `Web.config` arquivo.

[!code-xml[Main](overview/samples/sample17.xml)]

Observe o seguinte sobre como alvo uma versão específica do .NET Framework:

- Em um pool de aplicativos do .NET Framework 4, o sistema de compilação do ASP.NET pressupõe que o .NET Framework 4 como um destino se o `Web.config` arquivo não inclui o *targetFramework* atributo ou se o `Web.config` arquivo está ausente. (Talvez você precise fazer alterações de codificação ao seu aplicativo para que ele seja executado no .NET Framework 4.)
- Se você incluir a *targetFramework* atributo e se o *System. CodeDom* elemento é definido no `Web.config` arquivo, esse arquivo deve conter as entradas corretas para o .NET Framework 4.
- Se você estiver usando o *aspnet\_compilador* comando pré-compilar seu aplicativo (como em um ambiente de compilação), você deve usar a versão correta do *aspnet\_compilador* comando para a estrutura de destino. Use o compilador fornecido com o .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) para compilar para o .NET Framework 3.5 e versões anteriores. Use o compilador fornecido com o .NET Framework 4 para compilar aplicativos criados usando essa estrutura ou versões posteriores.
- Em tempo de execução, o compilador usa os últimos assemblies do framework que estão instalados no computador (e, portanto, no GAC). Se uma atualização é executada mais tarde para a estrutura (por exemplo, uma versão hipotética 4.1 está instalada), você poderá usar os recursos na versão mais recente do framework, embora a *targetFramework* atributo tem como alvo uma versão inferior (por exemplo, 4.0). (No entanto, em tempo de design no Visual Studio 2010 ou quando você usa o *aspnet\_compilador* de comando, usando os recursos mais recentes do framework causará erros de compilador).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery inclusa com Web Forms e MVC

Modelos do Visual Studio para Web Forms e MVC incluem a biblioteca jQuery do código-fonte aberto. Quando você cria um novo site ou projeto, é criada uma pasta de Scripts que contém os arquivos de 3 a seguir:

- jQuery-1.4.1.js – o legível por humanos, unminified versão da biblioteca jQuery.
- jQuery-14.1.min.js – a versão reduzida da biblioteca jQuery.
- jQuery-1.4.1-VSDoc – o arquivo de documentação do Intellisense para a biblioteca jQuery.

Inclua a versão unminified do jQuery ao desenvolver um aplicativo. Inclua a versão reduzida do jQuery para aplicativos de produção.

Por exemplo, a página de Web Forms a seguir ilustra como você pode usar o jQuery para alterar a cor do plano de fundo de controles TextBox do ASP.NET como amarelo quando eles tem o foco.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Suporte de rede de distribuição de conteúdo

A Microsoft Ajax Content Delivery Network (CDN) permite que você adicione facilmente o ASP.NET Ajax e jQuery scripts aos seus aplicativos Web. Por exemplo, você pode começar a usar a biblioteca jQuery simplesmente adicionando um `<script>` marca para a página que aponta para Ajax.microsoft.com como este:

[!code-html[Main](overview/samples/sample19.html)]

Ao tirar proveito da CDN do Microsoft Ajax, você pode melhorar significativamente o desempenho de seus aplicativos Ajax. O conteúdo da CDN do Microsoft Ajax é armazenados em cache em servidores localizados em todo o mundo. Além disso, a CDN do Microsoft Ajax permite que navegadores para reutilizar arquivos armazenados em cache de JavaScript para sites da Web que estão localizados em domínios diferentes.

A Microsoft Ajax Content Delivery Network dá suporte a SSL (HTTPS), caso seja necessário atender a uma página da web usando o protocolo SSL.

Implemente um fallback quando o CDN não estiver disponível. Teste o fallback.

Para saber mais sobre a CDN do Microsoft Ajax, visite o site a seguir:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

O ScriptManager do ASP.NET oferece suporte a CDN do Microsoft Ajax. Basta definir uma propriedade, a propriedade EnableCdn, você pode recuperar todos os arquivos de JavaScript do ASP.NET framework do CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Depois de definir a propriedade EnableCdn para o valor true, a estrutura do ASP.NET irá recuperar todos os arquivos de JavaScript do ASP.NET framework do CDN, incluindo todos os arquivos JavaScript usados para validação e o UpdatePanel. A definição dessa propriedade de um pode ter um impacto significativo no desempenho do seu aplicativo web.

Você pode definir o caminho da CDN para seus próprios arquivos JavaScript usando o atributo WebResource. A nova propriedade CdnPath Especifica o caminho para o CDN usado quando você definir a propriedade EnableCdn como o valor true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Scripts explícito do ScriptManager

No passado, se você tiver usado o ScriptManger ASP.NET, em seguida, era necessário para carregar a biblioteca inteira de monolítico do Ajax ASP.NET. Ao tirar proveito da nova propriedade ScriptManager.AjaxFrameworkMode, você pode controlar exatamente quais componentes da biblioteca do Ajax ASP.NET são carregados e carregar somente os componentes da biblioteca do Ajax ASP.NET que você precisa.

A propriedade ScriptManager.AjaxFrameworkMode pode ser definida com os seguintes valores:

- Habilitado – Especifica que o controle ScriptManager inclui automaticamente o arquivo de script MicrosoftAjax. js, que é um arquivo de script combinado de todos os scripts de estrutura de núcleo (comportamento herdado).
- Desativado – Especifica que todos os recursos de script do Microsoft Ajax são desabilitados e que o controle ScriptManager não faz referência a todos os scripts automaticamente.
- Explícito – Especifica que você incluirá explicitamente referências de script ao arquivo de script do core framework individuais exige que sua página e que você incluirá referências às dependências que requer que cada arquivo de script.

Por exemplo, se você definir a propriedade AjaxFrameworkMode como o valor explícito, em seguida, você pode especificar os scripts de componente específicos do ASP.NET Ajax que você precisa:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Web Forms tem sido um recurso de núcleo do ASP.NET desde o lançamento do ASP.NET 1.0. Muitos aprimoramentos foram nessa área para ASP.NET 4, incluindo o seguinte:

- A capacidade de definir *meta* marcas.
- Mais controle sobre o estado de exibição.
- Maneiras fáceis de trabalhar com recursos do navegador.
- Suporte para usar o roteamento com Web Forms do ASP.NET.
- Mais controle sobre as IDs geradas.
- A capacidade de persistir as linhas selecionadas em controles de dados.
- Mais controle sobre o HTML renderizado na *FormView* e *ListView* controles.
- Filtragem de suporte para controles de fonte de dados.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Definir marcas Meta com as propriedades de Page.MetaDescription e Page.MetaKeywords

ASP.NET 4 adiciona duas propriedades para o *página* classe, *MetaKeywords* e *MetaDescription*. Essas duas propriedades representam correspondente *meta* marcas na sua página, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Essas duas propriedades funcionam da mesma forma que a página *título* propriedade faz. Eles seguem estas regras:

1. Se não houver nenhuma *meta* marcas na *head* elemento que correspondem aos nomes de propriedade (ou seja, nome = "palavras-chave" para *Page.MetaKeywords* e o nome = "Descrição" para  *Page.MetaDescription*, o que significa que essas propriedades não tem sido definidas), o *meta* marcas serão adicionadas à página quando ele é renderizado.
2. Se já houver *meta* marcas com esses nomes, essas propriedades atuam como obter e definir métodos para o conteúdo das marcas existentes.

Você pode definir essas propriedades em tempo de execução, que permite que você obtenha o conteúdo de um banco de dados ou outra fonte e que permite que você defina as marcas dinamicamente para descrever o que é de uma página específica para.

Você também pode definir as *palavras-chave* e *descrição* propriedades no *@ Page* diretiva no topo da marcação da página de Web Forms, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Isso substituirá a *meta* marcar o conteúdo (se houver) já está declarado na página.

O conteúdo da descrição *meta* marca são usados para melhorar a pesquisa listando as visualizações no Google. (Para obter detalhes, consulte [melhorar trechos de código com uma transformação de descrição de meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) no blog do Google Webmaster Central.) Google e Windows Live Search não usam o conteúdo, as palavras-chave para qualquer coisa, mas talvez outros mecanismos de pesquisa. Para obter mais informações, consulte [conselhos de palavras-chave do Meta](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) no site da Web de guia de mecanismo de pesquisa.

Essas novas propriedades são um recurso simples, mas salvar você da necessidade de adicioná-las manualmente ou de escrever seu próprio código para criar o *meta* marcas.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Habilitando o estado de exibição de controles individuais

Por padrão, o estado de exibição está habilitado para a página, com o resultado que cada controle na página potencialmente armazena o estado de exibição, mesmo se ele não é necessário para o aplicativo. Dados de estado de exibição estão incluídos na marcação que uma página gera e aumenta a quantidade de tempo que leva para enviar uma página para o cliente e postá-lo novamente. Armazenar estado de exibição mais do que o necessário pode causar degradação significativa no desempenho. Em versões anteriores do ASP.NET, os desenvolvedores foi possível desabilitar o estado de exibição de controles individuais para reduzir o tamanho da página, mas tinham que fazer então explicitamente para controles individuais. No ASP.NET 4, controles de servidor Web incluem um *ViewStateMode* propriedade que permite que você desabilite o estado de exibição por padrão e, em seguida, habilitá-lo somente para os controles que exigem a ele na página.

O *ViewStateMode* propriedade usa uma enumeração que tem três valores: *Enabled*, *desabilitado*, e *herdar*. *Habilitada* permite exibir o estado para o controle e para quaisquer controles filho que são definidos como *herdar* ou que nada configurou. *Desabilitado* desabilita o estado de exibição, e *herdar* Especifica que o controle usa o *ViewStateMode* configuração do controle pai.

A exemplo a seguir mostra como o *ViewStateMode* propriedade funciona. A marcação e código para os controles na página a seguir inclui valores para o *ViewStateMode* propriedade:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Como você pode ver, o código desabilita o estado de exibição para o controle PlaceHolder1. O controle de label1 filho herda este valor de propriedade (*Inherit* é o valor padrão para *ViewStateMode* para controles.) e não salva, portanto, nenhum estado de exibição. No controle PlaceHolder2, *ViewStateMode* é definido como *habilitado*, portanto, label2 herda essa propriedade e salva o estado de exibição. Quando a página é carregada pela primeira vez, o *texto* propriedade de ambos *rótulo* controles é definido como a cadeia de caracteres "[DynamicValue]".

O efeito dessas configurações é que, quando a página for carregada na primeira vez, a saída a seguir é exibida no navegador:

Desabilitado `: [DynamicValue]`

Habilitado:`[DynamicValue]`

Depois de um postback, no entanto, a seguinte saída é exibida:

Desabilitado `: [DeclaredValue]`

Habilitado:`[DynamicValue]`

O controle label1 (cujos *ViewStateMode* valor é definido como *desabilitado*) não foi preservado o valor que ele foi definido como no código. No entanto, o label2 controlar (cujos *ViewStateMode* valor é definido como *habilitado*) tem seu estado preservado.

Você também pode definir *ViewStateMode* na *@ Page* diretiva, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample26.aspx)]

O *página* classe é apenas outro controle; ele atua como o controle pai para todos os outros controles na página. O valor padrão de *ViewStateMode* é *Enabled* para instâncias de *página*. Porque os controles padrão para *herdar*, controles herdarão a *Enabled* , a menos que você defina o valor de propriedade *ViewStateMode* no nível de página ou controle.

O valor da *ViewStateMode* propriedade determina se o estado de exibição é mantido somente se o *EnableViewState* estiver definida como *verdadeiro*. Se o *EnableViewState* estiver definida como *falso*, o estado de exibição não será mantido, mesmo se *ViewStateMode* é definido como *habilitado*.

Um bom uso para esse recurso é com *ContentPlaceHolder* controles em páginas mestras, onde é possível definir *ViewStateMode* para *desabilitado* para o mestre de página e, em seguida, habilitá-lo individualmente para *ContentPlaceHolder* controles que por sua vez, contêm controles que exigem o estado de exibição.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Alterações em recursos do navegador

ASP.NET determina os recursos do navegador que um usuário está usando para procurar seu site por meio de um recurso chamado *recursos do navegador*. Recursos do navegador são representados pela *HttpBrowserCapabilities* objeto (exposta pelo *Request. browser* propriedade). Por exemplo, você pode usar o *HttpBrowserCapabilities* objeto para determinar se o tipo e versão do navegador atual dá suporte a uma versão específica do JavaScript. Ou, você pode usar o *HttpBrowserCapabilities* objeto para determinar se a solicitação foi originada de um dispositivo móvel.

O *HttpBrowserCapabilities* objeto é controlado por um conjunto de arquivos de definição do navegador. Esses arquivos contêm informações sobre os recursos dos navegadores específicos. No ASP.NET 4, esses arquivos de definição do navegador foram atualizados para conter informações sobre dispositivos, como Google Chrome, pesquisa em smartphones BlackBerry de movimento e Apple iPhone e navegadores introduzidas recentemente.

A lista a seguir mostra o novo navegador arquivos de definição:

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Usando provedores de recursos do navegador

No ASP.NET versão 3.5 Service Pack 1, você pode definir os recursos de um navegador das seguintes maneiras:

- No nível do computador, você cria ou atualiza um `.browser` arquivo XML na seguinte pasta:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Depois de definir os recursos do navegador, execute o seguinte comando do Visual Studio Prompt de comando para recompilar o assembly de recursos do navegador e adicioná-lo no GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Para um aplicativo individual, você cria um `.browser` arquivo na caixa de diálogo `App_Browsers` pasta.

Esses métodos exigem que você altere arquivos XML e para alterações no nível do computador, você deve reiniciar o aplicativo depois de executar o aspnet\_regbrowsers.exe processo.

ASP.NET 4 inclui um recurso conhecido como *provedores de recursos do navegador*. Como o nome sugere, essa permite que você compilar um provedor que por sua vez, permite que você use seu próprio código para determinar os recursos do navegador.

Na prática, os desenvolvedores geralmente não definem recursos de navegador personalizado. Navegador de arquivos são difíceis de atualizar, o processo para atualizá-los é bastante complicado e a sintaxe XML para `.browser` arquivos podem ser complexos para usar e definir. O que seria tornar esse processo muito mais fácil é se houvesse uma sintaxe comum de definição do navegador ou um banco de dados que continha as definições de navegador atualizado, ou até mesmo um serviço Web para um banco de dados. O novo recurso de provedores de recursos do navegador torna esses cenários possíveis e prático para desenvolvedores de terceiros.

Há duas abordagens principais para usar o novo recurso de provedor de recursos de navegador ASP.NET 4: estendendo os recursos do navegador ASP.NET a funcionalidade de definição ou totalmente substituí-la. Primeiro, as seções a seguir descrevem como substituir a funcionalidade e como estendê-lo.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Substituir a funcionalidade de recursos do navegador ASP.NET

Para substituir completamente a funcionalidade de definição de recursos de navegador do ASP.NET, siga estas etapas:

1. Criar uma classe de provedor que deriva *HttpCapabilitiesProvider* e que substitui o *GetBrowserCapabilities* método, como no exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    O código neste exemplo cria um novo *HttpBrowserCapabilities* de objeto, especificando somente o recurso chamado navegador e configurando esse recurso para MyCustomBrowser.
2. Registre o provedor com o aplicativo. 

    Para usar um provedor com um aplicativo, você deve adicionar o *provedor* atributo para o *browserCaps* seção os `Web.config` ou `Machine.config` arquivos. (Você também pode definir os atributos de provedor em um *local* elemento para diretórios específicos no aplicativo, como em uma pasta para um dispositivo móvel específico.) O exemplo a seguir mostra como definir a *provedor* atributo em um arquivo de configuração:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Outra maneira de registrar a nova definição de recurso do navegador é usar o código, conforme mostrado no exemplo a seguir:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Esse código deve ser executado *aplicativo\_começar* evento do `Global.asax` arquivo. Sabendo que o *id* atributo para elementos renderizados é importante se seu aplicativo incluir um script de cliente que faz referência a esses elementos.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>O identificação atributo em HTML que é renderizado para controles de servidor Web é gerado com base nas ClientID propriedade do controle.

Até que o ASP.NET 4, o algoritmo para gerar o *identificação* de atributos do ClientID propriedade tiver sido concatenar o contêiner de nomenclatura (se houver) com a ID e, no caso de controles repetidos (como em controles de dados), para adicionar um prefixo e um número sequencial. Embora isso tenha sempre a garantia de que as IDs dos controles na página sejam exclusivas, o algoritmo resultou em identificações de controle que não foram previsível e, portanto, eram difíceis de referência no script de cliente. O novo ClientIDMode propriedade permite que você especificar mais precisamente como a ID do cliente é gerada para controles. Você pode definir as *ClientIDMode* propriedade para qualquer controle, inclusive para a página. Configurações possíveis são os seguintes: Siga estas etapas:

1. AutoID* – isso é equivalente ao algoritmo para a geração ClientID valores de propriedade que foi usado em versões anteriores do ASP.NET. 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Estática – Especifica que o ClientID valor será igual à ID sem concatenando as IDs dos contêineres de nomenclatura do pai. Isso pode ser útil em controles de usuário da Web. Como um controle de usuário da Web pode ser localizado em diferentes páginas e nos controles de contêiner diferente, pode ser difícil de escrever script de cliente para controles que usam o AutoID algoritmo porque você não pode prever quais serão os valores de ID .
2. Previsível – essa opção é principalmente para uso em controles de dados que usam modelos de repetição.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Ele concatena as propriedades de identificação de contêineres de nomenclatura do controle, mas gerado ClientID valores não contêm cadeias de caracteres como "ctlxxx".

Essa configuração funciona em conjunto com o *ClientIDRowSuffix* propriedade do controle. Defina as ClientIDRowSuffix propriedade para o nome de um campo de dados e o valor desse campo é usada como o sufixo para o gerado ClientID valor. Normalmente você usaria a chave primária de um registro de dados como o ClientIDRowSuffix valor. Herdar – essa configuração é o comportamento padrão para controles, ela especifica que a geração de ID do controle é o mesmo que seu pai.

1. Você pode definir as *ClientIDMode* propriedade no nível da página. 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Isso define o padrão ClientIDMode valor para todos os controles na página atual. O padrão *ClientIDMode* é o valor no nível da página *AutoID*e o padrão ClientIDMode valor no nível de controle é herdar.
2. Como resultado, se você não definir essa propriedade em qualquer lugar no seu código, todos os controles padrão serão o AutoID algoritmo.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Definir o valor de nível de página na @ Page diretiva, conforme mostrado no exemplo a seguir:

Você também pode definir as ClientIDMode valor no arquivo de configuração no nível do computador (computador) ou no nível do aplicativo. Isso define o padrão ClientIDMode configuração para todos os controles em todas as páginas no aplicativo. Se você definir o valor no nível do computador, ele define o padrão ClientIDMode configuração para todos os sites nesse computador.

1. Você pode definir as *ClientIDMode* propriedade no nível da página. 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    A exemplo a seguir mostra a *ClientIDMode* configuração no arquivo de configuração:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Conforme observado anteriormente, o valor de ClientID propriedade é derivada do contêiner de nomenclatura para o pai do controle. Em alguns cenários, como quando você estiver usando as páginas mestras, controles podem acabar com IDs de como as mostradas na seguinte HTML renderizado:

    - Mesmo que o *entrada* mostrado na marcação de elemento (de um *caixa de texto* controle) é apenas dois contêineres de nomenclatura abaixo na página (aninhada *ContentPlaceholder* controles), Por causa da maneira que as páginas mestras são processadas, o resultado final é uma ID de controle semelhante ao seguinte: Essa ID é garantido que seja exclusivo na página, mas é desnecessariamente longo para a maioria das finalidades.
    - Imagine que você deseja reduzir o tamanho da ID renderizada e para ter mais controle sobre como a ID é gerada. (Por exemplo, você deseja eliminar os prefixos "ctlxxx".) A maneira mais fácil de fazer isso é, definindo o *ClientIDMode* propriedade conforme mostrado no exemplo a seguir: 

        > [!NOTE]
        > Neste exemplo, o *ClientIDMode* estiver definida como estático para mais externo NamingPanel elemento e definido como previsível para interna NamingControl elemento.
2. Essas configurações resultam na marcação a seguir (o restante da página e a página mestra é considerado ser o mesmo, conforme mostrado no exemplo anterior):

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>O estáticos configuração tem o efeito de redefinir na hierarquia de nomenclatura para todos os controles dentro do mais externo NamingPanel elemento e de eliminar o ContentPlaceHolder e MasterPage IDs da identificação do gerado.

(O nome atributo de elementos renderizados é afetado, portanto, a funcionalidade normal do ASP.NET é retida por eventos, o estado de exibição e assim por diante.) Um efeito colateral de redefinição na hierarquia de nomenclatura é que, mesmo se você mover a marcação para o NamingPanel elementos para outra ContentPlaceholder controle, as IDs de cliente renderizado permanecem os mesmos. Observe que cabe a você certifique-se de que as IDs de controle renderizado são exclusivas. Se não, isso pode interromper qualquer funcionalidade que exija IDs exclusivos para elementos HTML individuais, como o cliente getElementById função.

[!code-console[Main](overview/samples/sample36.cmd)]

Criação de IDs de cliente previsíveis em controles ligados a dados

[!code-console[Main](overview/samples/sample37.cmd)]

O ClientID valores que são gerados para controles em um controle de lista associado a dados pelo algoritmo herdado podem ser longos e não são realmente previsíveis. O [ClientIDMode] funcionalidade pode ajudá-lo a ter mais controle sobre como essas IDs são geradas.") A marcação no exemplo a seguir inclui um ListView controle: No exemplo anterior, o ClientIDMode e RowClientIDRowSuffix propriedades são definidas na marcação.

- O *ClientIDRowSuffix* propriedade pode ser usada somente em controles ligados a dados e seu comportamento é diferente dependendo de qual controle você está usando. As diferenças são estes:
- GridView* controle — você pode especificar o nome de uma ou mais colunas na fonte de dados, que são combinados em tempo de execução para criar o cliente IDs. Por exemplo, se você definir RowClientIDRowSuffix para "ProductName, ProductId," IDs de controle para elementos renderizados terá um formato semelhante ao seguinte:
- ListView* controle — você pode especificar uma única coluna na fonte de dados que é acrescentado à ID de cliente.
- *Por exemplo, se você definir *ClientIDRowSuffix* para "NomeDoProduto", as IDs de controle renderizada terá um formato semelhante ao seguinte:
- *Nesse caso, o 1 à direita é derivado da ID do produto do item de dados atual.
- Repetidor* controle — esse controle não dá suporte a *ClientIDRowSuffix* propriedade.

#### <a name="routing-for-web-forms-pages"></a>Em um Repeater controle, o índice da linha atual é usado.

Quando você usa ClientIDMode = "Previsível" com um *Repeater* controlar, as IDs são geradas de cliente que têm o seguinte formato:

[!code-csharp[Main](overview/samples/sample38.cs)]

O 0 à direita é o índice da linha atual. O *FormView* e DetailsView controles não exibem várias linhas, portanto, eles não dão suporte a ClientIDRowSuffix propriedade.

[!code-csharp[Main](overview/samples/sample39.cs)]

Manter seleção de linha nos controles de dados O GridView e ListView controles podem permitir que usuários selecionem uma linha.

Nas versões anteriores do ASP.NET, seleção tem sido com base no índice de linha na página.

- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*
- *Por exemplo, se você selecionar o terceiro item na página 1 e, em seguida, mova para a página 2, o terceiro item nessa página é selecionado.*
- *Persistente seleção foi inicialmente tem suporte apenas em projetos de dados dinâmicos no .NET Framework 3.5 SP1.*

O *checkPhysicalUrlAccess* parâmetro especifica se a rota deve verificar as permissões de segurança para a página física que está sendo roteado para (nesse caso, aspx) e as permissões da URL de entrada (nesse caso, pesquisar / {searchterm}). Se o valor de *checkPhysicalUrlAccess* é *falso*, somente as permissões de URL de entrada serão verificadas. Essas permissões são definidas no `Web.config` arquivo usando as configurações, como o seguinte:

[!code-xml[Main](overview/samples/sample40.xml)]

Na configuração de exemplo, o acesso é negado para a página física `search.aspx` para todos os usuários, exceto aqueles que estão na função de administrador. Quando o *checkPhysicalUrlAccess* parâmetro for definido como *verdadeiro* (que é o valor padrão), apenas usuários administradores têm permissão para acessar a URL /search/ {searchterm}, porque é o aspx da página física restrito a usuários nessa função. Se *checkPhysicalUrlAccess* é definido como *falso* e o site está configurado como mostrado no exemplo anterior, todos os usuários autenticados têm permissão para acessar a URL /search/ {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Ler informações de roteamento em uma página Web Forms

No código da página física do Web Forms, você pode acessar as informações de roteamento tem extraídos da URL (ou outra informação que outro objeto adicionado para o *RouteData* objeto) por meio de duas novas propriedades:  *HttpRequest.RequestContext* e *Page.RouteData*. (*Page.RouteData* encapsula *HttpRequest.RequestContext.RouteData*.) O exemplo a seguir mostra como usar *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

O código extrai o valor que foi passado para o parâmetro searchterm, conforme definido anteriormente na rota de exemplo. Considere a seguinte URL de solicitação:

[!code-console[Main](overview/samples/sample42.cmd)]

Quando essa solicitação é feita, a palavra "scott" seria processado no `search.aspx` página.

#### <a name="accessing-routing-information-in-markup"></a>Acessando informações de roteamento na marcação

O método descrito na seção anterior mostra como obter dados de rota no código em uma página de Web Forms. Você também pode usar expressões na marcação que fornecem acesso às mesmas informações. Construtores de expressões são uma maneira eficiente e elegante para trabalhar com o código declarativo. (Para obter mais informações, consulte a entrada [Express por conta própria com personalizado construtores de expressões](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) no blog de Phil Haack.)

O ASP.NET 4 inclui dois novos construtores de expressão para o roteamento de formulários da Web. O exemplo a seguir mostra como usá-los.

[!code-aspx[Main](overview/samples/sample43.aspx)]

No exemplo, o *RouteUrl* expressão é usada para definir uma URL com base em um parâmetro de rota. Isso evita a necessidade de codificar a URL completa na marcação e permite que você altere a estrutura de URL mais tarde, sem a necessidade de qualquer alteração para este link.

Essa marcação com base na rota de definida anteriormente, gera a seguinte URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET funciona automaticamente a rota correta (ou seja, ele gera a URL correta) com base em parâmetros de entrada. Você também pode incluir um nome de rota na expressão, que permite que você especifique uma rota para usar.

O exemplo a seguir mostra como usar o *RouteValue* expressão.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Quando a página que contém este controle é executado, o valor "scott" é exibido no rótulo.

O *RouteValue* expressão torna simples de usar dados de rota na marcação, e evita a necessidade de trabalhar com o mais complexo Page.RouteData["x"] sintaxe na marcação.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Usando dados de rota para parâmetros de controle de fonte de dados

O *RouteParameter* classe permite que você especifique os dados de rota como um valor de parâmetro para consultas em um controle de fonte de dados. Ele [funciona bem como o](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) de classe, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample46.aspx)]

Nesse caso, o valor de searchterm de parâmetro de rota será usado para o @companyname parâmetro na <em>selecione</em> instrução.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>IDs de cliente de configuração

O novo *ClientIDMode* propriedade endereços sempre representou um problema no ASP.NET, ou seja, como criar controles de *id* atributo para elementos que eles sejam renderizados. Sabendo que o *id* atributo para elementos renderizados é importante se seu aplicativo incluir um script de cliente que faz referência a esses elementos.

O *identificação* atributo em HTML que é renderizado para controles de servidor Web é gerado com base nas *ClientID* propriedade do controle. Até que o ASP.NET 4, o algoritmo para gerar o *identificação* de atributos do *ClientID* propriedade tiver sido concatenar o contêiner de nomenclatura (se houver) com a ID e, no caso de controles repetidos (como em controles de dados), para adicionar um prefixo e um número sequencial. Embora isso tenha sempre a garantia de que as IDs dos controles na página sejam exclusivas, o algoritmo resultou em identificações de controle que não foram previsível e, portanto, eram difíceis de referência no script de cliente.

O novo *ClientIDMode* propriedade permite que você especificar mais precisamente como a ID do cliente é gerada para controles. Você pode definir as *ClientIDMode* propriedade para qualquer controle, inclusive para a página. Configurações possíveis são os seguintes:

- *AutoID* – isso é equivalente ao algoritmo para a geração *ClientID* valores de propriedade que foi usado em versões anteriores do ASP.NET.
- *Estática* – Especifica que o *ClientID* valor será igual à ID sem concatenando as IDs dos contêineres de nomenclatura do pai. Isso pode ser útil em controles de usuário da Web. Como um controle de usuário da Web pode ser localizado em diferentes páginas e nos controles de contêiner diferente, pode ser difícil de escrever script de cliente para controles que usam o *AutoID* algoritmo porque você não pode prever quais serão os valores de ID .
- *Previsível* – essa opção é principalmente para uso em controles de dados que usam modelos de repetição. Ele concatena as propriedades de identificação de contêineres de nomenclatura do controle, mas gerado *ClientID* valores não contêm cadeias de caracteres como "ctlxxx". Essa configuração funciona em conjunto com o *ClientIDRowSuffix* propriedade do controle. Defina as *ClientIDRowSuffix* propriedade para o nome de um campo de dados e o valor desse campo é usada como o sufixo para o gerado *ClientID* valor. Normalmente você usaria a chave primária de um registro de dados como o *ClientIDRowSuffix* valor.
- *Herdar* – essa configuração é o comportamento padrão para controles, ela especifica que a geração de ID do controle é o mesmo que seu pai.

Você pode definir as *ClientIDMode* propriedade no nível da página. Isso define o padrão *ClientIDMode* valor para todos os controles na página atual.

O padrão *ClientIDMode* é o valor no nível da página *AutoID*e o padrão *ClientIDMode* valor no nível de controle é *herdar*. Como resultado, se você não definir essa propriedade em qualquer lugar no seu código, todos os controles padrão serão o *AutoID* algoritmo.

Definir o valor de nível de página na *@ Page* diretiva, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Você também pode definir as *ClientIDMode* valor no arquivo de configuração no nível do computador (computador) ou no nível do aplicativo. Isso define o padrão *ClientIDMode* configuração para todos os controles em todas as páginas no aplicativo. Se você definir o valor no nível do computador, ele define o padrão *ClientIDMode* configuração para todos os sites nesse computador. A exemplo a seguir mostra a *ClientIDMode* configuração no arquivo de configuração:

[!code-xml[Main](overview/samples/sample48.xml)]

Conforme observado anteriormente, o valor de *ClientID* propriedade é derivada do contêiner de nomenclatura para o pai do controle. Em alguns cenários, como quando você estiver usando as páginas mestras, controles podem acabar com IDs de como as mostradas na seguinte HTML renderizado:

[!code-html[Main](overview/samples/sample49.html)]

Mesmo que o *entrada* mostrado na marcação de elemento (de um *caixa de texto* controle) é apenas dois contêineres de nomenclatura abaixo na página (aninhada *ContentPlaceholder* controles), Por causa da maneira que as páginas mestras são processadas, o resultado final é uma ID de controle semelhante ao seguinte:

[!code-console[Main](overview/samples/sample50.cmd)]

Essa ID é garantido que seja exclusivo na página, mas é desnecessariamente longo para a maioria das finalidades. Imagine que você deseja reduzir o tamanho da ID renderizada e para ter mais controle sobre como a ID é gerada. (Por exemplo, você deseja eliminar os prefixos "ctlxxx".) A maneira mais fácil de fazer isso é, definindo o *ClientIDMode* propriedade conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample51.aspx)]

Neste exemplo, o *ClientIDMode* estiver definida como *estático* para mais externo *NamingPanel* elemento e definido como *previsível* para interna *NamingControl* elemento. Essas configurações resultam na marcação a seguir (o restante da página e a página mestra é considerado ser o mesmo, conforme mostrado no exemplo anterior):

[!code-html[Main](overview/samples/sample52.html)]

O *estáticos* configuração tem o efeito de redefinir na hierarquia de nomenclatura para todos os controles dentro do mais externo *NamingPanel* elemento e de eliminar o *ContentPlaceHolder* e *MasterPage* IDs da identificação do gerado. (O *nome* atributo de elementos renderizados é afetado, portanto, a funcionalidade normal do ASP.NET é retida por eventos, o estado de exibição e assim por diante.) Um efeito colateral de redefinição na hierarquia de nomenclatura é que, mesmo se você mover a marcação para o *NamingPanel* elementos para outra *ContentPlaceholder* controle, as IDs de cliente renderizado permanecem os mesmos.

> [!NOTE]
> Observe que cabe a você certifique-se de que as IDs de controle renderizado são exclusivas. Se não, isso pode interromper qualquer funcionalidade que exija IDs exclusivos para elementos HTML individuais, como o cliente *getElementById* função.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Criação de IDs de cliente previsíveis em controles ligados a dados

O *ClientID* valores que são gerados para controles em um controle de lista associado a dados pelo algoritmo herdado podem ser longos e não são realmente previsíveis. O *ClientIDMode* funcionalidade pode ajudá-lo a ter mais controle sobre como essas IDs são geradas.

A marcação no exemplo a seguir inclui um *ListView* controle:

[!code-aspx[Main](overview/samples/sample53.aspx)]

No exemplo anterior, o *ClientIDMode* e *RowClientIDRowSuffix* propriedades são definidas na marcação. O *ClientIDRowSuffix* propriedade pode ser usada somente em controles ligados a dados e seu comportamento é diferente dependendo de qual controle você está usando. As diferenças são estes:

- *GridView* controle — você pode especificar o nome de uma ou mais colunas na fonte de dados, que são combinados em tempo de execução para criar o cliente IDs. Por exemplo, se você definir *RowClientIDRowSuffix* para "ProductName, ProductId," IDs de controle para elementos renderizados terá um formato semelhante ao seguinte:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* controle — você pode especificar uma única coluna na fonte de dados que é acrescentado à ID de cliente. Por exemplo, se você definir *ClientIDRowSuffix* para "NomeDoProduto", as IDs de controle renderizada terá um formato semelhante ao seguinte:

- [!code-console[Main](overview/samples/sample55.cmd)]

- Nesse caso, o 1 à direita é derivado da ID do produto do item de dados atual.

- *Repetidor* controle — esse controle não dá suporte a *ClientIDRowSuffix* propriedade. Em um *Repeater* controle, o índice da linha atual é usado. Quando você usa ClientIDMode = "Previsível" com um *Repeater* controlar, as IDs são geradas de cliente que têm o seguinte formato:

- [!code-console[Main](overview/samples/sample56.cmd)]

- O 0 à direita é o índice da linha atual.

O *FormView* e *DetailsView* controles não exibem várias linhas, portanto, eles não dão suporte a *ClientIDRowSuffix* propriedade.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Manter seleção de linha nos controles de dados

O *GridView* e *ListView* controles podem permitir que usuários selecionem uma linha. Nas versões anteriores do ASP.NET, seleção tem sido com base no índice de linha na página. Por exemplo, se você selecionar o terceiro item na página 1 e, em seguida, mova para a página 2, o terceiro item nessa página é selecionado.

Persistente seleção foi inicialmente tem suporte apenas em projetos de dados dinâmicos no .NET Framework 3.5 SP1. Quando esse recurso está habilitado, o item selecionado atual se baseia a chave de dados para o item. Isso significa que, se você selecionar a terceira linha na página 1 e move para a página 2, nada será selecionado na página 2. Quando você move de volta à página 1, a terceira linha ainda está selecionada. Agora há suporte para seleção persistente para o *GridView* e *ListView* controles em todos os projetos usando o *EnablePersistedSelection* propriedade, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Controle de gráfico do ASP.NET

O ASP.NET *gráfico* controle se expande as ofertas de visualização de dados no .NET Framework. Usando o *gráfico* controle, você pode criar páginas ASP.NET que tenham gráficos visualmente atraentes e intuitivos para análises complexas de estatísticas ou financeiros. O ASP.NET *gráfico* controle foi introduzido como um complemento para a versão do .NET Framework versão 3.5 SP1 e é parte da versão do .NET Framework 4.

O controle inclui os seguintes recursos:

- tipos de 35 distintas de gráfico.
- Um número ilimitado de anotações, títulos, legendas e áreas do gráfico.
- Uma ampla variedade de configurações de aparência para todos os elementos do gráfico.
- Suporte para a maioria dos tipos de gráfico 3D.
- Rótulos de dados inteligentes que podem se ajustar automaticamente em torno de pontos de dados.
- As faixas, quebras de escala e a escala logarítmica.
- Mais de 50 fórmulas financeiras e estatísticas para análise de dados e a transformação.
- Associação simples e manipulação de dados do gráfico.
- Suporte para formatos de dados comuns, como datas, horas e moedas.
- Suporte para personalização de orientada a eventos e interatividade, incluindo o cliente usando o Ajax de eventos de clique.
- Gerenciamento de estado.
- Um fluxo binário.

As figuras a seguir mostram exemplos de gráficos financeiros que são produzidos pelo controle do gráfico do ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figura 2: Exemplos de controle de gráfico do ASP.NET

Para obter mais exemplos de como usar o controle de gráfico do ASP.NET, baixe o código de exemplo sobre o [ambiente de exemplos para Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) página no site do MSDN. Você pode encontrar mais exemplos da comunidade de conteúdo na [Fórum de controle de gráfico](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Adicionando o controle de gráfico a uma página ASP.NET

O exemplo a seguir mostra como adicionar um *gráfico* controle a uma página ASP.NET usando marcação. No exemplo, o *gráfico* controle produz um gráfico de colunas para pontos de dados estáticos.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Usando gráficos 3D

O *gráfico* controle contém um *ChartAreas* coleção, que pode conter *ChartArea* objetos que definem as características de áreas de gráfico. Por exemplo, para usar o 3D para uma área do gráfico, use o *Area3DStyle* propriedade, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample59.aspx)]

A figura a seguir mostra um gráfico 3D com quatro séries do *barra* tipo de gráfico.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figura 3: gráfico de barras 3D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Usando quebras de escala e escalas logarítmicas

Quebras de escala e escalas logarítmicas são duas outras maneiras de adicionar a sofisticação ao gráfico. Esses recursos são específicos para cada eixo em uma área do gráfico. Por exemplo, para usar esses recursos no eixo Y primário de uma área do gráfico, use o *AxisY.IsLogarithmic* e *ScaleBreakStyle* as propriedades em um *ChartArea* objeto. O trecho a seguir mostra como usar quebras de escala no eixo Y primário.

[!code-aspx[Main](overview/samples/sample60.aspx)]

A figura a seguir mostra o eixo Y com quebras de escala habilitadas.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figura 4: Quebras de escala

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrando dados com o controle QueryExtender

É uma tarefa muito comum para os desenvolvedores que criam páginas da Web orientadas a dados filtrar dados. Isso tradicionalmente foi executado, criando *onde* controles de fonte de cláusulas nos dados. Essa abordagem pode ser complicada e, em alguns casos os *onde* sintaxe não permitem que você aproveite a funcionalidade completa do banco de dados subjacente.

Para tornar mais fácil, filtragem de um novo *QueryExtender* controle foi adicionado no ASP.NET 4. Esse controle pode ser adicionado à *EntityDataSource* ou *LinqDataSource* controles para filtrar os dados retornados por esses controles. Porque o *QueryExtender* controle depende de LINQ, o filtro é aplicado no servidor de banco de dados antes dos dados são enviados para a página, o que resulta em operações muito eficientes.

O *QueryExtender* controle dá suporte a uma variedade de opções de filtro. As seções a seguir descrevem essas opções e fornecem exemplos de como usá-los.

#### <a name="search"></a>Pesquisar

Para a opção de pesquisa, o *QueryExtender* controle executa uma pesquisa nos campos especificados. No exemplo a seguir, o controle usa o texto que é inserido no controle TextBoxSearch e procura por seu conteúdo na `ProductName` e `Supplier.CompanyName` colunas nos dados que são retornados o *LinqDataSource* controle.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Intervalo

A opção de intervalo é semelhante à opção de pesquisa, mas Especifica um par de valores para definir o intervalo. No exemplo a seguir, o *QueryExtender* pesquisas de controlar a `UnitPrice` as colunas nos dados retornados da *LinqDataSource* controle. O intervalo é lido dos controles TextBoxFrom e TextBoxTo na página.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

A opção de expressão de propriedade permite definir uma comparação para um valor da propriedade. Se a expressão for avaliada como *verdadeira*, os dados que está sendo examinados são retornados. No exemplo a seguir, o *QueryExtender* controle filtra os dados, comparando os dados no `Discontinued` coluna para o valor do controle CheckBoxDiscontinued na página.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Por fim, você pode especificar uma expressão personalizada para usar com o *QueryExtender* controle. Essa opção permite que você chamar uma função na página que define a lógica de filtro personalizado. O exemplo a seguir mostra como especificar uma expressão personalizada de forma declarativa a *QueryExtender* controle.

[!code-aspx[Main](overview/samples/sample64.aspx)]

O exemplo a seguir mostra a função personalizada que é invocada com o *QueryExtender* controle. Nesse caso, em vez de usar uma consulta de banco de dados que inclui um *onde* cláusula, o código usa uma consulta LINQ para filtrar os dados.

[!code-csharp[Main](overview/samples/sample65.cs)]

Estes exemplos mostram apenas uma expressão que está sendo usada na *QueryExtender* controle cada vez. No entanto, você pode incluir várias expressões dentro de *QueryExtender* controle.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Expressões de código codificadas em HTML

Alguns sites ASP.NET (especialmente com o ASP.NET MVC) dependem muito do uso `<%` =  `expression %>` sintaxe (geralmente chamado de "nuggets de código") para escrever algum texto para a resposta. Quando você usa expressões de código, é fácil esquecer de entrada de texto, se o texto vem do usuário, ele pode deixar páginas aberto a um ataque XSS (Cross Site Scripting) com codificação HTML.

ASP.NET 4 introduz a nova sintaxe para expressões de código a seguir:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Essa sintaxe usa a codificação HTML por padrão ao gravar a resposta. Esta nova expressão efetivamente converte para o seguinte:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Por exemplo, &lt;%: solicitação ["UserInput"] %&gt; executa a codificação de HTML no valor de *solicitar ["UserInput"]*.

O objetivo desse recurso é para que seja possível substituir todas as instâncias da sintaxe antiga com a nova sintaxe para que você não será forçado a decidir em cada etapa qual delas usar. No entanto, há casos em que o texto que está sendo a saída deve ser HTML ou já está codificado, caso em que isso pode levar à codificação dupla.

Para esses casos, o ASP.NET 4 apresenta uma nova interface, *IHtmlString*, juntamente com uma implementação concreta, *HtmlString*. Instâncias desses tipos permitem que você indicar que o valor de retorno é já devidamente codificado (ou caso contrário, examinado) para exibir como HTML, e que, portanto, o valor não deve ser codificada em HTML novamente. Por exemplo, a seguir não devem ser (e não é) codificado em HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Métodos de auxiliares do ASP.NET MVC 2 foram atualizados para trabalhar com essa nova sintaxe para que não são duplas codificado, mas somente quando você estiver executando o ASP.NET 4. Essa nova sintaxe não funciona quando você executa um aplicativo usando ASP.NET 3.5 SP1.

Tenha em mente que isso não garante a proteção contra ataques XSS. Por exemplo, o HTML que usa valores de atributo que não estão entre aspas pode conter entradas do usuário que ainda são suscetíveis. Observe que a saída de controles do ASP.NET e os auxiliares do ASP.NET MVC sempre inclui valores de atributo entre aspas, que é a abordagem recomendada.

Da mesma forma, essa sintaxe não faz a codificação de JavaScript, como quando você cria uma cadeia de caracteres de JavaScript com base na entrada do usuário.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Alterações do modelo de projeto

Em versões anteriores do ASP.NET, quando você usa o Visual Studio para criar um novo projeto de Site da Web ou um projeto de aplicativo Web, os projetos resultantes contêm somente uma página Default. aspx, um padrão `Web.config` arquivo e o `App_Data` pasta, conforme mostrado no exemplo a seguir ilustração:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio também dá suporte a um tipo de projeto de Site vazio, que não contém nenhum arquivo algum, conforme mostrado na figura a seguir:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

O resultado é que para iniciantes, há muito pouca orientação sobre como criar um aplicativo Web de produção. Portanto, o ASP.NET 4 apresenta três novos modelos, um para um projeto de aplicativo Web vazio e um para um projeto de aplicativo Web e o Site da Web.

#### <a name="empty-web-application-template"></a>Modelo de aplicativo Web vazio

Como o nome sugere, o modelo de aplicativo Web vazio é um projeto de aplicativo Web simplificado. Você pode selecionar esse modelo de projeto na caixa de diálogo Novo projeto do Visual Studio, conforme mostrado na figura a seguir:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image8.png))

Quando você cria um aplicativo de Web ASP.NET vazio, o Visual Studio cria o layout de pasta a seguir:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Isso é semelhante ao layout do Site da Web vazio de versões anteriores do ASP.NET, com uma exceção. No Visual Studio 2010, projetos de aplicativo Web vazio e o Site da Web vazio contêm o seguinte mínimo `Web.config` arquivo que contém informações usadas pelo Visual Studio para identificar a estrutura de destino do projeto:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Sem essa *targetFramework* propriedade, os padrões do Visual Studio para o direcionamento do .NET Framework 2.0 para preservar a compatibilidade ao abrir aplicativos mais antigos.

#### <a name="web-application-and-web-site-project-templates"></a>Modelos de projeto de Site da Web e aplicativo Web

Outros dois novos modelos de projeto que são fornecidos com o Visual Studio 2010 contêm alterações importantes. A figura a seguir mostra o layout de projeto que é criado quando você cria um novo projeto de aplicativo Web. (O layout para um projeto de Site da Web é praticamente idêntico).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

O projeto inclui um número de arquivos que não foram criados em versões anteriores. Além disso, o novo projeto de aplicativo Web está configurado com a funcionalidade de associação básica, que permite que você começar a usar rapidamente ao proteger o acesso para o novo aplicativo. Devido a essa inclusão, a `Web.config` do arquivo para o novo projeto inclui as entradas que são usadas para configurar a associação, funções e perfis. A exemplo a seguir mostra o `Web.config` arquivo para um novo projeto de aplicativo Web. (Nesse caso, *roleManager* está desabilitado.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image14.png))

O projeto também contém um segundo `Web.config` arquivo o `Account` directory. O segundo arquivo de configuração fornece uma maneira de proteger o acesso à página ChangePassword para não-os usuários conectados. O exemplo a seguir mostra o conteúdo do segundo `Web.config` arquivo.

![](overview/_static/image15.png)

As páginas criadas por padrão em novos modelos de projeto também contêm o conteúdo mais do que nas versões anteriores. O projeto contém uma página mestra padrão e o arquivo CSS, e a página padrão (default. aspx) é configurada para usar a página mestra por padrão. O resultado é que, quando você executar o aplicativo Web ou site da Web pela primeira vez, a página de padrão (home) já está funcional. Na verdade, é semelhante à página padrão que você vê quando você iniciar um novo aplicativo MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image18.png))

A intenção dessas alterações para os modelos de projeto é fornecer orientações sobre como começar a criar um novo aplicativo Web. Com semanticamente correta strict XHTML 1.0 compatível com marcação e layout que é especificado usando a CSS, as páginas nos modelos representam as práticas recomendadas para criação de aplicativos Web do ASP.NET 4. As páginas padrão também têm um layout de duas colunas que você pode personalizar facilmente.

Por exemplo, imagine que, para um novo aplicativo Web você deseja alterar algumas das cores e inserir o logotipo da empresa em vez do logotipo do meu aplicativo ASP.NET. Para fazer isso, você cria um novo diretório no `Content` para armazenar sua imagem de logotipo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Para adicionar a imagem para a página, você, em seguida, abra o `Site.Master` do arquivo, encontre onde o texto do meu aplicativo ASP.NET é definido e substituí-lo com um *imagem* elemento cujo *src* atributo é definido como o novo logotipo imagem, como no exemplo a seguir:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image22.png))

Você pode ir para o arquivo de CSS e modificar definições de classe CSS para alterar a cor do plano de fundo da página, bem como de cabeçalho.

O resultado dessas alterações é que você pode exibir uma home page personalizada com muito pouco esforço:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Aprimoramentos de CSS

Uma das principais áreas de trabalho no ASP.NET 4 foi ajudam a renderizar HTML que está em conformidade com os padrões mais recentes do HTML. Isso inclui alterações em como os controles de servidor Web do ASP.NET usam estilos CSS.

#### <a name="compatibility-setting-for-rendering"></a>Configuração de compatibilidade para renderização

Por padrão, quando um aplicativo Web ou site da Web tem como alvo o .NET Framework 4, o *controlRenderingCompatibilityVersion* atributo da *páginas* elemento for definido como "4.0". Esse elemento é definido no nível do computador `Web.config` de arquivo e por padrão se aplica a todos os aplicativos ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

O valor para *controlRenderingCompatibility* é uma cadeia de caracteres, que permite potencial novas definições de versão em versões futuras. Na versão atual, os valores a seguir têm suporte para essa propriedade:

- "3.5". Essa configuração indica a marcação e renderização herdada. Marcação processada pelos controles é 100% compatível com versões anteriores e a configuração do *xhtmlConformance* propriedade será respeitada.
- "4.0". Se a propriedade tiver essa configuração, os controles de servidor Web do ASP.NET faça o seguinte:
- O *xhtmlConformance* propriedade sempre será tratada como "Estrito". Como resultado, controles de renderizam a marcação XHTML 1.0 Strict.
- Desabilitar os controles de entrada não renderiza estilos inválidos.
- *div* elementos em torno de campos ocultos são denominados agora para que eles não interfiram com regras CSS criadas pelo usuário.
- Controles de menu renderizam a marcação que é semanticamente correta e em conformidade com as diretrizes de acessibilidade.
- Controles de validação não renderizam estilos embutidos.
- Controla o que anteriormente renderizado borda = "0" (controles que derivam do ASP.NET *tabela* controle e o ASP.NET *imagem* controle) não processará mais esse atributo.

#### <a name="disabling-controls"></a>A desabilitação dos controles

No ASP.NET 3.5 SP1 e versões anteriores, o framework renderiza os *desabilitada* atributo na marcação HTML para qualquer controle cujo *Enabled* propriedade definida como *false*. No entanto, de acordo com a especificação de HTML 4.01, apenas *entrada* elementos devem ter esse atributo.

No ASP.NET 4, você pode definir as *controlRenderingCompatabilityVersion* propriedade como "3.5", como no exemplo a seguir:

[!code-xml[Main](overview/samples/sample70.xml)]

Você pode criar a marcação para um *rótulo* controle semelhante à seguinte, que desabilita o controle:

[!code-aspx[Main](overview/samples/sample71.aspx)]

O *rótulo* controle deve renderizar o HTML a seguir:

[!code-html[Main](overview/samples/sample72.html)]

No ASP.NET 4, você pode definir as *controlRenderingCompatabilityVersion* como "4.0". Nesse caso, apenas controles que processem *entrada* elementos renderizará uma *desabilitada* atributo quando o controle *habilitado* propriedade é definida como *false* . Controles que não processam HTML *entrada* elementos renderizados em vez disso, um *classe* atributo que faz referência a uma classe CSS que você pode usar para definir uma aparência desabilitada para o controle. Por exemplo, o *rótulo* controle mostrado no exemplo anterior geraria a seguinte marcação:

[!code-html[Main](overview/samples/sample73.html)]

O valor padrão para a classe que é especificado para esse controle é "aspNetDisabled". No entanto, você pode alterar esse valor padrão definindo estático *DisabledCssClass* propriedade estática da *WebControl* classe. Para desenvolvedores de controle, o comportamento a ser usado para um controle específico também pode ser definido usando o *SupportsDisabledAttribute* propriedade.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ocultando div elementos em torno de campos ocultos

O ASP.NET 2.0 e versões posteriores renderizam os campos ocultos específicos do sistema (como o *ocultos* elemento usado para armazenar informações de estado de exibição) dentro *div* elemento para manter a conformidade com o padrão XHTML. No entanto, isso pode causar um problema quando uma regra de CSS afeta *div* elementos em uma página. Por exemplo, isso pode resultar em uma linha de um pixel que aparecem na página de aproximadamente oculto *div* elementos. No ASP.NET 4 *div* elementos que coloque os campos ocultos gerados pelo ASP.NET adicionam uma referência de classe CSS como no exemplo a seguir:

[!code-html[Main](overview/samples/sample74.html)]

Em seguida, você pode definir uma classe CSS que se aplica apenas à *ocultos* elementos que são gerados pelo ASP.NET, como no exemplo a seguir:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Uma tabela externa de renderização para controles de modelo

Por padrão, os seguintes controles de servidor Web do ASP.NET que dão suporte a modelos são automaticamente encapsulados em uma tabela externa que é usada para aplicar estilos embutidos:

- *FormView*
- *Logon*
- *PasswordRecovery*
- *ChangePassword*
- *Assistente*
- *CreateUserWizard*

Uma nova propriedade chamada *RenderOuterTable* foi adicionado a esses controles que permite que a tabela externa a ser removido da marcação. Por exemplo, considere o exemplo a seguir de um *FormView* controle:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Essa marcação processa a saída a seguir para a página, que inclui uma tabela HTML:

[!code-html[Main](overview/samples/sample77.html)]

Para impedir que a tabela está sendo processado, você pode definir as *FormView* do controle *RenderOuterTable* propriedade, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample78.aspx)]

O exemplo anterior renderiza a seguinte saída, sem a *tabela*, *tr*, e *td* elementos:

> Conteúdo


Esse aprimoramento pode tornar mais fácil ao estilo o conteúdo do controle com o CSS, porque nenhuma marca inesperada é renderizada pelo controle.

> [!NOTE]
> Observe que essa alteração desabilita o suporte para a função de formatação automática no designer do Visual Studio 2010, porque não há um *tabela* elemento que pode hospedar os atributos de estilo que são gerados pela opção de formatação automática.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Aprimoramentos de controles ListView

O *ListView* controle se tornou mais fácil de usar no ASP.NET 4. A versão anterior do controle necessário especificar um modelo de layout que continha um controle de servidor com uma ID conhecida. A marcação a seguir mostra um exemplo típico de como usar o *ListView* controle no ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

No ASP.NET 4, o *ListView* controle não exige um modelo de layout. A marcação mostrada no exemplo anterior pode ser substituída com a seguinte marcação:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Aprimoramentos de controle RadioButtonList e CheckBoxList

No ASP.NET 3.5, você pode especificar o layout para o *CheckBoxList* e *RadioButtonList* usando duas configurações a seguir:

- *Fluxo*. O controle é processado *span* elementos para conter o seu conteúdo.
- *Tabela*. O controle processa uma *tabela* elemento para conter o seu conteúdo.

O exemplo a seguir mostra a marcação para cada um desses controles.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Por padrão, os controles de renderização HTML semelhante ao seguinte:

[!code-html[Main](overview/samples/sample82.html)]

Como esses controles contêm listas de itens para renderizar HTML semanticamente correta, eles deverão renderizar conteúdo usando a lista em HTML (*li*) elementos. Isso torna mais fácil para os usuários que ler páginas da Web usando a tecnologia assistencial e facilita os controles de estilo usando CSS.

No ASP.NET 4, o *CheckBoxList* e *RadioButtonList* controles dão suporte a novos valores a seguir para o *RepeatLayout* propriedade:

- *OrderedList* – o conteúdo é renderizado como *li* elementos dentro de uma *ol* elemento.
- *UnorderedList* – o conteúdo é renderizado como *li* elementos dentro de uma *ul* elemento.

O exemplo a seguir mostra como usar esses novos valores.

[!code-aspx[Main](overview/samples/sample83.aspx)]

A marcação anterior gera o seguinte HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Observação: se você definir *RepeatLayout* à *OrderedList* ou *UnorderedList*, o *RepeatDirection* propriedade não podem ser usada e serão gera uma exceção em tempo de execução se a propriedade foi definida em sua marcação ou código. A propriedade não teria nenhum valor, porque o layout visual desses controles é definido usando a CSS em vez disso.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Melhorias no controle de menu

Antes do ASP.NET 4, o *Menu* controle renderizado uma série de tabelas HTML. Isso tornou mais difícil aplicar estilos CSS fora definindo propriedades embutidas e também não estava em conformidade com padrões de acessibilidade.

No ASP.NET 4, o controle agora renderiza HTML usando marcação semântica que consiste em uma lista não ordenada e elementos de lista. O exemplo a seguir mostra a marcação em uma página ASP.NET para o *Menu* controle.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Quando a página é renderizada, o controle gera o seguinte HTML (o *onclick* código foi omitido por motivos de clareza):

[!code-html[Main](overview/samples/sample86.html)]

Além das melhorias de renderização, navegação de teclado do menu foi aperfeiçoada usando o gerenciamento de foco. Quando o *Menu* controle obtém foco, você pode usar as teclas de direção para navegar elementos. O *Menu* controle agora também anexa acessível avançados da internet (ARIA) as funções de aplicativos e atributos seg[wing a](http://www.w3.org/TR/wai-aria-practices/#menu "diretrizes Menu ARIA")para aprimorado acessibilidade.

Estilos para o controle de menu são renderizados em um bloco de estilo na parte superior da página, em vez de acordo com os elementos HTML renderizados. Se você deseja assumir controle total sobre o estilo para o controle, você pode definir o novo *IncludeStyleBlock* propriedade *falso*, caso em que o bloco de estilo não é emitido. Uma maneira de usar essa propriedade é usar o recurso de formatação automática no designer do Visual Studio para definir a aparência do menu. Você pode executar a página, abra a origem da página e, em seguida, copie o bloco de estilo renderizado para um arquivo CSS externo. No Visual Studio, desfazer o estilo e o conjunto *IncludeStyleBlock* à *falso*. O resultado é que a aparência de menu é definida usando estilos em uma folha de estilos externa.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Assistente e CreateUserWizard controles

O ASP.NET *assistente* e *CreateUserWizard* controles dão suporte a modelos que permitem que você defina o HTML que eles sejam renderizados. (*CreateUserWizard* deriva *assistente*.) O exemplo a seguir mostra a marcação para um modelo totalmente *CreateUserWizard* controle:

[!code-aspx[Main](overview/samples/sample87.aspx)]

O controle renderiza o HTML semelhante ao seguinte:

[!code-html[Main](overview/samples/sample88.html)]

No ASP.NET 3.5 SP1, embora você possa alterar o conteúdo do modelo, você ainda tem controle limitado sobre a saída a *assistente* controle. No ASP.NET 4, você pode criar uma *LayoutTemplate* modelo e inserção *espaço reservado* controles (usando nomes reservados) para especificar como deseja que o *controle Wizard* para renderizar. O exemplo a seguir mostra isso:

[!code-aspx[Main](overview/samples/sample89.aspx)]

O exemplo contém os seguintes espaços reservados no nomeados a *LayoutTemplate* elemento:

- *headerPlaceholder* – em tempo de execução, isso é substituído pelo conteúdo do *HeaderTemplate* elemento.
- *sideBarPlaceholder* – em tempo de execução, isso é substituído pelo conteúdo do *SideBarTemplate* elemento.
- *wizardStepPlaceHolder* – em tempo de execução, isso é substituído pelo conteúdo do *WizardStepTemplate* elemento.
- *navigationPlaceholder* – em tempo de execução, isso é substituído por nenhum modelo de navegação que você definiu.

A marcação no exemplo que usa espaços reservados renderiza o HTML a seguir (sem realmente definido nos modelos de conteúdo):

[!code-html[Main](overview/samples/sample90.html)]

O HTML único é agora não definidas pelo usuário é um *span* elemento. (Prevemos que em futuras versões, até mesmo os *span* elemento não será renderizado.) Isso lhe controle total sobre praticamente todo o conteúdo que é gerado pelo *assistente* controle.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC foi introduzido como uma estrutura de complemento para o ASP.NET 3.5 SP1 em março de 2009. Visual Studio 2010 inclui o ASP.NET MVC 2, que inclui recursos e funcionalidades.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Áreas de suporte

Áreas permitem que você grupo controladores e exibições em seções de um aplicativo grande em isolamento relativo de outras seções. Cada área pode ser implementada como um projeto ASP.NET MVC separado que pode ser referenciado pelo aplicativo principal. Isso ajuda a gerenciar a complexidade ao compilar um aplicativo grande e torna mais fácil para várias equipes para trabalhar em conjunto em um único aplicativo.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Suporte de validação do atributo de anotação de dados

*DataAnnotations* atributos permitem que você anexar a lógica de validação a um modelo por meio de atributos de metadados. *DataAnnotations* atributos foram introduzidos no dados dinâmicos do ASP.NET no ASP.NET 3.5 SP1. Esses atributos foram integrados ao associador de modelos padrão e fornecem um meio controlada por metadados para validar a entrada do usuário.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Auxiliares com modelos

Auxiliares com modelos permitem que você associar automaticamente editar e exibem modelos com tipos de dados. Por exemplo, você pode usar um auxiliar de modelo para especificar que um elemento de interface do usuário do seletor de data automaticamente será renderizado para um *System. DateTime* valor. Isso é semelhante aos modelos de campo no dados dinâmicos do ASP.NET.

O *HTML. editorfor* e *Html.DisplayFor* métodos auxiliares têm suporte interno para tipos de dados padrão de processamento complexos, bem como objetos com várias propriedades. Eles também personalizar renderização, permitindo que você aplicar atributos de anotação de dados, como *DisplayName* e *ScaffoldColumn* para o *ViewModel* objeto.

Muitas vezes você deseja personalizar a saída de auxiliares de interface do usuário ainda mais e tem controle total sobre o que é gerado. O *HTML. editorfor* e *Html.DisplayFor* métodos auxiliares dar suporte a isso usando um mecanismo de modelagem que permite que você defina modelos externos que podem ser substituídos e controle a saída renderizada. Os modelos podem ser renderizados individualmente para uma classe.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dados dinâmicos

Dados dinâmicos foi introduzidos na versão do .NET Framework 3.5 SP1 em meados de 2008. Esse recurso oferece muitos aprimoramentos para a criação de aplicativos controlados por dados, incluindo o seguinte:

- Uma experiência RAD para criar rapidamente um site controlado por dados.
- Validação automática com base nas restrições definidas no modelo de dados.
- A capacidade de alterar facilmente a marcação que é gerada para campos de *GridView* e *DetailsView* controles usando modelos de campo que fazem parte do seu projeto de dados dinâmicos.

> [!NOTE]
> Observação: para obter mais informações, consulte o [documentação de dados dinâmicos](https://msdn.microsoft.com/library/cc488545.aspx) na biblioteca MSDN.


Para o ASP.NET 4, dados dinâmicos foi aprimorado para dar aos desenvolvedores ainda mais poderosas para criar rapidamente sites controlados por dados.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Habilitar dados dinâmicos para projetos existentes

Recursos dinâmicos de dados que foram enviadas no .NET Framework 3.5 SP1 trouxe novos recursos, como o seguinte:

- Modelos de campo – eles fornecem modelos com base no tipo de dados para controles ligados a dados. Modelos de campo fornecem uma maneira mais simples para personalizar a aparência dos controles de dados que o uso de campos do modelo para cada campo.
- – A validação dinâmica de dados permite usar atributos em classes de dados para especificar a validação para cenários comuns, como os campos obrigatórios, verificação de intervalo, verificação de tipo, correspondência de padrões usando expressões regulares e validação personalizada. Validação é imposta por controles de dados.

No entanto, esses recursos tinham os seguintes requisitos:

- A camada de acesso a dados precisava se basear em LINQ to SQL ou Entity Framework.
- A únicos fonte de dados controles com suporte para esses recursos foram os *EntityDataSource* ou *LinqDataSource* controles.
- Os recursos necessários um projeto da Web que haviam sido criado usando os modelos de entidades de dados dinâmicos ou dados dinâmicos para ter todos os arquivos que foram necessários para suporte ao recurso.

É uma grande meta do suporte a dados dinâmicos no ASP.NET 4 habilitar a nova funcionalidade de dados dinâmicos para qualquer aplicativo ASP.NET. O exemplo a seguir mostra a marcação para controles que podem tirar proveito da funcionalidade de dados dinâmicos em uma página existente.

[!code-aspx[Main](overview/samples/sample91.aspx)]

No código da página, o código a seguir deve ser adicionado para habilitar o suporte de dados dinâmicos para esses controles:

[!code-csharp[Main](overview/samples/sample92.cs)]

Quando o *GridView* controle está no modo de edição, dados dinâmicos automaticamente que valida os dados inseridos no formato correto. Se não estiver, uma mensagem de erro é exibida.

Essa funcionalidade também fornece outros benefícios, como a capacidade de especificar padrão de valores para o modo de inserção. Sem dados dinâmicos, para implementar um valor padrão para um campo, você deve anexar a um evento, localize o controle (usando *FindControl*) e defina seu valor. No ASP.NET 4, o *EnableDynamicData* chamada dá suporte a um segundo parâmetro que permite que você passe valores padrão para qualquer campo no objeto, conforme mostrado neste exemplo:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Sintaxe declarativa controle de DynamicDataManager

O *DynamicDataManager* controle foi aprimorado para que você pode configurá-lo de forma declarativa, assim como acontece com a maioria dos controles no ASP.NET, em vez de apenas no código. A marcação para o *DynamicDataManager* controle é semelhante ao exemplo a seguir:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Essa marcação habilita o comportamento de dados dinâmicos para o controle GridView1 mencionada na *DataControls* seção o *DynamicDataManager* controle.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Modelos de entidade

Modelos de entidade oferecem uma nova maneira de personalizar o layout dos dados sem exigir que você criar uma página personalizada. Uso de modelos de página de *FormView* controle (em vez da *DetailsView* controle, conforme usado nos modelos de página em versões anteriores dos dados dinâmicos) e o *DynamicEntity* controle para renderizar os modelos de entidade. Isso lhe dá mais controle sobre a marcação que é renderizado por dados dinâmicos.

A lista a seguir mostra o novo layout de diretório do projeto que contém os modelos de entidade:

[!code-console[Main](overview/samples/sample95.cmd)]

O `EntityTemplate` diretório contém modelos para saber como exibir os objetos de modelo de dados. Por padrão, os objetos são renderizados usando o `Default.ascx` modelo, que oferece a marcação que se parece com a marcação criada pelo *DetailsView* controle usado por dados dinâmicos no ASP.NET 3.5 SP1. O exemplo a seguir mostra a marcação para o `Default.ascx` controle:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Os modelos padrão podem ser editados para alterar a aparência de todo o site. Há modelos para exibir, editar e operações de inserção. Novos modelos podem ser adicionados com base no nome do objeto de dados para alterar a aparência de apenas um tipo de objeto. Por exemplo, você pode adicionar o modelo a seguir:

[!code-console[Main](overview/samples/sample97.cmd)]

O modelo pode conter a seguinte marcação:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Os novos modelos de entidade são exibidos em uma página usando o novo *DynamicEntity* controle. Em tempo de execução, esse controle é substituído pelo conteúdo do modelo de entidade. A marcação a seguir mostra a *FormView* no controlar o `Detail.aspx` modelo de página que usa o modelo de entidade. Observe que o *DynamicEntity* elemento na marcação.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Novos modelos de campo para URLs e endereços de Email

O ASP.NET 4 apresenta dois novos modelos de campo interno, `EmailAddress.ascx` e `Url.ascx`. Esses modelos são usados para os campos que são marcados como *EmailAddress* ou *Url* com o *DataType* atributo. Para *EmailAddress* objetos, o campo é exibido como um hiperlink que é criado usando o *mailto:* protocolo. Quando os usuários clicarem no link, ele abre o cliente de email do usuário e cria uma mensagem de esqueleto. Objetos digitados como *Url* são exibidos como hiperlinks comuns.

O exemplo a seguir mostra como campos pode ser marcados.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Criação de Links com o controle DynamicHyperLink

Dados dinâmicos usam o novo recurso de roteamento que foi adicionado no .NET Framework 3.5 SP1 para controlar as URLs que os usuários finais veem quando eles acessarem o site da Web. O novo *DynamicHyperLink* controle torna fácil criar links para páginas em um site de dados dinâmicos. O exemplo a seguir mostra como usar o *DynamicHyperLink* controle:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Essa marcação cria um link que aponta para a página de lista para o `Products` tabela com base nas rotas que são definidas na `Global.asax` arquivo. O controle usa automaticamente o nome da tabela padrão que a página de dados dinâmicos se baseia.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Suporte para herança no modelo de dados

O Entity Framework e o LINQ to SQL dá suporte à herança em seus modelos de dados. Um exemplo disso pode ser um banco de dados que tem um `InsurancePolicy` tabela. Ele também pode conter `CarPolicy` e `HousePolicy` tabelas que têm os mesmos campos que `InsurancePolicy` e, em seguida, adicione mais campos. Dados dinâmicos foi modificados para entender a objetos herdados no modelo de dados e oferecer suporte a scaffolding para tabelas herdadas.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Suporte para relações muitos-para-muitos (apenas no Entity Framework)

O Entity Framework tem compatibilidade avançada para relações muitos-para-muitos entre tabelas, que é implementado, expondo a relação como uma coleção em uma *entidade* objeto. Novos `ManyToMany.ascx` e `ManyToMany_Edit.ascx` modelos de campo foram adicionados para oferecer suporte para exibir e editar dados que estão envolvidos em relações muitos-para-muitos.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Novos atributos para exibição do controle e suporte a enumerações

O *DisplayAttribute* foi adicionado para lhe dar mais controle sobre como os campos são exibidos. O *DisplayName* atributo em versões anteriores dos dados dinâmicos, é possível alterar o nome que é usado como uma legenda para um campo. O novo *DisplayAttribute* classe permite que você especifique mais opções para exibir um campo, como a ordem em que um campo é exibido e se um campo será usado como um filtro. O atributo também fornece controle independente do nome usado para os rótulos em um *GridView* controlar, o nome usado em uma *DetailsView* controlam, o texto de ajuda para o campo, e a marca d'água usados para o campo (se o campo aceita entrada de texto).

O *EnumDataTypeAttribute* classe foi adicionada para permitir que você mapear campos para enumerações. Quando você aplicar esse atributo a um campo, você pode especificar um tipo de enumeração. Dados dinâmicos usam o novo `Enumeration.ascx` modelo de campo para criar a interface do usuário para exibir e editar valores de enumeração. O modelo mapeia os valores do banco de dados para os nomes na enumeração.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Suporte aprimorado para filtros

Dynamic Data 1.0 fornecido com os filtros internos para colunas de boolianos e as colunas de chave estrangeira. Os filtros não permitiu que você especifique se eles foram exibidos ou em qual ordem que eles foram exibidos. O novo *DisplayAttribute* atributo soluciona ambos esses problemas, oferecendo a você controle sobre se uma coluna é exibida como um filtro e na ordem na qual ele será exibido.

Um aprimoramento adicional é que o suporte à filtragem foi[escrito para usar o novo](#0.2__QueryExtender "_QueryExtender") recurso de Web Forms. Isso permite criar filtros sem a necessidade de Conhecimento sobre o controle de fonte de dados que serão usados com os filtros. Junto com essas extensões, filtros também foram ativados em controles de modelo, que permite que você adicionar novos. Por fim, o *DisplayAttribute* classe mencionado anteriormente permite que o filtro padrão a ser substituído, da mesma forma que *UIHint* permite que o modelo de campo padrão para uma coluna a ser substituído.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Melhorias de desenvolvimento da Web do Visual Studio 2010

Desenvolvimento da Web no Visual Studio 2010 foi aprimorado para maior compatibilidade com CSS, aumento da produtividade por meio de trechos de código de marcação HTML e ASP.NET e o novo IntelliSense JavaScript dinâmico.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilidade com CSS aprimorado

O designer do Visual Web Developer no Visual Studio 2010 foi atualizado para melhorar a conformidade com os padrões CSS 2.1. O designer melhor preserva a integridade do código-fonte HTML e é mais robusto do que nas versões anteriores do Visual Studio. Nos bastidores, melhorias na arquitetura também foram feitas que permite melhor futuros aprimoramentos na capacidade de manutenção, layout e renderização.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Trechos de JavaScript e HTML

No editor de HTML, IntelliSense é preenchida automaticamente nomes de marca. O recurso de trechos de código do IntelliSense é preenchida automaticamente marcas inteiras e muito mais. No Visual Studio 2010, trechos de código do IntelliSense têm suporte para JavaScript, juntamente com o c# e Visual Basic, que foram suportados nas versões anteriores do Visual Studio.

Visual Studio 2010 inclui mais de 200 trechos de código que ajudam você a auto-completar ASP.NET e HTML marcas comuns, incluindo os atributos necessários (como runat = "server") e específicos a uma marca de atributos comuns (como *identificação*,  *DataSourceID*, *ControlToValidate*, e *texto*).

Você pode baixar trechos de código adicionais, ou você pode escrever seus próprios trechos de código que encapsulam os blocos de marcação que você ou sua equipe usar para tarefas comuns.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Aprimoramentos do JavaScript IntelliSense

No Visual 2010, o JavaScript IntelliSense foi reprojetado para fornecer uma experiência de edição ainda mais avançada. Agora, o IntelliSense reconhece objetos dinamicamente geradas por métodos como *registerNamespace* e por técnicas semelhantes usadas por outras estruturas de JavaScript. Desempenho foi aprimorado para analisar grandes bibliotecas de script e exibir o IntelliSense com pouco ou nenhum atraso de processamento. Compatibilidade foi aumentada drasticamente para dar suporte a quase todas as bibliotecas de terceiros e para dar suporte a diversos estilos de codificação. Comentários de documentação agora são analisados conforme você digita e imediatamente é usados pelo IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Implantação de aplicativos Web com o Visual Studio 2010

Quando os desenvolvedores do ASP.NET implanta um aplicativo Web, eles geralmente encontrarem que encontram problemas como os seguintes:

- Implantando em um site de hospedagem compartilhado exige tecnologias, como FTP, que pode ser lenta. Além disso, manualmente, você deve executar tarefas como a execução de scripts SQL para configurar um banco de dados e você deve alterar as configurações do IIS, como a configuração de uma pasta de diretório virtual como um aplicativo.
- Em um ambiente empresarial, além de implantar os arquivos do aplicativo Web, os administradores frequentemente devem modificar arquivos de configuração do ASP.NET e as configurações do IIS. Os administradores de banco de dados devem executar uma série de scripts SQL para obter o banco de dados do aplicativo em execução. Essas instalações são muito trabalhosas, muitas vezes levar horas para ser concluído e deve ser cuidadosamente documentadas.

Visual Studio 2010 inclui tecnologias que esses problemas e que permitem que você implante facilmente aplicativos Web. Uma dessas tecnologias é a ferramenta de implantação da Web do IIS (MsDeploy.exe).

Recursos de implantação da Web no Visual Studio 2010 incluem as seguintes áreas principais:

- Empacotamento de Web
- Transformação de Web. config
- Implantação de banco de dados
- Um clique para publicar para aplicativos Web

As seções a seguir fornecem detalhes sobre esses recursos.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Empacotamento de Web

Visual Studio 2010 usa a ferramenta MSDeploy para criar um arquivo compactado (. zip) para seu aplicativo, que é conhecido como um *pacote da Web*. O arquivo de pacote contém metadados sobre seu aplicativo mais o seguinte conteúdo:

- Configurações de IIS, que inclui as configurações do pool de aplicativos, configurações de página de erro e assim por diante.
- O conteúdo da Web real, que inclui páginas da Web, controles de usuário, conteúdo estático (imagens e arquivos HTML) e assim por diante.
- Dados e esquemas de banco de dados do SQL Server.
- Certificados de segurança, componentes a serem instalados no GAC, as configurações do registro e assim por diante.

Um pacote da Web pode ser copiado para qualquer servidor e, em seguida, instalado manualmente usando o Gerenciador do IIS. Como alternativa, para a implantação automática, o pacote pode ser instalado usando os comandos de linha de comando ou usando APIs de implantação.

Visual Studio 2010 fornece tarefas do MSBuild e destinos para criar pacotes da Web internos. Para obter mais informações, consulte [visão geral de implantação de projeto de aplicativo Web ASP.NET](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) no site do MSDN e [10 + 20 motivos por que você deve criar um pacote da Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) no blog de Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformação de Web. config

Para a implantação de aplicativo Web, o Visual Studio 2010 introduz [transformação XDT (documentos XML)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), que é um recurso que permite transformar um `Web.config` arquivo de configurações de desenvolvimento para as configurações de produção. Configurações de transformação são especificadas em arquivos de transformação denominados `web.debug.config`, `web.release.config`e assim por diante. (Os nomes desses arquivos corresponderem as configurações do MSBuild.) Um arquivo de transformação inclui apenas as alterações que você precisa fazer implantado `Web.config` arquivo. Você pode especificar as alterações usando a sintaxe simple.

O exemplo a seguir mostra uma parte de um `web.release.config` arquivo que pode ser produzido para a implantação da sua configuração de versão. A palavra-chave de substituir o exemplo especifica que durante a implantação do *connectionString* nó no `Web.config` arquivo será substituído pelos valores que estão listados no exemplo.

[!code-xml[Main](overview/samples/sample102.xml)]

Para obter mais informações, consulte [sintaxe de transformação Web. config para implantação de projeto de aplicativo da Web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) no MSDN <a id="0.2_a"> </a> site da Web e[implantação da Web: transformação de Web. config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)no blog de Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Implantação de banco de dados

Um pacote de implantação do Visual Studio 2010 pode incluir dependências em bancos de dados do SQL Server. Como parte da definição do pacote, você pode fornecer a cadeia de caracteres de conexão do banco de dados de origem. Quando você cria o pacote da Web, o Visual Studio 2010 cria scripts SQL para o esquema de banco de dados e, opcionalmente, para os dados e, em seguida, adiciona-os ao pacote. Você também pode fornecer scripts SQL personalizados e especifique a sequência na qual ele deve ser executado no servidor. No momento da implantação, você fornecer uma cadeia de conexão que é apropriada para o servidor de destino; o processo de implantação, em seguida, usa essa cadeia de conexão para executar os scripts que cria o esquema de banco de dados e adiciona os dados.

Além disso, ao usar um único clique, publicar, você pode configurar a implantação para publicar seu banco de dados diretamente quando o aplicativo é publicado em um site remoto de hospedagem compartilhado. Para obter mais informações, consulte [como: implantar um banco de dados com um projeto de aplicativo Web](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) no site do MSDN e [implantação de banco de dados com o VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) no blog de Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Um clique para publicar para aplicativos Web

Visual Studio 2010 também permite usar o serviço de gerenciamento remoto do IIS para publicar um aplicativo Web para um servidor remoto. Você pode criar um perfil de publicação para sua conta de hospedagem ou para servidores de teste ou servidores de teste. Cada perfil pode salvar com segurança as credenciais apropriadas. Você pode implantar em qualquer um de destino da barra de ferramentas de publicação de servidores com um clique por meio da Web em um único clique. Com o Visual Studio 2010, você também pode publicar usando a linha de comando do MSBuild. Isso permite que você configurar seu ambiente de compilação de equipe para incluir a publicação em um modelo de integração contínua.

Para obter mais informações, consulte [como: implantar uma Web Application Project usando um clique a publicação e implantação da Web](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) no site do MSDN e [Web publicar 1 Clique com o VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) no blog de Vishal Joshi. Para exibir apresentações de vídeo sobre a implantação de aplicativo Web no Visual Studio 2010, consulte [VS 2010 para visualizações de desenvolvedor da Web](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) no blog de Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Recursos

Os sites a seguir fornecem informações adicionais sobre o ASP.NET 4 e Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — a documentação oficial do ASP.NET 4 no site do MSDN.
- [https://www.asp.net/](https://www.asp.net/) — O ASP.NET no site da Web da equipe.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) e [mapa de conteúdo de dados dinâmicos ASP.NET](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — recursos Online no site de equipe do ASP.NET e na documentação oficial para dados dinâmicos do ASP.NET.
- [https://www.asp.net/ajax/](../../ajax/index.md) — O recurso da Web principal para o desenvolvimento do ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — O blog da equipe do Visual Web Developer, que inclui informações sobre os recursos no Visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — o recurso da Web principal para as versões de visualização do ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este é um documento preliminar e pode ser alterado substancialmente antes da versão comercial final do software descrito aqui.

As informações contidas neste documento representam a visão atual da Microsoft Corporation sobre os problemas discutidos a partir da data da publicação. Como a Microsoft deve responder às condições do mercado em constante mudança, este documento não deve ser interpretado como um compromisso por parte da Microsoft, e a Microsoft não pode garantir a exatidão de nenhuma informação apresentada após a data da publicação.

Este whitepaper é apenas para fins informativos. A MICROSOFT NÃO OFERECE GARANTIA, EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA, DAS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO.

Estar em conformidade com todas as leis de direitos autorais aplicáveis é responsabilidade do usuário. Sem limitar os direitos autorais, nenhuma parte deste documento pode ser reproduzida, armazenada ou inserida em um sistema de recuperação, ou transmitida de qualquer forma, por qualquer meio (eletrônico, mecânico, fotocópia, gravação ou outro) ou para qualquer fim, sem a permissão expressa, por escrito, da Microsoft Corporation.

A Microsoft pode ter patentes, solicitações de patente, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual que abrangem o conteúdo deste documento. A não ser que especificado expressamente em um contrato de licença da Microsoft, o fornecimento deste documento não confere a você nenhum direito sobre as supracitadas patentes, marcas comerciais, direitos autorais ou outra propriedade intelectual.

A menos que indicado o contrário, os exemplos de empresas que as organizações, produtos, nomes de domínio, endereços de email, logotipos, pessoas, lugares e eventos aqui representados são fictícios e nenhuma associação com qualquer empresa, organização, produto, nome de domínio, email endereço, logotipo, pessoa, lugar ou evento é intencional ou deve ser inferido.

© 2009 Microsoft Corporation. Todos os direitos reservados.

Microsoft e Windows são marcas registradas ou comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.

Os nomes de empresas reais e produtos mencionados aqui podem ser marcas comerciais de seus respectivos proprietários.
