---
uid: single-page-application/overview/introduction/knockoutjs-template
title: "Aplicativo de página única: Modelo KnockoutJS | Microsoft Docs"
author: MikeWasson
description: "Modelo de separação"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: e6c0c45bed098a8a1160ff11e4f77244bf55ffd3
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
<a name="single-page-application-knockoutjs-template"></a>Aplicativo de página única: Modelo de KnockoutJS
====================
por [Mike Wasson](https://github.com/MikeWasson)

> O modelo MVC Knockout faz parte do ASP.NET e Web Tools 2012.2
> 
> [Baixe o ASP.NET e Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


A atualização do ASP.NET e Web Tools 2012.2 inclui um modelo de aplicativo de página única (SPA) para o ASP.NET MVC 4. Este modelo é desenvolvido para você começar a criar rapidamente aplicativos web interativos do lado do cliente.

"Aplicativo de página única" (SPA) é o termo geral para um aplicativo web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar as novas páginas. Após o carregamento de página inicial, o SPA se comunica com o servidor por meio de solicitações do AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX não é nada novo, mas atualmente há estruturas de JavaScript que tornam mais fácil de criar e manter um grande aplicativo SPA sofisticado. Além disso, HTML 5 e CSS3 estão tornando mais fácil criar interfaces do usuário avançados.

Para começar, o modelo SPA cria um aplicativo de exemplo "Lista de tarefas". Neste tutorial, vamos dar um tour guiado do modelo. Primeiro, será observar o aplicativo de lista de tarefas e examinar as partes de tecnologia que fazê-lo funcionar.

## <a name="create-a-new-spa-template-project"></a>Criar um novo projeto de modelo do SPA

Requisitos:

- O Visual Studio 2012 ou Visual Studio Express 2012 para Web
- Atualização de ferramentas da Web do ASP.NET 2012.2. Você pode instalar a atualização [aqui](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Inicie o Visual Studio e selecione **novo projeto** na página de início. Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

![](knockoutjs-template/_static/image2.png)

No **novo projeto** assistente, selecione **aplicativo de página única**.

![](knockoutjs-template/_static/image3.png)

Pressione F5 para compilar e executar o aplicativo. Quando o aplicativo é executado pela primeira vez, ele exibe uma tela de logon.

![](knockoutjs-template/_static/image4.png)

Clique o &quot;inscrever&quot; vincular e criar um novo usuário.

![](knockoutjs-template/_static/image5.png)

Após fazer logon, o aplicativo cria uma lista de tarefas padrão com dois itens. Você pode clicar em "Adicionar a lista de tarefas" para adicionar uma nova lista.

![](knockoutjs-template/_static/image6.png)

A lista de renomeação, adicionar itens à lista e desmarcá-los. Você também pode excluir itens ou excluir uma lista completa. As alterações são automaticamente persistidas para um banco de dados no servidor (realmente LocalDB neste momento, pois você está executando o aplicativo localmente).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Arquitetura do modelo SPA

Este diagrama mostra os principais blocos de construção para o aplicativo.

![](knockoutjs-template/_static/image8.png)

No lado do servidor, o ASP.NET MVC serve o HTML e também lida com autenticação baseada em formulários.

API da Web ASP.NET manipula todas as solicitações relacionam aos ToDoLists e ToDoItems, incluindo obtendo, criação, atualização e exclusão. O cliente de troca de dados com a API da Web no formato JSON.

Entity Framework (EF) é a camada de S/RM. Atua como mediador entre o mundo orientado a objeto do ASP.NET e o banco de dados subjacente. O banco de dados usa o LocalDB, mas você pode alterar isso no arquivo Web. config. Normalmente você usaria LocalDB para desenvolvimento local e, em seguida, implantar um banco de dados SQL no servidor, usando a migração de código primeiro EF.

No lado do cliente, a biblioteca Knockout. js manipula atualizações de página de solicitações do AJAX. Knockout usa associação de dados para sincronizar a página com os dados mais recentes. Dessa forma, você não precisa escrever nenhum código que percorre os dados JSON e atualiza o DOM. Em vez disso, você deve colocar atributos declarativos no HTML que informam Knockout como apresentar os dados.

Uma grande vantagem dessa arquitetura é que ele separa a camada de apresentação da lógica do aplicativo. Você pode criar a parte da API da Web sem saber nada sobre a aparência de sua página da web. No lado do cliente, você deve criar um modelo de exibição"" para representar dados e o modelo de exibição usa Knockout para associar a HTML. Que lhe permite alterar facilmente o HTML sem alterar o modelo de exibição. (Analisaremos Knockout um pouco mais tarde.)

## <a name="models"></a>Modelos

No projeto do Visual Studio, na pasta Modelos contém os modelos que são usados no lado do servidor. (Também há modelos no lado do cliente; obterá às).

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Esses são os modelos de banco de dados para o Entity Framework Code First. Observe que esses modelos têm propriedades que apontem para outro. `ToDoList` contém uma coleção de ToDoItems e cada `ToDoItem` tem uma referência à sua lista de tarefas do pai. Essas propriedades são chamadas de propriedades de navegação, e eles representam a relação um-para-muitos, uma lista de tarefas e seus itens de tarefas pendentes.

O `ToDoItem` classe também usa o **[ForeignKey]** atributo para especificar que `ToDoListId` é uma chave estrangeira para a `ToDoList` tabela. Isso informa ao EF para adicionar uma restrição de chave estrangeira no banco de dados.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Essas classes definem os dados que serão enviados ao cliente. "DTO" significa "objeto de transferência de dados". O DTO define como as entidades serão serializadas em JSON. Em geral, há várias razões para usar DTOs:

- Para controlar quais propriedades são serializadas. O DTO pode conter um subconjunto das propriedades do modelo de domínio. Você pode fazer isso por motivos de segurança (ocultar dados confidenciais) ou simplesmente para reduzir a quantidade de dados que você enviar.
- Para alterar a forma dos dados – por exemplo, para mesclar uma estrutura de dados mais complexa.
- Para manter qualquer lógica de negócios fora DTO (separação de preocupações).
- Se seus modelos de domínio não podem ser serializados por alguma razão. Por exemplo, as referências circulares podem causar problemas ao serializar um objeto que não há formas de lidar com esse problema na API da Web (consulte [tratamento de referências de objeto Circular](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); mas simplesmente usar um DTO evita o problema completamente.

No modelo SPA, os DTOs contém os mesmos dados que os modelos de domínio. No entanto, eles ainda são úteis porque eles evitam referências circulares de propriedades de navegação, e eles demonstram o padrão geral de DTO.

**AccountModels.cs**

Este arquivo contém modelos para associação de site. O `UserProfile` classe define o esquema para perfis de usuário na associação de banco de dados. (Nesse caso, a única informação que é o nome de usuário e a ID de usuário.) As outras classes de modelo no arquivo são usadas para criar formulários de logon e registro de usuário.

## <a name="entity-framework"></a>Entity Framework

O modelo do SPA usa EF Code First. No desenvolvimento de Code First, você define os modelos de primeiro no código e EF usa o modelo para criar o banco de dados. Você também pode usar o EF com um banco de dados existente ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

O `TodoItemContext` deriva de classe na pasta modelos **DbContext**. Essa classe fornece a "cola" entre os modelos e EF. O `TodoItemContext` mantém um `ToDoItem` coleção e um `TodoList` coleção. Para consultar o banco de dados, você simplesmente escrever uma consulta LINQ em relação a essas coleções. Por exemplo, aqui está como você pode selecionar todas as listas de tarefas do usuário "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Você também pode adicionar novos itens à coleção, atualizar itens, ou excluir itens da coleção e manter as alterações no banco de dados.

## <a name="aspnet-web-api-controllers"></a>Controladores de API da Web ASP.NET

Na API da Web do ASP.NET, os controladores são objetos que manipulam solicitações HTTP. Conforme mencionado, o modelo SPA usa a API da Web para habilitar operações CRUD em `ToDoList` e `ToDoItem` instâncias. Os controladores estão localizados na pasta controladores da solução.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Trata as solicitações HTTP para os itens de tarefas pendentes
- `TodoListController`: Trata as solicitações HTTP para as listas de tarefas.

Esses nomes são significativos, pois a API da Web corresponda ao caminho URI para o nome do controlador. (Para saber como a API da Web encaminha solicitações HTTP para controladores, consulte [roteamento na API da Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Vamos examinar a `ToDoListController` classe. Ele contém um membro de dados único:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

O `TodoItemContext` é usado para se comunicar com o EF, conforme descrito anteriormente. Os métodos no controlador de implementam as operações CRUD. API da Web mapeia solicitações HTTP do cliente para os métodos do controlador, da seguinte forma:

| Solicitação HTTP | Método do controlador | Descrição |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Obtém uma coleção de listas de tarefas. |
| GET/api/tarefas/*id* | `GetTodoList` | Obtém uma lista de tarefas por ID |
| PUT/api/tarefas/*id* | `PutTodoList` | Atualiza uma lista de tarefas. |
| POST /api/todo | `PostTodoList` | Cria uma nova lista de tarefas. |
| Excluir/api/tarefas/*id* | `DeleteTodoList` | Exclui uma lista de tarefas. |

Observe que os URIs para algumas operações contêm espaços reservados para o valor de ID. Por exemplo, para excluir uma a lista com uma ID de 42, o URI é `/api/todo/42`.

Para saber mais sobre como usar a API da Web para operações CRUD, consulte [criando uma API da Web que dá suporte a operações de CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). O código para esse controlador é bastante simples. Aqui estão alguns pontos interessantes:

- O `GetTodoLists` método usa uma consulta LINQ para filtrar os resultados pela ID do usuário conectado. Dessa forma, um usuário vê somente os dados que pertencem a ele. Além disso, observe que uma instrução Select é usada para converter o `ToDoList` instâncias em `TodoListDto` instâncias.
- Os métodos PUT e POST verificam o estado do modelo antes de modificar o banco de dados. Se **ModelState** é false, esses métodos retornam HTTP 400 Solicitação incorreta. Leia mais sobre a validação do modelo na API da Web em [validação de modelo](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- A classe do controlador também é decorada com o **[autorizar]** atributo. Esse atributo verifica se a solicitação HTTP é autenticada. Se a solicitação não é autenticada, o cliente recebe HTTP 401 não autorizado. Leia mais sobre a autenticação no [autenticação e autorização em ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

O `TodoController` classe é muito semelhante ao `TodoListController`. A maior diferença é que ele não define nenhum método GET, porque o cliente receberá os itens de tarefas pendentes junto com cada lista de tarefas.

## <a name="mvc-controllers-and-views"></a>Modos de exibição e controladores MVC

Os controladores MVC também estão localizados na pasta controladores da solução. `HomeController` renderiza o HTML principal para o aplicativo. O modo de exibição para o controlador inicial é definido em Views/Home/Index.cshtml. O modo de exibição de Home renderiza conteúdo diferente dependendo se o usuário estiver conectado:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Quando os usuários estiverem conectados, eles veem a interface do usuário principal. Caso contrário, eles veem o painel de logon. Observe que esse processamento condicional ocorre no lado do servidor. Nunca tentar ocultar conteúdo confidencial no lado do cliente & #8212anything que você enviar uma resposta de HTTP é visível para alguém que está observando as mensagens HTTP brutas.

## <a name="client-side-javascript-and-knockoutjs"></a>Knockout. js e JavaScript do lado do cliente

Agora vamos do lado do servidor do aplicativo para o cliente. O modelo do SPA usa uma combinação de jQuery e Knockout. js para criar uma interface do usuário interativo e sem interrupções. Knockout. js é uma biblioteca de JavaScript que torna mais fácil associar HTML aos dados. Knockout. js usa um padrão chamado "Model-View-ViewModel."

- O modelo é que os dados do domínio (listas de tarefas e itens de tarefas).
- O modo de exibição é um documento HTML.
- O modelo de exibição é um objeto JavaScript que contém os dados de modelo. O modelo de exibição é uma abstração de código da interface do usuário. Ele não tem conhecimento da representação de HTML. Em vez disso, ele representa abstratos recursos do modo de exibição, como "uma lista de itens de tarefas".

O modo de exibição associado a dados para o modelo de exibição. Atualizações para o modelo de exibição são refletidas automaticamente no modo de exibição. Associações de trabalho outra direção. Eventos no DOM (tais como cliques) são dados associados a funções no modelo, o modo de exibição que disparam chamadas AJAX.

O modelo do SPA organiza o JavaScript do lado do cliente em três camadas:

- todo.DataContext.js: envia solicitações do AJAX.
- todo.Model.js: define os modelos.
- todo.ViewModel.js: define o modelo de exibição.

![](knockoutjs-template/_static/image11.png)

Esses arquivos de script estão localizados na pasta Scripts/aplicativo da solução.

![](knockoutjs-template/_static/image12.png)

**todo.DataContext** controla todas as chamadas AJAX para os controladores de API da Web. (As chamadas AJAX para log em são definidas em outro lugar, em ajaxlogin.js.)

**todo.Model.js** define os modelos de cliente (navegador) para as listas de tarefas pendentes. Há duas classes de modelo: todoItem e lista de tarefas.

Muitas das propriedades nas classes de modelo são do tipo "ko.observable". Observáveis são como Knockout faz sua mágica. Do [documentação Knockout](http://knockoutjs.com/documentation/introduction.html): observável é um "objeto JavaScript que pode notificar assinantes sobre alterações." Quando o valor de observável alterado, Knockout atualiza todos os elementos HTML que estão associados a esses observáveis. Por exemplo, todoItem tem observáveis para as propriedades title e isDone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Você também pode assinar observável no código. Por exemplo, a classe todoItem assina alterações nas propriedades de "isDone" e "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Modelo de exibição**

O modelo de exibição é definido em todo.viewmodel.js. O modelo de exibição é o ponto central onde o aplicativo associa os elementos da página HTML para os dados de domínio. No modelo SPA, o modelo de exibição contém uma matriz observável de todoLists. O código a seguir no modelo de exibição informa Knockout para aplicar as associações:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML e associação de dados

O HTML da página principal é definido em Views/Home/Index.cshtml. Como estamos usando associação de dados, o HTML é apenas um modelo para o que realmente é renderizado. Usa Knockout *declarativa* associações. Vincular elementos da página de dados adicionando um atributo de "data-bind" para o elemento. Aqui está um exemplo muito simples, feito na documentação do Knockout:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

Neste exemplo, Knockout atualiza o conteúdo do  **&lt;abrangem&gt;**  elemento com o valor de `myItems.count()`. Sempre que esse valor é alterado, Knockout atualiza o documento.

Knockout fornece uma variedade de tipos diferentes de associação. Aqui estão algumas das associações usadas no modelo SPA:

- **foreach**: permite que você percorrer um loop e aplicar a mesma marcação para cada item na lista. Isso é usado para processar as listas de tarefas e itens de tarefas. Dentro de **foreach**, as associações são aplicadas aos elementos da lista.
- **visível**: usada para alternar a visibilidade. Ocultar marcação quando uma coleção está vazia ou verifique a mensagem de erro visível.
- **valor**: usado para preencher os valores de formulário.
- **Clique em**: associa um evento de clique a uma função no modelo de exibição.

## <a name="anti-csrf-protection"></a>Proteção contra CSRF

Falsificação de solicitação entre sites (CSRF) é um ataque em que um site mal-intencionado envia uma solicitação para um site vulnerável onde o usuário está atualmente conectado. Para ajudar a evitar ataques CSRF, ASP.NET MVC usa *tokens antifalsificação*, também chamado de solicitação de tokens de verificação. A ideia é que o servidor coloca um token gerado aleatoriamente em uma página da web. Quando o cliente envia dados para o servidor, esse valor deve incluir na mensagem de solicitação.

Tokens antifalsificação funciona porque a página mal-intencionado não é possível ler os tokens do usuário, devido às diretivas de mesma origem. (Políticas de mesma origem impedir que os documentos hospedados em dois locais diferentes de acessar o conteúdo do outro.)

ASP.NET MVC fornece suporte interno para tokens antifalsificação, por meio de [AntiForgery](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) classe e o [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) atributo. No momento, essa funcionalidade não é interno ao API da Web. No entanto, o modelo SPA inclui uma implementação personalizada de API da Web. Esse código é definido no `ValidateHttpAntiForgeryTokenAttribute` classe, que está localizado na pasta de filtros da solução. Para saber mais sobre anti-CSRF na API da Web, consulte [ataques impedindo intersite solicitações forjadas (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusão

O modelo do SPA foi projetado para começar a escrever rapidamente aplicativos web modernos e interativos. Ele usa a biblioteca de Knockout. js para separar a apresentação (como marcação HTML) da lógica de dados e aplicativos. Mas Knockout não é a biblioteca JavaScript somente que você pode usar para criar um SPA. Se você quiser explorar algumas outras opções, examine o [modelos criados pela comunidade SPA](../templates/index.md).
