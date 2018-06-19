---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Uma visão geral do projeto Katana | Microsoft Docs
author: howarddierking
description: A estrutura do ASP.NET já existe há mais de 10 anos, e a plataforma habilitou o desenvolvimento de inúmeros sites e serviços. Como o aplicativo da Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 3c2bcbbc6e506af759f6d77af17d015278cc0bdf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878757"
---
<a name="an-overview-of-project-katana"></a>Uma visão geral do projeto Katana
====================
por [Howard Dierking](https://github.com/howarddierking)

> A estrutura do ASP.NET já existe há mais de 10 anos, e a plataforma habilitou o desenvolvimento de inúmeros sites e serviços. Como evoluíram estratégias de desenvolvimento de aplicativo Web, a estrutura foi capaz de evoluir na etapa com tecnologias como ASP.NET MVC e a API da Web do ASP.NET. Como o desenvolvimento de aplicativo Web entra em seu próximo passo para o mundo da computação em nuvem, projeto [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) fornece o conjunto subjacente de componentes para aplicativos ASP.NET, permitindo que eles sejam flexível, portátil, leve e oferecer melhor desempenho – em outras palavras, o projeto [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) nuvem otimiza os aplicativos ASP.NET.


## <a name="why-katana--why-now"></a>Por que Katana – por que agora?

 Independentemente se um estiver discutindo um produto de framework ou do usuário final do desenvolvedor, é importante entender as motivações subjacentes para a criação do produto e a parte do que inclui saber quem o produto foi criado para. ASP.NET foi criado originalmente com dois clientes em mente.   
  
**O primeiro grupo de clientes foi desenvolvedores ASP clássicos.** No momento, o ASP foi uma das tecnologias principais para criar dinâmicos, controlados por dados sites e aplicativos Web por interweaving marcação e o script do lado do servidor. O tempo de execução do ASP fornecido pelo script do lado do servidor com um conjunto de objetos que abstraídos aspectos fundamentais do protocolo HTTP subjacente e do servidor Web e desde que tais gerenciamento de estado de sessão e de aplicativo, serviços de acesso a mais de cache, etc. Embora poderoso, aplicativos de ASP clássicos se tornou um desafio para gerenciar, como eles aumentou de tamanho e complexidade. Isso foi amplamente devido à falta de estrutura encontrada em ambientes juntamente com a eliminação de duplicação de código resultante da intercalação de código e a marcação de script. Para aproveitar os pontos fortes do ASP clássico ao abordar alguns de seus desafios, ASP.NET aproveitou da organização código fornecida pelas linguagens orientada a objeto do .NET Framework e também preservar o modelo de programação do lado do servidor para que o ASP clássico, os desenvolvedores cresceu acostumados.

**O segundo grupo de clientes-alvo do ASP.NET foi os desenvolvedores de aplicativos de negócios do Windows.** Ao contrário de desenvolvedores ASP clássicos, o que estavam acostumados a gravação de marcação HTML e o código para gerar mais uma marcação HTML, os desenvolvedores do WinForms (assim como os desenvolvedores de VB6 antes delas) estava acostumado a uma experiência de tempo de design que incluiu uma tela e um conjunto avançado de usuário controles de interface. A primeira versão do ASP.NET – também é conhecido como "Web Forms" fornecida uma experiência semelhante de tempo de design junto com um modelo de evento do lado do servidor para os componentes de interface de usuário e um conjunto de recursos de infraestrutura (por exemplo, o ViewState) para criar uma experiência de desenvolvedor simples entre cliente e programação do lado do servidor. Formulários da Web hid efetivamente a natureza sem monitoração de estado da Web em um modelo de evento com monitoração de estado que estava familiar para desenvolvedores do WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Desafios gerados pelo modelo de histórico

**O resultado líquido foi desenvolvido de recursos para o tempo de execução e modelo de programação do desenvolvedor.** No entanto, com que a riqueza de recurso fornecido alguns desafios importantes. Em primeiro lugar, a estrutura foi **monolítico**, com logicamente diferentes unidades de funcionalidade que está sendo acoplado no mesmo assembly System.Web.dll (por exemplo, os objetos principais HTTP com a estrutura de formulários da Web). Em segundo lugar, o ASP.NET foi incluída como parte do maior do .NET Framework, que significa que o **tempo entre as versões foi na ordem de anos.** Isso tornou difícil para o ASP.NET acompanhar todas as alterações ocorram em rápida evolução de desenvolvimento na Web. Por fim, System.Web.dll próprio foi ligado em algumas maneiras diferentes de uma Web específica, opção de hospedagem: serviços de informações da Internet (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Etapas evolutivas: ASP.NET MVC e a API da Web do ASP.NET

E muitas alterações estava acontecendo no desenvolvimento da Web! Aplicativos da Web estão sendo desenvolvidos cada vez mais como uma série de pequenos, voltada para componentes, em vez de estruturas grandes. O número de componentes, bem como a frequência com que elas foram lançadas estava aumentando a uma taxa mais rápida do que nunca. Ficou claro que manter ritmo com Web exigiria estruturas para obter o menor, separado e mais direcionados em vez de maiores e mais rica em recursos, portanto o **equipe ASP.NET levou várias etapas evolutiva para habilitar o ASP.NET como uma família de componentes da Web conectáveis em vez de uma única estrutura**.

Uma das alterações iniciais era o aumento na popularidade do conhecido model-view-controller (MVC) padrão de design graças estruturas de desenvolvimento de Web como Ruby nos trilhos. Esse estilo de criação de aplicativos Web deu o desenvolvedor maior controle sobre a marcação do seu aplicativo e ainda preservar a separação de marcação e lógica de negócios, que era um dos pontos de venda inicias para o ASP.NET. Para atender à demanda para esse estilo de desenvolvimento de aplicativo Web, Microsoft levou a oportunidade de se posicionar melhor para o futuro por **desenvolvimento ASP.NET MVC fora da banda** (e não incluí-lo no .NET Framework). ASP.NET MVC foi lançado como um download independente. Isso dá a equipe de engenharia a flexibilidade para fornecer atualizações com muito mais frequência do que havia sido possível anteriormente.

Outra grande mudança no desenvolvimento de aplicativo Web foi a mudança de páginas da Web dinâmicas gerados pelo servidor para a marcação inicial estática com dinâmicas seções da página gerados a partir de script do lado do cliente se comunicar **com APIs da Web de back-end por meio de Solicitações do AJAX**. Essa mudança arquitetônica ajudou a impulsionar o surgimento de APIs da Web e o desenvolvimento do framework API Web do ASP.NET. Como no caso do ASP.NET MVC, a versão do ASP.NET Web API fornecida outra oportunidade de evoluir ASP.NET como uma estrutura mais modular. A equipe de engenharia necessário aproveitar a oportunidade e **criado API Web do ASP.NET, de modo que não tinha nenhuma dependência em qualquer um dos tipos de framework core encontrados no System.Web.dll**. Essa opção habilitada duas coisas: primeiro, significa que pode evoluir API da Web ASP.NET de maneira totalmente independente (e ele pode continuar a iterar rapidamente, porque ele é distribuído por meio do NuGet). Segundo, porque não havia sem dependências externas para System.Web.dll e, portanto, nenhuma dependência no IIS, ASP.NET Web API incluída a capacidade de executar em um host personalizado (por exemplo, um aplicativo de console de serviço do Windows, etc.)

### <a name="the-future-a-nimble-framework"></a>O futuro: Uma estrutura ágil

Desacoplar componentes do framework uns dos outros e, em seguida, liberá-los no NuGet, estruturas podem agora **iterar mais rápida e mais independentemente**. Além disso, a potência e flexibilidade de capacidade de hospedagem interna da API da Web provado que ele é muito atraentes para os desenvolvedores que desejavam um **host pequeno e leve** para seus serviços. Isso tornava tão atraente, na verdade, ou outras estruturas também queriam esse recurso e isso apresentados um novo desafio que cada estrutura foi executado em seu próprio processo de host em seu próprio endereço de base e precisa ser gerenciado (início, parada, etc.) independentemente. Um aplicativo da Web moderno geralmente oferece suporte a servidores de arquivo estático, geração de página dinâmica, API da Web e mais recentemente real-tempo/notificações por push. Esperar que cada um desses serviços deve ser executada e gerenciada de forma independente simplesmente não era realista.

O que era necessário foi uma única abstração de hospedagem que permitem que um desenvolvedor para compor um aplicativo de uma variedade de diferentes componentes e estruturas e, em seguida, executar esse aplicativo em um host de suporte.

## <a name="the-open-web-interface-for-net-owin"></a>A Interface Web aberta para .NET (OWIN)

 Inspirado pelos benefícios obtidos por [Rack](http://rack.github.io/) na comunidade de Ruby, vários membros da comunidade do .NET definidos para criar uma abstração entre os servidores Web e os componentes da estrutura. Duas metas de design para a abstração de OWIN foram foi simple e que levou o mínimo possível de dependências em outros tipos de estrutura. Esses dois objetivos garantir:

- Novos componentes poderiam ser mais facilmente desenvolvidos e consumidos.
- Aplicativos podem ser movidos mais facilmente entre hosts e potencialmente toda plataformas/sistemas operacionais.

A abstração resultante consiste em dois elementos principais. A primeira é o dicionário do ambiente. Essa estrutura de dados é responsável por armazenar todos os estados necessários para processar uma solicitação HTTP e resposta, bem como qualquer estado de servidor relevantes. O dicionário do ambiente é definido da seguinte maneira:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Um servidor Web compatível com o OWIN é responsável pela população o dicionário do ambiente com dados, como os fluxos de corpo e as coleções de cabeçalho para uma solicitação e resposta HTTP. Em seguida, é responsabilidade dos componentes de aplicativo ou framework para popular ou atualizar o dicionário com valores adicionais e gravar no fluxo de corpo de resposta.

Além de especificar o tipo para o dicionário do ambiente, a especificação de OWIN define uma lista de pares de chave-valor do dicionário principal. Por exemplo, a tabela a seguir mostra as chaves de dicionário necessários para uma solicitação HTTP:

| Nome da chave | Descrição de valor |
| --- | --- |
| `"owin.RequestBody"` | Um fluxo com o corpo da solicitação, se houver. Stream pode ser usado como um espaço reservado, se não houver nenhum corpo de solicitação. Consulte [corpo da solicitação](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Um `IDictionary<string, string[]>` de cabeçalhos de solicitação. Consulte [cabeçalhos](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Um `string` que contém o método de solicitação HTTP da solicitação (por exemplo, `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Um `string` que contém o caminho da solicitação. O caminho deve ser relativo ao "raiz" do delegado do aplicativo; consulte [caminhos](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Um `string` que contém a parte do caminho da solicitação correspondente para "raiz" do delegado do aplicativo; consulte [caminhos](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Um `string` que contém o nome do protocolo e versão (por exemplo, `"HTTP/1.0"` ou `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Um `string` que contém o componente de cadeia de caracteres de consulta da solicitação HTTP URI, sem o prefixo "?" (por exemplo, `"foo=bar&baz=quux"`). O valor pode ser uma cadeia de caracteres vazia. |
| `"owin.RequestScheme"` | Um `string` que contém o esquema URI usado para a solicitação (por exemplo, `"http"`, `"https"`); consulte [esquema URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

O segundo elemento chave do OWIN é o representante do aplicativo. Isso é uma assinatura de função que serve como a principal interface entre todos os componentes em um aplicativo OWIN. A definição do representante de aplicativo é o seguinte:

`Func<IDictionary<string, object>, Task>;`

O representante do aplicativo, em seguida, é simplesmente uma implementação do tipo Func delegado em que a função aceita o dicionário do ambiente como entrada e retorna uma tarefa. Esse design tem várias implicações para desenvolvedores:

- Há um número muito pequeno de dependências de tipo necessárias para escrever componentes OWIN. Isso aumenta muito a acessibilidade do OWIN para desenvolvedores.
- O design assíncrono permite a abstração a eficiência com a manipulação de recursos de computação, especialmente em operações intensivas de e/s mais.
- Como o representante do aplicativo é uma unidade atômica de execução e porque o dicionário do ambiente é executado como um parâmetro sobre o delegado, componentes OWIN podem ser facilmente encadeados juntos para criar pipelines de processamento de HTTP complexas.

De uma perspectiva de implementação, OWIN é uma especificação ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Sua meta não é deve ser a estrutura da Web Avançar, mas em vez disso, uma especificação de como estruturas da Web e servidores Web interagem.

Se você tiver investigado [OWIN](http://owin.org/) ou [Katana](https://github.com/aspnet/AspNetKatana/wiki), você também deve ter notado o [pacote Owin NuGet](http://nuget.org/packages/Owin) e Owin.dll. Esta biblioteca contém uma única interface, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), que formaliza e codifies a sequência de inicialização descrita em [seção 4](http://owin.org/html/owin.html#4-application-startup) da especificação de OWIN. Embora não seja necessário para criar servidores OWIN a [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interface fornece um ponto de referência concreta, e ele é usado pelos componentes do projeto Katana.

## <a name="project-katana"></a>Projeto Katana

Enquanto tanto o [OWIN](http://owin.org/html/owin.html) especificação e *Owin.dll* são de propriedade de comunidade e comunidade executar esforços de código-fonte aberto, o [Katana](https://github.com/aspnet/AspNetKatana/wiki) projeto representa o conjunto de OWIN componentes que, embora o código-fonte aberto ainda, são criados e lançados pela Microsoft. Esses componentes incluem componentes de infraestrutura, como hosts e servidores, bem como os componentes funcionais, como componentes de autenticação e as associações para estruturas como [SignalR](../../../signalr/index.md) e [da Web do ASP.NET API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). O projeto tem as três seguintes metas de alto níveis: 

- **Portátil** – componentes devem ser capazes de facilmente ser substituídos por novos componentes, assim que estiverem disponíveis. Isso inclui todos os tipos de componentes do framework para o servidor e o host. A implicação essa meta é que estruturas de terceiros perfeitamente poderá executar em servidores da Microsoft, enquanto as estruturas da Microsoft poderão ser executadas em hosts e servidores de terceiros.
- **Modular/flexível**– ao contrário de muitas estruturas que incluem uma grande variedade de recursos que são ativados por padrão, componentes do projeto Katana devem ser pequeno e se concentrem, oferecendo controle o desenvolvedor do aplicativo para determinar quais componentes Use em seu aplicativo.
- **Leve e funcionais/dimensionável** – dividindo a noção tradicional de uma estrutura em um conjunto de pequenos, voltada para componentes que são adicionados explicitamente pelo desenvolvedor do aplicativo, um aplicativo Katana resultante pode consumir menos de computação recursos e como resultado, lidar com mais carga, que com outros tipos de servidores e estruturas. Como os requisitos do aplicativo exigem mais recursos de infraestrutura subjacente, aqueles podem ser adicionados ao pipeline OWIN, mas que deve ser uma decisão explícita por parte do desenvolvedor do aplicativo. Além disso, o substitutability dos componentes de nível inferiores significa que assim que estiverem disponíveis, novos servidores de alto desempenho podem perfeitamente ser introduzidos para melhorar o desempenho de aplicativos OWIN sem interromper os aplicativos.

## <a name="getting-started-with-katana-components"></a>Guia de Introdução com componentes Katana

Quando ele foi introduzido pela primeira vez, um aspecto de [Node. js](http://nodejs.org/) framework que imediatamente desenhou atenção estava a simplicidade com o qual um pode criar e executar um servidor Web. Se Katana metas foram enquadradas light de [Node. js](http://nodejs.org/), um pode resumi-los dizendo que Katana traz muitos dos benefícios de [Node. js](http://nodejs.org/) (e estruturas, como ele) sem forçar o desenvolvedor lançar tudo o que ela sabe sobre o desenvolvimento de aplicativos Web ASP.NET. Para esta instrução verdadeiras, Introdução ao projeto Katana deve ser igualmente simple na natureza [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Criando "Hello World!"

Uma diferença notável entre JavaScript e .NET development é a presença (ou ausência) de um compilador. Como tal, o ponto de partida para um servidor Katana simples é um projeto do Visual Studio. No entanto, podemos começar com o mínimo de tipos de projetos: o aplicativo Web ASP.NET vazio.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Em seguida, vamos instalar o [systemweb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) pacote NuGet para o projeto. Esse pacote fornece um servidor OWIN que é executado no pipeline de solicitação do ASP.NET. Ele pode ser encontrado na [Galeria NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) e pode ser instalado usando a caixa de diálogo do Gerenciador de pacote do Visual Studio ou o console do Gerenciador de pacote com o seguinte comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Instalando o `Microsoft.Owin.Host.SystemWeb` pacote irá instalar alguns pacotes adicionais como dependências. Uma dessas dependências é `Microsoft.Owin`, uma biblioteca que fornece vários tipos de auxiliar e métodos para o desenvolvimento de aplicativos OWIN. Podemos usar esses tipos para escrever rapidamente o seguinte servidor de "Olá, mundo".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Esse servidor Web muito simple pode agora ser executada usando o Visual Studio **F5** de comando e inclui suporte completo para depuração.

## <a name="switching-hosts"></a>Alternância de hosts

Por padrão, o exemplo anterior de "hello world" é executado no pipeline de solicitação do ASP.NET, que usa o System. Web no contexto do IIS. Isso pode por si só agregar valor conforme ele nos permite aproveitar a flexibilidade e capacidade de combinação de um pipeline OWIN com os recursos de gerenciamento e maturidade geral do IIS. No entanto, pode haver casos em que os benefícios fornecidos pelo IIS não são necessários e o desejo é para um host menor e mais leve. O que é necessário, em seguida, executar nosso servidor da Web simple fora do IIS e System. Web?

Para ilustrar a meta de portabilidade, a movimentação de um host de servidor Web para um host de linha de comando requer simplesmente adicionando novas dependências do host e o servidor para a pasta de saída do projeto e, em seguida, iniciar o host. Neste exemplo, podemos vai hospedar nosso servidor Web em um host Katana chamado `OwinHost.exe` e usará o servidor com base em Katana HttpListener. Da mesma forma para os outros componentes Katana, eles serão adquiridos do NuGet usando o seguinte comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Na linha de comando, pode, em seguida, navegue até a pasta raiz do projeto e simplesmente executar o `OwinHost.exe` (que foi instalado na pasta Ferramentas de seu respectivo pacote de NuGet). Por padrão, `OwinHost.exe` está configurado para procurar o servidor baseado em HttpListener e, portanto, nenhuma configuração adicional é necessária. Navegando em um navegador da Web para `http://localhost:5000/` mostra o aplicativo em execução por meio do console.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Arquitetura de Katana

 A arquitetura de componente Katana divide um aplicativo em quatro camadas lógicas, conforme descrito abaixo: *host, o servidor, o middleware,* e *aplicativo*. A arquitetura de componente é acrescentada de forma que implementações dessas camadas podem ser facilmente substituídas, em muitos casos, sem a necessidade de recompilação do aplicativo.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 O host é responsável por:

- Gerenciar o processo subjacente.
- Organizar o fluxo de trabalho que resulta na seleção de um servidor e a construção de um pipeline OWIN por meio de quais solicitações é manipulada.

  No momento, há 3 opções de hospedagem primárias para aplicativos baseados em Katana:  
  
**IIS/ASP.NET**: usando os tipos padrão HttpModule e HttpHandler, OWIN pipelines podem executados no IIS como parte de um fluxo de solicitação do ASP.NET. Suporte à hospedagem de ASP.NET é habilitado por instalar o pacote NuGet de Microsoft.AspNet.Host.SystemWeb em um projeto de aplicativo Web. Além disso, como IIS atua como um host e um servidor, a diferença de host do servidor OWIN é confundida neste pacote NuGet, que significa que, se usar o host SystemWeb, um desenvolvedor não pode substituir uma implementação de servidor alternativo.  
  
**Host personalizado**: Katana o conjunto de componentes permite que um desenvolvedor para hospedar aplicativos no próprio processo personalizado, se é um aplicativo de console, o serviço do Windows, etc. Essa funcionalidade é semelhante ao recurso de auto-host fornecido pela API da Web. O exemplo a seguir mostra um host personalizado de código da API da Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

A configuração do próprio host de um aplicativo Katana é semelhante:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Uma diferença notável entre as API da Web e Katana exemplos de hospedagem interna é que o código de configuração de API da Web está ausente do exemplo Katana próprio host. Para habilitar a portabilidade e capacidade de composição, Katana separa o código que inicia o servidor do código que configura o pipeline de processamento da solicitação. O código que configura a API da Web, em seguida, está contido na classe de inicialização, que Além disso é especificada como o parâmetro de tipo no WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

A classe de inicialização será discutida em maiores detalhes posteriormente neste artigo. No entanto, o código necessário para iniciar um Katana auto-host processo parece bastante semelhante ao código que você pode estar usando atualmente em aplicativos de hospedagem interna de API da Web ASP.NET.

**OwinHost.exe**: enquanto alguns desejar gravar um processo personalizado para executar aplicativos da Web de Katana, muitos prefere simplesmente iniciar um executável predefinido que pode iniciar um servidor e executar seu aplicativo. Para este cenário, o conjunto de componentes Katana inclui `OwinHost.exe`. Quando executado no diretório raiz do projeto, este executável iniciarão um servidor (usa o servidor HttpListener por padrão) e usar as convenções para localizar e executar a classe de inicialização do usuário. Para um controle mais granular, o executável fornece um número de parâmetros de linha de comando adicionais.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Servidor

 Enquanto o host é responsável por iniciar e manter o processo no qual o aplicativo é executado, a responsabilidade do servidor é abrir um soquete de rede, escutar solicitações e enviá-los por meio do pipeline de componentes OWIN especificado pelo usuário (como você já deve ter notado, este pipeline é especificado na classe de inicialização do desenvolvedor do aplicativo). No momento, o projeto Katana inclui duas implementações de servidor: 

- **Systemweb**: como mencionado anteriormente, o IIS junto com o pipeline do ASP.NET atua como um host e um servidor. Portanto, ao escolher essa opção de hospedagem, IIS gerencia preocupações de nível de host, como a ativação de processos tanto de escuta solicitações HTTP. Para aplicativos Web ASP.NET, envia as solicitações no pipeline do ASP.NET. O host Katana SystemWeb registra um HttpModule ASP.NET e HttpHandler para interceptar solicitações forem fluem por meio do pipeline HTTP e enviá-los por meio do pipeline OWIN especificado pelo usuário.
- **HttpListener**: como seu nome indica, este servidor Katana usa a classe do .NET Framework HttpListener para abrir um soquete e enviar solicitações em um pipeline OWIN especificado pelo desenvolvedor. Este é a seleção de servidor padrão para a API de hospedagem interna de Katana e OwinHost.exe.

## <a name="middlewareframework"></a>Middleware/framework

 Como mencionado anteriormente, quando o servidor aceita uma solicitação de um cliente é responsável por passá-lo por meio de um pipeline de componentes OWIN, que são especificadas pelo código de inicialização do desenvolvedor. Esses componentes do pipeline são conhecidos como middleware.  
 Em um nível muito básico, um componente de middleware OWIN simplesmente precisa implementar o representante do aplicativo OWIN para que ele possa ser chamado.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

No entanto, para simplificar o desenvolvimento e a composição de componentes de middleware, Katana dá suporte a uma série de tipos de auxiliar e convenções para componentes de middleware. O mais comum deles é o `OwinMiddleware` classe. Um componente de middleware personalizados criado usando essa classe seria semelhante à seguinte: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Essa classe é derivada de `OwinMiddleware`, implementa um construtor que aceita uma instância do próximo middleware no pipeline, como um de seus argumentos e, em seguida, passa para o construtor base. Argumentos adicionais usados para configurar o middleware também são declarados como parâmetros do construtor após o próximo parâmetro de middleware.   
  
Em tempo de execução, o middleware é executado por meio de substituído `Invoke` método. Esse método aceita um único argumento do tipo `OwinContext`. Esse objeto de contexto é fornecido pelo `Microsoft.Owin` pacote NuGet descrito anteriormente e fornece acesso fortemente tipado ao dicionário de solicitação e resposta de ambiente, juntamente com alguns tipos auxiliares.   
  
A classe de middleware pode ser facilmente adicionada para o pipeline OWIN no código de inicialização do aplicativo da seguinte maneira:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Como a infraestrutura Katana simplesmente cria um pipeline de componentes de middleware OWIN, e os componentes simplesmente precisam oferecer suporte o representante do aplicativo para participar do pipeline, componentes de middleware podem variar em complexidade de simples agentes de log para todas estruturas, como ASP.NET, API da Web, ou [SignalR](../../../signalr/index.md). Por exemplo, a adição de API da Web do ASP.NET para o pipeline OWIN anterior requer adicionando o seguinte código de inicialização:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

A infraestrutura Katana criará o pipeline de componentes de middleware com base na ordem em que foram adicionados ao objeto IAppBuilder no método de configuração. Em nosso exemplo, em seguida, LoggerMiddleware pode lidar com todas as solicitações que fluem por meio do pipeline, independentemente de como essas solicitações, por fim, são tratadas. Isso permite cenários avançados em que um componente de middleware (por exemplo, um componente de autenticação) pode processar solicitações de um pipeline que inclui vários componentes e estruturas (por exemplo, API da Web do ASP.NET SignalR e um servidor de arquivos estáticos).
 
## <a name="applications"></a>Aplicativos

Como ilustrado nos exemplos anteriores, OWIN e o projeto Katana devem não ser considerados como um novo modelo de programação de aplicativo, mas como uma abstração para desassociar os modelos de programação de aplicativo e estruturas de servidor e infraestrutura de hospedagem. Por exemplo, ao criar aplicativos de API da Web, o framework developer continuará usar a estrutura do ASP.NET Web API, independentemente de estarem ou não o aplicativo é executado em um pipeline OWIN usando componentes de projeto Katana. O único local onde o código relacionado ao OWIN será visível para o desenvolvedor do aplicativo será o código de inicialização do aplicativo, em que o desenvolvedor compõe o pipeline OWIN. No código de inicialização, o desenvolvedor registrará uma série de instruções de UseXx, geralmente uma para cada componente de middleware que processará as solicitações de entrada. Essa experiência terá o mesmo efeito que o registro de módulos HTTP no mundo atual de System. Web. Normalmente, um middleware estrutura maior, como a API da Web ASP.NET ou [SignalR](../../../signalr/index.md) será registrado no final do pipeline. Componentes de middleware abrangentes, como aquelas para autenticação ou armazenar em cache, geralmente são registrados em direção ao início do pipeline para que ele processe as solicitações para todas as estruturas e os componentes registrados mais tarde no pipeline. Essa separação dos componentes de middleware umas das outras e os componentes da infraestrutura subjacente permite que os componentes desenvolver em diferentes velocidades enquanto garante que todo o sistema permaneça estável.

## <a name="components--nuget-packages"></a>Componentes – pacotes do NuGet

Como muitas estruturas e bibliotecas atuais, os componentes do projeto Katana são entregues como um conjunto de pacotes do NuGet. Para a próxima versão 2.0, o gráfico de dependência de pacote Katana procura. (Clique na imagem de exibição maior).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Quase todos os pacotes no projeto Katana depende, direta ou indiretamente, o pacote de Owin. Você deve se lembrar de que este é o pacote que contém a interface IAppBuilder, que fornece uma implementação concreta da sequência de inicialização de aplicativo descrita na seção 4 da especificação de OWIN. Além disso, muitos dos pacotes dependem pt, que fornece um conjunto de tipos de auxiliar para trabalhar com solicitações e respostas HTTP. O restante do pacote pode ser classificado como hospedagem pacotes de infraestrutura (servidores ou hosts) ou middleware. Pacotes e dependências externas ao projeto Katana são exibidas em laranja.

A infraestrutura de hospedagem para Katana 2.0 inclui o SystemWeb e servidores baseados em HttpListener, o pacote OwinHost para executar aplicativos OWIN usando OwinHost.exe e o pacote Hosting para auto-hospedagem aplicativos OWIN em um host personalizado (por exemplo, aplicativo de console, serviço do Windows, etc.)

Para Katana 2.0, os componentes de middleware principalmente são se concentra em fornecer meios diferentes de autenticação. Um componente de middleware adicionais de diagnóstico for fornecido, o que habilita o suporte para uma página de início e de erro. À medida que cresce OWIN para a abstração de hospedagem de fato, o ecossistema de componentes de middleware, tanto os desenvolvido pela Microsoft e de terceiros, também aumentam em número.

## <a name="conclusion"></a>Conclusão

 Desde o começo meta do projeto Katana não foi criar e, portanto, forçar os desenvolvedores a aprender ainda outra estrutura da Web. Em vez disso, a meta foi criar uma abstração para fornecer os desenvolvedores de aplicativos Web .NET mais opções que foi anteriormente possíveis. Dividindo as camadas lógicas de uma pilha de aplicativo Web típica em um conjunto de componentes, o projeto Katana permite componentes em toda a pilha para melhorar a taxa de qualquer faz sentido para os componentes. Criando todos os componentes de uma solução alternativa para a abstração de OWIN simple, Katana habilita estruturas e os aplicativos criados sobrelos para ser portátil em uma variedade de hosts e servidores diferentes. Colocando o desenvolvedor do controle da pilha, Katana garante que o desenvolvedor torna a escolha final sobre como simples ou como recursos sua pilha da Web deve ser.  
  

## <a name="for-more-information-about-katana"></a>Para obter mais informações sobre Katana

- O projeto Katana no GitHub: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Vídeo: [projeto Katana - OWIN para o ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), por Howard Dierking.

## <a name="acknowledgements"></a>Confirmações

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick é escritor técnico de programação para a Microsoft se concentra no Azure e MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
