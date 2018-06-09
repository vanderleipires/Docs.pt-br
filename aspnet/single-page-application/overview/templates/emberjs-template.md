---
uid: single-page-application/overview/templates/emberjs-template
title: Modelo de EmberJS | Microsoft Docs
author: xqiu
description: Modelo de EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506795"
---
<a name="emberjs-template"></a>Modelo de EmberJS
====================
por [Xinyang Qiu](https://github.com/xqiu)

> O modelo MVC EmberJS é escrito por Xinyang Qiu, Thiago Santos e Nathan Totten.
> 
> [Baixe o modelo MVC EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)


O modelo EmberJS SPA foi projetado para começar a criar rapidamente aplicativos web interativos de cliente usando EmberJS.

"Aplicativo de página única" (SPA) é o termo geral para um aplicativo web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar as novas páginas. Após o carregamento de página inicial, o SPA se comunica com o servidor por meio de solicitações do AJAX.

![](emberjs-template/_static/image1.png)

AJAX não é nada novo, mas atualmente há estruturas de JavaScript que tornam mais fácil de criar e manter um grande aplicativo SPA sofisticado. Além disso, HTML 5 e CSS3 estão tornando mais fácil criar interfaces do usuário avançados.

O modelo do SPA EmberJS usa o [Ember](http://emberjs.com/) biblioteca JavaScript para manipular atualizações de página de solicitações do AJAX. Ember.js usa associação de dados para sincronizar a página com os dados mais recentes. Dessa forma, você não precisa escrever nenhum código que percorre os dados JSON e atualiza o DOM. Em vez disso, você deve colocar atributos declarativos no HTML que indicam Ember.js como apresentar os dados.

No lado do servidor, o modelo EmberJS é quase idêntico de [modelo SPA KnockoutJS](../introduction/knockoutjs-template.md). Ele usa o ASP.NET MVC para servir de documentos HTML e ASP.NET Web API para lidar com solicitações do AJAX do cliente. Para obter mais informações sobre esses aspectos do modelo, consulte o [KnockoutJS modelo](../introduction/knockoutjs-template.md) documentação. Este tópico aborda as diferenças entre o modelo de separação e o modelo EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Criar um projeto de modelo do SPA EmberJS

Baixe e instale o modelo, clique no botão de Download acima. Talvez seja necessário reiniciar o Visual Studio.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

![](emberjs-template/_static/image2.png)

No **novo projeto** assistente, selecione **Ember.js SPA projeto**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Visão geral do modelo EmberJS SPA

O modelo de EmberJS usa uma combinação de jQuery, Ember.js, Handlebars.js para criar uma interface do usuário interativo e sem interrupções.

Ember.js é uma biblioteca de JavaScript que usa um padrão MVC do lado do cliente.

- Um *modelo*escrito na linguagem de modelagem guidões, descreve a interface de usuário do aplicativo. No modo de versão, o [compilador Guidões](https://github.com/Myslik/csharp-ember-handlebars) é usado para agrupar e compilar o modelo Guidões.
- Um *modelo* armazena os dados de aplicativo que ele obtém do servidor (listas de tarefas e itens de tarefas).
- Um *controlador* armazena o estado do aplicativo. Controladores geralmente apresentam dados de modelo para os modelos correspondentes.
- Um *exibição* converte primitivo eventos do aplicativo e os transmite para o controlador.
- Um *roteador* gerencia o estado do aplicativo, mantendo os modelos e URLs em sincronia.

Além disso, a biblioteca Ember dados pode ser usada para sincronizar objetos JSON (obtidos do servidor por meio de uma API RESTful) e os modelos de cliente.

O modelo EmberJS SPA organiza os scripts em oito camadas:

- webapi\_adapter.js, webapi\_serializer.js: estende a biblioteca Ember dados para trabalhar com a API da Web do ASP.NET.
- Scripts/Helpers.js: Define os auxiliares Ember Guidões novo.
- Scripts/App.js: Cria o aplicativo e configura o adaptador e o serializador.
- Aplicativo/scripts/modelos/\*. js: define os modelos.
- Aplicativo/scripts/exibições/\*. js: define as exibições.
- Aplicativo/scripts/controladores/\*. js: define os controladores.
- Aplicativo/scripts/rotas, Scripts/app/router.js: Define as rotas.
- Modelos /\*.hbs: define os modelos Guidões.

Vamos examinar alguns desses scripts em mais detalhes.

## <a name="models"></a>Modelos

Os modelos são definidos na pasta Scripts/aplicativo/modelos. Há dois arquivos de modelo: todoItem.js e todoList.js.

**todo.Model.js** define os modelos de cliente (navegador) para as listas de tarefas pendentes. Há duas classes de modelo: todoItem e lista de tarefas. Em Ember, os modelos são subclasses de DS. Modelo. Um modelo pode ter propriedades com atributos:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modelos podem definir relações com outros modelos:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modelos podem ter computada propriedades vincular a outras propriedades:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modelos podem ter funções do observador, que são invocadas quando uma propriedade observada alterações:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Exibições

As exibições são definidas na pasta Scripts/aplicativo/modos de exibição. Um modo de exibição converte os eventos do aplicativo da interface do usuário. Um manipulador de eventos pode retornar chamada para funções de controlador ou simplesmente chamar o contexto de dados diretamente.

Por exemplo, o código a seguir é de views/TodoItemEditView.js. Ele define o manipulação de eventos para um campo de texto de entrada.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controlador

Os controladores são definidos na pasta Scripts/aplicativo/controladores. Para representar um único modelo, estender `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Um controlador também pode representar uma coleção de modelos, estendendo `Ember.ArrayController`. Por exemplo, o TodoListController representa uma matriz de `todoList` objetos. O controlador classifica por ID de lista de tarefas, em ordem decrescente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

O controlador define uma função chamada `addTodoList`, que cria uma nova lista de tarefas e o adiciona à matriz. Para ver como essa função é chamada, abra o arquivo de modelo chamado todoListTemplate.html na pasta modelos. O código de modelo a seguir associa um botão para o `addTodoList` função:

[!code-html[Main](emberjs-template/samples/sample8.html)]

O controlador também contém um `error` propriedade, que contém uma mensagem de erro. Aqui está o código de modelo para exibir a mensagem de erro (também em todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Rotas

Router.js define as rotas e o modelo padrão para exibir conjuntos de backup do estado do aplicativo e URLs de correspondências para rotas:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js carrega dados para o TodoListRoute, substituindo a função setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember usa convenções de nomenclatura para corresponder URLs, nomes de rotas, controladores e modelos. Para obter mais informações, consulte [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) a documentação de EmberJS.

## <a name="templates"></a>Modelos

A pasta de modelos contém quatro modelos:

- Application.HBS: O modelo padrão que é renderizado quando o aplicativo é iniciado.
- About.HBS: O modelo da rota "/ sobre".
- index.HBS: O modelo para a raiz de rota "/".
- todoList.hbs: O modelo para o "/ todo" rota.
- \_NavBar.HBS: O modelo define o menu de navegação.

O modelo de aplicativo age como uma página mestra. Ele contém um cabeçalho, um rodapé e uma "{{tomada}}" para inserir os outros modelos dependendo da rota. Para obter mais informações sobre modelos de aplicativos em Ember, consulte [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

O "/ todoList" modelo contém duas expressões de loop. O loop externo é `{{#each controller}}`e o loop é `{{#each todos}}`. O código a seguir mostra um interno `Ember.Checkbox` exibir um personalizado `App.TodoItemEditView`e um link com um `deleteTodo` ação.

[!code-html[Main](emberjs-template/samples/sample12.html)]

O `HtmlHelperExtensions` classe, definida no Controllers/HtmlHelperExensions.cs, define um auxiliar de função em cache e inserir modelo arquivos ao **depurar** é definido como **true** no arquivo Web. config. Essa função é chamada do arquivo de exibição do ASP.NET MVC definido na Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

A função chamado sem argumentos, processa todos os arquivos de modelo na pasta modelos. Você também pode especificar uma subpasta ou um arquivo de modelo específico.

Quando **depurar** é **false** em Web. config, o aplicativo inclui o item de pacote "~/bundles/templates". Este item de pacote é adicionado no BundleConfig.cs, usando a biblioteca de compilador Guidões:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
