---
uid: single-page-application/overview/templates/backbonejs-template
title: Modelo backbone | Microsoft Docs
author: madskristensen
description: Modelo SPA backbone. js
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 325c4f5370340b2e223521fada77cf0e78a67b5b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830609"
---
<a name="backbone-template"></a>Modelo backbone
====================
por [Mads Kristensen](https://github.com/madskristensen)

> O modelo do SPA Backbone foi escrito por Kazi Manzur Rashid
> 
> [Baixe o modelo SPA backbone. js](https://go.microsoft.com/fwlink/?LinkId=293631)


O modelo backbone. js SPA é projetado para ajudá-lo a começar a criar rapidamente aplicativos web interativos do lado do cliente usando [backbone. js.](http://backbonejs.org/)

O modelo fornece um esqueleto inicial para o desenvolvimento de um aplicativo de backbone. js no ASP.NET MVC. Fora da caixa, ele fornece funcionalidade de logon de usuário básicos, incluindo redefinição de senha de inscrição, entrar, usuário e a confirmação do usuário com modelos de email básico.

Requisitos:

- [Atualização do ASP.NET e Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Criar um projeto de modelo Backbone

Baixe e instale o modelo clicando no botão de Download acima. O modelo é empacotado como um arquivo de extensão VSIX (Visual Studio). Talvez você precise reiniciar o Visual Studio.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

![](backbonejs-template/_static/image1.png)

No **novo projeto** assistente, selecione projeto do SPA backbone. js.

![](backbonejs-template/_static/image2.png)

Pressione Ctrl-F5 para compilar e executar o aplicativo sem depuração ou pressione F5 para executar com depuração.

![](backbonejs-template/_static/image3.png)

Clicar em "Minha conta" abre a página de logon:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Passo a passo: Código de cliente

Vamos começa com o lado do cliente. Os scripts de aplicativo do cliente estão localizados na pasta ~/Scripts/application. O aplicativo é escrito em [TypeScript](http://www.typescriptlang.org/) (arquivos. TS) que é compilado em JavaScript (arquivos. js).

**Aplicativo**

`Application` é definido em application.ts. Esse objeto inicializa o aplicativo e atua como o namespace raiz. Ele mantém informações de configuração e o estado são compartilhadas entre o aplicativo, como se o usuário está conectado.

O `application.start` método cria os modos de exibição modais e anexa os manipuladores de eventos para eventos de nível de aplicativo, como a entrada do usuário. Em seguida, ele cria o roteador padrão e verifica se qualquer URL do lado do cliente é especificado. Se não, ele redireciona para a url padrão (#! /).

**Eventos**

Eventos sempre são importantes ao desenvolver fracamente acoplados componentes. Aplicativos geralmente executam várias operações em resposta a uma ação do usuário. Backbone fornece eventos internos com componentes, como modelo, coleta e exibição. Em vez de criar interdependências entre esses componentes, o modelo usa um modelo de "pub/sub": O `events` objeto, definido em events.ts, atua como um hub de eventos para publicar e assinar eventos de aplicativo. O `events` objeto é um singleton. O código a seguir mostra como assinar um evento e, em seguida, disparar o evento:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Roteador**

No backbone. js, um roteador fornece métodos de roteamento de páginas do lado do cliente e conectá-las para ações e eventos. O modelo define um único roteador, em router.ts. O roteador cria os modos de exibição activable e mantém o estado ao alternar os modos de exibição. (Activable exibições são descritas na próxima seção). Inicialmente, o projeto tem dois modos de exibição fictícios, sobre e domésticos. Ele também tem um modo de exibição não encontrado, que será exibido se a rota não é conhecida.

**Exibições**

As exibições são definidas em ~/Scripts/application/exibições. Há dois tipos de modos de exibição, modos de exibição activable e modos de exibição da caixa de diálogo modal. Modos de exibição activable são invocados pelo roteador. Quando uma exibição activable é mostrada, todos os outros modos de exibição activable se tornar inativos. Para criar uma exibição activable, estender a exibição com o `Activable` objeto:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Estendendo com `Activable` adiciona dois novos métodos para o modo de exibição `activate` e `deactivate`. O roteador chama esses métodos para ativar e desativar o modo de exibição.

Modos de exibição modais são implementados como [Twitter Bootstrap](http://twitter.github.com/bootstrap/) caixas de diálogo modais. O `Membership` e `Profile` modos de exibição são modais. Modos de exibição de modelo podem ser invocados por quaisquer eventos de aplicativo. Por exemplo, no `Navigation` exibição, clicando no link "Minha conta" mostra ao `Membership` modo de exibição ou o `Profile` exibição, dependendo se o usuário estiver conectado. O `Navigation` anexa clique manipuladores de eventos para elementos filho que têm o `data-command` atributo. Aqui está a marcação HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Aqui está o código em navigation.ts para ligar os eventos:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelos**

Os modelos são definidos em ~/Scripts/application ou os modelos. Todos os modelos têm três coisas básicas: atributos, as regras de validação e um ponto de extremidade do lado do servidor padrão. Aqui está um exemplo típico:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Plug-ins**

A pasta ~/Scripts/application/lib contém alguns plug-ins de jQuery útil. O arquivo de form.ts define um plug-in para trabalhar com dados de formulário. Muitas vezes você precisa serializar ou desserializar dados de formulário e mostrar erros de validação do modelo. O plug-in de form.ts tem métodos como `serializeFields`, `deserializeFields`, e `showFieldErrors`. O exemplo a seguir mostra como serializar um formulário para um modelo.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

O plug-in de flashbar.ts oferece vários tipos de mensagens de comentários ao usuário. Os métodos são `$.showSuccessbar`, `$.showErrorbar` e `$.showInfobar`. Nos bastidores, ele usa alertas Twitter Bootstrap para mostrar mensagens bem animadas.

O plug-in de confirm.ts substitui o navegador confirme se a caixa de diálogo, embora a API é um pouco diferente:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Passo a passo: Código de servidor

Agora vamos dar uma olhada no lado do servidor.

**Controladores**

Em um aplicativo de página única, o servidor é reproduzido apenas uma pequena função na interface do usuário. Normalmente, o servidor processa a página inicial e, em seguida, envia e recebe dados JSON.

O modelo tem dois controladores MVC: `HomeController` renderiza a página inicial, e `SupportsController` é usado para confirmar as novas contas de usuário e redefinir senhas. Todos os outros controladores no modelo são controladores de API Web ASP.NET, que enviam e recebem dados JSON. Por padrão, os controladores usam o novo `WebSecurity` classe para executar tarefas relacionadas ao usuário. No entanto, eles também têm construtores opcionais que permitem que você passe em delegados para essas tarefas. Isso facilita os testes e permite que você substitua `WebSecurity` com algo, por meio de um contêiner de IoC. Veja um exemplo:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Exibições

As exibições são projetadas para serem modular: cada seção de uma página tem sua própria exibição dedicada. Em um aplicativo de página única, é comum incluir modos de exibição que não têm qualquer controlador correspondente. Você pode incluir uma exibição chamando `@Html.Partial('myView')`, mas isso é entediante. Para facilitar essa tarefa, o modelo define um método auxiliar, `IncludeClientViews`, que processa todas as exibições em uma pasta especificada:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Se o nome da pasta não for especificado, o nome da pasta padrão é "ClientViews". Se o modo de exibição do cliente também usa as exibições parciais, nomeie a exibição parcial com um caractere de sublinhado (por exemplo, `_SignUp`). O `IncludeClientViews` método exclui todas as exibições cujo nome começa com um sublinhado. Para incluir uma exibição parcial na exibição do cliente, chame `Html.ClientView('SignUp')` em vez de `Html.Partial('_SignUp')`.

**Enviar Email**

Para enviar email, o modelo usa [Postal](http://aboutcode.net/postal). No entanto, a caixa Postal é abstraído do restante do código com o `IMailer` da interface, portanto, você pode facilmente substituí-lo em outra implementação. Os modelos de email estão localizados na pasta exibições/Emails. Endereço de email do remetente é especificado no arquivo Web. config, nos `sender.email` chave do **appSettings** seção. Além disso, quando `debug="true"` no Web. config, o aplicativo não requer confirmação de email do usuário, para acelerar o desenvolvimento.

## <a name="github"></a>GitHub

Você também pode encontrar o modelo backbone. js SPA no [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
