---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Associação e administração | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para nós...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 58eda260ccf2d0f237aaade65017760ed87a8c4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368292"
---
<a name="membership-and-administration"></a>Associação e administração
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Este tutorial mostra como atualizar o aplicativo de exemplo Wingtip Toys para adicionar uma função personalizada e usar a identidade do ASP.NET. Ele também mostra como implementar uma página de administração do qual o usuário com uma função personalizada pode adicionar e remover produtos no site.

[O ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) é o sistema de associação usado para criar um aplicativo da web ASP.NET e está disponível no ASP.NET 4.5. O ASP.NET Identity é usado no modelo de projeto do Web Forms do Visual Studio 2013, bem como os modelos para [ASP.NET MVC](../../../../mvc/index.md), [API Web ASP.NET](../../../../web-api/index.md), e [o aplicativo de página única ASP.NET](../../../../single-page-application/index.md). Também, especificamente, você pode instalar o sistema de identidade do ASP.NET usando o NuGet quando você começa com um aplicativo Web vazio. No entanto, nessa série de tutoriais que você usa o **Web Forms**projecttemplate, que inclui o sistema de identidade do ASP.NET. O ASP.NET Identity torna mais fácil integrar dados de perfil de usuário específico com os dados de aplicativo. Além disso, o ASP.NET Identity permite que você escolha o modelo de persistência para perfis de usuário em seu aplicativo. Você pode armazenar os dados em um banco de dados do SQL Server ou outro armazenamento de dados, incluindo *NoSQL* armazenamentos de dados, como tabelas de armazenamento do Windows Azure.

Este tutorial se baseia no tutorial anterior intitulado "Check-out e pagamento com o PayPal" a série de tutoriais de Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como usar o código para adicionar uma função personalizada e um usuário para o aplicativo.
- Como restringir o acesso para a pasta de administração e a página.
- Como fornecer uma navegação para o usuário que pertence à função personalizada.
- Como usar a associação de modelo para preencher uma [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) controle com categorias de produtos.
- Como carregar um arquivo para o aplicativo web usando o [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) controle.
- Como usar controles de validação para implementar a validação de entrada.
- Como adicionar e remover os produtos do aplicativo.

## <a name="these-features-are-included-in-the-tutorial"></a>Esses recursos estão incluídos no tutorial:

- ASP.NET Identity
- Configuração e autorização
- Associação de modelos
- Validação não invasiva

Web Forms do ASP.NET fornece recursos de associação. Usando o modelo padrão, você tem funcionalidade integrada de associação que você pode usar imediatamente quando o aplicativo é executado. Este tutorial mostra como usar o ASP.NET Identity para adicionar uma função personalizada e atribuir um usuário a essa função. Você aprenderá a restringir o acesso à pasta de administração. Você adicionará uma página para a pasta de administração que permite que um usuário com uma função personalizada para adicionar e remover os produtos e visualizar um produto após ele ter sido adicionado.

## <a name="adding-a-custom-role"></a>Adicionando uma função personalizada

Usando a identidade do ASP.NET, você pode adicionar uma função personalizada e atribuir um usuário a essa função usando o código.

1. Na **Gerenciador de soluções**, clique com botão direito no *lógica* pasta e crie uma nova classe.
2. Nomeie a nova classe *RoleActions.cs*.
3. Modificar o código para que ele apareça da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. Na **Gerenciador de soluções**, abra o *Global.asax.cs* arquivo.
5. Modificar a *Global.asax.cs* arquivo adicionando o código realçado em amarelo para que ele apareça da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Observe que `AddUserAndRole` está sublinhado em vermelho. Clique duas vezes o código AddUserAndRole.  
   A letra "A" no início do método realçado será sublinhada.
7. Passe o mouse sobre a letra "A" e clique em interface do usuário que permite que você gerar um stub de método para o `AddUserAndRole` método. 

    ![Associação e Advministration - Gerar Stub do método](membership-and-administration/_static/image1.png)
8. Clique na opção:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Abra o *RoleActions.cs* do arquivo da *lógica* pasta.  
   O `AddUserAndRole` método foi adicionado ao arquivo de classe.
10. Modificar a *RoleActions.cs* arquivo removendo o `NotImplementedeException` e adicionando o código realçado em amarelo, para que ele apareça da seguinte maneira:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Primeiro, o código acima estabelece um contexto de banco de dados para o banco de dados de associação. O banco de dados de associação também é armazenado como um *. mdf* arquivo na *App\_dados* pasta. Você poderá exibir esse banco de dados depois que o primeiro usuário entrou nesse aplicativo web. 

> [!NOTE] 
> 
> Se você deseja armazenar os dados de associação, juntamente com os dados do produto, você pode considerar usar o mesmo **DbContext** que você usou para armazenar os dados de produto no código acima.


 O *interno* palavra-chave é um modificador de acesso para membros de tipo (como métodos ou propriedades) e tipos (como classes). Tipos ou membros internos são acessíveis somente em arquivos contidos no mesmo assembly *(. dll* arquivo). Quando você compila seu aplicativo, um arquivo de assembly *(. dll*) é criado que contém o código que é executado quando você executar o aplicativo. 

Um `RoleStore` objeto, que fornece gerenciamento de função, é criado com base no contexto do banco de dados.

> [!NOTE] 
> 
> Observe que, quando o `RoleStore` objeto é criado ele usa um genérico `IdentityRole` tipo. Isso significa que o `RoleStore` só pode conter `IdentityRole` objetos. Também, usando os genéricos, recursos na memória são tratados melhor.


Em seguida, o `RoleManager` de objeto, é criado com base no `RoleStore` objeto que você acabou de criar. o `RoleManager` objeto expõe relacionada à função API que pode ser usado para salvar automaticamente as alterações para o `RoleStore`. O `RoleManager` só pode conter `IdentityRole` objetos porque o código usa o `<IdentityRole>` tipo genérico.

Você chama o `RoleExists` método para determinar se a função "CanEdit" está presente no banco de dados de associação. Se não for, você pode criar a função.

Criando o `UserManager` objeto parece ser mais complicado do que o `RoleManager` controlar, no entanto é quase o mesmo. Ele apenas é codificado em uma linha em vez de várias. Aqui, o parâmetro que você está passando é criar uma instância como um novo objeto contido entre parênteses.

Em seguida crie o usuário "canEditUser", criando um novo `ApplicationUser` objeto. Em seguida, se você criar com êxito o usuário, adicione o usuário para a nova função.

> [!NOTE] 
> 
> O tratamento de erros será atualizado durante o tutorial "Tratamento de erros do ASP.NET" mais adiante nesta série de tutoriais.


Na próxima vez que o aplicativo é iniciado, o usuário chamado "canEditUser" será adicionado como a função "canEdit" do aplicativo de chamada. Posteriormente no tutorial, você fará logon como o usuário "canEditUser" para exibir os recursos adicionais que você será adicionado durante este tutorial. Para obter detalhes de API sobre a identidade do ASP.NET, consulte o [ASPNET Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Para obter mais detalhes sobre como inicializar o sistema de identidade do ASP.NET, consulte o [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Restringindo o acesso para a página de administração

O aplicativo de exemplo Wingtip Toys permite que usuários anônimos e os usuários autorizados a exibir e comprar produtos. No entanto, o usuário conectado que tenha a função "canEdit" personalizada pode acessar uma página restrita para adicionar e remover produtos.

#### <a name="add-an-administration-folder-and-page"></a>Adicionar uma pasta de administração e a página

Em seguida, você criará uma pasta chamada *Admin* aplicativo de exemplo para o usuário "canEditUser" que pertence à função personalizada da Wingtip Toys.

1. Clique com botão direito no nome do projeto (**Wingtip Toys**) em **Gerenciador de soluções** e selecione **Add**  - &gt; **nova pasta**.
2. Nomeie a nova pasta *Admin*.
3. Clique com botão direito do *Admin* pasta e, em seguida, selecione **Add**  - &gt; **Novo Item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
4. Selecione o <strong>Visual c#</strong> - &gt; <strong>Web</strong> grupo de modelos à esquerda. Na lista do meio, selecione <strong>Web Form com página mestra</strong>, nomeie- <em>AdminPage.aspx</em><strong>,</strong> e, em seguida, selecione <strong>adicionar</strong>.
5. Selecione o *Master* file como a página mestra e, em seguida, escolha **Okey**.

#### <a name="add-a-webconfig-file"></a>Adicionar um arquivo Web. config

Adicionando um *Web. config* do arquivo para o *Admin* pasta, você pode restringir o acesso à página contida na pasta.

1. Clique com botão direito do *Admin* pasta e selecione **Add**  - &gt; **Novo Item**.  
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Na lista de modelos de web do Visual c#, selecione <strong>arquivo de configuração Web</strong>na lista intermediária, aceite o nome padrão de <em>Web. config</em><strong>,</strong> e, em seguida, selecione <strong>Adicionar</strong>.
3. Substitua o conteúdo em XML existente a *Web. config* arquivo com o seguinte:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Salvar a *Web. config* arquivo. O *Web. config* arquivo Especifica que apenas o usuário que pertencem à função "canEdit" do aplicativo pode acessar a página contida na *Admin* pasta.

### <a name="including-custom-role-navigation"></a>Incluindo a navegação de função personalizada

Para habilitar o usuário da função "canEdit" personalizadas navegar até a seção de administração do aplicativo, você deve adicionar um link para o *Master* página. Somente os usuários que pertencem à função "canEdit" poderão ver os **Admin** vincular e acessar a seção de administração.

1. No Solution Explorer, localize e abra a *Master* página.
2. Para criar um link para o usuário da função "canEdit", adicione a marcação realçada em amarelo para a seguinte lista não ordenada `<ul>` elemento para que a lista é exibida como segue:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Abra o *Site.Master.cs* arquivo. Verifique as **Admin** link visível somente para o usuário "canEditUser" Adicionando o código realçado em amarelo para o `Page_Load` manipulador. O `Page_Load` manipulador será exibida da seguinte maneira:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Quando a página for carregada, o código verifica se o usuário conectado tem a função de "canEdit". Se o usuário pertence à função "canEdit", o elemento de intervalo que contém o link para o *AdminPage.aspx* página (e, consequentemente, o link de dentro da extensão) ficam visíveis.

### <a name="enabling-product-administration"></a>Habilitar administração de produto

Até agora, você criou a função "CanEdit" e adicionou um usuário de "canEditUser", uma pasta de administração e uma página de administração. Você definiu os direitos de acesso para a pasta de administração e a página e adicionou um link de navegação para o usuário da função "canEdit" para o aplicativo. Em seguida, você adicionará a marcação para o *AdminPage.aspx* página e de código para o *AdminPage.aspx.cs* arquivo code-behind que permitirá que o usuário com a função "CanEdit" adicionar e remover os produtos.

1. No **Gerenciador de soluções**, abra o *AdminPage.aspx* arquivo o *Admin* pasta.
2. Substitua a marcação existente pelo seguinte:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Em seguida, abra o *AdminPage.aspx.cs* arquivo de code-behind clicando com o *AdminPage.aspx* e clicando em **Exibir código**.
4. Substitua o código existente na *AdminPage.aspx.cs* arquivo code-behind com o código a seguir:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

No código que você inseriu para o *AdminPage.aspx.cs* arquivo de code-behind, uma classe chamada `AddProducts` faz o trabalho real de adição de produtos no banco de dados. Essa classe ainda não existe, portanto, você irá criá-lo agora.

1. No **Gerenciador de soluções**, com o botão direito do *lógica* pasta e, em seguida, selecione **Add**  - &gt; **Novo Item**.   
   A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual c#**  - &gt; **código** grupo de modelos à esquerda. Em seguida, selecione **classe**do meio lista e nomeie-o *AddProducts.cs*.   
   O novo arquivo de classe é exibido.
3. Substitua o código existente pelo seguinte:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

O *AdminPage.aspx* página permite que o usuário que pertencem à função "canEdit" adicionar e remover os produtos. Quando um novo produto é adicionado, os detalhes sobre o produto são validados e, em seguida, inseridos no banco de dados. O novo produto é imediatamente disponibilizado para todos os usuários do aplicativo web.

#### <a name="unobtrusive-validation"></a>Validação não invasiva

Os detalhes do produto que o usuário fornece sobre o *AdminPage.aspx* página são validados usando controles de validação (`RequiredFieldValidator` e `RegularExpressionValidator`). Esses controles usam automaticamente a validação não invasiva. Validação não invasiva permite que os controles de validação usar JavaScript para a lógica de validação do lado do cliente, o que significa que a página não requer uma viagem para o servidor a ser validado. Por padrão, a validação não invasiva está incluída na *Web. config* arquivo baseia-se a seguinte configuração:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Expressões regulares

O preço do produto sobre o *AdminPage.aspx* página é validada usando uma **RegularExpressionValidator** controle. Esse controle valida se o valor do controle de entrada associado (a caixa de texto "AddProductPrice") corresponde ao padrão especificado pela expressão regular. Uma expressão regular é uma notação de correspondência de padrões que permite que você rapidamente Localize e correspondência de padrões de caracteres específica. O **RegularExpressionValidator** controle inclui uma propriedade chamada `ValidationExpression` que contém a expressão regular usada para validar a entrada de preço, conforme mostrado abaixo:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Controle fileUpload

Além dos controles de entrada e validação, você adicionou o **FileUpload** o controle para o *AdminPage.aspx* página. Esse controle fornece a capacidade de carregar arquivos. Nesse caso, você está permitindo somente arquivos de imagem a ser carregado. No arquivo code-behind (*AdminPage.aspx.cs*), quando o `AddProductButton` é clicado, o código verifica a `HasFile` propriedade do **FileUpload** controle. Se o controle tem um arquivo e se o tipo de arquivo (com base na extensão de arquivo) é permitido, a imagem é salva para o *imagens* pasta e o *imagens/polegares* pasta do aplicativo.

#### <a name="model-binding"></a>Associação de modelos

Anteriormente nessa série de tutoriais, você usou a associação de modelo para preencher uma **ListView** controle, um **FormsView** controle, um **GridView** controle e um  **DetailView** controle. Neste tutorial, você deve usar a associação de modelo para preencher uma **DropDownList** controle com uma lista de categorias de produto.

A marcação que você adicionou para a *AdminPage.aspx* arquivo contém uma **DropDownList** controle chamado `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Você usa a associação de modelo para preencher isso **DropDownList** definindo o `ItemType` atributo e o `SelectMethod` atributo. O `ItemType` atributo especifica que você usa o `WingtipToys.Models.Category` de tipo quando o preenchimento do controle. Você definiu esse tipo no início desta série de tutoriais, criando o `Category` classe (mostrado abaixo). O `Category` classe está no *modelos* pasta dentro de *Category.cs* arquivo.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

O `SelectMethod` atributo do **DropDownList** controle especifica que você usa o `GetCategories` método (mostrado abaixo) que é incluído no arquivo code-behind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Esse método Especifica que um `IQueryable` interface é usada para avaliar uma consulta em relação a um `Category` tipo. O valor retornado é usado para preencher a **DropDownList** na marcação da página (*AdminPage.aspx*).

O texto exibido para cada item na lista é especificado, definindo o `DataTextField` atributo. O `DataTextField` atributo usa o `CategoryName` da `Category` classe (mostrado acima) para exibir cada categoria no **DropDownList** controle. O valor real que é passado quando um item é selecionado na **DropDownList** controle se baseia o `DataValueField` atributo. O `DataValueField` atributo é definido como o `CategoryID` à medida que define no `Category` classe (mostrado acima).

### <a name="how-the-application-will-work"></a>Como o aplicativo funcionará

Quando o usuário que pertencem à função "canEdit" navega para a página pela primeira vez, o `DropDownAddCategory` **DropDownList** controle é preenchido conforme descrito acima. O `DropDownRemoveProduct` **DropDownList** controle também é preenchido com os produtos usando a mesma abordagem. O usuário que pertencem à função "canEdit" seleciona o tipo de categoria e adiciona detalhes do produto (**nome**, **descrição**, **preço**, e **doarquivodeimagem**). Quando o usuário que pertencem à função "canEdit" clica o **adicionar produto** botão, o `AddProductButton_Click` manipulador de eventos é disparado. O `AddProductButton_Click` manipulador de eventos localizado no arquivo code-behind (*AdminPage.aspx.cs*) verifica o arquivo de imagem para verificar se ele corresponde os tipos de arquivos permitidos *(. gif*, *. PNG*, *JPEG*, ou *. jpg*). Em seguida, o arquivo de imagem é salvo em uma pasta do aplicativo de exemplo Wingtip Toys. Em seguida, o novo produto é adicionado ao banco de dados. Para realizar a adição de um novo produto, uma nova instância do `AddProducts` classe é criada e nomeada products. O `AddProducts` classe tem um método chamado `AddProduct`, e o objeto de produtos chama esse método para adicionar produtos ao banco de dados.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Se o código adiciona com êxito o novo produto no banco de dados, a página for recarregada com o valor de cadeia de caracteres de consulta `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Quando a página é recarregado, a cadeia de caracteres de consulta está incluída na URL. Por recarregar a página, o usuário que pertencem à função "canEdit" pode ver imediatamente as atualizações na **DropDownList** controles na *AdminPage.aspx* página. Além disso, ao incluir a cadeia de caracteres de consulta com a URL, a página pode exibir uma mensagem de êxito para o usuário que pertence à função "canEdit".

Quando o *AdminPage.aspx* página recargas, o `Page_Load` evento é chamado.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

O `Page_Load` manipulador de eventos verifica o valor de cadeia de caracteres de consulta e determina se é necessário mostrar uma mensagem de êxito.

## <a name="running-the-application"></a>Executando o aplicativo

Você pode executar o aplicativo agora para ver como você pode adicionar, exclusão e atualização de itens no carrinho de compras. O total do carrinho de compras refletirá o custo total de todos os itens no carrinho de compras.

1. No Solution Explorer, pressione **F5** para executar o aplicativo de exemplo Wingtip Toys.  
   O navegador é aberta e mostra a *default. aspx* página.
2. Clique o **faça logon no** link na parte superior da página. 

    ![Associação e administração - faça logon no Link](membership-and-administration/_static/image2.png)

   O *login. aspx* página é exibida.
3. Use o seguinte nome de usuário e senha:  
   Nome de usuário: canEditUser@wingtiptoys.com  
   Senha: Pa$ $word1 

    ![Associação e administração - página de logon](membership-and-administration/_static/image3.png)
4. Clique o **faça logon no** botão na parte inferior da página.
5. Na parte superior da página seguinte, selecione o **Admin** link para navegar até a *AdminPage.aspx* página. 

    ![Associação e administração - Link de administração](membership-and-administration/_static/image4.png)
6. Para testar a validação de entrada, clique o **adicionar produto** botão sem adicionar qualquer detalhes do produto. 

    ![Associação e administração - página de administração](membership-and-administration/_static/image5.png)

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
13. Clique em **fazer logoff** exista o modo de administração.   
    Observe que não mostra o painel de navegação superior a **Admin** item de menu.

## <a name="summary"></a>Resumo

Neste tutorial, você adicionou uma função personalizada e um usuário que pertence à função personalizada, o acesso restrito para a pasta de administração e a página e forneceu a navegação para o usuário que pertencem à função personalizada. Associação de modelo é usado para preencher uma **DropDownList** controle com os dados. Você já implementou as **FileUpload** controle e controles de validação. Além disso, você aprendeu a adicionar e remover os produtos de um banco de dados. No próximo tutorial, você aprenderá a implementar o roteamento do ASP.NET.

## <a name="additional-resources"></a>Recursos adicionais

[Web. config - autorização elemento](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[Identidade do ASP.NET](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Implantar um aplicativo de formulários da Web do ASP.NET seguro com associação, OAuth e banco de dados SQL em um Site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](checkout-and-payment-with-paypal.md)
> [Próximo](url-routing.md)
