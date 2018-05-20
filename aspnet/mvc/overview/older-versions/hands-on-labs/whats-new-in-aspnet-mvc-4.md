---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: O que há de novo no ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 485d2ba7a1274bbb36cfbcbca9322cecc8c8d77c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/18/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>O que há de novo no ASP.NET MVC 4

Por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 é uma estrutura para criar aplicativos web escalonável, baseado em padrões usando padrões de design bem estabelecidos e o poder do ASP.NET e o .NET framework. Essa nova quarta versão do framework enfoca facilita o desenvolvimento de aplicativos web móvel.

Em primeiro lugar, quando você cria um novo projeto ASP.NET MVC 4 é agora um modelo de projeto de aplicativo móvel que você pode usar para compilar um aplicativo autônomo especificamente para dispositivos móveis. Além disso, ASP.NET MVC 4 integra-se com o jQuery Mobile por meio de um pacote jQuery.Mobile.MVC NuGet. o jQuery Mobile é uma estrutura baseada em HTML5 para o desenvolvimento de aplicativos web que são compatíveis com todas as plataformas de dispositivo móvel popular, incluindo Windows Phone, iPhone, Android e assim por diante. No entanto, se você precisar de especialização, ASP.NET MVC 4 também permite que você atender a diferentes modos de exibição para diferentes dispositivos e fornecer otimizações específicas do dispositivo.

Este laboratório prático, você iniciará com o ASP.NET MVC 4 &quot;aplicativo de Internet&quot; modelo de projeto para criar um aplicativo da Galeria de fotos. Você aprimorará progressivamente o aplicativo usando o jQuery Mobile e recursos novos do ASP.NET MVC 4 para torná-lo compatível com navegadores da web da área de trabalho e de dispositivos móveis diferentes. Você também aprenderá sobre novo receitas de código para geração de código e como o ASP.NET MVC 4 torna mais fácil para gravar os métodos de ação assíncrono com o suporte à tarefa&lt;ActionResult&gt; tipos de retorno.

> [!NOTE]
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). O projeto específico para este laboratório está disponível em [o que há de novo no Web Forms no ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Aproveitar os aprimoramentos para o ASP.NET MVC projeto modelos, incluindo o novo modelo de projeto de aplicativo móvel
- Use o atributo de visor do HTML5 e consultas de mídia CSS para melhorar a exibição em dispositivos móveis
- Use o jQuery Mobile para aprimoramentos progressivos e para a criação de interface da web otimizada para toque
- Criar exibições específicas para dispositivos móveis
- Use o componente de alternância de exibição para alternar entre modos de exibição móveis e área de trabalho do aplicativo
- Criar controladores assíncronos usando o suporte da tarefa

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).
- [ASP.NET MVC 4](../../../mvc4.md) (incluído na instalação do Microsoft Visual Studio 2012)
- Emulador do Windows Phone (incluído no [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Opcional - [o WebMatrix 2](https://www.microsoft.com/web/webmatrix/) com **Electric Plum iPhone simulador** extensão (somente para o Exercício 3 usada para procurar o aplicativo web com um simulador de iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configuração

Em todo o documento de laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maioria do código é fornecido como o Visual Studio trechos de código que pode ser usado de dentro do Visual Studio para evitar a necessidade para adicioná-lo manualmente.

Para instalar os trechos de código:

1. Abra uma janela do Windows Explorer e navegue até o laboratório **Source\Setup** pasta.
2. Clique duas vezes o **Setup.cmd** arquivo nessa pasta para instalar os trechos de código do Visual Studio.

Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice a:](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Novos modelos de projeto do ASP.NET MVC 4](#Exercise1)
2. [Criando o aplicativo Web de galeria de fotos](#Exercise2)
3. [Adicionando suporte para dispositivos móveis](#Exercise3)
4. [Usando controladores assíncronos](#Exercise4)

> [!NOTE]
> Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios. Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.


Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Exercício 1: Novos modelos de projeto do ASP.NET MVC 4

Neste exercício, você irá explorar os aprimoramentos nos modelos de projeto do ASP.NET MVC 4. Além do modelo de aplicativo de Internet, já está presente no MVC 3, esta versão agora inclui um modelo separado para aplicativos móveis. Primeiro, você examinará alguns recursos relevantes de cada um dos modelos. Em seguida, você trabalhará na sua página corretamente em diferentes plataformas de processamento usando a abordagem certa.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Tarefa 1: Explorando o modelo de aplicativo de Internet

1. Abra **Visual Studio**.
2. Selecione o **arquivo | Novo | Projeto** comando de menu. No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha **aplicativo Web do ASP.NET MVC 4.** Nomeie o projeto **Galeriadefotos**, selecione um local (ou deixe o padrão) e clique em **Okey**.

    > [!NOTE]
    > Posteriormente, você personalizará a solução Galeriadefotos ASP.NET MVC 4 que você está criando agora.

    ![Criar um novo projeto](whats-new-in-aspnet-mvc-4/_static/image1.png "criar um novo projeto")

    *Criar um novo projeto*
3. No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo de projeto e clique em **Okey**. Verifique se que você selecionou Razor como o mecanismo de exibição.

    ![Criar um novo aplicativo ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "criar um novo aplicativo ASP.NET MVC 4 da Internet")

    *Criar um novo aplicativo ASP.NET MVC 4 da Internet*

    > [!NOTE]
    > A sintaxe do Razor foi introduzida no ASP.NET MVC 3. Seu objetivo é minimizar o número de caracteres e pressionamentos de tecla necessários em um arquivo, permitindo um rápido e fluido fluxo de trabalho de codificação. Aproveita Razor existente c# / VB (ou outros) habilidades de linguagem e fornece uma sintaxe de marcação de modelo que permite que um fluxo de trabalho de construção awesome HTML.
4. Pressione **F5** para executar a solução e ver os modelos renovados. Você pode conferir os recursos a seguir:

    **Modelos de estilo moderno**

    Os modelos foram renovados, fornecendo mais estilos aparência moderna.

    ![Modelos do ASP.NET MVC 4 remodelado](whats-new-in-aspnet-mvc-4/_static/image3.png "remodelado modelos do MVC 4")

    *Modelos do ASP.NET MVC 4 remodelado*

    ![Nova página de contato](whats-new-in-aspnet-mvc-4/_static/image4.png "página novo contato")

    *Nova página de contato*

    **Renderização adaptável**

    Check-out de redimensionar a janela do navegador e observe como o layout de página adapta dinamicamente para o novo tamanho da janela. Esses modelos de usam a técnica de renderização adaptável para processar corretamente em plataformas móveis e desktop sem nenhuma personalização.

    ![Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador](whats-new-in-aspnet-mvc-4/_static/image5.png "modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador")

    *Modelo de projeto ASP.NET MVC 4 em tamanhos diferentes do navegador*

    **Interface de usuário mais rica com JavaScript**

    Outro aprimoramento para modelos de projeto padrão é o uso de JavaScript para fornecer um JavaScript mais interativo. Os links de logon e registro usados no modelo exemplificam como usar o jQuery validações para validar os campos de entrada do lado do cliente.

    ![jQuery validação](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery validação*

    > [!NOTE]
    > Observe que os dois logon nas seções, a primeira seção você pode fazer logon usando uma conta de marca do site e a segunda seção que pode altenativelly logon usando outro serviço de autenticação com o google (desabilitado por padrão).
5. Feche o navegador para interromper o depurador e retornar ao Visual Studio.
6. Abra o arquivo **AuthConfig.cs** localizado sob o **aplicativo\_iniciar** pasta.
7. Remova o comentário da última linha para registrar o cliente do Google para *OAuth* autenticação.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Observe que você pode facilmente habilitar autenticação usando qualquer serviço OpenID ou OAuth do Facebook, Twitter, Microsoft, etc.
8. Pressione **F5** para executar a solução e navegue até a página de logon.
9. Selecione **Google** serviço para fazer logon.

    ![Selecionar o log no serviço](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Selecionar o log no serviço*
10. Faça logon usando a conta do Google.
11. Permitir que o site (localhost) recuperar informações de conta do Google.
12. Por fim, você terá que registrar no site para associar a conta do Google.

   ![Associe a conta do Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Associando a conta do Google*
13. Feche o navegador para interromper o depurador e retornar ao Visual Studio.
14. Explore agora a solução para check-out de alguns outros novos recursos introduzidos pelo ASP.NET MVC 4 no modelo de projeto.

   ![O modelo de projeto de aplicativo do ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "o modelo de projeto de aplicativo de Internet do ASP.NET MVC 4")

   *O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4*

   - **HTML 5 marcação**

       Procure exibições de modelo para descobrir a nova marcação de tema.

       ![Novo modelo, usando About.cshtml de marcação Razor e HTML5. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Novo modelo, usando About.cshtml de marcação Razor e HTML5.")

       *Novo modelo, usando marcação Razor e HTML5 (About.cshtml).*
   - **Bibliotecas de JavaScript atualizadas**

       O modelo de padrão do ASP.NET MVC 4 agora inclui KnockoutJS, uma estrutura de JavaScript MVVM que permite que você crie avançados e aplicativos web altamente responsivo usando JavaScript e HTML. Como em MVC3, jQuery e jQuery bibliotecas de interface do usuário também estão incluídas no ASP.NET MVC 4.

     > [!NOTE]
     > Você pode obter mais informações sobre a biblioteca KnockOutJS neste link: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Além disso, você pode aprender sobre jQuery e jQuery UI em [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Tarefa 2: explorar o modelo de aplicativo móvel

ASP.NET MVC 4 facilita o desenvolvimento de sites da Web para dispositivos móveis e navegadores do tablet. Esse modelo tem a mesma estrutura de aplicativo como o modelo de aplicativo de Internet (Observe que o código do controlador é praticamente idêntico), mas seu estilo foi modificado para ser processada corretamente em dispositivos móveis baseados em toque.

1. Selecione o **arquivo | Novo | Projeto** comando de menu. No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha o **aplicativo Web do ASP.NET MVC 4.** Nomeie o projeto **PhotoGallery.Mobile**, selecione um local (ou deixe o padrão), selecione &quot;adicionar à solução&quot; e clique em **Okey**.
2. No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **Mobile Application** modelo de projeto e clique em **Okey**. Verifique se que você selecionou Razor como o mecanismo de exibição.

    ![Criar um novo aplicativo ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "criar um novo aplicativo ASP.NET MVC 4 Mobile")

    *Criar um novo aplicativo ASP.NET MVC 4 Mobile*
3. Agora é possível explorar a solução e check-out de alguns dos novos recursos introduzidos pelo modelo de solução do ASP.NET MVC 4 para dispositivos móveis:

    - **jQuery Mobile biblioteca**

        O modelo de projeto de aplicativo móvel inclui a biblioteca jQuery Mobile, que é uma biblioteca de software livre para compatibilidade com navegadores móveis. o jQuery Mobile aplica aprimoramento progressivo para navegadores de dispositivos móveis que oferecem suporte a CSS e JavaScript. Aprimoramento progressivo permite que todos os navegadores exibir o conteúdo básico de uma página da web, enquanto ele só permite que os navegadores mais eficientes exibir o conteúdo. Os arquivos JavaScript e CSS, incluídos no jQuery Mobile estilo, ajudam navegadores móveis para ajustar o conteúdo na tela sem fazer qualquer alteração na marcação da página.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *biblioteca de móveis jQuery incluída no modelo*
    - **Marcação de HTML5 com base**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Modelo de aplicativo móvel usando HTML5 marcação, (cshtml e cshtml)*
4. Pressione **F5** para executar a solução.
5. Abra o **Windows Phone 7 emulador**.
6. Na tela de início do telefone, abra o Internet Explorer. Confira a URL onde o aplicativo de área de trabalho foi iniciado e navegue para a URL do telefone (por exemplo, `http://localhost:[PortNumber]/`).
7. Agora é possível inserir a página de logon ou check-out de sobre a página. Observe que o estilo do site se baseia o novo aplicativo do Metro para dispositivos móveis. O modelo de projeto ASP.NET MVC 4 é exibido corretamente em dispositivos móveis, tornando-se de que todos os elementos da página são visíveis e habilitados. Observe que os links no cabeçalho grandes o suficiente para ser clicado ou tocado.

    ![Páginas de modelo em um dispositivo móvel do projeto](whats-new-in-aspnet-mvc-4/_static/image14.png "projeto páginas de modelo em um dispositivo móvel")

    *Páginas de modelo de projeto em um dispositivo móvel*
8. O novo modelo também usa o **marca meta Viewport**. A maioria dos navegadores definem uma largura de uma janela do navegador virtual ou &quot;visor&quot;, que é maior do que a largura real do dispositivo móvel. Isso permite que navegadores móveis exibir a página da web inteira dentro a exibição virtual. O **marca meta Viewport** permite que os desenvolvedores da web definir a largura, altura e escala da área do navegador em dispositivos móveis **.** O modelo do ASP.NET MVC 4 para aplicativos móveis define o visor para a largura do dispositivo (&quot;largura = dispositivo largura&quot;) no modelo de layout (*exibições \ compartilhadas\_cshtml*), de modo que todos os a páginas terão seu visor definida como a largura de tela do dispositivo. Observe que a marca meta Viewport não alterará o modo de exibição de navegador padrão.
9. Abra  **\_cshtml**, localizado no **exibições | Compartilhado** pasta e comente a marca meta do visor. Executar o aplicativo, se não estiver já aberto e conferir as diferenças.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![O site após a marca meta viewport de comentário](whats-new-in-aspnet-mvc-4/_static/image15.png "o site após a marca meta viewport de comentário")

*O site após a marca meta viewport de comentário*
10. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.
11. Remova a marca meta viewport.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Tarefa 3: usar a renderização adaptável

Nesta tarefa, você aprenderá a outro método para renderizar uma página da Web corretamente em dispositivos móveis e navegadores da Web ao mesmo tempo sem nenhuma personalização. Você já usou marca meta Viewport com uma finalidade semelhante. Agora você atenderá outro método avançado: *processamento adaptável*.

Renderização adaptável é uma técnica que usa **consultas de mídia CSS3** para personalizar o estilo aplicado a uma página. As consultas de mídia definem condições dentro de uma folha de estilos, agrupando os estilos CSS em uma determinada condição. Somente quando a condição for true, o estilo é aplicado aos objetos declarados.

A flexibilidade fornecida pela técnica processamento adaptável permite que qualquer personalização para exibir o site em diferentes dispositivos. Você pode definir estilos quantos desejar em uma folha de estilos única sem escrever código de lógica para escolher o estilo. Portanto, é uma maneira muito interessante de adaptar estilos de página, pois reduz a quantidade de código duplicado e lógica para fins de processamento. Por outro lado, o consumo de largura de banda aumentaria, como o tamanho dos arquivos CSS pode aumentar um pouco.

Usando a técnica de renderização adaptável, o site será **exibido corretamente, independentemente do navegador.** No entanto, você deve considerar se a largura de banda extra de carga é uma preocupação.

> [!NOTE]
> O formato básico de uma consulta de mídia é: @media \[escopo: todos os | portáteis | imprimir | projeção | tela\] ([: valor da propriedade] e... [: valor da propriedade])


Exemplos de consultas de mídia: &gt;  <strong>@media todas as e (largura máxima: 1000px) e (largura mínima: 700px) {}:</strong> para todas as resoluções entre 700px e 1000px.

> <strong>@media tela e (largura mínima: 400px) e (largura máxima: 700px) {...}:</strong> somente para telas. A resolução deve ficar entre 400 e 700px.
> 
> <strong>@media portáteis e (largura mínima: 20em), tela e (largura mínima: 20em) {...}:</strong> para telas e portáteis (móvel e dispositivos). A largura mínima deve ser maior que 20em.
> 
> Você pode encontrar mais informações sobre isso no [site do W3C](http://www.w3.org/TR/css3-mediaqueries/).


Agora você explorará como funciona a renderização adaptável, melhorando a legibilidade do ASP.NET MVC 4 padrão modelo de site.

1. Abra o **PhotoGallery.sln** solução que você criou na tarefa 1 e selecione o **Galeriadefotos** projeto. Pressione **F5** para executar a solução.
2. Redimensione a largura do navegador, configuração do windows para a metade ou para menos de um quarto de seu tamanho original. Observe o que acontece com os itens no cabeçalho: alguns elementos não aparecerá na área visível do cabeçalho.
3. Abra <strong>Site.css</strong> arquivo no Visual Studio Gerenciador de soluções, localizado em <strong>conteúdo</strong> pasta do projeto. Pressione <strong>CTRL + F</strong> para abrir a pesquisa integrada do Visual Studio e gravar <strong>@media</strong> para localizar o <strong>consulta de mídia do CSS</strong>.

    A condição de consulta de mídia definida neste modelo funciona desta forma: quando o tamanho da janela do navegador é abaixo **850 px**, as regras de CSS aplicadas são aquelas definidas neste bloco de mídia.

    ![Localizando a consulta de mídia](whats-new-in-aspnet-mvc-4/_static/image16.png "Localizando a consulta de mídia")

    *Localizando a consulta de mídia*
4. Substitua o valor do atributo de largura máxima definido 850 px com **10px**, para desativar o processamento adaptável e pressione **CTRL + S** para salvar as alterações. Voltar para o navegador e pressione **CTRL + F5** para atualizar a página com as alterações feitas por você. Observe as diferenças entre as duas páginas ao ajustar a largura da janela.

    ![À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido](whats-new-in-aspnet-mvc-4/_static/image17.png "à esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido")

    <em>À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido</em>

    Agora, vamos conferir o que acontece em dispositivos móveis:

    ![À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido](whats-new-in-aspnet-mvc-4/_static/image18.png "à esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido")

    <em>À esquerda, a página está aplicando o @media estilo, no canto direito, o estilo for omitido</em>

    Embora você observará que as alterações quando a página é processada em um navegador da Web não são muito significativas, ao usar um dispositivo móvel as diferenças se tornam mais óbvias. No lado esquerdo da imagem, podemos ver que o estilo personalizado aprimorada a legibilidade.

    Renderização adaptável pode ser usada em muitos mais cenários, tornando mais fácil aplicar o estilo para um site da Web e resolver problemas comuns de estilo com uma abordagem interessante condicional.

    A marca meta do visor e consultas de mídia CSS não são específicas para ASP.NET MVC 4, portanto você pode tirar proveito desses recursos em um aplicativo web.
5. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Exercício 2: Criar o aplicativo Web de galeria de fotos

Neste exercício, você trabalhará em um aplicativo da Galeria de fotos para exibir fotos. Inicie com o modelo de projeto ASP.NET MVC 4 e, em seguida, você adicionará um recurso para recuperar fotos de um serviço e exibi-las na home page.

O exercício a seguir, você irá atualizar esta solução para aperfeiçoar a maneira como ele é exibido em dispositivos móveis.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Tarefa 1: Criando um serviço de simulação de fotos

Nesta tarefa, você criará uma simulação do serviço para recuperar o conteúdo que será exibido na Galeria de fotos. Para fazer isso, você adicionará um novo controlador que retornará apenas um arquivo JSON com os dados de cada foto.

1. Abra **Visual Studio** se ainda não estiver aberto.
2. Selecione o **arquivo | Novo | Projeto** comando de menu. No **novo projeto** caixa de diálogo, selecione o **Visual c# | Web** modelo no painel esquerdo da árvore e escolha **aplicativo Web do ASP.NET MVC 4.** Nomeie o projeto **Galeriadefotos**, selecione um local (ou deixe o padrão) e clique em **Okey**. Como alternativa, você pode continuar trabalhando de seu existente ASP.NET MVC 4 **aplicativo de Internet** solução de **Exercício 1** e passar para a próxima etapa.
3. No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo de projeto e clique em **Okey**. Certifique-se de que ter selecionado como o mecanismo de exibição do Razor.
4. No **Solution Explorer**, com o botão direito do **aplicativo\_dados** pasta do seu projeto e selecione **adicionar | Item existente**. Navegue até o **Source\Assets\App\_dados** pasta deste laboratório e adicione o **Photos.json** arquivo.
5. Criar um novo controlador com o nome **PhotoController**. Para fazer isso, clique duas vezes no **controladores** pasta, vá para **adicionar** e selecione **controlador.** Complete o nome do controlador, deixe o **controlador MVC vazio** modelo e clique em **adicionar**.

    ![Adicionando o PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "adicionando o PhotoController")

    *Adicionando o PhotoController*
6. Substitua o **índice** método com o seguinte **galeria** ação e retornar o conteúdo do arquivo JSON que você recentemente adicionou ao projeto.

    (Código de trecho - *ação de galeria do ASP.NET MVC 4 laboratório - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Pressione **F5** para executar a solução e, em seguida, navegue até a URL a seguir para testar o serviço de foto fictícias: `http://localhost:[port]/photo/gallery` (o valor [port] depende da porta atual em que o aplicativo foi iniciado). A solicitação para essa URL deve recuperar o conteúdo a **Photos.json** arquivo.

    ![Testando o serviço de foto fictícios](whats-new-in-aspnet-mvc-4/_static/image20.png "testando o serviço de foto fictícios")

    *Testando o serviço de foto fictícios*

Em uma implementação real, você pode usar [ASP.NET Web API](../../../../web-api/index.md) para implementar o serviço de galeria de fotos. API da Web do ASP.NET é uma estrutura que torna mais fácil criar serviços HTTP que alcançam uma ampla gama de clientes, incluindo navegadores e dispositivos móveis. A API do ASP.NET Web é uma plataforma ideal para criar aplicativos com REST no .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Tarefa 2 - exibindo a Galeria de fotos

Nesta tarefa, você atualizará a página inicial para mostrar a Galeria de fotos usando o serviço fictícios que você criou na primeira tarefa deste exercício. Você adicionará os arquivos de modelo e atualizar os modos de exibição de galeria.

1. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.
2. Criar o **foto** classe no **modelos** pasta. Para fazer isso, clique duas vezes no **modelos** pasta, selecione **adicionar** e clique em **classe**. Em seguida, defina o nome como **Photo.cs** e clique em **adicionar**.
3. Adicionar os seguintes membros para o **foto** classe.

    (Código de trecho - *modelo foto do ASP.NET MVC 4 laboratório - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Abra o arquivo **HomeController.cs** da pasta **Controladores**.
5. Adicione o seguinte usando instruções.

    (Código de trecho - *usos do ASP.NET MVC 4 laboratório - Ex02 - HomeController*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Atualização de **índice** ação a ser usada **HttpClient** para recuperar os dados da galeria e, em seguida, use o **JavaScriptSerializer** desserializá-lo para o modelo de exibição.

    (Código de trecho - *ação de índice do ASP.NET MVC 4 laboratório - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Abra o **cshtml** arquivo localizado sob o **Views\Home** pasta e substitua todo o conteúdo com o código a seguir.

    Esse código percorre todas as fotos recuperadas do serviço e os exibe em uma lista não ordenada.

    (Código de trecho - *lista de fotos do ASP.NET MVC 4 laboratório - Ex02 -*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. No **Solution Explorer**, com o botão direito do **conteúdo** pasta do seu projeto e selecione **adicionar | Item existente**. Navegue até o **Source\Assets\Content** pasta deste laboratório e adicione o **Site.css** arquivo. Você precisará confirmar sua substituição. Se você tiver o **Site.css** arquivo aberto, você precisará confirmar para recarregar o arquivo também.
9. Abra o Explorador de arquivos e copie todo o **fotos** pasta localizado sob o **Source\Assets** pasta deste laboratório para a pasta raiz do seu projeto no Gerenciador de soluções.
10. Execute o aplicativo. Agora você deve ver a home page exibe as fotos na Galeria.

    ![Galeria de fotos](whats-new-in-aspnet-mvc-4/_static/image21.png "Galeria de fotos")

    *Galeria de fotos*
11. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Exercício 3: Adicionando suporte para dispositivos móveis

Uma das atualizações de chave no ASP.NET MVC 4 é o suporte para desenvolvimento móvel. Neste exercício, você irá explorar os novos recursos do ASP.NET MVC 4 para aplicativos móveis ao estender a solução Galeriadefotos que você criou no exercício anterior.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Tarefa 1: instalar o jQuery Mobile em um aplicativo do ASP.NET MVC 4

1. Abra o **começar** solução localizado em **fonte/Ex3-MobileSupport/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **Package Manager Console** clicando o **ferramentas** &gt; **Gerenciador de biblioteca de pacote** &gt; **Package Manager Console** opção de menu.

    ![Abrindo o NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "abrindo o NuGet Package Manager Console")

    *Abrindo o Console do Gerenciador de pacote do NuGet*
3. No Console do Gerenciador de pacotes, execute o seguinte comando para instalar o **jQuery.Mobile.MVC** pacote.

    jQuery Mobile é uma biblioteca de código aberto para a criação de interface da web otimizada para toque. O pacote jQuery.Mobile.MVC NuGet inclui auxiliares para usar o jQuery Mobile com um aplicativo ASP.NET MVC 4.

    > [!NOTE]
    > Ao executar o comando a seguir, você irá ser baixando a biblioteca de jQuery.Mobile.MVC do Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Esse comando instala o jQuery Mobile e alguns arquivos auxiliares, incluindo o seguinte:

    - **Exibições/compartilhadas/\_Layout.Mobile.cshtml**: é um layout com base em Mobile jQuery otimizado para uma tela menor. Quando o site recebe uma solicitação de um navegador móvel, ele substituirá o layout original (\_cshtml) com este.
    - Um componente de alternância de exibição: consiste de **exibições/compartilhadas/\_ViewSwitcher.cshtml** exibição parcial e o **ViewSwitcherController.cs** controlador. Este componente exibirá um link em navegadores de dispositivos móveis para permitir que os usuários alternar para a versão da área de trabalho da página.  
        ![Projeto da Galeria de fotos com suporte móvel](whats-new-in-aspnet-mvc-4/_static/image23.png "projeto da Galeria de fotos com suporte móvel")

        *Projeto da Galeria de fotos com suporte móvel*
4. Registre os pacotes móveis. Para fazer isso, abra o **Global.asax.cs** e adicione a linha a seguir.

    (Código de trecho - *pacotes móvel do ASP.NET MVC 4 laboratório - Ex03 - Register*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Execute o aplicativo usando um navegador da web da área de trabalho.
6. Abra o **emulador do Windows Phone 7,** localizado em **Menu Iniciar | Todos os programas | Windows Phone SDK 7.1 | Emulador do Windows Phone.**
7. Na tela de início do telefone, abra o Internet Explorer. Verifique a URL onde o aplicativo é iniciado e navegue até essa URL com o navegador do telefone (por exemplo, `http://localhost:[PortNumber]/`).

    Você observará que seu aplicativo terá uma aparência diferente no emulador do Windows Phone, como o jQuery.Mobile.MVC criou novos ativos em seu projeto que mostram exibições otimizadas para dispositivos móveis.

    Observe a mensagem na parte superior do telefone, mostrando o link que alterna para o modo de exibição da área de trabalho. Além disso, o  **\_Layout.Mobile.cshtml** layout que foi criado com o pacote que você instalou incluem um layout diferente no aplicativo.

    > [!NOTE]
    > Até agora, há um link para voltar ao modo de exibição móvel. Ele será incluído em versões posteriores.

    ![Modo de exibição móvel da página inicial da Galeria de fotos](whats-new-in-aspnet-mvc-4/_static/image24.png "exibição móvel da página inicial da Galeria de fotos")

    *Modo de exibição móvel da página inicial da Galeria de fotos*
8. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Tarefa 2: Criando modos de exibição móvel

Nesta tarefa, você criará uma versão móvel do modo de exibição de índice com conteúdo adaptado para appareance melhor em dispositivos móveis.

1. Copie o **Views\Home\Index.cshtml** exibir e cole-o para criar uma cópia, renomeie o novo arquivo **Index.Mobile.cshtml**.
2. Abra o novo criado **Index.Mobile.cshtml** exibir e substituir o &lt;ul&gt; marca com este código. Ao fazer isso, você atualizará o &lt;ul&gt; marca com anotações de dados móveis jQuery para usar os temas móveis do jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Observe que:
    > 
    > - O **função dados** atributo definido como **listview** irá renderizar a lista usando estilos de listview.
    > 
    > - O **inserção de dados** atributo definido como verdadeiro mostrará a lista com borda arredondada e margem.
    > 
    > - O **filtro de dados** atributo definido como **true** irá gerar uma caixa de pesquisa.
    > 
    > Você pode aprender mais sobre as convenções de jQuery Mobile na documentação do projeto: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Pressione **CTRL + S** para salvar as alterações.
4. Alterne para o **emulador do Windows Phone** e atualizar o site. Observe a nova aparência da lista da galeria, bem como a nova caixa de pesquisa localizada na parte superior. Em seguida, digite uma palavra na caixa de pesquisa (por exemplo, **Tulips**) para iniciar uma pesquisa na Galeria de fotos.

    ![Galeria usando o estilo de listview com filtragem](whats-new-in-aspnet-mvc-4/_static/image25.png "galeria usando o estilo de listview com filtragem")

    *Usando o estilo de listview com a filtragem de galeria*

    Para resumir, você usou a receita mobilizador de exibição para criar uma cópia do modo de exibição de índice com o &quot;móvel&quot; sufixo. Este sufixo indica ao ASP.NET MVC 4 que cada solicitação gerada a partir de um dispositivo móvel usará esta cópia do índice. Além disso, você atualizou a versão móvel do modo de exibição de índice para usar o jQuery Mobile para melhorar a aparência e funcionalidade do site em dispositivos móveis.
5. Volte para o Visual Studio e abra **Site.Mobile.css** localizado sob o **conteúdo** pasta.
6. Corrija o posicionamento do título da foto para mostrá-la no lado direito da imagem. Para fazer isso, adicione o seguinte código para o **Site.Mobile.css** arquivo.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Pressione **CTRL + S** para salvar as alterações.
8. Volte para o **emulador do Windows Phone** e atualizar o site. Observe que o título de foto corretamente é posicionado agora.

    ![Título posicionado no lado direito da imagem](whats-new-in-aspnet-mvc-4/_static/image26.png "título posicionado no lado direito da imagem")

    *Título posicionado no lado direito da imagem*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Tarefa 3 - jQuery Mobile temas

Cada layout e o widget em jQuery Mobile foi projetada para um novo orientada a objeto CSS do framework que seja possível aplicar um tema concluída design visual unificada para sites e aplicativos.

padrão do jQuery Mobile tema inclui 5 amostras que recebem letras (a, b, c, r, e) para referência rápida.

Nesta tarefa, você atualizará o layout móvel para usar um tema diferente do padrão.

1. Alterne para o Visual Studio.
2. Abra o  **\_Layout.Mobile.cshtml** arquivo localizado em **exibições \ compartilhadas**.
3. Localizar o elemento div com a função de dados definida como &quot;página&quot; e atualize o **dados tema** para &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Pressione **CTRL + S** para salvar as alterações.
5. Atualizar o site no **emulador do Windows Phone** e observe o novo esquema de cores.

    ![Layout para dispositivos móveis com um esquema de cores diferentes](whats-new-in-aspnet-mvc-4/_static/image27.png "layout com um esquema de cores diferentes para dispositivos móveis")

    *Layout para dispositivos móveis com um esquema de cores diferentes*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Tarefa 4 - usando o componente de alternância de exibição e o navegador substituindo recursos

É uma convenção mobile otimizado páginas da web para adicionar um link cujo texto é algo como o modo de exibição da área de trabalho ou modo completo do site que permite aos usuários a alternar para uma versão de área de trabalho da página. O pacote jQuery.Mobile.MVC inclui um exemplo **alternância de exibição** componente para essa finalidade usada no  **\_Layout.Mobile.cshtml** exibição.

![Link para alternar para modo de área de trabalho](whats-new-in-aspnet-mvc-4/_static/image28.png "Link para alternar para modo de área de trabalho")

*Para alternar para modo de exibição da área de trabalho*

A alternância de exibição usa um novo recurso chamado **navegador substituindo**. Esse recurso permite que seu aplicativo lide com solicitações como se estivessem vindo de um navegador diferente (agente do usuário) que eles realmente são provenientes.

Nesta tarefa, você irá explorar a implementação de exemplo de um alternador de exibição adicionado jQuery.Mobile.MVC e o novo navegador substituindo recursos no ASP.NET MVC 4.

1. Alterne para o Visual Studio.
2. Abra o  **\_Layout.Mobile.cshtml** exibição localizado sob o **exibições \ compartilhadas** pasta e observe o componente de alternância de exibição que está sendo referenciado como uma exibição parcial.

    ![Layout para dispositivos móveis usando o componente de alternância de exibição](whats-new-in-aspnet-mvc-4/_static/image29.png "layout para dispositivos móveis usando o componente de alternância de exibição")

    *Layout para dispositivos móveis usando o componente de alternância de exibição*
3. Abra o  **\_ViewSwitcher.cshtml** exibição parcial.

    A exibição parcial usa o novo método **ViewContext.HttpContext.GetOverriddenBrowser()** para determinar a origem da solicitação da web e mostrar o link correspondente para alternar entre os modos de exibição do Desktop ou Mobile.

    O **GetOverridenBrowser** método retorna um **HttpBrowserCapabilitiesBase** instância que corresponde ao agente do usuário definido atualmente para a solicitação (reais ou substituído). Você pode usar esse valor para obter as propriedades como **IsMobileDevice**.

    ![Exibição parcial ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "exibição parcial ViewSwitcher")

    *Exibição parcial ViewSwitcher*
4. Abra o **ViewSwitcherController.cs** classe localizado no **controladores** pasta. Check-out ação SwitchView é chamado por um link no componente ViewSwitcher e observe os novos métodos de HttpContext.

    - O **HttpContext.ClearOverridenBrowser()** método remove qualquer agente de usuário substituído para a solicitação atual.
    - O **HttpContext.SetOverridenBrowser()** método substitui o valor do agente de usuário real da solicitação usando o agente do usuário especificado.  
        ![Controlador ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher controlador")  
*Controlador ViewSwitcher*

        Substituição do navegador é dos principais recursos do ASP.NET MVC 4, que também está disponível mesmo se você não instalar o pacote jQuery.Mobile.MVC. No entanto, esse recurso afeta somente exibição, layout e exibição parcial, e ele não afeta os recursos que dependem do objeto Request. browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Tarefa 5: adicionar a alternância de exibição na exibição da área de trabalho

Nesta tarefa, você atualizará o layout da área de trabalho para incluir a alternância de exibição. Isso permitirá que os usuários móveis voltar para o modo de exibição móvel ao procurar o modo de exibição da área de trabalho.

1. Atualizar o site no **emulador do Windows Phone**.
2. Clique no **exibição da área de trabalho** link na parte superior da Galeria. Observe que não há nenhuma alternância de exibição na exibição da área de trabalho para permitir que você retornar à exibição móvel.
3. Volte para o Visual Studio e abra o  **\_cshtml** exibição.
4. Localize a seção de login e inserir uma chamada para renderizar o  **\_ViewSwitcher** exibição parcial abaixo o  **\_LogOnPartial** exibição parcial. Em seguida, pressione **CTRL + S** para salvar as alterações.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Pressione **CTRL + S** para salvar as alterações.
6. Atualize a página no emulador do Windows Phone e clique duas vezes na tela para aumentar o zoom. Observe que a página inicial agora mostra o **exibição móvel** link que alterna de móveis para o modo de área de trabalho.

    ![Exibir alternador renderizado no modo de exibição da área de trabalho](whats-new-in-aspnet-mvc-4/_static/image32.png "alternância de exibição renderizado no modo de exibição da área de trabalho")

    *Alternância de exibição renderizada no modo de exibição da área de trabalho*
7. Alterne para o modo de exibição móvel novamente e navegue até <strong>sobre</strong> página (http://localhost[porta]/Home/About). Observe que, mesmo se você não criou uma exibição About.Mobile.cshtml, a página sobre é exibida usando o layout para dispositivos móveis (\_Layout.Mobile.cshtml).

    ![Sobre a página](whats-new-in-aspnet-mvc-4/_static/image33.png "sobre a página")

    *Sobre a página*
8. Por fim, abra o site em um navegador da Web da área de trabalho. Observe que nenhuma das atualizações do anteriores afetou a exibição da área de trabalho.

    ![Exibição de área de trabalho Galeriadefotos](whats-new-in-aspnet-mvc-4/_static/image34.png "Galeriadefotos exibição de área de trabalho")

    *Exibição de área de trabalho Galeriadefotos*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Tarefa 6: criar novos modos de exibição

O novo recurso de modos de exibição permite que um aplicativo selecione exibições dependendo do navegador que está gerando a solicitação. Por exemplo, se um navegador desktop solicitar a página inicial, o aplicativo retornará o **Views\Home\Index.cshtml** modelo. Em seguida, se um navegador móvel solicitar a página inicial, o aplicativo retornará o **Views\Home\Index.mobile.cshtml** modelo.

Nesta tarefa, você criará um layout personalizado para os dispositivos iPhone e você precisará simular solicitações de dispositivos iPhone. Para fazer isso, você pode usar qualquer um emulador/simulador de iPhone (como [Electric simulador de móveis](http://www.electricplum.com/)) ou um navegador com os complementos que modificam o agente do usuário. Para obter instruções sobre como definir a cadeia de caracteres de agente do usuário em um navegador Safari emular um iPhone, consulte [como permitir que o Safari fingem é IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) no blog de David Alison.

**Observe que essa tarefa é opcional e você pode continuar em todo o laboratório sem executá-la.**

1. No Visual Studio, pressione **SHIFT** + **F5** para parar a depuração do aplicativo.
2. Abra **Global.asax.cs** e adicione o seguinte usando a instrução.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Adicione o seguinte código realçado no aplicativo\_método Start.

    (Código de trecho - *ASP.NET MVC 4 laboratório - Ex03 - iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Você registrou um novo **DefaultDisplayMode chamado &quot;iPhone&quot;**, dentro de estático **DisplayModeProvider.Instance.Modes** lista estática, o que será comparada com cada solicitação de entrada. Se a solicitação de entrada contiver a cadeia de caracteres &quot;iPhone&quot;, ASP.NET MVC encontrará os modos de exibição cujo nome contém o &quot;iPhone&quot; sufixo. O parâmetro 0 indica como específico é o novo modo; Por exemplo, essa exibição é mais específica que geral &quot;.mobile&quot; regra que corresponde a solicitações de dispositivos móveis.

Depois que esse código é executado, quando um navegador de iPhone gera uma solicitação, o aplicativo usará o **exibições \ compartilhadas\\_Layout.iPhone.cshtml** layout, você criará as próximas etapas.

> [!NOTE]
> Essa maneira de testar a solicitação para iPhone foi simplificado para fins de demonstração e pode não funcionar conforme o esperado para cada cadeia de caracteres de agente de usuário do iPhone (para teste de exemplo diferencia maiusculas de minúsculas).

4. Criar uma cópia do  **\_Layout.Mobile.cshtml** arquivo o **exibições \ compartilhadas** pasta e renomeie a cópia de &quot; **\_Layout.iPhone.csthml**&quot;.
5. Abra  **\_Layout.iPhone.csthml** criado na etapa anterior.
6. Localizar o elemento div com o atributo de dados de função definido como **página** e altere o **dados tema** atributo &quot; **um**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Agora você tem 3 layouts em seu aplicativo ASP.NET MVC 4:

1. **\_Cshtml**: layout padrão usado para navegadores de desktop.
2. **\_Layout.Mobile.cshtml**: layout padrão usado para dispositivos móveis.
3. **\_Layout.iPhone.cshtml**: layout específicas para os dispositivos iPhone, usando um esquema de cores diferente para diferenciar de \_Layout.mobile.cshtml.
7. Pressione **F5** para executar o aplicativo e navegue até o site no **emulador do Windows Phone**.
8. Abra um **simulador de iPhone** (consulte [Apêndice C](#AppendixC) para obter instruções sobre como instalar e configurar um simulador de iPhone) e navegue até o site muito. Observe que cada telefone está usando o modelo específico.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Usando modos de exibição diferentes para cada dispositivo móvel*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Exercício 4: Usando controladores assíncronos

Microsoft .NET Framework 4.5 apresenta novos recursos de idioma em c# e Visual Basic para fornecer uma nova base de assincronia na programação .NET. Essa nova base torna programação assíncrona semelhantes e sobre tão simples quanto - programação síncrona. Agora é possível gravar os métodos de ação assíncrono no ASP.NET MVC 4 usando o **AsyncController** classe. Você pode usar métodos de ação assíncrono de longa duração, CPU não associado a solicitações. Isso evita que o servidor Web da execução de trabalho enquanto a solicitação está sendo processada de bloqueio. A classe AsyncController normalmente é usada para chamadas de serviço Web de longa execução.

Este exercício explica os conceitos básicos de uma operação assíncrona no ASP.NET MVC 4. Se você quiser se aprofundar mais, confira o seguinte artigo: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Tarefa 1: Implementando um controlador assíncrono

1. Abra o **começar** solução localizado em **fonte/Ex4-Async/Begin/** pasta. Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.

   1. Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **criar** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. Abra o **HomeController** classe o **controladores** pasta.
3. Adicione o seguinte usando a instrução.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Atualização de **HomeController** classe para herdar de **AsyncController**. Controladores que derivam de AsyncController habilitar o ASP.NET processar solicitações assíncronas e ainda pode métodos de ação síncrono do serviço.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Adicionar o **async** palavra-chave para o **índice** método e o tipo de retorno **tarefa&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > O **async** palavra-chave é uma das novas palavras-chave do .NET Framework 4.5 fornece; ele informa ao compilador que este método contém código assíncrono. Um **tarefa** objeto representa uma operação assíncrona que pode ser concluída em algum momento no futuro.
6. Substitua o **cliente. GetAsync()** chamada com a versão assíncrona completa usando palavra-chave await conforme mostrado abaixo.

    (Código de trecho - *GetAsync do ASP.NET MVC 4 laboratório - Ex04 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > Na versão anterior, você estava usando o **resultados** propriedade o **tarefa** objeto para bloquear o thread até que o resultado é retornado (versão de sincronização).
    > 
    > Adicionando o **await** palavra-chave informa ao compilador de forma assíncrona espera para a tarefa retornada da chamada de método. Isso significa que o restante do código será executado como um retorno de chamada somente depois que o método espera é concluída. Outra coisa a observar é que você não precisa alterar seu bloco try-catch para que funcione: as exceções que ocorrem em segundo plano ou em primeiro plano ainda serão capturadas sem qualquer trabalho extra usando um manipulador fornecido pelo framework.
7. Alterar o código para continuar com a implementação assíncrona, substituindo as linhas com o novo código, conforme mostrado abaixo

    (Código de trecho - *ReadAsStringAsync do ASP.NET MVC 4 laboratório - Ex04 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Execute o aplicativo. Você não observará nenhuma alteração principal, mas seu código não impedirá que um thread do pool de threads, tornando um melhor uso dos recursos do servidor e melhorar o desempenho.

    > [!NOTE]
    > Você pode aprender mais sobre os novos recursos de programação assíncronos no laboratório &quot; **programação assíncrona no .NET 4.5 com c# e Visual Basic** &quot; incluído no Kit de treinamento do Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Tarefa 2 - tempos limite de tratamento com Tokens de cancelamento

Métodos de ação assíncrono que retornam instâncias de tarefa também podem dar suporte a tempos limite. Nesta tarefa, você atualizará o código do método de índice para lidar com um cenário de tempo limite usando um token de cancelamento.

1. Volte para o Visual Studio e pressione **SHIFT + F5** para parar a depuração.
2. Adicione o seguinte usando a instrução de **HomeController** arquivo.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Atualizar a ação de índice para receber um **CancellationToken** argumento.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Atualização de **GetAsync** chamada para passar o token de cancelamento.

    (Código de trecho - *ASP.NET MVC 4 laboratório - Ex04 - SendAsync com CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Decore o *índice* método com um **AsyncTimeout** atributo definido como 500 milissegundos e um **HandleError** atributo configurado para tratar  **TaskCanceledException** redirecionando para um **TimedOut** exibição.

    (Código de trecho - *atributos do ASP.NET MVC 4 laboratório - Ex04 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Abra o **PhotoController** classe e atualizar o **galeria** método para atrasar os milissegundos de execução 1000 (1 segundo) para simular uma tarefa demorada.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Abra o **Web. config** de arquivos e habilitar os erros personalizados, adicionando o seguinte elemento.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Criar uma nova exibição em **exibições \ compartilhadas** chamado **TimedOut** e use o layout padrão. No Gerenciador de soluções, clique com botão direito do **exibições \ compartilhadas** pasta e selecione **adicionar | Exibição**.

    ![Usando modos de exibição diferentes para cada dispositivo móvel](whats-new-in-aspnet-mvc-4/_static/image36.png "usando diferentes modos de exibição para cada dispositivo móvel")

    *Usando modos de exibição diferentes para cada dispositivo móvel*
9. Atualização de **TimedOut** exibir conteúdo, conforme mostrado abaixo.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Execute o aplicativo e navegue até a URL raiz. Como você adicionou um **Sleep** de 1000 milissegundos, você obterá um erro de tempo limite, gerado pelo **AsyncTimeout** de atributos e catch pelo **HandleError** atributo.

    ![Exceção de tempo limite tratada](whats-new-in-aspnet-mvc-4/_static/image37.png "tratada de exceção de tempo limite")

    *Exceção de tempo limite tratada*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice d: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste prático laboratório, você tenha observado alguns dos novos recursos residente no ASP.NET MVC 4. Foram discutidos os seguintes conceitos:

- Aproveitar os aprimoramentos para o ASP.NET MVC projeto modelos, incluindo o novo modelo de projeto de aplicativo móvel
- Use o atributo de visor do HTML5 e consultas de mídia CSS para melhorar a exibição em dispositivos móveis
- Use o jQuery Mobile para aprimoramentos progressivos e para a criação de interface da web otimizada para toque
- Criar exibições específicas para dispositivos móveis
- Use o componente de alternância de exibição para alternar entre modos de exibição móveis e área de trabalho do aplicativo
- Criar controladores assíncronos usando o suporte da tarefa

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apêndice a: usar trechos de código

Com trechos de código, você tem todo o código que é necessário ao seu alcance. O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.

![Usando trechos de código do Visual Studio para inserir o código no seu projeto](whats-new-in-aspnet-mvc-4/_static/image38.png "trechos de código com o Visual Studio para inserir código em seu projeto")

*Usando trechos de código do Visual Studio para inserir código em seu projeto*

***Para adicionar um trecho de código usando o teclado (apenas c#)***

1. Posicione o cursor onde você deseja inserir o código.
2. Comece a digitar o nome do trecho (sem espaços ou hifens).
3. Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.
4. Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).
5. Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.

![Comece a digitar o nome do fragmento](whats-new-in-aspnet-mvc-4/_static/image39.png "comece a digitar o nome do trecho de código")

*Comece a digitar o nome do trecho de código*

![Pressione Tab para selecionar o trecho de código realçado](whats-new-in-aspnet-mvc-4/_static/image40.png "pressione Tab para selecionar o trecho de código realçado")

*Pressione Tab para selecionar o trecho de código realçado*

![Pressione Tab novamente e o trecho expandirá](whats-new-in-aspnet-mvc-4/_static/image41.png "expandirá pressione Tab novamente e o trecho de código")

*Pressione Tab novamente e o trecho de código serão expandida*

***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)***

1. Clique com botão direito em que você deseja inserir o trecho de código.
2. Selecione **Inserir trecho** seguido por **Meus trechos de código**.
3. Selecione o trecho relevante na lista, clicando nele.

![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](whats-new-in-aspnet-mvc-4/_static/image42.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")

*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*

![Selecione o trecho relevante na lista, clicando nele](whats-new-in-aspnet-mvc-4/_static/image43.png "escolher o trecho relevante na lista, clicando nele")

*Selecione o trecho relevante na lista, clicando nele*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apêndice b: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express para o bloco de Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Apêndice c: Instalando o WebMatrix 2 e o simulador de iPhone

Para executar o seu site em um dispositivo simulado iPhone, você pode usar a extensão do WebMatrix &quot;Electric simulador Mobile para iPhone&quot;. Além disso, você pode configurar a mesma extensão para executar o simulador do Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Tarefa 1: instalar o WebMatrix 2

1. Vá para [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; <em>o WebMatrix 2</em>&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar o WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalar o WebMatrix 2")

    *Instalar o WebMatrix 2*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](whats-new-in-aspnet-mvc-4/_static/image50.png "aceitar os termos de licença")

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](whats-new-in-aspnet-mvc-4/_static/image51.png "progresso da instalação")

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-aspnet-mvc-4/_static/image52.png "instalação concluída")

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Tarefa 2 - instalar o extensão do simulador de iPhone

1. Executar **WebMatrix** e abrir qualquer site existente ou criar um novo.
2. Clique o **executar** botão do **início** de faixa de opções e selecione **adicionar novo**.

    ![Adicionar nova extensão do WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "adicionar nova extensão do WebMatrix")

    *Adicionar nova extensão do WebMatrix*
3. Selecione **iPhone simulador** e clique em **instalar**.

    ![Procurando extensões WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "extensões de pesquisa no WebMatrix")

    *Procurando extensões do WebMatrix*
4. Nos detalhes do pacote, clique em **instalar** para continuar com a instalação da extensão.

    ![extensão do simulador de iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "extensão simulador de iPhone")

    *extensão do simulador de iPhone*
5. Leia e aceite o EULA de extensão.

    ![Extensão do WebMatrix EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "EULA de extensão do WebMatrix")

    *Extensão do WebMatrix EULA*
6. Agora, você pode executar o site do WebMatrix usando o opção de simulador de iPhone.

    ![Executar usando iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "executados usando iPhone")

    *Executar usando iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Tarefa 3: configurar o Visual Studio 2012 para executar o simulador de iPhone

1. Abra **Visual Studio 2012** e abrir qualquer site da Web ou criar um novo projeto.
2. Clique na seta para baixo no botão Executar e selecione **procurar com**.

    ![Procurar com](whats-new-in-aspnet-mvc-4/_static/image58.png "procurar com")

    *Procurar com*
3. No &quot;procurar com&quot; caixa de diálogo, clique em **adicionar**.
4. No &quot;adicionar programa&quot; caixa de diálogo, use os seguintes valores:

   - <strong>Programa</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (adequadamente o caminho de atualização)</em>
   - **Argumentos**: &quot;1&quot;
   - **Nome amigável**: simulador de iPhone

     ![Adicionar programa](whats-new-in-aspnet-mvc-4/_static/image59.png "adicionar programa")

     *Adicionar programa para procurar com*
5. Clique em **Okey** e feche as caixas de diálogo.
6. Agora é possível executar seus aplicativos Web no iPhone simulador do Visual Studio 2012.

    ![Procurar com iPhone simulador](whats-new-in-aspnet-mvc-4/_static/image60.png "procurar com simulador de iPhone")

    *Procurar com simulador de iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice d: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarefa 1: criar um novo Site do Windows Azure Portal

1. Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce. Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).

    ![Faça logon no portal do Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal de gerenciamento do Azure do Windows*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo Site](whats-new-in-aspnet-mvc-4/_static/image62.png "criar um novo Site")

    *Criando um novo Site*
3. Clique em **de computação** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site e clique em **Criar Site**.

    > [!NOTE]
    > Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](whats-new-in-aspnet-mvc-4/_static/image63.png "criar um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando para o novo site](whats-new-in-aspnet-mvc-4/_static/image64.png "navegando para o novo site")

    *Navegando para o novo site*

    ![Site da Web em execução](whats-new-in-aspnet-mvc-4/_static/image65.png "site em execução")

    *Site da Web em execução*
6. Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](whats-new-in-aspnet-mvc-4/_static/image66.png "abrir páginas de gerenciamento do site")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.

    ![Baixando o site da web de perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image67.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação para um local conhecido. Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image68.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL. Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**. Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados, como ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique no ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) botão.

    ![Adicionar endereço IP do cliente](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Adicionar endereço IP do cliente*
3. Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confirmar alterações*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.

    ![Publicar o aplicativo](whats-new-in-aspnet-mvc-4/_static/image73.png "a publicação do aplicativo")

    *Publicar o site*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importar o perfil de publicação](whats-new-in-aspnet-mvc-4/_static/image74.png "importar o perfil de publicação")

    *Importar o perfil de publicação*
3. Clique em **validar Conexão**. Quando a validação estiver concluída, clique em **próximo**.

    > [!NOTE]
    > A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.

    ![Validando a conexão](whats-new-in-aspnet-mvc-4/_static/image75.png "validação de conexão")

    *Validação de conexão*
4. No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
   - Em **nome de usuário** digite seu nome de logon de administrador do servidor.
   - Em **senha** digite sua senha de logon de administrador do servidor.
   - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

     ![Configurando a cadeia de caracteres de conexão de destino](whats-new-in-aspnet-mvc-4/_static/image77.png "Configurando a cadeia de caracteres de conexão de destino")

     *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](whats-new-in-aspnet-mvc-4/_static/image78.png "criar a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **visualização** , clique em **publicar**.

    ![Publicar o aplicativo web](whats-new-in-aspnet-mvc-4/_static/image80.png "publicar o aplicativo web")

    *Publicar o aplicativo web*
9. Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.

    ![Aplicativo publicado no Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplicativo publicado no Windows Azure")

    *Aplicativos publicados no Windows Azure*
