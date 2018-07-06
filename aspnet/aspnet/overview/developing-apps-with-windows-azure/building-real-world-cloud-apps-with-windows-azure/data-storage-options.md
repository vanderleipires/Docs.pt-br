---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opções de armazenamento de dados (compilando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 16388bf00c291ee21eec7bc72a9c01c33cec9371
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820328"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opções de armazenamento de dados (compilando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


A maioria das pessoas são usadas para bancos de dados relacionais, e eles tendem a ignorar outras opções de armazenamento de dados quando eles estiver projetando um aplicativo de nuvem. O resultado pode ser desempenho abaixo do ideal, despesas alta – ou pior, pois [NoSQL](http://en.wikipedia.org/wiki/NoSQL) bancos de dados (não relacionais) podem lidar com algumas tarefas de forma mais eficiente que bancos de dados relacionais. Quando os clientes nos pedem para ajudar a resolver um problema de armazenamento de dados críticos, muitas vezes é porque eles têm um banco de dados relacional em que uma das opções NoSQL teria funcionado melhor. Nessas situações o cliente teria sido melhor se tivesse a solução NoSQL implementadas antes de implantar o aplicativo em produção.

Por outro lado, também seria um equívoco supõem que um banco de dados NoSQL pode fazer tudo bem ou bem o suficiente. Não há nenhuma opção de gerenciamento de dados melhor único para todas as tarefas de armazenamento de dados; soluções de gerenciamento de dados diferentes são otimizadas para tarefas diferentes. A maioria dos aplicativos de nuvem do mundo real têm uma variedade de requisitos de armazenamento de dados e são geralmente atendidas melhor por uma combinação de várias soluções de armazenamento de dados.

O objetivo deste capítulo é dar um sentido mais amplo das opções de armazenamento de dados disponíveis para um aplicativo de nuvem e algumas orientações básicas sobre como escolher aqueles que se ajustam ao seu cenário. É melhor estar ciente das opções disponíveis para você e pensar sobre seus pontos fortes e fracos antes de desenvolver um aplicativo. Alterar opções de armazenamento de dados em um aplicativo de produção pode ser extremamente difíceis, como a necessidade de alterar um motor a jato enquanto o plano está em trânsito.

## <a name="data-storage-options-on-azure"></a>Opções de armazenamento de dados no Azure

A nuvem torna relativamente fácil de usar uma variedade de relacional e armazenamentos de dados NoSQL. Aqui estão algumas das plataformas de armazenamento de dados que você pode usar no Azure.

![](data-storage-options/_static/image1.png)

A tabela mostra quatro tipos de bancos de dados NoSQL:

- [Bancos de dados de chave/valor](https://msdn.microsoft.com/library/dn313285.aspx#sec7) armazenar um único objeto serializado para cada valor de chave. Eles são bons para armazenar grandes volumes de dados no qual você deseja obter um item para um determinado valor de chave e você não tiver a consulta com base em outras propriedades do item.

    [O armazenamento de BLOBs do Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) é um banco de dados de chave/valor que funciona como o armazenamento de arquivos na nuvem, com valores de chave que correspondem aos nomes de arquivo e pasta. Você recuperar um arquivo, por sua pasta e o nome do arquivo, não ao procurar por valores no conteúdo do arquivo.

    [Armazenamento de tabela do Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) também é um banco de dados de chave/valor. Cada valor é chamado um *entity* (semelhante a uma linha, identificada por uma chave de partição e chave de linha) e contém várias *propriedades* (semelhante às colunas, mas nem todas as entidades em uma tabela possuem compartilhar o mesmo colunas). Consultando em colunas que não sejam a chave é extremamente ineficiente e deve ser evitado. Por exemplo, você pode armazenar dados de perfil do usuário, com uma partição armazenar informações sobre um único usuário. Você pode armazenar dados, como o nome de usuário, o hash de senha, data de nascimento e assim por diante, em propriedades separadas de uma entidade ou em entidades separadas na mesma partição. Mas você não iria querer de consulta para todos os usuários com um determinado intervalo de datas de nascimento e não é possível executar uma consulta de junção entre a tabela de perfil e outra tabela. Armazenamento de tabela é mais escalonável e mais barato do que um banco de dados relacional, mas ele não permite consultas complexas ou junções.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) são bancos de dados de chave/valor no qual os valores são *documentos*. "Documento" aqui não é usado no sentido de um documento do Word ou Excel, mas significa que uma coleção de campos nomeados e valores, qualquer uma delas pode ser um documento filho. Por exemplo, em uma tabela de histórico do pedido um documento de pedido pode ter número do pedido, data do pedido e campos do cliente; e o campo do cliente pode ter campos de nome e endereço. O banco de dados codifica os dados de campo em um formato como XML, YAML, JSON ou BSON; ou ele pode usar texto sem formatação. Um recurso que define os bancos de dados de documento além dos bancos de dados de chave/valor é a capacidade de consultar em campos de não-chave e definir índices secundários para tornar a consulta mais eficiente. Essa capacidade torna um banco de dados de documentos mais adequado para aplicativos que precisam recuperar dados com base em critérios mais complexos do que o valor da chave do documento. Por exemplo, em um banco de dados do documento de histórico do pedido de vendas você pode consultar em vários campos, como ID do produto, ID do cliente, nome do cliente e assim por diante. [MongoDB](http://www.mongodb.org/) é um banco de dados de documento populares.
- [Bancos de dados de família de coluna](https://msdn.microsoft.com/library/dn313285.aspx#sec9) são armazenamentos de dados que permitem que você para estruturar o armazenamento de dados em coleções de colunas relacionadas chamadas famílias de colunas de chave/valor. Por exemplo, um banco de dados de censo pode ter um grupo de colunas para nome de uma pessoa (primeiro, segundo nome, sobrenome), um grupo para o endereço da pessoa e um grupo para informações de perfil da pessoa (DOB, sexo, etc.). O banco de dados, em seguida, pode armazenar cada família de colunas em uma partição separada, mantendo todos os dados de uma pessoa relacionados à mesma chave. Em seguida, você pode ler todas as informações de perfil sem ter que ler por meio de todas as informações de nome e endereço. [Cassandra](http://cassandra.apache.org/) é um popular banco de dados de família de colunas.
- [Bancos de dados de grafo](https://msdn.microsoft.com/library/dn313285.aspx#sec10) armazenam informações como uma coleção de objetos e relações. A finalidade de um banco de dados do gráfico é permitir que um aplicativo execute eficientemente consultas que atravessam a rede de objetos e as relações entre eles. Por exemplo, os objetos podem ser funcionários em um banco de dados de recursos humanos, e você poderá facilitar consultas como "encontram todos os funcionários que trabalham direta ou indiretamente para Scott." [Neo4j](http://www.neo4j.org/) é um banco de dados do gráfico populares.

Em comparação com bancos de dados relacionais, as opções de NoSQL oferecem muito maior escalabilidade e economia de custo para armazenamento e análise de dados não estruturados. A desvantagem é que eles não fornecem o queryability avançado e recursos de integridade de dados robusta de bancos de dados relacionais. NoSQL funciona bem para dados de log do IIS, que envolve um alto volume, sem necessidade de consultas de junção. NoSQL não funcionaria tão bem para bancários transações, que exige a integridade dos dados absoluto e envolve muitas relações com outros dados relacionados à conta.

Também há uma categoria mais recente da plataforma de banco de dados chamada [NewSQL](http://en.wikipedia.org/wiki/NewSQL) que combina a escalabilidade de um banco de dados NoSQL com o queryability e a integridade transacional de um banco de dados relacional. Bancos de dados NewSQL são projetados para armazenamento distribuído e processamento de consulta, que geralmente é difícil de implementar em bancos de dados "OldSQL". [NuoDB](http://www.nuodb.com/) é um exemplo de um banco de dados NewSQL que pode ser usado no Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop e MapReduce

Os altos volumes de dados que você pode armazenar em bancos de dados NoSQL podem ser difícil analisar de forma eficiente de maneira oportuna. Para fazer o que você pode usar uma estrutura como o [Hadoop](http://hadoop.apache.org/) que implementa [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funcionalidade. Basicamente a que um processo MapReduce faz é o seguinte:

- Limite o tamanho dos dados que precisam ser processados selecionando-se aos dados armazenar apenas os dados que você realmente precisa analisar. Por exemplo, você deseja saber a composição da sua base por ano de nascimento do usuário para que você selecionar apenas os anos de nascimento fora de seu armazenamento de dados de perfil do usuário.
- Dividir os dados em partes e enviá-los em diferentes computadores para processamento. O computador A calcula o número de pessoas com as datas de 1950 1959, computador B faz 1960 1969, etc. Esse grupo de computadores é chamado um *cluster do Hadoop*.
- Coloca os resultados de cada parte-las novamente após o processamento nas partes é concluído. Agora você tem uma lista relativamente curta de quantas pessoas para cada ano de nascimento e a tarefa de cálculo de percentuais nessa lista geral é gerenciável.

No Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) permite que você processe, analise e obtenha novas informações de big data usando o poder do Hadoop. Por exemplo, você pode usá-lo para analisar logs do servidor web:

- Habilite o log do servidor web para sua conta de armazenamento. Isso configura do Azure para gravar logs para o serviço Blob para cada solicitação HTTP ao seu aplicativo. O serviço Blob é basicamente o armazenamento de arquivos de nuvem, e ele se integra perfeitamente com o HDInsight. 

    ![Logs no armazenamento de BLOBs](data-storage-options/_static/image2.png)
- À medida que o aplicativo obtiver o tráfego, logs IIS do servidor web são gravados no armazenamento de Blob. 

    ![Logs do servidor Web](data-storage-options/_static/image3.png)
- No portal, clique em **New** - **serviços de dados** - **HDInsight** - **criação rápida**, e especifique um nome de cluster do HDInsight, o tamanho do cluster (número de nós de dados do cluster HDInsight) e um nome de usuário e senha para o cluster HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

Agora, você pode definir os trabalhos de MapReduce para analisar os logs e obtenha respostas para perguntas como:

- As horas do dia meu aplicativo obter o tráfego de mais ou menos?
- Quais países é o meu tráfego proveniente?
- Qual é a receita média vizinhança das áreas em que o meu tráfego proveniente. (Há um conjunto de dados público que dá renda vizinhança por endereço IP, e pode corresponder que em relação ao endereço IP nos logs do servidor web).
- Como o rendimento da vizinhança se correlacionam a páginas específicas ou produtos no site?

Em seguida, você pode usar as respostas a perguntas como essas direcionar anúncios com base na probabilidade de que um cliente seria interessante ou probabilidade de comprar um produto específico.

Conforme explicado a [capítulo automatizar tudo](automate-everything.md), a maioria das funções que você pode fazer no portal do podem ser automatizadas, e que inclui configurar e executar trabalhos de análise de HDInsight. Um script típico do HDInsight pode conter as seguintes etapas:

- Provisionar um cluster HDInsight e vinculá-lo à sua conta de armazenamento para a entrada de armazenamento de Blob.
- Carregue os executáveis de trabalho do MapReduce (arquivos. jar ou .exe) para o cluster HDInsight.
- Envie um MapReduce que armazena os dados de saída no armazenamento de BLOBs.
- Aguarde a conclusão do trabalho.
- Exclua o cluster HDInsight.
- Acesse a saída do armazenamento de BLOBs.

Ao executar um script que faz tudo isso, você deve minimizar a quantidade de tempo que o cluster HDInsight é provisionado, o que minimiza os custos.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Plataforma como um serviço (PaaS) em vez de infraestrutura como serviço (IaaS)

As opções de armazenamento de dados listadas anteriormente incluem soluções de infraestrutura-como um serviço (IaaS) e de plataforma-como um serviço (PaaS). PaaS significa que podemos gerenciar a infraestrutura de hardware e software, e você usar apenas o serviço. Banco de dados SQL é um recurso de PaaS do Azure. Peça para bancos de dados e em segundo plano Azure define e configura as VMs e configura os bancos de dados neles. Você não tem acesso direto às VMs e não precise gerenciá-los. IaaS significa que você define, configurar e gerenciar VMs que executam em nossa infraestrutura de data center, e você coloca tudo que quiser neles. Nós fornecemos uma galeria de imagens VM pré-configuradas para as configurações comuns de VM. Por exemplo, você pode instalar imagens VM pré-configuradas para o Windows Server 2008, Windows Server 2012, o BizTalk Server, Oracle WebLogic Server, banco de dados Oracle, etc.

Soluções de dados PaaS oferecidos pelo Azure incluem:

- Azure SQL Database (anteriormente conhecido como SQL Azure). Um nuvem banco de dados relacional baseado no SQL Server.
- Armazenamento de tabela do Azure. Uma chave/valor banco de dados NoSQL.
- Armazenamento de BLOBs do Azure. Armazenamento de arquivos na nuvem.

Para IaaS, você pode executar qualquer coisa que você pode carregar em uma VM, por exemplo:

- Bancos de dados relacionais, como o SQL Server, Oracle, MySQL, SQL Compact, SQLite ou Postgres.
- Armazenamentos de dados de chave/valor, como Memcached, Redis, Cassandra e Riak.
- Armazenamentos de dados de coluna, como o HBase.
- Bancos de dados de documento, como, CouchDB, MongoDB e RavenDB.
- Bancos de dados do gráfico como Neo4j.

![Opções de armazenamento de dados no Azure](data-storage-options/_static/image5.png)

A opção de IaaS oferece opções de armazenamento de dados praticamente ilimitados e muitos deles são especialmente fáceis de usar porque você pode criar VMs usando as imagens pré-configuradas. Por exemplo, no portal de gerenciamento, vá para **máquinas virtuais**, clique no **imagens** guia e, em seguida, clique em **procurar VM Depot**.

![Procurar VM Depot](data-storage-options/_static/image6.png)

Você verá uma lista dos [centenas de imagens de VM pré-configuradas](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), e você pode criar uma VM de uma imagem que tenha um sistema de gerenciamento de banco de dados pré-instalado, como o CouchDB, Neo4J, Redis, Cassandra ou MongoDB:

![MongoDB na VM Depot](data-storage-options/_static/image7.png)

O Azure torna tão fáceis de usar possíveis opções de armazenamento de dados de IaaS, mas as ofertas de PaaS têm muitas vantagens que os tornam mais econômico e prático para muitos cenários:

- Você não precisa criar VMs, basta usar o portal ou um script para configurar um repositório de dados. Se você quiser um armazenamento de dados de 200 terabytes, você poderá simplesmente clicar em um botão ou executar um comando, e em segundos, ele está pronto para uso.
- Você não precisa gerenciar ou corrigir as VMs usadas pelo serviço; Microsoft faz isso para você automaticamente.-você não precisa se preocupar sobre como configurar a infraestrutura para colocação em escala ou de alta disponibilidade; Microsoft cuida de tudo isso para você.
- Você não precisa comprar licenças; taxas de licença estão incluídas nas taxas de serviço.
- Você paga apenas pelo que usar.

Opções de armazenamento de dados de PaaS no Azure incluem ofertas por provedores de terceiros. Por exemplo, você pode escolher o [complemento MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) da Store do Azure para provisionar um banco de dados do MongoDB como um serviço.

## <a name="choosing-a-data-storage-option"></a>Escolhendo uma opção de armazenamento de dados

Não há uma abordagem é adequada para todos os cenários. Se qualquer pessoa que disser que essa tecnologia é a resposta, a primeira coisa a fazer é "Qual é a questão?", pois soluções diferentes são otimizadas para coisas diferentes. Há muitas vantagens para o modelo relacional; é por isso que ele já existe há, desde. Mas também há lados de baixo para o SQL que pode ser resolvido com uma solução NoSQL.

Muitas vezes o que vemos funcionam melhor, que é uma abordagem composicional, onde você usa SQL e NoSQL, em uma única solução. Até mesmo quando as pessoas dizem eles estiver adotando o NoSQL, se você analisar o que estão fazendo você geralmente find que elas estão usando várias estruturas diferentes do NoSQL: eles estão usando [CouchDB](http://wiki.apache.org/couchdb/Introduction), e [Redis](http://redis.io/)e [ Riak](http://basho.com/riak/) para ações diferentes. Até mesmo Facebook, que usa extensivamente NoSQL, usa diferentes estruturas de NoSQL para diferentes partes do serviço. A flexibilidade de misturar e combinar as abordagens de armazenamento de dados é uma das coisas que é legal sobre a nuvem, porque ele é fácil de usar várias soluções de dados e integrá-los em um único aplicativo.

Aqui estão algumas perguntas pensar ao escolher uma abordagem:

| Semântica de dados | -O que é o núcleo armazenamento e dados de acesso a dados semântico (você está armazenando dados relacionais ou não estruturados)? Dados não estruturados como arquivos de mídia melhor se encaixa no armazenamento de BLOBs uma coleção de dados relacionados, como produtos, inventários, fornecedores, os pedidos do cliente, etc., melhor se encaixa em um banco de dados relacional. |
| --- | --- |
| Suporte a consultas | -É fácil para consultar os dados? -Quais tipos de perguntas que podem ser eficientemente frequentes? Armazenamentos de dados de chave/valor são muito boas na obtenção de uma única linha, dada um valor de chave, mas não tão bom para consultas complexas. Para um repositório de dados de perfil do usuário em que você sempre está obtendo os dados para um usuário específico, um armazenamento de dados de chave/valor funcionaria bem; para um catálogo de produtos em que você deseja obter agrupamentos diferentes com base em vários atributos de produto um banco de dados relacional pode funcionar melhor. Bancos de dados NoSQL podem armazenar grandes volumes de dados com eficiência, mas você tem que estruturar o banco de dados em torno de como o aplicativo consulta os dados, e isso torna as consultas ad hoc mais difícil de fazer. Com um banco de dados relacional, você pode criar praticamente qualquer tipo de consulta. |
| Projeção funcional | -Pode perguntas, as agregações, etc., ser executado no servidor? Se eu executar SELECT COUNT (\*) de uma tabela no SQL, ele com muita eficiência fazer todo o trabalho no servidor e retornar o número que estou procurando. Se eu quiser o mesmo cálculo de um repositório de dados NoSQL que não dá suporte a agregação, essa é uma "unbounded consulta" ineficiente e provavelmente atingirá o tempo limite. Mesmo se a consulta for bem-sucedida, eu preciso recuperar todos os dados do servidor para o cliente e contar as linhas no cliente. -Quais idiomas ou tipos de expressões podem ser usados? Com um banco de dados relacional, eu posso usar o SQL. Com alguns bancos de dados NoSQL, como o armazenamento de tabela do Azure, que usarei [OData](http://www.odata.org/), e tudo o que posso fazer é que filtrar na chave primária e obter as projeções (Selecionar um subconjunto dos campos disponíveis). |
| Facilidade de escalabilidade | -A frequência e o muito serão os dados precisem ser dimensionados? -A plataforma nativamente neimplementuje scale-out? -É fácil adicionar ou remover capacidade (tamanho e taxa de transferência)? Tabelas e bancos de dados relacionais não são particionadas automaticamente para torná-las escalonável, para que sejam difíceis de dimensionar além de certas limitações. Armazenamentos de dados NoSQL, como o armazenamento de tabela do Azure inerentemente particionar tudo, e há quase não há limite para a adição de partições. Você prontamente pode dimensionar o armazenamento de tabelas até 200 terabytes, mas o tamanho máximo do banco de dados para o banco de dados SQL é 500 GB. Você pode escalar dados relacionais, o particionamento em vários bancos de dados, mas a configuração de um aplicativo para dar suporte a esse modelo envolve muito trabalho de programação. |
| Instrumentação e a capacidade de gerenciamento | -Quão fácil é a plataforma para instrumentar, monitorar e gerenciar? Você precisará manter-se informado sobre a integridade e o desempenho de seu armazenamento de dados, portanto, você precisa saber com antecedência quais métricas são uma plataforma oferece gratuitamente, e o que você precise desenvolver por conta própria. |
| Operações | -Quão fácil é a plataforma para implantar e executar no Azure? PaaS? IaaS? Linux? Armazenamento de tabela e banco de dados SQL são fáceis de configurar no Azure. Plataformas que não são soluções internas de PaaS do Azure exigem mais esforço. |
| Suporte de API | -É uma API disponível que torna mais fácil trabalhar com a plataforma? Para o serviço de tabela do Azure, há um SDK com uma API do .NET que suporta o modelo de programação assíncrono do .NET 4.5. Se você estiver escrevendo um aplicativo .NET, será muito mais fácil de escrever e testar o código para o serviço de tabela do Azure em comparação com outra chave/valor coluna store plataforma de dados que tenha nenhuma API ou um menos abrangente. |
| Consistência de dados e a integridade transacional | -É crítico para a plataforma de dar suporte a transações para assegurar a consistência dos dados? Para acompanhar emails em massa enviadas, desempenho e custo de armazenamento de dados baixa podem ser mais importantes do que o suporte automático para transações ou a integridade referencial na plataforma de dados, tornando o serviço de tabela do Azure uma boa opção. Para acompanhar a conta bancária equilibra ou ordens de compra de uma plataforma de banco de dados relacional que fornece fortes garantias transacionais seria uma opção melhor. |
| Continuidade dos negócios | -Como é fácil são backup, restauração e recuperação de desastres? Mais cedo ou tarde, dados de produção serão são corrompidos e você precisará de uma função de desfazer. Bancos de dados relacionais geralmente têm mais recursos de restauração refinado, como a capacidade de restaurar para um ponto no tempo. Noções básicas sobre quais recursos de restauração estão disponíveis em cada plataforma que você está considerando é um fator importante a considerar. |
| Custo | -Se mais de uma plataforma pode dar suporte a sua carga de trabalho de dados, como eles se comparam em custo? Por exemplo, se você usar a identidade do ASP.NET, você pode armazenar dados de perfil do usuário no serviço tabela do Azure ou banco de dados SQL. Se você não precisa sofisticada consultar recursos do banco de dados SQL, você pode escolher as tabelas do Azure em parte porque custa muito menos para uma determinada quantidade de armazenamento. |

O que nós geralmente recomendamos é saber a resposta para as perguntas em cada uma dessas categorias, antes de escolher suas soluções de armazenamento de dados.

Além disso, sua carga de trabalho pode ter requisitos específicos que algumas plataformas podem dar suporte a melhor do que outros. Por exemplo:

- Exigem seu aplicativo recursos de auditoria?
- Quais são seus requisitos de durabilidade de dados--você precisa de recursos de arquivamento ou de limpeza automatizado?
- Você tem necessidades de segurança especializada? Por exemplo, os dados incluem PII (informações de identificação pessoal), mas você precisa ser capaz de garantir que as PII é excluída de resultados da consulta.
- Se você tiver alguns dados que não podem ser armazenados na nuvem por motivos regulatórios ou tecnológicos, talvez seja necessário uma plataforma de armazenamento de dados de nuvem que facilita a integração com o armazenamento local.

## <a name="demo--using-sql-database-in-azure"></a>Demonstração – usando o banco de dados SQL no Azure

O aplicativo Fix It usa um banco de dados relacional para armazenar tarefas. O script de Windows PowerShell de criação do ambiente mostrado na [capítulo automatizar tudo](automate-everything.md) cria duas instâncias de banco de dados SQL. Você pode ver isso no portal clicando o **bancos de dados SQL** guia.

![Bancos de dados SQL no portal](data-storage-options/_static/image8.png)

Também é fácil criar bancos de dados por meio do portal.

Clique em **novos – serviços de dados** -- **banco de dados SQL** -- **criação rápida**, insira um nome de banco de dados, escolha um servidor que você já tem em sua conta ou crie um novo e clique em **criar banco de dados SQL**.

![Novo Banco de Dados SQL](data-storage-options/_static/image9.png)

Aguarde alguns segundos e você tiver um banco de dados no Azure pronto para uso.

![Novo banco de dados SQL criado](data-storage-options/_static/image10.png)

Portanto, o Azure em poucos segundos, o que pode levar você por dia ou por semana ou mais tempo para realizar no ambiente local. E desde que você pode criar facilmente bancos de dados automaticamente em um script ou usando uma API de gerenciamento, você pode aumentar de forma dinâmica, distribuindo seus dados em vários bancos de dados de < o:p >, desde que seu aplicativo tenha sido programado para fazer isso. < /o : p >

Este é um exemplo de nosso modelo de plataforma como serviço. Você não precisa gerenciar os servidores, podemos fazê-lo. Você não precisa se preocupar sobre backups, podemos fazê-lo. Ele está em execução em alta disponibilidade – os dados no banco de dados são replicados automaticamente em três servidores. Se um computador é desativado, estamos automaticamente o failover e você não perder nenhum dado. O servidor é corrigido regularmente, você não precisa se preocupar com isso.

Clique em um botão e obter a cadeia de caracteres de conexão exato necessário e pode começar a usar imediatamente o novo banco de dados.

![Cadeias de caracteres de Conexão](data-storage-options/_static/image11.png)

O painel mostra o histórico de conexão e a quantidade de armazenamento usado.

![Painel de banco de dados SQL](data-storage-options/_static/image12.png)

Você pode gerenciar bancos de dados no portal ou usando ferramentas do SQL Server você já conhece, incluindo o SQL Server Management Studio (SSMS) e as ferramentas do Visual Studio, SQL Server objeto Explorer (SSOX) e o Gerenciador de servidores.

![SSOX](data-storage-options/_static/image13.png)

Outro aspecto interessante é o modelo de preços. Você pode começar o desenvolvimento com um banco de dados de 20 MB gratuito e um banco de dados de produção começa em cerca de US $5 por mês. Você paga apenas pela quantidade de dados que você realmente armazenar no banco de dados, não a capacidade máxima. Você não precisa comprar uma licença.

É fácil de dimensionar o banco de dados SQL. Para o aplicativo corrigi-lo, o banco de dados que criamos em nosso script de automação é limitado a 1 GB. Para dimensioná-lo até 150 GB, você poderá simplesmente entrar no portal e altere a configuração ou executar um comando de API REST e em segundos, você tem um banco de dados de 150 GB que você pode implantar os dados para o.

![Tamanhos e as edições do banco de dados SQL](data-storage-options/_static/image14.png)

Esse é o poder da nuvem para pé a infra-estrutura de forma rápida e fácil e começar a usá-lo imediatamente.

O aplicativo Fix It usa dois bancos de dados SQL, uma para a associação (autenticação e autorização) e para os dados, e isso é tudo o que você precisa fazer para provisioná-la e dimensioná-lo. Você viu anteriormente como provisionar os bancos de dados por meio de scripts do Windows PowerShell, e agora você viu como é fácil fazer no portal.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework em vez de acesso direto do banco de dados usando ADO.NET

O aplicativo Fix It acessa esses bancos de dados usando o Entity Framework, o ORM (mapeador relacional de objeto) de recomendada pela Microsoft para aplicativos .NET. Um ORM é uma ótima ferramenta que facilita a produtividade do desenvolvedor, mas a produtividade vem às custas da degradação do desempenho em alguns cenários. Em um aplicativo de nuvem do mundo real você não vai fazer uma escolha entre usar o EF ou usando o ADO.NET diretamente, pois você usará os dois. Na maioria das vezes, quando você estiver escrevendo código que funciona com o banco de dados, obter o máximo desempenho não é crítico e você pode aproveitar a codificação simplificada e testes que você obtém com o Entity Framework. Em situações em que o EF sobrecarga causaria um desempenho inaceitável, você pode escrever e executar suas próprias consultas usando o ADO.NET, idealmente chamando procedimentos armazenados.

Qualquer método que você usa para acessar o banco de dados que você deseja minimizar "conversa" tanto quanto possível. Em outras palavras, se você pode obter todos os dados que necessários em um resultado de consulta maior definido em vez de dezenas ou centenas de outros menores, que geralmente é preferível. Por exemplo, se você precisar para alunos de lista e os cursos de que eles serem registrados no, é melhor obter todos os dados na consulta de uma junção em vez de obter os alunos em uma consulta e executar consultas separadas para os cursos de cada aluno.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Bancos de dados SQL e o Entity Framework no aplicativo Fix It

No aplicativo corrigi-lo a `FixItContext` classe, que deriva do Entity Framework `DbContext` de classe, identifica o banco de dados e especifica as tabelas no banco de dados. O contexto Especifica um conjunto de entidades (tabela) para tarefas e o código passa para o contexto o nome de cadeia de caracteres de conexão. Esse nome se refere a uma cadeia de caracteres de conexão que é definida no arquivo Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

A cadeia de conexão na *Web. config* arquivo é nomeado appdb (aqui apontando para o banco de dados de desenvolvimento local):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

O Entity Framework cria uma *FixItTasks* tabela com base em propriedades incluídas no `FixItTask` classe de entidade. Isso é uma classe POCO (objeto Plain Old CLR) simples, o que significa que ele não herda de ou ter nenhuma dependência no Entity Framework. Mas o Entity Framework sabe como criar uma tabela com base nele e executar as operações CRUD (criar-ler-atualizar-excluir) com ele.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabela FixItTasks](data-storage-options/_static/image15.png)

O aplicativo Fix It inclui uma interface de repositório que ele usa para trabalhar com o armazenamento de dados de operações de CRUD.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Observe que os métodos de repositório são todos assíncronos, portanto, todo o acesso de dados pode ser feito de forma assíncrona completamente.

As chamadas de implementação de repositório métodos assíncronos de Entity Framework para trabalhar com os dados, incluindo consultas LINQ, bem como para inserir, atualizar e excluir operações. Aqui está um exemplo de código para pesquisar uma tarefa para corrigi-lo.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Você observará que também há algum tempo e código de registro em log de erro aqui, veremos que posteriormente na [capítulo de monitoramento e telemetria](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Escolher o banco de dados SQL (PaaS) versus SQL Server em uma VM (IaaS) no Azure

Uma coisa boa sobre o SQL Server e banco de dados SQL é que o modelo de programação principais para ambos é idêntico. Você pode usar a maioria das mesmas habilidades em ambos os ambientes. Você pode até usar um banco de dados do SQL Server no desenvolvimento e uma instância de banco de dados SQL na nuvem, que é como o aplicativo Fix It está configurado.

Como alternativa, você pode executar o mesmo SQL Server na nuvem que você execute local instalando-o em VMs IaaS. Para alguns aplicativos herdados, executando o SQL Server em uma VM pode ser uma solução melhor. Como um banco de dados do SQL Server é executado em uma VM dedicada, ele tem mais recursos disponíveis para ela que um banco de dados do banco de dados SQL que é executado em um servidor compartilhado. Isso significa que um banco de dados do SQL Server pode ser maiores e ainda funcionam bem. Em geral, quanto menor o tamanho do banco de dados e tabela, melhor o caso de uso funciona para banco de dados SQL (PaaS).

Aqui estão algumas diretrizes sobre como escolher entre os dois modelos.

| Banco de dados SQL do Azure (PaaS) | SQL Server em uma máquina Virtual (IaaS) |
| --- | --- |
| **Os profissionais de** -você não precisa criar ou gerenciar máquinas virtuais, atualizar ou aplicar o patch de sistema operacional ou do SQL; Azure faz isso para você. -Alta disponibilidade interna, com um SLA de nível de banco de dados. -Baixo custo total de propriedade (TCO) porque você paga apenas pelo que usar (sem licença necessária). -BOM para lidar com grandes números de bancos de dados menores (&lt;= 500 GB). -Fácil de criar dinamicamente novos bancos de dados para habilitar o escalonamento horizontal. | ***Os profissionais de*** - recurso compatível com a versão SQL Server no local. -Pode implementar o SQL Server [alta disponibilidade por meio do AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) em mais de 2 VMs, com SLA de nível de VM. -Você tem controle completo sobre como o SQL é gerenciado. -Pode reutilizar licenças do SQL, você já possui ou paga por hora para um. -BOM para tratar de menos mas maiores (1 TB +) bancos de dados. |
| **Contras** -algumas lacunas em comparação ao SQL Server no local de recursos (falta de [integração CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [suporte à compactação](https://technet.microsoft.com/library/cc280449.aspx), [do SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), etc.)-limite de tamanho do banco de dados de 500 GB. | ***Contras*** – atualizações/patches (sistema operacional e SQL) são de sua responsabilidade - criação e gerenciamento de bancos de dados são de sua responsabilidade - limitado a aproximadamente 8.000 (por meio de unidades de dados de 16) o disco IOPS (operações de entrada/saída por segundo). |

Se você quiser usar o SQL Server em uma VM, você pode usar sua própria licença do SQL Server, ou você pode pagar por um por hora. Por exemplo, no portal ou por meio da API REST, você pode criar uma nova VM usando uma imagem do SQL Server.

![Criar VM com o SQL Server](data-storage-options/_static/image16.png)

![Lista de imagens de VM do SQL Server](data-storage-options/_static/image17.png)

Quando você cria uma VM com uma imagem do SQL Server, vamos o custo de licença do SQL Server por hora com base no uso da VM. Se você tiver um projeto que só será executado por alguns meses, é mais barato de pagar por hora. Se você achar que seu projeto irá durar por anos, é mais barato comprar a licença da maneira que faria normalmente.

## <a name="summary"></a>Resumo

Torna mais prático de combinar de computação em nuvem e abordagens de armazenamento de dados de correspondência para melhor atender às necessidades do seu aplicativo. Se você estiver criando um novo aplicativo, pense cuidadosamente sobre as perguntas listadas aqui para escolher as abordagens que continuarão a funcionar bem quando o aplicativo cresce. O [próximo capítulo](data-partitioning-strategies.md) explicará algumas estratégias de particionamento que você pode usar para combinar várias abordagens de armazenamento de dados.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos.

Escolher uma plataforma de banco de dados:

- [Acesso a dados para soluções altamente escalonáveis: usando SQL, NoSQL e persistência Poliglota](http://aka.ms/dag-doc). Livro eletrônico por Microsoft Patterns and Practices que entra em detalhes em diferentes tipos de dados armazena disponível para aplicativos de nuvem.
- [Microsoft Patterns and Practices - diretrizes do Azure](https://msdn.microsoft.com/library/ff898430.aspx). Consulte Primer de consistência de dados, replicação de dados e diretrizes de sincronização, o padrão de tabela de índice, o padrão de exibição materializada.
- [BASE: Acid alternativa](http://queue.acm.org/detail.cfm?id=1394128). O artigo sobre as compensações entre consistência de dados e a escalabilidade.
- [Bancos de dados de sete de sete semanas: um guia para bancos de dados modernos e movimento NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Livro de Eric Redmond e Jim R. Wilson. Altamente recomendada para apresentar a você mesmo a variedade de plataformas de armazenamento de dados disponíveis hoje.

Escolhendo entre o SQL Server e banco de dados SQL:

- [A visualização Premium para orientação do banco de dados SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Uma introdução ao SQL Database Premium e diretrizes sobre quando escolhê-lo sobre as edições de banco de dados Web e Business.
- [Diretrizes e limitações (banco de dados SQL do Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Página do portal que contém links para documentação sobre as limitações do banco de dados SQL, incluindo uma que se concentra em recursos do SQL Server, esse banco de dados SQL não dá suporte.
- [SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Página do portal que contém links para documentação sobre a execução do SQL Server no Azure.
- [Scott Guthrie explica bancos de dados SQL no Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6 minutos vídeo de Introdução ao banco de dados SQL por Scott Guthrie.
- [Padrões de aplicativo e estratégias de desenvolvimento do SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Usando o Entity Framework e o banco de dados SQL em um aplicativo Web do ASP.NET

- [Introdução ao EF 6 usando o MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série de tutoriais de nove partes que orienta você durante a criação de um aplicativo MVC que usa o EF e implanta o banco de dados do Azure e banco de dados SQL.
- [Implantação de Web do ASP.NET usando o Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Parte de doze série de tutoriais que dá mais detalhes sobre como implantar um banco de dados usando o EF Code First.
- [Implantar um aplicativo ASP.NET MVC 5 seguro com associação, OAuth e banco de dados SQL em um Site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Tutorial passo a passo que orienta você durante a criação de um aplicativo web que usa a autenticação, armazena tabelas de aplicativo no banco de dados de associação, modifica o esquema de banco de dados e implanta o aplicativo do Azure.
- [Mapa de conteúdo de acesso de dados do ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Links para recursos para trabalhar com o EF e o banco de dados SQL.

Usando o MongoDB no Azure:

- [MongoLab - MongoDB no Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Página do portal para obter a documentação sobre como executar o MongoDB no Azure.
- [Criar um site do Azure que se conecta ao MongoDB em execução em uma máquina virtual no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Tutorial passo a passo que mostra como usar um banco de dados do MongoDB em um aplicativo web ASP.NET.

HDInsight (Hadoop no Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). A documentação do HDInsight no portal do [Azure](https://azure.microsoft.com/) site.
- [Hadoop e HDInsight: Big Data no Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artigo da MSDN Magazine, Bruno Terkaly e Ricardo Villalobos, Introdução ao Hadoop no Azure.
- [Microsoft Patterns and Practices - diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte MapReduce padrão.

> [!div class="step-by-step"]
> [Anterior](single-sign-on.md)
> [Próximo](data-partitioning-strategies.md)
