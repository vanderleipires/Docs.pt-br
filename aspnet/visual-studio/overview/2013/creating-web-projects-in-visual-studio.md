---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Criação de projetos da Web do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: Este tópico explica as opções para a criação de projetos web ASP.NET no Visual Studio 2013 com atualização 3 aqui estão alguns dos novos recursos para o c de desenvolvimento da web...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 3d96d796d22c3511fedc45c024274300143b119b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825083"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Criando projetos Web do ASP.NET no Visual Studio 2013
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Este tópico explica as opções para a criação de projetos web ASP.NET no Visual Studio 2013 com atualização 3
> 
> Aqui estão alguns dos novos recursos para desenvolvimento da web em comparação comparada versões anteriores do Visual Studio:
> 
> - Uma interface do usuário simple para a criação de projetos que oferecem [dar suporte a várias estruturas de ASP.NET](#add) (Web Forms, MVC e API da Web).
> - [O ASP.NET Identity](#indauth), um novo sistema de associação do ASP.NET que funciona da mesma em todas as estruturas do ASP.NET e funciona com o software que não seja o IIS de hospedagem na web.
> - O uso de [Bootstrap](#bootstrap) para fornecer recursos dinâmicos de design e temas.
> - Novos recursos para formulários da Web que eram oferecidos apenas para MVC, como [criação do projeto de teste automático](#testproj) e uma [modelo de site de Intranet](#winauth).
> 
> Para obter informações sobre como criar projetos da web para serviços de nuvem do Azure ou serviços móveis do Azure, consulte [Introdução aos serviços de nuvem do Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) e [criando um App Leaderboard com .NET de serviços móveis do Azure Back-end](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Pré-requisitos

Este artigo se aplica ao [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) com [atualização 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) instalado.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projetos de aplicativos Web versus projetos de site

ASP.NET permite que você escolha entre dois tipos de projetos da web: *projetos de aplicativo web* e *projetos de site*. Recomendamos que os projetos de aplicativos web para novos desenvolvimentos, e este artigo se aplica somente a projetos de aplicativos web. Para obter mais informações, consulte [projetos de aplicativos Web versus projetos de Site da Web no Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) no site do MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Visão geral da criação do projeto de aplicativo web

As etapas a seguir mostram como criar um projeto da web:

1. Clique em **novo projeto** na **iniciar** página ou nos **arquivo** menu.
2. No **novo projeto** caixa de diálogo, clique em **Web** no painel esquerdo e **aplicativo Web ASP.NET** no painel central.

    ![Caixa de diálogo Novo Projeto](creating-web-projects-in-visual-studio/_static/image1.png)

    Você pode escolher **Cloud** no painel esquerdo para criar um [serviço de nuvem do Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [serviço móvel do Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), ou [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Este tópico não aborda esses modelos.
3. No painel direito, clique no **adicionar Application Insights ao projeto** caixa de seleção se desejar que o monitoramento da integridade e uso para seu aplicativo. Para obter mais informações, consulte [monitorar o desempenho em aplicativos da web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Especificar projeto **nome**, **local**e outras opções e depois clique em **Okey**.

    O **novo projeto ASP.NET** caixa de diálogo é exibida.

    ![Caixa de diálogo Novo Projeto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Clique em um modelo.

    ![Selecione um modelo](creating-web-projects-in-visual-studio/_static/image3.png)
6. Se você quiser adicionar suporte a estruturas adicionais não incluídos no modelo, clique na caixa de seleção apropriada. (No exemplo mostrado, você poderia adicionar MVC e/ou API da Web a um projeto de Web Forms.)

    ![Adicionar estruturas](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Se você quiser adicionar um projeto de teste de unidade, clique em **adicionar testes de unidade**.

    ![Adicionar testes de unidade](creating-web-projects-in-visual-studio/_static/image5.png)
8. Se você quiser um método de autenticação diferente do que o modelo fornece por padrão, clique em **alterar autenticação**.

    ![Configurar o botão de autenticação](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Configurar a caixa de diálogo de autenticação](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Criar um aplicativo web ou máquina virtual no Azure

Visual Studio inclui recursos que tornam mais fácil trabalhar com os serviços do Azure para hospedar aplicativos web. Por exemplo, você pode fazer o seguinte diretamente do IDE do Visual Studio:

- Criar e gerenciar aplicativos web ou máquinas virtuais que tornar seu aplicativo disponível pela Internet.
- Exibir os logs criados pelo aplicativo conforme ele é executado na nuvem.
- Execute no modo de depuração remotamente enquanto o aplicativo é executado na nuvem.
- Viiew e gerenciar outros serviços do Azure, como bancos de dados SQL.

Você pode [criar uma conta do Azure](https://www.windowsazure.com/pricing/free-trial/) que inclui serviços básicos, como aplicativos web gratuitamente, e se você for assinante do MSDN, você pode [ativar os benefícios de](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) que lhe dar créditos Azure adicionais serviços. 

Por padrão o **novo projeto ASP.NET** caixa de diálogo permite que você crie um aplicativo web ou máquina virtual para um novo projeto web. Se você não quiser criar um novo aplicativo web ou máquina virtual, desmarque a **hospedar na nuvem** caixa de seleção.

![Criar recursos remotos](creating-web-projects-in-visual-studio/_static/image8.png)

A legenda da caixa de seleção pode ser **hospedar na nuvem** ou **criar recursos remotos**, e em ambos os casos, o efeito é o mesmo. Se você deixar a caixa de seleção marcada, o Visual Studio cria um aplicativo web no serviço de aplicativo do Azure por padrão. Você pode usar a caixa suspensa para alterar isso para um **Máquina Virtual** se você preferir. Se você ainda não entrou Azure, você será solicitado para credenciais do Azure. Depois de entrar, uma caixa de diálogo permite que você configurar os recursos que o Visual Studio cria para seu projeto. A ilustração a seguir mostra a caixa de diálogo para um aplicativo web; diferentes opções aparecerão se você optar por criar uma máquina virtual.

![Definir configurações de aplicativo do Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Para obter mais informações sobre como usar esse processo para a criação de recursos do Azure, consulte [Introdução ao Azure e ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) e [criar uma máquina virtual para um site da web com o Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

O restante deste artigo fornece mais informações sobre os modelos disponíveis e suas opções. O artigo também apresenta o framework de Bootstrap, o layout e temas usado nos modelos.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Modelos de projeto Web do Visual Studio 2013

Visual Studio usa modelos para criar projetos da web. Um modelo de projeto pode criar arquivos e pastas no novo projeto, instalar os pacotes NuGet e fornecem código de exemplo para um aplicativo funcional rudimentar. Modelos de implementam os padrões da web mais recente e têm como objetivo demonstrar as práticas recomendadas para saber como usar as tecnologias do ASP.NET, bem como oferecem a você um salto iniciar sobre como criar seu próprio aplicativo.

Visual Studio 2013 fornece as seguintes opções para modelos de projeto web para projetos que usam o .NET 4.5 ou versões posteriores do .NET framework:

- [Modelo vazio](#empty)
- [Modelo de formulários da Web](#wf)
- [Modelo do MVC](#mvc)
- [Modelo de API da Web](#webapi)
- [Modelo de aplicativo de página único](#spa)
- [Modelo de serviço móvel do Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modelos do Visual Studio 2012](#vs2012)

Você também pode instalar uma extensão do Visual Studio que fornece um [Facebook modelo](#facebook).

Para obter informações sobre como criar projetos que usam o .NET 4, consulte [modelos do Visual Studio 2012](#vs2012) mais adiante neste tópico.

Para obter informações sobre como criar aplicativos do ASP.NET para clientes móveis, consulte [Mobile oferecem suporte no ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modelo vazio

O modelo vazio fornece as pastas de mínimo vazia e os arquivos para um aplicativo web ASP.NET, como um arquivo de projeto (*. csproj* ou. *vbproj*) e uma *Web. config* arquivo. Você pode adicionar suporte para Web Forms, MVC e/ou API da Web usando as caixas de seleção sob o **adicionar pastas e os principais referências para:** rótulo.

Para o modelo vazio sem opções de autenticação estão disponíveis. Funcionalidade de autenticação é implementada em aplicativos de exemplo e o modelo vazio não cria um aplicativo de exemplo.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modelo de formulários da Web

O Web Forms framework fornece os seguintes recursos que permitem que você rapidamente crie sites da web que sejam ricos em dados e da interface do usuário acessar recursos:

- Um designer WYSIWYG do Visual Studio.
- Controles de servidor que processam HTML e que você pode personalizar, definindo as propriedades e estilos.
- Um conjunto rico de controles de acesso a dados e exibição de dados.
- Um modelo de evento que expõe eventos que você pode programar como você faria programar um aplicativo cliente, como o WPF.
- Preservação automática do estado (dados) entre as solicitações HTTP.

Em geral, a criação de um aplicativo Web Forms requer menos esforço de programação de criar o mesmo aplicativo usando a estrutura ASP.NET MVC. No entanto, formulários da Web não é apenas para desenvolvimento rápido de aplicativos. Há muitos aplicativos comerciais complexos e estruturas criadas com base em formulários da Web.

Como uma página Web Forms e os controles na página geram automaticamente grande parte da marcação que é enviada ao navegador, você não tem o tipo de controle refinado sobre o HTML que o ASP.NET MVC oferece. O modelo declarativo para configurar páginas e controles minimiza a quantidade de código, você precisa escrever, mas oculta alguns comportamentos de HTML e HTTP. Por exemplo, não é sempre possível especificar exatamente quais marcações podem ser geradas por um controle.

A estrutura de Web Forms não se prestam tão prontamente quanto o ASP.NET MVC para práticas de desenvolvimento baseada em padrões, como [desenvolvimento orientado por testes](http://en.wikipedia.org/wiki/Test-driven_development), [separação de preocupações](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversão de controle](http://en.wikipedia.org/wiki/Inversion_of_control), e [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection). Se você quiser escrever código fatorados dessa forma, você poderá; não é tão automático como ele está na estrutura do ASP.NET MVC. O [MVP em ASP.NET Web Forms](http://webformsmvp.com/) projeto mostra uma abordagem que facilita a separação de preocupações e capacidade de teste, mantendo o rápido desenvolvimento formulários da Web foi criado para oferecer. Microsoft SharePoint baseia-se nos Web Forms MVP.

O modelo de formulários da Web cria um aplicativo de formulários da Web de exemplo que usa [Bootstrap](#bootstrap) para fornecer recursos dinâmicos de design e temas. A ilustração a seguir mostra a home page.

![Home page do Web Forms modelo aplicativo](creating-web-projects-in-visual-studio/_static/image10.png)

Para obter mais informações sobre Web Forms, consulte [ASP.NET Web Forms](https://asp.net/web-forms). Para obter informações sobre o que o modelo de Web Forms faz para você, consulte [criando um aplicativo Web Forms básico usando o Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modelo do MVC

ASP.NET MVC foi desenvolvido para facilitar as práticas de desenvolvimento baseada em padrões, como [desenvolvimento orientado por testes](http://en.wikipedia.org/wiki/Test-driven_development), [separação de preocupações](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversão de controle](http://en.wikipedia.org/wiki/Inversion_of_control), e [injeção de dependência](http://en.wikipedia.org/wiki/Dependency_injection). A estrutura incentiva a separar a camada de lógica comercial de um aplicativo web da camada de apresentação. Ao dividir o aplicativo em modelos (M), as exibições (V) e controladores (C), ASP.NET MVC pode tornar mais fácil gerenciar a complexidade em aplicativos maiores.

Com o ASP.NET MVC, você trabalha mais diretamente com HTML e HTTP do que em formulários da Web. Por exemplo, Web Forms automaticamente pode preservar o estado entre as solicitações HTTP, mas você tem para o código que explicitamente no MVC. A vantagem do modelo MVC é que ele permite que você tenha controle completo sobre exatamente o que seu aplicativo está fazendo e como ele se comporta no ambiente de web. A desvantagem é que você precisa escrever mais código.

MVC foi projetado para ser extensível, oferecendo aos desenvolvedores do power a capacidade de personalizar a estrutura para suas necessidades de aplicativo. Além disso, o código-fonte ASP.NET MVC está disponível sob uma licença OSI.

O modelo MVC cria um aplicativo MVC 5 de exemplo que usa [Bootstrap](#bootstrap) para fornecer recursos dinâmicos de design e temas. A ilustração a seguir mostra a home page.

![Aplicativo de exemplo do MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Para obter mais informações sobre o MVC, consulte [ASP.NET MVC](https://asp.net/mvc). Para obter informações sobre como selecionar o modelo MVC 4, consulte [modelos do Visual Studio 2012](#vs2012) mais adiante neste artigo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modelo de API da Web

O modelo de API da Web cria um serviço web de exemplo com base na API Web, incluindo páginas de ajuda de API com base no MVC.

API Web ASP.NET é uma estrutura que torna mais fácil criar serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. API Web ASP.NET é uma plataforma ideal para criar serviços RESTful no .NET Framework.

O modelo de API da Web cria um serviço web de exemplo. As ilustrações a seguir mostram exemplos de páginas de Ajuda.

![Página de Ajuda da API da Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Página de ajuda de API da Web para API obter](creating-web-projects-in-visual-studio/_static/image13.png)

Para obter mais informações sobre a API da Web, consulte [API Web ASP.NET](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Modelo de aplicativo de página única

O modelo de aplicativo de página única (SPA) cria um aplicativo de exemplo que usa JavaScript, HTML 5, e [KnockoutJS](http://knockoutjs.com/) no cliente e API Web do ASP.NET no servidor.

É a opção de autenticação apenas para o modelo SPA [contas de usuário individuais](#indauth).

A ilustração a seguir mostra o estado inicial do aplicativo de exemplo que cria o modelo do SPA.

![Aplicativo de exemplo do SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Para obter informações sobre como criar um aplicativo usando o modelo do SPA, consulte [API Web - serviços de autenticação externos](../../../web-api/overview/security/external-authentication-services.md).

Para obter mais informações sobre aplicativos de página única do ASP.NET e modelos SPA adicionais que usam estruturas de JavaScript que não sejam KnockoutJS, consulte os seguintes recursos:

- [Aplicativo de página única ASP.NET](../../../single-page-application/index.md).
- [Noções básicas sobre os recursos de segurança no modelo do SPA para VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Aplicativos de página única: Compilar aplicativos Web dinâmicos e modernos com o ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modelo para Facebook

Você pode instalar um [extensão do Visual Studio que fornece um modelo para Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Este modelo cria um aplicativo de exemplo é projetado para ser executado dentro do site da web de Facebook. Ele se baseia no ASP.NET MVC e usa a API da Web para a funcionalidade de atualização em tempo real.

Não há opções de autenticação estão disponíveis para o modelo do Facebook como aplicativos do Facebook são executados dentro do site do Facebook e dependem da autenticação do Facebook.

Para obter mais informações sobre aplicativos do Facebook do ASP.NET, consulte [atualizando a API do Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modelos do Visual Studio 2012

A caixa de diálogo de criação do projeto do Visual Studio 2013 web não fornece acesso a alguns modelos que estavam disponíveis no Visual Studio 2012. Se você quiser usar um desses modelos, você pode clicar no nó do Visual Studio 2012 no painel esquerdo da caixa de diálogo Novo projeto do Visual Studio.

![Modelos do Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

O **Visual Studio 2012** permite de nó que você escolha os seguintes modelos da web que você não tiver acesso a na lista de modelos padrão para o Visual Studio 2013:

- Aplicativo Web ASP.NET MVC 4
- Aplicativo Web de entidades de dados dinâmicos do ASP.NET
- Controle de servidor AJAX ASP.NET
- Extensor de controle de servidor AJAX ASP.NET
- Controle de servidor ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Inicializar em modelos de projeto da web do Visual Studio 2013

Usam os modelos de projeto do Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), uma estrutura de layout e temas criada por meio do Twitter. O Bootstrap usa CSS3 para fornecer design responsivo, o que significa layouts dinamicamente podem adaptar para tamanhos de janela de navegador diferente. Por exemplo, em uma janela de navegador amplas a home page criada pelo modelo de Web Forms se parece com a ilustração a seguir:

![Home page do Web Forms modelo aplicativo](creating-web-projects-in-visual-studio/_static/image16.png)

Deixar a janela mais estreita, e as colunas organizadas na horizontal moverem para a organização vertical:

![Organização de colunas verticais Bootstrap](creating-web-projects-in-visual-studio/_static/image17.png)

Restringir a janela de um pouco mais e menu superior horizontal se transforma em um ícone que você pode clicar para expandir em um menu orientado verticalmente:

![Ícone do menu de inicialização](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu vertical Bootstrap](creating-web-projects-in-visual-studio/_static/image19.png)

Você também pode usar o recurso de temas do Bootstrap para facilmente fazer uma alteração na aparência do aplicativo. Por exemplo, você pode fazer as seguintes etapas para alterar o tema.

1. No seu navegador, acesse [ http://Bootswatch.com ](http://Bootswatch.com), escolha um tema e, em seguida, clique em **baixar**. (Isso baixa *Bootstrap* por padrão; se você quiser examinar o código CSS, obterá *Bootstrap* em vez da versão minificada.)
2. Copie o conteúdo do arquivo baixado de CSS.
3. No Visual Studio, crie um novo **folha de estilos** arquivo chamado *bootstrap-Theme* no *conteúdo* pasta e colar o CSS baixado código nele.
4. Abra *App\_Start/Bundle.config* e altere *Bootstrap* para *bootstrap-Theme*.

Execute o projeto novamente e o aplicativo tem uma nova aparência. A ilustração a seguir mostra o efeito do tema Amelia:

![Tema do Bootstrap Amelia](creating-web-projects-in-visual-studio/_static/image20.png)

Muitos temas do Bootstrap estão disponíveis, as versões gratuitas e premium. O Bootstrap também oferece uma ampla variedade de componentes de interface do usuário, como [menus suspensos](http://twitter.github.io/bootstrap/components.html#dropdowns), [botão grupos](http://twitter.github.io/bootstrap/components.html#buttonGroups), e [ícones](http://twitter.github.io/bootstrap/base-css.html#images). Para obter mais informações sobre o Bootstrap, consulte [o site de Bootstrap](http://twitter.github.io/bootstrap/).

Se você usar o designer de formulários da Web no Visual Studio, observe que o designer não dá suporte a CSS3, portanto ela não mostra precisamente todos os efeitos de temas do Bootstrap ou alterações de layout dinâmico. No entanto, as páginas de Web Forms serão exibido corretamente quando exibidos com um navegador.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Adicionando suporte a estruturas adicionais

Quando você seleciona um modelo, a caixa de seleção para as estruturas usadas pelo modelo é selecionada automaticamente. Por exemplo, se você selecionar o **Web Forms** modelo, o **Web Forms** caixa de seleção está marcada e você não pode limpá-lo.

![Selecione um modelo](creating-web-projects-in-visual-studio/_static/image21.png)

![Adicionar estruturas](creating-web-projects-in-visual-studio/_static/image22.png)

Você pode selecionar a caixa de seleção para uma estrutura que não está incluída no modelo para adicionar suporte para essa estrutura quando o projeto é criado. Por exemplo, para habilitar o uso de Web Forms *. aspx* páginas depois de selecionar o modelo MVC, selecione o **Web Forms** caixa de seleção. Ou para permitir que o MVC quando você estiver usando o modelo de formulários da Web, clique no **MVC** caixa de seleção. Adicionando um framework habilita o suporte de tempo de design, bem como tempo de execução. Por exemplo, se você adicionar o suporte MVC para um projeto de Web Forms, você poderá criar o scaffolding de controladores e exibições.

Se você combinar os Web Forms e MVC em um projeto e habilite [URLs amigáveis](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) em Web Forms, pode haver inesperado de roteamento de problemas em que uma URL tem vários destinos possíveis. As rotas que são definidas primeiro terá precedência. Por exemplo, se você tiver um `Home` controlador e uma *home. aspx* página, o `http://contoso.com/home` vai para URL *aspx* se você chamar o `EnableFriendlyUrls` método antes de chamar o `MapRoute`método na *RouteConfig.cs*, ou a mesma URL de ir para a exibição padrão para seus `Home` controlador se você chamar `MapRoute` antes de `EnableFriendlyUrls`.

Adicionar uma estrutura não adiciona qualquer funcionalidade do aplicativo de exemplo. Por exemplo, se você adicionar formulários da Web suporte depois de selecionar o modelo MVC, não *default. aspx* arquivo home page é criado. Somente as pastas, arquivos e as referências necessárias para dar suporte a estrutura são adicionadas. Por esse motivo, adicionar estruturas não altera as opções de autenticação, que são implementadas pelo código em aplicativos de exemplo criados pelos modelos. Por exemplo, se você selecionar o modelo vazio e adiciona Web Forms ou MVC suporte a **configurar a autenticação** botão ainda será desabilitado.

As seções a seguir descrevem brevemente o efeito de cada caixa de seleção.

### <a name="add-web-forms-support"></a>Adicionar suporte a formulários da Web

Cria vazia *App\_dados* e *modelos* pastas e uma *global. asax* arquivo. Eles já são criados por todos os modelos que não seja o modelo vazio, portanto, marcando a caixa de seleção de formulários da Web não faz diferença para outros modelos.

O modelo de Web Forms habilita URLs amigáveis, por padrão, mas quando você adiciona a que Web Forms dão suporte a outros modelos, marcando a caixa de seleção de Web Forms que URLs amigáveis não são habilitadas automaticamente.

### <a name="add-mvc-support"></a>Adicionar suporte do MVC

Instala pacotes WebPages NuGet, Razor e MVC, cria vazia *App\_dados*, *controladores*, *modelos*, e *exibições*pastas, cria *App\_iniciar* pasta com *RouteConfig.cs* de arquivo e cria *global. asax* arquivo.

### <a name="add-web-api-support"></a>Adicionar suporte a API da Web

Instala pacotes do NuGet newtonsoft. JSON e de API da Web, cria vazia *App\_dados*, *controladores*, e *modelos* pastas, cria  *Aplicativo\_inicie* pasta com *WebApiConfig.cs* de arquivo e cria *global. asax* arquivo.

<a id="auth"></a>
## <a name="authentication-methods"></a>Métodos de autenticação

Visual Studio 2013 oferece várias opções de autenticação para os modelos de Web Forms, MVC e API da Web:

- [Sem autenticação](#noauth)
- [Contas de usuário individuais](#indauth) (identidade do ASP.NET, anteriormente conhecido como associação do ASP.NET)
- [As contas organizacionais](#orgauth) (Windows Server Active Directory ou do Azure Active Directory)
- [Autenticação do Windows](#winauth) (Intranet)

![Configurar a caixa de diálogo de autenticação](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Sem autenticação

Se você selecionar **sem autenticação**, o aplicativo de exemplo não conterá nenhuma páginas da web para fazer logon, não interface do usuário que indica quem está conectado, não há classes de entidade para um banco de dados de associação e nenhuma cadeia de caracteres de conexão para um banco de dados de associação.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Contas de usuário individuais

Se você selecionar **contas de usuário individuais**, o aplicativo de exemplo será configurado para usar a identidade do ASP.NET (anteriormente conhecido como associação do ASP.NET) para autenticação do usuário. Identidade do ASP.NET permite que um usuário registrar uma conta com a criação de um nome de usuário e senha no site ou ao entrar com provedores sociais, como Facebook, Google, Account da Microsoft ou Twitter. O armazenamento de dados padrão para os perfis de usuário no ASP.NET Identity é um banco de dados do SQL Server LocalDB, que pode ser implantado no SQL Server ou banco de dados SQL para o site de produção.

No Visual Studio 2013, esses recursos são os mesmos como no Visual Studio 2012, mas o código subjacente para o sistema de associação do ASP.NET foi reescrito. Vantagens da nova base de código incluem o seguinte:

- O novo sistema de associação se baseia [OWIN](http://owin.org/) em vez do módulo de autenticação de formulários do ASP.NET. Isso significa que você pode usar o mesmo mecanismo de autenticação, se você estiver usando o Web Forms ou MVC no IIS ou você estiver auto-hospedagem da API Web ou SignalR.
- O novo banco de dados de associação é gerenciado pelo Entity Framework Code First, e todas as tabelas são representadas por classes de entidade que você pode modificar. Isso significa que você pode personalizar facilmente o esquema de banco de dados e a web relacionados ao perfil da interface do usuário de acordo com suas necessidades, e você pode implantar facilmente as atualizações usando migrações do Code First.

O novo sistema de associação é implementado automaticamente em novos modelos, e pode ser implementado manualmente em qualquer projeto que tem como alvo o .NET 4.5 ou posterior.

Identidade do ASP.NET é uma boa opção se você estiver criando um site da web da Internet que é principalmente para clientes externos. Se sua organização usa o Active Directory ou do Office 365 e você deseja criar um projeto que permite a single-sign-on para os funcionários e parceiros de negócios, o **contas organizacionais** opção pode ser uma opção melhor.

Para obter mais informações sobre a opção de contas de usuário individuais, consulte os seguintes recursos:

- [www.asp.net/identity](../../../identity/index.md). Documentação sobre a identidade do ASP.NET no site da web ASP.NET.
- [Criar um aplicativo ASP.NET MVC 5 com o Facebook e Google OAuth2 e OpenID logon](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Também mostra como personalizar os dados de perfil do usuário.
- [API da Web - serviços de autenticação externa](../../../web-api/overview/security/external-authentication-services.md)
- [Adicionar logons externos ao seu aplicativo ASP.NET no Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Contas organizacionais

Se você selecionar **contas organizacionais**, o aplicativo de exemplo será configurado para usar o Windows Identity Foundation (WIF) para autenticação com base em contas de usuário no Azure AD (Active Directory do Azure, que inclui o Office 365) ou Windows Server Active Directory. Para obter mais informações, consulte [opções de autenticação de conta organizacional](#orgauthoptions) mais adiante neste tópico.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticação do Windows

Se você selecionar **autenticação do Windows**, o aplicativo de exemplo será configurado para usar o módulo do IIS de autenticação do Windows para autenticação. O aplicativo exibirá a ID de usuário e domínio do Active directory ou conta do computador local que está conectada ao Windows, mas não incluem o registro de usuário ou de log da interface do usuário. Essa opção destina-se a sites da Intranet.

Como alternativa, você pode criar um site da Intranet que usa a autenticação do AD, escolhendo a [opção em contas organizacionais locais](#orgauthonprem). A opção de local usa o Windows Identity Foundation (WIF), em vez do módulo de autenticação do Windows. Algumas etapas adicionais são necessárias para configurar a opção de local, mas o WIF habilita os recursos que não estão disponíveis com o módulo de autenticação do Windows. Por exemplo, com o WIF configurar acesso do aplicativo do Active Directory e consultar dados de diretório.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opções de autenticação de conta organizacional

O **configurar a autenticação** caixa de diálogo fornece várias opções para o Azure AD (Active Directory do Azure, que inclui o Office 365) ou autenticação de conta do Windows Server Active Directory (AD):

- [Nuvem - organização única](#orgauthsingle) (Azure AD ou AD usando a integração de diretório com o Azure AD)
- [Nuvem - organização Multi](#orgauthmulti) (Azure AD ou AD usando a integração de diretório com o Azure AD)
- [Local](#orgauthonprem) (AD)

Se você quiser tentar uma das opções do Azure AD, mas ainda não tiver uma conta, [clique aqui para se inscrever para uma conta do Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Se você escolher uma das opções do Azure AD, seu projeto requer um banco de dados e você precisará entrar uma conta de administrador global para seu locatário do AD do Azure. Insira o nome e senha para uma conta organizacional (por exemplo, admin@contoso.onmicrosoft.com) que tenha permissões administrativas para seu locatário do Azure AD.
> 
> **Não insira as credenciais para uma conta da Microsoft (por exemplo, contoso@hotmail.com) na caixa de diálogo de logon.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Nuvem - organização única autenticação

![Autenticação única organização](creating-web-projects-in-visual-studio/_static/image24.png)

Escolha esta opção se você quiser habilitar a autenticação de contas de usuário que são definidos em um Azure AD [locatário](https://technet.microsoft.com/library/jj573650.aspx). Por exemplo, o site for contoso.com, e ele será disponibilizado para os funcionários da empresa Contoso que estiverem no locatário contoso.onmicrosoft.com. Você não poderá configurar o Azure AD para permitir que os usuários de outros locatários acessar o aplicativo.

#### <a name="domain"></a>Domain

Insira o domínio do AD do Azure que você deseja configurar o aplicativo, por exemplo: `contoso.onmicrosoft.com`. Se você tiver um [domínio personalizado](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), como `contoso.com` em vez de `contoso.onmicrosoft.com`, insira-o aqui.

#### <a name="access-level"></a>Nível de acesso

Se o aplicativo precisa para consultar ou atualizar as informações de diretório usando a API do Graph, escolha **logon único, ler dados do diretório** ou **logon único, ler e gravar dados do diretório**. Caso contrário, escolha **Single Sign-On**. Para obter mais informações, consulte [níveis de acesso do aplicativo](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) e [usando a API do Graph para consultar o Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI da ID do aplicativo

Por padrão, o modelo cria um URI da ID do aplicativo para você, acrescentando o nome do projeto para o domínio do AD do Azure. Por exemplo, se o nome do projeto está `Example` e o domínio é `contoso.onmicrosoft.com`, o aplicativo torna-se do URI da ID `https://contoso.onmicrosoft.com/Example`. Se você quiser especificar manualmente o URI de ID do aplicativo, expanda a **mais opções** seção e insira o URI de ID do aplicativo na caixa de texto. O aplicativo no URI de ID deve começar com `https://`.

Por padrão, se um aplicativo que já foi provisionado no AD do Azure tem o mesmo URI da ID de aplicativo como aquela que está usando o Visual Studio para o projeto, o projeto será conectado ao aplicativo existente em vez de provisionar um novo. Se você quiser um novo aplicativo a ser provisionada nesse caso, desmarque a **substituir a entrada do aplicativo, se já existir outro com a mesma ID** caixa de seleção.

Se o **substituir** caixa de seleção estiver desmarcada e o Visual Studio encontra um aplicativo existente com o mesmo URI de ID de aplicativo, ele cria um novo URI anexando um número para o URI que irá usar. Por exemplo, suponha que é o nome do projeto `Example`, você deixe a caixa de texto em branco, se você desmarcar a **substituição** caixa de seleção e o locatário do AD do Azure já tem um aplicativo com o URI `https://contoso.onmicrosoft.com/Example`. Nesse caso, um novo aplicativo será provisionado com um aplicativo, como URI da ID `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Provisionamento do aplicativo no Azure AD

Para provisionar o aplicativo no Azure AD ou conectar-se o projeto a um aplicativo existente, o Visual Studio precisa das credenciais de administrador global para o domínio. Quando você clica em **Okey** na **configurar a autenticação** caixa de diálogo, você será solicitado para o nome de usuário e a senha de um administrador global para o domínio especificado. Posteriormente, quando você clica **criar projeto** na **novo projeto ASP.NET** caixa de diálogo, o Visual Studio configura o aplicativo no Azure AD. Observe que, como parte desse processo Visual Studio insere valores de segredo do cliente no arquivo Web. config que expira um ano após a criação.

Para obter informações sobre como criar aplicativos que usam **nuvem - organização única** autenticação, consulte os seguintes recursos:

- [Autenticação do Azure](../2012/windows-azure-authentication.md)
- [Adicionando logon ao seu aplicativo Web usando o Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Desenvolvendo aplicativos do ASP.NET com o Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteger API da Web ASP.NET com o Azure AD e os componentes Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Os tutoriais ainda não foram atualizados para o Visual Studio 2013; alguns dos quais os tutoriais direcionará você para fazer manualmente é automatizado no Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Nuvem - organização Multi-Authentication

![Autenticação de organização múltipla](creating-web-projects-in-visual-studio/_static/image25.png)

Escolha esta opção se você quiser habilitar a autenticação de contas de usuário que são definidos em vários Azure AD [locatários](https://technet.microsoft.com/library/jj573650.aspx). Por exemplo, o site for contoso.com, e ele será disponibilizado para os funcionários da empresa Contoso que estiverem no locatário contoso.onmicrosoft.com e funcionários da empresa Fabrikam que estiverem no locatário fabrikam.onmicrosoft.com.

As configurações que você insira e o etapa de provisionamento de aplicativo são semelhantes às [autenticação única organização](#orgauthsingle).

Para obter informações sobre como criar aplicativos que usam **nuvem - organização Multi** autenticação, consulte os seguintes recursos:

- [Fácil integração de aplicativos Web com o Active Directory do Azure, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) no blog da equipe do Active Directory.
- [Desenvolvimento de aplicativos Web de multilocatário com o Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) tutorial. O tutorial ainda não foi atualizado para o Visual Studio 2013; alguns dos quais o tutorial orienta você a fazer manualmente é automatizado no Visual Studio 2013.
- [Você precisa inscrever-se com seu próprio aplicativo de ASP.NET várias organizações antes que você possa entrar](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog de Vittorio Bertocci que explica como resolver uma pessoas problema comum encontrar ao criar um projeto que usa a autenticação de várias organizações.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticação organizacional local

![Autenticação organizacional local](creating-web-projects-in-visual-studio/_static/image26.png)

Escolha esta opção se você deseja habilitar a autenticação para contas de usuário que são definidas no Windows Server Active Directory (AD), e você não quiser usar o Azure AD. Você pode usar essa opção para criar um site de Intranet ou um site da Internet. Para um site da Internet, use os serviços de Federação do Active Directory (ADFS) para fornecer acesso ao AD. Para obter mais informações, consulte [usar a opção de autenticação organizacional local (ADFS) com o ASP.NET no Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Para um site da Intranet, como alternativa você pode escolher [autenticação do Windows](#winauth) em vez dessa opção. Para a opção de autenticação do Windows, você não precisa fornecer uma URL do documento de metadados. No entanto, a autenticação do Windows não dá a capacidade de controlar o acesso do aplicativo no Active Directory ou para consultar dados de diretório.

#### <a name="on-premises-authority"></a>Autoridade local

Insira uma URL que aponta para o documento de metadados. O documento de metadados contém as coordenadas da autoridade. O aplicativo utilizará essas coordenadas para impulsionar o fluxo de logon da web.

#### <a name="application-id-uri"></a>URI da ID do aplicativo

Forneça um URI exclusivo que o AD pode usar para identificar este aplicativo, ou deixe em branco para permitir que o Visual Studio crie uma.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Próximas etapas

Este documento tem fornecido alguns ajuda básica para a criação de um novo projeto web ASP.NET no Visual Studio 2013. Para obter mais informações sobre como usar o Visual Studio para desenvolvimento da web, consulte [ https://www.asp.net/visual-studio/ ](../../index.md).
