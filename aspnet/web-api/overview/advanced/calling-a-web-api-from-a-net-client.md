---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chamar uma API Web de um cliente .NET (c#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: be237bee43bc5e32939cb0b3e0948fd8b35bd1eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823473"
---
<a name="call-a-web-api-from-a-net-client-c"></a>Chamar uma API Web de um cliente .NET (c#)
====================
por [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[Baixe o projeto concluído](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Instruções de download](/aspnet/core/tutorials/#how-to-download-a-sample). 

Este tutorial mostra como chamar uma API da web de um aplicativo .NET, usando [HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

Neste tutorial, um aplicativo cliente é escrito que consome a API da web seguinte:

| Ação | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter um produto por ID | OBTER | /API/produtos/*id* |
| Criar um novo produto | POSTAR | produtos/api / |
| Atualizar um produto | PUT | /API/produtos/*id* |
| Excluir um produto | DELETE | /API/produtos/*id* |

Para saber como implementar essa API com a API Web ASP.NET, consulte [criando uma API da Web que dá suporte a operações de CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Para simplificar, o aplicativo de cliente neste tutorial é um aplicativo de console do Windows. **HttpClient** também há suporte para aplicativos do Windows Phone e Windows Store. Para obter mais informações, consulte [escrevendo código de cliente de API da Web para várias plataformas usando bibliotecas portáteis](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Criar o aplicativo de Console

No Visual Studio, crie um novo aplicativo de console do Windows chamado **HttpClientSample** e cole no código a seguir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

O código anterior é o aplicativo de cliente completa.

`RunAsync` execuções e bloqueia até que ela seja concluída. A maioria dos **HttpClient** métodos são assíncronos, porque elas executam e/s de rede. Todas as tarefas de async terminar dentro `RunAsync`. Normalmente um aplicativo não bloqueia o thread principal, mas este aplicativo não permite que qualquer interação.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Instalar as bibliotecas de cliente da API da Web

Use o Gerenciador de pacotes de NuGet para instalar o pacote de bibliotecas de cliente de API da Web.

No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**. No pacote Manager Console (PMC), digite o seguinte comando:

`Install-Package Microsoft.AspNet.WebApi.Client`

O comando anterior adiciona os seguintes pacotes NuGet ao projeto:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET é uma estrutura popular de JSON de alto desempenho para .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Adicionar uma classe de modelo

Examinar o `Product` classe:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Essa classe corresponde ao modelo de dados usado pela API da web. Um aplicativo pode usar **HttpClient** para ler um `Product` instância de uma resposta HTTP. O aplicativo não precisa escrever nenhum código de desserialização.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Criar e inicializar o HttpClient

Examinar estático **HttpClient** propriedade:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** é se destina a ser instanciado uma vez e reutilizado durante a vida útil de um aplicativo. As condições a seguir podem resultar em **SocketException** erros:

* Criando um novo **HttpClient** instância por solicitação.
* Servidor sob carga pesada.

Criando um novo **HttpClient** instância por solicitação pode esgotar os soquetes disponíveis.

O código a seguir inicializa o **HttpClient** instância:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

O código anterior:

* Define o URI de base para solicitações HTTP. Altere o número da porta para a porta usada no aplicativo do servidor. O aplicativo não funcionará a menos que a porta para o aplicativo de servidor é usada.
* Define o cabeçalho Accept, como "application/json". Definir esse cabeçalho informa ao servidor para enviar dados em formato JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Envie uma solicitação GET para recuperar um recurso

O código a seguir envia uma solicitação GET para um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

O **GetAsync** método envia a solicitação HTTP GET. Quando o método for concluído, ele retorna um **HttpResponseMessage** que contém a resposta HTTP. Se o código de status na resposta é um código de êxito, o corpo da resposta contém a representação JSON de um produto. Chame **ReadAsAsync** desserializar o conteúdo JSON para um `Product` instância. O **ReadAsAsync** método é assíncrono, como o corpo da resposta pode ser arbitrariamente grande.

**HttpClient** não lança uma exceção quando a resposta HTTP contém um código de erro. Em vez disso, o **IsSuccessStatusCode** é de propriedade **falso** se o status é um código de erro. Se você preferir tratar os códigos de erro HTTP como exceções, chame [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) no objeto de resposta. `EnsureSuccessStatusCode` gera uma exceção se o código de status fica fora do intervalo de 200&ndash;299. Observe que **HttpClient** pode gerar exceções por outros motivos &mdash; por exemplo, se a solicitação expira.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formatadores de tipo de mídia a ser desserializado

Quando **ReadAsAsync** é chamado sem parâmetros, ele usa um conjunto padrão de *formatadores de mídia* para ler o corpo da resposta. Os formatadores padrão dão suporte a JSON, XML e dados codificados de url do formulário.

Em vez de usar os formatadores de padrão, você pode fornecer uma lista de formatadores para o **ReadAsAsync** método.  Usando uma lista de formatadores é útil se você tiver um formatador personalizado do tipo de mídia:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Para obter mais informações, consulte [formatadores de mídia na API Web ASP.NET 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Enviar uma solicitação POST para criar um recurso

O código a seguir envia uma solicitação POST que contém um `Product` instância no formato JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

O **PostAsJsonAsync** método:

* Serializa um objeto em JSON.
* Envia o conteúdo JSON em uma solicitação POST.

Se a solicitação for bem-sucedida:

* Ele deverá retornar uma resposta de 201 (criado).
* A resposta deve incluir a URL, os recursos criados no cabeçalho Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Enviar uma solicitação PUT para atualizar um recurso

O código a seguir envia uma solicitação PUT para atualizar um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

O **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, exceto que ele envia uma solicitação PUT, em vez de POSTAGEM.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Enviar uma solicitação DELETE para excluir um recurso

O código a seguir envia uma solicitação DELETE para excluir um produto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Como GET, uma solicitação de exclusão não tem um corpo de solicitação. Você não precisa especificar o formato JSON ou XML com a exclusão.

## <a name="test-the-sample"></a>O exemplo de teste

Para testar o aplicativo cliente:

1. [Baixar](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) e executar o aplicativo de servidor. [Instruções de download](/aspnet/core/tutorials/#how-to-download-a-sample). Verifique se que o aplicativo de servidor está funcionando. Para exaxmple, `http://localhost:64195/api/products` deve retornar uma lista de produtos.
2. Defina o URI de base para solicitações HTTP. Altere o número da porta para a porta usada no aplicativo do servidor.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Execute o aplicativo cliente. A saída a seguir será produzida:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
