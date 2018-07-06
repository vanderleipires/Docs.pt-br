---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Registro e check-out | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 9 aborda o registro e check-out.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812867"
---
<a name="part-9-registration-and-checkout"></a>Parte 9: Registro e check-out
====================
por [Jon Galloway](https://github.com/jongalloway)

> A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.  
>   
> A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.  
>   
> Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 9 aborda o registro e check-out.


Nesta seção, criaremos um CheckoutController que coletará informações de pagamento e endereço do comprador. Será exigido que os usuários registrem-se em nosso site antes de fazer check-out, portanto, esse controlador exigirá autorização.

Os usuários navegará para o processo de check-out do carrinho de compras, clicando no botão "Check-out".

![](mvc-music-store-part-9/_static/image1.jpg)

Se o usuário não estiver conectado, ele serão solicitados a.

![](mvc-music-store-part-9/_static/image1.png)

Após o logon bem-sucedido, o usuário, em seguida, é mostrado o modo de exibição de endereço e o pagamento.

![](mvc-music-store-part-9/_static/image2.png)

Depois que eles tem preenchido o formulário e enviar o pedido, eles serão mostrados a tela de confirmação do pedido.

![](mvc-music-store-part-9/_static/image3.png)

A tentativa de exibir uma ordem não existente ou um pedido que não pertence a você mostrará a exibição de erro.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrar o carrinho de compras

Enquanto o processo de compra é anônimo, quando o usuário clica no botão de check-out, serão solicitados a registrar e faça logon. Os usuários esperam que vamos manter suas informações do carrinho de compras entre visitas, portanto, precisamos de informações do carrinho de compras associar um usuário quando concluírem o registro ou logon.

Isso é realmente muito simple de fazer, como a nossa classe ShoppingCart já tem um método que será associada a todos os itens no carrinho atual um nome de usuário. Apenas precisamos chamar esse método quando o usuário concluir o registro ou logon.

Abra o **AccountController** classe que adicionamos ao foram podemos configurar associação e a autorização. Adicione um usando a instrução que faz referência MvcMusicStore.Models, em seguida, adicione o seguinte método MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Em seguida, modifique a ação de postagem de LogOn para chamar MigrateShoppingCart depois que o usuário tiver sido validado, conforme mostrado abaixo:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Fazer com que a mesma alteração para o registro de postagem de ação, imediatamente depois que a conta de usuário é criada com êxito:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Isso é tudo - agora um carrinho de compras anônimo serão automaticamente transferido para uma conta de usuário após o registro foi bem-sucedido ou logon.

## <a name="creating-the-checkoutcontroller"></a>Criando o CheckoutController

Clique com botão direito na pasta Controllers e adicionar um novo controlador ao projeto nomeado CheckoutController usando o modelo de controlador vazia.

![](mvc-music-store-part-9/_static/image5.png)

Primeiro, adicione o atributo Authorize acima da declaração de classe de controlador para exigir que os usuários registrem antes do check-out:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Observação: Isso é semelhante à alteração que fizemos anteriormente para o StoreManagerController, mas nesse caso, o atributo Authorize necessário que o usuário seja em uma função de administrador. No controlador de check-out, podemos está exigindo que o usuário estar conectado, mas não exigir que eles sejam administradores.*

Para simplificar, estamos não lidar com informações de pagamento neste tutorial. Em vez disso, estamos permitindo que os usuários façam check-out usando um código promocional. Armazenaremos esse código promocional usando uma constante chamada código promocional.

Como no StoreController, podemos declarar um campo para armazenar uma instância da classe MusicStoreEntities, chamada storeDB. Para fazer uso da classe MusicStoreEntities, precisamos adicionar uma instrução para o namespace MvcMusicStore.Models. A parte superior do nosso controlador de check-out é exibida abaixo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

O CheckoutController terá as seguintes ações do controlador:

**AddressAndPayment (método GET)** exibirá um formulário para permitir que o usuário insira suas informações.

**AddressAndPayment (método POST)** validará a entrada e processar o pedido.

**Completa** será mostrada após um usuário foi concluído com êxito o processo de check-out. Este modo de exibição incluirá o número do pedido do usuário, como confirmação.

Primeiro, vamos renomear a ação do controlador de índice (que foi gerada quando criamos o controlador) para AddressAndPayment. Esta ação do controlador apenas exibe o formulário de check-out, portanto, ele não requer qualquer informação de modelo.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Nosso método POST AddressAndPayment seguirá o mesmo padrão que usamos o StoreManagerController: ele tentará aceitar o envio do formulário e concluir o pedido e exibirá novamente o formulário se ele falhar.

Depois de validar a entrada de formulário atende aos nossos requisitos de validação para um pedido, serão verificamos o valor de formulário de código promocional diretamente. Supondo que tudo está correto, vamos salvar as informações atualizadas com a ordem, informar ao objeto ShoppingCart para concluir o processo de pedido e redirecionar para a ação de conclusão.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Após a conclusão bem-sucedida do processo de check-out, os usuários serão redirecionados para a ação do controlador completo. Essa ação executará uma verificação simple para validar que a ordem realmente pertencem ao usuário conectado antes de mostrar o número do pedido como uma confirmação.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Observação: A exibição de erro foi criada automaticamente para nós na pasta /Views/Shared quando começamos o projeto.*

O código completo do CheckoutController é da seguinte maneira:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Adicionando o modo de exibição AddressAndPayment

Agora, vamos criar o modo de exibição AddressAndPayment. Clique duas vezes em uma das ações de controlador AddressAndPayment e adicione uma exibição nomeada AddressAndPayment que é fortemente tipada como um pedido e usa o modelo de edição, conforme mostrado abaixo.

![](mvc-music-store-part-9/_static/image6.png)

Este modo de exibição fará com que usam duas das técnicas que vimos durante a criação de exibição de StoreManagerEdit:

- Nós usaremos Html.EditorForModel() para exibir campos de formulário para o modelo de ordem
- Aproveitaremos usando uma classe de ordem com atributos de validação de regras de validação

Vamos começar atualizando o código de formulário para usar Html.EditorForModel(), seguido por uma caixa de texto adicional para o código promocional. O código completo para o modo de exibição AddressAndPayment é mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definindo regras de validação para o pedido

Agora que nossa exibição é definida, definiremos as regras de validação para nosso modelo de ordem como fizemos anteriormente para o modelo de álbum. Clique com botão direito na pasta modelos e adicione uma classe denominada ordem. Além dos atributos de validação que foram usados anteriormente para o álbum, também usaremos uma expressão Regular para validar o endereço de email do usuário.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

A tentativa de enviar o formulário com ausente ou informações inválidas agora mostrará a mensagem de erro usando a validação do lado do cliente.

![](mvc-music-store-part-9/_static/image7.png)

Okey, fizemos a maioria do trabalho pesado para o processo de check-out; Nós só temos alguns Miscelânea para concluir. Precisamos adicionar dois modos de exibição simples, e precisamos para cuidar da entrega das informações de carrinho durante o processo de logon.

## <a name="adding-the-checkout-complete-view"></a>Adicionando a visão completa do check-out

A visão completa do check-out é muito simple, porque ele só precisa exibir a ID de ordem. Clique com botão direito na ação de controlador completo e adicionar uma exibição nomeada concluir que é fortemente tipada como um int.

![](mvc-music-store-part-9/_static/image8.png)

Agora, atualizaremos o código de exibição para exibir a ID de ordem, conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Atualizando o erro exibição

O modelo padrão inclui uma exibição de erro na pasta de exibições de compartilhado para que ele pode ser reutilizado em outro lugar no site. Este modo de exibição de erro contém um erro muito simple e não usa o nosso site de Layout, portanto, iremos atualizá-lo.

Como essa é uma página de erro genérica, o conteúdo é muito simple. Incluiremos uma mensagem e um link para navegar até a página anterior no histórico se o usuário deseja tentar novamente sua ação.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-8.md)
> [Próximo](mvc-music-store-part-10.md)
