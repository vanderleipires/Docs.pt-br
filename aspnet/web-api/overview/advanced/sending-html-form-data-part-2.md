---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Enviar dados de formulário HTML na API Web ASP.NET: Upload de arquivo e com diversas partes MIME | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: adaa59b47cb16def5b423181ca06729d43cd77de
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804339"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Enviar dados de formulário HTML na API Web ASP.NET: Upload de arquivo e MIME com diversas partes
====================
por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Parte 2: Upload do arquivo e MIME com diversas partes

Este tutorial mostra como carregar arquivos para uma API da web. Ele também descreve como processar dados com várias partes de MIME.

> [!NOTE]
> [Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Aqui está um exemplo de um formulário HTML para carregar um arquivo:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Este formulário contém um controle de entrada de texto e um controle de entrada do arquivo. Quando um formulário contém um controle de entrada de arquivo, o **enctype** atributo deve ser sempre &quot;multipart/form-data&quot;, que especifica que o formulário será enviado como uma mensagem MIME de várias partes.

O formato de uma mensagem MIME de várias partes é mais fácil de entender examinando um exemplo de solicitação:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Esta mensagem é dividida em duas *partes*, uma para cada controle de formulário. Limites de parte são indicados por linhas que começam com traços.

> [!NOTE]
> O limite de parte inclui um componente aleatório (&quot;41184676334&quot;) para garantir que a cadeia de caracteres de limite não acidentalmente aparece dentro de uma parte da mensagem.


Cada parte da mensagem contém um ou mais cabeçalhos, seguidos pelo conteúdo de parte.

- O cabeçalho Content-Disposition inclui o nome do controle. Para arquivos, ele também contém o nome do arquivo.
- O cabeçalho Content-Type descreve os dados na parte. Se esse cabeçalho for omitido, o padrão é texto/simples.

No exemplo anterior, o usuário carregou um arquivo chamado GrandCanyon.jpg, com o tipo de conteúdo de imagem/jpeg; e o valor da entrada de texto era &quot;férias de verão&quot;.

## <a name="file-upload"></a>Upload de arquivo

Agora vamos dar uma olhada em um controlador de API da Web que lê arquivos de uma mensagem MIME de várias partes. O controlador lerá os arquivos de forma assíncrona. API da Web dá suporte a ações assíncronas usando o [modelo de programação baseado em tarefa](https://msdn.microsoft.com/library/dd460693.aspx). Primeiro, aqui está o código se você estiver direcionando o .NET Framework 4.5, que oferece suporte a **async** e **await** palavras-chave.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Observe que a ação do controlador não leva nenhum parâmetro. Isso ocorre porque podemos processar o corpo da solicitação dentro da ação, sem invocar um formatador de tipo de mídia.

O **IsMultipartContent** método verifica se a solicitação contém uma mensagem MIME de várias partes. Caso contrário, o controlador retorna o código de status HTTP 415 (tipo de mídia sem suporte).

O **MultipartFormDataStreamProvider** classe é um objeto auxiliar que aloca fluxos de arquivos para arquivos carregados. Para ler a mensagem MIME de várias partes, chame o **ReadAsMultipartAsync** método. Esse método extrai todas as partes da mensagem e grava-os em fluxos fornecidos pelo **MultipartFormDataStreamProvider**.

Quando o método for concluído, você pode obter informações sobre os arquivos a partir de **FileData** propriedade, que é uma coleção de **MultipartFileData** objetos.

- **MultipartFileData.FileName** é o nome de arquivo local no servidor, onde o arquivo foi salvo.
- **MultipartFileData.Headers** contém o cabeçalho da parte (*não* o cabeçalho de solicitação). Você pode usar isso para acessar o conteúdo\_cabeçalhos Disposition e Content-Type.

Como o nome sugere, **ReadAsMultipartAsync** é um método assíncrono. Para executar o trabalho depois que o método for concluído, use uma [tarefa de continuação](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) ou o **await** palavra-chave (.NET 4.5).

Aqui está a versão do .NET Framework 4.0 do código anterior:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Leitura de dados de controle de formulário

O formulário HTML que mostrei anteriormente tinha um controle de entrada de texto.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Você pode obter o valor do controle do **FormData** propriedade da **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** é um **NameValueCollection** que contém pares nome/valor para os controles de formulário. A coleção pode conter chaves duplicadas. Considere este formulário:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

O corpo da solicitação pode ter esta aparência:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

Nesse caso, o **FormData** coleção conteria os seguintes pares chave/valor:

- viagem: ida e volta
- opções: nonstop
- opções: datas
- estação: janela
