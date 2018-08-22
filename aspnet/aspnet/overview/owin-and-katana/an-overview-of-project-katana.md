---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Uma visão geral do projeto Katana | Microsoft Docs
author: howarddierking
description: A estrutura do ASP.NET já existe há mais de dez anos, e a plataforma habilitou o desenvolvimento de inúmeros sites da Web e serviços. Como o aplicativo Web...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 52007eba109de28c6d178505b82b1d5ff2883b47
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824356"
---
<a name="an-overview-of-project-katana"></a>Uma visão geral do projeto Katana
====================
por [Howard Dierking](https://github.com/howarddierking)

> A estrutura do ASP.NET já existe há mais de dez anos, e a plataforma habilitou o desenvolvimento de inúmeros sites da Web e serviços. Conforme as estratégias de desenvolvimento de aplicativos Web evoluíram, o framework foi capaz de evoluir em etapa com tecnologias como ASP.NET MVC e API Web ASP.NET. Desenvolvimento de aplicativo Web leva sua próxima etapa revolucionário no mundo da computação em nuvem, do projeto [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) fornece o conjunto subjacente de componentes para aplicativos ASP.NET, permitindo que eles ser flexível e portátil, leve e fornecer um desempenho melhor – em outras palavras, o projeto [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) nuvem otimiza a seus aplicativos ASP.NET.


## <a name="why-katana--why-now"></a>Por que Katana – por que agora?

 Independentemente se um está discutindo um produto de framework ou do usuário final do desenvolvedor, é importante compreender as motivações subjacentes para criar o produto – e a parte do que inclui saber quem o produto foi criado para. ASP.NET foi criado originalmente com dois clientes em mente.   
  
**O primeiro grupo de clientes foi de desenvolvedores do ASP clássico.** No momento, o ASP foi uma das principais tecnologias para criar dinâmicos, controlados por dados, sites e aplicativos Web por intercalação marcação e o script do lado do servidor. O tempo de execução do ASP fornecido pelo script do lado do servidor com um conjunto de objetos que abstrai aspectos essenciais do protocolo HTTP subjacente e o servidor Web e desde que tal gerenciamento de estado de sessão e de aplicativo, serviços de acesso a mais armazenar em cache, etc. Embora poderoso, aplicativos ASP clássicos se tornou um desafio gerenciar como eles aumentou de tamanho e complexidade. Isso era em grande parte devido à falta de estrutura encontrada em ambientes juntamente com a eliminação de duplicação de código resultante da intercalação de código e marcação de script. Para capitalizar os pontos fortes do ASP clássico enquanto atende a alguns de seus desafios, ASP.NET aproveitei a organização de código fornecida pelas linguagens orientadas a objeto do .NET Framework, também, preservando o modelo de programação do lado do servidor Para quais ASP clássico os desenvolvedores tinham estão acostumados.

**O segundo grupo de clientes-alvo para o ASP.NET era que os desenvolvedores de aplicativos de negócios do Windows.** Ao contrário de desenvolvedores do ASP clássico, que estavam acostumados a escrever a marcação HTML e o código para gerar mais uma marcação HTML, os desenvolvedores de WinForms (assim como os desenvolvedores de VB6 antes delas) estava acostumado a uma experiência de tempo de design que incluía uma tela e um conjunto avançado de usuário controles de interface. A primeira versão do ASP.NET – também conhecido como "Web Forms" forneceu uma experiência de tempo de design semelhante, juntamente com um modelo de evento do lado do servidor para os componentes de interface de usuário e um conjunto de recursos de infraestrutura (por exemplo, o ViewState) para criar uma experiência perfeita de desenvolvedor entre cliente e programação do lado do servidor. Web Forms hid efetivamente a natureza sem monitoração de estado da Web em um modelo de evento com monitoração de estado que fosse familiar aos desenvolvedores do WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Desafios gerados pelo modelo do histórico

**O resultado foi um tempo de execução maduro e ricas em recursos e o modelo de programação do desenvolvedor.** No entanto, com essa riqueza de recurso veio alguns desafios importantes. Em primeiro lugar, a estrutura foi **monolítico**, com unidades logicamente distintas de funcionalidade sendo fortemente acoplados no mesmo assembly dll (por exemplo, os objetos principais HTTP com a estrutura de formulários da Web). Em segundo lugar, o ASP.NET foi incluído como parte do maior do .NET Framework, que significa que o **tempo entre as versões foi na ordem de anos.** Isso tornou difícil para o ASP.NET acompanhar todas as alterações ocorrendo em rápida evolução de desenvolvimento da Web. Por fim, a DLL em si foi acoplado de algumas maneiras diferentes para uma opção de hospedagem de Web específica: serviços de informações da Internet (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Etapas evolutivas: ASP.NET MVC e API Web ASP.NET

E muita alteração estava acontecendo no desenvolvimento da Web! Aplicativos Web cada vez mais sendo desenvolvidos como uma série de pequenos, voltada para componentes em vez de estruturas grandes. O número de componentes, bem como a frequência com a qual elas foram lançadas foi aumentando a uma taxa mais rápida do que nunca. Ficou claro que manter ritmo com a Web exigiria estruturas para obter menor, desacoplada e mais concentrados em vez de maiores e mais rica em recursos, portanto o **equipe ASP.NET levou várias etapas evolutivas para habilitar o ASP.NET como uma família de componentes da Web conectáveis em vez de uma única estrutura**.

Uma das alterações antecipadas foi o aumento na popularidade do conhecido model-view-controller (MVC) padrão de design graças a estruturas de desenvolvimento da Web como Ruby on Rails. O desenvolvedor deu a esse estilo de criação de aplicativos Web maior controle sobre a marcação do seu aplicativo, preservando ainda a separação da lógica de negócios e marcação, que foi um dos pontos de venda inicias para o ASP.NET. Para atender à demanda por esse estilo de desenvolvimento de aplicativos Web, a Microsoft fez a oportunidade de se posicionar melhor para o futuro por **desenvolvimento do ASP.NET MVC fora da banda** (e não incluí-lo no .NET Framework). ASP.NET MVC foi lançado como um download independente. Isso proporcionou a equipe de engenharia a flexibilidade para fornecer atualizações com uma frequência muito mais do que tinha sido possível anteriormente.

Outra grande mudança no desenvolvimento de aplicativos Web foi a mudança de páginas de Web dinâmicas, gerados pelo servidor para marcação inicial estática com seções dinâmicas da página gerada de script do lado do cliente se comunicar **com as APIs da Web de back-end por meio de As solicitações AJAX**. Essa mudança arquitetônica ajudou a impulsionar o crescimento de APIs da Web e o desenvolvimento de estrutura da API de Web do ASP.NET. Como no caso do ASP.NET MVC, a versão da API Web ASP.NET fornecido outra oportunidade de evoluir ASP.NET ainda mais como uma estrutura mais modular. A equipe de engenharia de TI aproveitaram a oportunidade e **compilados para API Web ASP.NET que ele tinha sem dependências em qualquer um dos tipos de framework core encontrados na dll**. Isso permitiu que duas coisas: em primeiro lugar, isso significava que API Web ASP.NET pode evoluir de maneira totalmente independente (e ele pode continuar a iterar rapidamente, pois ela é entregue via NuGet). Em segundo lugar, porque não havia nenhuma dependência externa para dll e, portanto, nenhuma dependência no IIS, ASP.NET Web API incluída a capacidade de executar em um host personalizado (por exemplo, um aplicativo de console, serviço do Windows, etc.)

### <a name="the-future-a-nimble-framework"></a>O futuro: Uma estrutura ágil

Dissociação de componentes do framework uns dos outros e, em seguida, liberando-os no NuGet, estruturas poderia agora **iterar de forma mais independente e mais rapidamente**. Além disso, a potência e flexibilidade da funcionalidade de hospedagem interna da API Web provado que ele é muito atraentes para os desenvolvedores que desejavam um **host pequeno e leve** para seus serviços. Se provou tão atraente, na verdade, ou outras estruturas também queriam essa funcionalidade e isso exposto a um novo desafio em que cada estrutura em seu próprio processo de host em seu próprio endereço de base e precisava ser gerenciado (iniciado, interrompido, etc.) independentemente. Um aplicativo da Web modernos geralmente oferece suporte à geração de página dinâmica, fornecimento de arquivos estáticos, API da Web e mais recentemente real-tempo/notificações por push. Esperar que cada um desses serviços deve ser executada e gerenciada de forma independente simplesmente não era realista.

O que era necessário era uma única abstração de hospedagem que permitem que um desenvolvedor para compor um aplicativo de uma variedade de diferentes componentes e estruturas e, em seguida, execute esse aplicativo em um host de suporte.

## <a name="the-open-web-interface-for-net-owin"></a>A Interface Web aberta para .NET (OWIN)

 Inspirado pelos benefícios obtidos pela [Rack](http://rack.github.io/) na comunidade do Ruby, vários membros da comunidade do .NET é propus a elaborar uma abstração entre servidores Web e componentes do framework. Duas metas de design para a abstração de OWIN eram que fosse simple e que levou o mínimo possível de dependências em outros tipos de estrutura. Essas duas metas ajudam a garantir:

- Novos componentes poderia mais facilmente desenvolvidos e consumidos.
- Aplicativos poderiam ser mais facilmente transportados entre hosts e sistemas operacionais/plataformas da potencialmente todo.

A abstração resultante consiste em dois elementos principais. A primeira é o dicionário do ambiente. Essa estrutura de dados é responsável por armazenar todos os estados necessários para processar uma solicitação HTTP e resposta, bem como qualquer estado do servidor relevante. O dicionário do ambiente é definido da seguinte maneira:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Um servidor Web compatível com o OWIN é responsável por preencher o dicionário do ambiente com os dados, como os fluxos de corpo e as coleções de cabeçalho para uma solicitação e resposta HTTP. Em seguida, é responsabilidade dos componentes do aplicativo ou estrutura para preencher ou atualizar o dicionário com valores adicionais e gravar no fluxo de corpo de resposta.

Além de especificar o tipo para o dicionário do ambiente, a especificação do OWIN define uma lista de pares de chave-valor do dicionário principal. Por exemplo, a tabela a seguir mostra as chaves de dicionário necessários para uma solicitação HTTP:

| Nome da chave | Descrição do valor |
| --- | --- |
| `"owin.RequestBody"` | Um Stream com o corpo da solicitação, se houver. NULL pode ser usado como um espaço reservado, se não houver nenhum corpo de solicitação. Ver [corpo da solicitação](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Um `IDictionary<string, string[]>` de cabeçalhos de solicitação. Ver [cabeçalhos](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Um `string` que contém o método de solicitação HTTP da solicitação (por exemplo, `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Um `string` que contém o caminho da solicitação. O caminho deve ser relativo à "raiz" do representante do aplicativo; ver [caminhos](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Um `string` que contém a parte do caminho da solicitação correspondente à "raiz" do representante do aplicativo; consulte [caminhos](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Um `string` que contém o nome do protocolo e a versão (por exemplo, `"HTTP/1.0"` ou `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Um `string` que contém o componente de cadeia de caracteres de consulta do URI, da solicitação HTTP sem o entrelinhamento "?" (por exemplo, `"foo=bar&baz=quux"`). O valor pode ser uma cadeia de caracteres vazia. |
| `"owin.RequestScheme"` | Um `string` que contém o esquema de URI usado para a solicitação (por exemplo, `"http"`, `"https"`); consulte [esquema URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

O segundo elemento chave do OWIN é o representante do aplicativo. Isso é uma assinatura de função que serve como a principal interface entre todos os componentes em um aplicativo OWIN. A definição para o representante do aplicativo é o seguinte:

`Func<IDictionary<string, object>, Task>;`

O representante do aplicativo, em seguida, é simplesmente uma implementação do tipo de delegado Func em que a função aceita o dicionário do ambiente como entrada e retorna uma tarefa. Esse design tem várias implicações para os desenvolvedores:

- Há um número muito pequeno de dependências de tipo necessário para escrever componentes do OWIN. Isso aumenta muito a acessibilidade do OWIN para desenvolvedores.
- O design assíncrono permite que a abstração ser eficiente com sua manipulação de recursos de computação, especialmente em operações com uso intensivo de e/s mais.
- Porque o representante do aplicativo é uma unidade atômica de execução e porque o dicionário do ambiente é executado como um parâmetro no representante, componentes do OWIN podem ser facilmente encadeados juntos para criar pipelines de processamento de HTTP complexa.

Da perspectiva de implementação, o OWIN é uma especificação ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Sua meta não é deve ser a estrutura da Web próxima, mas em vez disso, uma especificação para como estruturas da Web e servidores Web interagem.

Se você investigamos [OWIN](http://owin.org/) ou [Katana](https://github.com/aspnet/AspNetKatana/wiki), você talvez tenha observado também a [pacote NuGet do Owin](http://nuget.org/packages/Owin) e Owin.dll. Esta biblioteca contém uma única interface, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), que formaliza e codifica a sequência de inicialização descrita em [seção 4](http://owin.org/html/owin.html#4-application-startup) da especificação do OWIN. Embora não seja necessário a fim de criar servidores do OWIN, o [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interface fornece um ponto de referência concreta, e ele é usado pelos componentes do projeto Katana.

## <a name="project-katana"></a>Projeto Katana

Ao passo que tanto a [OWIN](http://owin.org/html/owin.html) especificação e *Owin.dll* são de propriedade de comunidade e comunidade executar esforços de código-fonte aberto, o [Katana](https://github.com/aspnet/AspNetKatana/wiki) projeto representa o conjunto de OWIN componentes que, embora ainda software livre, são criados e lançados pela Microsoft. Esses componentes incluem componentes de infraestrutura, como hosts e servidores, bem como componentes funcionais, como componentes de autenticação e as associações para estruturas, como [SignalR](../../../signalr/index.md) e [da Web do ASP.NET API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). O projeto tem as três seguintes metas de alto níveis: 

- **Portátil** – componentes devem ser capazes de facilmente ser substituídos por novos componentes, assim que estiverem disponíveis. Isso inclui todos os tipos de componentes do framework para o servidor e o host. A implicação desse objetivo é que as estruturas de terceiros podem executar perfeitamente em servidores da Microsoft enquanto as estruturas da Microsoft podem potencialmente operar em hosts e servidores de terceiros.
- **Modular/flexível**– ao contrário de muitas estruturas que incluem uma infinidade de recursos que são ativados por padrão, os componentes do projeto Katana devem ser pequeno e focado, oferecendo controle ao desenvolvedor de aplicativos na determinação de quais componentes Use em seu aplicativo.
- **Lightweight/alto desempenho/escalonável** – dividindo a noção tradicional de uma estrutura em um conjunto de pequenos, voltada para componentes que são adicionados explicitamente pelo desenvolvedor do aplicativo, um aplicativo do Katana resultante pode consumir menos de computação recursos e como resultado, lidar com mais carga, que com outros tipos de servidores e estruturas. Como os requisitos do aplicativo exigem mais recursos da infraestrutura subjacente, aqueles podem ser adicionados ao pipeline do OWIN, mas que deve ser uma decisão explícita por parte do desenvolvedor do aplicativo. Além disso, a possibilidade de substituição de componentes de nível inferior significa que assim que estiverem disponíveis, novos servidores de alto desempenho podem perfeitamente ser introduzidos para melhorar o desempenho de aplicativos OWIN sem interromper os aplicativos.

## <a name="getting-started-with-katana-components"></a>Introdução aos componentes do Katana

Quando ele foi introduzido pela primeira vez, um aspecto do [Node. js](http://nodejs.org/) framework que desenhou imediatamente atenção foi a simplicidade com que um pode criar e executar um servidor Web. Se os objetivos do Katana foram enquadrados light da [Node. js](http://nodejs.org/), um pode resumi-los dizendo que o Katana traz muitos benefícios de [Node. js](http://nodejs.org/) (e estruturas como ele) sem forçar o desenvolvedor lançar tudo o que ela sabe sobre o desenvolvimento de aplicativos Web do ASP.NET. Para esta instrução servir, Introdução ao projeto Katana deve ser igualmente simple na natureza [Node. js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Criação de "Hello World!"

Uma diferença importante entre o desenvolvimento de JavaScript e .NET é a presença (ou ausência) de um compilador. Como tal, o ponto de partida para um servidor do Katana simples é um projeto do Visual Studio. No entanto, podemos começar com o mínimo de tipos de projeto: o aplicativo Web ASP.NET vazio.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Em seguida, vamos instalar o [systemweb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) pacote do NuGet no projeto. Esse pacote fornece um servidor OWIN que executa no pipeline de solicitação do ASP.NET. Ela pode ser encontrada na [Galeria do NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) e pode ser instalado usando a caixa de diálogo do Gerenciador de pacote do Visual Studio ou o console do Gerenciador de pacotes com o seguinte comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Instalando o `Microsoft.Owin.Host.SystemWeb` pacote irá instalar alguns pacotes adicionais como dependências. Uma dessas dependências é `Microsoft.Owin`, uma biblioteca que fornece vários tipos auxiliares e métodos para desenvolvimento de aplicativos do OWIN. Podemos usar esses tipos para rapidamente escrever o seguinte servidor de "hello world".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Esse servidor Web muito simple agora pode ser executado usando o Visual Studio **F5** de comando e inclui suporte completo para depuração.

## <a name="switching-hosts"></a>Alternância de hosts

Por padrão, o exemplo anterior de "hello world" é executado no pipeline de solicitação do ASP.NET, que usa o System. Web no contexto do IIS. Isso pode por si só adicionar um enorme valor conforme ele nos permite aproveitar a flexibilidade e a capacidade de composição de um pipeline do OWIN com os recursos de gerenciamento e a maturidade geral do IIS. No entanto, pode haver casos em que os benefícios fornecidos pelo IIS não são necessários e o desejo é para um host menor e mais leve. O que é necessário, em seguida, executar nosso servidor Web simple fora do IIS e System. Web?

Para ilustrar a meta de portabilidade, movendo de um host de servidor Web para um host de linha de comando requer simplesmente adicionar novas dependências de servidor e host a pasta de saída do projeto e, em seguida, iniciando o host. Neste exemplo, hospedaremos nosso servidor Web em um host do Katana chamado `OwinHost.exe` e usará o servidor com base no Katana HttpListener. Da mesma forma para os outros componentes do Katana, eles serão adquiridos do NuGet usando o seguinte comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Na linha de comando, podemos, em seguida, navegue até a pasta raiz do projeto e simplesmente executar o `OwinHost.exe` (que foi instalado na pasta Ferramentas de seu respectivo pacote de NuGet). Por padrão, `OwinHost.exe` está configurado para procurar o servidor baseada no HttpListener e, portanto, nenhuma configuração adicional é necessária. Navegando em um navegador da Web para `http://localhost:5000/` mostra o aplicativo agora em execução por meio do console.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Arquitetura do Katana

 A arquitetura de componente do Katana divide um aplicativo em quatro camadas de lógicas, conforme descrito abaixo: *host, server, middleware,* e *aplicativo*. A arquitetura do componente é decomposta de forma que as implementações dessas camadas podem ser facilmente substituídas, em muitos casos, sem a necessidade de recompilação do aplicativo.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 O host é responsável por:

- Gerenciar o processo subjacente.
- Orquestrar o fluxo de trabalho que resulta na seleção de um servidor e a construção de um pipeline do OWIN por meio do quais as solicitações será manipulada.

  No momento, há 3 opções de hospedagem primárias para aplicativos baseados no Katana:  
  
**IIS/ASP.NET**: usando os tipos de HttpModule e HttpHandler padrão, pipelines do OWIN podem executar no IIS como parte de um fluxo de solicitação do ASP.NET. Suporte à hospedagem do ASP.NET está habilitado com a instalação do pacote Microsoft.AspNet.Host.SystemWeb NuGet em um projeto de aplicativo Web. Além disso, como o IIS atua como um host e um servidor, a distinção de host do servidor OWIN é confundida nesse pacote de NuGet, que significa que, se usar o host SystemWeb, um desenvolvedor não pode substituir uma implementação de servidor alternativo.  
  
**Host personalizado**: conjunto de componentes do Katana o permite que um desenvolvedor para hospedar aplicativos em seu próprio processo personalizado, independentemente de ser um aplicativo de console, o serviço do Windows, etc. Essa funcionalidade é semelhante ao recurso de hospedar internamente fornecido pela API da Web. O exemplo a seguir mostra um host personalizado do código de API da Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

A configuração de hospedagem interna para um aplicativo do Katana é semelhante:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Uma diferença importante entre os exemplos de auto-hospedar a API Web e Katana é que o código de configuração de API da Web está ausente do exemplo de hospedar internamente Katana. Para habilitar a portabilidade e capacidade de composição, o Katana separa o código que inicia o servidor do código que configura o pipeline de processamento de solicitação. O código que configura a API da Web, em seguida, está contido na classe de inicialização, o que Além disso é especificada como parâmetro de tipo em WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

A classe de inicialização será discutida em mais detalhes posteriormente neste artigo. No entanto, o código necessário para iniciar um Katana hospedar internamente o processo parece bastante semelhante ao código que você possa estar usando hoje mesmo em aplicativos de hospedagem interna de API Web do ASP.NET.

**OwinHost.exe**: enquanto alguns vai querer escrever um processo personalizado para executar aplicativos da Web do Katana, muitos prefere simplesmente inicializar um executável criado previamente que pode iniciar um servidor e executar seu aplicativo. Para este cenário, o conjunto de componentes do Katana inclui `OwinHost.exe`. Quando executado no diretório raiz de um projeto, este executável será iniciar um servidor (ele usa o servidor de HttpListener por padrão) e usar as convenções para localizar e executar a classe de inicialização do usuário. Para um controle mais granular, o executável fornece um número de parâmetros de linha de comando adicionais.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Servidor

 Enquanto o host é responsável por iniciar e manter o processo no qual o aplicativo é executado, a responsabilidade do servidor é abrir um soquete de rede, escutar solicitações e enviá-los por meio do pipeline de componentes do OWIN especificado pelo usuário (como você já deve ter notado, este pipeline é especificado na classe de inicialização do desenvolvedor do aplicativo). Atualmente, o projeto Katana inclui duas implementações de servidor: 

- **Systemweb**: conforme mencionado anteriormente, o IIS junto com o pipeline do ASP.NET atua como um host e um servidor. Portanto, ao escolher essa opção de hospedagem, IIS tanto gerencia as preocupações de nível de host, como a ativação de processos e escuta solicitações HTTP. Para aplicativos Web do ASP.NET, ele envia as solicitações no pipeline do ASP.NET. O host Katana SystemWeb registra um ASP.NET HttpModule e HttpHandler para interceptar solicitações conforme eles fluem por meio do pipeline HTTP e enviá-los por meio do pipeline OWIN especificado pelo usuário.
- **Microsoft.Owin.Host.HttpListener**: como seu nome indica, esse servidor do Katana usa a classe de HttpListener do .NET Framework para abrir um soquete e enviar solicitações em um pipeline do OWIN especificados pelo desenvolvedor. Atualmente, isso é a seleção de servidor padrão para a API de hospedagem interna do Katana e OwinHost.exe.

## <a name="middlewareframework"></a>Middleware/estrutura

 Conforme mencionado anteriormente, quando o servidor aceita uma solicitação de um cliente é responsável por passando-o por meio de um pipeline de componentes do OWIN, que são especificados pelo código de inicialização do desenvolvedor. Esses componentes de pipeline são conhecidos como middleware.  
 Em um nível muito básico, um componente de middleware OWIN simplesmente precisa implementar o representante do aplicativo OWIN para que ele pode ser chamado.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

No entanto, para simplificar o desenvolvimento e a composição de componentes de middleware, Katana dá suporte a um punhado de convenções e tipos auxiliares para componentes de middleware. A mais comum deles é o `OwinMiddleware` classe. Um componente de middleware personalizado criado usando essa classe seria semelhante ao seguinte: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Essa classe deriva `OwinMiddleware`, implementa um construtor que aceita uma instância do próximo middleware no pipeline como um de seus argumentos e, em seguida, passa-o para o construtor base. Argumentos adicionais usados para configurar o middleware também são declarados como parâmetros do construtor após o próximo parâmetro de middleware.   
  
Em tempo de execução, o middleware é executado por meio de substituído `Invoke` método. Esse método usa um único argumento do tipo `OwinContext`. Esse objeto de contexto é fornecido pelo `Microsoft.Owin` pacote NuGet descrito anteriormente e fornece acesso fortemente tipado para o dicionário de solicitação, resposta e ambiente, juntamente com alguns tipos auxiliares adicionais.   
  
A classe de middleware pode ser facilmente adicionada ao pipeline do OWIN no código de inicialização do aplicativo da seguinte maneira:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Como a infraestrutura do Katana simplesmente cria um pipeline de componentes de middleware do OWIN, e os componentes precisam simplesmente dar suporte ao representante do aplicativo para participar do pipeline, componentes de middleware podem variar em complexidade de simples agentes a serem todas estruturas como ASP.NET, API da Web, ou [SignalR](../../../signalr/index.md). Por exemplo, a adição de API Web do ASP.NET para o pipeline OWIN anterior requer adicionando o seguinte código de inicialização:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

A infraestrutura do Katana criará o pipeline de componentes de middleware baseado na ordem em que foram adicionados ao objeto IAppBuilder no método de configuração. Em nosso exemplo, em seguida, LoggerMiddleware pode lidar com todas as solicitações que fluem por meio do pipeline, independentemente de como essas solicitações, por fim, são tratadas. Isso possibilita cenários poderosos em que um componente de middleware (por exemplo, um componente de autenticação) possa processar solicitações de um pipeline que inclui vários componentes e estruturas (por exemplo, API Web ASP.NET, SignalR e um servidor de arquivos estáticos).
 
## <a name="applications"></a>Aplicativos

Conforme ilustrado pelos exemplos anteriores, OWIN e o projeto Katana devem não ser considerados como um novo modelo de programação de aplicativo, mas em vez disso, como uma abstração para desassociar os modelos de programação de aplicativo e estruturas do servidor e infraestrutura de hospedagem. Por exemplo, ao criar aplicativos de API da Web, a estrutura de desenvolvedor continuará a usar a estrutura de API Web ASP.NET, independentemente de estarem ou não o aplicativo é executado em um pipeline do OWIN usando componentes do projeto Katana. O único local onde o código relacionado ao OWIN será visível para o desenvolvedor do aplicativo será o código de inicialização do aplicativo, em que o desenvolvedor compõe o pipeline do OWIN. No código de inicialização, o desenvolvedor registrará uma série de instruções de UseXx, geralmente, uma para cada componente de middleware que processará as solicitações de entrada. Essa experiência terá o mesmo efeito que registrando módulos HTTP no mundo atual do System. Web. Normalmente, um middleware framework maior, como API Web ASP.NET ou [SignalR](../../../signalr/index.md) será registrado no final do pipeline. Componentes de middleware abrangentes, como aquelas para autenticação ou armazenamento em cache, geralmente são registrados em direção ao início do pipeline para que eles processará solicitações para todas as estruturas e componentes registrados posteriormente no pipeline. Essa separação de componentes de middleware uns dos outros e os componentes de infraestrutura subjacente permite que os componentes de evoluir em velocidades diferentes, garantindo que o sistema geral permanece estável.

## <a name="components--nuget-packages"></a>Componentes – pacotes NuGet

Como muitas estruturas e bibliotecas atuais, os componentes do projeto Katana são entregues como um conjunto de pacotes do NuGet. Para a próxima versão 2.0, o grafo de dependência de pacote do Katana será semelhante ao seguinte. (Clique na imagem para aumentar a exibição).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Quase todos os pacotes no projeto Katana depende, direta ou indiretamente, o pacote do Owin. Você pode se lembrar de que este é o pacote que contém a interface de IAppBuilder, que fornece uma implementação concreta da sequência de inicialização de aplicativo descrita na seção 4 da especificação do OWIN. Além disso, muitos dos pacotes dependem do Microsoft. owin, que fornece um conjunto de tipos auxiliares para trabalhar com solicitações e respostas HTTP. O restante do pacote pode ser classificado como hospedagem pacotes de infraestrutura (servidores ou hosts) ou middleware. Pacotes e dependências externas ao projeto Katana são exibidas em laranja.

A infraestrutura de hospedagem para Katana 2.0 inclui o SystemWeb e servidores baseados em HttpListener, o pacote OwinHost para execução de aplicativos do OWIN usando OwinHost.exe e pacote Hosting para hospedagem interna de aplicativos OWIN em um host personalizado (por exemplo, aplicativo de console, serviço do Windows, etc.)

Para Katana 2.0, os componentes de middleware estão concentrados principalmente no fornecimento de maneira diferente de autenticação. Um componente de middleware adicionais para diagnóstico for fornecido, que habilita o suporte para uma página de início e de erro. À medida que cresce OWIN para a abstração de hospedagem de fato, o ecossistema de componentes de middleware, tanto aqueles desenvolvidos pela Microsoft e de terceiros, também aumentará em número.

## <a name="conclusion"></a>Conclusão

 Desde seu início, meta do projeto Katana não foi criar e, assim, forçar os desenvolvedores precisam aprender outra estrutura de Web. Em vez disso, a meta foi criar uma abstração para dar mais uma opção que anteriormente foi possível de desenvolvedores de aplicativos Web do .NET. Ao dividir as camadas lógicas de uma pilha de aplicativo Web típica em um conjunto de componentes substituíveis, o projeto Katana permite que componentes em toda a pilha para melhorar o acordo com qualquer taxa faz sentido para esses componentes. Ao compilar todos os componentes em torno de abstração simples do OWIN, o Katana permite que estruturas e os aplicativos criados sobrelos como portáteis entre uma variedade de diferentes servidores e os hosts. Colocando o desenvolvedor no controle da pilha, o Katana garante que o desenvolvedor torna a escolha final sobre como leve ou rico de recursos como sua pilha da Web deve ser.  
  

## <a name="for-more-information-about-katana"></a>Para obter mais informações sobre o Katana

- O projeto Katana no GitHub: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Vídeo: [projeto Katana - OWIN para ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), por Howard Dierking.

## <a name="acknowledgements"></a>Confirmações

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick é um escritor de programação sênior da Microsoft, concentrando-se no Azure e no MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
