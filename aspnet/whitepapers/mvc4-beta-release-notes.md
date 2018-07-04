---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este documento descreve a versão do ASP.NET MVC 4 Beta para Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: ''
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 62bd641b1c5c926ec33ed3948854365aea99e979
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390602"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Este documento descreve a versão do ASP.NET MVC 4 Beta para Visual Studio 2010.
> 
> > [!NOTE]
> > Isso não é a versão mais atual. As notas de versão RC do ASP.NET MVC 4 estão disponíveis [aqui](mvc4-release-notes.md).


- [Notas de instalação](#_Toc303253802)
- [Documentação](#_Toc303253803)
- [Suporte](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Atualizando um projeto do ASP.NET MVC 3 para o ASP.NET MVC 4](#_Toc303253806)
- [Novos recursos na versão Beta do ASP.NET MVC 4](#_Toc303253807)

    - [API Web ASP.NET](#_Toc317096197)
    - [Aplicativo de página única do ASP.NET](#_Toc317096198)
    - [Aprimoramentos para modelos de projeto padrão](#_Toc303253808)
    - [Modelo de projeto móvel](#_Toc303253809)
    - [Modos de exibição](#_Toc303253810)
    - [jQuery Mobile, o alternador de exibições e substituindo o navegador](#_Toc303253811)
    - [Receitas para a geração de código no Visual Studio](#_Toc303253812)
    - [Suporte de tarefa para controladores assíncronos](#_Toc303253813)
    - [SDK do Azure](#_Toc303253814)
    - [Problemas conhecidos e alterações recentes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de instalação

ASP.NET MVC 4 Beta para Visual Studio 2010 pode ser instalado a partir de [homepage do ASP.NET MVC 4](../mvc/mvc4.md) usando o Web Platform Installer.

Você deve desinstalar quaisquer versões prévias instaladas anteriormente do ASP.NET MVC 4 antes de instalar o ASP.NET MVC 4 Beta.

Esta versão não é compatível com a visualização do desenvolvedor do .NET Framework 4.5. Você deve desinstalar a visualização do desenvolvedor do .NET 4.5 antes de instalar o ASP.NET MVC 4 Beta.

ASP.NET MVC 4 pode ser instalado e pode ser executado lado a lado com o ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentação

Documentação do ASP.NET MVC está disponível no site da MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página do site do ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Suporte

Essa é uma versão de visualização e não é oficialmente suportada. Se você tiver dúvidas sobre como trabalhar com esta versão, poste-os para o fórum do ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), onde os membros da comunidade do ASP.NET são com frequência pode oferecer suporte informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes do ASP.NET MVC 4 para Visual Studio exigem o PowerShell 2.0 e o Visual Studio 2010 com Service Pack 1 ou o Visual Web Developer Express 2010 com Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Atualizando um projeto do ASP.NET MVC 3 para o ASP.NET MVC 4

ASP.NET MVC 4 pode ser instalado lado a lado com o ASP.NET MVC 3 no mesmo computador, que lhe dá flexibilidade na escolha quando atualizar um aplicativo ASP.NET MVC 3 para o ASP.NET MVC 4.

A maneira mais simples de atualização é para criar um novo projeto ASP.NET MVC 4 e copiar todos os os modos de exibição, controladores, código e conteúdo arquivos de projeto MVC 3 existente para o novo projeto e, em seguida, atualizar o assembly faz referência no novo projeto para corresponder ao projeto antigo. Se você fez alterações no arquivo Web. config no projeto do MVC 3, você também deve mesclar essas alterações no arquivo Web. config no projeto do MVC 4.

Para atualizar manualmente um aplicativo existente do ASP.NET MVC 3 para a versão 4, faça o seguinte:

1. Em todos os arquivos Web. config no projeto (há um na raiz do projeto, um na pasta modos de exibição e um na pasta modos de exibição para cada área em seu projeto), substitua cada ocorrência do texto a seguir:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    com o seguinte texto correspondente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. No arquivo Web. config raiz, atualize o *webPages:Version* elemento para "2.0.0.0" e adicione um novo *PreserveLoginUrl* chave que tem o valor "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. No Solution Explorer, exclua a referência a *System.Web.Mvc* (que aponta para a versão 3 DLL). Em seguida, adicione uma referência a *System.Web.Mvc* (v4.0.0.0). Em particular, faça as seguintes alterações para atualizar as referências de assembly. Aqui estão os detalhes:

    1. No Solution Explorer, exclua as referências aos assemblies a seguir: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Adicione uma referência aos assemblies a seguir: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. No Gerenciador de soluções, clique com botão direito no nome do projeto e, em seguida, selecione descarregar projeto. Em seguida, clique no nome novamente e selecione Editar *ProjectName*. csproj.
5. Localize o *ProjectTypeGuids* elemento e substitua {E53F8FEA-EAE0-44A6-8774-FFD645390401} por {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salve as alterações, feche o arquivo de projeto (. csproj) que você estava editando, clique com botão direito no projeto e, em seguida, selecione Recarregar projeto.
7. Se o projeto faz referência a quaisquer bibliotecas de terceiros que são compiladas usando versões anteriores do ASP.NET MVC, abra o arquivo Web. config de raiz e adicione as três seguintes *bindingRedirect* elementos sob o  *configuração* seção: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Novos recursos na versão Beta do ASP.NET MVC 4

Esta seção descreve os recursos que foram introduzidos na versão Beta do ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API da Web do ASP.NET

ASP.NET MVC 4 agora inclui a API Web ASP.NET, uma nova estrutura para criar serviços HTTP que pode acessar uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. API Web ASP.NET também é uma plataforma ideal para criar serviços RESTful.

API Web ASP.NET inclui suporte para os seguintes recursos:

- **Modelo de programação HTTP moderno:** diretamente acessar e manipular solicitações HTTP e respostas em suas APIs Web usando um modelo de objeto novo e com rigidez de tipos do HTTP. O mesmo modelo de programação e pipeline de HTTP é simetricamente disponível no cliente por meio do novo tipo HttpClient.
- **Suporte total para rotas**: as APIs da Web agora dão suporte ao conjunto completo de recursos de rota que sempre foram uma parte da pilha da Web, incluindo os parâmetros de rota e restrições. Além disso, mapeamento para ações tem suporte completo para as convenções, portanto, você não precisa aplicar atributos como [HttpPost] às classes e métodos.
- **Negociação de conteúdo**: O cliente e servidor podem trabalhar juntos para determinar o formato correto para os dados retornados de uma API. Fornecemos suporte padrão para XML, JSON e formatos codificada em URL do formulário, e você pode estender esse suporte, adicionando seus próprios formatadores ou até mesmo substituir a estratégia de negociação de conteúdo padrão.
- **Associação de modelo e validação:** associadores de modelo fornecem uma maneira fácil para extrair dados de várias partes de uma solicitação HTTP e converter essas partes da mensagem em objetos .NET que podem ser usados pelas ações de API da Web.
- **Filtros:** APIs da Web agora dá suporte a filtros, incluindo filtros bem conhecidos, como o atributo [autorizar]. Você pode criar e conectar seus próprios filtros de ações, autorização e manipulação de exceção.
- **Composição de consultas:** com o simples retorno IQueryable&lt;T&gt;, sua API da Web dará suporte a consulta por meio de convenções de URL do OData.
- **Melhor capacidade de teste dos detalhes HTTP:** em vez de definir os detalhes HTTP em objetos de contexto estático, ações de API Web agora podem trabalhar com instâncias de HttpRequestMessage e HttpResponseMessage. Também existem versões genéricas desses objetos para permitir que você trabalhe com seus tipos personalizados além dos tipos de HTTP.
- **Aprimorado de IoC (inversão controle) por meio do DependencyResolver:** API Web agora usa o padrão de localizador de serviço implementado pelo resolvedor de dependência do MVC para obter as instâncias para muitos recursos diferentes.
- **Configuração baseada em código:** configuração da API da Web é feita somente por meio de código, deixando sua configuração de limpeza de arquivos.
- **Hospedar internamente:** APIs da Web podem ser hospedadas em seu próprio processo além do IIS enquanto estiver usando todo o potencial de rotas e outros recursos da API da Web.

Para obter mais detalhes sobre a API Web do ASP.NET, visite [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Aplicativo de página única do ASP.NET

Agora, o ASP.NET MVC 4 inclui uma visualização prévia da experiência de criação de aplicativos de única página com interações significativas do lado do cliente usando JavaScript e APIs da Web. Esse suporte inclui:

- Um conjunto de bibliotecas de JavaScript para interações de locais mais ricas com os dados armazenados em cache
- Componentes de API da Web adicionais para a unidade de trabalho e suporte DAL
- Um modelo de projeto do MVC com o scaffolding para se familiarizar rapidamente

Para obter mais detalhes sobre o aplicativo de página única de suporte no ASP.NET MVC 4, visite [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Aprimoramentos para modelos de projeto padrão

O modelo que é usado para criar novos projetos do ASP.NET MVC 4 foi atualizado para criar um site de aparência mais moderna:

![](mvc4-beta-release-notes/_static/image1.png)

Além das melhorias superficiais, aumenta a funcionalidade no novo modelo. O modelo emprega uma técnica chamada renderização adaptável para ter boa em navegadores de desktop e navegadores móveis sem nenhuma personalização.

![](mvc4-beta-release-notes/_static/image2.png)

Para ver a renderização adaptável em ação, você pode usar um emulador móvel ou apenas tente redimensionar a janela da área de trabalho para um tamanho menor. Quando a janela do navegador é pequena o suficiente, o layout da página será alterado.

Outro aprimoramento para o modelo de projeto padrão é o uso de JavaScript para fornecer uma interface do usuário mais avançada. Os links de logon e registre-se que são usados no modelo são exemplos de como usar a caixa de diálogo de interface do usuário do jQuery para apresentar uma tela de logon avançadas:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modelo de projeto móvel

Se você estiver começando um novo projeto e deseja criar um site especificamente para dispositivos móveis e navegadores de tablet, você pode usar o novo modelo de projeto de aplicativo móvel. Isso é baseado no jQuery Mobile, uma biblioteca de código-fonte aberto para a criação de interface do usuário otimizada para toque:

![](mvc4-beta-release-notes/_static/image4.png)

Este modelo contém a mesma estrutura de aplicativo que o modelo de aplicativo de Internet (e o código do controlador é praticamente idêntico), mas é estilizadas usando jQuery Mobile boa aparência e se comportem corretamente em dispositivos móveis baseados em toque. Para saber mais sobre como estruturar e definir o estilo de interface do usuário móvel, consulte o [site do projeto móvel de jQuery](http://jquerymobile.com/).

Se você já tiver um site orientado a área de trabalho que você deseja adicionar exibições otimizadas para mobilidade a, ou se você quiser criar um único site que serve de modos de exibição com um estilo diferente aos navegadores da área de trabalho e móveis, você pode usar o novo recurso de modos de exibição. (Consulte a próxima seção).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de exibição

O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está fazendo a solicitação. Por exemplo, se um navegador da área de trabalho solicita a página inicial, o aplicativo pode usar o modelo Views\Home\Index.cshtml. Se um navegador móvel solicita a página inicial, o aplicativo pode retornar o modelo Views\Home\Index.mobile.cshtml.

Layouts e parciais também podem ser substituídos para tipos de navegador específico. Por exemplo:

- Se sua pasta Views\Shared contém ambas as \_layout. cshtml e \_cshtml modelos, por padrão, o aplicativo usará \_cshtml durante solicitações de navegadores para dispositivos móveis e de \_Layout. cshtml durante outras solicitações.
- Se uma pasta contém ambos \_MyPartial.cshtml e \_MyPartial.mobile.cshtml, a instrução @Html.Partial("\_MyPartial") será renderizado \_MyPartial.mobile.cshtml durante as solicitações do mobile navegadores, e \_MyPartial.cshtml durante a outras solicitações.

Se você quiser criar exibições mais específicas, layouts ou modos de exibição parciais para outros dispositivos, você pode registrar um novo *DefaultDisplayMode* instância para especificar qual nome a ser pesquisado quando uma solicitação atende a condições específicas. Por exemplo, você poderia adicionar o código a seguir para o *Application\_iniciar* método no arquivo global. asax para registrar a cadeia de caracteres "iPhone" como um modo de exibição que se aplica quando o navegador do iPhone da Apple faz uma solicitação:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Depois que esse código é executado, quando um navegador do iPhone da Apple faz uma solicitação, o aplicativo usará o Views\Shared\\_Layout.iPhone.cshtml layout (se houver).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, o alternador de exibições e substituindo o navegador

jQuery Mobile é uma biblioteca de código-fonte aberto para a criação de interface do usuário web otimizada para toque. Se você quiser usar o jQuery Mobile com um aplicativo ASP.NET MVC 4, você pode baixar e instalar um pacote do NuGet que ajuda você a começar a usar. Para instalá-lo no Visual Studio Package Manager Console, digite o seguinte comando:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Isso instala jQuery Mobile e alguns arquivos auxiliares, incluindo o seguinte:

- Views/Shared/\_cshtml, que é um layout com base em Mobile do jQuery.
- Um componente do alternador de exibição, que consiste deViews/Shared/\_ViewSwitcher.cshtml de exibição parcial e o controlador de ViewSwitcherController.cs.

Depois de instalar o pacote, execute seu aplicativo usando um navegador móvel (ou equivalente, como o Firefox [alternador de agente do usuário](http://chrispederick.com/work/user-agent-switcher/) complemento). Você verá que suas páginas têm aparências bastante diferentes, como jQuery Mobile manipula o layout e estilo. Para tirar proveito disso, você pode fazer o seguinte:

- Criar substituições de exibição móveis específicas, conforme descrito em [modos de exibição](#_Toc303253810) anteriormente (por exemplo, criar Views\Home\Index.mobile.cshtml para substituir Views\Home\Index.cshtml para navegadores móveis).
- Leia as [jQuery Mobile documentação](http://jquerymobile.com/) para saber mais sobre como adicionar elementos de interface do usuário otimizada para toque nos modos de exibição móveis.

Uma convenção para as páginas de web otimizada para celular é adicionar um link cujo texto é algo como o modo de exibição da área de trabalho ou o modo completo do site que permite aos usuários mudar para uma versão da área de trabalho da página. O pacote jQuery.Mobile.MVC inclui um exemplo de componente do alternador de exibição para essa finalidade. Ele é usado no padrão Views\Shared\\exibição cshtml, e ele se parece com isso quando a página é renderizada:

![](mvc4-beta-release-notes/_static/image5.png)

Se os visitantes clicarem no link, eles estiverem alternados para a versão da área de trabalho da mesma página.

Porque o layout da área de trabalho não incluirá um alternador de exibição por padrão, os visitantes não terão uma maneira de chegar ao modo móvel. Para habilitar isso, adicione a seguinte referência à  *\_ViewSwitcher* ao seu layout da área de trabalho, apenas dentro de *&lt;corpo&gt;* elemento:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Alternância de exibição usa um novo recurso chamado de substituição do navegador. Esse recurso permite que seu aplicativo lide com solicitações como se eles foram provenientes de um navegador diferente (agente do usuário) daquele que elas realmente da. A tabela a seguir lista os métodos de substituição do navegador fornece.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Substitui o valor do agente de usuário real da solicitação usando o agente de usuário especificado. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Retorna o valor de substituição do agente de usuário da solicitação ou a cadeia de caracteres de agente do usuário real se nenhuma substituição tiver sido especificada. |
| `HttpContext.GetOverriddenBrowser()` | Retorna um *HttpBrowserCapabilitiesBase* instância que corresponde ao agente do usuário definido atualmente para a solicitação (reais ou substituído). Você pode usar esse valor para obter as propriedades, como *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Remove qualquer agente do usuário substituído para a solicitação atual. |

Substituição do navegador é um recurso fundamental do ASP.NET MVC 4 e está disponível, mesmo se você não instalar o pacote jQuery.Mobile.MVC. No entanto, ele afeta somente exibição, layout e seleção de exibição parcial — ele não afeta qualquer outro recurso do ASP.NET que depende do *Request. browser* objeto.

Por padrão, a substituição de agente do usuário é armazenada usar um cookie. Se você quiser armazenar a substituição em outro lugar (por exemplo, em um banco de dados), você pode substituir o provedor padrão (*BrowserOverrideStores.Current*). Documentação para esse provedor estarão disponível para acompanhar uma versão posterior do ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Receitas para a geração de código no Visual Studio

O novo recurso de receitas permite que o Visual Studio gerar o código específico da solução com base em pacotes que podem ser instalados usando o NuGet. A estrutura de receitas torna mais fácil para os desenvolvedores escrevam plugins de geração de código, que você também pode usar para substituir os geradores de código interno para adicionar área, adicionar controlador e adicionar modo de exibição. Como receitas são implantadas como pacotes NuGet, podem facilmente ser verificadas no controle do código-fonte e compartilhados com todos os desenvolvedores no projeto automaticamente. Eles também estão disponíveis em uma base por solução.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Suporte de tarefa para controladores assíncronos

Agora você pode escrever métodos de ação assíncrono como uma única métodos que retornam um objeto do tipo *tarefa* ou *tarefa&lt;ActionResult&gt;*.

Por exemplo, se você estiver usando o Visual c# 5 (ou usando o [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), você pode criar um método de ação assíncrono que é semelhante ao seguinte:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

No método de ação anteriores, as chamadas para *newsService.GetHeadlinesAsync* e *sportsService.GetScoresAsync* são chamados de forma assíncrona e bloqueia um thread do pool de threads.

Métodos de ação assíncrono que retornam *tarefa* instâncias também podem dar suporte a tempos limite. Para tornar o seu método de ação cancelável, adicione um parâmetro do tipo *CancellationToken* à assinatura do método de ação. O exemplo a seguir mostra um método de ação assíncrono que tem um tempo limite de 2500 milissegundos e que exibe uma *TimedOut* exibir para o cliente se ocorrer um tempo limite.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK do Azure

ASP.NET MVC 4 Beta suporta a versão 1.5 de setembro de 2011 do SDK do Windows Azure.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

- **Depois de instalar a versão Beta do ASP.NET MVC 4, o editor CSHTML/VBHTML no editor do Visual Studio 2010 Service Pack 1 CSHTML/VBHTML pode pausar por um longo tempo depois de digitar o trecho de código ou JavaScript nos arquivos cshtml ou vbhtml.** Isso ocorre apenas em aplicativos ASP.NET MVC 4 que acabou de ser criados e ainda não foram compilados.

    A solução alternativa é compilar o projeto para colocar os assemblies na pasta bin. Observe que, se você limpar o projeto que remove os assemblies da pasta bin, o problema de editor será voltar.

    Isso será corrigido na próxima versão.
- **Modelos de projeto c# para Visual Studio 11 Beta contêm uma cadeia de caracteres de conexão incorretas em Global.asax.cs.** A conexão padrão especificado no aplicativo\_método Start para projetos criados no Visual Studio 11 Beta contêm uma cadeia de caracteres de conexão LocalDB que contém uma barra invertida sem escape (\) caractere. Isso resulta em um erro de conexão após tentativas de acessar um DbContext do Entity Framework, que gera uma SqlException.

    Para corrigir esse problema, escapar do caractere de barra invertida no aplicativo\_início – método de Global.asax.cs para exibi-lo da seguinte maneira:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Aplicativos ASP.NET MVC 4 cujo destino .NET 4.5 lançará um FileLoadException após a tentativa de acessar o assembly System.Net.Http.dll quando executado no .NET 4.0.** Aplicativos ASP.NET MVC 4 criados no .NET 4.5 contêm uma associação de redirecionamento que resulta em um FileLoadException que que afirma "não foi possível carregar arquivo ou assembly 'Http' ou uma de suas dependências." Quando o aplicativo é executado em um sistema com o .NET 4.0 instalado. Para corrigir esse problema, remova o seguinte redirecionamento de associação do Web. config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    O elemento de associação de assembly na Web. config modificado deve aparecer da seguinte maneira:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>O modelo de item de "Adicionar controlador" em projetos do Visual Basic gera um namespace incorreto quando invocado</strong><strong>de dentro de uma área.</strong> Quando você adiciona um controlador para uma área em um projeto do ASP.NET MVC que usa o Visual Basic, o modelo de item insere o controlador ao namespace errado. O resultado é um erro de "arquivo não encontrado" quando você navegar para qualquer ação no controlador.  
  
  O namespace gerado omite tudo após o namespace raiz. Por exemplo, é o namespace gerado *RootNamespace* , mas deve ser *RootNamespace.Areas.AreaName.Controllers* .
- **Alterações significativas no mecanismo de exibição do Razor.** Como parte de uma reescrita do analisador Razor, os seguintes tipos foram removidos da *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Os métodos a seguir também foram removidos: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Quando WebMatrix.WebData.dll é incluído no diretório /bin de um aplicativos ASP.NET MVC 4, ele assume a URL para a autenticação de formulários.** Adicionando o assembly WebMatrix.WebData.dll ao seu aplicativo (por exemplo, selecionando "ASP.NET páginas da Web com sintaxe do Razor" ao usar a caixa de diálogo Adicionar dependências implantáveis) substituirá o redirecionamento de logon de autenticação para o logon/conta/vez / conta/logon conforme o esperado pelo controlador de conta do ASP.NET MVC padrão. Para evitar esse comportamento e usar a URL especificada já na seção de autenticação da Web. config, você pode adicionar um appSetting chamado PreserveLoginUrl e defini-lo como true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **O Gerenciador de pacotes do NuGet Falha na instalação ao tentar instalar o ASP.NET MVC 4 para instalações lado a lado do Visual Studio 2010 e o Visual Web Developer 2010.** Para executar o Visual Studio 2010 e o Visual Web Developer 2010 lado a lado com o ASP.NET MVC 4, você deve instalar ASP.NET MVC 4 depois que ambas as versões do Visual Studio já foram instaladas.
- **Desinstalar o ASP.NET MVC 4 falhará se os pré-requisitos já tiverem sido desinstalados.** Para desinstalar corretamente o ASP.NET MVC 4you deve desinstalar o ASP.NET MVC 4 antes de desinstalar o Visual Studio.
- **Executar um projeto de API da Web padrão mostra instruções que incorretamente direcionam o usuário adicionar rotas usando o método RegisterApis, que não existe.** As rotas devem ser adicionadas no método RegisterRoutes usando a tabela de rotas do ASP.NET.
- **Instalar o ASP.NET MVC 4 Beta interrompe aplicativos da versão RTM do ASP.NET MVC 3.** Aplicativos ASP.NET MVC 3 que foram criados com o RTM versão (não com a versão de atualização de ferramentas do ASP.NET MVC 3) exigem as seguintes alterações para funcionar lado a lado com o ASP.NET MVC 4 Beta. Criando o projeto sem fazer esses resultados de atualizações em erros de compilação. 

    **Atualizações necessárias**

  1. No arquivo Web. config raiz, adicione um novo *&lt;appSettings&gt;* entrada com a chave *webPages:Version* e o valor *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. No Gerenciador de soluções, clique com botão direito no nome do projeto e, em seguida, selecione descarregar projeto. Em seguida, clique no nome novamente e selecione Editar *ProjectName*. csproj.
  3. Localize as seguintes referências de assembly: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Substitua-os pelo seguinte:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Salve as alterações, feche o arquivo de projeto (. csproj) que você estava editando e, em seguida, clique com botão direito no projeto e selecione Recarregar.
