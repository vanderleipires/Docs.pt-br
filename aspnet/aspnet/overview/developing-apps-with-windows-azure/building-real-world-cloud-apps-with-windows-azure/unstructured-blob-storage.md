---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: "Armazenamento de Blob não estruturados (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs"
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 6cb77e8ef301c2eeef7df3e391e14f4e2c0364e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Armazenamento de Blob não estruturados (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


No capítulo anterior, examinamos os esquemas de particionamento e explicou como o aplicativo corrigir armazena imagens no serviço de Blob de armazenamento do Azure e outros dados de tarefa no banco de dados do SQL Azure. Neste capítulo aprofundar-se no serviço Blob e mostrar como ele é implementado no código do projeto corrigi-lo.

## <a name="what-is-blob-storage"></a>O que é o armazenamento de Blob?

O serviço de Blob de armazenamento do Azure fornece uma maneira de armazenar arquivos na nuvem. O serviço Blob tem um número de vantagens em relação ao armazenamento de arquivos em um sistema de arquivos de rede local:

- É altamente dimensionável. Uma única conta de armazenamento pode armazenar [centenas de terabytes](https://msdn.microsoft.com/en-us/library/windowsazure/dn249410.aspx), e você pode ter várias contas de armazenamento. Alguns dos clientes do Azure maiores armazenam centenas de petabytes. Microsoft SkyDrive usa o armazenamento de blob.
- É durável. Todos os arquivos do que repositório no serviço Blob é feito automaticamente.
- Ele fornece alta disponibilidade. O [SLA para armazenamento](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promessas 99,9% ou 99,99% tempo de atividade, dependendo da opção de redundância geográfica que você escolher.
- É um recurso (PaaS) de plataforma como serviço do Azure, o que significa que você acabou de armazena e recupera arquivos, pagar apenas para a quantidade real de armazenamento que você usar, e o Azure automaticamente cuidará de configurar e gerenciar todas as VMs e unidades de disco necessárias para o serviço.
- Você pode acessar o serviço Blob usando uma API REST ou usando uma linguagem de programação API. SDKs estão disponíveis para .NET, Java, Ruby e outros.
- Quando você armazena um arquivo no serviço Blob, você pode facilmente tornar publicamente disponível pela Internet.
- Você pode proteger arquivos no Blob de serviço para que eles possam acessar somente por usuários autorizados, ou você pode fornecer tokens de acesso temporário que disponibiliza a alguém apenas para um período de tempo limitado.

Sempre que você estiver criando um aplicativo do Azure e deseja armazenar uma grande quantidade de dados que entrarão em arquivos – como imagens, vídeos, em um ambiente local PDFs, planilhas, etc. – Considere o serviço Blob.

## <a name="creating-a-storage-account"></a>Criar uma conta de armazenamento

Para começar a usar o serviço Blob, você criar uma conta de armazenamento no Azure. No portal, clique em **novo** -- **Data Services** -- **armazenamento** -- **criação rápida**, e, em seguida, digite uma URL e um local de centro de dados. O local de centro de dados deve ser o mesmo que seu aplicativo web.

![Criar uma conta de armazenamento](unstructured-blob-storage/_static/image1.png)

Escolha a região principal onde você deseja armazenar o conteúdo e, se você escolher o [georeplicação](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) opção, o Azure cria réplicas de todos os dados em um data center diferente em outra região do país. Por exemplo, se você escolher o Centro de dados Oeste dos EUA, quando você armazena um arquivo que ele passa para o data center Oeste dos EUA, mas no plano de fundo do Azure também copiá-lo para um dos outros centros de dados de Estados Unidos. Se ocorrer um desastre em uma região do país, seus dados serão ainda seguros.

Azure não replicar dados entre limites de políticas de replicação geográfica: se o local principal estiver nos EUA, os arquivos só serão replicados para outra região dentro dos EUA; Se o local primário for Austrália, os arquivos só serão replicados para outro data center na Austrália.

Naturalmente, você também pode criar uma conta de armazenamento executando comandos de um script, como visto anteriormente. Aqui está um comando do Windows PowerShell para criar uma conta de armazenamento:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Depois que você tiver uma conta de armazenamento, você pode começar imediatamente armazenando arquivos no serviço Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Usando o armazenamento de Blob no aplicativo corrigir

O aplicativo corrigir permite que você carregue as fotos.

![Criar uma tarefa corrigir](unstructured-blob-storage/_static/image2.png)

Quando você clica em **criar o FixIt**, o aplicativo carrega o arquivo de imagem especificado e o armazena no serviço Blob.

### <a name="set-up-the-blob-container"></a>Configurar o contêiner de Blob

Para armazenar um arquivo no serviço Blob é necessário um *contêiner* para armazená-los em. Um contêiner de serviço Blob corresponde a uma pasta do sistema de arquivos. Os scripts de criação do ambiente, examinamos no [capítulo automatizar tudo](automate-everything.md) criar a conta de armazenamento, mas eles não criarem um contêiner. Para a finalidade do `CreateAndConfigure` método do `PhotoService` classe é criar um contêiner se ele ainda não existir. Este método é chamado da `Application_Start` método *global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

A chave de acesso e do nome de conta do armazenamento são armazenados no `appSettings` coleção do *Web. config* de arquivo e código no `StorageUtils.StorageAccount` método usa esses valores para construir uma cadeia de caracteres de conexão e estabelecer uma conexão:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

O `CreateAndConfigureAsync` método cria um objeto que representa o serviço Blob, e um objeto que representa um contêiner (pasta) denominada "imagens" no serviço Blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Se um contêiner denominado "imagens" ainda não existe - que será true na primeira vez que você executar o aplicativo em uma nova conta de armazenamento – o código cria o contêiner e define permissões para torná-lo público. (Por padrão, novos contêineres de blob são particulares e são acessíveis somente a usuários que têm permissão para acessar sua conta de armazenamento.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Armazenar a foto carregada no armazenamento de Blob

Para carregar e salvar o arquivo de imagem, o aplicativo usa um `IPhotoService` interface e uma implementação da interface no `PhotoService` classe. O *PhotoService.cs* arquivo contém todo o código no corrigir aplicativo que se comunica com o serviço Blob.

O seguinte método do controlador MVC é chamado quando o usuário clica **criar o FixIt**. Nesse código, `photoService` refere-se a uma instância do `PhotoService` classe, e `fixittask` refere-se a uma instância do `FixItTask` classe da entidade que armazena dados para uma nova tarefa.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

O `UploadPhotoAsync` método o `PhotoService` classe armazena o arquivo carregado no serviço Blob e retorna uma URL que aponta para o novo blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Como no `CreateAndConfigure` método, o código conecta-se à conta de armazenamento e cria um objeto que representa o contêiner de blob "imagens", exceto aqui, ele assume o contêiner já existe.

Em seguida, ele cria um identificador exclusivo para a imagem prestes a ser carregada, concatenando um novo valor GUID com a extensão de arquivo:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

O código, em seguida, usa o objeto de contêiner de blob e o novo identificador exclusivo para criar um objeto de blob, define um atributo no objeto que indica o tipo de arquivo ele é e, em seguida, usa o objeto de blob para armazenar o arquivo no armazenamento de blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Finalmente, ele obtém uma URL que faz referência ao blob. Essa URL será armazenado no banco de dados e pode ser usado em corrigir páginas da web para exibir a imagem carregada.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Essa URL é armazenado no banco de dados como uma das colunas da tabela FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Com apenas a URL no banco de dados e imagens no armazenamento de Blob, o aplicativo corrigir mantém o banco de dados pequeno, escalonável e de baixo custo, enquanto as imagens são armazenadas onde o armazenamento é capaz de lidar com terabytes ou petabytes e econômica. Uma conta de armazenamento pode armazenar centenas de terabytes de corrigir fotos e você só paga pelo que usa. Para que você pode começar pequenos centavos 9 pagantes para o primeiro gigabyte e adicionar mais imagens para centavos por gigabyte adicional.

### <a name="display-the-uploaded-file"></a>Exibir o arquivo carregado

O aplicativo corrigir exibe o arquivo de imagem carregada quando ele exibe os detalhes de uma tarefa.

![Corrija-detalhes da tarefa com foto](unstructured-blob-storage/_static/image3.png)

Para exibir a imagem, basta que o modo de exibição do MVC é incluir o `PhotoUrl` valor no HTML e enviada para o navegador. O servidor web e o banco de dados não estiver usando ciclos para exibir a imagem, eles são apenas serviços de alguns bytes para a URL da imagem. No código a seguir Razor, `Model` refere-se a uma instância do `FixItTask` classe da entidade.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Se você examinar o código HTML da página que exibe, você verá a URL aponta diretamente para a imagem no armazenamento de blob, semelhante a este:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Resumo

Você viu como o aplicativo corrigir armazena imagens no serviço Blob e apenas as URLs de imagem no banco de dados SQL. Usando o serviço Blob mantém o banco de dados SQL bem menor do que de outra forma seria, possibilita a escala até um número quase ilimitado de tarefas e pode ser feito sem escrever tanto código.

Você pode ter centenas de terabytes em uma conta de armazenamento e o custo de armazenamento é muito menos dispendioso do que o armazenamento de banco de dados SQL, iniciando em aproximadamente 3 centavos por gigabyte por mês mais uma taxa de transações menor. E lembre-se que você não está pagando para a capacidade máxima, mas apenas para o valor que você realmente armazena, portanto, mas você não prestar capacidade extra por tudo o que seu aplicativo está pronto para dimensionar.

No [próximo capítulo](design-to-survive-failures.md) falaremos sobre a importância de criação de um aplicativo de nuvem pode lidar normalmente com falhas.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos:

- [Uma introdução ao armazenamento de BLOBs do Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog de Mike madeira.
- [Como usar o serviço de armazenamento de Blob do Azure no .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentação oficial sobre o site MicrosoftAzure.com. Uma breve introdução ao blob de armazenamento seguido de exemplos de código mostrando como se conectar ao armazenamento de blob, criar contêineres, carregar e baixar blobs, etc.
- [À prova de falhas: Criação de serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta os conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, com histórias extraídas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Para uma discussão sobre o serviço de armazenamento do Azure e blobs, consulte episódio 5 começando em 35:13.
- [Padrões e práticas - diretrizes do Azure Microsoft](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Consulte manobrista chave padrão.

>[!div class="step-by-step"]
[Anterior](data-partitioning-strategies.md)
[Próximo](design-to-survive-failures.md)
