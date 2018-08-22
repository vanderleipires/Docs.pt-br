---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Trabalhando com imagens em um Site do ASP.NET Web Pages (Razor) | Microsoft Docs
author: tfitzmac
description: Este capítulo mostra como adicionar, exibir e manipular imagens (redimensionar, inverter e adicionar marcas d'água) no seu site.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: e609cd1c6ab74b5b40d28bde353501dbacb5d544
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824386"
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Trabalhando com imagens em um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo mostra como adicionar, exibir e manipular imagens (redimensionar, inverter e adicionar marcas d'água) em um site de páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como adicionar uma imagem para uma página dinamicamente.
> - Como permitir que os usuários carregar uma imagem.
> - Como redimensionar uma imagem.
> - Como inverter ou girar uma imagem.
> - Como adicionar uma marca d'água a uma imagem.
> - Como usar uma imagem como uma marca d'água.
> 
> Estes são os recursos introduzidos no artigo de programação do ASP.NET:
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

Você pode adicionar imagens ao seu site e para páginas individuais, enquanto você estiver desenvolvendo o site. Você também pode permitir que os usuários carregar imagens, que podem ser úteis para tarefas como permitir que eles adicionar uma foto de perfil.

Se uma imagem já está disponível em seu site e você deseja apenas para exibi-lo em uma página, você use um HTML `<img>` elemento como este:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Às vezes, no entanto, você precisa ser capaz de exibir imagens dinamicamente &#8212; ou seja, você não souber o que a imagem para exibir até que a página está em execução.

O procedimento nesta seção mostra como exibir uma imagem em tempo real em que os usuários especificar o nome do arquivo de imagem de uma lista de nomes de imagem. Eles selecionam o nome da imagem de uma lista suspensa e, ao enviar a página, a imagem selecionada é exibida.

![[imagem]](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. No WebMatrix, crie um novo site.
2. Adicionar uma nova página chamada *DynamicImage.cshtml*.
3. Na pasta raiz do site, adicione uma nova pasta e nomeie- *imagens*.
4. Adicione quatro imagens para o *imagens* pasta recém-criada. (Todas as imagens você tem será útil fazer, mas eles devem se ajustar em uma página). Renomear as imagens *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, e *Photo4.jpg*. (Você não usará *Photo4.jpg* neste procedimento, mas você vai usá-lo posteriormente neste artigo.)
5. Verifique se que as quatro imagens não são marcadas como somente leitura.
6. Substitua o conteúdo existente no página com o seguinte:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    O corpo da página tem uma lista suspensa (um `<select>` elemento) que é chamada `photoChoice`. A lista tem três opções e o `value` atributo de cada opção de lista tem o nome de uma das imagens que você colocar nas *imagens* pasta. Essencialmente, a lista permite ao usuário selecionar um nome amigável, como &quot;Photo 1&quot;, e, em seguida, passa a *jpg* nome do arquivo quando a página é enviada.

    No código, você pode obter a seleção do usuário (em outras palavras, o nome do arquivo de imagem) da lista lendo `Request["photoChoice"]`. Você primeiro consulte se há uma seleção em todos os. Se houver, você pode construir um caminho para a imagem que consiste o nome da pasta para as imagens e o nome do arquivo de imagem do usuário. (Se você tentou criar um caminho, mas não havia nada no `Request["photoChoice"]`, você obterá um erro.) Isso resulta em um caminho relativo, como este:

    *imagens/Photo1.jpg*

    O caminho é armazenado na variável chamada `imagePath` que você vai precisar posteriormente na página.

    No corpo, há também um `<img>` elemento que é usado para exibir a imagem escolhida pelo usuário. O `src` atributo não está definido como um nome de arquivo ou uma URL, como você faria para exibir um elemento estático. Em vez disso, ele é definido como `@imagePath`, que significa que ele obtém seu valor do caminho definidos no código.

    Na primeira vez que a página é executada, no entanto, não há nenhuma imagem a ser exibida, porque o usuário não tenha selecionado qualquer coisa. Isso normalmente significa que o `src` atributo estariam vazio e a imagem deve ser mostrado como um vermelho &quot;x&quot; (ou seja qual for o navegador renderiza quando não conseguir encontrar uma imagem). Para evitar isso, você coloca o `<img>` elemento em um `if` bloco que testa para ver se o `imagePath` variável tiver algo em. Se o usuário fez uma seleção, `imagePath` contém o caminho. Se o usuário não tenha selecionado uma imagem ou se esta for a primeira vez em que a página é exibida, o `<img>` elemento ainda não é renderizado.
7. Salve o arquivo e execute a página em um navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.)
8. Selecione uma imagem na lista suspensa e, em seguida, clique em **imagem de exemplo**. Certifique-se de que você vê imagens diferentes para diferentes opções.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Carregar uma imagem

O exemplo anterior mostrou como exibir uma imagem dinamicamente, mas ele funcionava apenas com imagens que já estavam em seu site. Este procedimento mostra como permitir que os usuários carregar uma imagem, que é exibida na página. No ASP.NET, você pode manipular imagens em tempo real usando o `WebImage` auxiliar, que tem métodos que permitem a você cria, manipular e salva as imagens. O `WebImage` auxiliar dá suporte a todas as comuns da web imagem tipos de arquivo, incluindo *. jpg*, *PNG*, e *bmp*. Ao longo deste artigo, você usará *. jpg* imagens, mas você pode usar qualquer um dos tipos de imagem.

![[imagem]](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Adicione uma nova página e nomeie- *UploadImage.cshtml*.
2. Substitua o conteúdo existente no página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    O corpo do texto tem um `<input type="file">` elemento, que permite aos usuários selecionar um arquivo para carregar. Ao clicar **enviar**, o arquivo eles selecionados é enviado junto com o formulário.

    Para obter a imagem carregada, você deve usar o `WebImage` auxiliar, que tem todos os tipos de métodos úteis para trabalhar com imagens. Especificamente, você usa `WebImage.GetImageFromRequest` para obter a imagem carregada (se houver) e armazená-lo em uma variável chamada `photo`.

    Uma grande parte do trabalho neste exemplo envolve a obtenção e definição de nomes de arquivo e caminho. O problema é que você deseja obter o nome (e apenas o nome) da imagem que carregou o usuário e, em seguida, crie um novo caminho para onde você pretende armazenar a imagem. Como os usuários potencialmente poderiam carregar várias imagens que têm o mesmo nome, você usa um pouco de código extra para criar nomes exclusivos e certifique-se de que os usuários não substitua imagens existentes.

    Se uma imagem, na verdade, foi carregada (o teste `if (photo != null)`), você obtém o nome da imagem da imagem do `FileName` propriedade. Quando o usuário carrega a imagem, `FileName` contém o nome do usuário original, que inclui o caminho do computador do usuário. Ela teria esta aparência:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Você não deseja que todas essas informações de caminho, embora &#8212; você deseja apenas o nome do arquivo real (*SamplePhoto1.jpg*). Você pode remover apenas o arquivo de um caminho usando o `Path.GetFileName` método, como este:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Você, em seguida, crie um novo nome de arquivo exclusivo adicionando um GUID para o nome original. (Para obter mais informações sobre GUIDs, consulte [sobre GUIDs](#SB_AboutGUIDs) mais adiante neste artigo.) Em seguida, você pode construir um caminho completo que você pode usar para salvar a imagem. Salvar caminho é composto de um novo nome de arquivo, a pasta (imagens) e o local do site atual.

    > [!NOTE]
    > Para seu código para salvar arquivos em que o *imagens* pasta, o aplicativo precisa de permissões de leitura-gravação para aquela pasta. No computador de desenvolvimento isso não é normalmente um problema. No entanto, quando você publica seu site para o servidor web de um provedor de hospedagem, você talvez precise definir explicitamente essas permissões. Se você executa esse código no servidor de um provedor de hospedagem e receber mensagens de erro, verifique com o provedor de hospedagem para saber como definir essas permissões.

    Finalmente, você passa o salvamento caminho para o `Save` método da `WebImage` auxiliar. Isso armazena a imagem carregada com o novo nome. Salvar método tem esta aparência: `photo.Save(@"~\" + imagePath)`. O caminho completo é acrescentado ao `@"~\"`, que é o local do site atual. (Para obter informações sobre o `~` operador, consulte [Introdução ao ASP.NET Web de programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Como no exemplo anterior, o corpo da página contém um `<img>` elemento para exibir a imagem. Se `imagePath` tiver sido definido, o `<img>` elemento é processado e seu `src` atributo é definido como o `imagePath` valor.
3. Execute a página em um navegador.
4. Carregar uma imagem e verifique se que ele é exibido na página.
5. Em seu site, abra o *imagens* pasta. Você verá que um novo arquivo foi adicionado, cujo nome de arquivo pode ter esta aparência: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Esta é a imagem que você carregou com um GUID como o nome do prefixo. (Seu próprio arquivo terá um GUID diferente e provavelmente é chamado algo diferente de *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>GUIDs
> 
> Um GUID (identificador globalmente exclusivo) é um identificador que geralmente é renderizado em um formato como este: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Os números e letras (de A-F) são diferentes para cada GUID, mas todos eles seguem o padrão de uso de grupos de 8-4-4-4-12 caracteres. (Tecnicamente, um GUID é um número de 16 bytes/128 bits). Quando você precisar de um GUID, você pode chamar código especializado que gera um GUID para você. A ideia por trás de GUIDs é que entre o tamanho considerável do número (3,4 x 10<sup>38</sup>) e o algoritmo para gerá-lo, o número resultante é praticamente garantido para ser um de um tipo. GUIDs, portanto, são uma boa maneira de gerar nomes para as coisas quando você deve garantir que você não use o mesmo nome duas vezes. A desvantagem, claro, é que GUIDs não são particularmente amigável, então, eles tendem a ser usado quando o nome é usado apenas em código.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Redimensionando uma imagem

Se seu site aceitar imagens de um usuário, você talvez queira redimensionar as imagens antes de exibir ou salvá-los. Você pode usar novamente o `WebImage` auxiliar para isso.

Este procedimento mostra como redimensionar uma imagem carregada para criar uma miniatura e, em seguida, salvar a miniatura e a imagem original no site. A miniatura são exibidas na página e use um hiperlink para redirecionar usuários para a imagem em tamanho normal.

![[imagem]](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Adicionar uma nova página chamada *Thumbnail.cshtml*.
2. No *imagens* pasta, crie uma subpasta chamada *polegares*.
3. Substitua o conteúdo existente no página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Esse código é semelhante ao código do exemplo anterior. A diferença é que esse código salva a imagem duas vezes, uma vez normalmente e uma vez depois de criar uma cópia em miniatura da imagem. Primeiro você pode obter a imagem carregada e salve-o na *imagens* pasta. Você, em seguida, constrói um novo caminho para a imagem em miniatura. Para realmente criar a miniatura, chame o `WebImage` do auxiliar `Resize` método para criar uma imagem de pixel de 60 por 60 pixels. O exemplo mostra como você pode preservar a taxa de proporção e como você pode impedir que a imagem que está sendo ampliada (no caso do novo tamanho, na verdade, tornaria a imagem ampliada). A imagem redimensionada, em seguida, é salvo na *polegares* subpasta.

    No final da marcação, use o mesmo `<img>` elemento com o dynamic `src` atributo que você viu nos exemplos anteriores para mostrar condicionalmente a imagem. Nesse caso, você pode exibir a miniatura. Você também usar um `<a>` elemento para criar um hiperlink para a versão grande da imagem. Assim como acontece com o `src` atributo do `<img>` elemento, defina o `href` atributo do `<a>` elemento dinamicamente a tudo o que está em `imagePath`. Para certificar-se de que o caminho pode funcionar como uma URL, você deve passar `imagePath` para o `Html.AttributeEncode` método, que converte caracteres reservados no caminho em caracteres que são okey em uma URL.
4. Execute a página em um navegador.
5. Carregar uma foto e verifique se a miniatura é exibida.
6. Clique na miniatura para ver a imagem em tamanho normal.
7. No *imagens* e *imagens/polegares*, observe que os novos arquivos foram adicionados.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Girar e inverter uma imagem

O `WebImage` auxiliar também lhe permite inverter e girar imagens. Este procedimento mostra como obter uma imagem do servidor, inverter a imagem de cabeça para baixo (verticalmente), salve-o e, em seguida, exiba a imagem invertida na página. Neste exemplo, você estiver usando apenas um arquivo que você já tem no servidor (*Photo2.jpg*). Em um aplicativo real, você provavelmente seria inverter uma imagem cujo nome você obter dinamicamente, como você fez nos exemplos anteriores.

![[imagem]](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Adicionar uma nova página chamada *FlipImage.cshtml*.
2. Substitua o conteúdo existente no página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    O código usa o `WebImage` auxiliar para obter uma imagem do servidor. Você cria o caminho para a imagem usando a mesma técnica usada nos exemplos anteriores para salvar imagens e passar esse caminho, quando você cria uma imagem usando `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Se uma imagem for encontrada, você pode construir um novo caminho e nome, como você fez nos exemplos anteriores. Para inverter a imagem, você deve chamar o `FlipVertical` método e, em seguida, salve a imagem novamente.

    A imagem é exibida novamente na página usando o `<img>` elemento com o `src` atributo definido como `imagePath`.
3. Execute a página em um navegador. Na imagem para *Photo2.jpg* é mostrado de cabeça para baixo.
4. Atualize a página ou solicite a página novamente para ver que a imagem é invertida direita backup novamente.

Para girar uma imagem, você usa o mesmo código, exceto que em vez de chamar o `FlipVertical` ou `FlipHorizontal`, você chama `RotateLeft` ou `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Adicionar uma marca d'água a uma imagem

Quando você adiciona imagens ao seu site, você talvez queira adicionar uma marca d'água à imagem antes de salvá-lo ou exibi-lo em uma página. As pessoas costumam usar marcas d'água para adicionar informações de direitos autorais para uma imagem ou para anunciar o seu nome de negócios.

![[imagem]](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Adicionar uma nova página chamada *Watermark.cshtml*.
2. Substitua o conteúdo existente no página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Esse código é como o código a *FlipImage.cshtml* página anterior (embora desta vez, usa o *Photo3.jpg* arquivo). Para adicionar a marca d'água, chame o `WebImage` do auxiliar `AddTextWatermark` método antes de salvar a imagem. Na chamada para `AddTextWatermark`, passar o texto &quot;marca d'água meu&quot;, defina a cor da fonte para amarelo e definir a família de fontes para Arial. (Embora não seja mostrado aqui, o `WebImage` auxiliar também permite especificar a opacidade, família de fontes e tamanho da fonte e a posição do texto de marca d'água.) Quando você salva a imagem não deve ser somente leitura.

    Como você viu antes, a imagem é exibida na página usando o `<img>` elemento com o atributo src definida como `@imagePath`.
3. Execute a página em um navegador. Observe o texto "Minhas marca d'água" no canto inferior direito da imagem.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Usando uma imagem como uma marca d'água

Em vez de usar o texto para uma marca d'água, você pode usar outra imagem. As pessoas usam, às vezes, imagens, como um logotipo da empresa como uma marca d'água ou usar uma imagem de marca d'água em vez de texto para obter informações de direitos autorais.

![[imagem]](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Adicionar uma nova página chamada *ImageWatermark.cshtml*.
2. Adicionar uma imagem para o *imagens* pasta que você pode usar como um logotipo e renomeie a imagem *MyCompanyLogo.jpg*. Essa imagem deve ser uma imagem que você pode ver claramente quando ela é definida como 80 pixels de largura e 20 pixels de altura.
3. Substitua o conteúdo existente no página com o seguinte: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Essa é outra variação no código dos exemplos anteriores. Nesse caso, você chama `AddImageWatermark` para adicionar a imagem de marca d'água à imagem de destino (*Photo3.jpg*) antes de salvar a imagem. Quando você chama `AddImageWatermark`, defina sua largura para 80 pixels e a altura para 20 pixels. O *MyCompanyLogo.jpg* imagem é alinhada horizontalmente no centro e alinhada verticalmente na parte inferior da imagem de destino. A opacidade é definida como 100% e o preenchimento é definido como 10 pixels. Se a imagem de marca d'água é maior do que a imagem de destino, nada acontecerá. Se a imagem de marca d'água é maior do que a imagem de destino e definir o preenchimento para a marca d'água de imagem como zero, a marca d'água é ignorada.

    Como antes, você exibe a imagem usando o `<img>` elemento e um dinâmico `src` atributo.
4. Execute a página em um navegador. Observe que a imagem de marca d'água é exibido na parte inferior da imagem principal.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Trabalhando com arquivos em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introdução a páginas da Web ASP.NET usando a sintaxe Razor de programação](https://go.microsoft.com/fwlink/?LinkID=251587)
