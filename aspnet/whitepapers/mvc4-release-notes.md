---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este documento descreve a versão do ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: dbcea6090a0376b8732e02c0891721672bfe50f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898574"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Este documento descreve a versão do ASP.NET MVC 4.


- [Notas de instalação](#_Toc303253802)
- [Documentação](#_Toc303253803)
- [Suporte](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Novos recursos no ASP.NET MVC 4](#_Toc303253807)

    - [API Web ASP.NET](#_Toc317096197)
    - [Aprimoramentos para modelos de projeto padrão](#_Toc303253808)
    - [Modelo de projeto móvel](#_Toc303253809)
    - [Modos de exibição](#_Toc303253810)
    - [jQuery Mobile, a alternância de exibição e sobreposição de navegador](#_Toc303253811)
    - [Suporte a tarefa assíncronos controladores](#_Toc303253813)
    - [SDK do Azure](#_Toc303253814)
    - [Migrações de banco de dados](#_Toc303253818)
    - [Modelo de projeto vazio](#_Toc303253819)
    - [Adicionar controlador para qualquer pasta de projeto](#_Toc303253820)
    - [Agrupamento e minificação](#_Toc303253821)
    - [Habilitar os logons do Facebook e outros Sites usando OpenID e OAuth](#_Toc303253822)
- [Atualizando um projeto ASP.NET MVC 3 para o ASP.NET MVC 4](#_Toc303253806)
- [Alterações da versão Release Candidate do ASP.NET MVC 4](#_Toc303253817)
- [Problemas conhecidos e as alterações recentes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de instalação

ASP.NET MVC 4 para Visual Studio 2010 pode ser instalado a partir de [home page do ASP.NET MVC 4](../mvc/mvc4.md) usando o Web Platform Installer.

É recomendável desinstalar qualquer visualizações instaladas anteriormente do ASP.NET MVC 4 antes de instalar o ASP.NET MVC 4. Você pode atualizar a versão Beta do ASP.NET MVC 4 e a versão Release Candidate para ASP.NET MVC 4 sem desinstalar.

Esta versão não é compatível com qualquer versões de visualização do .NET Framework 4.5. Separadamente, você deve atualizar qualquer instalado versões de visualização do .NET Framework 4.5 para a versão final antes de instalar o ASP.NET MVC 4.

ASP.NET MVC 4 pode ser instalado e execute-lado a lado com o ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentação

Documentação do ASP.NET MVC está disponível no site do MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página do site da Web do ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Suporte

ASP.NET MVC 4 é totalmente compatível. Se você tiver dúvidas sobre como trabalhar com esta versão também é possível lançá-los para o fórum do ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), onde os membros da comunidade do ASP.NET são frequentemente pode oferecer suporte informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes do ASP.NET MVC 4 para o Visual Studio requerem PowerShell 2.0 e o Visual Studio 2010 com Service Pack 1 ou Visual Web Developer Express 2010 com Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Novos recursos no ASP.NET MVC 4

Esta seção descreve os recursos que foram introduzidos na versão do ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API da Web do ASP.NET

O ASP.NET MVC 4 inclui API Web do ASP.NET, uma nova estrutura para criar serviços HTTP que podem alcançar uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. ASP.NET Web API também é uma plataforma ideal para criar serviços RESTful.

ASP.NET Web API inclui suporte para os seguintes recursos:

- **Modelo de programação HTTP moderno:** diretamente acessar e manipular solicitações HTTP e respostas em suas APIs da Web usando um modelo de objeto novo, fortemente tipado do HTTP. O mesmo modelo de programação e pipeline HTTP é simetricamente disponível no cliente por meio do novo *HttpClient* tipo.
- **Suporte total para rotas:** ASP.NET Web API dá suporte ao conjunto completo de recursos de rota de roteamento do ASP.NET, incluindo os parâmetros de rota e restrições. Além disso, use as convenções de simples para mapear ações para os métodos HTTP.
- **Negociação de conteúdo:** o cliente e o servidor podem trabalhar juntos para determinar o formato correto para dados que estão sendo retornados de uma API da web. ASP.NET Web API fornece suporte padrão para XML, JSON e formatos de codificação de URL do formulário e você pode estender esse suporte adicionando seus próprios formatadores ou até mesmo substituir a estratégia de negociação de conteúdo padrão.
- **Modelo de associação e de validação:** associadores de modelo fornecem uma maneira fácil para extrair dados de várias partes de uma solicitação HTTP e converter as partes da mensagem em objetos .NET que podem ser usados pelas ações de API da Web. A validação também é executada em parâmetros de ação com base em anotações de dados.
- **Filtros:** ASP.NET Web API dá suporte a filtros, incluindo filtros conhecidos, como o *[autorizar]* atributo. Você pode criar e conectar seus próprios filtros para ações, autorização e tratamento de exceção.
- **Composição de consultas:** Use o *[Queryable]* atributo de filtro em uma ação que retorna *IQueryable* para habilitar o suporte para consultar sua API da web por meio de convenções de consulta OData.
- **Melhor capacidade de teste:** em vez de definir os detalhes HTTP em objetos de contexto estático, web trabalho de ações de API com instâncias de *HttpRequestMessage* e *HttpResponseMessage*. Crie um projeto de teste de unidade juntamente com seu projeto de API da Web para começar a escrever rapidamente testes de unidade para a funcionalidade da API da Web.
- **Configuração baseada em código:** configuração da API Web do ASP.NET é realizada apenas por meio de código, deixando a sua configuração de limpeza de arquivos. Use o padrão de localizador de serviço fornecido para configurar pontos de extensibilidade.
- **Maior suporte para contêineres de inversão de controle (IoC):** ASP.NET Web API fornece excelente suporte para contêineres IoC por meio de uma abstração de resolvedor de dependência aprimorado
- **Auto-host:** APIs da Web pode ser hospedado em seu próprio processo além do IIS enquanto estiver usando toda a capacidade de rotas e outros recursos da API da Web.
- **Ajuda personalizada para criar e testar páginas:** você agora pode facilmente criar Ajuda personalizada e teste páginas para suas APIs da web usando o novo *IApiExplorer* service para obter uma descrição completa de tempo de execução de suas APIs da web.
- **Monitoramento e diagnóstico:** ASP.NET Web API agora fornece a infraestrutura de rastreamento de peso leve que facilita a integração com soluções de log existentes, como System. Diagnostics, ETW e estruturas de registro em log de terceiros. Você pode habilitar o rastreamento, fornecendo um *ITraceWriter* implementação e adicioná-lo à sua configuração de API da web.
- **Vincular a geração:** usar a API da Web ASP.NET *UrlHelper* para gerar links para recursos relacionados no mesmo aplicativo.
- **Modelo de projeto de API da Web:** selecione o novo formulário de projeto de API da Web o Assistente de novo projeto MVC 4 para obter rapidamente em funcionamento com API da Web do ASP.NET.
- **Scaffolding:** Use o **Adicionar controlador** diálogo Scaffold rapidamente um controlador de API da web com base em uma estrutura de entidade com base em tipo de modelo.

Para obter mais detalhes sobre a API da Web do ASP.NET, visite [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Aprimoramentos para modelos de projeto padrão

O modelo que é usado para criar novos projetos do ASP.NET MVC 4 foi atualizado para criar um site de aparência mais modernos:

![](mvc4-release-notes/_static/image1.png)

Além de melhorias superficiais, aumenta a funcionalidade no novo modelo. O modelo usa uma técnica chamada processamento adaptável a aparência em navegadores de desktop e navegadores móveis sem nenhuma personalização.

![](mvc4-release-notes/_static/image2.png)

Para ver o processamento adaptável em ação, você pode usar um emulador móvel ou apenas tente redimensionar a janela do navegador de área de trabalho para um tamanho menor. Quando a janela do navegador é pequena o suficiente, o layout da página será alterado.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modelo de projeto móvel

Se você estiver iniciando um novo projeto e deseja criar um site especificamente para dispositivos móveis e navegadores de tablet, você pode usar o novo modelo de projeto de aplicativo móvel. Isso se baseia no jQuery Mobile, uma biblioteca de código aberto para a criação de interface do usuário otimizada para toque:

![](mvc4-release-notes/_static/image3.png)

Este modelo contém a mesma estrutura de aplicativo como o modelo de aplicativo de Internet (e o código do controlador é praticamente idêntico), mas é com o jQuery Mobile para uma boa aparência e comportamento bem em dispositivos móveis baseados em toque estilo. Para obter mais informações sobre a estrutura e estilo da interface do usuário móvel, consulte o [site do projeto móvel jQuery](http://jquerymobile.com/).

Se você já tiver um site que você deseja adicionar exibições mobile otimizado para, ou se você quiser criar um único site que funciona diferentemente com estilo modos de exibição para os navegadores de desktop e mobile e orientada a área de trabalho, você pode usar o novo recurso de modos de exibição. (Consulte a próxima seção).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de exibição

O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está fazendo a solicitação. Por exemplo, se um navegador desktop solicitar a página inicial, o aplicativo pode usar o modelo Views\Home\Index.cshtml. Se um navegador móvel solicitar a página inicial, o aplicativo pode retornar o modelo de Views\Home\Index.mobile.cshtml.

Layouts e existe meio-termo também pode ser substituído para tipos de navegador específico. Por exemplo:

- Se sua pasta exibições \ compartilhadas contém o \_cshtml e \_Layout.mobile.cshtml modelos, por padrão, o aplicativo usará \_Layout.mobile.cshtml durante solicitações de navegadores móveis e \_Cshtml durante outras solicitações.
- Se uma pasta contém \_MyPartial.cshtml e \_MyPartial.mobile.cshtml, a instrução @Html.Partial("\_MyPartial") será renderizado \_MyPartial.mobile.cshtml durante solicitações de celular navegadores, e \_MyPartial.cshtml durante outras solicitações.

Se você quiser criar modos de exibição mais específicos, layouts ou modos de exibição parciais para outros dispositivos, você pode registrar um novo *DefaultDisplayMode* instância para especificar qual nome para pesquisar quando uma solicitação satisfaz condições específicas. Por exemplo, você pode adicionar o código a seguir para o *aplicativo\_iniciar* método no arquivo global. asax para registrar a cadeia de caracteres "iPhone" como um modo de exibição que se aplica quando o navegador de iPhone da Apple faz uma solicitação:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Depois que esse código é executado, quando um navegador de iPhone da Apple faz uma solicitação, o aplicativo usará a exibições \ compartilhadas\\_Layout.iPhone.cshtml layout (se houver). Para obter mais informações sobre o modo de exibição, consulte [recursos do ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Aplicativos que usam DisplayModeProvider devem instalar o [DisplayModes fixa](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacote NuGet. O [ASP.NET estão 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) inclui o [DisplayModes fixa](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacote NuGet em novos modelos de projeto. Consulte [Fixedd de Bug de cache do ASP.NET MVC 4 Mobile](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Recursos de celular e jQuery Mobile

Para obter informações sobre a criação de aplicativos móveis com o ASP.NET MVC 4 usando o jQuery Mobile, consulte o tutorial [recursos do ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Suporte a tarefa assíncronos controladores

Agora você pode escrever os métodos de ação assíncrono como uma única métodos que retornam um objeto do tipo *tarefa* ou *tarefa&lt;ActionResult&gt;*.

 Para obter mais informações, consulte [usando os métodos assíncronos no ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK do Azure

ASP.NET MVC 4 oferece suporte as 1.6 e versões mais recentes do SDK do Windows Azure.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrações de banco de dados

Projetos do ASP.NET MVC 4 agora incluem o Entity Framework 5. Um dos excelentes recursos do Entity Framework 5 é o suporte para migrações de banco de dados. Esse recurso permite que você facilmente desenvolver seu esquema de banco de dados usando uma migração voltada para o código enquanto preserva os dados no banco de dados. Para obter mais informações sobre migrações de banco de dados, consulte [adicionar um novo campo para o modelo de filme e tabela](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) no [Introdução ao tutorial do ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Modelo de projeto vazio

O modelo de projeto MVC vazio agora está realmente vazio para que você possa começar do início completamente. A versão anterior do modelo de projeto vazio foi renomeada para básico.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Adicionar controlador para qualquer pasta de projeto

Você pode agora clique com botão direito e selecione **Adicionar controlador** de qualquer pasta em seu projeto MVC. Isso lhe dá mais flexibilidade para organizar seus controladores de maneira que desejar, incluindo a manter os controladores MVC e a API da Web em pastas separadas.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Empacotamento e minimização

A estrutura de empacotamento e minimização permite que você reduza o número de solicitações HTTP que uma página da Web precisa tornar combinando arquivos individuais em um único arquivo agrupado para scripts e CSS. Ele, em seguida, pode reduzir o tamanho geral dessas solicitações, minimizando o conteúdo do pacote. Minimizando pode incluir atividades como eliminar espaço em branco para encurtar os nomes de variável para o mesmo recolhendo seletores CSS com base em sua semântica. Pacotes são declarados e são configurados no código e são facilmente referenciadas em exibições por meio de métodos auxiliares que podem gerar um um link único para o pacote ou, ao depurar, vários links para o conteúdo individual do pacote. Para obter mais informações, consulte [empacotamento e minimização](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitar os logons do Facebook e outros Sites usando OpenID e OAuth

Os modelos padrão no modelo de projeto do ASP.NET MVC 4 Internet agora inclui suporte para o logon do OpenID e OAuth usando a biblioteca DotNetOpenAuth. Para obter informações sobre como configurar um provedor OAuth ou OpenID, consulte [suporte OAuth/OpenID WebForms, MVC e páginas da Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) e [OpenID e OAuth recurso documentação em páginas da Web do ASP.NET](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Atualizando um projeto ASP.NET MVC 3 para o ASP.NET MVC 4

ASP.NET MVC 4 pode ser instalado lado a lado com o ASP.NET MVC 3 no mesmo computador, o que proporciona flexibilidade na escolha quando atualizar um aplicativo ASP.NET MVC 3 para o ASP.NET MVC 4.

A maneira mais simples de atualizar é para criar um novo projeto ASP.NET MVC 4 e copie todos os os modos de exibição, controladores, código e conteúdo arquivos de projeto MVC 3 existente para o novo projeto e, em seguida, atualizar o assembly faz referência a no novo projeto para coincidir com qualquer modelo MVC não em assembiles incluídos que você está usando. Se você fez alterações no arquivo Web. config no projeto MVC 3, você também deve mesclar as alterações no arquivo Web. config no projeto MVC 4.

Para atualizar manualmente um aplicativo ASP.NET MVC 3 existente para a versão 4, faça o seguinte:

1. No Web. config todos os arquivos no projeto (há um na raiz do projeto, um em modos de exibição e um na pasta modos de exibição para cada área em seu projeto), substitua cada ocorrência do texto a seguir (Observação: System.Web.WebPages, Version = 1.0.0.0 não foi encontrado em projetos criados com o Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    com o seguinte texto correspondente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. No arquivo Web. config raiz, atualize o *webPages:Version* elemento "2.0.0.0" e adicione um novo *PreserveLoginUrl* chave que tem o valor "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. No Gerenciador de soluções, clique com botão direito nas referências e selecione Gerenciar pacotes NuGet. No painel esquerdo, selecione **origem de pacote oficial Online\NuGet**, em seguida, atualize o seguinte:

    - ASP.NET MVC 4
    - (Opcional) jQuery, jQuery validação e jQuery UI
    - (Opcional) Entity Framework
    - (Optonal) Modernizr
4. No Gerenciador de soluções, clique no nome do projeto e selecione Unload Project. Em seguida, clique no nome novamente e selecione Editar *ProjectName*. csproj.
5. Localize o *ProjectTypeGuids* elemento e substitua {E53F8FEA-EAE0-44A6-8774-FFD645390401} por {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salvar as alterações, feche o arquivo de projeto (. csproj) que você estava editando, clique com o botão direito e, em seguida, selecione Recarregar projeto.
7. Se o projeto referencia todas as bibliotecas de terceiros que são compiladas usando versões anteriores do ASP.NET MVC, abra o arquivo Web. config de raiz e adicione as três seguintes *bindingRedirect* elementos sob o  *configuração* seção: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Alterações da versão Release Candidate do ASP.NET MVC 4

Notas de versão do ASP.NET MVC 4 Release Candidate podem ser encontradas aqui:

As alterações principais do ASP.NET MVC 4 Release Candidate nesta versão são resumidas a seguir:

- **Por configuração do controlador:** controladores da API da Web ASP.NET podem ser atribuídos com um atributo personalizado que implementa *IControllerConfiguration* para configurar suas próprias formatadores, seletor da ação e associadores de parâmetro . O *HttpControllerConfigurationAttribute* foi removido.
- **Por manipuladores de mensagens de rota:** agora você pode especificar o manipulador de mensagem final da cadeia de solicitação para uma rota em questão. Isso habilita o suporte ao longo do jornada estruturas usar o serviço de roteamento de mensagens para enviar a sua própria (não -*IHttpController*) pontos de extremidade.
- **Notificações de progresso:** o *ProgressMessageHandler* gera a notificação do progresso de entidades de solicitação que está sendo carregadas e entidades de resposta sendo baixadas. Usando esse manipulador é possível controlar quanto você está carregando um corpo de solicitação ou baixar um corpo de resposta.
- **Enviar por push o conteúdo:** o *PushStreamContent* classe permite cenários onde um produtor de dados deseja gravar diretamente a uma solicitação ou resposta (forma síncrona ou assíncrona) usando um fluxo. Quando o *PushStreamContent* está pronto para aceitar os dados que ele chama um delegado de ação com o fluxo de saída. Em seguida, o desenvolvedor pode gravar no fluxo para desde que o necessário e fechar o fluxo durante a gravação foi concluída. O *PushStreamContent* detecta o fechamento do fluxo e conclui subjacente assíncrona *tarefa* para gravar o conteúdo.
- **Criar respostas de erro:** Use o *HttpError* tipo consistentemente representar informações de erro, como erros de validação e exceções ainda respeitar o *IncludeErrorDetailPolicy*. Use a nova *CreateErrorResponse* métodos de extensão para criar facilmente as respostas de erro com *HttpError* como conteúdo. O *HttpError* conteúdo é totalmente conteúdo negociado.
- **MediaRangeMapping removido:** intervalos de tipos de mídia agora são manipulados pelo Negociador de conteúdo padrão.
- **Associação de parâmetro padrão para parâmetros de tipo simples é agora [FromUri]:** nas versões anteriores do ASP.NET Web API a associação de parâmetros de padrão para parâmetros de tipo simples usados com associação de modelo. A associação de parâmetro padrão para parâmetros de tipo simples é agora *[FromUri]*.
- **Seleção de ação honra parâmetros obrigatórios:** seleção de ação na API da Web ASP.NET agora apenas selecionará uma ação se todos os parâmetros necessários que vêm do URI são fornecidos. Um parâmetro pode ser especificado como opcional, fornecendo um valor padrão para o argumento na assinatura do método de ação.
- **Personalizar as associações de parâmetro HTTP:** usar o *ParameterBindingAttribute* para personalizar a associação de parâmetro para um parâmetro de ação específica ou usar o *ParameterBindingRules* no *HttpConfiguration* para personalizar as associações de parâmetro mais amplamente.
- **Aprimoramentos do MediaTypeFormatter:** formatadores agora tem acesso a toda *HttpContent* instância.
- **Seleção de política de buffer do host:** implementar e configurar o *IHostBufferPolicySelector* serviço ASP.NET Web API para habilitar os hosts determinar a política para quando o buffer estiver a ser usado.
- **Acessar certificados de cliente de maneira independente do host:** Use o *GetClientCertificate* método de extensão para obter o certificado de cliente fornecido da mensagem de solicitação.
- **Extensibilidade de negociação de conteúdo:** personalizar negociação de conteúdo derivando de *DefaultContentNegotiator* e substituindo qualquer aspecto da negociação de conteúdo que você deseja.
- **Suporte para retornar 406 respostas não aceitável:** você agora pode retornar 406 respostas não aceitável na API da Web do ASP.NET quando um formatador adequado não foi encontrado com a criação de um *DefaultContentNegotiator* com o *excludeMatchOnTypeOnly* parâmetro definido como *true*.
- **Ler dados de formulário como NameValueCollection ou JToken:** você pode ler dados do formulário na cadeia de caracteres de consulta URI ou no corpo da solicitação como um *NameValueCollection* usando o *ParseQueryString* e  *ReadAsFormDataAsync* métodos de extensão respectivamente. Da mesma forma, você pode ler dados do formulário na cadeia de caracteres de consulta URI ou no corpo da solicitação como um *JToken* usando o *TryReadQueryAsJson* e *ReadAsAsync*&lt;T&gt; métodos de extensão respectivamente.
- **Aprimoramentos de várias partes:** agora é possível gravar um *MultipartStreamProvider* completamente adequado para o tipo de dados com diversas partes MIME que pode ler e apresentará o resultado da maneira ideal para o usuário. Você também pode utilizar uma etapa de pós-processamento no *MultipartStreamProvider* que permite a implementação fazer o pós-processamento ele deseja nas partes de corpo com diversas partes MIME. Por exemplo, o *MultipartFormDataStreamProvider* implementação lê o formulário HTML partes de dados e os adiciona a um *NameValueCollection* para que seja fácil de obter do chamador.
- **Aprimoramentos de geração de link:** o *UrlHelper* não depende mais *HttpControllerContext*. Agora você pode acessar o *UrlHelper* de qualquer contexto em que o *HttpRequestMessage* está disponível.
- **Alteração de ordem de execução do manipulador de mensagens:** manipuladores de mensagens agora são executadas na ordem em que eles são configurados em vez de em ordem inversa.
- **Auxiliar para a conexão dos manipuladores de mensagens:** novo *HttpClientFactory* que pode conectar *DelegatingHandlers* e criar um *HttpClient* com o pipeline desejado pronto para começar. Ele também fornece funcionalidade para Programando com manipuladores internos alternativos (o padrão é *HttpClientHandler*), bem como fazer a ligação de ao usar *HttpMessageInvoker* ou outro  *DelegatingHandler* em vez de *HttpClient* como o chamador superior.
- **Suporte para CDNs na otimização da Web ASP.NET:** otimização da Web de ASP.NET agora oferece suporte para caminhos alternativos CDN, permitindo que você especificar para cada pacote mais uma URL que aponta para o mesmo recurso em uma rede de fornecimento de conteúdo. Suporte CDNs permite que você acesse seus pacotes de script e estilo geograficamente para consumidores finais dos seus aplicativos Web. Aplicativos de produção devem implementar um fallback quando o CDN não estiver disponível. Teste o fallback.
- **Rotas de API da Web do ASP.NET e configuração movido para *WebApiConfig.Register* método estático pode ser resused no código de teste.** Rotas do ASP.NET Web API foram adicionadas anteriormente em *RouteConfig.RegisterRoutes* juntamente com o padrão MVC rotas. O padrão que roteia de API da Web do ASP.NET e a configuração agora são tratados em um separado *WebApiConfig.Register* método para facilitar o teste.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e as alterações recentes

- **A versão RC e RTM do ASP.NET MVC 4 incorretamente retornado exibições de área de trabalho em cache quando exibições móveis devem ser retornadas.**

    - Consulte [ASP.NET MVC 4 Mobile cache Bug fixa](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção. A correção pode ser instalada a partir de [DisplayModes fixa](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacote NuGet.
- **Alterações significativas no mecanismo de exibição Razor**. Os seguintes tipos foram removidos do *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Os métodos a seguir também foram removidos: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Quando WebMatrix.WebData.dll é incluído em uma pasta de um aplicativos ASP.NET MVC 4 /bin, ele assume a URL para autenticação de formulários.** Adicionar o assembly WebMatrix.WebData.dll ao seu aplicativo (por exemplo, selecionando "ASP.NET páginas da Web com sintaxe Razor" ao usar a caixa de diálogo Adicionar dependências implantáveis) substituirá o redirecionamento de logon de autenticação para o logon/conta em vez de / conta/logon conforme o esperado pelo controlador de conta do ASP.NET MVC padrão. Para evitar esse comportamento e usar a URL especificada já na seção de autenticação da Web. config, você pode adicionar um appSetting chamado PreserveLoginUrl e defina-a como true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **O NuGet package manager Falha na instalação ao tentar instalar o ASP.NET MVC 4 para instalações lado a lado do Visual Studio 2010 e o Visual Web Developer 2010.** Para executar o Visual Studio 2010 e o Visual Web Developer 2010 lado a lado com o ASP.NET MVC 4, você deve instalar ASP.NET MVC 4 depois de já tem sido instaladas ambas as versões do Visual Studio.
- **Desinstalar o ASP.NET MVC 4 falhará se os pré-requisitos já tem sido desinstalados.** Para desinstalar completamente o ASP.NET MVC 4you deve desinstalar o ASP.NET MVC 4 antes de desinstalar o Visual Studio.
- **Instalar o ASP.NET MVC 4 quebras de aplicativos ASP.NET MVC 3 RTM.** A versão de aplicativos do ASP.NET MVC 3 que foram criados com o RTM (não com o [atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/download/details.aspx?id=1491) de versão) requerem as seguintes alterações para funcionar lado a lado com o ASP.NET MVC 4. Compilar o projeto sem fazer esses resultados de atualizações em erros de compilação. 

    **Atualizações necessárias**

  1. No arquivo Web. config raiz, adicione um novo *&lt;appSettings&gt;* entrada com a chave *webPages:Version* e o valor *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. No Gerenciador de soluções, clique no nome do projeto e selecione Unload Project. Em seguida, clique no nome novamente e selecione Editar *ProjectName*. csproj.
  3. Localize as seguintes referências de assembly: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Substitua-os com o seguinte:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Salvar as alterações, feche o arquivo de projeto (. csproj) que você estava editando e, em seguida, clique com o botão direito e selecione Recarregar.

- **Alterar um projeto ASP.NET MVC 4 de destino 4.0 de 4.5 não atualiza a referência de assembly EntityFramework:** se você alterar um projeto ASP.NET MVC 4 de destino 4.0 após direcionando 4.5 continuarão a apontar para a referência ao assembly EntityFramework a versão 4.5. Para corrigir a desinstalação deste problema e reinstalar o pacote EntityFramework NuGet.
- **403 Proibido ao executar um aplicativo ASP.NET MVC 4 no Azure após a mudança para o destino 4.0 do 4.5:** se você alterar um projeto ASP.NET MVC 4 de destino 4.0 após direcionando 4.5 e, em seguida, implanta no Azure, você verá um erro de 403 Proibido em tempo de execução. Para solucionar este problema, adicione o seguinte ao Web. config: `<modules runAllManagedModulesForAllRequests="true" />`
- **O Visual Studio 2012 falhar quando você digita um '\' em uma cadeia de caracteres literal em um arquivo Razor.** Para trabalhar em todo o problema, insira as aspas de fechamento da cadeia de caracteres literal primeiro.
- <strong>Navegando para &quot;conta/gerenciar&quot; nos resultados do modelo da Internet em um erro em tempo de execução de idiomas CHS, TRK e CHT.</strong> Para corrigir o problema modificá-la para separar <em>@User.Identity.Name</em> colocando-o como o único conteúdo dentro do <em>&lt;forte&gt;</em> marca.
- **Google e LinkedIn provedores não têm suporte em Sites do Azure.** Use provedores de autenticação alternativo durante a implantação para Sites do Azure.
- **Ao usar UriPathExtensionMapping com o IIS 8 Express/IIS, você receberia 404 erros não encontrado ao tentar usar a extensão.** O manipulador de arquivo estático interferirá solicitações para APIs da web que use *UriPathExtensionMappings*. Definir *runAllManagedModulesForAllRequests = true* em Web. config para contornar o problema.
- **Método Controller.Execute não é chamado.** Todos os controladores MVC assincronamente agora são sempre executados.
