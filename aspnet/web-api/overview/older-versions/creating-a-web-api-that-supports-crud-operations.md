---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar operações CRUD de ASP.NET Web API 1 | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando ASP.NET Web API. Versões de software usadas no tutorial Visual Studio 2012 Web ponto de acesso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "29153002"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Habilitar operações CRUD de ASP.NET Web API 1
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Visual Studio 2012
> - API Web 1 (também funciona com a API Web 2)


CRUD significa &quot;criar, ler, atualizar e excluir,&quot; quais são as quatro operações de banco de dados básico. Muitos serviços HTTP também modelo operações CRUD por meio de REST ou APIs de REST.

Neste tutorial, você criará uma API para gerenciar uma lista de produtos de web muito simple. Cada produto contém um nome, o preço e a categoria (como &quot;toys&quot; ou &quot;hardware&quot;), além de uma ID de produto.

Os produtos de API expõe métodos a seguir.

| Ação | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter uma lista de todos os produtos | OBTER | produtos/api / |
| Obter um produto por ID | OBTER | /API/produtos/*id* |
| Obter um produto por categoria | OBTER | /api/products?category=*category* |
| Criar um novo produto | POSTAR | produtos/api / |
| Atualização de um produto | PUT | /API/produtos/*id* |
| Excluir um produto | DELETE | /API/produtos/*id* |

Observe que alguns dos URIs incluem a identificação do produto no caminho. Por exemplo, para obter o produto cuja ID é 28, o cliente envia uma solicitação GET `http://hostname/api/products/28`.

### <a name="resources"></a>Recursos

Os produtos API define URIs para os dois tipos de recursos:

| Recurso | URI |
| --- | --- |
| A lista de todos os produtos. | produtos/api / |
| Um produto individual. | /API/produtos/*id* |

### <a name="methods"></a>Métodos

Os quatro principais métodos HTTP (GET, PUT, POST e DELETE) podem ser mapeados para as operações CRUD da seguinte maneira:

- GET recupera a representação do recurso em um URI especificado. GET deve não têm efeitos colaterais no servidor.
- PUT atualiza um recurso em um URI especificado. PUT também pode ser usado para criar um novo recurso em um URI especificado, se o servidor permite que os clientes especificar os URIs de novo. Para este tutorial, a API não dará suporte a criação por meio de PUT.
- POST cria um novo recurso. O servidor atribui o URI para o novo objeto e retorna esse URI como parte da mensagem de resposta.
- DELETE Exclui um recurso em um URI especificado.

Observação: O método PUT substitui a entidade de produtos inteiro. Ou seja, o cliente deve enviar uma representação completa do produto atualizado. Se você desejar oferecer suporte a atualizações parciais, o método de PATCH é preferencial. Este tutorial não implementa o PATCH.

## <a name="create-a-new-web-api-project"></a>Criar um novo projeto de API da Web

Comece executando o Visual Studio e selecione **novo projeto** do **iniciar** página. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto &quot;ProductStore&quot; e clique em **Okey**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **API da Web** e clique em **Okey**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Adicionando um modelo

Um *modelo (model)* é um objeto que representa os dados em seu aplicativo. Na API da Web do ASP.NET, fortemente tipados objetos CLR podem ser usados como modelos e ele serão automaticamente serializados como XML ou JSON para o cliente.

Para a API ProductStore, nossos dados consistem em produtos, portanto, vamos criar uma nova classe chamada `Product`.

Se o Gerenciador de Soluções não estiver visível, clique no menu **Exibir** e selecione **Gerenciador de Soluções**. No Gerenciador de soluções, clique com botão direito do **modelos** pasta. Meny contexto, selecione **adicionar**, em seguida, selecione **classe**. Nomeie a classe &quot;Produt&quot; (produto).

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Adicione as seguintes propriedades para a classe `Product`.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Adicionar um repositório

É necessário armazenar um conjunto de produtos. É uma boa ideia para separar a coleção de nossa implementação de serviço. Dessa forma, podemos alterar o repositório de backup sem reescrever a classe de serviço. Esse tipo de design é chamado de *repositório* padrão. Começar definindo uma interface genérica para o repositório.

No Gerenciador de soluções, clique com botão direito do **modelos** pasta. Selecione **adicionar**, em seguida, selecione **Novo Item**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

No **modelos** painel, selecione **modelos instalados** e expanda o nó C#. Em c#, selecione **código**. Na lista de modelos de código, selecione **Interface**. Nome da interface &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Adicione a implementação a seguir:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Agora adicione outra classe para a pasta de modelos, denominada &quot;ProductRepository.&quot; Essa classe implementará a interface `IProductRespository`. Adicione a implementação a seguir:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

O repositório mantém a lista na memória local. Isso é Okey para obter um tutorial, mas em um aplicativo real, você deve armazenar os dados externamente, um banco de dados ou no armazenamento em nuvem. O padrão de repositório tornará mais fácil de alterar a implementação mais tarde.

## <a name="adding-a-web-api-controller"></a>Adicionar um controlador de API da Web

Se você trabalhou com o ASP.NET MVC, em seguida, você já está familiarizado com controladores. Na API da Web do ASP.NET, um *controlador* é uma classe que trata as solicitações HTTP do cliente. O Assistente de novo projeto criado dois controladores quando ele criou o projeto. Para vê-los, expanda a pasta de controladores no Gerenciador de soluções.

- HomeController é um controlador MVC do ASP.NET tradicional. Ele é responsável pelo fornecimento de páginas HTML para o site e não está diretamente relacionado ao nosso API da web.
- ValuesController é um controlador de WebAPI de exemplo.

Vá em frente e excluir ValuesController, clicando duas vezes o arquivo no Gerenciador de soluções e selecionando **excluir.** Agora adicione um novo controlador, da seguinte maneira:

Em **Gerenciador de Soluções**, clique com o botão direito na pasta controllers (controladores).  Selecione **Adicionar** e, em seguida, selecione **Controlador**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

No **Adicionar controlador** assistente, o nome do controlador &quot;ProductsController&quot;. No **modelo** lista suspensa, selecione **controlador API vazio**. Em seguida, clique em **adicionar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Não é necessário colocar seus controladores em uma pasta chamada controladores. O nome da pasta não é importante; ele é simplesmente uma maneira conveniente de organizar seus arquivos de origem.


O **Adicionar controlador** assistente criará um arquivo chamado ProductsController.cs na pasta controladores. Se esse arquivo não estiver aberto, clique duas vezes no arquivo para abri-lo. Adicione o seguinte **usando** instrução:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Adicionar um campo que contém um **IProductRepository** instância.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Chamando `new ProductRepository()` no controlador não é o melhor design, porque ela unirá o controlador a uma implementação específica de `IProductRepository`. Uma abordagem melhor, consulte [usando o resolvedor de dependência de API da Web](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Obtendo um recurso

A API ProductStore irá expor vários &quot;ler&quot; ações como métodos HTTP GET. Cada ação corresponderá a um método em de `ProductsController` classe.

| Ação | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter uma lista de todos os produtos | OBTER | produtos/api / |
| Obter um produto por ID | OBTER | /API/produtos/*id* |
| Obter um produto por categoria | OBTER | /api/products?category=*category* |

Para obter a lista de todos os produtos, adicione este método para o `ProductsController` classe:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

O nome do método começa com &quot;obter&quot;, portanto, por convenção, ele será mapeado para solicitações GET. Além disso, porque o método não tem parâmetros, ele será mapeado para um URI que não contém um *&quot;id&quot;* segmento do caminho.

Para obter um produto por ID, adicione este método para o `ProductsController` classe:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Esse nome de método também inicia com &quot;obter&quot;, mas o método tem um parâmetro denominado *id*. Esse parâmetro é mapeado para o &quot;id&quot; segmento do caminho URI. A estrutura do ASP.NET Web API converte automaticamente a ID para o tipo de dados correto (**int**) para o parâmetro.

O método GetProduct lança uma exceção do tipo **HttpResponseException** se *id* não é válido. Essa exceção será convertida pela estrutura em um erro 404 (não encontrado).

Finalmente, adicione um método para localizar produtos por categoria:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Se o URI da solicitação tem uma cadeia de caracteres de consulta, API da Web tenta corresponder aos parâmetros de consulta para parâmetros de método do controlador. Portanto, um URI do formulário "api/produtos? categoria =*categoria*" será mapeado para esse método.

## <a name="creating-a-resource"></a>Criar um recurso

Em seguida, vamos adicionar um método para o `ProductsController` classe para criar um novo produto. Aqui está uma simple implementação do método:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Observe que duas coisas sobre este método:

- O nome do método começa com &quot;Post... &quot;. Para criar um novo produto, o cliente envia uma solicitação HTTP POST.
- O método usa um parâmetro de tipo de produto. Na API da Web, os parâmetros com tipos complexos são desserializar o corpo da solicitação. Portanto, esperamos que o cliente envie uma representação serializada de um objeto de produto, em formato XML ou JSON.

Essa implementação funcionará, mas não está completo. Idealmente, gostaríamos de receber a resposta HTTP para incluem o seguinte:

- **Código de resposta:** por padrão, a estrutura da API Web define o código de status de resposta para 200 (Okey). Mas de acordo com o protocolo HTTP/1.1, quando uma solicitação POST resulta na criação de um recurso, o servidor deve responder com status 201 (criado).
- **Local:** quando o servidor cria um recurso, ele deve incluir o URI do novo recurso no cabeçalho de local da resposta.

ASP.NET Web API facilita manipular a mensagem de resposta HTTP. Esta é a implementação aprimorada:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Observe que o tipo de retorno do método agora é **HttpResponseMessage**. Retornando um **HttpResponseMessage** em vez de um produto, é possível controlar os detalhes da mensagem de resposta HTTP, incluindo o código de status e o cabeçalho de local.

O **CreateResponse** método cria um **HttpResponseMessage** e grava uma representação serializada do objeto Product automaticamente para o corpo fo a mensagem de resposta.

> [!NOTE]
> Este exemplo não valida o `Product`. Para obter informações sobre a validação de modelo, consulte [validação de modelo no ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Atualizando um recurso

Atualizar um produto com PUT é simples:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

O nome do método começa com &quot;colocar... &quot;, portanto, API da Web corresponde a ele para solicitações PUT. O método aceita dois parâmetros, a ID do produto e o produto atualizado. O *id* parâmetro é obtido do caminho de URI e o *produto* parâmetro é desserializado do corpo da solicitação. Por padrão, a estrutura do ASP.NET Web API usa os tipos de parâmetro simples do roteiro e tipos complexos do corpo da solicitação.

## <a name="deleting-a-resource"></a>Excluir um recurso

Para excluir um resourse, defina um método "Excluir...".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Se uma solicitação de exclusão for bem-sucedida, ela pode retornar o status 200 (Okey) com um corpo de entidade que descreve o status; status 202 (aceito) se a exclusão ainda pendentes; ou status 204 (sem conteúdo) sem corpo de entidade. Nesse caso, o `DeleteProduct` método tem um `void` tipo de retorno, para a API da Web ASP.NET converte isso automaticamente em status 204 (sem conteúdo) de código.
