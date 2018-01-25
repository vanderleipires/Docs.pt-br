---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "Enviar dados de formulário HTML na API da Web do ASP.NET: com diversas partes MIME e o carregamento de arquivo | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Enviar dados de formulário HTML na API da Web do ASP.NET: com diversas partes MIME e o carregamento de arquivo
====================
por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Parte 2: Carregamento de arquivo e com diversas partes MIME

Este tutorial mostra como carregar arquivos para uma API da web. Ele também descreve como processar dados MIME de várias partes.

> [!NOTE]
> [Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Aqui está um exemplo de um formulário HTML para carregar um arquivo:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Este formulário contém um controle de entrada de texto e um controle de entrada de arquivo. Quando um formulário contém um controle de entrada de arquivo, o **enctype** atributo deve ser sempre &quot;com diversas partes/dados de formulário&quot;, que especifica que o formulário será enviado como uma mensagem MIME de várias partes.

O formato de uma mensagem MIME de várias partes é mais fácil entender examinando um exemplo de solicitação:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Esta mensagem é dividida em dois *partes*, uma para cada controle de formulário. Limites de parte são indicados por linhas que começam com traços.

> [!NOTE]
> O limite de parte inclui um componente aleatório (&quot;41184676334&quot;) para garantir que a cadeia de caracteres de limite não apareçam acidentalmente dentro de uma parte da mensagem.


Cada parte da mensagem contém um ou mais cabeçalhos, seguidos pelo conteúdo de parte.

- O cabeçalho Content-Disposition inclui o nome do controle. Para arquivos, ele também contém o nome do arquivo.
- O cabeçalho Content-Type descreve os dados na parte. Se esse cabeçalho for omitido, o padrão é texto/sem formatação.

No exemplo anterior, o usuário carregar um arquivo chamado GrandCanyon.jpg, com o tipo de conteúdo de imagem/jpeg; e o valor do texto de entrada foi &quot;férias de verão&quot;.

## <a name="file-upload"></a>Carregamento de arquivo

Agora vamos dar uma olhada em um controlador de API da Web que lê os arquivos de uma mensagem MIME de várias partes. O controlador lerá os arquivos de forma assíncrona. API da Web oferece suporte a ações assíncronas usando o [modelo de programação baseado em tarefa](https://msdn.microsoft.com/library/dd460693.aspx). Primeiro, aqui está o código se estiver direcionando o .NET Framework 4.5, que oferece suporte a **async** e **await** palavras-chave.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Observe que a ação de controlador não usa nenhum parâmetro. Isso ocorre porque podemos processar o corpo da solicitação em ação, sem invocar um formatador de tipo de mídia.

O **IsMultipartContent** método verifica se a solicitação contém uma mensagem MIME de várias partes. Caso contrário, o controlador retorna o código de status HTTP 415 (tipo de mídia sem suporte).

O **MultipartFormDataStreamProvider** classe é um objeto auxiliar que aloca fluxos de arquivos para arquivos carregados. Para ler a mensagem MIME de várias partes, chame o **ReadAsMultipartAsync** método. Esse método extrai todas as partes da mensagem e os grava em fluxos fornecidos pelo **MultipartFormDataStreamProvider**.

Quando o método é concluído, você pode obter informações sobre os arquivos a partir de **FileData** propriedade, que é uma coleção de **MultipartFileData** objetos.

- **MultipartFileData.FileName** é o nome de arquivo local no servidor, onde o arquivo foi salvo.
- **MultipartFileData.Headers** contém o cabeçalho de parte (*não* o cabeçalho de solicitação). Você pode usar isso para acessar o conteúdo\_cabeçalhos Content-Type e eliminação.

Como o nome sugere, **ReadAsMultipartAsync** é um método assíncrono. Para executar o trabalho depois que o método é concluído, use um [tarefa de continuação](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) ou o **await** palavra-chave (.NET 4.5).

Aqui está a versão do .NET Framework 4.0 do código anterior:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Leitura de dados de controle de formulário

O formulário HTML que mostrei anteriormente tinha um controle de entrada de texto.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Você pode obter o valor do controle do **FormData** propriedade o **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** é um **NameValueCollection** que contém pares de nome/valor para os controles de formulário. A coleção pode conter chaves duplicadas. Considere este formulário:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

O corpo da solicitação pode ter esta aparência:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

Nesse caso, o **FormData** coleção conterá os seguintes pares chave/valor:

- viagem: viagem
- opções: nonstop
- opções: datas
- estação: janela
