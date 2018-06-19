---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Check-out e o registro | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 9 abrange o registro e o check-out.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e7e83b70f2508b6dfc0c078b992747a76e4d0ff2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870109"
---
<a name="part-9-registration-and-checkout"></a>Parte 9: Check-out e o registro
====================
por [Jon Galloway](https://github.com/jongalloway)

> O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.  
>   
> O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 9 abrange o registro e o check-out.


Nesta seção, estamos criando uma CheckoutController que coletará informações de pagamento e endereço do comprador. Podemos exigirá que os usuários registrem-se em nosso site antes de fazer check-out, para que esse controlador exigirá autorização.

Os usuários navegará para o processo de check-out do seu carrinho de compras clicando no botão "Check-out".

![](mvc-music-store-part-9/_static/image1.jpg)

Se o usuário não estiver conectado, ele serão solicitado a.

![](mvc-music-store-part-9/_static/image1.png)

Após o logon bem-sucedido, o usuário, em seguida, é mostrado a exibição de endereço e pagamento.

![](mvc-music-store-part-9/_static/image2.png)

Depois que tem preenchido o formulário e enviar o pedido, elas serão mostradas a tela de confirmação de ordem.

![](mvc-music-store-part-9/_static/image3.png)

A tentativa de exibir uma ordem não existente ou um pedido que não pertence a você mostrará a exibição de erro.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrando o carrinho de compras

Enquanto o processo de compra é anônimo, quando o usuário clica no botão de check-out, elas serão necessárias para registrar e logon. Os usuários esperam que manteremos suas informações do carrinho de compras entre visitas, portanto, precisamos de informações do carrinho de compras associar um usuário quando ele concluir o registro ou logon.

Isso é realmente muito simple, conforme nossa classe ShoppingCart já tem um método que vai associar todos os itens no carrinho atual com um nome de usuário. Apenas precisamos chamar este método quando o usuário concluir o registro ou logon.

Abra o **AccountController** classe adicionamos quando foram estamos configurando associação e a autorização. Adicionar um usando a instrução faz referência a MvcMusicStore.Models, em seguida, adicione o seguinte método MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Em seguida, modifique a ação de postagem de LogOn para chamar MigrateShoppingCart depois que o usuário é validado, conforme mostrado abaixo:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Fazer a mesma alteração para o registro após a ação, imediatamente depois que a conta de usuário é criada com êxito:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

-- Que é agora um carrinho de compras anônimo serão automaticamente transferido para uma conta de usuário após o registro bem-sucedido ou logon.

## <a name="creating-the-checkoutcontroller"></a>Criando o CheckoutController

Com o botão direito na pasta controladores e adicionar um novo controlador para o projeto chamado CheckoutController usando o modelo de controlador vazio.

![](mvc-music-store-part-9/_static/image5.png)

Primeiro, adicione o atributo de autorizar acima da declaração de classe do controlador para exigir que os usuários se registrem antes do check-out:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Observação: Isso é semelhante para a alteração que são feitas anteriormente para o StoreManagerController, mas nesse caso o atributo de autorização necessário que o usuário seja em uma função de administrador. No controlador de check-out, está exigindo o usuário conectado, mas não exigir que eles sejam administradores.*

Para simplificar, nós não lidar com informações de pagamento neste tutorial. Em vez disso, podemos está permitindo que os usuários façam check-out usando um código promocional. Armazenaremos esse código promocional usando uma constante chamada PromoCode.

Como StoreController, podemos irá declarar um campo para manter uma instância da classe MusicStoreEntities, denominado storeDB. Para fazer uso da classe MusicStoreEntities, precisamos adicionar um usando a instrução do namespace MvcMusicStore.Models. A parte superior do nosso controlador check-out será exibido abaixo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

O CheckoutController terá as seguintes ações do controlador:

**AddressAndPayment (método GET)** exibirá um formulário para permitir que o usuário insira suas informações.

**AddressAndPayment (método POST)** será validar a entrada e processar o pedido.

**Concluída** será mostrado após um usuário concluído com êxito o processo de check-out. Este modo de exibição incluirá o número do pedido do usuário, como confirmação.

Primeiro, vamos renomear a ação de controlador de índice (que foi gerada quando criamos o controlador) para AddressAndPayment. Esta ação de controlador apenas exibe o formulário de check-out, portanto ele não requer qualquer informação de modelo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Nosso método POST AddressAndPayment seguirão o mesmo padrão que usamos o StoreManagerController: ele tentará aceitar o envio do formulário e concluir o pedido e exibirá novamente o formulário se ele falhar.

Após validar a entrada de formulário atende os requisitos de validação para uma ordem, podemos verificará o valor de formulário PromoCode diretamente. Se que tudo estiver correto, que podemos salvará as informações atualizadas com a ordem, informe o objeto ShoppingCart para concluir o processo e redirecionar para a ação.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Após a conclusão bem-sucedida do processo de check-out, os usuários serão redirecionados para a ação de controlador concluída. Esta ação executará uma verificação simple para validar a ordem realmente pertencem ao usuário conectado antes de mostrar o número do pedido como uma confirmação.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Observação: O modo de exibição de erro foi criado automaticamente para que possamos na pasta /Views/Shared quando começamos o projeto.*

O código completo de CheckoutController é o seguinte:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Adicionando o modo de exibição AddressAndPayment

Agora, vamos criar o modo de exibição AddressAndPayment. Clique duas vezes em uma das ações de controlador AddressAndPayment e adicione uma exibição nomeada AddressAndPayment é fortemente tipada como uma ordem e que usa o modelo de edição, conforme mostrado abaixo.

![](mvc-music-store-part-9/_static/image6.png)

Este modo de exibição fará com que o uso de duas das técnicas examinamos ao criar o modo de exibição de StoreManagerEdit:

- Usaremos Html.EditorForModel() para exibir campos de formulário para o modelo de ordem
- Estenderemos usando uma classe de ordem com atributos de validação de regras de validação

Vamos começar atualizando o código de formulário para usar Html.EditorForModel(), seguido por uma caixa de texto adicional para o código de promoção. O código completo para o modo de exibição de AddressAndPayment é mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definir regras de validação para o pedido

Agora que nossa exibição estiver definida, podemos configurará as regras de validação para nosso modelo de ordem como fizemos anteriormente para o modelo de álbum. Com o botão direito na pasta modelos e adicione uma classe denominada ordem. Além dos atributos de validação que foram usados anteriormente para o álbum, também usaremos uma expressão Regular para validar o endereço de email do usuário.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

A tentativa de enviar o formulário com ausente ou inválidas informações agora mostrará a mensagem de erro usando a validação do lado do cliente.

![](mvc-music-store-part-9/_static/image7.png)

Fizemos Okey, a maioria do trabalho de disco rígido para o processo de check-out. Assim, temos alguns termina e de probabilidade para concluir. Precisamos adicionar dois modos de exibição simples e é necessário cuidar da entrega as informações do carrinho durante o processo de logon.

## <a name="adding-the-checkout-complete-view"></a>Adicionando a exibição completa de check-out

A exibição completa de check-out é muito simple, como ele só precisa exibir a ID da ordem. Clique na ação do controlador completa e adicionar uma exibição nomeada concluir que é fortemente tipada como int.

![](mvc-music-store-part-9/_static/image8.png)

Agora atualizaremos o código de exibição para exibir a ID da ordem, conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Atualizando o erro exibição

O modelo padrão inclui uma exibição de erro na pasta compartilhada de modos de exibição para que pode ser reutilizada em outro lugar no site. Este modo de exibição de erro contém um erro muito simple e não usa o nosso site de Layout, portanto vamos atualizá-lo.

Como esta é uma página de erro genérica, o conteúdo é muito simple. Podemos incluirá uma mensagem e um link para navegar até a página anterior no histórico se o usuário deseja tentar novamente a ação.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-8.md)
> [Próximo](mvc-music-store-part-10.md)
