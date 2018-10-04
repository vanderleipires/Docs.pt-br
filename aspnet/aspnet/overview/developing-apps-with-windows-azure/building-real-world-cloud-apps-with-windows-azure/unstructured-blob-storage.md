---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Armazenamento de Blob não estruturados (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1a56a76c9bf27fdd7afb090ca473ebeee4065fe2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577412"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Armazenamento de Blob não estruturados (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


No capítulo anterior, examinamos os esquemas de particionamento e explicou como o aplicativo Fix It armazena imagens no serviço de armazenamento de BLOBs do Azure e outros dados de tarefa no banco de dados SQL. Neste capítulo vamos detalhar o serviço Blob e mostrar como ele é implementado no código do projeto Fix It.

## <a name="what-is-blob-storage"></a>O que é o armazenamento de BLOBs?

O serviço de armazenamento de BLOBs do Azure fornece uma maneira de armazenar arquivos na nuvem. O serviço Blob tem uma série de vantagens em relação ao armazenamento de arquivos em um sistema de arquivos de rede local:

- É altamente escalonável. Uma única conta de armazenamento pode armazenar [centenas de terabytes](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), e você pode ter várias contas de armazenamento. Alguns dos maiores clientes do Azure armazenam de centenas de petabytes. Microsoft SkyDrive usa o armazenamento de blob.
- Ele é durável. Cada arquivo que você armazena no serviço Blob é feito automaticamente.
- Ele fornece alta disponibilidade. O [SLA para armazenamento](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promessas de 99,9% ou 99,99% tempo de atividade, dependendo de qual opção de redundância geográfica em que você escolher.
- É um recurso (PaaS) de plataforma como serviço do Azure, o que significa que você acabou de armazena e recupera arquivos, pagando somente a quantidade real de armazenamento que você usar, e o Azure cuida automaticamente da configuração e gerenciamento de todas as VMs e as unidades de disco necessárias para o serviço.
- Você pode acessar o serviço Blob, usando uma API REST ou usando uma linguagem de programação API. SDKs estão disponíveis para .NET, Java, Ruby e outras pessoas.
- Quando você armazena um arquivo no serviço Blob, você pode facilmente disponibilizá-lo publicamente na Internet.
- Você pode proteger arquivos no Blob do serviço para que eles possam acessado apenas por usuários autorizados, ou você pode fornecer tokens de acesso temporário que disponibiliza a alguém somente para um período de tempo limitado.

Sempre que você está compilando um aplicativo do Azure e desejar armazenar uma grande quantidade de dados que, em um ambiente local, iriam em arquivos--como imagens, vídeos, PDFs, planilhas, etc. – Considere o serviço Blob.

## <a name="creating-a-storage-account"></a>Criar uma conta de armazenamento

Para começar a usar com o serviço Blob, você cria uma conta de armazenamento no Azure. No portal, clique em **New** -- **serviços de dados** -- **armazenamento** -- **criação rápida**, e, em seguida, digite uma URL e um local de data center. O local de centro de dados deve ser o mesmo que seu aplicativo web.

![Criar uma conta de armazenamento](unstructured-blob-storage/_static/image1.png)

Você escolher a região primária, onde você deseja armazenar o conteúdo e, se você escolher o [replicação geográfica](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) opção, o Azure cria réplicas de todos os dados em um data center diferente em outra região do país. Por exemplo, se você escolher o oeste dos EUA data center, quando você armazena um arquivo que ele vai para o data center do Oeste dos EUA, mas em segundo plano do Azure também copia para um de outros data centers dos EUA. Se ocorrer um desastre em uma região do país, seus dados estão seguros ainda.

Azure não replica dados entre limites de geopolítica: se seu local primário estiver nos EUA, os arquivos só serão replicados para outra região dentro dos EUA; Se o local principal estiver na Austrália, os arquivos só serão replicados para outro data center na Austrália.

É claro, você também pode criar uma conta de armazenamento, executando os comandos de um script, como vimos anteriormente. Aqui está um comando do Windows PowerShell para criar uma conta de armazenamento:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Quando você tiver uma conta de armazenamento, você pode começar imediatamente armazenar arquivos no serviço Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Usando o armazenamento de BLOBs no aplicativo Fix It

O aplicativo Fix It permite que você carregue fotos.

![Criar uma tarefa Fix It](unstructured-blob-storage/_static/image2.png)

Quando você clica em **criar o FixIt**, o aplicativo carrega o arquivo de imagem especificado e armazena-o no serviço Blob.

### <a name="set-up-the-blob-container"></a>Configurar o contêiner de Blob

Para armazenar um arquivo no serviço Blob, você precisa de uma *contêiner* armazená-los em. Um contêiner de serviço Blob corresponde a uma pasta de sistema de arquivos. Os scripts de criação de ambiente que analisamos na [capítulo automatizar tudo](automate-everything.md) criar a conta de armazenamento, mas eles não criam um contêiner. Portanto, a finalidade do `CreateAndConfigure` método da `PhotoService` classe é criar um contêiner se ele ainda não existir. Esse método é chamado do `Application_Start` método no *global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

A chave de acesso e o nome de conta do armazenamento são armazenadas na `appSettings` coleção do *Web. config* do arquivo e o código no `StorageUtils.StorageAccount` método usa esses valores para construir uma cadeia de caracteres de conexão e estabelecer uma conexão:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

O `CreateAndConfigureAsync` método, em seguida, cria um objeto que representa o serviço Blob e um objeto que representa um contêiner (pasta) denominado "imagens" no serviço Blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Se um contêiner chamado "imagens" ainda não exista, que será true na primeira vez em que você executar o aplicativo em uma nova conta de armazenamento – o código cria o contêiner e define permissões para torná-lo público. (Por padrão, novos contêineres de blob são particulares e são acessíveis somente aos usuários que têm permissão para acessar sua conta de armazenamento.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Store a foto carregada no armazenamento de Blob

Para carregar e salvar o arquivo de imagem, o aplicativo usa um `IPhotoService` interface e uma implementação da interface no `PhotoService` classe. O *PhotoService.cs* arquivo contém todo o código no Fix It aplicativo que se comunica com o serviço Blob.

O seguinte método de controlador MVC é chamado quando o usuário clica **criar o FixIt**. Nesse código, `photoService` refere-se a uma instância das `PhotoService` classe, e `fixittask` refere-se a uma instância da `FixItTask` classe de entidade que armazena dados para uma nova tarefa.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

O `UploadPhotoAsync` método no `PhotoService` classe armazena o arquivo carregado no serviço Blob e retorna uma URL que aponta para o novo blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Como mostra a `CreateAndConfigure` método, o código conecta-se à conta de armazenamento e cria um objeto que representa o contêiner de blob "imagens", exceto aqui ele pressupõe que o contêiner já existir.

Em seguida, ele cria um identificador exclusivo para a imagem prestes a ser carregado, concatenando um novo valor GUID com a extensão de arquivo:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

O código, em seguida, usa o objeto de contêiner de blob e o novo identificador exclusivo para criar um objeto de blob, define um atributo no objeto indicando o tipo de arquivo ela está e, em seguida, usa o objeto de blob para armazenar o arquivo no armazenamento de BLOBs.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Por fim, ele obtém uma URL que faz referência ao blob. Essa URL será armazenado no banco de dados e pode ser usado em corrigir páginas da web para exibir a imagem carregada.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Essa URL é armazenada no banco de dados como uma das colunas da tabela FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Com apenas a URL no banco de dados e imagens no armazenamento de Blob, o aplicativo Fix It mantém o banco de dados pequeno, escalonável e de baixo custo, enquanto as imagens são armazenadas em que o armazenamento é barato e capaz de lidar com terabytes ou petabytes. Uma conta de armazenamento pode armazenar centenas de terabytes de fotos de corrigi-lo, e você paga apenas pelo que usar. Para que você possa começar pequeno centavos 9 pagantes para o primeiro gigabyte e adicionar mais imagens para centavos por gigabyte adicional.

### <a name="display-the-uploaded-file"></a>Exibir o arquivo carregado

O aplicativo Fix It exibe o arquivo de imagem carregada quando ela exibe detalhes de uma tarefa.

![Corrigir detalhes da tarefa com fotos](unstructured-blob-storage/_static/image3.png)

Para exibir a imagem, basta que o modo de exibição do MVC é incluir o `PhotoUrl` valor no HTML e enviada ao navegador. O servidor web e o banco de dados não estiver usando ciclos para exibir a imagem, eles são apenas serviços de alguns bytes para a URL da imagem. No código a seguir Razor, `Model` refere-se a uma instância da `FixItTask` classe de entidade.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Se você examinar o HTML da página que exibe, você verá a URL que aponta diretamente para a imagem no armazenamento de BLOBs, algo assim:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Resumo

Você viu como o aplicativo Fix It armazena imagens no serviço Blob e apenas as URLs de imagem no banco de dados SQL. Usando o serviço Blob mantém o banco de dados do SQL muito menor do que, caso contrário, seria, torna possível escalar verticalmente para um número quase ilimitado de tarefas e pode ser feito sem escrever uma grande quantidade de código.

Você pode ter centenas de terabytes em uma conta de armazenamento e o custo de armazenamento é muito menos dispendioso do que o armazenamento de banco de dados SQL, começando em cerca de 3 centavos por gigabyte por mês mais um encargo de pequenas transações. E lembre-se de que você não está pagando para a capacidade máxima, mas apenas para a quantidade que você realmente armazena, portanto, seu aplicativo está pronto para ser dimensionado, mas você não está pagando capacidade extra por tudo o que.

No [próximo capítulo](design-to-survive-failures.md) falaremos sobre a importância de tomar um aplicativo de nuvem capaz de lidar normalmente com falhas.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos:

- [Uma introdução ao armazenamento de BLOBs do Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog de Mike Wood.
- [Como usar o serviço de armazenamento de BLOBs do Azure no .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentação oficial no site MicrosoftAzure.com. Uma breve introdução ao seguido de exemplos de código mostrando como se conectar ao armazenamento de BLOBs do armazenamento de BLOBs criar contêineres, carregar e baixar blobs, etc.
- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, histórias desenhadas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Para uma discussão do serviço de armazenamento do Azure e blobs, consulte episódio 5 começando em 35:13.
- [Microsoft Patterns and Practices - diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Padrão de Valet Key consulte.

> [!div class="step-by-step"]
> [Anterior](data-partitioning-strategies.md)
> [Próximo](design-to-survive-failures.md)
