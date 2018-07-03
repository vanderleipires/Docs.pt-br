---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar operações CRUD na API 1 da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando a API Web do ASP.NET. Versões de software usadas no tutorial Visual Studio 2012 Web ponto de acesso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 1658e120225cd3c9425168238981133c96ff398a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402922"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Habilitar operações CRUD na API 1 da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando a API Web do ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Visual Studio 2012
> - API Web 1 (também funciona com a API Web 2)


Representa o CRUD &quot;criação, leitura, atualização e exclusão,&quot; que são as quatro operações de banco de dados básico. Muitos serviços HTTP do modelo também operações CRUD por meio do REST ou APIs de REST.

Neste tutorial, você criará uma API para gerenciar uma lista de produtos de web muito simple. Cada produto contém um nome, preço e categoria (tal como &quot;toys&quot; ou &quot;hardware&quot;), além de uma ID de produto.

Os produtos de API irá expor métodos a seguir.

| Ação | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter uma lista de todos os produtos | OBTER | produtos/api / |
| Obter um produto por ID | OBTER | /API/produtos/*id* |
| Obter um produto por categoria | OBTER | /api/products?category=*category* |
| Criar um novo produto | POSTAR | produtos/api / |
| Atualizar um produto | PUT | /API/produtos/*id* |
| Excluir um produto | DELETE | /API/produtos/*id* |

Observe que alguns dos URIs incluem a ID do produto no caminho. Por exemplo, para obter o produto cuja ID é 28, o cliente envia uma solicitação GET `http://hostname/api/products/28`.

### <a name="resources"></a>Recursos

Os produtos de API define os URIs para dois tipos de recursos:

| Recurso | URI |
| --- | --- |
| A lista de todos os produtos. | produtos/api / |
| Um produto individual. | /API/produtos/*id* |

### <a name="methods"></a>Métodos

Os quatro principais métodos HTTP (GET, PUT, POST e DELETE) podem ser mapeados para operações CRUD da seguinte maneira:

- GET recupera a representação do recurso em um URI especificado. GET não deve ter nenhum efeito colateral no servidor.
- PUT atualiza um recurso em um URI especificado. PUT também pode ser usado para criar um novo recurso em um URI especificado, se o servidor permite que os clientes especifiquem os novos URIs. Para este tutorial, a API não dará suporte à criação por meio de PUT.
- POST cria um novo recurso. O servidor atribui o URI para o novo objeto e retorna esse URI como parte da mensagem de resposta.
- DELETE Exclui um recurso em um URI especificado.

Observação: O método PUT substitui a entidade product inteiro. Ou seja, o cliente deve enviar uma representação completa do produto atualizado. Se você quiser dar suporte a atualizações parciais, o método de PATCH é preferencial. Este tutorial não implementa o PATCH.

## <a name="create-a-new-web-api-project"></a>Criar um novo projeto de API da Web

Comece executando o Visual Studio e selecione **novo projeto** da **iniciar** página. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto &quot;ProductStore&quot; e clique em **Okey**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **API da Web** e clique em **Okey**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Adicionando um modelo

Um *modelo (model)* é um objeto que representa os dados em seu aplicativo. Na API Web ASP.NET, você pode usar objetos CLR fortemente tipados como modelos e eles serão automaticamente serializados para JSON ou XML para o cliente.

Para a API ProductStore, nossos dados consistem em produtos, portanto, vamos criar uma nova classe chamada `Product`.

Se o Gerenciador de Soluções não estiver visível, clique no menu **Exibir** e selecione **Gerenciador de Soluções**. No Gerenciador de soluções, clique com botão direito do **modelos** pasta. A partir do meny contexto, selecione **Add**, em seguida, selecione **classe**. Nomeie a classe &quot;Produt&quot; (produto).

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Adicione as seguintes propriedades para a classe `Product`.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Adicionar um repositório

É necessário armazenar uma coleção de produtos. É uma boa ideia separar a coleção da nossa implementação de serviço. Dessa forma, podemos alterar o repositório de backup sem reescrever a classe de serviço. Esse tipo de design é chamado de *repositório* padrão. Comece definindo uma interface genérica para o repositório.

No Gerenciador de soluções, clique com botão direito do **modelos** pasta. Selecione **Add**, em seguida, selecione **Novo Item**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

No **modelos** painel, selecione **modelos instalados** e expanda o nó do c#. Em c#, selecione **código**. Na lista de modelos de código, selecione **Interface**. Nomeie a interface &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Adicione a seguinte implementação:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Agora, adicione outra classe para a pasta de modelos, denominada &quot;ProductRepository.&quot; Essa classe implementará a interface `IProductRespository`. Adicione a seguinte implementação:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

O repositório mantém a lista na memória local. Isso é Okey para obter um tutorial, mas em um aplicativo real, você poderia armazenar os dados externamente, um banco de dados ou no armazenamento em nuvem. O padrão de repositório tornará mais fácil alterar a implementação mais tarde.

## <a name="adding-a-web-api-controller"></a>Adicionando um controlador de API da Web

Se você tiver trabalhado com o ASP.NET MVC, em seguida, você já está familiarizados com os controladores. Na API Web ASP.NET, uma *controlador* é uma classe que manipula as solicitações HTTP do cliente. O Assistente de novo projeto criado dois controladores para você quando ele criou o projeto. Para vê-las, expanda a pasta de controladores no Gerenciador de soluções.

- HomeController é um controlador de MVC do ASP.NET tradicional. Ele é responsável por fornecer páginas HTML para o site e não está diretamente relacionado à nossa API da web.
- ValuesController é um controlador de API da Web de exemplo.

Vá em frente e exclua ValuesController, clicando duas vezes no arquivo no Gerenciador de soluções e selecionando **excluir.** Agora adicione um novo controlador, da seguinte maneira:

Em **Gerenciador de Soluções**, clique com o botão direito na pasta controllers (controladores).  Selecione **Adicionar** e, em seguida, selecione **Controlador**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

No **Adicionar controlador** assistente, nomeie o controlador &quot;ProductsController&quot;. No **modelo** lista suspensa, selecione **controlador API vazio**. Em seguida, clique em **adicionar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Não é necessário colocar seus controladores em uma pasta chamada controladores. O nome da pasta não é importante; é simplesmente uma maneira conveniente de organizar seus arquivos de origem.


O **Adicionar controlador** assistente criará um arquivo chamado ProductsController.cs na pasta controladores. Se esse arquivo ainda não estiver aberto, clique duas vezes no arquivo para abri-lo. Adicione o seguinte **usando** instrução:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Adicionar um campo que contém um **IProductRepository** instância.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Chamando `new ProductRepository()` no controlador não é o melhor design, porque ele liga o controlador para uma implementação específica de `IProductRepository`. Para uma abordagem melhor, consulte [usando o resolvedor de dependência do Web API](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Introdução a um recurso

A API ProductStore irá expor várias &quot;ler&quot; ações como métodos HTTP GET. Cada ação corresponderá a um método no `ProductsController` classe.

| Ação | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter uma lista de todos os produtos | OBTER | produtos/api / |
| Obter um produto por ID | OBTER | /API/produtos/*id* |
| Obter um produto por categoria | OBTER | /api/products?category=*category* |

Para obter a lista de todos os produtos, adicione este método para o `ProductsController` classe:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

O nome do método começa com &quot;obter&quot;, portanto, por convenção, ele é mapeado para solicitações GET. Além disso, porque o método não tiver parâmetros, ele é mapeado para um URI que não contém um *&quot;id&quot;* segmento no caminho.

Para obter um produto por ID, adicione este método para o `ProductsController` classe:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Esse nome de método também começa com &quot;Obtenha&quot;, mas o método tem um parâmetro denominado *id*. Esse parâmetro é mapeado para o &quot;id&quot; segmento do caminho do URI. A estrutura da API Web ASP.NET converte automaticamente a ID para o tipo de dados correto (**int**) para o parâmetro.

O método GetProduct lança uma exceção do tipo **HttpResponseException** se *id* não é válido. Essa exceção será convertida pela estrutura em um erro 404 (não encontrado).

Por fim, adicione um método para localizar produtos por categoria:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Se o URI da solicitação tem uma cadeia de caracteres de consulta, API da Web tenta corresponder os parâmetros de consulta para parâmetros do método do controlador. Portanto, um URI do formulário "api/produtos? categoria =*categoria*" será mapeado para esse método.

## <a name="creating-a-resource"></a>Criar um recurso

Em seguida, adicionaremos um método para o `ProductsController` classe para criar um novo produto. Aqui está uma implementação simples do método:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Observe duas coisas sobre esse método:

- O nome do método começa com &quot;Post... &quot;. Para criar um novo produto, o cliente envia uma solicitação HTTP POST.
- O método utiliza um parâmetro de tipo de produto. Na API da Web, os parâmetros com tipos complexos são desserializados do corpo da solicitação. Portanto, esperamos que o cliente envie uma representação serializada de um objeto de produto, em formato XML ou JSON.

Essa implementação funcionará, mas não está completa. Idealmente, gostaríamos de resposta HTTP para incluir o seguinte:

- **Código de resposta:** por padrão, a estrutura da API Web define o código de status de resposta a 200 (Okey). Mas de acordo com o protocolo HTTP/1.1, quando uma solicitação POST resulta na criação de um recurso, o servidor deverá responder com status 201 (criado).
- **Local:** quando o servidor cria um recurso, ele deve incluir o URI do novo recurso no cabeçalho Location da resposta.

API Web ASP.NET torna fácil manipular a mensagem de resposta HTTP. Aqui está a implementação aprimorada:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Observe que o tipo de retorno do método agora é **HttpResponseMessage**. Retornando um **HttpResponseMessage** em vez de um produto, podemos controlar os detalhes da mensagem de resposta HTTP, incluindo o código de status e o cabeçalho de localização.

O **CreateResponse** método cria um **HttpResponseMessage** e gravará automaticamente uma representação serializada do objeto Product no corpo da fo a mensagem de resposta.

> [!NOTE]
> Este exemplo não valida o `Product`. Para obter informações sobre a validação de modelo, consulte [validação do modelo na API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Atualizar um recurso

Atualizar um produto com PUT é simples:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

O nome do método começa com &quot;colocar... &quot;, portanto, a API da Web corresponde a ele para solicitações PUT. O método utiliza dois parâmetros, a ID do produto e o produto atualizado. O *identificação* parâmetro é obtido do caminho do URI e o *produto* parâmetro é desserializado do corpo da solicitação. Por padrão, a estrutura da API Web do ASP.NET usa tipos de parâmetro simples da rota e tipos complexos do corpo da solicitação.

## <a name="deleting-a-resource"></a>Excluir um recurso

Para excluir um resourse, defina um método de "Excluir...".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Se uma solicitação de exclusão for bem-sucedida, ela pode retornar o status 200 (Okey) com um corpo de entidade que descreve o status; status 202 (aceito) se a exclusão ainda estiver pendente; ou de status 204 (sem conteúdo) sem o corpo da entidade. Nesse caso, o `DeleteProduct` método tem um `void` tipo de retorno, portanto, a API Web ASP.NET automaticamente converte isso em status 204 (sem conteúdo) de código.
