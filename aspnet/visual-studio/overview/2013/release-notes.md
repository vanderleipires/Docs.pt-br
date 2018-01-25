---
uid: visual-studio/overview/2013/release-notes
title: "ASP.NET e Web Tools para notas de versão do Visual Studio 2013 | Microsoft Docs"
author: microsoft
description: "Este documento descreve a versão do ASP.NET e Web Tools para Visual Studio 2013."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 7f38a0f2693aeb2a4884b9c03719b583423957a8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET e Web Tools para notas de versão do Visual Studio 2013
====================
por [Microsoft](https://github.com/microsoft)

> Este documento descreve a versão do ASP.NET e Web Tools para Visual Studio 2013.


## <a name="contents"></a>Conteúdo

- [Notas de instalação](#TOC1)
- [Documentação](#TOC2)
- [Requisitos de software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Novos recursos no ASP.NET e Web Tools para Visual Studio 2013

- [Um ASP.NET](#TOC6)
- [Nova experiência de projeto da Web](#newproj)
- [Estrutura do ASP.NET](#scaffold)
- [Link do navegador](#browser-link)
- [Aprimoramentos do Editor do Visual Studio Web](#web-editor)
- [Suporte a aplicativos da Web do serviço de aplicativo do Azure no Visual Studio](#waws)
- [Aprimoramentos de publicação da Web](#publish)
- [NuGet 2.7](#nuget)
- [Web Forms do ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [Identidade do ASP.NET](#TOC8)
- [Componentes do Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Suspender o aplicativo ASP.NET](#TOC15)
- [Problemas conhecidos e as alterações recentes](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notas de instalação

ASP.NET e Web Tools para Visual Studio 2013 são empacotadas no instalador do principal e pode ser baixado [aqui](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o ASP.NET e Web Tools para Visual Studio 2013 estão disponíveis no [site da web ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisitos de software

ASP.NET e ferramentas da Web requer o Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Novos recursos no ASP.NET e Web Tools para Visual Studio 2013

As seções a seguir descrevem os recursos que foram introduzidos na versão.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Um ASP.NET

Com o lançamento do Visual Studio 2013, levamos um passo para unificar a experiência de uso de tecnologias ASP.NET, para que você pode facilmente combinar e coincidir com o que você deseja. Por exemplo, você pode iniciar um projeto usando o MVC e facilmente adicionar páginas Web Forms ao projeto mais tarde ou scaffold APIs da Web em um projeto Web Forms. É um ASP.NET sobre tornando mais fácil para você como um desenvolvedor de fazer as coisas que você adora no ASP.NET. Não importa qual tecnologia você escolher, você pode ter certeza de que você está criando na estrutura subjacente confiável do ASP.NET de um.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nova experiência de projeto da Web

É aprimorada a experiência de criação de novos projetos da web no Visual Studio 2013. No **novo projeto da Web ASP.NET** caixa de diálogo que você pode selecionar o tipo de projeto você deseja, configurar qualquer combinação de tecnologias (Web Forms, MVC, Web API), configurar opções de autenticação e adicionar um projeto de teste de unidade.

![Novo projeto ASP.NET](release-notes/_static/image1.png)

A nova caixa de diálogo permite que você altere as opções de autenticação padrão para muitos dos modelos. Por exemplo, quando você cria um projeto ASP.NET Web Forms você pode selecionar qualquer uma das seguintes opções:

- Sem autenticação
- Contas de usuário individuais (associação ASP.NET ou sociais do provedor de log no)
- Contas organizacionais (Active Directory em um aplicativo de internet)
- Autenticação do Windows (Active Directory em um aplicativo de intranet)

![Opções de autenticação](release-notes/_static/image2.png)

Para obter mais informações sobre o novo processo para a criação de projetos da web, consulte [criar projetos da Web do ASP.NET no Visual Studio 2013](creating-web-projects-in-visual-studio.md). Para obter mais informações sobre as novas opções de autenticação, consulte [ASP.NET Identity](#TOC8) mais adiante neste documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Estrutura do ASP.NET

Estrutura do ASP.NET é uma estrutura de geração de código para aplicativos Web ASP.NET. Torna mais fácil adicionar código clichê ao seu projeto que interage com um modelo de dados.

Nas versões anteriores do Visual Studio, scaffolding foi limitado a projetos do ASP.NET MVC. Com o Visual Studio 2013, agora você pode usar o scaffolding para qualquer projeto ASP.NET, incluindo formulários da Web. Visual Studio 2013 não oferece suporte para gerar páginas para um projeto de Web Forms, mas você ainda pode usar o scaffolding com formulários da Web Adicionando dependências MVC ao projeto. O suporte para a geração de páginas para formulários da Web será adicionado em uma atualização futura.

Ao usar o scaffolding, podemos garantir que todos os dependências estejam instaladas no projeto. Por exemplo, se você iniciar com um projeto Web Forms do ASP.NET e, em seguida, use o scaffolding para adicionar um controlador de API da Web, os pacotes do NuGet necessários e as referências são adicionadas ao seu projeto automaticamente.

Para adicionar o scaffolding MVC para um projeto de Web Forms, adicione um **Novo Item de Scaffold** e selecione **MVC 5 dependências** na janela de diálogo. Há duas opções para a estrutura MVC; Mínimo e completo. Se você selecionar no mínimo, somente os pacotes NuGet e referências para o ASP.NET MVC são adicionadas ao seu projeto. Se você selecionar a opção completo, as dependências mínimo são adicionadas, bem como os arquivos de conteúdo necessários para um projeto MVC.

Suporte para a estrutura async controladores usa os novos recursos de assíncronas do Entity Framework 6.

Para obter mais informações e tutoriais, consulte [visão geral do ASP.NET Scaffolding](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Link do navegador – SignalR canal entre o navegador e o Visual Studio

O novo [Link do navegador](using-browser-link.md) recurso permite que você se conectar a vários navegadores para o Visual Studio e atualizá-los ao clicar em um botão na barra de ferramentas. Você pode se conectar a vários navegadores para seu site de desenvolvimento, incluindo os emuladores de dispositivos móveis e clique em atualizar para atualizar todos os navegadores todos ao mesmo tempo. Link do navegador também expõe uma API para permitir que os desenvolvedores criem extensões de Link do navegador.

![](release-notes/_static/image3.png)

Permitindo que os desenvolvedores aproveitar a API de Link do navegador, será possível criar cenários muito avançados que atravessa os limites entre o Visual Studio e qualquer navegador que está conectado. Web Essentials aproveita a API para criar uma experiência integrada entre o Visual Studio e ferramentas de desenvolvedor do navegador, remotas controlando emuladores de dispositivos móveis e muito mais.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Aprimoramentos do Editor do Visual Studio Web

Visual Studio 2013 inclui um novo editor de HTML para arquivos Razor e arquivos HTML em aplicativos da web. O novo editor de HTML fornece um único esquema unificado com base em HTML5. Ela tem preenchimento automático de chaves, jQuery UI e AngularJS atributo IntelliSense, atributo de agrupamento do IntelliSense, ID e nome da classe Intellisense e outros aprimoramentos, incluindo um desempenho melhor, formatação e SmartTags.

Captura de tela a seguir demonstra como usar o atributo Bootstrap IntelliSense no editor de HTML.

![IntelliSense no editor de HTML](release-notes/_static/image4.png)

Visual Studio 2013 também vem com ambos os CoffeeScript e menos editores internos. O editor LESS vem com todos os recursos interessantes do editor de CSS e tem Intellisense específico para variáveis e mixins em todos os menos documentos no @import cadeia.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Suporte a aplicativos da Web do serviço de aplicativo do Azure no Visual Studio

No Visual Studio 2013 com o SDK do Azure para .NET 2.2, você pode usar **Server Explorer** interagir diretamente com seus aplicativos web remoto. Você pode entrar sua conta do Azure, criar novos aplicativos web, configure aplicativos, exibir logs em tempo real e muito mais. Surgimento logo após 2.2 do SDK for lançado, você poderá executar no modo de depuração remotamente no Azure. A maioria dos novos recursos para aplicativos de Web do serviço de aplicativo do Azure também funcionam no Visual Studio 2012 quando você instala a versão atual do SDK do Azure para .NET.

Para obter mais informações, consulte os seguintes recursos:

- [Criar um aplicativo web ASP.NET no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Aprimoramentos de publicação da Web

Visual Studio 2013 inclui recursos novos e aprimorados de publicação na Web. Aqui estão alguns deles:

- Facilmente [automatizam a criptografia do arquivo Web. config](https://go.microsoft.com/fwlink/?LinkId=325529). (Este link e os dois seguintes apontam para documentação no MSDN pode não estar disponível até o dia em 10/17 no final.)
- Facilmente [automatizar colocar um aplicativo offline durante a implantação](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configurar a implantação da Web para [usar soma de verificação do arquivo em vez de alterado a última data](https://go.microsoft.com/fwlink/?LinkId=325531) para determinar quais arquivos devem ser copiados para o servidor.
- Quando você estiver usando o FTP ou métodos de publicação do sistema de arquivos, bem como com implantação da Web rapidamente publica os arquivos selecionados individuais (incluindo o Web. config).

Para obter mais informações sobre a implantação da web do ASP.NET, consulte [o site ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

2.7 NuGet inclui um conjunto avançado de novos recursos que são descritas em detalhes em [notas de versão 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versão do NuGet também remove a necessidade de fornecer consentimento explícito para o recurso de restauração do pacote do NuGet baixar os pacotes. Consentimento (e a caixa de seleção na caixa de diálogo de preferências do NuGet) agora são concedidas por instalar o NuGet. Agora restauração do pacote simplesmente funciona por padrão.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Web Forms do ASP.NET

### <a name="one-aspnet"></a>Um ASP.NET

Os modelos de projeto Web Forms integração perfeita com a nova experiência One ASP.NET. Você pode adicionar MVC e a API da Web dão suporte ao seu projeto de formulários da Web, e você pode configurar a autenticação usando o Assistente de criação de projeto do ASP.NET de um. Para obter mais informações, consulte [criar projetos da Web do ASP.NET no Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Os modelos de projeto de formulários da Web oferecem suporte a nova estrutura de identidade do ASP.NET. Além disso, os modelos agora oferecem suporte a criação de um projeto de intranet de formulários da Web. Para obter mais informações, consulte [métodos de autenticação](creating-web-projects-in-visual-studio.md#auth) na **criar projetos da Web do ASP.NET no Visual Studio 2013**.

### <a name="bootstrap"></a>inicialização

Usam os modelos de Web Forms [Bootstrap](http://twitter.github.io/bootstrap/) para fornecer uma elegante e ágeis aparência que você pode personalizar facilmente. Para obter mais informações, consulte [Bootstrap em modelos de projeto da web do Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Um ASP.NET

Os modelos de projeto da Web MVC integração perfeita com a nova experiência One ASP.NET. Você pode personalizar seu projeto MVC e configurar a autenticação usando o Assistente de criação do ASP.NET de um projeto. Um tutorial de Introdução ao ASP.NET MVC 5 pode ser encontrado em [guia de Introdução ao ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obter informações sobre como atualizar projetos MVC 4 a 5 do MVC, consulte [como atualizar um ASP.NET MVC 4 e o projeto de API da Web para ASP.NET MVC 5 e Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Os modelos de projeto MVC foram atualizados para usar a identidade do ASP.NET para autenticação e gerenciamento de identidade. Um tutorial com a autenticação do Facebook e do Google e a nova API de associação pode ser encontrado em [criar um aplicativo do ASP.NET MVC 5 com o Facebook e Google OAuth2 e Sign-on OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) e [criar um aplicativo ASP.NET MVC com autenticação e Banco de dados SQL e implantar o serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>inicialização

O modelo de projeto MVC foi atualizado para usar [Bootstrap](http://getbootstrap.com/) para fornecer uma elegante e ágeis aparência que você pode personalizar facilmente. Para obter mais informações, consulte [Bootstrap em modelos de projeto da web do Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtros de autenticação

Filtros de autenticação são um novo tipo de filtro no ASP.NET MVC que executar antes de filtros de autorização no pipeline do ASP.NET MVC e permitem que você especifique autenticação lógica por ação, por controlador, ou globalmente para todos os controladores. Filtros de autenticação processam as credenciais na solicitação e fornecem uma entidade correspondente. Filtros de autenticação também podem adicionar os desafios de autenticação em resposta a solicitações não autorizadas.

### <a name="filter-overrides"></a>Substituições de filtro

Agora você pode substituir os filtros se aplicam a um controlador ou método de ação em questão, especificando um filtro de substituição. Os filtros de substituição de especificar um conjunto de tipos de filtro não deve ser executado para um determinado escopo (ação ou controlador). Isso permite que você configurar os filtros que se aplicam globalmente, mas, em seguida, excluir determinados filtros globais da aplicação para ações específicas ou controladores.

### <a name="attribute-routing"></a>Roteamento de atributo

ASP.NET MVC agora dá suporte a roteamento de atributo, graças uma contribuição por Tim McCall, o autor do [http://attributerouting.net](http://attributerouting.net). Com o roteamento de atributo, você pode especificar suas rotas, anotando ações e controladores.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>API Web ASP.NET 2

### <a name="attribute-routing"></a>Roteamento de atributo

ASP.NET Web API agora dá suporte a roteamento de atributo, graças uma contribuição por Tim McCall, o autor do [http://attributerouting.net](http://attributerouting.net). Com o roteamento de atributo, você pode especificar rotas sua API da Web, anotando ações e controladores assim:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Roteamento de atributo fornece mais controle sobre os URIs em sua API da web. Por exemplo, você pode definir facilmente uma hierarquia de recursos usando um único controlador de API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Roteamento de atributo também fornece uma sintaxe conveniente para especificar parâmetros opcionais, valores padrão e restrições da rota:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Para obter mais informações sobre o roteamento de atributo, consulte [atributo roteamento na API Web 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Os modelos de projeto de API da Web e o aplicativo de página única agora oferecem suporte a autorização usando OAuth 2.0. OAuth 2.0 é uma estrutura para autorizar o acesso de cliente aos recursos protegidos. Ele funciona para uma variedade de clientes, incluindo navegadores e dispositivos móveis.

Suporte para OAuth 2.0 baseia-se na nova middleware de segurança fornecidos pelos componentes Microsoft OWIN para autenticação do portador e implementar a função de servidor de autorização. Como alternativa, os clientes podem ser autorizados usando um servidor de autorização organizacionais, como o Active Directory do Azure ou o AD FS no Windows Server 2012 R2.

### <a name="odata-improvements"></a>Aprimoramentos de OData

**Suporte para $select, $expand, $batch e $value**

ASP.NET Web API OData agora tem suporte completo para $select, $expand e $value. Você também pode usar $batch para solicitação de envio em lote e processamento de conjuntos de alterações.

Opções de $select e $expand permitem que você alterar a forma dos dados retornados de um ponto de extremidade OData. Para obter mais informações, consulte [Introducing $select e $expand suporte no OData da API da Web](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Extensibilidade aprimorada**

Os formatadores OData agora são extensíveis. Você pode adicionar metadados de entrada do Atom, o suporte nomeadas entradas de link de mídia e fluxo, adicionar anotações de instância e personalizar como os links são gerados.

**Tipo sem suporte**

Agora você pode criar serviços OData sem a necessidade de definir tipos CLR para seus tipos de entidade. Em vez disso, os controladores OData podem levar ou retornar instâncias de **IEdmObject**, que são os formatadores OData serializar/desserializar.

**Reutilizar um modelo existente**

Se você já tiver um modelo de dados de entidade (EDM) existente, você pode agora reutilizá-lo diretamente, em vez de criar um novo. Por exemplo, se você estiver usando o Entity Framework, você pode usar o EDM EF cria para você.

### <a name="request-batching"></a>Solicitação de envio em lote

Solicitação em lote combina várias operações em uma única solicitação HTTP POST, para reduzir o tráfego de rede e fornecer mais suave, menos a interface do usuário verborrágicas. ASP.NET Web API agora dá suporte a várias estratégias para solicitação em lote:

- Use o ponto de extremidade $batch de um serviço OData.
- Empacotar várias solicitações em uma única solicitação de várias partes MIME.
- Use um formato personalizado de envio em lote.

Para habilitar a solicitação de envio em lote, basta adicione uma rota com um manipulador de envio em lote para sua configuração de API da Web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

Você também pode controlar se solicitações ou executadas em sequência ou em qualquer ordem.

### <a name="portable-aspnet-web-api-client"></a>Portátil ASP.NET Web API do cliente

Agora você pode usar o cliente do ASP.NET Web API para criar bibliotecas de classes portáteis que funcionam em seus aplicativos da Windows Store e Windows Phone 8. Você também pode criar formatadores portátil que podem ser compartilhados entre cliente e servidor.

### <a name="improved-testability"></a>Melhor capacidade de teste

Web API 2 torna muito mais fácil a unidade de seus controladores de API de teste. Precisa instanciar seu controlador API com a mensagem de solicitação e a configuração e, em seguida, chame o método de ação que você deseja testar. Também é fácil simular a **UrlHelper** classe, para métodos de ação que executam a geração de link.

### <a name="ihttpactionresult"></a>IHttpActionResult

Agora você pode implementar IHttpActionResult para encapsular o resultado de métodos de ação de API da Web. Um IHttpActionResult retornado de um método de ação de API da Web é executado pelo tempo de execução ASP.NET Web API para produzir a mensagem de resposta resultante. Um IHttpActionResult pode ser retornado de qualquer ação de API da Web para simplificar a unidade de teste de sua implementação de API da Web. Para um número de implementações de IHttpActionResult são fornecidos prontos, incluindo os resultados para retornar códigos de status específicas de conveniência, formatado conteúdas negociado conteúdo ou respostas.

### <a name="httprequestcontext"></a>HttpRequestContext

O novo **HttpRequestContext** controla qualquer estado que está associado à solicitação, mas não está imediatamente disponível da solicitação. Por exemplo, você pode usar o **HttpRequestContext** para obter dados de rota, a entidade de segurança associado à solicitação, o certificado do cliente, o **UrlHelper** e o caminho virtual raiz. Você pode criar facilmente um **HttpRequestContext** fins de teste de unidade.

Porque a entidade de segurança para a solicitação flui com a solicitação em vez de depender **CurrentPrincipal**, a entidade de segurança agora está disponível em todo o tempo de vida da solicitação enquanto ele está no pipeline de API da Web.

### <a name="cors"></a>CORS

Graças a outra ótima contribuição de Brock Allen, ASP.NET agora totalmente dá suporte a origem da solicitação CORS (compartilhamento entre).

Segurança do navegador impede que uma página da web fazer solicitações do AJAX para outro domínio. [CORS](http://www.w3.org/TR/cors/) é um padrão de W3C que permite que um servidor atenuar a política de mesma origem. Usando CORS, um servidor pode permitir explicitamente algumas solicitações entre origens durante a rejeição de outras pessoas.

Web API 2 agora dá suporte a CORS, incluindo manipulação automática de solicitações de simulação. Para obter mais informações, consulte [habilitando solicitações entre origens no ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtros de autenticação

Filtros de autenticação são um novo tipo de filtro na API Web do ASP.NET que executar antes de filtros de autorização no pipeline do ASP.NET Web API e permitem que você especifique autenticação lógica por ação, por controlador, ou globalmente para todos os controladores. Filtros de autenticação processam as credenciais na solicitação e fornecem uma entidade correspondente. Filtros de autenticação também podem adicionar os desafios de autenticação em resposta a solicitações não autorizadas.

### <a name="filter-overrides"></a>Substituições de filtro

Agora você pode substituir os filtros se aplicam a um método de ação em questão ou controlador, especificando um filtro de substituição. Os filtros de substituição de especificar um conjunto de tipos de filtro não deve ser executado para um determinado escopo (ação ou controlador). Isso permite que você adicione filtros globais, mas, em seguida, excluir alguns controladores ou ações específicas.

### <a name="owin-integration"></a>Integração de OWIN

ASP.NET Web API agora totalmente suporta OWIN e pode ser executada em qualquer host com capacidade de OWIN. Também há um **HostAuthenticationFilter** que fornece integração com o sistema de autenticação OWIN.

Com a integração de OWIN, você pode hospedar automaticamente API da Web em seu próprio processo junto com outros middleware OWIN, como o SignalR. Para obter mais informações, consulte [OWIN de uso Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>ASP.NET SignalR 2.0

As seções a seguir descrevem os recursos do SignalR 2.0.

- [Criado em OWIN](#builtonowin)
- [MapHubs e MapConnection agora são MapSignalR](#MapSignalR)
- [Suporte de domínio cruzado](#crossdomain)
- [iOS e Android suportam via MonoTouch e MonoDroid](#mobile)
- [Cliente .NET portátil](#portable)
- [Novo pacote de hospedagem interna](#selfhost)
- [Suporte do servidor compatível](#backwardcompat)
- [Removeu o suporte do servidor para o .NET 4.0](#remove40)
- [Enviando uma mensagem para uma lista de clientes e grupos](#messagelist)
- [Enviando uma mensagem para um usuário específico](#sendtouser)
- [Melhor suporte a manipulação de erro](#errorhandling)
- [Unidade mais fácil de teste de hubs](#unittesting)
- [Tratamento de erros de JavaScript](#javascripterror)

Para obter um exemplo de como atualizar um projeto existente de 1. x para o SignalR 2.0, consulte [atualizando um SignalR 1. x projeto](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Criado em OWIN

SignalR 2.0 se baseia totalmente no [OWIN (a Interface Web aberta para .NET)](http://owin.org/). Essa alteração torna o processo de instalação para o SignalR muito mais consistente entre os aplicativos SignalR auto-hospedado e hospedado na web, mas também é necessário um número de alterações de API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs e MapConnection agora são MapSignalR

Para compatibilidade com os padrões OWIN, esses métodos foram renomeados para `MapSignalR`. `MapSignalR`chamado sem parâmetros mapeará todos os hubs (como `MapHubs` na versão 1. x); para mapear individuais **PersistentConnection** objetos, especifique o tipo de conexão como o parâmetro de tipo e a extensão de URL para a conexão como o primeiro argumento.

O `MapSignalR` método é chamado em uma classe de inicialização Owin. Visual Studio 2013 contém um novo modelo para uma classe de inicialização Owin; Para usar esse modelo, faça o seguinte:

1. Clique com botão direito no projeto
2. Selecione **adicionar**, **Novo Item...**
3. Selecione **classe de inicialização Owin**. Nomeie a nova classe **Startup.cs**.

Em um **aplicativo Web,** Owin inicialização classe que contém o `MapSignalR` método é adicionado ao processo de inicialização do Owin usando uma entrada no nó de configurações de aplicativo do arquivo Web. config, conforme mostrado abaixo.

Em um **auto-hospedado aplicativo**, a classe de inicialização é passada como o parâmetro de tipo do `WebApp.Start` método.

**Mapeamento de hubs e conexões no SignalR 1. x (a partir do arquivo de aplicativo global em um aplicativo web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Hubs de mapeamento e conexões no SignalR 2.0 (de um arquivo de classe de inicialização Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

Em um **auto-hospedado aplicativo**, a classe de inicialização é passada como o parâmetro de tipo para o `WebApp.Start` método, conforme mostrado abaixo.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Suporte de domínio cruzado

No SignalR 1. x, solicitações de domínio cruzado foram controlado por um sinalizador EnableCrossDomain único. Esse sinalizador controlado solicitações JSONP e CORS. Para obter maior flexibilidade, todos os CORS suporte foi removido do componente de servidor do SignalR (JavaScript lients ainda usarem CORS normalmente se ele for detectado que o navegador oferece suporte a ele), e o middleware OWIN nova foi disponibilizado dar suporte a esses cenários.

SignalR 2.0, JSONP se é necessário no cliente (para dar suporte a solicitações de domínio cruzado em navegadores mais antigos), ele precisará ser habilitada explicitamente definindo `EnableJSONP` no `HubConfiguration` do objeto para `true`, conforme mostrado abaixo. JSONP está desabilitado por padrão, pois é menos segura do que CORS.

Para adicionar o novo middleware CORS no SignalR 2.0, adicione o `Microsoft.Owin.Cors` biblioteca ao seu projeto e chame `UseCors` antes de seu middleware SignalR, conforme mostrado na seção a seguir.

**Adicionando owin ao seu projeto**: para instalar essa biblioteca, execute o seguinte comando no Console do Gerenciador de pacotes:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Esse comando adicionará o 2.0.0 versão do pacote ao seu projeto.

**Chamar UseCors**

Os trechos de código a seguir demonstram como implementar conexões entre domínios no SignalR 1. x e 2.0.

**Implementando solicitações entre domínios no SignalR 1. x (do arquivo de aplicativo global)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementando solicitações entre domínios no SignalR 2.0 (de um arquivo de código c#)**

O código a seguir demonstra como habilitar o CORS ou JSONP em um projeto de SignalR 2.0. Este exemplo de código usa `Map` e `RunSignalR` em vez de `MapSignalR`, de modo que o middleware CORS é executado apenas para as solicitações de SignalR que exigem suporte de CORS (em vez de todo o tráfego no caminho especificado em `MapSignalR`.) `Map` também pode ser usado para qualquer outro tipo de middleware que precisa ser executado para um prefixo de URL específico, em vez de todo o aplicativo.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>iOS e Android suportam via MonoTouch e MonoDroid

Foi adicionado suporte para iOS e Android clientes usando componentes MonoTouch e MonoDroid da [Xamarin biblioteca](https://xamarin.com/). Para obter mais informações sobre como usá-los, consulte [usando componentes de Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Esses componentes estejam disponíveis no [Xamarin Store](https://store.xamarin.com/) quando a versão RTW SignalR está disponível.

<a id="portable"></a># # # Portátil de cliente .NET

Para melhor, facilitando o desenvolvimento de plataforma cruzada, o Silverlight, WinRT e clientes do Windows Phone foram substituídos por um único cliente .NET portátil que suporta as seguintes plataformas:

- NET 4.5
- Silverlight 5
- WinRT (.NET para aplicativos da Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Novo pacote de hospedagem interna

Agora é um pacote do NuGet para tornar mais fácil começar com o próprio Host do SignalR (SignalR aplicativos que são hospedados em um processo ou outro aplicativo, em vez de hospedado em um servidor web). Para atualizar um projeto auto-host criado com o SignalR 1. x, remova o pacote Microsoft.AspNet.SignalR.Owin e adicionar o pacote Microsoft.AspNet.SignalR.SelfHost. Para obter mais informações sobre como começar com o próprio host do pacote, consulte [Tutorial: SignalR auto-host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Suporte do servidor compatível

Em versões anteriores do SignalR, as versões do pacote SignalR usados no cliente e servidor precisam ser idênticos. Para oferecer suporte a aplicativos cliente espesso que seriam difícil de atualizar, SignalR 2.0 agora dá suporte usando uma versão mais recente do servidor com um cliente mais antigo. **Observação: SignalR 2.0 não oferece suporte a servidores criados com versões anteriores com clientes mais recentes.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Removeu o suporte do servidor para o .NET 4.0

SignalR 2.0 caiu suporte para interoperabilidade de servidor com o .NET 4.0. .NET 4.5 deve ser usado com servidores SignalR 2.0. Ainda é um cliente .NET 4.0 para o SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Enviando uma mensagem para uma lista de clientes e grupos

SignalR 2.0, é possível enviar uma mensagem usando uma lista de IDs de grupos e de cliente. Os trechos de código a seguir demonstram como fazer isso.

**Enviando uma mensagem para uma lista de clientes e grupos usando PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Enviando uma mensagem para uma lista de clientes e grupos de Hubs**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Enviando uma mensagem para um usuário específico

Esse recurso permite que os usuários especifiquem qual é a ID do usuário com base em um IRequest por meio de uma nova interface IUserIdProvider:

**A interface IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

Por padrão, haverá uma implementação que usa IPrincipal.Identity.Name do usuário como o nome de usuário.

Em hubs, você poderá enviar mensagens para esses usuários por meio de uma nova API:

**Usando a API Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Melhor suporte a manipulação de erro

Os usuários agora podem gerar **HubException** de qualquer invocação de hub. O construtor do **HubException** pode levar a uma mensagem de cadeia de caracteres e um objeto de dados extras de erro. SignalR irá serializar automaticamente a exceção e enviá-lo para o cliente onde ele será usado para a invocação de método do hub de rejeitar/falha.

O **Mostrar exceções de hub detalhados** configuração não tem suporte no **HubException** que está sendo enviada de volta ao cliente ou não; ele é sempre enviado.

**Código do lado do servidor demonstrando enviando um HubException ao cliente**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Código de cliente JavaScript demonstrando respondendo a um HubException enviado do servidor**

[!code-html[Main](release-notes/samples/sample16.html)]

**Código de cliente .NET demonstrando respondendo a um HubException enviado do servidor**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Unidade mais fácil de teste de hubs

SignalR 2.0 inclui uma interface chamada `IHubCallerConnectionContext` em Hubs que torna mais fácil criar invocações do lado de cliente de simulação. Os trechos de código a seguir demonstram o uso desta interface com o aproveitamento do teste populares [xUnit.net](https://github.com/xunit/xunit) e [moq](https://code.google.com/p/moq/).

**Testes de unidade SignalR com xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Testes de unidade SignalR com moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Tratamento de erros de JavaScript

SignalR 2.0, todos os retornos de chamada de tratamento de erros de JavaScript retornam objetos de erro de JavaScript em vez de cadeias de caracteres brutos. Isso permite que o SignalR para fluxo de informações mais ricas para os manipuladores de erro. Você pode obter a exceção interna do `source` propriedade do erro.

**Código de cliente JavaScript que manipula a exceção Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Novo sistema de associação do ASP.NET

Identidade do ASP.NET é o novo sistema de associação para aplicativos ASP.NET. Identidade do ASP.NET facilita a integrar dados de perfil de usuário específico com dados de aplicativo. Identidade do ASP.NET também permite que você escolha o modelo de persistência para os perfis de usuário em seu aplicativo. Você pode armazenar os dados em um banco de dados do SQL Server ou outro repositório de dados, incluindo dados NoSQL, como tabelas de armazenamento do Azure. Para obter mais informações, consulte [contas de usuário individuais](creating-web-projects-in-visual-studio.md#indauth) na **criar projetos da Web do ASP.NET no Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticação baseada em declarações

Agora, o ASP.NET oferece suporte à autenticação baseada em declarações, onde a identidade do usuário é representada como um conjunto de declarações de um emissor confiável. Os usuários podem ser autenticados usando um nome de usuário e senha mantidos em um banco de dados de aplicativo ou usando provedores de identidade social (por exemplo: Accounts da Microsoft, Facebook, Google, Twitter), ou usando as contas organizacionais por meio do Active Directory do Azure ou Serviços de Federação do Active Directory (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integração com o Active Directory do Azure e Active Directory do Windows Server

Agora você pode criar projetos do ASP.NET que usam o Active Directory do Azure ou o Windows Server Active Directory (AD) para autenticação. Para obter mais informações, consulte [contas organizacionais](creating-web-projects-in-visual-studio.md#orgauth) na **criar projetos da Web do ASP.NET no Visual Studio 2013**.

### <a name="owin-integration"></a>Integração de OWIN

A autenticação do ASP.NET baseia-se agora a owin que pode ser usado em qualquer host do OWIN. Para obter mais informações sobre o OWIN, consulte o seguinte [componentes do Microsoft OWIN](#TOC7) seção.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componentes do Microsoft OWIN

[Abra a Interface da Web para .NET](http://owin.org/) (OWIN) define uma abstração entre os servidores de web do .NET e aplicativos da web. OWIN separa o aplicativo web do servidor, tornando o host de aplicativos da web independente. Por exemplo, você pode hospedar um aplicativo web com base em OWIN no IIS ou Self hospedá-lo em um processo personalizado.

Alterações nos componentes do Microsoft OWIN (também conhecido como o projeto Katana) incluem novos componentes de servidor e host, novas bibliotecas de auxiliar e middleware e novo middleware de autenticação.

Para obter mais informações sobre OWIN e Katana, consulte [o que há de novo no OWIN e Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Observação: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplicativos não podem ser executado no modo clássico do IIS; eles devem ser executados no modo integrado.**

**Observação: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplicativos devem ser executados em confiança total.**

### <a name="new-servers-and-hosts"></a>Hosts e servidores de novo

Com esta versão, os novos componentes foram adicionados para habilitar cenários de hospedagem interna. Esses componentes incluem os seguintes pacotes do NuGet:

- **Microsoft.Owin.Host.HttpListener**. Fornece um servidor OWIN que usa **HttpListener** para escutar solicitações HTTP e encaminhá-los no pipeline OWIN.
- **Hosting** fornece uma biblioteca para desenvolvedores que desejam hospedagem interna de um pipeline OWIN em um processo personalizado, como um aplicativo de console ou o serviço do Windows.
- **OwinHost**. Fornece um executável autônomo que encapsula `Microsoft.Owin.Hosting` e lhe permite hospedar internamente um pipeline OWIN sem precisar escrever um aplicativo de host personalizado.

Além disso, o `Microsoft.Owin.Host.SystemWeb` middleware fornecer dicas ao pacote agora permite que o **SystemWeb** servidor, indicando que o middleware deve ser chamado durante um estágio específico de pipeline do ASP.NET. Esse recurso é particularmente útil para middleware de autenticação, que deve ser executados no início do pipeline do ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Middleware e bibliotecas auxiliares

Embora seja possível escrever componentes OWIN usando apenas as definições de função e o tipo da especificação de OWIN, o novo `Microsoft.Owin` pacote fornece um conjunto de abstrações de mais amigável. Esse pacote combina vários pacotes anteriores (por exemplo, `Owin.Extensions`, `Owin.Types`) em um modelo de objeto único, bem estruturados que pode ser facilmente usado por outros componentes do OWIN. Na verdade, a maioria dos componentes do Microsoft OWIN agora usar este pacote.

> [!NOTE]
> [OWIN](http://www.owin.org) aplicativos não podem ser executado no modo clássico do IIS; eles devem ser executados no modo integrado.

> [!NOTE]
> [OWIN](http://www.owin.org) aplicativos devem ser executados em confiança total.

Esta versão também inclui o pacote pt, que inclui o middleware para validar um aplicativo OWIN em execução, além de middleware de página de erro para ajudar a investigar falhas.

### <a name="authentication-components"></a>Componentes de autenticação

Os seguintes componentes de autenticação estão disponíveis.

- **Microsoft.Owin.Security.ActiveDirectory**. Habilita a autenticação usando os serviços de diretório local ou baseado em nuvem.
- **Owin** habilita a autenticação de uso de cookies. Este pacote era denominado `Microsoft.Owin.Security.Forms`.
- **Owin** habilita a autenticação usando o serviço de com base em OAuth do Facebook.
- **Owin** habilita a autenticação usando o serviço do Google OpenID.
- **Owin** habilita a autenticação usando tokens JWT.
- **MicrosoftAccount** habilita a autenticação usando contas da Microsoft.
- **Microsoft.Owin.Security.OAuth**. Fornece um servidor de autorização de OAuth, bem como o middleware de autenticação de tokens de portador.
- **Owin** habilita a autenticação usando o serviço do Twitter OAuth.

Esta versão também inclui o `Microsoft.Owin.Cors` pacote, que contém o middleware para processamento de solicitações HTTP entre origens.

> [!NOTE]
> Suporte para a assinatura de JWT foi removido na versão final do Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Para obter uma lista de novos recursos e outras alterações no Entity Framework 6, consulte [histórico de versão do Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 inclui os seguintes novos recursos:

- Suporte para edição de guia. Preivously, o **documento** comando recuo automático e auto formatação no Visual Studio não funcionou corretamente ao usar o **manter tabulações** opção. Essa alteração corrige a formatação de código Razor para o guia de formatação de Visual Studio.
- Suporte para regras de reescrita de URL ao gerar links.
- Remoção do atributo de segurança transparente.
 > [!NOTE]
 > Isso é uma alteração significativa e torna Razor 3 incompatível com MVC4 e anterior, enquanto Razor 2 é incompatível com MVC5 ou assemblies compilados em MVC5.

Problemas de Razor 3 corrigidos no Visual Studio 2013 de versões de pré-lançamento podem ser encontrados [aqui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Suspender o aplicativo ASP.NET

ASP.NET App Suspend é um recurso revolucionária no .NET Framework 4.5.1 que altera radicalmente a experiência do usuário e o modelo econômico para hospedagem grandes números de sites do ASP.NET em um único computador. Para obter mais informações, consulte [ASP.NET App Suspend – responsivo compartilhada de hospedagem web .NET](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

Esta seção descreve problemas conhecidos e alterações significativas no ASP.NET e Web Tools para Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Nova restauração do pacote não funciona em Mono ao usar arquivos SLN](https://nuget.codeplex.com/workitem/3596) – será corrigido em um download nuget.exe futuros e [NuGet.CommandLine pacote](http://www.nuget.org/packages/NuGet.CommandLine/) atualizar.
- [Nova restauração do pacote não funciona com projetos Wix](https://nuget.codeplex.com/workitem/3598) – será corrigido em um download nuget.exe futuros e [NuGet.CommandLine pacote](http://www.nuget.org/packages/NuGet.CommandLine/) atualizar.
- [Restauração de pacote automática não funciona para projetos em uma pasta de solução](https://nuget.codeplex.com/workitem/3625) – será corrigido em 2.8 NuGet.

### <a name="aspnet-web-api"></a>API da Web do ASP.NET

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)`não retorna `IQueryable<T>` sempre, como adicionamos suporte para `$select` e `$expand`.

    Nossos exemplos anteriores para `ODataQueryOptions<T>` sempre converter o valor de retorno de `ApplyTo` para `IQueryable<T>`. Isso trabalhado anteriormente porque as opções de consulta que é suportado anteriormente (`$filter`, `$orderby`, `$skip`, `$top`) não altere a forma da consulta. Agora que nós oferecemos suporte `$select` e `$expand` o valor de retorno `ApplyTo` não poderá ser `IQueryable<T>` sempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Se você estiver usando o código de exemplo anteriores, ele continuarão a funcionar se o cliente não enviar `$select` e `$expand`. No entanto, se você deseja dar suporte `$select` e `$expand` você precisa alterar o código para isso.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url ou RequestContext.Url é null durante uma solicitação em lote**

    Em um cenário de envio em lote, **UrlHelper** é nulo quando acessado a partir do **Request.Url** ou **RequestContext.Url**.

    Esse problema é controlado no momento aqui: [BatchRequestContext.Url é nula para a solicitação de envio em lote](http://aspnetwebstack.codeplex.com/workitem/1301).

    A solução alternativa para esse problema é criar uma nova instância da **UrlHelper**, conforme mostrado no exemplo a seguir:

    **Criando uma nova instância da UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Ao usar MVC5 e OrgAuth, se você tiver modos de exibição que realizam validação AntiForgerToken, você pode se deparar com o seguinte erro quando você publicar dados para o modo de exibição:

    **Erro**:

    *Erro de servidor no aplicativo '/'.*

    *Uma declaração de tipo 'http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier' ou 'http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider' não estava presente no ClaimsIdentity fornecido. Para habilitar o suporte de token antifalsificação com autenticação baseada em declarações, verifique se que o provedor de declarações configurado é fornecer dessas declarações em instâncias ClaimsIdentity gera. Se o provedor de declarações configurado em vez disso, usa um tipo de declaração diferente como um identificador exclusivo, ele pode ser configurado definindo a propriedade estática antiforgeryconfig. Uniqueclaimtypeidentifier.*

    **Solução alternativa**:

    Adicione a seguinte linha no global. asax para corrigi-lo:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Isso será corrigido para a próxima versão.
2. Depois de atualizar um aplicativo MVC4 para MVC5, compile a solução e iniciá-lo. Você deve ver o seguinte erro:

    [A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection. Digite uma se origina de ' webpages, versão = 2.0.0.0, cultura = neutral, PublicKeyToken = 31bf3856ad364e35' no contexto 'Default' no local ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v 4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Tipo B se origina de ' webpages, versão = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' no contexto 'Default' local ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Para corrigir o erro acima, abra *todos os* arquivos Web. config (incluindo aqueles na pasta modos de exibição) no seu projeto e faça o seguinte:

    1. Atualize todas as ocorrências da versão "4.0.0.0" de "System.Web.Mvc" para "5.0.0.0".
    2. Atualizar todas as ocorrências de "2.0.0.0" versão "Helpers" &quot;System.Web.WebPages&quot; e &quot;webpages&quot; para "3.0.0.0"

    Por exemplo, depois de fazer as alterações acima, as associações do assembly devem ser assim:

    [!code-xml[Main](release-notes/samples/sample24.xml)]

    Para obter informações sobre como atualizar projetos MVC 4 a 5 do MVC, consulte [como atualizar um ASP.NET MVC 4 e o projeto de API da Web para ASP.NET MVC 5 e Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Ao usar a validação do lado do cliente com jQuery validação discreto, a mensagem de validação, às vezes, é incorreta para um elemento HTML de entrada com tipo = 'número'. O erro de validação para um valor obrigatório ("Idade o campo é obrigatório") é mostrado quando um número inválido é inserido em vez da mensagem correta, que é necessário um número válido.

    Esse problema é comumente encontrado com o código de scaffolding para um modelo com uma propriedade de inteiro as criar e editar modos de exibição.

    Para contornar esse problema, altere o auxiliar do editor de:

    `@Html.EditorFor(person => person.Age)`

    Para:

    `@Html.TextBoxFor(person => person.Age)`
4. O ASP.NET MVC 5 não dá suporte a confiança parcial. Projetos de vinculação para os binários do MVC ou WebAPI devem remover o [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) atributo e o [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) atributo. Remover esses atributos elimina erros de compilador como a seguir.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Observe que, como um efeito colateral disso, você não pode usar assemblies 4.0 e 5.0 no mesmo aplicativo. Você precisa atualizar todos eles para 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Modelo do SPA com autorização de Facebook pode causar instabilidade no IE, enquanto o site da web é hospedado na zona da intranet

O modelo do SPA fornece log externo com o Facebook. Quando o projeto criado com o modelo está em execução localmente, entrar pode causar IE falhar.

Solução:

1. Hospedar o site da web na zona da internet; ou

2. Teste o cenário em um navegador que não seja o Internet Explorer.

### <a name="web-forms-scaffolding"></a>Estrutura de formulários da Web

Scaffolding de formulários da Web foi removido do VS2013 e estarão disponível em uma atualização futura para o Visual Studio. No entanto, você ainda pode usar o scaffolding dentro de um projeto Web Forms Adicionando dependências MVC e gera a estrutura do MVC. Seu projeto contém uma combinação de Web Forms e MVC.

Para adicionar o MVC ao projeto Web Forms, adicionar um novo item de scaffolding e selecione **MVC 5 dependências**. Selecione mínimo ou completo dependendo se é necessário que todos os arquivos de conteúdo, como scripts. Em seguida, adicione um item de scaffolding para MVC, que irá criar modos de exibição e um controlador em seu projeto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC e Web API Scaffolding - HTTP 404 não encontrado erro

Se for encontrado um erro ao adicionar um item de scaffolding para um projeto, é possível que seu projeto será deixado em um estado inconsistente. Algumas das alterações feitas a ser scaffolding serão revertidas, mas outras alterações, como os pacotes do NuGet instaladas, serão não ser revertidas. Se as alterações de configuração de roteamento são revertidas, os usuários receberão um erro HTTP 404 quando navegar para Scaffold itens.

Solução alternativa:

- Para corrigir esse erro para MVC, adicionar um novo item de scaffolding e selecione as dependências do MVC 5 (mínimo ou completo). Esse processo adicionará todas as alterações necessárias ao seu projeto.
- Para corrigir esse erro para a API da Web:

    1. Adicione a classe WebApiConfig ao seu projeto.

        [!code-csharp[Main](release-notes/samples/sample25.cs)]

        [!code-vb[Main](release-notes/samples/sample26.vb)]
    2. Configure o aplicativo WebApiConfig.Register\_iniciar método em global. asax, da seguinte maneira:

        [!code-csharp[Main](release-notes/samples/sample27.cs)]

        [!code-vb[Main](release-notes/samples/sample28.vb)]
