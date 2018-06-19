---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interface do usuário e a navegação | Microsoft Docs
author: Erikre
description: Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para nós...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: d2d4101455a85c53e016e567c0cf1337642f1863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890236"
---
<a name="ui-and-navigation"></a>Interface do usuário e a navegação
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com o código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Neste tutorial, você modificará a interface do usuário do aplicativo Web padrão para oferecer suporte a recursos do aplicativo front Wingtip Toys repositório. Além disso, você adicionará simples e navegação de associação de dados. Este tutorial se baseia no tutorial anterior "Criar a camada de acesso a dados" e faz parte da série tutorial Wingtip Toys.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como alterar a interface do usuário para oferecer suporte a recursos do aplicativo front Wingtip Toys repositório.
- Como configurar um elemento do HTML5 para incluir a navegação de página.
- Como criar um controle controladas por dados para navegar para os dados de produto específico.
- Como exibir dados de um banco de dados criado usando o Entity Framework Code First.

ASP.NET Web Forms permitem que você crie conteúdo dinâmico para o aplicativo Web. Cada página da Web do ASP.NET é criada de forma semelhante a uma página da Web em HTML estática (uma página que não inclui o processamento baseado em servidor), mas a página da Web ASP.NET incluem elementos extras que o ASP.NET reconhece e processa para gerar HTML quando a página é executada.

Uma página HTML estático (*. htm* ou *. HTML* arquivo), o servidor atende um `Web` solicitação lendo o arquivo e enviá-lo como-para o navegador. Por outro lado, quando alguém solicita uma página da Web do ASP.NET (*. aspx* arquivo), a página é executada como um programa no servidor Web. Enquanto a página está em execução, ele pode executar qualquer tarefa que requeira o site da Web, incluindo o cálculo de valores, ler ou gravar as informações de banco de dados ou chamada de outros programas. Como sua saída, a página dinamicamente produz marcação (como elementos HTML) e envia essa saída dinâmica para o navegador.

## <a name="modifying-the-ui"></a>Modificando a interface do usuário

Você continuará a série de tutoriais, modificando o *Default.aspx* página. Você modificará a interface do usuário que já está estabelecida pelo modelo padrão usado para criar o aplicativo. O tipo de modificações que você fará são típicos durante a criação de qualquer aplicativo Web Forms. Isso será feito alterando o título, substituindo alguns conteúdos e remover conteúdo padrão desnecessários.

1. Abra ou alterne para o *Default.aspx* página.
2. Se a página é exibida no **Design** exibir, alterne para o **fonte** exibição.
3. Na parte superior da página de `@Page` alteração de diretiva, o `Title` atributo "Bem-vindo", conforme mostrado realçada em amarelo abaixo. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Também no *Default.aspx* página, substitua todo o conteúdo padrão contido o `<asp:Content>` marca para que a marcação aparece como abaixo. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Salve o *Default.aspx* página selecionando **salvar Default.aspx** do **arquivo** menu.

   Resultante *Default.aspx* página será exibida da seguinte maneira: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

No exemplo, você configurou o `Title` atributo da `@Page` diretiva. Quando o HTML é exibido em um navegador, o código do servidor `<%: Page.Title %>` resolve para o conteúdo de `Title` atributo.

A página de exemplo inclui os elementos básicos que constituem uma página da Web do ASP.NET. A página contém texto estático que você pode ter em uma página HTML, juntamente com os elementos que são específicos para ASP.NET. O conteúdo a *Default.aspx* página será integrada com o conteúdo de página mestra que será explicado posteriormente neste tutorial.

### <a name="page-directive"></a>@Page Diretiva

Web Forms do ASP.NET normalmente contêm diretivas que permitem que você especifique a página Propriedades e informações de configuração para a página. As diretivas são usadas pelo ASP.NET como instruções sobre como processar a página, mas eles não são renderizados como parte da marcação que é enviada para o navegador.

A diretiva mais comumente usada é a `@Page` diretiva, que permite que você especificar várias opções de configuração para a página, incluindo o seguinte:

1. O servidor de linguagem de código na página, como c# de programação.
2. Se a página é uma página de código do servidor diretamente na página, que é chamado de uma página de arquivo único, ou se é uma página de código em um arquivo de classe separada, que é chamado de uma página code-behind.
3. Se a página tem uma página mestra associada e, portanto, deve ser tratada como uma página de conteúdo.
4. Depuração e opções de rastreamento.

Se você não incluir um `@Page` diretiva na página, ou se a diretiva não incluir uma configuração específica, uma configuração será herdada do *Web. config* arquivo de configuração ou o *Machine. config* arquivo de configuração. O *Machine. config* arquivo fornece configurações adicionais para todos os aplicativos em execução em um computador.

> [!NOTE] 
> 
> O *Machine. config* também fornece detalhes sobre todas as configurações possíveis.


### <a name="web-server-controls"></a>Controles de servidor Web

Na maioria dos aplicativos Web Forms do ASP.NET, você irá adicionar controles que permitem que o usuário interaja com a página, como caixas de texto, botões, listas e assim por diante. Esses controles de servidor Web são semelhantes aos botões HTML e elementos de entrada. No entanto, eles são processados no servidor, permitindo que você use o código do servidor para definir suas propriedades. Esses controles também geração eventos que você pode manipular no código do servidor.

Controles de servidor usam uma sintaxe especial que ASP.NET reconhece quando a página é executada. O nome da marca para controles de servidor ASP.NET começa com um `asp:` prefixo. Isso permite que o ASP.NET reconhece e processar esses controles de servidor. O prefixo pode ser diferente se o controle não faz parte do .NET Framework. Além de `asp:` prefixo, controles de servidor ASP.NET também incluem o `runat="server"` atributo e um `ID` que você pode usar para referenciar o controle no código do servidor.

Quando a página é executada, o ASP.NET identifica os controles de servidor e executa o código que está associado esses controles. Muitos controles processam alguns HTML ou outra marcação para a página quando ele for exibido em um navegador.

### <a name="server-code"></a>Código do servidor

A maioria dos aplicativos Web Forms do ASP.NET incluem o código que é executado no servidor quando a página é processada. Conforme mencionado acima, o código de servidor pode ser usado para realizar diversas tarefas, como adicionar dados a um controle ListView. ASP.NET oferece suporte a vários idiomas para ser executado no servidor, incluindo c#, Visual Basic, j# e outras pessoas.

ASP.NET oferece suporte a dois modelos para escrever código de servidor para uma página da Web. No modelo de arquivo único, o código para a página está em um elemento script onde a marca de abertura inclui o `runat="server"` atributo. Como alternativa, você pode criar o código para a página em um arquivo de classe separada, que é conhecido como o modelo de code-behind. Nesse caso, a página de Web Forms do ASP.NET geralmente não contém nenhum código de servidor. Em vez disso, o `@Page` diretiva inclui informações que vinculam a *. aspx* página com seu arquivo code-behind associado.

O `CodeBehind` atributo contido no `@Page` diretiva especifica o nome do arquivo de classe separado e o `Inherits` atributo especifica o nome da classe dentro do arquivo de code-behind que corresponde à página.

### <a name="updating-the-master-page"></a>Atualizando a página mestra

Em Web Forms do ASP.NET, páginas mestras permitem que você crie um layout consistente para as páginas em seu aplicativo. Uma única página mestra define a aparência e o comportamento padrão que você deseja para todas as páginas (ou um grupo de páginas) em seu aplicativo. Em seguida, você pode criar páginas de conteúdo individuais que contêm o conteúdo que você deseja exibir, conforme explicado anteriormente. Quando os usuários solicitam as páginas de conteúdo, o ASP.NET os mescla com a página mestra para produzir saída que combina o layout da página mestra com o conteúdo da página de conteúdo.

O novo site precisa de um único logotipo para exibição em cada página. Para adicionar esse logotipo, você pode modificar o HTML da página mestra.

1. Em **Solution Explorer**, localize e abra a **Site.Master** página.
2. Se a página estiver no **Design** exibir, alterne para o **fonte** exibição.
3. Atualize a página mestra por **modificando ou adicionando** a marcação realçada em amarelo: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Este HTML exibirá a imagem chamada *jpg* do *imagens* pasta do aplicativo da Web, você adicionará posteriormente. Quando uma página que usa a página mestra é exibida em um navegador, o logotipo será exibido. Se um usuário clicar no logotipo, o usuário será navegue de volta para o *Default.aspx* página. A marca de âncora HTML `<a>` envolve o controle de imagem do servidor e permite que a imagem a ser incluído como parte do link. O `href` para a marca de âncora Especifica a raiz do atributo "`~/`" do site da Web como o local de link. Por padrão, o *Default.aspx* página é exibida quando o usuário navega para a raiz do site da Web. O **imagem** `<asp:Image>` controle de servidor inclui propriedades de adição, como `BorderStyle`, que renderizam como HTML quando exibido em um navegador.

### <a name="master-pages"></a>Páginas mestras

Uma página mestra é um arquivo do ASP.NET com a extensão. Master (por exemplo, *Site.Master*) com um layout predefinido que pode incluir texto estático, elementos HTML e controles de servidor. A página mestra é identificada por um especial `@Master` diretiva que substitui o `@Page` diretiva é usada para comum *. aspx* páginas.

Além de `@Master` diretiva, a página mestra também contém todos os elementos HTML de nível superior para uma página, como `html`, `head`, e `form`. Por exemplo, na página mestre adicionados acima, você use um HTML `table` para o layout, um `img` elemento para o logotipo da empresa, texto estático e controles de servidor para lidar com associação comum para seu site. Você pode usar qualquer HTML e quaisquer elementos ASP.NET como parte da página mestra.

Além de texto estático e controles que serão exibidos em todas as páginas, a página mestra também inclui um ou mais **ContentPlaceHolder** controles. Esses controles de espaço reservado definem áreas onde conteúdo substituível aparecerá. Por sua vez, o conteúdo substituível é definido em páginas de conteúdo, como *Default.aspx*, usando o **conteúdo** controle de servidor.

#### <a name="adding-image-files"></a>Adicionando arquivos de imagem

A imagem do logotipo citado acima, juntamente com todas as imagens de produto, deve ser adicionada ao aplicativo Web para que pode ser vistos quando o projeto é exibido em um navegador.

#### <a name="download-from-msdn-samples-site"></a>Faça o download do site de exemplos do MSDN:

[Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

O download inclui recursos de *WingtipToys ativos* pasta que são usados para criar o aplicativo de exemplo.

1. Se você ainda não o fez, baixe os arquivos de exemplo compactado usando o link acima do site de exemplos do MSDN.
2. Após o download, abra o arquivo. zip e copie o conteúdo para uma pasta local em seu computador.
3. Localize e abra a *WingtipToys ativos* pasta.
4. Arrastando e soltando, copie o *catálogo* pasta da pasta local para a raiz do projeto de aplicativo da Web no **Solution Explorer** do Visual Studio. 

    ![Interface do usuário e a navegação - copiar arquivos](ui_and_navigation/_static/image1.png)
5. Em seguida, crie uma nova pasta chamada *imagens* clicando com o **WingtipToys** project no **Solution Explorer** e selecionando **adicionar**  - &gt; **Nova pasta**.
6. Cópia a *jpg* arquivo o *WingtipToys ativos* pasta **Explorador de arquivos** para o *imagens* pasta do aplicativo Web projeto em **Solution Explorer** do Visual Studio.
7. Clique o **Mostrar todos os arquivos** opção na parte superior do **Solution Explorer** para atualizar a lista de arquivos, se você não vir os novos arquivos.  
  
    **Gerenciador de soluções** agora mostra os arquivos de projeto atualizado. 

    ![Interface do usuário e a navegação - Gerenciador de soluções](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Adição de páginas

Antes de adicionar a navegação para o aplicativo Web, adicionará duas novas páginas que você vai navegar para. Mais tarde esta série de tutoriais, você exibirá os produtos e os detalhes do produto nessas páginas novas.

1. Em **Solution Explorer**, clique com botão direito **WingtipToys**, clique em **adicionar**e, em seguida, clique em **Novo Item**.   
 A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual C#**  - &gt; **Web** grupo de modelos, à esquerda. Em seguida, selecione **Web Form com página mestra** do meio lista e nomeie-o *ProductList. aspx*. 

    ![Interface do usuário e a navegação - adicionar a caixa de diálogo Novo Item](ui_and_navigation/_static/image3.png)
3. Selecione **Site.Master** para anexar a página mestra para recém-criado *. aspx* página. 

    ![Interface do usuário e a navegação - selecione a página mestra](ui_and_navigation/_static/image4.png)
4. Adicionar uma página adicional denominada *ProductDetails.aspx* seguindo essas mesmas etapas.

### <a name="updating-bootstrap"></a>Atualização de inicialização

Usam os modelos de projeto do Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), uma estrutura de layout e temas criada pelo Twitter. Inicialização usa CSS3 para fornecer um design responsivo, o que significa layouts dinamicamente podem adaptar a tamanhos de janela de navegador diferente. Você também pode usar o recurso de temas da inicialização facilmente efetuar uma alteração na aparência do aplicativo. Por padrão, o modelo de aplicativo Web ASP.NET no Visual Studio 2013 inclui Bootstrap como um pacote do NuGet.

Neste tutorial, você irá alterar a aparência do aplicativo Wingtip Toys, substituindo os arquivos de inicialização CSS.

1. Em **Solution Explorer**, abra o *conteúdo* pasta.
2. Com o botão direito do *bootstrap.css* de arquivo e renomeá-lo como *original.css inicialização*.
3. Renomear o *bootstrap.min.css* para *original.min.css inicialização*.
4. Em **Solution Explorer**, com o botão direito do *conteúdo* pasta e selecione **Abrir pasta no Explorador de arquivos**.  
   O Explorador de arquivos será exibida. Você salvará uma arquivos CSS bootstrap baixados neste local.
5. Em seu navegador, vá para [ http://Bootswatch.com ](http://bootswatch.com/).
6. Role a janela do navegador até que você veja o tema Cerulean. 

    ![Interface do usuário e a navegação - Cerulean tema](ui_and_navigation/_static/image5.png)
7. Baixar ambos o *bootstrap.css* arquivo e o *bootstrap.min.css* o arquivo para o *conteúdo* pasta. Use o caminho para a pasta de conteúdo que é exibida no **Explorador de arquivos** janela aberta anteriormente.
8. Em **Visual Studio** na parte superior do **Solution Explorer**, selecione o **Mostrar todos os arquivos** opção para exibir os novos arquivos na pasta de conteúdo. 

    ![Interface do usuário e a navegação - Gerenciador de soluções](ui_and_navigation/_static/image6.png)

   Você verá os dois novos arquivos CSS no **conteúdo** pasta, mas observe que o ícone ao lado de cada nome de arquivo está esmaecido. Isso significa que o arquivo ainda não foi adicionado ao projeto.
9. Clique com botão direito do *bootstrap.css* e *bootstrap.min.css* arquivos e selecione **incluir no projeto**.   
   Quando você executa o aplicativo Wingtip Toys posteriormente nesse tutorial, será exibida a nova interface do usuário.

> [!NOTE] 
> 
> O modelo de aplicativo Web ASP.NET usa o *Bundle.config* arquivo na raiz do projeto para armazenar o caminho dos arquivos de inicialização CSS.


### <a name="modifying-the-default-navigation"></a>Modificar a navegação padrão

A navegação padrão para cada página no aplicativo pode ser modificada alterando o elemento de lista não ordenada de navegação que está na *Site.Master* página.

1. Em **Solution Explorer**, localize e abra a *Site.Master* página.
2. Adicione o link de navegação adicionais realçado em amarelo na lista não ordenada mostrado abaixo:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Como você pode ver no HTML acima, você modificou cada item de linha `<li>` que contém uma marca de âncora `<a>` com um link `href` atributo. Cada `href` aponta para uma página no aplicativo Web. No navegador, quando um usuário clica em um desses links (como **produtos**), eles serão navegar até a página contida no `href` (como **ProductList. aspx**). Você executará o aplicativo no final deste tutorial.

> [!NOTE] 
> 
> O til (`~`) caractere é usado para especificar que o `href` caminho começa na raiz do projeto.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Adicionando um controle de dados para exibir dados de navegação

Em seguida, você adicionará um controle para exibir todas as categorias do banco de dados. Cada categoria atuará como um link para o *ProductList. aspx* página. Quando um usuário clica em um link de categoria no navegador, eles serão navegar até a página de produtos e ver apenas os produtos associados a categoria selecionada.

Você usará um **ListView** controle para exibir todas as categorias contidas no banco de dados. Para adicionar um **ListView** controle para a página mestra:

1. No *Site.Master* página, adicione o seguinte realçado `<div>` elemento **depois** o `<div>` elemento que contém o `id="TitleContent"` que você adicionou anteriormente:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Esse código irá exibir todas as categorias do banco de dados. O **ListView** controle exibe cada nome de categoria como texto de link e inclui um link para o *ProductList. aspx* página com um valor de cadeia de caracteres de consulta que contém o `ID` da categoria. Definindo o `ItemType` propriedade o **ListView** controlar, a expressão de associação de dados `Item` está disponível na `ItemTemplate` nó e o controle se torna fortemente tipado. Você pode selecionar detalhes do `Item` objeto usando o IntelliSense, como especificar o `CategoryName`. Esse código está contido dentro do contêiner `<%#: %>` que marca uma expressão de associação de dados. Adicionando (:) ao final do `<%#` prefixo, o resultado da expressão de associação de dados é codificada em HTML. Quando o resultado é codificada em HTML, seu aplicativo fique melhor protegido contra sites de script injeção (XSS) e ataques de injeção de HTML.

> [!NOTE] 
> 
> **Dica**
> 
> Quando você adiciona o código, digitando durante o desenvolvimento, você pode ter certeza de que um membro válido de um objeto foi encontrado porque fortemente tipado a controles de dados mostram membros disponíveis com base no IntelliSense. Conforme você digita o código, como propriedades, métodos e objetos, o IntelliSense oferece opções de código apropriada ao contexto.


A próxima etapa, você implementará o `GetCategories` método para recuperar dados.

### <a name="linking-the-data-control-to-the-database"></a>Vinculando o controle de dados para o banco de dados

Antes de você pode exibir dados no controle de dados, você precisa vincular o controle de dados para o banco de dados. Para tornar o link, você pode modificar o código por trás do *Site.Master.cs* arquivo.

1. Em **Solution Explorer**, com o botão direito do *Site.Master* página e, em seguida, clique em **Exibir código**. O *Site.Master.cs* arquivo é aberto no editor.
2. Perto o início do *Site.Master.cs* de arquivo, adicione dois namespaces adicionais para que todos os namespaces incluídos aparecer da seguinte maneira:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Adicionar o realçado `GetCategories` método após o `Page_Load` manipulador de eventos da seguinte maneira:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

O código acima é executado quando qualquer página que usa a página mestra é carregada no navegador. O `ListView` controle (nomeado "categoryList") que você adicionou anteriormente neste tutorial usa associação de modelo para selecionar dados. Na marcação do `ListView` você definir o controle de controle `SelectMethod` propriedade para o `GetCategories` método, mostrado acima. O `ListView` controlar chamadas de `GetCategories` ciclo de método no momento apropriado na vida de página e vincula automaticamente os dados retornados. Você aprenderá mais sobre associação de dados do tutorial Avançar.

### <a name="running-the-application-and-creating-the-database"></a>Executar o aplicativo e criar o banco de dados

Anteriormente na série tutorial você criou uma classe de inicializador (chamada de "ProductDatabaseInitializer") e especificou desta classe no *global.asax.cs* arquivo. O Entity Framework irá gerar o banco de dados quando o aplicativo é executado na primeira vez, porque o `Application_Start` método contido o *global.asax.cs* arquivo chamará a classe de inicializador. A classe de inicializador usará as classes de modelo (`Category` e `Product`) que você adicionou anteriormente nesta série tutorial para criar o banco de dados.

1. Em **Solution Explorer**, com o botão direito do *Default.aspx* página e selecione **definir como página inicial**.
2. Em pressione do Visual Studio **F5**.   
 Levará algum tempo para configurar tudo durante essa primeira execução.   
    ![Interface do usuário e a navegação - janelas do navegador](ui_and_navigation/_static/image7.png)  
 Quando você executa o aplicativo, o aplicativo será compilado e o banco de dados denominado *wingtiptoys.mdf* será criado na *aplicativo\_dados* pasta. No navegador, você verá um menu de navegação de categoria. Esse menu foi gerado por recuperar as categorias do banco de dados. O seguinte tutorial, você implementará a navegação.
3. Feche o navegador para interromper o aplicativo em execução.

### <a name="reviewing-the-database"></a>Revisando o banco de dados

Abra o *Web. config* de arquivo e examine a seção de cadeia de caracteres de conexão. Você pode ver que o `AttachDbFilename` valor na cadeia de conexão aponta para o `DataDirectory` para o projeto de aplicativo Web. O valor `|DataDirectory|` é um valor reservado que representa o *aplicativo\_dados* pasta do projeto. Esta pasta é onde se encontra o banco de dados que foi criado a partir de classes de entidade.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Se o *aplicativo\_dados* pasta não está visível ou se a pasta estiver vazia, selecione o **atualizar** ícone e, em seguida, o **Mostrar todos os arquivos** ícone na parte superior do **Solution Explorer** janela. Expandindo a largura do **Solution Explorer** windows pode ser necessária para mostrar todos os ícones disponíveis.


Agora você pode inspecionar os dados contidos no *wingtiptoys.mdf* arquivo de banco de dados usando o **Server Explorer** janela.

1. Expanda o *aplicativo\_dados* pasta. Se o *aplicativo\_dados* pasta não estiver visível, consulte a observação acima.
2. Se o *wingtiptoys.mdf* arquivo de banco de dados não estiver visível, selecione o **atualizar** ícone e, em seguida, o **Mostrar todos os arquivos** no canto superior do **Gerenciador de soluções**  janela.
3. Clique com botão direito do *wingtiptoys.mdf* arquivo de banco de dados e selecione **abrir**.  
    **Gerenciador de servidores** é exibido. 

    ![Interface do usuário e a navegação - Gerenciador de servidores](ui_and_navigation/_static/image8.png)
4. Expanda o *tabelas* pasta.
5. Clique com botão direito do **produtos**de tabela e selecione **Mostrar dados da tabela**.  
 O **produtos** tabela é exibida. 

    ![Interface do usuário e a navegação - tabela de produtos](ui_and_navigation/_static/image9.png)
6. Este modo de exibição permite visualizar e modificar os dados de **produtos** tabela manualmente.
7. Fechar o **produtos** janela da tabela.
8. No **Server Explorer**, com o botão direito do **produtos** tabela novamente e selecione **abrir definição de tabela**.  
 Os dados de design para o **produtos** tabela é exibida. 

    ![Interface do usuário e a navegação - Design de produtos](ui_and_navigation/_static/image10.png)
9. No **T-SQL** guia, você verá a instrução SQL DDL que foi usada para criar a tabela. Você também pode usar a interface do usuário no **Design** guia para modificar o esquema.
10. No **Server Explorer**, clique com botão direito **WingtipToys** de banco de dados e selecione **fechar Conexão**.   
 Desanexando o banco de dados do Visual Studio, o esquema de banco de dados poderá ser modificado posteriormente na série tutorial.
11. Retorne ao **Solution Explorer**selecionando o **Solution Explorer** guia na parte inferior da **Server Explorer** janela.

## <a name="summary"></a>Resumo

Este tutorial da série, você adicionou alguma interface do usuário básica, gráficos, páginas e navegação. Além disso, você executou o aplicativo Web, que criou o banco de dados de classes de dados que você adicionou no tutorial anterior. Você também exibiu o conteúdo do *produtos* tabela do banco de dados, exibindo o banco de dados diretamente. O seguinte tutorial, você exibirá itens de dados e os detalhes do banco de dados.

## <a name="additional-resources"></a>Recursos adicionais

[Introdução à programação de páginas da Web ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Visão geral sobre controles de servidor Web do ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Tutorial CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Anterior](create_the_data_access_layer.md)
> [Próximo](display_data_items_and_details.md)
