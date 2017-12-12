---
uid: web-pages/overview/data/working-with-files
title: "Trabalhando com arquivos em um Site do ASP.NET páginas da Web (Razor) | Microsoft Docs"
author: tfitzmac
description: "Este capítulo explica como ler, gravar, acrescentar, excluir e carregar arquivos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: b3497ee17809070227115db197093c9cd0ca6c70
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Trabalhando com arquivos em um Site de páginas (Razor) da Web do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como ler, gravar, acrescentar, excluir e carregar arquivos em um site de páginas da Web do ASP.NET (Razor).
> 
> > [!NOTE]
> > Se você deseja carregar imagens e manipulá-los (por exemplo, inverter ou redimensioná-las), consulte [trabalhar com imagens em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **O que você aprenderá:** 
> 
> - Como criar um arquivo de texto e a gravação de dados.
> - Como acrescentar dados a um arquivo existente.
> - Como ler um arquivo e exibi-lo.
> - Como excluir arquivos de um site.
> - Como permitir que os usuários carregar um ou vários arquivos.
> 
> Esses são o ASP.NET recursos introduzidos no artigo de programação:
> 
> - O `File` objeto, que fornece uma maneira de gerenciar os arquivos.
> - O `FileUpload` auxiliar.
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


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Criando um arquivo de texto e gravar dados nele

Além de usar um banco de dados em seu site, você pode trabalhar com arquivos. Por exemplo, você pode usar arquivos de texto como uma maneira simple para armazenar dados do site. (Um arquivo de texto que é usado para armazenar dados às vezes é chamado um *arquivo simples*.) Arquivos de texto podem ser em formatos diferentes, como *. txt*, *. XML*, ou *. csv* (valores delimitados por vírgulas).

Se você quiser armazenar dados em um arquivo de texto, você pode usar o `File.WriteAllText` método para especificar o arquivo a ser criado e os dados para gravar nele. Neste procedimento, você criará uma página que contém um formulário simples com três `input` elementos (nome, sobrenome e endereço de email) e um **enviar** botão. Quando o usuário envia o formulário, você vai armazenar a entrada do usuário em um arquivo de texto.

1. Criar uma nova pasta chamada *aplicativo\_dados*, se ele já não existe.
2. Na raiz do seu site, crie um novo arquivo denominado *UserData.cshtml*.
3. Substitua o conteúdo existente com o seguinte: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    A marcação HTML cria o formulário com três caixas de texto. No código, você deve usar o `IsPost` para determinar se a página foi enviada antes de você iniciar o processamento.

    A primeira tarefa é obter a entrada do usuário e atribuí-lo a variáveis. O código, em seguida, concatena os valores das variáveis separadas em uma cadeia de delimitada por vírgulas de mensagens, que é armazenada em uma variável diferente. Observe que o separador de vírgula é uma cadeia de caracteres entre aspas (","), porque você está inserindo literalmente uma vírgula na cadeia de caracteres grande que você está criando. No final dos dados que você pode concatenar juntos, você deve adicionar `Environment.NewLine`. Isso adiciona uma quebra de linha (caractere de nova linha). O que você está criando com todos os este concatenação é uma cadeia de caracteres que tem esta aparência:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Com uma linha invisível quebra no final.)

    Em seguida, cria uma variável (`dataFile`) que contém o local e o nome do arquivo para armazenar os dados. Definir o local requer algum tratamento especial. Em sites, é uma prática inadequada para referir-se em código para caminhos absolutos como *C:\Folder\File.txt* para arquivos no servidor web. Se um site for movido, um caminho absoluto será errado. Além disso, para um site hospedado (em vez de em seu próprio computador) normalmente não souber qual é o caminho correto quando você está escrevendo o código.

    Mas, às vezes, (como now, para gravar um arquivo) é necessário um caminho completo. A solução é usar o `MapPath` método o `Server` objeto. Isso retorna o caminho completo para o seu site. Para obter o caminho para a raiz do site, você é usuário do `~` operador (para foram reproduzidos o site do virtual raiz) para `MapPath`. (Você também pode passar um nome de subpasta para ele, como *~/App\_dados /*, para obter o caminho para a subpasta.) Em seguida, você pode concatenar informações adicionais para o método retorna para criar um caminho completo. Neste exemplo, você adiciona um nome de arquivo. (Você pode ler mais sobre como trabalhar com caminhos de arquivo e pasta no [Introdução ao ASP.NET páginas da Web de programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    O arquivo é salvo no *aplicativo\_dados* pasta. Essa pasta é uma pasta especial no ASP.NET que é usado para armazenar arquivos de dados, conforme descrito em [Introdução ao trabalho com um banco de dados em Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=195209).

    O `WriteAllText` método o `File` objeto grava os dados no arquivo. Esse método usa dois parâmetros: o nome (com o caminho) do arquivo para gravar e os dados reais para gravação. Observe que o nome do primeiro parâmetro tem um `@` caractere como um prefixo. Isso informa ao ASP.NET que você está fornecendo uma cadeia de caracteres textual literal e caracteres como "/" não deve ser interpretado de maneira especial. (Para obter mais informações, consulte [Introdução ao ASP.NET Web programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Para que seu código para salvar arquivos de *aplicativo\_dados* pasta, o aplicativo precisa de leitura-gravação permissões para essa pasta. No computador de desenvolvimento isso não é geralmente um problema. No entanto, quando você publica seu site ao servidor da web de um provedor de hospedagem, você precisará definir explicitamente as permissões. Se você executa esse código no servidor de um provedor de hospedagem e obter erros, verifique com o provedor de hospedagem para saber como definir essas permissões.

- Execute a página em um navegador. 

    ![](working-with-files/_static/image1.jpg)
- Insira valores para os campos e, em seguida, clique em **enviar**.
- Feche o navegador.
- Volte para o projeto e atualizar a exibição.
- Abra o *txt* arquivo. Os dados enviados no formulário são no arquivo. 

    ![[imagem]](working-with-files/_static/image2.jpg)
- Fechar o *txt* arquivo.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Adicionar dados a um arquivo existente

No exemplo anterior, você usou `WriteAllText` para criar um arquivo de texto que tem apenas uma parte dos dados nele. Se você chama o método novamente e passar a ele o mesmo nome de arquivo, o arquivo existente será substituído completamente. No entanto, depois de criar um arquivo geralmente deseja adicionar novos dados ao final do arquivo. Você pode fazer essa usando o `AppendAllText` método o `File` objeto.

1. No site, faça uma cópia do *UserData.cshtml* de arquivo e nomeie a cópia *UserDataMultiple.cshtml*.
2. Substitua o bloco de código antes da abertura `<!DOCTYPE html>` marca com o bloco de código a seguir: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Esse código tem uma alteração do exemplo anterior. Em vez de usar `WriteAllText`, ele usa `the AppendAllText` método. Os métodos são semelhantes, exceto que `AppendAllText` adiciona os dados ao final do arquivo. Assim como acontece com `WriteAllText`, `AppendAllText` criará o arquivo se ele ainda não existir.
3. Execute a página em um navegador.
4. Insira valores para os campos e, em seguida, clique em **enviar**.
5. Adicionar mais dados e envie o formulário novamente.
6. Retornar ao seu projeto, clique na pasta de projeto e, em seguida, clique em **atualizar**.
7. Abra o *txt* arquivo. Agora, ele contém os novos dados que você acabou de digitar. 

    ![[imagem]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lendo e exibindo dados de um arquivo

Mesmo se você não precisa gravar dados em um arquivo de texto, provavelmente, às vezes, você precisará ler dados de um. Para fazer isso, você pode usar novamente o `File` objeto. Você pode usar o `File` objeto para ler cada linha individualmente (separados por quebras de linha) ou para ler um item individual, independentemente de como eles são separados.

Este procedimento mostra como ler e exibir os dados que você criou no exemplo anterior.

1. Na raiz do seu site, crie um novo arquivo denominado *DisplayData.cshtml*.
2. Substitua o conteúdo existente com o seguinte: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    O código começa lendo o arquivo criado no exemplo anterior em uma variável chamada `userData`, usando esta chamada de método:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    O código para fazer isso está dentro de um `if` instrução. Quando você quiser ler um arquivo, é recomendável usar o `File.Exists` método para determinar primeiro se o arquivo está disponível. O código também verifica se o arquivo está vazio.

    O corpo da página contém dois `foreach` loops, um aninhado em outro. Externa `foreach` loop obtém uma linha por vez do arquivo de dados. Nesse caso, as linhas são definidas por quebras de linha no arquivo de &#8212; ou seja, cada item de dados está em sua própria linha. O loop externo cria um novo item (`<li>` elemento) dentro de uma lista ordenada (`<ol>` elemento).

    O loop interno divide cada linha de dados em itens (campos) usando uma vírgula como delimitador. (Com base no exemplo anterior, isso significa que cada linha contém três campos &#8212; o nome, sobrenome e endereço de email, cada um separado por vírgula). O loop interno também cria um `<ul>` lista e exibe uma lista de item para cada campo na linha de dados.

    O código demonstra como usar dois tipos de dados, uma matriz e o `char` tipo de dados. A matriz é necessária porque o `File.ReadAllLines` método retorna dados como uma matriz. O `char` tipo de dados é necessário porque o `Split` método retorna um `array` no qual cada elemento é do tipo `char`. (Para obter informações sobre matrizes, consulte [Introdução ao ASP.NET Web programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Execute a página em um navegador. Os dados inseridos nos exemplos anteriores, são exibidos. 

    ![[imagem]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Exibindo dados de um arquivo delimitado por vírgula do Microsoft Excel**
> 
> Você pode usar o Microsoft Excel para salvar os dados contidos em uma planilha como um arquivo delimitado por vírgula (*. csv* arquivo). Quando você fizer isso, o arquivo é salvo em texto sem formatação, não está no formato do Excel. Cada linha da planilha é separada por uma quebra de linha no arquivo de texto, e cada item de dados é separado por uma vírgula. Você pode usar o código mostrado no exemplo anterior para ler um arquivo delimitado por vírgula do Excel apenas alterando o nome do arquivo de dados em seu código.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Excluindo arquivos

Para excluir arquivos do seu site, você pode usar o `File.Delete` método. Este procedimento mostra como permitir que os usuários excluir uma imagem (*. jpg* arquivo) de um *imagens* se souberem o nome do arquivo de pasta.

> [!NOTE] 
> 
> **Importante** em um site de produção, você normalmente restringir quem tem permissão para fazer alterações nos dados. Para obter informações sobre como configurar a associação e sobre maneiras para autorizar usuários a executar tarefas no site, consulte [adicionando segurança e associação a um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. No site, crie uma subpasta chamada *imagens*.
2. Copiar um ou mais *. jpg* arquivos para o *imagens* pasta.
3. Na raiz do site, crie um novo arquivo denominado *FileDelete.cshtml*.
4. Substitua o conteúdo existente com o seguinte: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Esta página contém um formulário onde os usuários podem inserir o nome de um arquivo de imagem. Eles não inserirem o *. jpg* extensão de nome de arquivo; restringindo o nome do arquivo como esta, você ajuda impede que os usuários excluindo arquivos arbitrários no seu site.

    O código lê o nome do arquivo que o usuário tiver inserido e, em seguida, constrói um caminho completo. Para criar o caminho, o código usa o caminho atual do site (como retornado pelo `Server.MapPath` método), o *imagens* ". jpg" como uma cadeia de caracteres literal, nome da pasta e o nome que o usuário forneceu.

    Para excluir o arquivo, o código chama o `File.Delete` método, passando o caminho completo que você acabou de criar. No final da marcação, código exibe uma mensagem de confirmação que o arquivo foi excluído.
5. Execute a página em um navegador. 

    ![[imagem]](working-with-files/_static/image5.jpg)
6. Digite o nome do arquivo e, em seguida, clique em **enviar**. Se o arquivo foi excluído, o nome do arquivo é exibido na parte inferior da página.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Permitindo que os usuários carregar um arquivo

O `FileUpload` auxiliar permite que os usuários carregar arquivos em seu site. O procedimento a seguir mostra como permitir que os usuários carregar um único arquivo.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você não adicionou anteriormente.
2. No *aplicativo\_dados* pasta, crie um novo uma pasta e nomeie- *UploadedFiles*.
3. Na raiz, crie um novo arquivo denominado *FileUpload.cshtml*.
4. Substitua o conteúdo existente na página com o seguinte: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    A parte do corpo da página usa o `FileUpload` auxiliar para criar a caixa de carregamento e botões que você provavelmente está familiarizado com:

    ![[imagem]](working-with-files/_static/image6.jpg)

    As propriedades que você definiu para o `FileUpload` auxiliar especifique que você deseja que uma única caixa carregar o arquivo e que você deseja que o botão de envio para ler **carregar**. (Você adicionará mais caixas posteriormente neste artigo.)

    Quando o usuário clica **carregar**, obtém o arquivo de código na parte superior da página e salva-o. O `Request` objeto que você normalmente usa para obter valores de campos de formulário também tem um `Files` matriz que contém o arquivo (ou arquivos) que foram carregados. Você pode obter dos arquivos individuais posições específicas na matriz &#8212; Por exemplo, para obter o primeiro arquivo carregado, você receberá `Request.Files[0]`, para obter o segundo arquivo, você obtém `Request.Files[1]`, e assim por diante. (Lembre-se de que em programação, contando geralmente começa em zero.)

    Quando você busca um arquivo carregado, você colocá-lo em uma variável (aqui, `uploadedFile`) para que você possa manipulá-los. Para determinar o nome do arquivo carregado, você obtém apenas seu `FileName` propriedade. No entanto, quando o usuário carrega um arquivo, `FileName` contém o nome do usuário original, que inclui o caminho completo. Ele pode parecer com isso:

    *C:\Users\Public\Sample.txt*

    Você não quer todas essas informações de caminho, no entanto, porque esse é o caminho no computador do usuário, não para o servidor. Você quiser apenas o nome do arquivo real (*exemplo.txt*). Você pode extrair apenas o arquivo de um caminho usando o `Path.GetFileName` método, como este:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    O `Path` objeto é um utilitário que tem um número de métodos assim que você pode usar para remover caminhos, combinar caminhos e assim por diante.

    Depois que o nome do arquivo carregado, você pode criar um novo caminho para onde você deseja armazenar o arquivo carregado em seu site. Nesse caso, você combinar `Server.MapPath`, os nomes de pasta (*aplicativo\_dados/UploadedFiles*) e o nome do arquivo extraído recentemente para criar um novo caminho. Em seguida, você pode chamar o arquivo carregado `SaveAs` método realmente salvar o arquivo.
5. Execute a página em um navegador. 

    ![[imagem]](working-with-files/_static/image7.jpg)
6. Clique em **procurar** e, em seguida, selecione um arquivo para carregar. 

    ![[imagem]](working-with-files/_static/image8.jpg)

    Caixa de texto ao lado de **procurar** botão conterá o local do arquivo e caminho.

    ![[imagem]](working-with-files/_static/image9.jpg)
7. Clique em **carregar**.
8. No site, clique na pasta de projeto e, em seguida, clique em **atualizar**.
9. Abra o *UploadedFiles* pasta. É o arquivo que você carregou na pasta. 

    ![[imagem]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Permitir que os usuários carregar vários arquivos

No exemplo anterior, permitem aos usuários carregar um arquivo. Mas você pode usar o `FileUpload` auxiliar para carregar mais de um arquivo por vez. Isso é útil para cenários como o carregamento de fotos, onde carregar um arquivo por vez é entediante. (Você pode ler sobre como carregar fotos em [trabalhar com imagens em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897).) Este exemplo mostra como permitir que os usuários carregar dois ao mesmo tempo, embora você possa usar a mesma técnica para carregar mais do que isso.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.
2. Criar uma nova página chamada *FileUploadMultiple.cshtml*.
3. Substitua o conteúdo existente na página com o seguinte:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    Neste exemplo, o `FileUpload` auxiliar no corpo da página é configurado para permitir que usuários carregar dois arquivos por padrão. Porque `allowMoreFilesToBeAdded` é definido como `true`, o auxiliar renderiza um link que permite que o usuário adicione mais caixas de carregamento:

    ![[imagem]](working-with-files/_static/image11.jpg)

    Para processar os arquivos que o usuário carrega, o código usa a mesma técnica básica que você usou no exemplo anterior &#8212; obter um arquivo de `Request.Files` e, em seguida, salvá-lo. (Incluindo várias coisas você precisa fazer para que o nome correto do arquivo e o caminho.) A inovação neste momento é que o usuário pode ser upload de vários arquivos e você não souber muitos. Para descobrir, você pode obter `Request.Files.Count`.

    Com esse número em mão, você pode fazer loop por meio de `Request.Files`, buscar cada arquivo por sua vez e salvá-lo. Quando você quiser um número conhecido de vezes por meio de uma coleção de loop, você pode usar um `for` loop, da seguinte maneira:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    A variável `i` é um contador temporário que irá de zero para qualquer limite superior definido. Nesse caso, o limite superior é o número de arquivos. Mas, como o contador começa em zero, como é comum para a contagem de cenários no ASP.NET, o limite superior é realmente uma menor que a contagem de arquivos. (Se os três arquivos são carregados, a contagem é zero para 2.)

    O `uploadedCount` variável totais de todos os arquivos que são carregados e salvos com êxito. Esse código de contas para a possibilidade de que um arquivo esperado não poderá ser carregado.
4. Execute a página em um navegador. O navegador exibirá a página e suas caixas de carregamento de dois.
5. Selecione dois arquivos para carregar.
6. Clique em **adicionar outro arquivo**. A página exibe uma nova caixa de carregamento. 

    ![[imagem]](working-with-files/_static/image12.jpg)
7. Clique em **carregar**.
8. No site, clique na pasta de projeto e, em seguida, clique em **atualizar**.
9. Abra o *UploadedFiles* pasta para ver os arquivos carregados com êxito.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Trabalhando com imagens em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportando para um arquivo CSV](https://msdn.microsoft.com/en-us/library/ms155919.aspx)
