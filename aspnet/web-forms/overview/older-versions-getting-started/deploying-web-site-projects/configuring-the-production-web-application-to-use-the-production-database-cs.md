---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Configurando o aplicativo Web de produção para usar o banco de dados de produção (c#) | Microsoft Docs
author: rick-anderson
description: Conforme discutido nos tutoriais anteriores, não é incomum para obter informações de configuração são diferentes entre os ambientes de desenvolvimento e produção. Isso é es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: b6899c199ea593e74d4d423cfc3f832e38bd7e57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374944"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Configurando o aplicativo Web de produção para usar o banco de dados de produção (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Conforme discutido nos tutoriais anteriores, não é incomum para obter informações de configuração são diferentes entre os ambientes de desenvolvimento e produção. Isso é especialmente verdadeiro para aplicativos web controlados por dados, como as cadeias de caracteres de conexão de banco de dados diferem entre os ambientes de desenvolvimento e produção. Este tutorial explora maneiras para configurar o ambiente de produção para incluir a cadeia de caracteres de conexão apropriado em mais detalhes.


## <a name="introduction"></a>Introdução

Aplicativos web controlados por dados normalmente usam um banco de dados diferente em desenvolvimento que se na produção. Para aplicativos hospedados por um provedor de host da web e desenvolveu localmente, o banco de dados de desenvolvimento será geralmente reside no computador s desenvolvedor enquanto o banco de dados de produção está hospedado em um servidor de banco de dados na instalação de s da empresa de hospedagem web. Implantar um aplicativo web controlado por dados envolve a cópia do banco de dados de desenvolvimento para o servidor de banco de dados de produção. No tutorial anterior analisamos maneiras de realizar esta etapa.

O aplicativo web usa as informações em um *cadeia de caracteres de conexão* para estabelecer uma conexão com o banco de dados. A cadeia de conexão, que normalmente é armazenada em `Web.config`, especifica o nome do servidor de banco de dados, o nome do banco de dados, o contexto de segurança e outras informações. Como o banco de dados usado pelo aplicativo web depende se o aplicativo web está em execução em ambientes de desenvolvimento ou produção, as cadeias de caracteres de conexão devem ser diferentes entre os dois ambientes.

Não é incomum para obter informações de configuração são diferentes entre os ambientes de desenvolvimento e produção. O *diferenças de configuração comuns entre desenvolvimento e produção* tutorial discute técnicas para manter informações de configuração separado entre esses dois ambientes, bem como uma breve discussão sobre cadeias de caracteres de conexão de banco de dados. Este tutorial explora maneiras para configurar o ambiente de produção para incluir a cadeia de caracteres de conexão apropriado em mais detalhes.

## <a name="examining-the-connection-string-information"></a>Examinando as informações de cadeia de caracteres de Conexão

A cadeia de conexão usada pelo aplicativo web resenhas de livros é armazenada no arquivo de configuração do aplicativo s, `Web.config`. `Web.config` inclui uma seção especial para armazenar cadeias de caracteres de conexão, apropriadamente denominadas [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). O `Web.config` arquivo para o site de resenhas de livros possui uma cadeia de caracteres de conexão definida nesta seção denominada `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

A cadeia de caracteres de conexão - fonte de dados =. \SQLEXPRESS; AttachDbFilename = | Segurança DataDirectory|\Reviews.mdf;Integrated = True; Instância de usuário = True - é composto de uma série de opções e valores, com pares de opção/valor delimitados por ponto e vírgula e cada opção e valor delimitado por um sinal de igual. As quatro opções usadas nesta cadeia de caracteres de conexão são:

- `Data Source` -Especifica o local do servidor de banco de dados e o nome de instância do servidor de banco de dados (se houver). O valor `.\SQLEXPRESS`, é um exemplo onde há um servidor de banco de dados e um nome de instância. O período de Especifica que o servidor de banco de dados está no mesmo computador que o aplicativo; o nome da instância é `SQLEXPRESS`.
- `AttachDbFilename` -Especifica o local do arquivo de banco de dados. O valor contém o espaço reservado `|DataDirectory|`, que é resolvido para o caminho completo da s aplicativo `App_Data` pasta em tempo de execução.
- `Integrated Security` -um valor booliano que indica se deve usar uma nome de usuário e senha especificada ao se conectar ao banco de dados (false) ou o Windows atual, as credenciais da conta (true).
- `User Instance` -uma opção de configuração específica para as edições SQL Server Express que indica se deve permitir que usuários não administrativos no computador local, anexar e se conectar a um banco de dados do SQL Server Express Edition. Ver [instâncias do SQL Server Express usuário](https://msdn.microsoft.com/library/ms254504.aspx) para obter mais informações sobre essa configuração.
  

As opções de cadeia de caracteres de conexão permitidos dependem do banco de dados que você está se conectando e o provedor de banco de dados do ADO.NET que está sendo usado. Por exemplo, a cadeia de conexão para se conectar a um Microsoft SQL Server difere do banco de dados que é usado para se conectar ao banco de dados Oracle. Da mesma forma, se conectar a um banco de dados do Microsoft SQL Server usando o provedor SqlClient usa uma cadeia de caracteres de conexão diferente que ao usar o provedor OLE DB.

Você pode criar a cadeia de caracteres de conexão de banco de dados manualmente usando um site, como [ConnectionStrings.com](http://www.connectionstrings.com/) como um recurso para quais opções estão disponíveis. No entanto, uma abordagem mais fácil é adicionar o banco de dados para o Gerenciador de servidores no Visual Studio e, em seguida, pegue a cadeia de caracteres de conexão na janela Propriedades. Permitir que o s usem esta última técnica para construir a cadeia de caracteres de conexão para o servidor de banco de dados de produção.

Abra o Visual Studio e, em seguida, navegue até a janela do Gerenciador de servidores (no Visual Web Developer, essa janela é chamada Gerenciador de banco de dados). Clique na opção conexões de dados e escolha a opção de Adicionar Conexão no menu de contexto. Isso abre o assistente mostrado na Figura 1. Escolha a fonte de dados apropriada e clique em continuar.


[![Optar por adicionar um novo banco de dados para o Gerenciador de servidores](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Figura 1**: escolha Adicionar um novo banco de dados para o Gerenciador de servidores ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


Em seguida, especifique as várias informações de conexão de banco de dados (veja a Figura 2). Quando você se inscreveu com sua empresa de hospedagem de web que eles devem ter fornecido informações sobre como se conectar ao banco de dados - o nome do servidor de banco de dados, o nome do banco de dados, o nome de usuário e senha a ser usada para se conectar ao banco de dados e assim por diante. Depois de inserir essas informações, clique em Okey para concluir este assistente e adicionar o banco de dados para o Gerenciador de servidores.


[![Especifique as informações de Conexão de banco de dados](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Figura 2**: especifique as informações de Conexão de banco de dados ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


O banco de dados do ambiente de produção deve agora estar listado no Gerenciador de servidores. Selecione o banco de dados do Gerenciador de servidores e vá para a janela Propriedades. Lá você encontrará uma propriedade de cadeia de caracteres de Conexão com a cadeia de caracteres de conexão do banco de dados s nomeada. Supondo que você estiver usando um banco de dados do Microsoft SQL Server em produção e o provedor do SqlClient sua cadeia de conexão deve ser semelhante ao seguinte:

<strong>Fonte de dados =<em>serverName</em>; Catálogo inicial =<em>databaseName</em>; Atributo Persist Security Info = True; ID de usuário =<em>nome de usuário</em>; Senha =*senha</strong>*

Em que *nome_do_servidor*, *databaseName*, *username*, e *senha* estão com os valores para o nome de servidor de banco de dados, o banco de dados nome, o nome de usuário e a senha fornecidos a você por sua empresa de host da web.

## <a name="deploying-the-book-reviews-web-application"></a>Implantar o aplicativo de Web do catálogo revisões

O tutorial anterior percorreu copiando o banco de dados de desenvolvimento para o ambiente de produção, mas não explorar a implantação do aplicativo controlados por dados. Neste ponto o ambiente de produção contém o banco de dados, mas está usando a versão do aplicativo de livro examina com revisões estáticas. É necessário implantar o aplicativo novo e controlado por dados para o servidor de produção, juntamente com as informações de configuração atualizada.

Reserve um tempo para implantar o aplicativo orientado a dados do ambiente de desenvolvimento para a produção. Esse processo foi abordado em detalhes nos tutoriais anteriores. Se você precisar de um lembrete, consulte o *Implantando o site usando um cliente FTP* ou o *implantando seu site usando o Visual Studio* tutoriais. Você precisará garantir que a cadeia de caracteres de conexão de banco de dados de produção é aquele usado no ambiente de produção, o que significa que uma alternativa `Web.config` arquivo deve ser implantado. Especificamente, isso modificado `Web.config` arquivo s `<connectionStrings>` elemento deve conter a cadeia de caracteres de conexão de banco de dados de produção e deve ser semelhante ao seguinte:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Observe que a conexão de cadeia de caracteres na `<connectionStrings>` elemento tiver o mesmo nome (`ReviewsConnectionString`), mas agora contém a cadeia de caracteres de conexão de banco de dados de produção em vez da cadeia de conexão de banco de dados de desenvolvimento.

A menos que você tenha um fluxo de trabalho de implantação mais formal, qualquer um modificar manualmente o `Web.config` arquivo para usar a cadeia de caracteres de conexão de banco de dados de produção antes de implantar (Lembre-se de revertê-lo voltar a usar a cadeia de caracteres de conexão de banco de dados de desenvolvimento Depois disso) ou manter um separado `Web.config` arquivo com as informações de configuração do ambiente de produção que são carregados no ambiente de produção como parte do processo de implantação.

> [!NOTE]
> Se você implantar acidentalmente um `Web.config` arquivo que contém a cadeia de caracteres de conexão de banco de dados de desenvolvimento, haverá um erro quando o aplicativo em produção tenta se conectar ao banco de dados. Esse erro se manifesta como um `SqlException` com uma mensagem que informa que o servidor não foi encontrado ou não estava acessível.


Depois que o site foi implantado para produção, visite o site de produção por meio de seu navegador. Você deve ver e aproveite a mesma experiência de usuário que, ao executar o aplicativo controlado por dados localmente. Claro quando você visitar o site de produção o site é alimentado pelo servidor de banco de dados de produção, enquanto que visitar o site no ambiente de desenvolvimento usa o banco de dados no desenvolvimento. A Figura 3 mostra a *ensinar por conta própria ASP.NET 3.5 in 24 horas* examine a página do site no ambiente de produção (Observe a URL na barra de endereços do navegador s).


[![O aplicativo controlado por dados está agora disponível em produção!](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Figura 3**: The Data-Driven aplicativo está agora disponível em produção! ([Clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Armazenando cadeias de caracteres de Conexão em um arquivo de configuração separado

Uma técnica comum para manter informações de configuração separado em ambientes de desenvolvimento e produção é ter duas versões do `Web.config`: uma para o ambiente de desenvolvimento e outra para produção. No momento da implantação apropriado `Web.config` versão pode ser copiado para o ambiente de produção. Idealmente, esse processo seria automatizado como parte do fluxo de trabalho de implantação.

Em vez de manter dois separado `Web.config` arquivos que você pode, opcionalmente, fornecem as diferenças mais granulares. Os elementos que compõem o `Web.config` arquivo pode ser definido em um arquivos de configuração externos que são referenciados no `Web.config` arquivo. Em poucas palavras pode ter um `Web.config` do arquivo para ambos os ambientes que faz referência a um arquivo de databaseConnectionStrings.config, que contém as cadeias de caracteres de conexão usada pelo aplicativo e devem ser exclusivos para cada ambiente. Acho que separar as informações de configuração diferentes em arquivos separados fornece uma mais organizada e mais simples `Web.config` arquivo e muito mais claramente destaca as diferenças de configuração entre ambientes de desenvolvimento e produção.

Para usar essa técnica, comece criando uma nova pasta no aplicativo da web denominado `ConfigSections`. Em seguida, adicione dois arquivos para essa nova pasta chamada databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config. Em seguida, copie o `<connectionStrings>` elemento `Web.config` em arquivos databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config e, em seguida, modifique a cadeia de conexão no databaseConnectionStrings.production.config arquivo para que ele especifica a cadeia de caracteres de conexão de banco de dados de produção. Por exemplo, o arquivo de databaseConnectionStrings.dev.config deve conter apenas o `<connectionStrings>` elemento com uma cadeia de caracteres de conexão que referencia o banco de dados de desenvolvimento:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Da mesma forma, o arquivo de databaseConnectionStrings.production.config deve conter apenas um `<connectionStrings>` elemento, mas que tem a cadeia de caracteres de conexão de banco de dados de produção.

Faça uma cópia do arquivo databaseConnectionStrings.dev.config e nomeie-a como databaseConnectionStrings.config.

> [!NOTE]
> Você pode nomear o arquivo de configuração algo diferente de databaseConnectionStrings.config, se você d como, por exemplo, `connectionStrings.config` ou `dbInfo.config`. No entanto, certifique-se de nomear o arquivo com um `.config` extensão como `.config` arquivos, por padrão, não são realizados pelo mecanismo ASP.NET. Se você nomear o arquivo outra coisa, como `connectionStrings.txt`, um usuário poderia apontar seu navegador para [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) e exibir o conteúdo do arquivo!


Neste ponto o `ConfigSections` pasta deve conter três arquivos (veja a Figura 4). Os arquivos databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config contêm as cadeias de caracteres de conexão para os ambientes de desenvolvimento e produção, respectivamente. O arquivo databaseConnectionStrings.config contém as informações de cadeia de caracteres de conexão que serão usadas pelo aplicativo web em tempo de execução. Consequentemente, o arquivo databaseConnectionStrings.config deve ser idêntico ao arquivo databaseConnectionStrings.dev.config no ambiente de desenvolvimento, enquanto em produção, o arquivo databaseConnectionStrings.config deve ser idêntico ao databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Figura 4**: ConfigSections ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


Agora, precisamos instruir `Web.config` para usar o arquivo de databaseConnectionStrings.config para seu armazenamento de cadeia de caracteres de conexão. Abra `Web.config` e substitua o elemento `<connectionStrings>` existente pelo seguinte:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

O `configSource` atributo especifica um caminho físico relativo para o `Web.config` arquivo. Se o externo `.config` arquivo está no mesmo diretório que `Web.config` , em seguida, defina esse atributo como o nome de arquivo a `.config` arquivo. Se ele s em um subdiretório, como é o caso com databaseConnectionStrings.config, especifique a subpasta usando uma barra invertida para delimitar os nomes de pasta e arquivo, como ConfigSections\databaseConnectionStrings.config.

Com essa modificação, ambientes de desenvolvimento e produção contêm os mesmos `Web.config` arquivo. Agora, a única diferença é o arquivo databaseConnectionStrings.config. Copie o arquivo de databaseConnectionStrings.production.config para produção e renomeie-o para databaseConnectionStrings.config. Se, no futuro, há alterações para a cadeia de caracteres de conexão de banco de dados de produção você precisará torná-los para o arquivo databaseConnectionStrings.production.config e, em seguida, carregar esse arquivo para a produção, renomeá-lo databaseConnectionStrings.config.

> [!NOTE]
> Você pode especificar as informações para qualquer `Web.config` elemento em um arquivo separado e usar o `configSource` atributo para fazer referência a esse arquivo no `Web.config`.


## <a name="summary"></a>Resumo

Aplicativos controlados por dados normalmente usam bancos de dados diferentes nos ambientes de desenvolvimento e produção. Consequentemente, as cadeias de caracteres de conexão de banco de dados armazenadas na configuração de s do aplicativo web devem ser exclusivas para cada ambiente. Neste tutorial vimos como determinar a cadeia de conexão de banco de dados de produção e maneiras de manter informações de cadeia de caracteres de conexão exclusivo nos dois ambientes.

Boa programação!

#### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Cadeias de conexão e arquivos de configuração](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informações @ ConnectionStrings.com de cadeias de caracteres de configuração de banco de dados](http://www.connectionstrings.com/)
- [Mover as configurações no arquivo Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentação técnica para o &lt;connectionStrings&gt; elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-a-database-cs.md)
> [Próximo](configuring-a-website-that-uses-application-services-cs.md)
