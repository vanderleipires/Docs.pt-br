---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Criando um ponto de extremidade OData v3 com a API Web 2 | Microsoft Docs
author: MikeWasson
description: O Open Data Protocol (OData) é um protocolo de acesso de dados para a web. O OData fornece uma maneira uniforme para a estrutura de dados, consultar os dados e manipular os dados...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 2e0d3b45fd51192d227d852dc2f05b45ca42944c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910909"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Criando um ponto de extremidade OData v3 com a API Web 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> O [Open Data Protocol](http://www.odata.org/) (OData) é um protocolo de acesso de dados para a web. O OData fornece uma maneira uniforme para a estrutura de dados, consultar os dados e manipular o conjunto de dados por meio de operações de CRUD (criar, ler, atualizar e excluir). Dá suporte a OData, os formatos de JSON e AtomPub (XML). OData define também uma maneira de expor metadados sobre os dados. Clientes podem usar os metadados para descobrir as informações de tipo e relações para o conjunto de dados.
>
> API Web ASP.NET simplifica a criação de um ponto de extremidade OData para um conjunto de dados. Você pode controlar exatamente quais operações OData oferece suporte a ponto de extremidade. Você pode hospedar vários pontos de extremidade OData, juntamente com pontos de extremidade não OData. Você tem controle total sobre seu modelo de dados, a lógica de negócios de back-end e a camada de dados.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - API Web 2
> - OData versão 3
> - Entity Framework 6
> - [Web Fiddler (opcional) do Proxy de depuração](http://www.fiddler2.com)
>
> Suporte de OData da API da Web foi adicionado no [ASP.NET e Web Tools 2012.2 atualização](https://go.microsoft.com/fwlink/?LinkId=282650). No entanto, este tutorial usa scaffolding que foi adicionado no Visual Studio 2013.


Neste tutorial, você criará um simple ponto de extremidade OData que os clientes podem consultar. Você também criará um cliente c# para o ponto de extremidade. Depois de concluir este tutorial, o próximo conjunto de tutoriais mostram como adicionar mais funcionalidade, incluindo as relações de entidade, ações, e selecione $Expandir / $.

- [Criar o projeto do Visual Studio](#create-project)
- [Adicionar um modelo de entidade](#add-model)
- [Adicionar um controlador de OData](#add-controller)
- [Adicione o EDM e roteiro](#edm)
- [Propagar o banco de dados (opcional)](#seed-db)
- [Explorando o ponto de extremidade OData](#explore)
- [Formatos de serialização do OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Criar o projeto do Visual Studio

Neste tutorial, você criará um ponto de extremidade OData que suporta operações CRUD básicas. O ponto de extremidade irá expor um único recurso, uma lista de produtos. Tutoriais subsequentes adicionarão mais recursos.

Inicie o Visual Studio e selecione **novo projeto** na página inicial. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

No **modelos** painel, selecione **modelos instalados** e expanda o nó Visual c#. Em **Visual C#**, selecione **Web**. Selecione **o aplicativo Web ASP.NET** modelo.

![](creating-an-odata-endpoint/_static/image1.png)

Na caixa de diálogo **Novo Aplicativo Web ASP.NET**, selecione o modelo **Vazio**. Sob &quot;adicionar pastas e os principais referências para... &quot;, verifique **API Web**. Clique em **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Adicionar um modelo de entidade

Um *modelo (model)* é um objeto que representa os dados em seu aplicativo. Para este tutorial, precisamos de um modelo que representa um produto. O modelo corresponde ao nosso tipo de entidade OData.

No Gerenciador de Soluções, clique com o botão direito na pasta de modelos (Models). No menu de contexto, selecione **Adicionar** , em seguida, selecione **Classe**.

![](creating-an-odata-endpoint/_static/image3.png)

No **adicionar novo** Item de caixa de diálogo, nomeie a classe &quot;produto&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Por convenção, as classes de modelo são colocadas na pasta de modelos. Você não precisa seguir essa convenção em seus próprios projetos, mas usaremos para este tutorial.


No arquivo Product.cs, adicione a seguinte definição de classe:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

A propriedade ID será a chave de entidade. Os clientes podem consultar produtos por ID. Este campo também seria a chave primária no banco de dados back-end.

Compile o projeto agora. Na próxima etapa, vamos usar algum scaffolding do Visual Studio que usa reflexão para encontrar o tipo de produto.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Adicionar um controlador de OData

Um *controlador* é uma classe que manipula as solicitações HTTP. Você define um controlador separado para cada conjunto de entidades em que o serviço OData. Neste tutorial, vamos criar um único controlador.

No Gerenciador de soluções, clique com botão direito na pasta controladores. Selecione **Adicionar** e, em seguida, selecione **Controlador**.

![](creating-an-odata-endpoint/_static/image5.png)

No **adicionar Scaffold** caixa de diálogo, selecione &quot;controlador Web API 2 OData com ações, usando o Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

No **Adicionar controlador** caixa de diálogo, nomeie o controlador "ProductsController". Selecione o &quot;usar ações do controlador assíncrono&quot; caixa de seleção. No **modelo** lista suspensa, selecione a classe Product.

![](creating-an-odata-endpoint/_static/image7.png)

Clique o **novo contexto de dados...**  botão. Deixe o nome padrão para o tipo de contexto de dados e, em seguida, clique em **adicionar**.

![](creating-an-odata-endpoint/_static/image8.png)

Clique em Adicionar na caixa de diálogo Adicionar controlador para adicionar o controlador.

![](creating-an-odata-endpoint/_static/image9.png)

Observação: Se você receber uma mensagem de erro que diz &quot;Ocorreu um erro ao obter o tipo... &quot;, certifique-se de que você compilou o projeto do Visual Studio depois que você adicionou a classe Product. O scaffolding usa reflexão para localizar a classe.

![](creating-an-odata-endpoint/_static/image10.png)

O scaffolding adiciona dois arquivos de código ao projeto:

- Products.cs define o controlador da API Web que implementa o ponto de extremidade OData.
- ProductServiceContext.cs fornece métodos para consultar o banco de dados subjacente, usando o Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Adicione o EDM e roteiro

No Gerenciador de soluções, expanda o aplicativo\_iniciar pasta e abra o arquivo chamado WebApiConfig.cs. Essa classe contém o código de configuração para a API da Web. Substitua este código com o seguinte:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Esse código faz duas coisas:

- Cria uma entidade de dados EDM (modelo) para o ponto de extremidade OData.
- Adiciona uma rota para o ponto de extremidade.

Um EDM é um modelo abstrato dos dados. O EDM é usado para criar o documento de metadados e definir os URIs para o serviço. O **ODataConventionModelBuilder** cria um EDM usando um conjunto padrão de convenções de nomenclatura EDM. Essa abordagem requer o mínimo de código. Se você quiser mais controle sobre o EDM, você pode usar o **ODataModelBuilder** classe para criar o EDM com a adição de propriedades, chaves e propriedades de navegação explicitamente.

O **EntitySet** método adiciona uma conjunto de entidades ao EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

A cadeia de caracteres "Produtos" define o nome do conjunto de entidades. O nome do controlador deve corresponder ao nome do conjunto de entidades. Neste tutorial, o conjunto de entidades é denominado "Produtos", e o controlador `ProductsController`. Se você nomeou o conjunto de entidades "ProductSet", você deve nomear o controlador `ProductSetController`. Observe que um ponto de extremidade pode ter vários conjuntos de entidades. Chame **EntitySet&lt;T&gt;**  para cada entidade definida e, em seguida, definir um controlador correspondente.

O **MapODataRoute** método adiciona uma rota para o ponto de extremidade OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

O primeiro parâmetro é um nome amigável para a rota. Os clientes de seu serviço não veem esse nome. O segundo parâmetro é o prefixo URI para o ponto de extremidade. Considerando esse código, o URI para o conjunto de entidades de produtos é http://<em>hostname</em>  /odata/produtos. O aplicativo pode ter mais de um ponto de extremidade OData. Para cada ponto de extremidade, chame <strong>MapODataRoute</strong> e forneça um nome de rota exclusivo e um prefixo URI exclusivo.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Propagar o banco de dados (opcional)

Nesta etapa, você usará o Entity Framework para propagar o banco de dados com alguns dados de teste. Esta etapa é opcional, mas permite que você teste seu ponto de extremidade OData imediatamente.

Dos **ferramentas** menu, selecione **Gerenciador de pacotes NuGet**, em seguida, selecione **Package Manager Console**. Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Isso adiciona uma pasta chamada migrações e um arquivo de código chamado Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Abra esse arquivo e adicione o seguinte código para o `Configuration.Seed` método.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Na janela do Console do Gerenciador de pacotes, digite os seguintes comandos:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Esses comandos geram código que cria o banco de dados e, em seguida, executa esse código.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Explorando o ponto de extremidade OData

Nesta seção, vamos usar o [Fiddler Web Debugging Proxy](http://www.fiddler2.com) para enviar solicitações para o ponto de extremidade e examinar as mensagens de resposta. Isso ajudará você a compreender os recursos de um ponto de extremidade OData.

No Visual Studio, pressione F5 para iniciar a depuração. Por padrão, o Visual Studio abre seu navegador para `http://localhost:*port*`, onde *porta* é o número de porta configurado nas configurações do projeto.

Você pode alterar o número da porta nas configurações do projeto. No Gerenciador de soluções, clique com botão direito no projeto e selecione **propriedades**. Na janela Propriedades, selecione **Web**. Insira o número da porta sob **Url do projeto**.

### <a name="service-document"></a>Documento de serviço

O *documento de serviço* contém uma lista dos conjuntos de entidade para o ponto de extremidade OData. Para obter o documento de serviço, envie uma solicitação GET para a raiz do URI do serviço.

Usando o Fiddler, insira o seguinte URI na **Composer** guia: `http://localhost:port/odata/`, onde *porta* é o número da porta.

![](creating-an-odata-endpoint/_static/image13.png)

Clique o **Execute** botão. Fiddler envia uma solicitação HTTP GET para seu aplicativo. Você deve ver a resposta na lista de sessões da Web. Se tudo está funcionando, o código de status será 200.

![](creating-an-odata-endpoint/_static/image14.png)

Clique duas vezes a resposta na lista de sessões da Web para ver os detalhes da mensagem de resposta na guia inspetores.

![](creating-an-odata-endpoint/_static/image15.png)

A mensagem de resposta HTTP bruta deve ser semelhante ao seguinte:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Por padrão, a API Web retorna o documento de serviço no formato AtomPub. Para solicitar o JSON, adicione o seguinte cabeçalho à solicitação HTTP:

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

OData dá suporte a vários formatos de serialização:

- Atom Pub (XML)
- JSON "light" (introduzida no OData v3)
- JSON "detalhado" (OData v2)

Por padrão, a API Web usa formato de "light" AtomPubJSON.

Para obter o formato AtomPub, defina o cabeçalho Accept como "application/atom + xml". Aqui está um exemplo de corpo de resposta:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Você pode ver uma desvantagem óbvia do formato Atom: é muito mais detalhado do que a luz JSON. No entanto, se você tiver um cliente que compreende AtomPub, o cliente talvez prefira nesse formato JSON.

Aqui está a versão leve do JSON da mesma entidade:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

O formato JSON light foi introduzido na versão 3 do protocolo OData. Para compatibilidade com versões anteriores, o cliente pode solicitar o antigo formato JSON "detalhado". Para solicitar JSON detalhado, defina o cabeçalho Accept `application/json;odata=verbose`. Aqui está a versão detalhada:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Esse formato transmite mais metadados no corpo da resposta, que pode adicionar uma sobrecarga considerável ao longo de uma sessão inteira. Além disso, ele adiciona um nível de indireção ao encapsular o objeto em uma propriedade chamada "d".

## <a name="next-steps"></a>Próximas etapas

- [Adicionar relações de entidade](working-with-entity-relations.md)
- [Adicionar ações de OData](odata-actions.md)
- [Chamar o serviço OData de um cliente .NET](calling-an-odata-service-from-a-net-client.md)
