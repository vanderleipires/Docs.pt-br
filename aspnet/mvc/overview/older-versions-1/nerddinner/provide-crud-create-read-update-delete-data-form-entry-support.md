---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fornecer CRUD (criar, ler, atualizar e excluir) suporte de entrada de formulário de dados | Microsoft Docs
author: microsoft
description: Etapa 5 mostra como tirar nossa classe DinnersController ainda mais, habilite o suporte para editar, criar e excluir jantares com ele também.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bd906282db5c620476966ffbe09cecb5ade66ee4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876742"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fornecer CRUD (criar, ler, atualizar e excluir) suporte de entrada de formulário de dados
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 5 da livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.
> 
> Etapa 5 mostra como tirar nossa classe DinnersController ainda mais, habilite o suporte para editar, criar e excluir jantares com ele também.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner etapa 5: Criar, atualizar e excluir os cenários de formulário

Nós introduzidos controladores e exibições e coberto como usá-los para implementar uma experiência de listagem/detalhes para jantares no site. Nossa próxima etapa será fazer nossa classe DinnersController ainda mais e habilitar o suporte para editar, criar e excluir jantares com ele também.

### <a name="urls-handled-by-dinnerscontroller"></a>URLs tratadas pelo DinnersController

Adicionamos anteriormente métodos de ação para DinnersController que implementou o suporte para duas URLs: */Dinners* e */Dinners/detalhes / [id]*.

| **URL** | **VERBO** | **Finalidade** |
| --- | --- | --- |
| */Dinners/* | OBTER | Exiba uma lista HTML de jantares futuros. |
| */Dinners/Details/[id]* | OBTER | Exibir detalhes sobre uma refeição específico. |

Agora vamos adicionar métodos de ação para implementar as três URLs adicionais: <em>/Dinners/Editar / [id], jantares/criar,</em>e<em>/Dinners/excluir / [id]</em>. Essas URLs serão habilitar o suporte para edição jantares existentes, criando novos jantares e excluindo jantares.

Podemos oferecerá suporte a interações de verbo HTTP GET e POST HTTP com essas novas URLs. Solicitações HTTP GET para essas URLs exibirá o modo de exibição HTML inicial de dados (um formulário preenchido com os dados de uma refeição no caso de "edit", um formulário em branco no caso de "criar" e uma tela de confirmação de exclusão no caso de "excluir"). Solicitações de HTTP POST para essas URLs serão salvar/atualizar/excluir os dados de uma refeição em nosso DinnerRepository (e daí no banco de dados).

| **URL** | **VERBO** | **Finalidade** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | OBTER | Exiba um formulário HTML editável preenchido com dados de uma refeição. |
| POSTAR | Salve as alterações de formato para uma refeição específica para o banco de dados. |
| */Dinners/Create* | OBTER | Exiba um formulário HTML vazio que permite aos usuários definir jantares novo. |
| POSTAR | Criar uma novo refeição e salvá-lo no banco de dados. |
| */Dinners/Delete/[id]* | OBTER | Exibir a tela de confirmação de exclusão. |
| POSTAR | Exclui uma refeição especificada do banco de dados. |

### <a name="edit-support"></a>Editar o suporte

Vamos implementar o cenário "edit".

#### <a name="the-http-get-edit-action-method"></a>O método de ação de edição de HTTP GET

Vamos começar por implementar o comportamento de "GET" de HTTP de nosso método de ação de edição. Esse método será chamado quando o */Dinners/Editar / [id]* URL é solicitada. Nossa implementação se parecerá com:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

O código acima usa o DinnerRepository para recuperar um objeto de uma refeição. Ela então processa um modelo de exibição usando o objeto refeição. Porque estamos ainda não tiver passado explicitamente um nome de modelo para o *View()* método auxiliar, ele usará o caminho padrão baseado em convenção para resolver o modelo de exibição: /Views/Dinners/Edit.aspx.

Agora vamos criar esse modelo de exibição. Faremos isso clicando duas vezes no método editar e selecionar o comando de menu de contexto "Adicionar modo de exibição":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

A caixa de diálogo "Adicionar modo de exibição" será indicamos que está passando um objeto de uma refeição ao nosso modelo de exibição como seu modelo e optar por scaffold automaticamente um modelo de "Editar":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Quando, clique no botão "Adicionar", o Visual Studio adiciona um novo arquivo de modelo de exibição "Edit.aspx" para que possamos dentro do diretório "\Views\Dinners". Ele também abrirá o novo modelo de exibição de "Edit.aspx" no editor de código-– preenchido com uma "Editar" scaffold implementação inicial como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Vamos fazer algumas alterações para o padrão "Editar" scaffold gerado e atualizar o modelo de exibição de edição para que o conteúdo (o que elimina algumas das propriedades que não queremos expor):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Quando executamos a solicitação e o aplicativo de *"/ Editar/jantares/1"* URL veremos a seguinte página:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

A marcação HTML gerada pelo nosso ver a aparência abaixo. É HTML padrão – com um &lt;formulário&gt; elemento que realiza um HTTP POST para o */Dinners/Edit/1* URL quando o "Salvar" &lt;tipo de entrada = "submit" /&gt; botão é pressionado. Uma HTML &lt;tipo de entrada = "texto" /&gt; elemento tiver sido a saída para cada propriedade editável:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() e Html.TextBox() auxiliar Html métodos

Nosso modelo de exibição "Edit.aspx" está usando vários métodos de "Auxiliar Html": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() e Html.ValidationMessage(). Além de gerar uma marcação HTML para nós, esses métodos auxiliares fornecem tratamento de erros internos e validação de suporte.

##### <a name="htmlbeginform-helper-method"></a>Método auxiliar de Html.BeginForm()

O método auxiliar Html.BeginForm() é qual saída HTML &lt;formulário&gt; elemento em nossa marcação. Nosso modelo de exibição de Edit.aspx, você observará que estamos aplicando um c# "using" instrução ao usar esse método. A chave aberta indica o início do &lt;formulário&gt; conteúdo e a chave de fechamento é o que indica o fim do &lt;/formulário&gt; elemento:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Como alternativa, se você encontrar a instrução "using" abordagem artificial para um cenário como esse, você pode usar uma combinação de Html.BeginForm() e Html.EndForm() (que faz a mesma coisa):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Chamar Html.BeginForm() sem nenhum parâmetro fará com que a saída de um elemento de formulário que faz um HTTP POST para a URL da solicitação atual. Isto é porque nossa exibição de edição gera um *&lt;da ação de formulário = "/ Editar/jantares/1" método = "post"&gt;* elemento. Pode ter como alternativa passamos parâmetros explícitos para Html.BeginForm() se quiséssemos post para uma URL diferente.

##### <a name="htmltextbox-helper-method"></a>Método auxiliar de Html.TextBox()

Nossa visão Edit.aspx usa o método auxiliar Html.TextBox() para saída &lt;tipo de entrada = "texto" /&gt; elementos:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

O método Html.TextBox() acima usa um único parâmetro – o que está sendo usado para especificar os atributos de nome/id do &lt;tipo de entrada = "text" /&gt; saída, bem como a propriedade de modelo para preencher o valor da caixa de texto do elemento. Por exemplo, o objeto de uma refeição são passados para o modo de exibição de edição tinha um valor da propriedade "Title" de ".NET futuros", e então nosso método Html.TextBox("Title") chamadas de saída: *&lt;id de entrada = "Title" nome = "Title" type = "text" value = ".NET futuros" /&gt;*.

Como alternativa, podemos usar o primeiro parâmetro Html.TextBox() para especificar o nome/id do elemento e, em seguida, explicitamente passe o valor a ser usado como um segundo parâmetro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Geralmente, desejamos executar formatação personalizada no valor de saída. O método estático de string integrado ao .NET é útil para esses cenários. Nosso modelo de exibição Edit.aspx está usando este para formatar o valor de data doevento (que é do tipo DateTime) para que ela não mostra segundos para o tempo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Um terceiro parâmetro Html.TextBox() opcionalmente pode ser usado para os atributos HTML adicionais de saída. O trecho de código abaixo demonstra como renderizar um tamanho adicional = atributo "30" e uma classe = "mycssclass" atributo no &lt;tipo de entrada = "texto" /&gt; elemento. Observe como nós são escape o nome do atributo de classe usando um "@" character because "classe" é uma palavra reservada no c#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementando o método de ação de edição de HTTP POST

Agora temos a versão de HTTP GET do nosso método de ação de edição implementado. Quando um usuário solicita a */Dinners/Edit/1* URL, eles recebem uma página HTML com o seguinte:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Pressionar o botão "Salvar" faz com que uma postagem de formulário para o */Dinners/Edit/1* URL, e envia o HTML &lt;entrada&gt; valores usando o verbo HTTP POST de formulário. Agora, vamos implementar o comportamento de HTTP POST do nosso método de ação de edição – que tratará a salvar a refeição.

Vamos começar com a adição de um método de ação sobrecarregado "Editar" para nosso DinnersController que tem um atributo "AcceptVerbs" que indica que ele lida com cenários de HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Quando o atributo [AcceptVerbs] é aplicado a métodos de ação sobrecarregado, o ASP.NET MVC controla automaticamente solicitações de distribuição para o método de ação apropriada dependendo do verbo HTTP de entrada. Solicitações HTTP POST para <em>/Dinners/Editar / [id]</em> URLs para ir para o método de edição acima, enquanto todas as outras solicitações de verbo HTTP para <em>/Dinners/Editar / [id]</em>URLs para ir para o primeiro método Editar implementamos (que foi não tem um atributo [AcceptVerbs]).

| **Tópico de lado: Por que diferencia por meio de verbos HTTP?** |
| --- |
| Você pode perguntar – por que estamos usando uma única URL e diferenciar seu comportamento via o verbo HTTP. Por que não tem apenas duas URLs separados para tratar o carregamento e salvar as alterações? Por exemplo: /Dinners/Editar / [id] para exibir o formulário inicial e /Dinners/Save / [id] para lidar com a postagem de formulário para salvá-lo? A desvantagem com publicação duas URLs separados é que em casos onde podemos postar /Dinners/Save/2 e, em seguida, é necessário exibir novamente o formulário HTML devido a um erro de entrada, o usuário final será acabar tendo a URL de salvamento/jantares/2 na barra de endereços do navegador (desde que foi a URL do formulário postado). Se o usuário final indicadores nesta página redisplayed à sua lista de favoritos do navegador, ou a URL de cópia/cola e envia por email para um amigo, elas terminarão salvando uma URL que não funcionará no futuro (desde que essa URL depende de valores de post). Ao expor uma única URL (como: /Dinners/Edit/[id]) e diferenciar o processamento por um verbo HTTP, é seguro para os usuários finais para a página de edição de indicadores e/ou enviar a URL para outras pessoas. |

#### <a name="retrieving-form-post-values"></a>Recuperando valores de postagem de formulário

Há várias maneiras que podemos acessar postadas parâmetros de formulário em nosso método "Editar" de HTTP POST. Uma abordagem simple é usar apenas a propriedade Request na classe base do controlador para acessar a coleção de formulário e recuperar os valores postados diretamente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

A abordagem acima é um pouco verbose, porém, especialmente quando podemos adicionar lógica de tratamento de erros.

A melhor abordagem para esse cenário é utilizar o interno *UpdateModel()* método auxiliar na classe base do controlador. Ele dá suporte à atualização das propriedades de um objeto que é passá-lo usando os parâmetros de formulário de entrada. Ele usa reflexão para determinar os nomes de propriedade no objeto e, em seguida, automaticamente converte e atribui valores a elas com base nos valores de entrada enviados pelo cliente.

Podemos usar o método UpdateModel() para simplificar a nossa HTTP POST Editar ação usando este código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Agora podemos pode visitar o */Dinners/Edit/1* URL e altere o título do nosso refeição:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Quando é clicar no botão "Salvar", executará uma postagem de formulário para nosso Editar ação e os valores atualizados serão mantidos no banco de dados. Podemos será, em seguida, ser redirecionados para a URL de detalhes do diagrama (que exibe os valores salvos recentemente):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Tratamento de erros de edição

Nossa atual funciona de implementação de HTTP POST excelente – exceto quando há erros.

Quando um usuário fizer um erro de edição de um formulário, é preciso certificar-se de que o formulário é exibida novamente com uma mensagem de erro informativa orientá-los para corrigi-lo. Isso inclui os casos em que um usuário final envia a entrada incorreta (por exemplo: uma cadeia de caracteres malformada data), como também casos onde o formato de entrada é válido, mas há uma violação de regra de negócios. Quando ocorrem erros que o formulário deve preservar dados de entrada, o usuário inserido originalmente para que eles não precisam preencher novamente suas alterações manualmente. Esse processo deve ser repetida quantas vezes forem necessárias até que o formulário seja concluída com êxito.

O ASP.NET MVC inclui bons recursos internos que tornam o tratamento de erros e reexibição de forma fácil. Para ver esses recursos em ação vamos atualizar nossos método de ação de edição com o código a seguir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

O código acima é semelhante à nossa implementação anterior, exceto pelo fato de agora são encapsulamento um bloco de tratamento de erros try/catch em torno de nosso trabalho. Se ocorrer uma exceção ao chamar UpdateModel() ou quando tentar e salvar o DinnerRepository (o que irá gerar uma exceção se o objeto de uma refeição que estamos tentando salvar é inválido devido a uma violação de regra em nosso modelo), bloco de tratamento de erro nosso catch será Execute. Nele, um loop quaisquer violações de regra que existem no objeto refeição e adicioná-los a um objeto ModelState (que discutiremos em breve). Podemos reexiba o modo de exibição.

Para ver isso funcionando vamos executar novamente o aplicativo, editar uma refeição e alterá-lo para ter um título vazio, um data doevento de "BOGUS" e use um número de telefone do Reino Unido com um valor de país dos EUA. Quando, pressione o botão "Salvar" nosso método HTTP POST editar não será capaz de salvar a refeição (porque há erros) e será exibir novamente o formulário:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nosso aplicativo tem uma experiência de erro razoável. Os elementos de texto com a entrada inválida são realçados em vermelho e mensagens de erro de validação são exibidas para o usuário final sobre eles. O formulário também é preservar os dados de entrada que o usuário inseriu originalmente – para que eles não tenham reabastecimento nada.

Como fazer isso, você pode perguntar, isso ocorriam? Como caixas de texto Título, data doevento e ContactPhone realçar próprios em vermelho e saber para gerar os valores de usuário digitado originalmente? E, como mensagens de erro são exibidas na lista na parte superior? A boa notícia é que isso não ocorre magic – em vez disso, era porque usamos alguns dos recursos internos do ASP.NET MVC que tornam o erro e validação de entrada cenários de tratamento de fácil.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Noções básicas sobre ModelState e os métodos auxiliares HTML de validação

Classes do controlador tem uma coleção de propriedades "ModelState" que fornece uma maneira para indicar a existem de erros com um objeto de modelo que está sendo passado para um modo de exibição. Entradas de erro na coleção ModelState identificam o nome da propriedade do modelo com o problema (por exemplo: "Título", "Data doevento" ou "ContactPhone") e permitem que uma mensagem de erro amigável humanos seja especificada (por exemplo: "Título é necessário").

O *UpdateModel()* método auxiliar preenche automaticamente a coleção ModelState quando encontrar erros ao tentar atribuir valores de formulário a propriedades no objeto de modelo. Por exemplo, a propriedade de data doevento do nosso objeto refeição é do tipo DateTime. Quando o método UpdateModel() era não é possível atribuir o valor de cadeia de caracteres "BOGUS" a ele no cenário acima, o método UpdateModel() adicionado a uma entrada à coleção ModelState indicando um erro de atribuição tivesse ocorrido com essa propriedade.

Os desenvolvedores também podem escrever código para adicionar entradas de erro na coleção de ModelState explicitamente como estamos fazendo abaixo em nosso "catch" Erro tratamento de bloco, que é popular a coleção ModelState com entradas com base nas violações de regra ativa do Objeto refeição:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integração do auxiliar HTML com ModelState

Métodos de auxiliares HTML - como Html.TextBox() - Verifique a coleção de ModelState durante a renderização de saída. Se houver um erro para o item, eles renderizam o valor inserido pelo usuário e uma classe de erro CSS.

Por exemplo, em nosso "Editar" exibição estamos usando o método auxiliar Html.TextBox() para renderizar o data doevento de nosso objeto refeição:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Quando o modo de exibição foi renderizado no cenário de erro, o método Html.TextBox() check coleção ModelState para ver se ocorreram erros associados com a propriedade "Data doevento" de nosso objeto refeição. Quando isso foi determinado que houve um erro, renderizado a entrada de usuário enviado ("BOGUS") como o valor e adicionado a uma classe de erro de css para o &lt;tipo de entrada = "texto" /&gt; marcação gerada:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Você pode personalizar a aparência da classe css erro Pesquisar que desejar. A classe de erro CSS padrão – "entrada-erro de validação" – é definida no *\content\site.css* folha de estilos e parece como abaixo:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Essa regra CSS é o que causou a nossos elementos de entrada inválidos ser realçados como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() Helper Method

O método auxiliar Html.ValidationMessage() pode ser usado para gerar a mensagem de erro ModelState associada a uma propriedade de modelo específico:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Gera o código acima:  *&lt;abrangem classe = "Erro de validação de campo"&gt; o valor 'BOGUS' é inválido &lt; /span&gt;*

O método auxiliar Html.ValidationMessage() também oferece suporte a um segundo parâmetro que permite aos desenvolvedores substituir a mensagem de texto de erro é exibida:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Gera o código acima:  <em>&lt;abrangem classe = "Erro de validação de campo"&gt;\*&lt;/span&gt;</em>em vez do texto de erro padrão quando houver um erro para o Propriedade de data doevento.

##### <a name="htmlvalidationsummary-helper-method"></a>Método auxiliar de Html.ValidationSummary()

O método auxiliar Html.ValidationSummary() pode ser usado para processar uma mensagem de erro de resumo, acompanhada por um &lt;ul&gt;&lt;li /&gt;&lt;/UL&gt; lista de erro detalhada de todas as mensagens no ModelState coleção:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

O método auxiliar Html.ValidationSummary() aceita um parâmetro de cadeia de caracteres opcional – que define uma mensagem de erro de resumo para exibir acima da lista de erros detalhados:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Opcionalmente, você pode usar CSS para substituir a aparência de lista de erros.

#### <a name="using-a-addruleviolations-helper-method"></a>Usando um método auxiliar AddRuleViolations

Nossa implementação de HTTP POST Editar inicial usada uma instrução foreach seu bloco catch loop sobre violações de regras do objeto refeição e adicioná-los à coleção de ModelState do controlador:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Podemos tornar esse código um pouco limpador adicionando "ControllerHelpers" classe ao projeto NerdDinner e implementar um método de extensão "AddRuleViolations" dentro dele que adiciona um método auxiliar para a classe ModelStateDictionary do ASP.NET MVC. Esse método de extensão pode encapsular a lógica necessária preencher o ModelStateDictionary com uma lista de erros de RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Em seguida, podemos atualizar nosso método de ação HTTP POST Editar para usar esse método de extensão para preencher a coleção ModelState com nosso violações de regra refeição.

#### <a name="complete-edit-action-method-implementations"></a>Conclua as implementações de método de ação de edição

O código a seguir implementa toda a lógica de controlador necessária para o nosso cenário de edição:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

A vantagem sobre a implementação de edição é que nossa classe de controlador, nem nosso modelo de exibição tem sabe nada sobre validação específica ou está sendo impostas pelo nosso modelo refeição as regras de negócio. Podemos pode adicionar mais regras para nosso modelo no futuro e não precisará fazer alterações de código para o nosso controlador ou o modo de exibição para poderem ser suportados. Isso fornece a flexibilidade de evoluir facilmente nossos requisitos do aplicativo no futuro com um mínimo de alterações de código.

### <a name="create-support"></a>Criar suporte

Podemos acabou de implementar o comportamento de "Edit" de nossa classe DinnersController. Agora vamos implementar o suporte de "Criar" nele – que permite que os usuários adicionem jantares novo.

#### <a name="the-http-get-create-action-method"></a>O HTTP GET criar método de ação

Vamos começar ao implementar o comportamento de "GET" de HTTP do nosso criar método de ação. Esse método será chamado quando alguém visita o *jantares/criar* URL. Nossa implementação é semelhante a:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

O código acima cria um novo objeto de uma refeição e atribui a propriedade data doevento para ser uma semana no futuro. Ele renderiza uma exibição com base no novo objeto refeição. Porque estamos ainda não tiver passado explicitamente um nome para o *View()* método auxiliar, ele usará o caminho padrão baseado em convenção para resolver o modelo de exibição: /Views/Dinners/Create.aspx.

Agora vamos criar esse modelo de exibição. Podemos fazer isso clicando duas vezes no método de ação de criar e selecionando o comando de menu de contexto "Adicionar modo de exibição". A caixa de diálogo "Adicionar modo de exibição" será indicamos que está passando um objeto de uma refeição para o modelo de exibição e optar por scaffold automaticamente um modelo de "Criar":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Quando, clique no botão "Adicionar", o Visual Studio salvar um novo scaffold "Create.aspx" modo de exibição baseado no diretório "\Views\Dinners" e abri-lo no IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Vamos fazer algumas alterações para o padrão "criar" scaffold arquivo que foi gerado para nós e modificá-lo para cima para observar como abaixo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

E agora quando executamos nosso aplicativo e o acesso a *"Jantares/criar"* URL dentro do navegador, ele processará a interface do usuário como abaixo da nossa implementação da ação criar:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementando o HTTP POST criar método de ação

Temos a versão de HTTP GET do nosso método de ação de criação implementado. Quando um usuário clica no botão "Salvar", ele executa uma postagem de formulário para o *jantares/criar* URL, e envia o HTML &lt;entrada&gt; valores usando o verbo HTTP POST de formulário.

Agora, vamos implementar o comportamento de HTTP POST do nosso criar método de ação. Vamos começar com a adição de um método de ação de "Criar" sobrecarregado para nosso DinnersController que tem um atributo "AcceptVerbs" que indica que ele lida com cenários de HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Há várias maneiras de que acessar os parâmetros de formulário postado em nosso método "Criar" de HTTP POST habilitado.

Uma abordagem é criar um novo objeto de uma refeição e, em seguida, usar o *UpdateModel()* método auxiliar (como fizemos com a ação de edição) para preenchê-la com os valores de formulário postado. Podemos, em seguida, adicioná-la à nossa DinnerRepository, persisti-los para o banco de dados e redirecionará o usuário para nossa ação detalhes para mostrar a refeição recém-criado usando o código a seguir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Como alternativa, podemos usar uma abordagem em que temos nosso método de ação Create () utilizam um objeto de uma refeição como um parâmetro de método. ASP.NET MVC será, em seguida, automaticamente criar um novo objeto de refeição para nós, preencher suas propriedades usando as entradas de formulário e passá-lo para nosso método de ação:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Nosso método de ação acima verifica se o objeto de uma refeição com êxito preenchido, com os valores de postagem de formulário, verificando a propriedade ModelState. Isso retornará false se não houver problemas de conversão de entrada (por exemplo: uma cadeia de caracteres "BOGUS" para a propriedade data doevento), e se houver algum problema nosso método de ação exibe novamente o formulário.

Se os valores de entrada são válidos, o método de ação tentará adicionar e salve a novo refeição o DinnerRepository. Ele encapsula esse trabalho em um bloco try/catch e exibe novamente o formulário, se houver quaisquer violações de regra de negócios (que faria com que o método dinnerRepository.Save() gerar uma exceção).

Para ver esse comportamento na ação de tratamento de erros, é possível solicitar a *jantares/criar* URL e preencha os detalhes sobre uma refeição novo. Entrada incorretova ou valores fará com que o formulário de criação ser exibida novamente com os erros realçados como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Observe como o nosso formulário de criação está honrando exata mesmas regras validação e business como nosso formulário de edição. Isso ocorre porque o nosso validação e regras de negócios foram definidas no modelo e não foram inseridas dentro da interface do usuário ou o controlador do aplicativo. Isso significa que podemos pode posteriormente alterar/evoluir nosso validação ou regras de negócio em um único colocam e têm-los se aplicam em todo nosso aplicativo. Nós não terá que alterar qualquer código dentro de um nosso editar ou criar métodos de ação para aceitar automaticamente quaisquer novas regras ou modificações aos existentes.

Quando podemos corrigir os valores de entrada e clique no botão "Salvar" novamente, nossos além de DinnerRepository será bem-sucedida e uma refeição novo será adicionada ao banco de dados. É, em seguida, serão redirecionados para a */Dinners/detalhes / [id]* URL – onde podemos verá detalhes sobre uma refeição recém-criado:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Excluir o suporte

Agora vamos adicionar suporte de "Excluir" para nosso DinnersController.

#### <a name="the-http-get-delete-action-method"></a>O método de ação de exclusão de HTTP GET

Vamos começar ao implementar o comportamento de HTTP GET do nosso método de ação de exclusão. Esse método será chamado quando alguém visita o */Dinners/excluir / [id]* URL. Abaixo está a implementação:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

O método de ação tenta recuperar uma refeição a ser excluído. Se uma refeição existir, ele renderiza uma exibição baseada em objeto refeição. Se o objeto não existe (ou já foi excluído) retorna uma exibição que renderiza o "NotFound" Exibir modelo criado anteriormente para nosso método de ação "Detalhes".

Podemos criar o modelo de exibição "Excluir" clicando duas vezes no método de ação de excluir e selecionar o comando de menu de contexto "Adicionar modo de exibição". A caixa de diálogo "Adicionar modo de exibição" será indicamos que está passando um objeto de uma refeição ao nosso modelo de exibição como seu modelo e optar por criar um modelo vazio:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Quando, clique no botão "Adicionar", o Visual Studio adiciona um novo arquivo de modelo de exibição "Delete.aspx" para que possamos em nosso diretório "\Views\Dinners". Vamos adicionar alguns HTML e o código para o modelo para implementar uma tela de confirmação de exclusão como abaixo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

O código acima exibe o título de uma refeição a ser excluído e saídas de uma &lt;formulário&gt; elemento que faz um POST para a URL de /Dinners/excluir / [id] se o usuário final clicar no botão "Excluir" dentro dele.

Quando executamos nosso aplicativo e o acesso a *"/ jantares/Delete / [id]"* URL para uma refeição válida de objeto processa a interface do usuário como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Tópico de lado: Por que estamos fazendo uma POSTAGEM?** |
| --- |
| Você pode perguntar – por que prosseguimos o esforço de criação de um &lt;formulário&gt; em nossa tela de confirmação de exclusão? Por que não usar apenas um hiperlink padrão para vincular a um método de ação que faz a operação de exclusão real? O motivo é que queremos ter cuidado para proteger contra rastreadores da web e descobrir nossas URLs e inadvertidamente fazendo com que dados sejam excluídos quando eles sigam os links de mecanismos de pesquisa. Com base em HTTP GET URLs são considerados "seguras" para que eles/rastreamento de acesso e que deveriam ser não seguem as HTTP POST. Uma boa regra é certificar-se sempre colocar destrutivo ou operações de modificação de dados por trás das solicitações HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementando o método de ação de exclusão de HTTP POST

Agora temos a versão de HTTP GET do nosso método de ação de exclusão implementado que exibe uma tela de confirmação de exclusão. Quando um usuário final clicar no botão "Excluir" ele executará uma postagem de formulário para o */Dinners refeição / [id]* URL.

Agora, vamos implementar o comportamento de "POST" de HTTP do método de ação de exclusão usando o código a seguir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

A versão de HTTP POST do nosso método de ação de exclusão tenta recuperar o objeto de uma refeição para excluir. Se ele não é possível localizar (porque ela já foi excluída) ele renderiza nosso modelo de "NotFound". Se ele encontrar a refeição, ele exclui-lo a DinnerRepository. Em seguida, ele processa um modelo de "Excluído".

Para implementar o modelo de "Excluído" Vamos o método de ação do mouse e escolha o menu de contexto "Adicionar modo de exibição". Vamos nomear nossa visão "Excluído" e que ele seja um modelo vazio (e não em um objeto de modelo fortemente tipado). Em seguida, vamos adicionar conteúdo HTML a ela:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

E agora quando executamos nosso aplicativo e o acesso a *"/ jantares/Delete / [id]"* URL para uma refeição válida confirmação de exclusão de objeto, ele processará uma refeição nossa tela como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Quando é clicar no botão "Excluir" ele executará um HTTP POST para o */Dinners/excluir / [id]* URL, que irá excluir a refeição do nosso banco de dados e exibir nosso modelo de exibição "Excluído":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Segurança de associação de modelo

Discutimos duas maneiras de usar os recursos internos do modelo de associação do ASP.NET MVC. O primeiro usando o método UpdateModel() para atualizar as propriedades em um objeto de modelo existente e o segundo usando o suporte de ASP.NET MVC para passar objetos de modelo no como parâmetros de método de ação. Ambas as técnicas são muito eficientes e extremamente úteis.

Essa capacidade também traz responsabilidade. É importante sempre ser paranoica sobre segurança ao aceitar a entrada do usuário, e isso também é verdadeiro quando a associação de objetos de entrada de formulário. Você deve ter cuidado para sempre HTML codificar valores inseridos pelo usuário para evitar ataques de injeção de HTML e JavaScript e o cuidado de ataques de injeção de SQL (Observação: estamos usando o LINQ to SQL em nosso aplicativo, que codifica automaticamente os parâmetros para evitar esses tipos de ataques). Você nunca deve depende da validação do lado do cliente apenas e sempre empregar a validação do lado do servidor para se proteger contra os hackers tentar enviar valores falsos.

Um item de segurança adicional para certificar-se de que você pensar sobre quando usar os recursos de associação do ASP.NET MVC é o escopo dos objetos que você está associando. Especificamente, você deseja certificar-se de que compreender as implicações de segurança das propriedades que estiver permitindo a ser associado e verifique se você permitir que somente as propriedades que realmente devem ser atualizadas por um usuário final a ser atualizado.

Por padrão, o método UpdateModel() tenta atualizar todas as propriedades no objeto de modelo que correspondem a valores de parâmetro de formulário de entrada. Da mesma forma, os objetos passados como parâmetros de método de ação também por padrão podem ter todas as suas propriedades definidas por meio de parâmetros do formulário.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Bloqueio de associação em uma base por utilização

Você pode bloquear a política de associação em uma base por uso fornecendo um explícita "incluir lista" das propriedades que podem ser atualizadas. Isso pode ser feito, passando um parâmetro de matriz de cadeia de caracteres extras para o método UpdateModel() como abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objetos passados como parâmetros de método de ação também oferecem suporte a um atributo [ligação] que permite que um "incluir lista" de permitidas propriedades seja especificada como abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bloqueio de associação de tipo

Você também pode bloquear as regras de associação em uma base por tipo. Isso permite que você especifique as regras de associação de uma vez, e, então, eles se aplicam em todos os cenários (incluindo os cenários de parâmetro de método UpdateModel e ação) em todos os controladores e os métodos de ação.

Você pode personalizar as regras de associação por tipo, adicionando um atributo [ligação] para um tipo ou registrando-o no arquivo global. asax do aplicativo (útil para cenários em que você não tem o tipo). Você pode usar Include ligação do atributo e excluir propriedades para controlar quais propriedades são associáveis para a determinada classe ou interface.

Vamos usar essa técnica para a classe refeição em nosso aplicativo NerdDinner e adicionar um atributo [ligação] que restringe a lista de propriedades vinculáveis ao seguinte:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Observe que não permitem a coleção RSVPs ser manipulados por meio de associação, nem são, permitindo que as propriedades DinnerID ou HostedBy a ser definido por meio de associação. Por motivos de segurança, em vez disso, vai manipular apenas essas propriedades específicas usando código explícito em nossos métodos de ação.

### <a name="crud-wrap-up"></a>Encerramento CRUD

O ASP.NET MVC inclui uma série de recursos internos que ajudam a implementar cenários de lançamento do formulário. Usamos uma variedade desses recursos para oferecer suporte a interface de usuário CRUD sobre nosso DinnerRepository.

Estamos usando uma abordagem voltada para o modelo para implementar o nosso aplicativo. Isso significa que todos os nossos lógica é definida na camada nosso modelo – e não em nosso controladores ou modos de exibição de regra de negócio e validação. Nossa classe de controlador, nem os nossos modelos de exibição sabem nada sobre as regras de negócio específico que está sendo impostas pelo nossa classe de modelo refeição.

Isso manterá nossa arquitetura de aplicativo limpa e facilitar o teste. Podemos adicionar regras de negócios adicionais à nossa camada de modelo no futuro e *não precisará fazer alterações de código* nosso controlador ou exibição para poderem ser suportados. Isso irá fornecer uma grande quantidade de agilidade para desenvolver e alterar o nosso aplicativo no futuro.

Nosso DinnersController agora permite que uma refeição listagens/detalhes, bem como criar, editar e excluir suporte. O código completo para a classe pode ser encontrado abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Próxima etapa

Agora temos suporte básico de CRUD (criar, ler, atualizar e excluir) implementar em nossa classe DinnersController.

Agora, vamos ver como podemos pode usar classes ViewData e ViewModel para habilitar a interface do usuário ainda melhores em nosso formulários.

> [!div class="step-by-step"]
> [Anterior](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Próximo](use-viewdata-and-implement-viewmodel-classes.md)
