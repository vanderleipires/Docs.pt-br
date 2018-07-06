---
uid: single-page-application/overview/templates/emberjs-template
title: Modelo EmberJS | Microsoft Docs
author: xqiu
description: Modelo EmberJS
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 2488e9c10550bd9b11c675572c70618f6ca4ac05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823771"
---
<a name="emberjs-template"></a>Modelo EmberJS
====================
por [Xinyang Qiu](https://github.com/xqiu)

> O modelo MVC EmberJS é gravado por Nathan Totten, Thiago Santos e Xinyang Qiu.
> 
> [Baixe o modelo MVC EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)


O modelo EmberJS SPA é projetado para ajudá-lo a começar a criar rapidamente aplicativos web interativos do lado do cliente usando o EmberJS.

"Aplicativo de página única" (SPA) é o termo geral para um aplicativo web que carrega uma página HTML única e, em seguida, atualiza a página dinamicamente, em vez de carregar novas páginas. Após o carregamento de página inicial, o SPA se comunica com o servidor por meio de solicitações AJAX.

![](emberjs-template/_static/image1.png)

AJAX não é novidade, mas hoje em dia há estruturas de JavaScript que facilitam criar e manter um grande aplicativo SPA sofisticado. Além disso, HTML 5 e CSS3 são tornando mais fácil criar interfaces do usuário avançadas.

O modelo de SPA EmberJS usa o [Ember](http://emberjs.com/) biblioteca de JavaScript para manipular atualizações de página de solicitações AJAX. Ember usa associação de dados para sincronizar a página com os dados mais recentes. Dessa forma, você não precisa escrever nenhum código que percorre os dados JSON e atualiza o DOM. Em vez disso, você deve colocar atributos declarativos em HTML que informam o ember como apresentar os dados.

No lado do servidor, o modelo EmberJS é quase idêntico do [modelo KnockoutJS SPA](../introduction/knockoutjs-template.md). Ele usa o ASP.NET MVC para servir de documentos HTML e a API Web do ASP.NET para manipular solicitações AJAX do cliente. Para obter mais informações sobre esses aspectos do modelo, consulte o [modelo KnockoutJS](../introduction/knockoutjs-template.md) documentação. Este tópico enfoca as diferenças entre o modelo do Knockout e o modelo EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Criar um projeto de modelo do SPA EmberJS

Baixe e instale o modelo clicando no botão de Download acima. Talvez você precise reiniciar o Visual Studio.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

![](emberjs-template/_static/image2.png)

No **novo projeto** assistente, selecione **projeto do SPA ember**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Visão geral do modelo EmberJS SPA

O modelo EmberJS usa uma combinação de jQuery, ember, Handlebars.js para criar uma interface de usuário interativa suave.

Ember é uma biblioteca de JavaScript que usa um padrão MVC do lado do cliente.

- Um *modelo*escrito na linguagem de modelagem Handlebars, descreve a interface de usuário do aplicativo. No modo de versão, o [compilador Handlebars](https://github.com/Myslik/csharp-ember-handlebars) é usado para agrupar e compilar o modelo handlebars.
- Um *modelo* armazena os dados de aplicativo que ele obtém do servidor (listas de tarefas pendentes e itens de tarefas).
- Um *controlador* armazena o estado do aplicativo. Controladores geralmente apresentam dados de modelo para os modelos correspondentes.
- Um *exibição* converte primitivos eventos do aplicativo e os passa para o controlador.
- Um *roteador* gerencia o estado do aplicativo, mantendo as URLs e modelos em sincronia.

Além disso, a biblioteca Ember dados pode ser usada para sincronizar objetos JSON (obtidos do servidor por meio de uma API RESTful) e os modelos de cliente.

O modelo EmberJS SPA organiza os scripts em oito camadas:

- webapi\_adapter.js, webapi\_serializer.js: estende a biblioteca Ember dados para trabalhar com a API Web do ASP.NET.
- Scripts/Helpers.js: Define novos auxiliares de Handlebars Ember.
- Scripts/App.js: Cria o aplicativo e configura o adaptador e o serializador.
- App/scripts/modelos/\*. js: define os modelos.
- Scripts/app/exibições/\*. js: define as exibições.
- App/scripts/controladores/\*. js: define os controladores.
- Scripts/app/rotas, Scripts/app/router.js: Define as rotas.
- Modelos /\*.hbs: define os modelos handlebars.

Vamos examinar alguns desses scripts mais detalhadamente.

## <a name="models"></a>Modelos

Os modelos são definidos na pasta Scripts/app/modelos. Existem dois arquivos de modelo: todoitem. js e todoList.js.

**todo.Model.js** define os modelos do lado do cliente (navegador) para as listas de tarefas pendentes. Há duas classes de modelo: todoItem e lista de tarefas pendentes. No Ember, os modelos são subclasses de DS. Modelo. Um modelo pode ter propriedades com atributos:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modelos podem definir relações com outros modelos:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modelos podem ter propriedades que se associam a outras propriedades computadas:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modelos podem ter funções de observador, que são chamadas quando uma propriedade observada ser alterada:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Exibições

As exibições são definidas na pasta Scripts/app/exibições. Um modo de exibição converte os eventos do aplicativo da interface do usuário. Um manipulador de eventos pode chamada de retorno para funções de controlador ou simplesmente chamar o contexto de dados diretamente.

Por exemplo, o código a seguir é de views/TodoItemEditView.js. Ele define o manipulação de eventos para um campo de texto de entrada.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controlador

Os controladores são definidos na pasta Scripts/app/controladores. Para representar um único modelo, estender `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Um controlador também pode representar uma coleção de modelos, estendendo `Ember.ArrayController`. Por exemplo, o TodoListController representa uma matriz de `todoList` objetos. O controlador classifica por ID de lista de tarefas pendentes, em ordem decrescente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

O controlador define uma função chamada `addTodoList`, que cria uma nova lista de tarefas e o adiciona à matriz. Para ver como essa função é chamada, abra o arquivo de modelo chamado todoListTemplate.html, na pasta modelos. O código de modelo a seguir associa um botão para o `addTodoList` função:

[!code-html[Main](emberjs-template/samples/sample8.html)]

O controlador também contém um `error` propriedade, que contém uma mensagem de erro. Aqui está o código de modelo para exibir a mensagem de erro (também em todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Rotas

Router.js define as rotas e o modelo padrão para exibir conjuntos de backup do estado do aplicativo e corresponde a URLs às rotas:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js carrega dados para o TodoListRoute, substituindo a função setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

O ember usa as convenções de nomenclatura para corresponder a URLs, nomes de rotas, controladores e modelos. Para obter mais informações, consulte [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) na documentação EmberJS.

## <a name="templates"></a>Modelos

A pasta Modelos contém quatro modelos:

- Application.HBS: O modelo padrão que é renderizado quando o aplicativo é iniciado.
- About.HBS: O modelo da rota "/ sobre".
- index.HBS: O modelo para a raiz "/" rota.
- todoList.hbs: O modelo para o "/ todo" rota.
- \_NavBar.HBS: O modelo define o menu de navegação.

O modelo de aplicativo funciona como uma página mestra. Ele contém um cabeçalho, um rodapé e uma "{{outlet}}" para inserir outros modelos no dependendo da rota. Para obter mais informações sobre modelos de aplicativo no Ember, consulte [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

O "/ lista de tarefas pendentes" modelo contém duas expressões de loop. O loop externo é `{{#each controller}}`e o loop é `{{#each todos}}`. O código a seguir mostra uma interna `Ember.Checkbox` exibir, um personalizado `App.TodoItemEditView`e um link com um `deleteTodo` ação.

[!code-html[Main](emberjs-template/samples/sample12.html)]

O `HtmlHelperExtensions` classe, definida na Controllers/HtmlHelperExensions.cs, define um auxiliar de função em cache e inserir modelo arquivos ao **debug** é definido como **true** no arquivo Web. config. Essa função é chamada do arquivo de exibição do MVC do ASP.NET definido no Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

A função chamado sem argumentos, processa todos os arquivos de modelo na pasta modelos. Você também pode especificar uma subpasta ou um arquivo de modelo específico.

Quando **debug** é **falso** no Web. config, o aplicativo inclui o item de pacote "~/bundles/templates". Este item de pacote for adicionado no BundleConfig.cs, usando a biblioteca de compilador Handlebars:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
