---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: "Iteração #3 – adicionar validação do formulário (VB) | Microsoft Docs"
author: microsoft
description: "Na terceira iteração, podemos adicionar validação de forma básica. Vamos impedir que pessoas enviando um formulário sem concluir os campos obrigatórios do formulário. Além disso, podemos validar e-mail..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: e9ed182fb58addd8c5dadbe6e3d09c391840ca00
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="iteration-3--add-form-validation-vb"></a>Iteração #3 – adicionar validação do formulário (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> Na terceira iteração, podemos adicionar validação de forma básica. Vamos impedir que pessoas enviando um formulário sem concluir os campos obrigatórios do formulário. Além disso, podemos validar endereços de email e números de telefone.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (VB)
  

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

Essa segunda iteração do aplicativo Gerenciador de contato, adicionamos validação de forma básica. Vamos impedir que pessoas enviando um contato sem inserir valores para os campos obrigatórios do formulário. Além disso, podemos validar números de telefone e endereços de email (consulte a Figura 1).


[![A caixa de diálogo Novo projeto](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Figura 01**: um formulário com validação ([clique para exibir a imagem em tamanho normal](iteration-3-add-form-validation-vb/_static/image2.png))


Essa iteração, adicionamos a lógica de validação diretamente para as ações do controlador. Em geral, isso não é a maneira recomendada para adicionar validação a um aplicativo ASP.NET MVC. Uma abordagem melhor é colocar uma lógica de validação do aplicativo s em uma separada [camada de serviço](http://martinfowler.com/eaaCatalog/serviceLayer.html). Na próxima iteração, podemos refatorar o aplicativo Gerenciador de contato para tornar o aplicativo mais fácil manutenção.

Essa iteração, para manter as coisas simples, vamos escrever todo o código de validação manualmente. Em vez de escrever código de validação de nós, podemos pode tirar proveito de uma estrutura de validação. Por exemplo, você pode usar o Microsoft Enterprise Library validação aplicativo bloco (VAB) para implementar a lógica de validação para o seu aplicativo ASP.NET MVC. Para saber mais sobre o bloco de aplicativo de validação, consulte:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Adicionando validação para o modo de criação

Permitir que o s comece adicionando lógica de validação para o modo de criação. Felizmente, porque é gerado o Create view com o Visual Studio, o modo de exibição de criar já contém toda a lógica de interface de usuário necessários para exibir mensagens de validação. O modo de exibição de criar está contido na listagem 1.

**Listando 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

A classe de erro de validação de campo é usada para definir o estilo a saída processada pelo auxiliar de Html.ValidationMessage(). A classe de erro de validação de entrada é usada para definir o estilo de caixa de texto (entrada) processado pelo auxiliar de Html.TextBox(). A classe de erros de resumo de validação é usada para definir o estilo de lista não ordenada renderizada pelo auxiliar de Html.ValidationSummary().

> [!NOTE] 
> 
> Você pode modificar as classes de folha de estilo descritas nesta seção para personalizar a aparência das mensagens de erro de validação.


## <a name="adding-validation-logic-to-the-create-action"></a>Adicionando a lógica de validação a criar ação

Neste momento, a criar nunca exibe mensagens de erro de validação porque nós não foram gravadas a lógica para gerar todas as mensagens. Para exibir mensagens de erro de validação, você precisa adicionar as mensagens de erro para o ModelState.

> [!NOTE] 
> 
> O método UpdateModel() adiciona mensagens de erro para o ModelState automaticamente quando há um erro ao atribuir o valor de um campo de formulário a uma propriedade. Por exemplo, se você tentar atribuir a cadeia de caracteres "apple" em uma propriedade de data de nascimento que aceita valores de data e hora, o método UpdateModel() adiciona um erro ao ModelState.


O método Create () modificado na listagem 2 contém uma nova seção que valida as propriedades da classe contato antes do novo contato é inserido no banco de dados.

**A listagem 2 - Controllers\ContactController.vb (criar com validação)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

A seção de validar impõe quatro regras de validação distintos:

- A propriedade de nome deve ter um comprimento maior que zero (e ele não pode conter somente espaços)
- A propriedade LastName deve ter um comprimento maior que zero (e ele não pode conter somente espaços)
- Se a propriedade de telefone tem um valor (tem um comprimento maior que 0), em seguida, a propriedade do telefone deve corresponder a uma expressão regular.
- Se a propriedade de Email tem um valor (tem um comprimento maior que 0), em seguida, a propriedade de Email deve corresponder a uma expressão regular.

Quando há uma violação de regra de validação, uma mensagem de erro é adicionada ao ModelState com a Ajuda do método AddModelError(). Quando você adiciona uma mensagem para o ModelState, forneça o nome de uma propriedade e o texto de uma mensagem de erro de validação. Essa mensagem de erro é exibida no modo de exibição, os métodos auxiliares Html.ValidationSummary() e Html.ValidationMessage().

Depois que as regras de validação são executadas, a propriedade IsValid de ModelState é verificada. A propriedade IsValid retorna false quando as mensagens de erro de validação foram adicionadas ao ModelState. Se a validação falhar, o formulário de criação é exibida novamente com as mensagens de erro.

> [!NOTE] 
> 
> Recebi as expressões regulares para validar o telefone e endereço de email do repositório de expressão regular no [ *http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Adicionando lógica de validação para a ação de edição

A ação Edit() atualiza um contato. A ação Edit() precisa executar a mesma validação como a ação Create (). Em vez de duplicar o mesmo código de validação, podemos deve refatorar o controlador de contato para que as ações de Create () e Edit() chamar o mesmo método de validação.

A classe do controlador contato modificada está contida na listagem 3. Essa classe tem um novo método ValidateContact() que é chamado dentro de Create () e Edit() ações.

**A listagem 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Resumo

Essa iteração, adicionamos validação do formulário básico para nosso aplicativo Gerenciador de contato. Nossa lógica de validação impede que os usuários enviar um novo contato ou editar um contato existente sem fornecer valores para as propriedades de nome e sobrenome. Além disso, os usuários devem fornecer endereços de email e números de telefone válido.

Essa iteração, adicionamos a lógica de validação ao nosso aplicativo Gerenciador de contato da maneira mais fácil possível. No entanto, misturar nossa lógica de validação em nossa lógica do controlador criará problemas para que possamos a longo prazo. Nosso aplicativo poderá ser mais difícil de manter e modificar ao longo do tempo.

Na próxima iteração, podemos será refatorar nossa lógica de validação e a lógica de acesso a banco de dados fora de nosso controladores. Vamos dar aproveitar vários princípios de design de software que permitem a criação de um aplicativo mais flexível e mais fácil manutenção.

>[!div class="step-by-step"]
[Anterior](iteration-2-make-the-application-look-nice-vb.md)
[Próximo](iteration-4-make-the-application-loosely-coupled-vb.md)
