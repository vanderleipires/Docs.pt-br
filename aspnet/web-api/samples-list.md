---
uid: web-api/samples-list
title: Lista de exemplos de API da Web | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 1e1f43bbeedfc052f0b3a3924f51b544a5a79dca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038982"
---
<a name="web-api-samples-list"></a>Lista de exemplos de API da Web
====================
## <a name="httpclient-samples"></a>Amostras de HttpClient

**Exemplo do Bing traduzir** | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Mostra como chamar o [serviço Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) usando o **HttpClient** classe. A API do serviço Microsoft Translator requer um token de OAuth, o aplicativo obtém enviando uma solicitação para o servidor de token do Azure para cada solicitação para o serviço de conversor. O resultado do servidor de token é alimentado na solicitação enviada para o serviço de tradução. Antes de executar este exemplo, você deve obter um [chave de aplicativo do Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) e preencha as informações na classe de exemplo AccessTokenMessageHandler.

**Exemplo do Google Maps** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Usa **HttpClient** para baixar um mapa de Redmond, WA de [API do Google Maps](https://developers.google.com/maps/), salva-o como um arquivo local e abre o Visualizador de imagem padrão.

**Exemplo de cliente do Twitter** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Mostra como gravar um cliente simples do Twitter usando **HttpClient**. O exemplo usa um **HttpMessageHandler** para inserir informações de autenticação OAuth para a saída **HttpRequestMessage**. O resultado do Twitter é leitura usando JSON.NET. Antes de executar este exemplo, você deve obter um [chave de aplicativo do Twitter](https://dev.twitter.com/)e preencha as informações na classe de exemplo OAuthMessageHandler.

**Exemplo de banco do mundo** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Mostra como recuperar dados do site de dados do banco do mundo, usando JSON.NET para analisar o resultado.

## <a name="web-api-samples"></a>Exemplos de API da Web

**Introdução ao ASP.NET Web API** | [fonte VS 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Mostra como criar uma web básico API que oferece suporte a solicitações HTTP GET. Contém o código-fonte para o tutorial [sua primeira API da Web ASP.NET](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.NET Web API JavaScript cenários – comentários** | [fonte VS 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Mostra como usar a API da Web do ASP.NET para criar APIs da web que oferecem suporte a clientes de navegador e podem facilmente ser chamado usando jQuery.

**Entre em contato com o Gerenciador** | [fonte VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Este exemplo usa a API da Web do ASP.NET para criar um aplicativo simples de Gerenciador de contato. O aplicativo consiste em um Gerenciador de contato API da web que é usado por um aplicativo ASP.NET MVC e um aplicativo do Windows Phone para exibir e gerenciar uma lista de contatos.

**Processamento em lotes de exemplo** | [descrição detalhada](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Mostra como implementar HTTP envio em lote dentro do ASP.NET. O envio em lote consiste em colocar várias solicitações HTTP em um corpo de entidade com diversas partes MIME único, que é enviada ao servidor como um HTTP POST. As solicitações são processadas individualmente, e as respostas são colocadas no corpo da entidade com diversas partes MIME outra, que é retornado ao cliente.

**Exemplo de controlador de conteúdo** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 fonte](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Mostra como ler e gravar as entidades de solicitação e resposta usando fluxos de forma assíncrona. O controlador de exemplo possui duas ações: uma ação de PUT que lê o corpo da entidade de solicitação de forma assíncrona e as armazena em um arquivo local e uma ação de GET que retorna o conteúdo do arquivo local.

**Exemplo de resolvedor de Assembly personalizado** | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Mostra como modificar a API Web do ASP.NET para dar suporte à descoberta de controladores de um assembly de biblioteca carregados dinamicamente. O exemplo implementa um personalizado **IAssembliesResolver** que chama a implementação padrão e, em seguida, adiciona o assembly de biblioteca para os resultados de padrão.

**Exemplo de formatador do tipo de mídia personalizado** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Mostra como criar um formatador de tipo de mídia personalizado usando o **BufferedMediaTypeFormatter** classe base. Essa classe base é destinado formatadores que principalmente usam leitura síncrona e operações de gravação. Além de Mostrar formatador do tipo de mídia, o exemplo mostra como conectar, registrando-os como parte do **HttpConfiguration** para seu aplicativo. Observe que também é possível usar o **MediaTypeFormatter** classe base diretamente, formatadores que usem principalmente assíncrona de leitura e operações de gravação.

**O exemplo de associação de parâmetro personalizado** | [descrição detalhada](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Mostra como personalizar o processo de associação de parâmetro, que é o processo que determina como as informações de uma solicitação estão vinculadas a parâmetros de ação. Neste exemplo, o controlador Home tem quatro ações:

1. BindPrincipal mostra como associar um parâmetro de IPrincipal de um objeto genérico personalizado, não a partir de uma mensagem HTTP GET;
2. BindCustomComplexTypeFromUriOrBody mostra como associar um parâmetro de tipo complexo, que pode vir do corpo da mensagem ou da solicitação de URI de uma mensagem HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty mostra como associar um parâmetro de tipo complexo com uma propriedade renomeada que vem do URI de uma mensagem HTTP POST; de solicitação
4. PostMultipleParametersFromBody mostra como associar vários parâmetros de corpo de uma mensagem de POSTAGEM.

**Exemplo de carregamento de arquivo** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Mostra como carregar arquivos para um **ApiController** usando o carregamento do arquivo com diversas partes MIME e como configurar notificações de andamento com **HttpClient** usando **ProgressNotificationHandler**. O controlador lê o conteúdo de um carregamento de arquivo HTML de forma assíncrona e grava uma ou mais partes de corpo para um arquivo local. A resposta contém informações sobre o arquivo carregado (ou arquivos).

**Arquivo de carregamento para o exemplo de armazenamento de Blob do Azure** | [descrição detalhada](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Este exemplo é semelhante ao exemplo de carregar arquivo, mas em vez de salvar os arquivos carregados no disco local, ele carrega assincronamente os arquivos a serem [armazenamento de Blob do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) usando [SDK do Windows Azure para .NET](https://www.windowsazure.com/develop/net/). Ele também fornece um mecanismo para listar os blobs presentes no momento uma [contêiner de armazenamento de Blob do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Você pode testar o exemplo executando em **emulador de armazenamento do Azure** que vem com o SDK do Azure. Se você tiver um [conta de armazenamento do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), você pode executar no serviço de armazenamento real.

**Exemplo de Pipeline de manipulador de mensagem HTTP** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Mostra como conectar **HttpMessageHandler** instâncias em que o cliente (**HttpClient**) e o servidor (API da Web do ASP.NET). No exemplo, o mesmo manipulador é usado no cliente e no servidor. Embora seja raro que o mesmo manipulador exato seria executado em ambos, o modelo de objeto é o mesmo no lado do cliente e servidor.

**Exemplo de carregamento de JSON** | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Mostra como carregar e baixar o JSON para e de um **ApiController**. O exemplo usa um mínimo **ApiController** e acessa usando **HttpClient**.

**Exemplo de mashup** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Mostra como acessar vários sites remotos de forma assíncrona de dentro um **ApiController** ação. Cada vez que a ação for atingida, as solicitações são executadas de forma assíncrona, para que nenhum thread é bloqueado.

**Exemplo de rastreamento de memória** | [descrição detalhada](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Este projeto de exemplo cria um pacote do Nuget que instalará um gravador de rastreamento personalizada de na memória em aplicativos de API da Web ASP.NET.

**Exemplo do MongoDB** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Mostra como usar o MongoDB como o armazenamento persistente para um **ApiController**, usando um padrão de repositório.

**Exemplo de processador de corpo de resposta** | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Mostra como copiar uma entidade de resposta (ou seja, um corpo de resposta HTTP) para um arquivo local antes de ser transmitido para o cliente e executar o processamento adicional sobre esse arquivo de forma assíncrona. O exemplo implementa um **HttpMessageHandler** que encapsula a entidade de resposta com uma que ambos sejam gravados em si para a saída como normal e um arquivo local.

**Carregar um XDocument exemplo** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [fonte VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Mostra como carregar um XDocument para um **ApiController** usando **PushStreamContent** e **HttpClient**.

**Exemplo de validação de** | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Mostra como usar atributos de validação em seus modelos no ASP.NET WebAPI para validar o conteúdo da solicitação HTTP. Demonstra como marcar propriedades conforme necessário, como usar ambas definidas pelo framework e atributos de validação personalizada para anotar o seu modelo e a retornar respostas de erro para estados de modelo inválido.

**Exemplo de formulário de Web** | [descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Mostra um ApiController adicionado a um projeto de Web Forms.

**[Exemplo de RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs é um aplicativo que mostra como usar a API da Web do ASP.NET e a nova biblioteca de cliente HTTP para criar um sistema orientado hipermídia de monitoramento de erros simples. O exemplo inclui implementações de cliente e servidor, usando a API da Web do ASP.NET. O servidor usa um formatador personalizado do Razor gerar representações de recurso. O exemplo também fornece um servidor node.js para ilustrar os benefícios que vêm com o uso de um design de hipermídia para desacoplar a clientes e servidores.

## <a name="web-api-extensions-preview-samples"></a>Exemplos de visualização de extensões de API da Web

**Exemplo de consulta OData** | [descrição detalhada](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Mostra como introduzir consultas OData na API Web do ASP.NET usando o `[Queryable]` de atributo ou usando o **ODataQueryOptions** parâmetro de ação que permite que a ação a ser inspecionado manualmente a consulta antes que ele está sendo executado.

O CustomerController mostra o uso de atributo [Queryable] e o OrderController mostra como usar o parâmetro ODataQueryOptions. O ResponseController é semelhante a CustomerController, mas em vez da ação GET retornando `IEnumerable<Customer>` retorna um **HttpResponseMessage**. Isso permite adicionar campos de cabeçalho extra, manipular o código de status, etc. enquanto estiver usando a funcionalidade de consulta. O exemplo ilustra consultas usando $orderby, $skip, $top, any (), All (), Skip e $ $filter.

**Exemplo de serviço OData** | [descrição detalhada](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [fonte VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Este exemplo ilustra como criar um serviço OData consiste em três entidades e três controladores da API da Web. Os controladores fornecem vários níveis de funcionalidade em termos de expõem a funcionalidade de OData:

O SupplierController expõe um subconjunto da funcionalidade, incluindo a consulta, obter chave e criar, tratando essas solicitações:

- OBTER /Suppliers
- OBTER /Suppliers(key)
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

Expõe o ProductsController GET, PUT, POST, excluir e PATCH com a implementação de uma ação para cada uma dessas operações diretamente.

O ProductFamilesController utiliza a classe de base EntitySetController que expõe um padrão útil para implementar um serviço OData avançado.

Além disso, o serviço OData expõe um documento $metadata que permite que os dados para o consumido pelos clientes do WCF Data Services e outros clientes que aceita o formato $metadata.
