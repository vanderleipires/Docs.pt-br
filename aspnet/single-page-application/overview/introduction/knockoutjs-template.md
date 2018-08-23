---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Aplicativo de página única: Modelo KnockoutJS | Microsoft Docs'
author: MikeWasson
description: Modelo Knockout
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 328046363666944f121dedc1883bbe83f5b079d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830623"
---
<a name="single-page-application-knockoutjs-template"></a>Aplicativo de página única: Modelo KnockoutJS
====================
por [Mike Wasson](https://github.com/MikeWasson)

> O modelo MVC Knockout é parte do ASP.NET e Web Tools 2012.2
> 
> [Baixar o ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


A atualização do ASP.NET e Web Tools 2012.2 inclui um modelo de aplicativo de página única (SPA) para o ASP.NET MVC 4. Esse modelo é projetado para ajudá-lo a começar a criar rapidamente aplicativos web interativos do lado do cliente.

"Aplicativo de página única" (SPA) é o termo geral para um aplicativo web que carrega uma página HTML única e, em seguida, atualiza a página dinamicamente, em vez de carregar novas páginas. Após o carregamento de página inicial, o SPA se comunica com o servidor por meio de solicitações AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX não é novidade, mas hoje em dia há estruturas de JavaScript que facilitam criar e manter um grande aplicativo SPA sofisticado. Além disso, HTML 5 e CSS3 são tornando mais fácil criar interfaces do usuário avançadas.

Para começar, o modelo SPA cria um aplicativo de exemplo "Lista de tarefas pendentes". Neste tutorial, vamos dar um tour guiado do modelo. Primeiro, vamos vai examinar o aplicativo de lista de tarefas pendentes e examinar as partes de tecnologia que fazê-lo funcionar.

## <a name="create-a-new-spa-template-project"></a>Criar um novo projeto de modelo do SPA

Requisitos:

- Visual Studio 2012 ou Visual Studio Express 2012 para Web
- ASP.NET Web Tools 2012.2 atualizam. Você pode instalar a atualização [aqui](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Inicie o Visual Studio e selecione **novo projeto** na página inicial. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

![](knockoutjs-template/_static/image2.png)

No **novo projeto** assistente, selecione **aplicativo de página única**.

![](knockoutjs-template/_static/image3.png)

Pressione F5 para compilar e executar o aplicativo. Quando o aplicativo é executado pela primeira vez, ele exibe uma tela de logon.

![](knockoutjs-template/_static/image4.png)

Clique o &quot;Inscreva-se&quot; vincular e criar um novo usuário.

![](knockoutjs-template/_static/image5.png)

Depois de fazer logon, o aplicativo cria uma lista de tarefas padrão com dois itens. Você pode clicar em "Adicionar a lista de tarefas" para adicionar uma nova lista.

![](knockoutjs-template/_static/image6.png)

A lista de renomear, adicionar itens à lista e desmarcá-las. Você também pode excluir itens ou excluir uma lista inteira. As alterações são persistidas automaticamente para um banco de dados no servidor (na verdade, LocalDB neste ponto, pois você está executando o aplicativo localmente).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Arquitetura do modelo de SPA

Este diagrama mostra os principais blocos de construção para o aplicativo.

![](knockoutjs-template/_static/image8.png)

No lado do servidor, o ASP.NET MVC serve o HTML e também lida com autenticação baseada em formulários.

API Web ASP.NET manipula todas as solicitações relacionam aos ToDoLists e ToDoItems, incluindo a introdução, criar, atualizar e excluir. O cliente troca dados com a API Web no formato JSON.

Entity Framework (EF) é a camada do / RM. Ele atua como mediador entre o mundo orientado a objeto do ASP.NET e banco de dados subjacente. O banco de dados usa o LocalDB, mas você pode alterar isso no arquivo Web. config. Normalmente você usaria LocalDB para o desenvolvimento local e, em seguida, implantar um banco de dados SQL no servidor, usando a migração do EF código primeiro.

No lado do cliente, a biblioteca Knockout. js manipula atualizações de página de solicitações AJAX. O Knockout usa associação de dados para sincronizar a página com os dados mais recentes. Dessa forma, você não precisa escrever nenhum código que percorre os dados JSON e atualiza o DOM. Em vez disso, você deve colocar atributos declarativos em HTML que dizer a Knockout como apresentar os dados.

Uma grande vantagem dessa arquitetura é que ele separa a camada de apresentação da lógica do aplicativo. Você pode criar a parte da API da Web sem saber nada sobre qual será a aparência de sua página da web. No lado do cliente, você cria um "modelo de exibição" para representar esses dados e o modelo de exibição usa o Knockout para vincular ao HTML. Que lhe permite alterar facilmente o HTML sem alterar o modelo de exibição. (Vamos examinar o Knockout um pouco mais tarde.)

## <a name="models"></a>Modelos

No projeto do Visual Studio, na pasta Modelos contém os modelos que são usados no lado do servidor. (Também há modelos no lado do cliente, falaremos sobre aqueles.)

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Esses são os modelos de banco de dados para o Entity Framework Code First. Observe que esses modelos têm propriedades que apontam para si. `ToDoList` contém uma coleção de ToDoItems e cada `ToDoItem` tem uma referência à sua lista de tarefas pai. Essas propriedades são chamadas de propriedades de navegação e eles representam a relação um-para-muitos, uma lista de tarefas e seus itens de tarefas pendentes.

O `ToDoItem` classe também usa o **[ForeignKey]** atributo para especificar que `ToDoListId` é uma chave estrangeira para a `ToDoList` tabela. Isso informa ao EF para adicionar uma restrição de chave estrangeira no banco de dados.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Essas classes definem os dados que serão enviados ao cliente. "DTO" significa "objeto de transferência de dados". O DTO define como as entidades serão serializadas em JSON. Em geral, há várias razões para usar DTOs:

- Para controlar quais propriedades são serializadas. O DTO pode conter um subconjunto das propriedades do modelo de domínio. Você pode fazer isso por motivos de segurança (ocultar dados confidenciais) ou simplesmente para reduzir a quantidade de dados que você envia.
- Para alterar a forma dos dados; por exemplo, para mesclar uma estrutura de dados mais complexa.
- Para manter qualquer lógica de negócios fora do DTO (separação de preocupações).
- Se seus modelos de domínio não podem ser serializados por algum motivo. Por exemplo, referências circulares podem causar problemas quando você serializa um objeto existem maneiras de lidar com esse problema na API Web (consulte [tratamento referências circulares do objeto](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); mas simplesmente usar um DTO evita o problema completamente.

No modelo do SPA, os DTOs contém os mesmos dados que os modelos de domínio. No entanto, eles ainda são úteis porque eles evitam referências circulares das propriedades de navegação, e eles demonstram o padrão geral de DTO.

**AccountModels.cs**

Esse arquivo contém modelos para a associação do site. O `UserProfile` classe define o esquema para perfis de usuário na associação de banco de dados. (Nesse caso, a única informação que é o nome de usuário e a ID de usuário.) As outras classes de modelo nesse arquivo são usadas para criar formulários de registro e login de usuário.

## <a name="entity-framework"></a>Entity Framework

O modelo do SPA usa o EF Code First. No desenvolvimento Code First, você define os modelos pela primeira vez no código e, em seguida, o EF usa o modelo para criar o banco de dados. Você também pode usar o EF com um banco de dados ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

O `TodoItemContext` deriva de classe na pasta Models **DbContext**. Essa classe fornece a "cola" entre os modelos e o EF. O `TodoItemContext` mantém uma `ToDoItem` coleção e um `TodoList` coleção. Para consultar o banco de dados, você simplesmente escrever uma consulta LINQ em relação a essas coleções. Por exemplo, aqui está como você pode selecionar todas as listas de tarefas pendentes para o usuário "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Você também pode adicionar novos itens à coleção, atualizar itens, ou excluir itens da coleção e manter as alterações no banco de dados.

## <a name="aspnet-web-api-controllers"></a>Controladores de API Web ASP.NET

Na API Web ASP.NET, os controladores são objetos que manipulam solicitações HTTP. Conforme mencionado, o modelo do SPA usa a API da Web para habilitar operações CRUD em `ToDoList` e `ToDoItem` instâncias. Os controladores estão localizados na pasta controladores da solução.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Trata solicitações HTTP para itens de tarefas pendentes
- `TodoListController`: Lida com solicitações HTTP para listas de tarefas pendentes.

Esses nomes são significativos, como API Web corresponde o caminho URI para o nome do controlador. (Para saber como o API Web encaminha solicitações HTTP para controladores, consulte [roteamento na API Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Vamos examinar o `ToDoListController` classe. Ele contém um membro de dados único:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

O `TodoItemContext` é usado para se comunicar com o EF, conforme descrito anteriormente. Os métodos no controlador de implementam as operações CRUD. API Web mapeia solicitações HTTP do cliente para os métodos do controlador, da seguinte maneira:

| Solicitação HTTP | Método do controlador | Descrição |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Obtém uma coleção de listas de tarefas. |
| GET/API/todo/*id* | `GetTodoList` | Obtém uma lista de tarefas por ID |
| PUT/API/todo/*id* | `PutTodoList` | Atualiza uma lista de tarefas. |
| POST /api/todo | `PostTodoList` | Cria uma nova lista de tarefas pendentes. |
| Excluir/API/todo/*id* | `DeleteTodoList` | Exclui uma lista de tarefas Pendentes. |

Observe que os URIs para algumas operações contêm espaços reservados para o valor da ID. Por exemplo, para excluir uma a lista com uma ID de 42, o URI é `/api/todo/42`.

Para saber mais sobre como usar a API da Web para operações CRUD, consulte [criando uma API da Web que dá suporte a operações de CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). O código para esse controlador é bem simples. Aqui estão alguns pontos interessantes:

- O `GetTodoLists` método usa uma consulta LINQ para filtrar os resultados pela ID do usuário conectado. Dessa forma, um usuário só vê os dados que pertencem a ele. Além disso, observe que uma instrução Select é usada para converter o `ToDoList` instâncias em `TodoListDto` instâncias.
- Os métodos PUT e POST verificam o estado de modelo antes de modificar o banco de dados. Se **ModelState** for false, esses métodos retornam HTTP 400, solicitação incorreta. Leia mais sobre a validação do modelo na API Web no [validação de modelo](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- A classe do controlador também é decorada com o **[autorizar]** atributo. Esse atributo verifica se a solicitação HTTP é autenticada. Se a solicitação não for autenticada, o cliente recebe HTTP 401 não autorizado. Leia mais sobre a autenticação no [autenticação e autorização na API Web ASP.NET](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

O `TodoController` classe é muito semelhante ao `TodoListController`. A maior diferença é que ele não define quaisquer métodos GET, porque o cliente obtenha os itens de tarefas pendentes, juntamente com cada lista de tarefas pendentes.

## <a name="mvc-controllers-and-views"></a>Exibições e controladores MVC

Os controladores do MVC também estão localizados na pasta controladores da solução. `HomeController` renderiza o HTML principal para o aplicativo. O modo de exibição para o controlador inicial é definido em Views/Home/Index.cshtml. O modo de exibição de Home renderiza conteúdo diferente dependendo se o usuário estiver conectado:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Quando os usuários estiverem conectados, eles veem a principal interface do usuário. Caso contrário, eles veem o painel de logon. Observe que essa renderização condicional ocorre no lado do servidor. Nunca tentar ocultar conteúdo confidencial no lado do cliente & 8212anything # que enviam em uma resposta HTTP é visível para alguém que está observando as mensagens HTTP brutas.

## <a name="client-side-javascript-and-knockoutjs"></a>Knockout. js e JavaScript do lado do cliente

Agora vamos voltar do lado do servidor do aplicativo para o cliente. O modelo do SPA usa uma combinação de jQuery e o Knockout. js para criar uma interface de usuário interativa suave. Knockout. js é uma biblioteca de JavaScript que torna mais fácil associar dados HTML. Knockout. js usa um padrão chamado "Model-View-ViewModel."

- O modelo é que os dados do domínio (listas de tarefas pendentes e itens de tarefas).
- O modo de exibição é o documento HTML.
- O modelo de exibição é um objeto JavaScript que contém os dados de modelo. O modelo de exibição é uma abstração de código da interface do usuário. Ele não tem conhecimento da representação HTML. Em vez disso, ele representa recursos abstratos da exibição, como "uma lista de itens de tarefas pendentes".

O modo de exibição associado a dados para o modelo de exibição. Atualizações para o modelo de exibição são refletidas automaticamente no modo de exibição. Ligações funcionam outra direção. Eventos no DOM (por exemplo, como cliques) estiverem associada a dados para as funções no modelo de exibição, que disparam chamadas AJAX.

O modelo do SPA organiza o JavaScript do lado do cliente em três camadas:

- todo.DataContext.js: envia solicitações AJAX.
- todo.Model.js: define os modelos.
- todo.ViewModel.js: define o modelo de exibição.

![](knockoutjs-template/_static/image11.png)

Esses arquivos de script estão localizados na pasta Scripts/app da solução.

![](knockoutjs-template/_static/image12.png)

**todo.DataContext** manipula todas as chamadas AJAX para os controladores de API da Web. (As chamadas AJAX para fazer logon são definidas em outro lugar, em ajaxlogin.js.)

**todo.Model.js** define os modelos do lado do cliente (navegador) para as listas de tarefas pendentes. Há duas classes de modelo: todoItem e lista de tarefas pendentes.

Muitas das propriedades nas classes de modelo são do tipo "ko. observable". Os observáveis são como Knockout faz sua mágica. Dos [documentação do Knockout](http://knockoutjs.com/documentation/introduction.html): um observável é um "objeto JavaScript que pode notificar assinantes sobre as alterações." Quando o valor de um observável é alterada, o Knockout atualiza todos os elementos HTML associados a esses observables. Por exemplo, o todoItem tem observáveis para as propriedades title e for feito:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Você também pode assinar um observável no código. Por exemplo, a classe todoItem assina alterações nas propriedades de "for feito" e "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Modelo de exibição**

O modelo de exibição é definido em todo.viewmodel.js. O modelo de exibição é o ponto central no qual o aplicativo associa os elementos da página HTML para os dados do domínio. No modelo do SPA, o modelo de exibição contém uma matriz observável do todoLists. O código a seguir no modelo de exibição informa ao Knockout para aplicar as associações:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML e vinculação de dados

O HTML da página principal é definido em Views/Home/Index.cshtml. Porque estamos usando a ligação de dados, o HTML é apenas um modelo para o que realmente é renderizado. Usa o Knockout *declarativa* associações. Vincular elementos de página a dados, adicionando um atributo de "data-bind" para o elemento. Aqui está um exemplo muito simples, obtido da documentação do Knockout:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

Neste exemplo, o Knockout atualiza o conteúdo do **&lt;span&gt;** elemento com o valor de `myItems.count()`. Sempre que esse valor é alterado, o Knockout atualiza o documento.

Knockout fornece uma série de diferentes tipos de associação. Aqui estão algumas das associações usadas no modelo do SPA:

- **foreach**: permite que você iterar por meio de um loop e aplicar a mesma marcação para cada item na lista. Isso é usado para renderizar a listas de tarefas pendentes e itens de tarefas pendentes. Dentro de **foreach**, as associações são aplicadas aos elementos da lista.
- **visível**: usado para alternar a visibilidade. Ocultar marcação quando uma coleção está vazia ou verifique a mensagem de erro visível.
- **valor**: usado para preencher os valores de formulário.
- **Clique em**: associa um evento de clique a uma função no modelo de exibição.

## <a name="anti-csrf-protection"></a>Proteção de anti-CSRF

Falsificação de solicitação entre sites (CSRF) é um ataque em que um site mal-intencionado envia uma solicitação para um site vulnerável no qual o usuário fez logon. Para ajudar a impedir ataques CSRF, o ASP.NET MVC usa *tokens antifalsificação*, também chamada de solicitação de tokens de verificação. A ideia é que o servidor coloca um token gerado aleatoriamente em uma página da web. Quando o cliente envia dados para o servidor, ele deve incluir esse valor na mensagem de solicitação.

Tokens antifalsificação funciona porque a página mal-intencionados não é possível ler os tokens do usuário, devido às diretivas de mesma origem. (A mesma origem políticas impedem a documentos hospedados em dois locais diferentes de acessar o conteúdo uns dos outros.)

ASP.NET MVC fornece suporte interno para tokens antifalsificação, por meio de [Antifalsificação](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) classe e o [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) atributo. Atualmente, essa funcionalidade não está integrada ao API da Web. No entanto, o modelo SPA inclui uma implementação personalizada para a API da Web. Esse código é definido no `ValidateHttpAntiForgeryTokenAttribute` classe, que está localizado na pasta Filters da solução. Para saber mais sobre anti-CSRF na API da Web, consulte [ataques impedindo Cross-Site CSRF (solicitação)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusão

O modelo do SPA é projetado para ajudá-lo a começar a escrever rapidamente aplicativos web modernos e interativos. Ele usa a biblioteca Knockout. js para separar a apresentação (marcação HTML) da lógica de dados e aplicativos. Mas o Knockout não é a única biblioteca de JavaScript que você pode usar para criar um SPA. Se você quiser explorar algumas outras opções, examine os [modelos criados pela comunidade do SPA](../templates/index.md).
