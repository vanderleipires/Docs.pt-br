---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fornecer CRUD (criar, ler, atualizar e excluir) dados formam o suporte de entrada | Microsoft Docs
author: microsoft
description: Etapa 5 mostra como realizar a nossa classe DinnersController ainda mais ao habilitar suporte para edição, criação e a exclusão de jantares com ele também.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bfb8446ec8b39ad6fc88a0d5b747f0cec33bbd25
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817630"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fornecer CRUD (criar, ler, atualizar e excluir) dados de suporte de entrada de formulário
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 5 do grátis [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que orienta-through como criar um pequeno, mas concluir, o aplicativo web usando ASP.NET MVC 1.
> 
> Etapa 5 mostra como realizar a nossa classe DinnersController ainda mais ao habilitar suporte para edição, criação e a exclusão de jantares com ele também.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga a [obtendo iniciado com o MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner etapa 5: Criar, atualizar e excluir os cenários de formulário

Já apresentamos controladores e exibições e abordou como usá-los para implementar uma experiência de listagem/detalhes para jantares no site. Nossa próxima etapa será executar nossa classe DinnersController adicional e habilitar o suporte para edição, criação e a exclusão de jantares com ele também.

### <a name="urls-handled-by-dinnerscontroller"></a>URLs manipuladas pelo DinnersController

Adicionamos anteriormente métodos de ação ao DinnersController que implementou o suporte para duas URLs: */Dinners* e */Dinners/detalhes / [id]*.

| **URL** | **VERBO** | **Finalidade** |
| --- | --- | --- |
| */Dinners/* | OBTER | Exiba uma lista HTML de jantares futuros. |
| */Dinners/detalhes / [id]* | OBTER | Exibir detalhes sobre um jantar específico. |

Agora, adicionaremos métodos de ação para implementar as três URLs adicionais: <em>/Dinners/Editar / [id], / jantares/criar,</em>e<em>/Dinners/Delete / [id]</em>. Essas URLs serão habilitar o suporte para edição jantares existentes, criando novos jantares e excluindo jantares.

Daremos suporte a interações de verbo HTTP GET e HTTP POST com essas novas URLs. Solicitações HTTP GET para essas URLs exibirá o modo de exibição HTML inicial dos dados (um formulário preenchido com os dados de jantar no caso de "Editar", um formulário em branco no caso de "criar" e uma tela de confirmação de exclusão no caso de "exclusão"). Solicitações de HTTP POST a essas URLs serão salvar/atualizar/excluir os dados de jantar nosso DinnerRepository (e daí para o banco de dados).

| **URL** | **VERBO** | **Finalidade** |
| --- | --- | --- |
| */Dinners/Editar / [id]* | OBTER | Exiba um formulário HTML editável preenchido com dados de jantar. |
| POSTAR | Salve as alterações do formulário em um jantar específico para o banco de dados. |
| */Dinners/Create* | OBTER | Exiba um formulário HTML vazio que permite aos usuários definir jantares novo. |
| POSTAR | Crie um novo jantar e salvá-lo no banco de dados. |
| */Dinners/Delete/[id]* | OBTER | Exibir a tela de confirmação de exclusão. |
| POSTAR | Exclui o jantar especificado do banco de dados. |

### <a name="edit-support"></a>Editar o suporte

Comecemos Implementando o cenário de "Editar".

#### <a name="the-http-get-edit-action-method"></a>O método de ação de edição de HTTP-GET

Vamos começar implementando o comportamento de "Introdução" de HTTP de nosso método de ação de editar. Esse método será invocado quando o */Dinners/Editar / [id]* URL é solicitada. Nossa implementação se parecerá com:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

O código acima usa o DinnerRepository para recuperar um objeto do jantar. Ela então processa um modelo de exibição usando o objeto de jantar. Como ainda não explicitamente passamos um nome de modelo para o *View()* método auxiliar, ele usará o caminho do baseado na convenção padrão para resolver o modelo de exibição: /Views/Dinners/Edit.aspx.

Agora, vamos criar esse modelo de exibição. Faremos isso clicando duas vezes dentro do método Edit e selecionando o comando de menu de contexto "Adicionar exibição":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Na caixa de diálogo "Adicionar exibição" indicaremos que está passando um objeto de jantar ao nosso modelo de exibição como seu modelo e escolher o modelo "Editar" para o scaffold automaticamente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Quando clicamos no botão "Adicionar", o Visual Studio irá adicionar um novo arquivo de modelo de exibição de "Edit" para que possamos dentro do diretório "\Views\Dinners". Ele também abrirá o novo modelo de exibição de "Edit" no editor de código-– preenchido com uma "Editar" scaffold implementação inicial como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Vamos fazer algumas alterações para o padrão "Editar" scaffold gerado e atualizar o modelo de exibição de edição para que o conteúdo abaixo (o que remove algumas das propriedades que não queremos expor):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Quando executamos o aplicativo e a solicitação a *"/ Editar/jantares/1"* URL veremos a seguinte página:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

A marcação HTML gerada pelo nosso ver a aparência abaixo. É HTML padrão – com uma &lt;formulário&gt; elemento que realiza um HTTP POST para o */Dinners/Edit/1* URL quando o "Salvar" &lt;tipo de entrada = "Enviar" /&gt; botão é pressionado. Uma HTML &lt;tipo de entrada = "text" /&gt; elemento tiver sido a saída para cada propriedade editável:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() e métodos de auxiliar Html Html.TextBox()

Nosso modelo de exibição de "Edit" está usando vários métodos "Auxiliares Html": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() e Html.ValidationMessage(). Além de gerar uma marcação HTML para nós, esses métodos auxiliares fornecem validação e manipulação de erros internos dão suporte.

##### <a name="htmlbeginform-helper-method"></a>Método auxiliar de Html.BeginForm()

O método auxiliar Html.BeginForm() é o que o HTML de saída &lt;formulário&gt; elemento na nossa marcação. Nosso modelo de exibição de Edit, você observará que estamos aplicando uma "instrução c# using" ao usar esse método. A chave de abertura aberto indica o início do &lt;formulário&gt; conteúdo e a chave de fechamento é o que indica o fim do &lt;/formulário&gt; elemento:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Como alternativa, se você achar que a instrução "using" abordar não natural para um cenário como esse, você pode usar uma combinação de Html.BeginForm() e Html.EndForm() (que faz a mesma coisa):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Chamar Html.BeginForm() sem nenhum parâmetro fará com que um elemento de formulário que faz um POST HTTP para a URL da solicitação de saída. Isto é por que nossa exibição de edição gera uma *&lt;ação de formulário = método "/ Editar/jantares/1" = "post"&gt;* elemento. Poderia ter como alternativa, passamos parâmetros explícitos para Html.BeginForm() se quiséssemos postar em uma URL diferente.

##### <a name="htmltextbox-helper-method"></a>Método auxiliar de Html.TextBox()

Nossa exibição Edit usa o método auxiliar Html.TextBox() para gerar &lt;tipo de entrada = "text" /&gt; elementos:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Método Html.TextBox() acima usa um único parâmetro – o que está sendo usado para especificar os atributos de nome/id do &lt;tipo de entrada = "text" /&gt; saída, bem como a propriedade do modelo para preencher o valor da caixa de texto do elemento. Por exemplo, o objeto de jantar são passados para a exibição de edição tinha um valor da propriedade "Title" de ".NET futura", e então, nosso método Html.TextBox("Title") chamadas de saída: *&lt;identificação de entrada = "Title" name = "Title" type = "text" value = "Futura do .NET" /&gt;*.

Como alternativa, podemos usar o primeiro parâmetro Html.TextBox() para especificar o nome/id do elemento e, em seguida, transmitir explicitamente o valor a ser usado como um segundo parâmetro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Geralmente queremos executar a formatação personalizada no valor que é a saída. O método estático integrado ao .NET Format () é útil para esses cenários. Nosso modelo de exibição de Edit está usando isso para formatar o valor de EventDate (que é do tipo DateTime) para que ele não mostra segundos para o tempo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Um terceiro parâmetro Html.TextBox() opcionalmente pode ser usado para a saída de atributos adicionais de HTML. O trecho de código abaixo demonstra como renderizar um adicionais de tamanho = atributo "30" e uma classe = "mycssclass" atributo na &lt;tipo de entrada = "text" /&gt; elemento. Observe como estamos são escape o nome do atributo de classe usando um "@" character because "classe" é uma palavra-chave reservada no c#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementando o método de ação de edição de HTTP POST

Agora temos a versão de HTTP GET do nosso método de ação Editar implementado. Quando um usuário solicita a */Dinners/Edit/1* URL, eles recebem uma página HTML semelhante ao seguinte:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Pressionar o botão "Salvar" faz com que uma postagem de formulário para o */Dinners/Edit/1* URL, e envia o HTML &lt;entrada&gt; valores usando o verbo HTTP POST de formulário. Agora, vamos implementar o comportamento do HTTP POST do nosso método de ação Editar – que irá manipular salvando o jantar.

Vamos começar pela adição de um método sobrecarregado da ação de "Editar" para nosso DinnersController que tem um atributo "AcceptVerbs" nele que indica que ele lida com cenários de HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Quando o atributo [AcceptVerbs] é aplicado aos métodos de ação sobrecarregado, o ASP.NET MVC controla automaticamente solicitações de expedição para o método de ação apropriada dependendo do verbo HTTP de entrada. Solicitações HTTP POST <em>/Dinners/Editar / [id]</em> URLs ir para o método acima de edição, enquanto todas as outras solicitações de verbo HTTP para <em>/Dinners/Editar / [id]</em>URLs passará para o primeiro método de edição que implementamos (que foi não tem um atributo [AcceptVerbs]).

| **Tópico de lado: Por que diferencia a por meio de verbos HTTP?** |
| --- |
| Você pode perguntar – por que estamos usando uma única URL e diferenciar seu comportamento por meio do verbo HTTP? Por que não tem apenas duas URLs separadas para tratar o carregamento e salvar as alterações de edição? Por exemplo: /Dinners/Editar / [id] para exibir o formulário inicial e /Dinners/Save / [id] para lidar com a postagem de formulário para salvá-lo? A desvantagem com publicação duas URLs separadas é que em casos onde podemos enviar para /Dinners/Save/2 e, em seguida, precisa exibir novamente o formulário HTML devido a um erro de entrada, o usuário final acabará tendo a URL de jantares/Save/2 na barra de endereços do seu navegador (desde que foi a URL do formulário postado). Se o usuário final indicadores desta página redisplayed para sua lista de favoritos do navegador, ou cópia/cola a URL e envia por email para um amigo, elas terminarão salvando uma URL que não funcionará no futuro (já que essa URL depende de valores de post). Expondo uma única URL (como: /Dinners/Edit/[id]) e diferenciar o processamento dele por um verbo HTTP, é seguro aos usuários finais para a página de edição de indicador e/ou enviar a URL para outras pessoas. |

#### <a name="retrieving-form-post-values"></a>Recuperando valores de postagem de formulário

Há várias maneiras que podemos acessar postadas parâmetros de formulário dentro do nosso método de "Editar" de HTTP POST. Uma abordagem simples é usar apenas a propriedade de solicitação na classe base do controlador para acessar a coleção de formulário e recuperar os valores postados diretamente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

A abordagem acima é um pouco detalhada, no entanto, especialmente quando podemos adicionar lógica de tratamento de erros.

Uma melhor abordagem para esse cenário é aproveitar o interno *UpdateModel()* método auxiliar na classe base do controlador. Ele dá suporte à atualização das propriedades de um objeto que passamos a ela usando os parâmetros de formulário de entrada. Ele usa a reflexão para determinar os nomes de propriedade no objeto e, em seguida, automaticamente converte e atribui valores a elas com base nos valores de entrada enviados pelo cliente.

Poderíamos usar o método UpdateModel() para simplificar nossa ação de editar do HTTP POST usando este código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Agora podemos pode visitar o */Dinners/Edit/1* URL e altere o título do nosso jantar:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Quando clicamos no botão "Salvar", executaremos uma postagem de formulário para nossa ação de editar e os valores atualizados serão mantidos no banco de dados. Podemos será, em seguida, ser redirecionados para a URL de detalhes para o jantar (que exibe os valores salvos recentemente):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Tratamento de erros de edição

Nosso atual funciona de implementação de HTTP-POST bem – exceto quando há erros.

Quando um usuário faz um engano um formulário de edição, é necessário certificar-se de que o formulário é reexibido com uma mensagem de erro informativas-os para corrigi-lo. Isso inclui os casos em que um usuário final envia entrada incorreta (por exemplo: uma cadeia de caracteres de data malformados), bem como casos onde o formato de entrada é válido, mas há uma violação de regra de negócios. Quando ocorrem erros que o formulário deve preservar os dados de entrada o usuário inserido originalmente para que eles não precisam preencher novamente suas alterações manualmente. Esse processo deve ser repetida quantas vezes forem necessárias até que o formulário for concluída com êxito.

O ASP.NET MVC inclui bons recursos internos que facilitam o tratamento de erros e a nova exibição de formulário. Para ver esses recursos em ação vamos atualizar nosso método de ação Editar com o código a seguir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

O código acima é semelhante à nossa implementação de anterior, exceto pelo fato de que estamos agora encapsulando um bloco de tratamento de erro try/catch em torno de nosso trabalho. Se ocorrer uma exceção ao chamar UpdateModel() ou quando podemos testar e salvar o DinnerRepository (que irá gerar uma exceção se o objeto de Dinner que estamos tentando salvar é inválido devido a uma violação de regra dentro do nosso modelo), nosso bloco de tratamento de erro catch será Execute. Dentro dele vamos executar um loop sobre quaisquer violações de regra que existe no objeto de jantar e adicioná-los a um objeto ModelState (que discutiremos em breve). Podemos, em seguida, exiba novamente a exibição.

Para ver isso funcionando vamos executar o aplicativo novamente, editar um jantar e alterá-lo para ter um título vazio, um EventDate de "BOGUS" e use um número de telefone do Reino Unido com um valor de país do EUA. Quando clicamos no botão "Salvar" nosso método Editar do HTTP POST não poderá salvar o jantar (porque há erros) e exibir o formulário será novamente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nosso aplicativo tem uma experiência de erro decente. Os elementos de texto com uma entrada inválida são realçados em vermelho, e mensagens de erro de validação são exibidas para o usuário final sobre eles. O formulário também é preservar os dados de entrada que o usuário inseriu originalmente – para que eles não precisem reabastecer nada.

Como fazer isso, você pode perguntar, isso ocorreu? Como as caixas de texto do título, EventDate e ContactPhone destacar a mesmos em vermelho e saber para gerar os valores de usuário inserido originalmente? E, como as mensagens de erro exibidas na lista na parte superior? A boa notícia é que isso não ocorria por mágica — em vez disso, ele foi porque usamos alguns dos recursos internos do ASP.NET MVC que facilitam a validação de entrada e de erro de cenários de tratamento.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Noções básicas sobre ModelState e os métodos de auxiliares de HTML de validação

As classes do controlador tem uma coleção de propriedades "ModelState" que fornece uma maneira para indicar a existem de erros com um objeto de modelo que está sendo passado para um modo de exibição. Entradas de erro dentro da coleção ModelState identificam o nome da propriedade do modelo com o problema (por exemplo: "Title", "EventDate" ou "ContactPhone") e permitir que uma mensagem de erro amigável a humanos sejam especificados (por exemplo: "Título é obrigatório").

O *UpdateModel()* método auxiliar preenche automaticamente a coleção ModelState quando ele encontra erros ao tentar atribuir valores de formulário a propriedades no objeto de modelo. Por exemplo, nosso jantar EventDate propriedade de objeto é do tipo DateTime. Quando o método UpdateModel() era não é possível atribuir o valor de cadeia de caracteres "BOGUS" a ele no cenário acima, o método UpdateModel() adicionado a uma entrada à coleção ModelState indicando um erro de atribuição tivesse ocorrido com essa propriedade.

Os desenvolvedores também podem escrever código para adicionar entradas de erro na coleção ModelState explicitamente, como estamos fazendo abaixo em nosso "catch" Erro tratamento de bloco, que está preenchendo a coleção ModelState com entradas com base em violações de regra ativa no Objeto de jantar:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integração de auxiliar HTML com ModelState

Métodos de auxiliares HTML - como Html.TextBox() - verificam a coleção de ModelState durante a renderização de saída. Se houver um erro para o item, eles processam o valor inserido pelo usuário e uma classe de erro CSS.

Por exemplo, na nossa exibição "Editar" Estamos usando o método auxiliar Html.TextBox() para renderizar o EventDate do nosso objeto de jantar:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Quando o modo de exibição foi renderizado no cenário de erro, o método Html.TextBox() verificada a coleção de ModelState para ver se houve erros associados com a propriedade "EventDate" do nosso objeto de jantar. Quando isso foi determinado que houve um erro, renderizado a entrada de usuário enviado ("BOGUS") como o valor e adicionado a uma classe de erro de css para o &lt;tipo de entrada = "caixa de texto" /&gt; marcação gerado por ele:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Você pode personalizar a aparência da classe css de erro para procurar a desejar. A classe de erro CSS padrão – "entrada-erro de validação" – é definida na *\content\site.css* folha de estilos e se parece como abaixo:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Essa regra CSS é o que causou a nossos elementos de entrada inválidos para ser realçada como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Método auxiliar de Html.ValidationMessage()

O método auxiliar Html.ValidationMessage() pode ser usado para gerar a mensagem de erro ModelState associada com uma propriedade de modelo específico:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Gera o código acima:  *&lt;estendem a classe = "Erro de validação do campo"&gt; o valor 'BOGUS' é inválido &lt; /span&gt;*

O método auxiliar Html.ValidationMessage() também dá suporte a um segundo parâmetro que permite que os desenvolvedores substituam a mensagem de texto de erro é exibida:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Gera o código acima:  <em>&lt;estendem a classe = "Erro de validação do campo"&gt;\*&lt;/span&gt;</em>em vez do texto de erro padrão quando houver um erro para o Propriedade EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Método auxiliar de Html.ValidationSummary()

O método auxiliar Html.ValidationSummary() pode ser usado para processar uma mensagem de erro resumido, acompanhada por uma &lt;ul&gt;&lt;li /&gt;&lt;/UL&gt; lista de erros detalhado de todas as mensagens no Coleção ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

O método auxiliar Html.ValidationSummary() aceita um parâmetro de cadeia de caracteres opcional – que define uma mensagem de resumo de erro a ser exibido acima da lista de erros detalhados:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Opcionalmente, você pode usar o CSS para substituir a aparência de lista de erros.

#### <a name="using-a-addruleviolations-helper-method"></a>Usando um método auxiliar AddRuleViolations

Nossa implementação de HTTP-POST Editar inicial usado uma instrução foreach dentro do seu bloco catch para executar um loop sobre violações de regra do objeto de jantar e adicioná-los à coleção de ModelState do controlador:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Podemos fazer esse código um pouco limpo, adicionando um "ControllerHelpers" classe ao projeto NerdDinner e implementar um método de extensão "AddRuleViolations" dentro dele que adiciona um método auxiliar para a classe ModelStateDictionary do ASP.NET MVC. Esse método de extensão pode encapsular a lógica necessária popular o ModelStateDictionary com uma lista de erros de RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Em seguida, podemos atualizar nosso método de ação Editar do HTTP POST para usar esse método de extensão para popular a coleção ModelState com nosso violações de regra de jantar.

#### <a name="complete-edit-action-method-implementations"></a>Conclua as implementações de método de ação Editar

O código a seguir implementa toda a lógica do controlador necessária para o nosso cenário de edição:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

A boa notícia sobre nossa implementação de edição é nossa classe de controlador nem o nosso modelo de exibição tem saber nada sobre a validação específica ou as regras de negócio que está sendo impostas pelo nosso modelo de jantar. Podemos pode adicionar mais regras ao nosso modelo no futuro e não precisa fazer nenhuma alteração de código para o nosso controlador ou exibição na ordem para que eles possam ser suportados. Isso fornece a flexibilidade de evoluir facilmente a nossos requisitos de aplicativos no futuro com um mínimo de alterações de código.

### <a name="create-support"></a>Crie o suporte

Concluímos a implementar o comportamento de "Editar" da nossa classe DinnersController. Vamos agora passar para implementar o suporte de "Criar" nele – que permitirá que os usuários adicionem novos jantares.

#### <a name="the-http-get-create-action-method"></a>HTTP-GET criar método de ação

Começaremos por implementar o comportamento de "Introdução" de HTTP de nosso método de ação de criar. Esse método será chamado quando alguém visita a *jantares/criar* URL. Nossa implementação é semelhante a:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

O código anterior cria um novo objeto de jantar e atribui sua propriedade EventDate como uma semana no futuro. Ele, em seguida, renderiza uma exibição com base no novo objeto de jantar. Como ainda não explicitamente passamos um nome para o *View()* método auxiliar, ele usará o caminho do baseado na convenção padrão para resolver o modelo de exibição: /Views/Dinners/Create.aspx.

Agora, vamos criar esse modelo de exibição. Podemos fazer isso clicando duas vezes no método de ação criar e selecionando o comando de menu de contexto "Adicionar exibição". Na caixa de diálogo "Adicionar exibição" indicaremos que estiver passando um objeto de jantar para o modelo de exibição e optar por scaffold automaticamente um modelo de "Criar":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Quando clicamos no botão "Adicionar", o Visual Studio salvar uma nova exibição de "Aspx" com base em scaffold no diretório "\Views\Dinners" e abri-lo dentro do IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Vamos fazer algumas alterações no padrão "criar" scaffold arquivo que foi gerado para nós e modificá-lo para cima a aparência abaixo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

E agora quando executamos nosso aplicativo e o acesso a *"Jantares/criar"* URL no navegador, ele será renderizado da interface do usuário como abaixo da nossa implementação de ação de criar:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementando o HTTP-POST criar método de ação

Temos a versão de HTTP GET do nosso método de ação Create implementado. Quando um usuário clica no botão "Salvar", ele executa uma postagem de formulário para o *jantares/Create* URL, e envia o HTML &lt;entrada&gt; valores usando o verbo HTTP POST de formulário.

Agora, vamos implementar o comportamento do HTTP POST do nosso método de ação de criar. Vamos começar pela adição de um método sobrecarregado de ação "Criar" para nosso DinnersController que tem um atributo "AcceptVerbs" nele que indica que ele lida com cenários de HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Há uma variedade de maneiras que podemos acessar os parâmetros de formulário postados no nosso método de "Criar" de HTTP-POST habilitado.

Uma abordagem é criar um novo objeto de jantar e, em seguida, use o *UpdateModel()* método auxiliar (como fizemos com a ação Editar) para preenchê-lo com os valores de formulário postados. Em seguida, podemos adicioná-la à nossa DinnerRepository, mantê-lo no banco de dados e redirecionar o usuário para nossa ação de detalhes para mostrar o jantar recém-criado usando o código a seguir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Como alternativa, podemos usar uma abordagem em que temos nosso método de ação Create () utilizam um objeto de jantar como um parâmetro de método. ASP.NET MVC, em seguida, automaticamente instanciar um novo objeto de jantar para nós, preencher as propriedades usando as entradas de formulário e passá-la ao nosso método de ação:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Nosso método de ação acima verifica que o objeto de jantar com êxito preenchido, com os valores de postagem de formulário, verificando a propriedade ModelState. Isso retornará false se não houver problemas de conversão de entrada (por exemplo: uma cadeia de caracteres de "BOGUS" para a propriedade EventDate), e se houver problemas de nosso método de ação exibe novamente o formulário.

Se os valores de entrada forem válidos, o método de ação tenta adicionar e salvar o jantar novo para o DinnerRepository. Ele encapsula esse trabalho dentro de um bloco try/catch e exibe novamente o formulário se há quaisquer violações de regra de negócios (o que faria com que o método dinnerRepository.Save() gerar uma exceção).

Para ver esse comportamento em ação de tratamento de erro, podemos pode solicitar a *jantares/criar* URL e preencha os detalhes sobre um novo jantar. Entrada incorreta ou valores fará com que o formulário Criar ser exibida novamente com os erros realçados como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Observe como o nosso formulário de criação é respeitando as exatas mesmas validação e regras de negócios como nosso formulário de edição. Isso é porque nossas regras de negócios e validação foram definidas no modelo e não estiverem inseridas dentro da interface do usuário ou o controlador do aplicativo. Isso significa que podemos pode posteriormente alterar/evoluir nosso validação ou regras de negócio em uma única colocarem e aplique-os em todo o nosso aplicativo. Não temos que alterar qualquer código dentro de qualquer um de nossos editar ou criar métodos de ação para respeitar automaticamente quaisquer novas regras ou modificações aos existentes.

Ao corrigir os valores de entrada e clique no botão "Salvar" novamente, nossa além de DinnerRepository terá êxito e um jantar novo será adicionado ao banco de dados. Podemos, em seguida, serão redirecionados para o */Dinners/detalhes / [id]* URL – onde podemos verá detalhes sobre o jantar recém-criado:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Excluir o suporte

Agora, vamos adicionar suporte de "Excluir" para nosso DinnersController.

#### <a name="the-http-get-delete-action-method"></a>O método de ação de exclusão de HTTP-GET

Vamos começar implementando o comportamento HTTP GET do nosso método de ação de exclusão. Esse método será chamado quando alguém visita a */Dinners/Delete / [id]* URL. Abaixo está a implementação:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

O método de ação tenta recuperar o jantar a ser excluído. Se o jantar existir, ele renderiza uma exibição baseada em objeto jantar. Se o objeto não existe (ou já foi excluído) ele retorna uma exibição que renderiza o "NotFound" Exibir modelo criado anteriormente para nosso método de ação "Detalhes".

Podemos criar o modelo de exibição de "Excluir" clicando duas vezes dentro do método de ação de exclusão e selecionando o comando de menu de contexto "Adicionar exibição". Na caixa de diálogo "Adicionar exibição" indicaremos que está passando um objeto de jantar ao nosso modelo de exibição como seu modelo e optar por criar um modelo vazio:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Quando clicamos no botão "Adicionar", o Visual Studio irá adicionar um novo arquivo de modelo de exibição de "Delete.aspx" para que possamos dentro do nosso diretório "\Views\Dinners". Vamos adicionar algum HTML e o código para o modelo para implementar uma tela de confirmação de exclusão como abaixo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

O código acima exibe o título do jantar a ser excluído e saídas de um &lt;formulário&gt; elemento que faz um POST para a URL de /Dinners/Delete / [id] se o usuário final clica no botão "Excluir" dentro dele.

Quando executamos nosso aplicativo e o acesso a *"/ jantares/Delete / [id]"* URL em um jantar válido de objeto renderiza a interface do usuário, como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Tópico de lado: Por que estamos fazendo uma POSTAGEM?** |
| --- |
| Você pode perguntar – por que tivemos o esforço de criação de um &lt;formulário&gt; dentro de nossa tela de confirmação de exclusão? Por que não usar apenas um hiperlink padrão para vincular a um método de ação que faz a operação de exclusão real? O motivo é que queremos ter cuidado para proteger contra rastreadores da web e descobrindo nossas URLs e inadvertidamente, fazendo com que dados sejam excluídos quando eles sigam os links de mecanismos de pesquisa. Com base em HTTP-GET URLs são consideradas "seguras" para que eles/rastreamento de acesso, e eles se destinam a seguir não aquelas de HTTP-POST. Uma boa regra é garantir que você sempre colocar destrutivas ou operações de modificação de dados por trás das solicitações HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementando o método de ação de exclusão de HTTP POST

Agora temos a versão de HTTP GET do nosso método de ação de exclusão implementada que exibe uma tela de confirmação de exclusão. Quando um usuário final clica no botão "Excluir", ele executará uma postagem de formulário para o */Dinners/jantar / [id]* URL.

Agora, vamos implementar o comportamento de "POST" de HTTP do método de ação de exclusão usando o código a seguir:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

A versão do HTTP POST do nosso método de ação de exclusão tenta recuperar o objeto de jantar para excluir. Se ele não é possível encontrá-lo (porque ele já foi excluído) ele processa o nosso modelo de "NotFound". Se ele encontrar o jantar, ele exclui-lo a DinnerRepository. Ela então processa um modelo de "Excluído".

Para implementar o modelo de "Excluído" Vamos com o botão direito no método de ação e escolha o menu de contexto "Adicionar exibição". Vamos nomear nossa exibição "Excluído" e tiver de ser um modelo vazio (e não utilizam um objeto de modelo fortemente tipado). Em seguida, vamos adicionar algum conteúdo HTML a ele:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

E agora quando executamos nosso aplicativo e o acesso a *"/ jantares/Delete / [id]"* URL em um jantar válido, confirmação de exclusão de objeto, ele será renderizado da nossa jantar tela como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Quando clicamos no botão "Excluir", ele executará um POST HTTP para o */Dinners/Delete / [id]* URL, que excluirá o jantar do nosso banco de dados e exibir o nosso modelo de exibição de "Excluído":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Segurança de associação de modelo

Discutimos duas maneiras diferentes de usar os recursos de associação de modelos internos do ASP.NET MVC. O primeiro usando o método UpdateModel() para atualizar as propriedades em um objeto de modelo existente e o segundo usando o suporte de ASP.NET MVC para passar objetos de modelo no como parâmetros de método de ação. Ambas as técnicas são muito eficientes e extremamente útil.

Essa capacidade também traz consigo responsabilidade. É importante sempre ser paranoicos sobre a segurança ao aceitar a entrada do usuário, e isso também é verdadeiro quando a associação de objetos a entrada de formulário. Você deve ter cuidado para sempre o HTML codificar quaisquer valores inseridos pelo usuário para evitar ataques de injeção de HTML e JavaScript e tenha cuidado com ataques de injeção de SQL (Observação: estamos usando LINQ to SQL para nosso aplicativo, o que codifica automaticamente os parâmetros para evitar esses tipos de ataques). Você nunca deve depende da validação do lado do cliente apenas e sempre empregar a validação do lado do servidor para se proteger contra hackers tentar enviar a você valores falsos.

Um item de segurança adicional para garantir que você pensar sobre quando usar os recursos de associação do ASP.NET MVC é o escopo dos objetos que você está associando. Especificamente, você deseja certificar-se de que você entenda as implicações de segurança das propriedades que for definida permissão a ser associado e verifique se você permitir que somente as propriedades que realmente devem ser atualizadas por um usuário final a ser atualizada.

Por padrão, o método UpdateModel() tentará atualizar todas as propriedades no objeto de modelo que correspondem aos valores de parâmetro de formulário de entrada. Da mesma forma, podem ter objetos passados como parâmetros de método de ação também por padrão todas as suas propriedades definidas por meio de parâmetros de formulário.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Bloquear a associação em uma base por uso

Você pode bloquear a política de associação em uma base por uso por fornecendo um explícito "incluir lista" de propriedades que podem ser atualizadas. Isso pode ser feito, passando um parâmetro de matriz de cadeia de caracteres extras para o método UpdateModel() como abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Os objetos passados como parâmetros de método de ação também dão suporte a um atributo [Bind] que permite que um "incluir lista" de permitidas propriedades sejam especificadas como abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bloquear a associação em uma base de tipo

Você também pode bloquear as regras de associação em uma base por tipo. Isso permite que você especificar as regras de associação de uma vez e, em seguida, aplique-los em todos os cenários (incluindo os cenários de parâmetro de método UpdateModel e ação) em todos os controladores e métodos de ação.

Você pode personalizar as regras de associação por tipo, adicionando um atributo [Bind] para um tipo ou registrando-o no arquivo global. asax do aplicativo (útil para cenários em que você não possui o tipo). Você pode usar Include a ligação do atributo e Exclude propriedades para controlar as propriedades que são associáveis para a determinada classe ou interface.

Nós vai usar essa técnica para a classe Dinner em nosso aplicativo NerdDinner e adicione um atributo [Bind] a ele que restringe a lista de propriedades vinculáveis ao seguinte:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Observe que não estamos permitindo a coleção RSVPs ser manipulados por meio de associação, nem são estamos permitindo que as propriedades DinnerID ou HostedBy a ser definido por meio de associação. Por motivos de segurança, em vez disso, vai manipular apenas essas propriedades específicas usando código explícito em nossos métodos de ação.

### <a name="crud-wrap-up"></a>Encerramento CRUD

O ASP.NET MVC inclui um número de recursos incorporados que ajudam a implementar cenários de postagem de formulário. Usamos uma variedade desses recursos para fornecer suporte de CRUD da interface do usuário na parte superior do nosso DinnerRepository.

Estamos usando uma abordagem voltada para o modelo para implementar nosso aplicativo. Isso significa que todos os nossos lógica é definida dentro de nossa camada de modelo – e não dentro do nosso controladores ou exibições de regra de negócio e validação. Nossa classe de controlador nem a nossos modelos de exibição saber nada sobre as regras de negócio específico que está sendo impostas por nossa classe de modelo do jantar.

Isso manterá a nossa arquitetura de aplicativo limpa e torná-lo mais fácil de testar. Podemos adicionar regras de negócio adicionais à nossa camada de modelo no futuro e *não precisa fazer nenhuma alteração de código* a nosso controlador ou a exibição para que eles possam ser suportados. Isso vai nos forneceu uma grande quantidade de agilidade para evoluírem e forem alterados no futuro, nosso aplicativo.

Nosso DinnersController agora permite que o jantar listagens/detalhes, bem como criar, editar e excluir suporte. O código completo para a classe pode ser encontrado abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Próxima etapa

Agora temos suporte básico de CRUD (criar, ler, atualizar e excluir) implementar dentro de nossa classe DinnersController.

Agora vejamos como podemos usar classes ViewData e ViewModel para habilitar o ainda mais avançado da interface do usuário em nossos formulários.

> [!div class="step-by-step"]
> [Anterior](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Próximo](use-viewdata-and-implement-viewmodel-classes.md)
