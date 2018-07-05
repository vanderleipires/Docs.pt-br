---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Como: Adicionar páginas móveis ao seu Web Forms do ASP.NET / aplicativo MVC | Microsoft Docs'
author: rick-anderson
description: Esse manual descreve várias maneiras para servir páginas otimizadas para dispositivos móveis do seu Web Forms do ASP.NET / aplicativo MVC e sugere, arquitetura e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: ''
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 075329087cb5e07d85bba0c546538e7cc55ac463
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366751"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Como: Adicionar páginas móveis ao seu Web Forms do ASP.NET / aplicativo MVC
====================
> **Aplica-se a**
> 
> - Versão de Web Forms do ASP.NET 4.0
> - ASP.NET MVC versão 3.0
> 
> **Resumo**
> 
> Esse manual descreve várias maneiras para servir páginas otimizadas para dispositivos móveis do seu Web Forms do ASP.NET / aplicativo MVC e sugere, arquitetura e design problemas a serem considerados ao direcionar uma ampla gama de dispositivos. Este documento também explica por que o os controles ASP.NET do ASP.NET 2.0 para 3.5 agora estão obsoletos e discute algumas alternativas modernas.


## <a name="contents"></a>Conteúdo

- Visão geral
- Opções de arquitetura
- Detecção de navegadores e dispositivos
- Como aplicativos de Web Forms do ASP.NET podem apresentar a páginas específicas para dispositivos móveis
- Como os aplicativos ASP.NET MVC podem apresentar páginas específicas para dispositivos móveis
- Recursos adicionais

Para obter exemplos de código para download que demonstra técnicas deste white paper para ASP.NET Web Forms e MVC, consulte [os aplicativos móveis e Sites com ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Visão geral

Dispositivos móveis – smartphones, celulares e tablets – continuam a crescer em termos de popularidade como um meio para acessar a Web. Para muitas empresas voltados para a web e os desenvolvedores da web, isso significa que é cada vez mais importante fornecer uma excelente experiência de navegação para visitantes que usam esses dispositivos.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Versões anteriores como de navegadores para dispositivos móveis com suporte do ASP.NET

Versões do ASP.NET 2.0 para 3.5 incluído *controles móveis do ASP.NET*: um conjunto de controles de servidor para dispositivos móveis nos *System* assembly e o  *MobileControls* namespace. O assembly ainda está incluído no ASP.NET 4, mas ela foi preterida. Os desenvolvedores são aconselhados a migrar as abordagens mais modernos, como as descritas neste documento.

O motivo por que os controles ASP.NET foram marcados como obsoleto é que o seu design é orientado em torno de celulares que eram comuns em torno de 2005 e versões anteriores. Os controles são projetados principalmente para renderizar marcação WML ou cHTML (em vez de HTML regular) para os navegadores WAP do que era. Mas, WAP e WML cHTML não são mais relevantes para a maioria dos projetos, porque o HTML agora é a linguagem de marcação de todos os lugares para os navegadores móveis e da área de trabalho semelhantes.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Os desafios de que dão suporte a dispositivos móveis

Embora navegadores móveis agora quase universalmente ofereçam suporte a HTML, você ainda enfrentará muitos desafios com o objetivo de criar excelentes experiências de navegação móveis:

- ***Tamanho da tela*** – os dispositivos móveis variam bastante no formulário e suas telas costumam ser muito menor do que os monitores de área de trabalho. Portanto, você precisa criar layouts de página completamente diferente para eles.
- ***Métodos de entrada*** – alguns dispositivos têm teclados, algumas têm styluses, outros usam o toque. Você talvez precise considerar a navegação com vários métodos de entrada de dados e mecanismos.
- ***Conformidade com padrões*** – muitos navegadores móveis não dão suporte os padrões mais recentes de HTML, CSS ou JavaScript.
- ***Largura de banda*** – desempenho de rede de dados de celular varia muito, e alguns usuários finais estão em tarifas cobram por megabytes.

Há uma solução padronizada; seu aplicativo terá que procurar e se comportam diferentemente, de acordo com o dispositivo acessá-lo. Dependendo de qual nível de suporte móvel que você deseja, isso pode ser um desafio maior para desenvolvedores da web que já era a área de trabalho "guerras de navegador".

Os desenvolvedores se aproximando do suporte do navegador móvel pela primeira vez geralmente inicialmente pensam que só é importante dar suporte a smartphones mais sofisticadas e mais recentes (por exemplo, Windows Phone 7, iPhone ou Android), talvez porque os desenvolvedores geralmente pessoal possuem como dispositivos. No entanto, mais baratos telefones ainda são muito populares e seus proprietários usá-los para navegar na web – especialmente em países em que os celulares são mais fáceis de obter que uma conexão de banda larga. Sua empresa precisará decidir quais variedade de dispositivos para dar suporte a considerando seus clientes provavelmente. Se você estiver criando um folheto on-line para um spa de integridade de luxo, você pode tomar uma decisão de negócios somente para smartphones avançados, de destino enquanto se você estiver criando um sistema de reservas de tíquete para um cinema, você precisa levar em conta os visitantes com recurso menos potentes provavelmente telefones.

## <a name="architectural-options"></a>Opções de arquitetura

Antes de entrarmos em detalhes técnicos específicos do ASP.NET Web Forms ou MVC, observe que os desenvolvedores da web em geral têm três opções possíveis principais para dar suporte a navegadores para dispositivos móveis:

1. ***Não faça nada –*** você pode simplesmente criar um aplicativo orientado a área de trabalho, standard, web e dependem de navegadores para dispositivos móveis para renderizá-lo de forma aceitável. 

    - **Vantagem**: é a opção mais econômica de implementar e manter – um trabalho não adicional
    - **Desvantagem**: fornece a experiência do usuário final pior: 

        - Os smartphones mais recente pode processar seu HTML tão bem quanto um navegador da área de trabalho, mas os usuários ainda serão forçados para ampliar e rolar horizontalmente e verticalmente para consumir o conteúdo em uma tela pequena. Isso está longe de ser ideal.
        - Dispositivos mais antigos e celulares podem falhar ao renderizar sua marcação de maneira satisfatória.
        - Mesmo em dispositivos tablet mais recentes (cujas telas podem ser tão grandes quanto as telas de laptop), interação com diferentes regras se aplicam. Entrada baseados em toque funciona melhor com maior botões e links de disseminação ainda mais distantes, e não é possível passar o mouse de um cursor do mouse sobre um menu suspenso.
2. ***Resolver o problema no cliente* –** com o uso cuidadoso de CSS e [aprimoramento progressivo](http://en.wikipedia.org/wiki/Progressive_enhancement) você pode criar marcação, estilos e scripts que se adaptam a qualquer navegador é executá-los. Por exemplo, com [consultas de mídia do CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), você pode criar um layout de várias coluna que se transforma em um layout de coluna única em dispositivos cujas telas são mais estreitas do que um limite determinado. 

    - **Vantagem**: otimiza o processamento para o dispositivo específico em uso, mesmo para dispositivos desconhecidos de futuros acordo com qualquer tela e características de entrada tiverem
    - **Vantagem**: facilmente permite que você compartilhe a lógica do lado do servidor em todos os tipos de dispositivo – mínimo de duplicação de código ou esforço
    - **Desvantagem**: dispositivos móveis são tão diferentes dispositivos da área de trabalho que você pode realmente deseja suas páginas móveis ser completamente diferente de suas páginas da área de trabalho, mostrando informações diferentes. Essas variações podem ser ineficientes ou impossíveis de atingir com robustez por meio da CSS sozinho, especialmente considerando como inconsistentemente dispositivos mais antigos interpretam as regras de CSS. Isso é especialmente verdadeiro para atributos de CSS 3.
    - **Desvantagem**: não oferece suporte para fluxos de trabalho para diferentes dispositivos e variada lógica do lado do servidor. Você não pode, por exemplo, implementar um simplificada compras carrinho check-out fluxo de trabalho para usuários móveis por meio de CSS sozinho.
    - **Desvantagem**: uso ineficiente de largura de banda. Servidor você pode ter que transmitir a marcação e estilos que se aplicam a todos os dispositivos possíveis, mesmo que o dispositivo de destino usará apenas um subconjunto dessas informações.
3. ***Resolver o problema no servidor* –** se seu servidor sabe qual dispositivo está acessando – ou pelo menos as características desse dispositivo, como seu tamanho da tela e o método de entrada, e se ele é um dispositivo móvel – pode ser executada uma lógica diferente e marcação HTML diferente de saída. 

    - **Vantagem:** flexibilidade máxima. Não há nenhum limite para quanto você pode variar sua lógica do lado do servidor para celulares ou otimizar sua marcação para o layout desejado, específico do dispositivo.
    - **Vantagem:** uso eficiente da largura de banda. Você só precisar transmitir a marcação e informações de estilo que vai usar o dispositivo de destino.
    - **Desvantagem:** às vezes força a repetição de esforço ou de código (por exemplo, fazer com que você crie cópias semelhantes, mas ligeiramente diferentes de suas páginas Web Forms ou as exibições do MVC). Onde possível que será fatorar lógica comum em uma camada subjacente ou serviço, mas ainda assim, algumas partes do seu código de interface do usuário ou a marcação talvez precise ser duplicado e, em seguida, são mantidos em paralelo.
    - **Desvantagem:** detecção do dispositivo não é trivial. Ele requer uma lista ou um banco de dados de tipos de dispositivos conhecidos e suas características (que nem sempre podem ser perfeitamente atualizadas) e não é garantido para corresponder com precisão a cada solicitação de entrada. Este documento descreve algumas opções e suas armadilhas mais tarde.

Para obter os melhores resultados, a maioria dos desenvolvedores localizar que precisam para combinar as opções (2) e (3). Pequenas diferenças estilísticas são melhor acomodadas no cliente usando CSS ou até mesmo JavaScript, enquanto as principais diferenças de dados, fluxo de trabalho ou marcação com mais eficiência são implementadas no código do lado do servidor.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Este documento se concentra em técnicas do lado do servidor

Como Web Forms do ASP.NET e MVC são ambas as tecnologias do lado do servidor principalmente, este white paper se concentra em técnicas de lado do servidor que permitem que você produza uma marcação diferente e a lógica para navegadores de dispositivos móveis. É claro, você também pode combinar isso com qualquer técnica do lado do cliente (por exemplo, CSS 3 consultas de mídia, o JavaScript de aprimoramento progressivo), mas que é mais uma questão de design da web de desenvolvimento do ASP.NET.

## <a name="browser-and-device-detection"></a>Detecção de navegadores e dispositivos

O pré-requisito de chave para todas as técnicas do lado do servidor para dar suporte a dispositivos móveis é saber qual dispositivo está usando o seu visitante. Na verdade, ainda melhor do que saber o número de fabricante e modelo de que o dispositivo é saber o *características* do dispositivo. Características podem incluir:

- É um dispositivo móvel?
- Método de entrada (mouse/teclado, toque, teclado, joystick,...)
- Tamanho da tela (fisicamente e em pixels)
- Formatos de mídia e os dados com suporte
- Etc.

É melhor tomar decisões com base nas características que o número do modelo, porque, em seguida, você estará mais bem equipado para lidar com dispositivos futuros.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Usando o ASP. Suporte de detecção de navegador interno da rede

Os desenvolvedores ASP.NET Web Forms e MVC imediatamente podem descobrir características importantes de um navegador visitando inspecionando as propriedades do *Request. browser* objeto. Por exemplo, consulte

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ... e muitos outros

Nos bastidores, a plataforma ASP.NET corresponde à entrada *User-Agent* cabeçalho HTTP (UA) contra expressões regulares em um conjunto de arquivos XML de definição do navegador. Por padrão, a plataforma inclui definições de vários dispositivos móveis comuns, e você pode adicionar arquivos de definição de navegador personalizados para outras pessoas que você deseja reconhecer. Para obter mais detalhes, consulte a página do MSDN [controles de servidor Web ASP.NET e recursos do navegador](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Usando o banco de dados do dispositivo WURFL via 51Degrees.mobi Foundation

Embora o ASP. Suporte de detecção de navegador interno da rede será suficiente para muitos aplicativos, há dois principais casos em que talvez não seja suficiente:

- ***Você deseja reconhecer os dispositivos mais recentes***(sem criar manualmente os arquivos de definição do navegador para eles). Observe que os arquivos de definição de navegador do .NET 4 não são suficientemente recentes para reconhecer Apple iPads, telefones Android, navegadores Opera Mobile ou Windows Phone 7.
- ***Você precisa obter informações mais detalhadas sobre os recursos de dispositivo***. Talvez você precise saber sobre o método de entrada de um dispositivo (por exemplo, o teclado vs de toque) ou quais áudio formata o navegador dá suporte. Essas informações não estão disponíveis nos arquivos de definição do navegador padrão.

O [ *arquivo de recurso Universal sem fio* project (WURFL)](http://wurfl.sourceforge.net/) mantém muito mais informações atualizadas e detalhadas sobre os dispositivos móveis em uso hoje em dia.

A ótima notícia para desenvolvedores do .NET é que o ASP. O recurso de detecção de navegador do NET é extensível, portanto, é possível aperfeiçoá-lo para superar esses problemas. Por exemplo, você pode adicionar código-fonte aberto [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) biblioteca ao seu projeto. É um IHttpModule do ASP.NET ou um navegador recursos de provedor (pode ser usado em aplicativos Web Forms e MVC), que lê dados WURFL diretamente e o vincula ao ASP. Mecanismo de detecção de navegador interno da rede. Depois que você instalou o módulo *Request. browser* repentinamente conterá informações detalhadas e muito mais precisas: ele corretamente reconhecerá muitos dos dispositivos mencionados anteriormente e listar seus recursos (incluindo recursos adicionais, como método de entrada). Consulte a documentação do projeto para obter mais detalhes.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Como aplicativos de formulários da Web podem apresentar a páginas específicas para dispositivos móveis

Por padrão, aqui está como um novo aplicativo de Web Forms exibe em dispositivos móveis comuns:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Obviamente, nem layout ficaria muito amigáveis para dispositivos móveis – esta página foi projetada para um monitor grande, com orientação paisagem, não para uma tela pequena e orientada a retrato. Portanto, o que você pode fazer sobre ele?

Conforme discutido anteriormente neste documento, há várias maneiras para personalizar suas páginas para dispositivos móveis. Algumas técnicas são baseados em servidor, outros executam no cliente.

### <a name="creating-a-mobile-specific-master-page"></a>Criando uma página mestra do específicas para dispositivos móveis

Dependendo dos seus requisitos, você poderá usar os mesmos formulários da Web para todos os visitantes, mas têm duas páginas mestres separadas: uma para visitantes de área de trabalho, outro para visitantes por dispositivos móveis. Isso lhe dá a flexibilidade de alterar sua marcação HTML de nível superior de acordo com os dispositivos móveis, sem forçá-lo a duplicar a lógica de qualquer página ou de folha de estilos CSS.

Isso é fácil de fazer. Por exemplo, você pode adicionar um manipulador de PreInit como o seguinte para um formulário da Web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Agora, crie uma página mestra chamada Mobile.Master na pasta de nível superior do seu aplicativo, e ele será usado quando um dispositivo móvel for detectado. A página mestra móvel pode fazer referência a uma folha de estilos CSS específicas para dispositivos móveis se necessário. Os visitantes da área de trabalho ainda verá a página mestra padrão, não aquela móvel.

### <a name="creating-independent-mobile-specific-web-forms"></a>Criando independentes do Web Forms móveis específicas

Para máxima flexibilidade, você pode ir muito mais do que ter apenas páginas mestras separadas para diferentes tipos de dispositivos. Você pode implementar dois *totalmente separadas conjuntos de páginas de Web Forms* – um outro conjunto para navegadores de desktop, definido para celulares. Isso funciona melhor se você quiser apresentar informações muito diferentes ou fluxos de trabalho aos visitantes por dispositivos móveis. O restante desta seção descreve essa abordagem em detalhes.

Supondo que você já tiver um aplicativo Web Forms projetado para navegadores de desktop, a maneira mais fácil para prosseguir é criar uma subpasta chamada "Móvel" dentro de seu projeto e criar suas páginas de dispositivos móveis. Você pode construir um subsite inteira, com suas próprias páginas mestras, folhas de estilo e páginas, usando as mesmas técnicas que você deseja usar para qualquer outro aplicativo de formulários da Web. Você não precisa necessariamente para produzir um equivalente de móvel para *cada* página no site da área de trabalho; você pode escolher o subconjunto da funcionalidade faz sentido para visitantes por dispositivos móveis.

Páginas de dispositivos móveis podem compartilhar recursos estáticos comuns (como imagens, JavaScript ou CSS arquivos) com suas páginas regulares se desejar. Uma vez que a pasta "Móvel" será *não* ser marcado como um aplicativo separado quando hospedado no IIS (ele é apenas uma subpasta simple de páginas Web Forms), ele também compartilhará a mesma configuração, dados de sessão e outra infraestrutura como seu páginas da área de trabalho.

> [!NOTE]
> Como essa abordagem geralmente envolve algumas eliminação de duplicação de código (páginas móveis provavelmente compartilham algumas semelhanças com páginas da área de trabalho), é importante para criar qualquer negócio dados ou lógica de acesso código comum em uma camada subjacente compartilhada ou um serviço. Caso contrário, você vai double o esforço de criação e manutenção de seu aplicativo.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Redirecionando visitantes por dispositivos móveis às suas páginas móveis

É conveniente redirecionar os visitantes por dispositivos móveis para as páginas móveis apenas na *primeiro* solicitar em sua sessão de navegação (e não em cada solicitação em sua sessão), porque:

- Em seguida, você pode facilmente permitir visitantes por dispositivos móveis acessar suas páginas da área de trabalho caso queiram – apenas colocar um link na página mestra que vai para "Versão da área de trabalho". O visitante não ser redirecionado para uma página móvel, porque não é a primeira solicitação em sua sessão.
- Ele evita o risco de interferir solicitações para quaisquer recursos dinâmicos compartilhados entre partes móveis e desktops do seu site (por exemplo, se você tiver um formulário da Web comuns que exibem de partes móveis e desktops do seu site em um IFRAME, ou certos manipuladores de Ajax)

Para fazer isso, você pode colocar a lógica de redirecionamento em um **sessão\_iniciar** método. Por exemplo, adicione o seguinte método ao seu arquivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurando a autenticação de formulários para respeitar a suas páginas para dispositivos móveis

Observe que a autenticação de formulários faz determinadas suposições sobre onde ele pode redirecionar os visitantes durante e após o processo de autenticação:

- Quando um usuário precisa ser autenticada, autenticação de formulários serão redirecioná-las à sua página de logon da área de trabalho, independentemente se eles estão um usuário móvel ou da área de trabalho (porque ele tem um conceito de apenas *um* URL de logon). Supondo que você deseja definir o estilo de sua página de logon móvel diferente, você precisa aprimorar sua página de logon da área de trabalho, de modo que ele redireciona os usuários móveis a uma página de logon móvel separado. Por exemplo, adicione o seguinte código ao seu **desktop** lógica para página de logon: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Depois que um usuário faz logon com êxito, autenticação de formulários será por padrão redirecioná-las para a home page da área de trabalho (porque ele tem um conceito de apenas *um* página padrão). Você precisa aprimorar sua página de logon móveis, de modo que ele redireciona para a home page móvel após um logon bem-sucedido. Por exemplo, adicione o seguinte código ao seu **móvel** lógica para página de logon: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Esse código pressupõe que sua página tiver um controle de servidor de logon chamado LoginUser, como no modelo de projeto padrão.

### <a name="working-with-output-caching"></a>Trabalhando com o cache de saída

Se você estiver usando o cache de saída, lembre-se de que por padrão, é possível que um usuário da área de trabalho visitar um determinado URL (fazendo com que sua saída seja armazenada em cache), seguido por um usuário móvel que, em seguida, recebe a saída da área de trabalho do cache. Esse aviso se aplica independentemente de você apenas estiver variando a página mestra por tipo de dispositivo ou implementar totalmente separadas do Web Forms por tipo de dispositivo.

Para evitar o problema, você pode instruir o ASP.NET para variar a entrada de cache de acordo com se o visitante está usando um dispositivo móvel. Adicione um parâmetro de VaryByCustom a declaração de OutputCache da sua página da seguinte maneira:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Em seguida, defina *isMobileDevice* como um cache personalizado parâmetro, adicionando o seguinte método de substituição ao seu arquivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Isso garantirá que os visitantes por dispositivos móveis para a página não recebem saída colocar previamente no cache por um visitante da área de trabalho.

### <a name="a-working-example"></a>Um exemplo de trabalho

Para ver essas técnicas em ação, baixe [exemplos de código neste white paper](https://docs.microsoft.com/aspnet/mobile/overview). O aplicativo de exemplo de formulários da Web redireciona automaticamente os usuários móveis a um conjunto de páginas específicas para dispositivos móveis em uma subpasta chamada Mobile. A marcação e o estilo dessas páginas melhor é otimizado para navegadores de dispositivos móveis, como você pode ver de capturas de tela as seguir:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Para obter mais dicas sobre como otimizar sua marcação e CSS para navegadores de dispositivos móveis, consulte a seção "Estilos páginas móveis para navegadores de dispositivos móveis" neste documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Como os aplicativos ASP.NET MVC podem apresentar páginas específicas para dispositivos móveis

Uma vez que o padrão Model-View-Controller separa a lógica do aplicativo (em controladores) da lógica de apresentação (nos modos de exibição), você pode escolher de qualquer uma das seguintes abordagens para lidar com suporte móvel no código do lado do servidor:

1. ***Usar o mesmo controladores e modos de exibição para navegadores móveis e desktops, mas renderizar as exibições com diferentes layouts de Razor dependendo do tipo de dispositivo *.** Essa opção funciona melhor se você estiver exibindo dados idênticos em todos os dispositivos, mas simplesmente deseja fornecer diferentes folhas de estilo CSS ou alterar alguns elementos de HTML de nível superior para celulares.
2. ***Usar os mesmos controladores para navegadores móveis e desktops, mas renderizar exibições diferentes dependendo do tipo de dispositivo***. Essa opção funciona melhor se você estiver exibindo aproximadamente os mesmos dados e fornecendo os mesmos fluxos de trabalho para os usuários finais, mas deseja renderizar a marcação HTML muito diferente de acordo com o dispositivo que está sendo usado.
3. ***Criar áreas separadas para navegadores móveis e desktops, Implementando independentes controladores e modos de exibição para cada *.** Essa opção funciona melhor se você estiver exibindo as telas muito diferentes, que contém informações diferentes e leva o usuário por meio de diferentes fluxos de trabalho otimizada para seu tipo de dispositivo. Pode significar algumas repetições de código, mas você pode minimizar que ao fatorar a lógica comum em uma camada ou o serviço subjacente.

Se você quiser executar o **primeiro** opção e variar somente o layout do Razor por tipo de dispositivo, é muito fácil. Basta modificar seu \_viewstart. cshtml arquivos da seguinte maneira:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Agora você pode criar um layout específicas para dispositivos móveis chamado \_LayoutMobile.cshtml com uma estrutura de página e CSS regras otimizados para dispositivos móveis.

Se você quiser executar o **segundo** opção totalmente diferentes modos de exibição de renderização acordo com o tipo de dispositivo do visitante, consulte [postagem de blog de Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

O restante deste documento se concentra na **terceiro** opção – Criando controladores separados *e* modos de exibição para dispositivos móveis, portanto, você pode controlar exatamente quais subconjunto da funcionalidade é oferecido para visitantes por dispositivos móveis.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configurando uma área móvel dentro de seu aplicativo ASP.NET MVC

Você pode adicionar uma área chamada "Móvel" para um aplicativo ASP.NET MVC existente da maneira normal: clique com botão direito no nome do projeto no Gerenciador de soluções e escolha Adicionar à área. Em seguida, você pode adicionar controladores e exibições como você faria para qualquer outra área dentro de um aplicativo ASP.NET MVC. Por exemplo, adicione à sua área móvel um novo controlador chamado HomeController para atuar como uma home page para visitantes por dispositivos móveis.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Garantindo a URL /Mobile atinge a home page da móvel

Se você quiser /Mobile a URL para acessar a ação Index em HomeController dentro de sua área móvel, você precisará fazer duas pequenas alterações em sua configuração de roteamento. Primeiro, atualize sua classe MobileAreaRegistration para que o HomeController é o controlador padrão em sua área móvel, conforme mostrado no código a seguir:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Isso significa que a home page da móvel agora estará localizado em /Mobile, em vez de/móvel/Home, como "Home" agora é o implicitamente o nome de controlador padrão para páginas para dispositivos móveis.

Em seguida, observe que, ao adicionar um segundo HomeController ao seu aplicativo (ou seja, móvel aquele, além de área de trabalho um existente), você vai ter dividido sua home page da área de trabalho regular. Ocorrerá uma falha com o erro "*foram encontrados vários tipos que correspondem ao controlador chamado 'Home'*". Para resolver esse problema, atualize sua configuração de roteamento de nível superior (em Global.asax.cs) para especificar que o HomeController da área de trabalho deve têm prioridade quando não há ambiguidade:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Agora, o erro irá distância e a URL http://<em>seusite</em>/ atingirá a home page da área de trabalho e http://<em>seusite</em>/mobile/ alcançará a móvel da home page.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Redirecionando visitantes por dispositivos móveis na sua área de móvel

Há muitos pontos de extensibilidade diferentes no ASP.NET MVC, portanto, há muitas maneiras possíveis de injetar lógica de redirecionamento. Uma opção legal é criar um atributo de filtro, [RedirectMobileDevicesToMobileArea], que executa um redirecionamento se as seguintes condições forem atendidas:

1. É a primeira solicitação na sessão do usuário (ou seja, Session.IsNewSession é igual a true)
2. A solicitação vier de um navegador móvel (ou seja, Request.Browser.IsMobileDevice é igual a true)
3. O usuário já não solicita um recurso na área de trabalho móvel (ou seja, o *caminho* parte da URL não começa com /Mobile)

O exemplo disponível para download que acompanha este white paper inclui uma implementação da lógica do programa. Ele é implementado como um filtro de autorização, derivado de AuthorizeAttribute, o que significa que ele pode funcionar corretamente, mesmo se você estiver usando o cache de saída (caso contrário, se um acessa primeiro uma URL determinado, a saída da área de trabalho do visitante da área de trabalho pode ser armazenados em cache e, em seguida, servido a visitantes móveis subsequentes).

Como ele é um filtro, você pode escolher para aplicá-la para controladores específicos e ações, por exemplo,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… ou você pode aplicá-la a todos os controladores e ações como um MVC 3 *filtro global* em seu arquivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

O exemplo disponível para download também demonstra como você pode criar subclasses que redirecionam para locais específicos dentro de sua área móvel desse atributo. Isso significa que, por exemplo, você pode:

- Registre um filtro global conforme mostrado acima que envia os visitantes por dispositivos móveis para a home page de móvel por padrão.
- Também se aplicam um filtro [RedirectMobileDevicesToMobileProductPage] especial a uma ação de "Exibir produto" que aceita visitantes por dispositivos móveis para a versão móvel de qualquer página do produto que tinham solicitados.
- Também aplicar outras subclasses especiais do filtro para ações específicas, redirecionando os visitantes por dispositivos móveis para a página móvel equivalente

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configurando a autenticação de formulários para respeitar a suas páginas para dispositivos móveis

Se você estiver usando autenticação de formulários, você deve observar que quando um usuário precisa para fazer logon, ele automaticamente redireciona o usuário para uma única "logon" URL específica, que por padrão é **/conta/LogOn**. Isso significa que os usuários móveis podem ser redirecionados para a ação da área de trabalho "fazer logon".

Para evitar esse problema, adicione lógica para a ação de "logon" da área de trabalho, de modo que ele redireciona os usuários móveis novamente para uma ação de "logon" móvel. Se você estiver usando o modelo de aplicativo do ASP.NET MVC padrão, atualize a ação de LogOn do AccountController da seguinte maneira:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… e, em seguida, implemente uma ação de "logon" específicas para dispositivos móveis adequado em um controlador chamado AccountController em sua área móvel.

### <a name="working-with-output-caching"></a>Trabalhando com o cache de saída

Se você estiver usando o filtro [OutputCache], você deve forçar a entrada de cache para variar por tipo de dispositivo. Por exemplo, escreva:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Em seguida, adicione o seguinte método ao seu arquivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Isso garantirá que os visitantes por dispositivos móveis para a página não recebem saída colocar previamente no cache por um visitante da área de trabalho.

### <a name="a-working-example"></a>Um exemplo de trabalho

Para ver essas técnicas em ação, baixe [código deste white paper associado exemplos](https://docs.microsoft.com/aspnet/mobile/overview). O exemplo inclui um aplicativo ASP.NET MVC 3 (versão Release Candidate) aprimorado para dar suporte a dispositivos móveis usando os métodos descritos acima.

## <a name="further-guidance-and-suggestions"></a>Obter mais diretrizes e sugestões

A discussão a seguir se aplica a Web Forms e desenvolvedores MVC que estão usando as técnicas abordadas neste documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Aprimorando sua lógica de redirecionamento usando 51Degrees.mobi Foundation

A lógica de redirecionamento mostrada neste documento pode ser perfeitamente suficiente para seu aplicativo, mas ele não funcionará se você precisar desabilitar sessões ou com navegadores de dispositivos móveis que rejeitam cookies (eles não podem ter sessões), porque ele não saberá se uma determinada solicitação é o primeiro do que o visitante.

Você já aprendeu como o código-fonte aberto de 51Degrees.mobi Foundation pode melhorar a precisão do ASP. Detecção de navegador do NET. Ele também tem uma capacidade interna para redirecionar os visitantes por dispositivos móveis em locais específicos configurados em Web. config. Ele é capaz de trabalhar sem dependendo das sessões do ASP.NET (e, portanto, cookies), armazenando um log temporário de hashes de cabeçalhos HTTP e os endereços IP dos visitantes, portanto, ele sabe se cada solicitação é o primeiro de um determinada de visitante.

O seguinte elemento adicionado à seção fiftyOne do arquivo Web. config será redirecionado para a página, a primeira solicitação de um dispositivo móvel detectado ~ / Mobile/Default.aspx. Todas as solicitações para páginas sob a pasta móvel serão *não* ser redirecionada, independentemente do tipo de dispositivo. Se o dispositivo móvel ficar inativo por um período de 20 minutos ou mais o dispositivo será esquecido e solicitações subsequentes serão tratadas como novos para fins de redirecionamento.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Para obter mais detalhes, consulte [51degrees.mobi documentação Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Você *pode* recurso de redirecionamento do uso 51Degrees.mobi Foundation em aplicativos ASP.NET MVC, mas você precisará definir sua configuração de redirecionamento em termos de simples URLs, não em termos de parâmetros de roteamento ou colocando os filtros MVC em ações. Isso ocorre porque (no momento da gravação) 51Degrees.mobi Foundation não reconhece filtros ou roteamento.


### <a name="disabling-transcoders-and-proxy-servers"></a>Desabilitando transcodificadores e servidores Proxy

Operadores de rede móvel tem dois objetivos amplos em sua abordagem para a internet móvel:

1. Forneça como conteúdo relevante muito possível
2. Maximizar o número de clientes que podem compartilhar a largura de banda de rede limitada de rádio

Uma vez que a maioria das páginas da web foram criados para telas grandes em tamanho da área de trabalho e conexões de banda larga fixo linha rápida, muitos operadores usam *transcodificadores* ou *servidores proxy* que alteram dinamicamente o conteúdo da web. Eles podem modificar sua marcação HTML ou CSS para atender às telas menores (especialmente para "telefones de recurso" que não têm a capacidade de processamento para lidar com layouts complexos), e eles podem recompactar suas imagens (reduzindo significativamente a qualidade) para melhorar as velocidades de entrega de página.

Mas se você executou o esforço necessário para produzir uma versão otimizada para celular do seu site, você provavelmente não quer interferir com ele qualquer ainda mais o operador de rede. Você pode adicionar a seguinte linha para a página\_eventos de carga em qualquer formulário de Web do ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Ou, para um controlador ASP.NET MVC, você pode adicionar a seguinte substituição do método para que ele se aplica a todas as ações nesse controlador:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

A mensagem HTTP resultante informa transcodificadores compatíveis do W3C e proxies não deve alterar o conteúdo. Obviamente, não há nenhuma garantia de que os operadores de rede móvel respeitará essa mensagem.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Definir o estilo de páginas para dispositivos móveis para navegadores de dispositivos móveis

Ele está além do escopo deste documento descrever em detalhes quais tipos de trabalho de marcação HTML corretamente ou quais técnicas de design da web maximizar a usabilidade em dispositivos específicos. Ele tem-se para localizar um layout simples suficientemente, otimizado para uma tela de tamanho de celular, sem o uso de não-confiável HTML ou CSS truques de posicionamento. No entanto, é uma técnica importante que vale a pena mencionar, o *marca meta viewport*.

Determinados navegadores móveis modernos, em um esforço exibir as páginas da web para monitores de área de trabalho, renderizarem a página em uma tela de virtual, também chamada de "visor" (por exemplo, o visor virtual é 980 pixels de largura no iPhone e 850 pixels de largura no Opera Mobile por padrão) e, em seguida, Reduza o resultado para caber em pixels físicos da tela. O usuário pode, em seguida, aplicar zoom e panorâmica desse visor. Isso tem a vantagem que ele permite que o navegador exibir a página em seu layout desejado, mas é também tem a desvantagem de que ela obriga o zoom e panorâmica, que é inconveniente para o usuário. Se você estiver criando para dispositivos móveis, é melhor design para uma tela de estreita para que nenhuma aplicação de zoom ou de rolagem horizontal é necessário.

Uma forma de dizer ao navegador móvel a largura do visor deve ter é por meio de não-padrão *visor* marca meta. Por exemplo, se você adicionar o seguinte à seção de cabeçalho da sua página,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… em seguida, dar suporte a navegadores do smartphone irá dispor a página em uma tela de 480 pixels virtual amplos. Isso significa que, se seus elementos HTML definem a largura em termos percentuais, as porcentagens serão interpretadas em relação a esse 480 pixels de largura, não a largura do visor padrão. Como resultado, o usuário é menos propensos a aplique zoom e panorâmica horizontalmente – consideravelmente, melhorando a experiência de navegação móvel.

Se você quiser que a largura do visor para corresponder os pixels de física do dispositivo, você pode especificar o seguinte:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Para que isso funcione corretamente, você deve explicitamente forçar elementos exceder essa largura (por exemplo, usando um *largura* atributo ou propriedade CSS), caso contrário, o navegador será forçado a usar um visor maior independentemente. Consulte também: [mais detalhes sobre a marca viewport não padrão](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Suporte a mais modernos smartphones *orientação dupla*: eles podem ser mantidos em modo retrato ou paisagem. Portanto, é importante não fazer suposições sobre a largura da tela em pixels. Não mesmo presuma que a largura da tela é fixo, porque o usuário novamente pode orientar seu dispositivo enquanto eles estiverem em sua página.

Dispositivos Windows Mobile e Blackberry mais antigos também podem aceitar as seguintes marcas de meta no cabeçalho da página para informá-los conteúdo foi otimizado para dispositivos móveis e, portanto, não devem ser transformados.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Recursos adicionais

Para obter uma lista de emuladores de dispositivos móveis e simuladores, você pode usar para testar seu aplicativo de web móvel ASP.NET, consulte a página [simular dispositivos móveis populares para teste](../mobile/device-simulators.md).

## <a name="credits"></a>Créditos

- Autor principal: Steven Sanderson
- Os revisores / adicionais gravadores de conteúdo: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
