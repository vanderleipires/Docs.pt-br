---
title: "Criando serviços de back-end para aplicativos móveis nativos"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3b6a32f2-5af9-4ede-9b7f-17ab300526d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: be1cd9f4fe41f1a79669975cb6a89439cdd9e5c7
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>Criando serviços de back-end para aplicativos móveis nativos

Por [Steve Smith](https://ardalis.com/)

Aplicativos móveis podem facilmente se comunicar com serviços de back-end do ASP.NET Core.

[Exibir ou baixar o código de exemplo dos serviços de back-end](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Exemplo do aplicativo móvel nativo

Este tutorial demonstra como criar serviços de back-end usando o ASP.NET Core MVC para dar suporte a aplicativos móveis nativo. Ele usa o [aplicativo Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) como seu cliente nativo, que inclui clientes nativos separados para dispositivos Android, iOS, Universal do Windows e Windows Phone. Você pode seguir o tutorial vinculado para criar o aplicativo nativo (e instalar as ferramentas Xamarin gratuitas necessárias), bem como baixar a solução de exemplo Xamarin. O exemplo do Xamarin inclui um projeto de serviços ASP.NET Web API 2, que substitui o aplicativo do ASP.NET Core deste artigo (com nenhuma alteração exigida pelo cliente).

![Aplicativo ToDoRest em execução em um smartphone Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Recursos

O aplicativo ToDoRest pode listar, adicionar, excluir e atualizar itens de tarefas. Cada item possui uma ID, um nome, algumas anotações e uma propriedade que indica se ele ainda está em andamento.

O modo de exibição principal dos itens, como mostrado acima, lista o nome de cada item e indica se está concluído com uma marca de seleção.

Tocar o `+` ícone abre uma caixa de diálogo Adicionar item:

![Item de caixa de diálogo Adicionar](native-mobile-backend/_static/todo-android-new-item.png)

Ao tocar em um item na lista da tela principal, uma caixa de diálogo é aberta para que você possa editar o nome, as observações e as configurações do item, ou excluir o item:

![Editar caixa de diálogo de item](native-mobile-backend/_static/todo-android-edit-item.png)

Este exemplo é configurado por padrão para usar serviços de back-end hospedados em developer.xamarin.com, que permitem operações somente leitura. Para testá-lo por conta própria em relação o aplicativo do ASP.NET Core criado na próxima seção em execução no seu computador, você precisará atualizar o aplicativo `RestUrl` constante. Navegue até o `ToDoREST` do projeto e abra o *Constants.cs* arquivo. Substitua o `RestUrl` com uma URL que inclui o IP do seu computador, endereço (não localhost ou 127.0.0.1, desde que esse endereço é usado do emulador de dispositivo, não em seu computador). Inclua o número da porta (5000). Para testar se os serviços de trabalhar com um dispositivo, verifique se que você não tiver um firewall ativas bloqueando o acesso a essa porta.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Criando o projeto do ASP.NET Core

Crie um novo aplicativo de Web do ASP.NET Core no Visual Studio. Escolha o modelo de API da Web e sem autenticação. Nomeie o projeto *ToDoApi*.

![Caixa de diálogo nova aplicativo Web ASP.NET com modelo de projeto de API da Web selecionado](native-mobile-backend/_static/web-api-template.png)

O aplicativo deve responder a todas as solicitações feitas para a porta 5000. Atualização *Program.cs* para incluir `.UseUrls("http://*:5000")` para fazer isso:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Certifique-se de que executar o aplicativo diretamente, em vez de por trás do IIS Express, que ignora solicitações não local por padrão. Executar `dotnet run` em um prompt de comando, ou escolha o perfil de nome do aplicativo no menu suspenso de destino de depuração na barra de ferramentas do Visual Studio.

Adicione uma classe de modelo para representar itens pendentes. Marca necessários campos usando a `[Required]` atributo:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

Os métodos da API exigem alguma maneira de trabalhar com dados. Use a mesma `IToDoRepository` interface os usos de exemplo originais do Xamarin:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

Para este exemplo, a implementação apenas usa um conjunto particular de itens:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Configurar a implementação em *Startup.cs*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

Neste ponto, você está pronto para criar o *ToDoItemsController*.

> [!TIP]
> Saiba mais sobre como criar web APIs em [criando sua primeira API da Web com ASP.NET MVC de núcleos e do Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Criando o controlador

Adicionar um novo controlador para o projeto, *ToDoItemsController*. Ele deve herdar de Microsoft.AspNetCore.Mvc.Controller. Adicionar um `Route` atributo para indicar que o controlador de manipular as solicitações feitas para caminhos que começam com `api/todoitems`. O `[controller]` token na rota é substituído pelo nome do controlador (omitindo o `Controller` sufixo) e é especialmente útil para rotas global. Saiba mais sobre [roteamento](../fundamentals/routing.md).

O controlador requer um `IToDoRepository` a função; solicitar uma instância desse tipo por meio do construtor do controlador. Em tempo de execução, esta instância será fornecida com suporte do framework [injeção de dependência](../fundamentals/dependency-injection.md).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Essa API dá suporte a quatro verbos HTTP diferentes para executar operações CRUD (criação, leitura, atualização, exclusão) na fonte de dados. A mais simples delas é a operação de leitura, que corresponde a uma solicitação HTTP GET.

### <a name="reading-items"></a>Lendo itens

Solicitar uma lista de itens é feito com uma solicitação GET para a `List` método. O `[HttpGet]` atributo no `List` método indica que esta ação só deve tratar as solicitações GET. A rota para esta ação é a rota especificada no controlador. Você não precisa necessariamente usar o nome da ação como parte da rota. Você precisa garantir que cada ação tem uma rota exclusiva e não ambígua. Atributos de roteamento podem ser aplicados nos níveis de método para criar rotas específico e controlador.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

O `List` método retorna um código de resposta Okey 200 e todos os itens de tarefas, serializados como JSON.

Você pode testar o novo método de API usando uma variedade de ferramentas, como [carteiro](https://www.getpostman.com/docs/), mostrado aqui:

![Console carteiro mostrando uma solicitação GET para todoitems e o corpo da resposta mostrando o JSON para três itens retornados](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Criando itens

Por convenção, criando novos itens de dados é mapeado para o verbo HTTP POST. O `Create` método tem um `[HttpPost]` atributo aplicado a ele e aceita um `ToDoItem` instância. Como o `item` argumento será passado no corpo da POSTAGEM, este parâmetro é decorado com o `[FromBody]` atributo.

Dentro do método, o item é verificado para validade e existência anterior no repositório de dados e se nenhum problema ocorrer, ele é adicionado usando o repositório. Verificando `ModelState.IsValid` executa [validação do modelo](../mvc/models/validation.md)e deve ser feito em todos os métodos de API que aceita a entrada do usuário.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

O exemplo usa uma enumeração que contém códigos de erro que são passados para o cliente móvel:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Teste a adição de novos itens usando carteiro escolhendo o verbo POST fornecendo o novo objeto no formato JSON no corpo da solicitação. Você também deve adicionar um cabeçalho de solicitação que especifica um `Content-Type` de `application/json`.

![Console carteiro mostrando um POST e resposta](native-mobile-backend/_static/postman-post.png)

O método retorna o item recém-criado na resposta.

### <a name="updating-items"></a>Atualizando itens

Modificando registros é feito usando solicitações HTTP PUT. Além desta mudança, o `Edit` método é quase idêntico ao `Create`. Observe que, se o registro não for encontrado, o `Edit` ação irá retornar um `NotFound` resposta (404).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Para testar com carteiro, altere o verbo para PUT. Especifique os dados do objeto atualizado no corpo da solicitação.

![Console carteiro mostrando um PUT e resposta](native-mobile-backend/_static/postman-put.png)

Este método retorna um `NoContent` resposta (204) quando obtiver êxito, para manter a consistência com a API já existente.

### <a name="deleting-items"></a>A exclusão de itens

Excluindo registros é feito, fazendo solicitações de exclusão para o serviço e passar a ID do item a ser excluído. Como com as atualizações, solicitações de itens que não existem receberão `NotFound` respostas. Caso contrário, receberá uma solicitação bem-sucedida um `NoContent` resposta (204).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Observe que ao testar a funcionalidade de exclusão, nada é necessário no corpo da solicitação.

![Console carteiro mostrando uma exclusão e resposta](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Convenções de API da Web comuns

À medida que desenvolve os serviços de back-end para seu aplicativo, você desejará criar um conjunto de convenções ou políticas para a manipulação resolvem preocupações consistente. Por exemplo, no serviço mostrado acima, as solicitações de registros específicos que não foram encontrados recebidos um `NotFound` resposta, em vez de `BadRequest` resposta. Da mesma forma, os comandos feitos para este serviço passados em tipos de modelo associado sempre verificados `ModelState.IsValid` e retornado um `BadRequest` para tipos de modelo inválido.

Depois de identificar uma diretiva comum para suas APIs, você geralmente pode encapsulá-lo em uma [filtro](../mvc/controllers/filters.md). Saiba mais sobre [como encapsular políticas comuns da API em aplicativos ASP.NET MVC de núcleo](https://msdn.microsoft.com/magazine/mt767699.aspx).
