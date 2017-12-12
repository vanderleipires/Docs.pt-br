---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: "Associação e administração | Microsoft Docs"
author: Erikre
description: "Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para nós..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: a10dbfe1ca49baee1604aac8dd9a1f93ccfcb7f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="membership-and-administration"></a>Associação e administração
====================
Por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com o código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Este tutorial mostra como atualizar o aplicativo de exemplo Wingtip Toys para adicionar uma função personalizada e usar a identidade do ASP.NET. Ele também mostra como implementar uma página de administração do qual o usuário com uma função personalizada pode adicionar e remover produtos no site.

[Identidade do ASP.NET](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) é o sistema de associação usado para criar o aplicativo web do ASP.NET e está disponível no ASP.NET 4.5. Identidade do ASP.NET é usada no modelo de projeto do Visual Studio 2013 Web Forms, bem como modelos para [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), e [aplicativo de página únicadoASP.NET](../../../../single-page-application/index.md). Também, especificamente, você pode instalar o sistema de identidade do ASP.NET usando o NuGet quando você começa com um aplicativo Web vazio. No entanto, nesta série tutorial você usar o **Web Forms**projecttemplate, que inclui o sistema de identidade do ASP.NET. Identidade do ASP.NET facilita a integrar dados de perfil de usuário específico com dados de aplicativo. Além disso, a identidade do ASP.NET permite que você escolha o modelo de persistência para os perfis de usuário em seu aplicativo. Você pode armazenar os dados em um banco de dados do SQL Server ou outro repositório de dados, incluindo *NoSQL* repositórios de dados, como tabelas de armazenamento do Windows Azure.

Este tutorial se baseia no tutorial anterior intitulado "Check-out e pagamento com PayPal" na série tutorial Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como usar código para adicionar uma função personalizada e um usuário para o aplicativo.
- Como restringir o acesso à pasta de administração e página.
- Como fornecer uma navegação para o usuário que pertence à função personalizada.
- Como usar a associação de modelo para preencher um [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) controle com categorias de produtos.
- Como carregar um arquivo para o aplicativo web usando o [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) controle.
- Como usar os controles de validação para implementar a validação de entrada.
- Como adicionar e remover produtos do aplicativo.

## <a name="these-features-are-included-in-the-tutorial"></a>Esses recursos estão incluídos no tutorial:

- ASP.NET Identity
- Configuração e autorização
- Associação de modelo
- Validação discreta

ASP.NET Web Forms fornece recursos de associação. Usando o modelo padrão, você tem a funcionalidade de associação interna que você pode usar imediatamente quando o aplicativo é executado. Este tutorial mostra como usar a identidade do ASP.NET para adicionar uma função personalizada e atribuir um usuário a essa função. Você aprenderá a restringir o acesso à pasta de administração. Você adicionará uma página para a pasta de administração que permite que um usuário com uma função personalizada para adicionar e remover produtos e visualizar um produto depois que ele foi adicionado.

## <a name="adding-a-custom-role"></a>Adicionando uma função personalizada

Usando a identidade do ASP.NET, você pode adicionar uma função personalizada e atribuir um usuário a essa função usando o código.

1. Em **Solution Explorer**, com o botão direito no *lógica* pasta e criar uma nova classe.
2. Nomeie a nova classe *RoleActions.cs*.
3. Modifique o código para que ele aparece da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. Em **Solution Explorer**, abra o *Global.asax.cs* arquivo.
5. Modificar o *Global.asax.cs* arquivo adicionando o código realçado em amarelo para que ela aparece da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Observe que `AddUserAndRole` está sublinhado em vermelho. Clique duas vezes o código AddUserAndRole.  
 A letra "A" no início do método realçado será sublinhada.
7. Passe o mouse sobre a letra "A" e clique em interface do usuário que permite que você gere um stub de método para o `AddUserAndRole` método. 

    ![Associação e Advministration - Gerar Stub do método](membership-and-administration/_static/image1.png)
8. Clique na opção de chamada:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Abra o *RoleActions.cs* arquivo o *lógica* pasta.  
 O `AddUserAndRole` método foi adicionado ao arquivo de classe.
10. Modificar o *RoleActions.cs* arquivo removendo o `NotImplementedeException` e adicionando o código realçado em amarelo, para que ela aparece da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

O código acima primeiro estabelece um contexto de banco de dados para o banco de dados de associação. O banco de dados de associação também é armazenado como um *. mdf* arquivo o *aplicativo\_dados* pasta. Você poderá exibir esse banco de dados quando o primeiro usuário tiver entrado para esse aplicativo web. 

> [!NOTE] 
> 
> Se você deseja armazenar os dados de associação junto com os dados de produto, você pode considerar usando a mesma **DbContext** que é usada para armazenar os dados de produto no código acima.


 O *interno* palavra-chave é um modificador de acesso para membros de tipo (como métodos ou propriedades) e tipos (como classes). Tipos internos ou os membros são acessíveis apenas dentro de arquivos contidos no mesmo assembly *(. dll* arquivo). Quando você cria seu aplicativo, um arquivo de assembly *(. dll*) é criado que contém o código que é executado quando você executar o aplicativo. 

Um `RoleStore` object, que fornece gerenciamento de função, é criado com base no contexto do banco de dados.

> [!NOTE] 
> 
> Observe que, quando o `RoleStore` objeto é criado ele usa um genérico `IdentityRole` tipo. Isso significa que o `RoleStore` só pode conter `IdentityRole` objetos. Também usando genéricos, recursos na memória são tratados melhor.


Em seguida, o `RoleManager` de objeto, é criado com base no `RoleStore` objeto que você acabou de criar. o `RoleManager` objeto expõe relacionada à função API que pode ser usado para salvar automaticamente as alterações para o `RoleStore`. O `RoleManager` só pode conter `IdentityRole` objetos porque o código usa o `<IdentityRole>` tipo genérico.

Chamar o `RoleExists` método para determinar se a função "canEdit" está presente no banco de dados de associação. Se não estiver, você pode criar a função.

Criando o `UserManager` objeto parece ser mais complicado do que o `RoleManager` controlar, porém é praticamente o mesmo. Apenas codificada em uma linha em vez de várias. Aqui, o parâmetro que você está passando é criar uma instância como um novo objeto contido entre parênteses.

Em seguida crie o usuário "canEditUser" Criando um novo `ApplicationUser` objeto. Em seguida, se o usuário for criado com êxito, adicione o usuário para a nova função.

> [!NOTE] 
> 
> O tratamento de erro será atualizado durante o tutorial "Tratamento de erros do ASP.NET" Esta série de tutoriais.


Na próxima vez que o aplicativo for iniciado, o usuário chamado "canEditUser" será adicionado como função chamada "canEdit" do aplicativo. Posteriormente neste tutorial, você irá fazer logon como o usuário "canEditUser" para exibir os recursos adicionais que você vai adicionados durante este tutorial. Para obter detalhes de API sobre a identidade do ASP.NET, consulte o [ASPNET Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Para obter detalhes adicionais sobre como inicializar o sistema de identidade do ASP.NET, consulte o [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Restringindo o acesso para a página de administração

O aplicativo de exemplo Wingtip Toys permite que usuários anônimos e os usuários autorizados a exibir e comprar produtos. No entanto, o usuário conectado que tenha a função personalizada "canEdit" pode acessar uma página restrita para adicionar e remover produtos.

#### <a name="add-an-administration-folder-and-page"></a>Adicionar uma pasta de administração e a página

Em seguida, você criará uma pasta chamada *Admin* aplicativo de exemplo para o usuário "canEditUser" que pertencem à função personalizada da Wingtip Toys.

1. Clique no nome do projeto (**Wingtip Toys**) em **Solution Explorer** e selecione **adicionar**  - &gt; **nova pasta**.
2. Nomeie a nova pasta *Admin*.
3. Clique com botão direito do *Admin* pasta e, em seguida, selecione **adicionar**  - &gt; **Novo Item**.   
 A caixa de diálogo **Adicionar Novo Item** é exibida.
4. Selecione o **Visual C#** - &gt; **Web** grupo de modelos, à esquerda. Na lista intermediária, selecione **Web Form com página mestra**, nomeie-o *AdminPage.aspx***,** e, em seguida, selecione **adicionar**.
5. Selecione o *Site.Master* de arquivos como a página mestra e, em seguida, escolha **Okey**.

#### <a name="add-a-webconfig-file"></a>Adicionar um arquivo Web. config

Adicionando um *Web. config* o arquivo para o *Admin* pasta, você pode restringir o acesso à página contida na pasta.

1. Clique com botão direito do *Admin* pasta e selecione **adicionar**  - &gt; **Novo Item**.  
 A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Na lista de modelos de web do Visual c#, selecione **arquivo de configuração Web**na lista intermediária, aceite o nome padrão do *Web. config***,** e, em seguida, selecione **Adicionar**.
3. Substitua o conteúdo existente do XML de *Web. config* arquivo com o seguinte:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Salve o *Web. config* arquivo. O *Web. config* arquivo Especifica o único usuário que pertencem à função de "canEdit" do aplicativo pode acessar a página contida no *Admin* pasta.

### <a name="including-custom-role-navigation"></a>Incluindo navegação de função personalizada

Para habilitar o usuário da função personalizada "canEdit" navegar até a seção Administração do aplicativo, você deve adicionar um link para o *Site.Master* página. Somente os usuários que pertencem à função de "canEdit" será capazes de ver o **Admin** vincular e acessar a seção de administração.

1. No Solution Explorer, localize e abra a *Site.Master* página.
2. Para criar um link para o usuário da função de "canEdit", adicione a marcação realçada em amarelo para a seguinte lista não ordenada `<ul>` elemento para que a lista é exibida como segue:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Abra o *Site.Master.cs* arquivo. Verifique o **Admin** link visível apenas para o usuário "canEditUser" Adicionando o código realçado em amarelo para o `Page_Load` manipulador. O `Page_Load` manipulador será exibida da seguinte maneira:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Quando a página for carregada, o código verifica se o usuário conectado tem a função de "canEdit". Se o usuário pertence à função de "canEdit", o elemento de intervalo que contém o link para o *AdminPage.aspx* página (e, consequentemente, o link dentro da extensão) ficam visíveis.

### <a name="enabling-product-administration"></a>Habilitar a administração de produto

Até agora, você criou a função "canEdit" e adicionou um usuário "canEditUser", uma pasta de administração e uma página de administração. Você definiu direitos de acesso para a pasta de administração e a página e adicionou um link de navegação para o usuário da função de "canEdit" para o aplicativo. Em seguida, você adicionará a marcação para o *AdminPage.aspx* página e de código para o *AdminPage.aspx.cs* arquivo code-behind que permitirá que o usuário com a função "canEdit" adicionar e remover produtos.

1. Em **Solution Explorer**, abra o *AdminPage.aspx* arquivo o *Admin* pasta.
2. Substitua a marcação existente com o seguinte:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Em seguida, abra o *AdminPage.aspx.cs* arquivo code-behind clicando com o *AdminPage.aspx* e clicando em **Exibir código**.
4. Substitua o código existente no *AdminPage.aspx.cs* arquivo code-behind com o código a seguir:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

No código que você inseriu para o *AdminPage.aspx.cs* arquivo code-behind, uma classe chamada `AddProducts` faz o trabalho real de adição de produtos no banco de dados. Essa classe ainda não existe, portanto, você vai criar agora.

1. Em **Solution Explorer**, com o botão direito do *lógica* pasta e, em seguida, selecione **adicionar**  - &gt; **Novo Item**.   
 A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual C#**  - &gt; **código** grupo de modelos, à esquerda. Em seguida, selecione **classe**do meio lista e nomeie-o *AddProducts.cs*.   
 O novo arquivo de classe é exibido.
3. Substitua o código existente pelo seguinte:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

O *AdminPage.aspx* página permite que o usuário que pertencem à função de "canEdit" adicionar e remover produtos. Quando um novo produto é adicionado, os detalhes sobre o produto são validados e, em seguida, são inseridos no banco de dados. O novo produto está imediatamente disponível para todos os usuários do aplicativo web.

#### <a name="unobtrusive-validation"></a>Validação discreta

Os detalhes do produto que fornece o usuário sobre o *AdminPage.aspx* página são validados usando controles de validação (`RequiredFieldValidator` e `RegularExpressionValidator`). Esses controles automaticamente usam validação discreta. Validação discreta permite que os controles de validação usar JavaScript para a lógica de validação do lado do cliente, o que significa que a página não requer uma viagem para o servidor a ser validado. Por padrão, a validação discreta está incluída no *Web. config* arquivo baseia-se a seguinte configuração:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Expressões regulares

O preço do produto sobre o *AdminPage.aspx* página é validada usando um **RegularExpressionValidator** controle. Esse controle valida se o valor do controle associado de entrada (caixa de texto "AddProductPrice") corresponde ao padrão especificado pela expressão regular. Uma expressão regular é uma notação de correspondência de padrões que permite que você localize rapidamente e correspondência de padrões de caractere específico. O **RegularExpressionValidator** controle inclui uma propriedade chamada `ValidationExpression` que contém a expressão regular usada para validar a entrada de preço, conforme mostrado abaixo:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Controle de carregamento de arquivos

Além dos controles de entrada e a validação, você adicionou o **FileUpload** o controle para o *AdminPage.aspx* página. Esse controle fornece a capacidade de carregar arquivos. Nesse caso, você está permitindo somente arquivos de imagem a ser carregada. No arquivo code-behind (*AdminPage.aspx.cs*), quando o `AddProductButton` é clicado, o código verifica o `HasFile` propriedade o **FileUpload** controle. Se o controle tiver um arquivo e se é permitido o tipo de arquivo (com base na extensão de arquivo), a imagem será salvo o *imagens* pasta e o *imagens/miniaturas* pasta do aplicativo.

#### <a name="model-binding"></a>Associação de modelo

Anteriormente na série tutorial associação de modelo é usado para popular um **ListView** controle, uma **FormsView** controle, uma **GridView** controle e um  **DetailView** controle. Neste tutorial, você usa associação de modelo para popular um **DropDownList** controle com uma lista de categorias de produto.

A marcação que você adicionou para a *AdminPage.aspx* arquivo contém um **DropDownList** controle chamado `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Você usa associação de modelo para preencher essa **DropDownList** definindo o `ItemType` atributo e o `SelectMethod` atributo. O `ItemType` atributo especifica que você use o `WingtipToys.Models.Category` digite ao popular o controle. Você definiu esse tipo no início desta série tutorial criando o `Category` classe (mostrada abaixo). O `Category` classe está no *modelos* pasta dentro de *Category.cs* arquivo.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

O `SelectMethod` atributo o **DropDownList** controle especifica que você usa o `GetCategories` método (mostrado abaixo) que é incluído no arquivo code-behind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Esse método Especifica que um `IQueryable` interface é usada para avaliar uma consulta em relação a um `Category` tipo. O valor retornado é usado para popular o **DropDownList** na marcação da página (*AdminPage.aspx*).

O texto exibido para cada item na lista é especificado definindo a `DataTextField` atributo. O `DataTextField` atributo usa o `CategoryName` do `Category` classe (mostrado acima) para exibir cada categoria no **DropDownList** controle. O valor real que é passado quando um item é selecionado no **DropDownList** controle se baseia o `DataValueField` atributo. O `DataValueField` atributo é definido como o `CategoryID` como definir no `Category` classe (mostrado acima).

### <a name="how-the-application-will-work"></a>Como o aplicativo funcionará

Quando o usuário que pertencem à função de "canEdit" navega para a página pela primeira vez, o `DropDownAddCategory` **DropDownList** controle é populado conforme descrito acima. O `DropDownRemoveProduct` **DropDownList** controle também é preenchido com produtos usando a mesma abordagem. O usuário que pertencem à função de "canEdit" seleciona o tipo de categoria e adiciona os detalhes do produto (**nome**, **descrição**, **preço**, e **doarquivodeimagem**). Quando o usuário que pertencem à função de "canEdit" clica o **adicionar produto** botão, o `AddProductButton_Click` manipulador de eventos é disparado. O `AddProductButton_Click` localizado do manipulador de eventos no arquivo code-behind (*AdminPage.aspx.cs*) verifica o arquivo de imagem para certificar-se de que ele corresponda aos tipos de arquivo permitido *(. gif*, *. PNG*, *. JPEG*, ou *. jpg*). Em seguida, o arquivo de imagem é salvo em uma pasta do aplicativo de exemplo Wingtip Toys. Em seguida, o novo produto é adicionado ao banco de dados. Para realizar a adição de um novo produto, uma nova instância do `AddProducts` classe é criada e nomeada produtos. O `AddProducts` classe tem um método chamado `AddProduct`, e o objeto de produtos chama esse método para adicionar os produtos no banco de dados.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Se o código adiciona com êxito o novo produto no banco de dados, a página é recarregada com o valor de cadeia de caracteres de consulta `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Quando a página é recarregado, a cadeia de caracteres de consulta está incluída na URL. Recarregando a página, o usuário que pertencem à função de "canEdit" pode ver imediatamente as atualizações no **DropDownList** controles de *AdminPage.aspx* página. Além disso, incluindo a cadeia de caracteres de consulta com a URL, a página pode exibir uma mensagem de êxito para o usuário que pertencem à função de "canEdit".

Quando o *AdminPage.aspx* página recargas, o `Page_Load` é chamado de evento.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

O `Page_Load` manipulador de eventos verifica o valor de cadeia de caracteres de consulta e determina se deve mostrar uma mensagem de êxito.

## <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver como você pode adicionar, exclusão e atualização de itens no carrinho de compras. O total de carrinho de compras refletirá o custo total de todos os itens no carrinho de compras.

1. No Solution Explorer, pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
 O navegador é aberto e mostra o *Default.aspx* página.
2. Clique o **login** link na parte superior da página. 

    ![Associação e administração - Log no Link](membership-and-administration/_static/image2.png)

 O *Login.aspx* página é exibida.
3. Use o seguinte nome de usuário e senha:  
 Nome de usuário:canEditUser@wingtiptoys.com  
 Senha: Pa$ $word1 

    ![Associação e administração - página de logon](membership-and-administration/_static/image3.png)
4. Clique o **login** botão na parte inferior da página.
5. Na parte superior da página seguinte, selecione o **Admin** link para navegar até o *AdminPage.aspx* página. 

    ![Associação e administração - Link Admin](membership-and-administration/_static/image4.png)
6. Para testar a validação de entrada, clique o **adicionar produto** botão sem adicionar os detalhes do produto. 

    ![Associação e administração - página de administrador](membership-and-administration/_static/image5.png)

 Observe que as mensagens de campo obrigatório são exibidas.
7. Adicione os detalhes de um novo produto e, em seguida, clique no **adicionar produto** botão. 

    ![Associação e administração - adicionar produto](membership-and-administration/_static/image6.png)
8. Selecione **produtos** no menu de navegação superior para exibir o novo produto que você adicionou. 

    ![Associação e administração - Mostrar novo produto](membership-and-administration/_static/image7.png)
9. Clique o **Admin** link para retornar à página de administração.
10. No **Remover produto** seção da página, selecione o novo produto que você adicionou o **DropDownListBox**.
11. Clique o **Remover produto** botão para remover o novo produto do aplicativo. 

    ![Associação e administração - Remover produto](membership-and-administration/_static/image8.png)
12. Selecione **produtos** no menu de navegação superior para confirmar que o produto foi removido.
13. Clique em **logoff** existir de modo de administração.   
 Observe que não mostra o painel de navegação superior a **Admin** item de menu.

## <a name="summary"></a>Resumo

Neste tutorial, você adicionado a uma função personalizada e um usuário que pertence à função personalizada, acesso restrito para a pasta de administração e a página e fornecido a navegação para o usuário que pertencem à função personalizada. Associação de modelo é usado para popular um **DropDownList** controle com dados. Você já implementou o **FileUpload** controle e controles de validação. Além disso, você aprendeu como adicionar e remover produtos de um banco de dados. O tutorial Avançar, você aprenderá como implementar roteamento ASP.NET.

## <a name="additional-resources"></a>Recursos adicionais

[Web. config - autorização elemento](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[Identidade do ASP.NET](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Implantar um aplicativo de formulários da Web do ASP.NET seguro com associação, OAuth e o banco de dados SQL em um Site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versão de avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
[Anterior](checkout-and-payment-with-paypal.md)
[Próximo](url-routing.md)
