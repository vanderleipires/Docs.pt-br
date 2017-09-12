---
title: "Componentes do modo de exibição"
author: rick-anderson
description: "Componentes do modo de exibição destinam-se em qualquer lugar que a lógica de processamento reutilizáveis."
keywords: "ASP.NET Core, componentes do modo de exibição, exibição parcial"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 07a2aca731b8017450a1b0da00ddef25306c122e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="view-components"></a>Componentes do modo de exibição

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)

## <a name="introducing-view-components"></a>Introdução ao exibir componentes

Novo para ASP.NET MVC de núcleo, exiba os componentes são semelhantes às exibições parciais, mas eles são muito mais eficientes. Componentes do modo de exibição não usar a associação de modelo e apenas dependem dos dados que você fornecer ao chamar nela. Um componente do modo de exibição:

* Renderiza um bloco em vez de uma resposta inteira
* Inclui as mesmas separação de preocupações e os benefícios de capacidade de teste encontrados entre um controlador e o modo de exibição
* Pode ter parâmetros e lógica de negócios
* É geralmente chamado de uma página de layout

Componentes de exibição destinam-se em qualquer lugar que a lógica de processamento reutilizável que é muito complexa para uma exibição parcial, como:

* Menus de navegação dinâmica
* Nuvem de marcas (onde ele consulta o banco de dados)
* Painel de logon
* Carrinho de compras
* Recentemente artigos publicados
* Conteúdo da barra lateral em um blog típico
* Um painel de logon que deve ser renderizado em cada página e mostra links para fazer logoff ou de log, dependendo do estado do usuário de login

Um componente do modo de exibição consiste em duas partes: a classe (normalmente é derivado de [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) e o resultado retorna (normalmente um modo de exibição). Controladores, um componente do modo de exibição pode ser um POCO, mas a maioria dos desenvolvedores deseja aproveitar os métodos e propriedades disponíveis derivando de `ViewComponent`.

## <a name="creating-a-view-component"></a>Criando um componente de exibição

Esta seção contém os requisitos de alto nível para criar um componente de visualização. Posteriormente neste artigo, vamos examinar cada etapa e criar um componente de visualização.

### <a name="the-view-component-class"></a>A classe de componente do modo de exibição

Uma classe de componente do modo de exibição pode ser criada por qualquer um dos seguintes:

* Derivando de *ViewComponent*
* Decoração de uma classe com o `[ViewComponent]` atributo ou derivado de uma classe com o `[ViewComponent]` atributo
* Criando uma classe em que o nome termina com o sufixo *ViewComponent*

Como controladores, componentes de exibição devem ser públicos não aninhados, classes e não-abstrato. O nome do componente de exibição é o nome da classe com o sufixo "ViewComponent" removido. Ele também pode ser explicitamente especificado usando o `ViewComponentAttribute.Name` propriedade.

Uma classe de componente do modo de exibição:

* Dá suporte total ao construtor [injeção de dependência](../../fundamentals/dependency-injection.md)

* Não faz parte do ciclo de vida de controlador, o que significa que você não pode usar [filtros](../controllers/filters.md) em um componente do modo de exibição

### <a name="view-component-methods"></a>Métodos de componente do modo de exibição

Um componente de visualização define a lógica em uma `InvokeAsync` método que retorna um `IViewComponentResult`. Parâmetros vir diretamente da invocação do componente de exibição, não da associação de modelo. Um componente de visualização nunca diretamente manipula uma solicitação. Normalmente, um componente de visualização inicializa um modelo e passá-lo para um modo de exibição, chamando o `View` método. Em resumo, exiba os métodos de componente:

* Definir um `InvokeAsync` método que retorna um`IViewComponentResult`
* Inicializa um modelo normalmente e passá-lo para um modo de exibição, chamando o `ViewComponent` `View` método
* Parâmetros são provenientes do método de chamada, não HTTP, não há nenhuma associação de modelo
* São não pode ser acessado diretamente como um ponto de extremidade HTTP, eles são chamados de seu código (normalmente em um modo de exibição). Um componente de visualização nunca manipula uma solicitação
* Estão sobrecarregados a assinatura em vez de todos os detalhes da solicitação HTTP atual

### <a name="view-search-path"></a>Caminho de pesquisa de modo de exibição

Pesquisa o tempo de execução para o modo de exibição nos seguintes caminhos:

   * Modos de exibição /\<controller_name > /Components/\<view_component_name > /\<view_name >
   * Modos de exibição/Shared/componentes/\<view_component_name > /\<view_name >

O nome de exibição padrão para um componente de visualização é *padrão*, que significa que o arquivo de exibição geralmente será nomeado *cshtml*. Você pode especificar um nome de exibição diferente ao criar o resultado de componente do modo de exibição ou ao chamar o `View` método.

Recomendamos que você nomear o arquivo de exibição *cshtml* e usar o *compartilhado/exibições/componentes/\<view_component_name > /\<view_name >* caminho. O `PriorityList` usa o componente de exibição usado neste exemplo *Views/Shared/Components/PriorityList/Default.cshtml* para o modo de exibição de componente de exibição.

## <a name="invoking-a-view-component"></a>Invocar um componente de visualização

Para usar o componente de exibição, chame o seguinte em um modo de exibição:

```html
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Os parâmetros serão passados para o `InvokeAsync` método. O `PriorityList` exibição desenvolvidos no artigo é chamado da *Views/Todo/Index.cshtml* o arquivo de exibição. A seguir, o `InvokeAsync` método for chamado com dois parâmetros:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Invocar um componente do modo de exibição como um auxiliar de marca

Para o ASP.NET Core 1.1 e superior, você pode invocar um componente do modo de exibição como uma [auxiliar de marca](xref:mvc/views/tag-helpers/intro):

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Parâmetros de classe e método de maiusculas e minúsculas de Pascal para os auxiliares de marca são convertidos em seus [inferior caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). O auxiliar de marca para invocar um componente de exibição usa o `<vc></vc>` elemento. O componente de exibição é especificado da seguinte maneira:

```html
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Observação: Para usar um componente do modo de exibição como um auxiliar de marca, você deve registrar o assembly que contém o componente de exibição usando o `@addTagHelper` diretiva. Por exemplo, se o seu componente de exibição está em um assembly chamado "MyWebApp", adicione a seguinte diretiva para o `_ViewImports.cshtml` arquivo:

```csharp
@addTagHelper *, MyWebApp
```

Você pode registrar um componente de visualização como um auxiliar de marca para qualquer arquivo que referencia o componente de exibição. Consulte [gerenciamento de escopo do auxiliar de marca](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para obter mais informações sobre como registrar os auxiliares de marcação.

O `InvokeAsync` método usado neste tutorial:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Na marcação do auxiliar de marca:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

No exemplo acima, o `PriorityList` torna-se o componente de exibição `priority-list`. Os parâmetros para o componente de exibição são passados como atributos em letras minúsculas kebab.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Invocar um componente de visualização diretamente de um controlador

Componentes do modo de exibição normalmente são invocados em uma exibição, mas você pode invocá-los diretamente a partir de um método do controlador. Enquanto os componentes do modo de exibição não definir pontos de extremidade como controladores, você pode implementar facilmente uma ação do controlador que retorna o conteúdo de um `ViewComponentResult`.

Neste exemplo, o componente de exibição é chamado diretamente do controlador:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Passo a passo: Criando um componente de exibição simples

[Baixar](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compilar e testar o código inicial. É um projeto simple com um `Todo` controlador que exibe uma lista de *Todo* itens.

![Lista de ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Adicionar uma classe ViewComponent

Criar um *ViewComponents* pasta e adicione o seguinte `PriorityListViewComponent` classe:

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Observações sobre o código:

* Classes de componente do modo de exibição podem ser contidos em **qualquer** pasta do projeto.
* Porque o nome da classe PriorityList**ViewComponent** termina com o sufixo **ViewComponent**, o tempo de execução usará a cadeia de caracteres "PriorityList" ao referenciar o componente de classe de um modo de exibição. Explicarei que em mais detalhes posteriormente.
* O `[ViewComponent]` atributo pode alterar o nome usado para fazer referência a um componente de visualização. Por exemplo, pode ter dado a classe `XYZ` e aplicadas a `ViewComponent` atributo:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* O `[ViewComponent]` atributo acima informa o seletor de componente do modo de exibição para usar o nome `PriorityList` ao procurar os modos de exibição associados com o componente e como usar a cadeia de caracteres "PriorityList" ao referenciar o componente de classe de um modo de exibição. Explicarei que em mais detalhes posteriormente.
* O componente usa [injeção de dependência](../../fundamentals/dependency-injection.md) para disponibilizar o contexto de dados.
* `InvokeAsync`expõe um método que pode ser chamado de um modo de exibição e pode levar um número arbitrário de argumentos.
* O `InvokeAsync` método retorna o conjunto de `ToDo` itens que atendem a `isDone` e `maxPriority` parâmetros.

### <a name="create-the-view-component-razor-view"></a>Criar o modo de exibição do Razor de componente do modo de exibição

* Criar o *compartilhado/exibições/componentes* pasta. Essa pasta **deve** nomeado *componentes*.

* Criar o *exibições/compartilhado/componentes/PriorityList* pasta. Esse nome de pasta deve corresponder o nome da classe de componente do modo de exibição ou o nome da classe menos o sufixo (se seguido convenção e usado o *ViewComponent* sufixo no nome de classe). Se você usou o `ViewComponent` atributo, o nome da classe precisa corresponder a designação de atributo.

* Criar um *Views/Shared/Components/PriorityList/Default.cshtml* exibição Razor: [!code-html [principal](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   O modo de exibição Razor usa uma lista de `TodoItem` e os exibe. Se o componente de exibição `InvokeAsync` método não passa o nome do modo de exibição (como em nosso exemplo), *padrão* é usado para o nome de exibição por convenção. Posteriormente no tutorial, mostrarei como passar o nome da exibição. Para substituir o estilo padrão para um controlador específico, adicionar um modo de exibição para a pasta de exibição do controlador específico (por exemplo *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Se o componente de exibição é específico de controlador, você pode adicioná-lo para a pasta do controlador específico (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Adicionar um `div` que contém uma chamada para o componente de lista de prioridade para a parte inferior da *Views/Todo/index.cshtml* arquivo:

    [!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

A marcação `@await Component.InvokeAsync` mostra a sintaxe para chamar componentes do modo de exibição. O primeiro argumento é o nome do componente que você deseja invocar ou chamar. Os parâmetros subsequentes são passados para o componente. `InvokeAsync`pode levar a um número arbitrário de argumentos.

Teste o aplicativo. A imagem a seguir mostra a lista de tarefas e os itens de prioridade:

![itens de lista e prioridade de tarefas](view-components/_static/pi.png)

Você também pode chamar o componente Exibir diretamente do controlador:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![itens de prioridade da ação IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Especificando um nome de exibição

Um componente de visualização complexo talvez seja necessário especificar um modo de exibição não padrão em algumas condições. O código a seguir mostra como especificar o modo de exibição de "PVC" o `InvokeAsync` método. Atualização de `InvokeAsync` método o `PriorityListViewComponent` classe.

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Copie o *Views/Shared/Components/PriorityList/Default.cshtml* arquivo para uma exibição nomeada *Views/Shared/Components/PriorityList/PVC.cshtml*. Adicione um título para indicar que o modo de exibição de PVC está sendo usado.

[!code-html[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Atualização *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Executar o aplicativo e verifique se o modo de exibição de PVC.

![Componente de visualização de prioridade](view-components/_static/pvc.png)

Se o modo de exibição de PVC não é processado, verifique se que você estiver chamando o componente de exibição com uma prioridade de 4 ou superior.

### <a name="examine-the-view-path"></a>Examine o caminho de exibição

* Altere o parâmetro de prioridade para três ou menos para que o modo de exibição de prioridade não é retornado.
* Renomeie temporariamente o *Views/Todo/Components/PriorityList/Default.cshtml* para *1Default.cshtml*.
* Teste o aplicativo, você obterá o seguinte erro:

   <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Cópia *Views/Todo/Components/PriorityList/1Default.cshtml* para *Views/Shared/Components/PriorityList/Default.cshtml*.
* Adicionar algumas marcações para o *compartilhado* exibição de componente de exibição de tarefas para indicar o modo de exibição é do *compartilhado* pasta.
* Teste o **compartilhado** exibição do componente.

![Saída de tarefas com a exibição do componente compartilhado](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Evitando magic cadeias de caracteres

Se você quiser que a segurança de tempo de compilação, você pode substituir o nome do componente de exibição embutida com o nome da classe. Crie o componente de exibição sem o sufixo "ViewComponent":

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Adicionar um `using` instrução para o Razor Exibir arquivo e use o `nameof` operador:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>Recursos adicionais

* [Injeção de dependência em exibições](dependency-injection.md)
