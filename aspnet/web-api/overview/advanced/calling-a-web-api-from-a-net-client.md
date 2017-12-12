---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chamar uma API da Web de um cliente .NET (c#) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8fcc5e7c6bc39f961931589128a7a5863482aa4e
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2017
---
<a name="call-a-web-api-from-a-net-client-c"></a>Chamar uma API da Web de um cliente .NET (c#)
====================
por [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[Baixe o projeto concluído](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

Este tutorial mostra como chamar uma API da web de um aplicativo .NET, usando [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)

Neste tutorial, um aplicativo cliente é gravado que consome a API da web seguinte:

| Ação | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter um produto por ID | OBTER | /API/produtos/*id* |
| Criar um novo produto | POSTAR | produtos/api / |
| Atualização de um produto | PUT | /API/produtos/*id* |
| Excluir um produto | DELETE | /API/produtos/*id* |

Para saber como implementar essa API com a API da Web ASP.NET, consulte [criando uma API da Web que dá suporte a operações de CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Para simplificar, o aplicativo de cliente neste tutorial é um aplicativo de console do Windows. **HttpClient** também tem suporte para aplicativos do Windows Phone e da Windows Store. Para obter mais informações, consulte [escrevendo código de cliente de API da Web para várias plataformas usando bibliotecas portáteis](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Criar o aplicativo de Console

No Visual Studio, crie um novo aplicativo de console do Windows chamado **HttpClientSample** e cole o código a seguir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

O código anterior é o aplicativo cliente completa.

`RunAsync`é executado e bloqueia até que ela seja concluída. A maioria dos **HttpClient** métodos são assíncronas, porque eles realizam e/s de rede. Todas as tarefas assíncronas são feitas em `RunAsync`. Normalmente um aplicativo não bloqueia o thread principal, mas este aplicativo não permite que qualquer interação.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Instalar as bibliotecas de cliente de API da Web

Use o NuGet Package Manager para instalar o pacote de bibliotecas de cliente de API da Web.

No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**. No pacote Manager Console (PMC), digite o seguinte comando:

`Install-Package Microsoft.AspNet.WebApi.Client`

O comando anterior adiciona os seguintes pacotes do NuGet ao projeto:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft. JSON

Json.NET é uma estrutura JSON de alto desempenho popular para .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Examine o `Product` classe:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Essa classe coincide com o modelo de dados usado pela API da web. Um aplicativo pode usar **HttpClient** para ler um `Product` instância de uma resposta HTTP. O aplicativo não precisa escrever nenhum código de desserialização.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Criar e inicializar HttpClient

Examine estático **HttpClient** propriedade:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** é destinado a ser instanciado uma vez e reutilizados durante a vida útil de um aplicativo. As condições a seguir podem resultar em **SocketException** erros:

* Criando um novo **HttpClient** instância por solicitação.
* Servidor sob carga pesada.

Criando um novo **HttpClient** instância por solicitação pode esgotar os soquetes disponíveis.

O código a seguir inicializa o **HttpClient** instância:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

O código anterior:

* Define o URI de base para solicitações HTTP. Altere o número da porta para a porta usada para o aplicativo de servidor. O aplicativo não funcionará a menos que a porta para o aplicativo de servidor é usada.
* Define o cabeçalho Accept, como "application/json". Definir esse cabeçalho informa ao servidor para enviar dados em formato JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Enviar uma solicitação GET para recuperar um recurso

O código a seguir envia uma solicitação GET para um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

O **GetAsync** método envia a solicitação HTTP GET. Quando o método é concluído, ele retorna um **HttpResponseMessage** que contém a resposta HTTP. Se o código de status na resposta é um código de êxito, o corpo da resposta contém a representação JSON de um produto. Chamar **ReadAsAsync** para desserializar a carga de JSON para um `Product` instância. O **ReadAsAsync** método é assíncrono, como o corpo da resposta pode ser arbitrariamente grande.

**HttpClient** não lança uma exceção quando a resposta HTTP contém um código de erro. Em vez disso, o **IsSuccessStatusCode** é de propriedade **false** se o status é um código de erro. Se você preferir tratar os códigos de erro HTTP como exceções, chame [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) no objeto de resposta. `EnsureSuccessStatusCode`gera uma exceção se o código de status está fora do intervalo de 200&ndash;299. Observe que **HttpClient** pode lançar exceções para outros motivos &mdash; por exemplo, se a solicitação expire.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formatadores de tipo de mídia a ser desserializado

Quando **ReadAsAsync** é chamado sem parâmetros, ele usa um conjunto padrão de *formatadores de mídia* para ler o corpo da resposta. Os formatadores padrão oferecem suporte a JSON, XML e dados codificados de url do formulário.

Em vez de usar os formatadores padrão, você pode fornecer uma lista dos formatadores para o **ReadAsAsync** método.  Usando um uma lista dos formatadores é útil se você tiver um formatador personalizado do tipo de mídia:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Para obter mais informações, consulte [formatadores de mídia no ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Enviando uma solicitação POST para criar um recurso

O código a seguir envia uma solicitação POST que contém um `Product` instância no formato JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

O **PostAsJsonAsync** método:

* Serializa um objeto em JSON.
* Envia a carga de JSON em uma solicitação POST.

Se a solicitação tiver êxito:

* Ele deverá retornar uma resposta de 201 (criado).
* A resposta deve incluir a URL dos recursos criados no cabeçalho de localização.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Enviar uma solicitação PUT para atualizar um recurso

O código a seguir envia uma solicitação PUT para atualizar um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

O **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, exceto que ele envia uma solicitação PUT, em vez de POSTAGEM.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Enviar uma solicitação de exclusão para excluir um recurso

O código a seguir envia uma solicitação de exclusão para excluir um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Como obter, uma solicitação de exclusão não tem um corpo de solicitação. Você não precisa especificar o formato JSON ou XML com a exclusão.

## <a name="test-the-sample"></a>Testar o exemplo

Para testar o aplicativo cliente:

1. [Baixar](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) e executar o aplicativo de servidor. [Instruções de download](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample). Verifique se que o aplicativo de servidor está funcionando. Para exaxmple, `http://localhost:64195/api/products` deve retornar uma lista de produtos.
2. Defina o URI de base para solicitações HTTP. Altere o número da porta para a porta usada para o aplicativo de servidor.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Execute o aplicativo cliente. A saída a seguir será produzida:

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
