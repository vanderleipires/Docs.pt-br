---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iteração #3 – adicionar validação de formulário (VB) | Microsoft Docs'
author: microsoft
description: Na terceira iteração, podemos adicionar validação de formulário básico. Podemos impedir que pessoas enviando um formulário sem preencher os campos obrigatórios do formulário. Também podemos validar o e-mail...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: a02178bfb662f180343ad32a6453b910e8e70471
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396202"
---
<a name="iteration-3--add-form-validation-vb"></a>Iteração #3 – adicionar validação de formulário (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> Na terceira iteração, podemos adicionar validação de formulário básico. Podemos impedir que pessoas enviando um formulário sem preencher os campos obrigatórios do formulário. Podemos também validar endereços de email e números de telefone.


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

Nesta segunda iteração do aplicativo Contact Manager, podemos adicionar validação de formulário básico. Podemos impedir que pessoas enviando um contato sem inserir valores para os campos de formulário necessária. Podemos também validar números de telefone e endereços de email (veja a Figura 1).


[![A caixa de diálogo Novo projeto](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Figura 01**: um formulário com a validação ([clique para exibir a imagem em tamanho normal](iteration-3-add-form-validation-vb/_static/image2.png))


Essa iteração, adicionamos a lógica de validação diretamente para as ações do controlador. Em geral, isso não é a maneira recomendada para adicionar validação a um aplicativo ASP.NET MVC. Uma abordagem melhor é colocar uma lógica de validação de s do aplicativo em um separado [camada de serviço](http://martinfowler.com/eaaCatalog/serviceLayer.html). Na próxima iteração, podemos refatorar o aplicativo Gerenciador de contatos para tornar o aplicativo mais sustentável.

Nesta iteração, para manter as coisas simples, podemos escrever todo o código de validação manualmente. Em vez de escrever o código de validação sozinhos, podemos pode tirar proveito de uma estrutura de validação. Por exemplo, você pode usar o Microsoft Enterprise Library validação Application Block (VAB) para implementar a lógica de validação para o seu aplicativo ASP.NET MVC. Para saber mais sobre o bloco de aplicativo de validação, consulte:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Adicionar validação ao criar exibição

Deixe o s começar pela adição de lógica de validação para o modo de exibição de criar. Felizmente, como podemos gerado o Create view com o Visual Studio, o modo de exibição criar já contém toda a lógica de interface de usuário necessárias para exibir mensagens de validação. Modo de exibição criar está contido na listagem 1.

**Listagem 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

A classe de erro de validação de campo é usada para definir o estilo a saída processada pelo auxiliar de Html.ValidationMessage(). A classe de erro de validação de entrada é usada para definir o estilo de caixa de texto (entrada) renderizada pelo auxiliar de Html.TextBox(). A classe de erros de resumo de validação é usada para definir o estilo de lista não ordenada renderizada pelo auxiliar de Html.ValidationSummary().

> [!NOTE] 
> 
> Você pode modificar as classes de folha de estilo descritas nesta seção para personalizar a aparência das mensagens de erro de validação.


## <a name="adding-validation-logic-to-the-create-action"></a>Adicionando lógica de validação para a ação de criar

Agora, o modo de exibição criar nunca exibe mensagens de erro de validação porque não poderíamos ter escrito a lógica para gerar todas as mensagens. Para exibir mensagens de erro de validação, você precisará adicionar as mensagens de erro para o ModelState.

> [!NOTE] 
> 
> O método UpdateModel() adiciona mensagens de erro para o ModelState automaticamente quando ocorre um erro de atribuição do valor de um campo de formulário a uma propriedade. Por exemplo, se você tentar atribuir a cadeia de caracteres "apple" a uma propriedade de data de nascimento que aceita valores de data e hora, o método UpdateModel() adiciona um erro para o ModelState.


O método Create () modificado na listagem 2 contém uma nova seção que valida as propriedades da classe Contact antes que o novo contato é inserido no banco de dados.

**Listagem 2 - Controllers\ContactController.vb (criar com validação)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

A seção de validação aplica quatro regras de validação distintos:

- A propriedade FirstName deve ter um comprimento maior que zero (e ele não pode ser formado apenas por espaços)
- A propriedade LastName deve ter um comprimento maior que zero (e ele não pode ser formado apenas por espaços)
- Se a propriedade de telefone tem um valor (tem um comprimento maior que 0) e em seguida, a propriedade de telefone deve corresponder a uma expressão regular.
- Se a propriedade Email tem um valor (tem um comprimento maior que 0) e em seguida, a propriedade Email deve corresponder a uma expressão regular.

Quando há uma violação de regra de validação, uma mensagem de erro é adicionada ao ModelState com a Ajuda do método AddModelError(). Quando você adiciona uma mensagem para o ModelState, você deve fornecer o nome de uma propriedade e o texto de uma mensagem de erro de validação. Essa mensagem de erro é exibida no modo de exibição pelos métodos auxiliares Html.ValidationSummary() e Html.ValidationMessage().

Depois que as regras de validação são executadas, a propriedade IsValid de ModelState é verificada. A propriedade IsValid retorna false quando qualquer mensagem de erro de validação foram adicionadas ao ModelState. Se a validação falhar, o formulário Criar é exibida novamente com as mensagens de erro.

> [!NOTE] 
> 
> Eu tenho as expressões regulares para validar o telefone e endereço de email do repositório em expressões regulares [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Adicionar lógica de validação para a ação Editar

A ação de Edit () atualiza um contato. A ação de Edit () precisa realizar a mesma validação como a ação Create (). Em vez de duplicar o mesmo código de validação, podemos deve refatorar o controlador de contato para que as ações de Create () e Edit () chama o mesmo método de validação.

A classe de controlador de contato modificada está contida na listagem 3. Essa classe tem um novo método ValidateContact() que é chamado dentro de ações de Create () e Edit ().

**Listagem 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Resumo

Essa iteração, adicionamos a validação do formulário básico para nosso aplicativo Contact Manager. Nossa lógica de validação impede que os usuários enviar um novo contato ou editar um contato existente sem fornecer valores para as propriedades FirstName e LastName. Além disso, os usuários deverão fornecer endereços de email e números de telefone válido.

Essa iteração, adicionamos a lógica de validação ao nosso aplicativo Contact Manager da maneira mais fácil possível. No entanto, combinando nossa lógica de validação à nossa lógica do controlador criará problemas para nós a longo prazo. Nosso aplicativo poderá ser mais difícil de manter e modificar ao longo do tempo.

Na próxima iteração, vamos será refatorar nossa lógica de validação e a lógica de acesso de banco de dados fora do nosso controladores. Podemos aproveitar vários princípios de design de software que permitem criar um aplicativo mais flexível e mais sustentável.

> [!div class="step-by-step"]
> [Anterior](iteration-2-make-the-application-look-nice-vb.md)
> [Próximo](iteration-4-make-the-application-loosely-coupled-vb.md)
