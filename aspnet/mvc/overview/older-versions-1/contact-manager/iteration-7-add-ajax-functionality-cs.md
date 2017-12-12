---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: "Iteração #7 – funcionalidade Ajax adicionar (c#) | Microsoft Docs"
author: microsoft
description: "A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: db313d12dfd6a146347f253dc3a1f4a889bee780
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="iteration-7--add-ajax-functionality-c"></a>Iteração #7 – funcionalidade Ajax adicionar (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (c#)

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de entre em contato com toda a partir do início ao fim. O aplicativo Gerenciador de entrar em contato com permite armazenar informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Vamos criar o aplicativo em várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. O objetivo dessa abordagem iteração vários é para que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, criamos o gerente do contato da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básicos: criar, leitura, atualização e exclusão (CRUD).

- Iteração #2 - Verifique o aplicativo parecer adequado. Essa iteração, podemos melhorar a aparência do aplicativo, modificando a página mestra do ASP.NET MVC exibição padrão e em cascata a folha de estilos.

- Iteração #3 - adicionar validação do formulário. Na terceira iteração, podemos adicionar validação de forma básica. Vamos impedir que pessoas enviando um formulário sem concluir os campos obrigatórios do formulário. Além disso, podemos validar endereços de email e números de telefone.

- Iteração #4 - Verifique o aplicativo acoplados de forma flexível. Essa terceira iteração, podemos aproveitar vários padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contato. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, fazemos nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 - Use desenvolvimento controlado por testes. Essa iteração do sexto, adicionamos novas funcionalidades para nosso aplicativo escrevendo testes de unidade primeiro e escrever código os testes de unidade. Essa iteração, adicionamos grupos de contatos.

- Iteração #7 - adicionar funcionalidade Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.

## <a name="this-iteration"></a>Essa iteração

Essa iteração do aplicativo Gerenciador de contato, podemos refatorar nosso aplicativo fazer uso do Ajax. Aproveitando Ajax, fazemos nosso aplicativo mais ágil na resposta. Podemos evitar a renderização de uma página inteira quando precisamos atualizar apenas uma determinada região em uma página.

Podemos será refatorar nossa visualização de índice para que podemos don precisa exibir novamente toda a página sempre que alguém seleciona um novo grupo de contato. Em vez disso, quando alguém clicar em um grupo de contatos, vamos apenas atualizar a lista de contatos e deixe o resto da página sozinho.

Também vamos alterar a maneira como o link de nosso delete funciona. Em vez de exibir uma página de confirmação separado, podemos exibirá uma caixa de diálogo de confirmação de JavaScript. Se você confirmar que você deseja excluir um contato, uma operação HTTP DELETE é executada em relação ao servidor para excluir o registro de contato do banco de dados.

Além disso, podemos aproveitarão jQuery para adicionar efeitos de animação à nossa visualização de índice. Exibiremos uma animação quando a nova lista de contatos está sendo buscada no servidor.

Por fim, vamos dar vantagens do ASP.NET AJAX framework suporte para gerenciar o histórico do navegador. Vamos criar pontos de histórico sempre que podemos executar uma chamada Ajax para atualizar a lista de contatos. Dessa forma, o navegador para trás e frente botões vai funcionar.

## <a name="why-use-ajax"></a>Por que usar Ajax?

Usando Ajax tem muitos benefícios. Primeiro, adicionando funcionalidade Ajax para os resultados de um aplicativo em uma melhor experiência do usuário. De um aplicativo web normal, a página inteira deve ser publicada no servidor cada vez que um usuário executa uma ação. Sempre que você executa uma ação, os bloqueios de navegador e o usuário devem aguardar até que a página inteira é buscada e reexibida.

Isso seria uma experiência inaceitável no caso de um aplicativo de área de trabalho. No entanto, tradicionalmente, estiveram ativos com essa experiência ruim do usuário no caso de um aplicativo web porque sem saber que podemos fazer melhor. Achamos que era uma limitação de aplicativos da web quando, na realidade, é apenas uma limitação de nossa imaginação.

Em um aplicativo Ajax, você não precisa para trazer a experiência do usuário para parar apenas para atualizar uma página. Em vez disso, você pode executar uma solicitação assíncrona em segundo plano para atualizar a página. Você não forçar t para o usuário para aguardar enquanto a parte da página é atualizada.

Tirando proveito do Ajax, você também pode melhorar o desempenho do seu aplicativo. Considere como o aplicativo Gerenciador de contato agora funciona sem funcionalidade Ajax. Quando você clica em um grupo de contatos, toda a exibição do índice deve ser exibida novamente. A lista de contatos e lista de grupos de contato devem ser recuperadas do servidor de banco de dados. Todos esses dados devem ser passados pela conexão do servidor web para navegador da web.

Depois que adicionar funcionalidade Ajax para nosso aplicativo, no entanto, podemos evitar novamente toda a página quando um usuário clica em um grupo de contatos. Não precisamos mais obter os grupos de contato do banco de dados. Podemos também don precisa enviar por push toda a exibição índice pela conexão. Aproveitando Ajax, reduzir a quantidade de trabalho que deve executar nosso servidor de banco de dados e reduzir a quantidade de tráfego de rede, exigido pelo nosso aplicativo.

## <a name="don-t-be-afraid-of-ajax"></a>Anibal t ser medo de Ajax

Alguns desenvolvedores evitar usando Ajax porque eles se preocupar sobre navegadores de nível inferior. Eles querem Certifique-se de que seus aplicativos web ainda funcionará quando acessado por um navegador que não oferece suporte a JavaScript. Como Ajax depende de JavaScript, alguns desenvolvedores evitar usando Ajax.

No entanto, se tiver cuidado como implementar o Ajax, em seguida, você pode criar aplicativos que funcionam com navegadores de nível superior e de nível inferior. Nosso aplicativo Contact Manager funciona apenas com navegadores que oferecem suporte a JavaScript e navegadores que não.

Se você usar o aplicativo Gerenciador de contato com um navegador que ofereça suporte ao JavaScript, em seguida, você terá uma melhor experiência do usuário. Por exemplo, quando você clica em um grupo de contatos, apenas a região da página que exibe os contatos será atualizada.

Se, por outro lado, use o aplicativo Gerenciador de contato com um navegador que não oferece suporte a JavaScript (ou que tenha o JavaScript desabilitado), em seguida, você terá uma experiência de usuário um pouco menos desejável. Por exemplo, quando você clica em um grupo de contatos, toda a exibição do índice deve ser lançada ao navegador para exibir a lista de contatos de correspondência.

## <a name="adding-the-required-javascript-files"></a>Adicionar os arquivos JavaScript necessário

Precisaremos usar três arquivos JavaScript para adicionar funcionalidade Ajax para nosso aplicativo. Três desses arquivos são incluídos na pasta de Scripts de um novo aplicativo ASP.NET MVC.

Se você planeja usar Ajax em várias páginas em seu aplicativo, em seguida, faz sentido para incluir os arquivos JavaScript necessários em sua página do aplicativo s modo de exibição mestre. Dessa forma, os JavaScript arquivos serão incluídos em todas as páginas em seu aplicativo automaticamente.

Adicione o seguinte JavaScript inclui dentro de &lt;head&gt; marca de sua página de exibição mestre:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>A exibição do índice para usar Ajax de refatoração

Permitir que o s iniciar modificando nossa visualização de índice, de modo que apenas clicando em um grupo de contatos atualiza a região da exibição que mostra os contatos. A caixa vermelha na Figura 1 contém a região que queremos atualizar.


[![Atualizar apenas os contatos](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Figura 01**: atualizar apenas os contatos ([clique para exibir a imagem em tamanho normal](iteration-7-add-ajax-functionality-cs/_static/image2.png))


A primeira etapa é separar a parte do modo de exibição que desejamos para atualização de forma assíncrona em um parcial separado (controle de usuário de exibição). A seção do modo de exibição de índice que exibe a tabela de contatos foi movida para o parcial na listagem 1.

**Listando 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Observe que o parcial na listagem 1 tem um modelo diferente do que o modo de exibição do índice. O *Inherits* atributo o &lt;% @ Page %&gt; diretiva especifica que o parcial herda o ViewUserControl&lt;grupo&gt; classe.

A exibição índice atualizada está contida na listagem 2.

**A listagem 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Há duas coisas que você deve observar sobre o modo de exibição atualizado na listagem 2. Primeiro, observe que todo o conteúdo movido para o parcial é substituído por uma chamada para Html.RenderPartial(). O método Html.RenderPartial() é chamado quando o modo de exibição do índice é solicitado pela primeira vez para exibir o conjunto inicial de contatos.

Segundo, observe que o Html.ActionLink() usada para exibir grupos de contato foi substituído por um Ajax.ActionLink(). O Ajax.ActionLink() é chamado com os seguintes parâmetros:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

O primeiro parâmetro representa o texto a ser exibido para o link, o segundo parâmetro representa os valores de rota e o terceiro parâmetro representa as opções de Ajax. Nesse caso, usamos a opção UpdateTargetId Ajax para apontar para o HTML &lt;div&gt; marca que queremos atualizar após a solicitação Ajax. Queremos atualizar o &lt;div&gt; marca com a nova lista de contatos.

O método Index () atualizado do controlador contato está contido na listagem 3.

**A listagem 3 - Controllers\ContactController.cs (método de índice)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

A ação Index () atualizada condicionalmente retorna uma das duas coisas. Se a ação Index () é invocada por uma solicitação Ajax o controlador retorna um parcial. Caso contrário, a ação Index () retorna uma exibição inteira.

Observe que a ação Index () não precisa retornar todos os dados quando invocado por uma solicitação Ajax. No contexto de uma solicitação normal, a ação de índice retorna uma lista de todos os grupos de contato e o grupo selecionado de contato. No contexto de uma solicitação Ajax, a ação Index () retorna somente o grupo selecionado. AJAX significa menos trabalho em seu servidor de banco de dados.

Nossa visualização índice modificada funciona no caso de navegadores de nível superior e de nível inferior. Se você clicar em um grupo de contatos e seu navegador oferece suporte a JavaScript, é atualizado apenas a região da exibição que contém a lista de contatos. Se, por outro lado, o seu navegador não oferece suporte a JavaScript, toda a exibição é atualizada.


Nossa visualização atualizada do índice tem um problema. Quando você clicar em um grupo de contatos, o grupo selecionado não é realçado. Como a lista de grupos é exibida fora a região que é atualizada durante uma solicitação Ajax, o grupo correto não obter realçado. Corrigiremos esse problema na próxima seção.


## <a name="adding-jquery-animation-effects"></a>Adicionar efeitos de animação jQuery

Normalmente, quando você clica em um link em uma página da web, você pode usar a barra de progresso do navegador para detectar se o navegador está buscando ativamente o conteúdo atualizado ou não. Ao executar uma solicitação Ajax, por outro lado, a barra de progresso do navegador não mostra nenhum progresso. Isso pode tornar os usuários preocupar. Como saber se o navegador congelou?

Há várias maneiras que você pode indicar a um usuário que o trabalho está sendo executado durante a execução de uma solicitação Ajax. Uma abordagem é exibir uma animação simples. Por exemplo, você pode fazer o fade-out de uma região quando uma solicitação Ajax começa e desaparecer na região quando a solicitação é concluída.

Vamos usar a biblioteca jQuery incluído com a estrutura do Microsoft ASP.NET MVC, para criar os efeitos de animação. A exibição índice atualizada está contida na listagem 4.

**A listagem 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Observe que o modo de exibição atualizado do índice contém três novas funções de JavaScript. As primeiras duas funções usam jQuery desaparecer e desaparecer na lista de contatos, quando você clicar em um novo grupo de contato. A terceira função exibe uma mensagem de erro quando um resultados da solicitação Ajax em um erro (por exemplo, tempo limite de rede).

A primeira função também cuida de realce o grupo selecionado. Uma classe = atributo selecionado é adicionado ao elemento pai (o elemento LI) do elemento clicado. Novamente, o jQuery facilita a seleção do elemento certo e adicionar a classe CSS.

Esses scripts estão vinculados para os links de grupo com a Ajuda do parâmetro Ajax.ActionLink() AjaxOptions. A chamada do método Ajax.ActionLink() atualizada tem esta aparência:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Adicionando suporte de histórico do navegador

Normalmente, quando você clicar em um link para uma página de atualização, o histórico do navegador é atualizado. Dessa forma, você pode clicar do botão Voltar do navegador para voltar no tempo para o estado anterior da página. Por exemplo, se você clique no grupo de contato amigos e, em seguida, clique no grupo de contato de negócios, você pode clique o botão Voltar do navegador para navegar de volta para o estado da página quando o grupo de contato amigos foi selecionado.

Infelizmente, executar uma solicitação Ajax não atualizar o navegador histórico automaticamente. Se você clicar em um grupo de contatos, e é recuperar a lista de contatos de correspondência com uma solicitação Ajax, o histórico do navegador não é atualizado. Você não pode usar o botão Voltar do navegador para navegar de volta para um grupo de contatos depois de selecionar um novo grupo de contato.

Se desejar que os usuários possam usar o navegador novamente botão após a execução de solicitações do Ajax, em seguida, você precisa executar um pouco mais trabalho. Você precisa tirar proveito da funcionalidade de gerenciamento do histórico de navegador criado no ASP.NET AJAX Framework.

Histórico de navegação do ASP.NET AJAX, você precisa fazer três coisas:

1. Habilite o histórico do navegador, definindo a propriedade enableBrowserHistory como true.
2. Salve pontos de histórico quando muda o estado de um modo de exibição, chamando o método addHistoryPoint().
3. Reconstrua o estado da exibição quando o evento de navegação é gerado.

A exibição índice atualizada está contida na listagem 5.

**Listagem 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

Na listagem 5, o histórico do navegador é habilitado na função pageInit(). A função pageInit() também é usada para definir um manipulador de eventos para o evento de navegação. O evento de navegação é gerado sempre que o navegador para frente ou o botão Voltar faz com que o estado da página para alterar.

O método beginContactList() é chamado quando você clica em um grupo de contatos. Esse método cria um novo ponto de histórico, chamando o método addHistoryPoint(). A id do grupo do contato clicado é adicionada ao histórico.

A id do grupo é recuperada de um atributo expando no link de contato de grupo. O link é processado com a seguinte chamada a Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

O último parâmetro passado para o Ajax.ActionLink() adiciona um atributo expando denominado groupid o link (minúsculas para compatibilidade XHTML).

Quando um usuário pressiona o botão encaminhar ou voltar do navegador, o evento de navegação é gerado e o método navigate() é chamado. Este método atualizará os contatos exibidos na página para corresponder o estado da página que corresponde ao passado para o método navigate do ponto de histórico do navegador.

## <a name="performing-ajax-deletes"></a>Executar Ajax exclui

No momento, para excluir um contato, é necessário clicar no link excluir e, em seguida, clique no botão de exclusão exibido na página de confirmação de exclusão (consulte a Figura 2). Isso parece uma quantidade de solicitações de página para fazer algo simples, como a exclusão de um registro de banco de dados.


[![A página de confirmação de exclusão](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Figura 02**: A página de confirmação de exclusão ([clique para exibir a imagem em tamanho normal](iteration-7-add-ajax-functionality-cs/_static/image4.png))


É tentador para ignorar a página de confirmação de exclusão e excluir um contato diretamente da exibição do índice. Você deve evitar essa tentativa porque essa abordagem abre o seu aplicativo para falhas de segurança. Em geral, Anibal t deseja executar uma operação HTTP GET ao invocar uma ação que modifica o estado do seu aplicativo web. Ao executar uma exclusão, você deseja executar um HTTP POST, ou melhor ainda, uma operação HTTP DELETE.

O link excluir está contido no ContactList parcial. Uma versão atualizada do ContactList parcial está contida na listagem 6.

**Listando 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

O excluirá o link é processado com a chamada ao método Ajax.ImageActionLink() a seguir:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> O Ajax.ImageActionLink() não é parte da estrutura ASP.NET MVC padrão. O Ajax.ImageActionLink() é um métodos auxiliares personalizados incluído no projeto do gerente do contato.


O parâmetro AjaxOptions tem duas propriedades. Primeiro, a propriedade confirmar é usada para exibir uma caixa de diálogo de confirmação de JavaScript do pop-up. Em segundo lugar, a propriedade HttpMethod é usada para executar uma operação HTTP DELETE.

Listar 7 contém uma nova ação de AjaxDelete() que foi adicionada ao controlador de contato.

**Listando 7 - Controllers\ContactController.cs (AjaxDelete)**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

A ação AjaxDelete() é decorada com um atributo AcceptVerbs. Esse atributo impede que a ação seja chamado exceto por qualquer operação de HTTP que não seja uma operação HTTP DELETE. Em particular, você não pode chamar a ação com um HTTP GET.

Depois de excluir o registro do banco de dados, você precisa exibir a lista atualizada de contatos que não contém o registro excluído. O método AjaxDelete() retorna o ContactList parcial e a lista atualizada de contatos.

## <a name="summary"></a>Resumo

Essa iteração, adicionamos funcionalidade Ajax para nosso aplicativo Gerenciador de contato. Usamos Ajax para melhorar a capacidade de resposta e o desempenho do nosso aplicativo.

Primeiro, podemos refatorar o modo de exibição do índice para que clicar em um grupo de contato não atualiza a exibição inteira. Em vez disso, clicar em um grupo de contatos só atualiza a lista de contatos.

Em seguida, usamos os efeitos de animação jQuery desaparecer e desaparecer na lista de contatos. Adicionando animação a um aplicativo Ajax pode ser usado para fornecer aos usuários do aplicativo com o equivalente de uma barra de progresso do navegador.

Também adicionamos suporte de histórico do navegador para nosso aplicativo Ajax. É permitido que os usuários clique novamente no navegador e encaminhar os botões para alterar o estado da exibição de índice.

Por fim, criamos um link de exclusão que dá suporte a operações HTTP DELETE. Executando exclusões de Ajax, permitimos que usuários excluir registros de banco de dados sem exigir que o usuário solicitar uma página de confirmação de exclusão adicionais.

>[!div class="step-by-step"]
[Anterior](iteration-6-use-test-driven-development-cs.md)
[Próximo](iteration-1-create-the-application-vb.md)
