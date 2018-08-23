---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este documento descreve a versão do ASP.NET MVC 4.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: a4f78061850ef5ad8c3381daafdb5ea6bca4cb2f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832953"
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
    - [jQuery Mobile, o alternador de exibições e substituindo o navegador](#_Toc303253811)
    - [Suporte de tarefa para controladores assíncronos](#_Toc303253813)
    - [SDK do Azure](#_Toc303253814)
    - [Migrações de banco de dados](#_Toc303253818)
    - [Modelo de projeto vazio](#_Toc303253819)
    - [Adicionar controlador para qualquer pasta do projeto](#_Toc303253820)
    - [Agrupamento e minificação](#_Toc303253821)
    - [Habilitar os logons do Facebook e outros Sites usando OAuth e OpenID](#_Toc303253822)
- [Atualizando um projeto do ASP.NET MVC 3 para o ASP.NET MVC 4](#_Toc303253806)
- [Alterações da versão Release Candidate do ASP.NET MVC 4](#_Toc303253817)
- [Problemas conhecidos e alterações recentes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de instalação

ASP.NET MVC 4 para Visual Studio 2010 pode ser instalado a partir de [homepage do ASP.NET MVC 4](../mvc/mvc4.md) usando o Web Platform Installer.

É recomendável desinstalar quaisquer versões prévias instaladas anteriormente do ASP.NET MVC 4 antes de instalar o ASP.NET MVC 4. Você pode atualizar o ASP.NET MVC 4 Beta e Release Candidate para o ASP.NET MVC 4 sem desinstalar.

Esta versão não é compatível com quaisquer versões de visualização do .NET Framework 4.5. Separadamente, você deve atualizar qualquer instalado versões de visualização do .NET Framework 4.5 para a versão final antes de instalar o ASP.NET MVC 4.

ASP.NET MVC 4 pode ser instalada e executada lado a lado com o ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentação

Documentação do ASP.NET MVC está disponível no site da MSDN na seguinte URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Tutoriais e outras informações sobre o ASP.NET MVC estão disponíveis na página do site do ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Suporte

ASP.NET MVC 4 é totalmente suportado. Se você tiver dúvidas sobre como trabalhar com esta versão também poderá lançá-las para o fórum do ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), onde os membros da comunidade do ASP.NET são com frequência pode oferecer suporte informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Os componentes do ASP.NET MVC 4 para Visual Studio exigem o PowerShell 2.0 e o Visual Studio 2010 com Service Pack 1 ou o Visual Web Developer Express 2010 com Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Novos recursos no ASP.NET MVC 4

Esta seção descreve os recursos que foram introduzidos na versão do ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API da Web do ASP.NET

O ASP.NET MVC 4 inclui a API Web do ASP.NET, uma nova estrutura para criar serviços HTTP que podem chegar a uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. API Web ASP.NET também é uma plataforma ideal para criar serviços RESTful.

API Web ASP.NET inclui suporte para os seguintes recursos:

- **Modelo de programação HTTP moderno:** diretamente acessar e manipular solicitações HTTP e respostas em suas APIs Web usando um modelo de objeto novo e com rigidez de tipos do HTTP. O mesmo modelo de programação e pipeline de HTTP é simetricamente disponível no cliente por meio da nova *HttpClient* tipo.
- **Suporte total para rotas:** ASP.NET Web API dá suporte ao conjunto completo de recursos de rota do roteamento do ASP.NET, incluindo os parâmetros de rota e restrições. Além disso, use convenções simples para mapear ações para os métodos HTTP.
- **Negociação de conteúdo:** o cliente e servidor podem trabalhar juntos para determinar o formato correto para os dados retornados de uma API da web. API Web ASP.NET fornece suporte padrão para XML, JSON e formatos codificada em URL do formulário e você pode estender esse suporte, adicionando seus próprios formatadores ou até mesmo substituir a estratégia de negociação de conteúdo padrão.
- **Associação de modelo e validação:** associadores de modelo fornecem uma maneira fácil para extrair dados de várias partes de uma solicitação HTTP e converter essas partes da mensagem em objetos .NET que podem ser usados pelas ações de API da Web. A validação também é executada nos parâmetros de ação com base nas anotações dos dados.
- **Filtros:** API Web ASP.NET oferece suporte a filtros, incluindo filtros bem conhecidos, como o *[autorizar]* atributo. Você pode criar e conectar seus próprios filtros de ações, autorização e manipulação de exceção.
- **Composição de consultas:** Use o *[Queryable]* atributo de filtro em uma ação que retorna *IQueryable* para habilitar o suporte para consultar sua API da web por meio de convenções de consulta OData.
- **Melhor capacidade de teste:** em vez de definir os detalhes HTTP em objetos de contexto estático, de trabalho de ações de API com instâncias de web *HttpRequestMessage* e *HttpResponseMessage*. Crie um projeto de teste de unidade junto com seu projeto de API da Web para começar a escrever rapidamente testes de unidade para a funcionalidade da API Web.
- **Configuração baseada em código:** configuração da API Web ASP.NET é feita somente por meio de código, deixando sua configuração de limpeza de arquivos. Use o padrão de localizador de serviço fornecido para configurar pontos de extensibilidade.
- **Suporte aprimorado para contêineres de inversão de controle (IoC):** API Web do ASP.NET fornece excelente suporte para contêineres de IoC por meio de uma abstração de resolvedor de dependências aprimorada
- **Hospedar internamente:** APIs da Web podem ser hospedadas em seu próprio processo além do IIS enquanto estiver usando todo o potencial de rotas e outros recursos da API da Web.
- **Ajuda personalizada de criar e testar páginas:** você agora pode facilmente criar Ajuda personalizada e testar páginas para suas APIs web usando o novo *IApiExplorer* service para obter uma descrição completa de tempo de execução de suas APIs web.
- **Monitoramento e diagnóstico:** API Web do ASP.NET agora oferece a infra-estrutura de rastreamento de peso leve que torna mais fácil de integrar soluções existentes de registro em log, como System. Diagnostics, ETW e estruturas de registro em log de terceiros. Você pode habilitar o rastreamento, fornecendo uma *ITraceWriter* implementação e adicioná-lo à sua configuração de API da web.
- **Geração de link:** usar a API Web ASP.NET *UrlHelper* para gerar links para recursos relacionados no mesmo aplicativo.
- **Modelo de projeto de API da Web:** selecione o novo formulário de projeto de API da Web o Assistente de novo projeto MVC 4 para entrar rapidamente em funcionamento com a API Web do ASP.NET.
- **Scaffolding:** Use o **Adicionar controlador** caixa de diálogo para criar rapidamente o scaffolding de um controlador de API da web com base em uma estrutura de entidade com base em tipo de modelo.

Para obter mais detalhes sobre a API Web do ASP.NET, visite [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Aprimoramentos para modelos de projeto padrão

O modelo que é usado para criar novos projetos do ASP.NET MVC 4 foi atualizado para criar um site de aparência mais moderna:

![](mvc4-release-notes/_static/image1.png)

Além das melhorias superficiais, aumenta a funcionalidade no novo modelo. O modelo emprega uma técnica chamada renderização adaptável para ter boa em navegadores de desktop e navegadores móveis sem nenhuma personalização.

![](mvc4-release-notes/_static/image2.png)

Para ver a renderização adaptável em ação, você pode usar um emulador móvel ou apenas tente redimensionar a janela da área de trabalho para um tamanho menor. Quando a janela do navegador é pequena o suficiente, o layout da página será alterado.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modelo de projeto móvel

Se você estiver começando um novo projeto e deseja criar um site especificamente para dispositivos móveis e navegadores de tablet, você pode usar o novo modelo de projeto de aplicativo móvel. Isso é baseado no jQuery Mobile, uma biblioteca de código-fonte aberto para a criação de interface do usuário otimizada para toque:

![](mvc4-release-notes/_static/image3.png)

Este modelo contém a mesma estrutura de aplicativo que o modelo de aplicativo de Internet (e o código do controlador é praticamente idêntico), mas é estilizadas usando jQuery Mobile boa aparência e se comportem corretamente em dispositivos móveis baseados em toque. Para saber mais sobre como estruturar e definir o estilo de interface do usuário móvel, consulte o [site do projeto móvel de jQuery](http://jquerymobile.com/).

Se você já tiver um site orientado a área de trabalho que você deseja adicionar exibições otimizadas para mobilidade a, ou se você quiser criar um único site que serve de modos de exibição com um estilo diferente aos navegadores da área de trabalho e móveis, você pode usar o novo recurso de modos de exibição. (Consulte a próxima seção).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de exibição

O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está fazendo a solicitação. Por exemplo, se um navegador da área de trabalho solicita a página inicial, o aplicativo pode usar o modelo Views\Home\Index.cshtml. Se um navegador móvel solicita a página inicial, o aplicativo pode retornar o modelo Views\Home\Index.mobile.cshtml.

Layouts e parciais também podem ser substituídos para tipos de navegador específico. Por exemplo:

- Se sua pasta Views\Shared contém ambas as \_layout. cshtml e \_cshtml modelos, por padrão, o aplicativo usará \_cshtml durante solicitações de navegadores para dispositivos móveis e de \_Layout. cshtml durante outras solicitações.
- Se uma pasta contém ambos \_MyPartial.cshtml e \_MyPartial.mobile.cshtml, a instrução @Html.Partial("\_MyPartial") será renderizado \_MyPartial.mobile.cshtml durante as solicitações do mobile navegadores, e \_MyPartial.cshtml durante a outras solicitações.

Se você quiser criar exibições mais específicas, layouts ou modos de exibição parciais para outros dispositivos, você pode registrar um novo *DefaultDisplayMode* instância para especificar qual nome a ser pesquisado quando uma solicitação atende a condições específicas. Por exemplo, você poderia adicionar o código a seguir para o *Application\_iniciar* método no arquivo global. asax para registrar a cadeia de caracteres "iPhone" como um modo de exibição que se aplica quando o navegador do iPhone da Apple faz uma solicitação:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Depois que esse código é executado, quando um navegador do iPhone da Apple faz uma solicitação, o aplicativo usará o Views\Shared\\_Layout.iPhone.cshtml layout (se houver). Para obter mais informações sobre o modo de exibição, consulte [recursos do ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Aplicativos que usam DisplayModeProvider devem instalar o [DisplayModes fixa](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacote do NuGet. O [ASP.NET Fall 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) inclui as [DisplayModes fixa](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacote do NuGet em novos modelos de projeto. Ver [Fixedd de Bug de cache do ASP.NET MVC 4 Mobile](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile e recursos do Mobile

Para obter informações sobre a criação de aplicativos móveis com o ASP.NET MVC 4 usando o jQuery Mobile, consulte o tutorial [recursos do ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Suporte de tarefa para controladores assíncronos

Agora você pode escrever métodos de ação assíncrono como uma única métodos que retornam um objeto do tipo *tarefa* ou *tarefa&lt;ActionResult&gt;*.

 Para obter mais informações, consulte [usando os métodos assíncronos no ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK do Azure

ASP.NET MVC 4 oferece suporte as 1.6 e versões mais recentes do SDK do Windows Azure.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrações de banco de dados

Projetos do ASP.NET MVC 4 agora incluem o Entity Framework 5. Um dos excelentes recursos no Entity Framework 5 é o suporte para migrações de banco de dados. Este recurso permite que você desenvolva facilmente seu esquema de banco de dados usando uma migração focada no código enquanto preserva os dados no banco de dados. Para obter mais informações sobre as migrações de banco de dados, consulte [adicionando um novo campo para o modelo de filme e a tabela](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) na [Introdução ao tutorial do ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Modelo de projeto vazio

O modelo de projeto vazio do MVC agora está realmente vazio para que você possa começar de uma ficha limpa completamente. A versão anterior do modelo de projeto vazio foi renomeada para básico.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Adicionar controlador para qualquer pasta do projeto

Agora você pode clique com botão direito e selecione **Adicionar controlador** de qualquer pasta no seu projeto do MVC. Isso lhe dá mais flexibilidade para organizar seus controladores de maneira que desejar, incluindo a manter seus controladores MVC e API da Web em pastas separadas.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Agrupamento e Minificação

A estrutura de empacotamento e minimização permite reduzir o número de solicitações HTTP que uma página da Web precisa fazer ao combinar arquivos individuais em um único arquivo agrupado para scripts e CSS. Ele, em seguida, pode reduzir o tamanho geral dessas solicitações por minificação o conteúdo do pacote. Minificação pode incluir atividades como eliminar espaço em branco para encurtar os nomes de variáveis até mesmo os seletores de CSS com base em sua semântica de recolhimento. Pacotes são declarados e configurados no código e são facilmente referenciadas em exibições por meio de métodos auxiliares que podem gerar um um link único para o pacote ou, quando estiver depurando, vários links para o conteúdo individual do pacote. Para obter mais informações, consulte [agrupamento e Minificação](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitar os logons do Facebook e outros Sites usando OAuth e OpenID

Os modelos padrão no modelo de projeto do ASP.NET MVC 4 Internet agora inclui suporte para o logon OAuth e OpenID usando a biblioteca DotNetOpenAuth. Para obter informações sobre como configurar um provedor OAuth ou OpenID, consulte [o suporte para formulários da Web, MVC e páginas da Web OAuth/OpenID](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) e o [OAuth e OpenID de recursos de documentação em páginas da Web do ASP.NET](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Atualizando um projeto do ASP.NET MVC 3 para o ASP.NET MVC 4

ASP.NET MVC 4 pode ser instalado lado a lado com o ASP.NET MVC 3 no mesmo computador, que lhe dá flexibilidade na escolha quando atualizar um aplicativo ASP.NET MVC 3 para o ASP.NET MVC 4.

A maneira mais simples de atualização é para criar um novo projeto ASP.NET MVC 4 e copiar todos os os modos de exibição, controladores, código e conteúdo arquivos de projeto MVC 3 existente para o novo projeto e, em seguida, atualizar o assembly faz referência no novo projeto para corresponder a qualquer modelo não MVC incluí assembiles de que você está usando. Se você fez alterações no arquivo Web. config no projeto do MVC 3, você também deve mesclar essas alterações no arquivo Web. config no projeto do MVC 4.

Para atualizar manualmente um aplicativo existente do ASP.NET MVC 3 para a versão 4, faça o seguinte:

1. No Web. config todos os arquivos no projeto (há um na raiz do projeto, um na pasta modos de exibição e um na pasta modos de exibição para cada área em seu projeto), substitua cada ocorrência do texto a seguir (Observação: System.Web.WebPages, versão = 1.0.0.0 não for encontrado no projetos criados com o Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    com o seguinte texto correspondente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. No arquivo Web. config raiz, atualize o *webPages:Version* elemento para "2.0.0.0" e adicione um novo *PreserveLoginUrl* chave que tem o valor "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. No Gerenciador de soluções, clique com botão direito nas referências e selecione Gerenciar pacotes NuGet. No painel esquerdo, selecione **origem de pacote oficial Online\NuGet**, em seguida, atualize o seguinte:

    - ASP.NET MVC 4
    - (Opcional) jQuery, jQuery validação e o jQuery UI
    - (Opcional) Entity Framework
    - (Optonal) Modernizr
4. No Gerenciador de soluções, clique com botão direito no nome do projeto e, em seguida, selecione descarregar projeto. Em seguida, clique no nome novamente e selecione Editar *ProjectName*. csproj.
5. Localize o *ProjectTypeGuids* elemento e substitua {E53F8FEA-EAE0-44A6-8774-FFD645390401} por {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Salve as alterações, feche o arquivo de projeto (. csproj) que você estava editando, clique com botão direito no projeto e, em seguida, selecione Recarregar projeto.
7. Se o projeto faz referência a quaisquer bibliotecas de terceiros que são compiladas usando versões anteriores do ASP.NET MVC, abra o arquivo Web. config de raiz e adicione as três seguintes *bindingRedirect* elementos sob o  *configuração* seção: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Alterações da versão Release Candidate do ASP.NET MVC 4

Notas de versão do ASP.NET MVC 4 Release Candidate podem ser encontradas aqui:

As principais alterações do ASP.NET MVC 4 Release Candidate nesta versão estão resumidas abaixo:

- **Por configuração do controlador:** controladores da API Web ASP.NET podem ser atribuídos com um atributo personalizado que implementa *IControllerConfiguration* para configurar suas próprias formatadores, seletor de ações e os associadores de parâmetro . O *HttpControllerConfigurationAttribute* foi removido.
- **Por manipuladores de mensagens de rota:** agora é possível especificar o manipulador de mensagem final da cadeia de solicitação para uma determinada rota. Isso habilita o suporte para estruturas de corrida junto ao usar o roteamento para expedir a sua própria (não -*IHttpController*) pontos de extremidade.
- **Notificações de progresso:** as *ProgressMessageHandler* gera a notificação de progresso para as entidades de resposta que está sendo baixadas e as entidades de solicitação que está sendo carregadas. Usando esse manipulador é possível manter o controle de quanto você está carregando um corpo de solicitação ou baixando um corpo de resposta.
- **Enviar por push o conteúdo:** as *PushStreamContent* classe possibilita cenários em que um produtor de dados deseja gravar diretamente para a solicitação ou resposta (forma síncrona ou assíncrona) usando um fluxo. Quando o *PushStreamContent* está pronto para aceitar dados que ele chama um delegado de ação com o fluxo de saída. O desenvolvedor pode então escrever no fluxo para desde que o necessário e fechar o fluxo durante a gravação foi concluída. O *PushStreamContent* detecta o fechamento do fluxo e conclui subjacente assíncrona *tarefa* para gravar o conteúdo.
- **Criar respostas de erro:** Use o *HttpError* tipo para representar consistentemente informações de erro, como erros de validação e exceções ainda respeitar o *IncludeErrorDetailPolicy*. Use a nova *CreateErrorResponse* para criar facilmente as respostas de erro com os métodos de extensão *HttpError* como conteúdo. O *HttpError* conteúdo é totalmente conteúdo negociado.
- **MediaRangeMapping removido:** intervalos de tipos de mídia agora são manipulados pelo Negociador de conteúdo padrão.
- **Associação de parâmetro padrão para parâmetros de tipo simples é agora [FromUri]:** nas versões anteriores do ASP.NET Web API, a associação de parâmetros de padrão para parâmetros de tipo simples usado a associação de modelo. A associação de parâmetro padrão para parâmetros de tipo simples é agora *[FromUri]*.
- **Seleção de ação honra os parâmetros necessários:** seleção de ação na API Web ASP.NET agora apenas selecionará uma ação se todos os parâmetros necessários que vêm do URI são fornecidos. Um parâmetro pode ser especificado como opcional, fornecendo um valor padrão para o argumento na assinatura do método de ação.
- **Personalizar as associações de parâmetro HTTP:** usar o *ParameterBindingAttribute* para personalizar a associação de parâmetro para um parâmetro de ação específica ou usar os *ParameterBindingRules* no *HttpConfiguration* para personalizar as associações de parâmetro mais amplamente.
- **Melhorias do MediaTypeFormatter:** formatadores agora têm acesso a toda *HttpContent* instância.
- **Seleção de política de buffer do host:** implementar e configurar o *IHostBufferPolicySelector* serviço na API Web do ASP.NET para habilitar os hosts determinar a política para quando o buffer é a ser usado.
- **Acessar certificados de cliente de maneira independente de host:** Use o *GetClientCertificate* método de extensão para obter o certificado de cliente fornecido da mensagem de solicitação.
- **Extensibilidade de negociação de conteúdo:** personalizar negociação de conteúdo, derivando do *DefaultContentNegotiator* e substituindo todos os aspectos de negociação de conteúdo que você gostaria.
- **Suporte para retornar 406 respostas não aceitável:** agora você pode retornar 406 respostas não aceitável na API Web ASP.NET quando um formatador adequado não foi encontrado com a criação de um *DefaultContentNegotiator* com o *excludeMatchOnTypeOnly* parâmetro definido como *verdadeira*.
- **Ler dados de formulário como NameValueCollection ou JToken:** você pode ler dados de formulário na cadeia de caracteres de consulta URI ou no corpo da solicitação como um *NameValueCollection* usando o *ParseQueryString* e  *ReadAsFormDataAsync* métodos de extensão, respectivamente. Da mesma forma, você pode ler dados de formulário na cadeia de caracteres de consulta URI ou no corpo da solicitação como um *JToken* usando o *TryReadQueryAsJson* e *ReadAsAsync*&lt;T&gt; métodos de extensão, respectivamente.
- **Melhorias de várias partes:** agora é possível escrever um *MultipartStreamProvider* que completamente feita sob medida para o tipo de dados com diversas partes MIME que pode ler e apresentará o resultado da maneira ideal para o usuário. Você também pode utilizar uma etapa de processamento de postagem sobre a *MultipartStreamProvider* que permite a implementação fazer qualquer pós-processamento ele quer nas partes de corpo com diversas partes MIME. Por exemplo, o *MultipartFormDataStreamProvider* implementação lê o formulário HTML partes de dados e adiciona a um *NameValueCollection* então são fáceis de obter do chamador.
- **Aprimoramentos de geração de link:** as *UrlHelper* não depende mais *HttpControllerContext*. Agora você pode acessar o *UrlHelper* de qualquer contexto em que o *HttpRequestMessage* está disponível.
- **Alteração de ordem de execução do manipulador de mensagens:** manipuladores de mensagens agora são executadas na ordem em que eles são configurados em vez de em ordem inversa.
- **Auxiliar de conectar os manipuladores de mensagens:** o novo *HttpClientFactory* que pode conectar *DelegatingHandlers* e crie um *HttpClient* com o pipeline desejado pronto para começar. Ele também fornece funcionalidade para conectar com manipuladores internos alternativos (o padrão é *HttpClientHandler*), bem como fazer a ligação de ao usar *HttpMessageInvoker* ou em outro  *DelegatingHandler* em vez de *HttpClient* como o chamador da parte superior.
- **Suporte para as CDNs na otimização da Web ASP.NET:** ASP.NET Web Optimization agora dá suporte aos caminhos alternativos CDN, permitindo que você especificar para cada mais uma URL que aponta para o mesmo recurso em uma rede de distribuição de conteúdo do pacote. Suporte a CDNs permite que você obtenha seus pacotes de script e estilo geograficamente mais próximo para os consumidores finais dos seus aplicativos Web. Aplicativos de produção devem implementar um fallback quando o CDN não estiver disponível. Teste o fallback.
- **Rotas API Web ASP.NET e configuração é movido para *Webapiconfig* método estático que pode ser resused no código de teste.** Rotas API Web ASP.NET foram adicionadas anteriormente na *RouteConfig.RegisterRoutes* juntamente com o padrão MVC encaminha. O padrão que roteia de API Web ASP.NET e a configuração agora são manipulados em um separado *Webapiconfig* método para facilitar os testes.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conhecidos e alterações recentes

- **A versão RC e RTM do ASP.NET MVC 4 incorretamente retornado modos de exibição da área de trabalho em cache quando os modos de exibição móveis devem ser retornados.**

    - Ver [ASP.NET MVC 4 Mobile cache Bug fixa](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obter detalhes sobre a correção. A correção pode ser instalada do [DisplayModes fixa](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pacote do NuGet.
- **Alterações significativas no mecanismo de exibição Razor**. Os seguintes tipos foram removidos da *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Os métodos a seguir também foram removidos: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Quando WebMatrix.WebData.dll é incluído no diretório /bin de um aplicativos ASP.NET MVC 4, ele assume a URL para a autenticação de formulários.** Adicionando o assembly WebMatrix.WebData.dll ao seu aplicativo (por exemplo, selecionando "ASP.NET páginas da Web com sintaxe do Razor" ao usar a caixa de diálogo Adicionar dependências implantáveis) substituirá o redirecionamento de logon de autenticação para o logon/conta/vez / conta/logon conforme o esperado pelo controlador de conta do ASP.NET MVC padrão. Para evitar esse comportamento e usar a URL especificada já na seção de autenticação da Web. config, você pode adicionar um appSetting chamado PreserveLoginUrl e defini-lo como true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **O Gerenciador de pacotes do NuGet Falha na instalação ao tentar instalar o ASP.NET MVC 4 para instalações lado a lado do Visual Studio 2010 e o Visual Web Developer 2010.** Para executar o Visual Studio 2010 e o Visual Web Developer 2010 lado a lado com o ASP.NET MVC 4, você deve instalar ASP.NET MVC 4 depois que ambas as versões do Visual Studio já foram instaladas.
- **Desinstalar o ASP.NET MVC 4 falhará se os pré-requisitos já tiverem sido desinstalados.** Para desinstalar corretamente o ASP.NET MVC 4you deve desinstalar o ASP.NET MVC 4 antes de desinstalar o Visual Studio.
- **Instalar o ASP.NET MVC 4 interrompe aplicativos da versão RTM do ASP.NET MVC 3.** Versão de aplicativos do ASP.NET MVC 3 que foram criados com o RTM (não com o [atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/download/details.aspx?id=1491) release) exigem as seguintes alterações para funcionar lado a lado com o ASP.NET MVC 4. Criando o projeto sem fazer esses resultados de atualizações em erros de compilação. 

    **Atualizações necessárias**

  1. No arquivo Web. config raiz, adicione um novo *&lt;appSettings&gt;* entrada com a chave *webPages:Version* e o valor *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. No Gerenciador de soluções, clique com botão direito no nome do projeto e, em seguida, selecione descarregar projeto. Em seguida, clique no nome novamente e selecione Editar *ProjectName*. csproj.
  3. Localize as seguintes referências de assembly: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Substitua-os pelo seguinte:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Salve as alterações, feche o arquivo de projeto (. csproj) que você estava editando e, em seguida, clique com botão direito no projeto e selecione Recarregar.

- **Alterando um projeto ASP.NET MVC 4 para o destino 4.0 do 4.5 não atualiza a referência de assembly EntityFramework:** se você alterar um projeto ASP.NET MVC 4 para o destino 4.0 após direcionado 4.5 continuarão a apontar para a referência ao assembly EntityFramework a versão 4.5. Para corrigir a desinstalação do problema e reinstale o pacote EntityFramework NuGet.
- **403 Proibido ao executar um aplicativo ASP.NET MVC 4 no Azure após a mudança para o destino 4.0 do 4.5:** se você alterar um projeto ASP.NET MVC 4 para o destino 4.0 após direcionado 4.5 e, em seguida, implantar no Azure, você verá um erro de 403 Proibido em tempo de execução. Para solucionar este problema, adicione o seguinte ao Web. config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 falhar quando você digita um '\' em uma cadeia de caracteres literal em um arquivo Razor.** Para trabalhar em torno do problema inserir aspas de fechamento da cadeia de caracteres literal pela primeira vez.
- <strong>Navegando até &quot;conta/gerenciar&quot; nos resultados do modelo da Internet em um erro de tempo de execução de idiomas CHS, TRK e CHT.</strong> Para corrigir o problema de modificar a página para separar <em>@User.Identity.Name</em> colocando-o como o único conteúdo dentro de <em>&lt;forte&gt;</em> marca.
- **Google e LinkedIn provedores não têm suporte em Sites do Azure.** Use provedores de autenticação alternativo ao implantar em Sites do Azure.
- **Ao usar UriPathExtensionMapping com o IIS 8 Express/IIS, você receberia 404 erros não encontrado ao tentar usar a extensão.** O manipulador de arquivo estático irá interferir com as solicitações para APIs web que usam *UriPathExtensionMappings*. Definir *runAllManagedModulesForAllRequests = true* no Web. config para contornar o problema.
- **Método Controller.Execute não é chamado.** Todos os controladores do MVC agora sempre executados de maneira assíncrona.
