---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Criar o projeto | Microsoft Docs
author: Erikre
description: Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para nós...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 754f085e3e43f7efa155f410d02a0d29d3349612
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912287"
---
<a name="create-the-project"></a>Criar o projeto
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutoriais ensinará os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Neste tutorial você criar, revise e execute o projeto padrão no Visual Studio, o que permitirá que você se familiarizar com os recursos do ASP.NET. Além disso, você revisará o ambiente do Visual Studio.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como criar um novo projeto de formulários da Web.
- A estrutura de arquivo do projeto Web Forms.
- Como executar o projeto no Visual Studio.
- Os diferentes recursos do aplicativo de formulários da Web padrão.
- Algumas noções básicas sobre como usar o ambiente do Visual Studio.

## <a name="creating-the-project"></a>Criando o Projeto

1. Abra o Visual Studio.
2. Selecione **novo projeto** da **arquivo** menu do Visual Studio. 

    ![Criar o projeto - novo Item de Menu do projeto](create-the-project/_static/image1.png)
3. Selecione o **modelos**  - &gt; **Visual c#**  - &gt; **Web** grupo de modelos à esquerda.
4. Escolha o **aplicativo Web ASP.NET** modelo na coluna central.  
 Esta série de tutoriais é usando o .NET Framework 4.5.2.
5. Nomeie o projeto *WingtipToys* e escolha o **Okey** botão. 

    ![Criar o projeto - caixa de diálogo Novo projeto](create-the-project/_static/image2.png)

    > [!NOTE]
    > É o nome do projeto nessa série de tutoriais **WingtipToys**. É recomendável que você use isso *exata* nome do projeto para que o código fornecido em toda a série de tutoriais funções conforme o esperado.

6. Clique o **alterar autenticação** botão. Selecione **contas de usuário individuais** e clique no **Okey** botão.

7. Selecione o **Web Forms** modelo e clique no **Okey** botão.

    ![Criar o projeto - novo modelo de projeto](create-the-project/_static/image3.png)

O projeto levará algum tempo para criar. Quando estiver pronto, abra o **default. aspx** página.

![Criar o projeto - novo modelo de projeto](create-the-project/_static/image4.png)

Você pode alternar entre **Design** exibição e **origem** exibição selecionando uma opção na parte inferior da janela Central. **Design** exibe páginas da Web ASP.NET, páginas mestras, páginas de conteúdo, páginas HTML e controles de usuário usando uma exibição quase WYSIWYG. **Origem** modo de exibição exibe a marcação HTML para a página da Web, você pode editar.

> [!TIP] 
> 
> **Noções básicas sobre as estruturas do ASP.NET**
> 
> Web Forms do ASP.NET permite que você compilar sites dinâmicos usando um modelo familiar de arrastar e soltar, controlada por evento. Uma superfície de design e centenas de controles e componentes permitem que você crie rapidamente sofisticados, poderosos sites baseados em UI com acesso a dados. A Wingtip Toys Store baseia-se nos Web Forms do ASP.NET, mas muitos conceitos que você saiba nessa série de tutoriais são aplicáveis a todos os do ASP.NET.
> 
> O ASP.NET oferece quatro estruturas de desenvolvimento principal:
> 
> - [Web Forms do ASP.NET](../../../index.md)  
>  A estrutura de Web Forms destinado a desenvolvedores que preferem a programação declarativa e baseada em controle, como o Microsoft Windows Forms (WinForms) e o WPF/XAML/Silverlight. Ele oferece um modelo de WYSIWYG desenvolvimento controlado por designer, portanto, ele é popular com desenvolvedores que buscam um ambiente de desenvolvimento (RAD) do rápido de aplicativos para desenvolvimento da web. Se você for novo para programação da web e estiver familiarizado com as ferramentas de desenvolvimento de cliente Microsoft RAD tradicionais (por exemplo, para o Visual Basic e Visual c#), você pode criar rapidamente um aplicativo da web sem a necessidade de experiência em HTML e JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC é destinado a desenvolvedores que estejam interessados em padrões e os princípios, como desenvolvimento controlado por teste, a separação de preocupações, inversão de controle (IoC) e injeção de dependência (DI). Essa estrutura incentiva a separar a camada de lógica comercial de um aplicativo web da camada de apresentação.
> - [Páginas da Web do ASP.NET](../../../../web-pages/index.md)  
>  Páginas da Web ASP.NET é destinado a desenvolvedores que desejam uma história do desenvolvimento da web simples, junto com as linhas do PHP. No modelo de páginas da Web, você cria páginas HTML e, em seguida, adicionar código de servidor para a página para controlar dinamicamente como a marcação é renderizada. Páginas da Web foi especificamente projetado para ser uma estrutura leve e é o ponto de entrada mais fácil ao ASP.NET para as pessoas que conhecer HTML, mas podem não ter ampla experiência em programação - por exemplo, os alunos ou amadores. Também é uma boa maneira para os desenvolvedores da web que conhecem o PHP ou outras estruturas semelhantes para começar a usar o ASP.NET.
> - [Aplicativo de página única do ASP.NET](../../../../single-page-application/index.md)  
>  Aplicativo de página ASP.NET única (SPA) ajuda a criar aplicativos que incluam significativas interações do lado do cliente usando HTML 5, 3 de CSS e JavaScript. O ASP.NET e Web Tools 2012.2 atualização é fornecido um novo modelo para criar aplicativos de página única usando o Knockout. js e API Web do ASP.NET. Além do novo modelo do SPA, novos modelos SPA criados pela comunidade também estão disponíveis para download.
> 
> Além de quatro estruturas de desenvolvimento principal, o ASP.NET também oferece tecnologias adicionais que são importantes para estar atento e esteja familiarizado com, mas não são abordadas nesta série de tutoriais:
> 
> - [API Web ASP.NET](../../../../web-api/index.md) -uma estrutura para criar serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.
> - [ASP.NET SignalR](../../../../signalr/index.md) -uma biblioteca que facilita a funcionalidade de desenvolvimento da web em tempo real.


### <a name="reviewing-the-project"></a>Revisando o projeto

No Visual Studio, o **Gerenciador de soluções** janela permite que você gerencie arquivos para o projeto. Vamos dar uma olhada nas pastas que foram adicionados ao seu aplicativo no **Gerenciador de soluções**. O modelo de aplicativo da web adiciona uma estrutura de pasta básica:

![Criar o projeto - Gerenciador de soluções](create-the-project/_static/image5.png)

Visual Studio cria algumas pastas inicias e os arquivos para seu projeto. Os primeiros arquivos que você estará trabalhando com posteriormente no tutorial são os seguintes:

| **Arquivo** | **Finalidade** |
| --- | --- |
| *Default.aspx* | Normalmente a primeira página exibida quando o aplicativo é executado em um navegador. |
| *Site.Master* | Uma página que permite que você crie um layout e o uso padrão comportamento consistente para páginas em seu aplicativo. |
| *Global.asax* | Um arquivo opcional que contém o código para responder a eventos de nível de sessão e de nível de aplicativo gerados pelo ASP.NET ou por módulos HTTP. |
| *Web.config* | Os dados de configuração para um aplicativo. |

### <a name="running-the-default-web-application"></a>Executando o aplicativo Web padrão

O aplicativo Web padrão fornece uma experiência avançada com base no suporte e funcionalidade interna. Sem nenhuma alteração ao projeto de formulários da Web padrão, o aplicativo está pronto para ser executado no seu navegador Web local.

1. Pressione a ***F5*** chave ao mesmo tempo no Visual Studio.   
 O aplicativo será compilado e exibir no navegador da Web.  

    ![Criar o projeto - a página padrão](create-the-project/_static/image6.png)
2. Depois de ter concluído de revisão do aplicativo em execução, feche a janela do navegador.

Há três páginas principais neste aplicativo de Web padrão: *default. aspx* (base), *About*, e *Contact*. Cada uma dessas páginas pode ser acessada na barra de navegação superior. Há também duas páginas adicionais contidas na pasta de conta, a página de Register e a página de login. aspx. Essas duas páginas permitem que você use os recursos de associação do ASP.NET para criar, armazenar e validar as credenciais do usuário.

## <a name="aspnet-web-forms-background"></a>Web Forms do ASP.NET em segundo plano

Web Forms do ASP.NET são páginas que são baseadas na tecnologia do Microsoft ASP.NET, no qual o código que é executado no servidor dinamicamente gera saída de página da Web para o navegador ou dispositivo cliente. Uma página de Web Forms do ASP.NET automaticamente renderiza o HTML em conformidade com o navegador correto para recursos, como estilos, layout e assim por diante. Formulários da Web são compatíveis com qualquer linguagem compatível com o .NET common language runtime, como Microsoft Visual Basic e o Microsoft Visual c#. Além disso, os formulários da Web são criados na [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), que fornece benefícios como um ambiente gerenciado, segurança de tipos e herança.

Quando uma página de Web Forms do ASP.NET é executado, a página passa por um ciclo de vida em que ela executa uma série de etapas de processamento. Essas etapas incluem a inicialização, criando uma instância de controles, restaurando e mantendo o estado, executando o código de manipulador de eventos e renderização. Como você se familiariza com o poder do Web Forms do ASP.NET, é importante entender os [ciclo de vida de página ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) para que você possa escrever o código no estágio do ciclo de vida apropriado para o efeito que pretende.

Quando um servidor Web recebe uma solicitação para uma página, ele encontra a página, processa-os, envia para o navegador e, em seguida, descarta todas as informações de página. Se o usuário solicita a mesma página novamente, o servidor repete a sequência inteira, reprocessamento a página a partir do zero. Em outras palavras, um servidor não tem memória de páginas que ele tenha páginas processadas são sem monitoração de estado. A estrutura da página ASP.NET manipula automaticamente a tarefa de manter o estado da sua página e seus controles, e ele fornece maneiras explícitas para manter o estado de informações específicas do aplicativo.

> [!TIP] 
> 
> **Recursos de aplicativos Web no modelo de aplicativo de formulários da Web**
> 
> O modelo de aplicativo do ASP.NET Web Forms fornece um rico conjunto de funcionalidade interna. Ele fornece não só você com uma *home. aspx* página, um *About* página, uma *Contact* página, mas também inclui a funcionalidade de associação que registra os usuários e salva suas credenciais para que eles podem fazer logon em seu site. Esta visão geral fornece mais informações sobre alguns dos recursos contidos no modelo aplicativo ASP.NET Web Forms e como eles são usados no aplicativo Wingtip Toys.
> 
> **Associação**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) identidade armazena credenciais dos usuários em um banco de dados criado pelo aplicativo. Quando os usuários fazer logon, o aplicativo valida suas credenciais ao ler o banco de dados. Seu projeto *conta* pasta contém os arquivos que implementam as várias partes da associação: registrar, fazer logon, alterar uma senha e autorizar o acesso. Além disso, o ASP.NET Web Forms dá suporte a OAuth e OpenID. Esses aprimoramentos de autenticação permitem que os usuários façam logon em seu site usando as credenciais existentes, de tais contas como Facebook, Twitter, Windows Live e Google.
> 
> ![Criar o projeto - Gerenciador de soluções (identidade do ASP.NET)](create-the-project/_static/image7.png)
> 
> Por padrão, o modelo cria um banco de dados de associação usando um nome de banco de dados padrão em uma instância do SQL Server Express LocalDB, o servidor de banco de dados de desenvolvimento que vem com o Visual Studio Express 2013 para Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) é uma versão leve do SQL Server que tem muitos recursos de programação de banco de dados do SQL Server. SQL Server Express LocalDB é executado no modo de usuário e tem uma instalação rápida sem configuração que tem uma lista curta de pré-requisitos de instalação. No Microsoft SQL Server, qualquer banco de dados ou código Transact-SQL pode ser movido do SQL Server Express LocalDB para SQL Server e SQL Azure sem nenhuma etapa de atualização. Assim, o SQL Server Express LocalDB pode ser usado como um ambiente de desenvolvimento para aplicativos voltados para todas as edições do SQL Server. SQL Server Express LocalDB permite que recursos como procedimentos armazenados, funções definidas pelo usuário e agregações, integração com o .NET Framework, tipos espaciais e outras pessoas que não estão disponíveis no SQL Server Compact.
> 
> **Páginas mestras**
> 
> Uma [página mestra ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) define uma aparência e comportamento consistentes para todas as páginas em seu aplicativo. Mescla o layout da página mestra com o conteúdo de uma página de conteúdo individual para produzir a página final que o usuário vê. O aplicativo Wingtip Toys, você modificará o *Master* página mestra para que todas as páginas no site da Wingtip Toys compartilham a mesma barra de navegação e o logotipo de diferenciada.
> 
> **HTML5**
> 
> O modelo de aplicativo do ASP.NET Web Forms suporta [HTML5](http://www.w3schools.com/html/html5_intro.asp), que é a versão mais recente da linguagem de marcação HTML. HTML5 oferece suporte a novos elementos e a funcionalidade que o tornam mais fácil de criar sites da Web.
> 
> **Modernizr**
> 
> Para navegadores que não dão suporte a HTML5, você pode usar [Modernizr](http://www.modernizr.com/). O Modernizr é uma biblioteca de JavaScript de código-fonte aberto que pode detectar se um navegador oferece suporte a recursos HTML5 e habilitá-los, se não existir. No modelo de aplicativo do ASP.NET Web Forms, o Modernizr é instalado como um pacote do NuGet.
> 
> **Bootstrap**
> 
> Usam os modelos de projeto do Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), uma estrutura de layout e temas criada por meio do Twitter. O Bootstrap usa CSS3 para fornecer design responsivo, o que significa layouts dinamicamente podem adaptar para tamanhos de janela de navegador diferente. Você também pode usar o recurso de temas do Bootstrap para facilmente fazer uma alteração na aparência do aplicativo. Por padrão, o modelo de aplicativo Web ASP.NET no Visual Studio 2013 inclui o Bootstrap como um pacote do NuGet.
> 
> **Pacotes do NuGet**
> 
> O modelo de aplicativo do ASP.NET Web Forms inclui um conjunto de [NuGet](http://www.nuget.org/) pacotes. Esses pacotes fornecem funcionalidade dividida em componentes na forma de ferramentas e bibliotecas de software livre. Há uma grande variedade de pacotes para ajudá-lo a criar e testar seus aplicativos. Visual Studio torna fácil adicionar, remover e atualizar pacotes do NuGet. Os desenvolvedores podem criar e adicionar pacotes ao NuGet também.
> 
> ![Criar o projeto - caixa de diálogo do NuGet](create-the-project/_static/image8.png)
> 
> Quando você instala um pacote, o NuGet copia arquivos para sua solução e faz automaticamente as alterações são necessárias, como adicionar referências e alterar a configuração associada ao seu aplicativo Web. Se você decidir remover a biblioteca, o NuGet remove arquivos e reverte quaisquer alterações feita em seu projeto para que nenhuma confusão é deixado. O NuGet está disponível a partir de **ferramentas** menu do Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) é uma biblioteca JavaScript rápida e concisa que simplifica a percorrer de documento HTML, manipulação de eventos, animação e interações de Ajax para desenvolvimento da web rápida. A biblioteca JavaScript jQuery está incluída no modelo de aplicativo do Web Forms do ASP.NET como um pacote do NuGet.
> 
> **Validação não invasiva**
> 
> Controles de validação interna foram configurados para usar JavaScript discreto para lógica de validação do lado do cliente. Isso significativamente reduz a quantidade de JavaScript renderizado embutidos na marcação da página e reduz o tamanho de página geral. Validação não invasiva é adicionada globalmente para o modelo de aplicativo do ASP.NET Web Forms com base na configuração na &lt;appSettings&gt; elemento da *Web. config* arquivo na raiz do aplicativo.
> 
> **Entity Framework Code First**
> 
> Além de recursos no modelo de aplicativo do ASP.NET Web Forms, usa o aplicativo Wingtip Toys [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), que é uma biblioteca do NuGet que permite o desenvolvimento centrada em código quando você trabalha com dados. Simplificando, ele cria a parte do banco de dados do seu aplicativo com base no código que você escreve. Usando o Entity Framework, recuperar e manipular os dados como objetos fortemente tipados. Isso permite que você se concentrar na lógica de negócios do seu aplicativo em vez dos detalhes de como os dados são acessados.
> 
> Para obter informações adicionais sobre as bibliotecas instaladas e pacotes incluídos com o modelo de Web Forms do ASP.NET, consulte a lista de pacotes NuGet instalados. Para fazer isso, no Visual Studio crie um novo projeto de formulários da Web, selecione **ferramentas** > **Gerenciador de pacotes NuGet** > **gerenciar pacotes NuGet para solução**e selecione **pacotes instalados** no **gerenciar pacotes NuGet** caixa de diálogo.

### <a name="touring-visual-studio"></a>Passeio Visual Studio

O principal do windows no Visual Studio incluem o **Gerenciador de soluções**, o **Gerenciador de servidores** (**Database Explorer** no Express), o **propriedades Janela**, o **caixa de ferramentas**, o **barra de ferramentas**e o **janela de documento**.

![Criar o projeto - caixa de diálogo do NuGet](create-the-project/_static/image9.png)

Para obter mais informações sobre o Visual Studio, consulte [guia Visual para o Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Resumo

Neste tutorial você criou, revisadas e executar o aplicativo de formulários da Web padrão. Você revisou os diferentes recursos do aplicativo de formulários da Web padrão e aprendido algumas noções básicas sobre como usar o ambiente do Visual Studio. Nos tutoriais a seguir, você criará a camada de acesso a dados.

## <a name="additional-resources"></a>Recursos adicionais

[Escolhendo o modelo de programação correto](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Projetos de aplicativos Web versus projetos de Site](https://msdn.microsoft.com/library/dd547590.aspx)   
[Visão geral de páginas de formulários da Web ASP.NET](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Anterior](introduction-and-overview.md)
> [Próximo](create_the_data_access_layer.md)
