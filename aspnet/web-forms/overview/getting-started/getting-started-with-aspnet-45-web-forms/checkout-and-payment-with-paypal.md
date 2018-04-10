---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Check-out e pagamento com PayPal | Microsoft Docs
author: Erikre
description: Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 0dba613594686a28b82bc6d7701cda6e24b82e2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="checkout-and-payment-with-paypal"></a>Check-out e pagamento com o PayPal
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com o código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Este tutorial descreve como modificar o aplicativo de exemplo Wingtip Toys para incluir a autorização do usuário, o registro e pagamento usando PayPal. Somente os usuários que estão conectados no terá autorização para adquirir produtos. Funcionalidade de registro de usuário internas do modelo de projeto Web Forms do ASP.NET 4.5 já inclui muito o que você precisa. Você irá adicionar a funcionalidade de check-out PayPal Express. Neste tutorial você esteja usando o desenvolvedor do PayPal testar ambiente, por isso nenhuma fundos reais serão transferidos. No final do tutorial, você testará o aplicativo selecionando produtos para adicionar ao carrinho de compras, clicando no botão de check-out e transferir dados para o site de teste do PayPal. No site da web do teste PayPal, você confirme suas informações de envio e de pagamento e, em seguida, retornar ao aplicativo de exemplo Wingtip Toys local para confirmar e concluir a compra.

Há vários processadores de pagamento de terceiros experientes especializados em compras online, esse endereço escalabilidade e a segurança. Os desenvolvedores ASP.NET devem considerar as vantagens do uso de uma solução de pagamento de terceiros antes da implementação de compras e solução de compra.

> [!NOTE] 
> 
> O aplicativo de exemplo Wingtip Toys foi projetado para mostrado conceitos específicos do ASP.NET e os recursos disponíveis para os desenvolvedores da web do ASP.NET. Este aplicativo de exemplo não foi otimizado para todas as circunstâncias possíveis em termos de escalabilidade e segurança.


## <a name="what-youll-learn"></a>O que você aprenderá:

- Como restringir o acesso a páginas específicas em uma pasta.
- Como criar um carrinho de compras conhecido de um carrinho de compras anônimo.
- Como habilitar o SSL para o projeto.
- Como adicionar um provedor OAuth para o projeto.
- Como usar o PayPal a adquirir produtos usando o ambiente de teste do PayPal.
- Como exibir detalhes do PayPal em uma **DetailsView** controle.
- Como atualizar o banco de dados do aplicativo Wingtip Toys com detalhes obtidos do PayPal.

## <a name="adding-order-tracking"></a>Adicionando controle de ordem

Neste tutorial, você criará duas novas classes para acompanhar os dados da ordem de que um usuário foi criado. As classes de controlar dados sobre informações de envio, o total de compra e a confirmação de pagamento.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Adicionar a ordem e as Classes de modelo OrderDetail

Anteriormente na série de tutoriais, você definiu o esquema para categorias de produtos, e os itens do carrinho de compras criando o `Category`, `Product`, e `CartItem` classes de *modelos* pasta. Agora você adicionará duas novas classes para definir o esquema para a ordem de produto e os detalhes do pedido.

1. No **modelos** pasta, adicionar uma nova classe chamada *Order.cs*.   
   O novo arquivo de classe é exibido no editor.
2. Substitua o código padrão pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Adicionar uma *OrderDetail.cs* de classe para o *modelos* pasta.
4. Substitua o código padrão pelo seguinte código:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

O `Order` e `OrderDetail` classes contêm o esquema para definir as informações de ordem usadas para adquirir e envio.

Além disso, você precisará atualizar a classe de contexto de banco de dados que gerencia as classes de entidade e que fornece acesso a dados no banco de dados. Para fazer isso, você adicionará a ordem recém-criado e `OrderDetail` classes de modelo `ProductContext` classe.

1. Em **Solution Explorer**, localize e abra a *ProductContext.cs* arquivo.
2. Adicione o código realçado para o *ProductContext.cs* arquivo conforme mostrado abaixo:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Conforme mencionado anteriormente nesta série tutorial, o código de *ProductContext.cs* arquivo adiciona o `System.Data.Entity` namespace para que você tenha acesso a toda a funcionalidade de núcleo do Entity Framework. Essa funcionalidade inclui a capacidade de consultar, inserir, atualizar e excluir dados, trabalhando com objetos fortemente tipados. O código acima no `ProductContext` classe adiciona acesso do Entity Framework para recém-adicionado `Order` e `OrderDetail` classes.

## <a name="adding-checkout-access"></a>Adicionar acesso de check-out

O aplicativo de exemplo Wingtip Toys permite que usuários anônimos analisar e adicionar produtos a um carrinho de compras. No entanto, quando os usuários anônimos optarem por comprar os produtos que eles adicionados ao carrinho de compras, eles devem fazer logon no site. Depois que eles fizeram logon, eles podem acessar as páginas restritas do aplicativo Web que manipulam o check-out e o processo de compra. Essas páginas restritas estão contidas no *check-out* pasta do aplicativo.

### <a name="add-a-checkout-folder-and-pages"></a>Adicionar uma pasta de check-out e páginas

Agora você criará o *check-out* pasta e as páginas em que o cliente verá durante o processo de check-out. Você irá atualizar essas páginas mais tarde neste tutorial.

1. Clique no nome do projeto (**Wingtip Toys**) em **Solution Explorer** e selecione **adicionar uma nova pasta**. 

    ![Check-out e pagamento com PayPal - nova pasta](checkout-and-payment-with-paypal/_static/image1.png)
2. Nomeie a nova pasta *check-out*.
3. Clique com botão direito do *check-out* pasta e, em seguida, selecione **adicionar**-&gt;**Novo Item**. 

    ![Check-out e pagamento com PayPal - Novo Item](checkout-and-payment-with-paypal/_static/image2.png)
4. A caixa de diálogo **Adicionar Novo Item** é exibida.
5. Selecione o **Visual C#**  - &gt; **Web** grupo de modelos, à esquerda. Em seguida, no painel central, selecione **Web Form com página mestra**e nomeie-o *CheckoutStart.aspx*. 

    ![Check-out e pagamento com PayPal - adicionar a caixa de diálogo Novo Item](checkout-and-payment-with-paypal/_static/image3.png)
6. Como antes, selecione o *Site.Master* arquivo como a página mestra.
7. Adicione as seguintes páginas adicionais para o *check-out* pasta usando as mesmas etapas acima:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Adicionar um arquivo Web. config

Adicionando um novo *Web. config* o arquivo para o *check-out* pasta, você poderá restringir o acesso a todas as páginas contidas na pasta.

1. Clique com botão direito do *check-out* pasta e selecione **adicionar**  - &gt; **Novo Item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual C#**  - &gt; **Web** grupo de modelos, à esquerda. Em seguida, no painel central, selecione **arquivo de configuração Web**, aceite o nome padrão de *Web. config*e, em seguida, selecione **adicionar**.
3. Substitua o conteúdo existente do XML de *Web. config* arquivo com o seguinte:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Salve o *Web. config* arquivo.

O *Web. config* arquivo Especifica que todos os usuários desconhecidos do aplicativo Web devem ter acesso a páginas contidas a *check-out* pasta. No entanto, se o usuário registrou uma conta e estiver conectado, será um usuário conhecido e terão acesso às páginas de *check-out* pasta.

É importante observar que a configuração do ASP.NET segue uma hierarquia, onde cada *Web. config* arquivo aplica-se as definições de configuração para a pasta que está no e para todos os diretórios filho abaixo dele.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Habilitar o SSL para o projeto

 SSL Secure Sockets Layer () é um protocolo definido para permitir que os servidores Web e clientes da Web para se comunicar com mais segurança com o uso de criptografia. Quando o SSL não é usado, os dados enviados entre cliente e servidor são abertos a pacote detecção por qualquer pessoa com acesso físico à rede. Além disso, vários esquemas de autenticação comuns não são seguras por HTTP sem formatação. Em particular, a autenticação básica e autenticação de formulários enviam credenciais sem criptografia. Para ser seguro, esses esquemas de autenticação devem usar SSL. 

1. Em **Solution Explorer**, clique no **WingtipToys** projeto e, em seguida, pressione **F4** para exibir o **propriedades** janela.
2. Alterar **SSL habilitado** para `true`.
3. Copie o **SSL URL** para usá-lo mais tarde.   
 A URL de SSL será `https://localhost:44300/` , a menos que você criou anteriormente Sites da Web de SSL (conforme mostrado abaixo).   
    ![Propriedades de projeto](checkout-and-payment-with-paypal/_static/image4.png)
4. Em **Solution Explorer**, clique com botão direito do **WingtipToys** do projeto e clique em **propriedades**.
5. Na guia à esquerda, clique em **Web**.
6. Alterar o **Url do projeto** para usar o **SSL URL** que você salvou anteriormente.   
    ![Propriedades do projeto da Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Salve a página pressionando **CTRL + S**.
8. Pressione **CTRL+F5** para executar o aplicativo. O Visual Studio exibirá uma opção para que você possa evitar avisos de SSL.
9. Clique em **Sim** para confiar no certificado SSL do IIS Express e continuar.   
    ![Detalhes do certificado SSL do IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 É exibido um aviso de segurança.
10. Clique em **Sim** para instalar o certificado para o localhost.   
    ![Caixa de diálogo de aviso de segurança](checkout-and-payment-with-paypal/_static/image7.png)  
 A janela do navegador será exibida.

Agora você pode facilmente testar seu aplicativo Web localmente usando SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Adicionar um provedor OAuth 2.0

ASP.NET Web Forms oferece opções aprimoradas para autenticação e associação. Esses aprimoramentos incluem OAuth. OAuth é um protocolo aberto que permite que a autorização segura em um método simples e padrão de aplicativos de web, móveis e desktop. O modelo de Web Forms do ASP.NET usa OAuth para expor o Facebook, Twitter, Google e Microsoft como provedores de autenticação. Embora este tutorial usa apenas o Google como o provedor de autenticação, você pode modificar facilmente o código para usar qualquer um dos provedores. As etapas para implementar outros provedores são muito semelhantes às etapas que você verá neste tutorial.

Além de autenticação, o tutorial também usam funções para implementar a autorização. Somente os usuários que você adicionar ao `canEdit` função poderão alterar dados (criar, editar ou excluir contatos).

> [!NOTE] 
> 
> Aplicativos do Windows Live só aceitam uma URL ao vivo para um site de trabalho, portanto você não pode usar uma URL do site local para logons de teste.


As etapas a seguir permitirá que você adicionar um provedor de autenticação do Google.

1. Abra o *aplicativo\_Start\Startup.Auth.cs* arquivo.
2. Remova os caracteres de comentário do `app.UseGoogleAuthentication()` método para que o método aparece como segue: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navegue até o [Console de desenvolvedores do Google](https://console.developers.google.com/). Você também precisará entrar com sua conta de email de desenvolvedor do Google (gmail.com). Se você não tiver uma conta do Google, selecione o **criar uma conta** link.   
   Em seguida, você verá o **Console de desenvolvedores do Google**.   
    ![Console de desenvolvedores do Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Clique o **criar projeto** botão e digite um nome de projeto e ID (você pode usar os valores padrão). Em seguida, clique no **caixa de seleção de contrato** e **criar** botão.  

    ![Google - novo projeto](checkout-and-payment-with-paypal/_static/image9.png)

   Em alguns segundos o novo projeto será criado e o navegador exibirá a página de projetos novos.
5. Na guia à esquerda, clique em **APIs &amp; auth**e, em seguida, clique em **credenciais**.
6. Clique o **criar uma nova ID de cliente** em **OAuth**.   
   O **criar ID do cliente** caixa de diálogo será exibida.   
    ![Google - criar ID do cliente](checkout-and-payment-with-paypal/_static/image10.png)
7. No **criar ID do cliente** caixa de diálogo, mantenha o padrão **aplicativo Web** para o tipo de aplicativo.
8. Definir o **origens autorizadas de JavaScript** para a URL de SSL que você usou no início deste tutorial (`https://localhost:44300/` , a menos que você criou outros projetos SSL).   
   Essa URL é a origem para o seu aplicativo. Para este exemplo, você digitará apenas a URL de teste de localhost. No entanto, você pode inserir várias URLs para o host local e de produção.
9. Definir o **URI de redirecionamento autorizado** à seguinte: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Esse valor é o URI que OAuth ASP.NET usuários para se comunicar com o servidor do google OAuth. Lembre-se a URL de SSL usado acima ( `https://localhost:44300/` , a menos que você criou outros projetos SSL).
10. Clique o **criar ID do cliente** botão.
11. No menu à esquerda do Console de desenvolvedores do Google, clique o **tela consentimento** item de menu, em seguida, defina seu nome de produto e de endereço de email. Quando tiver preenchido o formulário, clique em **salvar**.
12. Clique o **APIs** item de menu, role para baixo e clique no **off** próximo ao **API do Google +**.   
    Aceitar esta opção permite que a API do Google +.
13. Você também deve atualizar o **pt** pacote NuGet para a versão 3.0.0.   
    Do **ferramentas** menu, selecione **NuGet Package Manager** e, em seguida, selecione **gerenciar pacotes NuGet para solução**.  
    Do **gerenciar pacotes NuGet** janela, localizar e atualizar o **pt** pacote para a versão 3.0.0.
14. No Visual Studio, atualize o `UseGoogleAuthentication` método o *Startup.Auth.cs* página Copiando e colando o **ID do cliente** e **segredo do cliente** no método. O **ID do cliente** e **segredo do cliente** valores mostrados abaixo são exemplos e não funcionará. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Pressione **CTRL + F5** para compilar e executar o aplicativo. Clique o **login** link.
16. Em **utilize outro serviço para fazer logon no**, clique em **Google**.  
    ![Iniciar sessão](checkout-and-payment-with-paypal/_static/image11.png)
17. Se você precisa inserir suas credenciais, você será redirecionado para o site do google onde você irá inserir suas credenciais.  
    ![Google - entrar](checkout-and-payment-with-paypal/_static/image12.png)
18. Depois de inserir suas credenciais, você deverá conceder permissões para o aplicativo web que você acabou de criar.  
    ![Conta de serviço do projeto padrão](checkout-and-payment-with-paypal/_static/image13.png)
19. Clique em **aceitar**. Você agora será redirecionado para a **registrar** página do **WingtipToys** aplicativo onde você pode registrar sua conta do Google.  
    ![Registrar com a conta do Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Você tem a opção de alterar o nome do registro de email local usado para a conta do Gmail, mas você geralmente deseja manter o alias de email padrão (ou seja, aquele usado para autenticação). Clique em **login** como mostrado acima.

### <a name="modifying-login-functionality"></a>Modificar a funcionalidade de logon

Como mencionado anteriormente neste tutorial série, muitas das funcionalidades de registro de usuário foi incluída no modelo de Web Forms do ASP.NET por padrão. Agora, você modificará o padrão *Login.aspx* e *Register* páginas para chamar o `MigrateCart` método. O `MigrateCart` método associa um usuário conectado recentemente um carrinho de compras anônimo. Associando o usuário e o carrinho de compras, o aplicativo de exemplo Wingtip Toys conseguirá manter o carrinho de compras do usuário entre visitas.

1. Em **Solution Explorer**, localize e abra a *conta* pasta.
2. Modifique a página de code-behind chamada *Login.aspx.cs* para incluir o código realçado em amarelo, para que ela aparece da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Salve o *Login.aspx.cs* arquivo.

Por enquanto, você pode ignorar o aviso de que há uma definição para o `MigrateCart` método. Você adicionará-lo um pouco mais tarde neste tutorial.

O *Login.aspx.cs* arquivo code-behind dá suporte a um método de logon. Inspecionando a página aspx, você verá esta página inclui um botão "Login" gatilhos de clique que, quando o `LogIn` manipulador no code-behind.

Quando o `Login` método o *Login.aspx.cs* é chamado, uma nova instância do carrinho de compras chamado `usersShoppingCart` é criado. A ID do carrinho de compras (uma GUID) é recuperada e definida como o `cartId` variável. Em seguida, o `MigrateCart` método é chamado, passando ambos o `cartId` e o nome do usuário conectado a este método. Quando o carrinho de compras é migrado, o GUID usado para identificar o carrinho de compras anônimo é substituído com o nome de usuário.

Além de modificar o *Login.aspx.cs* arquivo code-behind para migrar o carrinho de compras quando o usuário fizer logon, você também deve modificar o *arquivo de code-behind Register.aspx.cs* para migrar o carrinho de compras Quando o usuário cria uma nova conta e fizer logon.

1. No *conta* pasta, abra o arquivo code-behind chamado *Register.aspx.cs*.
2. Modifique o arquivo code-behind, incluindo o código em amarelo, para que ela aparece da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Salve o *Register.aspx.cs* arquivo. Uma vez, ignorar o aviso sobre o `MigrateCart` método.

Observe que o código usado no `CreateUser_Click` manipulador de eventos é muito semelhante ao código usado no `LogIn` método. Quando o usuário registra ou faz logon no site, uma chamada para o `MigrateCart` método será feito.

## <a name="migrating-the-shopping-cart"></a>Migrando o carrinho de compras

Agora que você tem o processo de logon e registro atualizado, você pode adicionar o código para migrar o carrinho compras usando o `MigrateCart` método.

1. Em **Solution Explorer**, localizar o *lógica* pasta e abra o *ShoppingCartActions.cs* arquivo de classe.
2. Adicione o código realçado em amarelo, o código existente no *ShoppingCartActions.cs* arquivo, para que o código de *ShoppingCartActions.cs* arquivo aparece da seguinte maneira:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

O `MigrateCart` método usa o cartId existente para localizar o carrinho de compras do usuário. Em seguida, o código percorre todos os itens de carrinho de compras e substitui o `CartId` propriedade (conforme especificado pelo `CartItem` esquema) com o nome de usuário que fez logon.

### <a name="updating-the-database-connection"></a>Atualizar a Conexão de banco de dados

Se você estiver seguindo este tutorial usando o **pré-compilada** Wingtip Toys exemplo de aplicativo, você deve recriar o banco de dados de associação padrão. Modificando a cadeia de caracteres de conexão padrão, o banco de dados de associação será criado na próxima vez que o aplicativo é executado.

1. Abra o *Web. config* arquivo na raiz do projeto.
2. Atualize a cadeia de caracteres de conexão padrão, de modo que ele aparece da seguinte maneira:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integração do PayPal

PayPal é uma plataforma de cobrança baseado na web que aceita pagamentos por comerciantes online. Em seguida, este tutorial explica como integrar a funcionalidade de check-out Express do PayPal no seu aplicativo. Check-out Express permite aos clientes usar PayPal para pagar para os itens que adicionou ao carrinho de compras.

### <a name="create-paylpal-test-accounts"></a>Criar contas de teste PaylPal

Para usar o PayPal no ambiente de teste, você deve criar e verificar uma conta de teste do desenvolvedor. Você usará a conta de teste do desenvolvedor para criar um comprador de conta de teste e uma conta de teste do vendedor. As credenciais de conta de teste do desenvolvedor também permitirá que o aplicativo de exemplo Wingtip Toys acessar o ambiente de teste do PayPal.

1. Em um navegador, navegue até o site de teste do desenvolvedor de PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Se você não tiver uma conta de desenvolvedor do PayPal, crie uma nova conta clicando **inscrever-se**e seguindo as etapas de inscrição. Se você tiver uma conta de desenvolvedor do PayPal, entrar clicando **logon**. Você precisará de sua conta de desenvolvedor do PayPal para testar o aplicativo de exemplo Wingtip Toys posteriormente neste tutorial.
3. Se você tem apenas se inscreveu para sua conta de desenvolvedor do PayPal, convém verificar sua conta de desenvolvedor do PayPal com PayPal. Você pode verificar sua conta, seguindo as etapas que PayPal enviada à sua conta de email. Depois de verificar sua conta de desenvolvedor do PayPal, faça logon novamente para o desenvolvedor do PayPal testando o site.
4. Depois de fazer logon site do desenvolvedor PayPal com sua conta de desenvolvedor do PayPal que é necessário criar uma conta de teste do PayPal comprador se você ainda não tiver um. Para criar uma conta de teste do comprador, no site do PayPal, clique no **aplicativos** guia e, em seguida, clique em **contas de área restrita**.   
 O **contas de teste de área restrita** página é exibida.   

    > [!NOTE] 
    > 
    > O site do desenvolvedor do PayPal já fornece uma conta de teste comercial.

    ![Check-out e pagamento com PayPal - contas de teste de área restrita](checkout-and-payment-with-paypal/_static/image15.png)
5. Na página de contas de teste seguro, clique em **criar conta**.
6. Sobre o **criar conta de teste** página Escolha um comprador de email da conta de teste e a senha de sua escolha.   

    > [!NOTE] 
    > 
    > Você precisará de endereços de email do comprador e a senha para testar o aplicativo de exemplo Wingtip Toys no final deste tutorial.

    ![Check-out e pagamento com PayPal - contas de teste de área restrita](checkout-and-payment-with-paypal/_static/image16.png)
7. Crie a conta de teste do comprador clicando o **criar conta** botão.  
 O **contas de teste de área restrita** página é exibida. 

    ![Check-out e pagamento com PayPal - PaylPal contas](checkout-and-payment-with-paypal/_static/image17.png)
8. Sobre o **contas de teste de área restrita** , clique no **facilitador** contas de email.  
    **Perfil** e **notificação** opções são exibidas.
9. Selecione o **perfil** opção e, em seguida, clique em **credenciais API** para exibir suas credenciais de API para a conta de teste comercial.
10. Copia as credenciais de API de teste para o bloco de notas.

Será necessário suas credenciais de API de teste clássico exibidas (nome de usuário, senha e assinatura) para fazer chamadas de API do aplicativo de exemplo Wingtip Toys do PayPal no ambiente de teste. Adicione as credenciais na próxima etapa.

### <a name="add-paypal-class-and-api-credentials"></a>Adicionar classe PayPal e credenciais de API

Coloque a maior parte do código PayPal em uma única classe. Essa classe contém os métodos usados para se comunicar com o PayPal. Além disso, você adicionará suas credenciais do PayPal para essa classe.

1. No aplicativo de amostra Wingtip Toys dentro do Visual Studio, clique com botão direito do **lógica** pasta e, em seguida, selecione **adicionar**  - &gt; **Novo Item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Em **Visual C#** do **instalado** painel à esquerda, selecione **código**.
3. No painel central, selecione **classe**. Nomeie essa nova classe **PayPalFunctions.cs**.
4. Clique em **Adicionar**.  
   O novo arquivo de classe é exibido no editor.
5. Substitua o código padrão pelo seguinte código:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Adicione as credenciais de API do fornecedor (nome de usuário, senha e assinatura) exibido anteriormente neste tutorial, para que você possa fazer chamadas de função para o ambiente de teste do PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> Este aplicativo de exemplo você está simplesmente adicionando as credenciais para um arquivo c# (cs). No entanto, em uma solução implementada, considere criptografar suas credenciais em um arquivo de configuração.


A classe NVPAPICaller contém a maioria da funcionalidade PayPal. O código na classe fornece os métodos necessários para fazer um teste de compra do ambiente de teste do PayPal. As três funções PayPal a seguir são usadas para fazer compras:

- `SetExpressCheckout` Função
- `GetExpressCheckoutDetails` Função
- `DoExpressCheckoutPayment` Função

O `ShortcutExpressCheckout` método coleta os detalhes de informações e o produto de compra do teste do carrinho de compras e chama o `SetExpressCheckout` função PayPal. O `GetCheckoutDetails` método confirma os detalhes da aquisição e chama o `GetExpressCheckoutDetails` PayPal função antes de fazer a compra de teste. O `DoCheckoutPayment` método concluir a compra de teste do ambiente de teste, chamando o `DoExpressCheckoutPayment` função PayPal. O código restante dá suporte a métodos do PayPal e o processo, como cadeias de caracteres de codificação, decodificação de cadeias de caracteres, matrizes de processamento e determinar as credenciais.

> [!NOTE] 
> 
> PayPal permite incluir detalhes da aquisição opcional com base em [especificação da API do PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Estendendo o código no aplicativo de amostra Wingtip Toys, você pode incluir detalhes de localização, as descrições de produto, imposto, um número de serviço do cliente, bem como muitos outros campos opcionais.


Observe que as URLs de retorno e de cancelamento que são especificadas no **ShortcutExpressCheckout** método usa um número de porta.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Quando o Visual Web Developer executa um projeto web usando SSL, geralmente a porta 44300 é usada para o servidor web. Como mostrado acima, o número da porta é 44300. Quando você executa o aplicativo, você pode ver um número de porta diferente. Suas necessidades de número de porta a ser configurado corretamente no código para que você possa bem-sucedida execução o aplicativo de exemplo Wingtip Toys no final deste tutorial. A próxima seção deste tutorial explica como recuperar o número de porta do host local e atualizar a classe do PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Atualizar o número da porta do LocalHost na classe PayPal

O aplicativo de exemplo Wingtip Toys adquire produtos navegando até o site de teste do PayPal e retornar à sua instância local do aplicativo de exemplo Wingtip Toys. Para ter PayPal retornar para a URL correta, você precisa especificar o número da porta da execução localmente exemplo de aplicativo no código do PayPal mencionado acima.

1. Clique no nome do projeto (**WingtipToys**) em **Solution Explorer** e selecione **propriedades**.
2. Na coluna esquerda, selecione o **Web** guia.
3. Recuperar o número da porta de **Url do projeto** caixa.
4. Se necessário, atualize o `returnURL` e `cancelURL` na classe PayPal (`NVPAPICaller`) no *PayPalFunctions.cs* arquivo para usar o número da porta do seu aplicativo web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Agora, o código que você adicionou corresponderá a porta esperada para seu aplicativo da Web local. PayPal poderá retornar para o URL correto no computador local.

### <a name="add-the-paypal-checkout-button"></a>Adicionar o botão de check-out do PayPal

Agora que as funções básicas do PayPal foram adicionadas ao aplicativo de exemplo, você pode começar a adicionar a marcação e o código necessário para chamar essas funções. Primeiro, você deve adicionar o botão de check-out que o usuário verá na página do carrinho de compras.

1. Abra o *ShoppingCart.aspx* arquivo.
2. Role até o final do arquivo e localize o `<!--Checkout Placeholder -->` comentário.
3. Substitua o comentário com um `ImageButton` controlar de forma que a marca de backup é substituída da seguinte maneira:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. No *ShoppingCart.aspx.cs* arquivo, após o `UpdateBtn_Click` manipulador de eventos no final do arquivo, adicione o `CheckOutBtn_Click` manipulador de eventos:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Também no *ShoppingCart.aspx.cs* de arquivo, adicione uma referência para o `CheckoutBtn`, de modo que o novo botão de imagem é referenciado como se segue:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Salve as alterações em ambos os *ShoppingCart.aspx* arquivo e o *ShoppingCart.aspx.cs* arquivo.
7. No menu, selecione **depurar**-&gt;**criar WingtipToys**.  
   O projeto será reconstruído com recém-adicionado **ImageButton** controle.

### <a name="send-purchase-details-to-paypal"></a>Enviar detalhes de compra para PayPal

Quando o usuário clica o **check-out** botão na página do carrinho de compras (*ShoppingCart.aspx*), ele começará o processo de compra. O código a seguir chama a primeira função PayPal necessária comprar produtos.

1. Do *check-out* pasta, abra o arquivo code-behind chamado *CheckoutStart.aspx.cs*.   
   Certifique-se de abrir o arquivo code-behind.
2. Substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Quando o usuário do aplicativo clica o **check-out** botão a página de carrinho de compras, o navegador navegará para a *CheckoutStart.aspx* página. Quando o *CheckoutStart.aspx* página for carregada, o `ShortcutExpressCheckout` método é chamado. Neste ponto, o usuário é transferido para o site de teste do PayPal. No site do PayPal, o usuário insere suas credenciais do PayPal, examina os detalhes de compra, aceitar o contrato do PayPal e retorna para o aplicativo de exemplo Wingtip Toys onde o `ShortcutExpressCheckout` método é concluído. Quando o `ShortcutExpressCheckout` método estiver concluído, ele redirecionará o usuário para o *CheckoutReview.aspx* página especificada no `ShortcutExpressCheckout` método. Isso permite que o usuário revisar os detalhes do pedido de dentro do aplicativo de exemplo Wingtip Toys.

### <a name="review-order-details"></a>Examine os detalhes de ordem

Após o retorno do PayPal, o *CheckoutReview.aspx* página do aplicativo de exemplo Wingtip Toys exibe os detalhes do pedido. Esta página permite que o usuário revisar os detalhes do pedido antes de adquirir os produtos. O *CheckoutReview.aspx* página deve ser criada da seguinte maneira:

1. No *check-out* pasta, abra a página chamada *CheckoutReview.aspx*.
2. Substitua a marcação existente com o seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Abra a página de code-behind chamada *CheckoutReview.aspx.cs* e substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

O **DetailsView** controle é usado para exibir os detalhes do pedido que tiverem sido retornados do PayPal. Além disso, o código acima salva os detalhes do pedido para o banco de dados do Wingtip Toys como um `OrderDetail` objeto. Quando o usuário clica no **pedido completo** botão, eles são redirecionados para o *CheckoutComplete.aspx* página.

> [!NOTE] 
> 
> **Dica**
> 
> Na marcação do *CheckoutReview.aspx* página, observe que o `<ItemStyle>` marca é usada para alterar o estilo dos itens dentro de **DetailsView** controle na parte inferior da página. Exibindo a página em **modo Design** (selecionando **Design** no canto inferior esquerdo do Visual Studio), selecionando o **DetailsView** controle e selecionando o  **Marca inteligente** (o ícone de seta na parte superior direita do controle), você poderá ver o **tarefas DetailsView**.
> 
> ![Check-out e pagamento com PayPal - editar campos](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Selecionando **editar campos**, o **campos** caixa de diálogo será exibida. Na caixa de diálogo você pode controlar facilmente propriedades visuais, como **ItemStyle**, do **DetailsView** controle.
> 
> ![Check-out e pagamento com PayPal - caixa de diálogo de campos](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Concluir a compra

*CheckoutComplete.aspx* página faz a compra do PayPal. Conforme mencionado acima, o usuário deve clicar no **pedido completo** botão antes do aplicativo navegará para a *CheckoutComplete.aspx* página.

1. No *check-out* pasta, abra a página chamada *CheckoutComplete.aspx*.
2. Substitua a marcação existente com o seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Abra a página de code-behind chamada *CheckoutComplete.aspx.cs* e substitua o código existente pelo seguinte:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Quando o *CheckoutComplete.aspx* página for carregada, o `DoCheckoutPayment` método é chamado. Como mencionado anteriormente, o `DoCheckoutPayment` método concluir a compra do ambiente de teste do PayPal. Quando o PayPal concluir a compra do pedido, o *CheckoutComplete.aspx* página exibe uma transação de pagamento `ID` para o comprador.

### <a name="handle-cancel-purchase"></a>Lidar com cancelamento de compra

Se o usuário decidir cancelar a compra, eles serão direcionados para o *CheckoutCancel.aspx* página em que eles verão sua ordem foi cancelada.

1. Abra a página chamada *CheckoutCancel.aspx* no *check-out* pasta.
2. Substitua a marcação existente com o seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Tratar erros de compra

Erros durante o processo de compra serão tratados pelo *CheckoutError.aspx* página. O code-behind do *CheckoutStart.aspx* página, o *CheckoutReview.aspx* página e o *CheckoutComplete.aspx* cada página será redirecionada para o  *CheckoutError.aspx* página se ocorrer um erro.

1. Abra a página chamada *CheckoutError.aspx* no *check-out* pasta.
2. Substitua a marcação existente com o seguinte:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

O *CheckoutError.aspx* página é exibida com os detalhes do erro quando ocorre um erro durante o processo de check-out.

## <a name="running-the-application"></a>Executando o aplicativo

Execute o aplicativo para ver como a adquirir produtos. Observe que você estará sendo executada do PayPal ambiente de teste. Sem dinheiro real que está sendo trocado.

1. Verifique se todos os arquivos são salvos no Visual Studio.
2. Abra um navegador da Web e navegue até [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Faça logon com sua conta de desenvolvedor do PayPal que você criou anteriormente neste tutorial.  
   Para a área restrita do desenvolvedor do PayPal, você precisa entrar no [ https://developer.paypal.com ](https://developer.paypal.com/) para testar o check-out express. Isso se aplica apenas à proteção do PayPal teste, não para o ambiente em tempo real do PayPal.
4. No Visual Studio, pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
   Depois de recria o banco de dados, o navegador será aberto e mostre o *Default.aspx* página.
5. Adicionar três produtos diferentes ao carrinho de compras selecionando a categoria de produto, como "Carro" e, em seguida, clicando em **adicionar ao carrinho** ao lado de cada produto.  
   O carrinho de compras exibirá o produto que você selecionou.
6. Clique o **PayPal** botão para check-out. 

    ![Check-out e pagamento com PayPal - carrinho](checkout-and-payment-with-paypal/_static/image20.png)

   Fazendo check-out exigirá que você tenha uma conta de usuário para o aplicativo de exemplo Wingtip Toys.
7. Clique o **Google** link à direita da página para fazer logon com uma conta de email gmail.com existente.  
   Se você não tiver uma conta de gmail.com, você pode criar um para testes no [www.gmail.com](https://www.gmail.com/). Você também pode usar uma conta local padrão, clicando em "Registrar". 

    ![Check-out e pagamento com PayPal - login](checkout-and-payment-with-paypal/_static/image21.png)
8. Entrar com sua conta do gmail e senha. 

    ![Check-out e pagamento com PayPal - Gmail entrar](checkout-and-payment-with-paypal/_static/image22.png)
9. Clique o **login** botão para registrar sua conta do gmail com seu nome de usuário do aplicativo de exemplo Wingtip Toys. 

    ![Check-out e pagamento com PayPal - registro de conta](checkout-and-payment-with-paypal/_static/image23.png)
10. No site de teste do PayPal, adicione seu **comprador** email endereço e a senha que você criou anteriormente neste tutorial, clique no **logon** botão. 

    ![Check-out e pagamento com PayPal - PayPal entrar](checkout-and-payment-with-paypal/_static/image24.png)
11. Concordar com a política do PayPal e clique no **aceitar e continuar** botão.  
    Observe que essa página só é exibida na primeira vez que você usar essa conta PayPal. Novamente, observe que esta é uma conta de teste, sem dinheiro real é trocado. 

    ![Check-out e pagamento com PayPal - PayPal política](checkout-and-payment-with-paypal/_static/image25.png)
12. Examine as informações de pedido do PayPal teste de página de revisão do ambiente e clique em **continuar**. 

    ![Check-out e pagamento com PayPal - informações de revisão](checkout-and-payment-with-paypal/_static/image26.png)
13. Sobre o *CheckoutReview.aspx* página, verifique se o valor do pedido e exibir o endereço de envio gerado. Em seguida, clique no **pedido completo** botão. 

    ![Check-out e pagamento com PayPal - revisão de ordem](checkout-and-payment-with-paypal/_static/image27.png)
14. O **CheckoutComplete.aspx** página é exibida com uma ID de transação de pagamento. 

    ![Pagamento com PayPal - completa de check-out e check-out](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Revisando o banco de dados

Ao analisar os dados atualizados no banco de dados de aplicativo de exemplo Wingtip Toys depois de executar o aplicativo, você pode ver que o aplicativo registrado com êxito a compra de produtos.

Você pode inspecionar os dados contidos no *Wingtiptoys.mdf* arquivo de banco de dados usando o **Pesquisador de objetos de banco de dados** janela (**Server Explorer** janela no Visual Studio) como você fez anteriormente na série de tutoriais.

1. Se ainda estiver aberto, feche a janela do navegador.
2. No Visual Studio, selecione o **Mostrar todos os arquivos** no canto superior do **Solution Explorer** para permitir que você expanda os **aplicativo\_dados** pasta.
3. Expanda o **aplicativo\_dados** pasta.  
 Talvez seja necessário selecionar o **Mostrar todos os arquivos** ícone da pasta.
4. Clique com botão direito do *Wingtiptoys.mdf* arquivo de banco de dados e selecione **abrir**.  
    **Gerenciador de servidores** é exibido.
5. Expanda o **tabelas** pasta.
6. Clique com botão direito do **pedidos**de tabela e selecione **Mostrar dados da tabela**.  
 O **pedidos** tabela é exibida.
7. Examine o **PaymentTransactionID** coluna para confirmar transações com êxito. 

    ![Check-out e pagamento com PayPal - banco de dados de análise](checkout-and-payment-with-paypal/_static/image29.png)
8. Fechar o **pedidos** janela da tabela.
9. No Gerenciador de servidores, clique com botão direito do **OrderDetails** de tabela e selecione **Mostrar dados da tabela**.
10. Examine o `OrderId` e `Username` valores no **OrderDetails** tabela. Observe que esses valores correspondem a `OrderId` e `Username` valores incluídos no **pedidos** tabela.
11. Fechar o **OrderDetails** janela da tabela.
12. Clique no arquivo de banco de dados Wingtip Toys (*Wingtiptoys.mdf*) e selecione **fechar Conexão**.
13. Se você não vir o **Solution Explorer** janela, clique em **Solution Explorer** na parte inferior da **Server Explorer** janela para mostrar o **Gerenciador de soluções**  novamente.

## <a name="summary"></a>Resumo

Neste tutorial, você adicionou ordem e esquemas de detalhes do pedido para acompanhar a compra de produtos. Você também PayPal funcionalidade integrada para o aplicativo de exemplo Wingtip Toys.

## <a name="additional-resources"></a>Recursos adicionais

[Visão geral sobre a configuração do ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Implantar um aplicativo de formulários da Web do ASP.NET seguro com associação, OAuth e o banco de dados SQL para serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versão de avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este tutorial contém código de exemplo. Esse código de exemplo é fornecido "como está" sem garantias de qualquer tipo. Da mesma forma, a Microsoft não garante a precisão, a integridade ou a qualidade do código de exemplo. Você concorda em usar o código de exemplo por seu próprio risco. Sob nenhuma circunstância Microsoft será responsável de qualquer forma por um código de exemplo, o conteúdo, incluindo, mas não se limitando a, quaisquer erros ou omissões em qualquer código de exemplo, conteúdo, ou qualquer perda ou dano de qualquer natureza resultante do uso de qualquer código de exemplo. Você será notificado por meio deste e concorda em indenizar, salvar e mantenha livre de perda de todo, declarações de perda, lesões ou danos de qualquer tipo incluindo, sem limitação, aqueles occasioned por ou decorrentes do material lançar, a Microsoft transmitir, usar ou contar com incluindo, mas não limitado a, as exibições expressadas neste documento.

> [!div class="step-by-step"]
> [Anterior](shopping-cart.md)
> [Próximo](membership-and-administration.md)
