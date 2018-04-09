---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Implantando um banco de dados (c#) | Microsoft Docs
author: rick-anderson
description: Implantar um aplicativo web ASP.NET envolve obtendo os arquivos necessários e os recursos do ambiente de desenvolvimento para o ambiente de produção. Para da...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 203bf64da887f31e5f0727fc57173d6a573095da
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="deploying-a-database-c"></a>Implantando um banco de dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Implantar um aplicativo web ASP.NET envolve obtendo os arquivos necessários e os recursos do ambiente de desenvolvimento para o ambiente de produção. Para aplicativos web controlados por dados, isso inclui o esquema de banco de dados e os dados. Este tutorial é o primeiro em uma série que explora as etapas necessárias para implantar com êxito o banco de dados do ambiente de desenvolvimento para produção.


## <a name="introduction"></a>Introdução

Implantar um aplicativo web ASP.NET envolve obtendo os arquivos necessários e os recursos do ambiente de desenvolvimento para o ambiente de produção. Ao longo dos últimos seis tutoriais examinamos Implantando um aplicativo simples da web revisões de livros. Este site de demonstração foi composta de uma série de recursos do servidor - páginas ASP.NET, arquivos de configuração, um `Web.sitemap` arquivo e assim por diante - juntamente com recursos do lado do cliente, como imagens e arquivos CSS. Mas aplicativos orientados a dados e o web? Quais etapas adicionais devem ser executadas para implantar um aplicativo web que usa um banco de dados?

Sobre os próximos vários tutoriais falaremos sobre as etapas necessárias para implantar um aplicativo web controladas por dados. Este tutorial inicia examinando como obter um esquema de banco de dados s e o conteúdo do ambiente de desenvolvimento para o ambiente de produção, enquanto o tutorial subsequente examina as alterações de configuração necessárias. A seguir que vamos explorar os desafios de implantação de um banco de dados que usa os serviços de aplicativo (associação, funções, perfil e assim por diante).

## <a name="examining-the-updated-book-reviews-web-application"></a>Examinando o aplicativo de Web do catálogo atualizadas revisões

Para demonstrar a implantação de um aplicativo web controladas por dados, eu ve atualizado o aplicativo da web de revisões de livros de um site simple, estático para um controladas por dados. Como antes, há duas versões do aplicativo neste download tutorial s: um que usa o modelo de projeto de aplicativo Web e outro que usa o modelo de projeto de Site.

As revisões de catálogo atualizado web aplicativo usa um [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) banco de dados, que é armazenado no site de s `App_Data` pasta (`~/App_Data/Reviews.mdf`). Se você tiver instalado no computador do SQL Server 2008, em seguida, a demonstração deve ser executado sem erro. Se você tiver uma versão anterior do SQL Server, você pode instalar o SQL Server 2008 livre Express Edition, ou você pode usar o banco de dados scripts disponíveis neste tutorial s baixado para criar o banco de dados por conta própria.

O `Reviews.mdf` banco de dados contém quatro tabelas:

- `Genres` -inclui um registro para cada gênero, como a tecnologia, ficção e negócios.
- `Books` -inclui um registro para cada revisão, com colunas como `Title`, `GenreId`, `ReviewDate`, e `Review`, entre outros.
- `Authors` -inclui informações sobre cada autor que contribuiu para um livro revisado.
- `BooksAuthors` -uma tabela de junção de muitos-para-muitos que especifica que os autores de tem gravado que manuais.
  

A Figura 1 mostra um diagrama ER dessas quatro tabelas.


[![O aplicativo de Web do catálogo revisões banco de dados é composta de quatro tabelas](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Figura 1**: O aplicativo de Web do catálogo revisões banco de dados é composta de quatro tabelas ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image3.jpg))


A versão anterior do site revisões de livros tinha uma página ASP.NET separada para cada livro. Por exemplo, houve uma página chamada `~/Tech/TYASP35.aspx` que continha a revisão *ensinar por conta própria ASP.NET 3.5 nas 24 horas*. Essa nova versão controlada por dados do site tem as revisões armazenadas no banco de dados e uma única página do ASP.NET, Review.aspx?ID=*bookId*, que exibe a revisão para o catálogo especificado. Da mesma forma, há um Genre.aspx?ID=*genreId* página que lista os livros revisados no gênero especificado.

Figuras 2 e 3 mostrar o `Genre.aspx` e `Review.aspx` páginas em ação. Anote a URL na barra de endereços para cada página. In Figure 2 it s Genre.aspx?ID=85d164ba-1123-4c47-82a0-c8ec75de7e0e. Porque é 85d164ba-1123-4c47-82a0-c8ec75de7e0e a `GenreId` valor para o gênero de tecnologia, as leituras de cabeçalho de página s "Tecnologia revisa" e a lista com marcadores enumera as revisões no site que se enquadram nesse gênero.


[![A página de Gênero da tecnologia](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Figura 2**: A página de gênero de tecnologia ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image6.jpg))


[![A revisão para ensinar a você mesmo ASP.NET 3.5 em 24 horas](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Figura 3**: A revisão de *ensinar por conta própria ASP.NET 3.5 nas 24 horas* ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image9.jpg))


O aplicativo web de catálogo analisa também inclui uma seção de administração, onde os administradores podem adicionar, editar e excluir gêneros, revisões e informações do autor. Atualmente, qualquer visitante pode acessar a seção de administração. Um tutorial futuras, vamos adicionar suporte para contas de usuário e permitir que somente usuários autorizados nas páginas da administração.

Se você baixar o aplicativo de revisões de livros, tenha em mente que sua finalidade é demonstrar a implantação de um aplicativo orientado a dados. Não exibe as práticas recomendadas do ponto de vista design de aplicativo. Por exemplo, não há nenhum separada dados acesso DAL (camada); as páginas ASP.NET se comunicar diretamente com o banco de dados por meio do controle SqlDataSource ou código ADO.NET em suas classes code-behind. Para obter uma análise mais detalhada na criação de aplicativos orientados a dados usando uma arquitetura em camadas, consulte Meus [ *trabalhando com dados* tutoriais](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bancos de dados de desenvolvimento e produção

Quando você iniciar o desenvolvimento em um aplicativo web orientado a dados, você deve especificar uma cadeia de caracteres de conexão do banco de dados, que fornece os detalhes do aplicativo sobre como se conectar ao banco de dados. Essa cadeia de caracteres de conexão Especifica, entre outras coisas, o servidor de banco de dados, o nome do banco de dados e informações de segurança. Geralmente, o banco de dados usado pelo aplicativo durante o desenvolvimento é diferente do banco de dados usado quando ele s em produção. Há muitos benefícios do uso de bancos de dados diferentes para o desenvolvimento e produção. Com outro banco de dados no desenvolvimento significa você don t precisa se preocupar sobre modificação ou exclusão de dados ao vivo acidental. Ele também permite colocar nos dados fictícios de teste ou fazer alterações significativas ao modelo de dados sem precisar se preocupar sobre os efeitos sobre o aplicativo em produção. A desvantagem de ter um banco de dados diferente nos ambientes de desenvolvimento e produção é que quando o aplicativo é implantado o banco de dados e as alterações relevantes para o esquema de banco de dados s ou dados também devem ser implantados.

Antes da primeira implantação, há apenas uma instância do banco de dados, e essa instância é no ambiente de desenvolvimento. Ao implantar o aplicativo à produção pela primeira vez, deve não apenas copiar se os arquivos necessários do servidor e do lado do cliente, mas também copiar o banco de dados do ambiente de desenvolvimento para o ambiente de produção. Isso é onde podemos está à direita agora com o aplicativo web de revisões de livros - o banco de dados reside o `App_Data` pasta em nosso ambiente de desenvolvimento, mas tem não ainda foi passado para o ambiente de produção.

Depois que o aplicativo foi implantado, há duas cópias do banco de dados. Como o aplicativo amadurece, novos recursos podem ser adicionados, a necessidade de uma alteração para o modelo de dados (como adicionar novas colunas a tabelas existentes, fazer alterações para colunas existentes, adicionar novas tabelas e assim por diante). Quando o aplicativo web é implantado em seguida, as alterações aplicadas ao banco de dados no ambiente de desenvolvimento desde a última implantação deve ser aplicada ao banco de dados de produção. Algumas estratégias para gerenciar esse processo são discutidas em um tutorial futuras. Este tutorial se concentra na implantação de banco de dados inteiro do ambiente de desenvolvimento para produção.

## <a name="deploying-the-database-to-the-production-environment"></a>Implantando o banco de dados no ambiente de produção

O restante deste tutorial explica como implantar o banco de dados do ambiente de desenvolvimento para o ambiente de produção. Se você estiver acompanhando você precisa para certificar-se de que sua conta com seu provedor de host da web inclui o Microsoft SQL Server banco de dados de apoio. Você também precisará ter algumas informações à mão, ou seja, o nome do servidor de banco de dados, o nome do banco de dados e o nome de usuário e a senha usada para se conectar ao banco de dados.

Conforme observado anteriormente neste tutorial, o banco de dados do site s revisões de livros é um banco de dados do SQL Server 2008 Express Edition armazenado a `App_Data` pasta. Ele costumam motivo que a implantação de um banco de dados seria tão simple quanto copiar a `App_Data` pasta do ambiente de desenvolvimento para o ambiente de produção. No entanto, a maioria dos provedores de host da web não dão suporte a bancos de dados de hospedagem no `App_Data` pasta por motivos de segurança. Em vez disso, os hosts da web fornecem uma conta em um servidor de banco de dados do SQL Server no ambiente. Implantando o banco de dados do seu ambiente de desenvolvimento para o ambiente de produção requer a obtenção de seu banco de dados registrado no servidor web host s banco de dados.

Como obter o banco de dados do ambiente de desenvolvimento para o ambiente de produção? Há duas maneiras de fazer isso, dependendo de quais serviços as ofertas de host da web. Com alguns dos hosts, como DiscountASP.NET, você pode FTP um backup de banco de dados ou real `.mdf` o arquivo para seu site e, em seguida, no painel de controle, restaure o arquivo de backup ou anexar o `.mdf` arquivo para o servidor de banco de dados do SQL Server. Com essas ferramentas implantar o banco de dados é tão simple quanto copiar a `App_Data` pasta para o ambiente de produção e, em seguida, anexá-lo por meio do painel de controle. Essa é talvez a maneira mais fácil e rápida de publicar o banco de dados pela primeira vez.

Outra abordagem é usar o Assistente de publicação de banco de dados. O Assistente de publicação de banco de dados é um aplicativo de área de trabalho do Windows que irá gerar os comandos SQL para criar seu esquema s do banco de dados de tabelas, procedimentos armazenados, exibições, funções definidas pelo usuário e assim por diante - e, opcionalmente, os dados em suas tabelas. Você pode conectar-se ao servidor web host provedor s banco de dados por meio do SQL Server Management Studio e, em seguida, executar este script para duplicar o banco de dados de produção. Ainda melhor, se seu provedor de host da web oferece suporte ao Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) que o script gerado pelo Assistente de publicação de banco de dados executadas automaticamente no servidor de banco de dados em seu nome. Como o Assistente de publicação de banco de dados gera um script que cria o esquema de banco de dados s e os dados, ele funcionará independentemente se o seu provedor de host da web oferece recursos como anexar um carregado `.mdf` arquivo.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Gerar os comandos SQL para criar o esquema de banco de dados e os dados usando o Assistente de publicação de banco de dados

Permitir que o s percorrer usando o Assistente de publicação de banco de dados para implantar o banco de dados de catálogo revisões em produção. Se você estiver usando o Visual Studio 2008 ou posterior, o Assistente de publicação de banco de dados já está instalado. Se você estiver usando o Visual Studio 2005, você precisará primeiro [baixar e instalar](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) o assistente.

Abra o Visual Studio e navegue até o `Reviews.mdf` banco de dados. Se você estiver usando o Visual Web Developer, vá para o Gerenciador de banco de dados; Se você estiver usando o Visual Studio, use o Gerenciador de servidores. A Figura 4 mostra o `Reviews.mdf` banco de dados no Gerenciador de banco de dados no Visual Web Developer. Como mostra a Figura 4, o `Reviews.mdf` banco de dados é composto de quatro tabelas, três procedimentos armazenados e uma função definida pelo usuário.


[![Localize o banco de dados no Pesquisador de objetos de banco de dados ou no Gerenciador de servidores](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Figura 4**: Localize o banco de dados no Pesquisador de objetos de banco de dados ou no Gerenciador de servidores ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image12.jpg))


Clique com botão direito no nome do banco de dados e escolha a opção "Publicar no provedor" no menu de contexto. Isso inicia o Assistente de publicação de banco de dados (consulte a Figura 5). Clique em Avançar após a tela inicial.


[![O tela inicial do Assistente de publicação de banco de dados](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Figura 5**: O banco de dados de publicação inicial tela do assistente ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image15.jpg))


A segunda tela do assistente lista os bancos de dados acessíveis para o Assistente de publicação de banco de dados e permite que você escolha se deseja gerar script de todos os objetos no banco de dados selecionado ou para escolher quais objetos de script. Selecione o banco de dados apropriado e deixe a opção "Script todos os objetos no banco de dados selecionado" marcada.

> [!NOTE]
> Se você receber o erro "não existem objetos no banco de dados *databaseName* dos tipos programáveis por este assistente" ao clicar em Avançar na tela mostrada na Figura 6, certifique-se de que o caminho para o arquivo de banco de dados não é excessivamente longo. Foi descoberto que esse erro pode ocorrer se o caminho para o arquivo de banco de dados é muito longo.


[![O tela inicial do Assistente de publicação de banco de dados](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Figura 6**: O banco de dados de publicação inicial tela do assistente ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image18.jpg))


Na próxima tela você pode gerar um arquivo de script ou, se o host da web dá suporte a ele, publicar o banco de dados diretamente em seu servidor de banco de dados web host provedor s. Como mostra a Figura 7, tenho o script gravado no arquivo `C:\REVIEWS.MDF.sql`.


[![O banco de dados para um arquivo de script ou publicá-lo diretamente no seu provedor de Host da Web](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Figura 7**: o banco de dados para um arquivo de Script ou publicá-lo diretamente no seu provedor de Host da Web ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image21.jpg))


A tela subsequente solicitará uma variedade de opções de script. Você pode especificar se o script deve incluir instruções drop para remover esses objetos existentes. O padrão é True, que serve ao implantar um banco de dados pela primeira vez. Você também pode especificar se o banco de dados de destino é SQL Server 2000, SQL Server 2005 ou SQL Server 2008. Por fim, você pode indicar se o script do esquema e dados, apenas os dados ou apenas o esquema. O esquema é a coleção de objetos de banco de dados, tabelas, procedimentos armazenados, exibições e assim por diante. Os dados são as informações que residem nas tabelas.

Conforme ilustrado na Figura 8, temos o assistente configurado para remover objetos de banco de dados existentes, para gerar script para um banco de dados do SQL Server 2008 e publicar o esquema e os dados.


[![Especifique a publicação de opções](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Figura 8**: especificar as opções de publicação ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image24.jpg))


As finais duas telas resumem as ações que estão prestes a serem tomadas e, em seguida, exibir o status do script. O resultado da execução do assistente é que há um arquivo de script que contém os comandos SQL necessários para criar o banco de dados em produção e preenchê-la com os mesmos dados em desenvolvimento.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Executar os comandos SQL do banco de dados do ambiente de produção

Agora que temos o script que contém os comandos SQL para criar o banco de dados e seus dados que permanece é executar o script no banco de dados de produção. Alguns provedores de host web oferecem uma caixa de texto no painel de controle, onde você pode inserir comandos SQL para executar no banco de dados. Se você tiver um arquivo de script muito grande, essa opção pode não funcionar (o `REVIEWS.MDF.sql` arquivo de script é mais 425 KB de tamanho, por exemplo).

Uma abordagem melhor é se conectar diretamente ao servidor de banco de dados de produção usando o SQL Server Management Studio (SSMS). Se você tiver uma não - Express Edition do SQL Server instalada no computador, em seguida, você provavelmente já terão SSMS instalado. Caso contrário, você pode [baixar e instalar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) uma cópia gratuita do SQL Server Management Studio Express Edition.

Inicie o SSMS e conecte-se ao seu servidor de banco de dados da web host s usando as informações fornecidas por seu provedor de host da web.


[![Conecte-se ao seu servidor de banco de dados Web Host provedor s](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Figura 9**: conectar-se ao seu provedor de Host Web s servidor de banco de dados ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image27.jpg))


Expanda a guia de bancos de dados e localize o banco de dados. Clique no botão Nova consulta no canto superior esquerdo da barra de ferramentas, cole os comandos SQL do arquivo de script criado pelo Assistente de publicação de banco de dados e clique no botão Executar para executar esses comandos no servidor de banco de dados de produção. Se o arquivo de script é especialmente grande levará vários minutos para executar os comandos.


[![Conecte-se ao seu servidor de banco de dados Web Host provedor s](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Figura 10**: conectar-se ao seu provedor de Host Web s servidor de banco de dados ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image30.jpg))


Tudo lá que s é a ele! Neste ponto, o banco de dados de desenvolvimento foi duplicado para produção. Se você atualizar o banco de dados no SSMS, você verá os novos objetos de banco de dados. Figura 11 mostra as tabelas de banco de dados s de produção, os procedimentos armazenados e funções definidas pelo usuário, que são iguais do banco de dados de desenvolvimento. E como podemos instruir o Assistente de publicação de banco de dados para publicar os dados, as tabelas de s do banco de dados de produção têm os mesmos dados que as tabelas de s do banco de dados de desenvolvimento no momento em que o assistente foi executado. A Figura 12 mostra os dados de `Books` tabela no banco de dados de produção.


[![Os objetos de banco de dados foram duplicados no banco de dados de produção](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Figura 11**: O banco de dados objetos foram duplicados no banco de dados de produção ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image33.jpg))


[![O banco de dados de produção contém os mesmos dados que o banco de dados de desenvolvimento](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Figura 12**: O banco de dados de produção contém os mesmos dados no banco de dados de desenvolvimento ([clique para exibir a imagem em tamanho normal](deploying-a-database-cs/_static/image36.jpg))


Neste ponto, apenas ter implantado o banco de dados de desenvolvimento para produção. Ainda não é visto implantar o aplicativo web em si ou examinou as alterações de configuração são necessárias para o aplicativo na produção usam o banco de dados de produção. Abordaremos esses problemas no tutorial próximo!

## <a name="summary"></a>Resumo

Implantar um aplicativo web controladas por dados requer a cópia do banco de dados usado durante o desenvolvimento para o ambiente de produção. Muitos provedores de host web oferecem ferramentas para simplificar o processo de implantação de um banco de dados. Por exemplo, com DiscountASP.NET FTP seu banco de dados `.mdf` arquivo (ou um backup) e, em seguida, anexar o banco de dados para o servidor de banco de dados no painel de controle. Outra opção que funciona independentemente de quais recursos suas ofertas de provedor de host da web é a ferramenta de Assistente de publicação de banco de dados do Microsoft s, que gera um script de comandos SQL para criar o esquema de s do banco de dados de desenvolvimento e os dados. Depois que esse script foi gerado pode executá-lo no banco de dados de produção.

Agora que o banco de dados do catálogo revisões web aplicativo s está em produção, pode implantar o aplicativo. No entanto, as informações de configuração da web aplicativo s especificam a cadeia de caracteres de conexão para o banco de dados, e essa cadeia de caracteres de conexão referencia o banco de dados de desenvolvimento. É preciso atualizar essas informações de cadeia de caracteres de conexão ao implantar o site de produção. O seguinte tutorial examina essas diferenças de configuração e explica as etapas necessárias para publicar o site de revisões de livros controlada por dados em produção.

Boa programação!

#### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Baixar o banco de dados do Microsoft SQL Server 1.1 do Assistente de publicação](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Baixar o Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Anterior](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [Próximo](configuring-the-production-web-application-to-use-the-production-database-cs.md)
