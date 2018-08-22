---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (inclui de abril de 2011 Tools atualização) O ASP.NET MVC 3 é uma estrutura para criar aplicativos web escalonáveis baseados em padrões, usando o padrão de design bem estabelecidos...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 82d18865815568c5df9768fd9dd403f11ebd1714
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834901"
---
<a name="aspnet-mvc-3"></a>O ASP.NET MVC 3
====================
> *(inclui de abril de 2011 Tools atualização)*
> 
> O ASP.NET MVC 3 é uma estrutura para a criação de aplicativos web escalonáveis baseados em padrões, usando padrões de design bem estabelecidos e o poder do ASP.NET e o .NET Framework.
> 
> Ele é instalado lado a lado com o ASP.NET MVC 2, assim, começar a usá-lo hoje mesmo!
> 
> Baixe o [instalador aqui](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Principais recursos

- Sistema de Scaffolding integrado extensível por meio do NuGet
- Modelos de projeto habilitado 5 HTML
- Modos de exibição expressivos, incluindo o novo mecanismo de exibição do Razor
- Ganchos poderosos com injeção de dependência e os filtros de ação Global
- Suporte a Rich JavaScript com o JavaScript discreto, validação do jQuery e associação de JSON
- *Ler a lista completa de recursos [abaixo](#overview)*

## <a name="top-links"></a>Principais Links de

O que há de novo no ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 lançado](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [mvc3 do ASP.NET, o WebMatrix, o NuGet, IIS Express e Orchard lançado – a versão da Web de janeiro de Microsoft no contexto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [anunciando o lançamento do ASP.NET MVC 3, IIS Express, SQL CE 4, Framework do Web Farm, Orchard, o WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notas de versão do ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalação e a Ajuda

- Instale o ASP.NET MVC 3 usando o [Web Platform Installer (recomendado)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- Instale o ASP.NET MVC 3 usando o [instalador executável](https://go.microsoft.com/fwlink/?LinkID=208140)
- Instalar [ASP.NET MVC 3 para Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Leia o [Introdução ao tutorial do ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtenha ajuda e discutir o ASP.NET MVC 3 no [fóruns](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Visão geral do ASP.NET MVC 3

ASP.NET MVC 3 se baseia no ASP.NET MVC 1 e 2, adicionando os excelentes recursos que ambos simplificam seu código e permitem a extensibilidade mais profunda. Este tópico fornece uma visão geral de muitos dos novos recursos que estão incluídos nesta versão, organizados nas seguintes seções:

- [Scaffolding extensível com a integração de MvcScaffold](#BM_MvcScaffolding)
- [Modelos de projeto habilitado 5 HTML](#BM_HTML5)
- [O mecanismo de exibição do Razor](#BM_TheRazorViewEngine)
- [Suporte para vários mecanismos de exibição](#BM_Support_for_Multiple_View_Engines)
- [Melhorias do controlador](#BM_Controller_Improvements)
- [JavaScript e Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Aprimoramentos de validação de modelo](#BM_Model_Validation_Improvements)
- [Aprimoramentos de injeção de dependência](#BM_Dependency_Injection_Improvements)
- [Outros novos recursos](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Scaffolding extensível com a integração de MvcScaffold

O novo sistema de Scaffolding torna mais fácil para escolher e começar usando de forma produtiva se você estiver totalmente novo para a estrutura e automatizar tarefas comuns de desenvolvimento, se você tiver experiência e já sabe o que está fazendo.

Isso é suportado pelo NuGet novos *scaffolding* pacote chamado **MvcScaffolding**. O termo "Scaffolding" é usado por muitas tecnologias de software para significar "gerar rapidamente uma estrutura de tópicos básica de software que você pode, em seguida, editar e personalizar". O pacote de scaffolding que estamos criando para o ASP.NET MVC é muito benéfico em vários cenários:

- **Se você estiver aprendendo o ASP.NET MVC pela primeira vez**, pois ele fornece uma maneira rápida de obter algum código útil, de trabalho, que, em seguida, você pode editar e adaptar de acordo com suas necessidades. Ele evita que você o trauma de olhando para uma página em branco e não ter ideia de onde começar!
- **Se você conhece bem o ASP.NET MVC e agora está explorando alguma nova tecnologia de complemento** como um mapeador relacional de objeto, um mecanismo de exibição, uma biblioteca de teste, etc., como o criador do que a tecnologia pode também criou um pacote de scaffolding para ele.
- **Se o seu trabalho envolve a criação de repetidamente semelhantes classes ou arquivos de algum tipo**, porque você pode criar os scaffolders personalizados que a saída de Acessórios de teste, scripts de implantação ou qualquer outra coisa que você precisa. Todos em sua equipe podem usar seu scaffolders personalizados, também.

Outros recursos MvcScaffolding incluem:

- Suporte para projetos c# e VB
- Suporte para mecanismos de exibição do Razor e ASPX
- Dá suporte à scaffolding em áreas do ASP.NET MVC e usando layouts/mestres do modo de exibição personalizado
- Você pode personalizar facilmente a saída por meio da edição de modelos T4
- Você pode adicionar os scaffolders totalmente novos usando lógica personalizada do PowerShell e modelos T4 personalizados. Esses (e todos os parâmetros personalizados que forneceu-las) são exibidos automaticamente na lista de conclusão de tabulação do console.
- Você pode obter pacotes do NuGet contendo os scaffolders adicionais para tecnologias diferentes (por exemplo, há um prova de conceito um LINQ para SQL agora) misturar e combiná-los juntos

A atualização das ferramentas do ASP.NET MVC 3 inclui excelente suporte do Visual Studio para este sistema de scaffolding, como:

- Adicione caixa de diálogo de controlador agora dá suporte a scaffolding automático completo de criar, ler, atualizar e excluir ações do controlador e exibições correspondentes. Por padrão, isso aplica Scaffold em código de acesso a dados usando o EF Code First.
- Adiciona suportes de caixa de diálogo do controlador *scaffolds extensíveis* por meio do NuGet, como pacotes *MvcScaffolding*. Isso permite conectar scaffolds personalizadas na caixa de diálogo que permite que você crie scaffolds para outras tecnologias de acesso de dados como NHibernate ou até mesmo JET com ODBCDirect se desejar!

Para obter mais informações sobre o Scaffolding no ASP.NET MVC 3, consulte os seguintes recursos:

- Steve Sanderson lançar a série, incluindo: 

    1. [Introdução: Criar o scaffolding de seu projeto do ASP.NET MVC 3 com o pacote MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Uso padrão: casos típicos de uso e opções](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relações um-para-muitos](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Ações de scaffolding e testes de unidade](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Substituindo os modelos T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Esta postagem: Criando os scaffolders personalizados](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Postagem de Hanselman da sua sessão PDC 2010 [criando um Blog com a Microsoft "Sem nome pacote da Web adoram"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Modelos de projeto 5 HTML

A caixa de diálogo Novo projeto inclui versões de enable HTML 5 uma caixa de seleção de modelos de projeto. Esses modelos de aproveitar o Modernizr 1.7 para fornecer suporte de compatibilidade para HTML 5 e 3 de CSS em navegadores de nível inferior.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>O mecanismo de exibição do Razor

O ASP.NET MVC 3 vem com um novo mecanismo de exibição denominado Razor que oferece os seguintes benefícios:

- Sintaxe do Razor é limpo e conciso, exigindo um número mínimo de pressionamentos de tecla.
- O Razor é fácil de aprender, em parte porque ele se baseia em existentes linguagens como c# e Visual Basic.
- O Visual Studio inclui o IntelliSense e o código colorização de sintaxe do Razor.
- Exibições do Razor podem ter unidades testadas sem exigir que você executa o aplicativo ou iniciar um servidor web.

Alguns recursos novos do Razor incluem o seguinte:

- `@model` sintaxe para especificar o tipo que está sendo passado para o modo de exibição.
- `@* *@` sintaxe de comentário.
- A capacidade de especificar os padrões (como `layoutpage`) uma vez para todo o site.
- O `Html.Raw` método para exibir o texto sem codificação HTML-lo.
- Suporte para compartilhar código entre vários modos de exibição (*\_viewstart. cshtml* ou  *\_viewstart.vbhtml* arquivos).

Razor também inclui novos auxiliares HTML, como o seguinte:

- `Chart`. Processa um gráfico, oferecendo os mesmos recursos como o controle de gráfico no ASP.NET 4.
- `WebGrid`. Renderiza uma grade de dados, completa com a funcionalidade de paginação e classificação.
- `Crypto`. Usa algoritmos para criar corretamente de hash com salt e hash de senhas.
- `WebImage`. Renderiza uma imagem.
- `WebMail`. Envia uma mensagem de email.

Para obter mais informações sobre o Razor, consulte os seguintes recursos:

- [Postagem de blog de Guthrie apresentando o Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Postagem de blog de Guthrie apresentando o @model palavra-chave](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Postagem de blog de Guthrie apresentando layouts do Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Referência rápida da API de Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Suporte para vários mecanismos de exibição

O **adicionar exibição** caixa de diálogo no ASP.NET MVC 3 permite que você escolha o mecanismo de exibição que você deseja trabalhar, e o **novo projeto** caixa de diálogo permite que você especifique o mecanismo de exibição padrão para um projeto. Você pode escolher o mecanismo de exibição de formulários da Web (ASPX), Razor ou um mecanismo de exibição de código-fonte aberto, como [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), ou [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Melhorias do controlador

### <a name="global-action-filters"></a>Filtros de ação global

Às vezes você deseja executar a lógica antes de executa um método de ação ou após a execução de um método de ação. Para dar suporte a isso, o ASP.NET MVC 2 fornecido filtros de ação. Filtros de ação são os atributos personalizados que fornecem uma forma declarativa para adicionar o comportamento de ação pré e pós-ação aos métodos de ação do controlador específico. No entanto, em alguns casos você talvez queira especificar o comportamento de ação pré ou pós-ação que se aplica a todos os métodos de ação. MVC 3 permite que você especifique filtros globais ao adicioná-los para o `GlobalFilters` coleção. Para obter mais informações sobre filtros de ação global, consulte os seguintes recursos:

- [Blog de Guthrie sobre a visualização do MVC 3](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtragem no ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nova propriedade "ViewBag"

Suporte de controladores do MVC 2 um `ViewData` propriedade que permite que você passe dados para um modelo de exibição usando um dicionário de associação tardia API. No MVC 3, você também pode usar a sintaxe um pouco mais simples com o `ViewBag` propriedade para realizar a mesma finalidade. Por exemplo, em vez de escrever `ViewData["Message"]="text"`, você pode escrever `ViewBag.Message="text"`. Você não precisará definir todas as classes fortemente tipadas para usar o `ViewBag` propriedade. Porque é uma propriedade dinâmica, você pode em vez disso, basta obter ou definir propriedades e ele irá resolvê-los dinamicamente em tempo de execução. Internamente, `ViewBag` propriedades são armazenadas como pares de nome/valor no `ViewData` dicionário. (Observação: na maioria das versões de pré-lançamento do MVC 3, o `ViewBag` propriedade era denominada o `ViewModel` propriedade.)

### <a name="new-actionresult-types"></a>Novos tipos de "ActionResult"

O seguinte `ActionResult` tipos e métodos de auxiliares correspondentes são novos ou aprimorados no MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Retorna um código de status HTTP 404 para o cliente.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Retorna um redirecionamento temporário (código de status HTTP 302) ou um redirecionamento permanente (código de status HTTP 301), dependendo de um parâmetro booliano. Em conjunto com essa alteração, o [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) classe agora tem três métodos para executar redirecionamentos permanentes: `RedirectPermanent`, `RedirectToRoutePermanent`, e `RedirectToActionPermanent`. Esses métodos retornam uma instância do `RedirectResult` com o `Permanent` propriedade definida como `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Retorna um código de status HTTP especificado pelo usuário.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Aprimoramentos de Ajax e JavaScript

Por padrão, os auxiliares de Ajax e a validação no MVC 3 usam uma abordagem JavaScript discreta. JavaScript não invasivo evita injetando JavaScript embutido no HTML. Isso torna seu HTML menor e menos desorganizado e torna mais fácil trocar ou personalizar bibliotecas JavaScript. Os auxiliares de validação no MVC 3 também usam o `jQueryValidate` plug-in por padrão. Se você quiser o comportamento do MVC 2, você pode desabilitar o JavaScript discreto usando um *Web. config* arquivo de configuração. Para obter mais informações sobre as melhorias de JavaScript e Ajax, consulte os seguintes recursos:

- [Introdução básica sobre o JavaScript discreto no site da Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Postagem de JavaScript discreto de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Postagem de validação de JavaScript discreto de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Criando um aplicativo MVC 3 com Razor e JavaScript discreto](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial no site do ASP.NET)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validação do lado do cliente habilitada por padrão

Em versões anteriores do MVC, você precisa chamar explicitamente o `Html.EnableClientValidation` método de um modo de exibição para habilitar a validação do lado do cliente. No MVC 3 isso não é necessário porque a validação do lado do cliente está habilitada por padrão. (É possível desabilitar isso usando uma configuração na *Web. config* arquivo.)

Em ordem para a validação do lado do cliente trabalhar, você ainda precisará fazer referência a jQuery apropriado e bibliotecas de validação do jQuery em seu site. Você pode hospedar essas bibliotecas em seu próprio servidor ou referênciá-los de uma rede de distribuição de conteúdo (CDN), como CDNs da Microsoft ou Google.

### <a name="remote-validator"></a>Validador remoto

O ASP.NET MVC 3 oferece suporte a novos [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) classe que permite que você aproveite a validação do jQuery plug-in do suporte de validador remoto. Isso permite que a biblioteca de validação do lado do cliente chamar um método personalizado que você define no servidor para executar a lógica de validação só pode ser feita automaticamente no servidor.

No exemplo a seguir, o `Remote` atributo especifica que a validação do cliente chame uma ação chamada `UserNameAvailable` sobre o `UsersController` classe a fim de validar o `UserName` campo.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

O exemplo a seguir mostra o controlador correspondente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Para obter mais informações sobre como usar o `Remote` atributo, consulte [como: implementar a validação remota no ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) na biblioteca MSDN.

### <a name="json-binding-support"></a>Suporte à associação de JSON

O ASP.NET MVC 3 inclui suporte interno de associação de JSON que permite que os métodos de ação receber dados codificados em JSON e modelo-bind-la aos parâmetros de método de ação. Esse recurso é útil em cenários que envolvem a vinculação de dados e modelos de cliente. (Modelos de cliente permitem que você formatar e exibir um único item de dados ou conjunto de itens de dados usando modelos que são executados no cliente.) MVC 3 permite que você se conecte facilmente modelos de cliente com métodos de ação no servidor que envia e recebe dados JSON. Para obter mais informações sobre suporte à associação de JSON, consulte o **JavaScript e AJAX melhorias** seção [postagem de blog do MVC 3 versão prévia Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Aprimoramentos de validação de modelo

### <a name="dataannotations-metadata-attributes"></a>Atributos de metadados de "DataAnnotations"

Dá suporte ao ASP.NET MVC 3 `DataAnnotations` atributos de metadados como `DisplayAttribute`.

### <a name="validationattribute-class"></a>Classe "ValidationAttribute"

O `ValidationAttribute` classe foi aprimorado no .NET Framework 4 para oferecer suporte a um novo `IsValid` sobrecarga que fornece mais informações sobre o atual contexto de validação, como qual objeto está sendo validado. Isso permite cenários mais avançados em que você pode validar o valor atual com base em outra propriedade do modelo. Por exemplo, o novo `CompareAttribute` atributo permite que você compare os valores de duas propriedades de um modelo. No exemplo a seguir, o `ComparePassword` propriedade deve corresponder a `Password` campo para que seja válido.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validação

O [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interface permite que você executar a validação de nível de modelo e permite que você fornecer mensagens de erro que são específicas para o estado do modelo geral, ou entre duas propriedades dentro do modelo de validação . MVC 3 agora recupera de erros do `IValidatableObject` interface quando a associação de modelo e automaticamente sinalizadores ou destaques afetados campos dentro de uma exibição usando auxiliares de formulário HTML internos.

O [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interface permite que o ASP.NET MVC descobrir em tempo de execução se um validador tem suporte para validação do cliente. Essa interface foi projetada para que ele pode ser integrado com uma variedade de estruturas de validação.

Para obter mais informações sobre interfaces de validação, consulte o **aprimoramentos de validação de modelo** seção [postagem de blog do MVC 3 versão prévia Guthrie](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (No entanto, observe que a referência ao "IValidateObject" no blog deve ser "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Aprimoramentos de injeção de dependência

O ASP.NET MVC 3 fornece melhor suporte para a aplicação de injeção de dependência (DI) e para a integração com os contêineres de injeção de dependência ou inversão de controle (IOC). Foi adicionado suporte para injeção de dependência nas seguintes áreas:

- Controladores (Registrando e injeção de alocadores de controlador, injetando controladores).
- Modos de exibição (Registrando e injeção de mecanismos de exibição, injeção de dependências em páginas de exibição).
- Filtros de ação (Localizando e injetando filtros).
- Associadores de modelo (Registrando e injetando).
- Provedores de validação de modelo (Registrando e injetando).
- Provedores de metadados de modelo (Registrando e injetando).
- Provedores de valor (Registrando e injetando).

MVC 3 oferece suporte a [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) biblioteca e qualquer contêiner de injeção de dependência que dá suporte a essa biblioteca `IServiceLocator` interface. Ele também dá suporte a um novo `IDependencyResolver` interface que torna mais fácil integrar a estruturas da DI.

Para saber mais sobre DI no MVC 3, consulte os seguintes recursos:

- [Série de Brad Wilson de postagens do blog sobre o local do serviço](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Outros novos recursos

### <a name="nuget-integration"></a>Integração do NuGet

O ASP.NET MVC 3 automaticamente instala e habilita o NuGet como parte da sua instalação. O NuGet é um Gerenciador de pacotes de software livre gratuito que torna mais fácil localizar, instalar e usar bibliotecas .NET e ferramentas em seus projetos. Ele funciona com todos os tipos de projeto do Visual Studio (incluindo o Web Forms do ASP.NET e ASP.NET MVC).

O NuGet permite que os desenvolvedores responsáveis pela manutenção de projetos de software livre (por exemplo, projetos, como Moq, NHibernate, Ninject, StructureMap, NUnit, o Windsor, RhinoMocks e Elmah) para empacotar suas bibliotecas e registrá-los em uma galeria online. Em seguida, é fácil para desenvolvedores do .NET que desejam usar uma dessas bibliotecas para localizar o pacote e instalá-lo em projetos que estão trabalhando.

Com a atualização de ferramentas do ASP.NET 3, modelos de projeto incluem pacotes de NuGet pré-instalados bibliotecas de JavaScript, para que sejam atualizáveis por meio do NuGet. Entity Framework Code First é também previamente instalado como um pacote do NuGet.

Para saber mais sobre o NuGet, veja a [documentação do NuGet](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Cache de saída de página parcial

ASP.NET MVC tem suporte para o cache de saída de respostas de página completo desde a versão 1. MVC 3 também dá suporte a cache de saída de página parcial, que permite que você facilmente as regiões de cache ou fragmentos de uma resposta. Para obter mais informações sobre o cache, consulte a **cache de saída de página parcial** seção [postagem de blog de Guthrie sobre o MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) e o **filho ação o cache de saída** seção o [notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Controle granular sobre validação de solicitação

ASP.NET MVC tem a validação de solicitação interno que automaticamente ajuda a proteger contra ataques de injeção de XSS e HTML. No entanto, às vezes você deseja explicitamente desabilitar validação de solicitação, por exemplo, se você deseja permitir que os usuários lançar HTML conteúdo (por exemplo, em entradas de blog ou conteúdo CMS). Agora você pode adicionar um [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) para modelos de atributo ou exibir modelos para desabilitar a validação de solicitação em uma base por propriedade durante a associação de modelo. Para obter mais informações sobre a validação de solicitação, consulte os seguintes recursos:

- O **JavaScript discreto e validação** seção [postagem de blog de Guthrie sobre o MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Extensível "novo projeto" caixa de diálogo

No ASP.NET MVC 3, você pode adicionar modelos de projeto, mecanismos de exibição, e as estruturas de projeto para teste de unidade do **novo projeto** caixa de diálogo.

### <a name="template-scaffolding-improvements"></a>Aprimoramentos de Scaffolding do modelo

Modelos de scaffolding do ASP.NET MVC 3 fazem um trabalho melhor de identificar as propriedades de chave primária em modelos e manipulá-los adequadamente que em versões anteriores do MVC. (Por exemplo, os modelos de scaffolding agora, verifique se que a chave primária não é feito o scaffolding de um campo de formulário editável.)

Por padrão, as criar e editar scaffolds agora usam o `Html.EditorFor` auxiliar em vez do `Html.TextBoxFor` auxiliar. Isso melhora o suporte para metadados com o modelo na forma de dados de atributos de anotação quando o **adicionar exibição** caixa de diálogo gera uma exibição.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Novas sobrecargas para "Html.LabelFor" e "Html.LabelForModel"

Foram adicionadas novas sobrecargas de método para o `LabelFor` e `LabelForModel` métodos auxiliares. Novas sobrecargas permitem especificar ou substituir o texto do rótulo.

### <a name="sessionless-controller-support"></a>Suporte ao controlador sem sessão

No ASP.NET MVC 3, você pode indicar se você deseja que uma classe de controlador para usar o estado de sessão, em caso afirmativo, se o estado de sessão deve ser leitura/gravação ou somente leitura. Para obter mais informações sobre o suporte de controlador sem sessão, consulte [notas de versão do MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nova classe "AdditionalMetadataAttribute"

Você pode usar o [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) atributo para popular o `ModelMetadata.AdditionalValues` dicionário para uma propriedade de modelo. Por exemplo, se um modelo de exibição tem uma propriedade que deve ser exibida somente para o administrador, você pode anotar essa propriedade, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Esses metadados é disponibilizado para qualquer exibição ou o editor de modelo quando um modelo de exibição de produto é renderizado. Cabe a você interpretar as informações de metadados.

### <a name="accountcontroller-improvements"></a>Aprimoramentos de AccountController

O AccountController no modelo de projeto de Internet foi bastante aperfeiçoado.

### <a name="new-intranet-project-template"></a>Novo modelo de projeto de Intranet

Um novo modelo de projeto de Intranet está incluído que habilita a autenticação do Windows e remove o AccountController.
