---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: "Configurando o aplicativo da Web de produção para usar o banco de dados de produção (VB) | Microsoft Docs"
author: rick-anderson
description: "Como discutido nos tutoriais anteriores, não é incomum para obter informações de configuração será diferente entre os ambientes de desenvolvimento e produção. Isso é es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b193fa3256e5886481c7b36d88aa09c1fa7017c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>Configurando o aplicativo da Web de produção para usar o banco de dados de produção (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> Como discutido nos tutoriais anteriores, não é incomum para obter informações de configuração será diferente entre os ambientes de desenvolvimento e produção. Isso é especialmente verdadeiro para aplicativos web controlados por dados, como as cadeias de caracteres de conexão do banco de dados diferem entre os ambientes de desenvolvimento e produção. Este tutorial explora maneiras de configurar o ambiente de produção para incluir a cadeia de caracteres de conexão apropriado em mais detalhes.


## <a name="introduction"></a>Introdução

Aplicativos web orientado a dados geralmente usam um banco de dados diferente em desenvolvimento quando em produção. Para aplicativos hospedados por um provedor de host da web e desenvolvido localmente, o banco de dados de desenvolvimento que geralmente reside no computador desenvolvedor s enquanto o banco de dados de produção é hospedado em um servidor de banco de dados em que o recurso de s da empresa de hospedagem na web. Implantar um aplicativo web controladas por dados envolve copiar o banco de dados de desenvolvimento para o servidor de banco de dados de produção. No tutorial anterior analisamos maneiras de realizar esta etapa.

O aplicativo web usa as informações em um *cadeia de caracteres de conexão* para estabelecer uma conexão com o banco de dados. A cadeia de caracteres de conexão, que normalmente é armazenada em `Web.config`, especifica o nome do servidor de banco de dados, o nome do banco de dados, o contexto de segurança e outras informações. Como o banco de dados usado pelo aplicativo web depende se o aplicativo web está em execução em ambientes de desenvolvimento ou de produção, as cadeias de caracteres de conexão devem diferir entre os dois ambientes.

Não é incomum para obter informações de configuração será diferente entre os ambientes de desenvolvimento e produção. O *diferenças de configuração comuns entre o desenvolvimento e produção* tutorial discutiu técnicas para manter informações de configuração separados entre esses dois ambientes, bem como uma breve discussão sobre cadeias de conexão de banco de dados. Este tutorial explora maneiras de configurar o ambiente de produção para incluir a cadeia de caracteres de conexão apropriado em mais detalhes.

## <a name="examining-the-connection-string-information"></a>Examinar as informações de cadeia de caracteres de Conexão

A cadeia de conexão usada pelo aplicativo web do catálogo revisões é armazenada no arquivo de configuração do aplicativo s, `Web.config`. `Web.config`inclui uma seção especial para armazenar cadeias de caracteres de conexão, adequadamente chamadas [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/en-us/library/bf7sd233.aspx). O `Web.config` arquivo para o site do catálogo revisões possui uma cadeia de caracteres de conexão definida nesta seção denominada `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

A cadeia de caracteres de conexão - fonte de dados =. \SQLEXPRESS; AttachDbFilename = | Segurança DataDirectory|\Reviews.mdf;Integrated = True; User Instance = True - é composto de um número de opções e os valores com pares de valor da opção/delimitadas por ponto e vírgula e cada opção e valor delimitadas por um sinal de igual. As quatro opções usadas nesta cadeia de caracteres de conexão são:

- `Data Source`-Especifica o local do servidor de banco de dados e o nome de instância do servidor de banco de dados (se houver). O valor `.\SQLEXPRESS`, é um exemplo onde há um servidor de banco de dados e um nome de instância. O período de Especifica que o servidor de banco de dados está no mesmo computador que o aplicativo. o nome da instância é `SQLEXPRESS`.
- `AttachDbFilename`-Especifica o local do arquivo de banco de dados. O valor contém o espaço reservado `|DataDirectory|`, que é resolvido para o caminho completo da s aplicativo `App_Data` pasta em tempo de execução.
- `Integrated Security`-um valor booliano que indica se deve usar uma nome de usuário/senha especificada durante a conexão com o banco de dados (false) ou o Windows atual credenciais da conta (true).
- `User Instance`-uma opção de configuração específica para o SQL Server Express Edition que indica se deve permitir que usuários não administrativos no computador local anexam e se conectar a um banco de dados do SQL Server Express Edition. Consulte [instâncias do SQL Server Express usuário](https://msdn.microsoft.com/en-us/library/ms254504.aspx) para obter mais informações sobre essa configuração.
  

As opções de cadeia de caracteres de conexão permitidos dependem do banco de dados que você está se conectando e [ADO.NET](http://ADO.NET) provedor de banco de dados que está sendo usado. Por exemplo, a cadeia de conexão para se conectar a um Microsoft SQL Server difere do banco de dados que é usado para se conectar ao banco de dados Oracle. Da mesma forma, se conectar a um banco de dados do Microsoft SQL Server usando o provedor SqlClient usa uma cadeia de caracteres de conexão diferentes que ao usar o provedor OLE DB.

Você pode criar a cadeia de caracteres de conexão do banco de dados manualmente usando um site como [ConnectionStrings.com](http://www.connectionstrings.com/) como um recurso de quais opções estão disponíveis. No entanto, uma abordagem mais fácil é adicionar o banco de dados para o Gerenciador de servidores no Visual Studio e, em seguida, para obter a cadeia de caracteres de conexão da janela de propriedades. Permitir que o s use esta última técnica para construir a cadeia de caracteres de conexão para o servidor de banco de dados de produção.

Abra o Visual Studio e, em seguida, navegue para a janela do Gerenciador de servidores (no Visual Web Developer, esta janela é chamada Gerenciador de banco de dados). Clique na opção de conexões de dados e escolha a opção de Adicionar Conexão no menu de contexto. Isso abre o assistente mostrado na Figura 1. Escolha a fonte de dados apropriada e clique em continuar.


[![Adicionar um novo banco de dados para o Gerenciador de servidores](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**Figura 1**: adicionar um novo banco de dados para o Gerenciador de servidores ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))


Em seguida, especifique as várias informações de conexão de banco de dados (consulte a Figura 2). Quando você se inscreveu com seu web de hospedagem da empresa deve forneceu informações sobre como conectar-se ao banco de dados - o nome do servidor de banco de dados, o nome do banco de dados, o nome de usuário e senha a ser usada para se conectar ao banco de dados e assim por diante. Depois de inserir essas informações, clique em Okey para concluir este assistente e adicionar o banco de dados para o Gerenciador de servidores.


[![Especifique as informações de Conexão de banco de dados](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**Figura 2**: especifique as informações de Conexão de banco de dados ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))


O banco de dados do ambiente de produção deve agora estar listado no Gerenciador de servidores. Selecione o banco de dados do Gerenciador de servidores e vá para a janela de propriedades. Lá você encontrará uma propriedade denominada cadeia de caracteres de Conexão com a cadeia de caracteres de conexão do banco de dados s. Supondo que você estiver usando um banco de dados do Microsoft SQL Server em produção e o provedor SqlClient sua cadeia de caracteres de conexão deve ser semelhante ao seguinte:

**Fonte de dados =*serverName*; Catálogo inicial =*databaseName*; Atributo Persist Security Info = True; ID de usuário =*username*; Senha =*senha***

Onde *serverName*, *databaseName*, *username*, e *senha* com os valores para o nome de servidor de banco de dados, o banco de dados nome, o nome de usuário e a senha fornecida a você pela sua empresa de host da web.

## <a name="deploying-the-book-reviews-web-application"></a>Implantar o aplicativo de Web de revisões do catálogo

O tutorial anterior percorridas copiando o banco de dados de desenvolvimento para o ambiente de produção, mas não explorar a implantar o aplicativo orientado a dados. Neste ponto o ambiente de produção contém o banco de dados, mas está usando a versão do aplicativo livro analisa com análises estáticas. É necessário implantar o aplicativo de novo, controlada por dados para o servidor de produção junto com as informações de configuração atualizada.

Dedique alguns momentos para implantar o aplicativo orientado a dados do ambiente de desenvolvimento para produção. Esse processo foi abordado em detalhes em tutoriais anteriores. Se você precisar de uma atualização, consulte o *implantando seu site usando um cliente FTP* ou *implantando seu site usando o Visual Studio* tutoriais. Você precisará garantir que a cadeia de caracteres de conexão de banco de dados de produção é usado no ambiente de produção, o que significa que uma alternativa `Web.config` arquivo deve ser implantado. Especificamente, isso modificado `Web.config` arquivo s `<connectionStrings>` elemento deve conter a cadeia de caracteres de conexão de banco de dados de produção e deve ser semelhante à seguinte:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

Observe que a conexão de cadeia de caracteres no `<connectionStrings>` elemento é o mesmo nome (`ReviewsConnectionString`), mas agora contém a cadeia de caracteres de conexão de banco de dados de produção em vez da cadeia de caracteres de conexão de banco de dados de desenvolvimento.

A menos que você tenha um fluxo de trabalho de implantação mais formal, manualmente modifique o `Web.config` arquivo para usar a cadeia de caracteres de conexão de banco de dados de produção antes da implantação (Lembre-se de revertê-lo voltar a usar a cadeia de caracteres de conexão de banco de dados de desenvolvimento Depois disso) ou manter um separado `Web.config` arquivo com as informações de configuração do ambiente de produção que são carregados no ambiente de produção como parte do processo de implantação.

> [!NOTE]
> Se você implantar acidentalmente um `Web.config` arquivo que contém a cadeia de caracteres de conexão de banco de dados de desenvolvimento, haverá um erro quando o aplicativo em produção tenta se conectar ao banco de dados. Esse erro se manifesta como um `SqlException` com uma mensagem que informa que o servidor não foi encontrado ou não estava acessível.


Quando o site tiver sido implantado para produção, visite o site de produção por meio de seu navegador. Você deve ver e aproveitar a mesma experiência de usuário como ao executar o aplicativo orientado a dados localmente. Claro quando você visita o site em produção o site é ativado pelo servidor de banco de dados de produção, enquanto visitando o site no ambiente de desenvolvimento usa o banco de dados em desenvolvimento. A Figura 3 mostra o *ensinar por conta própria ASP.NET 3.5 nas 24 horas* examine a página do site no ambiente de produção (Observe a URL na barra de endereços do navegador s).


[![O aplicativo controlada por dados está agora disponível em produção!](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**Figura 3**: The Data-Driven aplicativo está agora disponível em produção! ([Clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>O armazenamento de cadeias de Conexão em um arquivo de configuração separado

Uma técnica comum para manter informações de configuração separadas em ambientes de desenvolvimento e produção é ter duas versões do `Web.config`: um para o ambiente de desenvolvimento e outro para produção. Ao implantar o tempo apropriado `Web.config` versão pode ser copiada para o ambiente de produção. Idealmente, esse processo será automatizado como parte do fluxo de trabalho de implantação.

Em vez de manter dois separado `Web.config` arquivos que você pode, opcionalmente, fornecem as diferenças mais granulares. Os elementos que compõem o `Web.config` arquivo pode ser definido em um arquivos externos de configuração que são referenciados no `Web.config` arquivo. Resumindo pode ter um `Web.config` arquivo para ambos os ambientes que faz referência a um arquivo de databaseConnectionStrings.config, que contém as cadeias de caracteres de conexão usada pelo aplicativo e deve ser exclusivos para cada ambiente. Localizar separar as informações de configuração diferentes em arquivos separados fornece um mais organizada e simples `Web.config` arquivos e muito mais claramente descreve as diferenças de configuração entre os ambientes de desenvolvimento e produção.

Para usar essa técnica, comece criando uma nova pasta no aplicativo da web denominado `ConfigSections`. Em seguida, adicione os dois arquivos para essa nova pasta denominada databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config. Em seguida, copie o `<connectionStrings>` elemento de `Web.config` arquivos databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config e, em seguida, modifique a cadeia de conexão do databaseConnectionStrings.production.config arquivos de forma que ele especifica a cadeia de caracteres de conexão de banco de dados de produção. Por exemplo, o arquivo de databaseConnectionStrings.dev.config deve conter apenas o `<connectionStrings>` elemento com uma cadeia de caracteres de conexão que referencia o banco de dados de desenvolvimento:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

Da mesma forma, o arquivo de databaseConnectionStrings.production.config deve conter apenas um `<connectionStrings>` elemento, mas com a cadeia de caracteres de conexão de banco de dados de produção.

Faça uma cópia do arquivo databaseConnectionStrings.dev.config e nomeie-a como databaseConnectionStrings.config.

> [!NOTE]
> Você pode nomear o arquivo de configuração algo diferente de databaseConnectionStrings.config, se d que, como `connectionStrings.config` ou `dbInfo.config`. No entanto, certifique-se de nomear o arquivo com um `.config` extensão como `.config` arquivos, por padrão, não são servidos pelo mecanismo ASP.NET. Se você nomear o arquivo algo diferente, como `connectionStrings.txt`, um usuário pode apontar seu navegador para [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) e exibir o conteúdo do arquivo!


Neste ponto o `ConfigSections` pasta deve conter três arquivos (consulte a Figura 4). Os arquivos databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config contêm as cadeias de caracteres de conexão para os ambientes de desenvolvimento e produção, respectivamente. O arquivo databaseConnectionStrings.config contém as informações de cadeia de caracteres de conexão que serão usadas pelo aplicativo web em tempo de execução. Consequentemente, o arquivo databaseConnectionStrings.config deve ser idêntico para o arquivo databaseConnectionStrings.dev.config no ambiente de desenvolvimento, enquanto em produção, o arquivo databaseConnectionStrings.config deve ser idêntico ao databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**Figura 4**: ConfigSections ([clique para exibir a imagem em tamanho normal](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))


Agora, precisamos instruir `Web.config` para usar o arquivo databaseConnectionStrings.config para seu armazenamento de cadeia de caracteres de conexão. Abra `Web.config` e substituir o `<connectionStrings>` elemento com o seguinte:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

O `configSource` atributo especifica um caminho físico relativo para o `Web.config` arquivo. Se o external `.config` arquivo está no mesmo diretório que `Web.config` , em seguida, defina este atributo como o nome de arquivo o `.config` arquivo. Se ele s em um subdiretório, como é o caso com databaseConnectionStrings.config, especifique a subpasta usando uma barra invertida para delimitar os nomes de pasta e arquivo, como ConfigSections\databaseConnectionStrings.config.

Com essa modificação, ambientes de desenvolvimento e produção contêm o mesmo `Web.config` arquivo. Agora, a única diferença é o arquivo databaseConnectionStrings.config. Copie o arquivo databaseConnectionStrings.production.config para produção e renomeie-o para databaseConnectionStrings.config. Se houver alterações para a cadeia de caracteres de conexão de banco de dados de produção no futuro, você precisará torná-los para o arquivo databaseConnectionStrings.production.config e, em seguida, carregue esse arquivo para produção, renomeá-la databaseConnectionStrings.config.

> [!NOTE]
> Você pode especificar as informações para qualquer `Web.config` elemento em um arquivo separado e use o `configSource` atributo para fazer referência a esse arquivo no `Web.config`.


## <a name="summary"></a>Resumo

Aplicativos controlados por dados normalmente usam bancos de dados diferentes nos ambientes de desenvolvimento e produção. Consequentemente, as cadeias de caracteres de conexão de banco de dados armazenadas na configuração de s do aplicativo web devem ser exclusivas em cada ambiente. Neste tutorial vimos como determinar a cadeia de conexão de banco de dados de produção e maneiras para manter informações de cadeia de caracteres de conexão exclusivo nos dois ambientes.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Cadeias de caracteres de Conexão e arquivos de configuração](https://msdn.microsoft.com/en-us/library/ms254494.aspx)
- [Informações @ ConnectionStrings.com de cadeias de caracteres de configuração de banco de dados](http://www.connectionstrings.com/)
- [Mover as configurações no arquivo Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentação técnica para o &lt;connectionStrings&gt; elemento](https://msdn.microsoft.com/en-us/library/bf7sd233.aspx)

>[!div class="step-by-step"]
[Anterior](deploying-a-database-vb.md)
[Próximo](configuring-a-website-that-uses-application-services-vb.md)
