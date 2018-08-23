---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Check-out e pagamento com o PayPal | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: b59a395e255823a732aef1b899612063e09b2424
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831608"
---
<a name="checkout-and-payment-with-paypal"></a>Check-out e pagamento com o PayPal
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Este tutorial descreve como modificar o aplicativo de exemplo Wingtip Toys para incluir a autorização do usuário, registro e pagamento usando PayPal. Somente os usuários que estão conectados no terá autorização para adquirir produtos. Funcionalidade de registro de usuário internas do modelo de projeto Web Forms do ASP.NET 4.5 já inclui muito o que você precisa. Você adicionará a funcionalidade de check-out PayPal Express. Neste tutorial você estar usando o desenvolvedor PayPal ambiente, de teste para que nenhum fundos reais serão transferidos. No final do tutorial, você testará o aplicativo, selecionando os produtos para adicionar ao carrinho de compras, clicando no botão de check-out e transferir dados para o site de teste do PayPal. No site de web de teste do PayPal, você confirmar suas informações de envio e de pagamento e, em seguida, retornar ao aplicativo de exemplo Wingtip Toys local para confirmar e concluir a compra.

Há vários processadores de pagamento de terceiros experientes especializados em compras online, essa segurança e escalabilidade do endereço. Os desenvolvedores ASP.NET devem considerar as vantagens de utilização de uma solução de pagamento de terceiros antes de implementar um processo de compras e comprar a solução.

> [!NOTE] 
> 
> O aplicativo de exemplo Wingtip Toys foi projetado para mostrado conceitos específicos do ASP.NET e os recursos disponíveis para os desenvolvedores da web do ASP.NET. Este aplicativo de exemplo não estava otimizado para todas as circunstâncias possíveis em relação à segurança e escalabilidade.


## <a name="what-youll-learn"></a>O que você aprenderá:

- Como restringir o acesso a páginas específicas em uma pasta.
- Como criar um carrinho de compras conhecido de um carrinho de compras anônimo.
- Como habilitar o SSL para o projeto.
- Como adicionar um provedor de OAuth para o projeto.
- Como usar o PayPal para comprar produtos usando o ambiente de teste do PayPal.
- Como exibir detalhes do PayPal em uma **DetailsView** controle.
- Como atualizar o banco de dados do aplicativo Wingtip Toys com detalhes obtidos do PayPal.

## <a name="adding-order-tracking"></a>Adicionando o acompanhamento de pedidos

Neste tutorial, você criará duas novas classes para controlar os dados da ordem de que um usuário criou. As classes irá controlar dados relativos a informações de envio, total de compra e confirmação de pagamento.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Adicionar a ordem e as Classes de modelo de OrderDetail

Anteriormente nessa série de tutoriais, você definiu o esquema para categorias de produtos, e itens de carrinho de compras, criando os `Category`, `Product`, e `CartItem` classes no *modelos* pasta. Agora, você adicionará duas novas classes para definir o esquema para a ordem de produto e os detalhes do pedido.

1. No **modelos** pasta, adicione uma nova classe chamada *{1&gt;Order.CS&lt;1*.   
   O novo arquivo de classe é exibido no editor.
2. Substitua o código padrão pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Adicionar um *OrderDetail.cs* de classe para o *modelos* pasta.
4. Substitua o código padrão pelo código a seguir:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

O `Order` e `OrderDetail` classes contêm o esquema para definir as informações de ordem usadas para compras e envio.

Além disso, você precisará atualizar a classe de contexto do banco de dados que gerencia as classes de entidade e que fornece acesso a dados no banco de dados. Para fazer isso, você adicionará o pedido recentemente criado e `OrderDetail` classes de modelo `ProductContext` classe.

1. Na **Gerenciador de soluções**, encontre e abra o *ProductContext.cs* arquivo.
2. Adicione o código realçado para o *ProductContext.cs* arquivo conforme mostrado abaixo:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Conforme mencionado anteriormente nessa série de tutoriais, o código a *ProductContext.cs* arquivo adiciona o `System.Data.Entity` namespace para que você tenha acesso a toda a funcionalidade de núcleo do Entity Framework. Essa funcionalidade inclui a capacidade de consultar, inserir, atualizar e excluir dados trabalhando com objetos com rigidez de tipos. O código acima na `ProductContext` classe adiciona o acesso do Entity Framework o recém-adicionado `Order` e `OrderDetail` classes.

## <a name="adding-checkout-access"></a>Adicionar acesso de check-out

O aplicativo de exemplo Wingtip Toys permite que usuários anônimos revisar e adicionar produtos a um carrinho de compras. No entanto, quando os usuários anônimos optarem por adquirir os produtos que eles adicionados ao carrinho de compras, ele devem fazer logon no site. Depois que eles fizeram logon, eles podem acessar as páginas restritas do aplicativo Web que lidar com o check-out e o processo de compra. Essas páginas restritas estão contidas na *check-out* pasta do aplicativo.

### <a name="add-a-checkout-folder-and-pages"></a>Adicione uma pasta de check-out e páginas

Agora você criará a *check-out* pasta e as páginas que o cliente verá durante o processo de check-out. Você irá atualizá-las posteriormente no tutorial.

1. Clique com botão direito no nome do projeto (**Wingtip Toys**) em **Gerenciador de soluções** e selecione **adicionar uma nova pasta**. 

    ![Check-out e pagamento com o PayPal - nova pasta](checkout-and-payment-with-paypal/_static/image1.png)
2. Nomeie a nova pasta *check-out*.
3. Clique com botão direito do *check-out* pasta e, em seguida, selecione **Add**-&gt;**Novo Item**. 

    ![Check-out e pagamento com o PayPal - Novo Item](checkout-and-payment-with-paypal/_static/image2.png)
4. A caixa de diálogo **Adicionar Novo Item** é exibida.
5. Selecione o **Visual c#**  - &gt; **Web** grupo de modelos à esquerda. Em seguida, no painel central, selecione **Web Form com página mestra**e nomeie-o *CheckoutStart.aspx*. 

    ![Check-out e pagamento com o PayPal - diálogo Adicionar Novo Item](checkout-and-payment-with-paypal/_static/image3.png)
6. Como antes, selecione a *Master* arquivo como a página mestra.
7. Adicione as seguintes páginas adicionais para o *check-out* pasta usando as mesmas etapas acima:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Adicionar um arquivo Web. config

Adicionando um novo *Web. config* do arquivo para o *check-out* pasta, você poderá restringir o acesso a todas as páginas contidas na pasta.

1. Clique com botão direito do *check-out* pasta e selecione **Add**  - &gt; **Novo Item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual c#**  - &gt; **Web** grupo de modelos à esquerda. Em seguida, no painel central, selecione **arquivo de configuração Web**, aceite o nome padrão de *Web. config*e, em seguida, selecione **adicionar**.
3. Substitua o conteúdo em XML existente a *Web. config* arquivo com o seguinte:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Salvar a *Web. config* arquivo.

O *Web. config* arquivo Especifica que todos os usuários desconhecidos do aplicativo Web devem ser acesso negados às páginas contidos na *check-out* pasta. No entanto, se o usuário tiver registrado com uma conta e fizer logon, eles será um usuário conhecido e terão acesso às páginas do *check-out* pasta.

É importante observar que a configuração do ASP.NET segue uma hierarquia, onde cada *Web. config* arquivo aplica-se as definições de configuração para a pasta em que ele e todos os diretórios filho abaixo dele.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Habilitar o SSL para o projeto

 Seguro SSL (Sockets Layer) é um protocolo definido para permitir que servidores Web e clientes Web para se comunicar de forma mais segura com o uso de criptografia. Quando o SSL não é usado, os dados enviados entre o cliente e servidor são abertos a detecção de pacotes por qualquer pessoa com acesso físico à rede. Além disso, vários esquemas de autenticação comuns não são seguras por HTTP simples. Em particular, a autenticação básica e autenticação de formulários enviam credenciais sem criptografia. Para ser seguro, esses esquemas de autenticação devem usar SSL. 

1. Na **Gerenciador de soluções**, clique no **WingtipToys** do projeto e, em seguida, pressione **F4** para exibir o **propriedades** janela.
2. Alteração **SSL habilitado** para `true`.
3. Cópia de **URL do SSL** para que você pode usá-lo mais tarde.   
 A URL do SSL será `https://localhost:44300/` , a menos que você tenha criado Sites da Web SSL anteriormente (conforme mostrado abaixo).   
    ![Propriedades de projeto](checkout-and-payment-with-paypal/_static/image4.png)
4. Na **Gerenciador de soluções**, clique com botão direito do **WingtipToys** do projeto e clique em **propriedades**.
5. Na guia à esquerda, clique em **Web**.
6. Alterar o **Url do projeto** usar o **URL do SSL** que você salvou anteriormente.   
    ![Propriedades do projeto da Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Salve a página pressionando **CTRL + S**.
8. Pressione **CTRL+F5** para executar o aplicativo. Visual Studio exibirá uma opção para que você possa evitar avisos de SSL.
9. Clique em **Sim** para confiar no certificado SSL do IIS Express e continuar.   
    ![Detalhes do certificado SSL do IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 É exibido um aviso de segurança.
10. Clique em **Sim** para instalar o certificado em seu localhost.   
    ![Caixa de diálogo de aviso de segurança](checkout-and-payment-with-paypal/_static/image7.png)  
 A janela do navegador será exibida.

Agora você pode testar facilmente seu aplicativo Web localmente usando SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Adicionar um provedor OAuth 2.0

Web Forms do ASP.NET oferecem opções aprimoradas de associação e autenticação. Esses aprimoramentos incluem OAuth. OAuth é um protocolo aberto que permite autorização segura em um método simple e padrão da web, aplicativos móveis e da área de trabalho. O modelo de Web Forms do ASP.NET usa OAuth para expor o Facebook, Twitter, Google e Microsoft como provedores de autenticação. Embora este tutorial usa apenas o Google como o provedor de autenticação, você pode facilmente modificar o código para usar qualquer um dos provedores. As etapas para implementar outros provedores são muito semelhantes às etapas que você verá neste tutorial.

Além da autenticação, o tutorial também usará funções para implementar a autorização. Somente os usuários que você adicionar ao `canEdit` função será capaz de alterar dados (criar, editar ou excluir contatos).

> [!NOTE] 
> 
> Aplicativos do Windows Live só aceitam uma URL ao vivo para um site de trabalho, portanto você não pode usar uma URL do site local para logons de teste.


As etapas a seguir permitirá que você adicionar um provedor de autenticação do Google.

1. Abra o *App\_Start\Startup.Auth.cs* arquivo.
2. Remova os caracteres de comentário do `app.UseGoogleAuthentication()` método para que o método é exibido como segue: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navegue até a [Console de desenvolvedores do Google](https://console.developers.google.com/). Você também precisará entrar com sua conta de email de desenvolvedor do Google (gmail.com). Se você não tiver uma conta do Google, selecione a **criar uma conta** link.   
   Em seguida, você verá a **Console de desenvolvedores do Google**.   
    ![Console de desenvolvedores do Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Clique o **criar projeto** e digitar um nome de projeto e a ID (você pode usar os valores padrão). Em seguida, clique o **caixa de seleção de contrato** e o **criar** botão.  

    ![Google - novo projeto](checkout-and-payment-with-paypal/_static/image9.png)

   Em poucos segundos o novo projeto será criado e o navegador exibirá a nova página de projetos.
5. Na guia à esquerda, clique em **APIs &amp; auth**e, em seguida, clique em **credenciais**.
6. Clique o **criar nova ID de cliente** sob **OAuth**.   
   O **criar ID do cliente** caixa de diálogo será exibida.   
    ![Google - criar ID do cliente](checkout-and-payment-with-paypal/_static/image10.png)
7. No **criar ID do cliente** caixa de diálogo, mantenha o padrão **aplicativo Web** para o tipo de aplicativo.
8. Defina a **origens de JavaScript autorizadas** para a URL do SSL usado neste tutorial (`https://localhost:44300/` , a menos que você criou outros projetos SSL).   
   Essa URL é a origem para o seu aplicativo. Para este exemplo, você irá inserir apenas a URL de teste do localhost. No entanto, você pode inserir várias URLs para levar em conta localhost e produção.
9. Defina as **URI de redirecionamento autorizado** ao seguinte: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Esse valor é o URI que o OAuth ASP.NET usuários para se comunicar com o servidor google OAuth. Lembre-se a URL do SSL usado acima ( `https://localhost:44300/` , a menos que você criou outros projetos SSL).
10. Clique o **criar ID do cliente** botão.
11. No menu à esquerda do Console de desenvolvedores do Google, clique no **tela de consentimento** item de menu, em seguida, defina seu nome de produto e o endereço de email. Quando você tiver preenchido o formulário, clique em **salvar**.
12. Clique o **APIs** item de menu, role para baixo e clique no **off** lado **API do Google +**.   
    Aceitar essa opção permitirá que a API do Google +.
13. Você também deve atualizar o **Microsoft. owin** pacote do NuGet para a versão 3.0.0.   
    Dos **ferramentas** menu, selecione **Gerenciador de pacotes NuGet** e, em seguida, selecione **gerenciar pacotes NuGet para solução**.  
    Dos **gerenciar pacotes NuGet** janela, localizar e atualizar o **Microsoft. owin** pacote para a versão 3.0.0.
14. No Visual Studio, atualize o `UseGoogleAuthentication` método da *Startup.Auth.cs* página Copiando e colando o **ID do cliente** e **segredo do cliente** para o método. O **ID do cliente** e **segredo do cliente** valores mostrados abaixo são exemplos e não funcionará. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Pressione **CTRL + F5** para compilar e executar o aplicativo. Clique o **faça logon no** link.
16. Sob **usar outro serviço para fazer logon**, clique em **Google**.  
    ![Iniciar sessão](checkout-and-payment-with-paypal/_static/image11.png)
17. Se você precisar inserir suas credenciais, você será redirecionado ao site do google, onde você vai inserir suas credenciais.  
    ![Google - entrar](checkout-and-payment-with-paypal/_static/image12.png)
18. Depois de inserir suas credenciais, você deverá conceder permissões para o aplicativo web que você acabou de criar.  
    ![Conta de serviço do projeto padrão](checkout-and-payment-with-paypal/_static/image13.png)
19. Clique em **aceitar**. Você será redirecionado para o **registre** página do **WingtipToys** aplicativo onde você pode registrar sua conta do Google.  
    ![Registre-se com sua conta do Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Você tem a opção de alterar o nome de registro de email local utilizado para sua conta do Gmail, mas você geralmente deseja manter o alias de email padrão (ou seja, aquele usado para autenticação). Clique em **faça logon no** conforme mostrado acima.

### <a name="modifying-login-functionality"></a>Modificar a funcionalidade de logon

Conforme mencionado anteriormente nessa série de tutoriais, grande parte da funcionalidade de registro do usuário foi incluído no modelo de Web Forms do ASP.NET por padrão. Agora, você modificará o padrão *login. aspx* e *Register* páginas para chamar o `MigrateCart` método. O `MigrateCart` método associa um usuário conectado recentemente um carrinho de compras anônimo. Ao associar o usuário e o carrinho de compras, o aplicativo de exemplo Wingtip Toys será capaz de manter o carrinho de compras do usuário entre visitas.

1. Na **Gerenciador de soluções**, encontre e abra o *conta* pasta.
2. Modificar a página code-behind chamada *Login.aspx.cs* para incluir o código realçado em amarelo, para que ele apareça da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Salvar a *Login.aspx.cs* arquivo.

Por enquanto, você pode ignorar o aviso que não há nenhuma definição para o `MigrateCart` método. Você será ser adicionando-lo um pouco mais tarde neste tutorial.

O *Login.aspx.cs* arquivo code-behind dá suporte a um método de logon. Inspecionando a página de login. aspx, você verá que essa página inclui um botão "Logon" gatilhos de clique que, quando o `LogIn` manipulador no code-behind.

Quando o `Login` método em de *Login.aspx.cs* é chamado, uma nova instância do carrinho de compras chamado `usersShoppingCart` é criado. A ID do carrinho de compras (um GUID) é recuperada e definida como o `cartId` variável. Em seguida, o `MigrateCart` método é chamado, passando os dois o `cartId` e o nome do usuário conectado a esse método. Quando o carrinho de compras é migrado, o GUID usado para identificar o carrinho de compras anônimo é substituído pelo nome de usuário.

Além de modificar o *Login.aspx.cs* arquivo de code-behind para migrar o carrinho de compras quando o usuário fizer logon, você também deve modificar o *arquivo de code-behind Register.aspx.cs* para migrar o carrinho de compras Quando o usuário cria uma nova conta e fizer logon.

1. No *conta* pasta, abra o arquivo code-behind chamado *Register.aspx.cs*.
2. Modifique o arquivo code-behind, incluindo o código em amarelo, para que ele apareça da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Salvar a *Register.aspx.cs* arquivo. Mais uma vez, ignore o aviso sobre o `MigrateCart` método.

Observe que o código usado na `CreateUser_Click` manipulador de eventos é muito semelhante ao código usado no `LogIn` método. Quando o usuário registra ou faz logon no site, uma chamada para o `MigrateCart` método será feito.

## <a name="migrating-the-shopping-cart"></a>Migrar o carrinho de compras

Agora que você tem o processo de logon e registro atualizado, você pode adicionar o código para migrar o carrinho compras usando o `MigrateCart` método.

1. Na **Gerenciador de soluções**, localize o *lógica* pasta e abra o *ShoppingCartActions.cs* arquivo de classe.
2. Adicione o código realçado em amarelo para o código existente na *ShoppingCartActions.cs* arquivo, para que o código a *ShoppingCartActions.cs* arquivo aparece da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

O `MigrateCart` método usa o cartId existente para localizar o carrinho de compras do usuário. Em seguida, o código executa um loop em todos os itens de carrinho de compras e substitui o `CartId` propriedade (conforme especificado pelo `CartItem` esquema) com o nome de usuário que fez logon.

### <a name="updating-the-database-connection"></a>Atualizando a Conexão de banco de dados

Se você estiver seguindo este tutorial usando o **predefinidos** Wingtip Toys exemplo de aplicativo, você deve recriar o banco de dados de associação padrão. Modificando a cadeia de caracteres de conexão padrão, o banco de dados de associação será criado na próxima vez que o aplicativo é executado.

1. Abra o *Web. config* arquivo na raiz do projeto.
2. Atualize a cadeia de caracteres de conexão padrão para que ele apareça da seguinte maneira:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>A integração do PayPal

PayPal é uma plataforma de cobrança baseado na web que aceita os pagamentos por comerciantes online. Em seguida, este tutorial explica como integrar a funcionalidade do Express de check-out do PayPal no seu aplicativo. Check-out Express permite que seus clientes para usar o PayPal para pagar para os itens que foram adicionados ao carrinho de compras.

### <a name="create-paylpal-test-accounts"></a>Criar contas de teste PaylPal

Para usar o PayPal no ambiente de teste, você deve criar e verificar uma conta de teste do desenvolvedor. Você usará a conta de teste do desenvolvedor para criar um comprador de conta de teste e uma conta de teste do vendedor. As credenciais de conta de teste do desenvolvedor também permitirá que o aplicativo de exemplo Wingtip Toys acessar o ambiente de teste do PayPal.

1. Em um navegador, navegue até o site de teste do desenvolvedor PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Se você não tiver uma conta de desenvolvedor do PayPal, crie uma nova conta clicando **inscrever-se**e seguindo as etapas de inscrição. Se você tiver uma conta existente de desenvolvedor do PayPal, entre no clicando **fazer logon**. Você precisará de sua conta de desenvolvedor do PayPal para testar o aplicativo de exemplo Wingtip Toys mais tarde neste tutorial.
3. Se você apenas se inscreveram para sua conta de desenvolvedor do PayPal, você precisa verificar sua conta de desenvolvedor do PayPal com o PayPal. Você pode verificar sua conta seguindo as etapas que PayPal enviado para sua conta de email. Depois de verificar sua conta de desenvolvedor do PayPal, faça logon novamente no site de teste do desenvolvedor PayPal.
4. Depois que você está conectado ao site do desenvolvedor do PayPal com sua conta de desenvolvedor do PayPal que você precisa criar uma conta de teste do comprador PayPal, se você ainda não tiver uma. Para criar uma conta de teste do comprador, o PayPal, clique em site do **aplicativos** guia e, em seguida, clique em **contas de área restrita**.   
 O **contas de teste de área restrita** página é exibida.   

    > [!NOTE] 
    > 
    > O site do desenvolvedor do PayPal já fornece uma conta de teste comercial.

    ![Check-out e pagamento com o PayPal - contas de área restrita de teste](checkout-and-payment-with-paypal/_static/image15.png)
5. Na página de contas de teste de área restrita, clique em **criar conta**.
6. Sobre o **criar conta de teste** página Escolha um comprador de teste de email da conta e senha de sua escolha.   

    > [!NOTE] 
    > 
    > Você precisará de endereços de email do comprador e a senha para testar o aplicativo de exemplo Wingtip Toys no final deste tutorial.

    ![Check-out e pagamento com o PayPal - contas de área restrita de teste](checkout-and-payment-with-paypal/_static/image16.png)
7. Crie a conta de comprador de teste clicando o **criar conta** botão.  
 O **contas de teste de área restrita** página é exibida. 

    ![Check-out e pagamento com o PayPal - PaylPal contas](checkout-and-payment-with-paypal/_static/image17.png)
8. Sobre o **contas de teste de área restrita** , clique no **facilitador** conta de email.  
    **Perfil** e **notificação** opções são exibidas.
9. Selecione o **perfil** opção e, em seguida, clique em **credenciais de API** para exibir suas credenciais de API para a conta de teste comercial.
10. Copia as credenciais de API de teste para o bloco de notas.

Você precisará suas credenciais de API clássica do teste exibidos (nome de usuário, senha e assinatura) para fazer chamadas à API do aplicativo de exemplo Wingtip Toys do PayPal no ambiente de teste. Você adicionará as credenciais na próxima etapa.

### <a name="add-paypal-class-and-api-credentials"></a>Adicionar classe PayPal e credenciais de API

Coloque a maioria do código PayPal em uma única classe. Essa classe contém métodos usados para se comunicar com o PayPal. Além disso, você adicionará suas credenciais do PayPal para essa classe.

1. No aplicativo de exemplo Wingtip Toys dentro do Visual Studio, clique com botão direito do **lógica** pasta e, em seguida, selecione **Add**  - &gt; **Novo Item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Sob **Visual c#** da **instalado** painel à esquerda, selecione **código**.
3. No painel central, selecione **classe**. Nomeie essa nova classe **PayPalFunctions.cs**.
4. Clique em **Adicionar**.  
   O novo arquivo de classe é exibido no editor.
5. Substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Adicione as credenciais de API do comerciante (nome de usuário, senha e assinatura) que é exibido neste tutorial, para que você possa fazer chamadas de função para o ambiente de teste do PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> Este aplicativo de exemplo simplesmente você está adicionando as credenciais para um arquivo c# (. cs). No entanto, em uma solução implementada, você deve considerar suas credenciais em um arquivo de configuração de criptografia.


A classe NVPAPICaller contém a maioria da funcionalidade PayPal. O código na classe fornece os métodos necessários para fazer um teste de compra do ambiente de teste do PayPal. As três funções PayPal a seguir são usadas para fazer compras:

- `SetExpressCheckout` função
- `GetExpressCheckoutDetails` função
- `DoExpressCheckoutPayment` função

O `ShortcutExpressCheckout` método de coleta os detalhes de produto e informações de compra do teste do carrinho de compras e chama o `SetExpressCheckout` função PayPal. O `GetCheckoutDetails` método confirma os detalhes de compra e chama o `GetExpressCheckoutDetails` PayPal função antes de fazer a compra de teste. O `DoCheckoutPayment` método conclui a compra de teste do ambiente de teste, chamando o `DoExpressCheckoutPayment` função PayPal. O código restante dá suporte a métodos do PayPal e o processo, como cadeias de caracteres de codificação, decodificação de cadeias de caracteres, matrizes de processamento e determinar as credenciais.

> [!NOTE] 
> 
> PayPal permite que você inclua os detalhes de compra opcional com base em [especificação de API do PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Estendendo o código no aplicativo de exemplo Wingtip Toys, você pode incluir detalhes de localização, as descrições de produto, imposto, um número de serviço do cliente, bem como muitos outros campos opcionais.


Observe que as URLs de retorno e de cancelamento que são especificadas na **ShortcutExpressCheckout** método usar um número de porta.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Quando o Visual Web Developer executa um projeto web usando SSL, geralmente a porta 44300 é usada para o servidor web. Como mostrado acima, o número da porta é 44300. Quando você executa o aplicativo, você pode ver um número de porta diferente. Suas necessidades de número de porta seja definido corretamente no código para que você possa bem-sucedida executar o aplicativo de exemplo do Wingtip Toys no final deste tutorial. A próxima seção deste tutorial explica como recuperar o número de porta do host local e atualize a classe do PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Atualizar o número da porta do LocalHost na classe PayPal

O aplicativo de exemplo Wingtip Toys adquire produtos navegando até o site de teste do PayPal e retornar à sua instância local do aplicativo de exemplo Wingtip Toys. Para que PayPal retornar para a URL correta, você precisará especificar o número da porta da execução localmente exemplo de aplicativo no código do PayPal mencionado acima.

1. Clique com botão direito no nome do projeto (**WingtipToys**) em **Gerenciador de soluções** e selecione **propriedades**.
2. Na coluna à esquerda, selecione o **Web** guia.
3. Recuperar o número da porta a **Url do projeto** caixa.
4. Se necessário, atualize o `returnURL` e `cancelURL` na classe PayPal (`NVPAPICaller`) na *PayPalFunctions.cs* arquivo a ser usado o número da porta do aplicativo web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Agora, o código que você adicionou corresponderá a porta esperada para seu aplicativo da Web local. PayPal será capaz de retornar para a URL correta em seu computador local.

### <a name="add-the-paypal-checkout-button"></a>Adicionar o botão de check-out do PayPal

Agora que as funções primárias do PayPal foram adicionadas ao aplicativo de exemplo, você pode começar a adicionar a marcação e o código necessário para chamar essas funções. Primeiro, você deve adicionar o botão de check-out que o usuário verá na página do carrinho de compras.

1. Abra o *ShoppingCart.aspx* arquivo.
2. Role até o final do arquivo e encontre o `<!--Checkout Placeholder -->` comentário.
3. Substitua o comentário com um `ImageButton` controlar de forma que a marca de backup é substituída da seguinte maneira:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. No *ShoppingCart.aspx.cs* do arquivo, após o `UpdateBtn_Click` manipulador de eventos perto do fim do arquivo, adicione o `CheckOutBtn_Click` manipulador de eventos:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Também na *ShoppingCart.aspx.cs* do arquivo, adicione uma referência para o `CheckoutBtn`, de modo que o novo botão de imagem é referenciado como se segue:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Salve as alterações em ambos os *ShoppingCart.aspx* arquivo e o *ShoppingCart.aspx.cs* arquivo.
7. No menu, selecione **Debug**-&gt;**Build WingtipToys**.  
   O projeto será recriado com recém-adicionada **ImageButton** controle.

### <a name="send-purchase-details-to-paypal"></a>Enviar detalhes de compra para PayPal

Quando o usuário clica o **check-out** botão na página do carrinho de compras (*ShoppingCart.aspx*), ele começará o processo de compra. O código a seguir chama a primeira função PayPal necessária comprar produtos.

1. Dos *check-out* pasta, abra o arquivo code-behind chamado *CheckoutStart.aspx.cs*.   
   Certifique-se de abrir o arquivo code-behind.
2. Substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Quando o usuário do aplicativo clica o **check-out** botão sobre a página de carrinho de compras, o navegador vai navegar para o *CheckoutStart.aspx* página. Quando o *CheckoutStart.aspx* página for carregada, o `ShortcutExpressCheckout` método é chamado. Neste ponto, o usuário é transferido para o site de teste do PayPal. No site do PayPal, o usuário insere suas credenciais do PayPal, examina os detalhes de compra, aceitar o contrato do PayPal e retorna para o aplicativo de exemplo Wingtip Toys onde o `ShortcutExpressCheckout` método é concluído. Quando o `ShortcutExpressCheckout` método for concluído, ele redirecionará o usuário para o *CheckoutReview.aspx* página especificada no `ShortcutExpressCheckout` método. Isso permite que o usuário revisar os detalhes do pedido de dentro do aplicativo de exemplo Wingtip Toys.

### <a name="review-order-details"></a>Examine os detalhes de ordem

Depois de retornar do PayPal, o *CheckoutReview.aspx* página do aplicativo de exemplo Wingtip Toys exibe os detalhes do pedido. Esta página permite que o usuário examine os detalhes do pedido antes de adquirir os produtos. O *CheckoutReview.aspx* página deve ser criada da seguinte maneira:

1. No *check-out* pasta, abra a página chamada *CheckoutReview.aspx*.
2. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Abra a página code-behind chamada *CheckoutReview.aspx.cs* e substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

O **DetailsView** controle é usado para exibir os detalhes do pedido que foram retornados do PayPal. Além disso, o código acima salva os detalhes do pedido no banco de dados da Wingtip Toys como um `OrderDetail` objeto. Quando o usuário clica na **ordem completo** botão, eles são redirecionados para o *CheckoutComplete.aspx* página.

> [!NOTE] 
> 
> **Dica**
> 
> Na marcação do *CheckoutReview.aspx* página, observe que o `<ItemStyle>` marca é usada para alterar o estilo dos itens dentro de **DetailsView** controle na parte inferior da página. Exibindo a página no **modo de exibição de Design** (selecionando **Design** no canto inferior esquerdo do Visual Studio), selecionando o **DetailsView** controlar e, em seguida, selecionando o  **Marca inteligente** (o ícone de seta na parte superior direita do controle), você será capaz de ver as **DetailsView tarefas**.
> 
> ![Check-out e pagamento com o PayPal - editar campos](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Selecionando **editar campos**, o **campos** caixa de diálogo será exibida. Na caixa de diálogo você pode controlar facilmente as propriedades visuais, tais como **ItemStyle**, da **DetailsView** controle.
> 
> ![Check-out e pagamento com o PayPal - caixa de diálogo campos](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Finalizar compra

*CheckoutComplete.aspx* página faz a compra do PayPal. Conforme mencionado acima, o usuário deve clicar na **ordem completo** botão antes do aplicativo navegará para o *CheckoutComplete.aspx* página.

1. No *check-out* pasta, abra a página chamada *CheckoutComplete.aspx*.
2. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Abra a página code-behind chamada *CheckoutComplete.aspx.cs* e substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Quando o *CheckoutComplete.aspx* página é carregada, o `DoCheckoutPayment` método é chamado. Como mencionado anteriormente, o `DoCheckoutPayment` método completa a compra do ambiente de teste do PayPal. Depois que PayPal foi concluída a compra do pedido, o *CheckoutComplete.aspx* página exibe uma transação de pagamento `ID` ao comprador.

### <a name="handle-cancel-purchase"></a>Manipular o Cancelar compra

Se o usuário decidir cancelar a compra, serão direcionados para o *CheckoutCancel.aspx* página em que eles verão sua ordem foi cancelada.

1. Abra a página chamada *CheckoutCancel.aspx* na *check-out* pasta.
2. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Tratar erros de compra

Erros durante o processo de compra serão tratados pelos *CheckoutError.aspx* página. O code-behind do *CheckoutStart.aspx* página, o *CheckoutReview.aspx* página e o *CheckoutComplete.aspx* cada página será redirecionada para o  *CheckoutError.aspx* página se ocorrer um erro.

1. Abra a página chamada *CheckoutError.aspx* na *check-out* pasta.
2. Substitua a marcação existente pelo seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

O *CheckoutError.aspx* página é exibida com os detalhes do erro quando ocorre um erro durante o processo de check-out.

## <a name="running-the-application"></a>Executando o aplicativo

Execute o aplicativo para ver como comprar produtos. Observe que serão executados do PayPal ambiente de teste. Não há dinheiro real que está sendo trocado.

1. Verifique se todos os arquivos são salvos no Visual Studio.
2. Abra um navegador da Web e navegue até [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Faça logon com sua conta de desenvolvedor do PayPal que você criou neste tutorial.  
   Para a área restrita do desenvolvedor do PayPal, você precisa estar conectado ao [ https://developer.paypal.com ](https://developer.paypal.com/) para testar o check-out express. Isso se aplica somente à área restrita do PayPal teste, não para o ambiente ao vivo do PayPal.
4. No Visual Studio, pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
   Depois que recria o banco de dados, o navegador será aberto e mostrará os *default. aspx* página.
5. Adicione três produtos diferentes ao carrinho de compras selecionando a categoria de produto, como "Carros" e, em seguida, clicando em **adicionar ao carrinho** ao lado de cada produto.  
   O carrinho de compras exibirá o produto que você selecionou.
6. Clique o **PayPal** botão fazer check-out. 

    ![Check-out e pagamento com o PayPal - carrinho](checkout-and-payment-with-paypal/_static/image20.png)

   Fazendo check-out exigirá que você tenha uma conta de usuário para o aplicativo de exemplo Wingtip Toys.
7. Clique o **Google** link à direita da página para fazer logon com uma conta de email gmail.com existente.  
   Se você não tiver uma conta de gmail.com, você pode criar um para testes no [www.gmail.com](https://www.gmail.com/). Você também pode usar uma conta local padrão, clicando em "Registrar". 

    ![Check-out e pagamento com o PayPal - faça logon no](checkout-and-payment-with-paypal/_static/image21.png)
8. Entre com sua conta do gmail e senha. 

    ![Check-out e pagamento com o PayPal - Gmail de entrada](checkout-and-payment-with-paypal/_static/image22.png)
9. Clique o **faça logon no** botão registrar sua conta do gmail com seu nome de usuário do aplicativo de exemplo Wingtip Toys. 

    ![Check-out e pagamento com o PayPal - registro de conta](checkout-and-payment-with-paypal/_static/image23.png)
10. No site de teste do PayPal, adicione sua **comprador** endereço de email e senha que você criou neste tutorial e, em seguida, clique no **Log In** botão. 

    ![Check-out e pagamento com o PayPal - PayPal de entrada](checkout-and-payment-with-paypal/_static/image24.png)
11. Concordar com a política do PayPal e clique no **Concordo e continuar** botão.  
    Observe que esta página só é exibida na primeira vez que você usar essa conta PayPal. Novamente, observe que esta é uma conta de teste, nenhum dinheiro real é trocado. 

    ![Check-out e pagamento com o PayPal - política PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Examine as informações de pedido do PayPal Testando a página de revisão do ambiente e clique em **continuar**. 

    ![Check-out e pagamento com o PayPal - informações de análise](checkout-and-payment-with-paypal/_static/image26.png)
13. Sobre o *CheckoutReview.aspx* página, verifique se o valor do pedido e exibir o endereço de envio gerado. Em seguida, clique o **ordem completo** botão. 

    ![Check-out e pagamento com o PayPal - revisão de ordem](checkout-and-payment-with-paypal/_static/image27.png)
14. O **CheckoutComplete.aspx** página é exibida com uma ID de transação de pagamento. 

    ![Check-out e pagamento com o PayPal - check-out completa](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Revisando o banco de dados

Ao analisar os dados atualizados no banco de dados de aplicativo de exemplo Wingtip Toys depois de executar o aplicativo, você pode ver que o aplicativo registrada com êxito a compra de produtos.

Você pode inspecionar os dados contidos na *Wingtiptoys.mdf* arquivo de banco de dados usando o **Database Explorer** janela (**Server Explorer** janela no Visual Studio) como você fez anteriormente nessa série de tutoriais.

1. Se ele ainda estiver aberto, feche a janela do navegador.
2. No Visual Studio, selecione o **Show All Files** ícone na parte superior da **Gerenciador de soluções** para permitir que você expanda o **aplicativo\_dados** pasta.
3. Expanda o **App\_dados** pasta.  
 Talvez você precise selecionar o **Show All Files** ícone da pasta.
4. Clique com botão direito do *Wingtiptoys.mdf* arquivo de banco de dados e selecione **abrir**.  
    **Gerenciador de servidores** é exibida.
5. Expanda o **tabelas** pasta.
6. Clique com botão direito do **pedidos**de tabela e selecione **Mostrar dados da tabela**.  
 O **pedidos** tabela é exibida.
7. Examine os **PaymentTransactionID** coluna para confirmar transações com êxito. 

    ![Check-out e pagamento com o PayPal - banco de dados de revisão](checkout-and-payment-with-paypal/_static/image29.png)
8. Fechar o **pedidos** janela da tabela.
9. No Gerenciador de servidores, clique com botão direito do **OrderDetails** de tabela e selecione **Mostrar dados da tabela**.
10. Examine os `OrderId` e `Username` os valores na **OrderDetails** tabela. Observe que esses valores correspondem a `OrderId` e `Username` valores incluídos na **pedidos** tabela.
11. Fechar o **OrderDetails** janela da tabela.
12. Clique no arquivo de banco de dados da Wingtip Toys (*Wingtiptoys.mdf*) e selecione **fechar Conexão**.
13. Se você não vir as **Gerenciador de soluções** janela, clique em **Gerenciador de soluções** na parte inferior da **Gerenciador de servidores** janela para mostrar o **Gerenciador de soluções**  novamente.

## <a name="summary"></a>Resumo

Neste tutorial, você adicionou ordem e esquemas de detalhes do pedido para acompanhar a compra de produtos. Você também PayPal funcionalidade integrada no aplicativo de exemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionais

[Visão geral da configuração do ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Implantar um aplicativo de formulários da Web do ASP.NET seguro com associação, OAuth e banco de dados SQL no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este tutorial contém código de exemplo. Esse código de exemplo é fornecido "como está" sem garantias de qualquer tipo. Da mesma forma, a Microsoft não garante a precisão, integridade ou a qualidade do código de exemplo. Você concorda em usar o código de exemplo em seu próprio risco. Sob nenhuma circunstância a Microsoft Corporation é responsável de qualquer maneira por qualquer código de exemplo, conteúdo, incluindo, mas não se limitando a quaisquer erros ou omissões em qualquer código de exemplo, conteúdo, ou qualquer perda ou dano de qualquer tipo incorrido em decorrência do uso de qualquer código de exemplo. Você será notificado por meio deste e concorda em indenizar, salvar e armazenar contra perda de toda e qualquer, declarações de perda, lesões ou dano de qualquer tipo incluindo, sem limitação, aqueles occasioned por ou decorrentes do material que você postar, a Microsoft transmitir, usar ou contar incluindo, mas sem limitação, as opiniões expressadas contido nele.

> [!div class="step-by-step"]
> [Anterior](shopping-cart.md)
> [Próximo](membership-and-administration.md)
