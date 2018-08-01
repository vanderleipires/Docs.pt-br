---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Iteração #7 – adicionar funcionalidade do Ajax (VB) | Microsoft Docs'
author: microsoft
description: A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: b39a251acc96a78245238da5b82365958223e4c7
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396173"
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Iteração #7 – adicionar funcionalidade do Ajax (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (VB)
  

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de contatos inteiro do início ao fim. O aplicativo Gerenciador de contatos permite que você armazene informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Criamos o aplicativo ao longo de várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. A meta dessa abordagem de iteração vários é que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, podemos criar o Gerenciador de contatos da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básico: criar, ler, atualizar e excluir (CRUD).

- Iteração #2 - tornar o aplicativo interessante. Nesta iteração, podemos melhorar a aparência do aplicativo modificando a página mestra do ASP.NET MVC exibição padrão e em cascata de folha de estilos.

- Iteração #3 - adicionar validação de formulário. Na terceira iteração, podemos adicionar validação de formulário básico. Podemos impedir que pessoas enviando um formulário sem preencher os campos obrigatórios do formulário. Podemos também validar endereços de email e números de telefone.

- Iteração #4 – tornar o aplicativo fracamente acoplado. Nesta quarta iteração, podemos tirar proveito dos diversos padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contatos. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, podemos tornar nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular a nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 – usar desenvolvimento controlado por teste. Essa iteração sexta, adicionamos novas funcionalidades ao nosso aplicativo escrevendo testes de unidade pela primeira vez e escrever código contra os testes de unidade. Essa iteração, adicionamos os grupos de contatos.

- Iteração #7 - adicionar a funcionalidade do Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.

## <a name="this-iteration"></a>Essa iteração

Essa iteração do aplicativo Contact Manager, podemos refatorar nosso aplicativo para fazer uso do Ajax. Ao tirar proveito do Ajax, podemos fazer nosso aplicativo mais ágil na resposta. Podemos pode evitar a renderização de uma página inteira quando precisamos atualizar somente uma determinada região em uma página.

Podemos refatorar nosso modo de exibição de índice, de modo que podemos don precisa exibir novamente a página inteira sempre que alguém seleciona um novo grupo de contato. Em vez disso, quando alguém clica em um grupo de contatos, vou atualizar a lista de contatos e deixe o restante da página sozinho.

Também vamos alterar a maneira de nossa excluir link funciona. Em vez de exibir uma página de confirmação separado, vamos exibir uma caixa de diálogo de confirmação de JavaScript. Se você confirmar que você deseja excluir um contato, uma operação HTTP DELETE é executada em relação ao servidor para excluir o registro de contato do banco de dados.

Além disso, podemos aproveitará do jQuery para adicionar efeitos de animação à nossa exibição índice. Vamos exibir uma animação quando a nova lista de contatos é que está sendo procurada do servidor.

Por fim, vamos dar vantagem do suporte do ASP.NET AJAX framework para gerenciar o histórico do navegador. Vamos criar pontos de histórico sempre que executamos uma chamada Ajax para atualizar a lista de contatos. Dessa forma, o navegador com versões anteriores e posteriores botões irá funcionar.

## <a name="why-use-ajax"></a>Por que usar o Ajax?

Usando Ajax traz muitos benefícios. Em primeiro lugar, adicionando funcionalidade Ajax a resultados um aplicativo em uma melhor experiência de usuário. De um aplicativo web normal, a página inteira deve ser publicada no servidor cada vez que um usuário executa uma ação. Sempre que você executar uma ação, os bloqueios de navegador e o usuário devem aguardar até que a página inteira seja buscada e exibida novamente.

Isso seria uma experiência inaceitável no caso de um aplicativo da área de trabalho. No entanto, tradicionalmente, vivemos com essa experiência de usuário incorreta no caso de um aplicativo web porque nós não sabia que podemos fazer melhor. Achamos que era uma limitação de aplicativos da web quando, na realidade, ele era apenas uma limitação de nossa imaginação.

Em um aplicativo Ajax, você não precisa para trazer a experiência do usuário para interromper apenas para atualizar uma página. Em vez disso, você pode executar uma solicitação assíncrona em segundo plano para atualizar a página. Você não forçar t, o usuário esperar enquanto a parte da página é atualizada.

Ao tirar proveito do Ajax, você também pode melhorar o desempenho do seu aplicativo. Considere como o aplicativo Gerenciador de contatos agora funciona sem a funcionalidade do Ajax. Quando você clica em um grupo de contatos, toda a exibição de índice deve ser exibida novamente. A lista de contatos e lista de grupos de contato devem ser recuperadas do servidor de banco de dados. Todos esses dados devem ser passados pela conexão do servidor web para o navegador da web.

Depois de adicionar funcionalidade do Ajax ao nosso aplicativo, no entanto, podemos evitar exibir novamente a página inteira quando um usuário clica em um grupo de contatos. Não precisamos mais pegar os grupos de contato do banco de dados. Podemos também don precisa enviar por push toda a exibição de índice na transmissão. Ao tirar proveito do Ajax, podemos reduzir a quantidade de trabalho que nosso servidor de banco de dados deve executar e podemos reduzir a quantidade de tráfego de rede exigida pelo nosso aplicativo.

## <a name="don-t-be-afraid-of-ajax"></a>Don t ser medo do Ajax

Alguns desenvolvedores Evite usar Ajax, pois eles se preocupar sobre navegadores de nível inferior. Eles querem garantir que seus aplicativos da web ainda funcionará quando acessado por um navegador que não oferece suporte a JavaScript. Como Ajax depende de JavaScript, alguns desenvolvedores evitar usando Ajax.

No entanto, se você for cuidadoso sobre como implementar o Ajax, em seguida, você pode criar aplicativos que funcionam com navegadores uplevel e de nível inferior. Nosso aplicativo Contact Manager funcionará com navegadores que oferecem suporte a JavaScript e navegadores que não.

Se você usar o aplicativo Gerenciador de contatos com um navegador que dê suporte a JavaScript, em seguida, você terá uma melhor experiência do usuário. Por exemplo, quando você clica em um grupo de contatos, somente a região da página que exibe os contatos será atualizada.

Se, por outro lado, usar o aplicativo Gerenciador de contatos com um navegador que não oferece suporte a JavaScript (ou que tenha o JavaScript desabilitado), em seguida, você terá uma experiência do usuário ligeiramente menos desejável. Por exemplo, quando você clica em um grupo de contatos, toda a exibição de índice deve ser lançada ao navegador para exibir a lista correspondente de contatos.

## <a name="adding-the-required-javascript-files"></a>Adicionar os arquivos JavaScript necessário

Precisaremos usar três arquivos de JavaScript para adicionar funcionalidade do Ajax ao nosso aplicativo. Todos os três desses arquivos são incluídos na pasta de Scripts de um novo aplicativo ASP.NET MVC.

Se você planeja usar o Ajax em várias páginas em seu aplicativo, em seguida, faz sentido para incluir os arquivos necessários do JavaScript em sua página do aplicativo s modo de exibição mestre. Dessa forma, os JavaScript arquivos serão incluídos em todas as páginas em seu aplicativo automaticamente.

Adicione o seguinte JavaScript inclui dentro de &lt;head&gt; marca da sua página de exibição mestre:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>A exibição de índice para usar o Ajax de refatoração

Permitir que o s comece modificando nossa exibição de índice, de modo que apenas clicando em um grupo de contatos atualiza a região da exibição que exibe os contatos. A caixa vermelha na Figura 1 contém a região que queremos atualizar.


[![Atualizar somente os contatos](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Figura 01**: atualizar somente os contatos ([clique para exibir a imagem em tamanho normal](iteration-7-add-ajax-functionality-vb/_static/image2.png))


A primeira etapa é separar a parte do modo de exibição que desejamos atualize de forma assíncrona em um parcial separado (controle de usuário do modo de exibição). A seção da exibição índice que exibe a tabela de contatos foi movida para parcial na listagem 1.

**Listagem 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Observe que a parcial na listagem 1 tem um modelo diferente do que o modo de exibição de índice. O *Inherits* atributo na &lt;% @ Page %&gt; diretiva especifica que a parcial herda o ViewUserControl&lt;grupo&gt; classe.

O modo de exibição de índice atualizado está contido na listagem 2.

**Listagem 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Há duas coisas que você deve observar sobre o modo de exibição atualizado na listagem 2. Primeiro, observe que todo o conteúdo movido para parcial é substituído por uma chamada para Html.RenderPartial(). O método Html.RenderPartial() é chamado quando o modo de exibição de índice é solicitado pela primeira vez para exibir o conjunto inicial de contatos.

Em segundo lugar, observe que o Html.ActionLink() usado para exibir grupos de contatos foi substituído por um Ajax.ActionLink(). O Ajax.ActionLink() é chamado com os seguintes parâmetros:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

O primeiro parâmetro representa o texto a ser exibida para o link, o segundo parâmetro representa os valores de rota e o terceiro parâmetro representa as opções de Ajax. Nesse caso, usamos a opção UpdateTargetId Ajax para apontar para o HTML &lt;div&gt; marca que queremos atualizar após a solicitação Ajax é concluída. Queremos atualizar o &lt;div&gt; marca com a nova lista de contatos.

O método Index () atualizado do controlador contato está contido na listagem 3.

**Listagem 3 - Controllers\ContactController.vb (método de índice)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

A ação atualizada de Index () retorna condicionalmente uma das duas coisas. Se a ação Index () é invocada por uma solicitação Ajax, em seguida, o controlador retorna um parcial. Caso contrário, a ação de Index () retorna uma exibição inteira.

Observe que a ação Index () não precisa retornar a quantidade de dados quando invocado por uma solicitação Ajax. No contexto de uma solicitação normal, a ação de índice retorna uma lista de todos os grupos de contato e o grupo de contato selecionado. No contexto de uma solicitação Ajax, a ação de Index () retorna somente o grupo selecionado. AJAX significa menos trabalho em seu servidor de banco de dados.

Nossa exibição índice modificado funciona no caso de navegadores uplevel e de nível inferior. Se você clicar em um grupo de contatos e o navegador dá suporte a JavaScript, somente a região da exibição que contém a lista de contatos é atualizada. Se, por outro lado, o seu navegador não dá suporte a JavaScript, toda a exibição é atualizada.


Nossa exibição atualizada do índice tem um problema. Quando você clica em um grupo de contatos, o grupo selecionado não é realçado. Como a lista de grupos é exibida fora da região que é atualizada durante uma solicitação Ajax, o grupo correto não obter realçado. Corrigiremos esse problema na próxima seção.


## <a name="adding-jquery-animation-effects"></a>A adição de efeitos de animação do jQuery

Normalmente, quando você clica em um link em uma página da web, você pode usar a barra de progresso do navegador para detectar se o navegador está ativamente buscando o conteúdo atualizado. Ao executar uma solicitação Ajax, por outro lado, a barra de progresso do navegador não mostra nenhum progresso. Isso pode tornar os usuários preocupados. Como saber se o navegador tem congelado?

Há várias maneiras que você pode indicar a um usuário que o trabalho está sendo executado durante a execução de uma solicitação Ajax. Uma abordagem é exibir uma animação simples. Por exemplo, você pode esmaecer uma região quando uma solicitação Ajax começa e aplicar fade-in da região quando a solicitação é concluída.

Vamos usar a biblioteca jQuery, que é incluída com a estrutura Microsoft ASP.NET MVC, para criar os efeitos de animação. O modo de exibição de índice atualizado está contido na listagem 4.

**Listagem 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Observe que o modo de exibição atualizado do índice contém três novas funções de JavaScript. As duas primeiras funções usarem jQuery para esmaecer e aplicar fade-in a lista de contatos quando você clica em um novo grupo de contato. A terceira função exibe uma mensagem de erro quando uma solicitação do Ajax resulta em um erro (por exemplo, tempo limite de rede).

A primeira função também se encarrega de realce o grupo selecionado. Uma classe = atributo selecionado é adicionado ao elemento pai (o elemento LI) do elemento clicado. Novamente, o jQuery facilita a seleção do elemento certo e adicione a classe CSS.

Esses scripts estão vinculados aos links de grupo com a Ajuda do parâmetro Ajax.ActionLink() AjaxOptions. A chamada de método Ajax.ActionLink() atualizada tem esta aparência:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Adicionar suporte de histórico do navegador

Normalmente, quando você clica em um link para uma página de atualização, o histórico do navegador é atualizado. Dessa forma, você pode clicar o botão Voltar do navegador para voltar no tempo para o estado anterior da página. Por exemplo, se você clique no grupo de contato de amigos e, em seguida, clique no grupo de contato de negócios, você pode clicar o botão Voltar do navegador para navegar de volta para o estado da página quando o grupo de contato de amigos foi selecionado.

Infelizmente, executar uma solicitação Ajax não atualiza o histórico do navegador automaticamente. Se você clicar em um grupo de contatos e a lista de contatos correspondentes é recuperada com uma solicitação Ajax, o histórico do navegador não é atualizado. Você não pode usar o botão Voltar do navegador para navegar de volta para um grupo de contatos depois de selecionar um novo grupo de contato.

Se você quiser que os usuários possam usar o navegador de volta botão depois de executar as solicitações Ajax, em seguida, você precisa executar um pouco mais trabalho. Você precisa aproveitar a funcionalidade de gerenciamento de histórico do navegador criada na estrutura ASP.NET AJAX.

Histórico do navegador ASP.NET AJAX, você precisa fazer três coisas:

1. Habilite o histórico do navegador, definindo a propriedade enableBrowserHistory como true.
2. Salve pontos de histórico quando muda o estado de uma exibição chamando o método addHistoryPoint().
3. Reconstrua o estado da exibição quando o evento navigate é gerado.

O modo de exibição de índice atualizado está contido na listagem 5.

**Listagem 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

Na listagem 5, o histórico do navegador é habilitado na função pageInit(). A função pageInit() também é usada para definir um manipulador de eventos para o evento navigate. O evento navigate é gerado sempre que o botão Voltar ou Avançar do navegador faz com que o estado da página para alterar.

O método beginContactList() é chamado quando você clica em um grupo de contatos. Esse método cria um novo ponto de histórico, chamando o método addHistoryPoint(). A id do grupo de contatos clicado é adicionada ao histórico.

A id do grupo é recuperada de um atributo expando no link contato do grupo. O link é renderizado com a seguinte chamada para Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

O último parâmetro passado para o Ajax.ActionLink() adiciona um atributo expando chamado groupid o link (letras minúsculas para compatibilidade XHTML).

Quando um usuário pressiona o botão Avançar ou voltar do navegador, o evento navigate é gerado e o método navigate() é chamado. Esse método atualiza os contatos exibidos na página para coincidir com o estado da página que corresponde ao passado para o método navigate do ponto de histórico do navegador.

## <a name="performing-ajax-deletes"></a>Executar o Ajax exclui

No momento, para excluir um contato, você precisa clicar no link excluir e, em seguida, clique no botão de exclusão exibido na página de confirmação de exclusão (veja a Figura 2). Isso parece muito de solicitações de página para fazer algo simples, como a exclusão de um registro de banco de dados.


[![A página de confirmação de exclusão](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Figura 02**: A página de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-7-add-ajax-functionality-vb/_static/image4.png))


É tentador para ignorar a página de confirmação de exclusão e excluir um contato diretamente da exibição índice. Você deve evitar essa tentação porque essa abordagem abre seu aplicativo para brechas de segurança. Em geral, don t deseja executar uma operação HTTP GET ao invocar uma ação que modifica o estado do seu aplicativo web. Quando executar um delete, que você deseja executar um HTTP POST, ou melhor ainda, uma operação HTTP DELETE.

O link excluir está contido no ContactList parcial. Uma versão atualizada do ContactList parcial está contida na listagem 6.

**Listagem 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

O link de exclusão é renderizado com a seguinte chamada para o método Ajax.ImageActionLink():

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> O Ajax.ImageActionLink() não é uma parte padrão da estrutura MVC do ASP.NET. O Ajax.ImageActionLink() é um método auxiliar personalizado incluído no projeto do Gerenciador de contatos.


O parâmetro AjaxOptions tem duas propriedades. Em primeiro lugar, a propriedade confirmar é usada para exibir uma caixa de diálogo de confirmação de JavaScript do pop-up. Em segundo lugar, a propriedade HttpMethod é usada para executar uma operação HTTP DELETE.

Listagem 7 contém uma nova ação de AjaxDelete() que foi adicionada ao controlador de contato.

**Listagem 7 - Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

A ação AjaxDelete() é decorada com um atributo AcceptVerbs. Esse atributo impede que a ação seja chamado, exceto por qualquer operação de HTTP diferente de uma operação HTTP DELETE. Em particular, você não pode chamar a ação com um HTTP GET.

Depois de excluir o registro de banco de dados, você precisa exibir a lista atualizada de contatos que não contém o registro excluído. O método AjaxDelete() retorna o ContactList parcial e a lista atualizada de contatos.

## <a name="summary"></a>Resumo

Essa iteração, adicionamos a funcionalidade do Ajax ao nosso aplicativo Contact Manager. Nós usamos o Ajax para melhorar a capacidade de resposta e o desempenho do nosso aplicativo.

Primeiro, podemos refatorar a exibição índice de modo que clicar em um grupo de contato não atualiza a exibição inteira. Em vez disso, clicar em um grupo de contato somente atualiza a lista de contatos.

Em seguida, usamos efeitos de animação do jQuery para esmaecer e aplicar fade-in a lista de contatos. Adicionando animação a um aplicativo Ajax pode ser usado para fornecer aos usuários do aplicativo com o equivalente de uma barra de progresso do navegador.

Também adicionamos suporte ao histórico de navegador para o nosso aplicativo Ajax. Habilitamos a usuários, clique novamente no navegador e encaminhar os botões para alterar o estado da exibição índice.

Por fim, criamos um link de exclusão que dá suporte a operações HTTP DELETE. Ao realizar exclusões de Ajax, podemos permitir que os usuários excluir registros de banco de dados sem exigir que o usuário solicitar uma página de confirmação de exclusão adicionais.

> [!div class="step-by-step"]
> [Anterior](iteration-6-use-test-driven-development-vb.md)
