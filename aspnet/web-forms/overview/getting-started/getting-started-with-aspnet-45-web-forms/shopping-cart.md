---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Carrinho de compras | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 2823970144e6dce4a3a9f4d28dabfabe9fe053d6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824198"
---
<a name="shopping-cart"></a>Carrinho de compras
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Este tutorial descreve a lógica comercial necessária para adicionar um carrinho de compras para o aplicativo de Web Forms do ASP.NET de exemplo Wingtip Toys. Este tutorial se baseia no tutorial anterior "Exibir dados de itens e detalhes" e faz parte da série de tutoriais de Wingtip Toys Store. Depois de concluir este tutorial, os usuários do seu aplicativo de exemplo será capazes de adicionar, remover e modificar os produtos no carrinho de compras.

## <a name="what-youll-learn"></a>O que você aprenderá:

1. Como criar um carrinho de compras para o aplicativo web.
2. Como habilitar usuários adicionar itens ao carrinho de compras.
3. Como adicionar um [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) controle para exibir os detalhes de carrinho de compras.
4. Como calcular e exibir o total do pedido.
5. Como remover e atualizar itens no carrinho de compras.
6. Como incluir um contador de carrinho de compras.

## <a name="code-features-in-this-tutorial"></a>Recursos de código neste tutorial:

1. Entity Framework Code First
2. Anotações de dados
3. Fortemente tipado a controles de dados
4. Associação de modelos

## <a name="creating-a-shopping-cart"></a>Criando um carrinho de compras

Anteriormente nesta série de tutoriais, você adicionou páginas e código para exibir dados de produto de um banco de dados. Neste tutorial, você criará um carrinho de compras para gerenciar os produtos que os usuários estão interessados em comprar. Os usuários poderão procurar e adicionar itens ao carrinho de compras, mesmo se eles são registrados ou não conectados. Para gerenciar o acesso de carrinho de compras, você atribuirá usuários um único `ID` usando um identificador global exclusivo (GUID) quando o usuário acessa o shopping cart pela primeira vez. Você armazená-la `ID` usando o estado de sessão do ASP.NET.

> [!NOTE] 
> 
> O estado de sessão do ASP.NET é um local conveniente para armazenar informações específicas do usuário que irá expirar depois que o usuário deixar o site. Embora o uso indevido do estado da sessão pode ter implicações de desempenho em sites maiores, claro do uso de sessão estado funciona bem para fins de demonstração. O projeto de exemplo Wingtip Toys mostra como usar o estado de sessão sem um provedor externo, em que o estado de sessão é armazenado em processo no servidor web que hospeda o site. Locais maiores que fornecem várias instâncias de um aplicativo ou para sites que executam várias instâncias de um aplicativo em diferentes servidores, considere usar **serviço de Cache do Windows Azure**. Esse serviço de Cache fornece um serviço de cache distribuído que é externo ao site da web e resolve o problema de usar o estado de sessão em processo. Para obter mais informações, consulte [como usar estado de sessão do ASP.NET com o Windows Azure Web Sites](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Adicionar CartItem como uma classe de modelo

Anteriormente nessa série de tutoriais, você definiu o esquema para os dados de categoria e produto, criando o `Category` e `Product` as classes a *modelos* pasta. Agora, adicione uma nova classe para definir o esquema para o carrinho de compras. Mais tarde neste tutorial, você adicionará uma classe para tratar o acesso a dados para o `CartItem` tabela. Essa classe fornece a lógica de negócios para adicionar, remover e atualizar itens no carrinho de compras.

1. Clique com botão direito do *modelos* pasta e selecione **Add**  - &gt; **Novo Item**. 

    ![Carrinho de compras - Novo Item](shopping-cart/_static/image1.png)
2. A caixa de diálogo **Adicionar Novo Item** é exibida. Selecione **código**e, em seguida, selecione **classe**. 

    ![-O carrinho de compras diálogo Adicionar Novo Item](shopping-cart/_static/image2.png)
3. Nomeie essa nova classe *CartItem.cs*.
4. Clique em **Adicionar**.  
   O novo arquivo de classe é exibido no editor.
5. Substitua o código padrão pelo código a seguir:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

O `CartItem` classe contém o esquema que define cada produto que um usuário adiciona ao carrinho de compras. Essa classe é semelhante a outras classes de esquema que você criou anteriormente na série de tutoriais. Por convenção, espera Entity Framework Code First que a chave primária para o `CartItem` tabela será `CartItemId` ou `ID`. No entanto, o código substitui o comportamento padrão usando a anotação de dados `[Key]` atributo. O `Key` atributo da propriedade ItemId Especifica que o `ItemID` propriedade é a chave primária.

O `CartId` propriedade especifica o `ID` do usuário que está associado com o item para comprar. Você adicionará código para criar esse usuário `ID` quando o usuário acessa o carrinho de compras. Isso `ID` também será armazenada como uma variável de sessão do ASP.NET.

### <a name="update-the-product-context"></a>Atualizar o contexto do produto

Além de adicionar o `CartItem` classe, você precisará atualizar a classe de contexto do banco de dados que gerencia as classes de entidade e que fornece acesso a dados no banco de dados. Para fazer isso, você adicionará o recém-criado `CartItem` modelo de classe para o `ProductContext` classe.

1. No **Gerenciador de soluções**, encontre e abra o *ProductContext.cs* arquivo o *modelos* pasta.
2. Adicione o código realçado para o *ProductContext.cs* arquivos da seguinte maneira:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Conforme mencionado anteriormente nessa série de tutoriais, o código a *ProductContext.cs* arquivo adiciona o `System.Data.Entity` namespace para que você tenha acesso a toda a funcionalidade de núcleo do Entity Framework. Essa funcionalidade inclui a capacidade de consultar, inserir, atualizar e excluir dados trabalhando com objetos com rigidez de tipos. O `ProductContext` classe adiciona o acesso ao recém-adicionada `CartItem` classe de modelo.

### <a name="managing-the-shopping-cart-business-logic"></a>Gerenciando a lógica de negócios compras do carrinho

Em seguida, você criará a `ShoppingCart` classe em uma nova *lógica* pasta. O `ShoppingCart` classe controla o acesso a dados para o `CartItem` tabela. A classe também incluirá a lógica de negócios para adicionar, remover e atualizar itens no carrinho de compras.

A lógica de carrinho de compras que você irá adicionar conterá a funcionalidade para gerenciar as seguintes ações:

1. Adicionando itens ao carrinho de compras
2. Removendo itens do carrinho de compras
3. Obtendo a ID de carrinho de compras
4. Recuperando itens do carrinho de compras
5. Totalizando o valor de todos os itens de carrinho de compras
6. Atualizando os dados do carrinho de compras

Uma página de compras (*ShoppingCart.aspx*) e a classe de carrinho de compras será usado em conjunto para acessar dados do carrinho de compras. A página de carrinho de compras exibirá todos os itens que o usuário adiciona ao carrinho de compras. Além do carrinho de compras página e classe, você criará uma página (*AddToCart.aspx*) para adicionar produtos ao carrinho de compras. Você também adicionará código para o *ProductList. aspx* página e o *ProductDetails.aspx* página que fornecerá um link para o *AddToCart.aspx* de página, para que o usuário pode adicionar produtos ao carrinho de compras.

O diagrama a seguir mostra o processo básico que ocorre quando o usuário adiciona um produto ao carrinho de compras.

![Carrinho de compras – adicionar ao carrinho de compras](shopping-cart/_static/image3.png)

Quando o usuário clica o **adicionar ao carrinho** em um link da *ProductList. aspx* página ou o *ProductDetails.aspx* página, o aplicativo navegará para o *AddToCart.aspx* página e, em seguida, automaticamente para a *ShoppingCart.aspx* página. O *AddToCart.aspx* página adicionará o selecionar o produto ao carrinho de compras, chamando um método na classe ShoppingCart. O *ShoppingCart.aspx* página exibirá os produtos que foram adicionados ao carrinho de compras.

#### <a name="creating-the-shopping-cart-class"></a>Criando a classe de carrinho de compras

O `ShoppingCart` classe será adicionado a uma pasta separada no aplicativo para que haja uma distinção clara entre o modelo (pasta de modelos), as páginas (pasta raiz) e a lógica (pasta lógica).

1. No **Gerenciador de soluções**, clique com botão direito do **WingtipToys**do projeto e selecione **adicionar**-&gt;**nova pasta**. Nomeie a nova pasta *lógica*.
2. Clique com botão direito do *lógica* pasta e, em seguida, selecione **Add**  - &gt; **Novo Item**.
3. Adicionar um novo arquivo de classe chamado *ShoppingCartActions.cs*.
4. Substitua o código padrão pelo código a seguir:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

O `AddToCart` método permite que os produtos individuais a serem incluídos no carrinho de compras com base no produto `ID`. O produto é adicionado ao carrinho, ou se o carrinho já contém um item para o produto, a quantidade é incrementada.

O `GetCartId` método retorna o carrinho `ID` para o usuário. O carrinho `ID` é usado para rastrear os itens que um usuário tem em seu carrinho de compras. Se o usuário não tiver um carrinho existente `ID`, um novo carrinho `ID` é criado para eles. Se o usuário está conectado como um usuário registrado, o carrinho `ID` é definido como seu nome de usuário. No entanto, se o usuário não está conectado no carrinho `ID` é definido como um valor exclusivo (um GUID). Um GUID garante que apenas um carrinho é criado para cada usuário, com base em sessão.

O `GetCartItems` método retorna uma lista de itens para o usuário do carrinho de compras. Mais tarde neste tutorial, você verá que a associação de modelo é usada para exibir os itens de carrinho de compras carrinho usando o `GetCartItems` método.

### <a name="creating-the-add-to-cart-functionality"></a>Criando a funcionalidade de adicionar ao carrinho

Como mencionado anteriormente, você criará uma página de processamento de chamada *AddToCart.aspx* que será usado para adicionar novos produtos ao carrinho de compras do usuário. Esta página será chamada a `AddToCart` método no `ShoppingCart` classe que você acabou de criar. O *AddToCart.aspx* página esperarão que um produto `ID` é passado para ele. Este produto `ID` será usado ao chamar o `AddToCart` método o `ShoppingCart` classe.

> [!NOTE] 
> 
> Vamos modificar o code-behind (*AddToCart.aspx.cs*) para essa página, não na página da interface do usuário (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Para criar o adicionar ao carrinho funcionalidade:

1. No **Gerenciador de soluções**, com o botão direito do **WingtipToys**do projeto, clique em **Add**  - &gt; **Novo Item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Adicionar uma nova página padrão (Web Form) para o aplicativo chamado *AddToCart.aspx*. 

    ![-O carrinho de compras Adicionar formulário da Web](shopping-cart/_static/image4.png)
3. Na **Gerenciador de soluções**, clique com botão direito do *AddToCart.aspx* página e, em seguida, clique em **Exibir código**. O *AddToCart.aspx.cs* arquivo code-behind é aberto no editor.
4. Substitua o código existente na *AddToCart.aspx.cs* de lógica com o seguinte:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Quando o *AddToCart.aspx* página é carregada, o produto `ID` é recuperado da cadeia de consulta. Em seguida, uma instância da classe de carrinho de compras é criada e usada para chamar o `AddToCart` método que você adicionou anteriormente neste tutorial. O `AddToCart` método, contido na *ShoppingCartActions.cs* de arquivos, que inclui a lógica para adicionar o produto selecionado ao carrinho de compras ou aumentar a quantidade de produtos do produto selecionado. Se o produto não tenha sido adicionado ao carrinho de compras, o produto é adicionado para o `CartItem` tabela do banco de dados. Se o produto já foi adicionado ao carrinho de compras e o usuário adiciona um item adicional do mesmo produto, a quantidade de produtos é incrementada no `CartItem` tabela. Por fim, a página redireciona de volta para o *ShoppingCart.aspx* página que você adicionará na próxima etapa, em que o usuário vê uma lista atualizada dos itens no carrinho.

Como mencionado anteriormente, um usuário `ID` é usado para identificar os produtos que estão associados um usuário específico. Isso `ID` é adicionado a uma linha de `CartItem` cada vez que o usuário adiciona um produto ao carrinho de compras de tabela.

### <a name="creating-the-shopping-cart-ui"></a>Criando o carrinho de compras da interface do usuário

O *ShoppingCart.aspx* página exibirá os produtos que o usuário foi adicionado ao carrinho de compras. Ele também fornecerá a capacidade de adicionar, remover e atualizar itens no carrinho de compras.

1. Na **Gerenciador de soluções**, clique com botão direito **WingtipToys**, clique em **Add**  - &gt; **Novo Item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Adicione uma nova página (formulário da Web) que inclui uma página mestra, selecionando **formulário da Web usando página mestra**. Nomeie a nova página *ShoppingCart.aspx*.
3. Selecione **Master** anexar a página mestra recém-criado *. aspx* página.
4. No *ShoppingCart.aspx* página, substitua a marcação existente pela seguinte marcação:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

O *ShoppingCart.aspx* página inclui uma **GridView** controle chamado `CartList`. Esse controle usa associação de modelo para associar os dados do carrinho de compras do banco de dados para o **GridView** controle. Quando você define o `ItemType` propriedade do **GridView** controlar, a expressão de associação de dados `Item` está disponível na marcação do controle e o controle torna-se com rigidez de tipos. Conforme mencionado anteriormente nesta série de tutoriais, você pode selecionar detalhes do `Item` usando o IntelliSense de objeto. Para configurar um controle de dados para usar a associação de modelo para selecionar dados, defina o `SelectMethod` propriedade do controle. Na marcação, você deve definir a `SelectMethod` usar o método GetShoppingCartItems que retorna uma lista de `CartItem` objetos. O **GridView** chama o método no momento apropriado no ciclo de vida da página de controle de dados e associa automaticamente os dados retornados. O `GetShoppingCartItems` método ainda deve ser adicionado.

#### <a name="retrieving-the-shopping-cart-items"></a>Recuperando os itens do carrinho de compras

Em seguida, adicione o código para o *ShoppingCart.aspx.cs* code-behind para recuperar e preencher a interface de usuário do carrinho de compras.

1. Na **Gerenciador de soluções**, clique com botão direito do *ShoppingCart.aspx* página e, em seguida, clique em **Exibir código**. O *ShoppingCart.aspx.cs* arquivo code-behind é aberto no editor.
2. Substitua o código existente pelo seguinte:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Conforme mencionado acima, o `GridView` chamadas de controle de dados a `GetShoppingCartItems` ciclo de método apropriado no momento o ciclo de vida da página e associa automaticamente os dados retornados. O `GetShoppingCartItems` método cria uma instância da `ShoppingCartActions` objeto. Em seguida, o código usa essa instância para retornar os itens no carrinho chamando o `GetCartItems` método.

### <a name="adding-products-to-the-shopping-cart"></a>Adicionar produtos ao carrinho de compras

Quando ambos os *ProductList. aspx* ou o *ProductDetails.aspx* página for exibida, o usuário será capaz de adicionar o produto ao carrinho de compras usando um link. Quando eles clicarem no link, o aplicativo navega para a página de processamento de chamada *AddToCart.aspx*. O *AddToCart.aspx* página chamará o `AddToCart` método no `ShoppingCart` classe que você adicionou anteriormente neste tutorial.

Agora, você adicionará um **adicionar ao carrinho** link para ambos os *ProductList. aspx* página e o *ProductDetails.aspx* página. Esse link incluirá o produto `ID` que é recuperado do banco de dados.

1. Na **Gerenciador de soluções**, encontre e abra a página chamada *ProductList. aspx*.
2. Adicione a marcação realçada em amarelo para o *ProductList. aspx* página para que toda a página seja exibida da seguinte maneira:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testando o carrinho de compras

Execute o aplicativo para ver como adicionar produtos ao carrinho de compras.

1. Pressione **F5** para executar o aplicativo.  
 Depois que o projeto recria o banco de dados, o navegador será aberto e mostrará os *default. aspx* página.
2. Selecione **carros** no menu de navegação de categoria.  
 O *ProductList. aspx* página é exibida, mostrando apenas os produtos incluídos na categoria "Carros". 

    ![Carrinho de compras - carros](shopping-cart/_static/image5.png)
3. Clique o **adicionar ao carrinho** link ao lado do primeiro produto listado (o carro conversível).   
 O *ShoppingCart.aspx* página for exibida, mostrando a seleção no carrinho de compras. 

    ![Carrinho - carrinho de compras](shopping-cart/_static/image6.png)
4. Exibir produtos adicionais, selecionando **planos** no menu de navegação de categoria.
5. Clique o **adicionar ao carrinho** link ao lado do primeiro produto listado.  
 O *ShoppingCart.aspx* página é exibida com o item adicional.
6. Feche o navegador.

### <a name="calculating-and-displaying-the-order-total"></a>Calcular e exibir o Total do pedido

Além de adicionar produtos ao carrinho de compras, você adicionará uma `GetTotal` método para o `ShoppingCart` de classe e exibir o valor total do pedido na página do carrinho de compras.

1. No **Gerenciador de soluções**, abra o *ShoppingCartActions.cs* arquivo o *lógica* pasta.
2. Adicione o seguinte `GetTotal` método realçado em amarelo para o `ShoppingCart` de classe, para que a classe aparece da seguinte maneira:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Primeiro, o `GetTotal` método obtém a ID do carrinho de compras para o usuário. Em seguida, o método obtém o carrinho total multiplicando o preço do produto pela quantidade de produto para cada produto listado no carrinho.

> [!NOTE] 
> 
> O código acima usa o tipo anulável "`int?`". Tipos que permitem valor nulos podem representar todos os valores de um tipo subjacente e também como um valor nulo. Para obter mais informações, consulte [usando tipos anuláveis](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Modificar a exibição de carrinho de compras

Em seguida, você modificará o código para o *ShoppingCart.aspx* página a ser chamada a `GetTotal` método e exibição total no *ShoppingCart.aspx* página quando a página for carregada.

1. Na **Gerenciador de soluções**, clique com botão direito do *ShoppingCart.aspx* página e selecione **Exibir código**.
2. No *ShoppingCart.aspx.cs* do arquivo, atualize o `Page_Load` manipulador adicionando o seguinte código realçado em amarelo:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Quando o *ShoppingCart.aspx* página for carregada, ele carrega o objeto de carrinho de compras e, em seguida, recupera o total do carrinho de compras por meio da chamada a `GetTotal` método da `ShoppingCart` classe. Se o carrinho de compras está vazio, uma mensagem para esse efeito será exibida.

### <a name="testing-the-shopping-cart-total"></a>Testando o Total do carrinho de compras

Execute o aplicativo agora para ver como você não só pode adicionar um produto ao carrinho de compras, mas você pode ver o total do carrinho de compras.

1. Pressione **F5** para executar o aplicativo.  
 O navegador será aberto e mostrará os *default. aspx* página.
2. Selecione **carros** no menu de navegação de categoria.
3. Clique o **adicionar ao carrinho** link ao lado do primeiro produto.   
 O *ShoppingCart.aspx* página é exibida com o total do pedido. 

    ![Total do carrinho de compras de carrinho-](shopping-cart/_static/image7.png)
4. Adicione alguns outros produtos (por exemplo, um plano) ao carrinho.
5. O *ShoppingCart.aspx* página é exibida com um total atualizado para todos os produtos que você adicionou. 

    ![Carrinho de compras - vários produtos](shopping-cart/_static/image8.png)
6. Interrompa o aplicativo em execução, fechando a janela do navegador.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Adicionar botões de check-out e a atualização ao carrinho de compras

Para permitir que os usuários modifiquem o carrinho de compras, você adicionará um **atualização** botão e uma **check-out** botão à página do carrinho de compras. O **check-out** botão não é usado até que mais tarde nessa série de tutoriais.

1. Na **Gerenciador de soluções**, abra o *ShoppingCart.aspx* página na raiz do projeto de aplicativo web.
2. Para adicionar o **atualização** botão e o **check-out** botão para o *ShoppingCart.aspx* página, adicione a marcação realçada em amarelo a marcação existente, conforme mostrado no o código a seguir:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Quando o usuário clica o **atualização** botão, o `UpdateBtn_Click` manipulador de eventos será chamado. Esse manipulador de eventos chamará o código que você adicionará na próxima etapa.

Em seguida, você pode atualizar o código contido na *ShoppingCart.aspx.cs* arquivo para executar um loop por meio de itens do carrinho e chamar o `RemoveItem` e `UpdateItem` métodos.

1. Na **Gerenciador de soluções**, abra o *ShoppingCart.aspx.cs* arquivo na raiz do projeto de aplicativo da web.
2. Adicione as seguintes seções de código realçadas em amarelo para o *ShoppingCart.aspx.cs* arquivo:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Quando o usuário clica o **atualização** botão a *ShoppingCart.aspx* página, o método UpdateCartItems é chamado. O método UpdateCartItems obtém os valores atualizados para cada item no carrinho de compras. Em seguida, chama o método UpdateCartItems o `UpdateShoppingCartDatabase` método (adicionado e explicado na próxima etapa) para adicionar ou remover itens do carrinho de compras. Depois que o banco de dados foi atualizado para refletir as atualizações ao carrinho de compras, o **GridView** controle será atualizado na página do carrinho de compras por meio da chamada a `DataBind` método para o **GridView**. Além disso, o valor total do pedido na página do carrinho de compras é atualizado para refletir a lista atualizada de itens.

### <a name="updating-and-removing-shopping-cart-items"></a>Atualizar e remover itens do carrinho de compras

Sobre o *ShoppingCart.aspx* página, você pode ver os controles foram adicionados para a quantidade de um item de atualização e remoção de um item. Agora, adicione o código que verifique esses controles funcionam.

1. No **Gerenciador de soluções**, abra o *ShoppingCartActions.cs* arquivo o *lógica* pasta.
2. Adicione o seguinte código realçado em amarelo para o *ShoppingCartActions.cs* arquivo de classe:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

O `UpdateShoppingCartDatabase` método, chamado do `UpdateCartItems` método o *ShoppingCart.aspx.cs* de página, que contém a lógica para atualizar ou remover itens do carrinho de compras. O `UpdateShoppingCartDatabase` método percorre todas as linhas dentro da lista de carrinho de compras. Se um item do carrinho de compras foi marcado para ser removido ou a quantidade for menor que 1, o `RemoveItem` método é chamado. Caso contrário, o item do carrinho de compras é verificado para atualizações quando o `UpdateItem` método é chamado. Depois que o item do carrinho de compras foi removido ou atualizado, as alterações de banco de dados são salvos.

O `ShoppingCartUpdates` estrutura é usada para manter todos os itens de carrinho de compras. O `UpdateShoppingCartDatabase` usa o `ShoppingCartUpdates` estrutura para determinar se qualquer um dos itens precisará ser atualizado ou removido.

O próximo tutorial, você aprenderá a usar o `EmptyCart` método para limpar as compras do carrinho após a compra de produtos. Mas por enquanto, você usará o `GetCount` método que você acabou de adicionar para o *ShoppingCartActions.cs* arquivo para determinar quantos itens estão no carrinho de compras.

### <a name="adding-a-shopping-cart-counter"></a>Adicionar um contador de carrinho de compras

Para permitir que o usuário exibir o número total de itens no carrinho de compras, você irá adicionar um contador para o *Master* página. Esse contador também atuará como um link para o carrinho de compras.

1. Na **Gerenciador de soluções**, abra o *Master* página.
2. Modificar a marcação, adicionando o link de contador carrinho de compras, conforme mostrado em amarelo para a seção de navegação para que ele apareça da seguinte maneira:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Em seguida, atualize o code-behind do *Site.Master.cs* arquivo adicionando o código realçado em amarelo, da seguinte maneira:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Antes da página é renderizada como HTML, o `Page_PreRender` é gerado. No `Page_PreRender` manipulador, a contagem total de carrinho de compras é determinado pela chamada a `GetCount` método. O valor retornado é adicionado para o `cartCount` span incluído na marcação do *Master* página. O `<span>` marcas permite que os elementos internos sejam renderizados corretamente. Quando qualquer página do site é exibida, o total do carrinho de compras será exibida. O usuário também pode clicar com o total do carrinho de compras para exibir o carrinho de compras.

## <a name="testing-the-completed-shopping-cart"></a>Teste concluído carrinho de compras

Você pode executar o aplicativo agora para ver como você pode adicionar, exclusão e atualização de itens no carrinho de compras. O total do carrinho de compras refletirá o custo total de todos os itens no carrinho de compras.

1. Pressione **F5** para executar o aplicativo.  
 O navegador é aberta e mostra a *default. aspx* página.
2. Selecione **carros** no menu de navegação de categoria.
3. Clique o **adicionar ao carrinho** link ao lado do primeiro produto.   
 O *ShoppingCart.aspx* página é exibida com o total do pedido.
4. Selecione **planos** no menu de navegação de categoria.
5. Clique o **adicionar ao carrinho** link ao lado do primeiro produto.
6. Defina a quantidade do primeiro item no carrinho de compras para 3 e selecione o **Remover Item** caixa de seleção do segundo item.<a id="a"></a>
7. Clique o **atualizar** botão para atualizar a página de carrinho de compras e exibir o novo total do pedido. 

    ![Carrinho de compras - atualização de carrinho](shopping-cart/_static/image9.png)

## <a name="summary"></a>Resumo

Neste tutorial, você criou um carrinho de compras para o aplicativo de exemplo Wingtip Toys Web Forms. Durante este tutorial, você usou Entity Framework Code First, as anotações de dados, controles de dados fortemente tipados e associação de modelos.

O carrinho de compras dá suporte a adicionar, excluir e atualizar itens que o usuário tiver selecionado para compra. Além de implementar a funcionalidade de carrinho de compras, você aprendeu como exibir itens de carrinho de compras em uma **GridView** controlar e calcular o total do pedido.

## <a name="addition-information"></a>Obter informações adicionais

[Visão geral do estado de sessão ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Anterior](display_data_items_and_details.md)
> [Próximo](checkout-and-payment-with-paypal.md)
