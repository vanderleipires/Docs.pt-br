---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: "Trabalhando com imagens em um Site do ASP.NET páginas da Web (Razor) | Microsoft Docs"
author: tfitzmac
description: "Este capítulo mostra como adicionar, exibir e manipular imagens (redimensionar, inverter e adicionar marcas d'água) no seu site."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 74838dd364a43f3f4c966c1417d0f0b2cc242f28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Trabalhando com imagens em um Site de páginas (Razor) da Web do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo mostra como adicionar, exibir e manipular imagens (redimensionar, inverter e adicionar marcas d'água) em um site de páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como adicionar uma imagem a uma página dinamicamente.
> - Como permitir que os usuários carregue uma imagem.
> - Como redimensionar uma imagem.
> - Como inverter ou girar uma imagem.
> - Como adicionar uma marca d'água a uma imagem.
> - Como usar uma imagem como uma marca d'água.
> 
> Esses são o ASP.NET recursos introduzidos no artigo de programação:
> 
> - O `WebImage` auxiliar.
> - O `Path` objeto, que fornece métodos que permitem a manipulação de nomes de arquivo e caminho.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - O WebMatrix 2
>   
> 
> Este tutorial também funciona com o WebMatrix 3.


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Adicionar uma imagem a uma página da Web dinamicamente

Você pode adicionar imagens ao seu site e para páginas individuais enquanto você estiver desenvolvendo o site. Você também pode permitir que os usuários carregar imagens, que podem ser úteis para tarefas, como permitir que eles adicionar uma foto de perfil.

Se uma imagem já está disponível em seu site e você quiser apenas para exibi-lo em uma página, use um HTML `<img>` elemento assim:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Às vezes, no entanto, você precisa ser capaz de exibir imagens dinamicamente &#8212; ou seja, você não sabe qual imagem para exibir até que a página está em execução.

O procedimento nesta seção mostra como exibir uma imagem em tempo real em que os usuários especificar o nome do arquivo de imagem de uma lista de nomes de imagem. Eles selecionem o nome da imagem de uma lista suspensa e quando eles enviam a página, a imagem que selecionado é exibida.

![[imagem] ] (9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. No WebMatrix, crie um novo site.
2. Adicionar uma nova página chamada *DynamicImage.cshtml*.
3. Na pasta raiz do site, adicione uma nova pasta e denomine- *imagens*.
4. Adicionar imagens de quatro a *imagens* na pasta recém-criada. (Todas as imagens tiverem será útil, mas eles devem caber em uma página.) Renomear as imagens *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, e *Photo4.jpg*. (Você não usará *Photo4.jpg* neste procedimento, mas você poderá usá-lo posteriormente neste artigo.)
5. Verifique se que as quatro imagens não são marcadas como somente leitura.
6. Substitua o conteúdo existente na página com o seguinte:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    O corpo da página tem uma lista suspensa (um `<select>` elemento) que é chamada `photoChoice`. A lista tem três opções e o `value` atributo de cada opção de lista tem o nome de uma das imagens que você colocou o *imagens* pasta. Essencialmente, a lista permite ao usuário selecionar um nome amigável como &quot;foto 1&quot;, e, em seguida, passa o *. jpg* nome de arquivo quando a página é enviada.

    No código, você pode obter a seleção do usuário (em outras palavras, o nome do arquivo de imagem) da lista lendo `Request["photoChoice"]`. Primeiro, você verá se há uma seleção em todos os. Se houver, você pode construir um caminho para a imagem que consiste do nome da pasta para as imagens e o nome do arquivo de imagem do usuário. (Se você tentou criar um caminho, mas não havia nada no `Request["photoChoice"]`, você receberia um erro.) Isso resulta em um caminho relativo, como este:

    *imagens/Photo1.jpg*

    O caminho é armazenado na variável nomeada `imagePath` que você vai precisar posteriormente na página.

    No corpo, também há um `<img>` elemento que é usado para exibir a imagem escolhida pelo usuário. O `src` atributo não está definido como um nome de arquivo ou URL, como faria para exibir um elemento estático. Em vez disso, ele é definido como `@imagePath`, que significa que ele obtém seu valor do caminho definido no código.

    Na primeira vez que a página é executada, no entanto, não há nenhuma imagem para exibir, porque o usuário não selecionou nada. Isso normalmente significa que o `src` atributo deve estar vazio e a imagem deve aparecer como um vermelho &quot;x&quot; (ou qualquer navegador renderiza quando não é possível localizar uma imagem). Para evitar isso, você deve colocar o `<img>` elemento em um `if` bloco verifica se o `imagePath` variável tiver algo nele. Se o usuário fez uma seleção, `imagePath` contém o caminho. Se o usuário não escolher uma imagem ou se esta for a primeira vez que a página é exibida, o `<img>` elemento ainda não está processado.
7. Salve o arquivo e execute a página em um navegador. (Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.)
8. Selecione uma imagem na lista suspensa e, em seguida, clique em **imagem de exemplo**. Certifique-se de que você verá diferentes imagens para diferentes opções.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Carregando uma imagem

O exemplo anterior mostrou como exibir uma imagem dinamicamente, mas ela funcionou apenas com imagens que já estavam em seu site. Este procedimento mostra como permitir que os usuários carregue uma imagem, que é exibida na página. No ASP.NET, você pode manipular imagens em tempo real usando o `WebImage` auxiliar, que tem métodos que permitem a você cria, manipular e salva imagens. O `WebImage` auxiliar dá suporte a todas as comuns da web imagem tipos de arquivo, incluindo *. jpg*, *. PNG*, e *. bmp*. Neste artigo, você usará *. jpg* imagens, mas você pode usar qualquer um dos tipos de imagem.

![[imagem] ] (9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Adicione uma nova página e denomine- *UploadImage.cshtml*.
2. Substitua o conteúdo existente na página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    O corpo do texto tem uma `<input type="file">` elemento, que permite aos usuários selecionar um arquivo para carregar. Quando eles clicarem **enviar**, o arquivo eles separado é enviado junto com o formulário.

    Para obter a imagem carregada, você deve usar o `WebImage` auxiliar, que tem todos os tipos de métodos úteis para trabalhar com imagens. Especificamente, você usa `WebImage.GetImageFromRequest` para obter a imagem carregada (se houver) e armazená-lo em uma variável chamada `photo`.

    Muito trabalho neste exemplo envolve obter e definir os nomes de arquivo e caminho. O problema é que você deseja obter o nome (e apenas o nome) da imagem que carregou o usuário e, em seguida, crie um novo caminho para onde você pretende armazenar a imagem. Como os usuários potencialmente podem carregar várias imagens que têm o mesmo nome, você usa um pouco de código adicional para criar nomes exclusivos e certifique-se de que os usuários não substitua imagens existentes.

    Se uma imagem, na verdade, foi carregada (o teste `if (photo != null)`), você obtém o nome da imagem da imagem do `FileName` propriedade. Quando o usuário carrega a imagem, `FileName` contém o nome do usuário original, que inclui o caminho do computador do usuário. Ele pode parecer com isso:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Você não deseja que todas essas informações de caminho, embora &#8212; você quiser apenas o nome do arquivo real (*SamplePhoto1.jpg*). Você pode extrair apenas o arquivo de um caminho usando o `Path.GetFileName` método, como este:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Crie um novo nome de arquivo exclusivo com a adição de um GUID para o nome original. (Para obter mais informações sobre GUIDs, consulte [sobre GUIDs](#SB_AboutGUIDs) posteriormente neste artigo.) Em seguida, você pode construir um caminho completo que você pode usar para salvar a imagem. Salvar caminho é composto do novo nome de arquivo, a pasta (imagens) e o local atual do site.

    > [!NOTE]
    > Para que seu código para salvar arquivos de *imagens* pasta, o aplicativo precisa de leitura-gravação permissões para essa pasta. No computador de desenvolvimento isso não é geralmente um problema. No entanto, quando você publica seu site ao servidor da web de um provedor de hospedagem, você precisará definir explicitamente as permissões. Se você executa esse código no servidor de um provedor de hospedagem e obter erros, verifique com o provedor de hospedagem para saber como definir essas permissões.

    Finalmente, você passa a salvar caminho para o `Save` método o `WebImage` auxiliar. Armazena a imagem carregada com o novo nome. Salvar método tem esta aparência: `photo.Save(@"~\" + imagePath)`. O caminho completo é acrescentado ao `@"~\"`, que é o local atual do site. (Para obter informações sobre o `~` operador, consulte [Introdução ao ASP.NET Web programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Como no exemplo anterior, o corpo da página contém um `<img>` elemento para exibir a imagem. Se `imagePath` tiver sido definido, o `<img>` elemento é processado e seu `src` atributo é definido como o `imagePath` valor.
3. Execute a página em um navegador.
4. Carregue uma imagem e verifique se que ele é exibido na página.
5. No seu site, abra o *imagens* pasta. Você vê que um novo arquivo foi adicionado cujo nome de arquivo pode ter esta aparência: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Esta é a imagem que você carregou com um GUID que o prefixo para o nome. (Seu próprio arquivo terá um GUID diferente e provavelmente é denominado algo diferente de *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Sobre GUIDs
> 
> Um GUID (ID exclusivo global) é um identificador que normalmente é processado em um formato como este: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Os números e letras (da A F) são diferentes para cada GUID, mas todas elas seguem o padrão do uso de grupos de caracteres de 8-4-4-4-12. (Tecnicamente, um GUID é um número de 16 bytes/128 bits.) Quando você precisar de um GUID, você pode chamar código especializado que gera um GUID para você. A ideia GUIDs é que, entre o tamanho gigantesco do número (3,4 x 10<sup>38</sup>) e o algoritmo para gerá-lo, o número resultante é praticamente garantido para ser um tipo. GUIDs, portanto, são uma boa maneira de gerar nomes de itens quando você deve assegurar que você não use o mesmo nome duas vezes. A desvantagem, é claro, GUIDs não são particularmente amigável, portanto eles tendem a ser usado quando o nome é usado somente no código.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Redimensionando uma imagem

Se seu site aceita imagens de um usuário, você talvez queira redimensionar as imagens antes de exibir ou salvá-los. Você pode usar novamente o `WebImage` auxiliar para isso.

Este procedimento mostra como redimensionar uma imagem carregada para criar uma miniatura e, em seguida, salvar a miniatura e a imagem original no site. Exibir a miniatura na página e use um hiperlink para redirecionar os usuários para a imagem em tamanho normal.

![[imagem] ] (9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Adicionar uma nova página chamada *Thumbnail.cshtml*.
2. No *imagens* pasta, crie uma subpasta chamada *miniaturas*.
3. Substitua o conteúdo existente na página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Esse código é semelhante ao código do exemplo anterior. A diferença é que esse código salva a imagem duas vezes, uma vez normalmente e uma vez depois de criar uma cópia da imagem. Primeiro você pode obter a imagem carregada e salvá-lo no *imagens* pasta. Você, em seguida, criar um novo caminho para a imagem em miniatura. Para realmente criar a miniatura, chame o `WebImage` do auxiliar `Resize` método para criar uma imagem de 60-por 60 pixels. O exemplo mostra como você pode preservar a taxa de proporção e como você pode impedir que a imagem que está sendo ampliada (no caso do novo tamanho seria, na verdade, aumentar a imagem). A imagem redimensionada é salvo no *miniaturas* subpasta.

    No final da marcação, use o mesmo `<img>` elemento com dinâmico `src` atributo que você viu nos exemplos anteriores para mostrar condicionalmente a imagem. Nesse caso, você pode exibir a miniatura. Você também usar uma `<a>` elemento para criar um hiperlink para a versão grande da imagem. Assim como acontece com o `src` atributo do `<img>` elemento, defina o `href` atributo do `<a>` elemento dinamicamente ao que está no `imagePath`. Para certificar-se de que o caminho pode funcionar como uma URL, você passa `imagePath` para o `Html.AttributeEncode` método, que converte caracteres reservados no caminho em caracteres que são okey em uma URL.
4. Execute a página em um navegador.
5. Carregar uma foto e verifique se a miniatura é mostrada.
6. Clique na miniatura para ver a imagem em tamanho normal.
7. No *imagens* e *imagens/miniaturas*, observe que os novos arquivos foram adicionados.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Girando e invertendo uma imagem

O `WebImage` auxiliar também lhe permite inverter e girar imagens. Este procedimento mostra como obter uma imagem do servidor, inverter a imagem de cabeça para baixo (vertical), salve-o e, em seguida, exiba a imagem invertida na página. Neste exemplo, você estiver usando apenas um arquivo que você já tem no servidor (*Photo2.jpg*). Em um aplicativo real, você provavelmente seria inverte uma imagem cujo nome você obter dinamicamente, como você fez nos exemplos anteriores.

![[imagem] ] (9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Adicionar uma nova página chamada *FlipImage.cshtml*.
2. Substitua o conteúdo existente na página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    O código usa o `WebImage` auxiliar para obter uma imagem do servidor. Criar o caminho para a imagem usando a mesma técnica usada nos exemplos anteriores para salvar imagens e passar esse caminho ao criar uma imagem usando `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Se for encontrada uma imagem, você pode construir um novo caminho e nome, como você fez nos exemplos anteriores. Para inverter a imagem, você deve chamar o `FlipVertical` método e, em seguida, salvá-la novamente.

    A imagem é exibida na página novamente usando o `<img>` elemento com o `src` atributo definido como `imagePath`.
3. Execute a página em um navegador. A imagem para *Photo2.jpg* é mostrado cabeça para baixo.
4. Atualize a página ou solicitar a página novamente para ver que a imagem é invertida direita backup novamente.

Para girar uma imagem, você usa o mesmo código, exceto que, em vez de chamar o `FlipVertical` ou `FlipHorizontal`, chamar `RotateLeft` ou `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Adicionar uma marca d'água a uma imagem

Quando você adicionar imagens ao seu site, você talvez queira adicionar uma marca d'água à imagem antes de salvá-lo ou exibi-lo em uma página. As pessoas geralmente usam marcas d'água para adicionar informações de copyright a uma imagem ou anunciar seu nome de negócios.

![[imagem] ] (9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Adicionar uma nova página chamada *Watermark.cshtml*.
2. Substitua o conteúdo existente na página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Esse código é como o código a *FlipImage.cshtml* página anteriores (embora desta vez usa o *Photo3.jpg* arquivo). Para adicionar a marca d'água, você deve chamar o `WebImage` do auxiliar `AddTextWatermark` método antes de salvar a imagem. Na chamada para `AddTextWatermark`, passar o texto &quot;marca d'água meu&quot;, defina a cor da fonte para amarelo e definir a família de fontes para Arial. (Embora não seja mostrado aqui, o `WebImage` auxiliar também permite que você especifique opacidade, família de fontes e tamanho da fonte e a posição do texto de marca d'água.) Quando você salvar a imagem não deve ser somente leitura.

    Como você viu antes, a imagem é exibida na página usando o `<img>` elemento com o atributo src definido como `@imagePath`.
3. Execute a página em um navegador. Observe o texto "Meus marca d'água" no canto inferior direito da imagem.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Usando uma imagem como uma marca d'água

Em vez de usar o texto de marca d'água, você pode usar outra imagem. As pessoas às vezes usam imagens como um logotipo da empresa como uma marca d'água ou usar uma imagem de marca d'água em vez de texto para obter informações de direitos autorais.

![[imagem] ] (9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Adicionar uma nova página chamada *ImageWatermark.cshtml*.
2. Adicionar uma imagem para o *imagens* pasta que você pode usar como um logotipo e renomeie a imagem *MyCompanyLogo.jpg*. Essa imagem deve ser uma imagem que você pode ver claramente quando ele é definido como 80 pixels de largura e 20 pixels de altura.
3. Substitua o conteúdo existente na página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Essa é uma variação no código dos exemplos anteriores. Nesse caso, você chama `AddImageWatermark` para adicionar a imagem de marca d'água à imagem de destino (*Photo3.jpg*) antes de salvar a imagem. Quando você chama `AddImageWatermark`, defina-a para 80 pixels e a altura como 20 pixels. O *MyCompanyLogo.jpg* imagem é alinhada horizontalmente no centro e alinhada verticalmente na parte inferior da imagem de destino. A opacidade é definida como 100% e o preenchimento é definido para 10 pixels. Se a imagem de marca d'água é maior do que a imagem de destino, nada acontecerá. Se a imagem de marca d'água é maior do que a imagem de destino e definir o preenchimento da marca d'água de imagem como zero, a marca d'água será ignorada.

    Como antes, você exibe a imagem usando o `<img>` elemento e um dinâmico `src` atributo.
4. Execute a página em um navegador. Observe que a imagem de marca d'água aparece na parte inferior da imagem principal.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Trabalhando com arquivos em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introdução a páginas da Web ASP.NET usando a sintaxe do Razor de programação](https://go.microsoft.com/fwlink/?LinkID=251587)
