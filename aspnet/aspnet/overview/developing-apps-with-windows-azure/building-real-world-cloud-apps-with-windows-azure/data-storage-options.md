---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: "Opções de armazenamento de dados (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs"
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 88f57244bfbfdf33df3bb265d8aa2c93689b2f24
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opções de armazenamento de dados (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


A maioria das pessoas são usadas para bancos de dados relacionais, e eles tendem a ignorar outras opções de armazenamento de dados quando eles está criando um aplicativo de nuvem. O resultado pode ser desempenho abaixo do ideal, despesas alta ou pior, pois [NoSQL](http://en.wikipedia.org/wiki/NoSQL) bancos de dados (não relacionais) podem lidar com algumas tarefas de forma mais eficiente que bancos de dados relacionais. Quando os clientes nos pedem para ajudar a resolver um problema de armazenamento de dados críticos, geralmente é porque eles têm um banco de dados relacional em que uma das opções NoSQL teria funcionado melhor. Nessas situações o cliente seria melhor se eles tinham implementar a solução de NoSQL antes de implantar o aplicativo para produção.

Por outro lado, também seria um erro assuma que um banco de dados NoSQL pode fazer tudo bem ou suficientemente. Não há nenhuma opção de gerenciamento de dados melhor única para todas as tarefas de armazenamento de dados; soluções de gerenciamento de dados diferentes são otimizadas para tarefas diferentes. A maioria dos aplicativos de nuvem do mundo real têm uma variedade de requisitos de armazenamento de dados e geralmente são atendidas melhor por uma combinação de várias soluções de armazenamento de dados.

O objetivo deste capítulo é fornecer um sentido mais amplo das opções de armazenamento de dados disponíveis para um aplicativo de nuvem e algumas diretrizes básicas sobre como escolher aqueles que se adaptam a seu cenário. É melhor conhecer as opções disponíveis para você e pensar sobre seus pontos fortes e fracos antes de desenvolver um aplicativo. Alterar opções de armazenamento de dados em um aplicativo de produção pode ser extremamente difíceis, como a necessidade de alterar um mecanismo jet enquanto o plano está em trânsito.

## <a name="data-storage-options-on-azure"></a>Opções de armazenamento de dados no Azure

A nuvem torna relativamente fácil de usar uma variedade de relacionais e de dados NoSQL. Aqui estão algumas das plataformas de armazenamento de dados que você pode usar no Azure.

![](data-storage-options/_static/image1.png)

A tabela mostra os quatro tipos de bancos de dados SQL:

- [Bancos de dados de chave/valor](https://msdn.microsoft.com/library/dn313285.aspx#sec7) armazenar um único objeto serializado para cada valor de chave. Elas são boas para armazenar grandes volumes de dados em que você deseja obter um item para um determinado valor de chave e você não tiver a consulta com base em outras propriedades do item.

    [Armazenamento de BLOBs do Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) é um banco de dados de chave/valor que funciona como um arquivo de armazenamento na nuvem, com valores de chave que correspondem aos nomes de arquivo e pasta. Você recuperar um arquivo por sua pasta e o nome do arquivo, não ao procurar por valores no conteúdo do arquivo.

    [Armazenamento de tabela do Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) também é um banco de dados de chave/valor. Cada valor é chamado um *entidade* (semelhante a uma linha, identificada por uma chave de partição e chave de linha) e contém várias *propriedades* (semelhante às colunas, mas nem todas as entidades em uma tabela compartilhar o mesmo colunas). A consulta em colunas que não seja a chave é extremamente ineficiente e deve ser evitado. Por exemplo, você pode armazenar dados de perfil de usuário, com uma partição de armazenar informações sobre um único usuário. Você pode armazenar dados, como o nome de usuário, o hash de senha, data de nascimento e assim por diante, em propriedades separadas de uma entidade ou entidades separadas na mesma partição. Mas você não deseja consultar todos os usuários com um determinado intervalo de datas de aniversário e você não pode executar uma consulta de junção entre a tabela de perfil e outra tabela. Armazenamento de tabela é mais escalonável e mais barato do que um banco de dados relacional, mas ele não permite consultas complexas ou junções.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) são bancos de dados de chave/valor no qual os valores são *documentos*. "Documento" aqui não é usado no sentido de um documento do Word ou Excel, mas significa uma coleção de campos nomeados e valores, que pode ser um documento filho. Por exemplo, em uma tabela de histórico de pedidos um documento de pedido pode ter número do pedido, data do pedido e campos do cliente; e o campo de cliente pode ter campos nome e endereço. O banco de dados codifica dados de campo em um formato como YAML, XML, JSON ou BSON; ou pode usar texto sem formatação. Um recurso que define os bancos de dados de documento além de bancos de dados de chave/valor é a capacidade de consultar em campos de não-chave e definir índices secundários para tornar a consulta mais eficiente. Essa capacidade torna um banco de dados de documento mais adequada para aplicativos que precisam recuperar dados com base em critérios mais complexos do que o valor da chave do documento. Por exemplo, em um banco de dados de documento de histórico de pedidos de vendas você poderia consultar em vários campos, como ID do produto, ID do cliente, nome do cliente e assim por diante. [MongoDB](http://www.mongodb.org/) é um banco de dados de documento populares.
- [Bancos de dados de coluna família](https://msdn.microsoft.com/library/dn313285.aspx#sec9) são os armazenamentos de dados que permitem que você para o armazenamento de dados de estrutura em coleções de colunas relacionadas chamadas famílias de coluna de chave/valor. Por exemplo, um banco de dados de censo pode ter um grupo de colunas de nome de uma pessoa (primeiro, segundo nome, sobrenome), um grupo para o endereço da pessoa e uma para informações de perfil da pessoa (DOB, sexo, etc.). O banco de dados, em seguida, pode armazenar cada família de coluna em uma partição separada, mantendo todos os dados para uma pessoa relacionada à mesma chave. Em seguida, você pode ler todas as informações de perfil sem ter que ler todas as informações de nome e endereço. [Cassandra](http://cassandra.apache.org/) é um banco de dados de coluna família popular.
- [Bancos de dados de gráfico](https://msdn.microsoft.com/library/dn313285.aspx#sec10) armazenar informações como uma coleção de objetos e relações. A finalidade de um banco de dados do gráfico é permitir que um aplicativo executar com eficiência as consultas que atravessam a rede de objetos e as relações entre eles. Por exemplo, os objetos podem ser funcionários em um banco de dados de recursos humanos, e talvez você queira para facilitar consultas, como "localizam todos os funcionários que trabalham direta ou indiretamente para Scott." [Neo4j](http://www.neo4j.org/) é um banco de dados do gráfico populares.

Em comparação com bancos de dados relacionais, as opções de NoSQL oferecem maior escalabilidade e custo-benefício para armazenamento e análise de dados não estruturados. A desvantagem é que eles não fornecem o queryability avançada e recursos de integridade de dados robustos de bancos de dados relacionais. NoSQL funcionaria bem para dados de log do IIS, que envolve um alto volume sem a necessidade de consultas de junção. NoSQL não funcionaria tão bem para serviços bancários, transações, que requer integridade dos dados absoluto e envolve muitas relações com outros dados relacionados à conta.

Também é uma categoria mais recente da plataforma de banco de dados chamada [NewSQL](http://en.wikipedia.org/wiki/NewSQL) que combina a escalabilidade do banco de dados NoSQL com a queryability e a integridade transacional do banco de dados relacional. Bancos de dados NewSQL são projetados para armazenamento distribuído e processamento de consulta, que geralmente é difícil de implementar em bancos de dados "OldSQL". [NuoDB](http://www.nuodb.com/) é um exemplo de um banco de dados NewSQL que pode ser usado no Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop e MapReduce

Grandes volumes de dados que você pode armazenar em bancos de dados NoSQL podem ser difícil analisar com eficiência de maneira oportuna. Para fazer o que você pode usar uma estrutura como [Hadoop](http://hadoop.apache.org/) que implementa [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funcionalidade. Essencialmente o que um processo de MapReduce faz é o seguinte:

- Limite o tamanho dos dados que precisam ser processada, selecionando os dados armazenar apenas os dados que você realmente precisa analisar. Por exemplo, você deseja saber a composição de seu usuário base por ano do aniversário, para que você selecionar apenas os anos de nascimento fora de seu armazenamento de dados de perfil de usuário.
- Dividir os dados em partes e enviá-los em computadores diferentes para processamento. O computador A calcula o número de pessoas com datas 1950 1959, computador B faz 1960 1969, etc. Este grupo de computadores é chamado um *cluster Hadoop*.
- Coloca os resultados de cada parte novo, após o processamento em partes. Agora você tem uma lista relativamente curta de quantas pessoas de cada ano do aniversário e a tarefa de cálculo porcentagens nesta lista geral é gerenciada.

No Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) permite processar, analisar e ter novas ideias do big data usando o poder do Hadoop. Por exemplo, você pode usá-lo para analisar logs do servidor web:

- Habilite o log do servidor web para sua conta de armazenamento. Isso define o Azure para gravar logs para o serviço Blob para cada solicitação HTTP para o seu aplicativo. O serviço Blob é, basicamente, armazenamento de arquivos de nuvem e ele se integra perfeitamente ao HDInsight. 

    ![Logs de armazenamento de Blob](data-storage-options/_static/image2.png)
- Como o aplicativo obtém tráfego, logs IIS do servidor web são gravados no armazenamento de Blob. 

    ![Logs do servidor Web](data-storage-options/_static/image3.png)
- No portal, clique em **novo** - **Data Services** - **HDInsight** - **criação rápida**, e especifique um nome de cluster HDInsight, tamanho do cluster (número de nós de dados de cluster do HDInsight) e um nome de usuário e senha para o cluster HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

Agora você pode definir os trabalhos de MapReduce para analisar os logs e obter respostas a perguntas como:

- Que horas do dia meu aplicativo obter o tráfego de mais ou menos?
- Quais países é o tráfego proveniente?
- O que é a renda média ambiente das áreas que o tráfego proveniente. (Há um conjunto de dados público que lhe renda ambiente por endereço IP, e você pode corresponder que endereço IP nos logs do servidor web.)
- Como o rendimento do ambiente se relaciona ao páginas específicas ou produtos no site?

Você pode usar as respostas a perguntas como essas direcionar anúncios com base na probabilidade de que um cliente seria interessante ou provavelmente comprará um produto específico.

Conforme explicado no [capítulo automatizar tudo](automate-everything.md), a maioria das funções que você pode fazer no portal do pode ser automatizada e que inclui a configuração e executar trabalhos de análise de HDInsight. Um script típico do HDInsight pode conter as seguintes etapas:

- Provisionar um cluster HDInsight e vinculá-la à sua conta de armazenamento para a entrada de armazenamento de Blob.
- Carregar o trabalho de MapReduce executáveis (arquivos. jar ou .exe) para o cluster HDInsight.
- Envie um MapReduce que armazena os dados de saída para o armazenamento de Blob.
- Aguarde a conclusão do trabalho.
- Exclua o cluster HDInsight.
- Acesso a saída do armazenamento de Blob.

Ao executar um script que faz isso, você minimiza a quantidade de tempo que o cluster HDInsight está provisionado, minimizando os custos.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Plataforma como serviço (PaaS) em vez de infraestrutura como serviço (IaaS)

As opções de armazenamento de dados listadas anteriormente incluem plataforma-como-um-serviço (PaaS) e soluções de infraestrutura-como-um-serviço (IaaS). PaaS significa que podemos gerenciar a infraestrutura de hardware e software, e você usar apenas o serviço. Banco de dados SQL é um recurso de PaaS do Azure. Peça para bancos de dados e nos bastidores Azure define e configura as VMs e configura os bancos de dados neles. Você não tem acesso direto às VMs e não é necessário gerenciá-los. IaaS significa que você configurar, configura e gerenciar as VMs que executam em nossa infraestrutura do data center, e você colocar tudo o que você quiser neles. Nós fornecemos uma galeria de imagens VM pré-configurada para configurações comuns de VM. Por exemplo, você pode instalar imagens VM pré-configurada para o Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, banco de dados Oracle, etc.

Soluções de dados de PaaS Azure oferece incluem:

- Azure SQL Database (anteriormente conhecido como SQL Azure). Um nuvem banco de dados relacional baseado no SQL Server.
- Armazenamento de tabela do Azure. Um chave/valor de banco de dados NoSQL.
- Armazenamento de BLOBs do Azure. Armazenamento de arquivo na nuvem.

Para IaaS, você pode executar qualquer coisa que você pode carregar em uma VM, por exemplo:

- Bancos de dados relacionais, como o SQL Server, Oracle, MySQL, SQL Compact, SQLite ou Postgres.
- Repositórios de dados de chave/valor como Memcached, Redis, Cassandra e Riak.
- Repositórios de dados de coluna como HBase.
- Bancos de dados de documento como MongoDB, RavenDB e CouchDB.
- Bancos de dados do gráfico como Neo4j.

![Opções de armazenamento de dados no Azure](data-storage-options/_static/image5.png)

A opção de IaaS oferece opções de armazenamento de dados praticamente ilimitada e muitas delas são especialmente fácil de usar porque você pode criar VMs usando imagens pré-configuradas. Por exemplo, no portal de gerenciamento, vá para **máquinas virtuais**, clique no **imagens** guia e, em seguida, clique em **procurar VM Depot**.

![Procurar VM Depot](data-storage-options/_static/image6.png)

Você verá uma lista de [centenas de imagens VM pré-configuradas](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), e você pode criar uma máquina virtual de uma imagem que tem um sistema de gerenciamento de banco de dados pré-instalado, como MongoDB, Neo4J, Redis, Cassandra ou CouchDB:

![MongoDB no VM Depot](data-storage-options/_static/image7.png)

Azure torna mais fácil de usar possíveis IaaS opções de armazenamento de dados, mas as ofertas PaaS tem muitas vantagens que os tornam mais econômico e prático para muitos cenários:

- Você não precisa criar VMs, basta usar o portal ou um script para configurar um repositório de dados. Se você quiser um repositório de dados de 200 terabytes, poderá simplesmente clicar em um botão ou executar um comando, e em segundos, ele está pronto para uso.
- Você não precisa gerenciar ou corrigir as VMs usadas pelo serviço; Microsoft faz isso para você automaticamente.-você não precisa se preocupar sobre como configurar a infraestrutura de escala ou de alta disponibilidade; A Microsoft lidará com tudo o que você.
- Você não precisa comprar licenças; taxas de licença são incluídas nas taxas de serviço.
- Você só paga pelo que usa.

Opções de armazenamento de dados de PaaS no Azure incluem ofertas por provedores de terceiros. Por exemplo, você pode escolher o [complemento MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) do armazenamento do Azure para provisionar um banco de dados do MongoDB como um serviço.

## <a name="choosing-a-data-storage-option"></a>Escolhendo uma opção de armazenamento de dados

Não há uma abordagem é adequada para todos os cenários. Se qualquer pessoa que diz que essa tecnologia é a resposta, a primeira coisa a fazer é "O que é a pergunta?", porque diferentes soluções são otimizadas para coisas diferentes. Há muitas vantagens para o modelo relacional; é por isso que ele foi contanto. Mas também há lados de baixo para o SQL que pode ser resolvido com uma solução NoSQL.

Geralmente o que podemos ver melhor de trabalho é uma abordagem composicional, onde você pode usar SQL e NoSQL em uma única solução. Mesmo quando as pessoas dizem que estiver adotando NoSQL, se você analisar o que estão fazendo você geralmente localizar que estiver usando várias estruturas NoSQL diferentes: elas estão usando [CouchDB](http://wiki.apache.org/couchdb/Introduction), e [Redis](http://redis.io/)e [ Riak](http://basho.com/riak/) para ações diferentes. Mesmo Facebook, que usa extensivamente NoSQL, usa estruturas de NoSQL diferentes para diferentes partes do serviço. A flexibilidade a combinação de métodos de armazenamento de dados é uma das coisas interessante sobre a nuvem, pois é fácil de usar várias soluções de dados e integrá-los em um único aplicativo.

Aqui estão algumas questões a considerar quando você escolhe uma abordagem:

| Semântica de dados | -O que é o principal de armazenamento e dados acesso a dados semântico (você está armazenando dados relacionais ou não estruturados)? Dados não estruturados, como arquivos de mídia adequado no armazenamento de blob; uma coleção de dados relacionados, como produtos, inventários, fornecedores, pedidos de clientes, etc., adequado no banco de dados relacional. |
| --- | --- |
| Suporte de consulta | -É fácil para consultar os dados? -Quais tipos de perguntas que podem ser eficiente solicitado? Armazenamentos de dados de chave/valor são muito boas obtendo uma única linha, dada um valor de chave, mas não tão bom para consultas complexas. Para um repositório de dados do perfil do usuário em que você sempre está obtendo os dados para um usuário específico, um repositório de dados de chave/valor funcionaria bem; para um catálogo de produtos em que você deseja obter agrupamentos diferentes com base em vários atributos de produto um banco de dados relacional pode funcionar melhor. Bancos de dados NoSQL podem armazenar grandes volumes de dados com eficiência, mas você tem a estrutura de banco de dados em torno de como o aplicativo consulta os dados e isso mais difícil consultas ad hoc fazer. Com um banco de dados relacional, você pode criar praticamente qualquer tipo de consulta. |
| Projeção funcional | -Pode perguntas, agregações, etc., ser executado do lado do servidor? Se eu executar selecione contagem (\*) de uma tabela no SQL, ele será muito eficiente fazer todo o trabalho no servidor e retornar o número que estou procurando. Se eu quiser o mesmo cálculo de um repositório de dados NoSQL que não oferece suporte a agregação, este é uma "unbounded consulta" ineficiente e provavelmente irá expirar. Mesmo se a consulta for bem-sucedida, eu preciso que recuperar todos os dados do servidor para o cliente e a contagem de linhas no cliente. -Quais idiomas ou tipos de expressões podem ser usados? Com um banco de dados relacional, pode usar o SQL. Com alguns bancos de dados NoSQL, como o armazenamento de tabela do Azure, estará usando [OData](http://www.odata.org/), e tudo o que posso fazer é filtrar na chave primária e obter projeções (Selecionar um subconjunto dos campos disponíveis). |
| Facilidade de escalabilidade | -A frequência e o máximo serão os dados precisam dimensionar? -A plataforma nativamente a implementar expansão? -É fácil para adicionar ou remover capacidade (tamanho e taxa de transferência)? Tabelas e bancos de dados relacionais automaticamente não são particionadas para torná-los escalonável, para que sejam difíceis de dimensionar além de certas limitações. Repositórios de dados NoSQL como armazenamento de tabela do Azure inerentemente particionar tudo, e há quase nenhum limite para a adição de partições. Dimensionar o armazenamento de tabela prontamente até 200 terabytes, mas o tamanho máximo do banco de dados para o banco de dados do SQL Azure é de 500 GB. Você pode dimensionar dados relacionais por particioná-la em vários bancos de dados, mas a configuração de um aplicativo para oferecer suporte a esse modelo envolve muita programação do trabalho. |
| Instrumentação e a capacidade de gerenciamento | -É fácil a plataforma para instrumentar, monitorar e gerenciar? Você precisará manter informado sobre a integridade e o desempenho do seu repositório de dados, portanto você precisa saber com antecedência quais métricas uma plataforma fornece gratuitamente, e o que você precisa desenvolver por conta própria. |
| Operações | -É fácil a plataforma para implantar e executar no Azure? PaaS? IaaS? Linux? Armazenamento de tabela e banco de dados SQL são fáceis de configurar no Azure. As plataformas que não são soluções internas de PaaS do Azure exigem mais esforço. |
| Suporte de API | -É uma API disponível que facilita o trabalho com a plataforma? Para o serviço de tabela do Azure, há um SDK com uma API .NET que suporta o modelo de programação assíncrono do .NET 4.5. Se você estiver escrevendo um aplicativo .NET, será muito mais fácil de escrever e testar o código para o serviço de tabela do Azure em comparação com outra chave/valor coluna repositório de plataforma de dados com nenhuma API ou um menos abrangente. |
| Consistência de dados e a integridade transacional | -É crítico para a plataforma oferece suporte a transações para garantir a consistência de dados? Para manter o controle de emails em massa enviados, desempenho e o custo de armazenamento de dados baixa podem ser mais importantes do que suporte automático para transações ou a integridade referencial na plataforma de dados, fazendo o serviço de tabela do Azure uma boa opção. Para o controle de conta bancária saldos ou ordens de compra de uma plataforma de banco de dados relacional que fornece fortes garantias transacionais seria uma opção melhor. |
| Continuidade de negócios | -Como é fácil tem backup, restauração e recuperação de desastres? Mais cedo ou mais tarde os dados de produção serão obter corrompidos e você precisará de uma função de desfazer. Bancos de dados relacionais geralmente têm mais recursos de restauração refinada, como a capacidade de restaurar para um ponto no tempo. Entender quais recursos de restauração estão disponíveis em cada plataforma que você está pensando em é um fator importante a considerar. |
| Custo | -Se mais de uma plataforma pode oferecer suporte a sua carga de trabalho de dados, como eles se comparam custo? Por exemplo, se você usar a identidade do ASP.NET, você pode armazenar dados de perfil de usuário no serviço de tabela do Azure ou banco de dados do SQL Azure. Se você não precisar o sofisticado consultar recursos do banco de dados SQL, você pode escolher tabelas do Azure em parte porque custa muito menos para uma determinada quantidade de armazenamento. |

O que geralmente recomendamos é saber a responder a perguntas em cada uma dessas categorias antes de escolher suas soluções de armazenamento de dados.

Além disso, sua carga de trabalho pode ter requisitos específicos que algumas plataformas podem suportar melhor do que outros. Por exemplo:

- Exigem o aplicativo recursos de auditoria?
- Quais são os requisitos de durabilidade de dados--você precisa de recursos de arquivamento ou limpeza automatizados?
- Você tem necessidades específicas de segurança? Por exemplo, os dados incluem PII (informações de identificação pessoal), mas você precisa ser capaz de garantir que informações de identificação pessoal é excluída de resultados da consulta.
- Se você tiver alguns dados que não podem ser armazenados na nuvem por motivos regulatórios ou tecnológicas, talvez seja necessário uma plataforma de armazenamento de dados de nuvem que facilita a integração com o armazenamento local.

## <a name="demo--using-sql-database-in-azure"></a>Demonstração – usando o banco de dados SQL no Azure

O aplicativo corrigir usa um banco de dados relacional para armazenar tarefas. O script de Windows PowerShell de criação do ambiente mostrado no [capítulo automatizar tudo](automate-everything.md) cria duas instâncias de banco de dados SQL. Você pode ver esses itens no portal clicando o **bancos de dados SQL** guia.

![Bancos de dados SQL no portal](data-storage-options/_static/image8.png)

Também é fácil criar bancos de dados usando o portal.

Clique em **New - serviços de dados** -- **banco de dados SQL** -- **criação rápida**, insira um nome de banco de dados, escolha um servidor que você já tem em sua conta ou crie um novo e clique em **criar banco de dados do SQL**.

![Novo Banco de Dados SQL](data-storage-options/_static/image9.png)

Aguarde alguns segundos e você tiver um banco de dados no Azure pronto para uso.

![Novo banco de dados SQL criado](data-storage-options/_static/image10.png)

Para o Azure em alguns segundos o que levará você um dia ou semana ou mais tempo para executar no ambiente local. E como a mesma facilidade você pode criar bancos de dados automaticamente em um script ou usando uma API de gerenciamento, você pode dinamicamente expandir distribuindo em seus dados em vários bancos de dados: < p >, desde que foi programado para que seu aplicativo. < /o : p >

Este é um exemplo de nosso modelo de plataforma como serviço. Não é necessário gerenciar os servidores, podemos fazer isso. Você não precisa se preocupar sobre backups, podemos fazer isso. Ele está em execução em alta disponibilidade – os dados no banco de dados são replicados em três servidores automaticamente. Se falhar, uma máquina, podemos automaticamente o failover e você não perderá nenhum dado. O servidor é corrigido regularmente, você não precisa se preocupar com isso.

Clique em um botão e obter a cadeia de caracteres de conexão exato necessário e pode começar a usar o novo banco de dados.

![Cadeias de caracteres de Conexão](data-storage-options/_static/image11.png)

O painel mostra a quantidade de armazenamento usada e de histórico de conexão.

![Painel de banco de dados SQL](data-storage-options/_static/image12.png)

Você pode gerenciar bancos de dados no portal ou usando ferramentas do SQL Server você já estiver familiarizado com, incluindo o SQL Server Management Studio (SSMS) e as ferramentas do Visual Studio, SQL Server objeto Explorer (SSOX) e o Gerenciador de servidores.

![SSOX](data-storage-options/_static/image13.png)

Outra vantagem é que o modelo de preços. Você pode iniciar o desenvolvimento com um banco de dados de 20 MB gratuito e inicia um banco de dados de produção em cerca de US $5 por mês. Você paga somente a quantidade de dados que você realmente armazena no banco de dados, não a capacidade máxima. Você não precisa comprar uma licença.

É fácil dimensionar o banco de dados SQL. Para o aplicativo corrigi-lo, o banco de dados que criamos em nosso script de automação está limitado a 1 GB. Se você quiser dimensioná-lo até 150 GB, basta entrar no portal e altere a configuração ou executar um comando de API REST e em segundos, você tem um banco de dados de 150 GB que você pode implantar os dados em.

![Tamanhos e as edições de banco de dados SQL](data-storage-options/_static/image14.png)

Esse é o poder da nuvem para preparar infraestrutura rapidamente e facilmente e começar a usá-lo imediatamente.

O aplicativo corrigir usa dois bancos de dados SQL, uma associação (autenticação e autorização) e para os dados, e isso é tudo o que você precisa fazer para provisioná-lo e dimensioná-lo. Anteriormente, você viu como provisionar os bancos de dados por meio de scripts do Windows PowerShell, e agora você viu como é fácil fazer no portal.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework em vez de acesso direto de banco de dados usando o ADO.NET

O aplicativo corrigir acessa esses bancos de dados usando o Entity Framework, da Microsoft recomenda ORM (mapeador relacional de objeto) para aplicativos .NET. Um ORM é uma excelente ferramenta que facilita a produtividade do desenvolvedor, mas a produtividade vem às custas de degradação do desempenho em alguns cenários. Em um aplicativo de nuvem do mundo real não fará uma escolha entre usar EF ou diretamente usando o ADO.NET - você vai usar ambos. Na maioria das vezes, quando você está escrevendo código que funciona com o banco de dados, obter desempenho máximo não for crítico e você pode aproveitar a codificação simplificada e de teste que você obtenha com o Entity Framework. Em situações em que o EF sobrecarga causará desempenho inaceitável, você pode gravar e executar suas próprias consultas usando ADO.NET, idealmente chamando procedimentos armazenados.

Qualquer método que você usa para acessar o banco de dados que você deseja minimizar "conversa" tanto quanto possível. Em outras palavras, se você pode obter todos os dados que necessários em um resultado de consulta maior definido em vez de dezenas ou centenas de menores, que geralmente é preferível. Por exemplo, se você precisar alunos da lista e os cursos de que eles serem registrados no, é melhor obter todos os dados na consulta de uma junção em vez de obter os alunos em uma consulta e executar consultas separadas para cursos de cada aluno.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Bancos de dados SQL e o Entity Framework no aplicativo corrigir

No aplicativo de corrigir o `FixItContext` classe que deriva de Entity Framework `DbContext` classe, identifica o banco de dados e especifica as tabelas no banco de dados. O contexto Especifica um conjunto de entidades (tabela) para tarefas e o código passa para o contexto o nome da cadeia de caracteres de conexão. Esse nome se refere a uma cadeia de caracteres de conexão que é definida no arquivo Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

A cadeia de conexão do *Web. config* arquivo é nomeado appdb (aqui apontando para o banco de dados de desenvolvimento local):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

O Entity Framework cria um *FixItTasks* tabela com base em propriedades incluídas no `FixItTask` classe da entidade. Isso é uma classe simples do POCO (objeto Plain Old CLR), o que significa que ele não herda de ou tem dependências no Entity Framework. Mas o Entity Framework sabe como criar uma tabela com base nele e execute as operações CRUD (criar-leitura-atualização-exclusão) com ele.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabela FixItTasks](data-storage-options/_static/image15.png)

O aplicativo corrigir inclui uma interface de repositório que ele usa para operações CRUD de trabalhar com o repositório de dados.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Observe que os métodos de repositório são todos assíncrono, para que todo o acesso de dados pode ser feito de maneira assíncrona completamente.

As chamadas de implementação de repositório métodos assíncronos de Entity Framework para trabalhar com os dados, incluindo consultas LINQ, bem como para inserir, atualizar e excluir operações. Aqui está um exemplo de código para pesquisar uma tarefa corrigir.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Você observará que também há algum tempo e o código de log de erro aqui, veremos que posteriormente no [capítulo de monitoramento e telemetria](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Escolha o banco de dados SQL (PaaS) em comparação com o SQL Server em uma VM (IaaS) no Azure

Uma vantagem do SQL Server e banco de dados do SQL Azure é que o modelo de programação de núcleo para ambos é idêntico. Você pode usar a maioria das mesmas habilidades em ambos os ambientes. Você pode até usar um banco de dados do SQL Server no desenvolvimento e uma instância de banco de dados SQL na nuvem, que é como o aplicativo corrigir é configurado.

Como alternativa, você pode executar o mesmo SQL Server na nuvem que você execute local instalando-o em VMs de IaaS. Para alguns aplicativos herdados, executando o SQL Server em uma VM pode ser a melhor solução. Como um banco de dados do SQL Server é executado em uma máquina virtual dedicada, ele tem mais recursos disponíveis para ele que um banco de dados do banco de dados SQL que é executado em um servidor compartilhado. Isso significa que um banco de dados do SQL Server pode ser maiores e ainda funcionam bem. Em geral, quanto menor o tamanho do banco de dados e tabela, melhor o caso de uso funciona para banco de dados SQL (PaaS).

Aqui estão algumas diretrizes sobre como escolher entre os dois modelos.

| Banco de dados do SQL Azure (PaaS) | SQL Server em uma máquina Virtual (IaaS) |
| --- | --- |
| **Os profissionais de** -você não precisa criar ou gerenciar VMs, atualizar ou corrigir o sistema operacional ou do SQL; Azure faz isso para você. -Alta disponibilidade interna, com um SLA de nível de banco de dados. -Baixo custo total de propriedade (TCO) porque você paga apenas pelo que usa (não é necessária uma licença). -BOM para tratar de grandes números de bancos de dados menores (&lt;= 500 GB). -Fácil de criar dinamicamente novos bancos de dados para permitir expansão. | ***Os profissionais de*** - recurso compatível com SQL Server no local. -Pode implementar o SQL Server [alta disponibilidade por meio do AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) em 2 + máquinas virtuais, com um SLA de nível de VM. -Você tem controle total sobre a forma de gerenciamento do SQL. -Pode reutilizar licenças SQL, você já possui ou de pagamento por hora para um. -BOM para lidar com menos mas maior (1 TB) bancos de dados. |
| **Contras** -algumas lacunas em comparação com o SQL Server no local de recursos (falta de [integração CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [suporte à compactação](https://technet.microsoft.com/library/cc280449.aspx), [SQL Servidor de relatório](https://technet.microsoft.com/library/ms159106.aspx), etc.)-limite de tamanho do banco de dados de 500 GB. | ***Contras*** - atualizações/patches (SO e SQL) são de sua responsabilidade - criação e gerenciamento de bancos de dados são de sua responsabilidade - disco de IOPS (operações de entrada/saída por segundo) limitada a aproximadamente 8.000 (por meio de unidades de dados de 16). |

Se você quiser usar o SQL Server em uma VM, você pode usar sua própria licença do SQL Server, ou você pode pagar por um por hora. Por exemplo, no portal ou por meio da API REST, você pode criar uma nova VM usando uma imagem do SQL Server.

![Criar VM com o SQL Server](data-storage-options/_static/image16.png)

![Lista de imagens de VM do SQL Server](data-storage-options/_static/image17.png)

Quando você cria uma máquina virtual com uma imagem do SQL Server, podemos taxa Pro o custo de licença do SQL Server por hora com base no uso da VM. Se você tiver um projeto que só será executada para alguns meses, é mais barato de pagamento por hora. Se você achar que seu projeto vai para a última por anos, é mais barato de comprar a licença da maneira que faria normalmente.

## <a name="summary"></a>Resumo

Torna mais prático misturar a computação em nuvem e abordagens de armazenamento de dados de correspondência para melhor atenderem às necessidades do seu aplicativo. Se você estiver criando um novo aplicativo, considere cuidadosamente as perguntas listadas aqui para escolher as abordagens que continuarão a funcionar bem quando seu aplicativo cresce. O [próximo capítulo](data-partitioning-strategies.md) explicará algumas estratégias de particionamento que você pode usar para combinar várias abordagens de armazenamento de dados.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos.

Escolhendo uma plataforma de banco de dados:

- [Acesso a dados para soluções altamente escalonáveis: usando SQL, NoSQL e persistência Polyglot](http://aka.ms/dag-doc). Livro eletrônico pelo Microsoft Patterns e práticas recomendadas que entra em detalhes em diferentes tipos de dados armazena disponível para aplicativos em nuvem.
- [Padrões e práticas - diretrizes do Azure Microsoft](https://msdn.microsoft.com/library/ff898430.aspx). Consulte o livro de instruções de consistência de dados, replicação de dados e diretrizes de sincronização, tabela de índice padrão, os padrões de exibição materializada.
- [BASE: An Acid Alternative](http://queue.acm.org/detail.cfm?id=1394128). O artigo sobre a relação entre a consistência dos dados e a escalabilidade.
- [Bancos de dados de sete dentro de sete semanas: um guia para bancos de dados modernos e a movimentação de NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Livro de Eric Redmond e Jim R. Wilson. Altamente recomendável para introduzir por conta própria para a variedade de plataformas de armazenamento de dados disponíveis no momento.

Escolhendo entre o SQL Server e banco de dados SQL:

- [Visualização Premium para orientação de banco de dados do SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Uma introdução para Premium do banco de dados SQL e orientações sobre quando escolhê-lo sobre as edições do SQL de banco de dados Web e Business.
- [Diretrizes e limitações (banco de dados do SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Página de portal que contém links para documentação sobre as limitações do banco de dados SQL, incluindo uma que se concentra nos recursos do SQL Server, esse banco de dados do SQL não oferece suporte.
- [SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Página de portal que contém links para documentação sobre a execução do SQL Server no Azure.
- [Scott Guthrie explica bancos de dados SQL no Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6 minutos vídeo de Introdução ao banco de dados SQL por Scott Guthrie.
- [Padrões de aplicativo e estratégias de desenvolvimento para SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Usando o Entity Framework e o banco de dados SQL em um aplicativo da Web do ASP.NET

- [Guia de Introdução ao EF 6 usando MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série de tutoriais de nove partes que orienta a criação de um aplicativo MVC que usa EF e implanta o banco de dados do Azure e banco de dados SQL.
- [Implantação de Web do ASP.NET usando o Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Doze tutorial série que entra em mais detalhes sobre como implantar um banco de dados usando EF Code First.
- [Implantar um aplicativo de seguro ASP.NET MVC 5 com associação, OAuth e o banco de dados SQL em um Site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Tutorial passo a passo que orienta você na criação de um aplicativo web que usa a autenticação, armazena tabelas de aplicativo no banco de dados de associação, modifica o esquema de banco de dados e implanta o aplicativo no Azure.
- [Mapa de conteúdo de acesso de dados do ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Links para recursos para trabalhar com o EF e banco de dados SQL.

Usando o MongoDB no Azure:

- [MongoLab - MongoDB no Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Página do portal para documentação sobre como executar o MongoDB no Azure.
- [Criar um site do Azure que se conecta ao MongoDB em execução em uma máquina virtual no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Tutorial passo a passo que mostra como usar um banco de dados do MongoDB em um aplicativo web ASP.NET.

HDInsight (Hadoop no Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). A documentação do HDInsight no portal de [Azure](https://azure.microsoft.com/) site.
- [Hadoop e HDInsight: grandes de dados no Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artigo da MSDN Magazine, Bruno Terkaly e Ricardo Villalobos, apresentando Hadoop no Azure.
- [Padrões e práticas - diretrizes do Azure Microsoft](https://msdn.microsoft.com/library/dn568099.aspx). Consulte MapReduce padrão.

>[!div class="step-by-step"]
[Anterior](single-sign-on.md)
[Próximo](data-partitioning-strategies.md)
