---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Criar um ponto de extremidade OData v3 com Web API 2 | Microsoft Docs
author: MikeWasson
description: O Open Data Protocol (OData) é um protocolo de acesso de dados para a web. O OData fornece uma maneira uniforme de estrutura de dados, consultar os dados e manipular os dados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 227faacd3f42731e08a4cd2b71075776309961b6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Criar um ponto de extremidade OData v3 com Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> O [Open Data Protocol](http://www.odata.org/) (OData) é um protocolo de acesso de dados para a web. O OData fornece uma maneira uniforme de estrutura de dados, consultar os dados e manipular o conjunto de dados por meio de operações CRUD (criar, ler, atualizar e excluir). OData oferece suporte a formatos JSON e AtomPub (XML). OData define também uma maneira para expor metadados sobre os dados. Os clientes podem usar os metadados para descobrir as informações de tipo e as relações para o conjunto de dados.
> 
> ASP.NET Web API torna fácil criar um ponto de extremidade OData para um conjunto de dados. Você pode controlar exatamente quais operações OData oferece suporte a ponto de extremidade. Você pode hospedar vários pontos de extremidade OData, juntamente com pontos de extremidade OData não. Você tem controle total sobre o modelo de dados, a lógica de negócios de back-end e a camada de dados.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData versão 3
> - Entity Framework 6
> - [O Fiddler Web Proxy (opcional) de depuração](http://www.fiddler2.com)
> 
> Suporte de OData da API da Web foi adicionado na [ASP.NET e Web Tools 2012.2 atualização](https://go.microsoft.com/fwlink/?LinkId=282650). No entanto, este tutorial usa scaffolding foi adicionado no Visual Studio 2013.


Neste tutorial, você criará um ponto de extremidade OData simple que os clientes podem consultar. Você também criará um cliente c# para o ponto de extremidade. Depois de concluir este tutorial, o próximo conjunto de tutoriais mostram como adicionar mais funcionalidade, incluindo as relações de entidade, ações, e selecione $Expandir / $.

- [Criar o projeto do Visual Studio](#create-project)
- [Adicionar um modelo de entidade](#add-model)
- [Adicionar um controlador OData](#add-controller)
- [Adicione o EDM e rota](#edm)
- [Propagação do banco de dados (opcional)](#seed-db)
- [Explorando o ponto de extremidade OData](#explore)
- [Formatos de serialização do OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Neste tutorial, você criará um ponto de extremidade OData que suporta operações CRUD básicas. O ponto de extremidade irá expor um único recurso, uma lista de produtos. Tutoriais posteriores adicionará mais recursos.

Inicie o Visual Studio e selecione **novo projeto** na página de início. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

No **modelos** painel, selecione **modelos instalados** e expanda o nó do Visual c#. Em **Visual C#**, selecione **Web**. Selecione **aplicativo Web ASP.NET** modelo.

![](creating-an-odata-endpoint/_static/image1.png)

Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**. Em &quot;adicionar pastas e referências de núcleo... &quot;, verifique **API da Web**. Clique em **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Adicionar um modelo de entidade

Um *modelo (model)* é um objeto que representa os dados em seu aplicativo. Para este tutorial, precisamos de um modelo que representa um produto. O modelo corresponde ao nosso tipo de entidade do OData.

No Gerenciador de Soluções, clique com o botão direito na pasta de modelos (Models). No menu de contexto, selecione **Adicionar** , em seguida, selecione **Classe**.

![](creating-an-odata-endpoint/_static/image3.png)

No **adicionar novo** Item de caixa de diálogo, nomeie a classe &quot;produto&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Por convenção, as classes de modelo são colocados na pasta modelos. Você não precisa seguir essa convenção em seus projetos, mas usaremos para este tutorial.


No arquivo Product.cs, adicione a seguinte definição de classe:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

A propriedade ID será a chave de entidade. Os clientes podem consultar os produtos por ID. Este campo também seria a chave primária no banco de dados back-end.

Compile o projeto agora. Na próxima etapa, vamos usar alguns scaffolding do Visual Studio que usa reflexão para encontrar o tipo de produto.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Adicionar um controlador OData

Um *controlador* é uma classe que trata as solicitações HTTP. Você definir um controlador separado para cada entidade definida no serviço OData. Neste tutorial, vamos criar um único controlador.

No Gerenciador de soluções, clique na pasta de controladores. Selecione **Adicionar** e, em seguida, selecione **Controlador**.

![](creating-an-odata-endpoint/_static/image5.png)

No **adicionar Scaffold** caixa de diálogo, selecione &quot;Web API 2 OData controlador com ações, usando o Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

No **Adicionar controlador** caixa de diálogo, o nome do controlador "ProductsController". Selecione o &quot;usar ações assíncronas do controlador&quot; caixa de seleção. No **modelo** lista suspensa, selecione a classe do produto.

![](creating-an-odata-endpoint/_static/image7.png)

Clique o **novo contexto de dados...**  botão. Deixe o nome padrão para o tipo de contexto de dados e, em seguida, clique em **adicionar**.

![](creating-an-odata-endpoint/_static/image8.png)

Na caixa de diálogo Adicionar controlador para adicionar o controlador, clique em Adicionar.

![](creating-an-odata-endpoint/_static/image9.png)

Observação: Se você receber uma mensagem de erro que diz &quot;Ocorreu um erro ao obter o tipo... &quot;, certifique-se de que você criou o projeto do Visual Studio depois que você adicionou a classe do produto. O scaffolding usa reflexão para localizar a classe.

![](creating-an-odata-endpoint/_static/image10.png)

O scaffolding adiciona dois arquivos de código ao projeto:

- Products.cs define o controlador de API da Web que implementa o ponto de extremidade OData.
- ProductServiceContext.cs fornece métodos para consultar o banco de dados subjacente, usando o Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Adicione o EDM e rota

No Gerenciador de soluções, expanda o aplicativo\_pasta e abra o arquivo chamado WebApiConfig.cs. Essa classe contém o código de configuração para a API da Web. Substitua esse código pelo seguinte:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Este código faz duas coisas:

- Cria uma entidade de dados EDM (modelo) para o ponto de extremidade OData.
- Adiciona uma rota para o ponto de extremidade.

Um EDM é um modelo abstrato dos dados. EDM é usado para criar o documento de metadados e definir os URIs para o serviço. O **ODataConventionModelBuilder** cria um EDM usando um conjunto de convenções de nomenclatura padrão EDM. Essa abordagem exige o mínimo de código. Se você quiser mais controle sobre o EDM, você pode usar o **ODataModelBuilder** classe para criar o EDM Adicionando propriedades, chaves e propriedades de navegação explicitamente.

O **EntitySet** método adiciona uma entidade definida como EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

A cadeia de caracteres "Produtos" define o nome do conjunto de entidades. O nome do controlador deve corresponder ao nome do conjunto de entidades. Neste tutorial, o conjunto de entidades é denominado "Produtos", e o controlador `ProductsController`. Se você nomeou o conjunto de entidades "ProductSet", você poderia nomear o controlador de `ProductSetController`. Observe que um ponto de extremidade pode ter vários conjuntos de entidades. Chamar **EntitySet&lt;T&gt;**  para cada entidade definida e, em seguida, defina um controlador correspondente.

O **MapODataRoute** método adiciona uma rota para o ponto de extremidade OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

O primeiro parâmetro é um nome amigável para a rota. Os clientes do serviço não vir esse nome. O segundo parâmetro é o prefixo do URI do ponto de extremidade. Devido a esse código, o URI para o conjunto de entidades de produtos é http://<em>hostname</em>  /odata/produtos. O aplicativo pode ter mais de um ponto de extremidade OData. Para cada ponto de extremidade, chame <strong>MapODataRoute</strong> e forneça um nome de rota exclusivo e um prefixo URI exclusivo.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Propagação do banco de dados (opcional)

Nesta etapa, você usará o Entity Framework para propagar o banco de dados com alguns dados de teste. Esta etapa é opcional, mas permite que você teste seu ponto de extremidade OData imediatamente.

Do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Isso adiciona uma pasta chamada migrações e um arquivo de código chamado Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Abra o arquivo e adicione o seguinte código para o `Configuration.Seed` método.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Na janela do Console do Gerenciador de pacotes, digite os seguintes comandos:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Esses comandos geram código que cria o banco de dados e, em seguida, execute esse código.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Explorando o ponto de extremidade OData

Nesta seção, vamos usar o [Proxy de depuração da Web Fiddler](http://www.fiddler2.com) para enviar solicitações para o ponto de extremidade e examine as mensagens de resposta. Isso ajudará você a entender os recursos de um ponto de extremidade OData.

No Visual Studio, pressione F5 para iniciar a depuração. Por padrão, o Visual Studio abre seu navegador para `http://localhost:*port*`, onde *porta* é o número de porta configurado nas configurações do projeto.

Você pode alterar o número da porta nas configurações do projeto. No Gerenciador de soluções, clique com o botão direito e selecione **propriedades**. Na janela Propriedades, selecione **Web**. Insira o número da porta em **Url do projeto**.

### <a name="service-document"></a>Documento de serviço

O *documento de serviço* contém uma lista de conjuntos de entidades para o ponto de extremidade OData. Para obter o documento de serviço, envie uma solicitação GET para a raiz do URI do serviço.

Usando o Fiddler, insira o seguinte URI no **criador** guia: `http://localhost:port/odata/`, onde *porta* é o número da porta.

![](creating-an-odata-endpoint/_static/image13.png)

Clique o **Execute** botão. Fiddler envia uma solicitação HTTP GET para seu aplicativo. Você deve ver a resposta na lista de sessões da Web. Se tudo está funcionando, o código de status será 200.

![](creating-an-odata-endpoint/_static/image14.png)

Clique duas vezes a resposta na lista de sessões da Web para ver os detalhes da mensagem de resposta, na guia inspetores.

![](creating-an-odata-endpoint/_static/image15.png)

A mensagem de resposta HTTP bruta deve ser semelhante ao seguinte:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Por padrão, a API Web retorna o documento de serviço no formato de AtomPub. Para solicitar o JSON, adicione o seguinte cabeçalho à solicitação HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Agora, a resposta HTTP contém uma carga JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Documento de metadados de serviço

O *documento de metadados de serviço* descreve o modelo de dados do serviço, usando uma linguagem XML chamada linguagem de definição de esquema (CSDL) conceitual. O documento de metadados mostra a estrutura dos dados no serviço e pode ser usado para gerar o código do cliente.

Para obter o documento de metadados, envie uma solicitação GET para `http://localhost:port/odata/$metadata`. Eis os metadados para o ponto de extremidade mostrado neste tutorial.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Conjunto de entidades

Para obter o conjunto de entidades de produtos, envie uma solicitação GET para `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entidade

Para obter um produto individual, envie uma solicitação GET para `http://localhost:port/odata/Products(1)`, onde "1" é a ID do produto.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formatos de serialização do OData

OData oferece suporte a vários formatos de serialização:

- Publicação do Atom (XML)
- JSON "claro" (introduzido no OData v3)
- JSON "verbose" (OData v2)

Por padrão, a API da Web usa formato de "light" AtomPubJSON. 

Para obter o formato de AtomPub, defina o cabeçalho Accept "application/atom + xml". Aqui está um exemplo de corpo de resposta:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Você pode ver uma desvantagem óbvia do formato Atom: é muito mais detalhado do que o JSON leve. No entanto, se você tiver um cliente que compreende AtomPub, o cliente talvez prefira formato JSON.

Aqui está a versão leve do JSON da mesma entidade:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

O formato JSON light foi introduzido na versão 3 do protocolo OData. Para compatibilidade com versões anteriores, o cliente pode solicitar o antigo formato JSON "detalhado". Para solicitar JSON detalhado, defina o cabeçalho Accept `application/json;odata=verbose`. Aqui está a versão detalhada:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Esse formato transmite mais metadados no corpo da resposta, que pode adicionar uma sobrecarga considerável através de uma sessão inteira. Além disso, ele adiciona um nível de erro encapsulando o objeto em uma propriedade chamada "d".

## <a name="next-steps"></a>Próximas etapas

- [Adicionar relações de entidade](working-with-entity-relations.md)
- [Adicionar ações de OData](odata-actions.md)
- [Chamar o serviço OData de um cliente .NET](calling-an-odata-service-from-a-net-client.md)
