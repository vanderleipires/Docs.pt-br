---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: "Como: Adicionar páginas móveis aos formulários da Web do ASP.NET / aplicativo MVC | Microsoft Docs"
author: rick-anderson
description: "Esse manual descreve várias maneiras de fornecer páginas otimizadas para dispositivos móveis de sua Web Forms do ASP.NET / aplicativo MVC e sugere arquitetura e..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: c7d893fb9633aaa8628f2f46a8db7f2c09f81830
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Como: Adicionar páginas móveis aos formulários da Web do ASP.NET / aplicativo MVC
====================
> **Aplica-se a**
> 
> - Versão de Web Forms do ASP.NET 4.0
> - ASP.NET MVC versão 3.0
> 
> **Resumo**
> 
> Esse manual descreve várias maneiras de fornecer páginas otimizadas para dispositivos móveis de sua Web Forms do ASP.NET / aplicativo MVC e sugere arquitetura e design questões a considerar ao direcionar uma ampla variedade de dispositivos. Este documento também explica por que os controles do ASP.NET móvel do ASP.NET 2.0 3.5 agora estão obsoletos e discute algumas alternativas modernas.


## <a name="contents"></a>Conteúdo

- Visão Geral
- Opções de arquitetura
- Detecção de navegadores e dispositivos
- Como os aplicativos ASP.NET Web Forms podem apresentar páginas específicas para dispositivos móveis
- Como os aplicativos ASP.NET MVC podem apresentar páginas específicas para dispositivos móveis
- Recursos adicionais

Para obter exemplos de código para download demonstram técnicas deste white paper Web Forms do ASP.NET e MVC, consulte [Sites com o ASP.NET & aplicativos móveis](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Visão Geral

Dispositivos móveis – smartphones, telefones de recurso e tablets – continuam crescendo em popularidade como um meio para acessar a Web. Para muitas empresas para a web e os desenvolvedores da web, isso significa que é cada vez mais importante fornecer uma ótima experiência de navegação para os visitantes que usam esses dispositivos.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Como as versões do ASP.NET suporte para navegadores de dispositivos móveis

Versões do ASP.NET 2.0 para 3.5 incluídos *os controles ASP.NET*: um conjunto de controles de servidor para dispositivos móveis no *System.Web.Mobile.dll* assembly e o  *MobileControls* namespace. O assembly ainda está incluído no ASP.NET 4, mas ela foi preterida. Os desenvolvedores são aconselhados a migrar para abordagens mais modernas, como as descritas neste documento.

O motivo por que os controles ASP.NET foram marcados como obsoletos é que seu design é voltado para os telefones celulares que eram comuns em torno de 2005 e versões anteriores. Os controles são projetados principalmente para renderizar marcação WML ou cHTML (em vez de HTML regular) para os navegadores WAP do que era. Mas, WAP e WML cHTML não são mais relevantes para a maioria dos projetos, porque o HTML tornou a linguagem de marcação onipresente para navegadores de desktop e móveis da mesma forma.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Os desafios de oferecer suporte a dispositivos móveis hoje

Embora navegadores móveis agora quase universalmente oferecem suporte a HTML, você ainda enfrentará muitos desafios quando o objetivo de criar grandes experiências de navegação móveis:

- ***Tamanho da tela*** - dispositivos móveis variam muito no formulário e suas telas são geralmente muito menor do que os monitores de área de trabalho. Portanto, talvez seja necessário criar layouts de página completamente diferente para eles.
- ***Métodos de entrada*** – alguns dispositivos têm teclados numéricos, algumas têm styluses, outros usam toque. Talvez seja necessário considerar navegação com vários métodos de entrada de mecanismos e os dados.
- ***Conformidade com padrões*** – muitos navegadores móveis não oferecem suporte para os padrões mais recentes de HTML, CSS ou JavaScript.
- ***Largura de banda*** – desempenho de rede de dados de celular varia muito, e alguns usuários finais estiver em tarifas cobram por megabytes.

Não há nenhuma solução padronizada; seu aplicativo terá a aparência e se comportam diferentemente, de acordo com o dispositivo acessá-lo. Dependendo de qual nível de suporte móvel que você deseja, isso pode ser um desafio maior para desenvolvedores da web de área de trabalho "conflitos de navegador" nunca foi.

Os desenvolvedores se aproximando de suporte do navegador móvel pela primeira vez geralmente inicialmente que só é importante dar suporte a smartphones mais sofisticados e mais recentes (por exemplo, Windows Phone 7, iPhone ou Android), talvez porque os desenvolvedores geralmente pessoal possuem como dispositivos. No entanto, telefones mais baratos ainda são extremamente populares e seus proprietários usá-los para navegar na web – especialmente em países em que os telefones celulares são mais fácil de obter que uma conexão de banda larga. Sua empresa precisa decidir quais variedade de dispositivos para dar suporte ao considerar a seus clientes provavelmente. Se você estiver criando uma publicação online para um spa de integridade de luxo, pode tomar uma decisão de negócios apenas para smartphones avançados, de destino enquanto se você estiver criando um sistema de reserva do tíquete para um cinema, você provavelmente precisa levar em conta os visitantes com recurso menos avançado telefones.

## <a name="architectural-options"></a>Opções de arquitetura

Antes de entrarmos em detalhes técnicos específicos do Web Forms do ASP.NET ou MVC, observe que os desenvolvedores da web em geral tem três opções possíveis principais para dar suporte a navegadores móveis:

1. ***Não fazer nada –*** você pode simplesmente criar um aplicativo web padrão e orientada a área de trabalho e se baseiam em navegadores de dispositivos móveis para renderizá-lo de forma aceitável. 

    - **Vantagem**: é a opção mais barato de implementar e manter – extra não funcionam
    - **Desvantagem**: fornece a experiência do usuário final pior: 

        - Os smartphones mais recente pode renderizar HTML, bem como um navegador de área de trabalho, mas os usuários ainda serão forçados zoom e rolar horizontalmente e verticalmente para consumir o conteúdo em uma tela pequena. Isso é bem inferior a ideal.
        - Telefones de recurso e dispositivos mais antigos podem falhar ao renderizar a marcação de forma satisfatória.
        - Mesmo em dispositivos tablet mais recentes (cujas telas podem ser tão grandes como telas de laptop), interação diferentes regras se aplicam. Toque de entrada funciona melhor com os botões maiores e vincula a visualização mais separa e não é possível passar o cursor de um cursor do mouse sobre um menu suspenso.
2. ***Resolver o problema no cliente* –** com uso cuidadoso de CSS e [aprimoramento progressivo](http://en.wikipedia.org/wiki/Progressive_enhancement) você pode criar marcação, estilos e scripts que se adaptar a qualquer navegador está executando. Por exemplo, com [as consultas de mídia CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), você pode criar um layout de várias coluna que se transforma em um layout de coluna única em dispositivos cujas telas estão mais estreitas do que um limite determinado. 

    - **Vantagem**: otimiza o processamento para o dispositivo específico em uso, mesmo para os dispositivos futuras desconhecidos acordo com qualquer tela e características de entrada tiverem
    - **Vantagem**: facilmente permite que você compartilhe lógica do lado do servidor em todos os tipos de dispositivo – eliminação de duplicação mínimo de esforço ou de código
    - **Desvantagem**: dispositivos móveis são tão diferentes de dispositivos de área de trabalho que pode realmente deseja que suas páginas móveis seja completamente diferente de suas páginas da área de trabalho, mostrando informações diferentes. Essas variações podem ser ineficientes ou impossível robustez por meio de CSS sozinho, especialmente considerando como inconsistente dispositivos mais antigos interpretam as regras de CSS. Isso é particularmente verdadeiro de atributos de CSS 3.
    - **Desvantagem**: não fornece suporte para fluxos de trabalho para diferentes dispositivos e lógica de servidor diferentes. Você não pode, por exemplo, implementar um simplificado compras carrinho check-out fluxo de trabalho para usuários móveis por meio de CSS sozinho.
    - **Desvantagem**: uso de largura de banda ineficiente. Servidor você pode ter que transmitir a marcação e estilos que se aplicam a todos os dispositivos possíveis, mesmo que o dispositivo de destino usará apenas um subconjunto dessas informações.
3. ***Resolver o problema no servidor* –** se o servidor sabe qual dispositivo está acessando – ou pelo menos as características do dispositivo, tais como o tamanho da tela e o método de entrada, e se ele é um dispositivo móvel – pode executar uma lógica diferente e marcação HTML diferente de saída. 

    - **Vantagem:** flexibilidade máxima. Não há nenhum limite quanto você pode variar a lógica do lado do servidor para celulares ou otimizar sua marcação para o layout desejado, específico do dispositivo.
    - **Vantagem:** uso eficiente da largura de banda. Você só precisar transmitir a marcação e informações de estilo que irá usar o dispositivo de destino.
    - **A desvantagem:** às vezes força a repetição de esforço ou de código (por exemplo, fazer com que você crie cópias semelhantes, mas um pouco diferentes de suas páginas de Web Forms ou exibições do MVC). Onde possível que você serão fatorados lógica comuns em uma camada subjacente ou serviço, mas ainda assim, algumas partes do seu código de interface do usuário ou a marcação talvez precise ser duplicado e, em seguida, são mantidas em paralelo.
    - **A desvantagem:** detecção do dispositivo não é algo trivial. Ele requer uma lista ou banco de dados de tipos de dispositivo conhecido e suas características (que podem não ser perfeitamente atualizadas) e não é garantido para corresponder com precisão de cada solicitação de entrada. Este documento descreve algumas opções e suas armadilhas mais tarde.

Para obter os melhores resultados, a maioria dos desenvolvedores localizar que precisam para combinar opções (2) e (3). Pequenas diferenças estilos são melhor acomodadas no cliente usando CSS ou JavaScript mesmo, enquanto as principais diferenças de dados, fluxo de trabalho ou marcação com mais eficácia são implementadas no código do lado do servidor.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Este documento se concentra em técnicas do lado do servidor

Como Web Forms do ASP.NET e MVC são ambas as tecnologias principalmente no lado do servidor, este white paper se concentrará em técnicas do lado do servidor que permitem que você gerar marcação diferente e lógica para navegadores de dispositivos móveis. Naturalmente, você também pode combinar isso com qualquer técnica do lado do cliente (por exemplo, CSS 3 consultas de mídia, aprimoramento progressivo JavaScript), mas é mais uma questão de design da web de desenvolvimento do ASP.NET.

## <a name="browser-and-device-detection"></a>Detecção de navegadores e dispositivos

O pré-requisito de chave para todas as técnicas do lado do servidor para dar suporte a dispositivos móveis é saber qual dispositivo está usando o visitante. Na verdade, ainda melhor do que saber o número de modelo e fabricante do dispositivo é saber a *características* do dispositivo. Características podem incluir:

- É um dispositivo móvel?
- Método de entrada (teclado/mouse, toque, teclado, joystick,...)
- Tamanho da tela (fisicamente e em pixels)
- Formatos de mídia e dados com suporte
- etc.

É melhor tomar decisões com base em características que o número de modelo, porque, em seguida, será melhor equipados para lidar com dispositivos futuras.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Usando ASP. Suporte de detecção do navegador interno do NET

Os desenvolvedores do ASP.NET Web Forms e MVC imediatamente podem descobrir características importantes de um navegador visitando inspecionando propriedades do *Request. browser* objeto. Por exemplo, consulte

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ... e muitos outros

Nos bastidores, a plataforma ASP.NET corresponde a entrada *User-Agent* cabeçalho HTTP (UA) em expressões regulares em um conjunto de arquivos XML de definição do navegador. Por padrão, a plataforma inclui definições para muitos dispositivos móveis comuns, e você pode adicionar arquivos de definição de navegador personalizados para outras pessoas que você deseja reconhecer. Para obter mais detalhes, consulte a página do MSDN [controles de servidor Web ASP.NET e recursos do navegador](https://msdn.microsoft.com/en-us/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Usando o banco de dados do dispositivo WURFL via 51Degrees.mobi Foundation

Enquanto o ASP. Suporte de detecção do navegador interno do NET será suficiente para muitos aplicativos, há dois principais casos em que talvez não seja suficiente:

- ***Você deseja reconhecer os dispositivos mais recentes***(sem criar manualmente os arquivos de definição de navegador para eles). Observe que os arquivos de definição de navegador do .NET 4 não são recentes o suficiente para reconhecer Apple iPads, telefones Android, navegadores Opera Mobile ou Windows Phone 7.
- ***Você precisa obter informações mais detalhadas sobre os recursos do dispositivo***. Você precisará saber sobre o método de entrada do dispositivo (por exemplo, toque vs teclado) ou o áudio formata o navegador oferece suporte. Essas informações não estão disponíveis nos arquivos de definição de navegador padrão.

O [ *arquivo de recurso Universal sem fio* projeto (WURFL)](http://wurfl.sourceforge.net/) mantém muito mais informações atualizadas e detalhadas sobre dispositivos móveis em uso hoje.

A boa notícia para desenvolvedores .NET é que o ASP. Recurso de detecção do navegador do NET é extensível, portanto, é possível aprimorá-lo para solucionar esses problemas. Por exemplo, você pode adicionar código-fonte aberto [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) biblioteca ao seu projeto. É um IHttpModule do ASP.NET ou um navegador recursos de provedor (pode ser usado em aplicativos Web Forms e MVC), que lê dados WURFL diretamente e conecta à ASP. Mecanismo de detecção do navegador interno do NET. Depois de instalar o módulo *Request. browser* repentinamente conterá informações detalhadas e muito mais precisas: ele corretamente reconhecerá muitos dispositivos mencionados anteriormente e lista seus recursos (incluindo recursos adicionais, como o método de entrada). Consulte a documentação do projeto para obter mais detalhes.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Como os aplicativos Web Forms podem apresentar páginas específicas para dispositivos móveis

Por padrão, aqui está como um novo aplicativo de Web Forms são exibidos em dispositivos móveis comuns:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Obviamente, nem layout parece muito amigáveis para dispositivos móveis – esta página foi projetada para um monitor grande, com orientação paisagem, não para uma tela pequena orientadas por retrato. Então o que você pode fazer sobre ele?

Como discutido neste documento, há muitas maneiras de personalizar as páginas para dispositivos móveis. Algumas técnicas são baseados em servidor, outras pessoas executam no cliente.

### <a name="creating-a-mobile-specific-master-page"></a>Criando uma página mestra específicas para dispositivos móveis

Dependendo dos requisitos, você poderá usar os mesmos formulários da Web para todos os visitantes, mas tem duas páginas mestras separadas: uma para os visitantes de área de trabalho, outro para os visitantes móveis. Isso oferece a flexibilidade de alterar sua marcação HTML nível superior de acordo com os dispositivos móveis, sem forçar duplicar qualquer lógica de página ou de folha de estilos CSS.

Isso é fácil. Por exemplo, você pode adicionar um manipulador de PreInit como o seguinte para um formulário da Web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Agora, crie uma página mestra chamada Mobile.Master na pasta de nível superior do seu aplicativo, e ele será usado quando um dispositivo móvel é detectado. A página principal móvel pode fazer referência a uma folha de estilos CSS específicas para dispositivos móveis se necessário. Os visitantes de área de trabalho ainda verá a página mestra padrão, não aquele móvel.

### <a name="creating-independent-mobile-specific-web-forms"></a>Criar formulários de Web independentes específicas para dispositivos móveis

Para obter a máxima flexibilidade, você pode ir muito mais do que apenas páginas mestras separadas para tipos diferentes de dispositivos. Você pode implementar dois *totalmente separar conjuntos de páginas Web Forms* – um conjunto para navegadores de desktop, outro conjunto de móveis. Isso funciona melhor se você quiser apresentar informações muito diferentes ou fluxos de trabalho para visitantes móveis. O restante desta seção descreve essa abordagem em detalhes.

Supondo que você já tiver um aplicativo Web Forms projetado para navegadores de desktop, a maneira mais fácil para prosseguir é criar uma subpasta chamada "Móvel" dentro de seu projeto e criar suas páginas de celular. Você pode construir um subsite inteiro, com suas próprias páginas mestras, folhas de estilo e páginas, usando as mesmas técnicas que você usaria para qualquer outro aplicativo de Web Forms. Não é necessário produzir um equivalente móvel para *cada* página em seu site de área de trabalho; você pode escolher o subconjunto da funcionalidade faz sentido para os visitantes móveis.

Páginas de celular podem compartilhar recursos estáticos comuns (como imagens, JavaScript ou CSS arquivos) com suas páginas regulares se desejar. Desde que a pasta "Móvel" será *não* ser marcado como um aplicativo separado quando hospedados no IIS (é apenas uma subpasta simple de páginas Web Forms), ele também compartilharão a mesma configuração, dados de sessão e outra infraestrutura como seu páginas da área de trabalho.

> [!NOTE]
> Como essa abordagem geralmente envolve uma duplicação de código (páginas móveis são provavelmente compartilham algumas semelhanças com páginas da área de trabalho), é importante para criar qualquer comuns business dados ou lógica de código de acesso em uma camada subjacente compartilhada ou um serviço. Caso contrário, você vai dobrar o esforço de criação e manutenção de seu aplicativo.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Redirecionando visitantes móveis às páginas móveis

É geralmente conveniente redirecionar visitantes móveis para as páginas móveis apenas o *primeiro* na sua sessão de navegação (e não em cada solicitação em sua sessão), a solicitação porque:

- Em seguida, você pode facilmente permitir visitantes móveis acessar as páginas da área de trabalho, se desejarem – colocar apenas um link em sua página mestra que vão para a "Versão da área de trabalho". O visitante não ser redirecionado para uma página móvel, porque ele não é mais a primeira solicitação na sua sessão.
- Isso evita o risco de interferir com solicitações de todos os recursos dinâmicos compartilhados entre partes móveis e desktop do seu site (por exemplo, se você tiver um formulário da Web comuns que exibem partes móveis e desktop de seu site em um IFRAME ou certos manipuladores Ajax)

Para fazer isso, você pode colocar sua lógica de redirecionamento em um **sessão\_iniciar** método. Por exemplo, adicione o seguinte método ao arquivo asax:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurando a autenticação de formulários para respeitar as páginas móveis

Observe que a autenticação de formulários faz algumas suposições sobre onde ele poderá redirecionar os visitantes durante e após o processo de autenticação:

- Quando um usuário precisa ser autenticada, autenticação de formulários redirecionará-los para a página de logon de área de trabalho, independentemente de se um usuário de desktop ou mobile (porque somente tem um conceito de *um* URL de logon). Supondo que você deseja definir o estilo de sua página de logon móvel diferente, você precisa melhorar sua página de logon de área de trabalho para que ele redireciona os usuários móveis para uma página de logon móvel separado. Por exemplo, adicione o seguinte código ao seu **desktop** lógica para página de logon: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Depois que um usuário fizer logon com êxito, autenticação de formulários por padrão redirecionará-los para a home page da área de trabalho (porque somente tem um conceito de *um* página padrão). Você precisa aumentar sua página de logon móveis para que ele redireciona para a home page móvel após um log bem-sucedido. Por exemplo, adicione o seguinte código ao seu **móvel** lógica para página de logon: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
 Esse código supõe que a página tem um controle de servidor de logon chamado LoginUser, como o modelo de projeto padrão.

### <a name="working-with-output-caching"></a>Trabalhando com o cache de saída

Se você estiver usando o cache de saída, lembre-se de que por padrão é possível que um usuário visitar um determinado URL (fazendo com que a saída para ser armazenado em cache), seguido por um usuário móvel que, em seguida, recebe a saída de área de trabalho do cache. Este aviso se aplica se você estiver apenas variando a página mestra por tipo de dispositivo ou implementando totalmente separadas Web Forms por tipo de dispositivo.

Para evitar o problema, você pode instruir o ASP.NET para variar a entrada de cache de acordo com se o visitante está usando um dispositivo móvel. Adicione um parâmetro de VaryByCustom a declaração de OutputCache da página da seguinte maneira:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Em seguida, defina *isMobileDevice* como um cache personalizado parâmetro adicionando o seguinte método substituir em seu arquivo asax:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Isso garantirá que visitantes móveis não recebem saída anteriormente colocada em cache por um visitante de área de trabalho.

### <a name="a-working-example"></a>Um exemplo de trabalho

Para ver essas técnicas em ação, baixe [exemplos de código deste white paper](https://docs.microsoft.com/aspnet/mobile/overview). O aplicativo de exemplo de formulários da Web automaticamente redireciona os usuários móveis para um conjunto de páginas específicas para dispositivos móveis em uma subpasta chamada móvel. A marcação e o estilo dessas páginas é otimizada para navegadores de dispositivos móveis, como você pode ver as capturas de tela a seguir:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Para obter mais dicas sobre como otimizar sua marcação e CSS para navegadores de dispositivos móveis, consulte a seção "Estilo móveis páginas para navegadores de dispositivos móveis" posteriormente neste documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Como os aplicativos ASP.NET MVC podem apresentar páginas específicas para dispositivos móveis

Como o padrão Model-View-Controller separa a lógica do aplicativo (nos controladores) da lógica de apresentação (nos modos de exibição), você pode escolher de qualquer uma das seguintes abordagens para lidar com suporte móvel no código do lado do servidor:

1. ***Usar o mesmo controladores e exibições para navegadores de desktop e móveis, mas renderizar exibições com diferentes layouts de Razor, dependendo do tipo de dispositivo*.** Essa opção funciona melhor se você estiver exibindo dados idênticos em todos os dispositivos, mas simplesmente deseja fornecer diferentes folhas de estilo CSS ou alterar alguns elementos de HTML de nível superior para celulares.
2. ***Use os mesmos controladores para navegadores de desktop e móveis, mas renderizar exibições diferentes dependendo do tipo de dispositivo***. Essa opção funciona melhor se você estiver exibindo aproximadamente os mesmos dados e fornecer os mesmos fluxos de trabalho para os usuários finais, mas deseja processar uma marcação HTML muito diferente de acordo com o dispositivo que está sendo usado.
3. ***Criar áreas separadas para navegadores de desktop e mobile, Implementando controladores independentes e modos de exibição para cada*.** Essa opção funciona melhor se você estiver exibindo as telas muito diferentes, que contém informações diferentes e à esquerda do usuário por meio de fluxos de trabalho diferentes, otimizado para o tipo de dispositivo. Pode significar algum repetição de código, mas você pode minimizar que fatorar lógica comuns em uma camada subjacente ou de serviço.

Se você quiser tirar o **primeiro** opção e variar somente o layout do Razor por tipo de dispositivo, é muito fácil. Basta modificar o \_ViewStart.cshtml arquivos da seguinte maneira:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Agora você pode criar um layout específicas para dispositivos móveis chamado \_LayoutMobile.cshtml com uma estrutura de página e CSS regras otimizado para dispositivos móveis.

Se você quiser tirar o **segundo** opção totalmente diferentes modos de exibição de renderização de acordo com o tipo de dispositivo do visitante, consulte [postagem no blog de Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

O restante deste documento se concentra no **terceiro** opção – Criando controladores separados *e* modos de exibição para dispositivos móveis, para que você possa controlar exatamente o subconjunto da funcionalidade é oferecido para visitantes móveis.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configurando uma área móvel dentro de seu aplicativo ASP.NET MVC

Você pode adicionar uma área chamada "Móvel" para um aplicativo ASP.NET MVC existente da maneira normal: clique no nome do projeto no Gerenciador de soluções e escolha Adicionar à área. Em seguida, você pode adicionar controladores e modos de exibição como faria com qualquer outra área dentro de um aplicativo ASP.NET MVC. Por exemplo, adicione a sua área móvel um novo controlador chamado HomeController para atuar como uma página inicial para os visitantes móveis.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Garantindo a URL /Mobile atinge a home page móvel

Se você quiser /Mobile a URL para acessar a ação de índice no HomeController dentro de sua área móvel, você precisará fazer duas pequenas alterações à sua configuração de roteamento. Atualize sua classe MobileAreaRegistration primeiro, para que HomeController o controlador padrão em sua área móvel, conforme mostrado no código a seguir:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Isso significa que a home page móvel agora serão localizada em /Mobile, em vez de celular/Home, porque "Início" é agora o implicitamente o nome de controlador padrão para páginas de celular.

Em seguida, observe que, adicionando um segundo HomeController para seu aplicativo (ou seja, móveis aquele, além da área de trabalho uma existente), você será desfeito sua home page de área de trabalho regular. Haverá falha com o erro "*vários tipos que corresponda o controlador nomeado 'Início' foram encontrados*". Para resolver esse problema, atualize sua configuração de roteamento de nível superior (em Global.asax.cs) para especificar que a área de trabalho HomeController deve têm prioridade quando há ambiguidade:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Agora o erro irá ausente e a URL http://*yoursite*/ atingirá a home page da área de trabalho e http://*yoursite*/mobile/ atingirá a home page móvel.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Redirecionando visitantes móveis na sua área de móvel

Há muitos pontos de extensibilidade diferentes no ASP.NET MVC, portanto, há muitas maneiras possíveis de injetar lógica de redirecionamento. É uma opção interessante criar um atributo de filtro, [RedirectMobileDevicesToMobileArea], que realiza um redirecionamento se as seguintes condições forem atendidas:

1. É a primeira solicitação na sessão do usuário (ou seja, Session.IsNewSession é igual a true)
2. A solicitação vier de um navegador móvel (ou seja, Request.Browser.IsMobileDevice é igual a true)
3. O usuário já não solicita um recurso na área de trabalho móvel (ou seja, o *caminho* parte da URL não começa com /Mobile)

O exemplo disponível para download que acompanha este white paper inclui uma implementação dessa lógica. Ele é implementado como um filtro de autorização, derivado de AuthorizeAttribute, o que significa que ele pode funcionar corretamente, mesmo se você estiver usando o cache de saída (caso contrário, se um acessos de primeiro visitante de área de trabalho uma URL determinado, a saída de área de trabalho podem ser armazenados em cache e, em seguida, servido para visitantes móveis subsequentes).

Como é um filtro, você pode escolher a específicos controladores e ações, por exemplo,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… ou você pode aplicá-la a todos os controladores e ações como um MVC 3 *filtro global* em seu arquivo asax:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

O exemplo disponível para download também demonstra como você pode criar as subclasses desse atributo redirecionam para locais específicos dentro de sua área móvel. Isso significa que, por exemplo, você pode:

- Registre um filtro global conforme mostrado acima que envia os visitantes móveis para a home page móvel por padrão.
- Também se aplicam um filtro [RedirectMobileDevicesToMobileProductPage] especial para uma ação de "Exibir produto" que leva os visitantes móveis para a versão móvel de qualquer página que eles solicitaram.
- Também aplicar outros subclasses especiais do filtro para ações específicas, redirecionando visitantes móveis para a página móvel equivalente

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurando a autenticação de formulários para respeitar as páginas móveis

Se você estiver usando autenticação de formulários, você deve observar que quando um usuário precisa entrar, ele automaticamente redireciona o usuário para um único "logon" URL específica, que, por padrão, é **conta/LogOn**. Isso significa que os usuários móveis podem ser redirecionados para a ação de área de trabalho "fazer logon".

Para evitar esse problema, adicione lógica para a ação de "logon" área de trabalho para que ele redireciona os usuários móveis novamente para uma ação de "logon" móveis. Se você estiver usando o modelo de aplicativo do ASP.NET MVC padrão, atualize ação de LogOn do AccountController da seguinte maneira:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… e, em seguida, implemente uma ação de "logon" específicas para dispositivos móveis adequado em um controlador chamado AccountController em sua área móvel.

### <a name="working-with-output-caching"></a>Trabalhando com o cache de saída

Se você estiver usando o filtro [OutputCache], você deve forçar a entrada de cache para variar por tipo de dispositivo. Por exemplo, escreva:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Em seguida, adicione o seguinte método ao arquivo asax:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Isso garantirá que visitantes móveis não recebem saída anteriormente colocada em cache por um visitante de área de trabalho.

### <a name="a-working-example"></a>Um exemplo de trabalho

Para ver essas técnicas em ação, baixe [código deste white paper associado exemplos](https://docs.microsoft.com/aspnet/mobile/overview). O exemplo inclui um aplicativo ASP.NET MVC 3 (versão Release Candidate) aprimorado para dar suporte a dispositivos móveis usando os métodos descritos acima.

## <a name="further-guidance-and-suggestions"></a>Mais diretrizes e sugestões

A discussão a seguir se aplica a formulários da Web e os desenvolvedores MVC que estão usando as técnicas discutidas neste documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Melhorando a sua lógica de redirecionamento usando 51Degrees.mobi Foundation

A lógica de redirecionamento mostrada neste documento pode ser perfeitamente suficiente para seu aplicativo, mas não funcionará se você precisar desativar sessões, ou com navegadores de dispositivos móveis que rejeitar cookies (eles não podem ter sessões), porque ele não sabe se uma determinada solicitação é o primeiro de que o visitante.

Você já aprendeu como o 51Degrees.mobi Foundation do código-fonte aberto pode melhorar a precisão do ASP. Detecção de navegador do NET. Ele também tem uma capacidade interna de redirecionar os visitantes móveis locais específicos configurados no Web. config. É possível trabalhar sem dependendo de sessões ASP.NET (e, portanto, os cookies) ao armazenar um log temporário de hashes de cabeçalhos HTTP e os endereços IP dos visitantes, portanto ele sabe se cada solicitação é o primeiro de um determinado vistor.

O seguinte elemento adicionado à seção fiftyOne do arquivo Web. config será redirecionado a primeira solicitação de um dispositivo móvel detectado para a página ~ / Mobile/Default.aspx. As solicitações para páginas sob a pasta móvel serão *não* ser redirecionado, independentemente do tipo de dispositivo. Se o dispositivo móvel ficar inativo por um período de 20 minutos ou mais o dispositivo será esquecido e as solicitações subsequentes serão tratadas como novos para fins de redirecionamento.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Para obter mais detalhes, consulte [51degrees.mobi documentação Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Você *pode* recurso de redirecionamento do uso 51Degrees.mobi Foundation em aplicativos ASP.NET MVC, mas você precisará definir a configuração de redirecionamento em termos de URLs simples, não em termos de parâmetros de roteamentos ou colocando o MVC filtros em ações. Isso ocorre porque (no momento da gravação) 51Degrees.mobi Foundation não reconhece roteamento ou filtros.


### <a name="disabling-transcoders-and-proxy-servers"></a>Desabilitando transcodificadores e servidores Proxy

Operadores de rede móvel tem dois objetivos amplos abordagem à internet móvel:

1. Fornecer conteúdo relevante muito possível
2. Maximizar o número de clientes que podem compartilhar largura de banda de rede limitada de rádio

Como a maioria das páginas da web foram criados para telas grandes porte a área de trabalho e conexões de banda larga da linha rápida fixa, muitos operadores usam *transcodificadores* ou *servidores proxy* que alterar dinamicamente o conteúdo da web. Eles podem modificar sua marcação HTML ou CSS de acordo com a telas menores (especialmente para "recurso telefones" que não têm a capacidade de processamento para lidar com layouts complexos), e eles podem recompactar as imagens (reduzindo significativamente a qualidade) para melhorar a velocidade de entrega da página.

Mas se você tiver feito o esforço para produzir uma versão otimizado para celular do seu site, provavelmente não deseja que o operador de rede para interferir com ele qualquer adicional. Você pode adicionar a linha a seguir para a página\_evento de carregamento de qualquer forma de Web do ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Ou, para um controlador do ASP.NET MVC, você pode adicionar a substituição de método a seguir para que ele se aplica a todas as ações desse controlador:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

A mensagem HTTP resultante informa transcodificadores compatíveis do W3C e proxies não deve alterar o conteúdo. Obviamente, não há nenhuma garantia de que os operadores de rede móvel respeitarão essa mensagem.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Páginas de celular de estilo para navegadores de dispositivos móveis

Ele está além do escopo deste documento descrevem em detalhes quais tipos de trabalho de marcação HTML corretamente ou quais técnicas de design de web maximizar a usabilidade em dispositivos específicos. Ele tem backup para localizar um layout simples suficientemente, otimizado para uma tela de tamanho de celular, sem usar confiável HTML ou posicionamento truques CSS. No entanto, é uma técnica importante vale a pena mencionar, o *marca meta viewport*.

Determinados navegadores móveis modernos, em uma página da web de exibição de esforço destinam-se para monitores de área de trabalho, renderizam a página em uma tela de virtual, também chamada de "visor" (por exemplo, o visor virtual é 980 pixels de largura no iPhone e 850 pixels de largura no Opera Mobile por padrão) e, em seguida, reduzi o resultado para caber em pixels físicos da tela. O usuário pode aplicar zoom e deslocar-se que visor. Isso tem a vantagem de que ele permite que o navegador exibirá a página em seu layout desejado, mas também tem a desvantagem é que ele força o zoom e panorâmica, que é inconveniente para o usuário. Se você estiver criando para dispositivos móveis, é melhor design para uma tela específica para que nenhuma ampliação ou rolagem horizontal é necessário.

É uma forma de dizer que o navegador móvel a largura do visor deve ser por meio do padrão *visor* marca meta. Por exemplo, se você adicionar o seguinte à seção de cabeçalho da página,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… suporte a navegadores do smartphone irá dispor a página em uma tela de virtual amplo 480 pixels. Isso significa que, se os elementos HTML definem as larguras em termos percentuais, as porcentagens serão interpretadas em relação a esse 480 pixels de largura, não a largura do visor padrão. Como resultado, o usuário é menos provável aplicar zoom e panorâmica horizontalmente – consideravelmente melhorar a experiência de navegação móvel.

Se você desejar que a largura do visor para corresponder pixels físicos do dispositivo, você pode especificar o seguinte:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Para que isso funcione corretamente, você deve não explicitamente forçar elementos exceda essa largura (por exemplo, usando um *largura* atributo ou propriedade CSS), caso contrário, o navegador será forçado a usar um visor maior independentemente. Consulte também: [mais detalhes sobre a marca viewport padrão](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Suporte a mais modernos smartphones *orientação dupla*: eles podem ser mantidos no modo retrato ou paisagem. Portanto, é importante não faça suposições sobre a largura da tela em pixels. Não mesmo presuma que a largura da tela é fixo, porque o usuário pode orientar novamente seu dispositivo enquanto eles estiverem em sua página.

Dispositivos mais antigos do Windows Mobile e Blackberry também podem aceitar as seguintes marcas meta no cabeçalho da página para informá-los conteúdo foi otimizado para dispositivos móveis e, portanto, não devem ser transformadas.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Recursos adicionais

Para obter uma lista de emuladores de dispositivos móveis e simuladores, você pode usar para testar seu aplicativo móvel de web do ASP.NET, consulte a página [simular dispositivos móveis populares para teste](../mobile/device-simulators.md).

## <a name="credits"></a>Créditos

- Autor principal: Steven Sanderson
- Revisores / adicionais gravadores de conteúdo: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
