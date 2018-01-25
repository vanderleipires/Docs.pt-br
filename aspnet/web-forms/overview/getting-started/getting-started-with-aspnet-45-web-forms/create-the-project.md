---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Criar o projeto | Microsoft Docs
author: Erikre
description: "Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para nós..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 094733dcbe31486385dda2f8b44ba77a17486c82
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="create-the-project"></a>Criar o projeto
====================
Por [Erik Reitan](https://github.com/Erikre)

[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta série de tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para Web. Um Visual Studio 2013 [projeto com o código-fonte c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponível para acompanhar esta série de tutoriais.


Neste tutorial você criar, analisar e executar o projeto padrão no Visual Studio, o que permitirá que você se familiarize com os recursos do ASP.NET. Além disso, examine o ambiente do Visual Studio.

## <a name="what-youll-learn"></a>O que você aprenderá:

- Como criar um novo projeto de formulários da Web.
- A estrutura do arquivo do projeto Web Forms.
- Como executar o projeto no Visual Studio.
- Os diferentes recursos do aplicativo de formulários da Web padrão.
- Algumas noções básicas sobre como usar o ambiente do Visual Studio.

## <a name="creating-the-project"></a>Criando o Projeto

1. Abra o Visual Studio.
2. Selecione **novo projeto** do **arquivo** menu do Visual Studio. 

    ![Criar o projeto - novo Item de Menu do projeto](create-the-project/_static/image1.png)
3. Selecione o **modelos**  - &gt; **Visual C#**  - &gt; **Web** grupo de modelos, à esquerda.
4. Escolha o **aplicativo Web ASP.NET** modelo na coluna central.  
 Esta série de tutoriais estiver usando o .NET Framework 4.5.2.
5. Nomeie o projeto *WingtipToys* e escolha o **Okey** botão. 

    ![Criar o projeto - caixa de diálogo Novo projeto](create-the-project/_static/image2.png)

    > [!NOTE]
    > É o nome do projeto nesta série de tutoriais **WingtipToys**. É recomendável que você use isso *exata* nome do projeto para que o código fornecido em toda a série de tutoriais funcionando conforme o esperado.
6. Em seguida, selecione o **Web Forms** modelo e escolha o **criar projeto** botão.  

    ![Criar o projeto - novo modelo de projeto](create-the-project/_static/image3.png)

O projeto levará algum tempo para criar. Quando estiver pronto, abra o **Default.aspx** página.

![Criar o projeto - novo modelo de projeto](create-the-project/_static/image4.png)

Você pode alternar entre **Design** exibição e **fonte** exibição selecionando uma opção na parte inferior da janela Central. **Design** exibe páginas da Web ASP.NET, páginas mestras, páginas de conteúdo, páginas HTML e controles de usuário usando uma exibição da próxima ao WYSIWYG. **Origem** exibe a marcação HTML para a página da Web, você pode editar.

> [!TIP] 
> 
> **Noções básicas sobre a estrutura do ASP.NET**
> 
> Web Forms do ASP.NET permite que você compilar sites dinâmicos usando um modelo familiar de arrastar e soltar, controlada por evento. Uma superfície de design e centenas de controles e componentes lhe permitem criar rapidamente sofisticados, poderosos sites baseados em UI com acesso a dados. O repositório de brinquedos Wingtip é baseado em Web Forms do ASP.NET, mas muitos dos conceitos de que aprender nesta série tutorial são aplicáveis a todos os do ASP.NET.
> 
> O ASP.NET oferece quatro estruturas de desenvolvimento principal:
> 
> - [Web Forms do ASP.NET](../../../index.md)  
>  A estrutura de Web Forms tem como alvo os desenvolvedores que preferem declarativa e com base em controle de programação, como Microsoft Windows Forms (WinForms) e XAML/WPF/Silverlight. Ele oferece um modelo de WYSIWYG desenvolvimento controlado por designer, portanto é popular com os desenvolvedores um ambiente de desenvolvimento (RAD) rápido de aplicativos para desenvolvimento na web. Se você for novo para programação da web e estiver familiarizado com as ferramentas de desenvolvimento de cliente Microsoft RAD tradicionais (por exemplo, para o Visual Basic e Visual c#), você pode criar rapidamente um aplicativo da web sem a necessidade de experiência em HTML e JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC tem como alvo os desenvolvedores interessados em padrões e princípios de desenvolvimento controlado por testes, a separação de preocupações, inversão de controle (IoC) e injeção de dependência (DI). Essa estrutura incentiva separando a camada de lógica comercial de um aplicativo web da camada de apresentação.
> - [Páginas da Web do ASP.NET](../../../../web-pages/index.md)  
>  Páginas da Web ASP.NET tem como alvo os desenvolvedores que desejam uma história de desenvolvimento da web simples, juntamente com as linhas do PHP. No modelo de páginas da Web, crie páginas HTML e, em seguida, adicionar código de servidor para a página para controlar dinamicamente como essa marcação é renderizada. Páginas da Web é especificamente projetado para ser uma estrutura leve e é o ponto de entrada mais fácil no ASP.NET para pessoas que conhecer HTML, mas podem não ter ampla experiência de programação - por exemplo, alunos ou interessados. Também é uma boa maneira para desenvolvedores da web que conhecem o PHP ou outras estruturas semelhantes para começar a usar o ASP.NET.
> - [Aplicativo de página única do ASP.NET](../../../../single-page-application/index.md)  
>  Aplicativo de página ASP.NET única (SPA) ajuda a criar aplicativos que incluem significativas interações de cliente usando HTML 5, 3 de CSS e JavaScript. O ASP.NET e Web Tools 2012.2 atualização é fornecido um novo modelo para criar aplicativos de única página usando Knockout. js e a API da Web do ASP.NET. Além do novo modelo SPA, novos modelos SPA criados pela comunidade também estão disponíveis para download.
> 
> Além de quatro estruturas de desenvolvimento principal, o ASP.NET também oferece tecnologias adicionais importante estar atento e esteja familiarizado com, mas não são cobertas por esta série de tutoriais:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -uma estrutura para criar serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.
> - [ASP.NET SignalR](../../../../signalr/index.md) -uma biblioteca que facilita a funcionalidade de desenvolvimento da web em tempo real.


### <a name="reviewing-the-project"></a>Revisando o projeto

No Visual Studio, o **Solution Explorer** janela permite que você gerencie arquivos para o projeto. Vamos analisar as pastas que foram adicionados ao seu aplicativo no **Gerenciador de soluções**. O modelo de aplicativo da web adiciona uma estrutura de pasta básica:

![Criar o projeto - Gerenciador de soluções](create-the-project/_static/image5.png)

Visual Studio cria alguns arquivos para o seu projeto e pastas inicias. Os primeiros arquivos que você estará trabalhando com posteriormente neste tutorial são os seguintes:

| **Arquivo** | **Finalidade** |
| --- | --- |
| *Default.aspx* | Normalmente a primeira página exibida quando o aplicativo é executado em um navegador. |
| *Site.Master* | Uma página que permite que você crie um layout e uso padrão um comportamento consistente para as páginas em seu aplicativo. |
| *Global.asax* | Um arquivo opcional que contém código para responder a eventos de nível de sessão e de nível de aplicativo gerados pelo ASP.NET ou por módulos HTTP. |
| *Web.config* | Os dados de configuração para um aplicativo. |

### <a name="running-the-default-web-application"></a>Executando o aplicativo Web padrão

O aplicativo Web padrão fornece uma experiência avançada com base no suporte para a funcionalidade interna. Sem alterações para o projeto de formulários da Web padrão, o aplicativo está pronto para ser executado no navegador da Web local.

1. Pressione a ***F5*** chave no Visual Studio.   
 O aplicativo irá criar e exibir no navegador da Web.  

    ![Criar o projeto - página padrão](create-the-project/_static/image6.png)
2. Depois que você tiver concluído revisão do aplicativo em execução, feche a janela do navegador.

Há três páginas principais neste aplicativo de Web padrão: *Default.aspx* (base), *About*, e *Contact.aspx*. Cada uma dessas páginas pode ser alcançada na barra de navegação superior. Também há duas páginas adicionais contidas na pasta de conta, a página de Register e página de Login.aspx. Essas duas páginas permitem que você use os recursos de associação do ASP.NET para criar, armazenar e validar as credenciais do usuário.

## <a name="aspnet-web-forms-background"></a>Web Forms do ASP.NET em segundo plano

Web Forms do ASP.NET são páginas que se baseiam na tecnologia do Microsoft ASP.NET, no qual o código que é executado no servidor dinamicamente gera saída de página da Web para o dispositivo cliente ou navegador. Uma página de Web Forms do ASP.NET automaticamente renderiza HTML compatível com o navegador correto para recursos como estilos, layout e assim por diante. Formulários da Web são compatíveis com qualquer linguagem com suporte pelo .NET common language runtime, como Microsoft Visual Basic e Microsoft Visual c#. Além disso, os formulários da Web são criados no [do Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), que fornece benefícios, como um ambiente gerenciado, a segurança de tipo e a herança.

Quando uma página de Web Forms do ASP.NET é executado, a página passa por um ciclo de vida em que ela executa uma série de etapas de processamento. Essas etapas incluem a inicialização, criando controles, restaurando e mantendo estado, executando o código de manipulador de eventos e processamento. Como você se familiarizar com o poder do ASP.NET Web Forms, é importante entender o [ciclo de vida de página ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) para que você pode escrever o código no estágio do ciclo de vida apropriado para o efeito que pretende.

Quando um servidor Web recebe uma solicitação para uma página, ele encontra a página, processa-os, envia para o navegador e, em seguida, descarta todas as informações de página. Se o usuário solicita a mesma página novamente, o servidor repete a sequência inteira, reprocessamento a página a partir do zero. Ou seja, um servidor não tem memória de páginas que ele tem páginas processados são sem monitoração de estado. A estrutura de página ASP.NET manipula automaticamente a tarefa de manter o estado da sua página e seus controles e ele lhe fornece maneiras explícita para manter o estado de informações específicas do aplicativo.

> [!TIP] 
> 
> **Recursos de aplicativos Web no modelo de aplicativo Web Forms**
> 
> O modelo de aplicativo do ASP.NET Web Forms fornece um conjunto avançado de funções internas. Ele fornece não só você com um *aspx* página, um *About* página, um *Contact.aspx* página, mas também inclui a funcionalidade de associação que registra a usuários e salva suas credenciais para que eles podem fazer logon no seu site. Esta visão geral fornece mais informações sobre os recursos contidos no modelo de aplicativo do ASP.NET Web Forms e como eles são usados no aplicativo Wingtip Toys.
> 
> **Associação**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) identidade armazena credenciais dos usuários em um banco de dados criado pelo aplicativo. Quando os usuários fazem logon, o aplicativo valida suas credenciais ao ler o banco de dados. Seu projeto *conta* pasta contém os arquivos que implementam as diversas partes da associação: registrar, logon, alterar uma senha e autorizar o acesso. Além disso, o ASP.NET Web Forms oferece suporte a OpenID e OAuth. Esses aprimoramentos de autenticação permitem que os usuários entrem no seu site usando as credenciais existentes, de tal contas como Facebook, Twitter, Windows Live e Google.
> 
> ![Criar o projeto - Gerenciador de soluções (identidade do ASP.NET)](create-the-project/_static/image7.png)
> 
> Por padrão, o modelo cria um banco de dados de associação usando um nome de banco de dados padrão em uma instância do SQL Server Express LocalDB, o servidor de banco de dados de desenvolvimento que acompanha o Visual Studio Express 2013 para Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) é uma versão leve do SQL Server que tem muitos recursos de programação de um banco de dados do SQL Server. SQL Server Express LocalDB é executado no modo de usuário e tem uma instalação rápida sem configuração que tem uma lista curta de pré-requisitos de instalação. No Microsoft SQL Server, qualquer banco de dados ou código Transact-SQL pode ser movido do SQL Server Express LocalDB para SQL Server e SQL Azure sem todas as etapas de atualização. Portanto, o SQL Server Express LocalDB pode ser usado como um ambiente de desenvolvedor para aplicativos voltados para todas as edições do SQL Server. SQL Server Express LocalDB habilita os recursos, como procedimentos armazenados, funções definidas pelo usuário e agregações, integração do .NET Framework, os tipos espaciais e outras pessoas que não estão disponíveis no SQL Server Compact.
> 
> **Páginas mestras**
> 
> Um [página mestra ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) define uma aparência e comportamento consistentes para todas as páginas em seu aplicativo. O layout da página mestra mescla com o conteúdo de uma página de conteúdo individual para produzir a página final que o usuário vê. No aplicativo Wingtip Toys, você deve modificar o *Site.master* página mestra para que todas as páginas no site do Wingtip Toys compartilham a mesma barra diferente de logotipo e navegação.
> 
> **HTML5**
> 
> O modelo de aplicativo do ASP.NET Web Forms suporta [HTML5](http://www.w3schools.com/html/html5_intro.asp), que é a versão mais recente da linguagem HTML. HTML5 oferece suporte a novos elementos e recursos que tornam mais fácil criar sites da Web.
> 
> **Modernizr**
> 
> Para navegadores que não oferecem suporte a HTML5, você pode usar [Modernizr](http://www.modernizr.com/). Modernizr é uma biblioteca de JavaScript do código-fonte aberto que pode detectar se um navegador dá suporte a recursos HTML5 e habilitá-los, se não existir. No modelo de aplicativo do ASP.NET Web Forms, Modernizr é instalado como um pacote do NuGet.
> 
> **Bootstrap**
> 
> Usam os modelos de projeto do Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), uma estrutura de layout e temas criada pelo Twitter. Inicialização usa CSS3 para fornecer um design responsivo, o que significa layouts dinamicamente podem adaptar a tamanhos de janela de navegador diferente. Você também pode usar o recurso de temas da inicialização facilmente efetuar uma alteração na aparência do aplicativo. Por padrão, o modelo de aplicativo Web ASP.NET no Visual Studio 2013 inclui Bootstrap como um pacote do NuGet.
> 
> **Pacotes do NuGet**
> 
> O modelo de aplicativo do ASP.NET Web Forms inclui um conjunto de [NuGet](http://www.nuget.org/) pacotes. Esses pacotes fornecem funcionalidade em componentes na forma de ferramentas e bibliotecas de código-fonte aberto. Há uma ampla variedade de pacotes para ajudá-lo a criar e testar seus aplicativos. O Visual Studio facilita a adicionar, remover e atualizar pacotes do NuGet. Os desenvolvedores podem criar e adicionar pacotes ao NuGet também.
> 
> ![Criar o projeto - caixa de diálogo do NuGet](create-the-project/_static/image8.png)
> 
> Quando você instala um pacote, o NuGet copia arquivos para sua solução e automaticamente faz com que as alterações são necessárias, como a adição de referências e alterar a configuração associada ao aplicativo Web. Se você optar por remover a biblioteca, o NuGet remove arquivos e reverte quaisquer alterações feita em seu projeto para que nenhum desordem for deixado. NuGet está disponível a partir de **ferramentas** menu do Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) é uma biblioteca JavaScript rápida e concisas que simplifica o desvio de documento HTML, manipulação de eventos, animação e interações de Ajax para desenvolvimento web rápido. A biblioteca JavaScript jQuery é incluída no modelo de aplicativo de Web Forms do ASP.NET como um pacote do NuGet.
> 
> **Validação discreta**
> 
> Controles de validação interna foram configurados para usar JavaScript discreto para lógica de validação do lado do cliente. Significativamente, isso reduz a quantidade de JavaScript processado embutido na marcação da página e reduz o tamanho da página geral. Validação discreta é adicionada globalmente para o modelo de aplicativo do ASP.NET Web Forms com base na configuração do &lt;appSettings&gt; elemento o *Web. config* arquivo na raiz do aplicativo.
> 
> **Entity Framework Code First**
> 
> Além de recursos no modelo de aplicativo do ASP.NET Web Forms, usa o aplicativo Wingtip Toys [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), que é uma biblioteca do NuGet que permite o desenvolvimento centrado em código ao trabalhar com dados. Em suma, ele cria a parte do banco de dados do seu aplicativo com base no código que você escreve. Usando o Entity Framework, recuperar e manipular dados como objetos fortemente tipados. Isso permite que você se concentre na lógica de negócios em seu aplicativo em vez dos detalhes de como os dados são acessados.
> 
> Para obter informações adicionais sobre as bibliotecas de instalado e os pacotes incluídos com o modelo de Web Forms do ASP.NET, consulte a lista de pacotes do NuGet instaladas. Para fazer isso, no Visual Studio, crie um novo projeto de formulários da Web, selecione **ferramentas**  - &gt; **Gerenciador de biblioteca de pacote**  - &gt; **gerenciar Pacotes do NuGet para a solução**e selecione **pacotes instalados** no **gerenciar pacotes NuGet** caixa de diálogo.


### <a name="touring-visual-studio"></a>O Visual Studio de passeio

O principal do windows no Visual Studio incluem o **Solution Explorer**, o **Server Explorer** (**Pesquisador de objetos de banco de dados** no Express), o **propriedades Janela**, o **caixa de ferramentas**, o **barra de ferramentas**e o **janela do documento**.

![Criar o projeto - caixa de diálogo do NuGet](create-the-project/_static/image9.png)

Para obter mais informações sobre o Visual Studio, consulte [guia Visual para o Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Resumo

Neste tutorial você criou, examinados e executar o aplicativo de formulários da Web padrão. Ter revisado os diferentes recursos do aplicativo de formulários da Web padrão e aprendeu alguns conceitos básicos sobre como usar o ambiente do Visual Studio. Os tutoriais a seguir, você criará a camada de acesso a dados.

## <a name="additional-resources"></a>Recursos adicionais

[Escolhendo o direito de modelo de programação](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Projetos de aplicativos Web versus projetos de Site](https://msdn.microsoft.com/library/dd547590.aspx)   
[Visão geral de páginas de formulários da Web do ASP.NET](https://msdn.microsoft.com/library/428509ah.aspx)

>[!div class="step-by-step"]
[Anterior](introduction-and-overview.md)
[Próximo](create_the_data_access_layer.md)
