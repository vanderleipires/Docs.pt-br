---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
title: Implantando um banco de dados (VB) | Microsoft Docs
author: rick-anderson
description: Implantar um aplicativo web ASP.NET envolve obter os arquivos necessários e os recursos do ambiente de desenvolvimento para o ambiente de produção. Para o Acelerador de desenvolvimento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 96ac3e69-04c7-4917-ad06-5f8968c3fbf1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 3dc5e9b4e189929b2b898b997b7577a623bdc8a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381316"
---
<a name="deploying-a-database-vb"></a>Implantando um banco de dados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_vb.pdf)

> Implantar um aplicativo web ASP.NET envolve obter os arquivos necessários e os recursos do ambiente de desenvolvimento para o ambiente de produção. Para aplicativos web controlados por dados, isso inclui o esquema de banco de dados e os dados. Este tutorial é a primeira de uma série que explora as etapas necessárias para implantar com êxito o banco de dados do ambiente de desenvolvimento para a produção.


### <a name="introduction"></a>Introdução

Implantar um aplicativo web ASP.NET envolve obter os arquivos necessários e os recursos do ambiente de desenvolvimento para o ambiente de produção. Ao longo dos últimos seis tutoriais nós examinamos Implantando um aplicativo simples da web resenhas de livros. Neste site de demonstração era composto por uma série de recursos do lado do servidor - páginas do ASP.NET, arquivos de configuração, um `Web.sitemap` arquivo e assim por diante – juntamente com recursos do lado do cliente, como imagens e arquivos CSS. Mas e quanto controlada por dados de aplicativos web? Quais etapas adicionais devem ser executadas para implantar um aplicativo web que usa um banco de dados?

Sobre os próxima vários tutoriais falaremos sobre as etapas necessárias para implantar um aplicativo web controlado por dados. Este tutorial começa examinando como obter um esquema do banco de dados s e o conteúdo do ambiente de desenvolvimento para o ambiente de produção, enquanto o tutorial subsequente examina as alterações de configuração necessárias. A seguir que vamos explorar os desafios de implantação de um banco de dados que usa os serviços de aplicativo (associação, funções, perfil e assim por diante).

## <a name="examining-the-updated-book-reviews-web-application"></a>Examinando o aplicativo de Web do catálogo atualizado revisões

Para demonstrar a implantação de um aplicativo web controlado por dados, eu ve atualizou o aplicativo de web de resenhas de livros de um site simple e estático para um orientado a dados. Como antes, há duas versões do aplicativo neste download tutorial s: um que usa o modelo de projeto de aplicativo Web e outro que usa o modelo de projeto de Site.

As resenhas de livros atualizado web aplicativo usa um [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) banco de dados, que é armazenado no site de s `App_Data` pasta (`~/App_Data/Reviews.mdf`). Se você tiver instalado no computador do SQL Server 2008, em seguida, a demonstração deve ser executado sem erro. Se você tiver uma versão anterior do SQL Server, você pode instalar o SQL Server 2008 livre Express Edition, ou você pode usar o banco de dados que o download de scripts disponíveis neste tutorial s ao criar o banco de dados por conta própria.

O `Reviews.mdf` banco de dados contém quatro tabelas:

- `Genres` -inclui um registro para cada gênero, como a tecnologia, ficção e negócios.
- `Books` -inclui um registro para cada revisão, com colunas, como `Title`, `GenreId`, `ReviewDate`, e `Review`, entre outros.
- `Authors` -inclui informações sobre cada autor que contribuiu para um livro revisado.
- `BooksAuthors` -uma tabela de junção de muitos-para-muitos que especifica quais autores tem escrito quais livros.
  

Figura 1 mostra um diagrama de ER dessas quatro tabelas.


[![O aplicativo de Web do catálogo revisões banco de dados é composto de quatro tabelas](deploying-a-database-vb/_static/image2.jpg)](deploying-a-database-vb/_static/image1.jpg) 

**Figura 1**: O aplicativo de Web do catálogo revisões s banco de dados é composto de quatro tabelas ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image3.jpg))


A versão anterior do site resenhas de livros tinha uma página ASP.NET separada para cada livro. Por exemplo, houve uma página chamada `~/Tech/TYASP35.aspx` que continha a revisão *ensinar por conta própria ASP.NET 3.5 in 24 horas*. Essa nova versão controlada por dados do site da Web tem as revisões armazenadas no banco de dados e uma única página do ASP.NET, Review.aspx?ID=*bookId*, que exibe a revisão para o catálogo especificado. Da mesma forma, há um Genre.aspx?ID=*genreId* página que lista os livros revisados no gênero especificado.

As figuras 2 e 3 mostrar o `Genre.aspx` e `Review.aspx` páginas em ação. Anote a URL na barra de endereços para cada página. Na Figura 2 it s Genre.aspx? ID = 85d164ba-1123-4 c 47-82a0-c8ec75de7e0e. Porque é 85d164ba-1123-4c47-82a0-c8ec75de7e0e o `GenreId` valor para o gênero de tecnologia, as leituras de título de página s "Revisões de tecnologia" e a lista com marcadores enumera essas análises no site que se enquadram nesse gênero.


[![A página de gênero de tecnologia](deploying-a-database-vb/_static/image5.jpg)](deploying-a-database-vb/_static/image4.jpg) 

**Figura 2**: A página de gênero de tecnologia ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image6.jpg))


[![A revisão para ensinar a mesmo ASP.NET 3.5 em 24 horas](deploying-a-database-vb/_static/image8.jpg)](deploying-a-database-vb/_static/image7.jpg) 

**Figura 3**: A revisão para *ensinar por conta própria ASP.NET 3.5 in 24 horas* ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image9.jpg))


O aplicativo web de revisões do livro também inclui uma seção de administração, em que os administradores podem adicionar, editar e excluir gêneros, revisões e informações do autor. Atualmente, qualquer visitante pode acessar a seção de administração. Um tutorial futuro, vamos adicionar suporte para contas de usuário e só permitir que usuários autorizados para as páginas de administração.

Se você baixar o aplicativo de resenhas de livros por favor, lembre-se de que sua finalidade é demonstrar a implantação de um aplicativo controlado por dados. Ele não apresenta as práticas recomendadas quanto ao design do aplicativo. Por exemplo, não há nenhum separada dados acesso DAL (camada); as páginas do ASP.NET se comunicar diretamente com o banco de dados por meio do controle SqlDataSource ou código ADO.NET em suas classes code-behind. Para obter uma análise mais detalhada na criação de aplicativos controlados por dados usando uma arquitetura em camadas, consulte a minha [ *trabalhando com dados* tutoriais](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bancos de dados no desenvolvimento Versus produção

Quando você iniciar o desenvolvimento em um aplicativo web controlado por dados, você deve especificar uma cadeia de caracteres de conexão do banco de dados, que fornece os detalhes do aplicativo sobre como conectar-se ao banco de dados. Essa cadeia de caracteres de conexão Especifica, entre outras coisas, o servidor de banco de dados, o nome do banco de dados e informações de segurança. Geralmente, o banco de dados usado pelo aplicativo durante o desenvolvimento é diferente do que o banco de dados usado quando ele s na produção. Há muitos benefícios do uso de bancos de dados diferentes para desenvolvimento versus produção. Don t ter outro banco de dados em meios de desenvolvimento você precisa se preocupar sobre acidentalmente modificar ou excluir dados em tempo real. Ele também permite colocar em dados fictícios de teste ou fazer alterações significativas no modelo de dados sem precisar se preocupar sobre os efeitos sobre o aplicativo em produção. A desvantagem de ter outro banco de dados em ambientes de desenvolvimento e produção é que, quando o aplicativo é implantado o banco de dados e quaisquer alterações pertinentes para o esquema de banco de dados s ou dados também devem ser implantados.

Antes da primeira implantação, há apenas uma instância do banco de dados, e essa instância é no ambiente de desenvolvimento. Ao implantar o aplicativo para produção pela primeira vez, deve não apenas copiar os arquivos de servidor e do lado do cliente necessários, mas também copiar o banco de dados do ambiente de desenvolvimento para o ambiente de produção. Isso é onde podemos está à direita agora com o aplicativo web de resenhas de livros - o banco de dados reside no `App_Data` pasta em nosso ambiente de desenvolvimento, mas não tiver ainda foram passados para o ambiente de produção.

Quando o aplicativo tiver sido implantado, há duas cópias do banco de dados. À medida que o aplicativo amadurece, novos recursos podem ser adicionados, a necessidade de uma alteração no modelo de dados (como adicionar novas colunas a tabelas existentes, fazendo alterações em colunas existentes, adicionando novas tabelas e assim por diante). Quando o aplicativo web é implantado em seguida, as alterações aplicadas ao banco de dados no ambiente de desenvolvimento, desde a última implantação deve ser aplicada no banco de dados de produção. Algumas estratégias para gerenciar esse processo são discutidas em um tutorial futuro. Este tutorial se concentra na implantação de banco de dados inteiro do ambiente de desenvolvimento para a produção.

## <a name="deploying-the-database-to-the-production-environment"></a>Implantando o banco de dados no ambiente de produção

O restante deste tutorial explica como implantar o banco de dados do ambiente de desenvolvimento para o ambiente de produção. Se você estiver acompanhando você precisa para certificar-se de que sua conta com seu provedor de host da web inclui o Microsoft SQL Server banco de dados de suporte. Você também precisará ter algumas informações em mãos, ou seja, o nome do servidor de banco de dados, o nome do banco de dados e o nome de usuário e a senha usada para se conectar ao banco de dados.

Conforme mencionado anteriormente neste tutorial, o banco de dados do site s resenhas de livros é um banco de dados do SQL Server 2008 Express Edition armazenado em do `App_Data` pasta. Chamasse a premissa de que a implantação de um banco de dados seria tão simple quanto copiar o `App_Data` pasta do ambiente de desenvolvimento para o ambiente de produção. No entanto, a maioria dos provedores de host da web não dão suporte a bancos de dados de hospedagem no `App_Data` pasta devido a motivos de segurança. Em vez disso, os hosts da web fornecem uma conta em um servidor de banco de dados do SQL Server em seus ambientes. Implantar o banco de dados do seu ambiente de desenvolvimento para o ambiente de produção requer colocar seu banco de dados registrado no servidor web host s banco de dados.

Então, como obter seu banco de dados do ambiente de desenvolvimento para o ambiente de produção? Há duas maneiras para fazer isso, dependendo de quais serviços suas ofertas de host da web. Com alguns hosts, tais como DiscountASP.NET, você pode FTP um backup de banco de dados ou o valor real `.mdf` do arquivo ao seu site e em seguida, no painel de controle, restaure o arquivo de backup ou anexar o `.mdf` arquivo para o servidor de banco de dados do SQL Server. Com essas ferramentas implantar o banco de dados é tão simple quanto copiar o `App_Data` pasta para o ambiente de produção e, em seguida, anexá-lo por meio do painel de controle. É talvez a maneira mais fácil e rápida de publicar seu banco de dados pela primeira vez.

Outra abordagem é usar o Assistente de publicação de banco de dados. O Assistente de publicação de banco de dados é um aplicativo de desktop do Windows que irão gerar os comandos SQL para criar seu esquema de s de banco de dados - a tabelas, procedimentos armazenados, exibições, funções definidas pelo usuário e assim por diante – e, opcionalmente, os dados em suas tabelas. Você pode conectar-se ao servidor web host provedor s banco de dados por meio do SQL Server Management Studio e, em seguida, execute este script para duplicar o banco de dados de produção. Ainda melhor, se seu provedor de host da web dá suporte a s Microsoft [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) que o script gerado pelo Assistente de publicação do banco de dados executado automaticamente no servidor de banco de dados em seu nome. Como o Assistente de publicação de banco de dados gera um script que cria o esquema de banco de dados s e os dados, ele funcionará, independentemente se o seu provedor de host da web oferece recursos como anexar um carregado `.mdf` arquivo.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Gerando os comandos SQL para criar o esquema de banco de dados e os dados usando o Assistente de publicação de banco de dados

Deixe o s percorrer usando o Assistente de publicação de banco de dados para implantar o banco de dados de resenhas de livros para a produção. Se você estiver usando o Visual Studio 2008 ou posteriores, o Assistente de publicação de banco de dados já está instalado. Se você estiver usando Visual Studio 2005, você precisará primeiro [Baixe e instale o](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) o assistente.

Abra o Visual Studio e navegue até o `Reviews.mdf` banco de dados. Se você estiver usando o Visual Web Developer, vá para o Gerenciador de banco de dados; Se você estiver usando o Visual Studio, use o Gerenciador de servidores. A Figura 4 mostra o `Reviews.mdf` banco de dados no Gerenciador de banco de dados no Visual Web Developer. Como mostra a Figura 4, o `Reviews.mdf` banco de dados é composto de quatro tabelas, três procedimentos armazenados e uma função definida pelo usuário.


[![Localize o banco de dados no banco de dados Explorer ou Gerenciador de servidores](deploying-a-database-vb/_static/image11.jpg)](deploying-a-database-vb/_static/image10.jpg) 

**Figura 4**: Localize o banco de dados no Server Explorer ou Gerenciador de banco de dados ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image12.jpg))


Clique com botão direito no nome do banco de dados e escolha a opção "Publicar no provedor de" no menu de contexto. Isso inicia o Assistente de publicação de banco de dados (consulte a Figura 5). Clique em Avançar após a tela inicial.


[![O tela inicial do Assistente de publicação de banco de dados](deploying-a-database-vb/_static/image14.jpg)](deploying-a-database-vb/_static/image13.jpg) 

**Figura 5**: O banco de dados de publicação inicial tela do assistente ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image15.jpg))


A segunda tela do assistente lista os bancos de dados acessíveis para o Assistente de publicação de banco de dados e permite que você escolha se deseja gerar script de todos os objetos no banco de dados selecionado ou escolher quais objetos para script. Selecione o banco de dados apropriado e deixe a opção "Script de todos os objetos no banco de dados selecionado" marcada.

> [!NOTE]
> Se você receber o erro "não existem objetos no banco de dados *databaseName* dos tipos passíveis de script por este assistente" ao clicar em Avançar na tela mostrada na Figura 6, certifique-se de que o caminho para o arquivo de banco de dados não é muito longo. Conforme observado na [este item de discussão](http://www.codeplex.com/sqlhost/Thread/View.aspx?ThreadId=11014) na página do Assistente de publicação de banco de dados de projeto, este erro pode ocorrer se o caminho para o arquivo de banco de dados é muito longo.


[![O tela inicial do Assistente de publicação de banco de dados](deploying-a-database-vb/_static/image17.jpg)](deploying-a-database-vb/_static/image16.jpg) 

**Figura 6**: O banco de dados de publicação inicial tela do assistente ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image18.jpg))


Na próxima tela você pode gerar um arquivo de script ou, se o host da web dá suporte a ele, publicar o banco de dados diretamente no seu servidor de banco de dados do web host provedor s. Como mostra a Figura 7, estou tendo o script gravado no arquivo `C:\REVIEWS.MDF.sql`.


[![O banco de dados para um arquivo de script ou publicá-lo diretamente no seu provedor de Host da Web](deploying-a-database-vb/_static/image20.jpg)](deploying-a-database-vb/_static/image19.jpg) 

**Figura 7**: o banco de dados para um arquivo de Script ou publicá-lo diretamente no seu provedor de Host da Web ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image21.jpg))


A tela subsequente solicita a você para uma variedade de opções de script. Você pode especificar se o script deve incluir as instruções drop para remover esses objetos existentes. Esse padrão é True, que é adequado ao implantar um banco de dados pela primeira vez. Você também pode especificar se o banco de dados de destino é o SQL Server 2000, SQL Server 2005 ou SQL Server 2008. Por fim, você pode indicar se deseja gerar script de esquema e dados, apenas os dados ou apenas o esquema. O esquema é a coleção de objetos de banco de dados, tabelas, procedimentos armazenados, exibições e assim por diante. Os dados são as informações que residem nas tabelas.

Como ilustra a Figura 8, eu temos o assistente configurado para remover objetos de banco de dados existentes, para gerar script para um banco de dados do SQL Server 2008 e publicar o esquema e os dados.


[![Especifique a publicação de opções](deploying-a-database-vb/_static/image23.jpg)](deploying-a-database-vb/_static/image22.jpg) 

**Figura 8**: especifique as opções de publicação ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image24.jpg))


As duas telas finais resumem as ações que estão prestes a serem tomadas e, em seguida, exibir o status do script. O resultado da execução do assistente é que temos um arquivo de script que contém os comandos SQL necessários para criar o banco de dados em produção e preenchê-lo com os mesmos dados como no desenvolvimento.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Executando os comandos SQL no banco de dados de ambiente de produção

Agora que temos o script que contém os comandos SQL para criar o banco de dados e seus dados que permanece é executar o script no banco de dados de produção. Alguns provedores de host da web oferecem uma caixa de texto no painel de controle, onde é possível digitar comandos SQL para executar no seu banco de dados. Se você tiver um arquivo de script muito grande, essa opção pode não funcionar (o `REVIEWS.MDF.sql` arquivo de script é mais 425 KB de tamanho, por exemplo).

Uma abordagem melhor é para se conectar diretamente ao servidor de banco de dados de produção usando o SQL Server Management Studio (SSMS). Se você tiver um não - Express Edition do SQL Server instalado no seu computador, em seguida, você provavelmente já tiver o SSMS instalado. Caso contrário, você pode [Baixe e instale o](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) uma cópia gratuita do SQL Server Management Studio Express Edition.

Inicie o SSMS e conecte-se ao seu servidor web host s banco de dados usando as informações fornecidas pelo seu provedor de host da web.


[![Conectar-se ao servidor Web Host provedor s banco de dados](deploying-a-database-vb/_static/image26.jpg)](deploying-a-database-vb/_static/image25.jpg) 

**Figura 9**: Conecte-se ao seu provedor de Host Web s servidor de banco de dados ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image27.jpg))


Expandir a guia de bancos de dados e localize seu banco de dados. Clique no botão de nova consulta no canto superior esquerdo da barra de ferramentas, cole os comandos SQL do arquivo de script criado pelo Assistente de publicação do banco de dados e clique no botão Executar para executar esses comandos no servidor de banco de dados de produção. Se seu arquivo de script é especialmente grande ele pode levar vários minutos para executar os comandos.


[![Conectar-se ao servidor Web Host provedor s banco de dados](deploying-a-database-vb/_static/image29.jpg)](deploying-a-database-vb/_static/image28.jpg) 

**Figura 10**: Conecte-se ao seu provedor de Host Web s servidor de banco de dados ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image30.jpg))


Tudo que s é a ele! Neste ponto, o banco de dados de desenvolvimento foi duplicado para produção. Se você atualizar o banco de dados no SSMS, você deve ver os novos objetos de banco de dados. Figura 11 mostra as tabelas de banco de dados s de produção, os procedimentos armazenados e funções definidas pelo usuário, que são iguais do banco de dados de desenvolvimento. E porque podemos instruir o Assistente de publicação de banco de dados para publicar os dados, as tabelas de s de banco de dados de produção têm os mesmos dados que as tabelas de s de banco de dados de desenvolvimento no momento em que o assistente foi executado. A Figura 12 mostra os dados no `Books` tabela no banco de dados de produção.


[![Os objetos de banco de dados foram duplicados no banco de dados de produção](deploying-a-database-vb/_static/image32.jpg)](deploying-a-database-vb/_static/image31.jpg) 

**Figura 11**: O banco de dados de objetos foram duplicados no banco de dados de produção ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image33.jpg))


[![O banco de dados de produção contém os mesmos dados que o banco de dados de desenvolvimento](deploying-a-database-vb/_static/image35.jpg)](deploying-a-database-vb/_static/image34.jpg) 

**Figura 12**: O banco de dados de produção contém os mesmos dados como no desenvolvimento de banco de dados ([clique para exibir a imagem em tamanho normal](deploying-a-database-vb/_static/image36.jpg))


Neste ponto, implantamos apenas o banco de dados de desenvolvimento para a produção. Temos ainda não examinou a implantar o aplicativo web em si ou examinado quais alterações de configuração são necessários para fazer com que o aplicativo em produção use o banco de dados de produção. Abordaremos esses problemas no próximo tutorial!

## <a name="summary"></a>Resumo

Implantar um aplicativo web controlado por dados requer a cópia de banco de dados usado durante o desenvolvimento para o ambiente de produção. Muitos provedores de host da web oferecem ferramentas para simplificar o processo de implantação de um banco de dados. Por exemplo, com DiscountASP.NET FTP seu banco de dados `.mdf` arquivo (ou um backup) e, em seguida, anexe o banco de dados para o servidor de banco de dados no painel de controle. Outra opção que funciona independentemente de quais recursos de suas ofertas do provedor de host da web é a ferramenta de Assistente de publicação de banco de dados do Microsoft s, que gera um script de comandos SQL para criar o esquema de s de banco de dados de desenvolvimento e os dados. Depois que esse script for gerado pode executá-lo no banco de dados de produção.

Agora que o banco de dados resenhas de livros da web application s está em produção podemos implantar o aplicativo. No entanto, as informações de configuração da web application s especificam a cadeia de caracteres de conexão ao banco de dados e o banco de dados de desenvolvimento faz referência a essa cadeia de caracteres de conexão. É necessário atualizar essas informações de cadeia de caracteres de conexão ao implantar o site de produção. O próximo tutorial aborda essas diferenças de configuração e explica as etapas necessárias para publicar o site de resenhas de livros controlada por dados para a produção.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Baixe o banco de dados do Microsoft SQL Server 1.1 do Assistente de publicação](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Baixe o Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Anterior](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
> [Próximo](configuring-the-production-web-application-to-use-the-production-database-vb.md)
