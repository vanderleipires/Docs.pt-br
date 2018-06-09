---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Criando projetos da Web do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: Este tópico explica as opções para a criação de projetos web ASP.NET no Visual Studio 2013 com atualização 3 aqui estão alguns dos novos recursos do c de desenvolvimento da web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: aacae7a9ccf483b21d3c6796c0411d558fa3c75b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28038859"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Criando projetos da Web do ASP.NET no Visual Studio 2013
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Este tópico explica as opções para a criação de projetos web ASP.NET no Visual Studio 2013 com atualização 3
> 
> Aqui estão alguns dos novos recursos para desenvolvimento na web em relação às versões anteriores do Visual Studio:
> 
> - Uma interface do usuário simple para a criação de projetos que oferecem [suporte para várias estruturas ASP.NET](#add) (Web Forms, MVC e API da Web).
> - [Identidade do ASP.NET](#indauth), um novo sistema de associação do ASP.NET que funciona da mesma em todas as estruturas do ASP.NET e funciona com o software que não seja o IIS de hospedagem na web.
> - O uso de [Bootstrap](#bootstrap) fornece recursos de design e temas responsivos.
> - Novos recursos para formulários da Web que costumava ser oferecido apenas para MVC, como [criação do projeto de teste automático](#testproj) e um [modelo de site de Intranet](#winauth).
> 
> Para obter informações sobre como criar projetos da web para serviços de nuvem do Azure ou serviços móveis do Azure, consulte [Introdução aos serviços de nuvem do Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) e [criando um Leaderboard App com .NET de serviços móveis do Azure Back-end](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Pré-requisitos

Este artigo se aplica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) com [atualização 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) instalado.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projetos de aplicativos Web versus projetos de site

ASP.NET permite que você escolha entre os dois tipos de projetos web: *projetos de aplicativo web* e *projetos de site*. Recomendamos que os projetos de aplicativo web para novos desenvolvimentos e este artigo se aplica somente a projetos de aplicativo web. Para obter mais informações, consulte [projetos de aplicativo da Web versus projetos de Site da Web no Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) no site do MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Visão geral da criação do projeto de aplicativo web

As etapas a seguir mostram como criar um projeto da web:

1. Clique em **novo projeto** no **iniciar** página ou o **arquivo** menu.
2. No **novo projeto** caixa de diálogo, clique em **Web** no painel esquerdo e **aplicativo Web ASP.NET** no painel central.

    ![Caixa de diálogo Novo Projeto](creating-web-projects-in-visual-studio/_static/image1.png)

    Você pode escolher **nuvem** no painel esquerdo para criar um [serviço de nuvem do Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [serviço móvel do Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), ou [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Este tópico não aborda esses modelos.
3. No painel direito, clique no **adicionar Application Insights ao projeto** caixa de seleção se desejar que o monitoramento da integridade e uso para seu aplicativo. Para obter mais informações, consulte [monitorar o desempenho em aplicativos da web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Especificar o projeto **nome**, **local**e outras opções e, em seguida, clique em **Okey**.

    O **novo projeto ASP.NET** caixa de diálogo é exibida.

    ![Caixa de diálogo Novo Projeto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Clique em um modelo.

    ![Selecione um modelo](creating-web-projects-in-visual-studio/_static/image3.png)
6. Se você deseja adicionar suporte para estruturas adicionais não incluídos no modelo, clique na caixa de seleção apropriada. (No exemplo mostrado, você pode adicionar MVC e/ou API da Web a um projeto de Web Forms.)

    ![Adicionar estruturas](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Se você deseja adicionar um projeto de teste de unidade, clique em **adicionar testes de unidade**.

    ![Adicionar testes de unidade](creating-web-projects-in-visual-studio/_static/image5.png)
8. Se você quiser um método de autenticação diferente do que o modelo fornece por padrão, clique em **alterar autenticação**.

    ![Configurar o botão de autenticação](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Configurar a caixa de diálogo de autenticação](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Criar um aplicativo web ou máquina virtual no Azure

O Visual Studio inclui recursos que tornam mais fácil trabalhar com os serviços do Azure para hospedar aplicativos web. Por exemplo, você pode fazer o seguinte diretamente do IDE do Visual Studio:

- Criar e gerenciar aplicativos web ou máquinas virtuais que tornar seu aplicativo disponível pela Internet.
- Exibir logs criados pelo aplicativo conforme ele é executado na nuvem.
- Execute no modo de depuração remotamente enquanto o aplicativo é executado na nuvem.
- Viiew e gerenciar outros serviços do Azure, como bancos de dados SQL.

Você pode [criar uma conta do Azure](https://www.windowsazure.com/pricing/free-trial/) que inclui serviços básicos, como aplicativos web gratuito e se você for assinante do MSDN, você pode [ativar benefícios](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) que lhe créditos mensais com o Azure adicional serviços. 

Por padrão o **novo projeto ASP.NET** caixa de diálogo permite que você crie um aplicativo web ou a máquina virtual para um novo projeto da web. Se você não deseja criar um novo aplicativo web ou a máquina virtual, desmarque o **Host na nuvem** caixa de seleção.

![Criar recursos remotos](creating-web-projects-in-visual-studio/_static/image8.png)

A legenda da caixa de seleção pode ser **Host na nuvem** ou **criar recursos remotos**, e em qualquer caso, o efeito é o mesmo. Se você deixar a caixa de seleção, o Visual Studio cria um aplicativo web no serviço de aplicativo do Azure por padrão. Você pode usar a caixa suspensa para alterar isso para um **Máquina Virtual** se você preferir. Se você ainda não entrou Azure, você será solicitado para as credenciais do Azure. Depois que você entrar, uma caixa de diálogo permite que você configure os recursos que o Visual Studio criará para o seu projeto. A ilustração a seguir mostra a caixa de diálogo para um aplicativo da web; diferentes opções aparecerão se você optar por criar uma máquina virtual.

![Definir configurações de aplicativo do Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Para obter mais informações sobre como usar esse processo para a criação de recursos do Azure, consulte [Introdução ao Azure e ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) e [criando uma máquina virtual para um site da web com o Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

O restante deste artigo fornece mais informações sobre os modelos disponíveis e suas opções. O artigo também apresenta o framework de inicialização, o layout e os temas usado nos modelos.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Modelos de projeto da Web do Visual Studio 2013

O Visual Studio usa modelos para criar projetos da web. Um modelo de projeto pode criar arquivos e pastas no novo projeto, instalar os pacotes NuGet e fornecem código de exemplo para um aplicativo de trabalho rudimentar. Modelos de implementam os padrões da web mais recentes e servem para demonstrar as práticas recomendadas sobre como usar tecnologias ASP.NET, bem como fornecer um salto iniciar sobre como criar seu próprio aplicativo.

Visual Studio 2013 fornece as seguintes opções para modelos de projeto da web para projetos que usam o .NET 4.5 ou versões posteriores do .NET framework:

- [Modelo vazio](#empty)
- [Modelo de formulários da Web](#wf)
- [Modelo MVC](#mvc)
- [Modelo de API da Web](#webapi)
- [Modelo de aplicativo de página único](#spa)
- [Modelo de serviço móvel do Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modelos do Visual Studio 2012](#vs2012)

Você também pode instalar uma extensão do Visual Studio que fornece um [Facebook modelo](#facebook).

Para obter informações sobre como criar projetos que usam o .NET 4, consulte [modelos do Visual Studio 2012](#vs2012) mais adiante neste tópico.

Para obter informações sobre como criar aplicativos ASP.NET para clientes móveis, consulte [suporte de celular no ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modelo vazio

O modelo vazio fornece os arquivos para um aplicativo web ASP.NET, como um arquivo de projeto e sem pastas mínimo (*. csproj* ou. *vbproj*) e um *Web. config* arquivo. Você pode adicionar suporte para Web Forms, MVC e/ou API da Web usando as caixas de seleção sob o **adicionar pastas e referências de núcleo:** rótulo.

Para o modelo vazio, não há opções de autenticação estão disponíveis. A funcionalidade de autenticação é implementada em aplicativos de exemplo e o modelo vazio não cria um aplicativo de exemplo.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modelo de formulários da Web

O Web Forms framework fornece os seguintes recursos que permitem que você rapidamente criar sites que sejam ricos em dados e da interface do usuário acessar recursos:

- Um designer WYSIWYG no Visual Studio.
- Controles de servidor que processam HTML e que você pode personalizar, definindo as propriedades e estilos.
- Um conjunto rico de controles de acesso a dados e exibição de dados.
- Um modelo de evento que expõe eventos que você pode programar que você programará um aplicativo cliente como WPF.
- Preservação automática do estado (dados) entre as solicitações HTTP.

Em geral, a criação de um aplicativo Web Forms requer menos esforço de programação que criar o mesmo aplicativo usando a estrutura ASP.NET MVC. No entanto, formulários da Web não é apenas para desenvolvimento rápido de aplicativos. Há muitos aplicativos comerciais complexas e estruturas criadas com base em formulários da Web.

Como uma página Web Forms e os controles na página geram automaticamente muito a marcação que é enviada para o navegador, você não tem o tipo de controle refinado sobre o HTML que oferece ASP.NET MVC. O modelo declarativo para configurar páginas e controles minimiza a quantidade de código que você precisa escrever mas oculta alguns comportamentos de HTML e HTTP. Por exemplo, nem sempre é possível especificar exatamente quais marcação pode ser gerada por um controle.

A estrutura de Web Forms não se prestam prontamente como ASP.NET MVC para práticas de desenvolvimento com base em padrões, como [desenvolvimento controlado por testes](http://en.wikipedia.org/wiki/Test-driven_development), [separação de preocupações](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversão de controle](http://en.wikipedia.org/wiki/Inversion_of_control), e [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection). Se você quiser escrever código fatorados dessa forma, você pode; não é como automático porque ele está sendo a estrutura ASP.NET MVC. O [ASP.NET Web Forms MVP](http://webformsmvp.com/) projeto mostra uma abordagem que facilitam a separação de preocupações e capacidade de teste, mantendo o rápido desenvolvimento Web Forms foi criado para fornecer. Microsoft SharePoint baseia-se no Web Forms MVP.

O modelo de Web Forms cria um aplicativo de formulários da Web de exemplo que usa [Bootstrap](#bootstrap) para fornecer recursos de design e temas responsivos. A ilustração a seguir mostra a página inicial.

![Home page do Web Forms modelo aplicativo](creating-web-projects-in-visual-studio/_static/image10.png)

Para obter mais informações sobre formulários da Web, consulte [Web Forms do ASP.NET](https://asp.net/web-forms). Para obter informações sobre o que faz o modelo de Web Forms para você, consulte [criando um aplicativo Web Forms básico usando o Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modelo MVC

ASP.NET MVC foi desenvolvido para facilitar a práticas de desenvolvimento com base em padrões, como [desenvolvimento controlado por testes](http://en.wikipedia.org/wiki/Test-driven_development), [separação de preocupações](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversão de controle](http://en.wikipedia.org/wiki/Inversion_of_control), e [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection). A estrutura incentiva separando a camada de lógica comercial de um aplicativo web da camada de apresentação. Ao dividir o aplicativo em controladores (C), modelos (M) e exibições (V), o ASP.NET MVC pode tornar mais fácil gerenciar a complexidade em aplicativos maiores.

Com o ASP.NET MVC, você trabalhar mais diretamente com HTML e HTTP que em Web Forms. Por exemplo, Web Forms automaticamente pode preservar o estado entre as solicitações HTTP, mas você precisa de código que explicitamente no MVC. A vantagem do modelo MVC é que ele permite que você assumir o controle total sobre exatamente o seu aplicativo e como ele se comporta no ambiente da web. A desvantagem é que você precisa escrever código mais.

MVC foi projetado para ser extensível, fornecendo aos desenvolvedores de energia a capacidade de personalizar a estrutura para suas necessidades de aplicativo. Além disso, o código-fonte ASP.NET MVC está disponível sob uma licença OSI.

O modelo MVC cria um aplicativo MVC 5 de exemplo que usa [Bootstrap](#bootstrap) para fornecer recursos de design e temas responsivos. A ilustração a seguir mostra a página inicial.

![Aplicativo de exemplo do MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Para obter mais informações sobre o MVC, consulte [ASP.NET MVC](https://asp.net/mvc). Para obter informações sobre como selecionar o modelo MVC 4, consulte [modelos do Visual Studio 2012](#vs2012) posteriormente neste artigo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modelo de API da Web

O modelo de API da Web cria um serviço da web de exemplo com base na API da Web, incluindo páginas de ajuda de API com base no MVC.

API da Web do ASP.NET é uma estrutura que torna mais fácil criar serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. API da Web do ASP.NET é uma plataforma ideal para criar serviços RESTful no .NET Framework.

O modelo de API da Web cria um serviço da web de exemplo. As ilustrações a seguir mostram exemplos de páginas de Ajuda.

![Página de Ajuda da API da Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Página de Ajuda da API da Web para obter API](creating-web-projects-in-visual-studio/_static/image13.png)

Para obter mais informações sobre a API da Web, consulte [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Modelo de aplicativo de página única

O modelo de aplicativo de página única (SPA) cria um aplicativo de exemplo que usa JavaScript, HTML 5 e [KnockoutJS](http://knockoutjs.com/) no cliente e API Web do ASP.NET no servidor.

A opção de autenticação apenas para o modelo do SPA é [contas de usuário individuais](#indauth).

A ilustração a seguir mostra o estado inicial do aplicativo de exemplo que cria o modelo SPA.

![Aplicativo de exemplo do SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Para obter informações sobre como criar um aplicativo usando o modelo do SPA, consulte [API da Web - serviços de autenticação externos](../../../web-api/overview/security/external-authentication-services.md).

Para obter mais informações sobre os aplicativos de única página ASP.NET e modelos SPA adicionais que usam JavaScript estruturas que não seja KnockoutJS, consulte os seguintes recursos:

- [Aplicativo de página única do ASP.NET](../../../single-page-application/index.md).
- [Noções básicas sobre os recursos de segurança no modelo SPA para RC VS2013](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Aplicativos de página única: Criar aplicativos Web modernos, capacidade de resposta com o ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modelo para Facebook

Você pode instalar um [extensão do Visual Studio que fornece um modelo para Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Este modelo cria um aplicativo de exemplo que é projetado para ser executado no site da web de Facebook. Ele se baseia no ASP.NET MVC e usa a API da Web para a funcionalidade de atualização em tempo real.

Não há opções de autenticação estão disponíveis para o modelo do Facebook porque aplicativos do Facebook executado dentro do site do Facebook e contam com a autenticação do Facebook.

Para obter mais informações sobre aplicativos ASP.NET Facebook, consulte [atualização da API do Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modelos do Visual Studio 2012

A caixa de diálogo de criação do projeto do Visual Studio 2013 web não fornece acesso a alguns modelos que estavam disponíveis no Visual Studio 2012. Se você quiser usar um desses modelos, você pode clicar no nó Visual Studio 2012 no painel esquerdo da caixa de diálogo Novo projeto do Visual Studio.

![Modelos do Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

O **Visual Studio 2012** permite que o nó escolher os seguintes modelos da web que você não tem acesso a na lista de modelos padrão para Visual Studio 2013:

- Aplicativo Web ASP.NET MVC 4
- Aplicativo Web de entidades de dados dinâmicos do ASP.NET
- Controle de servidor AJAX ASP.NET
- Extensor de controle de servidor ASP.NET AJAX
- Controle de servidor ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Inicializar em modelos de projeto da web do Visual Studio 2013

Usam os modelos de projeto do Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), uma estrutura de layout e temas criada pelo Twitter. Inicialização usa CSS3 para fornecer um design responsivo, o que significa layouts dinamicamente podem adaptar a tamanhos de janela de navegador diferente. Por exemplo, em uma janela de navegador ampla a home page criada pelo modelo de formulários da Web é semelhante a ilustração a seguir:

![Home page do Web Forms modelo aplicativo](creating-web-projects-in-visual-studio/_static/image16.png)

Deixar a janela mais estreitas e moverem as colunas organizadas horizontalmente no verticalmente:

![Organização de inicialização de colunas verticais](creating-web-projects-in-visual-studio/_static/image17.png)

Restringir a janela um pouco mais e menu superior horizontal se transforma em um ícone que você pode clicar para expandir para um menu orientação vertical:

![Ícone do menu de inicialização](creating-web-projects-in-visual-studio/_static/image18.png)

![Inicialização menu vertical](creating-web-projects-in-visual-studio/_static/image19.png)

Você também pode usar o recurso de temas da inicialização facilmente efetuar uma alteração na aparência do aplicativo. Por exemplo, você pode fazer as seguintes etapas para alterar o tema.

1. Em seu navegador, vá para [ http://Bootswatch.com ](http://Bootswatch.com), escolher um tema e, em seguida, clique em **baixar**. (Isso downloads *bootstrap.min.css* por padrão; se você deseja examinar o código de CSS, obter *bootstrap.css* em vez da versão minimizada.)
2. Copie o conteúdo do arquivo baixado de CSS.
3. No Visual Studio, crie um novo **folha de estilo** arquivo chamado *bootstrap theme.css* no *conteúdo* pasta e colar o CSS baixado de código para ele.
4. Abra *aplicativo\_Start/Bundle.config* e alterar *bootstrap.css* para *theme.css inicialização*.

Execute o projeto novamente e o aplicativo tem uma nova aparência. A ilustração a seguir mostra o efeito do tema Amelia:

![Tema de Amelia Bootstrap](creating-web-projects-in-visual-studio/_static/image20.png)

Vários temas Bootstrap estão disponíveis, versões gratuitas e premium. Inicialização também oferece uma ampla variedade de componentes de interface do usuário, como [suspensas](http://twitter.github.io/bootstrap/components.html#dropdowns), [botão grupos](http://twitter.github.io/bootstrap/components.html#buttonGroups), e [ícones](http://twitter.github.io/bootstrap/base-css.html#images). Para obter mais informações sobre a inicialização, consulte [o site Bootstrap](http://twitter.github.io/bootstrap/).

Se você usar o Web Forms designer no Visual Studio, observe que o designer não dá suporte a CSS3, portanto ela não mostra precisamente todos os efeitos de temas de inicialização ou alterações de layout responsivo. No entanto, as páginas Web Forms exibirá corretamente quando exibidos em um navegador.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Adicionando suporte para estruturas adicionais

Quando você seleciona um modelo, a caixa de seleção para o framework(s) usados pelo modelo é selecionada automaticamente. Por exemplo, se você selecionar o **Web Forms** modelo, o **Web Forms** caixa de seleção está marcada e você não é possível desmarcá-la.

![Selecione um modelo](creating-web-projects-in-visual-studio/_static/image21.png)

![Adicionar estruturas](creating-web-projects-in-visual-studio/_static/image22.png)

Você pode selecionar a caixa de seleção para uma estrutura que não está incluída no modelo para adicionar suporte para esse framework quando o projeto é criado. Por exemplo, para habilitar o uso de Web Forms *. aspx* páginas depois de selecionar o modelo MVC, selecione o **Web Forms** caixa de seleção. Ou para habilitar o MVC quando você estiver usando o modelo de Web Forms, clique o **MVC** caixa de seleção. Adicionando um framework habilita o suporte de tempo de design, bem como tempo de execução. Por exemplo, se você adicionar o suporte do MVC para um projeto de formulários da Web, você poderá Scaffold controladores e exibições.

Se você combinar Web Forms e MVC em um projeto e habilite [URLs amigáveis](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) em Web Forms, pode haver inesperado de roteamento problemas onde uma URL tem vários destinos possíveis. As rotas que são definidas primeiro terá precedência. Por exemplo, se você tiver um `Home` controlador e uma *aspx* página, o `http://contoso.com/home` vai para URL *aspx* se você chamar o `EnableFriendlyUrls` método antes de chamar o `MapRoute`método *RouteConfig.cs*, ou a mesma URL para ir para a exibição padrão para o `Home` controlador se você chamar `MapRoute` antes de `EnableFriendlyUrls`.

Adicionar uma estrutura não adiciona qualquer funcionalidade do aplicativo de exemplo. Por exemplo, se você adicionar Web Forms suporte depois de selecionar o modelo MVC, não *Default.aspx* arquivo home page é criado. Somente as pastas, arquivos e referências necessárias para oferecer suporte a estrutura são adicionadas. Por esse motivo, adicionar estruturas não altera as opções de autenticação, que são implementadas pelo código em aplicativos de exemplo criado pelos modelos. Por exemplo, se você selecionar o modelo vazio e adiciona formulários da Web ou MVC suportam o **configurar autenticação** botão ainda será desabilitado.

As seções a seguir descrevem brevemente o efeito de cada caixa de seleção.

### <a name="add-web-forms-support"></a>Adicionar suporte de formulários da Web

Cria vazio *aplicativo\_dados* e *modelos* pastas e um *global. asax* arquivo. Eles já são criados por todos os modelos que não seja o modelo vazio, para que marcar a caixa de seleção de formulários da Web não faz diferença para outros modelos.

O modelo de Web Forms permite URLs amigáveis, por padrão, mas quando você adiciona que suporte ao Web Forms para outros modelos, marcando a caixa de seleção de Web Forms que URLs amigáveis não são habilitadas automaticamente.

### <a name="add-mvc-support"></a>Adicionar suporte MVC

Instala os pacotes MVC Razor e WebPages NuGet, cria vazio *aplicativo\_dados*, *controladores*, *modelos*, e *exibições*pastas, cria *aplicativo\_iniciar* pasta com *RouteConfig.cs* de arquivos e cria *global. asax* arquivo.

### <a name="add-web-api-support"></a>Adicionar suporte a API da Web

Instala os pacotes WebApi e NuGet newtonsoft. JSON, cria vazio *aplicativo\_dados*, *controladores*, e *modelos* pastas, cria  *Aplicativo\_iniciar* pasta com *WebApiConfig.cs* de arquivos e cria *global. asax* arquivo.

<a id="auth"></a>
## <a name="authentication-methods"></a>Métodos de autenticação

Visual Studio 2013 oferece várias opções de autenticação para os modelos de Web Forms, MVC e API da Web:

- [Sem autenticação](#noauth)
- [Contas de usuário individuais](#indauth) (identidade do ASP.NET, anteriormente conhecido como associação do ASP.NET)
- [Contas organizacionais](#orgauth) (Windows Server Active Directory ou Active Directory do Azure)
- [Autenticação do Windows](#winauth) (Intranet)

![Configurar a caixa de diálogo de autenticação](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Sem autenticação

Se você selecionar **sem autenticação**, o aplicativo de exemplo não conterá nenhuma páginas da web para efetuar login, não interface do usuário indicando que está conectado, não classes de entidade para um banco de dados de associação e nenhuma cadeia de caracteres de conexão para um banco de dados de associação.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Contas de usuário individuais

Se você selecionar **contas de usuário individuais**, o aplicativo de exemplo será configurado para usar a identidade do ASP.NET (anteriormente conhecido como associação do ASP.NET) para autenticação do usuário. Identidade do ASP.NET permite que um usuário registrar uma conta, criando um nome de usuário e uma senha no site ou entrando com provedores sociais, como Facebook, Google, Microsoft Account ou Twitter. O repositório de dados padrão para perfis de usuário na identidade do ASP.NET é um banco de dados LocalDB do SQL Server, você pode implantar para o SQL Server ou banco de dados do SQL Azure para o site de produção.

No Visual Studio 2013, esses recursos são o mesmo do Visual Studio 2012, mas o código subjacente para o sistema de associação do ASP.NET foi recriado. Vantagens da nova base de código incluem o seguinte:

- O novo sistema de associação é baseado no [OWIN](http://owin.org/) em vez do módulo de autenticação de formulários do ASP.NET. Isso significa que você pode usar o mesmo mecanismo de autenticação, se você estiver usando o Web Forms ou MVC no IIS, ou estiver hospedagem própria API da Web ou SignalR.
- O novo banco de dados de associação é gerenciado pelo Entity Framework Code First e todas as tabelas são representadas pelas classes de entidade que você pode modificar. Isso significa que você pode personalizar facilmente o esquema de banco de dados e web relacionados ao perfil da interface do usuário de acordo com suas necessidades, e você pode facilmente implantar as atualizações usando migrações do Code First.

O novo sistema de associação foi implementado automaticamente os novos modelos, e pode ser implementado manualmente em qualquer projeto que tem como destino .NET 4.5 ou posterior.

Identidade do ASP.NET é uma boa opção se você estiver criando um site da Internet que é principalmente para clientes externos. Se sua organização usa o Active Directory ou Office 365 e você deseja criar um projeto que habilita logon único para funcionários e parceiros de negócios, o **contas organizacionais** opção pode ser uma opção melhor.

Para obter mais informações sobre a opção de contas de usuário individuais, consulte os seguintes recursos:

- [www.asp.net/identity](../../../identity/index.md). Documentação sobre a identidade do ASP.NET no site da web ASP.NET.
- [Criar um aplicativo ASP.NET MVC 5 com o Facebook e Google OAuth2 e logon do OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Também mostra como personalizar dados de perfil de usuário.
- [API da Web - serviços de autenticação externa](../../../web-api/overview/security/external-authentication-services.md)
- [Adicionar Logins externos ao seu aplicativo ASP.NET no Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Contas organizacionais

Se você selecionar **contas organizacionais**, o aplicativo de exemplo será configurado para usar o Windows Identity Foundation (WIF) para autenticação com base em contas de usuário no Azure Active Directory (AD do Azure, que inclui o Office 365) ou Windows Server Active Directory. Para obter mais informações, consulte [opções de autenticação de conta organizacional](#orgauthoptions) mais adiante neste tópico.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticação do Windows

Se você selecionar **autenticação do Windows**, o aplicativo de exemplo será configurado para usar o módulo do IIS de autenticação do Windows para autenticação. O aplicativo exibirá a ID de usuário e domínio da conta do computador local que está conectada ao Windows, mas não incluem o registro de usuário ou log-na interface do usuário ou do Active directory. Essa opção destina-se a sites da Intranet.

Como alternativa, você pode criar um site de Intranet que usa a autenticação do AD, escolhendo o [opção sob contas organizacionais locais](#orgauthonprem). A opção de local usa o Windows Identity Foundation (WIF) em vez do módulo de autenticação do Windows. Algumas etapas adicionais são necessárias para configurar a opção de local, mas o WIF habilita os recursos que não estão disponíveis com o módulo de autenticação do Windows. Por exemplo, com WIF você pode configurar o acesso do aplicativo no Active Directory e consultar dados de diretório.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opções de autenticação de conta organizacional

O **configurar autenticação** caixa de diálogo fornece várias opções para o Azure Active Directory (AD do Azure, que inclui o Office 365) ou autenticação de conta do Windows Server Active Directory (AD):

- [Nuvem - única organização](#orgauthsingle) (Azure AD ou AD usando a integração de diretórios com o AD do Azure)
- [Nuvem - organização de várias](#orgauthmulti) (Azure AD ou AD usando a integração de diretórios com o AD do Azure)
- [Local](#orgauthonprem) (AD)

Se você deseja tentar uma das opções do AD do Azure, mas não tiver uma conta, [clique aqui para se inscrever para uma conta do AD do Azure](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Se você escolher uma das opções do AD do Azure, seu projeto requer um banco de dados e você precisa entrar uma conta de administrador global para seu locatário do AD do Azure. Insira o nome e a senha para uma conta organizacional (por exemplo, admin@contoso.onmicrosoft.com) que tenha permissões administrativas para o seu locatário do AD do Azure.
> 
> **Não insira as credenciais para uma conta da Microsoft (por exemplo, contoso@hotmail.com) na caixa de diálogo de logon.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Nuvem - autenticação de organização único

![Autenticação de organização único](creating-web-projects-in-visual-studio/_static/image24.png)

Escolha esta opção se você deseja habilitar a autenticação de contas de usuário que são definidos no AD do Azure uma [locatário](https://technet.microsoft.com/library/jj573650.aspx). Por exemplo, o site seja contoso.com e ele será disponibilizado para os funcionários da empresa Contoso que estão no locatário contoso.onmicrosoft.com. Você não poderá configurar o AD do Azure para permitir que os usuários de outros locatários para acessar o aplicativo.

#### <a name="domain"></a>Domain

Digite o domínio do AD do Azure que você deseja configurar o aplicativo, por exemplo: `contoso.onmicrosoft.com`. Se você tiver um [domínio personalizado](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), como `contoso.com` em vez de `contoso.onmicrosoft.com`, que pode ser inserido aqui.

#### <a name="access-level"></a>Nível de acesso

Se o aplicativo precisar consultar ou atualizar as informações de diretório usando a API do Graph, escolha **o logon único, ler dados do diretório** ou **o logon único, ler e gravar dados do diretório**. Caso contrário, escolha **Single Sign-On**. Para obter mais informações, consulte [níveis de acesso do aplicativo](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) e [usando a API do Graph para consultar o AD do Azure](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI da ID do aplicativo

Por padrão, o modelo cria um URI de ID de aplicativo para você, acrescentando o nome do projeto ao domínio do AD do Azure. Por exemplo, se o nome do projeto é `Example` e o domínio é `contoso.onmicrosoft.com`, o aplicativo se torna o URI de ID `https://contoso.onmicrosoft.com/Example`. Se você quiser especificar manualmente o URI de ID do aplicativo, expanda o **mais opções** seção e insira o URI de ID do aplicativo na caixa de texto. O aplicativo de URI de ID deve começar com `https://`.

Por padrão, se um aplicativo que já foi provisionado no AD do Azure tem o mesmo aplicativo URI de ID como o Visual Studio está usando para o projeto, o projeto será conectado ao aplicativo existente em vez de provisionar um novo. Se quiser que um novo aplicativo a ser provisionado nesse caso, desmarque o **substituir a entrada do aplicativo se já existir uma com a mesma ID** caixa de seleção.

Se o **substituir** caixa de seleção é desmarcada e Visual Studio localiza um aplicativo existente com o mesmo URI de ID de aplicativo, ele cria um novo URI acrescentando um número para o URI que ele estava prestes a ser usado. Por exemplo, suponha que o nome do projeto é `Example`, você deixar a caixa de texto em branco, se você desmarcar a **substituir** caixa de seleção e o locatário do AD do Azure já tiver um aplicativo com o URI `https://contoso.onmicrosoft.com/Example`. Nesse caso, um novo aplicativo será configurado com um aplicativo como o URI de ID `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Provisionamento do aplicativo no AD do Azure

Para configurar o aplicativo no AD do Azure ou conectar-se o projeto para um aplicativo existente, o Visual Studio precisa das credenciais de um administrador global para o domínio. Quando você clica em **Okey** no **configurar autenticação** caixa de diálogo, você será solicitado para o nome de usuário e a senha de um administrador global para o domínio especificado. Posteriormente, quando você clicar em **criar projeto** no **novo projeto ASP.NET** caixa de diálogo, o Visual Studio configura o aplicativo no AD do Azure. Observe que, como parte desse processo Visual Studio insere valores de segredo do cliente no arquivo Web. config que expira um ano após a criação.

Para obter informações sobre como criar aplicativos que usam **nuvem - única organização** autenticação, consulte os seguintes recursos:

- [Autenticação do Azure](../2012/windows-azure-authentication.md)
- [Adicionar logon ao seu aplicativo Web usando o Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Desenvolvendo aplicativos do ASP.NET com o Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteger a API da Web ASP.NET com o Azure AD e componentes do Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Os tutoriais ainda não foram atualizados para o Visual Studio 2013; alguns dos quais os tutoriais direcioná-lo para fazer manualmente é automatizada no Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Nuvem - autenticação de organização de várias

![Autenticação de organização múltipla](creating-web-projects-in-visual-studio/_static/image25.png)

Escolha esta opção se você deseja habilitar a autenticação de contas de usuário que são definidos no AD do Azure vários [locatários](https://technet.microsoft.com/library/jj573650.aspx). Por exemplo, o site seja contoso.com e ele será disponibilizado para os funcionários da empresa Contoso que estão no locatário contoso.onmicrosoft.com e funcionários da empresa Fabrikam no locatário fabrikam.onmicrosoft.com.

As configurações que você inserir e o etapa de provisionamento de aplicativo são semelhantes às [autenticação única organização](#orgauthsingle).

Para obter informações sobre como criar aplicativos que usam **nuvem - organização de várias** autenticação, consulte os seguintes recursos:

- [Integração de aplicativo Web fácil com o Active Directory do Azure, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) no blog da equipe do Active Directory.
- [Desenvolvendo aplicativos da Web de multilocatário com o Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) tutorial. O tutorial ainda não foi atualizado para Visual Studio 2013; alguns dos quais o tutorial direciona você faça manualmente é automatizada no Visual Studio 2013.
- [Você precisa inscrever-se com seu próprio aplicativo de ASP.NET várias organizações para que você possa entrar](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog Vittorio Bertocci que explica como resolver uma pessoas problema comum encontrar quando criar um projeto que usa a autenticação de organização vários.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticação organizacional local

![Autenticação organizacional local](creating-web-projects-in-visual-studio/_static/image26.png)

Escolha esta opção se você deseja habilitar a autenticação de contas de usuário que são definidas no Windows Server Active Directory (AD), e você não quiser usar o AD do Azure. Você pode usar essa opção para criar um site de Intranet ou um site da Internet. Para um site da Internet, use os serviços de Federação do Active Directory (ADFS) para fornecer acesso ao AD. Para obter mais informações, consulte [usar a opção de autenticação organizacional local (ADFS) com ASP.NET no Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Para um site de Intranet, como alternativa você pode escolher [autenticação do Windows](#winauth) em vez desta opção. Para a opção de autenticação do Windows, você não precisa fornecer uma URL do documento de metadados. No entanto, a autenticação do Windows não lhe confere a capacidade para controlar o acesso do aplicativo no Active Directory ou para consultar dados de diretório.

#### <a name="on-premises-authority"></a>Autoridade local

Insira uma URL que aponta para o documento de metadados. O documento de metadados contém as coordenadas da autoridade. O aplicativo utilizará essas coordenadas para impulsionar o fluxo de logon da web.

#### <a name="application-id-uri"></a>URI da ID do aplicativo

Forneça um URI exclusivo que AD pode usar para identificar esse aplicativo, ou deixe em branco para permitir que o Visual Studio crie uma.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Próximas etapas

Este documento fornece ajuda básica para criar um novo projeto da web ASP.NET no Visual Studio 2013. Para obter mais informações sobre como usar o Visual Studio para desenvolvimento na web, consulte [ https://www.asp.net/visual-studio/ ](../../index.md).
