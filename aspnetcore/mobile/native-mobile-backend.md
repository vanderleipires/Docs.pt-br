---
title: "Criando serviços de back-end para aplicativos móveis nativos"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mobile/native-mobile-backend
ms.openlocfilehash: ff09f331cff5cca7b42fa89bff55c0ed5c7d82f4
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/31/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a>Criando serviços de back-end para aplicativos móveis nativos

Por [Steve Smith](https://ardalis.com/)

Os aplicativos móveis podem se comunicar com facilidade com os serviços de back-end do ASP.NET Core.

[Exibir ou baixar o código de exemplo de serviços de back-end](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Exemplo do aplicativo móvel nativo 

Este tutorial demonstra como criar serviços de back-end usando o ASP.NET Core MVC para dar suporte a aplicativos móveis nativos. Ele usa o [aplicativo Xamarin Forms ToDoRest](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) como seu cliente nativo, que inclui clientes nativos separados para dispositivos Android, iOS, Universal do Windows e Windows Phone. Siga o tutorial com links para criar o aplicativo nativo (e instale as ferramentas do Xamarin gratuitas necessárias), além de baixar a solução de exemplo do Xamarin. A amostra do Xamarin inclui um projeto de serviços da API Web ASP.NET 2, que substitui o aplicativo ASP.NET Core deste artigo (sem nenhuma alteração exigida pelo cliente).

![Aplicativo To Do Rest em execução em um smartphone Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Recursos

O aplicativo ToDoRest é compatível com listagem, adição, exclusão e atualização de itens de tarefas pendentes. Cada item tem uma ID, um Nome, Observações e uma propriedade que indica se ele já foi Concluído.

A exibição principal dos itens, conforme mostrado acima, lista o nome de cada item e indica se ele foi concluído com uma marca de seleção.

Tocar no ícone `+` abre uma caixa de diálogo de adição de itens:

![Caixa de diálogo de adição de itens](native-mobile-backend/_static/todo-android-new-item.png)

Tocar em um item na tela da lista principal abre uma caixa de diálogo de edição, na qual o Nome do item, Observações e configurações de Concluído podem ser modificados, ou o item pode ser excluído:

![Caixa de diálogo de edição de itens](native-mobile-backend/_static/todo-android-edit-item.png)

Esta amostra é configurada por padrão para usar os serviços de back-end hospedados em developer.xamarin.com, que permitem operações somente leitura. Para testá-la por conta própria no aplicativo ASP.NET Core criado na próxima seção em execução no computador, você precisará atualizar a constante `RestUrl` do aplicativo. Navegue para o projeto `ToDoREST` e abra o arquivo *Constants.cs*. Substitua o `RestUrl` por uma URL que inclui o endereço IP do computador (não localhost ou 127.0.0.1, pois esse endereço é usado no emulador do dispositivo, não no computador). Inclua o número da porta também (5000). Para testar se os serviços funcionam com um dispositivo, verifique se você não tem um firewall ativo bloqueando o acesso a essa porta.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Criando o projeto ASP.NET Core

Crie um novo aplicativo Web ASP.NET Core no Visual Studio. Escolha o modelo de API Web e a opção Sem Autenticação. Nomeie o projeto como *ToDoApi*.

![Caixa de diálogo Novo Aplicativo Web ASP.NET com modelo de projeto de API Web selecionado](native-mobile-backend/_static/web-api-template.png)

O aplicativo deve responder a todas as solicitações feitas para a porta 5000. Atualize *Program.cs* para que ele inclua `.UseUrls("http://*:5000")` para fazer isso:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Verifique se você está executando o aplicativo diretamente, em vez de com a proteção do IIS Express, que ignora as solicitações não locais por padrão. Execute `dotnet run` em um prompt de comando ou escolha o perfil de nome do aplicativo na lista suspensa Destino de Depuração na barra de ferramentas do Visual Studio.

Adicione uma classe de modelo para representar itens de tarefas pendentes. Marque os campos obrigatórios usando o atributo `[Required]`:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

Os métodos de API exigem alguma maneira para trabalhar com os dados. Use a mesma interface `IToDoRepository` usada pela amostra original do Xamarin:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

Para esta amostra, a implementação apenas usa uma coleção particular de itens:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Configure a implementação em *Startup.cs*:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

Neste ponto, você está pronto para criar o *ToDoItemsController*.

> [!TIP]
> Saiba mais sobre como criar APIs Web em [Criando sua primeira API Web com o ASP.NET Core MVC e o Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Criando o controlador

Adicione um novo controlador ao projeto, *ToDoItemsController*. Ele deve herdar de Microsoft.AspNetCore.Mvc.Controller. Adicione um atributo `Route` para indicar que o controlador manipulará as solicitações feitas para caminhos que começam com `api/todoitems`. O token `[controller]` na rota é substituído pelo nome do controlador (com a omissão do sufixo `Controller`) e é especialmente útil para rotas globais. Saiba mais sobre o [roteamento](../fundamentals/routing.md).

O controlador exige um `IToDoRepository` para funcionar; solicite uma instância desse tipo por meio do construtor do controlador. Em tempo de execução, essa instância receberá o suporte da estrutura para [injeção de dependência](../fundamentals/dependency-injection.md).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Essa API é compatível com quatro verbos HTTP diferentes para executar operações CRUD (Criar, Ler, Atualizar, Excluir) na fonte de dados. A mais simples delas é a operação Read, que corresponde a uma solicitação HTTP GET.

### <a name="reading-items"></a>Lendo itens

A solicitação de uma lista de itens é feita com uma solicitação GET para o método `List`. O atributo `[HttpGet]` no método `List` indica que essa ação apenas deve manipular solicitações GET. A rota para essa ação é a rota especificada no controlador. Você não precisa necessariamente usar o nome da ação como parte da rota. Basta garantir que cada ação tenha uma rota exclusiva e não ambígua. Os atributos de roteamento podem ser aplicados nos níveis do método e do controlador para criar rotas específicas.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

O método `List` retorna um código de resposta 200 OK e todos os itens de tarefas pendentes serializados como JSON.

Você pode testar o novo método de API usando uma variedade de ferramentas, como [Postman](https://www.getpostman.com/docs/), exibidas aqui:

![Console do Postman mostrando uma solicitação GET para itens de tarefas pendentes e o corpo da resposta mostrando o JSON para três itens retornados](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Criando itens

Por convenção, a criação de novos itens de dados é mapeada para o verbo HTTP POST. O método `Create` tem um atributo `[HttpPost]` aplicado a ele e aceita uma instância `ToDoItem`. Como o argumento `item` será passado no corpo do POST, esse parâmetro é decorado com o atributo `[FromBody]`.

Dentro do método, o item é verificado quanto à validade e existência anterior no armazenamento de dados e, se nenhum problema ocorrer, ele será adicionado usando o repositório. A verificação de `ModelState.IsValid` executa a [validação do modelo](../mvc/models/validation.md) e deve ser feita em todos os métodos de API que aceitam a entrada do usuário.

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

A amostra usa uma enumeração que contém códigos de erro que são passados para o cliente móvel:

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Teste a adição de novos itens usando Postman. Escolha o verbo POST, fornecendo o novo objeto no formato JSON no corpo da solicitação. Você também deve adicionar um cabeçalho de solicitação que especifica um `Content-Type` de `application/json`.

![Console Postman mostrando um POST e resposta](native-mobile-backend/_static/postman-post.png)

O método retorna o item recém-criado na resposta.

### <a name="updating-items"></a>Atualizando itens

A modificação de registros é feita com solicitações HTTP PUT. Além dessa alteração, o método `Edit` é quase idêntico ao `Create`. Observe que, se o registro não for encontrado, a ação `Edit` retornará uma resposta `NotFound` (404).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Para testar com o Postman, altere o verbo para PUT. Especifique os dados do objeto atualizado no Corpo da solicitação.

![Console do Postman mostrando um PUT e uma resposta](native-mobile-backend/_static/postman-put.png)

Esse método retorna uma resposta `NoContent` (204) quando obtém êxito, para consistência com a API pré-existente.

### <a name="deleting-items"></a>Excluindo itens

A exclusão de registros é feita com solicitações DELETE para o serviço e a passagem da ID do item a ser excluído. Assim como ocorre com as atualizações, as solicitações de itens que não existem receberão respostas `NotFound`. Caso contrário, uma solicitação bem-sucedida receberá uma resposta `NoContent` (204).

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Observe que, ao testar a funcionalidade de exclusão, nada é necessário no Corpo da solicitação.

![Console do Postman mostrando um DELETE e uma resposta](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Convenções comuns de API Web

À medida que você desenvolve serviços de back-end para seu aplicativo, desejará criar um conjunto consistente de convenções ou políticas para lidar com preocupações paralelas. Por exemplo, no serviço mostrado acima, as solicitações de registros específicos que não foram encontrados receberam uma resposta `NotFound`, em vez de uma resposta `BadRequest`. Da mesma forma, os comandos feitos para esse serviço que passaram tipos associados a um modelo sempre verificaram `ModelState.IsValid` e retornaram um `BadRequest` para tipos de modelo inválidos.

Depois de identificar uma política comum para suas APIs, você geralmente pode encapsulá-la em um [filtro](../mvc/controllers/filters.md). Saiba mais sobre [como encapsular políticas comuns da API em aplicativos ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).
